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
# <a name="scaffold-identity-in-aspnet-core-projects"></a>IdentityASP.NET Core 專案中的 Scaffold

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 提供[ASP.NET Core Identity ](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。 包含的應用程式 Identity 可以套用 scaffolder，以選擇性地新增包含在 Identity Razor 類別庫（RCL）中的原始程式碼。 建議您產生原始程式碼，以便能夠修改程式碼並變更行為。 例如，您可以指示 Scaffolder 產生註冊使用的程式碼。 產生的程式碼優先于 RCL 中的相同程式碼 Identity 。 若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整的 Identity UI 來源](#full)一節。

**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL Identity 套件。 您可以選擇 Identity 要產生的程式碼。

雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成流程。 本檔說明完成「基架構更新」所需的步驟 Identity 。

我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。 執行 scaffolder 後檢查變更 Identity 。

使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他安全性功能時，都需要服務 Identity 。 當樣板時，不會產生服務或服務存根 Identity 。 啟用這些功能的服務必須手動新增。 例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。

當 Identity 具有新資料內容的樣板加入具有現有個別帳戶的專案時：

* 在中 `Startup.ConfigureServices` ，移除對的呼叫：
  * `AddDbContext`
  * `AddDefaultIdentity`

例如， `AddDbContext` 和 `AddDefaultIdentity` 會在下列程式碼中加上批註：

[!code-csharp[](scaffold-identity/3.1sample/StartupRemove.cs?name=snippet)]

前面的程式碼會批註*區域/ Identity /IdentityHostingStartup.cs*中重複的程式碼

一般而言，使用個別帳戶建立的應用程式***不***應該建立新的資料內容。

## <a name="scaffold-identity-into-an-empty-project"></a>Scaffold Identity 至空的專案

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

使用與 `Startup` 下列類似的程式碼來更新類別：

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Scaffold Identity 至 Razor 沒有現有授權的專案

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

Identity會在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>遷移、UseAuthentication 和版面配置

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>啟用驗證

使用與 `Startup` 下列類似的程式碼來更新類別：

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>版面配置變更

選擇性：將登入部分（ `_LoginPartial` ）新增至配置檔案：

[!code-html[](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Scaffold Identity 至 Razor 具有授權的專案

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

有些 Identity 選項是在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Scaffold Identity 至沒有現有授權的 MVC 專案

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

選擇性：將登入部分（ `_LoginPartial` ）新增至*Views/Shared/_Layout. cshtml*檔案：

[!code-html[](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* 將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*

Identity會在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱 IHostingStartup。

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

使用與 `Startup` 下列類似的程式碼來更新類別：

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Scaffold Identity 至具有授權的 MVC 專案

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

## <a name="scaffold-identity-into-a-blazor-server-project-without-existing-authorization"></a>Scaffold Identity 至 Blazor 沒有現有授權的伺服器專案

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identity會在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

### <a name="migrations"></a>移轉

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

### <a name="pass-an-xsrf-token-to-the-app"></a>將 XSRF token 傳遞至應用程式

權杖可以傳遞給元件：

* 當驗證權杖已布建並儲存至驗證 cookie 時，可以將它們傳遞給元件。
* Razor元件無法 `HttpContext` 直接使用，因此無法取得[反要求偽造（XSRF）權杖](xref:security/anti-request-forgery)，以在張貼到 Identity 的登出端點 `/Identity/Account/Logout` 。 XSRF token 可以傳遞給元件。

如需詳細資訊，請參閱 <xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app> 。

在*Pages/_Host. cshtml*檔案中，在將它加入和類別之後，建立權杖 `InitialApplicationState` `TokenProvider` ：

```csharp
@inject Microsoft.AspNetCore.Antiforgery.IAntiforgery Xsrf

...

var tokens = new InitialApplicationState
{
    ...

    XsrfToken = Xsrf.GetAndStoreTokens(HttpContext).RequestToken
};
```

更新 `App` 元件（*razor*）以指派 `InitialState.XsrfToken` ：

```csharp
@inject TokenProvider TokenProvider

...

TokenProvider.XsrfToken = InitialState.XsrfToken;
```

本 `TokenProvider` 主題中所示範的服務用於 `LoginDisplay` 下列版面配置[和驗證流程變更](#layout-and-authentication-flow-changes)一節中的元件。

### <a name="enable-authentication"></a>啟用驗證

在 `Startup` 類別中：

* 確認 Razor 已在中新增頁面服務 `Startup.ConfigureServices` 。
* 如果使用[TokenProvider](xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app)，請註冊服務。
* `UseDatabaseErrorPage`針對開發環境，在的應用程式產生器上呼叫 `Startup.Configure` 。
* `UseAuthentication` `UseAuthorization` 在之後呼叫和 `UseRouting` 。
* 新增頁面的端點 Razor 。

[!code-csharp[](scaffold-identity/3.1sample/StartupBlazor.cs?highlight=3,6,14,27-28,32)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-and-authentication-flow-changes"></a>版面配置和驗證流程變更

`RedirectToLogin`在專案根目錄中，將元件（*RedirectToLogin*）新增至應用程式的*共用*資料夾：

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

將 `LoginDisplay` 元件（*LoginDisplay*）新增至應用程式的*共用*資料夾。 [TokenProvider 服務](xref:security/blazor/server/additional-scenarios#pass-tokens-to-a-blazor-server-app)會針對張貼至的登出端點的 HTML 表單，提供 XSRF token Identity ：

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

在 `MainLayout` 元件（*Shared/MainLayout*）中，將 `LoginDisplay` 元件加入至頂端資料列 `<div>` 元素的內容：

```razor
<div class="top-row px-4 auth">
    <LoginDisplay />
    <a href="https://docs.microsoft.com/aspnet/" target="_blank">About</a>
</div>
```

### <a name="style-authentication-endpoints"></a>樣式驗證端點

因為 Blazor 伺服器使用 Razor 頁面 Identity 頁面，所以當訪客在頁面和元件之間流覽時，UI 的樣式會變更 Identity 。 您有兩個選項可解決 incongruous 樣式：

#### <a name="build-identity-components"></a>組建 Identity 元件

不使用頁面的元件是用來 Identity 建立元件的方法 Identity 。 因為 `SignInManager` `UserManager` 元件中不支援和 Razor ，所以請使用伺服器應用程式中的 API 端點 Blazor 來處理使用者帳戶動作。

#### <a name="use-a-custom-layout-with-blazor-app-styles"></a>使用具有 Blazor 應用程式樣式的自訂版面配置

您 Identity 可以修改頁面版面配置和樣式，以產生使用預設主題的頁面 Blazor 。

> [!NOTE]
> 本節中的範例只是自訂的起點。 可能需要額外的工作，才能獲得最佳的使用者體驗。

建立新的 `NavMenu_IdentityLayout` 元件（*Shared/NavMenu_IdentityLayout razor*）。 如需元件的標記和程式碼，請使用應用程式元件的相同內容 `NavMenu` （*Shared/navmenu.cshtml*）。 去除 `NavLink` 無法匿名連線的任何元件，因為元件中的自動重新導向在 `RedirectToLogin` 需要驗證或授權的元件中失敗。

在*Pages/Shared/Layout*檔案中，進行下列變更：

* 將指示詞新增至檔案 Razor 頂端，以在*共用*資料夾中使用標籤協助程式和應用程式的元件：

  ```cshtml
  @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
  @using {APPLICATION ASSEMBLY}.Shared
  ```

  取代 `{APPLICATION ASSEMBLY}` 為應用程式的元件名稱。

* 將 `<base>` 標記和樣式表單新增 Blazor `<link>` 至 `<head>` 內容：

  ```cshtml
  <base href="~/" />
  <link rel="stylesheet" href="~/css/site.css" />
  ```

* 將標記的內容變更 `<body>` 如下：

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

## <a name="scaffold-identity-into-a-blazor-server-project-with-authorization"></a>Scaffold Identity 至 Blazor 具有授權的伺服器專案

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

有些 Identity 選項是在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>建立完整的 Identity UI 來源

若要維持 UI 的完全控制 Identity ，請執行 Identity scaffolder，然後選取 [覆**寫所有**檔案]。

下列反白顯示的程式碼顯示在 Identity Identity ASP.NET Core 2.1 web 應用程式中，將預設 UI 取代為的變更。 您可能想要這麼做，以擁有 UI 的完全控制權 Identity 。

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Identity下列程式碼會取代預設值：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

註冊執行 `IEmailSender` ，例如：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-a-page"></a>停用頁面

本章節說明如何停用 [註冊] 頁面，但此方法可用來停用任何頁面。

若要停用使用者註冊：

* Scaffold Identity 。 包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。 例如：

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* 更新*區域/ Identity /Pages/Account/Register.cshtml.cs* ，讓使用者無法從這個端點註冊：

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* 將*Areas/ Identity /Pages/Account/Register.cshtml*更新為與前述變更一致：

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* 批註外或移除*區域/ Identity /Pages/Account/Login.cshtml*的註冊連結

  ```cshtml
  @*
  <p>
      <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
  </p>
  *@
  ```

* 更新 [*區域/ Identity /Pages/Account/RegisterConfirmation* ] 頁面。

  * 移除來自 cshtml 檔案的程式碼和連結。
  * 從移除確認碼 `PageModel` ：

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
* 使用者會新增至 Identity 資料庫。
* 系統會通知使用者，並告知您變更密碼。

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

下列程式碼概述如何新增使用者：

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

針對生產案例，可以遵循類似的方法。

## <a name="prevent-publish-of-static-identity-assets"></a>防止發行靜態 Identity 資產

若要防止將靜態 Identity 資產發行至 web 根目錄，請參閱 <xref:security/authentication/identity#prevent-publish-of-static-identity-assets> 。

## <a name="additional-resources"></a>其他資源

* [將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 2.1 和更新版本[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。 包含的應用程式 Identity 可以套用 scaffolder，以選擇性地新增包含在 Identity Razor 類別庫（RCL）中的原始程式碼。 建議您產生原始程式碼，以便能夠修改程式碼並變更行為。 例如，您可以指示 Scaffolder 產生註冊使用的程式碼。 產生的程式碼優先于 RCL 中的相同程式碼 Identity 。 若要取得 UI 的完全控制，而不使用預設 RCL，請參閱[建立完整身分識別 UI 來源](#full)一節。

**不**包含驗證的應用程式可以套用 scaffolder 來新增 RCL Identity 套件。 您可以選擇 Identity 要產生的程式碼。

雖然 scaffolder 會產生大部分必要的程式碼，但您必須更新您的專案，才能完成此流程。 本檔說明完成「基架構更新」所需的步驟 Identity 。

Identity執行 scaffolder 時，會在專案目錄中建立*ScaffoldingReadme.txt*檔案。 *ScaffoldingReadme.txt*檔案包含完成樣板更新所需需求的一般指示 Identity 。 本檔包含比*ScaffoldingReadme.txt*檔案更完整的指示。

我們建議使用會顯示檔案差異的原始檔控制系統，並可讓您備份變更。 執行 scaffolder 後檢查變更 Identity 。

> [!NOTE]
> 使用[雙因素驗證](xref:security/authentication/identity-enable-qrcodes)、[帳戶確認和密碼](xref:security/authentication/accconfirm)復原，以及其他安全性功能時，都需要服務 Identity 。 當樣板時，不會產生服務或服務存根 Identity 。 啟用這些功能的服務必須手動新增。 例如，請參閱[需要電子郵件確認](xref:security/authentication/accconfirm#require-email-confirmation)。

## <a name="scaffold-identity-into-an-empty-project"></a>Scaffold Identity 至空的專案

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

將下列反白顯示的呼叫新增至 `Startup` 類別：

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Scaffold Identity 至 Razor 沒有現有授權的專案

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

Identity會在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>遷移、UseAuthentication 和版面配置

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>啟用驗證

在 `Configure` 類別的方法中 `Startup` ，于之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) `UseStaticFiles` ：

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>版面配置變更

選擇性：將登入部分（ `_LoginPartial` ）新增至配置檔案：

[!code-html[](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Scaffold Identity 至 Razor 具有授權的專案

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

有些 Identity 選項是在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱[IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)。

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Scaffold Identity 至沒有現有授權的 MVC 專案

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

選擇性：將登入部分（ `_LoginPartial` ）新增至*Views/Shared/_Layout. cshtml*檔案：

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* 將*Pages/shared/_LoginPartial. cshtml*檔案移至*Views/shared/_LoginPartial. cshtml*

Identity會在*區域/ Identity /IdentityHostingStartup.cs*中設定。 如需詳細資訊，請參閱 IHostingStartup。

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

在之後呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) `UseStaticFiles` ：

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Scaffold Identity 至具有授權的 MVC 專案

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

## <a name="create-full-identity-ui-source"></a>建立完整的 Identity UI 來源

若要維持 UI 的完全控制 Identity ，請執行 Identity scaffolder，然後選取 [覆**寫所有**檔案]。

下列反白顯示的程式碼顯示在 Identity Identity ASP.NET Core 2.1 web 應用程式中，將預設 UI 取代為的變更。 您可能想要這麼做，以擁有 UI 的完全控制權 Identity 。

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Identity下列程式碼會取代預設值：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

下列程式碼會設定[LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)和[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)：

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

註冊執行 `IEmailSender` ，例如：

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

* Scaffold Identity 。 包含帳戶。註冊、帳戶、登入和帳戶. RegisterConfirmation。 例如：

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* 更新*區域/ Identity /Pages/Account/Register.cshtml.cs* ，讓使用者無法從這個端點註冊：

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* 將*Areas/ Identity /Pages/Account/Register.cshtml*更新為與前述變更一致：

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* 批註外或移除*區域/ Identity /Pages/Account/Login.cshtml*的註冊連結

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* 更新 [*區域/ Identity /Pages/Account/RegisterConfirmation* ] 頁面。

  * 移除來自 cshtml 檔案的程式碼和連結。
  * 從移除確認碼 `PageModel` ：

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
* 使用者會新增至 Identity 資料庫。
* 系統會通知使用者，並告知您變更密碼。

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

下列程式碼概述如何新增使用者：

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

針對生產案例，可以遵循類似的方法。

## <a name="additional-resources"></a>其他資源

* [將驗證程式代碼變更為 ASP.NET Core 2.1 和更新版本](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end
