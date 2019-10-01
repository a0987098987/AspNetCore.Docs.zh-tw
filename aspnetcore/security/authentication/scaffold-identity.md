---
title: ASP.NET Core 專案中的 Scaffold 身分識別
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 專案中 scaffold 身分識別。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: f3ae089d344d95ed84c9720ab4ba2c697400901e
ms.sourcegitcommit: dc96d76f6b231de59586fcbb989a7fb5106d26a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703765"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="9ceab-103">ASP.NET Core 專案中的 Scaffold 身分識別</span><span class="sxs-lookup"><span data-stu-id="9ceab-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="9ceab-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9ceab-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9ceab-105">ASP.NET Core 2.1 和更新版本提供作為[Razor 類別庫](xref:razor-pages/ui-class)的[ASP.NET Core 身分識別](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="9ceab-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="9ceab-106">包含身分識別的應用程式可以套用 scaffolder，以選擇性地新增包含在身分識別 Razor 類別庫（RCL）中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ceab-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="9ceab-107">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="9ceab-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="9ceab-108">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ceab-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="9ceab-109">產生的程式碼優先於身分識別 RCL 中的相同程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ceab-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="9ceab-110">若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。</span><span class="sxs-lookup"><span data-stu-id="9ceab-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="9ceab-111">**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL 身分識別套件。</span><span class="sxs-lookup"><span data-stu-id="9ceab-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="9ceab-112">您可以選擇選取要產生的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ceab-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="9ceab-113">雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成此流程。</span><span class="sxs-lookup"><span data-stu-id="9ceab-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="9ceab-114">本檔說明完成身分識別架構更新所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="9ceab-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="9ceab-115">執行身分識別 scaffolder 時，會在專案目錄中建立*ScaffoldingReadme。*</span><span class="sxs-lookup"><span data-stu-id="9ceab-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="9ceab-116">*ScaffoldingReadme .txt*檔案包含完成身分識別架構更新所需事項的一般指示。</span><span class="sxs-lookup"><span data-stu-id="9ceab-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="9ceab-117">本檔包含比*ScaffoldingReadme*更完整的指示。</span><span class="sxs-lookup"><span data-stu-id="9ceab-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="9ceab-118">我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。</span><span class="sxs-lookup"><span data-stu-id="9ceab-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="9ceab-119">執行身分識別 scaffolder 之後，請檢查變更。</span><span class="sxs-lookup"><span data-stu-id="9ceab-119">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="9ceab-120">使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他具有身分識別的安全性功能時，都需要服務。</span><span class="sxs-lookup"><span data-stu-id="9ceab-120">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="9ceab-121">當基架構識別時，不會產生服務或服務存根。</span><span class="sxs-lookup"><span data-stu-id="9ceab-121">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="9ceab-122">啟用這些功能的服務必須手動新增。</span><span class="sxs-lookup"><span data-stu-id="9ceab-122">Services to enable these features must be added manually.</span></span> <span data-ttu-id="9ceab-123">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="9ceab-123">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="9ceab-124">將身分識別 Scaffold 到空的專案</span><span class="sxs-lookup"><span data-stu-id="9ceab-124">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="9ceab-125">將下列反白顯示的呼叫新增至 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="9ceab-125">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="9ceab-126">將身分識別 Scaffold 到不具現有授權的 Razor 專案</span><span class="sxs-lookup"><span data-stu-id="9ceab-126">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="9ceab-127">身分識別是在*Areas/identity/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="9ceab-127">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="9ceab-128">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="9ceab-128">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="9ceab-129">遷移、UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="9ceab-129">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="9ceab-130">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="9ceab-130">Enable authentication</span></span>

<span data-ttu-id="9ceab-131">在 `Startup` 類別的 `Configure` 方法中，在 `UseStaticFiles` 之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ：</span><span class="sxs-lookup"><span data-stu-id="9ceab-131">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="9ceab-132">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="9ceab-132">Layout changes</span></span>

<span data-ttu-id="9ceab-133">選擇性：將登入部分（`_LoginPartial`）新增至版面配置檔案：</span><span class="sxs-lookup"><span data-stu-id="9ceab-133">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="9ceab-134">使用授權將身分識別 Scaffold 到 Razor 專案</span><span class="sxs-lookup"><span data-stu-id="9ceab-134">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="9ceab-135">某些身分識別選項會在*區域/身分識別/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="9ceab-135">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="9ceab-136">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="9ceab-136">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="9ceab-137">將身分識別 Scaffold 至沒有現有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="9ceab-137">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="9ceab-138">選擇性：將登入部分（`_LoginPartial`）新增至*Views/Shared/_Layout. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="9ceab-138">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="9ceab-139">將*Pages/shared/_LoginPartial*檔案移至*Views/shared/_LoginPartial。 cshtml*</span><span class="sxs-lookup"><span data-stu-id="9ceab-139">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="9ceab-140">身分識別是在*Areas/identity/IdentityHostingStartup*中設定。</span><span class="sxs-lookup"><span data-stu-id="9ceab-140">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="9ceab-141">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="9ceab-141">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="9ceab-142">@No__t-1 之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ：</span><span class="sxs-lookup"><span data-stu-id="9ceab-142">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="9ceab-143">使用授權將身分識別 Scaffold 至 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="9ceab-143">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="9ceab-144">刪除*頁面/共用*資料夾，以及該資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9ceab-144">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="9ceab-145">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="9ceab-145">Create full identity UI source</span></span>

<span data-ttu-id="9ceab-146">若要維持身分識別 UI 的完全控制，請執行身分識別 scaffolder，然後選取 [覆**寫所有**檔案]。</span><span class="sxs-lookup"><span data-stu-id="9ceab-146">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="9ceab-147">下列反白顯示的程式碼顯示在 ASP.NET Core 2.1 web 應用程式中，將預設身分識別 UI 取代為身分識別的變更。</span><span class="sxs-lookup"><span data-stu-id="9ceab-147">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="9ceab-148">您可能想要執行此動作，以完全控制身分識別 UI。</span><span class="sxs-lookup"><span data-stu-id="9ceab-148">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="9ceab-149">下列程式碼會取代預設身分識別：</span><span class="sxs-lookup"><span data-stu-id="9ceab-149">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="9ceab-150">下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：</span><span class="sxs-lookup"><span data-stu-id="9ceab-150">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="9ceab-151">註冊 `IEmailSender` 的執行，例如：</span><span class="sxs-lookup"><span data-stu-id="9ceab-151">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="9ceab-152">其他資源</span><span class="sxs-lookup"><span data-stu-id="9ceab-152">Additional resources</span></span>

* [<span data-ttu-id="9ceab-153">將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本</span><span class="sxs-lookup"><span data-stu-id="9ceab-153">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)
