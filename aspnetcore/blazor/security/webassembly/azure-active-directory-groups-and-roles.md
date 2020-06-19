---
title: ASP.NET Core Blazor 具有 Azure Active Directory 群組和角色的 WebAssembly
author: guardrex
description: 瞭解如何將 Blazor WebAssembly 設定為使用 Azure Active Directory 群組和角色。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/aad-groups-roles
ms.openlocfilehash: 99ebe43da191153aa98cce6bae8fe98035bc7d6f
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103766"
---
# <a name="azure-ad-groups-administrative-roles-and-user-defined-roles"></a>Azure AD 群組、系統管理角色和使用者定義的角色

By [Luke Latham](https://github.com/guardrex)和[Javier Calvarro Nelson](https://github.com/javiercn)

Azure Active Directory （AAD）提供數個可與 ASP.NET Core 結合的授權方法 Identity ：

* 使用者定義的群組
  * 安全性
  * O365
  * 散發
* 角色
  * 內建的系統管理角色
  * 使用者定義角色

本文中的指導方針適用于 Blazor 下列主題中所述的 WEBASSEMBLY AAD 部署案例：

* [獨立的 Microsoft 帳戶](xref:blazor/security/webassembly/standalone-with-microsoft-accounts)
* [獨立的 AAD](xref:blazor/security/webassembly/standalone-with-azure-active-directory)
* [使用 AAD 託管](xref:blazor/security/webassembly/hosted-with-azure-active-directory)

### <a name="user-defined-groups-and-built-in-administrative-roles"></a>使用者定義的群組和內建的系統管理角色

若要在 Azure 入口網站中設定應用程式以提供 `groups` 成員資格宣告，請參閱下列 Azure 文章。 將使用者指派給使用者定義的 AAD 群組和內建的系統管理角色。

* [使用 Azure AD 安全性群組的角色](/azure/architecture/multitenant-identity/app-roles#roles-using-azure-ad-security-groups)
* [groupMembershipClaims 屬性](/azure/active-directory/develop/reference-app-manifest#groupmembershipclaims-attribute)

下列範例假設已將使用者指派給 AAD 內建*計費管理員*角色。

AAD 所傳送的單一宣告會將 `groups` 使用者的群組和角色顯示為 JSON 陣列中的物件識別碼（guid）。 應用程式必須將群組和角色的 JSON 陣列轉換成 `group` 應用程式可針對其建立[原則](xref:security/authorization/policies)的個別宣告。

擴充 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteUserAccount> 以包含群組和角色的陣列屬性。

*CustomUserAccount.cs*：

```csharp
using System.Text.Json.Serialization;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class CustomUserAccount : RemoteUserAccount
{
    [JsonPropertyName("groups")]
    public string[] Groups { get; set; }

    [JsonPropertyName("roles")]
    public string[] Roles { get; set; }
}
```

在託管解決方案的獨立應用程式或用戶端應用程式中，建立自訂的使用者 factory。 下列 factory 也設定為處理宣告 `roles` 陣列，其涵蓋于[使用者定義的角色](#user-defined-roles)一節中：

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;

public class CustomUserFactory
    : AccountClaimsPrincipalFactory<CustomUserAccount>
{
    public CustomUserFactory(NavigationManager navigationManager,
        IAccessTokenProviderAccessor accessor)
        : base(accessor)
    {
    }

    public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
        CustomUserAccount account,
        RemoteAuthenticationUserOptions options)
    {
        var initialUser = await base.CreateUserAsync(account, options);

        if (initialUser.Identity.IsAuthenticated)
        {
            var userIdentity = (ClaimsIdentity)initialUser.Identity;

            foreach (var role in account.Roles)
            {
                userIdentity.AddClaim(new Claim("role", role));
            }

            foreach (var group in account.Groups)
            {
                userIdentity.AddClaim(new Claim("group", group));
            }
        }

        return initialUser;
    }
}
```

不需要提供程式碼來移除原始宣告， `groups` 因為架構會自動移除該宣告。

在 `Program.Main` 託管解決方案的獨立應用程式或用戶端應用程式的（*Program.cs*）中註冊 factory：

```csharp
builder.Services.AddMsalAuthentication<RemoteAuthenticationState, 
    CustomUserAccount>(options =>
{
    builder.Configuration.Bind("AzureAd", 
        options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("...");
    
    ...
})
.AddAccountClaimsPrincipalFactory<RemoteAuthenticationState, CustomUserAccount, 
    CustomUserFactory>();
```

為中的每個群組或角色建立[原則](xref:security/authorization/policies) `Program.Main` 。 下列範例會建立 AAD 內建*計費管理員*角色的原則：

```csharp
builder.Services.AddAuthorizationCore(options =>
{
    options.AddPolicy("BillingAdministrator", policy => 
        policy.RequireClaim("group", "69ff516a-b57d-4697-a429-9de4af7b5609"));
});
```

如需 AAD 角色物件識別碼的完整清單，請參閱[Aad 系統管理角色群組識別碼](#aad-adminstrative-role-group-ids)一節。

在下列範例中，應用程式會使用上述原則來授權使用者。

[AuthorizeView 元件](xref:blazor/security/index#authorizeview-component)適用于下列原則：

```razor
<AuthorizeView Policy="BillingAdministrator">
    <Authorized>
        <p>
            The user is in the 'Billing Administrator' AAD Administrative Role
            and can see this content.
        </p>
    </Authorized>
    <NotAuthorized>
        <p>
            The user is NOT in the 'Billing Administrator' role and sees this
            content.
        </p>
    </NotAuthorized>
</AuthorizeView>
```

您可以使用[ `[Authorize]` attribute](xref:blazor/security/index#authorize-attribute)指示詞（），根據原則來存取整個元件 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ：

```razor
@page "/"
@using Microsoft.AspNetCore.Authorization
@attribute [Authorize(Policy = "BillingAdministrator")]
```

如果使用者未登入，系統會將他們重新導向至 AAD 登入頁面，並在登入後回到該元件。

原則檢查也可以在程式[代碼中以程式邏輯執行](xref:blazor/security/index#procedural-logic)：

```razor
@page "/checkpolicy"
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

<h1>Check Policy</h1>

<p>This component checks a policy in code.</p>

<button @onclick="CheckPolicy">Check 'BillingAdministrator' policy</button>

<p>Policy Message: @policyMessage</p>

@code {
    private string policyMessage = "Check hasn't been made yet.";

    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async void CheckPolicy()
    {
        var user = (await authenticationStateTask).User;

        if ((await AuthorizationService.AuthorizeAsync(user, 
            "BillingAdministrator")).Succeeded)
        {
            policyMessage = "Yes! The 'BillingAdministrator' policy is met.";
        }
        else
        {
            policyMessage = "No! 'BillingAdministrator' policy is NOT met.";
        }
    }
}
```

### <a name="user-defined-roles"></a>使用者定義角色

AAD 註冊的應用程式也可以設定為使用使用者定義的角色。

若要在 Azure 入口網站中設定應用程式以提供 `roles` 成員資格宣告，請參閱[如何：在您的應用程式中新增應用程式角色，並在](/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)Azure 檔的權杖中加以接收。

下列範例假設應用程式已設定兩個角色：

* `admin`
* `developer`

> [!NOTE]
> 雖然您無法將角色指派給沒有 Azure AD Premium 帳戶的安全性群組，您可以將使用者指派給角色，並接收 `roles` 具有標準 Azure 帳戶之使用者的宣告。 本節中的指引不需要 Azure AD Premium 帳戶。
>
> 在 Azure 入口網站中指派多個角色，方法是為每個額外的角色指派**_重新加入使用者_**。

AAD 所傳送的單一宣告會將 `roles` 使用者定義的角色顯示為 `appRoles` `value` JSON 陣列中的 s。 應用程式必須將角色的 JSON 陣列轉換成個別 `role` 宣告。

`CustomUserFactory`[[使用者定義群組] 和 [AAD 內建系統管理角色](#user-defined-groups-and-built-in-administrative-roles)] 區段中所顯示的，會設定為在 `roles` 具有 JSON 陣列值的宣告上採取動作。 `CustomUserFactory`在裝載解決方案的獨立應用程式或用戶端應用程式中，新增並註冊，如[使用者定義群組和 AAD 內建系統管理角色](#user-defined-groups-and-built-in-administrative-roles)一節中所示。 不需要提供程式碼來移除原始宣告， `roles` 因為架構會自動移除該宣告。

在 `Program.Main` 託管解決方案的獨立應用程式或用戶端應用程式中，將名為 "" 的宣告指定 `role` 為角色宣告：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...

    options.UserOptions.RoleClaim = "role";
});
```

此時，元件授權方法會正常運作。 元件中的任何授權機制都可以使用 `admin` 角色來授權使用者：

* [AuthorizeView 元件](xref:blazor/security/index#authorizeview-component)（範例： `<AuthorizeView Roles="admin">` ）
* [ `[Authorize]` attribute](xref:blazor/security/index#authorize-attribute)指示詞（ <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ）（範例： `@attribute [Authorize(Roles = "admin")]` ）
* 程式[邏輯](xref:blazor/security/index#procedural-logic)（範例： `if (user.IsInRole("admin")) { ... }` ）

  支援多個角色測試：

  ```csharp
  if (user.IsInRole("admin") && user.IsInRole("developer"))
  {
      ...
  }
  ```

## <a name="aad-adminstrative-role-group-ids"></a>AAD 系統管理角色群組識別碼

下表中顯示的物件識別碼是用來建立宣告[policies](xref:security/authorization/policies)的原則 `group` 。 原則允許應用程式授權使用者使用應用程式中的各種活動。 如需詳細資訊，請參閱[使用者定義群組和 AAD 內建系統管理角色](#user-defined-groups-and-built-in-administrative-roles)一節。

AAD 系統管理角色 | 物件識別碼
--- | ---
應用程式管理員 | fa11557b-4f15-4ddd-85d5-313c7cd74047
應用程式開發人員 | 68adcbb8-9504-44f6-89f2-5cd48dc74a2c
驗證系統管理員 | 02d110a1-96b1-419e-af87-746461b60ed7
Azure DevOps 管理員 | a5311ace-ca41-44cd-b833-8d22caa0b34f
Azure 資訊保護管理員 | 18632dce-f9b5-4f01-abb5-37051f06860e
B2C IEF 索引鍵集管理員 | 0c2e87e5-94f9-4adb-ae8c-bcafe11bd368
B2C IEF 原則管理員 | bfcab36c-10c6-4b13-b63c-4d8b62c0c44e
B2C 使用者流程管理員 | baa531b7-8cf0-44ad-8f98-eded88dae827
B2C 使用者流程屬性管理員 | dd0baca0-a535-48c1-b871-8431abe16452
計費管理員 | 69ff516a-b57d-4697-a429-9de4af7b5609
雲端應用程式系統管理員 | 250b5fe3-b553-458d-9a53-b782c13c34bf
雲端裝置管理員 | 26cd4b44-2636-4ddb-bdfa-27feae66f86d
規範管理員 | 9d6e1dd0-c9f8-45f8-b558-b134f700116c
合規性資料管理員 | 4c0ca3a2-231e-416c-9411-4abe57d5cb9d
條件式存取系統管理員 | 8f71a611-137d-49af-87ad-e97f1fd5da76
客戶 LockBox 存取核准者 | c18d54a8-b13e-4954-a1a4-7deaf2e4f184
電腦分析系統管理員 | c62c4ac5-e4c6-4096-8a2f-1ee3cbaaae15
目錄讀取器 | e1fc84a6-7762-4b9b-8e29-518b4adbc23b
Dynamics 365 管理員 | f20a9cfa-9fdf-49a8-a977-1afe446a1d6e
Exchange 系統管理員 | b2ec2cc0-d5c9-4864-ad9b-38dd9dba2652
外部 Identity 提供者系統管理員 | febfaeb4-e478-407a-b4b3-f4d9716618a2
全域管理員 | a45ba61b-44db-462c-924b-3b2719152588
全域讀取者 | f6903b21-6aba-4124-b44c-76671796b9d5
群組管理員 | 158b3e5a-d89d-460b-92b5-3b34985f0197
來賓邀請者 | 4c730a1d-cc22-44af-8f9f-4eec635c7502
服務台管理員 | 108678c8-6628-44e1-8d01-caf598a6a5f5
Intune 管理員 | 79950741-23fa-4189-b2cb-46640601c497
Kaizala 管理員 | d6322af2-48e7-42e0-8c68-0bbe31af3412
授權管理員 | 3355458a-e423-44bf-8b98-4ac5e572cea5
訊息中心隱私權讀取者 | 6395db95-9fb8-42b9-b1ed-30a2405eee6f
訊息中心讀取者 | fd5d37b8-4e24-434b-9e63-70ed3b759a16
Office 應用程式管理員 | 5f3870cd-b042-4f93-86d7-c9d77c664dc7
密碼管理員 | 466e48b7-5d66-4ae5-8911-1a118de74941
Power BI 管理員 | 984e83b8-8337-4255-91a1-acb663175ab4
Power Platform 管理員 | 76d6f95e-9a15-4d7d-8d21-00de00faf9fd
特殊權限驗證管理員 | 0829f731-b46d-419f-9742-aeb122367d11
特殊權限角色管理員 | f20a725a-d1c8-4107-83ea-1171c97d00c7
報表讀者 | 54635450-e8ed-4f2d-9632-07db2517b4de
搜尋管理員 | c770a2f1-c9ba-4e60-9176-9f52b1eb1a31
搜尋編輯者 | 6a6858c6-5f0d-44ac-87c7-0190fbedd271
安全性系統管理員 | 20fa50e3-6531-44d8-bd39-b251420568ad
安全性操作員 | 43aae017-8e51-4188-91ab-e6debd572800
安全性讀取者 | 45035cd3-fd97-4250-8197-3a53d3562d9b
服務支援管理員 | 2c92cf45-c914-48f8-9bf9-fc14b28818ab
SharePoint 管理員 | e1c32229-875e-461d-ae24-3cb99116e86c
商務用 Skype 的管理員 | 0a8cee12-e21d-43ef-abd9-f1ea85710e30
Microsoft Teams 通訊系統管理員 | 2393e455-6e13-4743-9f52-63fcec2b6a9c
Microsoft Teams 通訊支援工程師 | 802dd94e-d717-46f6-af98-b9167071e9fc
小組溝通專家 | ef547281-cf46-4cc6-bcaa-f5eac3f030c9
Microsoft Teams 服務管理員 | 8846a0be-197b-443a-b13c-11192691fa24
使用者管理員 | 1f6eed58-7dd3-460b-a298-666f975427a1

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/claims>
* <xref:blazor/security/index>
