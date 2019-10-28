---
title: ASP.NET Core 專案中的 Scaffold 身分識別
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 專案中 scaffold 身分識別。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: ca2046563281efc3c1cd8f4fec73fe4f8d3fbdda
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816096"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>ASP.NET Core 專案中的 Scaffold 身分識別

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

ASP.NET Core 2.1 和更新版本提供作為[Razor 類別庫](xref:razor-pages/ui-class)的[ASP.NET Core 身分識別](xref:security/authentication/identity)。 包含身分識別的應用程式可以套用 scaffolder，以選擇性地新增包含在身分識別 Razor 類別庫（RCL）中的原始程式碼。 建議您產生原始程式碼，以便能夠修改程式碼並變更行為。 例如，您可以指示 Scaffolder 產生註冊使用的程式碼。 產生的程式碼優先於身分識別 RCL 中的相同程式碼。 若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。

**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL 身分識別套件。 您可以選擇選取要產生的身分識別程式碼。

雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成此流程。 本檔說明完成身分識別架構更新所需的步驟。

執行身分識別 scaffolder 時，會在專案目錄中建立*ScaffoldingReadme。* *ScaffoldingReadme .txt*檔案包含完成身分識別架構更新所需事項的一般指示。 本檔包含比*ScaffoldingReadme*更完整的指示。

我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。 執行身分識別 scaffolder 之後，請檢查變更。

> [!NOTE]
> 使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他具有身分識別的安全性功能時，都需要服務。 當基架構識別時，不會產生服務或服務存根。 啟用這些功能的服務必須手動新增。 例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。

## <a name="scaffold-identity-into-an-empty-project"></a>將身分識別 Scaffold 到空的專案

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

將下列反白顯示的呼叫新增至 `Startup` 類別：

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>將身分識別 Scaffold 到不具現有授權的 Razor 專案

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

身分識別是在*Areas/identity/IdentityHostingStartup*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>遷移、UseAuthentication 和版面配置

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>啟用驗證

在 `Startup` 類別的 `Configure` 方法中，在 `UseStaticFiles`之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ：

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>版面配置變更

選擇性：將登入部分（`_LoginPartial`）新增至配置檔案：

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>使用授權將身分識別 Scaffold 到 Razor 專案

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
某些身分識別選項會在*區域/身分識別/IdentityHostingStartup*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>將身分識別 Scaffold 至沒有現有授權的 MVC 專案

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

選擇性：將登入部分（`_LoginPartial`）新增至*Views/Shared/_Layout. cshtml*檔案：

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* 將*Pages/shared/_LoginPartial*檔案移至*Views/shared/_LoginPartial。 cshtml*

身分識別是在*Areas/identity/IdentityHostingStartup*中設定。 如需詳細資訊，請參閱 IHostingStartup。

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

`UseStaticFiles`之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ：

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>使用授權將身分識別 Scaffold 至 MVC 專案

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

刪除*頁面/共用*資料夾，以及該資料夾中的檔案。

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>建立完整身分識別 UI 來源

若要維持身分識別 UI 的完全控制，請執行身分識別 scaffolder，然後選取 [覆**寫所有**檔案]。

下列反白顯示的程式碼顯示在 ASP.NET Core 2.1 web 應用程式中，將預設身分識別 UI 取代為身分識別的變更。 您可能想要執行此動作，以完全控制身分識別 UI。

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

下列程式碼會取代預設身分識別：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

註冊 `IEmailSender` 的執行，例如：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a>停用註冊頁面

若要停用使用者註冊：

* Scaffold 身分識別。 包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。 例如:

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* 更新*Areas/Identity/Pages/Account/Register. cshtml* ，讓使用者無法從這個端點註冊：

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* 更新*區域/身分識別/頁面/帳戶/* 暫存器，以與前述變更一致：

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* 將註冊連結從*區域/身分識別/頁面/帳戶/登*入批註掉或移除

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* 更新 [*區域/身分識別/頁面/帳戶/RegisterConfirmation* ] 頁面。

  * 移除來自 cshtml 檔案的程式碼和連結。
  * 從 `PageModel`中移除確認碼：

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
  
### <a name="use-another-app-to-add-users"></a>使用另一個應用程式來新增使用者

提供在 web 應用程式外部新增使用者的機制。 新增使用者的選項包括：

* 專用的管理 web 應用程式。
* 主控台應用程式。

下列程式碼概述新增使用者的一種方法：

* 使用者清單會讀入記憶體中。
* 為每個使用者產生強式唯一密碼。
* 使用者會新增至身分識別資料庫。
* 系統會通知使用者，並告知您變更密碼。

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

下列程式碼概述如何新增使用者：

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

針對生產案例，可以遵循類似的方法。

## <a name="additional-resources"></a>其他資源

* [將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本](xref:migration/20_21#changes-to-authentication-code)
