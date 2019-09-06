---
title: ASP.NET Core Blazor 驗證與授權
author: guardrex
description: 了解 Blazor 驗證與授權案例。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2019
uid: security/blazor/index
ms.openlocfilehash: 8714acbeb6e8a00992a601030811b24f53426b82
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310517"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="80f56-103">ASP.NET Core Blazor 驗證與授權</span><span class="sxs-lookup"><span data-stu-id="80f56-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="80f56-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="80f56-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="80f56-105">ASP.NET Core 支援在 Blazor 應用程式中設定及管理安全性。</span><span class="sxs-lookup"><span data-stu-id="80f56-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="80f56-106">Blazor 伺服器端和用戶端應用程式之間的安全性案例會有所差異。</span><span class="sxs-lookup"><span data-stu-id="80f56-106">Security scenarios differ between Blazor server-side and client-side apps.</span></span> <span data-ttu-id="80f56-107">由於 Blazor 伺服器端應用程式是在伺服器上執行，授權檢查將能決定：</span><span class="sxs-lookup"><span data-stu-id="80f56-107">Because Blazor server-side apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="80f56-108">要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。</span><span class="sxs-lookup"><span data-stu-id="80f56-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="80f56-109">適用於應用程式和元件之特定區域的存取規則。</span><span class="sxs-lookup"><span data-stu-id="80f56-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="80f56-110">Blazor 用戶端應用程式是在用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="80f56-110">Blazor client-side apps run on the client.</span></span> <span data-ttu-id="80f56-111">授權「僅」會被用來決定要顯示的 UI 選項。</span><span class="sxs-lookup"><span data-stu-id="80f56-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="80f56-112">由於用戶端檢查可以被使用者修改或略過，Blazor 用戶端應用程式並無法強制執行授權存取規則。</span><span class="sxs-lookup"><span data-stu-id="80f56-112">Since client-side checks can be modified or bypassed by a user, a Blazor client-side app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="80f56-113">驗證</span><span class="sxs-lookup"><span data-stu-id="80f56-113">Authentication</span></span>

<span data-ttu-id="80f56-114">Blazor 會使用現有的 ASP.NET Core 驗證機制來建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="80f56-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="80f56-115">確切的機制會取決於 Blazor 應用程式的裝載方式 (伺服器端或用戶端)。</span><span class="sxs-lookup"><span data-stu-id="80f56-115">The exact mechanism depends on how the Blazor app is hosted, server-side or client-side.</span></span>

### <a name="blazor-server-side-authentication"></a><span data-ttu-id="80f56-116">Blazor 伺服器端驗證</span><span class="sxs-lookup"><span data-stu-id="80f56-116">Blazor server-side authentication</span></span>

<span data-ttu-id="80f56-117">Blazor 伺服器端應用程式會在使用 SignalR 建立的即時連線上運作。</span><span class="sxs-lookup"><span data-stu-id="80f56-117">Blazor server-side apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="80f56-118">[SignalR 型應用程式中的驗證](xref:signalr/authn-and-authz)會在連線建立時處理。</span><span class="sxs-lookup"><span data-stu-id="80f56-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="80f56-119">驗證可以是以 Cookie 或其他持有人權杖為基礎。</span><span class="sxs-lookup"><span data-stu-id="80f56-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="80f56-120">Blazor 伺服器端專案範本可以在專案建立時為您設定驗證。</span><span class="sxs-lookup"><span data-stu-id="80f56-120">The Blazor server-side project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80f56-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80f56-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="80f56-122">請遵循 <xref:blazor/get-started> 一文中的 Visual Studio 指導方針，來建立具有驗證機制的新 Blazor 伺服器端專案。</span><span class="sxs-lookup"><span data-stu-id="80f56-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism.</span></span>

<span data-ttu-id="80f56-123">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中選擇 [Blazor 伺服器應用程式] 範本之後，請選取 [驗證] 下的 [變更]。</span><span class="sxs-lookup"><span data-stu-id="80f56-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="80f56-124">對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：</span><span class="sxs-lookup"><span data-stu-id="80f56-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="80f56-125">**無驗證**</span><span class="sxs-lookup"><span data-stu-id="80f56-125">**No Authentication**</span></span>
* <span data-ttu-id="80f56-126">**個別使用者帳戶** &ndash; 使用者帳戶能以下列方式儲存：</span><span class="sxs-lookup"><span data-stu-id="80f56-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="80f56-127">使用 ASP.NET Core 的[身分識別](xref:security/authentication/identity)系統儲存在應用程式內。</span><span class="sxs-lookup"><span data-stu-id="80f56-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="80f56-128">使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。</span><span class="sxs-lookup"><span data-stu-id="80f56-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="80f56-129">**公司或學校帳戶**</span><span class="sxs-lookup"><span data-stu-id="80f56-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="80f56-130">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="80f56-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="80f56-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80f56-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="80f56-132">請遵循 <xref:blazor/get-started> 一文中的 Visual Studio Code 指導方針，來建立具有驗證機制的新 Blazor 伺服器端專案：</span><span class="sxs-lookup"><span data-stu-id="80f56-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:</span></span>

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="80f56-133">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="80f56-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="80f56-134">驗證機制</span><span class="sxs-lookup"><span data-stu-id="80f56-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="80f56-135">`{AUTHENTICATION}` 值</span><span class="sxs-lookup"><span data-stu-id="80f56-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="80f56-136">無驗證</span><span class="sxs-lookup"><span data-stu-id="80f56-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="80f56-137">個人</span><span class="sxs-lookup"><span data-stu-id="80f56-137">Individual</span></span><br><span data-ttu-id="80f56-138">搭配 ASP.NET Core 身分識別儲存在應用程式中的使用者。</span><span class="sxs-lookup"><span data-stu-id="80f56-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="80f56-139">個人</span><span class="sxs-lookup"><span data-stu-id="80f56-139">Individual</span></span><br><span data-ttu-id="80f56-140">儲存在 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 中的使用者。</span><span class="sxs-lookup"><span data-stu-id="80f56-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="80f56-141">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="80f56-141">Work or School Accounts</span></span><br><span data-ttu-id="80f56-142">適用於單一租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="80f56-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="80f56-143">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="80f56-143">Work or School Accounts</span></span><br><span data-ttu-id="80f56-144">適用於多個租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="80f56-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="80f56-145">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="80f56-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="80f56-146">命令會建立搭配為 `{APP NAME}` 所提供的值來命名的資料夾，並會使用該資料夾名稱作為應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="80f56-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="80f56-147">如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。</span><span class="sxs-lookup"><span data-stu-id="80f56-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:

```console
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

### <a name="blazor-client-side-authentication"></a><span data-ttu-id="80f56-148">Blazor 用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="80f56-148">Blazor client-side authentication</span></span>

<span data-ttu-id="80f56-149">在 Blazor 用戶端應用程式中，驗證檢查是可以被略過的，因為使用者可以修改所有用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="80f56-149">In Blazor client-side apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="80f56-150">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="80f56-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="80f56-151">針對 Blazor 用戶端應用程式實作自訂 `AuthenticationStateProvider` 服務的內容已涵蓋於以下各節中。</span><span class="sxs-lookup"><span data-stu-id="80f56-151">Implementation of a custom `AuthenticationStateProvider` service for Blazor client-side apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="80f56-152">AuthenticationStateProvider 服務</span><span class="sxs-lookup"><span data-stu-id="80f56-152">AuthenticationStateProvider service</span></span>

<span data-ttu-id="80f56-153">Blazor 伺服器端應用程式包含內建的 `AuthenticationStateProvider` 服務，該服務能從 ASP.NET Core 的 `HttpContext.User` 取得驗證狀態資料。</span><span class="sxs-lookup"><span data-stu-id="80f56-153">Blazor server-side apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="80f56-154">這就是驗證狀態與現有 ASP.NET Core 伺服器端驗證機制之間的整合方式。</span><span class="sxs-lookup"><span data-stu-id="80f56-154">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="80f56-155">`AuthenticationStateProvider` 是 `AuthorizeView` 元件與 `CascadingAuthenticationState` 元件用來取得驗證狀態的基礎服務。</span><span class="sxs-lookup"><span data-stu-id="80f56-155">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="80f56-156">您通常不會直接使用 `AuthenticationStateProvider`。</span><span class="sxs-lookup"><span data-stu-id="80f56-156">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="80f56-157">請使用此文章稍後所述的 [AuthorizeView 元件](#authorizeview-component)或 [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) 方法。</span><span class="sxs-lookup"><span data-stu-id="80f56-157">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="80f56-158">使用 `AuthenticationStateProvider` 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。</span><span class="sxs-lookup"><span data-stu-id="80f56-158">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="80f56-159">`AuthenticationStateProvider` 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="80f56-159">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
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

<span data-ttu-id="80f56-160">如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="80f56-160">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="80f56-161">如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="80f56-161">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="80f56-162">實作自訂 AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="80f56-162">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="80f56-163">如果您是在建置 Blazor 用戶端應用程式，或是您應用程式的規格絕對需要自訂提供者，請實作提供者並覆寫 `GetAuthenticationStateAsync`：</span><span class="sxs-lookup"><span data-stu-id="80f56-163">If you're building a Blazor client-side app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
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

<span data-ttu-id="80f56-164">`CustomAuthStateProvider` 服務會在 `Startup.ConfigureServices` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="80f56-164">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

<span data-ttu-id="80f56-165">使用 `CustomAuthStateProvider` 時，所有使用者都會以 `mrfibuli` 的使用者名稱進行驗證。</span><span class="sxs-lookup"><span data-stu-id="80f56-165">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="80f56-166">將驗證狀態公開為階層式參數</span><span class="sxs-lookup"><span data-stu-id="80f56-166">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="80f56-167">如果驗證狀態資料需要程序性邏輯 (例如在執行由使用者觸發的動作時)，請透過定義 `Task<AuthenticationState>` 類型的階層式參數來取得驗證狀態資料：</span><span class="sxs-lookup"><span data-stu-id="80f56-167">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```cshtml
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

<span data-ttu-id="80f56-168">如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="80f56-168">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="80f56-169">`Task<AuthenticationState>` 使用`AuthorizeRouteView`和元件設定串聯參數： `CascadingAuthenticationState`</span><span class="sxs-lookup"><span data-stu-id="80f56-169">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

```cshtml
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

## <a name="authorization"></a><span data-ttu-id="80f56-170">Authorization</span><span class="sxs-lookup"><span data-stu-id="80f56-170">Authorization</span></span>

<span data-ttu-id="80f56-171">在使用者被驗證之後，系統便會套用「授權」規則以控制使用者可以執行的動作。</span><span class="sxs-lookup"><span data-stu-id="80f56-171">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="80f56-172">存取權通常會依據下列情況來授與或拒絕：</span><span class="sxs-lookup"><span data-stu-id="80f56-172">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="80f56-173">使用者已通過驗證 (已登入)。</span><span class="sxs-lookup"><span data-stu-id="80f56-173">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="80f56-174">使用者屬於某個「角色」。</span><span class="sxs-lookup"><span data-stu-id="80f56-174">A user is in a *role*.</span></span>
* <span data-ttu-id="80f56-175">使用者具有「宣告」。</span><span class="sxs-lookup"><span data-stu-id="80f56-175">A user has a *claim*.</span></span>
* <span data-ttu-id="80f56-176">已滿足某個「原則」。</span><span class="sxs-lookup"><span data-stu-id="80f56-176">A *policy* is satisfied.</span></span>

<span data-ttu-id="80f56-177">上述概念皆和 ASP.NET Core MVC 或 Razor Pages 應用程式中的概念相同。</span><span class="sxs-lookup"><span data-stu-id="80f56-177">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="80f56-178">如需 ASP.NET Core 安全性的詳細資訊，請參閱 [ASP.NET Core 安全性與身分識別](xref:security/index)下的文章。</span><span class="sxs-lookup"><span data-stu-id="80f56-178">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="80f56-179">AuthorizeView 元件</span><span class="sxs-lookup"><span data-stu-id="80f56-179">AuthorizeView component</span></span>

<span data-ttu-id="80f56-180">`AuthorizeView` 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。</span><span class="sxs-lookup"><span data-stu-id="80f56-180">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="80f56-181">此方法可讓您只需要向使用者「顯示」資料，而不需要在程序性邏輯中使用該使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="80f56-181">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="80f56-182">該元件會公開 `AuthenticationState` 類型的 `context` 變數，您可以使用它來存取已登入使用者的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="80f56-182">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="80f56-183">您也可以在使用者未經授權的情況下，提供不同的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="80f56-183">You can also supply different content for display if the user isn't authenticated:</span></span>

```cshtml
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

<span data-ttu-id="80f56-184">`<Authorized>` 和 `<NotAuthorized>` 的內容可以包含任意項目，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="80f56-184">The content of `<Authorized>` and `<NotAuthorized>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="80f56-185">授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。</span><span class="sxs-lookup"><span data-stu-id="80f56-185">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="80f56-186">如果未指定授權條件，`AuthorizeView` 會使用預設的原則，並將：</span><span class="sxs-lookup"><span data-stu-id="80f56-186">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="80f56-187">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="80f56-187">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="80f56-188">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="80f56-188">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="80f56-189">角色型和原則型授權</span><span class="sxs-lookup"><span data-stu-id="80f56-189">Role-based and policy-based authorization</span></span>

<span data-ttu-id="80f56-190">`AuthorizeView` 元件支援「角色型」或「原則型」授權。</span><span class="sxs-lookup"><span data-stu-id="80f56-190">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="80f56-191">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="80f56-191">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="80f56-192">如需詳細資訊，請參閱 <xref:security/authorization/roles>。</span><span class="sxs-lookup"><span data-stu-id="80f56-192">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="80f56-193">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="80f56-193">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="80f56-194">宣告型授權是特殊案例的原則型授權。</span><span class="sxs-lookup"><span data-stu-id="80f56-194">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="80f56-195">例如，您可以定義要求使用者具備特定宣告的原則。</span><span class="sxs-lookup"><span data-stu-id="80f56-195">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="80f56-196">如需詳細資訊，請參閱 <xref:security/authorization/policies>。</span><span class="sxs-lookup"><span data-stu-id="80f56-196">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="80f56-197">這些 API 可以用於 Blazor 伺服器端應用程式或 Blazor 用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="80f56-197">These APIs can be used in either Blazor server-side or Blazor client-side apps.</span></span>

<span data-ttu-id="80f56-198">如果未指定 `Roles` 和 `Policy`，`AuthorizeView` 便會使用預設原則。</span><span class="sxs-lookup"><span data-stu-id="80f56-198">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="80f56-199">在非同步驗證期間所顯示的內容</span><span class="sxs-lookup"><span data-stu-id="80f56-199">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="80f56-200">Blazor 允許以「非同步」方式判斷驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="80f56-200">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="80f56-201">此方法的主要案例是會向外部端點要求驗證的 Blazor 用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="80f56-201">The primary scenario for this approach is in Blazor client-side apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="80f56-202">在驗證期間，`AuthorizeView` 預設不會顯示任何內容。</span><span class="sxs-lookup"><span data-stu-id="80f56-202">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="80f56-203">若要在驗證發生時顯示內容，請使用 `<Authorizing>` 元素：</span><span class="sxs-lookup"><span data-stu-id="80f56-203">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```cshtml
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

<span data-ttu-id="80f56-204">此方法通常不適用於 Blazor 伺服器端應用程式。</span><span class="sxs-lookup"><span data-stu-id="80f56-204">This approach isn't normally applicable to Blazor server-side apps.</span></span> <span data-ttu-id="80f56-205">Blazor 伺服器端應用程式在驗證狀態建立時，便能立即得知該狀態。</span><span class="sxs-lookup"><span data-stu-id="80f56-205">Blazor server-side apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="80f56-206">可以在 Blazor 伺服器端應用程式的 `AuthorizeView` 元件中提供 `Authorizing` 內容，但該內容永遠不會顯示。</span><span class="sxs-lookup"><span data-stu-id="80f56-206">`Authorizing` content can be provided in a Blazor server-side app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="80f56-207">[Authorize] 屬性</span><span class="sxs-lookup"><span data-stu-id="80f56-207">[Authorize] attribute</span></span>

<span data-ttu-id="80f56-208">就像應用程式可以搭配 MVC 控制器或 Razor 頁面使用 `[Authorize]` 一般，`[Authorize]` 也可以搭配 Razor 元件使用：</span><span class="sxs-lookup"><span data-stu-id="80f56-208">Just like an app can use `[Authorize]` with an MVC controller or Razor page, `[Authorize]` can also be used with Razor Components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="80f56-209">請僅在透過 Blazor 路由器抵達的 `@page` 元件上使用 `[Authorize]`。</span><span class="sxs-lookup"><span data-stu-id="80f56-209">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="80f56-210">授權僅會以路由的層面執行，且「不」適用於在頁面內轉譯的子元件。</span><span class="sxs-lookup"><span data-stu-id="80f56-210">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="80f56-211">若要授權在頁面內顯示特定組件，請改為使用 `AuthorizeView`。</span><span class="sxs-lookup"><span data-stu-id="80f56-211">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="80f56-212">您可能需要將 `@using Microsoft.AspNetCore.Authorization` 加入元件或 *_Imports.razor* 檔案，使該元件能夠編譯。</span><span class="sxs-lookup"><span data-stu-id="80f56-212">You may need to add `@using Microsoft.AspNetCore.Authorization` either to the component or to the *_Imports.razor* file in order for the component to compile.</span></span>

<span data-ttu-id="80f56-213">`[Authorize]` 屬性也支援角色型或原則型授權。</span><span class="sxs-lookup"><span data-stu-id="80f56-213">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="80f56-214">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="80f56-214">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="80f56-215">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="80f56-215">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="80f56-216">如果未指定 `Roles` 與 `Policy`，`[Authorize]` 便會使用預設原則，它預設會將：</span><span class="sxs-lookup"><span data-stu-id="80f56-216">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="80f56-217">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="80f56-217">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="80f56-218">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="80f56-218">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="80f56-219">搭配 Router 元件自訂未經授權的內容</span><span class="sxs-lookup"><span data-stu-id="80f56-219">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="80f56-220">`Router` 元件`AuthorizeRouteView`與元件結合，可讓應用程式在下列情況指定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="80f56-220">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="80f56-221">找不到內容。</span><span class="sxs-lookup"><span data-stu-id="80f56-221">Content isn't found.</span></span>
* <span data-ttu-id="80f56-222">使用者無法滿足套用至元件的 `[Authorize]` 條件。</span><span class="sxs-lookup"><span data-stu-id="80f56-222">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="80f56-223">`[Authorize]` 屬性已涵蓋於 [[Authorize] 屬性](#authorize-attribute)一節。</span><span class="sxs-lookup"><span data-stu-id="80f56-223">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="80f56-224">正在進行非同步驗證。</span><span class="sxs-lookup"><span data-stu-id="80f56-224">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="80f56-225">在預設的 Blazor 伺服器端專案範本中，*App.razor* 檔案會示範如何設定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="80f56-225">In the default Blazor server-side project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
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

<span data-ttu-id="80f56-226">`<NotFound>`、`<NotAuthorized>` 及 `<Authorizing>` 的內容可以包含任意項目，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="80f56-226">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="80f56-227">如果`<NotAuthorized>`未指定，則`<AuthorizeRouteView>`會使用下列回溯訊息：</span><span class="sxs-lookup"><span data-stu-id="80f56-227">If `<NotAuthorized>` isn't specified, the `<AuthorizeRouteView>` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="80f56-228">關於驗證狀態變更的通知</span><span class="sxs-lookup"><span data-stu-id="80f56-228">Notification about authentication state changes</span></span>

<span data-ttu-id="80f56-229">如果應用程式判斷基礎驗證狀態資料已經變更 (例如由於使用者已登出，或是另一個使用者已變更其角色)，自訂的 `AuthenticationStateProvider` 可以選擇性地在 `AuthenticationStateProvider` 基底類別上叫用 `NotifyAuthenticationStateChanged` 方法。</span><span class="sxs-lookup"><span data-stu-id="80f56-229">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="80f56-230">這會通知驗證狀態資料的取用者 (例如 `AuthorizeView`) 使用新資料來重新轉譯。</span><span class="sxs-lookup"><span data-stu-id="80f56-230">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="80f56-231">程序性邏輯</span><span class="sxs-lookup"><span data-stu-id="80f56-231">Procedural logic</span></span>

<span data-ttu-id="80f56-232">如果作為程序性邏輯的一部分，應用程式必須檢查授權規則，請使用 `Task<AuthenticationState>` 類型的階層式參數來取得使用者的 <xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="80f56-232">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="80f56-233">`Task<AuthenticationState>` 可以與其他服務 (例如 `IAuthorizationService`) 結合來評估原則。</span><span class="sxs-lookup"><span data-stu-id="80f56-233">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```cshtml
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

## <a name="authorization-in-blazor-client-side-apps"></a><span data-ttu-id="80f56-234">Blazor 用戶端應用程式中的授權</span><span class="sxs-lookup"><span data-stu-id="80f56-234">Authorization in Blazor client-side apps</span></span>

<span data-ttu-id="80f56-235">在 Blazor 用戶端應用程式中，授權檢查是可以被略過的，因為使用者可以修改所有的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="80f56-235">In Blazor client-side apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="80f56-236">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="80f56-236">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="80f56-237">**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**</span><span class="sxs-lookup"><span data-stu-id="80f56-237">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="80f56-238">疑難排解錯誤</span><span class="sxs-lookup"><span data-stu-id="80f56-238">Troubleshoot errors</span></span>

<span data-ttu-id="80f56-239">常見錯誤：</span><span class="sxs-lookup"><span data-stu-id="80f56-239">Common errors:</span></span>

* <span data-ttu-id="80f56-240">**授權需要 Task\<AuthenticationState> 類型的階層式參數。請考慮使用 CascadingAuthenticationState 來提供此項目。**</span><span class="sxs-lookup"><span data-stu-id="80f56-240">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="80f56-241">**針對 `authenticationStateTask` 接收到 `null` 值**</span><span class="sxs-lookup"><span data-stu-id="80f56-241">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="80f56-242">專案很可能不是使用已啟用驗證的 Blazor 伺服器端範本建立。</span><span class="sxs-lookup"><span data-stu-id="80f56-242">It's likely that the project wasn't created using a Blazor server-side template with authentication enabled.</span></span> <span data-ttu-id="80f56-243">請將 `<CascadingAuthenticationState>` 包裝在 UI 樹狀的一部分，例如在 *App.razor* 中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="80f56-243">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="80f56-244">`CascadingAuthenticationState` 會提供 `Task<AuthenticationState>` 階層式參數，它接著會接收自底層 `AuthenticationStateProvider` DI 服務。</span><span class="sxs-lookup"><span data-stu-id="80f56-244">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80f56-245">其他資源</span><span class="sxs-lookup"><span data-stu-id="80f56-245">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server-side>
* <xref:security/authentication/windowsauth>
