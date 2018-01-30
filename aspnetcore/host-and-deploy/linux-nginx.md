---
title: "在 Linux 上使用 Nginx 裝載 ASP.NET Core"
author: rick-anderson
description: "描述如何設定 Nginx 做 Ubuntu 16.04 轉送 Kestrel 上執行 ASP.NET Core web 應用程式的 HTTP 流量上的反向 proxy。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 9939e420fee41b11e709da911d4051a048e789b3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>在 Linux 上使用 Nginx 裝載 ASP.NET Core

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。

**注意：** For Ubuntu 14.04 *supervisord*建議做為監視 Kestrel 程序的解決方案。 *systemd* Ubuntu 14.04 上無法使用。 [請參閱本文件的舊版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

本指南：

* 將現有的 ASP.NET Core 應用程式的反向 proxy 伺服器後方。
* 設定反向 proxy 伺服器，將要求轉送到 Kestrel web 伺服器。
* 可確保 web 應用程式啟動時執行的服務精靈為。
* 設定可協助您重新啟動 web 應用程式的程序管理工具。

## <a name="prerequisites"></a>必要條件

1. 以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器
1. 現有的 ASP.NET Core 應用程式

## <a name="copy-over-the-app"></a>複製應用程式

從開發環境執行 `dotnet publish`，將應用程式封裝到可在伺服器執行的獨立目錄中。

將 ASP.NET Core 應用程式複製到使用任何工具整合到組織的工作流程 （例如，SCP，FTP） 伺服器。 測試應用程式，例如：

* 從命令列執行`dotnet <app_assembly>.dll`。
* 在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 上正常運作。 
 
## <a name="configure-a-reverse-proxy-server"></a>設定反向 Proxy 伺服器

反向 proxy 是常見的安裝程式來服務動態 web 應用程式。 反向 proxy 終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。

### <a name="why-use-a-reverse-proxy-server"></a>為何使用反向 Proxy 伺服器？

Kestrel 適合用來從 ASP.NET Core 提供動態內容。 不過，web 服務功能不是做為伺服器，如 IIS、 Apache 或 Nginx 豐富的功能。 反向 proxy 伺服器可以卸載提供靜態內容、 快取要求、 壓縮要求，以及從 HTTP 伺服器的 SSL 終止的工作。 反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。

為達到本指南的目的，使用 Nginx 的單一執行個體。 它會在相同的伺服器上和 HTTP 伺服器一起執行。 根據需求，不同的安裝程式可能會選擇。

因為反向 Proxy 會轉送要求，請從 `Microsoft.AspNetCore.HttpOverrides` 套件使用 `ForwardedHeaders` 中介軟體。 此中介軟體會使用 `X-Forwarded-Proto` 標頭更新 `Request.Scheme`，讓重新導向 URI 和其他安全性原則正確運作。

設定反向 Proxy 伺服器時，驗證中介軟體需要 `UseForwardedHeaders` 先執行。 這種排序可確保驗證中介軟體能夠取用受影響的值，並產生正確的重新導向 URI。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

先叫用 `UseForwardedHeaders` 方法 (在 *Startup.cs* 的 `Configure`方法中)，再呼叫 `UseAuthentication` 或類似的驗證配置中介軟體：

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

先叫用 `UseForwardedHeaders` 方法 (在 *Startup.cs* 的 `Configure` 方法中)，再呼叫 `UseIdentity` 和 `UseFacebookAuthentication` 或類似的驗證配置中介軟體：

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

### <a name="install-nginx"></a>安裝 Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> 如果要安裝選擇性 Nginx 模組，建立從來源 Nginx 可能需要。

使用 `apt-get` 來安裝 Nginx。 安裝程式建立的系統 V init 初始化指令碼，會在系統啟動時將 Nginx 執行為精靈。 因為 Nginx 是第一次安裝，請透過執行明確啟動它：

```bash
sudo service nginx start
```

確認瀏覽器會顯示 Nginx 的預設登陸頁面。

### <a name="configure-nginx"></a>設定 Nginx

若要設定 Nginx 做為轉寄要求至我們的 ASP.NET Core 應用程式的反向 proxy，修改`/etc/nginx/sites-available/default`。 以文字編輯器開啟它，並以下列項目取代內容：

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

此 Nginx 組態檔會將連入的公用流量從連接埠 `80` 轉送到連接埠 `5000`。

一旦建立 Nginx 組態之後，執行`sudo nginx -t`驗證組態檔的語法。 如果組態檔測試成功時，強制以收取這些變更藉由執行 Nginx `sudo nginx -s reload`。

## <a name="monitoring-the-app"></a>監視應用程式

伺服器是安裝所做的要求轉送給`http://<serveraddress>:80`入 Kestrel 在上執行 ASP.NET Core 應用程式`http://127.0.0.1:5000`。 不過，Nginx 並未設定來管理 Kestrel 程序。 *systemd*可用來建立服務檔案，以啟動及監視基礎的 web 應用程式。 *systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。 

### <a name="create-the-service-file"></a>建立服務檔

建立服務定義檔：

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

以下是範例服務檔案的應用程式：

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

**注意：**如果使用者*www 資料*不會使用設定，此處定義的使用者必須先建立並提供適當的擁有權的檔案。
**注意：** Linux 具有區分大小寫的檔案系統。 設定為 「 生產 」 會產生組態檔的搜尋 ASPNETCORE_ENVIRONMENT *appsettings。Production.json*，而非*appsettings.production.json*。

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

Web 應用程式完全設定和可以從在本機電腦上的瀏覽器存取的反向 proxy 設定和管理透過 systemd Kestrel， `http://localhost`。 它也是可從遠端電腦，限制可能會封鎖任何防火牆存取。 檢查回應標頭`Server`標頭會顯示由 Kestrel ASP.NET Core 應用程式。

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>檢視記錄

因為 web 應用程式使用 Kestrel 管理使用`systemd`，所有事件和處理程序會都記錄到集中式的日誌。 不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。 若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>保護應用程式

### <a name="enable-apparmor"></a>啟用 AppArmor

Linux 安全性模組 (LSM) 是一種架構，屬於後 Linux 2.6 Linux 核心。 LSM 支援不同的安全性模組實作。 [AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。 確定已啟用並正確設定 AppArmor。

### <a name="configuring-the-firewall"></a>設定防火牆

關閉所有不在使用中的外部連接埠。 簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。 確認`ufw`設定為允許任何所需的連接埠上的流量。

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

請考慮使用 web 應用程式防火牆像*ModSecurity*強化應用程式。

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>設定 SSL

* 將伺服器設定為接聽連接埠上的 HTTPS 流量`443`藉由指定核發由受信任憑證授權單位 (CA) 的有效憑證。

* 利用一些下列所述的作法強化安全性*/etc/nginx/nginx.conf*檔案。 範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。

* 新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都只會透過 HTTPS。

* 請勿將加入 Strict 傳輸安全性標頭，或選擇適當`max-age`如果未來將會停用 SSL。

新增 */etc/nginx/Proxy.conf* 組態檔：

[!code-nginx[Main](linux-nginx/proxy.conf)]

編輯 */etc/nginx/Proxy.conf* 組態檔。 此範例在一個組態檔中同時包含 `http` 和 `server` 區段。

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>保護 Nginx 免於點閱綁架
點閱綁架是收集受感染使用者按一下動作的惡意技術。 點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。 使用 X-框架-選項來保護安全的站台。

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
