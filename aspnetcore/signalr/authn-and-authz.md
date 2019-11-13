---
title: ASP.NET Core SignalR 中的驗證和授權
author: bradygaster
description: 瞭解如何在 ASP.NET Core SignalR中使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/authn-and-authz
ms.openlocfilehash: 5a1e15ef46a3f89af3fbd3d505e7bd340c46e672
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963819"
---
# <a name="authentication-and-authorization-in-aspnet-core-opno-locsignalr"></a>ASP.NET Core SignalR 中的驗證和授權

[Andrew Stanton-護士](https://twitter.com/anurse)

[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-opno-locsignalr-hub"></a>驗證連接到 SignalR 中樞的使用者

SignalR 可以與[ASP.NET Core 驗證](xref:security/authentication/identity)搭配使用，以將使用者與每個連線建立關聯。 在中樞中，可以從[`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user)屬性存取驗證資料。 驗證可讓中樞在與使用者相關聯的所有連接上呼叫方法。 如需詳細資訊，請參閱[管理 SignalR中的使用者和群組](xref:signalr/groups)。 多個連接可能會與單一使用者相關聯。

以下是使用 SignalR 和 ASP.NET Core 驗證的 `Startup.Configure` 範例：

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
> 註冊 SignalR 和 ASP.NET Core 驗證中介軟體的順序很重要。 請一律在 `UseSignalR` 之前呼叫 `UseAuthentication`，讓 SignalR 在 `HttpContext`上擁有使用者。

::: moniker-end

### <a name="cookie-authentication"></a>Cookie 驗證

在以瀏覽器為基礎的應用程式中，cookie 驗證可讓您現有的使用者認證自動流向 SignalR 連線。 使用瀏覽器用戶端時，不需要進行其他設定。 如果使用者已登入您的應用程式，SignalR 連接會自動繼承此驗證。

Cookie 是用來傳送存取權杖的瀏覽器特定方式，但非瀏覽器用戶端可以傳送它們。 使用[.Net 用戶端](xref:signalr/dotnet-client)時，可以在 `.WithUrl` 呼叫中設定 `Cookies` 屬性，以提供 cookie。 不過，若要從 .NET 用戶端使用 cookie 驗證，應用程式必須提供 API 來交換 cookie 的驗證資料。

### <a name="bearer-token-authentication"></a>持有人權杖驗證

用戶端可以提供存取權杖，而不是使用 cookie。 伺服器會驗證權杖，並使用它來識別使用者。 只有在建立連接時，才會執行此驗證。 在連接的生命週期內，伺服器不會自動重新驗證以檢查權杖撤銷。

在伺服器上，持有人權杖驗證是使用[JWT 持有人中介軟體](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer)來設定。

在 JavaScript 用戶端中，可以使用[accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication)選項來提供權杖。

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=52-55)]

在 .NET 用戶端中，有一個類似的[AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication)屬性可用於設定權杖：

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> 您提供的存取權杖函式會在 SignalR所提出的**每個**HTTP 要求之前呼叫。 如果您需要更新權杖，以便讓連線保持作用中（因為它可能會在連線期間過期），請從這個函式中執行此動作，並傳回更新的權杖。

在標準 web Api 中，會以 HTTP 標頭傳送持有人權杖。 不過，在使用某些傳輸時，SignalR 無法在瀏覽器中設定這些標頭。 使用 Websocket 和伺服器傳送事件時，權杖會以查詢字串參數的形式傳送。 若要在伺服器上支援此設定，需要進行其他設定：

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

> [!NOTE]
> 由於瀏覽器 API 限制，在與 Websocket 和伺服器傳送的事件連接時，會在瀏覽器上使用查詢字串。 使用 HTTPS 時，查詢字串值會受到 TLS 連接的保護。 不過，許多伺服器會記錄查詢字串值。 如需詳細資訊，請參閱[ASP.NET Core SignalR中的安全性考慮](xref:signalr/security)。 SignalR 使用標頭在支援它們的環境（例如 .NET 和 JAVA 用戶端）中傳輸權杖。

### <a name="cookies-vs-bearer-tokens"></a>Cookie 和持有人權杖 

Cookie 是瀏覽器特有的。 相較于傳送持有人權杖，從其他類型的用戶端傳送它們會增加複雜性。 因此，除非應用程式只需要從瀏覽器用戶端驗證使用者，否則不建議使用 cookie 驗證。 使用瀏覽器用戶端以外的用戶端時，持有人權杖驗證是建議的方法。

### <a name="windows-authentication"></a>Windows 驗證

如果您的應用程式中已設定[Windows 驗證](xref:security/authentication/windowsauth)，SignalR 可以使用該身分識別來保護中樞。 不過，若要將訊息傳送給個別使用者，您需要新增自訂使用者識別碼提供者。 Windows 驗證系統未提供「名稱識別碼」宣告。 SignalR 使用宣告來判斷使用者名稱。

加入新的類別，以執行 `IUserIdProvider`，並從使用者抓取其中一個宣告做為識別碼。 例如，若要使用 "Name" 宣告（這是 `[Domain]\[Username]`的格式的 Windows 使用者名稱），請建立下列類別：

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

您可以使用 `User` 中的任何值（例如 Windows SID 識別碼等等），而不是 `ClaimTypes.Name`。

> [!NOTE]
> 您選擇的值在系統中的所有使用者之間必須是唯一的。 否則，適用于某位使用者的訊息最後可能會到達不同的使用者。

在您的 `Startup.ConfigureServices` 方法中註冊此元件。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

在 .NET 用戶端中，必須藉由設定[UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials)屬性來啟用 Windows 驗證：

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

只有在使用 Microsoft Internet Explorer 或 Microsoft Edge 時，瀏覽器用戶端才支援 Windows 驗證。

### <a name="use-claims-to-customize-identity-handling"></a>使用宣告來自訂身分識別處理

驗證使用者的應用程式可以從使用者宣告衍生 SignalR 使用者識別碼。 若要指定 SignalR 如何建立使用者識別碼，請執行 `IUserIdProvider` 並註冊該執行。

範例程式碼示範如何使用宣告來選取使用者的電子郵件地址做為識別屬性。 

> [!NOTE]
> 您選擇的值在系統中的所有使用者之間必須是唯一的。 否則，適用于某位使用者的訊息最後可能會到達不同的使用者。

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

帳戶註冊會將具有類型 `ClaimsTypes.Email` 的宣告新增至 ASP.NET 身分識別資料庫。

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

在您的 `Startup.ConfigureServices` 中註冊此元件。

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>授權使用者存取中樞和中樞方法

根據預設，中樞內的所有方法都可以由未經驗證的使用者呼叫。 若要要求驗證，請將[授權](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)屬性套用至中樞：

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

您可以使用 `[Authorize]` 屬性的函數引數和屬性，限制只有符合特定[授權原則](xref:security/authorization/policies)的使用者才能存取。 例如，如果您有一個稱為的自訂授權原則 `MyAuthorizationPolicy` 您可以確保只有符合該原則的使用者可以使用下列程式碼來存取中樞：

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

個別的中樞方法也可以套用 `[Authorize]` 屬性。 如果目前的使用者不符合套用至方法的原則，則會將錯誤傳回給呼叫者：

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

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a>使用授權處理常式自訂中樞方法授權

當中樞方法需要授權時，SignalR 會提供自訂資源給授權處理常式。 資源是 `HubInvocationContext` 的實例。 `HubInvocationContext` 包括 `HubCallerContext`、所叫用的中樞方法名稱，以及中樞方法的引數。

請考慮允許多個組織透過 Azure Active Directory 登入的聊天室範例。 具有 Microsoft 帳戶的任何人都可以登入交談，但只有擁有組織的成員才能夠禁止使用者或觀看使用者的聊天記錄。 此外，我們可能會想要限制特定使用者的特定功能。 使用 ASP.NET Core 3.0 中的更新功能，這是完全可行的。 請注意，`DomainRestrictedRequirement` 如何作為自訂 `IAuthorizationRequirement`。 現在，已將 `HubInvocationContext` 資源參數傳入，內部邏輯可以檢查正在呼叫中樞的內容，並決定是否要讓使用者執行個別的中樞方法。

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

在 `Startup.ConfigureServices` 中，新增新的原則，並提供自訂的 `DomainRestrictedRequirement` 需求做為參數，以建立 `DomainRestricted` 原則。

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

在上述範例中，`DomainRestrictedRequirement` 類別同時為該需求的 `IAuthorizationRequirement` 和它自己的 `AuthorizationHandler`。 將這兩個元件分割成不同的類別是可接受的，以因應不同的考慮。 範例方法的優點是，在啟動期間不需要插入 `AuthorizationHandler`，因為需求和處理常式是相同的。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [ASP.NET Core 中的持有人權杖驗證](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [以資源為基礎的授權](xref:security/authorization/resourcebased)
