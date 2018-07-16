---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: rick-anderson
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 62649351271deebcf1ed9d2f8b2258bed3478989
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126939"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="6c121-103">ASP.NET Core 中的 Kestrel 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="6c121-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="6c121-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="6c121-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="6c121-105">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="6c121-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="6c121-106">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="6c121-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="6c121-107">Kestrel 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="6c121-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="6c121-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6c121-108">HTTPS</span></span>
* <span data-ttu-id="6c121-109">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="6c121-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="6c121-110">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="6c121-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="6c121-111">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="6c121-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="6c121-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c121-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="6c121-113">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="6c121-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c121-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c121-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6c121-115">您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="6c121-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="6c121-116">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="6c121-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6c121-119">不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="6c121-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c121-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c121-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6c121-121">如果應用程式只接受來自內部網路的要求，就可直接使用 Kestrel 作為應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="6c121-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="6c121-123">如果將應用程式公開到網際網路，請使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。</span><span class="sxs-lookup"><span data-stu-id="6c121-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="6c121-124">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="6c121-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6c121-126">基於安全性考量，必須有反向 Proxy 才能進行邊緣部署 (公開到網際網路中的流量)。</span><span class="sxs-lookup"><span data-stu-id="6c121-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="6c121-127">Kestrel 的 1.x 版沒有針對攻擊的完整防禦補充，例如適當的逾時、大小上限，以及並行連線限制。</span><span class="sxs-lookup"><span data-stu-id="6c121-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="6c121-128">反向 Proxy 存在的情況如下：有多個在單一伺服器上執行的應用程式共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="6c121-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="6c121-129">Kestrel 不支援這種情況，因為 Kestrel 不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="6c121-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="6c121-130">當 Kestrel 設定為接聽通訊埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的主機標頭。</span><span class="sxs-lookup"><span data-stu-id="6c121-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="6c121-131">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="6c121-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="6c121-132">即使不需要反向 Proxy 伺服器，反向 Proxy 伺服器也是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="6c121-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="6c121-133">它可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="6c121-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="6c121-134">它提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="6c121-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="6c121-135">它能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="6c121-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="6c121-136">它簡化負載平衡和 SSL 組態。</span><span class="sxs-lookup"><span data-stu-id="6c121-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="6c121-137">只有反向 Proxy 伺服器需要 SSL 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="6c121-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="6c121-138">如果未使用反向 Proxy 並啟用主機篩選，則必須啟用[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="6c121-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="6c121-139">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="6c121-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c121-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c121-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="6c121-141">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) 套件包含在 [Microsoft.AspNetCore.App metapackage] 中 (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="6c121-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="6c121-142">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="6c121-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="6c121-143">在 *Program.cs* 中，範本程式碼會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)，而後者會呼叫場景背後的 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel)。</span><span class="sxs-lookup"><span data-stu-id="6c121-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c121-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c121-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="6c121-145">安裝 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="6c121-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="6c121-146">在 `Main` 方法中，於 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) 上呼叫 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) 擴充方法，並指定需要的任何 [Kestrel 選項](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)，如下一節所示。</span><span class="sxs-lookup"><span data-stu-id="6c121-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="6c121-147">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="6c121-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c121-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c121-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="6c121-149">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="6c121-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="6c121-150">可以自訂幾個重要限制：</span><span class="sxs-lookup"><span data-stu-id="6c121-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="6c121-151">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="6c121-151">Maximum client connections</span></span>
* <span data-ttu-id="6c121-152">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="6c121-152">Maximum request body size</span></span>
* <span data-ttu-id="6c121-153">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="6c121-153">Minimum request body data rate</span></span>

<span data-ttu-id="6c121-154">請在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 類別的 [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) 屬性中設定這些和其他條件約束。</span><span class="sxs-lookup"><span data-stu-id="6c121-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="6c121-155">`Limits` 屬性會保留 [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6c121-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="6c121-156">**用戶端連線數目上限**</span><span class="sxs-lookup"><span data-stu-id="6c121-156">**Maximum client connections**</span></span>

[<span data-ttu-id="6c121-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="6c121-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="6c121-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="6c121-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="6c121-159">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="6c121-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="6c121-160">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="6c121-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="6c121-161">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="6c121-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="6c121-162">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="6c121-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="6c121-163">**要求主體大小上限**</span><span class="sxs-lookup"><span data-stu-id="6c121-163">**Maximum request body size**</span></span>

[<span data-ttu-id="6c121-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="6c121-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="6c121-165">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="6c121-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="6c121-166">要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 屬性：</span><span class="sxs-lookup"><span data-stu-id="6c121-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="6c121-167">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="6c121-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="6c121-168">您可以在中介軟體中覆寫特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="6c121-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="6c121-169">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6c121-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="6c121-170">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="6c121-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="6c121-171">**要求主體資料速率下限**</span><span class="sxs-lookup"><span data-stu-id="6c121-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="6c121-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="6c121-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="6c121-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="6c121-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="6c121-174">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="6c121-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="6c121-175">如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="6c121-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="6c121-176">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="6c121-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="6c121-177">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="6c121-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="6c121-178">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="6c121-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="6c121-179">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="6c121-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="6c121-180">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="6c121-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="6c121-181">您可以在中介軟體中設定每個要求的速率：</span><span class="sxs-lookup"><span data-stu-id="6c121-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="6c121-182">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="6c121-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="6c121-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="6c121-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="6c121-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="6c121-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="6c121-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="6c121-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c121-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c121-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="6c121-187">如需 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="6c121-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="6c121-188">KestrelServerOptions 類別</span><span class="sxs-lookup"><span data-stu-id="6c121-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="6c121-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="6c121-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="6c121-190">端點組態</span><span class="sxs-lookup"><span data-stu-id="6c121-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c121-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c121-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="6c121-192">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="6c121-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="6c121-193">在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 上呼叫 [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 或 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 方法，以設定 Kestrel 的 URL 前置詞和連接埠。</span><span class="sxs-lookup"><span data-stu-id="6c121-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="6c121-194">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制。</span><span class="sxs-lookup"><span data-stu-id="6c121-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="6c121-195">`urls` 主機組態索引鍵必須來自主機組態，而不是應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="6c121-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="6c121-196">將 `urls` 索引鍵和值新增至 *appsettings.json* 並不會影響主機組態，因為在從組態檔讀取組態時，主機已完全初始化。</span><span class="sxs-lookup"><span data-stu-id="6c121-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="6c121-197">不過，*appsettings.json* 中的 `urls` 索引鍵可以搭配主機產生器上的 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 來設定主機：</span><span class="sxs-lookup"><span data-stu-id="6c121-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c121-198">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="6c121-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="6c121-199">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="6c121-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="6c121-200">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="6c121-200">A development certificate is created:</span></span>

* <span data-ttu-id="6c121-201">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="6c121-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="6c121-202">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-202">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="6c121-203">有些瀏覽器需要您授與明確的權限給瀏覽器，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="6c121-204">ASP.NET Core 2.1 和更新版本的專案範本會將應用程式設定為預設於 HTTPS 上執行，並包含 [HTTPS 重新導向和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="6c121-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="6c121-205">在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 上呼叫 [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 或 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 方法，以設定 Kestrel 的 URL 前置詞和連接埠。</span><span class="sxs-lookup"><span data-stu-id="6c121-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="6c121-206">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="6c121-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="6c121-207">ASP.NET Core 2.1 `KestrelServerOptions` 組態：</span><span class="sxs-lookup"><span data-stu-id="6c121-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="6c121-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="6c121-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="6c121-209">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="6c121-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="6c121-210">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="6c121-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="6c121-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="6c121-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="6c121-212">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="6c121-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="6c121-213">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="6c121-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="6c121-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="6c121-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="6c121-215">建立組態載入器，設定採用 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="6c121-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="6c121-216">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="6c121-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="6c121-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="6c121-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="6c121-218">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="6c121-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="6c121-219">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="6c121-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="6c121-220">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="6c121-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="6c121-221">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6c121-221">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="6c121-222">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="6c121-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="6c121-223">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="6c121-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="6c121-224">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="6c121-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="6c121-225">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="6c121-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="6c121-226">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="6c121-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="6c121-227">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="6c121-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="6c121-228">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="6c121-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="6c121-229">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="6c121-230">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="6c121-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="6c121-231">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="6c121-232">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="6c121-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="6c121-233">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="6c121-234">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="6c121-234">Supported configurations described next:</span></span>

* <span data-ttu-id="6c121-235">無組態</span><span class="sxs-lookup"><span data-stu-id="6c121-235">No configuration</span></span>
* <span data-ttu-id="6c121-236">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="6c121-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="6c121-237">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="6c121-237">Change the defaults in code</span></span>

<span data-ttu-id="6c121-238">*無組態*</span><span class="sxs-lookup"><span data-stu-id="6c121-238">*No configuration*</span></span>

<span data-ttu-id="6c121-239">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="6c121-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="6c121-240">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="6c121-240">Specify URLs using the:</span></span>

* <span data-ttu-id="6c121-241">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="6c121-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="6c121-242">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="6c121-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="6c121-243">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6c121-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="6c121-244">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6c121-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="6c121-245">如需詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="6c121-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="6c121-246">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="6c121-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="6c121-247">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="6c121-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="6c121-248">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="6c121-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="6c121-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 組態。</span><span class="sxs-lookup"><span data-stu-id="6c121-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="6c121-250">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="6c121-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="6c121-251">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="6c121-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="6c121-252">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="6c121-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="6c121-253">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="6c121-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="6c121-254">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="6c121-255">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="6c121-256">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="6c121-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="6c121-257">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="6c121-257">Schema notes:</span></span>

* <span data-ttu-id="6c121-258">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="6c121-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="6c121-259">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="6c121-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="6c121-260">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="6c121-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="6c121-261">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="6c121-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="6c121-262">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="6c121-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="6c121-263">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="6c121-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="6c121-264">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="6c121-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="6c121-265">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="6c121-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="6c121-266">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="6c121-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="6c121-267">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="6c121-268">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="6c121-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="6c121-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, options => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="6c121-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="6c121-270">您也可以直接存取 `KestrelServerOptions.ConfigurationLoader` 來保持反覆運算現有的載入器，例如 [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="6c121-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="6c121-271">每個端點的組態區段可用於 `Endpoint` 方法的選項，因此可讀取自訂組態。</span><span class="sxs-lookup"><span data-stu-id="6c121-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="6c121-272">可以藉由使用另一個區段再次呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="6c121-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="6c121-273">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="6c121-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="6c121-274">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="6c121-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="6c121-275">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="6c121-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="6c121-276">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="6c121-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="6c121-277">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="6c121-277">*Change the defaults in code*</span></span>

<span data-ttu-id="6c121-278">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="6c121-279">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="6c121-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="6c121-280">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="6c121-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="6c121-281">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="6c121-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="6c121-282">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="6c121-283">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="6c121-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="6c121-284">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="6c121-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="6c121-285">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="6c121-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="6c121-286">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="6c121-286">SNI support requires:</span></span>

* <span data-ttu-id="6c121-287">在目標 Framework `netcoreapp2.1` 上執行。</span><span class="sxs-lookup"><span data-stu-id="6c121-287">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="6c121-288">在 `netcoreapp2.0` 和 `net461` 上，會叫用回呼，但 `name` 一律為 `null`。</span><span class="sxs-lookup"><span data-stu-id="6c121-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="6c121-289">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="6c121-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="6c121-290">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="6c121-290">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="6c121-291">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="6c121-291">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

<span data-ttu-id="6c121-292">**繫結至 TCP 通訊端**</span><span class="sxs-lookup"><span data-stu-id="6c121-292">**Bind to a TCP socket**</span></span>

<span data-ttu-id="6c121-293">[Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 方法繫結至 TCP 通訊端，而選項 Lambda 則允許 SSL 憑證組態：</span><span class="sxs-lookup"><span data-stu-id="6c121-293">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="6c121-294">範例使用 [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) 來設定端點的 SSL。</span><span class="sxs-lookup"><span data-stu-id="6c121-294">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="6c121-295">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="6c121-295">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="6c121-296">**繫結至 Unix 通訊端**</span><span class="sxs-lookup"><span data-stu-id="6c121-296">**Bind to a Unix socket**</span></span>

<span data-ttu-id="6c121-297">使用 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 接聽 UNIX 通訊端以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="6c121-297">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="6c121-298">**連接埠 0**</span><span class="sxs-lookup"><span data-stu-id="6c121-298">**Port 0**</span></span>

<span data-ttu-id="6c121-299">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="6c121-299">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="6c121-300">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="6c121-300">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="6c121-301">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="6c121-301">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

<span data-ttu-id="6c121-302">**UseUrls、-url 命令列引數、urls 主機組態索引鍵和 ASPNETCORE_URLS 環境變數限制**</span><span class="sxs-lookup"><span data-stu-id="6c121-302">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="6c121-303">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="6c121-303">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="6c121-304">UseUrls</span><span class="sxs-lookup"><span data-stu-id="6c121-304">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="6c121-305">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="6c121-305">`--urls` command-line argument</span></span>
* <span data-ttu-id="6c121-306">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="6c121-306">`urls` host configuration key</span></span>
* <span data-ttu-id="6c121-307">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="6c121-307">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="6c121-308">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="6c121-308">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="6c121-309">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="6c121-309">However, be aware of these limitations:</span></span>

* <span data-ttu-id="6c121-310">SSL 無法搭配這些方法使用，除非在 HTTPS 端點組態中提供預設憑證 (例如，使用 `KestrelServerOptions` 組態或組態檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="6c121-310">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="6c121-311">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="6c121-311">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="6c121-312">**IIS 端點組態**</span><span class="sxs-lookup"><span data-stu-id="6c121-312">**IIS endpoint configuration**</span></span>

<span data-ttu-id="6c121-313">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="6c121-313">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="6c121-314">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="6c121-314">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c121-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c121-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="6c121-316">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="6c121-316">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="6c121-317">使用下列各項設定 URL 前置詞和 Kestrel 的連接埠：</span><span class="sxs-lookup"><span data-stu-id="6c121-317">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="6c121-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) 擴充方法</span><span class="sxs-lookup"><span data-stu-id="6c121-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="6c121-319">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="6c121-319">`--urls` command-line argument</span></span>
* <span data-ttu-id="6c121-320">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="6c121-320">`urls` host configuration key</span></span>
* <span data-ttu-id="6c121-321">ASP.NET Core 組態系統，包括 `ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="6c121-321">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="6c121-322">如需這些方法的詳細資訊，請參閱[裝載](xref:fundamentals/host/index)。</span><span class="sxs-lookup"><span data-stu-id="6c121-322">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="6c121-323">**IIS 端點組態**</span><span class="sxs-lookup"><span data-stu-id="6c121-323">**IIS endpoint configuration**</span></span>

<span data-ttu-id="6c121-324">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="6c121-324">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="6c121-325">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="6c121-325">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="6c121-326">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="6c121-326">Transport configuration</span></span>

<span data-ttu-id="6c121-327">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="6c121-327">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="6c121-328">對於升級到 2.1 的 ASP.NET Core 2.0 應用程式而言，這是一項重大變更，它們會呼叫 [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) 並相依於下列任一套件：</span><span class="sxs-lookup"><span data-stu-id="6c121-328">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="6c121-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="6c121-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="6c121-330">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="6c121-330">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="6c121-331">對於使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)且需要使用 Libuv 的 ASP.NET Core 2.1 或更新版本專案：</span><span class="sxs-lookup"><span data-stu-id="6c121-331">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="6c121-332">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="6c121-332">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="6c121-333">呼叫 [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv)：</span><span class="sxs-lookup"><span data-stu-id="6c121-333">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="6c121-334">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="6c121-334">URL prefixes</span></span>

<span data-ttu-id="6c121-335">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="6c121-335">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c121-336">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6c121-336">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="6c121-337">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="6c121-337">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="6c121-338">當使用 `UseUrls` 設定 URL 繫結時，Kestrel 不支援 SSL。</span><span class="sxs-lookup"><span data-stu-id="6c121-338">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="6c121-339">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-339">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="6c121-340">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="6c121-340">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="6c121-341">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-341">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="6c121-342">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="6c121-342">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="6c121-343">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-343">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="6c121-344">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="6c121-344">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="6c121-345">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="6c121-345">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="6c121-346">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="6c121-346">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="6c121-347">如果未使用反向 Proxy 並啟用主機篩選，請啟用[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="6c121-347">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="6c121-348">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-348">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="6c121-349">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="6c121-349">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="6c121-350">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="6c121-350">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="6c121-351">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="6c121-351">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c121-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c121-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="6c121-353">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-353">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="6c121-354">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="6c121-354">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="6c121-355">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-355">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="6c121-356">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="6c121-356">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="6c121-357">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-357">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="6c121-358">主機名稱、`*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="6c121-358">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="6c121-359">不是可辨識的 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="6c121-359">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="6c121-360">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [WebListener](xref:fundamentals/servers/weblistener) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="6c121-360">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="6c121-361">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="6c121-361">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="6c121-362">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="6c121-362">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="6c121-363">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="6c121-363">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="6c121-364">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="6c121-364">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="6c121-365">UNIX 通訊端</span><span class="sxs-lookup"><span data-stu-id="6c121-365">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="6c121-366">**連接埠 0**</span><span class="sxs-lookup"><span data-stu-id="6c121-366">**Port 0**</span></span>

<span data-ttu-id="6c121-367">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="6c121-367">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="6c121-368">除了 `localhost`，任何主機名稱或 IP 都允許繫結至連接埠 `0`。</span><span class="sxs-lookup"><span data-stu-id="6c121-368">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="6c121-369">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="6c121-369">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="6c121-370">**SSL 的 URL 前置詞**</span><span class="sxs-lookup"><span data-stu-id="6c121-370">**URL prefixes for SSL**</span></span>

<span data-ttu-id="6c121-371">如果呼叫 `UseHttps` 擴充方法，請務必使用 `https:` 包含 URL 前置詞：</span><span class="sxs-lookup"><span data-stu-id="6c121-371">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> <span data-ttu-id="6c121-372">HTTPS 和 HTTP 無法裝載在相同的連接埠上。</span><span class="sxs-lookup"><span data-stu-id="6c121-372">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="6c121-373">主機篩選</span><span class="sxs-lookup"><span data-stu-id="6c121-373">Host filtering</span></span>

<span data-ttu-id="6c121-374">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="6c121-374">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="6c121-375">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="6c121-375">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="6c121-376">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="6c121-376">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="6c121-377">此資訊完全不用來驗證要求 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="6c121-377">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c121-378">因應措施是反向 Proxy 後方的主機使用主機標頭篩選。</span><span class="sxs-lookup"><span data-stu-id="6c121-378">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="6c121-379">這是 Kestrel 在 ASP.NET Core 1.x 中唯一支援的案例。</span><span class="sxs-lookup"><span data-stu-id="6c121-379">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c121-380">因應措施是使用中介軟體，依 `Host` 標頭篩選要求：</span><span class="sxs-lookup"><span data-stu-id="6c121-380">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="6c121-381">在 `Startup.Configure` 中註冊前面的 `HostFilteringMiddleware`。</span><span class="sxs-lookup"><span data-stu-id="6c121-381">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="6c121-382">請注意，[中介軟體註冊的順序](xref:fundamentals/middleware/index#ordering)很重要。</span><span class="sxs-lookup"><span data-stu-id="6c121-382">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="6c121-383">註冊應該在註冊中介軟體註冊 (例如 `app.UseExceptionHandler`) 之後立即發生。</span><span class="sxs-lookup"><span data-stu-id="6c121-383">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="6c121-384">中介軟體預期在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中有 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6c121-384">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="6c121-385">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="6c121-385">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c121-386">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6c121-386">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="6c121-387">主機篩選中介軟體係由 [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) 套件提供，隨附於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="6c121-387">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="6c121-388">中介軟體是由 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 新增，它會呼叫 [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering)：</span><span class="sxs-lookup"><span data-stu-id="6c121-388">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="6c121-389">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6c121-389">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="6c121-390">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6c121-390">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="6c121-391">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="6c121-391">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c121-392">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="6c121-392">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="6c121-393">[轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) 選項。</span><span class="sxs-lookup"><span data-stu-id="6c121-393">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="6c121-394">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="6c121-394">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="6c121-395">當不保留主機標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="6c121-395">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="6c121-396">當使用 Kestrel 作為邊緣伺服器，或直接轉送主機標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="6c121-396">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="6c121-397">如需轉送標頭中介軟體的詳細資訊，請參閱[設定 ASP.NET Core 以與 Proxy 伺服器和負載平衡器搭配運作](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="6c121-397">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6c121-398">其他資源</span><span class="sxs-lookup"><span data-stu-id="6c121-398">Additional resources</span></span>

* [<span data-ttu-id="6c121-399">強制使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="6c121-399">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="6c121-400">Kestrel 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="6c121-400">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="6c121-401">RFC 7230：訊息語法和路由 (第 5.4 節：主機)</span><span class="sxs-lookup"><span data-stu-id="6c121-401">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="6c121-402">設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器</span><span class="sxs-lookup"><span data-stu-id="6c121-402">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
