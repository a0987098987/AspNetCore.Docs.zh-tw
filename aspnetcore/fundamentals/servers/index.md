---
title: ASP.NET Core 中的網頁伺服器實作
author: rick-anderson
description: 探索 ASP.NET Core 的網頁伺服器 Kestrel 與 HTTP.sys。 了解如何選擇伺服器，以及何時使用反向 Proxy 伺服器。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: c9ed385208df083f631174c7071ca31ed2114350
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="c3871-104">ASP.NET Core 中的網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="c3871-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="c3871-105">由 [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher) 提供</span><span class="sxs-lookup"><span data-stu-id="c3871-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="c3871-106">ASP.NET Core 應用程式執行時，需使用內含式 HTTP 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="c3871-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="c3871-107">伺服器實作會接聽 HTTP 要求，並以組成 [HttpContext](/dotnet/api/system.web.httpcontext) 的[要求功能](xref:fundamentals/request-features)集合形式，將它們呈現給應用程式。</span><span class="sxs-lookup"><span data-stu-id="c3871-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an [HttpContext](/dotnet/api/system.web.httpcontext).</span></span>

<span data-ttu-id="c3871-108">ASP.NET Core 提供兩個伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="c3871-108">ASP.NET Core ships two server implementations:</span></span>

* <span data-ttu-id="c3871-109">[Kestrel](xref:fundamentals/servers/kestrel) 是 ASP.NET Core 的預設跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="c3871-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="c3871-110">[HTTP.sys](xref:fundamentals/servers/httpsys) 是以 [HTTP.sys 核心驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)為基礎的僅限 Windows HTTP 伺服器與 HTTP 伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="c3871-110">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="c3871-111">(HTTP.sys 在 ASP.NET Core 1.x 中稱為 [WebListener](xref:fundamentals/servers/weblistener)。)</span><span class="sxs-lookup"><span data-stu-id="c3871-111">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

## <a name="kestrel"></a><span data-ttu-id="c3871-112">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c3871-112">Kestrel</span></span>

<span data-ttu-id="c3871-113">Kestrel 是內含於 ASP.NET Core 專案範本中的預設網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="c3871-113">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c3871-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c3871-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c3871-115">您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="c3871-115">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="c3871-116">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="c3871-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="c3871-119">不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="c3871-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="c3871-120">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="c3871-120">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c3871-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c3871-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c3871-122">如果應用程式只接受來自內部網路的要求，就可單獨使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="c3871-122">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="c3871-124">如果將應用程式公開到網際網路，Kestrel 必須使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。</span><span class="sxs-lookup"><span data-stu-id="c3871-124">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="c3871-125">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="c3871-125">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="c3871-127">使用反向 Proxy 進行邊緣部署 (公開到網際網路中的流量) 的最重要理由是安全性。</span><span class="sxs-lookup"><span data-stu-id="c3871-127">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="c3871-128">1.x 版的 Kestrel 並沒有可防禦來自網際網路攻擊的重要安全性功能。</span><span class="sxs-lookup"><span data-stu-id="c3871-128">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="c3871-129">這包括但不限於適當的逾時、要求大小限制和同時連線限制。</span><span class="sxs-lookup"><span data-stu-id="c3871-129">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="c3871-130">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="c3871-130">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

<span data-ttu-id="c3871-131">您無法在沒有 Kestrel 或[自訂伺服器實作](#custom-servers)的情況下使用 IIS、Nginx 或 Apache。</span><span class="sxs-lookup"><span data-stu-id="c3871-131">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="c3871-132">ASP.NET Core 是設計用來在自己的處理序中執行，使其行為可在平台之間保持一致。</span><span class="sxs-lookup"><span data-stu-id="c3871-132">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="c3871-133">IIS、Nginx 和 Apache 會聽寫自己的啟動程序和環境。</span><span class="sxs-lookup"><span data-stu-id="c3871-133">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="c3871-134">若要直接使用這些伺服器技術，ASP.NET Core 必須適應每一部伺服器的需求。</span><span class="sxs-lookup"><span data-stu-id="c3871-134">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="c3871-135">使用 Kestrel 等網頁伺服器實作，可讓 ASP.NET Core 裝載在不同的伺服器技術上時，控制啟動處理序和環境。</span><span class="sxs-lookup"><span data-stu-id="c3871-135">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="c3871-136">IIS 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="c3871-136">IIS with Kestrel</span></span>

<span data-ttu-id="c3871-137">當您使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 作為 ASP.NET Core 的反向 Proxy 時，ASP.NET Core 應用程式會在與 IIS 工作者處理序不同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="c3871-137">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="c3871-138">在 IIS 處理序中，[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)會協調反向 Proxy 關聯性。</span><span class="sxs-lookup"><span data-stu-id="c3871-138">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="c3871-139">ASP.NET Core Module 的主要功能是要啟動 ASP.NET Core 應用程式、在損毀時進行重新啟動應用程式，以及將 HTTP 流量轉送到應用程式中。</span><span class="sxs-lookup"><span data-stu-id="c3871-139">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="c3871-140">如需詳細資訊，請參閱 [ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="c3871-140">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="c3871-141">Nginx 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="c3871-141">Nginx with Kestrel</span></span>

<span data-ttu-id="c3871-142">如需如何在 Linux 上使用 Nginx 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)。</span><span class="sxs-lookup"><span data-stu-id="c3871-142">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="c3871-143">Apache 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="c3871-143">Apache with Kestrel</span></span>

<span data-ttu-id="c3871-144">如需如何在 Linux 上使用 Apache 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)。</span><span class="sxs-lookup"><span data-stu-id="c3871-144">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="c3871-145">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c3871-145">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c3871-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c3871-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c3871-147">如果您在 Windows 上執行 ASP.NET Core 應用程式，則 HTTP.sys 是 Kestrel 的替代方案。</span><span class="sxs-lookup"><span data-stu-id="c3871-147">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="c3871-148">通常建議使用 Kestrel 以達到最佳效能。</span><span class="sxs-lookup"><span data-stu-id="c3871-148">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="c3871-149">HTTP.sys 可以用於下列情況：應用程式公開到網際網路，且必要功能是由 HTTP.sys 而非 Kestrel 支援。</span><span class="sxs-lookup"><span data-stu-id="c3871-149">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required features are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="c3871-150">如需 HTTP.sys 功能的資訊，請參閱 [HTTP.sys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="c3871-150">For information on HTTP.sys features, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="c3871-152">HTTP.sys 也可用於只公開到內部網路的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c3871-152">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span> 

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c3871-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c3871-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c3871-155">HTTP.sys 在 ASP.NET Core 1.x 中名為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="c3871-155">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="c3871-156">如果 ASP.NET Core 應用程式是在 Windows 上執行，則在 IIS 無法用來主控應用程式的案例中，WebListener 可作為替代選項。</span><span class="sxs-lookup"><span data-stu-id="c3871-156">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="c3871-158">如果必要功能是由 WebListener 而非 Kestrel 支援，對於只公開到內部網路的應用程式，WebListener 也可用來取代 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="c3871-158">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required features are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="c3871-159">如需 WebListener 功能的資訊，請參閱 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="c3871-159">For information on WebListener features, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="c3871-161">ASP.NET Core 伺服器基礎結構</span><span class="sxs-lookup"><span data-stu-id="c3871-161">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="c3871-162">可在 `Startup.Configure` 方法中使用的 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會公開 [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 類型的 [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) 屬性。</span><span class="sxs-lookup"><span data-stu-id="c3871-162">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="c3871-163">Kestrel 和 HTTP.sys (ASP.NET Core 1.x 中的 WebListener) 只會個別公開單一功能，[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)，但不同的伺服器實作可能會公開其他的功能。</span><span class="sxs-lookup"><span data-stu-id="c3871-163">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="c3871-164">`IServerAddressesFeature` 可用來找出伺服器實作在執行階段已繫結的連接埠。</span><span class="sxs-lookup"><span data-stu-id="c3871-164">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="c3871-165">自訂伺服器</span><span class="sxs-lookup"><span data-stu-id="c3871-165">Custom servers</span></span>

<span data-ttu-id="c3871-166">如果內建伺服器不符合應用程式的需求，則可以建立自訂伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="c3871-166">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="c3871-167">[Open Web Interface for .NET (OWIN) 指南](xref:fundamentals/owin)示範如何撰寫以 [Nowin](https://github.com/Bobris/Nowin) 為基礎的 [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 實作。</span><span class="sxs-lookup"><span data-stu-id="c3871-167">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="c3871-168">只有應用程式使用的功能介面需要實作，但至少必須支援 [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) 和 [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature)。</span><span class="sxs-lookup"><span data-stu-id="c3871-168">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="c3871-169">伺服器啟動</span><span class="sxs-lookup"><span data-stu-id="c3871-169">Server startup</span></span>

<span data-ttu-id="c3871-170">使用 [Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 時，若應用程式是由整合式開發環境 (IDE) 啟動，就會啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="c3871-170">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="c3871-171">在 Visual Studio on Windows 中，您可以使用啟動設定檔搭配 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module) 或主控台來啟動應用程式和伺服器。</span><span class="sxs-lookup"><span data-stu-id="c3871-171">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="c3871-172">在 Visual Studio Code 中，應用程式和伺服器是由 [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) 啟動，可啟動 CoreCLR 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c3871-172">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="c3871-173">若使用 Visual Studio for Mac，則應用程式和伺服器會由 [Mono Soft-Mode 偵錯工具](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/)啟動。</span><span class="sxs-lookup"><span data-stu-id="c3871-173">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="c3871-174">當您在專案資料夾中使用命令提示字元啟動應用程式時，[dotnet run](/dotnet/core/tools/dotnet-run) 會啟動應用程式和伺服器 (僅限 Kestrel 和 HTTP.sys)。</span><span class="sxs-lookup"><span data-stu-id="c3871-174">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="c3871-175">組態是由 `-c|--configuration` 選項指定，會設為 `Debug` (預設值) 或 `Release`。</span><span class="sxs-lookup"><span data-stu-id="c3871-175">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="c3871-176">如果 launchSettings.json 檔案中出現啟動設定檔，請使用 `--launch-profile <NAME>` 選項來設定啟動設定檔 (例如，`Development` 或 `Production`)。</span><span class="sxs-lookup"><span data-stu-id="c3871-176">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="c3871-177">如需詳細資訊，請參閱 [dotnet run](/dotnet/core/tools/dotnet-run) 和 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)主題。</span><span class="sxs-lookup"><span data-stu-id="c3871-177">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c3871-178">其他資源</span><span class="sxs-lookup"><span data-stu-id="c3871-178">Additional resources</span></span>

* [<span data-ttu-id="c3871-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c3871-179">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="c3871-180">Kestrel 與 IIS</span><span class="sxs-lookup"><span data-stu-id="c3871-180">Kestrel with IIS</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="c3871-181">Linux 上使用 Nginx 的主機</span><span class="sxs-lookup"><span data-stu-id="c3871-181">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="c3871-182">Linux 上使用 Apache 的主機</span><span class="sxs-lookup"><span data-stu-id="c3871-182">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* <span data-ttu-id="c3871-183">[HTTP.sys](xref:fundamentals/servers/httpsys) (適用於 ASP.NET Core 1.x，請參閱 [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="c3871-183">[HTTP.sys](xref:fundamentals/servers/httpsys) (for ASP.NET Core 1.x, see [WebListener](xref:fundamentals/servers/weblistener))</span></span>
