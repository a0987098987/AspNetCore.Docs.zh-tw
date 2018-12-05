---
title: ASP.NET Core 中的網頁伺服器實作
author: guardrex
description: 探索 ASP.NET Core 的網頁伺服器 Kestrel 與 HTTP.sys。 了解如何選擇伺服器，以及何時使用反向 Proxy 伺服器。
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861352"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="37298-104">ASP.NET Core 中的網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="37298-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="37298-105">由 [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher) 提供</span><span class="sxs-lookup"><span data-stu-id="37298-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="37298-106">ASP.NET Core 應用程式執行時，需使用內含式 HTTP 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="37298-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="37298-107">伺服器實作會接聽 HTTP 要求，並以組成 <xref:Microsoft.AspNetCore.Http.HttpContext> 的[要求功能](xref:fundamentals/request-features)集形式，將它們呈現給應用程式。</span><span class="sxs-lookup"><span data-stu-id="37298-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="37298-108">Windows</span><span class="sxs-lookup"><span data-stu-id="37298-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="37298-109">ASP.NET Core 隨附下列項目：</span><span class="sxs-lookup"><span data-stu-id="37298-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="37298-110">[Kestrel 伺服器](xref:fundamentals/servers/kestrel)是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="37298-111">IIS HTTP 伺服器 (`IISHttpServer`) 是搭配 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)使用的 [IIS 同處理續伺服器](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model)實作。</span><span class="sxs-lookup"><span data-stu-id="37298-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="37298-112">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)是以 [HTTP.sys 核心驅動程式與 HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) 為基礎的僅限 Windows HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="37298-113">HTTP.sys 在 ASP.NET Core 1.x 中稱為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="37298-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="37298-114">macOS</span><span class="sxs-lookup"><span data-stu-id="37298-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="37298-115">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="37298-116">Linux</span><span class="sxs-lookup"><span data-stu-id="37298-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="37298-117">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="37298-118">Windows</span><span class="sxs-lookup"><span data-stu-id="37298-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="37298-119">ASP.NET Core 隨附下列項目：</span><span class="sxs-lookup"><span data-stu-id="37298-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="37298-120">[Kestrel 伺服器](xref:fundamentals/servers/kestrel)是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="37298-121">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)是以 [HTTP.sys 核心驅動程式與 HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) 為基礎的僅限 Windows HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="37298-122">HTTP.sys 在 ASP.NET Core 1.x 中稱為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="37298-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="37298-123">macOS</span><span class="sxs-lookup"><span data-stu-id="37298-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="37298-124">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="37298-125">Linux</span><span class="sxs-lookup"><span data-stu-id="37298-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="37298-126">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="37298-127">Kestrel</span><span class="sxs-lookup"><span data-stu-id="37298-127">Kestrel</span></span>

<span data-ttu-id="37298-128">Kestrel 是內含於 ASP.NET Core 專案範本中的預設網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="37298-129">Kestrel 的用法有：</span><span class="sxs-lookup"><span data-stu-id="37298-129">Kestrel can be used:</span></span>

* <span data-ttu-id="37298-130">供本身當作直接從網路 (包括網際網路) 處理要求的邊緣伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="37298-131">搭配「反向 Proxy 伺服器」使用，例如 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)。</span><span class="sxs-lookup"><span data-stu-id="37298-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="37298-132">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="37298-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="37298-135">不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="37298-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="37298-136">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="37298-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37298-137">如果應用程式只接受來自內部網路的要求，就可單獨使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="37298-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="37298-139">如果應用程式會向網際網路公開，Kestrel 就必須使用「反向 Proxy 伺服器」，例如 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)。</span><span class="sxs-lookup"><span data-stu-id="37298-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="37298-140">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="37298-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="37298-142">使用反向 Proxy 進行公眾對應 Edge Server 部署 (直接公開到網際網路) 的最重要理由是安全性。</span><span class="sxs-lookup"><span data-stu-id="37298-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="37298-143">1.x 版的 Kestrel 並不包含可防禦網際網路攻擊的重要安全性功能。</span><span class="sxs-lookup"><span data-stu-id="37298-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="37298-144">這包括但不限於適當的逾時、要求大小限制和同時連線限制。</span><span class="sxs-lookup"><span data-stu-id="37298-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="37298-145">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="37298-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="37298-146">IIS 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="37298-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="37298-147">使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，ASP.NET Core 應用程式會在與 IIS 背景工作處理序相同的處理序 (「同處理序」主控模型) 或在與 IIS 背景工作處理序不同的處理序 (「跨處理序」主控模型) 中執行。</span><span class="sxs-lookup"><span data-stu-id="37298-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="37298-148">[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)是一種原生 IIS 模組，可處理同處理序 IIS HTTP 伺服器或跨處理序 Kestrel 伺服器之間的原生 IIS 要求。</span><span class="sxs-lookup"><span data-stu-id="37298-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="37298-149">如需詳細資訊，請參閱<xref:fundamentals/servers/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="37298-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="37298-150">當您使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 作為 ASP.NET Core 的反向 Proxy 時，ASP.NET Core 應用程式會在與 IIS 工作者處理序不同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="37298-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="37298-151">在 IIS 處理序中，[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)會協調反向 Proxy 關聯性。</span><span class="sxs-lookup"><span data-stu-id="37298-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="37298-152">ASP.NET Core 模組的主要功能是啟動應用程式、在應用程式損毀時重新啟動，以及將 HTTP 流量轉送到應用程式。</span><span class="sxs-lookup"><span data-stu-id="37298-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="37298-153">如需詳細資訊，請參閱<xref:fundamentals/servers/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="37298-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="37298-154">Nginx 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="37298-154">Nginx with Kestrel</span></span>

<span data-ttu-id="37298-155">如需如何在 Linux 上使用 Nginx 作為 Kestrel 反向 Proxy 伺服器的資訊，請參閱 <xref:host-and-deploy/linux-nginx>。</span><span class="sxs-lookup"><span data-stu-id="37298-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="37298-156">Apache 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="37298-156">Apache with Kestrel</span></span>

<span data-ttu-id="37298-157">如需如何在 Linux 上使用 Apache 作為 Kestrel 反向 Proxy 伺服器的資訊，請參閱 <xref:host-and-deploy/linux-apache>。</span><span class="sxs-lookup"><span data-stu-id="37298-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="37298-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="37298-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="37298-159">如果您在 Windows 上執行 ASP.NET Core 應用程式，則 HTTP.sys 是 Kestrel 的替代方案。</span><span class="sxs-lookup"><span data-stu-id="37298-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="37298-160">通常建議使用 Kestrel 以達到最佳效能。</span><span class="sxs-lookup"><span data-stu-id="37298-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="37298-161">HTTP.sys 可以用於下列情況：應用程式公開到網際網路，且必要功能是由 HTTP.sys 而非 Kestrel 支援。</span><span class="sxs-lookup"><span data-stu-id="37298-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="37298-162">如需詳細資訊，請參閱<xref:fundamentals/servers/httpsys>。</span><span class="sxs-lookup"><span data-stu-id="37298-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="37298-164">HTTP.sys 也可用於只公開到內部網路的應用程式。</span><span class="sxs-lookup"><span data-stu-id="37298-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37298-166">HTTP.sys 在 ASP.NET Core 1.x 中名為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="37298-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="37298-167">如果 ASP.NET Core 應用程式是在 Windows 上執行，則在 IIS 無法用來主控應用程式的案例中，WebListener 可作為替代選項。</span><span class="sxs-lookup"><span data-stu-id="37298-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="37298-169">如果必要功能是由 WebListener 而非 Kestrel 支援，對於只公開到內部網路的應用程式，WebListener 也可用來取代 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="37298-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="37298-170">如需 WebListener 的資訊，請參閱 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="37298-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="37298-172">ASP.NET Core 伺服器基礎結構</span><span class="sxs-lookup"><span data-stu-id="37298-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="37298-173">可在 `Startup.Configure` 方法中使用的 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會公開 [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 類型的 [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) 屬性。</span><span class="sxs-lookup"><span data-stu-id="37298-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="37298-174">Kestrel 和 HTTP.sys (ASP.NET Core 1.x 中的 WebListener) 只會個別公開單一功能，[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)，但不同的伺服器實作可能會公開其他的功能。</span><span class="sxs-lookup"><span data-stu-id="37298-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="37298-175">`IServerAddressesFeature` 可用來找出伺服器實作在執行階段已繫結的連接埠。</span><span class="sxs-lookup"><span data-stu-id="37298-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="37298-176">自訂伺服器</span><span class="sxs-lookup"><span data-stu-id="37298-176">Custom servers</span></span>

<span data-ttu-id="37298-177">如果內建伺服器不符合應用程式的需求，則可以建立自訂伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="37298-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="37298-178">[Open Web Interface for .NET (OWIN) 指南](xref:fundamentals/owin)示範如何撰寫以 [Nowin](https://github.com/Bobris/Nowin) 為基礎的 [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 實作。</span><span class="sxs-lookup"><span data-stu-id="37298-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="37298-179">只有應用程式使用的功能介面需要實作，但至少必須支援 [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) 和 [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature)。</span><span class="sxs-lookup"><span data-stu-id="37298-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="37298-180">伺服器啟動</span><span class="sxs-lookup"><span data-stu-id="37298-180">Server startup</span></span>

<span data-ttu-id="37298-181">使用 [Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 時，若應用程式是由整合式開發環境 (IDE) 啟動，就會啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="37298-182">在 Visual Studio on Windows 中，您可以使用啟動設定檔搭配 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module) 或主控台來啟動應用程式和伺服器。</span><span class="sxs-lookup"><span data-stu-id="37298-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="37298-183">在 Visual Studio Code 中，應用程式和伺服器是由 [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) 啟動，可啟動 CoreCLR 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="37298-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="37298-184">若使用 Visual Studio for Mac，則應用程式和伺服器會由 [Mono Soft-Mode 偵錯工具](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/)啟動。</span><span class="sxs-lookup"><span data-stu-id="37298-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="37298-185">當您在專案資料夾中使用命令提示字元啟動應用程式時，[dotnet run](/dotnet/core/tools/dotnet-run) 會啟動應用程式和伺服器 (僅限 Kestrel 和 HTTP.sys)。</span><span class="sxs-lookup"><span data-stu-id="37298-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="37298-186">組態是由 `-c|--configuration` 選項指定，會設為 `Debug` (預設值) 或 `Release`。</span><span class="sxs-lookup"><span data-stu-id="37298-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="37298-187">如果 launchSettings.json 檔案中出現啟動設定檔，請使用 `--launch-profile <NAME>` 選項來設定啟動設定檔 (例如，`Development` 或 `Production`)。</span><span class="sxs-lookup"><span data-stu-id="37298-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="37298-188">如需詳細資訊，請參閱 [dotnet run](/dotnet/core/tools/dotnet-run) 和 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)主題。</span><span class="sxs-lookup"><span data-stu-id="37298-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="37298-189">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="37298-189">HTTP/2 support</span></span>

<span data-ttu-id="37298-190">在下列部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="37298-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="37298-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="37298-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="37298-192">作業系統</span><span class="sxs-lookup"><span data-stu-id="37298-192">Operating system</span></span>
    * <span data-ttu-id="37298-193">Windows Server 2016/Windows 10 或更新版本&dagger;</span><span class="sxs-lookup"><span data-stu-id="37298-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="37298-194">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="37298-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="37298-195">未來版本的 macOS 將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="37298-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="37298-196">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="37298-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="37298-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="37298-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="37298-198">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="37298-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="37298-199">目標 Framework：不適用於 HTTP.sys 部署。</span><span class="sxs-lookup"><span data-stu-id="37298-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="37298-200">IIS (同處理序)</span><span class="sxs-lookup"><span data-stu-id="37298-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="37298-201">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="37298-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="37298-202">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="37298-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="37298-203">IIS (跨處理序)</span><span class="sxs-lookup"><span data-stu-id="37298-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="37298-204">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="37298-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="37298-205">公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="37298-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="37298-206">目標 Framework：不適用於 IIS 跨處理序部署。</span><span class="sxs-lookup"><span data-stu-id="37298-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="37298-207">&dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="37298-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="37298-208">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="37298-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="37298-209">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="37298-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="37298-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="37298-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="37298-211">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="37298-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="37298-212">目標 Framework：不適用於 HTTP.sys 部署。</span><span class="sxs-lookup"><span data-stu-id="37298-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="37298-213">IIS (跨處理序)</span><span class="sxs-lookup"><span data-stu-id="37298-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="37298-214">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="37298-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="37298-215">公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="37298-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="37298-216">目標 Framework：不適用於 IIS 跨處理序部署。</span><span class="sxs-lookup"><span data-stu-id="37298-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="37298-217">HTTP/2 連線必須使用 [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 和 TLS 1.2 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="37298-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="37298-218">如需詳細資訊，請參閱與伺服器部署案例相關的主題。</span><span class="sxs-lookup"><span data-stu-id="37298-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37298-219">其他資源</span><span class="sxs-lookup"><span data-stu-id="37298-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="37298-220"><xref:fundamentals/servers/httpsys> (若為 ASP.NET Core 1.x，請參閱 <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="37298-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
