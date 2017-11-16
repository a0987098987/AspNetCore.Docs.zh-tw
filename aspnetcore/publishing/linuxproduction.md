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
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>在 Linux 上使用 Nginx 設定並部署到 ASP.NET Core 裝載環境

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。

**注意：**若為 Ubuntu 14.04，建議當成監視 Kestrel 程序的解決方案監督。 Ubuntu 14.04 無法使用 systemd。 [請參閱本文件的舊版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

本指南：

* 將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後端
* 設定反向 Proxy 伺服器，將要求轉送到 Kestrel 網頁伺服器。
* 確保 Web 應用程式在啟動時執行為精靈
* 設定程序管理工具以協助重新啟動 Web 應用程式

## <a name="prerequisites"></a>必要條件

1. 以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器
2. 現有的 ASP.NET Core 應用程式

## <a name="copy-over-your-app"></a>複製蓋過您的應用程式

從開發環境執行 `dotnet publish`，將應用程式封裝到可在伺服器執行的獨立目錄中。

使用任何工具 (SCP、FTP 等等) 將 ASP.NET Core 應用程式複製到伺服器，整合至您的工作流程。 測試應用程式，例如：
 - 從命令列執行 `dotnet yourapp.dll`
 - 在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 上正常運作。 
 
**注意：**使用 [Yeoman](xref:client-side/yeoman) 針對新專案建立新的 ASP.NET Core 應用程式。

## <a name="configure-a-reverse-proxy-server"></a>設定反向 Proxy 伺服器

反向 Proxy 是服務動態 Web 應用程式的常見安裝程式。 反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。

### <a name="why-use-a-reverse-proxy-server"></a>為何使用反向 Proxy 伺服器？

Kestrel 非常適合提供 ASP.NET Core 的動態內容，但 Web 服務組件不像 IIS、Apache 或 Nginx 等伺服器的功能豐富。 反向 Proxy 伺服器可以卸載像提供靜態內容、快取要求、壓縮要求，以及從 HTTP 伺服器終止 SSL 等工作。 反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。

為達到本指南的目的，使用 Nginx 的單一執行個體。 它會在相同的伺服器上和 HTTP 伺服器一起執行。 根據您的需求，您可以選擇不同的安裝程式。

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
> 如果您打算安裝選擇性 Nginx 模組，您可能需要從來源建置 Nginx。

使用 `apt-get` 來安裝 Nginx。 安裝程式建立的系統 V init 初始化指令碼，會在系統啟動時將 Nginx 執行為精靈。 因為 Nginx 是第一次安裝，請透過執行明確啟動它：

```bash
sudo service nginx start
```

確認瀏覽器會顯示 Nginx 的預設登陸頁面。

### <a name="configure-nginx"></a>設定 Nginx

若要將 Nginx 設定為反向 Proxy，將要求轉寄至我們的 ASP.NET Core 應用程式，請修改 `/etc/nginx/sites-available/default`。 以文字編輯器開啟它，並以下列項目取代內容：

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

此 Nginx 組態檔會將連入的公用流量從連接埠 `80` 轉送到連接埠 `5000`。

一旦完成對 Nginx 組態的變更，您就可以執行 `sudo nginx -t` 以驗證組態檔的語法。 如果組態檔測試成功，您可以要求 Nginx 藉由執行 `sudo nginx -s reload` 來挑選這些變更。

## <a name="monitoring-our-application"></a>監控應用程式

Nginx 已設定完成，可將向 `http://yourhost:80` 提出的要求轉送給在 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。 不過，Nginx 未設定管理 Kestrel 程序。 您可以使用 *systemd* 並建立服務檔案，啟動及監視基礎的 Web 應用程式。 *systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。 

### <a name="create-the-service-file"></a>建立服務檔

建立服務定義檔：

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

以下是我們的應用程式範例服務檔：

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

**注意：**如果您的組態不使用使用者 *www-data*，則必須先建立此處定義的使用者，並指定適當的檔案擁有權。

儲存檔案並啟用服務。

```bash
systemctl enable kestrel-hellomvc.service
```

啟動服務並確認它正在執行。

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

透過 systemd 設定反向 Proxy 及管理 Kestrel，可以完全設定 Web 應用程式，並從位在 `http://localhost` 的本機電腦瀏覽器存取它。 它也可以從遠端電腦存取，阻擋可能執行封鎖的任何防火牆。 檢查回應標頭，`Server` 標頭顯示 Kestrel 正在為 ASP.NET Core 應用程式提供服務。

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>檢視記錄

因為使用 `systemd` 管理使用 Kestrel 的 Web 應用程式，所以所有事件和處理程序都會記錄在集中式日誌中。 不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。 若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>保護應用程式

### <a name="enable-apparmor"></a>啟用 AppArmor

Linux 安全性模組 (LSM) 是一種架構，屬於 Linux 2.6 以後的 Linux 核心。 LSM 支援不同的安全性模組實作。 [AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。 確定已啟用並正確設定 AppArmor。

### <a name="configuring-our-firewall"></a>設定防火牆

關閉所有不在使用中的外部連接埠。 簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。 確認已設定 `ufw` 允許所需任何連接埠上的流量。

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

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>變更 Nginx 回應名稱

編輯 *src/http/ngx_http_header_filter_module.c*：

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>設定選項和組建

規則運算式需要 PCRE 程式庫。 規則運算式用於 ngx_http_rewrite_module 的位置指示詞。 http_ssl_module 新增 HTTPS 通訊協定的支援。

請考慮使用像 *ModSecurity* 這樣的 Web 應用程式防火牆來強化您的應用程式。

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>設定 SSL

* 藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，設定伺服器接聽連接埠 `443` 上的 HTTPS 流量。

* 運用以下 */etc/nginx/nginx.conf* 檔案所述的做法，強化您的安全性。 範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。

* 新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都只會透過 HTTPS。

* 如果未來打算停用 SSL，請不要新增 Strict-Transport-Security 標頭，或選擇適當的 `max-age`。

新增 */etc/nginx/Proxy.conf* 組態檔：

[!code-nginx[Main](linuxproduction/proxy.conf)]

編輯 */etc/nginx/Proxy.conf* 組態檔。 此範例在一個組態檔中同時包含 `http` 和 `server` 區段。

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>保護 Nginx 免於點閱綁架
點閱綁架是收集受感染使用者按一下動作的惡意技術。 點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。 使用 X-FRAME-OPTIONS 保護您的網站。

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
