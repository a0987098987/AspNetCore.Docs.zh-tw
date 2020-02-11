---
title: ASP.NET Core Blazor 驗證和授權
author: guardrex
description: 深入瞭解 Blazor 驗證和授權案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: c7b3788b5737073100e7fa449fd6bb4a83c0043a
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114883"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="67c8c-103">ASP.NET Core Blazor 驗證與授權</span><span class="sxs-lookup"><span data-stu-id="67c8c-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="67c8c-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="67c8c-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="67c8c-105">ASP.NET Core 支援在 Blazor 應用程式中設定及管理安全性。</span><span class="sxs-lookup"><span data-stu-id="67c8c-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="67c8c-106">Blazor 伺服器與 Blazor WebAssembly 應用程式之間的安全性案例不同。</span><span class="sxs-lookup"><span data-stu-id="67c8c-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="67c8c-107">由於 Blazor 伺服器應用程式是在伺服器上執行，因此授權檢查能夠判斷：</span><span class="sxs-lookup"><span data-stu-id="67c8c-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="67c8c-108">要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。</span><span class="sxs-lookup"><span data-stu-id="67c8c-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="67c8c-109">適用於應用程式和元件之特定區域的存取規則。</span><span class="sxs-lookup"><span data-stu-id="67c8c-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="67c8c-110">Blazor WebAssembly 應用程式會在用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="67c8c-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="67c8c-111">授權「僅」會被用來決定要顯示的 UI 選項。</span><span class="sxs-lookup"><span data-stu-id="67c8c-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="67c8c-112">由於用戶端檢查可由使用者修改或略過，因此 Blazor WebAssembly 應用程式無法強制執行授權存取規則。</span><span class="sxs-lookup"><span data-stu-id="67c8c-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="67c8c-113">驗證</span><span class="sxs-lookup"><span data-stu-id="67c8c-113">Authentication</span></span>

<span data-ttu-id="67c8c-114">Blazor 會使用現有的 ASP.NET Core 驗證機制來建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="67c8c-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="67c8c-115">確切的機制取決於 Blazor 應用程式的主控方式、Blazor 伺服器或 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="67c8c-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="67c8c-116">Blazor 伺服器驗證</span><span class="sxs-lookup"><span data-stu-id="67c8c-116">Blazor Server authentication</span></span>

<span data-ttu-id="67c8c-117">Blazor 伺服器應用程式會在使用 SignalR 建立的即時連線上運作。</span><span class="sxs-lookup"><span data-stu-id="67c8c-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="67c8c-118">[SignalR 型應用程式中的驗證](xref:signalr/authn-and-authz)會在連線建立時處理。</span><span class="sxs-lookup"><span data-stu-id="67c8c-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="67c8c-119">驗證可以是以 Cookie 或其他持有人權杖為基礎。</span><span class="sxs-lookup"><span data-stu-id="67c8c-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="67c8c-120">當您建立專案時，Blazor 伺服器專案範本可以為您設定驗證。</span><span class="sxs-lookup"><span data-stu-id="67c8c-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="67c8c-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="67c8c-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="67c8c-122">遵循 <xref:blazor/get-started> 文章中的 Visual Studio 指導方針，使用驗證機制建立新的 Blazor 伺服器專案。</span><span class="sxs-lookup"><span data-stu-id="67c8c-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="67c8c-123">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中選擇 [Blazor 伺服器應用程式] 範本之後，請選取 [驗證] 下的 [變更]。</span><span class="sxs-lookup"><span data-stu-id="67c8c-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="67c8c-124">對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：</span><span class="sxs-lookup"><span data-stu-id="67c8c-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="67c8c-125">**無驗證**</span><span class="sxs-lookup"><span data-stu-id="67c8c-125">**No Authentication**</span></span>
* <span data-ttu-id="67c8c-126">可以儲存 &ndash; 使用者帳戶的**個別使用者帳戶**：</span><span class="sxs-lookup"><span data-stu-id="67c8c-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="67c8c-127">使用 ASP.NET Core 的[身分識別](xref:security/authentication/identity)系統儲存在應用程式內。</span><span class="sxs-lookup"><span data-stu-id="67c8c-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="67c8c-128">使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。</span><span class="sxs-lookup"><span data-stu-id="67c8c-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="67c8c-129">**公司或學校帳戶**</span><span class="sxs-lookup"><span data-stu-id="67c8c-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="67c8c-130">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="67c8c-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="67c8c-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="67c8c-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="67c8c-132">遵循 <xref:blazor/get-started> 文章中的 Visual Studio Code 指導方針，使用驗證機制建立新的 Blazor 伺服器專案：</span><span class="sxs-lookup"><span data-stu-id="67c8c-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="67c8c-133">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="67c8c-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="67c8c-134">驗證機制</span><span class="sxs-lookup"><span data-stu-id="67c8c-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="67c8c-135">`{AUTHENTICATION}` 值</span><span class="sxs-lookup"><span data-stu-id="67c8c-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="67c8c-136">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="67c8c-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="67c8c-137">個人</span><span class="sxs-lookup"><span data-stu-id="67c8c-137">Individual</span></span><br><span data-ttu-id="67c8c-138">搭配 ASP.NET Core 身分識別儲存在應用程式中的使用者。</span><span class="sxs-lookup"><span data-stu-id="67c8c-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="67c8c-139">個人</span><span class="sxs-lookup"><span data-stu-id="67c8c-139">Individual</span></span><br><span data-ttu-id="67c8c-140">儲存在 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 中的使用者。</span><span class="sxs-lookup"><span data-stu-id="67c8c-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="67c8c-141">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="67c8c-141">Work or School Accounts</span></span><br><span data-ttu-id="67c8c-142">適用於單一租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="67c8c-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="67c8c-143">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="67c8c-143">Work or School Accounts</span></span><br><span data-ttu-id="67c8c-144">適用於多個租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="67c8c-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="67c8c-145">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="67c8c-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="67c8c-146">命令會建立搭配為 `{APP NAME}` 所提供的值來命名的資料夾，並會使用該資料夾名稱作為應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="67c8c-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="67c8c-147">如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。</span><span class="sxs-lookup"><span data-stu-id="67c8c-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor<span data-ttu-id="67c8c-148"> WebAssembly authentication</span><span class="sxs-lookup"><span data-stu-id="67c8c-148"> WebAssembly authentication</span></span>

<span data-ttu-id="67c8c-149">在 Blazor WebAssembly apps 中，可以略過驗證檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="67c8c-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="67c8c-150">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="67c8c-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="67c8c-151">將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/)的套件參考新增至應用程式的專案檔。</span><span class="sxs-lookup"><span data-stu-id="67c8c-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="67c8c-152">下列各節涵蓋 Blazor WebAssembly 應用程式的自訂 `AuthenticationStateProvider` 服務的執行。</span><span class="sxs-lookup"><span data-stu-id="67c8c-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="67c8c-153">AuthenticationStateProvider 服務</span><span class="sxs-lookup"><span data-stu-id="67c8c-153">AuthenticationStateProvider service</span></span>

Blazor<span data-ttu-id="67c8c-154"> 伺服器應用程式包含內建 `AuthenticationStateProvider` 服務，可從 ASP.NET Core 的 `HttpContext.User`取得驗證狀態資料。</span><span class="sxs-lookup"><span data-stu-id="67c8c-154"> Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="67c8c-155">這就是驗證狀態與現有 ASP.NET Core 伺服器端驗證機制之間的整合方式。</span><span class="sxs-lookup"><span data-stu-id="67c8c-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="67c8c-156">`AuthenticationStateProvider` 是 `AuthorizeView` 元件與 `CascadingAuthenticationState` 元件用來取得驗證狀態的基礎服務。</span><span class="sxs-lookup"><span data-stu-id="67c8c-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="67c8c-157">您通常不會直接使用 `AuthenticationStateProvider`。</span><span class="sxs-lookup"><span data-stu-id="67c8c-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="67c8c-158">請使用此文章稍後所述的 [AuthorizeView 元件](#authorizeview-component)或 [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) 方法。</span><span class="sxs-lookup"><span data-stu-id="67c8c-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="67c8c-159">使用 `AuthenticationStateProvider` 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。</span><span class="sxs-lookup"><span data-stu-id="67c8c-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="67c8c-160">`AuthenticationStateProvider` 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="67c8c-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

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

<span data-ttu-id="67c8c-161">如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="67c8c-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="67c8c-162">如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="67c8c-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="67c8c-163">實作自訂 AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="67c8c-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="67c8c-164">如果您要建立 Blazor WebAssembly 應用程式，或如果您的應用程式規格確實需要自訂提供者，請執行提供者並覆寫 `GetAuthenticationStateAsync`：</span><span class="sxs-lookup"><span data-stu-id="67c8c-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="67c8c-165">在 Blazor WebAssembly 應用程式中，`CustomAuthStateProvider` 服務會在*Program.cs*的 `Main` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="67c8c-165">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

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

<span data-ttu-id="67c8c-166">使用 `CustomAuthStateProvider` 時，所有使用者都會以 `mrfibuli` 的使用者名稱進行驗證。</span><span class="sxs-lookup"><span data-stu-id="67c8c-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="67c8c-167">將驗證狀態公開為階層式參數</span><span class="sxs-lookup"><span data-stu-id="67c8c-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="67c8c-168">如果驗證狀態資料需要程序性邏輯 (例如在執行由使用者觸發的動作時)，請透過定義 `Task<AuthenticationState>` 類型的階層式參數來取得驗證狀態資料：</span><span class="sxs-lookup"><span data-stu-id="67c8c-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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
> <span data-ttu-id="67c8c-169">在 Blazor WebAssembly 應用程式元件中，新增 `Microsoft.AspNetCore.Components.Authorization` 命名空間（`@using Microsoft.AspNetCore.Components.Authorization`）。</span><span class="sxs-lookup"><span data-stu-id="67c8c-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="67c8c-170">如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="67c8c-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="67c8c-171">使用*應用程式 razor*檔案中的 `AuthorizeRouteView` 和 `CascadingAuthenticationState` 元件，設定 `Task<AuthenticationState>` 級聯的參數：</span><span class="sxs-lookup"><span data-stu-id="67c8c-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

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

## <a name="authorization"></a><span data-ttu-id="67c8c-172">授權</span><span class="sxs-lookup"><span data-stu-id="67c8c-172">Authorization</span></span>

<span data-ttu-id="67c8c-173">在使用者被驗證之後，系統便會套用「授權」規則以控制使用者可以執行的動作。</span><span class="sxs-lookup"><span data-stu-id="67c8c-173">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="67c8c-174">存取權通常會依據下列情況來授與或拒絕：</span><span class="sxs-lookup"><span data-stu-id="67c8c-174">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="67c8c-175">使用者已通過驗證 (已登入)。</span><span class="sxs-lookup"><span data-stu-id="67c8c-175">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="67c8c-176">使用者屬於某個「角色」。</span><span class="sxs-lookup"><span data-stu-id="67c8c-176">A user is in a *role*.</span></span>
* <span data-ttu-id="67c8c-177">使用者具有「宣告」。</span><span class="sxs-lookup"><span data-stu-id="67c8c-177">A user has a *claim*.</span></span>
* <span data-ttu-id="67c8c-178">已滿足某個「原則」。</span><span class="sxs-lookup"><span data-stu-id="67c8c-178">A *policy* is satisfied.</span></span>

<span data-ttu-id="67c8c-179">上述概念皆和 ASP.NET Core MVC 或 Razor Pages 應用程式中的概念相同。</span><span class="sxs-lookup"><span data-stu-id="67c8c-179">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="67c8c-180">如需 ASP.NET Core 安全性的詳細資訊，請參閱 [ASP.NET Core 安全性與身分識別](xref:security/index)下的文章。</span><span class="sxs-lookup"><span data-stu-id="67c8c-180">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="67c8c-181">AuthorizeView 元件</span><span class="sxs-lookup"><span data-stu-id="67c8c-181">AuthorizeView component</span></span>

<span data-ttu-id="67c8c-182">`AuthorizeView` 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。</span><span class="sxs-lookup"><span data-stu-id="67c8c-182">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="67c8c-183">此方法可讓您只需要向使用者「顯示」資料，而不需要在程序性邏輯中使用該使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="67c8c-183">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="67c8c-184">該元件會公開 `context` 類型的 `AuthenticationState` 變數，您可以使用它來存取已登入使用者的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="67c8c-184">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="67c8c-185">您也可以在使用者未經授權的情況下，提供不同的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="67c8c-185">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="67c8c-186">`<Authorized>` 和 `<NotAuthorized>` 標記的內容可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="67c8c-186">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="67c8c-187">授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。</span><span class="sxs-lookup"><span data-stu-id="67c8c-187">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="67c8c-188">如果未指定授權條件，`AuthorizeView` 會使用預設的原則，並將：</span><span class="sxs-lookup"><span data-stu-id="67c8c-188">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="67c8c-189">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="67c8c-189">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="67c8c-190">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="67c8c-190">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="67c8c-191">角色型和原則型授權</span><span class="sxs-lookup"><span data-stu-id="67c8c-191">Role-based and policy-based authorization</span></span>

<span data-ttu-id="67c8c-192">`AuthorizeView` 元件支援「角色型」或「原則型」授權。</span><span class="sxs-lookup"><span data-stu-id="67c8c-192">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="67c8c-193">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="67c8c-193">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="67c8c-194">如需詳細資訊，請參閱 <xref:security/authorization/roles>。</span><span class="sxs-lookup"><span data-stu-id="67c8c-194">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="67c8c-195">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="67c8c-195">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="67c8c-196">宣告型授權是特殊案例的原則型授權。</span><span class="sxs-lookup"><span data-stu-id="67c8c-196">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="67c8c-197">例如，您可以定義要求使用者具備特定宣告的原則。</span><span class="sxs-lookup"><span data-stu-id="67c8c-197">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="67c8c-198">如需詳細資訊，請參閱 <xref:security/authorization/policies>。</span><span class="sxs-lookup"><span data-stu-id="67c8c-198">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="67c8c-199">這些 Api 可以在 Blazor Server 或 Blazor WebAssembly 應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="67c8c-199">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="67c8c-200">如果未指定 `Roles` 和 `Policy`，`AuthorizeView` 便會使用預設原則。</span><span class="sxs-lookup"><span data-stu-id="67c8c-200">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="67c8c-201">在非同步驗證期間所顯示的內容</span><span class="sxs-lookup"><span data-stu-id="67c8c-201">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="67c8c-202"> 允許以*非同步方式*判斷驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="67c8c-202"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="67c8c-203">這種方法的主要案例是在 Blazor WebAssembly 應用程式中提出對外部端點進行驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="67c8c-203">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="67c8c-204">在驗證期間，`AuthorizeView` 預設不會顯示任何內容。</span><span class="sxs-lookup"><span data-stu-id="67c8c-204">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="67c8c-205">若要在驗證發生時顯示內容，請使用 `<Authorizing>` 元素：</span><span class="sxs-lookup"><span data-stu-id="67c8c-205">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="67c8c-206">這種方法通常不適用於 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="67c8c-206">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="67c8c-207"> 伺服器應用程式會在狀態建立後立即知道驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="67c8c-207"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="67c8c-208">`Authorizing` 內容可以在 Blazor 伺服器應用程式的 `AuthorizeView` 元件中提供，但永遠不會顯示內容。</span><span class="sxs-lookup"><span data-stu-id="67c8c-208">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="67c8c-209">[Authorize] 屬性</span><span class="sxs-lookup"><span data-stu-id="67c8c-209">[Authorize] attribute</span></span>

<span data-ttu-id="67c8c-210">`[Authorize]` 屬性可以在 Razor 元件中使用：</span><span class="sxs-lookup"><span data-stu-id="67c8c-210">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="67c8c-211">在 Blazor WebAssembly 應用程式元件中，將 `Microsoft.AspNetCore.Authorization` 命名空間（`@using Microsoft.AspNetCore.Authorization`）新增至本節中的範例。</span><span class="sxs-lookup"><span data-stu-id="67c8c-211">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67c8c-212">請只在透過 Blazor 路由器到達 `@page` 元件上使用 `[Authorize]`。</span><span class="sxs-lookup"><span data-stu-id="67c8c-212">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="67c8c-213">授權僅會以路由的層面執行，且「不」適用於在頁面內轉譯的子元件。</span><span class="sxs-lookup"><span data-stu-id="67c8c-213">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="67c8c-214">若要授權在頁面內顯示特定組件，請改為使用 `AuthorizeView`。</span><span class="sxs-lookup"><span data-stu-id="67c8c-214">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="67c8c-215">`[Authorize]` 屬性也支援角色型或原則型授權。</span><span class="sxs-lookup"><span data-stu-id="67c8c-215">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="67c8c-216">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="67c8c-216">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="67c8c-217">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="67c8c-217">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="67c8c-218">如果未指定 `Roles` 與 `Policy`，`[Authorize]` 便會使用預設原則，它預設會將：</span><span class="sxs-lookup"><span data-stu-id="67c8c-218">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="67c8c-219">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="67c8c-219">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="67c8c-220">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="67c8c-220">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="67c8c-221">搭配 Router 元件自訂未經授權的內容</span><span class="sxs-lookup"><span data-stu-id="67c8c-221">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="67c8c-222">`Router` 元件與 `AuthorizeRouteView` 元件結合，可讓應用程式在下列情況指定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="67c8c-222">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="67c8c-223">找不到內容。</span><span class="sxs-lookup"><span data-stu-id="67c8c-223">Content isn't found.</span></span>
* <span data-ttu-id="67c8c-224">使用者無法滿足套用至元件的 `[Authorize]` 條件。</span><span class="sxs-lookup"><span data-stu-id="67c8c-224">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="67c8c-225">[`[Authorize]` 屬性](#authorize-attribute)一節涵蓋 `[Authorize]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="67c8c-225">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="67c8c-226">正在進行非同步驗證。</span><span class="sxs-lookup"><span data-stu-id="67c8c-226">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="67c8c-227">在預設的 Blazor Server 專案範本中，*應用程式 razor*檔案示範如何設定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="67c8c-227">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="67c8c-228">`<NotFound>`、`<NotAuthorized>`和 `<Authorizing>` 標記的內容可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="67c8c-228">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="67c8c-229">如果未指定 `<NotAuthorized>` 元素，則 `AuthorizeRouteView` 會使用下列回溯訊息：</span><span class="sxs-lookup"><span data-stu-id="67c8c-229">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="67c8c-230">關於驗證狀態變更的通知</span><span class="sxs-lookup"><span data-stu-id="67c8c-230">Notification about authentication state changes</span></span>

<span data-ttu-id="67c8c-231">如果應用程式判斷基礎驗證狀態資料已經變更 (例如由於使用者已登出，或是另一個使用者已變更其角色)，自訂的 `AuthenticationStateProvider` 可以選擇性地在 `NotifyAuthenticationStateChanged` 基底類別上叫用 `AuthenticationStateProvider` 方法。</span><span class="sxs-lookup"><span data-stu-id="67c8c-231">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="67c8c-232">這會通知驗證狀態資料的取用者 (例如 `AuthorizeView`) 使用新資料來重新轉譯。</span><span class="sxs-lookup"><span data-stu-id="67c8c-232">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="67c8c-233">程序性邏輯</span><span class="sxs-lookup"><span data-stu-id="67c8c-233">Procedural logic</span></span>

<span data-ttu-id="67c8c-234">如果作為程序性邏輯的一部分，應用程式必須檢查授權規則，請使用 `Task<AuthenticationState>` 類型的階層式參數來取得使用者的 <xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="67c8c-234">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="67c8c-235">`Task<AuthenticationState>` 可以與其他服務 (例如 `IAuthorizationService`) 結合來評估原則。</span><span class="sxs-lookup"><span data-stu-id="67c8c-235">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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
> <span data-ttu-id="67c8c-236">在 Blazor WebAssembly 應用程式元件中，新增 `Microsoft.AspNetCore.Authorization` 和 `Microsoft.AspNetCore.Components.Authorization` 命名空間：</span><span class="sxs-lookup"><span data-stu-id="67c8c-236">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="67c8c-237">Blazor WebAssembly 應用程式中的授權</span><span class="sxs-lookup"><span data-stu-id="67c8c-237">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="67c8c-238">在 Blazor WebAssembly apps 中，可以略過授權檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="67c8c-238">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="67c8c-239">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="67c8c-239">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="67c8c-240">**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**</span><span class="sxs-lookup"><span data-stu-id="67c8c-240">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="67c8c-241">針對錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="67c8c-241">Troubleshoot errors</span></span>

<span data-ttu-id="67c8c-242">常見錯誤：</span><span class="sxs-lookup"><span data-stu-id="67c8c-242">Common errors:</span></span>

* <span data-ttu-id="67c8c-243">**授權需要類型為 Task\<AuthenticationState > 的串聯參數。請考慮使用 CascadingAuthenticationState 來提供此。**</span><span class="sxs-lookup"><span data-stu-id="67c8c-243">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="67c8c-244">**針對 `null` 接收到 `authenticationStateTask` 值**</span><span class="sxs-lookup"><span data-stu-id="67c8c-244">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="67c8c-245">專案可能不是使用已啟用驗證的 Blazor 伺服器範本來建立。</span><span class="sxs-lookup"><span data-stu-id="67c8c-245">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="67c8c-246">請將 `<CascadingAuthenticationState>` 包裝在 UI 樹狀的一部分，例如在 *App.razor* 中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="67c8c-246">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="67c8c-247">`CascadingAuthenticationState` 會提供 `Task<AuthenticationState>` 階層式參數，它接著會接收自底層 `AuthenticationStateProvider` DI 服務。</span><span class="sxs-lookup"><span data-stu-id="67c8c-247">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="67c8c-248">其他資源</span><span class="sxs-lookup"><span data-stu-id="67c8c-248">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="67c8c-249">卓越的[Blazor：驗證](https://github.com/AdrienTorris/awesome-blazor#authentication)社區範例連結</span><span class="sxs-lookup"><span data-stu-id="67c8c-249">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
