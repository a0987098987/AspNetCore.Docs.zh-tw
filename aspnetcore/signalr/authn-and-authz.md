---
title: ASP.NET Core 中的驗證和授權SignalR
author: bradygaster
description: 瞭解如何在 ASP.NET Core SignalR中使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/authn-and-authz
ms.openlocfilehash: 0f0bb2040d2407817c91f64a4769e6601c37a07d
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775280"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="2492d-103">ASP.NET Core 中的驗證和授權SignalR</span><span class="sxs-lookup"><span data-stu-id="2492d-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="2492d-104">[Andrew Stanton-護士](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2492d-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="2492d-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="2492d-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="2492d-106">驗證連線到中樞的SignalR使用者</span><span class="sxs-lookup"><span data-stu-id="2492d-106">Authenticate users connecting to a SignalR hub</span></span>

SignalR<span data-ttu-id="2492d-107">可以與[ASP.NET Core 驗證](xref:security/authentication/identity)搭配使用，以將使用者與每個連線建立關聯。</span><span class="sxs-lookup"><span data-stu-id="2492d-107"> can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="2492d-108">在中樞中，可以從[HubConnectionCoNtext 的使用者](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user)屬性存取驗證資料。</span><span class="sxs-lookup"><span data-stu-id="2492d-108">In a hub, authentication data can be accessed from the [HubConnectionContext.User](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="2492d-109">驗證可讓中樞在與使用者相關聯的所有連接上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="2492d-109">Authentication allows the hub to call methods on all connections associated with a user.</span></span> <span data-ttu-id="2492d-110">如需詳細資訊，請參閱[管理中的SignalR使用者和群組](xref:signalr/groups)。</span><span class="sxs-lookup"><span data-stu-id="2492d-110">For more information, see [Manage users and groups in SignalR](xref:signalr/groups).</span></span> <span data-ttu-id="2492d-111">多個連接可能會與單一使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="2492d-111">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="2492d-112">以下是使用SignalR和 ASP.NET Core 驗證`Startup.Configure`的範例：</span><span class="sxs-lookup"><span data-stu-id="2492d-112">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chat");
        endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    app.UseAuthentication();

    app.UseSignalR(hubs =>
    {
        hubs.MapHub<ChatHub>("/chat");
    });

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

> [!NOTE]
> <span data-ttu-id="2492d-113">註冊SignalR和 ASP.NET Core 驗證中介軟體的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="2492d-113">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="2492d-114">一律在`UseAuthentication`之前`UseSignalR`呼叫， SignalR使上的使用者`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="2492d-114">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

::: moniker-end

### <a name="cookie-authentication"></a><span data-ttu-id="2492d-115">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="2492d-115">Cookie authentication</span></span>

<span data-ttu-id="2492d-116">在以瀏覽器為基礎的應用程式中，cookie 驗證可讓您現有的SignalR使用者認證自動流向連線。</span><span class="sxs-lookup"><span data-stu-id="2492d-116">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="2492d-117">使用瀏覽器用戶端時，不需要進行其他設定。</span><span class="sxs-lookup"><span data-stu-id="2492d-117">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="2492d-118">如果使用者已登入您的應用程式，則SignalR連接會自動繼承此驗證。</span><span class="sxs-lookup"><span data-stu-id="2492d-118">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="2492d-119">Cookie 是用來傳送存取權杖的瀏覽器特定方式，但非瀏覽器用戶端可以傳送它們。</span><span class="sxs-lookup"><span data-stu-id="2492d-119">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="2492d-120">使用[.Net 用戶端](xref:signalr/dotnet-client)時，可以`Cookies`在`.WithUrl`呼叫中設定屬性，以提供 cookie。</span><span class="sxs-lookup"><span data-stu-id="2492d-120">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call to provide a cookie.</span></span> <span data-ttu-id="2492d-121">不過，若要從 .NET 用戶端使用 cookie 驗證，應用程式必須提供 API 來交換 cookie 的驗證資料。</span><span class="sxs-lookup"><span data-stu-id="2492d-121">However, using cookie authentication from the .NET client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="2492d-122">持有人權杖驗證</span><span class="sxs-lookup"><span data-stu-id="2492d-122">Bearer token authentication</span></span>

<span data-ttu-id="2492d-123">用戶端可以提供存取權杖，而不是使用 cookie。</span><span class="sxs-lookup"><span data-stu-id="2492d-123">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="2492d-124">伺服器會驗證權杖，並使用它來識別使用者。</span><span class="sxs-lookup"><span data-stu-id="2492d-124">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="2492d-125">只有在建立連接時，才會執行此驗證。</span><span class="sxs-lookup"><span data-stu-id="2492d-125">This validation is done only when the connection is established.</span></span> <span data-ttu-id="2492d-126">在連接的生命週期內，伺服器不會自動重新驗證以檢查權杖撤銷。</span><span class="sxs-lookup"><span data-stu-id="2492d-126">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="2492d-127">在伺服器上，持有人權杖驗證是使用[JWT 持有人中介軟體](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)來設定。</span><span class="sxs-lookup"><span data-stu-id="2492d-127">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="2492d-128">在 JavaScript 用戶端中，可以使用[accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication)選項來提供權杖。</span><span class="sxs-lookup"><span data-stu-id="2492d-128">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=52-55)]

<span data-ttu-id="2492d-129">在 .NET 用戶端中，有一個類似的[AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication)屬性可用於設定權杖：</span><span class="sxs-lookup"><span data-stu-id="2492d-129">In the .NET client, there's a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="2492d-130">您提供的存取權杖函式會在**每個**發出的SignalRHTTP 要求之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="2492d-130">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="2492d-131">如果您需要更新權杖，以便讓連線保持作用中（因為它可能會在連線期間過期），請從這個函式中執行此動作，並傳回更新的權杖。</span><span class="sxs-lookup"><span data-stu-id="2492d-131">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="2492d-132">在標準 web Api 中，會以 HTTP 標頭傳送持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="2492d-132">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="2492d-133">不過， SignalR使用某些傳輸時，無法在瀏覽器中設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="2492d-133">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="2492d-134">使用 Websocket 和伺服器傳送事件時，權杖會以查詢字串參數的形式傳送。</span><span class="sxs-lookup"><span data-stu-id="2492d-134">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="2492d-135">若要在伺服器上支援此設定，需要進行其他設定：</span><span class="sxs-lookup"><span data-stu-id="2492d-135">To support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

> [!NOTE]
> <span data-ttu-id="2492d-136">由於瀏覽器 API 限制，在與 Websocket 和伺服器傳送的事件連接時，會在瀏覽器上使用查詢字串。</span><span class="sxs-lookup"><span data-stu-id="2492d-136">The query string is used on browsers when connecting with WebSockets and Server-Sent Events due to browser API limitations.</span></span> <span data-ttu-id="2492d-137">使用 HTTPS 時，查詢字串值會受到 TLS 連接的保護。</span><span class="sxs-lookup"><span data-stu-id="2492d-137">When using HTTPS, query string values are secured by the TLS connection.</span></span> <span data-ttu-id="2492d-138">不過，許多伺服器會記錄查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="2492d-138">However, many servers log query string values.</span></span> <span data-ttu-id="2492d-139">如需詳細資訊，請參閱[ASP.NET Core SignalR中的安全性考慮](xref:signalr/security)。</span><span class="sxs-lookup"><span data-stu-id="2492d-139">For more information, see [Security considerations in ASP.NET Core SignalR](xref:signalr/security).</span></span> SignalR<span data-ttu-id="2492d-140">使用標頭在支援它們的環境（例如 .NET 和 JAVA 用戶端）中傳輸權杖。</span><span class="sxs-lookup"><span data-stu-id="2492d-140"> uses headers to transmit tokens in environments which support them (such as the .NET and Java clients).</span></span>

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="2492d-141">Cookie 和持有人權杖</span><span class="sxs-lookup"><span data-stu-id="2492d-141">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="2492d-142">Cookie 是瀏覽器特有的。</span><span class="sxs-lookup"><span data-stu-id="2492d-142">Cookies are specific to browsers.</span></span> <span data-ttu-id="2492d-143">相較于傳送持有人權杖，從其他類型的用戶端傳送它們會增加複雜性。</span><span class="sxs-lookup"><span data-stu-id="2492d-143">Sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="2492d-144">因此，除非應用程式只需要從瀏覽器用戶端驗證使用者，否則不建議使用 cookie 驗證。</span><span class="sxs-lookup"><span data-stu-id="2492d-144">Consequently, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="2492d-145">使用瀏覽器用戶端以外的用戶端時，持有人權杖驗證是建議的方法。</span><span class="sxs-lookup"><span data-stu-id="2492d-145">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="2492d-146">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="2492d-146">Windows authentication</span></span>

<span data-ttu-id="2492d-147">如果您的應用程式中已設定[Windows 驗證](xref:security/authentication/windowsauth)， SignalR則可以使用該身分識別來保護中樞。</span><span class="sxs-lookup"><span data-stu-id="2492d-147">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="2492d-148">不過，若要將訊息傳送給個別使用者，您需要新增自訂使用者識別碼提供者。</span><span class="sxs-lookup"><span data-stu-id="2492d-148">However, to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="2492d-149">Windows 驗證系統未提供「名稱識別碼」宣告。</span><span class="sxs-lookup"><span data-stu-id="2492d-149">The Windows authentication system doesn't provide the "Name Identifier" claim.</span></span> SignalR<span data-ttu-id="2492d-150">使用宣告來判斷使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="2492d-150"> uses the claim to determine the user name.</span></span>

<span data-ttu-id="2492d-151">加入新的類別，以`IUserIdProvider`執行並抓取其中一個來自使用者的宣告，做為識別碼。</span><span class="sxs-lookup"><span data-stu-id="2492d-151">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="2492d-152">例如，若要使用 "Name" 宣告（這是格式`[Domain]\[Username]`的 Windows 使用者名稱），請建立下列類別：</span><span class="sxs-lookup"><span data-stu-id="2492d-152">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="2492d-153">而不`ClaimTypes.Name`是，您可以使用中的`User`任何值（例如 Windows SID 識別碼等等）。</span><span class="sxs-lookup"><span data-stu-id="2492d-153">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, and so on).</span></span>

> [!NOTE]
> <span data-ttu-id="2492d-154">您選擇的值在系統中的所有使用者之間必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="2492d-154">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="2492d-155">否則，適用于某位使用者的訊息最後可能會到達不同的使用者。</span><span class="sxs-lookup"><span data-stu-id="2492d-155">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="2492d-156">在您`Startup.ConfigureServices`的方法中註冊此元件。</span><span class="sxs-lookup"><span data-stu-id="2492d-156">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="2492d-157">在 .NET 用戶端中，必須藉由設定[UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials)屬性來啟用 Windows 驗證：</span><span class="sxs-lookup"><span data-stu-id="2492d-157">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="2492d-158">只有在使用 Microsoft Internet Explorer 或 Microsoft Edge 時，瀏覽器用戶端才支援 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="2492d-158">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="2492d-159">使用宣告來自訂身分識別處理</span><span class="sxs-lookup"><span data-stu-id="2492d-159">Use claims to customize identity handling</span></span>

<span data-ttu-id="2492d-160">驗證使用者的應用程式可以從SignalR使用者宣告衍生使用者識別碼。</span><span class="sxs-lookup"><span data-stu-id="2492d-160">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="2492d-161">若要指定SignalR如何建立使用者識別碼， `IUserIdProvider`請執行並註冊執行。</span><span class="sxs-lookup"><span data-stu-id="2492d-161">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="2492d-162">範例程式碼示範如何使用宣告來選取使用者的電子郵件地址做為識別屬性。</span><span class="sxs-lookup"><span data-stu-id="2492d-162">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="2492d-163">您選擇的值在系統中的所有使用者之間必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="2492d-163">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="2492d-164">否則，適用于某位使用者的訊息最後可能會到達不同的使用者。</span><span class="sxs-lookup"><span data-stu-id="2492d-164">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="2492d-165">帳戶註冊會將類型`ClaimsTypes.Email`為的宣告新增至 ASP.NET identity database。</span><span class="sxs-lookup"><span data-stu-id="2492d-165">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="2492d-166">在您`Startup.ConfigureServices`的中註冊此元件。</span><span class="sxs-lookup"><span data-stu-id="2492d-166">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="2492d-167">授權使用者存取中樞和中樞方法</span><span class="sxs-lookup"><span data-stu-id="2492d-167">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="2492d-168">根據預設，中樞內的所有方法都可以由未經驗證的使用者呼叫。</span><span class="sxs-lookup"><span data-stu-id="2492d-168">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="2492d-169">若要要求驗證，請將[授權](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)屬性套用至中樞：</span><span class="sxs-lookup"><span data-stu-id="2492d-169">To require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="2492d-170">您可以使用`[Authorize]`屬性的「函式引數」和「屬性」，限制只有符合特定[授權原則](xref:security/authorization/policies)的使用者才能存取。</span><span class="sxs-lookup"><span data-stu-id="2492d-170">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="2492d-171">例如，如果您有一個稱為`MyAuthorizationPolicy`的自訂授權原則，您可以確保只有符合該原則的使用者可以使用下列程式碼來存取中樞：</span><span class="sxs-lookup"><span data-stu-id="2492d-171">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

<span data-ttu-id="2492d-172">個別的`[Authorize]`中樞方法也可以套用屬性。</span><span class="sxs-lookup"><span data-stu-id="2492d-172">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="2492d-173">如果目前的使用者不符合套用至方法的原則，則會將錯誤傳回給呼叫者：</span><span class="sxs-lookup"><span data-stu-id="2492d-173">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```

::: moniker range=">= aspnetcore-3.0"

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a><span data-ttu-id="2492d-174">使用授權處理常式自訂中樞方法授權</span><span class="sxs-lookup"><span data-stu-id="2492d-174">Use authorization handlers to customize hub method authorization</span></span>

SignalR<span data-ttu-id="2492d-175">當中樞方法需要授權時，提供自訂資源給授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="2492d-175"> provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="2492d-176">資源是的實例`HubInvocationContext`。</span><span class="sxs-lookup"><span data-stu-id="2492d-176">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="2492d-177">`HubInvocationContext`包含`HubCallerContext`、所叫用之中樞方法的名稱，以及中樞方法的引數。</span><span class="sxs-lookup"><span data-stu-id="2492d-177">The `HubInvocationContext` includes the `HubCallerContext`, the name of the hub method being invoked, and the arguments to the hub method.</span></span>

<span data-ttu-id="2492d-178">請考慮允許多個組織透過 Azure Active Directory 登入的聊天室範例。</span><span class="sxs-lookup"><span data-stu-id="2492d-178">Consider the example of a chat room allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="2492d-179">具有 Microsoft 帳戶的任何人都可以登入交談，但只有擁有組織的成員才能夠禁止使用者或觀看使用者的聊天記錄。</span><span class="sxs-lookup"><span data-stu-id="2492d-179">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization should be able to ban users or view users' chat histories.</span></span> <span data-ttu-id="2492d-180">此外，我們可能會想要限制特定使用者的特定功能。</span><span class="sxs-lookup"><span data-stu-id="2492d-180">Furthermore, we might want to restrict certain functionality from certain users.</span></span> <span data-ttu-id="2492d-181">使用 ASP.NET Core 3.0 中的更新功能，這是完全可行的。</span><span class="sxs-lookup"><span data-stu-id="2492d-181">Using the updated features in ASP.NET Core 3.0, this is entirely possible.</span></span> <span data-ttu-id="2492d-182">請注意， `DomainRestrictedRequirement`如何作為自訂`IAuthorizationRequirement`。</span><span class="sxs-lookup"><span data-stu-id="2492d-182">Note how the `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="2492d-183">現在， `HubInvocationContext`資源參數已傳入，內部邏輯可以檢查正在呼叫中樞的內容，並決定是否要讓使用者執行個別的中樞方法。</span><span class="sxs-lookup"><span data-stu-id="2492d-183">Now that the `HubInvocationContext` resource parameter is being passed in, the internal logic can inspect the context in which the Hub is being called and make decisions on allowing the user to execute individual Hub methods.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}

public class DomainRestrictedRequirement : 
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>, 
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement, 
        HubInvocationContext resource)
    {
        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name) && 
            context.User.Identity.Name.EndsWith("@microsoft.com"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName,
        string currentUsername)
    {
        return !(currentUsername.Equals("asdf42@microsoft.com") && 
            hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="2492d-184">在`Startup.ConfigureServices`中，新增新的原則，並提供`DomainRestrictedRequirement`自訂需求做為參數來`DomainRestricted`建立原則。</span><span class="sxs-lookup"><span data-stu-id="2492d-184">In `Startup.ConfigureServices`, add the new policy, providing the custom `DomainRestrictedRequirement` requirement as a parameter to create the `DomainRestricted` policy.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services
        .AddAuthorization(options =>
        {
            options.AddPolicy("DomainRestricted", policy =>
            {
                policy.Requirements.Add(new DomainRestrictedRequirement());
            });
        });
}
```

<span data-ttu-id="2492d-185">在上述範例中， `DomainRestrictedRequirement`類別既是， `IAuthorizationRequirement`也是其本身`AuthorizationHandler`的需求。</span><span class="sxs-lookup"><span data-stu-id="2492d-185">In the preceding example, the `DomainRestrictedRequirement` class is both an `IAuthorizationRequirement` and its own `AuthorizationHandler` for that requirement.</span></span> <span data-ttu-id="2492d-186">將這兩個元件分割成不同的類別是可接受的，以因應不同的考慮。</span><span class="sxs-lookup"><span data-stu-id="2492d-186">It's acceptable to split these two components into separate classes to separate concerns.</span></span> <span data-ttu-id="2492d-187">範例方法的優點是，不需要在啟動`AuthorizationHandler`期間插入，因為需求和處理常式是相同的。</span><span class="sxs-lookup"><span data-stu-id="2492d-187">A benefit of the example's approach is there's no need to inject the `AuthorizationHandler` during startup, as the requirement and the handler are the same thing.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2492d-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="2492d-188">Additional resources</span></span>

* [<span data-ttu-id="2492d-189">ASP.NET Core 中的持有人權杖驗證</span><span class="sxs-lookup"><span data-stu-id="2492d-189">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="2492d-190">以資源為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="2492d-190">Resource-based Authorization</span></span>](xref:security/authorization/resourcebased)
