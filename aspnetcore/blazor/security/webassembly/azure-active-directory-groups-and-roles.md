---
title: Blazor WebAssembly具有 Azure Active Directory 群組和角色的 ASP.NET Core
author: guardrex
description: 瞭解如何設定 Blazor WebAssembly ，以使用 Azure Active Directory 群組和角色。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/aad-groups-roles
ms.openlocfilehash: 6e27b062d7b5a1b72804fe5d4ea31ec65358ce45
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85402152"
---
# <a name="azure-ad-groups-administrative-roles-and-user-defined-roles"></a><span data-ttu-id="d7274-103">Azure AD 群組、系統管理角色和使用者定義的角色</span><span class="sxs-lookup"><span data-stu-id="d7274-103">Azure AD Groups, Administrative Roles, and user-defined roles</span></span>

<span data-ttu-id="d7274-104">By [Luke Latham](https://github.com/guardrex)和[Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="d7274-104">By [Luke Latham](https://github.com/guardrex) and [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

<span data-ttu-id="d7274-105">Azure Active Directory （AAD）提供數個可與 ASP.NET Core 結合的授權方法 Identity ：</span><span class="sxs-lookup"><span data-stu-id="d7274-105">Azure Active Directory (AAD) provides several authorization approaches that can be combined with ASP.NET Core Identity:</span></span>

* <span data-ttu-id="d7274-106">使用者定義的群組</span><span class="sxs-lookup"><span data-stu-id="d7274-106">User-defined groups</span></span>
  * <span data-ttu-id="d7274-107">安全性</span><span class="sxs-lookup"><span data-stu-id="d7274-107">Security</span></span>
  * <span data-ttu-id="d7274-108">O365</span><span class="sxs-lookup"><span data-stu-id="d7274-108">O365</span></span>
  * <span data-ttu-id="d7274-109">散發</span><span class="sxs-lookup"><span data-stu-id="d7274-109">Distribution</span></span>
* <span data-ttu-id="d7274-110">角色</span><span class="sxs-lookup"><span data-stu-id="d7274-110">Roles</span></span>
  * <span data-ttu-id="d7274-111">內建的系統管理角色</span><span class="sxs-lookup"><span data-stu-id="d7274-111">Built-in Administrative Roles</span></span>
  * <span data-ttu-id="d7274-112">使用者定義角色</span><span class="sxs-lookup"><span data-stu-id="d7274-112">User-defined roles</span></span>

<span data-ttu-id="d7274-113">本文中的指導方針適用于 Blazor WebAssembly 下列主題中所述的 AAD 部署案例：</span><span class="sxs-lookup"><span data-stu-id="d7274-113">The guidance in this article applies to the Blazor WebAssembly AAD deployment scenarios described in the following topics:</span></span>

* [<span data-ttu-id="d7274-114">獨立的 Microsoft 帳戶</span><span class="sxs-lookup"><span data-stu-id="d7274-114">Standalone with Microsoft Accounts</span></span>](xref:blazor/security/webassembly/standalone-with-microsoft-accounts)
* [<span data-ttu-id="d7274-115">獨立的 AAD</span><span class="sxs-lookup"><span data-stu-id="d7274-115">Standalone with AAD</span></span>](xref:blazor/security/webassembly/standalone-with-azure-active-directory)
* [<span data-ttu-id="d7274-116">使用 AAD 託管</span><span class="sxs-lookup"><span data-stu-id="d7274-116">Hosted with AAD</span></span>](xref:blazor/security/webassembly/hosted-with-azure-active-directory)

### <a name="user-defined-groups-and-built-in-administrative-roles"></a><span data-ttu-id="d7274-117">使用者定義的群組和內建的系統管理角色</span><span class="sxs-lookup"><span data-stu-id="d7274-117">User-defined groups and built-in Administrative Roles</span></span>

<span data-ttu-id="d7274-118">若要在 Azure 入口網站中設定應用程式以提供 `groups` 成員資格宣告，請參閱下列 Azure 文章。</span><span class="sxs-lookup"><span data-stu-id="d7274-118">To configure the app in the Azure portal to provide a `groups` membership claim, see the following Azure articles.</span></span> <span data-ttu-id="d7274-119">將使用者指派給使用者定義的 AAD 群組和內建的系統管理角色。</span><span class="sxs-lookup"><span data-stu-id="d7274-119">Assign users to user-defined AAD groups and built-in Administrative Roles.</span></span>

* [<span data-ttu-id="d7274-120">使用 Azure AD 安全性群組的角色</span><span class="sxs-lookup"><span data-stu-id="d7274-120">Roles using Azure AD security groups</span></span>](/azure/architecture/multitenant-identity/app-roles#roles-using-azure-ad-security-groups)
* [<span data-ttu-id="d7274-121">`groupMembershipClaims`特性</span><span class="sxs-lookup"><span data-stu-id="d7274-121">`groupMembershipClaims` attribute</span></span>](/azure/active-directory/develop/reference-app-manifest#groupmembershipclaims-attribute)

<span data-ttu-id="d7274-122">下列範例假設已將使用者指派給 AAD 內建*計費管理員*角色。</span><span class="sxs-lookup"><span data-stu-id="d7274-122">The following examples assume that a user is assigned to the AAD built-in *Billing Administrator* role.</span></span>

<span data-ttu-id="d7274-123">AAD 所傳送的單一宣告會將 `groups` 使用者的群組和角色顯示為 JSON 陣列中的物件識別碼（guid）。</span><span class="sxs-lookup"><span data-stu-id="d7274-123">The single `groups` claim sent by AAD presents the user's groups and roles as Object IDs (GUIDs) in a JSON array.</span></span> <span data-ttu-id="d7274-124">應用程式必須將群組和角色的 JSON 陣列轉換成 `group` 應用程式可針對其建立[原則](xref:security/authorization/policies)的個別宣告。</span><span class="sxs-lookup"><span data-stu-id="d7274-124">The app must convert the JSON array of groups and roles into individual `group` claims that the app can build [policies](xref:security/authorization/policies) against.</span></span>

<span data-ttu-id="d7274-125">擴充 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteUserAccount> 以包含群組和角色的陣列屬性。</span><span class="sxs-lookup"><span data-stu-id="d7274-125">Extend <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteUserAccount> to include array properties for groups and roles.</span></span>

<span data-ttu-id="d7274-126">`CustomUserAccount.cs`:</span><span class="sxs-lookup"><span data-stu-id="d7274-126">`CustomUserAccount.cs`:</span></span>

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

<span data-ttu-id="d7274-127">在託管解決方案的獨立應用程式或用戶端應用程式中，建立自訂的使用者 factory。</span><span class="sxs-lookup"><span data-stu-id="d7274-127">Create a custom user factory in the standalone app or Client app of a Hosted solution.</span></span> <span data-ttu-id="d7274-128">下列 factory 也設定為處理宣告 `roles` 陣列，其涵蓋于[使用者定義的角色](#user-defined-roles)一節中：</span><span class="sxs-lookup"><span data-stu-id="d7274-128">The following factory is also configured to handle `roles` claim arrays, which are covered in the [User-defined roles](#user-defined-roles) section:</span></span>

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

<span data-ttu-id="d7274-129">不需要提供程式碼來移除原始宣告， `groups` 因為架構會自動移除該宣告。</span><span class="sxs-lookup"><span data-stu-id="d7274-129">There's no need to provide code to remove the original `groups` claim because it's automatically removed by the framework.</span></span>

<span data-ttu-id="d7274-130">在裝載解決方案的 `Program.Main` `Program.cs` 獨立應用程式或用戶端應用程式的（）中註冊 factory：</span><span class="sxs-lookup"><span data-stu-id="d7274-130">Register the factory in `Program.Main` (`Program.cs`) of the standalone app or Client app of a Hosted solution:</span></span>

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

<span data-ttu-id="d7274-131">為中的每個群組或角色建立[原則](xref:security/authorization/policies) `Program.Main` 。</span><span class="sxs-lookup"><span data-stu-id="d7274-131">Create a [policy](xref:security/authorization/policies) for each group or role in `Program.Main`.</span></span> <span data-ttu-id="d7274-132">下列範例會建立 AAD 內建*計費管理員*角色的原則：</span><span class="sxs-lookup"><span data-stu-id="d7274-132">The following example creates a policy for the AAD built-in *Billing Administrator* role:</span></span>

```csharp
builder.Services.AddAuthorizationCore(options =>
{
    options.AddPolicy("BillingAdministrator", policy => 
        policy.RequireClaim("group", "69ff516a-b57d-4697-a429-9de4af7b5609"));
});
```

<span data-ttu-id="d7274-133">如需 AAD 角色物件識別碼的完整清單，請參閱[Aad 系統管理角色群組識別碼](#aad-adminstrative-role-group-ids)一節。</span><span class="sxs-lookup"><span data-stu-id="d7274-133">For the complete list of AAD role Object IDs, see the [AAD Adminstrative Role Group IDs](#aad-adminstrative-role-group-ids) section.</span></span>

<span data-ttu-id="d7274-134">在下列範例中，應用程式會使用上述原則來授權使用者。</span><span class="sxs-lookup"><span data-stu-id="d7274-134">In the following examples, the app uses the preceding policy to authorize the user.</span></span>

<span data-ttu-id="d7274-135">此[ `AuthorizeView` 元件](xref:blazor/security/index#authorizeview-component)可與原則搭配運作：</span><span class="sxs-lookup"><span data-stu-id="d7274-135">The [`AuthorizeView` component](xref:blazor/security/index#authorizeview-component) works with the policy:</span></span>

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

<span data-ttu-id="d7274-136">您可以使用[ `[Authorize]` attribute](xref:blazor/security/index#authorize-attribute)指示詞（），根據原則來存取整個元件 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ：</span><span class="sxs-lookup"><span data-stu-id="d7274-136">Access to an entire component can be based on the policy using [`[Authorize]` attribute directive](xref:blazor/security/index#authorize-attribute) (<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>):</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Authorization
@attribute [Authorize(Policy = "BillingAdministrator")]
```

<span data-ttu-id="d7274-137">如果使用者未登入，系統會將他們重新導向至 AAD 登入頁面，並在登入後回到該元件。</span><span class="sxs-lookup"><span data-stu-id="d7274-137">If the user isn't logged in, they're redirected to the AAD sign-in page and then back to the component after they sign in.</span></span>

<span data-ttu-id="d7274-138">原則檢查也可以在程式[代碼中以程式邏輯執行](xref:blazor/security/index#procedural-logic)：</span><span class="sxs-lookup"><span data-stu-id="d7274-138">A policy check can also be [performed in code with procedural logic](xref:blazor/security/index#procedural-logic):</span></span>

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

### <a name="user-defined-roles"></a><span data-ttu-id="d7274-139">使用者定義角色</span><span class="sxs-lookup"><span data-stu-id="d7274-139">User-defined roles</span></span>

<span data-ttu-id="d7274-140">AAD 註冊的應用程式也可以設定為使用使用者定義的角色。</span><span class="sxs-lookup"><span data-stu-id="d7274-140">An AAD-registered app can also be configured to use user-defined roles.</span></span>

<span data-ttu-id="d7274-141">若要在 Azure 入口網站中設定應用程式以提供 `roles` 成員資格宣告，請參閱[如何：在您的應用程式中新增應用程式角色，並在](/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)Azure 檔的權杖中加以接收。</span><span class="sxs-lookup"><span data-stu-id="d7274-141">To configure the app in the Azure portal to provide a `roles` membership claim, see [How to: Add app roles in your application and receive them in the token](/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) in the Azure documentation.</span></span>

<span data-ttu-id="d7274-142">下列範例假設應用程式已設定兩個角色：</span><span class="sxs-lookup"><span data-stu-id="d7274-142">The following example assumes that an app is configured with two roles:</span></span>

* `admin`
* `developer`

> [!NOTE]
> <span data-ttu-id="d7274-143">雖然您無法將角色指派給沒有 Azure AD Premium 帳戶的安全性群組，您可以將使用者指派給角色，並接收 `roles` 具有標準 Azure 帳戶之使用者的宣告。</span><span class="sxs-lookup"><span data-stu-id="d7274-143">Although you can't assign roles to security groups without an Azure AD Premium account, you can assign users to roles and receive a `roles` claim for users with a standard Azure account.</span></span> <span data-ttu-id="d7274-144">本節中的指引不需要 Azure AD Premium 帳戶。</span><span class="sxs-lookup"><span data-stu-id="d7274-144">The guidance in this section doesn't require an Azure AD Premium account.</span></span>
>
> <span data-ttu-id="d7274-145">在 Azure 入口網站中指派多個角色，方法是為每個額外的角色指派**_重新加入使用者_**。</span><span class="sxs-lookup"><span data-stu-id="d7274-145">Multiple roles are assigned in the Azure portal by **_re-adding a user_** for each additional role assignment.</span></span>

<span data-ttu-id="d7274-146">AAD 所傳送的單一宣告會將 `roles` 使用者定義的角色顯示為 `appRoles` `value` JSON 陣列中的 s。</span><span class="sxs-lookup"><span data-stu-id="d7274-146">The single `roles` claim sent by AAD presents the user-defined roles as the `appRoles`'s `value`s in a JSON array.</span></span> <span data-ttu-id="d7274-147">應用程式必須將角色的 JSON 陣列轉換成個別 `role` 宣告。</span><span class="sxs-lookup"><span data-stu-id="d7274-147">The app must convert the JSON array of roles into individual `role` claims.</span></span>

<span data-ttu-id="d7274-148">`CustomUserFactory`[[使用者定義群組] 和 [AAD 內建系統管理角色](#user-defined-groups-and-built-in-administrative-roles)] 區段中所顯示的，會設定為在 `roles` 具有 JSON 陣列值的宣告上採取動作。</span><span class="sxs-lookup"><span data-stu-id="d7274-148">The `CustomUserFactory` shown in the [User-defined groups and AAD built-in Administrative Roles](#user-defined-groups-and-built-in-administrative-roles) section is set up to act on a `roles` claim with a JSON array value.</span></span> <span data-ttu-id="d7274-149">`CustomUserFactory`在裝載解決方案的獨立應用程式或用戶端應用程式中，新增並註冊，如[使用者定義群組和 AAD 內建系統管理角色](#user-defined-groups-and-built-in-administrative-roles)一節中所示。</span><span class="sxs-lookup"><span data-stu-id="d7274-149">Add and register the `CustomUserFactory` in the standalone app or Client app of a Hosted solution as shown in the [User-defined groups and AAD built-in Administrative Roles](#user-defined-groups-and-built-in-administrative-roles) section.</span></span> <span data-ttu-id="d7274-150">不需要提供程式碼來移除原始宣告， `roles` 因為架構會自動移除該宣告。</span><span class="sxs-lookup"><span data-stu-id="d7274-150">There's no need to provide code to remove the original `roles` claim because it's automatically removed by the framework.</span></span>

<span data-ttu-id="d7274-151">在 `Program.Main` 託管解決方案的獨立應用程式或用戶端應用程式中，將名為 "" 的宣告指定 `role` 為角色宣告：</span><span class="sxs-lookup"><span data-stu-id="d7274-151">In `Program.Main` of the standalone app or Client app of a Hosted solution, specify the claim named "`role`" as the role claim:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...

    options.UserOptions.RoleClaim = "role";
});
```

<span data-ttu-id="d7274-152">此時，元件授權方法會正常運作。</span><span class="sxs-lookup"><span data-stu-id="d7274-152">Component authorization approaches are functional at this point.</span></span> <span data-ttu-id="d7274-153">元件中的任何授權機制都可以使用 `admin` 角色來授權使用者：</span><span class="sxs-lookup"><span data-stu-id="d7274-153">Any of the authorization mechanisms in components can use the `admin` role to authorize the user:</span></span>

* <span data-ttu-id="d7274-154">[ `AuthorizeView` 元件](xref:blazor/security/index#authorizeview-component)（範例： `<AuthorizeView Roles="admin">` ）</span><span class="sxs-lookup"><span data-stu-id="d7274-154">[`AuthorizeView` component](xref:blazor/security/index#authorizeview-component) (Example: `<AuthorizeView Roles="admin">`)</span></span>
* <span data-ttu-id="d7274-155">[ `[Authorize]` attribute](xref:blazor/security/index#authorize-attribute)指示詞（ <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ）（範例： `@attribute [Authorize(Roles = "admin")]` ）</span><span class="sxs-lookup"><span data-stu-id="d7274-155">[`[Authorize]` attribute directive](xref:blazor/security/index#authorize-attribute) (<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>) (Example: `@attribute [Authorize(Roles = "admin")]`)</span></span>
* <span data-ttu-id="d7274-156">程式[邏輯](xref:blazor/security/index#procedural-logic)（範例： `if (user.IsInRole("admin")) { ... }` ）</span><span class="sxs-lookup"><span data-stu-id="d7274-156">[Procedural logic](xref:blazor/security/index#procedural-logic) (Example: `if (user.IsInRole("admin")) { ... }`)</span></span>

  <span data-ttu-id="d7274-157">支援多個角色測試：</span><span class="sxs-lookup"><span data-stu-id="d7274-157">Multiple role tests are supported:</span></span>

  ```csharp
  if (user.IsInRole("admin") && user.IsInRole("developer"))
  {
      ...
  }
  ```

## <a name="aad-adminstrative-role-group-ids"></a><span data-ttu-id="d7274-158">AAD 系統管理角色群組識別碼</span><span class="sxs-lookup"><span data-stu-id="d7274-158">AAD Adminstrative Role Group IDs</span></span>

<span data-ttu-id="d7274-159">下表中顯示的物件識別碼是用來建立宣告[policies](xref:security/authorization/policies)的原則 `group` 。</span><span class="sxs-lookup"><span data-stu-id="d7274-159">The Object IDs presented in the following table are used to create [policies](xref:security/authorization/policies) for `group` claims.</span></span> <span data-ttu-id="d7274-160">原則允許應用程式授權使用者使用應用程式中的各種活動。</span><span class="sxs-lookup"><span data-stu-id="d7274-160">Policies permit an app to authorize users for various activities in an app.</span></span> <span data-ttu-id="d7274-161">如需詳細資訊，請參閱[使用者定義群組和 AAD 內建系統管理角色](#user-defined-groups-and-built-in-administrative-roles)一節。</span><span class="sxs-lookup"><span data-stu-id="d7274-161">For more information, see the [User-defined groups and AAD built-in Administrative Roles](#user-defined-groups-and-built-in-administrative-roles) section.</span></span>

<span data-ttu-id="d7274-162">AAD 系統管理角色</span><span class="sxs-lookup"><span data-stu-id="d7274-162">AAD Administrative Role</span></span> | <span data-ttu-id="d7274-163">物件識別碼</span><span class="sxs-lookup"><span data-stu-id="d7274-163">Object ID</span></span>
--- | ---
<span data-ttu-id="d7274-164">應用程式管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-164">Application administrator</span></span> | <span data-ttu-id="d7274-165">fa11557b-4f15-4ddd-85d5-313c7cd74047</span><span class="sxs-lookup"><span data-stu-id="d7274-165">fa11557b-4f15-4ddd-85d5-313c7cd74047</span></span>
<span data-ttu-id="d7274-166">應用程式開發人員</span><span class="sxs-lookup"><span data-stu-id="d7274-166">Application developer</span></span> | <span data-ttu-id="d7274-167">68adcbb8-9504-44f6-89f2-5cd48dc74a2c</span><span class="sxs-lookup"><span data-stu-id="d7274-167">68adcbb8-9504-44f6-89f2-5cd48dc74a2c</span></span>
<span data-ttu-id="d7274-168">驗證系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-168">Authentication administrator</span></span> | <span data-ttu-id="d7274-169">02d110a1-96b1-419e-af87-746461b60ed7</span><span class="sxs-lookup"><span data-stu-id="d7274-169">02d110a1-96b1-419e-af87-746461b60ed7</span></span>
<span data-ttu-id="d7274-170">Azure DevOps 管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-170">Azure DevOps administrator</span></span> | <span data-ttu-id="d7274-171">a5311ace-ca41-44cd-b833-8d22caa0b34f</span><span class="sxs-lookup"><span data-stu-id="d7274-171">a5311ace-ca41-44cd-b833-8d22caa0b34f</span></span>
<span data-ttu-id="d7274-172">Azure 資訊保護管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-172">Azure Information Protection administrator</span></span> | <span data-ttu-id="d7274-173">18632dce-f9b5-4f01-abb5-37051f06860e</span><span class="sxs-lookup"><span data-stu-id="d7274-173">18632dce-f9b5-4f01-abb5-37051f06860e</span></span>
<span data-ttu-id="d7274-174">B2C IEF 索引鍵集管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-174">B2C IEF Keyset administrator</span></span> | <span data-ttu-id="d7274-175">0c2e87e5-94f9-4adb-ae8c-bcafe11bd368</span><span class="sxs-lookup"><span data-stu-id="d7274-175">0c2e87e5-94f9-4adb-ae8c-bcafe11bd368</span></span>
<span data-ttu-id="d7274-176">B2C IEF 原則管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-176">B2C IEF Policy administrator</span></span> | <span data-ttu-id="d7274-177">bfcab36c-10c6-4b13-b63c-4d8b62c0c44e</span><span class="sxs-lookup"><span data-stu-id="d7274-177">bfcab36c-10c6-4b13-b63c-4d8b62c0c44e</span></span>
<span data-ttu-id="d7274-178">B2C 使用者流程管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-178">B2C user flow administrator</span></span> | <span data-ttu-id="d7274-179">baa531b7-8cf0-44ad-8f98-eded88dae827</span><span class="sxs-lookup"><span data-stu-id="d7274-179">baa531b7-8cf0-44ad-8f98-eded88dae827</span></span>
<span data-ttu-id="d7274-180">B2C 使用者流程屬性管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-180">B2C user flow attribute administrator</span></span> | <span data-ttu-id="d7274-181">dd0baca0-a535-48c1-b871-8431abe16452</span><span class="sxs-lookup"><span data-stu-id="d7274-181">dd0baca0-a535-48c1-b871-8431abe16452</span></span>
<span data-ttu-id="d7274-182">計費管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-182">Billing administrator</span></span> | <span data-ttu-id="d7274-183">69ff516a-b57d-4697-a429-9de4af7b5609</span><span class="sxs-lookup"><span data-stu-id="d7274-183">69ff516a-b57d-4697-a429-9de4af7b5609</span></span>
<span data-ttu-id="d7274-184">雲端應用程式系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-184">Cloud application administrator</span></span> | <span data-ttu-id="d7274-185">250b5fe3-b553-458d-9a53-b782c13c34bf</span><span class="sxs-lookup"><span data-stu-id="d7274-185">250b5fe3-b553-458d-9a53-b782c13c34bf</span></span>
<span data-ttu-id="d7274-186">雲端裝置管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-186">Cloud device administrator</span></span> | <span data-ttu-id="d7274-187">26cd4b44-2636-4ddb-bdfa-27feae66f86d</span><span class="sxs-lookup"><span data-stu-id="d7274-187">26cd4b44-2636-4ddb-bdfa-27feae66f86d</span></span>
<span data-ttu-id="d7274-188">規範管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-188">Compliance administrator</span></span> | <span data-ttu-id="d7274-189">9d6e1dd0-c9f8-45f8-b558-b134f700116c</span><span class="sxs-lookup"><span data-stu-id="d7274-189">9d6e1dd0-c9f8-45f8-b558-b134f700116c</span></span>
<span data-ttu-id="d7274-190">合規性資料管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-190">Compliance data administrator</span></span> | <span data-ttu-id="d7274-191">4c0ca3a2-231e-416c-9411-4abe57d5cb9d</span><span class="sxs-lookup"><span data-stu-id="d7274-191">4c0ca3a2-231e-416c-9411-4abe57d5cb9d</span></span>
<span data-ttu-id="d7274-192">條件式存取系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-192">Conditional Access administrator</span></span> | <span data-ttu-id="d7274-193">8f71a611-137d-49af-87ad-e97f1fd5da76</span><span class="sxs-lookup"><span data-stu-id="d7274-193">8f71a611-137d-49af-87ad-e97f1fd5da76</span></span>
<span data-ttu-id="d7274-194">客戶 LockBox 存取核准者</span><span class="sxs-lookup"><span data-stu-id="d7274-194">Customer LockBox access approver</span></span> | <span data-ttu-id="d7274-195">c18d54a8-b13e-4954-a1a4-7deaf2e4f184</span><span class="sxs-lookup"><span data-stu-id="d7274-195">c18d54a8-b13e-4954-a1a4-7deaf2e4f184</span></span>
<span data-ttu-id="d7274-196">電腦分析系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-196">Desktop Analytics administrator</span></span> | <span data-ttu-id="d7274-197">c62c4ac5-e4c6-4096-8a2f-1ee3cbaaae15</span><span class="sxs-lookup"><span data-stu-id="d7274-197">c62c4ac5-e4c6-4096-8a2f-1ee3cbaaae15</span></span>
<span data-ttu-id="d7274-198">目錄讀取器</span><span class="sxs-lookup"><span data-stu-id="d7274-198">Directory readers</span></span> | <span data-ttu-id="d7274-199">e1fc84a6-7762-4b9b-8e29-518b4adbc23b</span><span class="sxs-lookup"><span data-stu-id="d7274-199">e1fc84a6-7762-4b9b-8e29-518b4adbc23b</span></span>
<span data-ttu-id="d7274-200">Dynamics 365 管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-200">Dynamics 365 administrator</span></span> | <span data-ttu-id="d7274-201">f20a9cfa-9fdf-49a8-a977-1afe446a1d6e</span><span class="sxs-lookup"><span data-stu-id="d7274-201">f20a9cfa-9fdf-49a8-a977-1afe446a1d6e</span></span>
<span data-ttu-id="d7274-202">Exchange 系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-202">Exchange administrator</span></span> | <span data-ttu-id="d7274-203">b2ec2cc0-d5c9-4864-ad9b-38dd9dba2652</span><span class="sxs-lookup"><span data-stu-id="d7274-203">b2ec2cc0-d5c9-4864-ad9b-38dd9dba2652</span></span>
<span data-ttu-id="d7274-204">外部 Identity 提供者系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-204">External Identity Provider administrator</span></span> | <span data-ttu-id="d7274-205">febfaeb4-e478-407a-b4b3-f4d9716618a2</span><span class="sxs-lookup"><span data-stu-id="d7274-205">febfaeb4-e478-407a-b4b3-f4d9716618a2</span></span>
<span data-ttu-id="d7274-206">全域管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-206">Global administrator</span></span> | <span data-ttu-id="d7274-207">a45ba61b-44db-462c-924b-3b2719152588</span><span class="sxs-lookup"><span data-stu-id="d7274-207">a45ba61b-44db-462c-924b-3b2719152588</span></span>
<span data-ttu-id="d7274-208">全域讀取者</span><span class="sxs-lookup"><span data-stu-id="d7274-208">Global reader</span></span> | <span data-ttu-id="d7274-209">f6903b21-6aba-4124-b44c-76671796b9d5</span><span class="sxs-lookup"><span data-stu-id="d7274-209">f6903b21-6aba-4124-b44c-76671796b9d5</span></span>
<span data-ttu-id="d7274-210">群組管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-210">Groups administrator</span></span> | <span data-ttu-id="d7274-211">158b3e5a-d89d-460b-92b5-3b34985f0197</span><span class="sxs-lookup"><span data-stu-id="d7274-211">158b3e5a-d89d-460b-92b5-3b34985f0197</span></span>
<span data-ttu-id="d7274-212">來賓邀請者</span><span class="sxs-lookup"><span data-stu-id="d7274-212">Guest inviter</span></span> | <span data-ttu-id="d7274-213">4c730a1d-cc22-44af-8f9f-4eec635c7502</span><span class="sxs-lookup"><span data-stu-id="d7274-213">4c730a1d-cc22-44af-8f9f-4eec635c7502</span></span>
<span data-ttu-id="d7274-214">服務台管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-214">Helpdesk administrator</span></span> | <span data-ttu-id="d7274-215">108678c8-6628-44e1-8d01-caf598a6a5f5</span><span class="sxs-lookup"><span data-stu-id="d7274-215">108678c8-6628-44e1-8d01-caf598a6a5f5</span></span>
<span data-ttu-id="d7274-216">Intune 管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-216">Intune administrator</span></span> | <span data-ttu-id="d7274-217">79950741-23fa-4189-b2cb-46640601c497</span><span class="sxs-lookup"><span data-stu-id="d7274-217">79950741-23fa-4189-b2cb-46640601c497</span></span>
<span data-ttu-id="d7274-218">Kaizala 管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-218">Kaizala administrator</span></span> | <span data-ttu-id="d7274-219">d6322af2-48e7-42e0-8c68-0bbe31af3412</span><span class="sxs-lookup"><span data-stu-id="d7274-219">d6322af2-48e7-42e0-8c68-0bbe31af3412</span></span>
<span data-ttu-id="d7274-220">授權管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-220">License administrator</span></span> | <span data-ttu-id="d7274-221">3355458a-e423-44bf-8b98-4ac5e572cea5</span><span class="sxs-lookup"><span data-stu-id="d7274-221">3355458a-e423-44bf-8b98-4ac5e572cea5</span></span>
<span data-ttu-id="d7274-222">訊息中心隱私權讀取者</span><span class="sxs-lookup"><span data-stu-id="d7274-222">Message center privacy reader</span></span> | <span data-ttu-id="d7274-223">6395db95-9fb8-42b9-b1ed-30a2405eee6f</span><span class="sxs-lookup"><span data-stu-id="d7274-223">6395db95-9fb8-42b9-b1ed-30a2405eee6f</span></span>
<span data-ttu-id="d7274-224">訊息中心讀取者</span><span class="sxs-lookup"><span data-stu-id="d7274-224">Message center reader</span></span> | <span data-ttu-id="d7274-225">fd5d37b8-4e24-434b-9e63-70ed3b759a16</span><span class="sxs-lookup"><span data-stu-id="d7274-225">fd5d37b8-4e24-434b-9e63-70ed3b759a16</span></span>
<span data-ttu-id="d7274-226">Office 應用程式管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-226">Office apps administrator</span></span> | <span data-ttu-id="d7274-227">5f3870cd-b042-4f93-86d7-c9d77c664dc7</span><span class="sxs-lookup"><span data-stu-id="d7274-227">5f3870cd-b042-4f93-86d7-c9d77c664dc7</span></span>
<span data-ttu-id="d7274-228">密碼管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-228">Password administrator</span></span> | <span data-ttu-id="d7274-229">466e48b7-5d66-4ae5-8911-1a118de74941</span><span class="sxs-lookup"><span data-stu-id="d7274-229">466e48b7-5d66-4ae5-8911-1a118de74941</span></span>
<span data-ttu-id="d7274-230">Power BI 管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-230">Power BI administrator</span></span> | <span data-ttu-id="d7274-231">984e83b8-8337-4255-91a1-acb663175ab4</span><span class="sxs-lookup"><span data-stu-id="d7274-231">984e83b8-8337-4255-91a1-acb663175ab4</span></span>
<span data-ttu-id="d7274-232">Power Platform 管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-232">Power platform administrator</span></span> | <span data-ttu-id="d7274-233">76d6f95e-9a15-4d7d-8d21-00de00faf9fd</span><span class="sxs-lookup"><span data-stu-id="d7274-233">76d6f95e-9a15-4d7d-8d21-00de00faf9fd</span></span>
<span data-ttu-id="d7274-234">特殊權限驗證管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-234">Privileged authentication administrator</span></span> | <span data-ttu-id="d7274-235">0829f731-b46d-419f-9742-aeb122367d11</span><span class="sxs-lookup"><span data-stu-id="d7274-235">0829f731-b46d-419f-9742-aeb122367d11</span></span>
<span data-ttu-id="d7274-236">特殊權限角色管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-236">Privileged role administrator</span></span> | <span data-ttu-id="d7274-237">f20a725a-d1c8-4107-83ea-1171c97d00c7</span><span class="sxs-lookup"><span data-stu-id="d7274-237">f20a725a-d1c8-4107-83ea-1171c97d00c7</span></span>
<span data-ttu-id="d7274-238">報表讀者</span><span class="sxs-lookup"><span data-stu-id="d7274-238">Reports reader</span></span> | <span data-ttu-id="d7274-239">54635450-e8ed-4f2d-9632-07db2517b4de</span><span class="sxs-lookup"><span data-stu-id="d7274-239">54635450-e8ed-4f2d-9632-07db2517b4de</span></span>
<span data-ttu-id="d7274-240">搜尋管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-240">Search administrator</span></span> | <span data-ttu-id="d7274-241">c770a2f1-c9ba-4e60-9176-9f52b1eb1a31</span><span class="sxs-lookup"><span data-stu-id="d7274-241">c770a2f1-c9ba-4e60-9176-9f52b1eb1a31</span></span>
<span data-ttu-id="d7274-242">搜尋編輯者</span><span class="sxs-lookup"><span data-stu-id="d7274-242">Search editor</span></span> | <span data-ttu-id="d7274-243">6a6858c6-5f0d-44ac-87c7-0190fbedd271</span><span class="sxs-lookup"><span data-stu-id="d7274-243">6a6858c6-5f0d-44ac-87c7-0190fbedd271</span></span>
<span data-ttu-id="d7274-244">安全性系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-244">Security administrator</span></span> | <span data-ttu-id="d7274-245">20fa50e3-6531-44d8-bd39-b251420568ad</span><span class="sxs-lookup"><span data-stu-id="d7274-245">20fa50e3-6531-44d8-bd39-b251420568ad</span></span>
<span data-ttu-id="d7274-246">安全性操作員</span><span class="sxs-lookup"><span data-stu-id="d7274-246">Security operator</span></span> | <span data-ttu-id="d7274-247">43aae017-8e51-4188-91ab-e6debd572800</span><span class="sxs-lookup"><span data-stu-id="d7274-247">43aae017-8e51-4188-91ab-e6debd572800</span></span>
<span data-ttu-id="d7274-248">安全性讀取者</span><span class="sxs-lookup"><span data-stu-id="d7274-248">Security reader</span></span> | <span data-ttu-id="d7274-249">45035cd3-fd97-4250-8197-3a53d3562d9b</span><span class="sxs-lookup"><span data-stu-id="d7274-249">45035cd3-fd97-4250-8197-3a53d3562d9b</span></span>
<span data-ttu-id="d7274-250">服務支援管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-250">Service support administrator</span></span> | <span data-ttu-id="d7274-251">2c92cf45-c914-48f8-9bf9-fc14b28818ab</span><span class="sxs-lookup"><span data-stu-id="d7274-251">2c92cf45-c914-48f8-9bf9-fc14b28818ab</span></span>
<span data-ttu-id="d7274-252">SharePoint 管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-252">SharePoint administrator</span></span> | <span data-ttu-id="d7274-253">e1c32229-875e-461d-ae24-3cb99116e86c</span><span class="sxs-lookup"><span data-stu-id="d7274-253">e1c32229-875e-461d-ae24-3cb99116e86c</span></span>
<span data-ttu-id="d7274-254">商務用 Skype 的管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-254">Skype for Business administrator</span></span> | <span data-ttu-id="d7274-255">0a8cee12-e21d-43ef-abd9-f1ea85710e30</span><span class="sxs-lookup"><span data-stu-id="d7274-255">0a8cee12-e21d-43ef-abd9-f1ea85710e30</span></span>
<span data-ttu-id="d7274-256">Microsoft Teams 通訊系統管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-256">Teams Communications Administrator</span></span> | <span data-ttu-id="d7274-257">2393e455-6e13-4743-9f52-63fcec2b6a9c</span><span class="sxs-lookup"><span data-stu-id="d7274-257">2393e455-6e13-4743-9f52-63fcec2b6a9c</span></span>
<span data-ttu-id="d7274-258">Microsoft Teams 通訊支援工程師</span><span class="sxs-lookup"><span data-stu-id="d7274-258">Teams Communications Support Engineer</span></span> | <span data-ttu-id="d7274-259">802dd94e-d717-46f6-af98-b9167071e9fc</span><span class="sxs-lookup"><span data-stu-id="d7274-259">802dd94e-d717-46f6-af98-b9167071e9fc</span></span>
<span data-ttu-id="d7274-260">小組溝通專家</span><span class="sxs-lookup"><span data-stu-id="d7274-260">Teams Communications Specialist</span></span> | <span data-ttu-id="d7274-261">ef547281-cf46-4cc6-bcaa-f5eac3f030c9</span><span class="sxs-lookup"><span data-stu-id="d7274-261">ef547281-cf46-4cc6-bcaa-f5eac3f030c9</span></span>
<span data-ttu-id="d7274-262">Microsoft Teams 服務管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-262">Teams Service Administrator</span></span> | <span data-ttu-id="d7274-263">8846a0be-197b-443a-b13c-11192691fa24</span><span class="sxs-lookup"><span data-stu-id="d7274-263">8846a0be-197b-443a-b13c-11192691fa24</span></span>
<span data-ttu-id="d7274-264">使用者管理員</span><span class="sxs-lookup"><span data-stu-id="d7274-264">User administrator</span></span> | <span data-ttu-id="d7274-265">1f6eed58-7dd3-460b-a298-666f975427a1</span><span class="sxs-lookup"><span data-stu-id="d7274-265">1f6eed58-7dd3-460b-a298-666f975427a1</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d7274-266">其他資源</span><span class="sxs-lookup"><span data-stu-id="d7274-266">Additional resources</span></span>

* <xref:security/authorization/claims>
* <xref:blazor/security/index>
