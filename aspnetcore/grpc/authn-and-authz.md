---
title: ASP.NET Core 的 gRPC 中的驗證和授權
author: jamesnk
description: 瞭解如何在 gRPC 中使用驗證和授權來 ASP.NET Core。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/26/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 34f7f8a5a22159329b3d6c4524943434c460c7fb
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602433"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="bbb1d-103">ASP.NET Core 的 gRPC 中的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="bbb1d-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="bbb1d-104">依[James 牛頓-王](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="bbb1d-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="bbb1d-105">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/)[(如何下載)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="bbb1d-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="bbb1d-106">驗證呼叫 gRPC 服務的使用者</span><span class="sxs-lookup"><span data-stu-id="bbb1d-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="bbb1d-107">gRPC 可與[ASP.NET Core 驗證](xref:security/authentication/identity)搭配使用, 以將使用者與每個呼叫產生關聯。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="bbb1d-108">以下是使用 gRPC 和 ASP.NET Core `Startup.Configure`驗證的範例:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        routes.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> <span data-ttu-id="bbb1d-109">您註冊 ASP.NET Core authentication 中介軟體的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="bbb1d-110">一律在`UseAuthentication` `UseAuthorization` 前後`UseEndpoints`呼叫和`UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="bbb1d-111">一旦設定好驗證之後, 就可以透過 gRPC 服務方法`ServerCallContext`來存取使用者。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="bbb1d-112">持有人權杖驗證</span><span class="sxs-lookup"><span data-stu-id="bbb1d-112">Bearer token authentication</span></span>

<span data-ttu-id="bbb1d-113">用戶端可以提供存取權杖以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="bbb1d-114">伺服器會驗證權杖, 並使用它來識別使用者。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="bbb1d-115">在伺服器上, 持有人權杖驗證是使用[JWT 持有人中介軟體](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)來設定。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="bbb1d-116">在 .NET gRPC 用戶端中, 可以使用標頭的呼叫來傳送權杖:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

### <a name="client-certificate-authentication"></a><span data-ttu-id="bbb1d-117">用戶端憑證驗證</span><span class="sxs-lookup"><span data-stu-id="bbb1d-117">Client certificate authentication</span></span>

<span data-ttu-id="bbb1d-118">用戶端也可以提供用於驗證的用戶端憑證。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="bbb1d-119">[憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)會在 TLS 層級進行, 長時間才會到達 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="bbb1d-120">當要求進入 ASP.NET Core 時,[用戶端憑證驗證封裝](xref:security/authentication/certauth)可讓您將憑證解析成`ClaimsPrincipal`。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="bbb1d-121">主機必須設定為接受用戶端憑證。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="bbb1d-122">如需在 Kestrel、IIS 和 Azure 中接受用戶端憑證的資訊, 請參閱[設定您的主機以要求憑證](xref:security/authentication/certauth#configure-your-host-to-require-certificates)。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="bbb1d-123">在 .net gRPC 用戶端中, 會將用戶端憑證`HttpClientHandler`新增至, 然後用來建立 gRPC 用戶端:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC client
    var httpClient = new HttpClient(handler);
    httpClient.BaseAddress = new Uri(baseAddress);

    return GrpcClient.Create<Ticketer.TicketerClient>(httpClient);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="bbb1d-124">其他驗證機制</span><span class="sxs-lookup"><span data-stu-id="bbb1d-124">Other authentication mechanisms</span></span>

<span data-ttu-id="bbb1d-125">許多 ASP.NET Core 支援的驗證機制可與 gRPC 搭配使用:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-125">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="bbb1d-126">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bbb1d-126">Azure Active Directory</span></span>
* <span data-ttu-id="bbb1d-127">用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="bbb1d-127">Client Certificate</span></span>
* <span data-ttu-id="bbb1d-128">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="bbb1d-128">IdentityServer</span></span>
* <span data-ttu-id="bbb1d-129">JWT 權杖</span><span class="sxs-lookup"><span data-stu-id="bbb1d-129">JWT Token</span></span>
* <span data-ttu-id="bbb1d-130">OAuth 2。0</span><span class="sxs-lookup"><span data-stu-id="bbb1d-130">OAuth 2.0</span></span>
* <span data-ttu-id="bbb1d-131">OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="bbb1d-131">OpenID Connect</span></span>
* <span data-ttu-id="bbb1d-132">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="bbb1d-132">WS-Federation</span></span>

<span data-ttu-id="bbb1d-133">如需有關在伺服器上設定驗證的詳細資訊, 請參閱[ASP.NET Core 驗證](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-133">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="bbb1d-134">將 gRPC 用戶端設定為使用驗證, 將取決於您所使用的驗證機制。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-134">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="bbb1d-135">先前的持有人權杖和用戶端憑證範例顯示 gRPC 用戶端可設定為使用 gRPC 呼叫來傳送驗證中繼資料的幾種方式:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-135">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="bbb1d-136">強型別 gRPC 客戶`HttpClient`端會在內部使用。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-136">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="bbb1d-137">您可以在上[`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)設定驗證, 或藉由將自訂`HttpClient` [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler)實例新增至。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-137">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="bbb1d-138">每個 gRPC 呼叫都有`CallOptions`一個選擇性引數。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-138">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="bbb1d-139">您可以使用選項的標頭集合來傳送自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-139">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="bbb1d-140">Windows 驗證 (NTLM/Kerberos/Negotiate) 無法與 gRPC 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-140">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="bbb1d-141">gRPC 需要 HTTP/2, 而且 HTTP/2 不支援 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-141">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="bbb1d-142">授權使用者存取服務和服務方法</span><span class="sxs-lookup"><span data-stu-id="bbb1d-142">Authorize users to access services and service methods</span></span>

<span data-ttu-id="bbb1d-143">根據預設, 服務中的所有方法都可以由未經驗證的使用者呼叫。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-143">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="bbb1d-144">若要要求驗證, 請將[[授權]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性套用至服務:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-144">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="bbb1d-145">您可以使用`[Authorize]`屬性的「函式引數」和「屬性」, 限制只有符合特定[授權原則](xref:security/authorization/policies)的使用者才能存取。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-145">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="bbb1d-146">例如, 如果您有稱為`MyAuthorizationPolicy`的自訂授權原則, 請確定只有符合該原則的使用者可以使用下列程式碼來存取服務:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-146">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="bbb1d-147">個別的`[Authorize]`服務方法也可以套用屬性。</span><span class="sxs-lookup"><span data-stu-id="bbb1d-147">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="bbb1d-148">如果目前使用者不符合**同時**套用至方法和類別的原則, 則會將錯誤傳回給呼叫者:</span><span class="sxs-lookup"><span data-stu-id="bbb1d-148">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="bbb1d-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="bbb1d-149">Additional resources</span></span>

* [<span data-ttu-id="bbb1d-150">ASP.NET Core 中的持有人權杖驗證</span><span class="sxs-lookup"><span data-stu-id="bbb1d-150">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="bbb1d-151">在 ASP.NET Core 中設定用戶端憑證驗證</span><span class="sxs-lookup"><span data-stu-id="bbb1d-151">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
