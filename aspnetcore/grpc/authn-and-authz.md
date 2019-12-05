---
title: Authentication and authorization in gRPC for ASP.NET Core
author: jamesnk
description: Learn how to use authentication and authorization in gRPC for ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/13/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 84903ee781588ff525d1dfce6a313e3867794762
ms.sourcegitcommit: 76d7fff62014c3db02564191ab768acea00f1b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2019
ms.locfileid: "74852697"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="58df7-103">Authentication and authorization in gRPC for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58df7-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="58df7-104">By [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="58df7-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="58df7-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="58df7-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="58df7-106">Authenticate users calling a gRPC service</span><span class="sxs-lookup"><span data-stu-id="58df7-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="58df7-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span><span class="sxs-lookup"><span data-stu-id="58df7-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="58df7-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span><span class="sxs-lookup"><span data-stu-id="58df7-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="58df7-109">The order in which you register the ASP.NET Core authentication middleware matters.</span><span class="sxs-lookup"><span data-stu-id="58df7-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="58df7-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="58df7-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="58df7-111">The authentication mechanism your app uses during a call needs to be configured.</span><span class="sxs-lookup"><span data-stu-id="58df7-111">The authentication mechanism your app uses during a call needs to be configured.</span></span> <span data-ttu-id="58df7-112">Authentication configuration is added in `Startup.ConfigureServices` and will be different depending upon the authentication mechanism your app uses.</span><span class="sxs-lookup"><span data-stu-id="58df7-112">Authentication configuration is added in `Startup.ConfigureServices` and will be different depending upon the authentication mechanism your app uses.</span></span> <span data-ttu-id="58df7-113">For examples of how to secure ASP.NET Core apps, see [Authentication samples](xref:security/authentication/samples).</span><span class="sxs-lookup"><span data-stu-id="58df7-113">For examples of how to secure ASP.NET Core apps, see [Authentication samples](xref:security/authentication/samples).</span></span>

<span data-ttu-id="58df7-114">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span><span class="sxs-lookup"><span data-stu-id="58df7-114">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="58df7-115">Bearer token authentication</span><span class="sxs-lookup"><span data-stu-id="58df7-115">Bearer token authentication</span></span>

<span data-ttu-id="58df7-116">The client can provide an access token for authentication.</span><span class="sxs-lookup"><span data-stu-id="58df7-116">The client can provide an access token for authentication.</span></span> <span data-ttu-id="58df7-117">The server validates the token and uses it to identify the user.</span><span class="sxs-lookup"><span data-stu-id="58df7-117">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="58df7-118">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="58df7-118">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="58df7-119">In the .NET gRPC client, the token can be sent with calls as a header:</span><span class="sxs-lookup"><span data-stu-id="58df7-119">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

<span data-ttu-id="58df7-120">Configuring `ChannelCredentials` on a channel is an alternative way to send the token to the service with gRPC calls.</span><span class="sxs-lookup"><span data-stu-id="58df7-120">Configuring `ChannelCredentials` on a channel is an alternative way to send the token to the service with gRPC calls.</span></span> <span data-ttu-id="58df7-121">The credential is run each time a gRPC call is made, which avoids the need to write code in multiple places to pass the token yourself.</span><span class="sxs-lookup"><span data-stu-id="58df7-121">The credential is run each time a gRPC call is made, which avoids the need to write code in multiple places to pass the token yourself.</span></span>

<span data-ttu-id="58df7-122">The credential in the following example configures the channel to send the token with every gRPC call:</span><span class="sxs-lookup"><span data-stu-id="58df7-122">The credential in the following example configures the channel to send the token with every gRPC call:</span></span>

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

### <a name="client-certificate-authentication"></a><span data-ttu-id="58df7-123">用戶端憑證驗證</span><span class="sxs-lookup"><span data-stu-id="58df7-123">Client certificate authentication</span></span>

<span data-ttu-id="58df7-124">A client could alternatively provide a client certificate for authentication.</span><span class="sxs-lookup"><span data-stu-id="58df7-124">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="58df7-125">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58df7-125">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="58df7-126">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="58df7-126">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="58df7-127">The host needs to be configured to accept client certificates.</span><span class="sxs-lookup"><span data-stu-id="58df7-127">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="58df7-128">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span><span class="sxs-lookup"><span data-stu-id="58df7-128">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="58df7-129">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span><span class="sxs-lookup"><span data-stu-id="58df7-129">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

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

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="58df7-130">Other authentication mechanisms</span><span class="sxs-lookup"><span data-stu-id="58df7-130">Other authentication mechanisms</span></span>

<span data-ttu-id="58df7-131">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span><span class="sxs-lookup"><span data-stu-id="58df7-131">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="58df7-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="58df7-132">Azure Active Directory</span></span>
* <span data-ttu-id="58df7-133">用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="58df7-133">Client Certificate</span></span>
* <span data-ttu-id="58df7-134">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="58df7-134">IdentityServer</span></span>
* <span data-ttu-id="58df7-135">JWT 權杖</span><span class="sxs-lookup"><span data-stu-id="58df7-135">JWT Token</span></span>
* <span data-ttu-id="58df7-136">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="58df7-136">OAuth 2.0</span></span>
* <span data-ttu-id="58df7-137">OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="58df7-137">OpenID Connect</span></span>
* <span data-ttu-id="58df7-138">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="58df7-138">WS-Federation</span></span>

<span data-ttu-id="58df7-139">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="58df7-139">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="58df7-140">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span><span class="sxs-lookup"><span data-stu-id="58df7-140">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="58df7-141">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span><span class="sxs-lookup"><span data-stu-id="58df7-141">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="58df7-142">Strongly typed gRPC clients use `HttpClient` internally.</span><span class="sxs-lookup"><span data-stu-id="58df7-142">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="58df7-143">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="58df7-143">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="58df7-144">Each gRPC call has an optional `CallOptions` argument.</span><span class="sxs-lookup"><span data-stu-id="58df7-144">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="58df7-145">Custom headers can be sent using the option's headers collection.</span><span class="sxs-lookup"><span data-stu-id="58df7-145">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="58df7-146">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span><span class="sxs-lookup"><span data-stu-id="58df7-146">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="58df7-147">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span><span class="sxs-lookup"><span data-stu-id="58df7-147">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="58df7-148">Authorize users to access services and service methods</span><span class="sxs-lookup"><span data-stu-id="58df7-148">Authorize users to access services and service methods</span></span>

<span data-ttu-id="58df7-149">By default, all methods in a service can be called by unauthenticated users.</span><span class="sxs-lookup"><span data-stu-id="58df7-149">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="58df7-150">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span><span class="sxs-lookup"><span data-stu-id="58df7-150">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="58df7-151">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="58df7-151">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="58df7-152">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span><span class="sxs-lookup"><span data-stu-id="58df7-152">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="58df7-153">Individual service methods can have the `[Authorize]` attribute applied as well.</span><span class="sxs-lookup"><span data-stu-id="58df7-153">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="58df7-154">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span><span class="sxs-lookup"><span data-stu-id="58df7-154">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="58df7-155">其他資源</span><span class="sxs-lookup"><span data-stu-id="58df7-155">Additional resources</span></span>

* [<span data-ttu-id="58df7-156">Bearer Token authentication in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58df7-156">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="58df7-157">Configure Client Certificate authentication in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58df7-157">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
