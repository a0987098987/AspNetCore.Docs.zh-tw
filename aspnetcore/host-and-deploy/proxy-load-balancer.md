---
<span data-ttu-id="ea54a-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-102">'Blazor'</span></span>
- <span data-ttu-id="ea54a-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-103">'Identity'</span></span>
- <span data-ttu-id="ea54a-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-105">'Razor'</span></span>
- <span data-ttu-id="ea54a-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-106">'SignalR' uid:</span></span> 

---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="ea54a-107">設定 ASP.NET Core 以與 Proxy 伺服器和負載平衡器搭配運作</span><span class="sxs-lookup"><span data-stu-id="ea54a-107">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="ea54a-108">依[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ea54a-108">By [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ea54a-109">在建議的 ASP.NET Core 設定中，是使用 IIS/ASP.NET Core 模組、Nginx 或 Apache 來裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-109">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="ea54a-110">Proxy 伺服器、負載平衡器及其他網路設備通常會在要求觸達應用程式之前，遮蔽要求的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="ea54a-110">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="ea54a-111">透過 HTTP 作為 Proxy 來處理 HTTPS 要求時，原始配置 (HTTPS) 會遺失而必須在標頭中轉送。</span><span class="sxs-lookup"><span data-stu-id="ea54a-111">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="ea54a-112">由於應用程式會從 Proxy 而不是從網際網路或公司網路上的真實來源收到要求，因此必須也在標頭中轉送原始用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-112">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="ea54a-113">此資訊在要求處理方面可能相當重要，例如在重新導向、驗證、連結產生、原則評估及用戶端地理位置方面。</span><span class="sxs-lookup"><span data-stu-id="ea54a-113">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geolocation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="ea54a-114">轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="ea54a-114">Forwarded headers</span></span>

<span data-ttu-id="ea54a-115">依照慣例，Proxy 會以 HTTP 標頭轉送資訊。</span><span class="sxs-lookup"><span data-stu-id="ea54a-115">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="ea54a-116">Header</span><span class="sxs-lookup"><span data-stu-id="ea54a-116">Header</span></span> | <span data-ttu-id="ea54a-117">描述</span><span class="sxs-lookup"><span data-stu-id="ea54a-117">Description</span></span> |
| ---
<span data-ttu-id="ea54a-118">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-118">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-119">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-119">'Blazor'</span></span>
- <span data-ttu-id="ea54a-120">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-120">'Identity'</span></span>
- <span data-ttu-id="ea54a-121">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-121">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-122">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-122">'Razor'</span></span>
- <span data-ttu-id="ea54a-123">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-123">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-124">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-124">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-125">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-125">'Blazor'</span></span>
- <span data-ttu-id="ea54a-126">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-126">'Identity'</span></span>
- <span data-ttu-id="ea54a-127">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-127">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-128">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-128">'Razor'</span></span>
- <span data-ttu-id="ea54a-129">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-129">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-130">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-130">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-131">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-131">'Blazor'</span></span>
- <span data-ttu-id="ea54a-132">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-132">'Identity'</span></span>
- <span data-ttu-id="ea54a-133">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-133">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-134">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-134">'Razor'</span></span>
- <span data-ttu-id="ea54a-135">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-135">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-136">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-136">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-137">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-137">'Blazor'</span></span>
- <span data-ttu-id="ea54a-138">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-138">'Identity'</span></span>
- <span data-ttu-id="ea54a-139">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-139">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-140">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-140">'Razor'</span></span>
- <span data-ttu-id="ea54a-141">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-141">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-142">------ | |X-轉送-適用于 |保存用戶端的相關資訊，以在 proxy 鏈中起始要求和後續的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ea54a-142">------ | | X-Forwarded-For | Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="ea54a-143">此參數可能包含 IP 位址 (以及視需要可能會有連接埠號碼)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-143">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="ea54a-144">在 Proxy 伺服器鏈結中，第一個參數會指出起始要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ea54a-144">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="ea54a-145">後面接著後續的 Proxy 識別碼。</span><span class="sxs-lookup"><span data-stu-id="ea54a-145">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="ea54a-146">鏈結中的最後一個 Proxy 並不在參數清單中。</span><span class="sxs-lookup"><span data-stu-id="ea54a-146">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="ea54a-147">最後一個 Proxy 的 IP 位址 (以及視需要會有連接埠號碼) 會在傳輸層以遠端 IP 位址的形式提供。</span><span class="sxs-lookup"><span data-stu-id="ea54a-147">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> <span data-ttu-id="ea54a-148">| |X-轉送-Proto |原始配置的值（HTTP/HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="ea54a-148">| | X-Forwarded-Proto | The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="ea54a-149">如果要求周遊了多個 Proxy，則此值也可能是一個配置清單。</span><span class="sxs-lookup"><span data-stu-id="ea54a-149">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> <span data-ttu-id="ea54a-150">| |X-轉送-主機 |[主機標頭] 欄位的原始值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-150">| | X-Forwarded-Host | The original value of the Host header field.</span></span> <span data-ttu-id="ea54a-151">通常，Proxy 不會修改主機標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-151">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="ea54a-152">如需有關權限提高弱點的資訊，請參閱 [Microsoft 資訊安全諮詢 CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) \(英文\)，此弱點會影響 Proxy 不會驗證或限制主機標頭為已知有效值的系統。</span><span class="sxs-lookup"><span data-stu-id="ea54a-152">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restrict Host headers to known good values.</span></span> |

<span data-ttu-id="ea54a-153">來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的「轉送的標頭中介軟體」會讀取這些標頭，並填入 <xref:Microsoft.AspNetCore.Http.HttpContext> 上相關聯的欄位。</span><span class="sxs-lookup"><span data-stu-id="ea54a-153">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="ea54a-154">中介軟體會更新：</span><span class="sxs-lookup"><span data-stu-id="ea54a-154">The middleware updates:</span></span>

* <span data-ttu-id="ea54a-155">[Server.remoteipaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress)：使用 `X-Forwarded-For` 標頭值設定。</span><span class="sxs-lookup"><span data-stu-id="ea54a-155">[HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress): Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="ea54a-156">額外的設定會影響中介軟體設定 `RemoteIpAddress`的方式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-156">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="ea54a-157">如需詳細資料，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-157">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="ea54a-158">[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme)：使用 `X-Forwarded-Proto` 標頭值設定。</span><span class="sxs-lookup"><span data-stu-id="ea54a-158">[HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme): Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="ea54a-159">[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpRequest.Host)：使用 `X-Forwarded-Host` 標頭值設定。</span><span class="sxs-lookup"><span data-stu-id="ea54a-159">[HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host): Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="ea54a-160">您可以設定「轉送的標頭中介軟體」的[預設設定](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-160">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="ea54a-161">預設設定值為：</span><span class="sxs-lookup"><span data-stu-id="ea54a-161">The default settings are:</span></span>

* <span data-ttu-id="ea54a-162">在應用程式與要求的來源之間只有「一個 Proxy」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ea54a-162">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="ea54a-163">針對已知的 Proxy 和已知的網路，只會設定回送位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-163">Only loopback addresses are configured for known proxies and known networks.</span></span>
* <span data-ttu-id="ea54a-164">轉送標頭名稱為 `X-Forwarded-For` 和 `X-Forwarded-Proto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-164">The forwarded headers are named `X-Forwarded-For` and `X-Forwarded-Proto`.</span></span>

<span data-ttu-id="ea54a-165">並非所有網路設備在新增 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭時都不含額外組態。</span><span class="sxs-lookup"><span data-stu-id="ea54a-165">Not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="ea54a-166">如果透過 Proxy 傳送的要求在觸達應用程式時未包含這些標頭，請向您的設備製造商尋求指引。</span><span class="sxs-lookup"><span data-stu-id="ea54a-166">Consult your appliance manufacturer's guidance if proxied requests don't contain these headers when they reach the app.</span></span> <span data-ttu-id="ea54a-167">如果設備使用的名稱不是 `X-Forwarded-For` 和 `X-Forwarded-Proto`，請設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> 與 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> 選項，以符合設備使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="ea54a-167">If the appliance uses different header names than `X-Forwarded-For` and `X-Forwarded-Proto`, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the appliance.</span></span> <span data-ttu-id="ea54a-168">如需詳細資訊，請參閱[轉送標頭中介軟體選項](#forwarded-headers-middleware-options)和[使用不同標頭名稱之 Proxy 的組態](#configuration-for-a-proxy-that-uses-different-header-names)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-168">For more information, see [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) and [Configuration for a proxy that uses different header names](#configuration-for-a-proxy-that-uses-different-header-names).</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="ea54a-169">IIS/IIS Express 和 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="ea54a-169">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="ea54a-170">當應用程式是在 IIS 和 ASP.NET Core 模組的後方進行[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)裝載時，[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)預設會啟用「轉送的標頭中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="ea54a-170">Forwarded Headers Middleware is enabled by default by [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when the app is hosted [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="ea54a-171">由於有與轉送標頭相關的信任考量 (例如 [IP 詐騙](https://www.iplocation.net/ip-spoofing))，因此「轉送的標頭中介軟體」在啟用後會先在中介軟體管線中搭配 ASP.NET Core 模組限定的設定來執行。</span><span class="sxs-lookup"><span data-stu-id="ea54a-171">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="ea54a-172">中介軟體會經設定來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，並限制成單一 localhost Proxy。</span><span class="sxs-lookup"><span data-stu-id="ea54a-172">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="ea54a-173">如果需要額外的設定，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-173">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ea54a-174">其他 Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="ea54a-174">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ea54a-175">除了在[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)裝載時使用 [IIS 整合](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)之外，都未預設啟用「轉送的標頭中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="ea54a-175">Outside of using [IIS Integration](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="ea54a-176">必須啟用「轉送的標頭中介軟體」，應用程式才能使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 來處理轉送的標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-176">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>.</span></span> <span data-ttu-id="ea54a-177">啟用此中介軟體之後，如果未將任何 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 指定給中介軟體，則預設的 [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) 會是 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-177">After enabling the middleware if no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span>

<span data-ttu-id="ea54a-178">搭配 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 設定中介軟體，以在 `Startup.ConfigureServices` 中轉送 `X-Forwarded-For` 與 `X-Forwarded-Proto` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-178">Configure the middleware with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ea54a-179">在呼叫其他中介軟體之前，請先在 `Startup.Configure` 中叫用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 方法：</span><span class="sxs-lookup"><span data-stu-id="ea54a-179">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="ea54a-180">若未在 `Startup.ConfigureServices` 中指定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，或未使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 直接指定到擴充方法，則要轉送的預設標頭是 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-180">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified in `Startup.ConfigureServices` or directly to the extension method with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, the default headers to forward are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> <span data-ttu-id="ea54a-181">必須使用要轉送的標頭設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> 屬性。</span><span class="sxs-lookup"><span data-stu-id="ea54a-181">The <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> property must be configured with the headers to forward.</span></span>

## <a name="nginx-configuration"></a><span data-ttu-id="ea54a-182">Nginx 組態</span><span class="sxs-lookup"><span data-stu-id="ea54a-182">Nginx configuration</span></span>

<span data-ttu-id="ea54a-183">若要轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，請參閱 <xref:host-and-deploy/linux-nginx#configure-nginx>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-183">To forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers, see <xref:host-and-deploy/linux-nginx#configure-nginx>.</span></span> <span data-ttu-id="ea54a-184">如需詳細資訊，請參閱 [NGINX：使用轉送的標頭](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-184">For more information, see [NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).</span></span>

## <a name="apache-configuration"></a><span data-ttu-id="ea54a-185">Apache 組態</span><span class="sxs-lookup"><span data-stu-id="ea54a-185">Apache configuration</span></span>

<span data-ttu-id="ea54a-186">`X-Forwarded-For` 會自動新增 (請參閱 [Apache 模組 mod_proxy：反向 Proxy 要求標頭](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers))。</span><span class="sxs-lookup"><span data-stu-id="ea54a-186">`X-Forwarded-For` is added automatically (see [Apache Module mod_proxy: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)).</span></span> <span data-ttu-id="ea54a-187">如需如何轉送 `X-Forwarded-Proto` 標頭的資訊，請參閱 <xref:host-and-deploy/linux-apache#configure-apache>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-187">For information on how to forward the `X-Forwarded-Proto` header, see <xref:host-and-deploy/linux-apache#configure-apache>.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="ea54a-188">轉送的標頭中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="ea54a-188">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="ea54a-189"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 會控制轉送標頭中介軟體的行為。</span><span class="sxs-lookup"><span data-stu-id="ea54a-189"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> control the behavior of the Forwarded Headers Middleware.</span></span> <span data-ttu-id="ea54a-190">下列範例會變更預設值：</span><span class="sxs-lookup"><span data-stu-id="ea54a-190">The following example changes the default values:</span></span>

* <span data-ttu-id="ea54a-191">將轉送標頭中的項目數限制為 `2`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-191">Limit the number of entries in the forwarded headers to `2`.</span></span>
* <span data-ttu-id="ea54a-192">新增已知的 Proxy 位址 `127.0.10.1`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-192">Add a known proxy address of `127.0.10.1`.</span></span>
* <span data-ttu-id="ea54a-193">將轉送標頭名稱從預設的 `X-Forwarded-For` 變更為 `X-Forwarded-For-My-Custom-Header-Name`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-193">Change the forwarded header name from the default `X-Forwarded-For` to `X-Forwarded-For-My-Custom-Header-Name`.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| <span data-ttu-id="ea54a-194">選項</span><span class="sxs-lookup"><span data-stu-id="ea54a-194">Option</span></span> | <span data-ttu-id="ea54a-195">描述</span><span class="sxs-lookup"><span data-stu-id="ea54a-195">Description</span></span> |
| ---
<span data-ttu-id="ea54a-196">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-196">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-197">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-197">'Blazor'</span></span>
- <span data-ttu-id="ea54a-198">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-198">'Identity'</span></span>
- <span data-ttu-id="ea54a-199">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-199">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-200">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-200">'Razor'</span></span>
- <span data-ttu-id="ea54a-201">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-201">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-202">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-202">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-203">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-203">'Blazor'</span></span>
- <span data-ttu-id="ea54a-204">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-204">'Identity'</span></span>
- <span data-ttu-id="ea54a-205">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-205">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-206">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-206">'Razor'</span></span>
- <span data-ttu-id="ea54a-207">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-207">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-208">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-208">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-209">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-209">'Blazor'</span></span>
- <span data-ttu-id="ea54a-210">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-210">'Identity'</span></span>
- <span data-ttu-id="ea54a-211">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-211">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-212">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-212">'Razor'</span></span>
- <span data-ttu-id="ea54a-213">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-213">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-214">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-214">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-215">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-215">'Blazor'</span></span>
- <span data-ttu-id="ea54a-216">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-216">'Identity'</span></span>
- <span data-ttu-id="ea54a-217">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-217">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-218">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-218">'Razor'</span></span>
- <span data-ttu-id="ea54a-219">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-219">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-220">------ | |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> |將主機的 `X-Forwarded-Host` 標頭限制為提供的值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-220">------ | | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> | Restricts hosts by the `X-Forwarded-Host` header to the values provided.</span></span><ul><li><span data-ttu-id="ea54a-221">比較值時，會使用序數忽略大小寫的方式來比較。</span><span class="sxs-lookup"><span data-stu-id="ea54a-221">Values are compared using ordinal-ignore-case.</span></span></li><li><span data-ttu-id="ea54a-222">必須排除連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="ea54a-222">Port numbers must be excluded.</span></span></li><li><span data-ttu-id="ea54a-223">如果清單空白，即表示允許所有主機。</span><span class="sxs-lookup"><span data-stu-id="ea54a-223">If the list is empty, all hosts are allowed.</span></span></li><li><span data-ttu-id="ea54a-224">最上層的萬用字元 `*` 代表會允許所有非空白的主機。</span><span class="sxs-lookup"><span data-stu-id="ea54a-224">A top-level wildcard `*` allows all non-empty hosts.</span></span></li><li><span data-ttu-id="ea54a-225">允許使用子網域萬用字元，但不會比對出根網域。</span><span class="sxs-lookup"><span data-stu-id="ea54a-225">Subdomain wildcards are permitted but don't match the root domain.</span></span> <span data-ttu-id="ea54a-226">例如，`*.contoso.com` 會比對出子網域 `foo.contoso.com`，但不會比對出根網域 `contoso.com`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-226">For example, `*.contoso.com` matches the subdomain `foo.contoso.com` but not the root domain `contoso.com`.</span></span></li><li><span data-ttu-id="ea54a-227">允許使用 Unicode 主機名稱，但會轉換成 [Punycode](https://tools.ietf.org/html/rfc3492) 來進行比對。</span><span class="sxs-lookup"><span data-stu-id="ea54a-227">Unicode host names are allowed but are converted to [Punycode](https://tools.ietf.org/html/rfc3492) for matching.</span></span></li><li><span data-ttu-id="ea54a-228">[IPv6 addresses](https://tools.ietf.org/html/rfc4291) 必須包含週框方括號，並採用[慣例格式](https://tools.ietf.org/html/rfc4291#section-2.2) (例如 `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-228">[IPv6 addresses](https://tools.ietf.org/html/rfc4291) must include bounding brackets and be in [conventional form](https://tools.ietf.org/html/rfc4291#section-2.2) (for example, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span></span> <span data-ttu-id="ea54a-229">IPv6 位址並未特別設計成會檢查不同格式間是否具有邏輯相等性，因此不會執行標準化。</span><span class="sxs-lookup"><span data-stu-id="ea54a-229">IPv6 addresses aren't special-cased to check for logical equality between different formats, and no canonicalization is performed.</span></span></li><li><span data-ttu-id="ea54a-230">如果無法限制可允許的主機，可能會讓攻擊者偽造服務所產生的連結。</span><span class="sxs-lookup"><span data-stu-id="ea54a-230">Failure to restrict the allowed hosts may allow an attacker to spoof links generated by the service.</span></span></li></ul><span data-ttu-id="ea54a-231">預設值是空的 `IList<string>`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-231">The default value is an empty `IList<string>`.</span></span> <span data-ttu-id="ea54a-232">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-232">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName).</span></span> <span data-ttu-id="ea54a-233">當 Proxy/轉寄站未使用 `X-Forwarded-For` 標頭，而使用其他標頭轉送資訊時，會使用此選項。</span><span class="sxs-lookup"><span data-stu-id="ea54a-233">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-For` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="ea54a-234">預設值為 `X-Forwarded-For`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-234">The default is `X-Forwarded-For`.</span></span> <span data-ttu-id="ea54a-235">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> |識別應該處理的轉寄站。</span><span class="sxs-lookup"><span data-stu-id="ea54a-235">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | Identifies which forwarders should be processed.</span></span> <span data-ttu-id="ea54a-236">如需適用的欄位清單，請參閱 [ForwardedHeaders 列舉](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-236">See the [ForwardedHeaders Enum](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) for the list of fields that apply.</span></span> <span data-ttu-id="ea54a-237">指派給此屬性的一般值為 `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-237">Typical values assigned to this property are `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`.</span></span><br><br><span data-ttu-id="ea54a-238">預設值為 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-238">The default value is [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> <span data-ttu-id="ea54a-239">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-239">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName).</span></span> <span data-ttu-id="ea54a-240">當 Proxy/轉寄站未使用 `X-Forwarded-Host` 標頭，而使用其他標頭轉送資訊時，會使用此選項。</span><span class="sxs-lookup"><span data-stu-id="ea54a-240">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Host` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="ea54a-241">預設值為 `X-Forwarded-Host`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-241">The default is `X-Forwarded-Host`.</span></span> <span data-ttu-id="ea54a-242">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-242">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName).</span></span> <span data-ttu-id="ea54a-243">當 Proxy/轉寄站未使用 `X-Forwarded-Proto` 標頭，而使用其他標頭轉送資訊時，會使用此選項。</span><span class="sxs-lookup"><span data-stu-id="ea54a-243">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Proto` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="ea54a-244">預設值為 `X-Forwarded-Proto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-244">The default is `X-Forwarded-Proto`.</span></span> <span data-ttu-id="ea54a-245">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> |限制所處理標頭中的專案數。</span><span class="sxs-lookup"><span data-stu-id="ea54a-245">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="ea54a-246">設定為 `null` 可停用限制，但應該只有在已設定 `KnownProxies` 或 `KnownNetworks` 的情況下，才這樣做。</span><span class="sxs-lookup"><span data-stu-id="ea54a-246">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span> <span data-ttu-id="ea54a-247">設置非 `null` 值是一種預防措施 (但不是保證)，以防止設定不正確的 Proxy 和來自網路上的旁路惡意要求。</span><span class="sxs-lookup"><span data-stu-id="ea54a-247">Setting a non-`null` value is a precaution (but not a guarantee) to guard against misconfigured proxies and malicious requests arriving from side-channels on the network.</span></span><br><br><span data-ttu-id="ea54a-248">「轉送的標頭中介軟體」會以相反順序 (從右至左) 處理標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-248">Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="ea54a-249">如果使用預設值 (`1`)，除非 `ForwardLimit` 的值增加，否則只會處理標頭中最右邊的值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-249">If the default value (`1`) is used, only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span><br><br><span data-ttu-id="ea54a-250">預設值為 `1`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-250">The default is `1`.</span></span> <span data-ttu-id="ea54a-251">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> |要接受轉送標頭的已知網路位址範圍。</span><span class="sxs-lookup"><span data-stu-id="ea54a-251">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | Address ranges of known networks to accept forwarded headers from.</span></span> <span data-ttu-id="ea54a-252">請使用無類別網域間路由 (CIDR) 標記法來提供 IP 範圍。</span><span class="sxs-lookup"><span data-stu-id="ea54a-252">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="ea54a-253">若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 提供 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-253">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="ea54a-254">請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-254">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="ea54a-255">透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-255">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="ea54a-256">如需詳細資訊，請參閱[以 IPv6 位址表示的 IPv4 位址設定](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address)一節。</span><span class="sxs-lookup"><span data-stu-id="ea54a-256">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="ea54a-257">預設值為包含單一  項目的 `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>`IPAddress.Loopback`>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-257">The default is an `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> containing a single entry for `IPAddress.Loopback`.</span></span> <span data-ttu-id="ea54a-258">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> |要接受轉送標頭的已知 proxy 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-258">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="ea54a-259">請使用 `KnownProxies` 來指定確切的相符 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-259">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="ea54a-260">若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 提供 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-260">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="ea54a-261">請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-261">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="ea54a-262">透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-262">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="ea54a-263">如需詳細資訊，請參閱[以 IPv6 位址表示的 IPv4 位址設定](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address)一節。</span><span class="sxs-lookup"><span data-stu-id="ea54a-263">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="ea54a-264">預設值為包含單一  項目的 `IList`\<<xref:System.Net.IPAddress>`IPAddress.IPv6Loopback`>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-264">The default is an `IList`\<<xref:System.Net.IPAddress>> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> <span data-ttu-id="ea54a-265">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-265">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).</span></span><br><br><span data-ttu-id="ea54a-266">預設值為 `X-Original-For`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-266">The default is `X-Original-For`.</span></span> <span data-ttu-id="ea54a-267">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-267">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).</span></span><br><br><span data-ttu-id="ea54a-268">預設值為 `X-Original-Host`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-268">The default is `X-Original-Host`.</span></span> <span data-ttu-id="ea54a-269">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-269">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).</span></span><br><br><span data-ttu-id="ea54a-270">預設值為 `X-Original-Proto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-270">The default is `X-Original-Proto`.</span></span> <span data-ttu-id="ea54a-271">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> |要求所處理的[ForwardedHeadersOptions](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders)之間必須同步標頭值的數目。</span><span class="sxs-lookup"><span data-stu-id="ea54a-271">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) being processed.</span></span><br><br><span data-ttu-id="ea54a-272">ASP.NET Core 1.x 中的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-272">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="ea54a-273">ASP.NET Core 2.0 或更新版本中的預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-273">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="ea54a-274">情節和使用案例</span><span class="sxs-lookup"><span data-stu-id="ea54a-274">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="ea54a-275">當可以新增轉送的標頭且所有要求都安全時</span><span class="sxs-lookup"><span data-stu-id="ea54a-275">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="ea54a-276">在某些情況下，可能無法將轉送的標頭新增至透過 Proxy 傳送給應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="ea54a-276">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="ea54a-277">如果 Proxy 強制要求所有公用外部要求都必須是 HTTPS，您可以在使用任何類型的中介軟體之前，先手動在 `Startup.Configure` 中設定該配置：</span><span class="sxs-lookup"><span data-stu-id="ea54a-277">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="ea54a-278">在開發或預備環境中，可以使用環境變數或其他組態設定來停用此程式碼。</span><span class="sxs-lookup"><span data-stu-id="ea54a-278">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="ea54a-279">處理路徑基底和變更要求路徑的 Proxy</span><span class="sxs-lookup"><span data-stu-id="ea54a-279">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="ea54a-280">有些 Proxy 會原封不動傳送路徑，但其中含有應移除才能正確路由傳送的應用程式基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ea54a-280">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="ea54a-281">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) 中介軟體會將該路徑分割成 [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path)，以及將應用程式基底路徑分割成 [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-281">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) middleware splits the path into [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) and the app base path into [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).</span></span>

<span data-ttu-id="ea54a-282">如果 `/foo` 是以 `/foo/api/1` 形式傳送之 Proxy 路徑的應用程式基底路徑，中介軟體就會使用下列命令，將 `Request.PathBase` 設定為 `/foo`，以及將 `Request.Path` 設定為 `/api/1`：</span><span class="sxs-lookup"><span data-stu-id="ea54a-282">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="ea54a-283">反向再次呼叫應用程式時，則會重新套用原始路徑和路徑基底。</span><span class="sxs-lookup"><span data-stu-id="ea54a-283">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="ea54a-284">如需中介軟體順序處理的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-284">For more information on middleware order processing, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="ea54a-285">如果 Proxy 會修剪路徑 (例如，將 `/foo/api/1` 轉送給 `/api/1`)，請設定要求的 [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) 屬性來修正重新導向和連結：</span><span class="sxs-lookup"><span data-stu-id="ea54a-285">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="ea54a-286">如果 Proxy 會新增路徑資料，請使用 <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> 並指派給 <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> 屬性，以捨棄部分路徑來修正重新導向和連結：</span><span class="sxs-lookup"><span data-stu-id="ea54a-286">If the proxy is adding path data, discard part of the path to fix redirects and links by using <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> and assigning to the <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a><span data-ttu-id="ea54a-287">使用不同標頭名稱之 Proxy 的組態</span><span class="sxs-lookup"><span data-stu-id="ea54a-287">Configuration for a proxy that uses different header names</span></span>

<span data-ttu-id="ea54a-288">如果 Proxy 未使用名為 `X-Forwarded-For` 和 `X-Forwarded-Proto` 的標頭轉送 Proxy 位址/連接埠與原始配置資訊，請設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> 與 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> 選項，以符合 Proxy 使用的標頭名稱：</span><span class="sxs-lookup"><span data-stu-id="ea54a-288">If the proxy doesn't use headers named `X-Forwarded-For` and `X-Forwarded-Proto` to forward the proxy address/port and originating scheme information, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the proxy:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a><span data-ttu-id="ea54a-289">以 IPv6 位址表示的 IPv4 位址設定</span><span class="sxs-lookup"><span data-stu-id="ea54a-289">Configuration for an IPv4 address represented as an IPv6 address</span></span>

<span data-ttu-id="ea54a-290">若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 或 `::ffff:a00:1` 提供 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-290">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1` or `::ffff:a00:1`).</span></span> <span data-ttu-id="ea54a-291">請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-291">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="ea54a-292">透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-292">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span>

<span data-ttu-id="ea54a-293">在下列範例中，提供轉寄標頭的網路位址已以 IPv6 格式新增到 `KnownNetworks` 清單中。</span><span class="sxs-lookup"><span data-stu-id="ea54a-293">In the following example, a network address that supplies forwarded headers is added to the `KnownNetworks` list in IPv6 format.</span></span>

<span data-ttu-id="ea54a-294">IPv4 位址：`10.11.12.1/8`</span><span class="sxs-lookup"><span data-stu-id="ea54a-294">IPv4 address: `10.11.12.1/8`</span></span>

<span data-ttu-id="ea54a-295">轉換的 IPv6 位址：`::ffff:10.11.12.1`</span><span class="sxs-lookup"><span data-stu-id="ea54a-295">Converted IPv6 address: `::ffff:10.11.12.1`</span></span>  
<span data-ttu-id="ea54a-296">轉換的首碼長度：104</span><span class="sxs-lookup"><span data-stu-id="ea54a-296">Converted prefix length: 104</span></span>

<span data-ttu-id="ea54a-297">您也能以十六進位格式提供位址 (`10.11.12.1` 在 IPv6 中以 `::ffff:0a0b:0c01` 表示)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-297">You can also supply the address in hexadecimal format (`10.11.12.1` represented in IPv6 as `::ffff:0a0b:0c01`).</span></span> <span data-ttu-id="ea54a-298">當您將 IPv4 位址轉換為 IPv6 時，請加入 96 到 CIDR 首碼長度 (在此範例中為 `8`) 以將額外的 `::ffff:` IPv6 首碼 (8 + 96 = 104) 納入考慮。</span><span class="sxs-lookup"><span data-stu-id="ea54a-298">When converting an IPv4 address to IPv6, add 96 to the CIDR Prefix Length (`8` in the example) to account for the additional `::ffff:` IPv6 prefix (8 + 96 = 104).</span></span> 

```csharp
// To access IPNetwork and IPAddress, add the following namespaces:
// using using System.Net;
// using Microsoft.AspNetCore.HttpOverrides;
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedHeaders =
        ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    options.KnownNetworks.Add(new IPNetwork(
        IPAddress.Parse("::ffff:10.11.12.1"), 104));
});
```

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a><span data-ttu-id="ea54a-299">轉送 Linux 和非 IIS 反向 Proxy 的配置</span><span class="sxs-lookup"><span data-stu-id="ea54a-299">Forward the scheme for Linux and non-IIS reverse proxies</span></span>

<span data-ttu-id="ea54a-300">如果部署至 Azure Linux App Service、Azure Linux 虛擬機器 (VM)，或 IIS 之外的任何其他反向 Proxy 後端，則呼叫 <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> 和 <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> 的這些應用程式會使站台進入無限迴圈。</span><span class="sxs-lookup"><span data-stu-id="ea54a-300">Apps that call <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> and <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> put a site into an infinite loop if deployed to an Azure Linux App Service, Azure Linux virtual machine (VM), or behind any other reverse proxy besides IIS.</span></span> <span data-ttu-id="ea54a-301">反向 Proxy 會終止 TLS，且正確的要求配置不知道有 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="ea54a-301">TLS is terminated by the reverse proxy, and Kestrel isn't made aware of the correct request scheme.</span></span> <span data-ttu-id="ea54a-302">OAuth 和 OIDC 在此組態中也失敗，因為它們會產生不正確的重新導向。</span><span class="sxs-lookup"><span data-stu-id="ea54a-302">OAuth and OIDC also fail in this configuration because they generate incorrect redirects.</span></span> <span data-ttu-id="ea54a-303"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 在 IIS 後端執行時，會新增並設定轉送標頭中介軟體，但沒有相符的 Linux (Apache 或 Nginx 整合) 自動組態。</span><span class="sxs-lookup"><span data-stu-id="ea54a-303"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> adds and configures Forwarded Headers Middleware when running behind IIS, but there's no matching automatic configuration for Linux (Apache or Nginx integration).</span></span>

<span data-ttu-id="ea54a-304">若要從非 IIS 案例的 Proxy 轉送配置，請新增並設定轉送標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ea54a-304">To forward the scheme from the proxy in non-IIS scenarios, add and configure Forwarded Headers Middleware.</span></span> <span data-ttu-id="ea54a-305">在 `Startup.ConfigureServices` 中，使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ea54a-305">In `Startup.ConfigureServices`, use the following code:</span></span>

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

if (string.Equals(
    Environment.GetEnvironmentVariable("ASPNETCORE_FORWARDEDHEADERS_ENABLED"), 
    "true", StringComparison.OrdinalIgnoreCase))
{
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = ForwardedHeaders.XForwardedFor | 
            ForwardedHeaders.XForwardedProto;
        // Only loopback proxies are allowed by default.
        // Clear that restriction because forwarders are enabled by explicit 
        // configuration.
        options.KnownNetworks.Clear();
        options.KnownProxies.Clear();
    });
}
```

## <a name="certificate-forwarding"></a><span data-ttu-id="ea54a-306">憑證轉送</span><span class="sxs-lookup"><span data-stu-id="ea54a-306">Certificate forwarding</span></span> 

### <a name="azure"></a><span data-ttu-id="ea54a-307">Azure</span><span class="sxs-lookup"><span data-stu-id="ea54a-307">Azure</span></span>

<span data-ttu-id="ea54a-308">若要設定憑證轉送的 Azure App Service，請參閱[設定 Azure App Service 的 TLS 相互驗證](/azure/app-service/app-service-web-configure-tls-mutual-auth)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-308">To configure Azure App Service for certificate forwarding, see [Configure TLS mutual authentication for Azure App Service](/azure/app-service/app-service-web-configure-tls-mutual-auth).</span></span> <span data-ttu-id="ea54a-309">下列指導方針適用于設定 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-309">The following guidance pertains to configuring the ASP.NET Core app.</span></span>

<span data-ttu-id="ea54a-310">在中 `Startup.Configure` ，于呼叫之前新增下列程式碼 `app.UseAuthentication();` ：</span><span class="sxs-lookup"><span data-stu-id="ea54a-310">In `Startup.Configure`, add the following code before the call to `app.UseAuthentication();`:</span></span>

```csharp
app.UseCertificateForwarding();
```


<span data-ttu-id="ea54a-311">設定憑證轉送中介軟體來指定 Azure 所使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="ea54a-311">Configure Certificate Forwarding Middleware to specify the header name that Azure uses.</span></span> <span data-ttu-id="ea54a-312">在中 `Startup.ConfigureServices` ，新增下列程式碼，以設定中介軟體用來建立憑證的標頭：</span><span class="sxs-lookup"><span data-stu-id="ea54a-312">In `Startup.ConfigureServices`, add the following code to configure the header from which the middleware builds a certificate:</span></span>

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="other-web-proxies"></a><span data-ttu-id="ea54a-313">其他 web proxy</span><span class="sxs-lookup"><span data-stu-id="ea54a-313">Other web proxies</span></span>

<span data-ttu-id="ea54a-314">如果使用的 proxy 不是 IIS 或 Azure App Service 的應用程式要求路由（ARR），請將 proxy 設定為轉送其在 HTTP 標頭中收到的憑證。</span><span class="sxs-lookup"><span data-stu-id="ea54a-314">If a proxy is used that isn't IIS or Azure App Service's Application Request Routing (ARR), configure the proxy to forward the certificate that it received in an HTTP header.</span></span> <span data-ttu-id="ea54a-315">在中 `Startup.Configure` ，于呼叫之前新增下列程式碼 `app.UseAuthentication();` ：</span><span class="sxs-lookup"><span data-stu-id="ea54a-315">In `Startup.Configure`, add the following code before the call to `app.UseAuthentication();`:</span></span>

```csharp
app.UseCertificateForwarding();
```

<span data-ttu-id="ea54a-316">設定憑證轉送中介軟體來指定標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="ea54a-316">Configure the Certificate Forwarding Middleware to specify the header name.</span></span> <span data-ttu-id="ea54a-317">在中 `Startup.ConfigureServices` ，新增下列程式碼，以設定中介軟體用來建立憑證的標頭：</span><span class="sxs-lookup"><span data-stu-id="ea54a-317">In `Startup.ConfigureServices`, add the following code to configure the header from which the middleware builds a certificate:</span></span>

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

<span data-ttu-id="ea54a-318">如果 proxy 不會以 base64 編碼憑證（如同 Nginx 的情況），請設定 `HeaderConverter` 選項。</span><span class="sxs-lookup"><span data-stu-id="ea54a-318">If the proxy isn't base64-encoding the certificate (as is the case with Nginx), set the `HeaderConverter` option.</span></span> <span data-ttu-id="ea54a-319">請考慮 `Startup.ConfigureServices` 中的下列範例：</span><span class="sxs-lookup"><span data-stu-id="ea54a-319">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddCertificateForwarding(options =>
{
    options.CertificateHeader = "YOUR_CUSTOM_HEADER_NAME";
    options.HeaderConverter = (headerValue) => 
    {
        var clientCertificate = 
           /* some conversion logic to create an X509Certificate2 */
        return clientCertificate;
    }
});
```

## <a name="troubleshoot"></a><span data-ttu-id="ea54a-320">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ea54a-320">Troubleshoot</span></span>

<span data-ttu-id="ea54a-321">當標頭未如預期般傳送時，請啟用[記錄功能](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-321">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="ea54a-322">如果記錄提供的資訊不足，無法針對問題進行疑難排解，則請列舉伺服器所收到的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-322">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="ea54a-323">使用內嵌中介軟體將要求標頭寫入應用程式回應或記錄標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-323">Use inline middleware to write request headers to an app response or log the headers.</span></span> 

<span data-ttu-id="ea54a-324">若要將標頭寫入至應用程式的回應，將下列終端機內嵌中介軟體緊接著放置於 `Startup.Configure` 中對 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 的呼叫之後：</span><span class="sxs-lookup"><span data-stu-id="ea54a-324">To write the headers to the app's response, place the following terminal inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`:</span></span>

```csharp
app.Run(async (context) =>
{
    context.Response.ContentType = "text/plain";

    // Request method, scheme, and path
    await context.Response.WriteAsync(
        $"Request Method: {context.Request.Method}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Path: {context.Request.Path}{Environment.NewLine}");

    // Headers
    await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

    foreach (var header in context.Request.Headers)
    {
        await context.Response.WriteAsync($"{header.Key}: " +
            $"{header.Value}{Environment.NewLine}");
    }

    await context.Response.WriteAsync(Environment.NewLine);

    // Connection: RemoteIp
    await context.Response.WriteAsync(
        $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
});
```

<span data-ttu-id="ea54a-325">您可以寫入至記錄，而不是回應本文。</span><span class="sxs-lookup"><span data-stu-id="ea54a-325">You can write to logs instead of the response body.</span></span> <span data-ttu-id="ea54a-326">寫入至記錄可讓網站在偵錯時正常運作。</span><span class="sxs-lookup"><span data-stu-id="ea54a-326">Writing to logs allows the site to function normally while debugging.</span></span>

<span data-ttu-id="ea54a-327">若要寫入記錄而不是回應本文：</span><span class="sxs-lookup"><span data-stu-id="ea54a-327">To write logs rather than to the response body:</span></span>

* <span data-ttu-id="ea54a-328">將 `ILogger<Startup>` 插入至 `Startup` 類別，如[在啟動中建立記錄](xref:fundamentals/logging/index#create-logs-in-startup)中所述。</span><span class="sxs-lookup"><span data-stu-id="ea54a-328">Inject `ILogger<Startup>` into the `Startup` class as described in [Create logs in Startup](xref:fundamentals/logging/index#create-logs-in-startup).</span></span>
* <span data-ttu-id="ea54a-329">將下列終端機內嵌中介軟體緊接著放置於 `Startup.Configure` 中對 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 的呼叫之後。</span><span class="sxs-lookup"><span data-stu-id="ea54a-329">Place the following inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // Request method, scheme, and path
    _logger.LogDebug("Request Method: {Method}", context.Request.Method);
    _logger.LogDebug("Request Scheme: {Scheme}", context.Request.Scheme);
    _logger.LogDebug("Request Path: {Path}", context.Request.Path);

    // Headers
    foreach (var header in context.Request.Headers)
    {
        _logger.LogDebug("Header: {Key}: {Value}", header.Key, header.Value);
    }

    // Connection: RemoteIp
    _logger.LogDebug("Request RemoteIp: {RemoteIpAddress}", 
        context.Connection.RemoteIpAddress);

    await next();
});
```

<span data-ttu-id="ea54a-330">處理後，`X-Forwarded-{For|Proto|Host}` 值會移至 `X-Original-{For|Proto|Host}`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-330">When processed, `X-Forwarded-{For|Proto|Host}` values are moved to `X-Original-{For|Proto|Host}`.</span></span> <span data-ttu-id="ea54a-331">如果指定的標頭中有多個值，「轉送的標頭中介軟體」會以相反順序 (從右至左) 處理標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-331">If there are multiple values in a given header, Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="ea54a-332">預設的 `ForwardLimit` 是 `1` (一)，因此除非 `ForwardLimit` 的值增加，否則只會處理標頭中最右邊的值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-332">The default `ForwardLimit` is `1` (one), so only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span>

<span data-ttu-id="ea54a-333">要求的原始遠端 IP 必須符合 `KnownProxies` 或 `KnownNetworks` 清單中的項目，系統才會處理轉送的標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-333">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before forwarded headers are processed.</span></span> <span data-ttu-id="ea54a-334">這可藉由不接受來自不受信任 Proxy 的轉送子，以限制標頭詐騙行為。</span><span class="sxs-lookup"><span data-stu-id="ea54a-334">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span> <span data-ttu-id="ea54a-335">當偵測到未知的 Proxy 時，記錄會指出 Proxy 的位址：</span><span class="sxs-lookup"><span data-stu-id="ea54a-335">When an unknown proxy is detected, logging indicates the address of the proxy:</span></span>

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

<span data-ttu-id="ea54a-336">在上述範例中，10.0.0.100 是 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ea54a-336">In the preceding example, 10.0.0.100 is a proxy server.</span></span> <span data-ttu-id="ea54a-337">如果伺服器是信任的 Proxy，請將伺服器的 IP 位址新增至 `Startup.ConfigureServices` 中的 `KnownProxies` (或將信任的網路新增至 `KnownNetworks`)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-337">If the server is a trusted proxy, add the server's IP address to `KnownProxies` (or add a trusted network to `KnownNetworks`) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ea54a-338">如需詳細資訊，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)一節。</span><span class="sxs-lookup"><span data-stu-id="ea54a-338">For more information, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) section.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> <span data-ttu-id="ea54a-339">只允許信任的 Proxy 以及網路轉送標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-339">Only allow trusted proxies and networks to forward headers.</span></span> <span data-ttu-id="ea54a-340">否則，[IP 詐騙](https://www.iplocation.net/ip-spoofing)攻擊有可能發生。</span><span class="sxs-lookup"><span data-stu-id="ea54a-340">Otherwise, [IP spoofing](https://www.iplocation.net/ip-spoofing) attacks are possible.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea54a-341">其他資源</span><span class="sxs-lookup"><span data-stu-id="ea54a-341">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
* <span data-ttu-id="ea54a-342">[Microsoft Security Advisory CVE-2018-0787：ASP.NET Core 權限提高弱點](https://github.com/aspnet/Announcements/issues/295) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="ea54a-342">[Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability](https://github.com/aspnet/Announcements/issues/295)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ea54a-343">在建議的 ASP.NET Core 設定中，是使用 IIS/ASP.NET Core 模組、Nginx 或 Apache 來裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-343">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="ea54a-344">Proxy 伺服器、負載平衡器及其他網路設備通常會在要求觸達應用程式之前，遮蔽要求的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="ea54a-344">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="ea54a-345">透過 HTTP 作為 Proxy 來處理 HTTPS 要求時，原始配置 (HTTPS) 會遺失而必須在標頭中轉送。</span><span class="sxs-lookup"><span data-stu-id="ea54a-345">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="ea54a-346">由於應用程式會從 Proxy 而不是從網際網路或公司網路上的真實來源收到要求，因此必須也在標頭中轉送原始用戶端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-346">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="ea54a-347">此資訊在要求處理方面可能相當重要，例如在重新導向、驗證、連結產生、原則評估及用戶端地理位置方面。</span><span class="sxs-lookup"><span data-stu-id="ea54a-347">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geolocation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="ea54a-348">轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="ea54a-348">Forwarded headers</span></span>

<span data-ttu-id="ea54a-349">依照慣例，Proxy 會以 HTTP 標頭轉送資訊。</span><span class="sxs-lookup"><span data-stu-id="ea54a-349">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="ea54a-350">Header</span><span class="sxs-lookup"><span data-stu-id="ea54a-350">Header</span></span> | <span data-ttu-id="ea54a-351">描述</span><span class="sxs-lookup"><span data-stu-id="ea54a-351">Description</span></span> |
| ---
<span data-ttu-id="ea54a-352">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-352">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-353">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-353">'Blazor'</span></span>
- <span data-ttu-id="ea54a-354">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-354">'Identity'</span></span>
- <span data-ttu-id="ea54a-355">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-355">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-356">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-356">'Razor'</span></span>
- <span data-ttu-id="ea54a-357">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-357">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-358">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-358">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-359">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-359">'Blazor'</span></span>
- <span data-ttu-id="ea54a-360">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-360">'Identity'</span></span>
- <span data-ttu-id="ea54a-361">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-361">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-362">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-362">'Razor'</span></span>
- <span data-ttu-id="ea54a-363">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-363">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-364">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-364">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-365">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-365">'Blazor'</span></span>
- <span data-ttu-id="ea54a-366">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-366">'Identity'</span></span>
- <span data-ttu-id="ea54a-367">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-367">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-368">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-368">'Razor'</span></span>
- <span data-ttu-id="ea54a-369">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-369">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-370">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-370">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-371">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-371">'Blazor'</span></span>
- <span data-ttu-id="ea54a-372">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-372">'Identity'</span></span>
- <span data-ttu-id="ea54a-373">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-373">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-374">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-374">'Razor'</span></span>
- <span data-ttu-id="ea54a-375">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-375">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-376">------ | |X-轉送-適用于 |保存用戶端的相關資訊，以在 proxy 鏈中起始要求和後續的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ea54a-376">------ | | X-Forwarded-For | Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="ea54a-377">此參數可能包含 IP 位址 (以及視需要可能會有連接埠號碼)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-377">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="ea54a-378">在 Proxy 伺服器鏈結中，第一個參數會指出起始要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ea54a-378">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="ea54a-379">後面接著後續的 Proxy 識別碼。</span><span class="sxs-lookup"><span data-stu-id="ea54a-379">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="ea54a-380">鏈結中的最後一個 Proxy 並不在參數清單中。</span><span class="sxs-lookup"><span data-stu-id="ea54a-380">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="ea54a-381">最後一個 Proxy 的 IP 位址 (以及視需要會有連接埠號碼) 會在傳輸層以遠端 IP 位址的形式提供。</span><span class="sxs-lookup"><span data-stu-id="ea54a-381">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> <span data-ttu-id="ea54a-382">| |X-轉送-Proto |原始配置的值（HTTP/HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="ea54a-382">| | X-Forwarded-Proto | The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="ea54a-383">如果要求周遊了多個 Proxy，則此值也可能是一個配置清單。</span><span class="sxs-lookup"><span data-stu-id="ea54a-383">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> <span data-ttu-id="ea54a-384">| |X-轉送-主機 |[主機標頭] 欄位的原始值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-384">| | X-Forwarded-Host | The original value of the Host header field.</span></span> <span data-ttu-id="ea54a-385">通常，Proxy 不會修改主機標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-385">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="ea54a-386">如需有關權限提高弱點的資訊，請參閱 [Microsoft 資訊安全諮詢 CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) \(英文\)，此弱點會影響 Proxy 不會驗證或限制主機標頭為已知有效值的系統。</span><span class="sxs-lookup"><span data-stu-id="ea54a-386">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restrict Host headers to known good values.</span></span> |

<span data-ttu-id="ea54a-387">來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的「轉送的標頭中介軟體」會讀取這些標頭，並填入 <xref:Microsoft.AspNetCore.Http.HttpContext> 上相關聯的欄位。</span><span class="sxs-lookup"><span data-stu-id="ea54a-387">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="ea54a-388">中介軟體會更新：</span><span class="sxs-lookup"><span data-stu-id="ea54a-388">The middleware updates:</span></span>

* <span data-ttu-id="ea54a-389">[Server.remoteipaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress)：使用 `X-Forwarded-For` 標頭值設定。</span><span class="sxs-lookup"><span data-stu-id="ea54a-389">[HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress): Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="ea54a-390">額外的設定會影響中介軟體設定 `RemoteIpAddress`的方式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-390">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="ea54a-391">如需詳細資料，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-391">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="ea54a-392">[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme)：使用 `X-Forwarded-Proto` 標頭值設定。</span><span class="sxs-lookup"><span data-stu-id="ea54a-392">[HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme): Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="ea54a-393">[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpRequest.Host)：使用 `X-Forwarded-Host` 標頭值設定。</span><span class="sxs-lookup"><span data-stu-id="ea54a-393">[HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host): Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="ea54a-394">您可以設定「轉送的標頭中介軟體」的[預設設定](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-394">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="ea54a-395">預設設定值為：</span><span class="sxs-lookup"><span data-stu-id="ea54a-395">The default settings are:</span></span>

* <span data-ttu-id="ea54a-396">在應用程式與要求的來源之間只有「一個 Proxy」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ea54a-396">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="ea54a-397">針對已知的 Proxy 和已知的網路，只會設定回送位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-397">Only loopback addresses are configured for known proxies and known networks.</span></span>
* <span data-ttu-id="ea54a-398">轉送標頭名稱為 `X-Forwarded-For` 和 `X-Forwarded-Proto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-398">The forwarded headers are named `X-Forwarded-For` and `X-Forwarded-Proto`.</span></span>

<span data-ttu-id="ea54a-399">並非所有網路設備在新增 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭時都不含額外組態。</span><span class="sxs-lookup"><span data-stu-id="ea54a-399">Not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="ea54a-400">如果透過 Proxy 傳送的要求在觸達應用程式時未包含這些標頭，請向您的設備製造商尋求指引。</span><span class="sxs-lookup"><span data-stu-id="ea54a-400">Consult your appliance manufacturer's guidance if proxied requests don't contain these headers when they reach the app.</span></span> <span data-ttu-id="ea54a-401">如果設備使用的名稱不是 `X-Forwarded-For` 和 `X-Forwarded-Proto`，請設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> 與 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> 選項，以符合設備使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="ea54a-401">If the appliance uses different header names than `X-Forwarded-For` and `X-Forwarded-Proto`, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the appliance.</span></span> <span data-ttu-id="ea54a-402">如需詳細資訊，請參閱[轉送標頭中介軟體選項](#forwarded-headers-middleware-options)和[使用不同標頭名稱之 Proxy 的組態](#configuration-for-a-proxy-that-uses-different-header-names)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-402">For more information, see [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) and [Configuration for a proxy that uses different header names](#configuration-for-a-proxy-that-uses-different-header-names).</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="ea54a-403">IIS/IIS Express 和 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="ea54a-403">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="ea54a-404">當應用程式是在 IIS 和 ASP.NET Core 模組的後方進行[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)裝載時，[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)預設會啟用「轉送的標頭中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="ea54a-404">Forwarded Headers Middleware is enabled by default by [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when the app is hosted [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="ea54a-405">由於有與轉送標頭相關的信任考量 (例如 [IP 詐騙](https://www.iplocation.net/ip-spoofing))，因此「轉送的標頭中介軟體」在啟用後會先在中介軟體管線中搭配 ASP.NET Core 模組限定的設定來執行。</span><span class="sxs-lookup"><span data-stu-id="ea54a-405">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="ea54a-406">中介軟體會經設定來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，並限制成單一 localhost Proxy。</span><span class="sxs-lookup"><span data-stu-id="ea54a-406">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="ea54a-407">如果需要額外的設定，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-407">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ea54a-408">其他 Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="ea54a-408">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ea54a-409">除了在[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)裝載時使用 [IIS 整合](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)之外，都未預設啟用「轉送的標頭中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="ea54a-409">Outside of using [IIS Integration](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="ea54a-410">必須啟用「轉送的標頭中介軟體」，應用程式才能使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 來處理轉送的標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-410">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>.</span></span> <span data-ttu-id="ea54a-411">啟用此中介軟體之後，如果未將任何 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 指定給中介軟體，則預設的 [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) 會是 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-411">After enabling the middleware if no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span>

<span data-ttu-id="ea54a-412">搭配 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 設定中介軟體，以在 `Startup.ConfigureServices` 中轉送 `X-Forwarded-For` 與 `X-Forwarded-Proto` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-412">Configure the middleware with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ea54a-413">在呼叫其他中介軟體之前，請先在 `Startup.Configure` 中叫用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 方法：</span><span class="sxs-lookup"><span data-stu-id="ea54a-413">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="ea54a-414">若未在 `Startup.ConfigureServices` 中指定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，或未使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 直接指定到擴充方法，則要轉送的預設標頭是 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-414">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified in `Startup.ConfigureServices` or directly to the extension method with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, the default headers to forward are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> <span data-ttu-id="ea54a-415">必須使用要轉送的標頭設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> 屬性。</span><span class="sxs-lookup"><span data-stu-id="ea54a-415">The <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> property must be configured with the headers to forward.</span></span>

## <a name="nginx-configuration"></a><span data-ttu-id="ea54a-416">Nginx 組態</span><span class="sxs-lookup"><span data-stu-id="ea54a-416">Nginx configuration</span></span>

<span data-ttu-id="ea54a-417">若要轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，請參閱 <xref:host-and-deploy/linux-nginx#configure-nginx>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-417">To forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers, see <xref:host-and-deploy/linux-nginx#configure-nginx>.</span></span> <span data-ttu-id="ea54a-418">如需詳細資訊，請參閱 [NGINX：使用轉送的標頭](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-418">For more information, see [NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).</span></span>

## <a name="apache-configuration"></a><span data-ttu-id="ea54a-419">Apache 組態</span><span class="sxs-lookup"><span data-stu-id="ea54a-419">Apache configuration</span></span>

<span data-ttu-id="ea54a-420">`X-Forwarded-For` 會自動新增 (請參閱 [Apache 模組 mod_proxy：反向 Proxy 要求標頭](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers))。</span><span class="sxs-lookup"><span data-stu-id="ea54a-420">`X-Forwarded-For` is added automatically (see [Apache Module mod_proxy: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)).</span></span> <span data-ttu-id="ea54a-421">如需如何轉送 `X-Forwarded-Proto` 標頭的資訊，請參閱 <xref:host-and-deploy/linux-apache#configure-apache>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-421">For information on how to forward the `X-Forwarded-Proto` header, see <xref:host-and-deploy/linux-apache#configure-apache>.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="ea54a-422">轉送的標頭中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="ea54a-422">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="ea54a-423"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 會控制轉送標頭中介軟體的行為。</span><span class="sxs-lookup"><span data-stu-id="ea54a-423"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> control the behavior of the Forwarded Headers Middleware.</span></span> <span data-ttu-id="ea54a-424">下列範例會變更預設值：</span><span class="sxs-lookup"><span data-stu-id="ea54a-424">The following example changes the default values:</span></span>

* <span data-ttu-id="ea54a-425">將轉送標頭中的項目數限制為 `2`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-425">Limit the number of entries in the forwarded headers to `2`.</span></span>
* <span data-ttu-id="ea54a-426">新增已知的 Proxy 位址 `127.0.10.1`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-426">Add a known proxy address of `127.0.10.1`.</span></span>
* <span data-ttu-id="ea54a-427">將轉送標頭名稱從預設的 `X-Forwarded-For` 變更為 `X-Forwarded-For-My-Custom-Header-Name`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-427">Change the forwarded header name from the default `X-Forwarded-For` to `X-Forwarded-For-My-Custom-Header-Name`.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| <span data-ttu-id="ea54a-428">選項</span><span class="sxs-lookup"><span data-stu-id="ea54a-428">Option</span></span> | <span data-ttu-id="ea54a-429">描述</span><span class="sxs-lookup"><span data-stu-id="ea54a-429">Description</span></span> |
| ---
<span data-ttu-id="ea54a-430">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-430">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-431">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-431">'Blazor'</span></span>
- <span data-ttu-id="ea54a-432">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-432">'Identity'</span></span>
- <span data-ttu-id="ea54a-433">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-433">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-434">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-434">'Razor'</span></span>
- <span data-ttu-id="ea54a-435">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-435">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-436">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-436">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-437">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-437">'Blazor'</span></span>
- <span data-ttu-id="ea54a-438">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-438">'Identity'</span></span>
- <span data-ttu-id="ea54a-439">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-439">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-440">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-440">'Razor'</span></span>
- <span data-ttu-id="ea54a-441">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-441">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-442">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-442">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-443">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-443">'Blazor'</span></span>
- <span data-ttu-id="ea54a-444">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-444">'Identity'</span></span>
- <span data-ttu-id="ea54a-445">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-445">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-446">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-446">'Razor'</span></span>
- <span data-ttu-id="ea54a-447">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-447">'SignalR' uid:</span></span> 

-
<span data-ttu-id="ea54a-448">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="ea54a-448">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="ea54a-449">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-449">'Blazor'</span></span>
- <span data-ttu-id="ea54a-450">'Identity'</span><span class="sxs-lookup"><span data-stu-id="ea54a-450">'Identity'</span></span>
- <span data-ttu-id="ea54a-451">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="ea54a-451">'Let's Encrypt'</span></span>
- <span data-ttu-id="ea54a-452">'Razor'</span><span class="sxs-lookup"><span data-stu-id="ea54a-452">'Razor'</span></span>
- <span data-ttu-id="ea54a-453">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="ea54a-453">'SignalR' uid:</span></span> 

<span data-ttu-id="ea54a-454">------ | |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> |將主機的 `X-Forwarded-Host` 標頭限制為提供的值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-454">------ | | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> | Restricts hosts by the `X-Forwarded-Host` header to the values provided.</span></span><ul><li><span data-ttu-id="ea54a-455">比較值時，會使用序數忽略大小寫的方式來比較。</span><span class="sxs-lookup"><span data-stu-id="ea54a-455">Values are compared using ordinal-ignore-case.</span></span></li><li><span data-ttu-id="ea54a-456">必須排除連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="ea54a-456">Port numbers must be excluded.</span></span></li><li><span data-ttu-id="ea54a-457">如果清單空白，即表示允許所有主機。</span><span class="sxs-lookup"><span data-stu-id="ea54a-457">If the list is empty, all hosts are allowed.</span></span></li><li><span data-ttu-id="ea54a-458">最上層的萬用字元 `*` 代表會允許所有非空白的主機。</span><span class="sxs-lookup"><span data-stu-id="ea54a-458">A top-level wildcard `*` allows all non-empty hosts.</span></span></li><li><span data-ttu-id="ea54a-459">允許使用子網域萬用字元，但不會比對出根網域。</span><span class="sxs-lookup"><span data-stu-id="ea54a-459">Subdomain wildcards are permitted but don't match the root domain.</span></span> <span data-ttu-id="ea54a-460">例如，`*.contoso.com` 會比對出子網域 `foo.contoso.com`，但不會比對出根網域 `contoso.com`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-460">For example, `*.contoso.com` matches the subdomain `foo.contoso.com` but not the root domain `contoso.com`.</span></span></li><li><span data-ttu-id="ea54a-461">允許使用 Unicode 主機名稱，但會轉換成 [Punycode](https://tools.ietf.org/html/rfc3492) 來進行比對。</span><span class="sxs-lookup"><span data-stu-id="ea54a-461">Unicode host names are allowed but are converted to [Punycode](https://tools.ietf.org/html/rfc3492) for matching.</span></span></li><li><span data-ttu-id="ea54a-462">[IPv6 addresses](https://tools.ietf.org/html/rfc4291) 必須包含週框方括號，並採用[慣例格式](https://tools.ietf.org/html/rfc4291#section-2.2) (例如 `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-462">[IPv6 addresses](https://tools.ietf.org/html/rfc4291) must include bounding brackets and be in [conventional form](https://tools.ietf.org/html/rfc4291#section-2.2) (for example, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span></span> <span data-ttu-id="ea54a-463">IPv6 位址並未特別設計成會檢查不同格式間是否具有邏輯相等性，因此不會執行標準化。</span><span class="sxs-lookup"><span data-stu-id="ea54a-463">IPv6 addresses aren't special-cased to check for logical equality between different formats, and no canonicalization is performed.</span></span></li><li><span data-ttu-id="ea54a-464">如果無法限制可允許的主機，可能會讓攻擊者偽造服務所產生的連結。</span><span class="sxs-lookup"><span data-stu-id="ea54a-464">Failure to restrict the allowed hosts may allow an attacker to spoof links generated by the service.</span></span></li></ul><span data-ttu-id="ea54a-465">預設值是空的 `IList<string>`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-465">The default value is an empty `IList<string>`.</span></span> <span data-ttu-id="ea54a-466">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-466">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName).</span></span> <span data-ttu-id="ea54a-467">當 Proxy/轉寄站未使用 `X-Forwarded-For` 標頭，而使用其他標頭轉送資訊時，會使用此選項。</span><span class="sxs-lookup"><span data-stu-id="ea54a-467">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-For` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="ea54a-468">預設值為 `X-Forwarded-For`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-468">The default is `X-Forwarded-For`.</span></span> <span data-ttu-id="ea54a-469">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> |識別應該處理的轉寄站。</span><span class="sxs-lookup"><span data-stu-id="ea54a-469">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | Identifies which forwarders should be processed.</span></span> <span data-ttu-id="ea54a-470">如需適用的欄位清單，請參閱 [ForwardedHeaders 列舉](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-470">See the [ForwardedHeaders Enum](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) for the list of fields that apply.</span></span> <span data-ttu-id="ea54a-471">指派給此屬性的一般值為 `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-471">Typical values assigned to this property are `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`.</span></span><br><br><span data-ttu-id="ea54a-472">預設值為 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-472">The default value is [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> <span data-ttu-id="ea54a-473">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-473">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName).</span></span> <span data-ttu-id="ea54a-474">當 Proxy/轉寄站未使用 `X-Forwarded-Host` 標頭，而使用其他標頭轉送資訊時，會使用此選項。</span><span class="sxs-lookup"><span data-stu-id="ea54a-474">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Host` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="ea54a-475">預設值為 `X-Forwarded-Host`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-475">The default is `X-Forwarded-Host`.</span></span> <span data-ttu-id="ea54a-476">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-476">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName).</span></span> <span data-ttu-id="ea54a-477">當 Proxy/轉寄站未使用 `X-Forwarded-Proto` 標頭，而使用其他標頭轉送資訊時，會使用此選項。</span><span class="sxs-lookup"><span data-stu-id="ea54a-477">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Proto` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="ea54a-478">預設值為 `X-Forwarded-Proto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-478">The default is `X-Forwarded-Proto`.</span></span> <span data-ttu-id="ea54a-479">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> |限制所處理標頭中的專案數。</span><span class="sxs-lookup"><span data-stu-id="ea54a-479">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="ea54a-480">設定為 `null` 可停用限制，但應該只有在已設定 `KnownProxies` 或 `KnownNetworks` 的情況下，才這樣做。</span><span class="sxs-lookup"><span data-stu-id="ea54a-480">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span> <span data-ttu-id="ea54a-481">設置非 `null` 值是一種預防措施 (但不是保證)，以防止設定不正確的 Proxy 和來自網路上的旁路惡意要求。</span><span class="sxs-lookup"><span data-stu-id="ea54a-481">Setting a non-`null` value is a precaution (but not a guarantee) to guard against misconfigured proxies and malicious requests arriving from side-channels on the network.</span></span><br><br><span data-ttu-id="ea54a-482">「轉送的標頭中介軟體」會以相反順序 (從右至左) 處理標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-482">Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="ea54a-483">如果使用預設值 (`1`)，除非 `ForwardLimit` 的值增加，否則只會處理標頭中最右邊的值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-483">If the default value (`1`) is used, only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span><br><br><span data-ttu-id="ea54a-484">預設值為 `1`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-484">The default is `1`.</span></span> <span data-ttu-id="ea54a-485">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> |要接受轉送標頭的已知網路位址範圍。</span><span class="sxs-lookup"><span data-stu-id="ea54a-485">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | Address ranges of known networks to accept forwarded headers from.</span></span> <span data-ttu-id="ea54a-486">請使用無類別網域間路由 (CIDR) 標記法來提供 IP 範圍。</span><span class="sxs-lookup"><span data-stu-id="ea54a-486">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="ea54a-487">若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 提供 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-487">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="ea54a-488">請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-488">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="ea54a-489">透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-489">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="ea54a-490">如需詳細資訊，請參閱[以 IPv6 位址表示的 IPv4 位址設定](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address)一節。</span><span class="sxs-lookup"><span data-stu-id="ea54a-490">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="ea54a-491">預設值為包含單一  項目的 `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>`IPAddress.Loopback`>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-491">The default is an `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> containing a single entry for `IPAddress.Loopback`.</span></span> <span data-ttu-id="ea54a-492">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> |要接受轉送標頭的已知 proxy 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-492">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="ea54a-493">請使用 `KnownProxies` 來指定確切的相符 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-493">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="ea54a-494">若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 提供 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-494">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="ea54a-495">請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-495">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="ea54a-496">透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-496">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="ea54a-497">如需詳細資訊，請參閱[以 IPv6 位址表示的 IPv4 位址設定](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address)一節。</span><span class="sxs-lookup"><span data-stu-id="ea54a-497">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="ea54a-498">預設值為包含單一  項目的 `IList`\<<xref:System.Net.IPAddress>`IPAddress.IPv6Loopback`>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-498">The default is an `IList`\<<xref:System.Net.IPAddress>> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> <span data-ttu-id="ea54a-499">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-499">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).</span></span><br><br><span data-ttu-id="ea54a-500">預設值為 `X-Original-For`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-500">The default is `X-Original-For`.</span></span> <span data-ttu-id="ea54a-501">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-501">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).</span></span><br><br><span data-ttu-id="ea54a-502">預設值為 `X-Original-Host`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-502">The default is `X-Original-Host`.</span></span> <span data-ttu-id="ea54a-503">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> |請使用這個屬性所指定的標頭，而不是[標 XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName)所指定的標題。</span><span class="sxs-lookup"><span data-stu-id="ea54a-503">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).</span></span><br><br><span data-ttu-id="ea54a-504">預設值為 `X-Original-Proto`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-504">The default is `X-Original-Proto`.</span></span> <span data-ttu-id="ea54a-505">| |<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> |要求所處理的[ForwardedHeadersOptions](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders)之間必須同步標頭值的數目。</span><span class="sxs-lookup"><span data-stu-id="ea54a-505">| | <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) being processed.</span></span><br><br><span data-ttu-id="ea54a-506">ASP.NET Core 1.x 中的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-506">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="ea54a-507">ASP.NET Core 2.0 或更新版本中的預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-507">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="ea54a-508">情節和使用案例</span><span class="sxs-lookup"><span data-stu-id="ea54a-508">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="ea54a-509">當可以新增轉送的標頭且所有要求都安全時</span><span class="sxs-lookup"><span data-stu-id="ea54a-509">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="ea54a-510">在某些情況下，可能無法將轉送的標頭新增至透過 Proxy 傳送給應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="ea54a-510">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="ea54a-511">如果 Proxy 強制要求所有公用外部要求都必須是 HTTPS，您可以在使用任何類型的中介軟體之前，先手動在 `Startup.Configure` 中設定該配置：</span><span class="sxs-lookup"><span data-stu-id="ea54a-511">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="ea54a-512">在開發或預備環境中，可以使用環境變數或其他組態設定來停用此程式碼。</span><span class="sxs-lookup"><span data-stu-id="ea54a-512">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="ea54a-513">處理路徑基底和變更要求路徑的 Proxy</span><span class="sxs-lookup"><span data-stu-id="ea54a-513">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="ea54a-514">有些 Proxy 會原封不動傳送路徑，但其中含有應移除才能正確路由傳送的應用程式基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ea54a-514">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="ea54a-515">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) 中介軟體會將該路徑分割成 [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path)，以及將應用程式基底路徑分割成 [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-515">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) middleware splits the path into [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) and the app base path into [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).</span></span>

<span data-ttu-id="ea54a-516">如果 `/foo` 是以 `/foo/api/1` 形式傳送之 Proxy 路徑的應用程式基底路徑，中介軟體就會使用下列命令，將 `Request.PathBase` 設定為 `/foo`，以及將 `Request.Path` 設定為 `/api/1`：</span><span class="sxs-lookup"><span data-stu-id="ea54a-516">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="ea54a-517">反向再次呼叫應用程式時，則會重新套用原始路徑和路徑基底。</span><span class="sxs-lookup"><span data-stu-id="ea54a-517">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="ea54a-518">如需中介軟體順序處理的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="ea54a-518">For more information on middleware order processing, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="ea54a-519">如果 Proxy 會修剪路徑 (例如，將 `/foo/api/1` 轉送給 `/api/1`)，請設定要求的 [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) 屬性來修正重新導向和連結：</span><span class="sxs-lookup"><span data-stu-id="ea54a-519">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="ea54a-520">如果 Proxy 會新增路徑資料，請使用 <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> 並指派給 <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> 屬性，以捨棄部分路徑來修正重新導向和連結：</span><span class="sxs-lookup"><span data-stu-id="ea54a-520">If the proxy is adding path data, discard part of the path to fix redirects and links by using <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> and assigning to the <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a><span data-ttu-id="ea54a-521">使用不同標頭名稱之 Proxy 的組態</span><span class="sxs-lookup"><span data-stu-id="ea54a-521">Configuration for a proxy that uses different header names</span></span>

<span data-ttu-id="ea54a-522">如果 Proxy 未使用名為 `X-Forwarded-For` 和 `X-Forwarded-Proto` 的標頭轉送 Proxy 位址/連接埠與原始配置資訊，請設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> 與 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> 選項，以符合 Proxy 使用的標頭名稱：</span><span class="sxs-lookup"><span data-stu-id="ea54a-522">If the proxy doesn't use headers named `X-Forwarded-For` and `X-Forwarded-Proto` to forward the proxy address/port and originating scheme information, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the proxy:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a><span data-ttu-id="ea54a-523">以 IPv6 位址表示的 IPv4 位址設定</span><span class="sxs-lookup"><span data-stu-id="ea54a-523">Configuration for an IPv4 address represented as an IPv6 address</span></span>

<span data-ttu-id="ea54a-524">若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 或 `::ffff:a00:1` 提供 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="ea54a-524">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1` or `::ffff:a00:1`).</span></span> <span data-ttu-id="ea54a-525">請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-525">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="ea54a-526">透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。</span><span class="sxs-lookup"><span data-stu-id="ea54a-526">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span>

<span data-ttu-id="ea54a-527">在下列範例中，提供轉寄標頭的網路位址已以 IPv6 格式新增到 `KnownNetworks` 清單中。</span><span class="sxs-lookup"><span data-stu-id="ea54a-527">In the following example, a network address that supplies forwarded headers is added to the `KnownNetworks` list in IPv6 format.</span></span>

<span data-ttu-id="ea54a-528">IPv4 位址：`10.11.12.1/8`</span><span class="sxs-lookup"><span data-stu-id="ea54a-528">IPv4 address: `10.11.12.1/8`</span></span>

<span data-ttu-id="ea54a-529">轉換的 IPv6 位址：`::ffff:10.11.12.1`</span><span class="sxs-lookup"><span data-stu-id="ea54a-529">Converted IPv6 address: `::ffff:10.11.12.1`</span></span>  
<span data-ttu-id="ea54a-530">轉換的首碼長度：104</span><span class="sxs-lookup"><span data-stu-id="ea54a-530">Converted prefix length: 104</span></span>

<span data-ttu-id="ea54a-531">您也能以十六進位格式提供位址 (`10.11.12.1` 在 IPv6 中以 `::ffff:0a0b:0c01` 表示)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-531">You can also supply the address in hexadecimal format (`10.11.12.1` represented in IPv6 as `::ffff:0a0b:0c01`).</span></span> <span data-ttu-id="ea54a-532">當您將 IPv4 位址轉換為 IPv6 時，請加入 96 到 CIDR 首碼長度 (在此範例中為 `8`) 以將額外的 `::ffff:` IPv6 首碼 (8 + 96 = 104) 納入考慮。</span><span class="sxs-lookup"><span data-stu-id="ea54a-532">When converting an IPv4 address to IPv6, add 96 to the CIDR Prefix Length (`8` in the example) to account for the additional `::ffff:` IPv6 prefix (8 + 96 = 104).</span></span> 

```csharp
// To access IPNetwork and IPAddress, add the following namespaces:
// using using System.Net;
// using Microsoft.AspNetCore.HttpOverrides;
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedHeaders =
        ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    options.KnownNetworks.Add(new IPNetwork(
        IPAddress.Parse("::ffff:10.11.12.1"), 104));
});
```

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a><span data-ttu-id="ea54a-533">轉送 Linux 和非 IIS 反向 Proxy 的配置</span><span class="sxs-lookup"><span data-stu-id="ea54a-533">Forward the scheme for Linux and non-IIS reverse proxies</span></span>

<span data-ttu-id="ea54a-534">如果部署至 Azure Linux App Service、Azure Linux 虛擬機器 (VM)，或 IIS 之外的任何其他反向 Proxy 後端，則呼叫 <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> 和 <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> 的這些應用程式會使站台進入無限迴圈。</span><span class="sxs-lookup"><span data-stu-id="ea54a-534">Apps that call <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> and <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> put a site into an infinite loop if deployed to an Azure Linux App Service, Azure Linux virtual machine (VM), or behind any other reverse proxy besides IIS.</span></span> <span data-ttu-id="ea54a-535">反向 Proxy 會終止 TLS，且正確的要求配置不知道有 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="ea54a-535">TLS is terminated by the reverse proxy, and Kestrel isn't made aware of the correct request scheme.</span></span> <span data-ttu-id="ea54a-536">OAuth 和 OIDC 在此組態中也失敗，因為它們會產生不正確的重新導向。</span><span class="sxs-lookup"><span data-stu-id="ea54a-536">OAuth and OIDC also fail in this configuration because they generate incorrect redirects.</span></span> <span data-ttu-id="ea54a-537"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 在 IIS 後端執行時，會新增並設定轉送標頭中介軟體，但沒有相符的 Linux (Apache 或 Nginx 整合) 自動組態。</span><span class="sxs-lookup"><span data-stu-id="ea54a-537"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> adds and configures Forwarded Headers Middleware when running behind IIS, but there's no matching automatic configuration for Linux (Apache or Nginx integration).</span></span>

<span data-ttu-id="ea54a-538">若要從非 IIS 案例的 Proxy 轉送配置，請新增並設定轉送標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ea54a-538">To forward the scheme from the proxy in non-IIS scenarios, add and configure Forwarded Headers Middleware.</span></span> <span data-ttu-id="ea54a-539">在 `Startup.ConfigureServices` 中，使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ea54a-539">In `Startup.ConfigureServices`, use the following code:</span></span>

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

if (string.Equals(
    Environment.GetEnvironmentVariable("ASPNETCORE_FORWARDEDHEADERS_ENABLED"), 
    "true", StringComparison.OrdinalIgnoreCase))
{
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = ForwardedHeaders.XForwardedFor | 
            ForwardedHeaders.XForwardedProto;
        // Only loopback proxies are allowed by default.
        // Clear that restriction because forwarders are enabled by explicit 
        // configuration.
        options.KnownNetworks.Clear();
        options.KnownProxies.Clear();
    });
}
```

## <a name="troubleshoot"></a><span data-ttu-id="ea54a-540">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ea54a-540">Troubleshoot</span></span>

<span data-ttu-id="ea54a-541">當標頭未如預期般傳送時，請啟用[記錄功能](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-541">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="ea54a-542">如果記錄提供的資訊不足，無法針對問題進行疑難排解，則請列舉伺服器所收到的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-542">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="ea54a-543">使用內嵌中介軟體將要求標頭寫入應用程式回應或記錄標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-543">Use inline middleware to write request headers to an app response or log the headers.</span></span> 

<span data-ttu-id="ea54a-544">若要將標頭寫入至應用程式的回應，將下列終端機內嵌中介軟體緊接著放置於 `Startup.Configure` 中對 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 的呼叫之後：</span><span class="sxs-lookup"><span data-stu-id="ea54a-544">To write the headers to the app's response, place the following terminal inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`:</span></span>

```csharp
app.Run(async (context) =>
{
    context.Response.ContentType = "text/plain";

    // Request method, scheme, and path
    await context.Response.WriteAsync(
        $"Request Method: {context.Request.Method}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Path: {context.Request.Path}{Environment.NewLine}");

    // Headers
    await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

    foreach (var header in context.Request.Headers)
    {
        await context.Response.WriteAsync($"{header.Key}: " +
            $"{header.Value}{Environment.NewLine}");
    }

    await context.Response.WriteAsync(Environment.NewLine);

    // Connection: RemoteIp
    await context.Response.WriteAsync(
        $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
});
```

<span data-ttu-id="ea54a-545">您可以寫入至記錄，而不是回應本文。</span><span class="sxs-lookup"><span data-stu-id="ea54a-545">You can write to logs instead of the response body.</span></span> <span data-ttu-id="ea54a-546">寫入至記錄可讓網站在偵錯時正常運作。</span><span class="sxs-lookup"><span data-stu-id="ea54a-546">Writing to logs allows the site to function normally while debugging.</span></span>

<span data-ttu-id="ea54a-547">若要寫入記錄而不是回應本文：</span><span class="sxs-lookup"><span data-stu-id="ea54a-547">To write logs rather than to the response body:</span></span>

* <span data-ttu-id="ea54a-548">將 `ILogger<Startup>` 插入至 `Startup` 類別，如[在啟動中建立記錄](xref:fundamentals/logging/index#create-logs-in-startup)中所述。</span><span class="sxs-lookup"><span data-stu-id="ea54a-548">Inject `ILogger<Startup>` into the `Startup` class as described in [Create logs in Startup](xref:fundamentals/logging/index#create-logs-in-startup).</span></span>
* <span data-ttu-id="ea54a-549">將下列終端機內嵌中介軟體緊接著放置於 `Startup.Configure` 中對 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 的呼叫之後。</span><span class="sxs-lookup"><span data-stu-id="ea54a-549">Place the following inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // Request method, scheme, and path
    _logger.LogDebug("Request Method: {Method}", context.Request.Method);
    _logger.LogDebug("Request Scheme: {Scheme}", context.Request.Scheme);
    _logger.LogDebug("Request Path: {Path}", context.Request.Path);

    // Headers
    foreach (var header in context.Request.Headers)
    {
        _logger.LogDebug("Header: {Key}: {Value}", header.Key, header.Value);
    }

    // Connection: RemoteIp
    _logger.LogDebug("Request RemoteIp: {RemoteIpAddress}", 
        context.Connection.RemoteIpAddress);

    await next();
});
```

<span data-ttu-id="ea54a-550">處理後，`X-Forwarded-{For|Proto|Host}` 值會移至 `X-Original-{For|Proto|Host}`。</span><span class="sxs-lookup"><span data-stu-id="ea54a-550">When processed, `X-Forwarded-{For|Proto|Host}` values are moved to `X-Original-{For|Proto|Host}`.</span></span> <span data-ttu-id="ea54a-551">如果指定的標頭中有多個值，「轉送的標頭中介軟體」會以相反順序 (從右至左) 處理標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-551">If there are multiple values in a given header, Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="ea54a-552">預設的 `ForwardLimit` 是 `1` (一)，因此除非 `ForwardLimit` 的值增加，否則只會處理標頭中最右邊的值。</span><span class="sxs-lookup"><span data-stu-id="ea54a-552">The default `ForwardLimit` is `1` (one), so only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span>

<span data-ttu-id="ea54a-553">要求的原始遠端 IP 必須符合 `KnownProxies` 或 `KnownNetworks` 清單中的項目，系統才會處理轉送的標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-553">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before forwarded headers are processed.</span></span> <span data-ttu-id="ea54a-554">這可藉由不接受來自不受信任 Proxy 的轉送子，以限制標頭詐騙行為。</span><span class="sxs-lookup"><span data-stu-id="ea54a-554">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span> <span data-ttu-id="ea54a-555">當偵測到未知的 Proxy 時，記錄會指出 Proxy 的位址：</span><span class="sxs-lookup"><span data-stu-id="ea54a-555">When an unknown proxy is detected, logging indicates the address of the proxy:</span></span>

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

<span data-ttu-id="ea54a-556">在上述範例中，10.0.0.100 是 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ea54a-556">In the preceding example, 10.0.0.100 is a proxy server.</span></span> <span data-ttu-id="ea54a-557">如果伺服器是信任的 Proxy，請將伺服器的 IP 位址新增至 `Startup.ConfigureServices` 中的 `KnownProxies` (或將信任的網路新增至 `KnownNetworks`)。</span><span class="sxs-lookup"><span data-stu-id="ea54a-557">If the server is a trusted proxy, add the server's IP address to `KnownProxies` (or add a trusted network to `KnownNetworks`) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ea54a-558">如需詳細資訊，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)一節。</span><span class="sxs-lookup"><span data-stu-id="ea54a-558">For more information, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) section.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> <span data-ttu-id="ea54a-559">只允許信任的 Proxy 以及網路轉送標頭。</span><span class="sxs-lookup"><span data-stu-id="ea54a-559">Only allow trusted proxies and networks to forward headers.</span></span> <span data-ttu-id="ea54a-560">否則，[IP 詐騙](https://www.iplocation.net/ip-spoofing)攻擊有可能發生。</span><span class="sxs-lookup"><span data-stu-id="ea54a-560">Otherwise, [IP spoofing](https://www.iplocation.net/ip-spoofing) attacks are possible.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea54a-561">其他資源</span><span class="sxs-lookup"><span data-stu-id="ea54a-561">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
* <span data-ttu-id="ea54a-562">[Microsoft Security Advisory CVE-2018-0787：ASP.NET Core 權限提高弱點](https://github.com/aspnet/Announcements/issues/295) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="ea54a-562">[Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability](https://github.com/aspnet/Announcements/issues/295)</span></span>

::: moniker-end
