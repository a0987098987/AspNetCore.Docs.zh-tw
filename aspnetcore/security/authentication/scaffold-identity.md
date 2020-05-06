---
title: ASP.NET Core Identity專案中的 Scaffold
author: rick-anderson
description: 瞭解如何在 ASP.NET Core Identity專案中 scaffold。
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
ms.openlocfilehash: 6f1ff69863e14c73e90496ea61188387f5267b19
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768386"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="f831c-103">ASP.NET Core Identity專案中的 Scaffold</span><span class="sxs-lookup"><span data-stu-id="f831c-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="f831c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f831c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f831c-105">ASP.NET Core 提供[ASP.NET Core Identity ](xref:security/authentication/identity)做為[ Razor類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="f831c-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="f831c-106">包含Identity的應用程式可以套用 scaffolder，以選擇性地新增包含在Identity Razor類別庫（RCL）中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="f831c-107">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="f831c-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="f831c-108">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="f831c-109">產生的程式碼優先于Identity RCL 中的相同程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="f831c-110">若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。</span><span class="sxs-lookup"><span data-stu-id="f831c-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="f831c-111">**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL Identity套件。</span><span class="sxs-lookup"><span data-stu-id="f831c-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="f831c-112">您可以選擇Identity要產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="f831c-113">雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成流程。</span><span class="sxs-lookup"><span data-stu-id="f831c-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="f831c-114">本檔說明完成「 Identity基架構更新」所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="f831c-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="f831c-115">我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。</span><span class="sxs-lookup"><span data-stu-id="f831c-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="f831c-116">執行Identity scaffolder 後檢查變更。</span><span class="sxs-lookup"><span data-stu-id="f831c-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="f831c-117">使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他安全性功能時，都需要服務Identity。</span><span class="sxs-lookup"><span data-stu-id="f831c-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="f831c-118">當樣板時Identity，不會產生服務或服務存根。</span><span class="sxs-lookup"><span data-stu-id="f831c-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="f831c-119">啟用這些功能的服務必須手動新增。</span><span class="sxs-lookup"><span data-stu-id="f831c-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="f831c-120">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="f831c-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="f831c-121">當Identity具有新資料內容的樣板加入具有現有個別帳戶的專案時：</span><span class="sxs-lookup"><span data-stu-id="f831c-121">When scaffolding Identity with a new data context into a project with existing individual accounts:</span></span>

* <span data-ttu-id="f831c-122">在`Startup.ConfigureServices`中，移除對的呼叫：</span><span class="sxs-lookup"><span data-stu-id="f831c-122">In `Startup.ConfigureServices`, remove the calls to:</span></span>
  * `AddDbContext`
  * `AddDefaultIdentity`

<span data-ttu-id="f831c-123">例如， `AddDbContext`和`AddDefaultIdentity`會在下列程式碼中加上批註：</span><span class="sxs-lookup"><span data-stu-id="f831c-123">For example, `AddDbContext` and `AddDefaultIdentity` are commented out in the following code:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRemove.cs?name=snippet)]

<span data-ttu-id="f831c-124">前面的程式碼會批註*區域/Identity/IdentityHostingStartup.cs*中重複的程式碼</span><span class="sxs-lookup"><span data-stu-id="f831c-124">The preceeding code comments out the code that is duplicated in *Areas/Identity/IdentityHostingStartup.cs*</span></span>

<span data-ttu-id="f831c-125">一般而言，使用個別帳戶建立的應用程式***不***應該建立新的資料內容。</span><span class="sxs-lookup"><span data-stu-id="f831c-125">Typically, apps that were created with individual accounts should ***not*** create a new data context.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="f831c-126">將身分識別 Scaffold 到空的專案</span><span class="sxs-lookup"><span data-stu-id="f831c-126">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="f831c-127">使用與`Startup`下列類似的程式碼來更新類別：</span><span class="sxs-lookup"><span data-stu-id="f831c-127">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="f831c-128">將身分識別 Scaffold Razor到沒有現有授權的專案</span><span class="sxs-lookup"><span data-stu-id="f831c-128">Scaffold identity into a Razor project without existing authorization</span></span>

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

Identity<span data-ttu-id="f831c-129">會在*區域/Identity/IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="f831c-129"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="f831c-130">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="f831c-130">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="f831c-131">遷移、UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="f831c-131">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="f831c-132">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="f831c-132">Enable authentication</span></span>

<span data-ttu-id="f831c-133">使用與`Startup`下列類似的程式碼來更新類別：</span><span class="sxs-lookup"><span data-stu-id="f831c-133">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="f831c-134">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="f831c-134">Layout changes</span></span>

<span data-ttu-id="f831c-135">選擇性：將登入部分（`_LoginPartial`）新增至配置檔案：</span><span class="sxs-lookup"><span data-stu-id="f831c-135">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="f831c-136">使用授權將Razor身分識別 Scaffold 至專案</span><span class="sxs-lookup"><span data-stu-id="f831c-136">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="f831c-137">有些Identity選項是在*區域/Identity/IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="f831c-137">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="f831c-138">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="f831c-138">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="f831c-139">將身分識別 Scaffold 至沒有現有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="f831c-139">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="f831c-140">選擇性：將登入部分（`_LoginPartial`）新增至*Views/Shared/_Layout. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="f831c-140">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="f831c-141">將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="f831c-141">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

Identity<span data-ttu-id="f831c-142">會在*區域/Identity/IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="f831c-142"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="f831c-143">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="f831c-143">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="f831c-144">使用與`Startup`下列類似的程式碼來更新類別：</span><span class="sxs-lookup"><span data-stu-id="f831c-144">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="f831c-145">使用授權將身分識別 Scaffold 至 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="f831c-145">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="f831c-146">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="f831c-146">Create full identity UI source</span></span>

<span data-ttu-id="f831c-147">若要維持Identity UI 的完全控制，請執行Identity scaffolder，然後選取 [覆**寫所有**檔案]。</span><span class="sxs-lookup"><span data-stu-id="f831c-147">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="f831c-148">下列反白顯示的程式碼顯示在 ASP.NET Core 2.1 Identity web 應用Identity程式中，將預設 UI 取代為的變更。</span><span class="sxs-lookup"><span data-stu-id="f831c-148">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="f831c-149">您可能想要這麼做，以擁有Identity UI 的完全控制權。</span><span class="sxs-lookup"><span data-stu-id="f831c-149">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="f831c-150">下列程式Identity代碼會取代預設值：</span><span class="sxs-lookup"><span data-stu-id="f831c-150">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="f831c-151">下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：</span><span class="sxs-lookup"><span data-stu-id="f831c-151">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="f831c-152">`IEmailSender`註冊執行，例如：</span><span class="sxs-lookup"><span data-stu-id="f831c-152">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="f831c-153">停用註冊頁面</span><span class="sxs-lookup"><span data-stu-id="f831c-153">Disable register page</span></span>

<span data-ttu-id="f831c-154">若要停用使用者註冊：</span><span class="sxs-lookup"><span data-stu-id="f831c-154">To disable user registration:</span></span>

* <span data-ttu-id="f831c-155">Scaffold Identity。</span><span class="sxs-lookup"><span data-stu-id="f831c-155">Scaffold Identity.</span></span> <span data-ttu-id="f831c-156">包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。</span><span class="sxs-lookup"><span data-stu-id="f831c-156">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="f831c-157">例如：</span><span class="sxs-lookup"><span data-stu-id="f831c-157">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="f831c-158">更新*區域/Identity/Pages/Account/Register.cshtml.cs* ，讓使用者無法從這個端點註冊：</span><span class="sxs-lookup"><span data-stu-id="f831c-158">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="f831c-159">將*Areas/Identity/Pages/Account/Register.cshtml*更新為與前述變更一致：</span><span class="sxs-lookup"><span data-stu-id="f831c-159">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="f831c-160">批註外或移除*區域/Identity/Pages/Account/Login.cshtml*的註冊連結</span><span class="sxs-lookup"><span data-stu-id="f831c-160">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="f831c-161">更新 [*區域/Identity/Pages/Account/RegisterConfirmation* ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f831c-161">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="f831c-162">移除來自 cshtml 檔案的程式碼和連結。</span><span class="sxs-lookup"><span data-stu-id="f831c-162">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="f831c-163">從移除確認碼`PageModel`：</span><span class="sxs-lookup"><span data-stu-id="f831c-163">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="f831c-164">使用另一個應用程式來新增使用者</span><span class="sxs-lookup"><span data-stu-id="f831c-164">Use another app to add users</span></span>

<span data-ttu-id="f831c-165">提供在 web 應用程式外部新增使用者的機制。</span><span class="sxs-lookup"><span data-stu-id="f831c-165">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="f831c-166">新增使用者的選項包括：</span><span class="sxs-lookup"><span data-stu-id="f831c-166">Options to add users include:</span></span>

* <span data-ttu-id="f831c-167">專用的管理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f831c-167">A dedicated admin web app.</span></span>
* <span data-ttu-id="f831c-168">主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="f831c-168">A console app.</span></span>

<span data-ttu-id="f831c-169">下列程式碼概述新增使用者的一種方法：</span><span class="sxs-lookup"><span data-stu-id="f831c-169">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="f831c-170">使用者清單會讀入記憶體中。</span><span class="sxs-lookup"><span data-stu-id="f831c-170">A list of users is read into memory.</span></span>
* <span data-ttu-id="f831c-171">為每個使用者產生強式唯一密碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-171">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="f831c-172">使用者會新增至Identity資料庫。</span><span class="sxs-lookup"><span data-stu-id="f831c-172">The user is added to the Identity database.</span></span>
* <span data-ttu-id="f831c-173">系統會通知使用者，並告知您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-173">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="f831c-174">下列程式碼概述如何新增使用者：</span><span class="sxs-lookup"><span data-stu-id="f831c-174">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="f831c-175">針對生產案例，可以遵循類似的方法。</span><span class="sxs-lookup"><span data-stu-id="f831c-175">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="f831c-176">防止發行靜態Identity資產</span><span class="sxs-lookup"><span data-stu-id="f831c-176">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="f831c-177">若要防止將Identity靜態資產發行至 web 根目錄， <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>請參閱。</span><span class="sxs-lookup"><span data-stu-id="f831c-177">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f831c-178">其他資源</span><span class="sxs-lookup"><span data-stu-id="f831c-178">Additional resources</span></span>

* [<span data-ttu-id="f831c-179">將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="f831c-179">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f831c-180">ASP.NET Core 2.1 和更新版本[提供Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="f831c-180">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="f831c-181">包含Identity的應用程式可以套用 scaffolder，以選擇性地新增包含在Identity Razor類別庫（RCL）中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-181">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="f831c-182">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="f831c-182">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="f831c-183">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-183">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="f831c-184">產生的程式碼優先于Identity RCL 中的相同程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-184">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="f831c-185">若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。</span><span class="sxs-lookup"><span data-stu-id="f831c-185">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="f831c-186">**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL Identity套件。</span><span class="sxs-lookup"><span data-stu-id="f831c-186">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="f831c-187">您可以選擇Identity要產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-187">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="f831c-188">雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成此流程。</span><span class="sxs-lookup"><span data-stu-id="f831c-188">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="f831c-189">本檔說明完成「 Identity基架構更新」所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="f831c-189">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="f831c-190">執行Identity scaffolder 時，會在專案目錄中建立*ScaffoldingReadme。*</span><span class="sxs-lookup"><span data-stu-id="f831c-190">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="f831c-191">*ScaffoldingReadme .txt*檔案包含完成Identity樣板更新所需的一般指示。</span><span class="sxs-lookup"><span data-stu-id="f831c-191">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="f831c-192">本檔包含比*ScaffoldingReadme*更完整的指示。</span><span class="sxs-lookup"><span data-stu-id="f831c-192">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="f831c-193">我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。</span><span class="sxs-lookup"><span data-stu-id="f831c-193">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="f831c-194">執行Identity scaffolder 後檢查變更。</span><span class="sxs-lookup"><span data-stu-id="f831c-194">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="f831c-195">使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他安全性功能時，都需要服務Identity。</span><span class="sxs-lookup"><span data-stu-id="f831c-195">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="f831c-196">當樣板時Identity，不會產生服務或服務存根。</span><span class="sxs-lookup"><span data-stu-id="f831c-196">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="f831c-197">啟用這些功能的服務必須手動新增。</span><span class="sxs-lookup"><span data-stu-id="f831c-197">Services to enable these features must be added manually.</span></span> <span data-ttu-id="f831c-198">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="f831c-198">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="f831c-199">將身分識別 Scaffold 到空的專案</span><span class="sxs-lookup"><span data-stu-id="f831c-199">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="f831c-200">將下列反白顯示的呼叫`Startup`新增至類別：</span><span class="sxs-lookup"><span data-stu-id="f831c-200">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="f831c-201">將身分識別 Scaffold Razor到沒有現有授權的專案</span><span class="sxs-lookup"><span data-stu-id="f831c-201">Scaffold identity into a Razor project without existing authorization</span></span>

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

Identity<span data-ttu-id="f831c-202">會在*區域/Identity/IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="f831c-202"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="f831c-203">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="f831c-203">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="f831c-204">遷移、UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="f831c-204">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="f831c-205">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="f831c-205">Enable authentication</span></span>

<span data-ttu-id="f831c-206">`Startup`在類別`Configure`的方法中，于之後[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) `UseStaticFiles`呼叫 UseAuthentication：</span><span class="sxs-lookup"><span data-stu-id="f831c-206">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="f831c-207">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="f831c-207">Layout changes</span></span>

<span data-ttu-id="f831c-208">選擇性：將登入部分（`_LoginPartial`）新增至配置檔案：</span><span class="sxs-lookup"><span data-stu-id="f831c-208">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="f831c-209">使用授權將Razor身分識別 Scaffold 至專案</span><span class="sxs-lookup"><span data-stu-id="f831c-209">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="f831c-210">有些Identity選項是在*區域/Identity/IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="f831c-210">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="f831c-211">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="f831c-211">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="f831c-212">將身分識別 Scaffold 至沒有現有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="f831c-212">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="f831c-213">選擇性：將登入部分（`_LoginPartial`）新增至*Views/Shared/_Layout. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="f831c-213">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="f831c-214">將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="f831c-214">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

Identity<span data-ttu-id="f831c-215">會在*區域/Identity/IdentityHostingStartup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="f831c-215"> is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="f831c-216">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="f831c-216">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="f831c-217">在[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`呼叫 UseAuthentication：</span><span class="sxs-lookup"><span data-stu-id="f831c-217">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="f831c-218">使用授權將身分識別 Scaffold 至 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="f831c-218">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="f831c-219">刪除*頁面/共用*資料夾，以及該資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="f831c-219">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="f831c-220">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="f831c-220">Create full identity UI source</span></span>

<span data-ttu-id="f831c-221">若要維持Identity UI 的完全控制，請執行Identity scaffolder，然後選取 [覆**寫所有**檔案]。</span><span class="sxs-lookup"><span data-stu-id="f831c-221">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="f831c-222">下列反白顯示的程式碼顯示在 ASP.NET Core 2.1 Identity web 應用Identity程式中，將預設 UI 取代為的變更。</span><span class="sxs-lookup"><span data-stu-id="f831c-222">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="f831c-223">您可能想要這麼做，以擁有Identity UI 的完全控制權。</span><span class="sxs-lookup"><span data-stu-id="f831c-223">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="f831c-224">下列程式Identity代碼會取代預設值：</span><span class="sxs-lookup"><span data-stu-id="f831c-224">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="f831c-225">下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：</span><span class="sxs-lookup"><span data-stu-id="f831c-225">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="f831c-226">`IEmailSender`註冊執行，例如：</span><span class="sxs-lookup"><span data-stu-id="f831c-226">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="f831c-227">停用註冊頁面</span><span class="sxs-lookup"><span data-stu-id="f831c-227">Disable register page</span></span>

<span data-ttu-id="f831c-228">若要停用使用者註冊：</span><span class="sxs-lookup"><span data-stu-id="f831c-228">To disable user registration:</span></span>

* <span data-ttu-id="f831c-229">Scaffold Identity。</span><span class="sxs-lookup"><span data-stu-id="f831c-229">Scaffold Identity.</span></span> <span data-ttu-id="f831c-230">包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。</span><span class="sxs-lookup"><span data-stu-id="f831c-230">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="f831c-231">例如：</span><span class="sxs-lookup"><span data-stu-id="f831c-231">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="f831c-232">更新*區域/Identity/Pages/Account/Register.cshtml.cs* ，讓使用者無法從這個端點註冊：</span><span class="sxs-lookup"><span data-stu-id="f831c-232">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="f831c-233">將*Areas/Identity/Pages/Account/Register.cshtml*更新為與前述變更一致：</span><span class="sxs-lookup"><span data-stu-id="f831c-233">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="f831c-234">批註外或移除*區域/Identity/Pages/Account/Login.cshtml*的註冊連結</span><span class="sxs-lookup"><span data-stu-id="f831c-234">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="f831c-235">更新 [*區域/Identity/Pages/Account/RegisterConfirmation* ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f831c-235">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="f831c-236">移除來自 cshtml 檔案的程式碼和連結。</span><span class="sxs-lookup"><span data-stu-id="f831c-236">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="f831c-237">從移除確認碼`PageModel`：</span><span class="sxs-lookup"><span data-stu-id="f831c-237">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="f831c-238">使用另一個應用程式來新增使用者</span><span class="sxs-lookup"><span data-stu-id="f831c-238">Use another app to add users</span></span>

<span data-ttu-id="f831c-239">提供在 web 應用程式外部新增使用者的機制。</span><span class="sxs-lookup"><span data-stu-id="f831c-239">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="f831c-240">新增使用者的選項包括：</span><span class="sxs-lookup"><span data-stu-id="f831c-240">Options to add users include:</span></span>

* <span data-ttu-id="f831c-241">專用的管理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f831c-241">A dedicated admin web app.</span></span>
* <span data-ttu-id="f831c-242">主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="f831c-242">A console app.</span></span>

<span data-ttu-id="f831c-243">下列程式碼概述新增使用者的一種方法：</span><span class="sxs-lookup"><span data-stu-id="f831c-243">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="f831c-244">使用者清單會讀入記憶體中。</span><span class="sxs-lookup"><span data-stu-id="f831c-244">A list of users is read into memory.</span></span>
* <span data-ttu-id="f831c-245">為每個使用者產生強式唯一密碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-245">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="f831c-246">使用者會新增至Identity資料庫。</span><span class="sxs-lookup"><span data-stu-id="f831c-246">The user is added to the Identity database.</span></span>
* <span data-ttu-id="f831c-247">系統會通知使用者，並告知您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="f831c-247">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="f831c-248">下列程式碼概述如何新增使用者：</span><span class="sxs-lookup"><span data-stu-id="f831c-248">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="f831c-249">針對生產案例，可以遵循類似的方法。</span><span class="sxs-lookup"><span data-stu-id="f831c-249">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f831c-250">其他資源</span><span class="sxs-lookup"><span data-stu-id="f831c-250">Additional resources</span></span>

* [<span data-ttu-id="f831c-251">將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="f831c-251">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end