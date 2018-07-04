---
title: ASP.NET Core 中的網頁伺服器實作
author: rick-anderson
description: 探索 ASP.NET Core 的網頁伺服器 Kestrel 與 HTTP.sys。 了解如何選擇伺服器，以及何時使用反向 Proxy 伺服器。
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: bb0331d7201d4e979e6c6524cbf630280c4eaeb6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274439"
---
# <a name="web-server-implementations-in-aspnet-core"></a>ASP.NET Core 中的網頁伺服器實作

由 [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher) 提供

ASP.NET Core 應用程式執行時，需使用內含式 HTTP 伺服器實作。 伺服器實作會接聽 HTTP 要求，並以組成 [HttpContext](/dotnet/api/system.web.httpcontext) 的[要求功能](xref:fundamentals/request-features)集合形式，將它們呈現給應用程式。

ASP.NET Core 提供兩個伺服器實作：

* [Kestrel](xref:fundamentals/servers/kestrel) 是 ASP.NET Core 的預設跨平台 HTTP 伺服器。
* [HTTP.sys](xref:fundamentals/servers/httpsys) 是以 [HTTP.sys 核心驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)為基礎的僅限 Windows HTTP 伺服器與 HTTP 伺服器 API。 (HTTP.sys 在 ASP.NET Core 1.x 中稱為 [WebListener](xref:fundamentals/servers/weblistener)。)

## <a name="kestrel"></a>Kestrel

Kestrel 是內含於 ASP.NET Core 專案範本中的預設網頁伺服器。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果應用程式只接受來自內部網路的要求，就可單獨使用 Kestrel。

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

如果將應用程式公開到網際網路，Kestrel 必須使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel，如下圖所示：

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

使用反向 Proxy 進行邊緣部署 (公開到網際網路中的流量) 的最重要理由是安全性。 1.x 版的 Kestrel 並沒有可防禦來自網際網路攻擊的重要安全性功能。 這包括但不限於適當的逾時、要求大小限制和同時連線限制。

如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。

---

您無法在沒有 Kestrel 或[自訂伺服器實作](#custom-servers)的情況下使用 IIS、Nginx 或 Apache。 ASP.NET Core 是設計用來在自己的處理序中執行，使其行為可在平台之間保持一致。 IIS、Nginx 和 Apache 會聽寫自己的啟動程序和環境。 若要直接使用這些伺服器技術，ASP.NET Core 必須適應每一部伺服器的需求。 使用 Kestrel 等網頁伺服器實作，可讓 ASP.NET Core 裝載在不同的伺服器技術上時，控制啟動處理序和環境。

### <a name="iis-with-kestrel"></a>IIS 與 Kestrel

當您使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 作為 ASP.NET Core 的反向 Proxy 時，ASP.NET Core 應用程式會在與 IIS 工作者處理序不同的處理序中執行。 在 IIS 處理序中，[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)會協調反向 Proxy 關聯性。 ASP.NET Core Module 的主要功能是要啟動 ASP.NET Core 應用程式、在損毀時進行重新啟動應用程式，以及將 HTTP 流量轉送到應用程式中。 如需詳細資訊，請參閱 [ASP.NET Core模組](xref:fundamentals/servers/aspnet-core-module)。 

### <a name="nginx-with-kestrel"></a>Nginx 與 Kestrel

如需如何在 Linux 上使用 Nginx 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)。

### <a name="apache-with-kestrel"></a>Apache 與 Kestrel

如需如何在 Linux 上使用 Apache 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)。

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果您在 Windows 上執行 ASP.NET Core 應用程式，則 HTTP.sys 是 Kestrel 的替代方案。 通常建議使用 Kestrel 以達到最佳效能。 HTTP.sys 可以用於下列情況：應用程式公開到網際網路，且必要功能是由 HTTP.sys 而非 Kestrel 支援。 如需 HTTP.sys 功能的資訊，請參閱 [HTTP.sys](xref:fundamentals/servers/httpsys)。

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

HTTP.sys 也可用於只公開到內部網路的應用程式。 

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys 在 ASP.NET Core 1.x 中名為 [WebListener](xref:fundamentals/servers/weblistener)。 如果 ASP.NET Core 應用程式是在 Windows 上執行，則在 IIS 無法用來主控應用程式的案例中，WebListener 可作為替代選項。

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

如果必要功能是由 WebListener 而非 Kestrel 支援，對於只公開到內部網路的應用程式，WebListener 也可用來取代 Kestrel。 如需 WebListener 功能的資訊，請參閱 [WebListener](xref:fundamentals/servers/weblistener)。

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a>ASP.NET Core 伺服器基礎結構

可在 `Startup.Configure` 方法中使用的 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會公開 [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 類型的 [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) 屬性。 Kestrel 和 HTTP.sys (ASP.NET Core 1.x 中的 WebListener) 只會個別公開單一功能，[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)，但不同的伺服器實作可能會公開其他的功能。

`IServerAddressesFeature` 可用來找出伺服器實作在執行階段已繫結的連接埠。

## <a name="custom-servers"></a>自訂伺服器

如果內建伺服器不符合應用程式的需求，則可以建立自訂伺服器實作。 [Open Web Interface for .NET (OWIN) 指南](xref:fundamentals/owin)示範如何撰寫以 [Nowin](https://github.com/Bobris/Nowin) 為基礎的 [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 實作。 只有應用程式使用的功能介面需要實作，但至少必須支援 [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) 和 [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature)。

## <a name="server-startup"></a>伺服器啟動

使用 [Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 時，若應用程式是由整合式開發環境 (IDE) 啟動，就會啟動伺服器。 在 Visual Studio on Windows 中，您可以使用啟動設定檔搭配 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module) 或主控台來啟動應用程式和伺服器。 在 Visual Studio Code 中，應用程式和伺服器是由 [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) 啟動，可啟動 CoreCLR 偵錯工具。 若使用 Visual Studio for Mac，則應用程式和伺服器會由 [Mono Soft-Mode 偵錯工具](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/)啟動。

當您在專案資料夾中使用命令提示字元啟動應用程式時，[dotnet run](/dotnet/core/tools/dotnet-run) 會啟動應用程式和伺服器 (僅限 Kestrel 和 HTTP.sys)。 組態是由 `-c|--configuration` 選項指定，會設為 `Debug` (預設值) 或 `Release`。 如果 launchSettings.json 檔案中出現啟動設定檔，請使用 `--launch-profile <NAME>` 選項來設定啟動設定檔 (例如，`Development` 或 `Production`)。 如需詳細資訊，請參閱 [dotnet run](/dotnet/core/tools/dotnet-run) 和 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)主題。

## <a name="additional-resources"></a>其他資源

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Kestrel 與 IIS](xref:fundamentals/servers/aspnet-core-module)
* [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)
* [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (適用於 ASP.NET Core 1.x，請參閱 [WebListener](xref:fundamentals/servers/weblistener))
