---
title: ASP.NET核心Blazor認證與授權
author: guardrex
description: 瞭解Blazor身份驗證和授權方案。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: 04bbf20d1d848edfa98e719f316b790c812bfd95
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80501317"
---
# <a name="aspnet-core-opno-locblazor-authentication-and-authorization"></a>ASP.NET核心Blazor認證與授權

由[史蒂夫·桑德森](https://github.com/SteveSandersonMS)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

> [!NOTE]
> 對於本文中適用於BlazorWeb 裝配的指導,需要 ASP.NET 核心BlazorWeb 組裝範本版本 3.2 或更高版本。 如果您不使用 Visual Studio 版本 16.6 預覽版或Blazor更高版本,請使用<xref:blazor/get-started>中的指南獲取最新的 Web 組裝範本。

ASP.NET核心支援Blazor應用程式中安全性的配置和管理。

伺服器和BlazorBlazorWeb 組裝應用之間的安全方案不同。 由於Blazor伺服器應用在伺服器上運行,因此授權檢查能夠確定:

* 要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。
* 適用於應用程式和元件之特定區域的存取規則。

Blazor在用戶端上運行 Web 組裝應用。 授權「僅」** 會被用來決定要顯示的 UI 選項。 由於使用者可以修改或繞過客戶端檢查Blazor, 因此 WebAssembly 應用無法強制實施授權存取規則。

[剃刀頁授權約定](xref:security/authorization/razor-pages-authorization)不適用於可路由的 Razor 元件。 如果[嵌入](xref:blazor/integrate-components#render-components-from-a-page-or-view)了不可路由的 Razor 元件,則該頁的授權約定會間接影響 Razor 元件以及頁面的其他內容。

> [!NOTE]
> <xref:Microsoft.AspNetCore.Identity.SignInManager%601>並且<xref:Microsoft.AspNetCore.Identity.UserManager%601>在 Razor 元件中不受支援。

## <a name="authentication"></a>驗證

Blazor使用現有的ASP.NET核心身份驗證機制來建立使用者的身份。 確切的機制取決於Blazor應用程式的託管方式、WebBlazor組裝Blazor或 伺服器。

### <a name="opno-locblazor-webassembly-authentication"></a>BlazorWeb 組裝身分驗證

在BlazorWebAssembly 應用中,可以繞過身份驗證檢查,因為使用者可以修改所有用戶端代碼。 這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。

新增下列內容：

* [Microsoft.AspNetCore.元件的](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/)包引用.對應用的專案檔的授權。
* `Microsoft.AspNetCore.Components.Authorization`應用 *_Imports.razor*檔的名稱空間。

要處理身份驗證,以下各節將介紹內置或自定義`AuthenticationStateProvider`服務的實現。

有關建立應用和設定的詳細資訊,請參閱<xref:security/blazor/webassembly/index>。

### <a name="opno-locblazor-server-authentication"></a>Blazor伺服器身份驗證

Blazor伺服器應用透過使用SignalR創建的即時連接運行。 建立連線時,將處理[SignalR基於中的應用程式中的身份驗證](xref:signalr/authn-and-authz)。 驗證可以是以 Cookie 或其他持有人權杖為基礎。

有關建立應用和設定的詳細資訊,請參閱<xref:security/blazor/server>。

## <a name="authenticationstateprovider-service"></a>AuthenticationStateProvider 服務

內置`AuthenticationStateProvider`服務從ASP.NET Core 獲取`HttpContext.User`身份驗證狀態數據。 這是身份驗證狀態與現有ASP.NET核心身份驗證機制集成的方式。

`AuthenticationStateProvider` 是 `AuthorizeView` 元件與 `CascadingAuthenticationState` 元件用來取得驗證狀態的基礎服務。

您通常不會直接使用 `AuthenticationStateProvider`。 使用本文後面介紹的[「授權檢視」元件](#authorizeview-component)或[\<任務 身份驗證狀態>](#expose-the-authentication-state-as-a-cascading-parameter)方法。 使用 `AuthenticationStateProvider` 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。

`AuthenticationStateProvider` 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：

```razor
@page "/"
@using System.Security.Claims
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<h3>ClaimsPrincipal Data</h3>

<button @onclick="GetClaimsPrincipalData">Get ClaimsPrincipal Data</button>

<p>@_authMessage</p>

@if (_claims.Count() > 0)
{
    <ul>
        @foreach (var claim in _claims)
        {
            <li>@claim.Type &ndash; @claim.Value</li>
        }
    </ul>
}

<p>@_surnameMessage</p>

@code {
    private string _authMessage;
    private string _surnameMessage;
    private IEnumerable<Claim> _claims = Enumerable.Empty<Claim>();

    private async Task GetClaimsPrincipalData()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            _authMessage = $"{user.Identity.Name} is authenticated.";
            _claims = user.Claims;
            _surnameMessage = 
                $"Surname: {user.FindFirst(c => c.Type == ClaimTypes.Surname)?.Value}";
        }
        else
        {
            _authMessage = "The user is NOT authenticated.";
        }
    }
}
```

如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。

如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。

## <a name="implement-a-custom-authenticationstateprovider"></a>實作自訂 AuthenticationStateProvider

如果應用程式需要自訂提供者,則實現`AuthenticationStateProvider`並重`GetAuthenticationStateAsync`寫 :

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

public class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

在BlazorWebAssembly 應用`CustomAuthStateProvider`中,`Main`服務註冊在*Program.cs*:

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

builder.Services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

在Blazor伺服器應用中`CustomAuthStateProvider`, 該服務`Startup.ConfigureServices`在中註冊:

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

`CustomAuthStateProvider`使用前面的示例中,所有使用者都使用使用者名`mrfibuli`進行身份驗證。

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>將驗證狀態公開為階層式參數

如果驗證狀態資料需要程序性邏輯 (例如在執行由使用者觸發的動作時)，請透過定義 `Task<AuthenticationState>` 類型的階層式參數來取得驗證狀態資料：

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

<p>@_authMessage</p>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private string _authMessage;

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            _authMessage = $"{user.Identity.Name} is authenticated.";
        }
        else
        {
            _authMessage = "The user is NOT authenticated.";
        }
    }
}
```

如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。

`Task<AuthenticationState>``AuthorizeRouteView`使用`App`元件中的元件`CascadingAuthenticationState`*(App.razor)* 設定級聯參數 :

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

在BlazorWebAssembly 應用程式中,將選項的服務與授權`Program.Main`加入 :

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

在BlazorServer 應用中,選項和授權的服務已經存在,因此無需執行進一步操作。

## <a name="authorization"></a>授權

在使用者被驗證之後，系統便會套用「授權」** 規則以控制使用者可以執行的動作。

存取權通常會依據下列情況來授與或拒絕：

* 使用者已通過驗證 (已登入)。
* 使用者屬於某個「角色」**。
* 使用者具有「宣告」**。
* 已滿足某個「原則」**。

上述概念皆和 ASP.NET Core MVC 或 Razor Pages 應用程式中的概念相同。 如需 ASP.NET Core 安全性的詳細資訊，請參閱 [ASP.NET Core 安全性與身分識別](xref:security/index)下的文章。

## <a name="authorizeview-component"></a>AuthorizeView 元件

`AuthorizeView` 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。 此方法可讓您只需要向使用者「顯示」** 資料，而不需要在程序性邏輯中使用該使用者的身分識別。

該元件會公開 `AuthenticationState` 類型的 `context` 變數，您可以使用它來存取已登入使用者的相關資訊：

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

您也可以在使用者未經授權的情況下，提供不同的顯示內容：

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

該`AuthorizeView`元件可用於`NavMenu`元件 *(Shared/NavMenu.razor)* 來`<li>...</li>``NavLink`顯示 的 清單項 ( ),但請注意,此方法僅從呈現的輸出中刪除清單項。 它不會阻止用戶導航到元件。

`<Authorized>`和`<NotAuthorized>`標記的內容可以包括任意專案,如其他互動式元件。

授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。

如果未指定授權條件，`AuthorizeView` 會使用預設的原則，並將：

* 已驗證 (已登入) 的使用者視為已授權。
* 未驗證 (已登出) 的使用者視為未經授權。

### <a name="role-based-and-policy-based-authorization"></a>角色型和原則型授權

`AuthorizeView` 元件支援「角色型」** 或「原則型」** 授權。

針對角色型授權，請使用 `Roles` 參數：

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

如需詳細資訊，請參閱 <xref:security/authorization/roles>。

針對原則型授權，請使用 `Policy` 參數：

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

宣告型授權是特殊案例的原則型授權。 例如，您可以定義要求使用者具備特定宣告的原則。 如需詳細資訊，請參閱 <xref:security/authorization/policies>。

這些 APIBlazor可用於Blazor伺服器或 Web 組裝應用。

如果未指定 `Roles` 和 `Policy`，`AuthorizeView` 便會使用預設原則。

### <a name="content-displayed-during-asynchronous-authentication"></a>在非同步驗證期間所顯示的內容

Blazor允許非*同步*確定身份驗證狀態。 此方法的主要方案是在BlazorWebAssembly 應用中,這些應用向外部終結點發出身份驗證請求。

在驗證期間，`AuthorizeView` 預設不會顯示任何內容。 若要在驗證發生時顯示內容，請使用 `<Authorizing>` 元素：

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

此方法通常不適用於Blazor伺服器應用。 Blazor伺服器應用在建立狀態后立即知道身份驗證狀態。 `Authorizing`內容可以在Blazor伺服器應用`AuthorizeView`的元件中提供,但內容永遠不會顯示。

## <a name="authorize-attribute"></a>[Authorize] 屬性

該`[Authorize]`屬性可用於 Razor 元件:

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> 僅對`[Authorize]``@page`通過路由器到達的Blazor元件使用。 授權僅會以路由的層面執行，且「不」** 適用於在頁面內轉譯的子元件。 若要授權在頁面內顯示特定組件，請改為使用 `AuthorizeView`。

`[Authorize]` 屬性也支援角色型或原則型授權。 針對角色型授權，請使用 `Roles` 參數：

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

針對原則型授權，請使用 `Policy` 參數：

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

如果未指定 `Roles` 與 `Policy`，`[Authorize]` 便會使用預設原則，它預設會將：

* 已驗證 (已登入) 的使用者視為已授權。
* 未驗證 (已登出) 的使用者視為未經授權。

## <a name="customize-unauthorized-content-with-the-router-component"></a>搭配 Router 元件自訂未經授權的內容

此`Router`元件與`AuthorizeRouteView`元件結合使用,允許應用程式在以下情況下指定自訂內容:

* 找不到內容。
* 使用者無法滿足套用至元件的 `[Authorize]` 條件。 屬性部分涵蓋該`[Authorize]`屬性[`[Authorize]`](#authorize-attribute)。
* 正在進行非同步驗證。

在預設Blazor的伺服器項目樣本中,`App`元件 (*App.razor*) 展示如何設定自訂內容:

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

和`<Authorizing>`標`<NotFound>`記`<NotAuthorized>`的內容 可以包括任意項,如其他互動式元件。

如果未指定`<NotAuthorized>`元素,`AuthorizeRouteView`則使用以下回退訊息:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>關於驗證狀態變更的通知

如果應用確定基礎身份驗證狀態數據已更改(例如,由於用戶註銷或其他使用者更改了其角色),[則自定義身份驗證狀態提供者](#implement-a-custom-authenticationstateprovider)可以選擇`NotifyAuthenticationStateChanged``AuthenticationStateProvider`調用 基類上的方法。 這會通知驗證狀態資料的取用者 (例如 `AuthorizeView`) 使用新資料來重新轉譯。

## <a name="procedural-logic"></a>程序性邏輯

如果作為程序性邏輯的一部分，應用程式必須檢查授權規則，請使用 `Task<AuthenticationState>` 類型的階層式參數來取得使用者的 <xref:System.Security.Claims.ClaimsPrincipal>。 `Task<AuthenticationState>` 可以與其他服務 (例如 `IAuthorizationService`) 結合來評估原則。

```razor
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

> [!NOTE]
> 在BlazorWebAssembly 套件中`Microsoft.AspNetCore.Authorization`,`Microsoft.AspNetCore.Components.Authorization`加入與命名空間:
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```
>
> 通過將這些命名空間添加到應用的 *_Imports.razor*檔案中,可以全域提供這些命名空間。

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a>Web 組Blazor裝 應用程式中的授權

在BlazorWebAssembly 應用中,可以繞過授權檢查,因為使用者可以修改所有用戶端代碼。 這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。

**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**

有關詳細資訊,請參閱<xref:security/blazor/webassembly/index>下的文章。

## <a name="troubleshoot-errors"></a>針對錯誤進行疑難排解

常見錯誤：

* **授權需要任務\<身份驗證狀態>類型的級聯參數。請考慮使用級聯身份驗證狀態來提供此功能。**

* **`null`收到的值`authenticationStateTask`**

專案很可能不是使用啟用身份驗證的Blazor伺服器範本創建的。 `<CascadingAuthenticationState>`環繞 UI 樹的某些部分,`App`例如在 元件 (*App.razor*) 中,如下所示:

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState` 會提供 `Task<AuthenticationState>` 階層式參數，它接著會接收自底層 `AuthenticationStateProvider` DI 服務。

## <a name="additional-resources"></a>其他資源

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* [真棒Blazor: 驗證](https://github.com/AdrienTorris/awesome-blazor#authentication)社區範例
