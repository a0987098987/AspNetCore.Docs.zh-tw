---
title: 設定 ASP.NET Core 以與 Proxy 伺服器和負載平衡器搭配運作
author: guardrex
description: 了解裝載在 Proxy 伺服器和負載平衡器 (通常會遮蔽重要要求資訊) 後方之應用程式的設定。
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 1797962d6eada9c48b31cd94e2c7481380301a0d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276772"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>設定 ASP.NET Core 以與 Proxy 伺服器和負載平衡器搭配運作

作者：[Luke Latham](https://github.com/guardrex) 和 [Chris Ross](https://github.com/Tratcher)

在建議的 ASP.NET Core 設定中，是使用 IIS/ASP.NET Core 模組、Nginx 或 Apache 來裝載應用程式。 Proxy 伺服器、負載平衡器及其他網路設備通常會在要求觸達應用程式之前，遮蔽要求的相關資訊：

* 透過 HTTP 作為 Proxy 來處理 HTTPS 要求時，原始配置 (HTTPS) 會遺失而必須在標頭中轉送。
* 由於應用程式會從 Proxy 而不是從網際網路或公司網路上的真實來源收到要求，因此必須也在標頭中轉送原始用戶端 IP 位址。

此資訊在要求處理方面可能相當重要，例如在重新導向、驗證、連結產生、原則評估及用戶端地理位置方面。

## <a name="forwarded-headers"></a>轉送的標頭

依照慣例，Proxy 會以 HTTP 標頭轉送資訊。

| 頁首 | 描述 |
| ------ | ----------- |
| X-Forwarded-For | 針對在 Proxy 鏈結中起始要求及後續 Proxy 的用戶端，保存用戶端的相關資訊。 此參數可能包含 IP 位址 (以及視需要可能會有連接埠號碼)。 在 Proxy 伺服器鏈結中，第一個參數會指出起始要求的用戶端。 後面接著後續的 Proxy 識別碼。 鏈結中的最後一個 Proxy 並不在參數清單中。 最後一個 Proxy 的 IP 位址 (以及視需要會有連接埠號碼) 會在傳輸層以遠端 IP 位址的形式提供。 |
| X-Forwarded-Proto | 原始配置的值 (HTTP/HTTPS)。 如果要求周遊了多個 Proxy，則此值也可能是一個配置清單。 |
| X-Forwarded-Host | 主機標頭欄位的原始值。 通常，Proxy 不會修改主機標頭。 如需有關權限提高弱點的資訊，請參閱 [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) \(英文\)，此弱點會影響 Proxy 不會驗證或限制主機標頭為已知有效值的系統。 |

來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的「轉送的標頭中介軟體」會讀取這些標頭，並填入 [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext) 上相關聯的欄位。

中介軟體會更新：

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; 使用 `X-Forwarded-For` 標頭值來設定。 額外的設定會影響中介軟體設定 `RemoteIpAddress`的方式。 如需詳細資料，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; 使用 `X-Forwarded-Proto` 標頭值來設定。
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; 使用 `X-Forwarded-Host` 標頭值來設定。

請注意，並非所有網路設備在新增 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭時都不含額外設定。 如果透過 Proxy 傳送的要求在觸達應用程式時未包含這些標頭，請向您的設備製造商尋求指引。

您可以設定「轉送的標頭中介軟體」的[預設設定](#forwarded-headers-middleware-options)。 預設設定包括：

* 在應用程式與要求的來源之間只有「一個 Proxy」。
* 針對已知的 Proxy 和已知的網路，只會設定回送位址。

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express 和 ASP.NET Core 模組

當應用程式是在 IIS 和 ASP.NET Core 模組的後方執行時，「IIS 整合中介軟體」會預設啟用「轉送的標頭中介軟體」。 由於有與轉送標頭相關的信任考量 (例如 [IP 詐騙](https://www.iplocation.net/ip-spoofing))，因此「轉送的標頭中介軟體」在啟用後會先在中介軟體管線中搭配 ASP.NET Core 模組限定的設定來執行。 中介軟體會經設定來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，並限制成單一 localhost Proxy。 如果需要額外的設定，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>其他 Proxy 伺服器和負載平衡器案例

除了使用「IIS 整合中介軟體」之外，都未預設啟用「轉送的標頭中介軟體」。 必須啟用「轉送的標頭中介軟體」，應用程式才能使用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).來處理轉送的標頭。 啟用此中介軟體之後，如果未將任何 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 指定給中介軟體，則預設的 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) 會是 [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。

請使用 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 來設定中介軟體以轉送 `Startup.ConfigureServices` 中的 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭。 在呼叫其他中介軟體之前，請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法：

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
> 如果沒有在 `Startup.ConfigureServices` 中指定任何 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)，或使用 [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_) 來直接指定給擴充方法，則要轉送的預設標頭會是 [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。 必須在 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) 屬性設定要轉送的標頭。

## <a name="nginx-configuration"></a>Nginx 組態

若要轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，請參閱[在 Linux 上使用 Nginx 裝載：設定 Nginx](xref:host-and-deploy/linux-nginx#configure-nginx)。 如需詳細資訊，請參閱 [NGINX：使用轉送的標頭](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)。

## <a name="apache-configuration"></a>Apache 組態

`X-Forwarded-For` 會自動新增 (請參閱 [Apache 模組 mod_proxy：反向 Proxy 要求標頭](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers))。 如需如何轉送 `X-Forwarded-Proto` 標頭的資訊，請參閱[在 Linux 上使用 Apache 裝載：設定 Apache](xref:host-and-deploy/linux-apache#configure-apache)。

## <a name="forwarded-headers-middleware-options"></a>轉送的標頭中介軟體選項

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 可控制「轉送的標頭中介軟體」的行為：

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

::: moniker range="<= aspnetcore-2.0"
| 選項 | 描述 |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername) 所指定的標頭。<br><br>預設為 `X-Forwarded-For`。 |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | 識別應該處理哪個轉送子。 如需適用的欄位清單，請參閱 [ForwardedHeaders 列舉](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。 指派給此屬性的一般值為 <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>。<br><br>預設值為 [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。 |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername) 所指定的標頭。<br><br>預設為 `X-Forwarded-Host`。 |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername) 所指定的標頭。<br><br>預設為 `X-Forwarded-Proto`。 |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | 限制所處理標頭中的項目數。 設定為 `null` 可停用限制，但應該只有在已設定 `KnownProxies` 或 `KnownNetworks` 的情況下，才這樣做。<br><br>預設為 1。 |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | 可從中接受轉送標頭的已知 Proxy 位址範圍。 請使用無類別網域間路由 (CIDR) 標記法來提供 IP 範圍。<br><br>預設值為包含單一 `IPAddress.Loopback` 項目的 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)>。 |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | 可從中接受轉送標頭的已知 Proxy 位址。 請使用 `KnownProxies` 來指定確切的相符 IP 位址。<br><br>預設值為包含單一 `IPAddress.IPv6Loopback` 項目的 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)>。 |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername) 所指定的標頭。<br><br>預設為 `X-Original-For`。 |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername) 所指定的標頭。<br><br>預設為 `X-Original-Host`。 |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername) 所指定的標頭。<br><br>預設為 `X-Original-Proto`。 |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | 要求所處理 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) 的標頭值數目必須同步。<br><br>ASP.NET Core 1.x 中的預設值為 `true`。 ASP.NET Core 2.0 或更新版本中的預設值為 `false`。 |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| 選項 | 描述 |
| ------ | ----------- |
| AllowedHosts | 依據 `X-Forwarded-Host` 標頭將主機限制成所提供的值。<ul><li>比較值時，會使用序數忽略大小寫的方式來比較。</li><li>必須排除連接埠號碼。</li><li>如果清單空白，即表示允許所有主機。</li><li>最上層的萬用字元 `*` 代表會允許所有非空白的主機。</li><li>允許使用子網域萬用字元，但不會比對出根網域。 例如，`*.contoso.com` 會比對出子網域 `foo.contoso.com`，但不會比對出根網域 `contoso.com`。</li><li>允許使用 Unicode 主機名稱，但會轉換成 [Punycode](https://tools.ietf.org/html/rfc3492) 來進行比對。</li><li>[IPv6 addresses](https://tools.ietf.org/html/rfc4291) 必須包含週框方括號，並採用[慣例格式](https://tools.ietf.org/html/rfc4291#section-2.2) (例如 `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`)。 IPv6 位址並未特別設計成會檢查不同格式間是否具有邏輯相等性，因此不會執行標準化。</li><li>如果無法限制可允許的主機，可能會讓攻擊者偽造服務所產生的連結。</li></ul>預設值為空白的 [IList\<string>](/dotnet/api/system.collections.generic.ilist-1)。 |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername) 所指定的標頭。<br><br>預設為 `X-Forwarded-For`。 |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | 識別應該處理哪個轉送子。 如需適用的欄位清單，請參閱 [ForwardedHeaders 列舉](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。 指派給此屬性的一般值為 <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>。<br><br>預設值為 [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)。 |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername) 所指定的標頭。<br><br>預設為 `X-Forwarded-Host`。 |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername) 所指定的標頭。<br><br>預設為 `X-Forwarded-Proto`。 |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | 限制所處理標頭中的項目數。 設定為 `null` 可停用限制，但應該只有在已設定 `KnownProxies` 或 `KnownNetworks` 的情況下，才這樣做。<br><br>預設為 1。 |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | 可從中接受轉送標頭的已知 Proxy 位址範圍。 請使用無類別網域間路由 (CIDR) 標記法來提供 IP 範圍。<br><br>預設值為包含單一 `IPAddress.Loopback` 項目的 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)>。 |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | 可從中接受轉送標頭的已知 Proxy 位址。 請使用 `KnownProxies` 來指定確切的相符 IP 位址。<br><br>預設值為包含單一 `IPAddress.IPv6Loopback` 項目的 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)>。 |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername) 所指定的標頭。<br><br>預設為 `X-Original-For`。 |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername) 所指定的標頭。<br><br>預設為 `X-Original-Host`。 |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername) 所指定的標頭。<br><br>預設為 `X-Original-Proto`。 |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | 要求所處理 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) 的標頭值數目必須同步。<br><br>ASP.NET Core 1.x 中的預設值為 `true`。 ASP.NET Core 2.0 或更新版本中的預設值為 `false`。 |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>情節和使用案例

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>當可以新增轉送的標頭且所有要求都安全時

在某些情況下，可能無法將轉送的標頭新增至透過 Proxy 傳送給應用程式的要求。 如果 Proxy 強制要求所有公用外部要求都必須是 HTTPS，您可以在使用任何類型的中介軟體之前，先手動在 `Startup.Configure` 中設定該配置：

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

在開發或預備環境中，可以使用環境變數或其他組態設定來停用此程式碼。

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>處理路徑基底和變更要求路徑的 Proxy

有些 Proxy 會原封不動傳送路徑，但其中含有應移除才能正確路由傳送的應用程式基底路徑。 [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) 中介軟體會將該路徑分割成 [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)，以及將應用程式基底路徑分割成 [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)。

如果 `/foo` 是以 `/foo/api/1` 形式傳送之 Proxy 路徑的應用程式基底路徑，中介軟體就會使用下列命令，將 `Request.PathBase` 設定為 `/foo`，以及將 `Request.Path` 設定為 `/api/1`：

```csharp
app.UsePathBase("/foo");
```

反向再次呼叫應用程式時，則會重新套用原始路徑和路徑基底。 如需有關中介軟體如何處理順序的詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)。

如果 Proxy 會修剪路徑 (例如，將 `/foo/api/1` 轉送給 `/api/1`)，請設定要求的 [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) 屬性來修正重新導向和連結：

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

如果 Proxy 會新增路徑資料，請使用 [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) 並指派給 [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) 屬性，以捨棄部分路徑來修正重新導向和連結：

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

當標頭未如預期般傳送時，請啟用[記錄功能](xref:fundamentals/logging/index)。 如果記錄提供的資訊不足，無法針對問題進行疑難排解，則請列舉伺服器所收到的要求標頭。 您可以使用內嵌中介軟體將標頭寫入到回應中：

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

請確定伺服器收到具有預期值的 X-Forwarded-* 標頭。 如果指定的標頭中有多個值，請注意，「轉送的標頭中介軟體」會以相反順序 (從右至左) 處理標頭。

要求的原始遠端 IP 必須符合 `KnownProxies` 或 `KnownNetworks` 清單中的項目，系統才會處理 X-Forwarded-For。 這可藉由不接受來自不受信任 Proxy 的轉送子，以限制標頭詐騙行為。

## <a name="additional-resources"></a>其他資源

* [Microsoft Security Advisory CVE-2018-0787：ASP.NET Core 權限提高弱點](https://github.com/aspnet/Announcements/issues/295) \(英文\)
