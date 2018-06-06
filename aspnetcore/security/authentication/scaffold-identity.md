---
title: Scaffold ASP.NET Core 專案中的身分識別
author: rick-anderson
description: 了解如何識別 scaffold ASP.NET Core 專案中。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 8933cf9c4063bd94f7f3a9ba53b5372b443eb7c8
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758748"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="4002e-103">Scaffold ASP.NET Core 專案中的身分識別</span><span class="sxs-lookup"><span data-stu-id="4002e-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="4002e-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4002e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4002e-105">2.1 和更新版本的 ASP.NET Core 提供[ASP.NET Core 識別](xref:security/authentication/identity)為[Razor 類別庫](xref:mvc/razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="4002e-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="4002e-106">包含識別的應用程式可以套用 scaffolder，選擇性地將包含識別 Razor 類別程式庫 (RCL) 中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="4002e-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="4002e-107">您可能想要產生原始程式碼，所以您可以修改程式碼，並變更行為。</span><span class="sxs-lookup"><span data-stu-id="4002e-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="4002e-108">例如，您也可以指示產生程式碼在註冊使用 scaffolder。</span><span class="sxs-lookup"><span data-stu-id="4002e-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="4002e-109">產生的程式碼會優先於相同的程式碼中識別 RCL。</span><span class="sxs-lookup"><span data-stu-id="4002e-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="4002e-110">應用程式**不**包含驗證可以套用 scaffolder 加入 RCL 識別封裝。</span><span class="sxs-lookup"><span data-stu-id="4002e-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="4002e-111">您可以選擇選取身分識別的程式碼產生。</span><span class="sxs-lookup"><span data-stu-id="4002e-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="4002e-112">雖然 scaffolder 會產生大部分的所需的程式碼，您必須更新專案，以完成程序。</span><span class="sxs-lookup"><span data-stu-id="4002e-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="4002e-113">本文件說明完成識別 scaffolding 更新所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="4002e-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="4002e-114">識別 scaffolder 執行時， *ScaffoldingReadme.txt*專案目錄中建立檔案。</span><span class="sxs-lookup"><span data-stu-id="4002e-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="4002e-115">*ScaffoldingReadme.txt*檔案包含需要什麼才能完成識別 scaffolding 更新的一般指示。</span><span class="sxs-lookup"><span data-stu-id="4002e-115">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="4002e-116">本文件包含更詳細的說明，比讀取*ScaffoldingReadme.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="4002e-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="4002e-117">我們建議使用原始檔控制系統，顯示檔案的差異，並可讓您將頻外變更。</span><span class="sxs-lookup"><span data-stu-id="4002e-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="4002e-118">檢查執行身分識別 scaffolder 之後的變更。</span><span class="sxs-lookup"><span data-stu-id="4002e-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="4002e-119">Scaffold 識別成空專案</span><span class="sxs-lookup"><span data-stu-id="4002e-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="4002e-120">加入下列反白顯示的呼叫`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="4002e-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="4002e-121">Scaffold 識別到 Razor 專案沒有現有的授權</span><span class="sxs-lookup"><span data-stu-id="4002e-121">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="4002e-122">在設定的識別*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="4002e-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="4002e-123">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="4002e-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="4002e-124">移轉、 UseAuthentication 和版面配置</span><span class="sxs-lookup"><span data-stu-id="4002e-124">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="4002e-125">在`Configure`方法`Startup`類別，請呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="4002e-125">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="4002e-126">版面配置變更</span><span class="sxs-lookup"><span data-stu-id="4002e-126">Layout changes</span></span>

<span data-ttu-id="4002e-127">選擇性： 加入部分的登入 (`_LoginPartial`) 配置檔案：</span><span class="sxs-lookup"><span data-stu-id="4002e-127">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="4002e-128">Scaffold 身分識別授權的 Razor 專案到</span><span class="sxs-lookup"><span data-stu-id="4002e-128">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="4002e-129">中設定某些身分識別選項*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="4002e-129">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="4002e-130">如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="4002e-130">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="4002e-131">Scaffold 識別未經現有的授權為 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="4002e-131">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="4002e-132">選擇性： 加入部分的登入 (`_LoginPartial`) 至*Views/Shared/_Layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="4002e-132">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="4002e-133">移動*Pages/Shared/_LoginPartial.cshtml*檔案*Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4002e-133">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="4002e-134">在設定的識別*Areas/Identity/IdentityHostingStartup.cs*。</span><span class="sxs-lookup"><span data-stu-id="4002e-134">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="4002e-135">如需詳細資訊，請參閱 IHostingStartup。</span><span class="sxs-lookup"><span data-stu-id="4002e-135">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="4002e-136">呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="4002e-136">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="4002e-137">Scaffold 身分識別授權為 MVC 專案</span><span class="sxs-lookup"><span data-stu-id="4002e-137">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="4002e-138">刪除*頁面/Shared*資料夾和檔案，該資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="4002e-138">Delete the *Pages/Shared* folder and the files in that folder.</span></span>
