---
title: ASP.NET Core 中的網頁伺服器實作
author: guardrex
description: 探索 ASP.NET Core 的網頁伺服器 Kestrel 與 HTTP.sys。 了解如何選擇伺服器，以及何時使用反向 Proxy 伺服器。
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/11/2019
uid: fundamentals/servers/index
ms.openlocfilehash: 4210d67397c85a1608f79fc4ed9d283521356226
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249486"
---
# <a name="web-server-implementations-in-aspnet-core"></a>ASP.NET Core 中的網頁伺服器實作

由 [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher) 提供

ASP.NET Core 應用程式執行時，需使用內含式 HTTP 伺服器實作。 伺服器實作會接聽 HTTP 要求，並以組成 <xref:Microsoft.AspNetCore.Http.HttpContext> 的一組[要求功能](xref:fundamentals/request-features)形式向應用程式呈現。

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core 隨附下列項目：

* [Kestrel 伺服器](xref:fundamentals/servers/kestrel)是預設、跨平台的 HTTP 伺服器實作。
* IIS HTTP 伺服器是 IIS 的[同處理序伺服器](#in-process-hosting-model)。
* [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)是以 [HTTP.sys 核心驅動程式與 HTTP 伺服器 API](/windows/desktop/Http/http-api-start-page) 為基礎的僅限 Windows HTTP 伺服器。

使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式可能會執行於：

* 與 IIS 背景工作處理序相同的處理序 ([同處理序裝載模型](#in-process-hosting-model))，並搭配 [IIS HTTP 伺服器](#iis-http-server)。 「同處理序」是建議的設定。
* 從 IIS 背景工作處理序中分離出的處理序 ([跨處理序裝載模型](#out-of-process-hosting-model))，並搭配 [Kestrel 伺服器](#kestrel)。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是一種原生 IIS 模組，可處理 IIS 與同處理序 IIS HTTP 伺服器或 Kestrel 之間的原生 IIS 要求。 如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。

## <a name="hosting-models"></a>裝載模型

### <a name="in-process-hosting-model"></a>同處理序裝載模型

使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。 這消除了透過回送配接器對要求進行 Proxy 處理的跨處理序效能損失。該配接器是一種網路介面，會將連出的網路流量傳回相同機器。 IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。

ASP.NET Core 模組：

* 執行應用程式初始化。
  * 載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。
  * 呼叫 `Program.Main`。
* 處理 IIS 原生要求的存留期。

以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。

下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：

![ASP.NET Core 模組](_static/ancm-inprocess.png)

要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。 驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。 模組會接收原生要求，並將它傳遞至 IIS HTTP 伺服器 (`IISHttpServer`)。 IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。

IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。 中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。 應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的用戶端。

現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。

### <a name="out-of-process-hosting-model"></a>跨處理序裝載模型

因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。 此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。 此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。

下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：

![ASP.NET Core 模組](_static/ancm-outofprocess.png)

要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。 驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。 此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。

此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{PORT}`。 將會執行額外檢查，不是源自模組的要求都會遭到拒絕。 此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。

Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。 中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。 IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。 應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。

如需 IIS 和 ASP.NET Core 模組的設定指南，請參閱下列主題：

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core 隨附下列項目：

* [Kestrel 伺服器](xref:fundamentals/servers/kestrel)是預設、跨平台的 HTTP 伺服器。
* [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)是以 [HTTP.sys 核心驅動程式與 HTTP 伺服器 API](/windows/desktop/Http/http-api-start-page) 為基礎的僅限 Windows HTTP 伺服器。

在使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式會執行於從 IIS 背景工作處理序中分離出的處理序 (跨處理序)，並搭配 [Kestrel 伺服器](#kestrel)。

因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。 此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。 此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。

下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：

![ASP.NET Core 模組](_static/ancm-outofprocess.png)

要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。 驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。 此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。

此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。 將會執行額外檢查，不是源自模組的要求都會遭到拒絕。 此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。

Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。 中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。 IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。 應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。

如需 IIS 和 ASP.NET Core 模組的設定指南，請參閱下列主題：

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。

---

::: moniker-end

## <a name="kestrel"></a>Kestrel

Kestrel 是內含於 ASP.NET Core 專案範本中的預設網頁伺服器。

::: moniker range=">= aspnetcore-2.0"

Kestrel 的用法有：

* 供本身當作直接從網路 (包括網際網路) 處理要求的邊緣伺服器。

  ![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

* 搭配「反向 Proxy 伺服器」使用，例如 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。

  ![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

不論裝載設定是否具有反向 Proxy 伺服器，ASP.NET Core 2.1 或更新版的應用程式均予以支援。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

如果應用程式只接受來自內部網路的要求，就可單獨使用 Kestrel。

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

如果應用程式會向網際網路公開，Kestrel 就必須使用「反向 Proxy 伺服器」，例如 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

使用反向 Proxy 進行公眾對應 Edge Server 部署 (直接公開到網際網路) 的最重要理由是安全性。 1.x 版的 Kestrel 並不包含可防禦網際網路攻擊的重要安全性功能。 這包括但不限於適當的逾時、要求大小限制和同時連線限制。

::: moniker-end

如需 Kestrel 設定指南及資訊，以了解在反向 Proxy 設定中使用 Kestrel 的時機，請參閱 <xref:fundamentals/servers/kestrel>。

### <a name="nginx-with-kestrel"></a>Nginx 與 Kestrel

如需如何在 Linux 上使用 Nginx 作為 Kestrel 反向 Proxy 伺服器的資訊，請參閱 <xref:host-and-deploy/linux-nginx>。

### <a name="apache-with-kestrel"></a>Apache 與 Kestrel

如需如何在 Linux 上使用 Apache 作為 Kestrel 反向 Proxy 伺服器的資訊，請參閱 <xref:host-and-deploy/linux-apache>。

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a>IIS HTTP 伺服器

IIS HTTP 伺服器是 IIS 的[同處理序伺服器](#in-process-hosting-model)，對於同處理序部署不可或缺。 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)會處理 IIS 與 IIS HTTP 伺服器之間的原生 IIS 要求。 如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

如果您在 Windows 上執行 ASP.NET Core 應用程式，則 HTTP.sys 是 Kestrel 的替代方案。 通常建議使用 Kestrel 以達到最佳效能。 HTTP.sys 可以用於下列情況：應用程式公開到網際網路，且必要功能是由 HTTP.sys 而非 Kestrel 支援。 如需詳細資訊，請參閱<xref:fundamentals/servers/httpsys>。

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

HTTP.sys 也可用於只公開到內部網路的應用程式。

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

如需 HTTP.sys 設定指南，請參閱 <xref:fundamentals/servers/httpsys>。

## <a name="aspnet-core-server-infrastructure"></a>ASP.NET Core 伺服器基礎結構

可在 `Startup.Configure` 方法中使用的 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會公開 [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 類型的 [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) 屬性。 Kestrel 和 HTTP.sys 只會個別公開單一功能 [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)，但不同的伺服器實作可能會公開其他的功能。

`IServerAddressesFeature` 可用來找出伺服器實作在執行階段已繫結的連接埠。

## <a name="custom-servers"></a>自訂伺服器

如果內建伺服器不符合應用程式的需求，則可以建立自訂伺服器實作。 [Open Web Interface for .NET (OWIN) 指南](xref:fundamentals/owin)示範如何撰寫以 [Nowin](https://github.com/Bobris/Nowin) 為基礎的 [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 實作。 只有應用程式使用的功能介面需要實作，但至少必須支援 [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) 和 [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature)。

## <a name="server-startup"></a>伺服器啟動

伺服器會在整合式開發環境 (IDE) 或編輯器啟動應用程式時啟動：

* [Visual Studio](https://www.visualstudio.com/vs/) &ndash; 您可以使用啟動設定檔搭配 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)或主控台來啟動應用程式和伺服器。
* [Visual Studio Code](https://code.visualstudio.com/) &ndash; 應用程式和伺服器會由 [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) 啟動，這也可啟動 CoreCLR 偵錯工具。
* [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; 應用程式和伺服器會由 [Mono Soft-Mode 偵錯工具](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/)啟動。

當您在專案資料夾中使用命令提示字元啟動應用程式時，[dotnet run](/dotnet/core/tools/dotnet-run) 會啟動應用程式和伺服器 (僅限 Kestrel 和 HTTP.sys)。 組態是由 `-c|--configuration` 選項指定，會設為 `Debug` (預設值) 或 `Release`。 如果 launchSettings.json 檔案中出現啟動設定檔，請使用 `--launch-profile <NAME>` 選項來設定啟動設定檔 (例如，`Development` 或 `Production`)。 如需詳細資訊，請參閱 [dotnet run](/dotnet/core/tools/dotnet-run) 和 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)。

## <a name="http2-support"></a>HTTP/2 支援

在下列部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * 作業系統
    * Windows Server 2016/Windows 10 或更新版本&dagger;
    * Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)
    * 未來版本的 macOS 將會支援 HTTP/2。
  * 目標 Framework：.NET Core 2.2 或更新版本
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 或更新版本
  * 目標 Framework:不適用於 HTTP.sys 部署。
* [IIS (同處理序)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本
  * 目標 Framework：.NET Core 2.2 或更新版本
* [IIS (跨處理序)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本
  * 公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。
  * 目標 Framework:不適用於 IIS 跨處理序部署。

&dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。 支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。 可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 或更新版本
  * 目標 Framework:不適用於 HTTP.sys 部署。
* [IIS (跨處理序)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本
  * 公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。
  * 目標 Framework:不適用於 IIS 跨處理序部署。

::: moniker-end

HTTP/2 連線必須使用 [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 和 TLS 1.2 或更新版本。 如需詳細資訊，請參閱與伺服器部署案例相關的主題。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
