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
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Scaffold ASP.NET Core 專案中的身分識別

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

2.1 和更新版本的 ASP.NET Core 提供[ASP.NET Core 識別](xref:security/authentication/identity)為[Razor 類別庫](xref:mvc/razor-pages/ui-class)。 包含識別的應用程式可以套用 scaffolder，選擇性地將包含識別 Razor 類別程式庫 (RCL) 中的原始程式碼。 您可能想要產生原始程式碼，所以您可以修改程式碼，並變更行為。 例如，您也可以指示產生程式碼在註冊使用 scaffolder。 產生的程式碼會優先於相同的程式碼中識別 RCL。

應用程式**不**包含驗證可以套用 scaffolder 加入 RCL 識別封裝。 您可以選擇選取身分識別的程式碼產生。

雖然 scaffolder 會產生大部分的所需的程式碼，您必須更新專案，以完成程序。 本文件說明完成識別 scaffolding 更新所需的步驟。

識別 scaffolder 執行時， *ScaffoldingReadme.txt*專案目錄中建立檔案。 *ScaffoldingReadme.txt*檔案包含需要什麼才能完成識別 scaffolding 更新的一般指示。 本文件包含更詳細的說明，比讀取*ScaffoldingReadme.txt*檔案。

我們建議使用原始檔控制系統，顯示檔案的差異，並可讓您將頻外變更。 檢查執行身分識別 scaffolder 之後的變更。

## <a name="scaffold-identity-into-an-empty-project"></a>Scaffold 識別成空專案

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

加入下列反白顯示的呼叫`Startup`類別：

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Scaffold 識別到 Razor 專案沒有現有的授權

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

在設定的識別*Areas/Identity/IdentityHostingStartup.cs*。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>移轉、 UseAuthentication 和版面配置

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

在`Configure`方法`Startup`類別，請呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>版面配置變更

選擇性： 加入部分的登入 (`_LoginPartial`) 配置檔案：

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Scaffold 身分識別授權的 Razor 專案到

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
中設定某些身分識別選項*Areas/Identity/IdentityHostingStartup.cs*。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Scaffold 識別未經現有的授權為 MVC 專案

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

選擇性： 加入部分的登入 (`_LoginPartial`) 至*Views/Shared/_Layout.cshtml*檔案：

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* 移動*Pages/Shared/_LoginPartial.cshtml*檔案*Views/Shared/_LoginPartial.cshtml*

在設定的識別*Areas/Identity/IdentityHostingStartup.cs*。 如需詳細資訊，請參閱 IHostingStartup。

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

呼叫[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)之後`UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Scaffold 身分識別授權為 MVC 專案

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

刪除*頁面/Shared*資料夾和檔案，該資料夾中的。
