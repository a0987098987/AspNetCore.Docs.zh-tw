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
# <a name="host-aspnet-core-on-linux-with-apache"></a>在 Linux 上使用 Apache 裝載 ASP.NET Core

作者：[Shayne Boyer](https://github.com/spboyer)

使用本指南來了解如何在 [CentOS 7](https://www.centos.org/) 上將 [Apache](https://httpd.apache.org/) 設定為反向 Proxy 伺服器，以將 HTTP 流量重新導向至在 [Kestrel](xref:fundamentals/servers/kestrel) 上執行的 ASP.NET Core Web 應用程式。 [mod_proxy 延伸模組](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)和相關的模組會建立伺服器的反向 Proxy。

## <a name="prerequisites"></a>必要條件

1. 執行 CentOS 7 的伺服器搭配具有 sudo 權限的標準使用者帳戶。
1. 在伺服器上安裝 .NET Core 執行階段。
   1. 請前往 [.NET Core 的 All Downloads (下載區)](https://www.microsoft.com/net/download/all) 頁面。
   1. 在 [執行階段] 下的清單中選取最新的非預覽執行階段。
   1. 選取並遵循 CentOS/Oracle 的指示。
1. 現有的 ASP.NET Core 應用程式。

## <a name="publish-and-copy-over-the-app"></a>跨應用程式發佈與複製

為[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)設定應用程式。

從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將應用程式封裝到可在伺服器上執行的目錄 (例如，*bin/Release/&lt;target_framework_moniker&gt;/publish*)：

```console
dotnet publish --configuration Release
```

如果您不想在伺服器上維護 .NET Core 執行階段，應用程式也可以發佈為[獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。

使用整合至組織工作流程的工具 (SCP、SFTP 等等) 將 ASP.NET Core 應用程式複製到伺服器。 Web 應用程式通常可在 *var* 目錄下找到 (例如，*var/aspnetcore/hellomvc*)。

> [!NOTE]
> 在生產環境部署案例中，持續整合工作流程會執行發佈應用程式並將資產複製到伺服器的工作。

## <a name="configure-a-proxy-server"></a>設定 Proxy 伺服器

反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。 反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET 應用程式。

Proxy 伺服器則是會將用戶端要求轉送至另一部伺服器，而不是自己完成這些要求。 反向 Proxy 會轉送至固定目的地，通常代表任意的用戶端。 在本指南中，是將 Apache 設定成反向 Proxy，且執行所在的伺服器與 Kestrel 為 ASP.NET Core 應用程式提供服務的伺服器相同。

由於反向 Proxy 會轉送要求，因此請使用來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。 此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。

任何依賴配置的元件，例如驗證、連結產生、重新導向和地理位置，都必須在叫用轉送的標頭中介軟體後放置。 轉送的標頭中介軟體是一般規則，應該先於診斷和錯誤處理中介軟體以外的其他中介軟體執行。 這種排序可確保依賴轉送標頭資訊的中介軟體可以耗用用於處理的標頭值。

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> 不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) 或類似的驗證配置中介軟體。 請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) 和 [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) 或類似的驗證配置中介軟體。 請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：

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

如果未將任何 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 指定給中介軟體，則要轉送的預設標頭會是 `None`。

Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

### <a name="install-apache"></a>安裝 Apache

將 CentOS 套件更新至其最新穩定版本：

```bash
sudo yum update -y
```

使用單一 `yum` 命令在 CentOS 上安裝 Apache 網頁伺服器：

```bash
sudo yum -y install httpd mod_ssl
```

執行命令之後的範例輸出：

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
> 在此範例中，輸出會反映 httpd.86_64，因為 CentOS 第 7 版是 64 位元。 若要確認 Apache 的安裝位置，請從命令提示字元執行 `whereis httpd`。

### <a name="configure-apache"></a>設定 Apache

Apache 的組態檔是位於 `/etc/httpd/conf.d/` 目錄內。 除了 `/etc/httpd/conf.modules.d/` (包含載入模組所需的所有設定檔) 中的模組設定檔之外，任何副檔名為 *.conf* 的檔案也會依字母順序處理。

為應用程式建立名為 *hellomvc.conf*的設定檔：

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

`VirtualHost` 區塊可以在伺服器上的一或多個檔案中出現多次。 在上述設定檔中，Apache 會在連接埠 80 接受公用流量。 所服務的網域是 `www.example.com`，而 `*.example.com` 別名則會解析成同一個網站。 如需詳細資訊，請參閱[名稱型虛擬主機支援](https://httpd.apache.org/docs/current/vhosts/name-based.html) \(英文\)。 要求會在根目錄透過 Proxy 傳送至位於 127.0.0.1 之伺服器的連接埠 5000。 如需進行雙向通訊，則必須要有 `ProxyPass` 和 `ProxyPassReverse`。 若要變更 Kestrel 的 IP/連接埠，請參閱 [Kestrel：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。

> [!WARNING]
> 如果無法在 **VirtualHost** 區塊中指定適當的 [ServerName 指示詞](https://httpd.apache.org/docs/current/mod/core.html#servername)，就會讓應用程式暴露在安全性弱點的風險下。 若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。 如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。

您可以使用 `ErrorLog` 和 `CustomLog` 指示詞來依 `VirtualHost` 設定記錄功能。 `ErrorLog` 是伺服器記錄錯誤的位置，而 `CustomLog` 則會設定記錄檔的檔案名稱和格式。 在此案例中，這就是記錄要求資訊的位置。 每個要求都會有一行。

請儲存檔案並測試設定。 如果所有項目都通過，回應應該是 `Syntax [OK]`。

```bash
sudo service httpd configtest
```

重新啟動 Apache：

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>監視應用程式

Apache 現在已設定完成，可將對 `http://localhost:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。  不過，並未設定 Apache 來管理 Kestrel 處理序。 請使用 *systemd* 並建立服務檔案，以啟動並監視基礎 Web 應用程式。 *systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。 

### <a name="create-the-service-file"></a>建立服務檔

建立服務定義檔：

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

應用程式的範例服務檔：

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
> **User** &mdash; 如果設定不是使用 *apache* 這個使用者，就必須先建立使用者並授與適當的檔案擁有權。

> [!NOTE]
> 有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。 請使用下列命令來產生要在設定檔中使用並已適當逸出的值：
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

儲存檔案並啟用服務：

```bash
systemctl enable kestrel-hellomvc.service
```

啟動服務並確認它正在執行：

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

設定好反向 Proxy 並透過 *systemd* 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。 檢查回應標頭時，**Server** 標頭會指出是由 Kestrel 為 ASP.NET Core 應用程式提供服務：

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>檢視記錄

由於是使用 *systemd* 來管理使用 Kestrel 的 Web 應用程式，因此會將事件和處理序都記錄在集中式日誌中。 不過，此日誌包含 *systemd* 所管理全部服務和處理序的項目。 若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

如需進行時間篩選，請搭配命令指定時間選項。 例如，使用 `--since today` 來針對今天進行篩選，或使用 `--until 1 hour ago` 來查看前一個小時的項目。 如需詳細資訊，請參閱 [journalctl 的手冊頁面](https://www.unix.com/man-page/centos/1/journalctl/) \(英文\)。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>確保應用程式的安全性

### <a name="configure-firewall"></a>設定防火牆

*Firewalld* 是一個可管理防火牆並支援網路區域的動態精靈。 您仍然可以使用 iptables 來管理連接埠和封包篩選。 預設應該安裝 *Firewalld*。 您可以使用 `yum` 來安裝套件或確認是否已安裝套件。

```bash
sudo yum install firewalld -y
```

請使用 `firewalld` 來僅開啟應用程式所需的連接埠。 在此情況下，將使用連接埠 80 和 443。 下列命令會將連接埠 80 和 443 永久設定為開啟：

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

重新載入防火牆設定。 檢查預設區域中可用的服務和連接埠。 您可以檢查 `firewall-cmd -h` 來取得選項。

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

為了設定適用於 SSL 的 Apache，會使用 *mod_ssl* 模組。 安裝 *httpd* 模組時，已一併安裝 *mod_ssl* 模組。 如果未安裝該模組，請使用 `yum` 將它新增到設定中。

```bash
sudo yum install mod_ssl
```

若要強制執行 SSL，請安裝 `mod_rewrite` 模組來啟用 URL 重寫：

```bash
sudo yum install mod_rewrite
```

修改 *hellomvc.conf* 檔案以啟用 URL 重寫並保護連接埠 443 上的通訊：

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
> 此範例使用本機產生的憑證。 **SSLCertificateFile** 應該是網域名稱的主要憑證檔案。 **SSLCertificateKeyFile** 應該是建立 CSR 時產生的金鑰檔。 **SSLCertificateChainFile** 應該是憑證授權單位所提供的中繼憑證檔案 (如果有的話)。

儲存檔案並測試設定：

```bash
sudo service httpd configtest
```

重新啟動 Apache：

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Apache 的其他建議

### <a name="additional-headers"></a>其他標頭

為了防範惡意攻擊，應該要修改或新增一些標頭。 請確認已安裝 `mod_headers` 模組：

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>保護 Apache 免於點閱綁架攻擊

[點閱綁架](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)(也稱為「UI 偽裝攻擊」) 是一種惡意攻擊，會誘騙網站訪客點選與其目前所瀏覽頁面不同的頁面上連結或按鈕。 請使用 `X-FRAME-OPTIONS` 來保護網站安全。

編輯 *httpd.conf* 檔案：

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

新增 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` 行。 儲存檔案。 重新啟動 Apache。

#### <a name="mime-type-sniffing"></a>MIME 類型探查

`X-Content-Type-Options` 標頭可防止 Internet Explorer 執行「MIME 探查」 (從檔案的內容判斷檔案的 `Content-Type`)。 如果伺服器將 `Content-Type` 標頭設定為 `text/html` 並搭配設定 `nosniff` 選項，則不論檔案內容為何，Internet Explorer 都會將該內容轉譯為 `text/html`。

編輯 *httpd.conf* 檔案：

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

新增 `Header set X-Content-Type-Options "nosniff"` 行。 儲存檔案。 重新啟動 Apache。

### <a name="load-balancing"></a>負載平衡

這個範例示範如何在 CentOS 7 上安裝和設定 Apache，以及如何在相同的執行個體電腦上安裝和設定 Kestrel。 為了避免產生單一失敗點的情況，使用 *mod_proxy_balancer* 並修改 **VirtualHost**將可允許管理位於 Apache Proxy 伺服器後方的多個 Web 應用程式執行個體。

```bash
sudo yum install mod_proxy_balancer
```

在以下所示的設定檔中，已將一個額外的 `hellomvc` 應用程式執行個體設定在連接埠 5001 上執行。 *Proxy* 區段中設定了平衡器設定，其中有兩個成員為 *byrequests* 進行負載平衡。

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

### <a name="rate-limits"></a>速率限制

使用 *mod_ratelimit* (包含在 *httpd* 模組中) 時，可以限制用戶端的頻寬：

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
此範例檔案將根目錄位置下的頻寬限制為 600 KB/秒：

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a>其他資源

* [設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)
