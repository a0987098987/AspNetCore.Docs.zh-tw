---
title: Scaffold ASP.NET Core 專案中的身分識別
author: rick-anderson
description: 了解如何識別 scaffold ASP.NET Core 專案中。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276314"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="be0ec-103">Scaffold ASP.NET Core 專案中的身分識別</span><span class="sxs-lookup"><span data-stu-id="be0ec-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="be0ec-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="be0ec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="be0ec-105">2.1 和更新版本的 ASP.NET Core 提供[ASP.NET Core 識別](xref:security/authentication/identity)為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="be0ec-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="be0ec-106">包含識別的應用程式可以套用 scaffolder，選擇性地將包含識別 Razor 類別程式庫 (RCL) 中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="be0ec-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="be0ec-107">您可能想要產生原始程式碼，所以您可以修改程式碼，並變更行為。</span><span class="sxs-lookup"><span data-stu-id="be0ec-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="be0ec-108">例如，您也可以指示產生程式碼在註冊使用 scaffolder。</span><span class="sxs-lookup"><span data-stu-id="be0ec-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="be0ec-109">產生的程式碼會優先於相同的程式碼中識別 RCL。</span><span class="sxs-lookup"><span data-stu-id="be0ec-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="be0ec-110">UI 的完整控制權，並使用預設 RCL，請參閱下節[建立完整的身分識別 UI 來源](#full)。</span><span class="sxs-lookup"><span data-stu-id="be0ec-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="be0ec-111">應用程式**不**包含驗證可以套用 scaffolder 加入 RCL 識別封裝。</span><span class="sxs-lookup"><span data-stu-id="be0ec-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="be0ec-112">您可以選擇選取身分識別的程式碼產生。</span><span class="sxs-lookup"><span data-stu-id="be0ec-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="be0ec-113">雖然 scaffolder 會產生大部分的所需的程式碼，您必須更新專案，以完成程序。</span><span class="sxs-lookup"><span data-stu-id="be0ec-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="be0ec-114">本文件說明完成識別 scaffolding 更新所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="be0ec-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="be0ec-115">識別 scaffolder 執行時， *ScaffoldingReadme.txt*專案目錄中建立檔案。</span><span class="sxs-lookup"><span data-stu-id="be0ec-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="be0ec-116">*ScaffoldingReadme.txt*檔案包含需要什麼才能完成識別 scaffolding 更新的一般指示。</span><span class="sxs-lookup"><span data-stu-id="be0ec-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="be0ec-117">本文件包含更詳細的說明，比*ScaffoldingReadme.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="be0ec-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="be0ec-118">我們建議使用原始檔控制系統，顯示檔案的差異，並可讓您將頻外變更。</span><span class="sxs-lookup"><span data-stu-id="be0ec-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="be0ec-119">檢查執行身分識別 scaffolder 之後的變更。</span><span class="sxs-lookup"><span data-stu-id="be0ec-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="be0ec-120">Scaffold 識別成空專案</span><span class="sxs-lookup"><span data-stu-id="be0ec-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="be0ec-121">加入下列反白顯示的呼叫`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="be0ec-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="be0ec-122">Scaffold 識別到 Razor 專案沒有現有的授權</span><span class="sxs-lookup"><span data-stu-id="be0ec-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="be0ec-123">在設定的識別*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="be0ec-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="be0ec-124">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="be0ec-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="be0ec-125">移轉、 UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="be0ec-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="be0ec-126">在`Configure`方法`Startup`類別，請呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="be0ec-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="be0ec-127">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="be0ec-127">Layout changes</span></span>

<span data-ttu-id="be0ec-128">選擇性： 加入部分的登入 (`_LoginPartial`) 配置檔案：</span><span class="sxs-lookup"><span data-stu-id="be0ec-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="be0ec-129">Scaffold 身分識別授權的 Razor 專案到</span><span class="sxs-lookup"><span data-stu-id="be0ec-129">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="be0ec-130">中設定某些身分識別選項*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="be0ec-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="be0ec-131">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="be0ec-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="be0ec-132">Scaffold 識別未經現有的授權為 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="be0ec-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="be0ec-133">選擇性： 加入部分的登入 (`_LoginPartial`) 至*Views/Shared/_Layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="be0ec-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="be0ec-134">移動*Pages/Shared/_LoginPartial.cshtml*檔案*Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="be0ec-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="be0ec-135">在設定的識別*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="be0ec-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="be0ec-136">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="be0ec-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="be0ec-137">呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="be0ec-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="be0ec-138">Scaffold 身分識別授權為 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="be0ec-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="be0ec-139">刪除*頁面/Shared*資料夾和檔案，該資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="be0ec-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="be0ec-140">建立完整的身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="be0ec-140">Create full identity UI source</span></span>

<span data-ttu-id="be0ec-141">若要維護的身分識別使用者介面的完整控制權，執行身分識別 scaffolder 並選取**覆寫所有檔案**。</span><span class="sxs-lookup"><span data-stu-id="be0ec-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="be0ec-142">下列反白顯示的程式碼會顯示預設識別 UI 取代 ASP.NET Core 2.1 web 應用程式中的身分識別的變更。</span><span class="sxs-lookup"><span data-stu-id="be0ec-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="be0ec-143">您可能想要這樣做可以具有的身分識別使用者介面的完整控制權。</span><span class="sxs-lookup"><span data-stu-id="be0ec-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="be0ec-144">下列程式碼取代預設身分識別的：</span><span class="sxs-lookup"><span data-stu-id="be0ec-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="be0ec-145">下列程式碼集[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)， [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)，和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="be0ec-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="be0ec-146">註冊`IEmailSender`實作，例如：</span><span class="sxs-lookup"><span data-stu-id="be0ec-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
