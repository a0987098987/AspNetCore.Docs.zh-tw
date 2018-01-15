---
title: "ASP.NET Core 中的網頁伺服器實作"
author: tdykstra
description: "介紹適用於 ASP.NET Core 的網頁伺服器 Kestrel 和 WebListener。 提供如何選擇網頁伺服器，以及何時搭配使用網頁伺服器與反向 Proxy 伺服器的指引。"
keywords: "ASP.NET Core, IServer, 網頁伺服器, Kestrel, WebListener, 反向 Proxy"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: b149cb316e4266e67d846b8ef8c2c7f2a25ded5c
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>ASP.NET Core 中的網頁伺服器實作

由 [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher) 提供

ASP.NET Core 應用程式執行時，需使用同處理序 HTTP 伺服器實作。 伺服器實作會接聽 HTTP 要求，並以組成 `HttpContext` 的[要求功能](https://docs.microsoft.com/aspnet/core/fundamentals/request-features)集合形式，將它們呈現給應用程式。

ASP.NET Core 提供兩個伺服器實作：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) 是以 [libuv](https://github.com/libuv/libuv) (跨平台非同步 I/O 程式庫) 為基礎的跨平台 HTTP 伺服器。

* [HTTP.sys](httpsys.md) 是以 [Http.Sys 核心驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)為基礎的僅限 Windows HTTP 伺服器。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) 是以 [libuv](https://github.com/libuv/libuv) (跨平台非同步 I/O 程式庫) 為基礎的跨平台 HTTP 伺服器。

* [WebListener](weblistener.md) 是以 [Http.Sys 核心驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)為基礎的僅限 Windows HTTP 伺服器。

---

## <a name="kestrel"></a>Kestrel

Kestrel 是 ASP.NET Core 新專案範本中預設隨附的網頁伺服器。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

如果 Kestrel 只公開到內部網路，則不論組態是否使用反向 Proxy 伺服器，皆可使用。

如需何時搭配使用 Kestrel 與反向 Proxy 的資訊，請參閱 [Kestrel 簡介](kestrel.md)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果應用程式只接受來自內部網路的要求，您可以單獨使用 Kestrel。

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

如果將應用程式公開到網際網路，您必須使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel，如下圖所示。

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

使用反向 Proxy 進行邊緣部署 (公開到網際網路中的流量) 的最重要理由是安全性。 Kestrel 的 1.x 版對於攻擊的防禦並不充足。 這包括但不限於適當的逾時、大小限制和同時連線限制。

如需何時搭配使用 Kestrel 與反向 Proxy 的資訊，請參閱 [Kestrel 簡介](kestrel.md)。

---

您無法在沒有 Kestrel 或[自訂伺服器實作](#custom-servers)的情況下使用 IIS、Nginx 或 Apache。 ASP.NET Core 是設計用來在自己的處理序中執行，使其行為可在平台之間保持一致。 IIS、Nginx 和 Apache 會指定他們自己的啟動處理序和環境；若要直接使用它們，ASP.NET Core 必須進行調整以符合每一個的需求。 使用 Kestrel 等網頁伺服器實作，可讓 ASP.NET Core 控制啟動處理序和環境。 因此，只需要設定 IIS、Nginx 或 Apache 來代理對 Kestrel 的要求，而不必嘗試調整 ASP.NET Core 以符合這些網頁伺服器。 無論您是在哪裡部署，這種安排可讓 `Program.Main` 和`Startup` 類別基本上相同。

### <a name="iis-with-kestrel"></a>IIS 與 Kestrel

當您使用 IIS 或 IIS Express 作為 ASP.NET Core 的反向 Proxy 時，ASP.NET Core 應用程式會在與 IIS 工作者處理序不同的處理序中執行。 在 IIS 處理序中，將執行一個特殊的 IIS 模組來協調反向 Proxy 關聯性。  這是 *ASP.NET Core Module*。 ASP.NET Core Module 的主要功能是要啟動 ASP.NET Core 應用程式、在其損毀時進行重新啟動，以及將 HTTP 流量轉送到其中。 如需詳細資訊，請參閱 [ASP.NET 核心模組](aspnet-core-module.md)。 

### <a name="nginx-with-kestrel"></a>Nginx 與 Kestrel

如需如何在 Linux 上使用 Nginx 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)。

### <a name="apache-with-kestrel"></a>Apache 與 Kestrel

如需如何在 Linux 上使用 Apache 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)。

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果您在 Windows 上執行 ASP.NET Core 應用程式，則 HTTP.sys 是 Kestrel 的替代方案。 針對將應用程式公開到網際網路，且需要 Kestrel 不支援之 HTTP.sys 功能的情況，您可以使用 HTTP.sys。 

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

HTTP.sys 也可用於只公開到內部網路的應用程式。 

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

對於內部網路案例，通常建議使用 Kestrel 以達到最佳效能；但在某些情況下，您可以使用只有 HTTP.sys 才提供的功能。 如需 HTTP.sys 功能的資訊，請參閱 [HTTP.sys](httpsys.md)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

HTTP.sys 在 ASP.NET Core 1.x 中名為 WebListener。 如果在 Windows 上執行 ASP.NET Core 應用程式，對於您要將應用程式公開到網際網路，但卻無法使用 IIS 的情況，WebListener 是您可以使用的替代方案。

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

如果您需要 Kestrel 不支援的 WebListener 功能，對於只公開到內部網路的應用程式，WebListener 也可用來取代 Kestrel。 

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

對於內部網路案例，通常建議使用 Kestrel 以達到最佳效能；但在某些情況下，您可以使用只有 WebListener 才提供的功能。 如需 WebListener 功能的資訊，請參閱 [WebListener](weblistener.md)。

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>ASP.NET Core 伺服器基礎結構的相關注意事項

`Startup` 類別 `Configure` 方法中提供的 [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會公開 [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection) 類型的 `ServerFeatures` 屬性。 Kestrel 和 WebListener 都只公開單一功能 [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)，但不同的伺服器實作可能會公開其他的功能。

`IServerAddressesFeature` 可用來找出伺服器實作在執行階段已繫結的連接埠。

## <a name="custom-servers"></a>自訂伺服器

如果內建伺服器不符合您的需求，您可以建立自訂伺服器實作。 [Open Web Interface for .NET (OWIN) 指南](../owin.md)示範如何撰寫以 [Nowin](https://github.com/Bobris/Nowin) 為基礎的 [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) 實作。 您可以隨意實作應用程式所需的功能介面，但至少必須支援 [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) 和 [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature)。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel 與 IIS](aspnet-core-module.md)
- [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)
- [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel 與 IIS](aspnet-core-module.md)
- [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)
- [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)
- [WebListener](weblistener.md)

---
