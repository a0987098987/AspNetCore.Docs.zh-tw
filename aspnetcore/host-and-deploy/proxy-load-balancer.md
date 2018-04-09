---
title: 設定使用 proxy 伺服器及負載平衡器的 ASP.NET Core
author: guardrex
description: 深入了解透過 proxy 伺服器所裝載的應用程式的組態和負載平衡器，通常會遮住重要要求資訊。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="00a45-103">設定使用 proxy 伺服器及負載平衡器的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00a45-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="00a45-104">由[Luke Latham](https://github.com/guardrex)和[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="00a45-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="00a45-105">中的 ASP.NET Core 之建議組態，是使用 ASP.NET 核心模組、 Nginx 或 Apache 裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="00a45-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="00a45-106">Proxy 伺服器、 負載平衡器、 與其他網路裝置中通常會到達應用程式之前遮住要求的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="00a45-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="00a45-107">當透過 HTTP proxy 的 HTTPS 要求，原始的配置 (HTTPS) 會遺失，而且必須在標頭要轉送。</span><span class="sxs-lookup"><span data-stu-id="00a45-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="00a45-108">因為應用程式 proxy，而非其網際網路或公司網路上，則為 true 來源從收到要求時，原始的用戶端 IP 位址也必須轉送標頭中。</span><span class="sxs-lookup"><span data-stu-id="00a45-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="00a45-109">這項資訊可能很重要的要求處理，例如在重新導向、 驗證、 連結產生、 原則評估及用戶端 geoloation。</span><span class="sxs-lookup"><span data-stu-id="00a45-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="00a45-110">轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="00a45-110">Forwarded headers</span></span>

<span data-ttu-id="00a45-111">依照慣例，proxy 會轉送 HTTP 標頭中的資訊。</span><span class="sxs-lookup"><span data-stu-id="00a45-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="00a45-112">頁首</span><span class="sxs-lookup"><span data-stu-id="00a45-112">Header</span></span> | <span data-ttu-id="00a45-113">描述</span><span class="sxs-lookup"><span data-stu-id="00a45-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="00a45-114">X-Forwarded-For</span><span class="sxs-lookup"><span data-stu-id="00a45-114">X-Forwarded-For</span></span> | <span data-ttu-id="00a45-115">保留用戶端起始的要求和後續 proxy 的 proxy 鏈結中的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="00a45-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="00a45-116">這個參數可能會包含 IP 位址 （甚至是 （選擇性） 連接埠號碼）。</span><span class="sxs-lookup"><span data-stu-id="00a45-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="00a45-117">Proxy 伺服器的鏈結，第一個參數會指出用戶端第一次提出要求。</span><span class="sxs-lookup"><span data-stu-id="00a45-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="00a45-118">請遵循後續 proxy 識別碼。</span><span class="sxs-lookup"><span data-stu-id="00a45-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="00a45-119">鏈結中的最後一個 proxy 不在參數清單中。</span><span class="sxs-lookup"><span data-stu-id="00a45-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="00a45-120">最後一個 proxy 的 IP 位址，及選擇性連接埠號碼，會提供傳輸層級的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="00a45-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="00a45-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="00a45-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="00a45-122">原始的配置 (HTTP/HTTPS) 的值。</span><span class="sxs-lookup"><span data-stu-id="00a45-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="00a45-123">如果要求已周遊多個 proxy，值也可能是配置的清單。</span><span class="sxs-lookup"><span data-stu-id="00a45-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="00a45-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="00a45-124">X-Forwarded-Host</span></span> | <span data-ttu-id="00a45-125">主機標頭欄位的原始值。</span><span class="sxs-lookup"><span data-stu-id="00a45-125">The original value of the Host header field.</span></span> <span data-ttu-id="00a45-126">通常，proxy 不會修改主機標頭。</span><span class="sxs-lookup"><span data-stu-id="00a45-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="00a45-127">請參閱[Microsoft 安全性摘要報告 CVE-2018年-0787年](https://github.com/aspnet/Announcements/issues/295)如需會影響的系統不會驗證 proxy 其中一個權限提高權限弱點或已知的良好值 restict 主機標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="00a45-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="00a45-128">轉送標頭中介軟體，從[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)封裝、 讀取這些標頭並填入相關聯的欄位上[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)。</span><span class="sxs-lookup"><span data-stu-id="00a45-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="00a45-129">中介軟體的更新：</span><span class="sxs-lookup"><span data-stu-id="00a45-129">The middleware updates:</span></span>

* <span data-ttu-id="00a45-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash;設定使用`X-Forwarded-For`標頭值。</span><span class="sxs-lookup"><span data-stu-id="00a45-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="00a45-131">其他設定會影響設定中介軟體的方式`RemoteIpAddress`。</span><span class="sxs-lookup"><span data-stu-id="00a45-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="00a45-132">如需詳細資訊，請參閱[轉送標頭中介軟體選項](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="00a45-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="00a45-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash;設定使用`X-Forwarded-Proto`標頭值。</span><span class="sxs-lookup"><span data-stu-id="00a45-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="00a45-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash;設定使用`X-Forwarded-Host`標頭值。</span><span class="sxs-lookup"><span data-stu-id="00a45-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="00a45-135">請注意，並非所有的網路裝置中新增`X-Forwarded-For`和`X-Forwarded-Proto`標頭，而不需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="00a45-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="00a45-136">如果代理的要求不包含這些標頭，其到達應用程式時，請參閱應用裝置製造商的指引。</span><span class="sxs-lookup"><span data-stu-id="00a45-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="00a45-137">轉送標頭中介軟體[預設設定](#forwarded-headers-middleware-options)可以設定。</span><span class="sxs-lookup"><span data-stu-id="00a45-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="00a45-138">預設設定如下：</span><span class="sxs-lookup"><span data-stu-id="00a45-138">The default settings are:</span></span>

* <span data-ttu-id="00a45-139">只有*一個 proxy*應用程式之間的要求來源。</span><span class="sxs-lookup"><span data-stu-id="00a45-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="00a45-140">已知的 proxy 設定及已知網路只有回送位址。</span><span class="sxs-lookup"><span data-stu-id="00a45-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="00a45-141">IIS/IIS Express 和 ASP.NET 核心模組</span><span class="sxs-lookup"><span data-stu-id="00a45-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="00a45-142">IIS 和 ASP.NET Core 模組背後執行應用程式時，預設的 IIS Integration 中介軟體會啟用轉送標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="00a45-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="00a45-143">轉送標頭中介軟體會啟動執行中介軟體管線，以限制特定組態中的第一個問題轉送標頭與信任的 ASP.NET 核心模組 (例如， [IP 詐騙](https://www.iplocation.net/ip-spoofing))。</span><span class="sxs-lookup"><span data-stu-id="00a45-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="00a45-144">中介軟體已設定為轉送`X-Forwarded-For`和`X-Forwarded-Proto`標頭，並且限制為單一 localhost proxy。</span><span class="sxs-lookup"><span data-stu-id="00a45-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="00a45-145">如果需要額外的設定，請參閱[轉送標頭中介軟體選項](#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="00a45-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="00a45-146">其他的 proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="00a45-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="00a45-147">除了使用 IIS Integration 中介軟體，預設不啟用轉送標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="00a45-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="00a45-148">轉送程序標頭與應用程式必須啟用轉送標頭中介軟體[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)。</span><span class="sxs-lookup"><span data-stu-id="00a45-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="00a45-149">如果未啟用中, 介軟體之後[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)中介軟體，預設值指定[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)是[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="00a45-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="00a45-150">設定與中的介軟體[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)轉送`X-Forwarded-For`和`X-Forwarded-Proto`中的標頭`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="00a45-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="00a45-151">叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`然後再呼叫其他中介軟體：</span><span class="sxs-lookup"><span data-stu-id="00a45-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

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
> <span data-ttu-id="00a45-152">如果沒有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)中指定`Startup.ConfigureServices`或直接擴充方法也具有[UseForwardedHeaders （IApplicationBuilder、 ForwardedHeadersOptions）](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_)，預設值標頭要轉送[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。</span><span class="sxs-lookup"><span data-stu-id="00a45-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="00a45-153">[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)屬性必須設定要轉寄的標頭。</span><span class="sxs-lookup"><span data-stu-id="00a45-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="00a45-154">轉送的標頭中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="00a45-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="00a45-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)控制轉送標頭中介軟體的行為：</span><span class="sxs-lookup"><span data-stu-id="00a45-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="00a45-156">選項</span><span class="sxs-lookup"><span data-stu-id="00a45-156">Option</span></span> | <span data-ttu-id="00a45-157">描述</span><span class="sxs-lookup"><span data-stu-id="00a45-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="00a45-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="00a45-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="00a45-159">使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername)。</span><span class="sxs-lookup"><span data-stu-id="00a45-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="00a45-160">預設值為 `X-Forwarded-For`。</span><span class="sxs-lookup"><span data-stu-id="00a45-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="00a45-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="00a45-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="00a45-162">識別應該處理哪一個轉寄站。</span><span class="sxs-lookup"><span data-stu-id="00a45-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="00a45-163">請參閱[ForwardedHeaders 列舉](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)套用的欄位清單。</span><span class="sxs-lookup"><span data-stu-id="00a45-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="00a45-164">一般的值指派給這個屬性為<code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>。</span><span class="sxs-lookup"><span data-stu-id="00a45-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="00a45-165">預設值是[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。</span><span class="sxs-lookup"><span data-stu-id="00a45-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="00a45-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="00a45-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="00a45-167">使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername)。</span><span class="sxs-lookup"><span data-stu-id="00a45-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="00a45-168">預設值為 `X-Forwarded-Host`。</span><span class="sxs-lookup"><span data-stu-id="00a45-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="00a45-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="00a45-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="00a45-170">使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername)。</span><span class="sxs-lookup"><span data-stu-id="00a45-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="00a45-171">預設值為 `X-Forwarded-Proto`。</span><span class="sxs-lookup"><span data-stu-id="00a45-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="00a45-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="00a45-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="00a45-173">限制處理的標頭中的項目數目。</span><span class="sxs-lookup"><span data-stu-id="00a45-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="00a45-174">設定為`null`停用限制，但這應該只執行`KnownProxies`或`KnownNetworks`設定。</span><span class="sxs-lookup"><span data-stu-id="00a45-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="00a45-175">預設為 1。</span><span class="sxs-lookup"><span data-stu-id="00a45-175">The default is 1.</span></span> |
| [<span data-ttu-id="00a45-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="00a45-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="00a45-177">位址範圍的已知的 proxy，以接受來自轉送標頭。</span><span class="sxs-lookup"><span data-stu-id="00a45-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="00a45-178">提供使用無類別網域間路由選擇 (CIDR) 標記法的 IP 範圍。</span><span class="sxs-lookup"><span data-stu-id="00a45-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="00a45-179">預設值是[IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> 包含的單一項目`IPAddress.Loopback`。</span><span class="sxs-lookup"><span data-stu-id="00a45-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="00a45-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="00a45-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="00a45-181">已知的 proxy，以接受來自轉送標頭的位址。</span><span class="sxs-lookup"><span data-stu-id="00a45-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="00a45-182">使用`KnownProxies`若要指定正確的 IP 位址比對。</span><span class="sxs-lookup"><span data-stu-id="00a45-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="00a45-183">預設值是[IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> 包含的單一項目`IPAddress.IPv6Loopback`。</span><span class="sxs-lookup"><span data-stu-id="00a45-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="00a45-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="00a45-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="00a45-185">使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername)。</span><span class="sxs-lookup"><span data-stu-id="00a45-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="00a45-186">預設值為 `X-Original-For`。</span><span class="sxs-lookup"><span data-stu-id="00a45-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="00a45-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="00a45-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="00a45-188">使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername)。</span><span class="sxs-lookup"><span data-stu-id="00a45-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="00a45-189">預設值為 `X-Original-Host`。</span><span class="sxs-lookup"><span data-stu-id="00a45-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="00a45-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="00a45-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="00a45-191">使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername)。</span><span class="sxs-lookup"><span data-stu-id="00a45-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="00a45-192">預設值為 `X-Original-Proto`。</span><span class="sxs-lookup"><span data-stu-id="00a45-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="00a45-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="00a45-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="00a45-194">要求的標頭值之間的同步處理數[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)正在處理。</span><span class="sxs-lookup"><span data-stu-id="00a45-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="00a45-195">預設值在 ASP.NET Core 1.x 是`true`。</span><span class="sxs-lookup"><span data-stu-id="00a45-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="00a45-196">在 ASP.NET Core 2.0 或更新版本的預設值是`false`。</span><span class="sxs-lookup"><span data-stu-id="00a45-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="00a45-197">案例與使用案例</span><span class="sxs-lookup"><span data-stu-id="00a45-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="00a45-198">當不可能加入轉送標頭和所有的要求是安全</span><span class="sxs-lookup"><span data-stu-id="00a45-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="00a45-199">在某些情況下，它不可能將轉送標頭新增至此應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="00a45-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="00a45-200">如果 proxy 會強制執行所有公用的外部要求 HTTPS，可以在中手動設定配置`Startup.Configure`之前使用任何類型的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="00a45-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="00a45-201">這段程式碼可以使用環境變數或其他組態設定開發或預備環境中的停用。</span><span class="sxs-lookup"><span data-stu-id="00a45-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="00a45-202">處理基底路徑和變更要求路徑的 proxy</span><span class="sxs-lookup"><span data-stu-id="00a45-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="00a45-203">某些 proxy 傳遞路徑不變，但與應用程式應該被移除，路由的基底路徑可正常運作。</span><span class="sxs-lookup"><span data-stu-id="00a45-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="00a45-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase)中介軟體會分割成路徑[HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)和應用程式基底路徑[HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)。</span><span class="sxs-lookup"><span data-stu-id="00a45-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="00a45-205">如果`/foo`是 proxy 傳遞路徑做為應用程式基底路徑`/foo/api/1`中, 介軟體集`Request.PathBase`至`/foo`和`Request.Path`至`/api/1`使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="00a45-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="00a45-206">中介軟體反過來再次呼叫時，就會重新套用的原始路徑和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="00a45-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="00a45-207">如需有關處理訂單的中介軟體的詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="00a45-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="00a45-208">如果 proxy 修剪路徑 (例如，轉送`/foo/api/1`至`/api/1`)，修正重新導向，並藉由設定要求的連結[PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)屬性：</span><span class="sxs-lookup"><span data-stu-id="00a45-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="00a45-209">如果 proxy 新增路徑資料，捨棄修正所使用的重新導向和連結路徑的一部分[StartsWithSegments （PathString，PathString）](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__)並將指派給[路徑](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)屬性：</span><span class="sxs-lookup"><span data-stu-id="00a45-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

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

## <a name="troubleshoot"></a><span data-ttu-id="00a45-210">疑難排解</span><span class="sxs-lookup"><span data-stu-id="00a45-210">Troubleshoot</span></span>

<span data-ttu-id="00a45-211">當標頭不轉送如預期般時，啟用[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="00a45-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="00a45-212">如果記錄檔沒有提供足夠的資訊來排解疑難問題，列舉伺服器所接收的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="00a45-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="00a45-213">可以寫入使用內嵌的中介軟體應用程式回應標頭：</span><span class="sxs-lookup"><span data-stu-id="00a45-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
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
    }
}
```

<span data-ttu-id="00a45-214">請確認轉送-X \* 預期的值與伺服器收到的標頭。</span><span class="sxs-lookup"><span data-stu-id="00a45-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="00a45-215">如果給定的標頭中有多個值，請注意轉送標頭中介軟體以反向順序由右至左的處理程序標頭。</span><span class="sxs-lookup"><span data-stu-id="00a45-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="00a45-216">要求的原始遠端 IP 必須符合中的項目`KnownProxies`或`KnownNetworks`列出 X 轉送的處理之前。</span><span class="sxs-lookup"><span data-stu-id="00a45-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="00a45-217">這會限制標頭詐騙不接受來自不受信任的 proxy 轉寄站。</span><span class="sxs-lookup"><span data-stu-id="00a45-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00a45-218">其他資源</span><span class="sxs-lookup"><span data-stu-id="00a45-218">Additional resources</span></span>

* [<span data-ttu-id="00a45-219">Microsoft 安全性摘要報告 CVE-2018年-0787: ASP.NET Core 的權限的弱點可能會提高權限</span><span class="sxs-lookup"><span data-stu-id="00a45-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
