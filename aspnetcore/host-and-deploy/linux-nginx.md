---
title: 在 Linux 上使用 Nginx 裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Ubuntu 16.04 上將 Nginx 設定為反向 Proxy，以將 HTTP 流量轉送至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: c936ff9a7aadd21ce99a0c37184ae8cf911c3070
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403972"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>在 Linux 上使用 Nginx 裝載 ASP.NET Core

由 [Sourabh Shirhatti](https://twitter.com/sshirhatti) 提供

本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。 這些指示可能使用較新版本的 Ubuntu，但未經測試。

如需有關 ASP.NET Core 支援之其他 Linux 發行版本的資訊，請參閱 [Linux 上 .NET Core 的必要條件](/dotnet/core/linux-prerequisites)。

> [!NOTE]
> 針對 Ubuntu 14.04，建議使用 *supervisord*作為監視 Kestrel 處理序的解決方案。 在 Ubuntu 14.04 上無法使用 *systemd*。 如需 Ubuntu 14.04 指示，請參閱[本主題前一版本](https://github.com/dotnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。

本指南：

* 將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後方。
* 設定反向 Proxy 伺服器以將要求轉送至 Kestrel 網頁伺服器。
* 確保 Web 應用程式在啟動時以精靈的形式執行。
* 設定程序管理工具以協助重新啟動 Web 應用程式。

## <a name="prerequisites"></a>必要條件

1. 以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器。
1. 在伺服器上安裝 .NET Core 執行階段。
   1. 請造訪[下載 .Net Core 頁面](https://dotnet.microsoft.com/download/dotnet-core)。
   1. 選取最新的非預覽 .NET Core 版本。
   1. 在 [**執行應用程式-運行**時間] 下的表格中，下載最新的非預覽執行時間。
   1. 選取 [Linux**套件管理員指示**] 連結，並遵循 ubuntu 版本的 ubuntu 指示。
1. 現有的 ASP.NET Core 應用程式。

在未來升級共用架構之後的任何時間點，重新開機伺服器所裝載的 ASP.NET Core 應用程式。

## <a name="publish-and-copy-over-the-app"></a>跨應用程式發佈與複製

為[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)設定應用程式。

如果應用程式在本機執行且未設定為進行安全連線 (HTTPS)，請採用下列任一方法：

* 設定應用程式以處理安全的本機連線。 如需詳細資訊，請參閱 [HTTPS 組態](#https-configuration)一節。
* 從 *Properties/launchSettings.json* 檔案中的 `applicationUrl` 屬性移除 `https://localhost:5001` (如果有的話)。

從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將應用程式封裝到可在伺服器上執行的目錄 (例如，*bin/Release/&lt;target_framework_moniker&gt;/publish*)：

```dotnetcli
dotnet publish --configuration Release
```

如果您不想在伺服器上維護 .NET Core 執行階段，應用程式也可以發佈為[獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。

使用整合至組織工作流程的工具 (SCP、SFTP 等等) 將 ASP.NET Core 應用程式複製到伺服器。 Web 應用程式通常可在 *var* 目錄下找到 (例如 *var/www/helloapp*)。

> [!NOTE]
> 在生產環境部署案例中，持續整合工作流程會執行發佈應用程式並將資產複製到伺服器的工作。

測試應用程式：

1. 從命令列執行應用程式：`dotnet <app_assembly>.dll`。
1. 在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 本機上正常運作。

## <a name="configure-a-reverse-proxy-server"></a>設定反向 Proxy 伺服器

反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。 反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。

### <a name="use-a-reverse-proxy-server"></a>使用反向 Proxy 伺服器

Kestrel 非常適用於從 ASP.NET Core 提供動態內容。 不過，Web 服務功能不像 IIS、Apache 或 Nginx 這類伺服器那樣豐富。 反向 Proxy 伺服器可以讓 HTTP 伺服器卸下提供靜態內容、快取要求、壓縮要求及終止 HTTPS 等工作的負擔。 反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。

為達到本指南的目的，使用 Nginx 的單一執行個體。 它會在相同的伺服器上和 HTTP 伺服器一起執行。 您可以根據需求，選擇不同的設定。

因為反向 proxy 會轉送要求，所以請使用[AspNetCore. 來自 microsoft.aspnetcore.HTTPoverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)套件中的[轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。 此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。


[!INCLUDE[](~/includes/ForwardedHeaders.md)]

<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> `Startup.Configure` 呼叫其他中介軟體之前，先叫用頂端的方法。 請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

如果未將任何 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 指定給中介軟體，則要轉送的預設標頭會是 `None`。

在預設情況下，在回送位址 (127.0.0.0/8, [::1]) 上執行的 Proxy (包括標準的本機位址 (127.0.0.1)) 是受信任的。 如果組織內有其他受信任的 Proxy 或網路處理網際網路與網頁伺服器之間的要求，請使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，將其新增至 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> 或 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> 清單。 下列範例會將 IP 位址 10.0.0.100 的受信任 Proxy 伺服器新增至 `Startup.ConfigureServices` 中「轉送的標頭中介軟體」的 `KnownProxies`：

```csharp
// using System.Net;

services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

如需詳細資訊，請參閱 <xref:host-and-deploy/proxy-load-balancer> 。

### <a name="install-nginx"></a>安裝 Nginx

使用 `apt-get` 來安裝 Nginx。 安裝程式建立的 *systemd* init 指令碼，會在系統啟動時將 Nginx 執行為精靈。 請遵循 [Nginx：Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：官方 Debian/Ubuntu 套件) 中適用於 Ubuntu 的安裝指示。

> [!NOTE]
> 如果需要選用的 Nginx 模組，可能要從來源建置 Nginx。

因為 Nginx 是第一次安裝，請透過執行明確啟動它：

```bash
sudo service nginx start
```

確認瀏覽器會顯示 Nginx 的預設登陸頁面。 登陸頁面位於 `http://<server_IP_address>/index.nginx-debian.html`。

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
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

如果應用程式是 Blazor Server 依賴 websocket 的應用程式 SignalR ，請參閱，以 <xref:blazor/host-and-deploy/server#linux-with-nginx> 取得如何設定標頭的相關資訊 `Connection` 。

當沒有任何與 `server_name` 相符的項目時，Nginx 會使用預設伺服器。 如果未定義任何預設伺服器，則設定檔中的第一個伺服器就是預設伺服器。 最佳做法是，在您的設定檔中新增一個會傳回狀態碼 444 的特定預設伺服器。 預設伺服器設定範例如下：

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

使用上述設定檔和預設伺服器時，Nginx 會在連接埠 80 接受主機標頭為 `example.com` 或 `*.example.com` 的公用流量。 不符合這些主機的要求將不會轉送至 Kestrel。 Nginx 會將相符的要求轉送至位於 `http://localhost:5000` 的 Kestrel。 如需詳細資訊，請參閱 [nginx 如何處理要求](https://nginx.org/docs/http/request_processing.html) \(英文\)。 若要變更 Kestrel 的 IP/連接埠，請參閱 [Kestrel：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。

> [!WARNING]
> 如果無法指定適當的 [server_name 指示詞](https://nginx.org/docs/http/server_names.html)，就會讓應用程式暴露在安全性弱點的風險下。 若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。 如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。

建立 Nginx 設定之後，請執行 `sudo nginx -t` 來確認設定檔的語法。 如果設定檔測試成功，請執行 `sudo nginx -s reload` 來強制 Nginx 套用這些變更。

直接在伺服器上執行應用程式：

1. 巡覽至應用程式目錄。
1. 執行應用程式：`dotnet <app_assembly.dll>`，其中 `app_assembly.dll` 是應用程式的組件檔名稱。

如果應用程式在伺服器上執行，但無法透過網際網路回應，請檢查伺服器的防火牆，確認連接埠 80 已開啟。 如果使用的是 Azure Ubuntu VM，請新增啓用輸入連接埠 80 流量的網路安全性群組 (NSG) 規則。 沒有必要啟用輸出連接埠 80 規則，因為啓用輸入規則時會自動授與輸出流量。

應用程式測試完成後，請在命令提示字元以 `Ctrl+C` 關閉應用程式。

## <a name="monitor-the-app"></a>監視應用程式

伺服器已設定完成，可將對 `http://<serveraddress>:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。 不過，並未設定 Nginx 來管理 Kestrel 處理序。 您可以使用 *systemd* 來建立服務檔案，以啟動並監視基礎 Web 應用程式。 *systemd*是 init 系統，提供許多強大的功能來啟動、停止和管理進程。 

### <a name="create-the-service-file"></a>建立服務檔

建立服務定義檔：

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

以下是一個應用程式範例服務檔：

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

在上述範例中，管理服務的使用者是由 `User` 選項指定。 使用者（ `www-data` ）必須存在，且具有應用程式檔案的適當擁有權。

使用 `TimeoutStopSec` 可設定應用程式收到初始中斷訊號之後等待關閉的時間。 如果應用程式在此期間後未關閉，則會發出 SIGKILL 來終止應用程式。 提供不具單位的秒值 (例如 `150`)、時間範圍值 (例如 `2min 30s`) 或 `infinity` (表示停用逾時)。 `TimeoutStopSec` 在管理員設定檔 (*systemd-system.conf*、*system.conf.d*、*systemd-user.conf*、*user.conf.d*) 的預設值為 `DefaultTimeoutStopSec`。 大多數發行版本的預設逾時為 90 秒。

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

Linux 的檔案系統會區分大小寫。 將 ASPNETCORE_ENVIRONMENT 設定為 "Production" 會導致搜尋 *appsettings.Production.json* 設定檔，而不是搜尋 *appsettings.production.json*。

有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。 請使用下列命令來產生要在設定檔中使用並已適當逸出的值：

```console
systemd-escape "<value-to-escape>"
```

::: moniker range=">= aspnetcore-3.0"

環境變數名稱不支援冒號 (`:`) 分隔符號。 請使用雙底線 (`__`) 來取代冒號。 [環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables)會在將環境變數讀入組態時，將雙底線轉換為冒號。 在下列範例中，連接字串索引鍵 `ConnectionStrings:DefaultConnection` 會設定為服務定義檔中的 `ConnectionStrings__DefaultConnection`：

::: moniker-end
::: moniker range="< aspnetcore-3.0"

環境變數名稱不支援冒號 (`:`) 分隔符號。 請使用雙底線 (`__`) 來取代冒號。 [環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)會在將環境變數讀入組態時，將雙底線轉換為冒號。 在下列範例中，連接字串索引鍵 `ConnectionStrings:DefaultConnection` 會設定為服務定義檔中的 `ConnectionStrings__DefaultConnection`：

::: moniker-end

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

儲存檔案並啟用服務。

```bash
sudo systemctl enable kestrel-helloapp.service
```

啟動服務並確認它正在執行。

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

◝ kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
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

### <a name="view-logs"></a>檢視記錄

由於是使用 `systemd` 來管理使用 Kestrel 的 Web 應用程式，因此會將所有事件和處理序都記錄在集中式日誌中。 不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。 若要檢視 `kestrel-helloapp.service` 的特定項目，請使用下列命令：

```bash
sudo journalctl -fu kestrel-helloapp.service
```

如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a>資料保護

[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)由數個 ASP.NET Core[ 中介軟體](xref:fundamentals/middleware/index)使用，包括驗證中介軟體 (例如，cookie 中介軟體) 與跨網站偽造要求 (CSRF) 保護。 即使資料保護 API 並非由使用者程式碼呼叫，仍應設定資料保護，以建立持續密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。 如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。

如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：

* 所有以 Cookie 為基礎的驗證權杖都會失效。
* 當使用者提出下一個要求時，需要再次登入。
* 所有以 Keyring 保護的資料都無法再解密。 這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。

若要設定資料保護來保存及加密金鑰環，請參閱：

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a>要求標頭欄位太長

Proxy 伺服器預設設定通常會將要求標頭欄位限制為 4 K 或 8 K （視平臺而定）。 應用程式可能需要比預設值更長的欄位（例如，使用[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)的應用程式）。 如果需要較長的欄位，proxy 伺服器的預設設定需要調整。 要套用的值取決於案例。 如需詳細資訊，請參閱您的伺服器文件。

* [proxy_buffer_size](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [proxy_buffers](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [proxy_busy_buffers_size](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [large_client_header_buffers](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> 除非必要，否則請勿增加 Proxy 緩衝區的預設值。 增加這些值會提高緩衝區溢位及惡意使用者發動拒絕服務 (DoS) 攻擊的風險。

## <a name="secure-the-app"></a>保護應用程式

### <a name="enable-apparmor"></a>啟用 AppArmor

Linux 安全性模組 (LSM) 是 Linux 2.6 之後 Linux 核心所包含的一個架構。 LSM 支援不同的安全性模組實作。 [AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。 確定已啟用並正確設定 AppArmor。

### <a name="configure-the-firewall"></a>設定防火牆

關閉所有不在使用中的外部連接埠。 簡單的防火牆（ufw）提供用來設定防火牆的 CLI，藉以提供的前端 `iptables` 。

> [!WARNING]
> 如未正確設定，防火牆會禁止存取整個系統。 未指定正確的 SSH 連接埠，將會導致您無法存取系統 (若您使用 SSH 連線至該連接埠)。 預設連接埠為 22。 如需詳細資訊，請參閱 [ufw 簡介](https://help.ubuntu.com/community/UFW)與[手冊](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html)。

安裝 `ufw` 並將其設定為允許任何所需連接埠上的流量。

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a>保護 Nginx

#### <a name="change-the-nginx-response-name"></a>變更 Nginx 回應名稱

編輯 *src/http/ngx_http_header_filter_module.c*：

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>設定選項

設定伺服器的其他所需模組。 請考慮使用 [ModSecurity](https://www.modsecurity.org/) 等 Web 應用程式防火牆來強化應用程式。

#### <a name="https-configuration"></a>HTTPS 設定

**設定應用程式以進行安全的本機連線 (HTTPS)**

[dotnet run](/dotnet/core/tools/dotnet-run) 命令使用應用程式的 *Properties/launchSettings.json* 檔案，其設定應用程式在 `applicationUrl` 屬性所提供的 URL 上接聽 (例如 `https://localhost:5001;http://localhost:5000`)。

使用下列其中一種方法，設定應用程式將憑證用在針對 `dotnet run` 命令的開發，或用在開發環境 (F5，若在 Visual Studio Code 中則為 Ctrl+F5)：

* [取代組態中的預設憑證](xref:fundamentals/servers/kestrel#configuration) (建議使用**)
* [KestrelServerOptions.ConfigureHttpsDefaults](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

**設定反向 Prooxy 以進行安全的用戶端連線 (HTTPS)**

* 藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，將伺服器設定成在連接埠 `443` 上接聽 HTTPS 流量。

* 採用以下 */etc/nginx/nginx.conf* 檔案所述的一些做法來強化安全性。 範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。

* 新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都會透過 HTTPS。

* 如果未來將會停用 HTTPS，請使用下列其中一種方法：

  * 不要新增 HSTS 標頭。
  * 選擇 short `max-age` 值。

新增 */etc/nginx/Proxy.conf* 組態檔：

[!code-nginx[](linux-nginx/proxy.conf)]

編輯 */etc/nginx/Proxy.conf* 組態檔。 此範例在一個組態檔中同時包含 `http` 和 `server` 區段。

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>保護 Nginx 免於點閱綁架

[點閱綁架](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)(也稱為「UI 偽裝攻擊」**) 是一種惡意攻擊，會誘騙網站訪客點選與其目前所瀏覽頁面不同的頁面上連結或按鈕。 請使用 `X-FRAME-OPTIONS` 來保護網站安全。

減輕點擊劫持攻擊：

1. 編輯 *nginx.conf* 檔案：

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   新增 `add_header X-Frame-Options "SAMEORIGIN";` 行。
1. 儲存檔案。
1. 重新啟動 Nginx。

#### <a name="mime-type-sniffing"></a>MIME 類型探查

此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。 使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。

編輯 *nginx.conf* 檔案：

```bash
sudo nano /etc/nginx/nginx.conf
```

新增 `add_header X-Content-Type-Options "nosniff";` 行並儲存檔案，然後重新啟動 Nginx。

## <a name="additional-nginx-suggestions"></a>其他 Nginx 建議

升級伺服器上的共用架構之後，請重新開機伺服器所主控的 ASP.NET Core 應用程式。

## <a name="additional-resources"></a>其他資源

* [Linux 上 .NET Core 的必要條件](/dotnet/core/linux-prerequisites)
* [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：二進位版本：正式的 Debian Ubuntu 套件)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [NGINX：使用轉送的標頭](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
