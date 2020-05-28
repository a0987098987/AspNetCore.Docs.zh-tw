---
標題： author： description： monikerRange： ms. author： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---

# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a>ASP.NET Core 的 gRPC 中的驗證和授權

依[James 牛頓-王](https://twitter.com/jamesnk)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-calling-a-grpc-service"></a>驗證呼叫 gRPC 服務的使用者

gRPC 可與[ASP.NET Core 驗證](xref:security/authentication/identity)搭配使用，以將使用者與每個呼叫產生關聯。

以下是 `Startup.Configure` 使用 gRPC 和 ASP.NET Core 驗證的範例：

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
> 您註冊 ASP.NET Core authentication 中介軟體的順序很重要。 一律在前後呼叫 `UseAuthentication` 和 `UseAuthorization` `UseRouting` `UseEndpoints` 。

需要設定您的應用程式在呼叫期間所使用的驗證機制。 會在中新增驗證設定 `Startup.ConfigureServices` ，而且會根據您的應用程式所使用的驗證機制而有所不同。 如需如何保護 ASP.NET Core 應用程式的範例，請參閱[驗證範例](xref:security/authentication/samples)。

一旦設定好驗證之後，就可以透過 gRPC 服務方法來存取使用者 `ServerCallContext` 。

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a>持有人權杖驗證

用戶端可以提供存取權杖以進行驗證。 伺服器會驗證權杖，並使用它來識別使用者。

在伺服器上，持有人權杖驗證是使用[JWT 持有人中介軟體](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)來設定。

在 .NET gRPC 用戶端中，可以使用標頭的呼叫來傳送權杖：

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

`ChannelCredentials`在通道上設定是使用 gRPC 呼叫將權杖傳送至服務的另一種方式。 每次進行 gRPC 呼叫時，就會執行此認證，這可避免需要在多個位置撰寫程式碼來自行傳遞權杖。

下列範例中的認證會設定通道，以使用每個 gRPC 呼叫來傳送權杖：

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

用戶端也可以提供用於驗證的用戶端憑證。 [憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)會在 TLS 層級進行，長時間才會到達 ASP.NET Core。 當要求進入 ASP.NET Core 時，[用戶端憑證驗證封裝](xref:security/authentication/certauth)可讓您將憑證解析成 `ClaimsPrincipal` 。

> [!NOTE]
> 主機必須設定為接受用戶端憑證。 如需在 Kestrel、IIS 和 Azure 中接受用戶端憑證的資訊，請參閱[設定您的主機以要求憑證](xref:security/authentication/certauth#configure-your-host-to-require-certificates)。

在 .NET gRPC 用戶端中，會將用戶端憑證新增至 `HttpClientHandler` ，然後用來建立 gRPC 用戶端：

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
        HttpHandler = handler
    });

    return new Ticketer.TicketerClient(channel);
}
```

### <a name="other-authentication-mechanisms"></a>其他驗證機制

許多 ASP.NET Core 支援的驗證機制可與 gRPC 搭配使用：

* Azure Active Directory
* 用戶端憑證
* IdentityServer
* JWT 權杖
* OAuth 2.0
* OpenID Connect
* WS-同盟

如需有關在伺服器上設定驗證的詳細資訊，請參閱[ASP.NET Core 驗證](xref:security/authentication/identity)。

將 gRPC 用戶端設定為使用驗證，將取決於您所使用的驗證機制。 先前的持有人權杖和用戶端憑證範例顯示 gRPC 用戶端可設定為使用 gRPC 呼叫來傳送驗證中繼資料的幾種方式：

* 強型別 gRPC 用戶端會 `HttpClient` 在內部使用。 您可以在[HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler)上設定驗證，或藉由將自訂[HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler)實例新增至 `HttpClient` 。
* 每個 gRPC 呼叫都有一個選擇性 `CallOptions` 引數。 您可以使用選項的標頭集合來傳送自訂標頭。

> [!NOTE]
> Windows 驗證（NTLM/Kerberos/Negotiate）無法與 gRPC 搭配使用。 gRPC 需要 HTTP/2，而且 HTTP/2 不支援 Windows 驗證。

## <a name="authorize-users-to-access-services-and-service-methods"></a>授權使用者存取服務和服務方法

根據預設，服務中的所有方法都可以由未經驗證的使用者呼叫。 若要要求驗證，請將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至服務：

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

您可以使用屬性的「函式引數」和「屬性」， `[Authorize]` 限制只有符合特定[授權原則](xref:security/authorization/policies)的使用者才能存取。 例如，如果您有稱為的自訂授權原則 `MyAuthorizationPolicy` ，請確定只有符合該原則的使用者可以使用下列程式碼來存取服務：

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

個別的服務方法也可以套用 `[Authorize]` 屬性。 如果目前使用者不符合**同時**套用至方法和類別的原則，則會將錯誤傳回給呼叫者：

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

* [ASP.NET Core 中的持有人權杖驗證](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [在 ASP.NET Core 中設定用戶端憑證驗證](xref:security/authentication/certauth)
