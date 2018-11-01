---
title: ASP.NET Core 中的網頁伺服器實作
author: rick-anderson
description: 探索 ASP.NET Core 的網頁伺服器 Kestrel 與 HTTP.sys。 了解如何選擇伺服器，以及何時使用反向 Proxy 伺服器。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 161ab3fdf48e58d8c9af991dc5531e46d9c5adff
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325857"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="a2997-104">ASP.NET Core 中的網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="a2997-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="a2997-105">由 [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher) 提供</span><span class="sxs-lookup"><span data-stu-id="a2997-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="a2997-106">ASP.NET Core 應用程式執行時，需使用內含式 HTTP 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="a2997-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="a2997-107">伺服器實作會接聽 HTTP 要求，並以組成 <xref:Microsoft.AspNetCore.Http.HttpContext> 的[要求功能](xref:fundamentals/request-features)集形式，將它們呈現給應用程式。</span><span class="sxs-lookup"><span data-stu-id="a2997-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="a2997-108">ASP.NET Core 提供三個伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="a2997-108">ASP.NET Core ships three server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a2997-109">[Kestrel](xref:fundamentals/servers/kestrel) 是 ASP.NET Core 的預設跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a2997-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="a2997-110">`IISHttpServer` 可在 Windows 上與[同處理序主控模型](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model)和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)搭配使用。</span><span class="sxs-lookup"><span data-stu-id="a2997-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="a2997-111">[HTTP.sys](xref:fundamentals/servers/httpsys) 是以 [HTTP.sys 核心驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)為基礎的僅限 Windows HTTP 伺服器與 HTTP 伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="a2997-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="a2997-112">(HTTP.sys 在 ASP.NET Core 1.x 中稱為 [WebListener](xref:fundamentals/servers/weblistener)。)</span><span class="sxs-lookup"><span data-stu-id="a2997-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="a2997-113">[Kestrel](xref:fundamentals/servers/kestrel) 是 ASP.NET Core 的預設跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a2997-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="a2997-114">[HTTP.sys](xref:fundamentals/servers/httpsys) 是以 [HTTP.sys 核心驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)為基礎的僅限 Windows HTTP 伺服器與 HTTP 伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="a2997-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="a2997-115">(HTTP.sys 在 ASP.NET Core 1.x 中稱為 [WebListener](xref:fundamentals/servers/weblistener)。)</span><span class="sxs-lookup"><span data-stu-id="a2997-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="a2997-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a2997-116">Kestrel</span></span>

<span data-ttu-id="a2997-117">Kestrel 是內含於 ASP.NET Core 專案範本中的預設網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="a2997-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a2997-118">您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="a2997-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="a2997-119">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a2997-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a2997-122">不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="a2997-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="a2997-123">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="a2997-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a2997-124">如果應用程式只接受來自內部網路的要求，就可單獨使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a2997-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="a2997-126">如果將應用程式公開到網際網路，Kestrel 必須使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。</span><span class="sxs-lookup"><span data-stu-id="a2997-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="a2997-127">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="a2997-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a2997-129">使用反向 Proxy 進行公眾對應 Edge Server 部署 (直接公開到網際網路) 的最重要理由是安全性。</span><span class="sxs-lookup"><span data-stu-id="a2997-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="a2997-130">1.x 版的 Kestrel 並沒有可防禦來自網際網路攻擊的重要安全性功能。</span><span class="sxs-lookup"><span data-stu-id="a2997-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="a2997-131">這包括但不限於適當的逾時、要求大小限制和同時連線限制。</span><span class="sxs-lookup"><span data-stu-id="a2997-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="a2997-132">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="a2997-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="a2997-133">您無法在沒有 Kestrel 或[自訂伺服器實作](#custom-servers)的情況下使用 IIS、Nginx 或 Apache。</span><span class="sxs-lookup"><span data-stu-id="a2997-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="a2997-134">ASP.NET Core 是設計用來在自己的處理序中執行，使其行為可在平台之間保持一致。</span><span class="sxs-lookup"><span data-stu-id="a2997-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="a2997-135">IIS、Nginx 和 Apache 會聽寫自己的啟動程序和環境。</span><span class="sxs-lookup"><span data-stu-id="a2997-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="a2997-136">若要直接使用這些伺服器技術，ASP.NET Core 必須適應每一部伺服器的需求。</span><span class="sxs-lookup"><span data-stu-id="a2997-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="a2997-137">使用 Kestrel 等網頁伺服器實作，可讓 ASP.NET Core 裝載在不同的伺服器技術上時，控制啟動處理序和環境。</span><span class="sxs-lookup"><span data-stu-id="a2997-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="a2997-138">IIS 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a2997-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2997-139">使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，ASP.NET Core 應用程式會在與 IIS 背景工作處理序相同的處理序 (「同處理序」主控模型) 或在與 IIS 背景工作處理序不同的處理序 (「跨處理序」主控模型) 中執行。</span><span class="sxs-lookup"><span data-stu-id="a2997-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="a2997-140">[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)是一種原生 IIS 模組，可處理同處理序 IIS Http 伺服器或跨處理序 Kestrel 伺服器之間的原生 IIS 要求。</span><span class="sxs-lookup"><span data-stu-id="a2997-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="a2997-141">如需詳細資訊，請參閱<xref:fundamentals/servers/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="a2997-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a2997-142">當您使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 作為 ASP.NET Core 的反向 Proxy 時，ASP.NET Core 應用程式會在與 IIS 工作者處理序不同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="a2997-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="a2997-143">在 IIS 處理序中，[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)會協調反向 Proxy 關聯性。</span><span class="sxs-lookup"><span data-stu-id="a2997-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="a2997-144">ASP.NET Core Module 的主要功能是要啟動 ASP.NET Core 應用程式、在損毀時進行重新啟動應用程式，以及將 HTTP 流量轉送到應用程式中。</span><span class="sxs-lookup"><span data-stu-id="a2997-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="a2997-145">如需詳細資訊，請參閱<xref:fundamentals/servers/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="a2997-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="a2997-146">Nginx 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a2997-146">Nginx with Kestrel</span></span>

<span data-ttu-id="a2997-147">如需如何在 Linux 上使用 Nginx 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)。</span><span class="sxs-lookup"><span data-stu-id="a2997-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="a2997-148">Apache 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a2997-148">Apache with Kestrel</span></span>

<span data-ttu-id="a2997-149">如需如何在 Linux 上使用 Apache 作為 Kestrel 之反向 Proxy 伺服器的資訊，請參閱 [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)。</span><span class="sxs-lookup"><span data-stu-id="a2997-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="a2997-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a2997-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a2997-151">如果您在 Windows 上執行 ASP.NET Core 應用程式，則 HTTP.sys 是 Kestrel 的替代方案。</span><span class="sxs-lookup"><span data-stu-id="a2997-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="a2997-152">通常建議使用 Kestrel 以達到最佳效能。</span><span class="sxs-lookup"><span data-stu-id="a2997-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="a2997-153">HTTP.sys 可以用於下列情況：應用程式公開到網際網路，且必要功能是由 HTTP.sys 而非 Kestrel 支援。</span><span class="sxs-lookup"><span data-stu-id="a2997-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="a2997-154">如需 HTTP.sys 的資訊，請參閱 [HTTP.sys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="a2997-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="a2997-156">HTTP.sys 也可用於只公開到內部網路的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a2997-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a2997-158">HTTP.sys 在 ASP.NET Core 1.x 中名為 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="a2997-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="a2997-159">如果 ASP.NET Core 應用程式是在 Windows 上執行，則在 IIS 無法用來主控應用程式的案例中，WebListener 可作為替代選項。</span><span class="sxs-lookup"><span data-stu-id="a2997-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="a2997-161">如果必要功能是由 WebListener 而非 Kestrel 支援，對於只公開到內部網路的應用程式，WebListener 也可用來取代 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a2997-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="a2997-162">如需 WebListener 的資訊，請參閱 [WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="a2997-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="a2997-164">ASP.NET Core 伺服器基礎結構</span><span class="sxs-lookup"><span data-stu-id="a2997-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="a2997-165">可在 `Startup.Configure` 方法中使用的 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會公開 [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 類型的 [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) 屬性。</span><span class="sxs-lookup"><span data-stu-id="a2997-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="a2997-166">Kestrel 和 HTTP.sys (ASP.NET Core 1.x 中的 WebListener) 只會個別公開單一功能，[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)，但不同的伺服器實作可能會公開其他的功能。</span><span class="sxs-lookup"><span data-stu-id="a2997-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="a2997-167">`IServerAddressesFeature` 可用來找出伺服器實作在執行階段已繫結的連接埠。</span><span class="sxs-lookup"><span data-stu-id="a2997-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="a2997-168">自訂伺服器</span><span class="sxs-lookup"><span data-stu-id="a2997-168">Custom servers</span></span>

<span data-ttu-id="a2997-169">如果內建伺服器不符合應用程式的需求，則可以建立自訂伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="a2997-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="a2997-170">[Open Web Interface for .NET (OWIN) 指南](xref:fundamentals/owin)示範如何撰寫以 [Nowin](https://github.com/Bobris/Nowin) 為基礎的 [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 實作。</span><span class="sxs-lookup"><span data-stu-id="a2997-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="a2997-171">只有應用程式使用的功能介面需要實作，但至少必須支援 [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) 和 [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature)。</span><span class="sxs-lookup"><span data-stu-id="a2997-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="a2997-172">伺服器啟動</span><span class="sxs-lookup"><span data-stu-id="a2997-172">Server startup</span></span>

<span data-ttu-id="a2997-173">使用 [Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 時，若應用程式是由整合式開發環境 (IDE) 啟動，就會啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="a2997-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="a2997-174">在 Visual Studio on Windows 中，您可以使用啟動設定檔搭配 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module) 或主控台來啟動應用程式和伺服器。</span><span class="sxs-lookup"><span data-stu-id="a2997-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="a2997-175">在 Visual Studio Code 中，應用程式和伺服器是由 [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) 啟動，可啟動 CoreCLR 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a2997-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="a2997-176">若使用 Visual Studio for Mac，則應用程式和伺服器會由 [Mono Soft-Mode 偵錯工具](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/)啟動。</span><span class="sxs-lookup"><span data-stu-id="a2997-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="a2997-177">當您在專案資料夾中使用命令提示字元啟動應用程式時，[dotnet run](/dotnet/core/tools/dotnet-run) 會啟動應用程式和伺服器 (僅限 Kestrel 和 HTTP.sys)。</span><span class="sxs-lookup"><span data-stu-id="a2997-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="a2997-178">組態是由 `-c|--configuration` 選項指定，會設為 `Debug` (預設值) 或 `Release`。</span><span class="sxs-lookup"><span data-stu-id="a2997-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="a2997-179">如果 launchSettings.json 檔案中出現啟動設定檔，請使用 `--launch-profile <NAME>` 選項來設定啟動設定檔 (例如，`Development` 或 `Production`)。</span><span class="sxs-lookup"><span data-stu-id="a2997-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="a2997-180">如需詳細資訊，請參閱 [dotnet run](/dotnet/core/tools/dotnet-run) 和 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)主題。</span><span class="sxs-lookup"><span data-stu-id="a2997-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="a2997-181">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="a2997-181">HTTP/2 support</span></span>

<span data-ttu-id="a2997-182">在下列部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="a2997-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="a2997-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a2997-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="a2997-184">作業系統</span><span class="sxs-lookup"><span data-stu-id="a2997-184">Operating system</span></span>
    * <span data-ttu-id="a2997-185">Windows Server 2012 R2/Windows 8.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-185">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="a2997-186">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="a2997-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="a2997-187">未來版本的 macOS 將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a2997-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="a2997-188">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="a2997-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a2997-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="a2997-190">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="a2997-191">目標 Framework：不適用於 HTTP.sys 部署。</span><span class="sxs-lookup"><span data-stu-id="a2997-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="a2997-192">IIS (同處理序)</span><span class="sxs-lookup"><span data-stu-id="a2997-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a2997-193">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a2997-194">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="a2997-195">IIS (跨處理序)</span><span class="sxs-lookup"><span data-stu-id="a2997-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a2997-196">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a2997-197">公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a2997-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a2997-198">目標 Framework：不適用於 IIS 跨處理序部署。</span><span class="sxs-lookup"><span data-stu-id="a2997-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="a2997-199">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a2997-199">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="a2997-200">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-200">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="a2997-201">目標 Framework：不適用於 HTTP.sys 部署。</span><span class="sxs-lookup"><span data-stu-id="a2997-201">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="a2997-202">IIS (跨處理序)</span><span class="sxs-lookup"><span data-stu-id="a2997-202">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a2997-203">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a2997-203">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a2997-204">公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a2997-204">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a2997-205">目標 Framework：不適用於 IIS 跨處理序部署。</span><span class="sxs-lookup"><span data-stu-id="a2997-205">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="a2997-206">HTTP/2 連線必須使用 [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 和 TLS 1.2 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="a2997-206">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="a2997-207">如需詳細資訊，請參閱與伺服器部署案例相關的主題。</span><span class="sxs-lookup"><span data-stu-id="a2997-207">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2997-208">其他資源</span><span class="sxs-lookup"><span data-stu-id="a2997-208">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="a2997-209"><xref:fundamentals/servers/httpsys> (若為 ASP.NET Core 1.x，請參閱 <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="a2997-209"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
