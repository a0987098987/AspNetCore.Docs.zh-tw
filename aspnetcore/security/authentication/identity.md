---
title: ASP.NET core 身分識別簡介
author: rick-anderson
description: 使用 ASP.NET Core 應用程式中使用身分識別。 了解如何設定密碼的需求 （RequireDigit、 RequiredLength、 RequiredUniqueChars，等等）。
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 355c53e0c957944cb35c37c6b01e724af5f93f44
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265475"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET core 身分識別簡介

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core Identity 是將登入功能加入至 ASP.NET Core 應用程式的成員資格系統。 使用者可以建立的帳戶與儲存在身分識別的登入資訊，或者可以使用外部登入提供者。 支援的外部登入提供者包括[Facebook、 Google、 Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。

可以使用 SQL Server 資料庫來儲存使用者名稱、 密碼和設定檔資料，設定身分識別。 或者，另一個持續性存放區可用，例如 Azure 資料表儲存體。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)([如何下載)](xref:index#how-to-download-a-sample))。

本主題中，您將了解如何使用身分識別來註冊、 登入，並登出使用者。 如需有關建立使用身分識別的應用程式的詳細指示，請參閱本文結尾 「 後續步驟 」 一節。

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity 和 AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP.NET Core 2.1 中引進。 呼叫`AddDefaultIdentity`類似於呼叫下列命令：

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

請參閱[AddDefaultIdentity 來源](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63)如需詳細資訊。

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>建立驗證的 Web 應用程式

個別使用者帳戶中建立 ASP.NET Core Web 應用程式專案。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 選取 [檔案]  >  [新增]  >  [專案]。
* 選取 [ASP.NET Core Web 應用程式]。 將專案命名為**WebApp1**將專案下載相同的命名空間。 按一下 [確定] 。
* 選取 ASP.NET Core **Web 應用程式**，然後選取**變更驗證**。
* 選取 **個別使用者帳戶**然後按一下**確定**。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

產生的專案提供[ASP.NET Core Identity](xref:security/authentication/identity)作為[Razor 類別庫](xref:razor-pages/ui-class)。 身分識別 Razor 類別庫會公開端點`Identity`區域。 例如: 

* / 身分識別/帳戶/登入
* / 身分識別/帳戶/登出
* / 身分識別/帳戶/管理

### <a name="test-register-and-login"></a>測試註冊和登入

執行應用程式並註冊的使用者。 根據您的螢幕大小，您可能需要選取瀏覽切換按鈕，以查看**註冊**並**登入**連結。

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>設定身分識別服務

服務會加入`ConfigureServices`。 典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

上述程式碼會使用預設選項值設定身分識別。 服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。

   藉由呼叫啟用身分識別[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。 `UseAuthentication` 新增驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。

   藉由呼叫應用程式啟用身分識別`UseAuthentication`在`Configure`方法。 `UseAuthentication` 新增驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   這些服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。

   藉由呼叫應用程式啟用身分識別`UseIdentity`在`Configure`方法。 `UseIdentity` 新增 cookie 為基礎的驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

如需詳細資訊，請參閱 < [IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)並[應用程式啟動](xref:fundamentals/startup)。

## <a name="scaffold-register-login-and-logout"></a>Scaffold 註冊、 登入和登出

請遵循[Scaffold Razor 專案具有授權的身分識別](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)產生程式碼顯示這一節的指示。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

新增註冊、 登入和登出的檔案。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您建立的專案名稱**WebApp1**，執行下列命令。 否則，請使用正確的命名空間，如`ApplicationDbContext`:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell 會使用分號做為命令分隔符號。 使用 PowerShell 時，逸出分號，在檔案清單，或置於雙引號括住，如上述範例所示的檔案清單。

---

### <a name="examine-register"></a>檢查暫存器

::: moniker range=">= aspnetcore-2.1"

   當使用者按一下**註冊**連結，`RegisterModel.OnPostAsync`叫用動作。 使用者由[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)上`_userManager`物件。 `_userManager` 是由提供相依性插入）：

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   當使用者按一下**註冊**連結`Register`上叫用動作`AccountController`。 `Register`動作會建立使用者，藉由呼叫`CreateAsync`上`_userManager`物件 (提供給`AccountController`由相依性插入):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   如果已成功建立使用者，使用者會登入的呼叫所`_signInManager.SignInAsync`。

   **注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)的步驟，以避免在註冊的立即登入。

### <a name="log-in"></a>登入

::: moniker range=">= aspnetcore-2.1"

登入表單顯示時：

* **登入**選取連結。
* 使用者嘗試存取受限制的頁面未獲授權可以存取**或**時在還沒有已驗證系統。

登入頁面上的表單提交時，`OnPostAsync`呼叫動作。 `PasswordSignInAsync` 呼叫`_signInManager`（由相依性插入提供） 的物件。

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   基底`Controller`類別會公開`User`屬性，您可以從控制器方法存取。 比方說，您可以列舉`User.Claims`並進行授權決策。 如需詳細資訊，請參閱<xref:security/authorization/introduction>。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

當使用者選取時，會顯示登入表單**登入**存取需要驗證的網頁時，會重新導向或連結。 當使用者提交表單時的登入頁面上， `AccountController` `Login`呼叫動作。

`Login`動作會呼叫`PasswordSignInAsync`上`_signInManager`物件 (提供給`AccountController`由相依性插入)。

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

基底 (`Controller`或是`PageModel`) 類別會公開`User`屬性。 比方說，`User.Claims`可以列舉來製作授權決策。

::: moniker-end

### <a name="log-out"></a>登出

::: moniker range=">= aspnetcore-2.1"

**登出** 連結會叫用`LogoutModel.OnPost`動作。 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)清除儲存在 cookie 中的使用者宣告。 不重新導向之後呼叫`SignOutAsync`或使用者將會**不**登出。

中所指定的張貼*Pages/Shared/_LoginPartial.cshtml*:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   按一下 **登出**連結呼叫`LogOut`動作。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   上述程式碼會呼叫`_signInManager.SignOutAsync`方法。 `SignOutAsync`方法會清除儲存在 cookie 中的使用者宣告。

::: moniker-end

## <a name="test-identity"></a>測試身分識別

預設的 web 專案範本允許匿名存取首頁。 若要測試識別，新增[ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)隱私權頁面。

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

如果您登入，登出。執行應用程式，然後選取**隱私權**連結。 將您重新導向至登入頁面。

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>瀏覽身分識別

若要更詳細地探索身分識別：

* [建立完整的身分識別 UI 來源](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* 檢查每個頁面和逐步執行偵錯工具的來源。

::: moniker-end

## <a name="identity-components"></a>身分識別元件

::: moniker range=">= aspnetcore-2.1"

中包含所有身分識別相依的 NuGet 套件[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

::: moniker-end

身分識別的主要套件[Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)。 此封裝包含一組核心介面的 ASP.NET Core 身分識別，並包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。

## <a name="migrating-to-aspnet-core-identity"></a>移轉至 ASP.NET Core 身分識別

如需詳細資訊和移轉您現有的身分識別存放區的指引，請參閱[移轉驗證和身分識別](xref:migration/identity)。

## <a name="setting-password-strength"></a>設定密碼強度

請參閱[組態](#pw)如需設定的最小的密碼需求的範例。

## <a name="next-steps"></a>後續步驟

* [設定身分識別](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
