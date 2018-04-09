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
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>設定使用 proxy 伺服器及負載平衡器的 ASP.NET Core

由[Luke Latham](https://github.com/guardrex)和[Chris Ross](https://github.com/Tratcher)

中的 ASP.NET Core 之建議組態，是使用 ASP.NET 核心模組、 Nginx 或 Apache 裝載應用程式。 Proxy 伺服器、 負載平衡器、 與其他網路裝置中通常會到達應用程式之前遮住要求的相關資訊：

* 當透過 HTTP proxy 的 HTTPS 要求，原始的配置 (HTTPS) 會遺失，而且必須在標頭要轉送。
* 因為應用程式 proxy，而非其網際網路或公司網路上，則為 true 來源從收到要求時，原始的用戶端 IP 位址也必須轉送標頭中。

這項資訊可能很重要的要求處理，例如在重新導向、 驗證、 連結產生、 原則評估及用戶端 geoloation。

## <a name="forwarded-headers"></a>轉送的標頭

依照慣例，proxy 會轉送 HTTP 標頭中的資訊。

| 頁首 | 描述 |
| ------ | ----------- |
| X-Forwarded-For | 保留用戶端起始的要求和後續 proxy 的 proxy 鏈結中的相關資訊。 這個參數可能會包含 IP 位址 （甚至是 （選擇性） 連接埠號碼）。 Proxy 伺服器的鏈結，第一個參數會指出用戶端第一次提出要求。 請遵循後續 proxy 識別碼。 鏈結中的最後一個 proxy 不在參數清單中。 最後一個 proxy 的 IP 位址，及選擇性連接埠號碼，會提供傳輸層級的遠端 IP 位址。 |
| X-Forwarded-Proto | 原始的配置 (HTTP/HTTPS) 的值。 如果要求已周遊多個 proxy，值也可能是配置的清單。 |
| X-Forwarded-Host | 主機標頭欄位的原始值。 通常，proxy 不會修改主機標頭。 請參閱[Microsoft 安全性摘要報告 CVE-2018年-0787年](https://github.com/aspnet/Announcements/issues/295)如需會影響的系統不會驗證 proxy 其中一個權限提高權限弱點或已知的良好值 restict 主機標頭資訊。 |

轉送標頭中介軟體，從[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)封裝、 讀取這些標頭並填入相關聯的欄位上[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)。 

中介軟體的更新：

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash;設定使用`X-Forwarded-For`標頭值。 其他設定會影響設定中介軟體的方式`RemoteIpAddress`。 如需詳細資訊，請參閱[轉送標頭中介軟體選項](#forwarded-headers-middleware-options)。
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash;設定使用`X-Forwarded-Proto`標頭值。
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash;設定使用`X-Forwarded-Host`標頭值。

請注意，並非所有的網路裝置中新增`X-Forwarded-For`和`X-Forwarded-Proto`標頭，而不需要額外的設定。 如果代理的要求不包含這些標頭，其到達應用程式時，請參閱應用裝置製造商的指引。

轉送標頭中介軟體[預設設定](#forwarded-headers-middleware-options)可以設定。 預設設定如下：

* 只有*一個 proxy*應用程式之間的要求來源。
* 已知的 proxy 設定及已知網路只有回送位址。

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express 和 ASP.NET 核心模組

IIS 和 ASP.NET Core 模組背後執行應用程式時，預設的 IIS Integration 中介軟體會啟用轉送標頭中介軟體。 轉送標頭中介軟體會啟動執行中介軟體管線，以限制特定組態中的第一個問題轉送標頭與信任的 ASP.NET 核心模組 (例如， [IP 詐騙](https://www.iplocation.net/ip-spoofing))。 中介軟體已設定為轉送`X-Forwarded-For`和`X-Forwarded-Proto`標頭，並且限制為單一 localhost proxy。 如果需要額外的設定，請參閱[轉送標頭中介軟體選項](#forwarded-headers-middleware-options)。

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>其他的 proxy 伺服器和負載平衡器案例

除了使用 IIS Integration 中介軟體，預設不啟用轉送標頭中介軟體。 轉送程序標頭與應用程式必須啟用轉送標頭中介軟體[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)。 如果未啟用中, 介軟體之後[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)中介軟體，預設值指定[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)是[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

設定與中的介軟體[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)轉送`X-Forwarded-For`和`X-Forwarded-Proto`中的標頭`Startup.ConfigureServices`。 叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`然後再呼叫其他中介軟體：

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
> 如果沒有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)中指定`Startup.ConfigureServices`或直接擴充方法也具有[UseForwardedHeaders （IApplicationBuilder、 ForwardedHeadersOptions）](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_)，預設值標頭要轉送[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)屬性必須設定要轉寄的標頭。

## <a name="forwarded-headers-middleware-options"></a>轉送的標頭中介軟體選項

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)控制轉送標頭中介軟體的行為：

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| 選項 | 描述 |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | 使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername)。<br><br>預設值為 `X-Forwarded-For`。 |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | 識別應該處理哪一個轉寄站。 請參閱[ForwardedHeaders 列舉](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)套用的欄位清單。 一般的值指派給這個屬性為<code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>。<br><br>預設值是[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。 |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | 使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername)。<br><br>預設值為 `X-Forwarded-Host`。 |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | 使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername)。<br><br>預設值為 `X-Forwarded-Proto`。 |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | 限制處理的標頭中的項目數目。 設定為`null`停用限制，但這應該只執行`KnownProxies`或`KnownNetworks`設定。<br><br>預設為 1。 |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | 位址範圍的已知的 proxy，以接受來自轉送標頭。 提供使用無類別網域間路由選擇 (CIDR) 標記法的 IP 範圍。<br><br>預設值是[IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> 包含的單一項目`IPAddress.Loopback`。 |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | 已知的 proxy，以接受來自轉送標頭的位址。 使用`KnownProxies`若要指定正確的 IP 位址比對。<br><br>預設值是[IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> 包含的單一項目`IPAddress.IPv6Loopback`。 |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | 使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername)。<br><br>預設值為 `X-Original-For`。 |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | 使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername)。<br><br>預設值為 `X-Original-Host`。 |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | 使用這個屬性，而不是所指定所指定的標頭[ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername)。<br><br>預設值為 `X-Original-Proto`。 |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | 要求的標頭值之間的同步處理數[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)正在處理。<br><br>預設值在 ASP.NET Core 1.x 是`true`。 在 ASP.NET Core 2.0 或更新版本的預設值是`false`。 |

## <a name="scenarios-and-use-cases"></a>案例與使用案例

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>當不可能加入轉送標頭和所有的要求是安全

在某些情況下，它不可能將轉送標頭新增至此應用程式的要求。 如果 proxy 會強制執行所有公用的外部要求 HTTPS，可以在中手動設定配置`Startup.Configure`之前使用任何類型的中介軟體：

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

這段程式碼可以使用環境變數或其他組態設定開發或預備環境中的停用。

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>處理基底路徑和變更要求路徑的 proxy

某些 proxy 傳遞路徑不變，但與應用程式應該被移除，路由的基底路徑可正常運作。 [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase)中介軟體會分割成路徑[HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)和應用程式基底路徑[HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)。

如果`/foo`是 proxy 傳遞路徑做為應用程式基底路徑`/foo/api/1`中, 介軟體集`Request.PathBase`至`/foo`和`Request.Path`至`/api/1`使用下列命令：

```csharp
app.UsePathBase("/foo");
```

中介軟體反過來再次呼叫時，就會重新套用的原始路徑和基底路徑。 如需有關處理訂單的中介軟體的詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)。

如果 proxy 修剪路徑 (例如，轉送`/foo/api/1`至`/api/1`)，修正重新導向，並藉由設定要求的連結[PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)屬性：

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

如果 proxy 新增路徑資料，捨棄修正所使用的重新導向和連結路徑的一部分[StartsWithSegments （PathString，PathString）](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__)並將指派給[路徑](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)屬性：

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

## <a name="troubleshoot"></a>疑難排解

當標頭不轉送如預期般時，啟用[記錄](xref:fundamentals/logging/index)。 如果記錄檔沒有提供足夠的資訊來排解疑難問題，列舉伺服器所接收的要求標頭。 可以寫入使用內嵌的中介軟體應用程式回應標頭：

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

請確認轉送-X * 預期的值與伺服器收到的標頭。 如果給定的標頭中有多個值，請注意轉送標頭中介軟體以反向順序由右至左的處理程序標頭。

要求的原始遠端 IP 必須符合中的項目`KnownProxies`或`KnownNetworks`列出 X 轉送的處理之前。 這會限制標頭詐騙不接受來自不受信任的 proxy 轉寄站。

## <a name="additional-resources"></a>其他資源

* [Microsoft 安全性摘要報告 CVE-2018年-0787: ASP.NET Core 的權限的弱點可能會提高權限](https://github.com/aspnet/Announcements/issues/295)
