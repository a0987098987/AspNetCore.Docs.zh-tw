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
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="28681-103">ASP.NET核心的 gRPC 中的身份驗證和授權</span><span class="sxs-lookup"><span data-stu-id="28681-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="28681-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="28681-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="28681-105">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/)[(如何下載)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="28681-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="28681-106">對呼叫 gRPC 服務的使用者進行身份驗證</span><span class="sxs-lookup"><span data-stu-id="28681-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="28681-107">gRPC 可與[ASP.NET 核心身份驗證](xref:security/authentication/identity)一起使用,以便將使用者與每個呼叫相關聯。</span><span class="sxs-lookup"><span data-stu-id="28681-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="28681-108">下面是使用 gRPC`Startup.Configure`和 ASP.NET 核心身份驗證的範例:</span><span class="sxs-lookup"><span data-stu-id="28681-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="28681-109">註冊ASP.NET核心身份驗證中間件的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="28681-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="28681-110">永遠打電話`UseAuthentication``UseAuthorization`,`UseRouting`前後與`UseEndpoints`之前 。</span><span class="sxs-lookup"><span data-stu-id="28681-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="28681-111">需要配置應用在調用期間使用的身份驗證機制。</span><span class="sxs-lookup"><span data-stu-id="28681-111">The authentication mechanism your app uses during a call needs to be configured.</span></span> <span data-ttu-id="28681-112">身份驗證配置被添加到中`Startup.ConfigureServices`,並且會根據應用使用的身份驗證機制而不同。</span><span class="sxs-lookup"><span data-stu-id="28681-112">Authentication configuration is added in `Startup.ConfigureServices` and will be different depending upon the authentication mechanism your app uses.</span></span> <span data-ttu-id="28681-113">有關如何保護ASP.NET核心應用的範例,請參閱[身份驗證範例](xref:security/authentication/samples)。</span><span class="sxs-lookup"><span data-stu-id="28681-113">For examples of how to secure ASP.NET Core apps, see [Authentication samples](xref:security/authentication/samples).</span></span>

<span data-ttu-id="28681-114">設定驗證後,可以透過 gRPC 服務方法`ServerCallContext`存取使用者 。</span><span class="sxs-lookup"><span data-stu-id="28681-114">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="28681-115">承載權杖身份驗證</span><span class="sxs-lookup"><span data-stu-id="28681-115">Bearer token authentication</span></span>

<span data-ttu-id="28681-116">用戶端可以提供用於身份驗證的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="28681-116">The client can provide an access token for authentication.</span></span> <span data-ttu-id="28681-117">伺服器驗證權杖並使用它來標識使用者。</span><span class="sxs-lookup"><span data-stu-id="28681-117">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="28681-118">在伺服器上,使用[JWT 承載器中間件](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)配置承載令牌身份驗證。</span><span class="sxs-lookup"><span data-stu-id="28681-118">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="28681-119">在 .NET gRPC 用戶端中,權杖可以作為標頭隨呼叫一起發送:</span><span class="sxs-lookup"><span data-stu-id="28681-119">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

<span data-ttu-id="28681-120">在通道`ChannelCredentials`上配置是使用 gRPC 調用將權杖發送到服務的替代方法。</span><span class="sxs-lookup"><span data-stu-id="28681-120">Configuring `ChannelCredentials` on a channel is an alternative way to send the token to the service with gRPC calls.</span></span> <span data-ttu-id="28681-121">每次進行 gRPC 調用時都會運行憑據,這避免了在多個位置編寫代碼以自行傳遞權杖的需要。</span><span class="sxs-lookup"><span data-stu-id="28681-121">The credential is run each time a gRPC call is made, which avoids the need to write code in multiple places to pass the token yourself.</span></span>

<span data-ttu-id="28681-122">以下範例的認證設定通道以送出權杖與每個 gRPC 呼叫:</span><span class="sxs-lookup"><span data-stu-id="28681-122">The credential in the following example configures the channel to send the token with every gRPC call:</span></span>

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

### <a name="client-certificate-authentication"></a><span data-ttu-id="28681-123">用戶端憑證驗證</span><span class="sxs-lookup"><span data-stu-id="28681-123">Client certificate authentication</span></span>

<span data-ttu-id="28681-124">用戶端可以提供用於身份驗證的用戶端證書。</span><span class="sxs-lookup"><span data-stu-id="28681-124">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="28681-125">[憑證身份驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)在 TLS 級別進行,遠在它到達 ASP.NET 核心之前。</span><span class="sxs-lookup"><span data-stu-id="28681-125">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="28681-126">當要求將 ASP.NET 的酷時,[客戶端憑證認證套件](xref:security/authentication/certauth)允許`ClaimsPrincipal`您將憑證解析為 。</span><span class="sxs-lookup"><span data-stu-id="28681-126">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="28681-127">需要將主機配置為接受客戶端證書。</span><span class="sxs-lookup"><span data-stu-id="28681-127">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="28681-128">有關在 Kestrel、IIS 與 Azure 中接受客戶端憑證的資訊,請參閱[將主機設定為需要憑證](xref:security/authentication/certauth#configure-your-host-to-require-certificates)。</span><span class="sxs-lookup"><span data-stu-id="28681-128">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="28681-129">在 .NET gRPC 用戶端中,`HttpClientHandler`將用戶端憑證 新增到該憑證中,然後用於建立 gRPC 用戶端:</span><span class="sxs-lookup"><span data-stu-id="28681-129">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

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

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="28681-130">其他認證機制</span><span class="sxs-lookup"><span data-stu-id="28681-130">Other authentication mechanisms</span></span>

<span data-ttu-id="28681-131">許多ASP.NET核心支援的身份驗證機制都使用 gRPC:</span><span class="sxs-lookup"><span data-stu-id="28681-131">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="28681-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="28681-132">Azure Active Directory</span></span>
* <span data-ttu-id="28681-133">用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="28681-133">Client Certificate</span></span>
* <span data-ttu-id="28681-134">識別伺服器</span><span class="sxs-lookup"><span data-stu-id="28681-134">IdentityServer</span></span>
* <span data-ttu-id="28681-135">JWT 權杖</span><span class="sxs-lookup"><span data-stu-id="28681-135">JWT Token</span></span>
* <span data-ttu-id="28681-136">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="28681-136">OAuth 2.0</span></span>
* <span data-ttu-id="28681-137">OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="28681-137">OpenID Connect</span></span>
* <span data-ttu-id="28681-138">WS-同盟</span><span class="sxs-lookup"><span data-stu-id="28681-138">WS-Federation</span></span>

<span data-ttu-id="28681-139">關於伺服器上設定認證的詳細資訊,請參考[ASP.NET 核心身份驗證](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="28681-139">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="28681-140">設定 gRPC 用戶端以使用身份驗證將取決於您使用的身份驗證機制。</span><span class="sxs-lookup"><span data-stu-id="28681-140">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="28681-141">前面的無記名權杖和客戶端憑證範例顯示 gRPC 客戶端設定為使用 gRPC 呼叫送出身份驗證中繼資料的幾種方法:</span><span class="sxs-lookup"><span data-stu-id="28681-141">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="28681-142">強類型 gRPC`HttpClient`客戶端在內部使用。</span><span class="sxs-lookup"><span data-stu-id="28681-142">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="28681-143">認證可以在[HTTPClientHandler](/dotnet/api/system.net.http.httpclienthandler)上設定,也可以透過自訂[HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler)實體新增`HttpClient`到 。</span><span class="sxs-lookup"><span data-stu-id="28681-143">Authentication can be configured on [HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="28681-144">每個 gRPC 調`CallOptions`用都有一個可選參數。</span><span class="sxs-lookup"><span data-stu-id="28681-144">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="28681-145">可以使用選項的標頭集合發送自定義標頭。</span><span class="sxs-lookup"><span data-stu-id="28681-145">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="28681-146">Windows 身份驗證(NTLM/Kerberos/協商)不能與 gRPC 一起使用。</span><span class="sxs-lookup"><span data-stu-id="28681-146">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="28681-147">gRPC 需要 HTTP/2,HTTP/2 不支援 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="28681-147">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="28681-148">授權使用者存取服務和服務方法</span><span class="sxs-lookup"><span data-stu-id="28681-148">Authorize users to access services and service methods</span></span>

<span data-ttu-id="28681-149">默認情況下,服務中的所有方法都可以由未經身份驗證的使用者調用。</span><span class="sxs-lookup"><span data-stu-id="28681-149">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="28681-150">要要求身份驗證,請將[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性應用於服務:</span><span class="sxs-lookup"><span data-stu-id="28681-150">To require authentication, apply the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="28681-151">可以使用建構函數參數和`[Authorize]`屬性的屬性限制存取僅對匹配特定[授權策略](xref:security/authorization/policies)的使用者。</span><span class="sxs-lookup"><span data-stu-id="28681-151">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="28681-152">例如,如果您有一個稱為`MyAuthorizationPolicy`的 自定義授權策略,請確保只有匹配該策略的使用者才能使用以下代碼存取服務:</span><span class="sxs-lookup"><span data-stu-id="28681-152">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="28681-153">各個服務方法也可以應用該`[Authorize]`屬性。</span><span class="sxs-lookup"><span data-stu-id="28681-153">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="28681-154">如果當前使用者**與應用於方法和**類的策略不匹配,則傳回錯誤給呼叫者:</span><span class="sxs-lookup"><span data-stu-id="28681-154">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="28681-155">其他資源</span><span class="sxs-lookup"><span data-stu-id="28681-155">Additional resources</span></span>

* [<span data-ttu-id="28681-156">ASP.NET核心中的承載權杖身份驗證</span><span class="sxs-lookup"><span data-stu-id="28681-156">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="28681-157">ASP.NET 核心中設定客戶端憑證認證</span><span class="sxs-lookup"><span data-stu-id="28681-157">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
