---
title: ASP.NET Core 專案中的 scaffold 身分識別
author: rick-anderson
description: 了解如何建立 ASP.NET Core 專案中的身分識別。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 7445db31e461bf61e8a91af7239187a6ece9d011
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897355"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>ASP.NET Core 專案中的 scaffold 身分識別

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 和更新版本可[ASP.NET Core Identity](xref:security/authentication/identity)作為[Razor 類別庫](xref:razor-pages/ui-class)。 包含身分識別應用程式，可以套用到建構者選擇性地加入 包含在識別 Razor 類別庫 (RCL) 的原始程式碼。 建議您產生原始程式碼，以便能夠修改程式碼並變更行為。 例如，您可以指示 Scaffolder 產生註冊使用的程式碼。 產生的程式碼優先於身分識別 RCL 中的相同程式碼。 若要完整掌控 UI，並使用預設 RCL，請參閱下節[建立完整的身分識別 UI 來源](#full)。

執行的應用程式**不**包含驗證可以套用 scaffolder 新增 RCL 識別套件。 您可以選擇選取要產生的身分識別程式碼。

雖然框架產生大部分的必要的程式碼，您必須更新專案，以完成程序。 本文件說明完成識別 scaffolding 更新所需的步驟。

執行身分識別框架時， *ScaffoldingReadme.txt*專案目錄中建立檔案。 *ScaffoldingReadme.txt*檔案包含需要的完成識別 scaffolding 更新內容的一般指示。 本文件包含更詳細的說明，比*ScaffoldingReadme.txt*檔案。

我們建議使用顯示檔案的差異，並可讓您將變更原始檔控制系統。 執行身分識別 scaffolder 之後檢查所做的變更。

> [!NOTE]
> 使用時，服務不需要[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)，[帳戶確認和密碼復原](xref:security/authentication/accconfirm)，以及其他與身分識別的安全性功能。 Scaffolding 身分識別時，不會產生服務或服務虛設常式。 若要啟用這些功能的服務必須手動加入。 例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。

## <a name="scaffold-identity-into-an-empty-project"></a>Scaffold 的身分識別至空專案

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

加入下列反白顯示的呼叫`Startup`類別：

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Scaffold 的身分識別至 Razor 專案沒有現有的授權

<!--
set projNam=RPnoAuth
set projType=webapp
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

已在中設定身分識別*Areas/Identity/IdentityHostingStartup.cs*。 如需詳細資訊，請參閱 < [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>移轉、 UseAuthentication 和版面配置

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>啟用驗證

在 `Configure`方法`Startup`類別中，呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>版面配置變更

選擇項：加入部分的登入 (`_LoginPartial`) 和配置檔案：

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Scaffold Razor 專案具有授權的身分識別

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
中所設定的一些身分識別選項*Areas/Identity/IdentityHostingStartup.cs*。 如需詳細資訊，請參閱 < [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>MVC 專案中現有的授權而成的 scaffold 身分識別

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

選擇項：加入部分的登入 (`_LoginPartial`) 來*Views/Shared/_Layout.cshtml*檔案：

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* 移動*Pages/Shared/_LoginPartial.cshtml*檔案*Views/Shared/_LoginPartial.cshtml*

已在中設定身分識別*Areas/Identity/IdentityHostingStartup.cs*。 如需詳細資訊，請參閱 IHostingStartup。

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Scaffold 的身分識別至具有授權的 MVC 專案

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

刪除*頁/Shared*資料夾，然後在該資料夾中的檔案。

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>建立完整的身分識別 UI 來源

若要維護的身分識別使用者介面的完整控制權，執行身分識別的框架，然後選取**覆寫所有檔案**。

下列醒目提示的程式碼會顯示預設識別 UI 取代 ASP.NET Core 2.1 web 應用程式中的身分識別的變更。 您可能想要這樣做能夠完整控制身分識別 UI。

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

預設身分識別會取代下列程式碼：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

下列程式碼集[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)， [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)，並[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

註冊`IEmailSender`實作，例如：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a>其他資源

* [變更用於 ASP.NET Core 2.1 和更新版本的驗證程式碼](xref:migration/20_21#changes-to-authentication-code)
