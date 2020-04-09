---
title: ASP.NET核心的 gRPC 中的身份驗證和授權
author: jamesnk
description: 瞭解如何在 gRPC 中使用身份驗證和授權ASP.NET核心。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 12/05/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: c0312b186bbb35e3b802984484b7213016d8bf04
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78964434"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a>ASP.NET核心的 gRPC 中的身份驗證和授權

由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/)[(如何下載)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-calling-a-grpc-service"></a>對呼叫 gRPC 服務的使用者進行身份驗證

gRPC 可與[ASP.NET 核心身份驗證](xref:security/authentication/identity)一起使用,以便將使用者與每個呼叫相關聯。

下面是使用 gRPC`Startup.Configure`和 ASP.NET 核心身份驗證的範例:

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> 註冊ASP.NET核心身份驗證中間件的順序很重要。 永遠打電話`UseAuthentication``UseAuthorization`,`UseRouting`前後與`UseEndpoints`之前 。

需要配置應用在調用期間使用的身份驗證機制。 身份驗證配置被添加到中`Startup.ConfigureServices`,並且會根據應用使用的身份驗證機制而不同。 有關如何保護ASP.NET核心應用的範例,請參閱[身份驗證範例](xref:security/authentication/samples)。

設定驗證後,可以透過 gRPC 服務方法`ServerCallContext`存取使用者 。

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a>承載權杖身份驗證

用戶端可以提供用於身份驗證的訪問權杖。 伺服器驗證權杖並使用它來標識使用者。

在伺服器上,使用[JWT 承載器中間件](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)配置承載令牌身份驗證。

在 .NET gRPC 用戶端中,權杖可以作為標頭隨呼叫一起發送:

```csharp
public bool DoAuthenticatedCall(
    Ticketer.TicketerClient client, string token)
{
    var headers = new Metadata();
    headers.Add("Authorization", $"Bearer {token}");

    var request = new BuyTicketsRequest { Count = 1 };
    var response = await client.BuyTicketsAsync(request, headers);

    return response.Success;
}
```

在通道`ChannelCredentials`上配置是使用 gRPC 調用將權杖發送到服務的替代方法。 每次進行 gRPC 調用時都會運行憑據,這避免了在多個位置編寫代碼以自行傳遞權杖的需要。

以下範例的認證設定通道以送出權杖與每個 gRPC 呼叫:

```csharp
private static GrpcChannel CreateAuthenticatedChannel(string address)
{
    var credentials = CallCredentials.FromInterceptor((context, metadata) =>
    {
        if (!string.IsNullOrEmpty(_token))
        {
            metadata.Add("Authorization", $"Bearer {_token}");
        }
        return Task.CompletedTask;
    });

    // SslCredentials is used here because this channel is using TLS.
    // CallCredentials can't be used with ChannelCredentials.Insecure on non-TLS channels.
    var channel = GrpcChannel.ForAddress(address, new GrpcChannelOptions
    {
        Credentials = ChannelCredentials.Create(new SslCredentials(), credentials)
    });
    return channel;
}
```

### <a name="client-certificate-authentication"></a>用戶端憑證驗證

用戶端可以提供用於身份驗證的用戶端證書。 [憑證身份驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)在 TLS 級別進行,遠在它到達 ASP.NET 核心之前。 當要求將 ASP.NET 的酷時,[客戶端憑證認證套件](xref:security/authentication/certauth)允許`ClaimsPrincipal`您將憑證解析為 。

> [!NOTE]
> 需要將主機配置為接受客戶端證書。 有關在 Kestrel、IIS 與 Azure 中接受客戶端憑證的資訊,請參閱[將主機設定為需要憑證](xref:security/authentication/certauth#configure-your-host-to-require-certificates)。

在 .NET gRPC 用戶端中,`HttpClientHandler`將用戶端憑證 新增到該憑證中,然後用於建立 gRPC 用戶端:

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC channel
    var channel = GrpcChannel.ForAddress(baseAddress, new GrpcChannelOptions
    {
        HttpClient = new HttpClient(handler)
    });

    return new Ticketer.TicketerClient(channel);
}
```

### <a name="other-authentication-mechanisms"></a>其他認證機制

許多ASP.NET核心支援的身份驗證機制都使用 gRPC:

* Azure Active Directory
* 用戶端憑證
* 識別伺服器
* JWT 權杖
* OAuth 2.0
* OpenID Connect
* WS-同盟

關於伺服器上設定認證的詳細資訊,請參考[ASP.NET 核心身份驗證](xref:security/authentication/identity)。

設定 gRPC 用戶端以使用身份驗證將取決於您使用的身份驗證機制。 前面的無記名權杖和客戶端憑證範例顯示 gRPC 客戶端設定為使用 gRPC 呼叫送出身份驗證中繼資料的幾種方法:

* 強類型 gRPC`HttpClient`客戶端在內部使用。 認證可以在[HTTPClientHandler](/dotnet/api/system.net.http.httpclienthandler)上設定,也可以透過自訂[HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler)實體新增`HttpClient`到 。
* 每個 gRPC 調`CallOptions`用都有一個可選參數。 可以使用選項的標頭集合發送自定義標頭。

> [!NOTE]
> Windows 身份驗證(NTLM/Kerberos/協商)不能與 gRPC 一起使用。 gRPC 需要 HTTP/2,HTTP/2 不支援 Windows 身份驗證。

## <a name="authorize-users-to-access-services-and-service-methods"></a>授權使用者存取服務和服務方法

默認情況下,服務中的所有方法都可以由未經身份驗證的使用者調用。 要要求身份驗證,請將[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性應用於服務:

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

可以使用建構函數參數和`[Authorize]`屬性的屬性限制存取僅對匹配特定[授權策略](xref:security/authorization/policies)的使用者。 例如,如果您有一個稱為`MyAuthorizationPolicy`的 自定義授權策略,請確保只有匹配該策略的使用者才能使用以下代碼存取服務:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

各個服務方法也可以應用該`[Authorize]`屬性。 如果當前使用者**與應用於方法和**類的策略不匹配,則傳回錯誤給呼叫者:

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
    public override Task<AvailableTicketsResponse> GetAvailableTickets(
        Empty request, ServerCallContext context)
    {
        // ... buy tickets for the current user ...
    }

    [Authorize("Administrators")]
    public override Task<BuyTicketsResponse> RefundTickets(
        BuyTicketsRequest request, ServerCallContext context)
    {
        // ... refund tickets (something only Administrators can do) ..
    }
}
```

## <a name="additional-resources"></a>其他資源

* [ASP.NET核心中的承載權杖身份驗證](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [ASP.NET 核心中設定客戶端憑證認證](xref:security/authentication/certauth)
