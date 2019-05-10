---
title: ASP.NET Core SignalR 中驗證和授權
author: bradygaster
description: 了解如何在 ASP.NET Core SignalR 使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 05/09/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e8f9dc48be780fb91bdec6ea4d579f5e4f16197b
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516946"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>ASP.NET Core SignalR 中驗證和授權

藉由[Andrew Stanton-nurse](https://twitter.com/anurse)

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>驗證連接到 SignalR 中樞的使用者

SignalR 可以搭配[ASP.NET Core 驗證](xref:security/authentication/identity)使用者相關聯的每個連線。 在中樞中，驗證資料可以從存取[ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user)屬性。 驗證可讓與使用者相關聯的所有連接上呼叫方法的中樞 (請參閱[管理使用者和群組 signalr](xref:signalr/groups)如需詳細資訊)。 多個連接可能會與單一使用者相關聯。

以下是範例`Startup.Configure`使用 SignalR 和 ASP.NET Core 的驗證：

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
> 您可以在其中註冊 SignalR 及 ASP.NET Core 驗證中介軟體的順序很重要。 請務必呼叫`UseAuthentication`之前`UseSignalR`使 SignalR 具有使用者`HttpContext`。

### <a name="cookie-authentication"></a>Cookie 驗證

在瀏覽器為基礎的應用程式中的 cookie 驗證可讓您現有的使用者認證，以自動流向 SignalR 連線。 使用瀏覽器用戶端時，不需要進行其他設定。 如果使用者登入您的應用程式，SignalR 連線會自動繼承此驗證。

Cookie 是瀏覽器特定的方式，傳送存取權杖，但非瀏覽器用戶端可以傳送給他們。 使用時[.NET 用戶端](xref:signalr/dotnet-client)，則`Cookies`屬性可以設定在`.WithUrl`呼叫，以提供 cookie。 不過，使用 cookie 驗證，從.NET 用戶端需要應用程式提供 API，以交換驗證 cookie 的資料。

### <a name="bearer-token-authentication"></a>持有人權杖驗證

用戶端可以提供存取權杖，而不是使用 cookie。 伺服器會驗證權杖，並使用它來識別使用者。 只有在建立連線時，才完成這項驗證。 連接的期間，伺服器不會自動重新驗證權杖撤銷檢查。

在伺服器上，持有人權杖驗證使用設定[JWT Bearer 中介軟體](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)。

在 JavaScript 用戶端，語彙基元，可供使用[accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication)選項。

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

在.NET 用戶端，沒有類似[AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication)可用來設定權杖的屬性：

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> 您提供的存取語彙基元函式之前，會呼叫**每個**SignalR 所提出的 HTTP 要求。 如果您需要更新權杖，才能讓連線保持作用中 （因為它可能會在連線期間過期），此函式中進行的並傳回更新的權杖。

在標準的 web Api，持有人權杖會傳送 HTTP 標頭。 不過，SignalR 是無法使用某些傳輸時，在瀏覽器中設定這些標頭。 使用 WebSockets 和 Server-Sent 事件時，權杖會傳送做為查詢字串參數。 若要支援此伺服器上，則需要其他組態：

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>持有人權杖與 cookie 

因為 cookie 特有的瀏覽器，從其他種類的用戶端傳送會增加複雜度，相較於傳送持有人權杖。 基於這個理由，不建議 cookie 驗證，除非應用程式只需要從瀏覽器用戶端驗證使用者。 使用非瀏覽器用戶端的用戶端時，持有人權杖驗證是建議的方法。

### <a name="windows-authentication"></a>Windows 驗證

如果[Windows 驗證](xref:security/authentication/windowsauth)已在您的應用程式，SignalR 可以使用該身分識別保護中樞。 不過，為了將訊息傳送給個別使用者，您需要新增自訂的使用者識別碼提供者。 這是因為 Windows 驗證系統並不提供 「 名稱識別碼 」 宣告 SignalR 用來判斷使用者名稱。

加入新的類別可實作`IUserIdProvider`和其中一個宣告擷取使用者做為識別碼。 例如，若要使用"Name"宣告 (這是在表單中的 Windows 使用者名稱`[Domain]\[Username]`)，建立下列類別：

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

而非`ClaimTypes.Name`，您可以使用的任何值`User`（例如 Windows SID 識別項等）。

> [!NOTE]
> 在您的系統中，您所選擇的值必須是所有使用者之間唯一的。 否則，最後適用於一位使用者的訊息可能會移至不同的使用者。

註冊此元件，在您`Startup.ConfigureServices`方法。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

在.NET 用戶端，必須啟用 Windows 驗證設定[UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials)屬性：

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

使用 Microsoft Internet Explorer 或 Microsoft Edge 時，瀏覽器用戶端只會支援 Windows 驗證。

### <a name="use-claims-to-customize-identity-handling"></a>使用自訂識別處理的宣告

驗證使用者的應用程式可以衍生自使用者宣告的 SignalR 使用者識別碼。 若要指定 SignalR 建立使用者識別碼的方式，實作`IUserIdProvider`和註冊的實作。

在 程式碼範例示範如何使用以選取使用者的電子郵件地址作為識別屬性的 宣告。 

> [!NOTE]
> 在您的系統中，您所選擇的值必須是所有使用者之間唯一的。 否則，最後適用於一位使用者的訊息可能會移至不同的使用者。

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

註冊帳戶加入類型宣告`ClaimsTypes.Email`ASP.NET 身分識別資料庫。

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

註冊此元件，在您`Startup.ConfigureServices`。

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>授權使用者存取中樞和中樞方法

根據預設，在中樞中的所有方法都可以都呼叫未經驗證的使用者。 需要驗證，才能套用[授權](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)屬性至中樞：

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

您可以使用的建構函式引數和屬性`[Authorize]`屬性來限制只有符合特定的使用者存取[授權原則](xref:security/authorization/policies)。 例如，如果您有自訂授權原則呼叫`MyAuthorizationPolicy`您可以確保只有符合該原則的使用者可以存取使用下列程式碼的中樞：

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

個別的中樞的方法可以有`[Authorize]`以及套用的屬性。 如果目前的使用者不符合原則套用至方法，錯誤會傳回給呼叫者：

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

## <a name="additional-resources"></a>其他資源

* [ASP.NET Core 中的持有人權杖驗證](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
