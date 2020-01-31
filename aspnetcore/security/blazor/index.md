---
title: ASP.NET Core Blazor 驗證和授權
author: guardrex
description: 深入瞭解 Blazor 驗證和授權案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: e9087c246f4805e5931180fa0869fc8a8d23a6c1
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885592"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>ASP.NET Core Blazor 驗證與授權

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

ASP.NET Core 支援在 Blazor 應用程式中設定及管理安全性。

Blazor 伺服器與 Blazor WebAssembly 應用程式之間的安全性案例不同。 由於 Blazor 伺服器應用程式是在伺服器上執行，因此授權檢查能夠判斷：

* 要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。
* 適用於應用程式和元件之特定區域的存取規則。

Blazor WebAssembly 應用程式會在用戶端上執行。 授權「僅」會被用來決定要顯示的 UI 選項。 由於用戶端檢查可由使用者修改或略過，因此 Blazor WebAssembly 應用程式無法強制執行授權存取規則。

## <a name="authentication"></a>驗證  (可能為英文網頁)

Blazor 會使用現有的 ASP.NET Core 驗證機制來建立使用者的身分識別。 確切的機制取決於 Blazor 應用程式的主控方式、Blazor 伺服器或 Blazor WebAssembly。

### <a name="blazor-server-authentication"></a>Blazor 伺服器驗證

Blazor 伺服器應用程式會在使用 SignalR 建立的即時連線上運作。 [SignalR 型應用程式中的驗證](xref:signalr/authn-and-authz)會在連線建立時處理。 驗證可以是以 Cookie 或其他持有人權杖為基礎。

當您建立專案時，Blazor 伺服器專案範本可以為您設定驗證。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

遵循 <xref:blazor/get-started> 文章中的 Visual Studio 指導方針，使用驗證機制建立新的 Blazor 伺服器專案。

在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中選擇 [Blazor 伺服器應用程式] 範本之後，請選取 [驗證] 下的 [變更]。

對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：

* **無驗證**
* 可以儲存 &ndash; 使用者帳戶的**個別使用者帳戶**：
  * 使用 ASP.NET Core 的[身分識別](xref:security/authentication/identity)系統儲存在應用程式內。
  * 使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。
* **公司或學校帳戶**
* **Windows 驗證**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

遵循 <xref:blazor/get-started> 文章中的 Visual Studio Code 指導方針，使用驗證機制建立新的 Blazor 伺服器專案：

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制                                                                 | `{AUTHENTICATION}` 值 |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| 無驗證                                                                        | `None`                   |
| 個人<br>搭配 ASP.NET Core 身分識別儲存在應用程式中的使用者。                        | `Individual`             |
| 個人<br>儲存在 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 中的使用者。 | `IndividualB2C`          |
| 公司或學校帳戶<br>適用於單一租用戶的組織驗證。            | `SingleOrg`              |
| 公司或學校帳戶<br>適用於多個租用戶的組織驗證。           | `MultiOrg`               |
| Windows 驗證                                                                   | `Windows`                |

命令會建立搭配為 `{APP NAME}` 所提供的值來命名的資料夾，並會使用該資料夾名稱作為應用程式的名稱。 如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor WebAssembly authentication

在 Blazor WebAssembly apps 中，可以略過驗證檢查，因為使用者可以修改所有的用戶端程式代碼。 這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。

將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/)的套件參考新增至應用程式的專案檔。

下列各節涵蓋 Blazor WebAssembly 應用程式的自訂 `AuthenticationStateProvider` 服務的執行。

## <a name="authenticationstateprovider-service"></a>AuthenticationStateProvider 服務

Blazor 伺服器應用程式包含內建 `AuthenticationStateProvider` 服務，可從 ASP.NET Core 的 `HttpContext.User`取得驗證狀態資料。 這就是驗證狀態與現有 ASP.NET Core 伺服器端驗證機制之間的整合方式。

`AuthenticationStateProvider` 是 `AuthorizeView` 元件與 `CascadingAuthenticationState` 元件用來取得驗證狀態的基礎服務。

您通常不會直接使用 `AuthenticationStateProvider`。 請使用此文章稍後所述的 [AuthorizeView 元件](#authorizeview-component)或 [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) 方法。 使用 `AuthenticationStateProvider` 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。

`AuthenticationStateProvider` 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。

如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。

## <a name="implement-a-custom-authenticationstateprovider"></a>實作自訂 AuthenticationStateProvider

如果您要建立 Blazor WebAssembly 應用程式，或如果您的應用程式規格確實需要自訂提供者，請執行提供者並覆寫 `GetAuthenticationStateAsync`：

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
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
}
```

在 Blazor WebAssembly 應用程式中，`CustomAuthStateProvider` 服務會在*Program.cs*的 `Main` 中註冊：

```csharp
using Microsoft.AspNetCore.Blazor.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

使用 `CustomAuthStateProvider` 時，所有使用者都會以 `mrfibuli` 的使用者名稱進行驗證。

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>將驗證狀態公開為階層式參數

如果驗證狀態資料需要程序性邏輯 (例如在執行由使用者觸發的動作時)，請透過定義 `Task<AuthenticationState>` 類型的階層式參數來取得驗證狀態資料：

```razor
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

> [!NOTE]
> 在 Blazor WebAssembly 應用程式元件中，新增 `Microsoft.AspNetCore.Components.Authorization` 命名空間（`@using Microsoft.AspNetCore.Components.Authorization`）。

如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。

使用*應用程式 razor*檔案中的 `AuthorizeRouteView` 和 `CascadingAuthenticationState` 元件，設定 `Task<AuthenticationState>` 級聯的參數：

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

## <a name="authorization"></a>授權

在使用者被驗證之後，系統便會套用「授權」規則以控制使用者可以執行的動作。

存取權通常會依據下列情況來授與或拒絕：

* 使用者已通過驗證 (已登入)。
* 使用者屬於某個「角色」。
* 使用者具有「宣告」。
* 已滿足某個「原則」。

上述概念皆和 ASP.NET Core MVC 或 Razor Pages 應用程式中的概念相同。 如需 ASP.NET Core 安全性的詳細資訊，請參閱 [ASP.NET Core 安全性與身分識別](xref:security/index)下的文章。

## <a name="authorizeview-component"></a>AuthorizeView 元件

`AuthorizeView` 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。 此方法可讓您只需要向使用者「顯示」資料，而不需要在程序性邏輯中使用該使用者的身分識別。

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

`<Authorized>` 和 `<NotAuthorized>` 標記的內容可以包含任意專案，例如其他互動式元件。

授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。

如果未指定授權條件，`AuthorizeView` 會使用預設的原則，並將：

* 已驗證 (已登入) 的使用者視為已授權。
* 未驗證 (已登出) 的使用者視為未經授權。

### <a name="role-based-and-policy-based-authorization"></a>角色型和原則型授權

`AuthorizeView` 元件支援「角色型」或「原則型」授權。

針對角色型授權，請使用 `Roles` 參數：

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

如需詳細資訊，請參閱<xref:security/authorization/roles>。

針對原則型授權，請使用 `Policy` 參數：

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

宣告型授權是特殊案例的原則型授權。 例如，您可以定義要求使用者具備特定宣告的原則。 如需詳細資訊，請參閱<xref:security/authorization/policies>。

這些 Api 可以在 Blazor Server 或 Blazor WebAssembly 應用程式中使用。

如果未指定 `Roles` 和 `Policy`，`AuthorizeView` 便會使用預設原則。

### <a name="content-displayed-during-asynchronous-authentication"></a>在非同步驗證期間所顯示的內容

Blazor 允許以*非同步方式*判斷驗證狀態。 這種方法的主要案例是在 Blazor WebAssembly 應用程式中提出對外部端點進行驗證的要求。

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

這種方法通常不適用於 Blazor 伺服器應用程式。 Blazor 伺服器應用程式會在狀態建立後立即知道驗證狀態。 `Authorizing` 內容可以在 Blazor 伺服器應用程式的 `AuthorizeView` 元件中提供，但永遠不會顯示內容。

## <a name="authorize-attribute"></a>[Authorize] 屬性

`[Authorize]` 屬性可以在 Razor 元件中使用：

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> 在 Blazor WebAssembly 應用程式元件中，將 `Microsoft.AspNetCore.Authorization` 命名空間（`@using Microsoft.AspNetCore.Authorization`）新增至本節中的範例。

> [!IMPORTANT]
> 請只在透過 Blazor 路由器到達 `@page` 元件上使用 `[Authorize]`。 授權僅會以路由的層面執行，且「不」適用於在頁面內轉譯的子元件。 若要授權在頁面內顯示特定組件，請改為使用 `AuthorizeView`。

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

`Router` 元件與 `AuthorizeRouteView` 元件結合，可讓應用程式在下列情況指定自訂內容：

* 找不到內容。
* 使用者無法滿足套用至元件的 `[Authorize]` 條件。 [`[Authorize]` 屬性](#authorize-attribute)一節涵蓋 `[Authorize]` 屬性。
* 正在進行非同步驗證。

在預設的 Blazor Server 專案範本中，*應用程式 razor*檔案示範如何設定自訂內容：

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

`<NotFound>`、`<NotAuthorized>`和 `<Authorizing>` 標記的內容可以包含任意專案，例如其他互動式元件。

如果未指定 `<NotAuthorized>` 元素，則 `AuthorizeRouteView` 會使用下列回溯訊息：

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>關於驗證狀態變更的通知

如果應用程式判斷基礎驗證狀態資料已經變更 (例如由於使用者已登出，或是另一個使用者已變更其角色)，自訂的 `AuthenticationStateProvider` 可以選擇性地在 `AuthenticationStateProvider` 基底類別上叫用 `NotifyAuthenticationStateChanged` 方法。 這會通知驗證狀態資料的取用者 (例如 `AuthorizeView`) 使用新資料來重新轉譯。

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
> 在 Blazor WebAssembly 應用程式元件中，新增 `Microsoft.AspNetCore.Authorization` 和 `Microsoft.AspNetCore.Components.Authorization` 命名空間：
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a>Blazor WebAssembly 應用程式中的授權

在 Blazor WebAssembly apps 中，可以略過授權檢查，因為使用者可以修改所有的用戶端程式代碼。 這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。

**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**

## <a name="troubleshoot-errors"></a>疑難排解錯誤

常見錯誤：

* **授權需要類型為 Task\<AuthenticationState > 的串聯參數。請考慮使用 CascadingAuthenticationState 來提供此。**

* **針對 `authenticationStateTask` 接收到 `null` 值**

專案可能不是使用已啟用驗證的 Blazor 伺服器範本來建立。 請將 `<CascadingAuthenticationState>` 包裝在 UI 樹狀的一部分，例如在 *App.razor* 中，如下所示：

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
