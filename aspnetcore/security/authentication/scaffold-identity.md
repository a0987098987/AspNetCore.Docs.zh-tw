---
title: IdentityASP.NET Core 專案中的 Scaffold
author: rick-anderson
description: 瞭解如何 Identity 在 ASP.NET Core 專案中 scaffold。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 5/1/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 36afa8ece58843b434ebfba6305bffdb9eb9bca0
ms.sourcegitcommit: d243fadeda20ad4f142ea60301ae5f5e0d41ed60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724285"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="2e941-103">IdentityASP.NET Core 專案中的 Scaffold</span><span class="sxs-lookup"><span data-stu-id="2e941-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="2e941-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2e941-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2e941-105">ASP.NET Core 提供[ASP.NET Core Identity ](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="2e941-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="2e941-106">包含的應用程式 Identity 可以套用 scaffolder，以選擇性地新增包含在 Identity Razor 類別庫（RCL）中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="2e941-107">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="2e941-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="2e941-108">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="2e941-109">產生的程式碼優先于 RCL 中的相同程式碼 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="2e941-110">若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整的 Identity UI 來源](#full)一節。</span><span class="sxs-lookup"><span data-stu-id="2e941-110">To gain full control of the UI and not use the default RCL, see the section [Create full Identity UI source](#full).</span></span>

<span data-ttu-id="2e941-111">**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL Identity 套件。</span><span class="sxs-lookup"><span data-stu-id="2e941-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="2e941-112">您可以選擇 Identity 要產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="2e941-113">雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成流程。</span><span class="sxs-lookup"><span data-stu-id="2e941-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="2e941-114">本檔說明完成「基架構更新」所需的步驟 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="2e941-115">我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。</span><span class="sxs-lookup"><span data-stu-id="2e941-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="2e941-116">執行 scaffolder 後檢查變更 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="2e941-117">使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他安全性功能時，都需要服務 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="2e941-118">當樣板時，不會產生服務或服務存根 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="2e941-119">啟用這些功能的服務必須手動新增。</span><span class="sxs-lookup"><span data-stu-id="2e941-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="2e941-120">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="2e941-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="2e941-121">當 Identity 具有新資料內容的樣板加入具有現有個別帳戶的專案時：</span><span class="sxs-lookup"><span data-stu-id="2e941-121">When scaffolding Identity with a new data context into a project with existing individual accounts:</span></span>

* <span data-ttu-id="2e941-122">在中 `Startup.ConfigureServices` ，移除對的呼叫：</span><span class="sxs-lookup"><span data-stu-id="2e941-122">In `Startup.ConfigureServices`, remove the calls to:</span></span>
  * `AddDbContext`
  * `AddDefaultIdentity`

<span data-ttu-id="2e941-123">例如， `AddDbContext` 和 `AddDefaultIdentity` 會在下列程式碼中加上批註：</span><span class="sxs-lookup"><span data-stu-id="2e941-123">For example, `AddDbContext` and `AddDefaultIdentity` are commented out in the following code:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRemove.cs?name=snippet)]

<span data-ttu-id="2e941-124">前面的程式碼會批註*區域/ Identity /IdentityHostingStartup.cs*中重複的程式碼</span><span class="sxs-lookup"><span data-stu-id="2e941-124">The preceeding code comments out the code that is duplicated in *Areas/Identity/IdentityHostingStartup.cs*</span></span>

<span data-ttu-id="2e941-125">一般而言，使用個別帳戶建立的應用程式***不***應該建立新的資料內容。</span><span class="sxs-lookup"><span data-stu-id="2e941-125">Typically, apps that were created with individual accounts should ***not*** create a new data context.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="2e941-126">Scaffold Identity 至空的專案</span><span class="sxs-lookup"><span data-stu-id="2e941-126">Scaffold Identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="2e941-127">使用與 `Startup` 下列類似的程式碼來更新類別：</span><span class="sxs-lookup"><span data-stu-id="2e941-127">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="2e941-128">Scaffold Identity 至 Razor 沒有現有授權的專案</span><span class="sxs-lookup"><span data-stu-id="2e941-128">Scaffold Identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef database drop
dotnet ef migrations add CreateIdentitySchema0
dotnet ef database update
-->

<!-- ERROR
There is already an object named 'AspNetRoles' in the database.

Fixed via dotnet ef database drop
before dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identity<span data-ttu-id="2e941-129">會在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-129"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-130">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="2e941-130">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="2e941-131">遷移、UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="2e941-131">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="2e941-132">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="2e941-132">Enable authentication</span></span>

<span data-ttu-id="2e941-133">使用與 `Startup` 下列類似的程式碼來更新類別：</span><span class="sxs-lookup"><span data-stu-id="2e941-133">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="2e941-134">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="2e941-134">Layout changes</span></span>

<span data-ttu-id="2e941-135">選擇性：將登入部分（ `_LoginPartial` ）新增至配置檔案：</span><span class="sxs-lookup"><span data-stu-id="2e941-135">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="2e941-136">Scaffold Identity 至 Razor 具有授權的專案</span><span class="sxs-lookup"><span data-stu-id="2e941-136">Scaffold Identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="2e941-137">有些 Identity 選項是在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-137">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-138">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="2e941-138">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="2e941-139">Scaffold Identity 至沒有現有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="2e941-139">Scaffold Identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="2e941-140">選擇性：將登入部分（ `_LoginPartial` ）新增至*Views/Shared/_Layout. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="2e941-140">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="2e941-141">將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="2e941-141">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

Identity<span data-ttu-id="2e941-142">會在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-142"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-143">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="2e941-143">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="2e941-144">使用與 `Startup` 下列類似的程式碼來更新類別：</span><span class="sxs-lookup"><span data-stu-id="2e941-144">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="2e941-145">Scaffold Identity 至具有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="2e941-145">Scaffold Identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

## <a name="scaffold-identity-into-a-blazor-server-project-without-existing-authorization"></a><span data-ttu-id="2e941-146">Scaffold Identity 至 Blazor 沒有現有授權的伺服器專案</span><span class="sxs-lookup"><span data-stu-id="2e941-146">Scaffold Identity into a Blazor Server project without existing authorization</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identity<span data-ttu-id="2e941-147">會在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-147"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-148">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="2e941-148">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

### <a name="migrations"></a><span data-ttu-id="2e941-149">移轉</span><span class="sxs-lookup"><span data-stu-id="2e941-149">Migrations</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

### <a name="pass-an-xsrf-token-to-the-app"></a><span data-ttu-id="2e941-150">將 XSRF token 傳遞至應用程式</span><span class="sxs-lookup"><span data-stu-id="2e941-150">Pass an XSRF token to the app</span></span>

<span data-ttu-id="2e941-151">權杖可以傳遞給元件：</span><span class="sxs-lookup"><span data-stu-id="2e941-151">Tokens can be passed to components:</span></span>

* <span data-ttu-id="2e941-152">當驗證權杖已布建並儲存至驗證 cookie 時，可以將它們傳遞給元件。</span><span class="sxs-lookup"><span data-stu-id="2e941-152">When authentication tokens are provisioned and saved to the authentication cookie, they can be passed to components.</span></span>
* Razor<span data-ttu-id="2e941-153">元件無法 `HttpContext` 直接使用，因此無法取得[反要求偽造（XSRF）權杖](xref:security/anti-request-forgery)，以在張貼到 Identity 的登出端點 `/Identity/Account/Logout` 。</span><span class="sxs-lookup"><span data-stu-id="2e941-153"> components can't use `HttpContext` directly, so there's no way to obtain an [anti-request forgery (XSRF) token](xref:security/anti-request-forgery) to POST to Identity's logout endpoint at `/Identity/Account/Logout`.</span></span> <span data-ttu-id="2e941-154">XSRF token 可以傳遞給元件。</span><span class="sxs-lookup"><span data-stu-id="2e941-154">An XSRF token can be passed to components.</span></span>

<span data-ttu-id="2e941-155">如需詳細資訊，請參閱 <xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app> 。</span><span class="sxs-lookup"><span data-stu-id="2e941-155">For more information, see <xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app>.</span></span>

<span data-ttu-id="2e941-156">在*Pages/_Host. cshtml*檔案中，在將它加入和類別之後，建立權杖 `InitialApplicationState` `TokenProvider` ：</span><span class="sxs-lookup"><span data-stu-id="2e941-156">In the *Pages/_Host.cshtml* file, establish the token after adding it to the `InitialApplicationState` and `TokenProvider` classes:</span></span>

```csharp
@inject Microsoft.AspNetCore.Antiforgery.IAntiforgery Xsrf

...

var tokens = new InitialApplicationState
{
    ...

    XsrfToken = Xsrf.GetAndStoreTokens(HttpContext).RequestToken
};
```

<span data-ttu-id="2e941-157">更新 `App` 元件（*razor*）以指派 `InitialState.XsrfToken` ：</span><span class="sxs-lookup"><span data-stu-id="2e941-157">Update the `App` component (*App.razor*) to assign the `InitialState.XsrfToken`:</span></span>

```csharp
@inject TokenProvider TokenProvider

...

TokenProvider.XsrfToken = InitialState.XsrfToken;
```

<span data-ttu-id="2e941-158">本 `TokenProvider` 主題中所示範的服務用於 `LoginDisplay` 下列版面配置[和驗證流程變更](#layout-and-authentication-flow-changes)一節中的元件。</span><span class="sxs-lookup"><span data-stu-id="2e941-158">The `TokenProvider` service demonstrated in the topic is used in the `LoginDisplay` component in the following [Layout and authentication flow changes](#layout-and-authentication-flow-changes) section.</span></span>

### <a name="enable-authentication"></a><span data-ttu-id="2e941-159">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="2e941-159">Enable authentication</span></span>

<span data-ttu-id="2e941-160">在 `Startup` 類別中：</span><span class="sxs-lookup"><span data-stu-id="2e941-160">In the `Startup` class:</span></span>

* <span data-ttu-id="2e941-161">確認 Razor 已在中新增頁面服務 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="2e941-161">Confirm that Razor Pages services are added in `Startup.ConfigureServices`.</span></span>
* <span data-ttu-id="2e941-162">如果使用[TokenProvider](xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app)，請註冊服務。</span><span class="sxs-lookup"><span data-stu-id="2e941-162">If using the [TokenProvider](xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app), register the service.</span></span>
* <span data-ttu-id="2e941-163">`UseDatabaseErrorPage`針對開發環境，在的應用程式產生器上呼叫 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="2e941-163">Call `UseDatabaseErrorPage` on the application builder in `Startup.Configure` for the Development environment.</span></span>
* <span data-ttu-id="2e941-164">`UseAuthentication` `UseAuthorization` 在之後呼叫和 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="2e941-164">Call `UseAuthentication` and `UseAuthorization` after `UseRouting`.</span></span>
* <span data-ttu-id="2e941-165">新增頁面的端點 Razor 。</span><span class="sxs-lookup"><span data-stu-id="2e941-165">Add an endpoint for Razor Pages.</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupBlazor.cs?highlight=3,6,14,27-28,32)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-and-authentication-flow-changes"></a><span data-ttu-id="2e941-166">版面配置和驗證流程變更</span><span class="sxs-lookup"><span data-stu-id="2e941-166">Layout and authentication flow changes</span></span>

<span data-ttu-id="2e941-167">`RedirectToLogin`在專案根目錄中，將元件（*RedirectToLogin*）新增至應用程式的*共用*資料夾：</span><span class="sxs-lookup"><span data-stu-id="2e941-167">Add a `RedirectToLogin` component (*RedirectToLogin.razor*) to the app's *Shared* folder in the project root:</span></span>

```razor
@inject NavigationManager Navigation
@code {
    protected override void OnInitialized()
    {
        Navigation.NavigateTo("Identity/Account/Login?returnUrl=" +
            Uri.EscapeDataString(Navigation.Uri), true);
    }
}
```

將 `LoginDisplay` 元件（*LoginDisplay*）新增至應用程式的*共用*資料夾。 <span data-ttu-id="2e941-169">[TokenProvider 服務](xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app)會針對張貼至的登出端點的 HTML 表單，提供 XSRF token Identity ：</span><span class="sxs-lookup"><span data-stu-id="2e941-169">The [TokenProvider service](xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app) provides the XSRF token for the HTML form that POSTs to Identity's logout endpoint:</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@inject NavigationManager Navigation
@inject TokenProvider TokenProvider

<AuthorizeView>
    <Authorized>
        <a href="Identity/Account/Manage/Index">
            Hello, @context.User.Identity.Name!
        </a>
        <form action="/Identity/Account/Logout?returnUrl=%2F" method="post">
            <button class="nav-link btn btn-link" type="submit">Logout</button>
            <input name="__RequestVerificationToken" type="hidden" 
                value="@TokenProvider.XsrfToken">
        </form>
    </Authorized>
    <NotAuthorized>
        <a href="Identity/Account/Register">Register</a>
        <a href="Identity/Account/Login">Login</a>
    </NotAuthorized>
</AuthorizeView>
```

<span data-ttu-id="2e941-170">在 `MainLayout` 元件（*Shared/MainLayout*）中，將 `LoginDisplay` 元件加入至頂端資料列 `<div>` 元素的內容：</span><span class="sxs-lookup"><span data-stu-id="2e941-170">In the `MainLayout` component (*Shared/MainLayout.razor*), add the `LoginDisplay` component to the top-row `<div>` element's content:</span></span>

```razor
<div class="top-row px-4 auth">
    <LoginDisplay />
    <a href="https://docs.microsoft.com/aspnet/" target="_blank">About</a>
</div>
```

### <a name="style-authentication-endpoints"></a><span data-ttu-id="2e941-171">樣式驗證端點</span><span class="sxs-lookup"><span data-stu-id="2e941-171">Style authentication endpoints</span></span>

<span data-ttu-id="2e941-172">因為 Blazor 伺服器使用 Razor 頁面 Identity 頁面，所以當訪客在頁面和元件之間流覽時，UI 的樣式會變更 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-172">Because Blazor Server uses Razor Pages Identity pages, the styling of the UI changes when a visitor navigates between Identity pages and components.</span></span> <span data-ttu-id="2e941-173">您有兩個選項可解決 incongruous 樣式：</span><span class="sxs-lookup"><span data-stu-id="2e941-173">You have two options to address the incongruous styles:</span></span>

#### <a name="build-identity-components"></a><span data-ttu-id="2e941-174">組建 Identity 元件</span><span class="sxs-lookup"><span data-stu-id="2e941-174">Build Identity components</span></span>

<span data-ttu-id="2e941-175">不使用頁面的元件是用來 Identity 建立元件的方法 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-175">An approach to using components for Identity instead of pages is to build Identity components.</span></span> <span data-ttu-id="2e941-176">因為 `SignInManager` `UserManager` 元件中不支援和 Razor ，所以請使用伺服器應用程式中的 API 端點 Blazor 來處理使用者帳戶動作。</span><span class="sxs-lookup"><span data-stu-id="2e941-176">Because `SignInManager` and `UserManager` aren't supported in Razor components, use API endpoints in the Blazor Server app to process user account actions.</span></span>

#### <a name="use-a-custom-layout-with-blazor-app-styles"></a><span data-ttu-id="2e941-177">使用具有 Blazor 應用程式樣式的自訂版面配置</span><span class="sxs-lookup"><span data-stu-id="2e941-177">Use a custom layout with Blazor app styles</span></span>

<span data-ttu-id="2e941-178">您 Identity 可以修改頁面版面配置和樣式，以產生使用預設主題的頁面 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="2e941-178">The Identity pages layout and styles can be modified to produce pages that use the default Blazor theme.</span></span>

> [!NOTE]
> <span data-ttu-id="2e941-179">本節中的範例只是自訂的起點。</span><span class="sxs-lookup"><span data-stu-id="2e941-179">The example in this section is merely a starting point for customization.</span></span> <span data-ttu-id="2e941-180">可能需要額外的工作，才能獲得最佳的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="2e941-180">Additional work is likely required for the best user experience.</span></span>

<span data-ttu-id="2e941-181">建立新的 `NavMenu_IdentityLayout` 元件（*Shared/NavMenu_IdentityLayout razor*）。</span><span class="sxs-lookup"><span data-stu-id="2e941-181">Create a new `NavMenu_IdentityLayout` component (*Shared/NavMenu_IdentityLayout.razor*).</span></span> <span data-ttu-id="2e941-182">如需元件的標記和程式碼，請使用應用程式元件的相同內容 `NavMenu` （*Shared/navmenu.cshtml*）。</span><span class="sxs-lookup"><span data-stu-id="2e941-182">For the markup and code of the component, use the same content of the app's `NavMenu` component (*Shared/NavMenu.razor*).</span></span> <span data-ttu-id="2e941-183">去除 `NavLink` 無法匿名連線的任何元件，因為元件中的自動重新導向在 `RedirectToLogin` 需要驗證或授權的元件中失敗。</span><span class="sxs-lookup"><span data-stu-id="2e941-183">Strip out any `NavLink`s to components that can't be reached anonymously because automatic redirects in the `RedirectToLogin` component fail for components requiring authentication or authorization.</span></span>

<span data-ttu-id="2e941-184">在*Pages/Shared/Layout*檔案中，進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="2e941-184">In the *Pages/Shared/Layout.cshtml* file, make the following changes:</span></span>

* <span data-ttu-id="2e941-185">將指示詞新增至檔案 Razor 頂端，以在*共用*資料夾中使用標籤協助程式和應用程式的元件：</span><span class="sxs-lookup"><span data-stu-id="2e941-185">Add Razor directives to the top of the file to use Tag Helpers and the app's components in the *Shared* folder:</span></span>

  ```cshtml
  @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
  @using {APPLICATION ASSEMBLY}.Shared
  ```

  <span data-ttu-id="2e941-186">取代 `{APPLICATION ASSEMBLY}` 為應用程式的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="2e941-186">Replace `{APPLICATION ASSEMBLY}` with the app's assembly name.</span></span>

* <span data-ttu-id="2e941-187">將 `<base>` 標記和樣式表單新增 Blazor `<link>` 至 `<head>` 內容：</span><span class="sxs-lookup"><span data-stu-id="2e941-187">Add a `<base>` tag and Blazor stylesheet `<link>` to the `<head>` content:</span></span>

  ```cshtml
  <base href="~/" />
  <link rel="stylesheet" href="~/css/site.css" />
  ```

* <span data-ttu-id="2e941-188">將標記的內容變更 `<body>` 如下：</span><span class="sxs-lookup"><span data-stu-id="2e941-188">Change the content of the `<body>` tag to the following:</span></span>

  ```cshtml
  <div class="sidebar" style="float:left">
      <component type="typeof(NavMenu_IdentityLayout)" 
          render-mode="ServerPrerendered" />
  </div>

  <div class="main" style="padding-left:250px">
      <div class="top-row px-4">
          @{
              var result = Engine.FindView(ViewContext, "_LoginPartial", 
                  isMainPage: false);
          }
          @if (result.Success)
          {
              await Html.RenderPartialAsync("_LoginPartial");
          }
          else
          {
              throw new InvalidOperationException("The default Identity UI " +
                  "layout requires a partial view '_LoginPartial'.");
          }
          <a href="https://docs.microsoft.com/aspnet/" target="_blank">About</a>
      </div>

      <div class="content px-4">
          @RenderBody()
      </div>
  </div>

  <script src="~/Identity/lib/jquery/dist/jquery.min.js"></script>
  <script src="~/Identity/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
  <script src="~/Identity/js/site.js" asp-append-version="true"></script>
  @RenderSection("Scripts", required: false)
  <script src="_framework/blazor.server.js"></script>
  ```

## <a name="scaffold-identity-into-a-blazor-server-project-with-authorization"></a><span data-ttu-id="2e941-189">Scaffold Identity 至 Blazor 具有授權的伺服器專案</span><span class="sxs-lookup"><span data-stu-id="2e941-189">Scaffold Identity into a Blazor Server project with authorization</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="2e941-190">有些 Identity 選項是在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-190">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-191">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="2e941-191">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="2e941-192">建立完整的 Identity UI 來源</span><span class="sxs-lookup"><span data-stu-id="2e941-192">Create full Identity UI source</span></span>

<span data-ttu-id="2e941-193">若要維持 UI 的完全控制 Identity ，請執行 Identity scaffolder，然後選取 [覆**寫所有**檔案]。</span><span class="sxs-lookup"><span data-stu-id="2e941-193">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="2e941-194">下列反白顯示的程式碼顯示在 Identity Identity ASP.NET Core 2.1 web 應用程式中，將預設 UI 取代為的變更。</span><span class="sxs-lookup"><span data-stu-id="2e941-194">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="2e941-195">您可能想要這麼做，以擁有 UI 的完全控制權 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-195">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="2e941-196">Identity下列程式碼會取代預設值：</span><span class="sxs-lookup"><span data-stu-id="2e941-196">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="2e941-197">下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：</span><span class="sxs-lookup"><span data-stu-id="2e941-197">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="2e941-198">註冊執行 `IEmailSender` ，例如：</span><span class="sxs-lookup"><span data-stu-id="2e941-198">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-a-page"></a><span data-ttu-id="2e941-199">停用頁面</span><span class="sxs-lookup"><span data-stu-id="2e941-199">Disable a page</span></span>

<span data-ttu-id="2e941-200">本章節說明如何停用 [註冊] 頁面，但此方法可用來停用任何頁面。</span><span class="sxs-lookup"><span data-stu-id="2e941-200">This sections show how to disable the register page but the approach can be used to disable any page.</span></span>

<span data-ttu-id="2e941-201">若要停用使用者註冊：</span><span class="sxs-lookup"><span data-stu-id="2e941-201">To disable user registration:</span></span>

* <span data-ttu-id="2e941-202">Scaffold Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-202">Scaffold Identity.</span></span> <span data-ttu-id="2e941-203">包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。</span><span class="sxs-lookup"><span data-stu-id="2e941-203">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="2e941-204">例如：</span><span class="sxs-lookup"><span data-stu-id="2e941-204">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="2e941-205">更新*區域/ Identity /Pages/Account/Register.cshtml.cs* ，讓使用者無法從這個端點註冊：</span><span class="sxs-lookup"><span data-stu-id="2e941-205">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="2e941-206">將*Areas/ Identity /Pages/Account/Register.cshtml*更新為與前述變更一致：</span><span class="sxs-lookup"><span data-stu-id="2e941-206">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="2e941-207">批註外或移除*區域/ Identity /Pages/Account/Login.cshtml*的註冊連結</span><span class="sxs-lookup"><span data-stu-id="2e941-207">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

  ```cshtml
  @*
  <p>
      <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
  </p>
  *@
  ```

* <span data-ttu-id="2e941-208">更新 [*區域/ Identity /Pages/Account/RegisterConfirmation* ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="2e941-208">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="2e941-209">移除來自 cshtml 檔案的程式碼和連結。</span><span class="sxs-lookup"><span data-stu-id="2e941-209">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="2e941-210">從移除確認碼 `PageModel` ：</span><span class="sxs-lookup"><span data-stu-id="2e941-210">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="2e941-211">使用另一個應用程式來新增使用者</span><span class="sxs-lookup"><span data-stu-id="2e941-211">Use another app to add users</span></span>

<span data-ttu-id="2e941-212">提供在 web 應用程式外部新增使用者的機制。</span><span class="sxs-lookup"><span data-stu-id="2e941-212">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="2e941-213">新增使用者的選項包括：</span><span class="sxs-lookup"><span data-stu-id="2e941-213">Options to add users include:</span></span>

* <span data-ttu-id="2e941-214">專用的管理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2e941-214">A dedicated admin web app.</span></span>
* <span data-ttu-id="2e941-215">主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="2e941-215">A console app.</span></span>

<span data-ttu-id="2e941-216">下列程式碼概述新增使用者的一種方法：</span><span class="sxs-lookup"><span data-stu-id="2e941-216">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="2e941-217">使用者清單會讀入記憶體中。</span><span class="sxs-lookup"><span data-stu-id="2e941-217">A list of users is read into memory.</span></span>
* <span data-ttu-id="2e941-218">為每個使用者產生強式唯一密碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-218">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="2e941-219">使用者會新增至 Identity 資料庫。</span><span class="sxs-lookup"><span data-stu-id="2e941-219">The user is added to the Identity database.</span></span>
* <span data-ttu-id="2e941-220">系統會通知使用者，並告知您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-220">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="2e941-221">下列程式碼概述如何新增使用者：</span><span class="sxs-lookup"><span data-stu-id="2e941-221">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="2e941-222">針對生產案例，可以遵循類似的方法。</span><span class="sxs-lookup"><span data-stu-id="2e941-222">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="2e941-223">防止發行靜態 Identity 資產</span><span class="sxs-lookup"><span data-stu-id="2e941-223">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="2e941-224">若要防止將靜態 Identity 資產發行至 web 根目錄，請參閱 <xref:security/authentication/identity#prevent-publish-of-static-identity-assets> 。</span><span class="sxs-lookup"><span data-stu-id="2e941-224">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e941-225">其他資源</span><span class="sxs-lookup"><span data-stu-id="2e941-225">Additional resources</span></span>

* [<span data-ttu-id="2e941-226">將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="2e941-226">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2e941-227">ASP.NET Core 2.1 和更新版本[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="2e941-227">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="2e941-228">包含的應用程式 Identity 可以套用 scaffolder，以選擇性地新增包含在 Identity Razor 類別庫（RCL）中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-228">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="2e941-229">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="2e941-229">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="2e941-230">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-230">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="2e941-231">產生的程式碼優先于 RCL 中的相同程式碼 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-231">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="2e941-232">若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。</span><span class="sxs-lookup"><span data-stu-id="2e941-232">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="2e941-233">**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL Identity 套件。</span><span class="sxs-lookup"><span data-stu-id="2e941-233">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="2e941-234">您可以選擇 Identity 要產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-234">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="2e941-235">雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成此流程。</span><span class="sxs-lookup"><span data-stu-id="2e941-235">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="2e941-236">本檔說明完成「基架構更新」所需的步驟 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-236">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="2e941-237">Identity執行 scaffolder 時，會在專案目錄中建立*ScaffoldingReadme.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="2e941-237">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="2e941-238">*ScaffoldingReadme.txt*檔案包含完成樣板更新所需需求的一般指示 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-238">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="2e941-239">本檔包含比*ScaffoldingReadme.txt*檔案更完整的指示。</span><span class="sxs-lookup"><span data-stu-id="2e941-239">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="2e941-240">我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。</span><span class="sxs-lookup"><span data-stu-id="2e941-240">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="2e941-241">執行 scaffolder 後檢查變更 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-241">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="2e941-242">使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他安全性功能時，都需要服務 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-242">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="2e941-243">當樣板時，不會產生服務或服務存根 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-243">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="2e941-244">啟用這些功能的服務必須手動新增。</span><span class="sxs-lookup"><span data-stu-id="2e941-244">Services to enable these features must be added manually.</span></span> <span data-ttu-id="2e941-245">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="2e941-245">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="2e941-246">Scaffold Identity 至空的專案</span><span class="sxs-lookup"><span data-stu-id="2e941-246">Scaffold Identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="2e941-247">將下列反白顯示的呼叫新增至 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="2e941-247">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="2e941-248">Scaffold Identity 至 Razor 沒有現有授權的專案</span><span class="sxs-lookup"><span data-stu-id="2e941-248">Scaffold Identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identity<span data-ttu-id="2e941-249">會在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-249"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-250">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="2e941-250">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="2e941-251">遷移、UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="2e941-251">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="2e941-252">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="2e941-252">Enable authentication</span></span>

<span data-ttu-id="2e941-253">在 `Configure` 類別的方法中 `Startup` ，于之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) `UseStaticFiles` ：</span><span class="sxs-lookup"><span data-stu-id="2e941-253">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="2e941-254">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="2e941-254">Layout changes</span></span>

<span data-ttu-id="2e941-255">選擇性：將登入部分（ `_LoginPartial` ）新增至配置檔案：</span><span class="sxs-lookup"><span data-stu-id="2e941-255">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="2e941-256">Scaffold Identity 至 Razor 具有授權的專案</span><span class="sxs-lookup"><span data-stu-id="2e941-256">Scaffold Identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="2e941-257">有些 Identity 選項是在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-257">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-258">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="2e941-258">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="2e941-259">Scaffold Identity 至沒有現有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="2e941-259">Scaffold Identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="2e941-260">選擇性：將登入部分（ `_LoginPartial` ）新增至*Views/Shared/_Layout. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="2e941-260">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="2e941-261">將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="2e941-261">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

Identity<span data-ttu-id="2e941-262">會在*區域/ Identity /IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="2e941-262"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2e941-263">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="2e941-263">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="2e941-264">在之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) `UseStaticFiles` ：</span><span class="sxs-lookup"><span data-stu-id="2e941-264">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="2e941-265">Scaffold Identity 至具有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="2e941-265">Scaffold Identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="2e941-266">刪除*頁面/共用*資料夾，以及該資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="2e941-266">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="2e941-267">建立完整的 Identity UI 來源</span><span class="sxs-lookup"><span data-stu-id="2e941-267">Create full Identity UI source</span></span>

<span data-ttu-id="2e941-268">若要維持 UI 的完全控制 Identity ，請執行 Identity scaffolder，然後選取 [覆**寫所有**檔案]。</span><span class="sxs-lookup"><span data-stu-id="2e941-268">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="2e941-269">下列反白顯示的程式碼顯示在 Identity Identity ASP.NET Core 2.1 web 應用程式中，將預設 UI 取代為的變更。</span><span class="sxs-lookup"><span data-stu-id="2e941-269">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="2e941-270">您可能想要這麼做，以擁有 UI 的完全控制權 Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-270">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="2e941-271">Identity下列程式碼會取代預設值：</span><span class="sxs-lookup"><span data-stu-id="2e941-271">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="2e941-272">下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：</span><span class="sxs-lookup"><span data-stu-id="2e941-272">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="2e941-273">註冊執行 `IEmailSender` ，例如：</span><span class="sxs-lookup"><span data-stu-id="2e941-273">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="2e941-274">停用註冊頁面</span><span class="sxs-lookup"><span data-stu-id="2e941-274">Disable register page</span></span>

<span data-ttu-id="2e941-275">若要停用使用者註冊：</span><span class="sxs-lookup"><span data-stu-id="2e941-275">To disable user registration:</span></span>

* <span data-ttu-id="2e941-276">Scaffold Identity 。</span><span class="sxs-lookup"><span data-stu-id="2e941-276">Scaffold Identity.</span></span> <span data-ttu-id="2e941-277">包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。</span><span class="sxs-lookup"><span data-stu-id="2e941-277">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="2e941-278">例如：</span><span class="sxs-lookup"><span data-stu-id="2e941-278">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="2e941-279">更新*區域/ Identity /Pages/Account/Register.cshtml.cs* ，讓使用者無法從這個端點註冊：</span><span class="sxs-lookup"><span data-stu-id="2e941-279">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="2e941-280">將*Areas/ Identity /Pages/Account/Register.cshtml*更新為與前述變更一致：</span><span class="sxs-lookup"><span data-stu-id="2e941-280">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="2e941-281">批註外或移除*區域/ Identity /Pages/Account/Login.cshtml*的註冊連結</span><span class="sxs-lookup"><span data-stu-id="2e941-281">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="2e941-282">更新 [*區域/ Identity /Pages/Account/RegisterConfirmation* ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="2e941-282">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="2e941-283">移除來自 cshtml 檔案的程式碼和連結。</span><span class="sxs-lookup"><span data-stu-id="2e941-283">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="2e941-284">從移除確認碼 `PageModel` ：</span><span class="sxs-lookup"><span data-stu-id="2e941-284">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="2e941-285">使用另一個應用程式來新增使用者</span><span class="sxs-lookup"><span data-stu-id="2e941-285">Use another app to add users</span></span>

<span data-ttu-id="2e941-286">提供在 web 應用程式外部新增使用者的機制。</span><span class="sxs-lookup"><span data-stu-id="2e941-286">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="2e941-287">新增使用者的選項包括：</span><span class="sxs-lookup"><span data-stu-id="2e941-287">Options to add users include:</span></span>

* <span data-ttu-id="2e941-288">專用的管理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2e941-288">A dedicated admin web app.</span></span>
* <span data-ttu-id="2e941-289">主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="2e941-289">A console app.</span></span>

<span data-ttu-id="2e941-290">下列程式碼概述新增使用者的一種方法：</span><span class="sxs-lookup"><span data-stu-id="2e941-290">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="2e941-291">使用者清單會讀入記憶體中。</span><span class="sxs-lookup"><span data-stu-id="2e941-291">A list of users is read into memory.</span></span>
* <span data-ttu-id="2e941-292">為每個使用者產生強式唯一密碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-292">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="2e941-293">使用者會新增至 Identity 資料庫。</span><span class="sxs-lookup"><span data-stu-id="2e941-293">The user is added to the Identity database.</span></span>
* <span data-ttu-id="2e941-294">系統會通知使用者，並告知您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="2e941-294">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="2e941-295">下列程式碼概述如何新增使用者：</span><span class="sxs-lookup"><span data-stu-id="2e941-295">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="2e941-296">針對生產案例，可以遵循類似的方法。</span><span class="sxs-lookup"><span data-stu-id="2e941-296">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2e941-297">其他資源</span><span class="sxs-lookup"><span data-stu-id="2e941-297">Additional resources</span></span>

* [<span data-ttu-id="2e941-298">將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="2e941-298">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end
