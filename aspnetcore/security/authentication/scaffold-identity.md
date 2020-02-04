---
title: ASP.NET Core 專案中的 Scaffold 身分識別
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 專案中 scaffold 身分識別。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: security/authentication/scaffold-identity
ms.openlocfilehash: b3e077aeac11e62d9e992884100476f7be35b59a
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2020
ms.locfileid: "76972037"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="49af9-103">ASP.NET Core 專案中的 Scaffold 身分識別</span><span class="sxs-lookup"><span data-stu-id="49af9-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="49af9-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49af9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="49af9-105">ASP.NET Core 提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="49af9-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="49af9-106">包含身分識別的應用程式可以套用 scaffolder，以選擇性地新增包含在身分識別 Razor 類別庫（RCL）中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="49af9-107">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="49af9-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="49af9-108">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="49af9-109">產生的程式碼優先於身分識別 RCL 中的相同程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="49af9-110">若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。</span><span class="sxs-lookup"><span data-stu-id="49af9-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="49af9-111">**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL 身分識別套件。</span><span class="sxs-lookup"><span data-stu-id="49af9-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="49af9-112">您可以選擇選取要產生的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="49af9-113">雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成流程。</span><span class="sxs-lookup"><span data-stu-id="49af9-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="49af9-114">本檔說明完成身分識別架構更新所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="49af9-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="49af9-115">我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。</span><span class="sxs-lookup"><span data-stu-id="49af9-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="49af9-116">執行身分識別 scaffolder 之後，請檢查變更。</span><span class="sxs-lookup"><span data-stu-id="49af9-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="49af9-117">使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他具有身分識別的安全性功能時，都需要服務。</span><span class="sxs-lookup"><span data-stu-id="49af9-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="49af9-118">當基架構識別時，不會產生服務或服務存根。</span><span class="sxs-lookup"><span data-stu-id="49af9-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="49af9-119">啟用這些功能的服務必須手動新增。</span><span class="sxs-lookup"><span data-stu-id="49af9-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="49af9-120">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="49af9-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="49af9-121">本檔包含的指示比執行 scaffolder 時所產生的 ScaffoldingReadme 更完整 *。*</span><span class="sxs-lookup"><span data-stu-id="49af9-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="49af9-122">將身分識別 Scaffold 到空的專案</span><span class="sxs-lookup"><span data-stu-id="49af9-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49af9-123">使用與下列類似的程式碼來更新 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="49af9-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="49af9-124">將身分識別 Scaffold 到不具現有授權的 Razor 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-124">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="49af9-125">身分識別是在*Areas/identity/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="49af9-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49af9-126">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="49af9-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="49af9-127">遷移、UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="49af9-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="49af9-128">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="49af9-128">Enable authentication</span></span>

<span data-ttu-id="49af9-129">使用與下列類似的程式碼來更新 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="49af9-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="49af9-130">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="49af9-130">Layout changes</span></span>

<span data-ttu-id="49af9-131">選擇性：將登入部分（`_LoginPartial`）新增至配置檔案：</span><span class="sxs-lookup"><span data-stu-id="49af9-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="49af9-132">使用授權將身分識別 Scaffold 到 Razor 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-132">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="49af9-133">某些身分識別選項會在*區域/身分識別/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="49af9-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49af9-134">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="49af9-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="49af9-135">將身分識別 Scaffold 至沒有現有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-135">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="49af9-136">選擇性：將登入部分（`_LoginPartial`）新增至*Views/Shared/_Layout. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="49af9-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="49af9-137">將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="49af9-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="49af9-138">身分識別是在*Areas/identity/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="49af9-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49af9-139">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="49af9-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="49af9-140">使用與下列類似的程式碼來更新 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="49af9-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="49af9-141">使用授權將身分識別 Scaffold 至 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="49af9-142">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="49af9-142">Create full identity UI source</span></span>

<span data-ttu-id="49af9-143">若要維持身分識別 UI 的完全控制，請執行身分識別 scaffolder，然後選取 [覆**寫所有**檔案]。</span><span class="sxs-lookup"><span data-stu-id="49af9-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="49af9-144">下列反白顯示的程式碼顯示在 ASP.NET Core 2.1 web 應用程式中，將預設身分識別 UI 取代為身分識別的變更。</span><span class="sxs-lookup"><span data-stu-id="49af9-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="49af9-145">您可能想要執行此動作，以完全控制身分識別 UI。</span><span class="sxs-lookup"><span data-stu-id="49af9-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="49af9-146">下列程式碼會取代預設身分識別：</span><span class="sxs-lookup"><span data-stu-id="49af9-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="49af9-147">下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：</span><span class="sxs-lookup"><span data-stu-id="49af9-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="49af9-148">註冊 `IEmailSender` 的執行，例如：</span><span class="sxs-lookup"><span data-stu-id="49af9-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="49af9-149">停用註冊頁面</span><span class="sxs-lookup"><span data-stu-id="49af9-149">Disable register page</span></span>

<span data-ttu-id="49af9-150">若要停用使用者註冊：</span><span class="sxs-lookup"><span data-stu-id="49af9-150">To disable user registration:</span></span>

* <span data-ttu-id="49af9-151">Scaffold 身分識別。</span><span class="sxs-lookup"><span data-stu-id="49af9-151">Scaffold Identity.</span></span> <span data-ttu-id="49af9-152">包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。</span><span class="sxs-lookup"><span data-stu-id="49af9-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="49af9-153">例如：</span><span class="sxs-lookup"><span data-stu-id="49af9-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="49af9-154">更新*Areas/Identity/Pages/Account/Register. cshtml* ，讓使用者無法從這個端點註冊：</span><span class="sxs-lookup"><span data-stu-id="49af9-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="49af9-155">更新*區域/身分識別/頁面/帳戶/* 暫存器，以與前述變更一致：</span><span class="sxs-lookup"><span data-stu-id="49af9-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="49af9-156">將註冊連結從*區域/身分識別/頁面/帳戶/登*入批註掉或移除</span><span class="sxs-lookup"><span data-stu-id="49af9-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="49af9-157">更新 [*區域/身分識別/頁面/帳戶/RegisterConfirmation* ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="49af9-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="49af9-158">移除來自 cshtml 檔案的程式碼和連結。</span><span class="sxs-lookup"><span data-stu-id="49af9-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="49af9-159">從 `PageModel`中移除確認碼：</span><span class="sxs-lookup"><span data-stu-id="49af9-159">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="49af9-160">使用另一個應用程式來新增使用者</span><span class="sxs-lookup"><span data-stu-id="49af9-160">Use another app to add users</span></span>

<span data-ttu-id="49af9-161">提供在 web 應用程式外部新增使用者的機制。</span><span class="sxs-lookup"><span data-stu-id="49af9-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="49af9-162">新增使用者的選項包括：</span><span class="sxs-lookup"><span data-stu-id="49af9-162">Options to add users include:</span></span>

* <span data-ttu-id="49af9-163">專用的管理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="49af9-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="49af9-164">主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="49af9-164">A console app.</span></span>

<span data-ttu-id="49af9-165">下列程式碼概述新增使用者的一種方法：</span><span class="sxs-lookup"><span data-stu-id="49af9-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="49af9-166">使用者清單會讀入記憶體中。</span><span class="sxs-lookup"><span data-stu-id="49af9-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="49af9-167">為每個使用者產生強式唯一密碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="49af9-168">使用者會新增至身分識別資料庫。</span><span class="sxs-lookup"><span data-stu-id="49af9-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="49af9-169">系統會通知使用者，並告知您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="49af9-170">下列程式碼概述如何新增使用者：</span><span class="sxs-lookup"><span data-stu-id="49af9-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="49af9-171">針對生產案例，可以遵循類似的方法。</span><span class="sxs-lookup"><span data-stu-id="49af9-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="49af9-172">防止發佈靜態身分識別資產</span><span class="sxs-lookup"><span data-stu-id="49af9-172">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="49af9-173">若要防止將靜態身分識別資產發行至 web 根目錄，請參閱 <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>。</span><span class="sxs-lookup"><span data-stu-id="49af9-173">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49af9-174">其他資源</span><span class="sxs-lookup"><span data-stu-id="49af9-174">Additional resources</span></span>

* [<span data-ttu-id="49af9-175">將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="49af9-175">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="49af9-176">ASP.NET Core 2.1 和更新版本提供作為[Razor 類別庫](xref:razor-pages/ui-class)的[ASP.NET Core 身分識別](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="49af9-176">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="49af9-177">包含身分識別的應用程式可以套用 scaffolder，以選擇性地新增包含在身分識別 Razor 類別庫（RCL）中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-177">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="49af9-178">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="49af9-178">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="49af9-179">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-179">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="49af9-180">產生的程式碼優先於身分識別 RCL 中的相同程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-180">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="49af9-181">若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。</span><span class="sxs-lookup"><span data-stu-id="49af9-181">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="49af9-182">**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL 身分識別套件。</span><span class="sxs-lookup"><span data-stu-id="49af9-182">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="49af9-183">您可以選擇選取要產生的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-183">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="49af9-184">雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成此流程。</span><span class="sxs-lookup"><span data-stu-id="49af9-184">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="49af9-185">本檔說明完成身分識別架構更新所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="49af9-185">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="49af9-186">執行身分識別 scaffolder 時，會在專案目錄中建立*ScaffoldingReadme。*</span><span class="sxs-lookup"><span data-stu-id="49af9-186">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="49af9-187">*ScaffoldingReadme .txt*檔案包含完成身分識別架構更新所需事項的一般指示。</span><span class="sxs-lookup"><span data-stu-id="49af9-187">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="49af9-188">本檔包含比*ScaffoldingReadme*更完整的指示。</span><span class="sxs-lookup"><span data-stu-id="49af9-188">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="49af9-189">我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。</span><span class="sxs-lookup"><span data-stu-id="49af9-189">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="49af9-190">執行身分識別 scaffolder 之後，請檢查變更。</span><span class="sxs-lookup"><span data-stu-id="49af9-190">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="49af9-191">使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他具有身分識別的安全性功能時，都需要服務。</span><span class="sxs-lookup"><span data-stu-id="49af9-191">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="49af9-192">當基架構識別時，不會產生服務或服務存根。</span><span class="sxs-lookup"><span data-stu-id="49af9-192">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="49af9-193">啟用這些功能的服務必須手動新增。</span><span class="sxs-lookup"><span data-stu-id="49af9-193">Services to enable these features must be added manually.</span></span> <span data-ttu-id="49af9-194">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="49af9-194">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="49af9-195">將身分識別 Scaffold 到空的專案</span><span class="sxs-lookup"><span data-stu-id="49af9-195">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49af9-196">將下列反白顯示的呼叫新增至 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="49af9-196">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="49af9-197">將身分識別 Scaffold 到不具現有授權的 Razor 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-197">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="49af9-198">身分識別是在*Areas/identity/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="49af9-198">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49af9-199">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="49af9-199">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="49af9-200">遷移、UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="49af9-200">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="49af9-201">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="49af9-201">Enable authentication</span></span>

<span data-ttu-id="49af9-202">在 `Startup` 類別的 `Configure` 方法中，在 `UseStaticFiles`之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ：</span><span class="sxs-lookup"><span data-stu-id="49af9-202">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="49af9-203">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="49af9-203">Layout changes</span></span>

<span data-ttu-id="49af9-204">選擇性：將登入部分（`_LoginPartial`）新增至配置檔案：</span><span class="sxs-lookup"><span data-stu-id="49af9-204">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="49af9-205">使用授權將身分識別 Scaffold 到 Razor 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-205">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="49af9-206">某些身分識別選項會在*區域/身分識別/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="49af9-206">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49af9-207">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="49af9-207">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="49af9-208">將身分識別 Scaffold 至沒有現有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-208">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="49af9-209">選擇性：將登入部分（`_LoginPartial`）新增至*Views/Shared/_Layout. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="49af9-209">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="49af9-210">將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="49af9-210">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="49af9-211">身分識別是在*Areas/identity/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="49af9-211">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49af9-212">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="49af9-212">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="49af9-213">`UseStaticFiles`之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ：</span><span class="sxs-lookup"><span data-stu-id="49af9-213">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="49af9-214">使用授權將身分識別 Scaffold 至 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="49af9-214">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="49af9-215">刪除*頁面/共用*資料夾，以及該資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="49af9-215">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="49af9-216">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="49af9-216">Create full identity UI source</span></span>

<span data-ttu-id="49af9-217">若要維持身分識別 UI 的完全控制，請執行身分識別 scaffolder，然後選取 [覆**寫所有**檔案]。</span><span class="sxs-lookup"><span data-stu-id="49af9-217">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="49af9-218">下列反白顯示的程式碼顯示在 ASP.NET Core 2.1 web 應用程式中，將預設身分識別 UI 取代為身分識別的變更。</span><span class="sxs-lookup"><span data-stu-id="49af9-218">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="49af9-219">您可能想要執行此動作，以完全控制身分識別 UI。</span><span class="sxs-lookup"><span data-stu-id="49af9-219">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="49af9-220">下列程式碼會取代預設身分識別：</span><span class="sxs-lookup"><span data-stu-id="49af9-220">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="49af9-221">下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：</span><span class="sxs-lookup"><span data-stu-id="49af9-221">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="49af9-222">註冊 `IEmailSender` 的執行，例如：</span><span class="sxs-lookup"><span data-stu-id="49af9-222">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="49af9-223">停用註冊頁面</span><span class="sxs-lookup"><span data-stu-id="49af9-223">Disable register page</span></span>

<span data-ttu-id="49af9-224">若要停用使用者註冊：</span><span class="sxs-lookup"><span data-stu-id="49af9-224">To disable user registration:</span></span>

* <span data-ttu-id="49af9-225">Scaffold 身分識別。</span><span class="sxs-lookup"><span data-stu-id="49af9-225">Scaffold Identity.</span></span> <span data-ttu-id="49af9-226">包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。</span><span class="sxs-lookup"><span data-stu-id="49af9-226">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="49af9-227">例如：</span><span class="sxs-lookup"><span data-stu-id="49af9-227">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="49af9-228">更新*Areas/Identity/Pages/Account/Register. cshtml* ，讓使用者無法從這個端點註冊：</span><span class="sxs-lookup"><span data-stu-id="49af9-228">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="49af9-229">更新*區域/身分識別/頁面/帳戶/* 暫存器，以與前述變更一致：</span><span class="sxs-lookup"><span data-stu-id="49af9-229">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="49af9-230">將註冊連結從*區域/身分識別/頁面/帳戶/登*入批註掉或移除</span><span class="sxs-lookup"><span data-stu-id="49af9-230">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="49af9-231">更新 [*區域/身分識別/頁面/帳戶/RegisterConfirmation* ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="49af9-231">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="49af9-232">移除來自 cshtml 檔案的程式碼和連結。</span><span class="sxs-lookup"><span data-stu-id="49af9-232">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="49af9-233">從 `PageModel`中移除確認碼：</span><span class="sxs-lookup"><span data-stu-id="49af9-233">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="49af9-234">使用另一個應用程式來新增使用者</span><span class="sxs-lookup"><span data-stu-id="49af9-234">Use another app to add users</span></span>

<span data-ttu-id="49af9-235">提供在 web 應用程式外部新增使用者的機制。</span><span class="sxs-lookup"><span data-stu-id="49af9-235">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="49af9-236">新增使用者的選項包括：</span><span class="sxs-lookup"><span data-stu-id="49af9-236">Options to add users include:</span></span>

* <span data-ttu-id="49af9-237">專用的管理 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="49af9-237">A dedicated admin web app.</span></span>
* <span data-ttu-id="49af9-238">主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="49af9-238">A console app.</span></span>

<span data-ttu-id="49af9-239">下列程式碼概述新增使用者的一種方法：</span><span class="sxs-lookup"><span data-stu-id="49af9-239">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="49af9-240">使用者清單會讀入記憶體中。</span><span class="sxs-lookup"><span data-stu-id="49af9-240">A list of users is read into memory.</span></span>
* <span data-ttu-id="49af9-241">為每個使用者產生強式唯一密碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-241">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="49af9-242">使用者會新增至身分識別資料庫。</span><span class="sxs-lookup"><span data-stu-id="49af9-242">The user is added to the Identity database.</span></span>
* <span data-ttu-id="49af9-243">系統會通知使用者，並告知您變更密碼。</span><span class="sxs-lookup"><span data-stu-id="49af9-243">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="49af9-244">下列程式碼概述如何新增使用者：</span><span class="sxs-lookup"><span data-stu-id="49af9-244">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="49af9-245">針對生產案例，可以遵循類似的方法。</span><span class="sxs-lookup"><span data-stu-id="49af9-245">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49af9-246">其他資源</span><span class="sxs-lookup"><span data-stu-id="49af9-246">Additional resources</span></span>

* [<span data-ttu-id="49af9-247">將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="49af9-247">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end