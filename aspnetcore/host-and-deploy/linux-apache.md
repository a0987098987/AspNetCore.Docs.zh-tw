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
ms.openlocfilehash: 61827f456ba01ffa726f3446401156409b29111d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>在 Linux 上使用 Apache 裝載 ASP.NET Core

作者：[Shayne Boyer](https://github.com/spboyer)

使用本指南，了解如何設定[Apache](https://httpd.apache.org/)為反向 proxy 伺服器上[CentOS 7](https://www.centos.org/) ASP.NET Core web 應用程式上執行的 HTTP 流量重新導向[Kestrel](xref:fundamentals/servers/kestrel)。 [Mod_proxy 延伸](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)和相關的模組建立伺服器的反向 proxy。

## <a name="prerequisites"></a>必要條件

1. 具有 sudo 權限的標準使用者帳戶執行 CentOS 7 的伺服器
2. ASP.NET Core 應用程式

## <a name="publish-the-app"></a>發行應用程式

發行應用程式做為[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)在 CentOS 7 執行階段的 [發行] 組態 (`centos.7-x64`)。 複製的內容*bin/Release/netcoreapp2.0/centos.7-x64/publish*使用 SCP、 FTP 或其他檔案傳送方法在伺服器的資料夾。

> [!NOTE]
> 在生產環境部署案例中，連續整合工作流程會發佈應用程式和資產複製到伺服器的工作。 

## <a name="configure-a-proxy-server"></a>設定 Proxy 伺服器

反向 proxy 是常見的安裝程式來服務動態 web 應用程式。 反向 proxy 終止 HTTP 要求，並將它轉送至 ASP.NET 應用程式。

用戶端將要求轉送至另一部伺服器，而不是本身完成要求的 proxy 伺服器。 反向 Proxy 會轉送至固定目的地，通常代表任意的用戶端。 在本指南中，Apache 被設定為在相同的伺服器上執行 Kestrel 提供服務的 ASP.NET Core 應用程式的反向 proxy。

要求會透過反向 proxy 來轉送的因為使用轉送標頭中介軟體從[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)封裝。 中介軟體更新`Request.Scheme`，並使用`X-Forwarded-Proto`標頭，以便讓該重新導向 Uri 和其他的安全性原則運作正確。

使用任何類型的驗證中介軟體時，必須先執行轉送標頭中介軟體。 這種排序可確保驗證中介軟體可以取用的標頭值，並產生正確的重新導向 Uri。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`之前先呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)或類似的驗證配置中介軟體：

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`之前先呼叫[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)和[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)或類似的驗證配置中介軟體：

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

如果沒有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)指定給中介軟體，轉送的預設標頭`None`。

### <a name="install-apache"></a>安裝 Apache

更新至最新穩定版本 CentOS 套件：

```bash
sudo yum update -y
```

CentOS 上安裝 Apache web 伺服器，以單一`yum`命令：

```bash
sudo yum -y install httpd mod_ssl
```

執行命令之後輸出範例：

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
> 在此範例中，輸出會反映 httpd.86_64 因為 CentOS 7 版本是 64 位元。 若要確認 Apache 的安裝位置，請從命令提示字元執行 `whereis httpd`。 

### <a name="configure-apache-for-reverse-proxy"></a>設定 Apache 以用於反向 Proxy

Apache 的組態檔是位於 `/etc/httpd/conf.d/` 目錄內。 任何檔案*.conf*除了模組組態檔中，依字母順序處理延伸模組`/etc/httpd/conf.modules.d/`，其中包含任何設定檔需要載入模組。

建立名為應用程式組態檔`hellomvc.conf`:

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

**VirtualHost**節點可在伺服器上的一個或多個檔案中出現多次。 **VirtualHost**設定為使用通訊埠 80 的任何 IP 位址上接聽。 下面兩行 port 5000 到 127.0.0.1 的伺服器根目錄的 proxy 要求會設定。 雙向通訊， *ProxyPass*和*ProxyPassReverse*所需。

記錄可以設定每個**VirtualHost**使用**ErrorLog**和**CustomLog**指示詞。 **錯誤記錄檔**是伺服器記錄錯誤，在其中的位置和**CustomLog**設定檔案名稱和記錄檔格式。 在此情況下，這是記錄要求資訊的位置。 沒有為每個要求的一列。

儲存檔案並測試組態。 如果所有項目都通過，回應應該是 `Syntax [OK]`。

```bash
sudo service httpd configtest
```

重新啟動 Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>監視應用程式

Apache 現在是安裝所做的要求轉送給`http://localhost:80`Kestrel 在上執行的 ASP.NET Core 應用程式`http://127.0.0.1:5000`。  不過，Apache 並未設定來管理 Kestrel 程序。 使用*systemd*並建立服務檔案，啟動及監視基礎的 web 應用程式。 *systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。 


### <a name="create-the-service-file"></a>建立服務檔

建立服務定義檔：

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

範例服務檔案的應用程式：

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
> **使用者**&mdash;如果使用者*apache*不會使用設定，使用者必須先建立並提供適當的擁有權的檔案。

儲存檔案，並啟用服務：

```bash
systemctl enable kestrel-hellomvc.service
```

啟動服務並確認正在執行：

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

設定反向 proxy 與透過管理 Kestrel *systemd*，完整設定 web 應用程式，並可從在本機電腦上的瀏覽器存取`http://localhost`。 檢查回應標頭**伺服器**標頭指出 ASP.NET Core 應用程式由 Kestrel:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>檢視記錄

因為 web 應用程式使用 Kestrel 管理使用*systemd*，事件和處理程序會記錄到集中式的日誌。 不過，此筆記本中內含的所有服務和處理程序受項目*systemd*。 若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

時間篩選時，使用命令所指定時間選項。 例如，使用`--since today`篩選目前的日期或`--until 1 hour ago`查看前一小時的項目。 如需詳細資訊，請參閱[journalctl man 頁面](https://www.unix.com/man-page/centos/1/journalctl/)。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>保護應用程式

### <a name="configure-firewall"></a>設定防火牆

*Firewalld*是動態管理網路區域的支援具有防火牆協助程式。 仍可由 iptables 管理連接埠以及封包篩選。 *Firewalld*預設應該安裝。 `yum`可用來安裝封裝，或檢查已安裝。

```bash
sudo yum install firewalld -y
```

使用`firewalld`開啟應用程式所需的連接埠。 在此情況下，將使用連接埠 80 和 443。 下列命令會永久設定連接埠 80 和 443 開啟：

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

重新載入的防火牆設定。 檢查可用的服務和連接埠的預設區域。 可用選項藉由檢查`firewall-cmd -h`。

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

若要設定 ssl，Apache *mod_ssl*使用模組。 當*httpd*模組已安裝， *mod_ssl*也安裝模組。 如果未安裝，請使用`yum`將它加入至組態。

```bash
sudo yum install mod_ssl
```
若要強制 SSL，請安裝`mod_rewrite`模組來啟用 URL 重寫：

```bash
sudo yum install mod_rewrite
```

修改*hellomvc.conf*啟用 URL 重寫和安全通訊連接埠 443 上的檔案：

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
> 這個範例會使用在本機產生憑證。 **SSLCertificateFile**應該是網域名稱的主要憑證檔案。 **SSLCertificateKeyFile**應金鑰檔案產生 CSR 建立時。 **SSLCertificateChainFile**應該是中繼憑證檔案 （如果有的話），由憑證授權單位所提供。

儲存檔案並測試組態：

```bash
sudo service httpd configtest
```

重新啟動 Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Apache 的其他建議

### <a name="additional-headers"></a>其他標頭

為了保護對抗惡意攻擊，有幾個標頭應該可以修改或加入。 請確認`mod_headers`模組安裝：

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Apache 防範 clickjacking 攻擊

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)，亦稱為*UI redress 攻擊*，是一種惡意攻擊網站訪客誘騙比目前瀏覽，按一下連結或不同的頁面上的按鈕。 使用`X-FRAME-OPTIONS`來保護安全的站台。

編輯*httpd.conf*檔案：

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

將行加入`Header append X-FRAME-OPTIONS "SAMEORIGIN"`。 儲存檔案。 重新啟動 Apache。

#### <a name="mime-type-sniffing"></a>MIME 類型探查

`X-Content-Type-Options`標頭會防止 Internet Explorer 的*MIME 探查*(判斷 tempdb 的檔案`Content-Type`檔案的內容中)。 如果伺服器設定`Content-Type`標頭`text/html`與`nosniff`選項集，Internet Explorer 會轉譯為內容`text/html`不論檔案的內容。

編輯*httpd.conf*檔案：

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

將行加入`Header set X-Content-Type-Options "nosniff"`。 儲存檔案。 重新啟動 Apache。

### <a name="load-balancing"></a>負載平衡 

這個範例示範如何在 CentOS 7 上安裝和設定 Apache，以及如何在相同的執行個體電腦上安裝和設定 Kestrel。 若要不需要單點失敗。使用*mod_proxy_balancer*和修改**VirtualHost**允許管理的 Apache proxy 伺服器後方的 web 應用程式的多個執行個體。

```bash
sudo yum install mod_proxy_balancer
```

在組態檔中顯示以下的其他執行個體`hellomvc`應用程式是在連接埠 5001 上執行安裝程式。 *Proxy*區段設定兩個成員平衡器組態，以進行負載平衡*byrequests*。

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

### <a name="rate-limits"></a>速率限制
使用*mod_ratelimit*，隨附於*httpd*模組，用戶端頻寬可能會受到限制：

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
範例檔案限制為 600 的 KB/秒根位置下的頻寬：

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
