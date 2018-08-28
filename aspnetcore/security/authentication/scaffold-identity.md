---
title: ASP.NET Core 專案中的 scaffold 身分識別
author: rick-anderson
description: 了解如何建立 ASP.NET Core 專案中的身分識別。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: e35836fa9c20729da7c857243410833749b3a595
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055844"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="82d22-103">ASP.NET Core 專案中的 scaffold 身分識別</span><span class="sxs-lookup"><span data-stu-id="82d22-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="82d22-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="82d22-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="82d22-105">ASP.NET Core 2.1 和更新版本可[ASP.NET Core Identity](xref:security/authentication/identity)作為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="82d22-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="82d22-106">包含身分識別應用程式，可以套用到建構者選擇性地加入 包含在識別 Razor 類別庫 (RCL) 的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="82d22-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="82d22-107">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="82d22-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="82d22-108">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="82d22-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="82d22-109">產生的程式碼優先於身分識別 RCL 中的相同程式碼。</span><span class="sxs-lookup"><span data-stu-id="82d22-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="82d22-110">若要完整掌控 UI，並使用預設 RCL，請參閱下節[建立完整的身分識別 UI 來源](#full)。</span><span class="sxs-lookup"><span data-stu-id="82d22-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="82d22-111">執行的應用程式**不**包含驗證可以套用 scaffolder 新增 RCL 識別套件。</span><span class="sxs-lookup"><span data-stu-id="82d22-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="82d22-112">您可以選擇選取要產生的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="82d22-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="82d22-113">雖然框架產生大部分的必要的程式碼，您必須更新專案，以完成程序。</span><span class="sxs-lookup"><span data-stu-id="82d22-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="82d22-114">本文件說明完成識別 scaffolding 更新所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="82d22-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="82d22-115">執行身分識別框架時， *ScaffoldingReadme.txt*專案目錄中建立檔案。</span><span class="sxs-lookup"><span data-stu-id="82d22-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="82d22-116">*ScaffoldingReadme.txt*檔案包含需要的完成識別 scaffolding 更新內容的一般指示。</span><span class="sxs-lookup"><span data-stu-id="82d22-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="82d22-117">本文件包含更詳細的說明，比*ScaffoldingReadme.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="82d22-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="82d22-118">我們建議使用顯示檔案的差異，並可讓您將變更原始檔控制系統。</span><span class="sxs-lookup"><span data-stu-id="82d22-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="82d22-119">執行身分識別 scaffolder 之後檢查所做的變更。</span><span class="sxs-lookup"><span data-stu-id="82d22-119">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="82d22-120">使用時，服務不需要[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)，[帳戶確認和密碼復原](xref:security/authentication/accconfirm)，以及其他與身分識別的安全性功能。</span><span class="sxs-lookup"><span data-stu-id="82d22-120">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="82d22-121">Scaffolding 身分識別時，不會產生服務或服務虛設常式。</span><span class="sxs-lookup"><span data-stu-id="82d22-121">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="82d22-122">若要啟用這些功能的服務必須手動加入。</span><span class="sxs-lookup"><span data-stu-id="82d22-122">Services to enable these features must be added manually.</span></span> <span data-ttu-id="82d22-123">例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。</span><span class="sxs-lookup"><span data-stu-id="82d22-123">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="82d22-124">Scaffold 的身分識別至空專案</span><span class="sxs-lookup"><span data-stu-id="82d22-124">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="82d22-125">加入下列反白顯示的呼叫`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="82d22-125">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="82d22-126">Scaffold 的身分識別至 Razor 專案沒有現有的授權</span><span class="sxs-lookup"><span data-stu-id="82d22-126">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
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

<span data-ttu-id="82d22-127">已在中設定身分識別*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="82d22-127">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="82d22-128">如需詳細資訊，請參閱 < [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="82d22-128">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="82d22-129">移轉、 UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="82d22-129">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="82d22-130">啟用驗證</span><span class="sxs-lookup"><span data-stu-id="82d22-130">Enable authentication</span></span>

<span data-ttu-id="82d22-131">在 `Configure`方法`Startup`類別中，呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="82d22-131">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="82d22-132">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="82d22-132">Layout changes</span></span>

<span data-ttu-id="82d22-133">選擇性： 新增部分登入 (`_LoginPartial`) 和配置檔案：</span><span class="sxs-lookup"><span data-stu-id="82d22-133">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="82d22-134">Scaffold Razor 專案具有授權的身分識別</span><span class="sxs-lookup"><span data-stu-id="82d22-134">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="82d22-135">中所設定的一些身分識別選項*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="82d22-135">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="82d22-136">如需詳細資訊，請參閱 < [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="82d22-136">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="82d22-137">MVC 專案中現有的授權而成的 scaffold 身分識別</span><span class="sxs-lookup"><span data-stu-id="82d22-137">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="82d22-138">選擇性： 新增部分登入 (`_LoginPartial`) 來*Views/Shared/_Layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="82d22-138">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="82d22-139">移動*Pages/Shared/_LoginPartial.cshtml*檔案*Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="82d22-139">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="82d22-140">已在中設定身分識別*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="82d22-140">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="82d22-141">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="82d22-141">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="82d22-142">呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="82d22-142">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="82d22-143">Scaffold 的身分識別至具有授權的 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="82d22-143">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="82d22-144">刪除*頁/Shared*資料夾，然後在該資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="82d22-144">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="82d22-145">建立完整的身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="82d22-145">Create full identity UI source</span></span>

<span data-ttu-id="82d22-146">若要維護的身分識別使用者介面的完整控制權，執行身分識別的框架，然後選取**覆寫所有檔案**。</span><span class="sxs-lookup"><span data-stu-id="82d22-146">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="82d22-147">下列醒目提示的程式碼會顯示預設識別 UI 取代 ASP.NET Core 2.1 web 應用程式中的身分識別的變更。</span><span class="sxs-lookup"><span data-stu-id="82d22-147">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="82d22-148">您可能想要這樣做能夠完整控制身分識別 UI。</span><span class="sxs-lookup"><span data-stu-id="82d22-148">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="82d22-149">預設身分識別會取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="82d22-149">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="82d22-150">下列程式碼集[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)， [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)，並[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="82d22-150">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="82d22-151">註冊`IEmailSender`實作，例如：</span><span class="sxs-lookup"><span data-stu-id="82d22-151">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
