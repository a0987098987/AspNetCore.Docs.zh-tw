---
title: 設定 ASP.NET Core 以與 Proxy 伺服器和負載平衡器搭配運作
author: guardrex
description: 了解裝載在 Proxy 伺服器和負載平衡器 (通常會遮蔽重要要求資訊) 後方之應用程式的設定。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/12/2019
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 3243f5d3254e6585ff9ca48900a3326aa9b6f502
ms.sourcegitcommit: 8a36be1bfee02eba3b07b7a86085ec25c38bae6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219170"
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
| X-Forwarded-Host | 主機標頭欄位的原始值。 通常，Proxy 不會修改主機標頭。 如需有關權限提高弱點的資訊，請參閱 [Microsoft 資訊安全諮詢 CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) \(英文\)，此弱點會影響 Proxy 不會驗證或限制主機標頭為已知有效值的系統。 |

來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的「轉送的標頭中介軟體」會讀取這些標頭，並填入 <xref:Microsoft.AspNetCore.Http.HttpContext> 上相關聯的欄位。

中介軟體會更新：

* [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; 使用 `X-Forwarded-For` 標頭值來設定。 額外的設定會影響中介軟體設定 `RemoteIpAddress`的方式。 如需詳細資料，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。
* [HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; 使用 `X-Forwarded-Proto` 標頭值來設定。
* [HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; 使用 `X-Forwarded-Host` 標頭值來設定。

您可以設定「轉送的標頭中介軟體」的[預設設定](#forwarded-headers-middleware-options)。 預設設定包括：

* 在應用程式與要求的來源之間只有「一個 Proxy」。
* 針對已知的 Proxy 和已知的網路，只會設定回送位址。
* 轉送標頭名稱為 `X-Forwarded-For` 和 `X-Forwarded-Proto`。

並非所有網路設備在新增 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭時都不含額外組態。 如果透過 Proxy 傳送的要求在觸達應用程式時未包含這些標頭，請向您的設備製造商尋求指引。 如果設備使用的名稱不是 `X-Forwarded-For` 和 `X-Forwarded-Proto`，請設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> 與 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> 選項，以符合設備使用的標頭名稱。 如需詳細資訊，請參閱[轉送標頭中介軟體選項](#forwarded-headers-middleware-options)和[使用不同標頭名稱之 Proxy 的組態](#configuration-for-a-proxy-that-uses-different-header-names)。

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express 和 ASP.NET Core 模組

當應用程式是在 IIS 和 ASP.NET Core 模組的後方進行[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)裝載時，[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)預設會啟用「轉送的標頭中介軟體」。 由於有與轉送標頭相關的信任考量 (例如 [IP 詐騙](https://www.iplocation.net/ip-spoofing))，因此「轉送的標頭中介軟體」在啟用後會先在中介軟體管線中搭配 ASP.NET Core 模組限定的設定來執行。 中介軟體會經設定來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，並限制成單一 localhost Proxy。 如果需要額外的設定，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)。

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>其他 Proxy 伺服器和負載平衡器案例

除了在[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)裝載時使用 [IIS 整合](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)之外，都未預設啟用「轉送的標頭中介軟體」。 必須啟用「轉送的標頭中介軟體」，應用程式才能使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 來處理轉送的標頭。 啟用此中介軟體之後，如果未將任何 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 指定給中介軟體，則預設的 [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) 會是 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。

搭配 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 設定中介軟體，以在 `Startup.ConfigureServices` 中轉送 `X-Forwarded-For` 與 `X-Forwarded-Proto` 標頭。 在呼叫其他中介軟體之前，請先在 `Startup.Configure` 中叫用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 方法：

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
> 若未在 `Startup.ConfigureServices` 中指定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，或未使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 直接指定到擴充方法，則要轉送的預設標頭是 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。 必須使用要轉送的標頭設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> 屬性。

## <a name="nginx-configuration"></a>Nginx 組態

若要轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭，請參閱 <xref:host-and-deploy/linux-nginx#configure-nginx>。 如需詳細資訊，請參閱 [NGINX：使用轉送的標頭](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) \(英文\)。

## <a name="apache-configuration"></a>Apache 組態

`X-Forwarded-For` 會自動新增 (請參閱 [Apache 模組 mod_proxy：反向 Proxy 要求標頭](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers) \(英文\))。 如需如何轉送 `X-Forwarded-Proto` 標頭的資訊，請參閱 <xref:host-and-deploy/linux-apache#configure-apache>。

## <a name="forwarded-headers-middleware-options"></a>轉送的標頭中介軟體選項

<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 會控制轉送標頭中介軟體的行為。 下列範例會變更預設值：

* 將轉送標頭中的項目數限制為 `2`。
* 新增已知的 Proxy 位址 `127.0.10.1`。
* 將轉送標頭名稱從預設的 `X-Forwarded-For` 變更為 `X-Forwarded-For-My-Custom-Header-Name`。

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| 選項 | 描述 |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> | 依據 `X-Forwarded-Host` 標頭將主機限制成所提供的值。<ul><li>比較值時，會使用序數忽略大小寫的方式來比較。</li><li>必須排除連接埠號碼。</li><li>如果清單空白，即表示允許所有主機。</li><li>最上層的萬用字元 `*` 代表會允許所有非空白的主機。</li><li>允許使用子網域萬用字元，但不會比對出根網域。 例如，`*.contoso.com` 會比對出子網域 `foo.contoso.com`，但不會比對出根網域 `contoso.com`。</li><li>允許使用 Unicode 主機名稱，但會轉換成 [Punycode](https://tools.ietf.org/html/rfc3492) 來進行比對。</li><li>[IPv6 addresses](https://tools.ietf.org/html/rfc4291) 必須包含週框方括號，並採用[慣例格式](https://tools.ietf.org/html/rfc4291#section-2.2) (例如 `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`)。 IPv6 位址並未特別設計成會檢查不同格式間是否具有邏輯相等性，因此不會執行標準化。</li><li>如果無法限制可允許的主機，可能會讓攻擊者偽造服務所產生的連結。</li></ul>預設值是空的 `IList<string>`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName) 所指定的標頭。 當 Proxy/轉寄站未使用 `X-Forwarded-For` 標頭，而使用其他標頭轉送資訊時，會使用此選項。<br><br>預設為 `X-Forwarded-For`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | 識別應該處理哪個轉送子。 如需適用的欄位清單，請參閱 [ForwardedHeaders 列舉](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。 指派給此屬性的一般值為 `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`。<br><br>預設值為 [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName) 所指定的標頭。 當 Proxy/轉寄站未使用 `X-Forwarded-Host` 標頭，而使用其他標頭轉送資訊時，會使用此選項。<br><br>預設為 `X-Forwarded-Host`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName) 所指定的標頭。 當 Proxy/轉寄站未使用 `X-Forwarded-Proto` 標頭，而使用其他標頭轉送資訊時，會使用此選項。<br><br>預設為 `X-Forwarded-Proto`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | 限制所處理標頭中的項目數。 設定為 `null` 可停用限制，但應該只有在已設定 `KnownProxies` 或 `KnownNetworks` 的情況下，才這樣做。 設置非 `null` 值是一種預防措施 (但不是保證)，以防止設定不正確的 Proxy 和來自網路上的旁路惡意要求。<br><br>「轉送的標頭中介軟體」會以相反順序 (從右至左) 處理標頭。 如果使用預設值 (`1`)，除非 `ForwardLimit` 的值增加，否則只會處理標頭中最右邊的值。<br><br>預設為 `1`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | 可從中接受轉送標頭的已知網路位址範圍。 請使用無類別網域間路由 (CIDR) 標記法來提供 IP 範圍。<br><br>若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 提供 IPv4 位址。 請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。 透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。 如需詳細資訊，請參閱[以 IPv6 位址表示的 IPv4 位址設定](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address)一節。<br><br>預設值為包含單一 `IPAddress.Loopback` 項目的 `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>>。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | 可從中接受轉送標頭的已知 Proxy 位址。 請使用 `KnownProxies` 來指定確切的相符 IP 位址。<br><br>若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 提供 IPv4 位址。 請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。 透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。 如需詳細資訊，請參閱[以 IPv6 位址表示的 IPv4 位址設定](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address)一節。<br><br>預設值為包含單一 `IPAddress.IPv6Loopback` 項目的 `IList`\<<xref:System.Net.IPAddress>>。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName) 所指定的標頭。<br><br>預設為 `X-Original-For`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName) 所指定的標頭。<br><br>預設為 `X-Original-Host`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | 使用此屬性所指定的標頭，而不是 [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName) 所指定的標頭。<br><br>預設為 `X-Original-Proto`。 |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | 要求所處理 [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) 的標頭值數目必須同步。<br><br>ASP.NET Core 1.x 中的預設值為 `true`。 ASP.NET Core 2.0 或更新版本中的預設值為 `false`。 |

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

有些 Proxy 會原封不動傳送路徑，但其中含有應移除才能正確路由傳送的應用程式基底路徑。 [UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) 中介軟體會將該路徑分割成 [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path)，以及將應用程式基底路徑分割成 [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase)。

如果 `/foo` 是以 `/foo/api/1` 形式傳送之 Proxy 路徑的應用程式基底路徑，中介軟體就會使用下列命令，將 `Request.PathBase` 設定為 `/foo`，以及將 `Request.Path` 設定為 `/api/1`：

```csharp
app.UsePathBase("/foo");
```

反向再次呼叫應用程式時，則會重新套用原始路徑和路徑基底。 如需中介軟體順序處理的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。

如果 Proxy 會修剪路徑 (例如，將 `/foo/api/1` 轉送給 `/api/1`)，請設定要求的 [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) 屬性來修正重新導向和連結：

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

如果 Proxy 會新增路徑資料，請使用 <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> 並指派給 <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> 屬性，以捨棄部分路徑來修正重新導向和連結：

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

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a>使用不同標頭名稱之 Proxy 的組態

如果 Proxy 未使用名為 `X-Forwarded-For` 和 `X-Forwarded-Proto` 的標頭轉送 Proxy 位址/連接埠與原始配置資訊，請設定 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> 與 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> 選項，以符合 Proxy 使用的標頭名稱：

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a>以 IPv6 位址表示的 IPv4 位址設定

若伺服器使用雙模式通訊端，會以 IPv6 格式 (例如，IPv4 中的 `10.0.0.1` 在 IPv6 中以 `::ffff:10.0.0.1` 表示) 或 `::ffff:a00:1` 提供 IPv4 位址。 請參閱 [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*)。 透過查看 [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*) 以判斷是否需要此格式。

在下列範例中，提供轉寄標頭的網路位址已以 IPv6 格式新增到 `KnownNetworks` 清單中。

IPv4 位址：`10.11.12.1/8`

轉換的 IPv6 位址：`::ffff:10.11.12.1`  
轉換的首碼長度：104

您也能以十六進位格式提供位址 (`10.11.12.1` 在 IPv6 中以 `::ffff:0a0b:0c01` 表示)。 當您將 IPv4 位址轉換為 IPv6 時，請加入 96 到 CIDR 首碼長度 (在此範例中為 `8`) 以將額外的 `::ffff:` IPv6 首碼 (8 + 96 = 104) 納入考慮。 

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

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a>轉送 Linux 和非 IIS 反向 Proxy 的配置

如果部署至 Azure Linux App Service、Azure Linux 虛擬機器 (VM)，或 IIS 之外的任何其他反向 Proxy 後端，則呼叫 <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> 和 <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> 的這些應用程式會使站台進入無限迴圈。 反向 Proxy 會終止 TLS，且正確的要求配置不知道有 Kestrel。 OAuth 和 OIDC 在此組態中也失敗，因為它們會產生不正確的重新導向。 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 在 IIS 後端執行時，會新增並設定轉送標頭中介軟體，但沒有相符的 Linux (Apache 或 Nginx 整合) 自動組態。

若要從非 IIS 案例的 Proxy 轉送配置，請新增並設定轉送標頭中介軟體。 在 `Startup.ConfigureServices` 中，使用下列程式碼：

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

## <a name="troubleshoot"></a>疑難排解

當標頭未如預期般傳送時，請啟用[記錄功能](xref:fundamentals/logging/index)。 如果記錄提供的資訊不足，無法針對問題進行疑難排解，則請列舉伺服器所收到的要求標頭。 使用內嵌中介軟體將要求標頭寫入應用程式回應或記錄標頭。 

若要將標頭寫入至應用程式的回應，將下列終端機內嵌中介軟體緊接著放置於 `Startup.Configure` 中對 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 的呼叫之後：

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

您可以寫入至記錄，而不是回應本文。 寫入至記錄可讓網站在偵錯時正常運作。

若要寫入記錄而不是回應本文：

* 將 `ILogger<Startup>` 插入至 `Startup` 類別，如[在啟動中建立記錄](xref:fundamentals/logging/index#create-logs-in-startup)中所述。
* 將下列終端機內嵌中介軟體緊接著放置於 `Startup.Configure` 中對 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 的呼叫之後。

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

處理後，`X-Forwarded-{For|Proto|Host}` 值會移至 `X-Original-{For|Proto|Host}`。 如果指定的標頭中有多個值，「轉送的標頭中介軟體」會以相反順序 (從右至左) 處理標頭。 預設的 `ForwardLimit` 是 `1` (一)，因此除非 `ForwardLimit` 的值增加，否則只會處理標頭中最右邊的值。

要求的原始遠端 IP 必須符合 `KnownProxies` 或 `KnownNetworks` 清單中的項目，系統才會處理轉送的標頭。 這可藉由不接受來自不受信任 Proxy 的轉送子，以限制標頭詐騙行為。 當偵測到未知的 Proxy 時，記錄會指出 Proxy 的位址：

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

在上述範例中，10.0.0.100 是 Proxy 伺服器。 如果伺服器是信任的 Proxy，請將伺服器的 IP 位址新增至 `Startup.ConfigureServices` 中的 `KnownProxies` (或將信任的網路新增至 `KnownNetworks`)。 如需詳細資訊，請參閱[轉送的標頭中介軟體選項](#forwarded-headers-middleware-options)一節。

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> 只允許信任的 Proxy 以及網路轉送標頭。 否則，[IP 詐騙](https://www.iplocation.net/ip-spoofing)攻擊有可能發生。

## <a name="certificate-forwarding"></a>憑證轉送 

### <a name="on-azure"></a>在 Azure 上

請參閱 [Azure 文件](/azure/app-service/app-service-web-configure-tls-mutual-auth)以設定 Azure Web Apps。 在您應用程式的 `Startup.Configure` 方法中，於 `app.UseAuthentication();` 呼叫的前面新增下列程式碼：

```csharp
app.UseCertificateForwarding();
```

您也需要設定憑證轉送中介軟體以指定 Azure 使用的標頭名稱。 在您應用程式的 `Startup.ConfigureServices` 方法中，新增下列程式碼以設定標頭，中介軟體會在其中建置憑證：

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="with-other-web-proxies"></a>搭配其他 Web Proxy

如果您使用的 Proxy 不是 IIS 或 Azure Web Apps 應用程式要求路由，請設定您的 Proxy 轉送它在 HTTP 標頭中收到的憑證。 在您應用程式的 `Startup.Configure` 方法中，於 `app.UseAuthentication();` 呼叫的前面新增下列程式碼：

```csharp
app.UseCertificateForwarding();
```

您也需要設定憑證轉送中介軟體以指定標頭名稱。 在您應用程式的 `Startup.ConfigureServices` 方法中，新增下列程式碼以設定標頭，中介軟體會在其中建置憑證：

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

最後，如果 Proxy 不是使用 base64 編碼憑證 (和 Nginx 一樣)，則請設定 `HeaderConverter` 選項。 請考慮 `Startup.ConfigureServices` 中的下列範例：

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

## <a name="additional-resources"></a>其他資源

* <xref:host-and-deploy/web-farm>
* [Microsoft 資訊安全諮詢 CVE-2018-0787：ASP.NET Core 權限提高弱點](https://github.com/aspnet/Announcements/issues/295) \(英文\)
