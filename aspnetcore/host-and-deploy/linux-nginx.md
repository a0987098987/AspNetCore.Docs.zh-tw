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
# <a name="host-aspnet-core-on-linux-with-nginx"></a>在 Linux 上使用 Nginx 裝載 ASP.NET Core

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。

> [!NOTE]
> 針對 Ubuntu 14.04，建議使用 *supervisord*作為監視 Kestrel 處理序的解決方案。 在 Ubuntu 14.04 上無法使用 *systemd*。 [請參閱本文件的舊版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md) \(英文\)。

本指南會：

* 將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後方。
* 設定反向 Proxy 伺服器以將要求轉送至 Kestrel 網頁伺服器。
* 確保 Web 應用程式在啟動時以精靈的形式執行。
* 設定程序管理工具以協助重新啟動 Web 應用程式。

## <a name="prerequisites"></a>必要條件

1. 以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器
1. 現有的 ASP.NET Core 應用程式

## <a name="copy-over-the-app"></a>將應用程式複製過去

從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish)，以將應用程式封裝至可在伺服器上執行的自封式目錄中。

使用任何工具 (SCP、FTP 等等) 將 ASP.NET Core 應用程式複製到伺服器，以整合至組織的工作流程。 測試應用程式，例如：

* 從命令列執行 `dotnet <app_assembly>.dll`。
* 在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 上正常運作。 
 
## <a name="configure-a-reverse-proxy-server"></a>設定反向 Proxy 伺服器

反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。 反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。

### <a name="why-use-a-reverse-proxy-server"></a>為何使用反向 Proxy 伺服器？

Kestrel 非常適用於從 ASP.NET Core 提供動態內容。 不過，Web 服務功能不像 IIS、Apache 或 Nginx 這類伺服器那樣豐富。 反向 Proxy 伺服器可以讓 HTTP 伺服器卸下提供靜態內容、快取要求、壓縮要求及終止 SSL 等工作的負擔。 反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。

為達到本指南的目的，使用 Nginx 的單一執行個體。 它會在相同的伺服器上和 HTTP 伺服器一起執行。 您可以根據需求，選擇不同的設定。

由於反向 Proxy 會轉送要求，因此請使用來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的「轉送的標頭中介軟體」。 此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。

有使用任何類型的驗證中介軟體時，必須先執行「轉送的標頭中介軟體」。 此排序可確保驗證中介軟體能夠取用標頭值，然後產生正確的重新導向 URI。

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

### <a name="install-nginx"></a>安裝 Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> 如果將安裝選用的 Nginx 模組，可能會需要從來源建置 Nginx。

使用 `apt-get` 來安裝 Nginx。 安裝程式建立的系統 V init 初始化指令碼，會在系統啟動時將 Nginx 執行為精靈。 因為 Nginx 是第一次安裝，請透過執行明確啟動它：

```bash
sudo service nginx start
```

確認瀏覽器會顯示 Nginx 的預設登陸頁面。

### <a name="configure-nginx"></a>設定 Nginx

若要將 Nginx 設定為反向 Proxy 以將要求轉送至您的 ASP.NET Core 應用程式，請修改 */etc/nginx/sites-available/default*。 以文字編輯器開啟它，並以下列項目取代內容：

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

當沒有任何與 `server_name` 相符的項目時，Nginx 會使用預設伺服器。 如果未定義任何預設伺服器，則設定檔中的第一個伺服器就是預設伺服器。 最佳做法是，在您的設定檔中新增一個會傳回狀態碼 444 的特定預設伺服器。 預設伺服器設定範例如下：

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

使用上述設定檔和預設伺服器時，Nginx 會在連接埠 80 接受主機標頭為 `example.com` 或 `*.example.com` 的公用流量。 不符合這些主機的要求將不會轉送至 Kestrel。 Nginx 會將相符的要求轉送至位於 `http://localhost:5000` 的 Kestrel。 如需詳細資訊，請參閱 [nginx 如何處理要求](https://nginx.org/docs/http/request_processing.html) \(英文\)。

> [!WARNING]
> 如果無法指定適當的 [server_name 指示詞](https://nginx.org/docs/http/server_names.html)，就會讓應用程式暴露在安全性弱點的風險下。 若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。 如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。

建立 Nginx 設定之後，請執行 `sudo nginx -t` 來確認設定檔的語法。 如果設定檔測試成功，請執行 `sudo nginx -s reload` 來強制 Nginx 套用這些變更。

## <a name="monitoring-the-app"></a>監視應用程式

伺服器已設定完成，可將對 `http://<serveraddress>:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。 不過，並未設定 Nginx 來管理 Kestrel 處理序。 您可以使用 *systemd* 來建立服務檔案，以啟動並監視基礎 Web 應用程式。 *systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。 

### <a name="create-the-service-file"></a>建立服務檔

建立服務定義檔：

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

以下是一個應用程式範例服務檔：

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

如果設定不是使用 *www-data* 這個使用者，就必須先建立這裡所定義的使用者，並授與適當的檔案擁有權。

Linux 的檔案系統會區分大小寫。 將 ASPNETCORE_ENVIRONMENT 設定為 "Production" 會導致搜尋 *appsettings.Production.json* 設定檔，而不是搜尋 *appsettings.production.json*。

> [!NOTE]
> 有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。 請使用下列命令來產生要在設定檔中使用並已適當逸出的值：
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

儲存檔案並啟用服務。

```bash
systemctl enable kestrel-hellomvc.service
```

啟動服務並確認它正在執行。

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

設定好反向 Proxy 並透過 systemd 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。 此外，也可以從遠端電腦存取它，除非遭到任何防火牆封鎖。 檢查回應標頭時，`Server` 標頭會顯示是由 Kestrel 為 ASP.NET Core 應用程式提供服務。

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>檢視記錄

由於是使用 `systemd` 來管理使用 Kestrel 的 Web 應用程式，因此會將所有事件和處理序都記錄在集中式日誌中。 不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。 若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>確保應用程式的安全性

### <a name="enable-apparmor"></a>啟用 AppArmor

Linux 安全性模組 (LSM) 是 Linux 2.6 之後 Linux 核心所包含的一個架構。 LSM 支援不同的安全性模組實作。 [AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。 確定已啟用並正確設定 AppArmor。

### <a name="configuring-the-firewall"></a>設定防火牆

關閉所有不在使用中的外部連接埠。 簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。 確認已設定 `ufw` 來允許所需任何連接埠上的流量。

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>保護 Nginx

Nginx 的預設分佈不會啟用 SSL。 為啟用其他的安全性功能，請從來源建置。

#### <a name="download-the-source-and-install-the-build-dependencies"></a>下載來源並安裝組建相依性

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>變更 Nginx 回應名稱

編輯 *src/http/ngx_http_header_filter_module.c*：

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>設定選項和組建

規則運算式需要 PCRE 程式庫。 規則運算式用於 ngx_http_rewrite_module 的位置指示詞。 http_ssl_module 新增 HTTPS 通訊協定的支援。

請考慮使用像 *ModSecurity* 這樣的 Web 應用程式防火牆來強化應用程式。

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>設定 SSL

* 藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，將伺服器設定成在連接埠 `443` 上接聽 HTTPS 流量。

* 採用以下 */etc/nginx/nginx.conf* 檔案所述的一些做法來強化安全性。 範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。

* 新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都只會透過 HTTPS。

* 如果未來將會停用 SSL，則不要新增 Strict-Transport-Security 標頭，或選擇適當的 `max-age`。

新增 */etc/nginx/Proxy.conf* 組態檔：

[!code-nginx[](linux-nginx/proxy.conf)]

編輯 */etc/nginx/Proxy.conf* 組態檔。 此範例在一個組態檔中同時包含 `http` 和 `server` 區段。

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>保護 Nginx 免於點閱綁架
點閱綁架是收集受感染使用者按一下動作的惡意技術。 點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。 使用 X-FRAME-OPTIONS 來保護網站。

編輯 *nginx.conf* 檔案：

```bash
sudo nano /etc/nginx/nginx.conf
```

新增行 `add_header X-Frame-Options "SAMEORIGIN";` 並儲存檔案，然後重新啟動 Nginx。

#### <a name="mime-type-sniffing"></a>MIME 類型探查

此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。 使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。

編輯 *nginx.conf* 檔案：

```bash
sudo nano /etc/nginx/nginx.conf
```

新增 `add_header X-Content-Type-Options "nosniff";` 行並儲存檔案，然後重新啟動 Nginx。
