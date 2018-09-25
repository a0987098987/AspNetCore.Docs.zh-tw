---
title: ASP.NET Core SignalR 中驗證和授權
author: tdykstra
description: 了解如何在 ASP.NET Core SignalR 使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: fa5e8d4aea6100c54fd8b98411ef877d1dd8621c
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028201"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="eab4f-103">ASP.NET Core SignalR 中驗證和授權</span><span class="sxs-lookup"><span data-stu-id="eab4f-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="eab4f-104">藉由[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="eab4f-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="eab4f-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="eab4f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="eab4f-106">驗證連接到 SignalR 中樞的使用者</span><span class="sxs-lookup"><span data-stu-id="eab4f-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="eab4f-107">SignalR 可以搭配[ASP.NET Core 驗證](xref:security/authentication/index)使用者相關聯的每個連線。</span><span class="sxs-lookup"><span data-stu-id="eab4f-107">SignalR can be used with [ASP.NET Core Authentication](xref:security/authentication/index) to associate a user with each connection.</span></span> <span data-ttu-id="eab4f-108">在中樞中，驗證資料可以從存取[ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user)屬性。</span><span class="sxs-lookup"><span data-stu-id="eab4f-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="eab4f-109">驗證可讓與使用者相關聯的所有連接上呼叫方法的中樞 (請參閱[管理使用者和群組 signalr](xref:signalr/groups)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="eab4f-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="eab4f-110">多個連接可能會與單一使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="eab4f-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="eab4f-111">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="eab4f-111">Cookie authentication</span></span>

<span data-ttu-id="eab4f-112">在瀏覽器為基礎的應用程式中的 cookie 驗證可讓您現有的使用者認證，以自動流向 SignalR 連線。</span><span class="sxs-lookup"><span data-stu-id="eab4f-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="eab4f-113">使用瀏覽器用戶端時，不需要進行其他設定。</span><span class="sxs-lookup"><span data-stu-id="eab4f-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="eab4f-114">如果使用者登入您的應用程式，SignalR 連線會自動繼承此驗證。</span><span class="sxs-lookup"><span data-stu-id="eab4f-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="eab4f-115">除非應用程式只需要從瀏覽器用戶端驗證使用者，不建議使用 cookie 驗證。</span><span class="sxs-lookup"><span data-stu-id="eab4f-115">Cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="eab4f-116">使用時[.NET 用戶端](xref:signalr/dotnet-client)，則`Cookies`屬性可以設定在`.WithUrl`呼叫，以提供 cookie。</span><span class="sxs-lookup"><span data-stu-id="eab4f-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="eab4f-117">不過，使用 cookie 驗證，從.NET 用戶端需要應用程式提供 API，以交換驗證 cookie 的資料。</span><span class="sxs-lookup"><span data-stu-id="eab4f-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="eab4f-118">持有人權杖驗證</span><span class="sxs-lookup"><span data-stu-id="eab4f-118">Bearer token authentication</span></span>

<span data-ttu-id="eab4f-119">使用非瀏覽器用戶端的用戶端時，持有人權杖驗證是建議的方法。</span><span class="sxs-lookup"><span data-stu-id="eab4f-119">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span> <span data-ttu-id="eab4f-120">這種方法，用戶端會提供伺服器驗證，並使用來識別使用者的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="eab4f-120">In this approach, the client provides an access token that the server validates and uses to identify the user.</span></span> <span data-ttu-id="eab4f-121">持有人權杖驗證的詳細資料已超出本文的範圍。</span><span class="sxs-lookup"><span data-stu-id="eab4f-121">The details of bearer token authentication are beyond the scope of this document.</span></span> <span data-ttu-id="eab4f-122">在伺服器上，持有人權杖驗證使用設定[JWT Bearer 中介軟體](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)。</span><span class="sxs-lookup"><span data-stu-id="eab4f-122">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="eab4f-123">在 JavaScript 用戶端，語彙基元，可供使用[accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication)選項。</span><span class="sxs-lookup"><span data-stu-id="eab4f-123">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="eab4f-124">在.NET 用戶端，沒有類似[AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication)可用來設定權杖的屬性：</span><span class="sxs-lookup"><span data-stu-id="eab4f-124">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="eab4f-125">您提供的存取語彙基元函式之前，會呼叫**每個**SignalR 所提出的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="eab4f-125">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="eab4f-126">如果您需要更新權杖，才能讓連線保持作用中 （因為它可能會在連線期間過期），此函式中進行的並傳回更新的權杖。</span><span class="sxs-lookup"><span data-stu-id="eab4f-126">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="eab4f-127">在標準的 web Api，持有人權杖會傳送 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="eab4f-127">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="eab4f-128">不過，SignalR 是無法使用某些傳輸時，在瀏覽器中設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="eab4f-128">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="eab4f-129">使用 WebSockets 和 Server-Sent 事件時，權杖會傳送做為查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="eab4f-129">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="eab4f-130">若要支援此伺服器上，則需要其他組態：</span><span class="sxs-lookup"><span data-stu-id="eab4f-130">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="windows-authentication"></a><span data-ttu-id="eab4f-131">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="eab4f-131">Windows authentication</span></span>

<span data-ttu-id="eab4f-132">如果[Windows 驗證](xref:security/authentication/windowsauth)已在您的應用程式，SignalR 可以使用該身分識別保護中樞。</span><span class="sxs-lookup"><span data-stu-id="eab4f-132">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="eab4f-133">不過，為了將訊息傳送給個別使用者，您需要新增自訂的使用者識別碼提供者。</span><span class="sxs-lookup"><span data-stu-id="eab4f-133">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="eab4f-134">這是因為 Windows 驗證系統並不提供 「 名稱識別碼 」 宣告 SignalR 用來判斷使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="eab4f-134">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="eab4f-135">加入新的類別可實作`IUserIdProvider`和其中一個宣告擷取使用者做為識別碼。</span><span class="sxs-lookup"><span data-stu-id="eab4f-135">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="eab4f-136">例如，若要使用"Name"宣告 (這是在表單中的 Windows 使用者名稱`[Domain]\[Username]`)，建立下列類別：</span><span class="sxs-lookup"><span data-stu-id="eab4f-136">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="eab4f-137">而非`ClaimTypes.Name`，您可以使用的任何值`User`（例如 Windows SID 識別項等）。</span><span class="sxs-lookup"><span data-stu-id="eab4f-137">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="eab4f-138">在您的系統中，您所選擇的值必須是所有使用者之間唯一的。</span><span class="sxs-lookup"><span data-stu-id="eab4f-138">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="eab4f-139">否則，最後適用於一位使用者的訊息可能會移至不同的使用者。</span><span class="sxs-lookup"><span data-stu-id="eab4f-139">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="eab4f-140">註冊此元件，在您`Startup.ConfigureServices`方法**之後**呼叫 `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="eab4f-140">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="eab4f-141">在.NET 用戶端，必須啟用 Windows 驗證設定[UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials)屬性：</span><span class="sxs-lookup"><span data-stu-id="eab4f-141">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="eab4f-142">使用 Microsoft Internet Explorer 或 Microsoft Edge 時，瀏覽器用戶端只會支援 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="eab4f-142">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="eab4f-143">授權使用者存取中樞和中樞方法</span><span class="sxs-lookup"><span data-stu-id="eab4f-143">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="eab4f-144">根據預設，在中樞中的所有方法都可以都呼叫未經驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="eab4f-144">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="eab4f-145">需要驗證，才能套用[授權](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)屬性至中樞：</span><span class="sxs-lookup"><span data-stu-id="eab4f-145">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="eab4f-146">您可以使用的建構函式引數和屬性`[Authorize]`屬性來限制只有符合特定的使用者存取[授權原則](xref:security/authorization/policies)。</span><span class="sxs-lookup"><span data-stu-id="eab4f-146">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="eab4f-147">例如，如果您有自訂授權原則呼叫`MyAuthorizationPolicy`您可以確保只有符合該原則的使用者可以存取使用下列程式碼的中樞：</span><span class="sxs-lookup"><span data-stu-id="eab4f-147">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="eab4f-148">個別的中樞的方法可以有`[Authorize]`以及套用的屬性。</span><span class="sxs-lookup"><span data-stu-id="eab4f-148">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="eab4f-149">如果目前的使用者不符合原則套用至方法，錯誤會傳回給呼叫者：</span><span class="sxs-lookup"><span data-stu-id="eab4f-149">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub: Hub
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
