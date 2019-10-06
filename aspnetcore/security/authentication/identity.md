---
title: ASP.NET Core 上的身分識別簡介
author: rick-anderson
description: 將身分識別與 ASP.NET Core 應用程式搭配使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 6701eb0ac1b1abb8699a5a529bcc29f295e5c7c9
ms.sourcegitcommit: 4115bf0e850c13d4e655beb5ab5e8ff431173cb6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981915"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET Core 上的身分識別簡介

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 身分識別是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。 使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。 支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。

您可以使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。 或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。

[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。

在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。 如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity 和 AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)是在 ASP.NET Core 2.1 中引進。 呼叫 `AddDefaultIdentity` 類似于呼叫下列各項：

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>建立具有驗證的 Web 應用程式

建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 選取 [檔案]  >  [新增]  >  [專案]。
* 選取 [ASP.NET Core Web 應用程式]。 將專案命名為**WebApp1** ，使其命名空間與專案下載相同。 按一下 [確定]。
* 選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。
* 選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。 身分識別 Razor 類別庫會公開具有 `Identity` 區域的端點。 例如:

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>套用移轉

將遷移套用至初始化資料庫。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [套件管理員主控台] （PMC）中執行下列命令：

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>測試註冊和登入

執行應用程式並註冊使用者。 視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>設定身分識別服務

服務會在 `ConfigureServices` 中新增。 典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

上述程式碼會使用預設選項值來設定身分識別。 服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。

   藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用身分識別。 `UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   服務可透過相依性[插入](xref:fundamentals/dependency-injection)來提供給應用程式。

   藉由在 `Configure` 方法中呼叫 `UseAuthentication`，為應用程式啟用身分識別。 `UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   這些服務可透過相依性[插入](xref:fundamentals/dependency-injection)來提供給應用程式。

   藉由在 `Configure` 方法中呼叫 `UseIdentity`，為應用程式啟用身分識別。 `UseIdentity` 會將以 cookie 為基礎的驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。

## <a name="scaffold-register-login-and-logout"></a>Scaffold 註冊、登入和登出

將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

新增註冊、登入和登出檔案。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您已建立名為**WebApp1**的專案，請執行下列命令。 否則，請為 `ApplicationDbContext` 使用正確的命名空間：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell 使用分號做為命令分隔符號。 使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。

---

### <a name="examine-register"></a>檢查 Register

::: moniker range=">= aspnetcore-2.1"

   當使用者按一下 [**註冊**] 連結時，就會叫用 `RegisterModel.OnPostAsync` 動作。 使用者是透過[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)在 `_userManager` 物件上建立的。 `_userManager` 是由相依性插入所提供）：

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   當使用者按一下 [**註冊**] 連結時，就會在 `AccountController` 上叫用 @no__t 1 動作。 @No__t-0 動作會藉由在 `_userManager` 物件上呼叫 `CreateAsync` 來建立使用者（透過相依性插入提供給 `AccountController`）：

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   如果成功建立使用者，則使用者會藉由對 `_signInManager.SignInAsync` 的呼叫來登入。

   **注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。

### <a name="log-in"></a>登錄

::: moniker range=">= aspnetcore-2.1"

登入表單的顯示時機如下：

* 已選取 [**登入**] 連結。
* 使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。

當登入頁面上的表單提交時，會呼叫 `OnPostAsync` 動作。 在 `_signInManager` 物件上呼叫 `PasswordSignInAsync` （由相依性插入所提供）。

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   基底 `Controller` 類別會公開您可以從控制器方法存取的 @no__t 1 屬性。 例如，您可以列舉 `User.Claims`，並進行授權決策。 如需詳細資訊，請參閱<xref:security/authorization/introduction>。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

當使用者選取 [**登入**] 連結時，或在存取需要驗證的網頁時，就會顯示登入表單。 當使用者在登入頁面上提交表單時，會呼叫 `AccountController` `Login` 動作。

@No__t 0 動作會在 `_signInManager` 物件上呼叫 `PasswordSignInAsync` （提供給相依性插入 `AccountController`）。

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

基底（`Controller` 或 `PageModel`）類別會公開 @no__t 2 屬性。 例如，您可以列舉 `User.Claims` 來做出授權決策。

::: moniker-end

### <a name="log-out"></a>登出

::: moniker range=">= aspnetcore-2.1"

[**登出**] 連結會叫用 @no__t 1 動作。 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。

Post 是在*Pages/Shared/_LoginPartial*中指定的：

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   按一下 [**登出**] 連結會呼叫 @no__t 1 動作。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   上述程式碼會呼叫 `_signInManager.SignOutAsync` 方法。 @No__t-0 方法會清除儲存在 cookie 中的使用者宣告。

::: moniker-end

## <a name="test-identity"></a>測試身分識別

預設的 Web 專案範本允許匿名存取首頁。 若要測試身分識別，請將[`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)新增至 [隱私權] 頁面。

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。 系統會將您重新導向至登入頁面。

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>探索身分識別

若要更詳細地探索身分識別：

* [建立完整身分識別 UI 來源](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* 檢查每個頁面的來源，並逐步執行偵錯工具。

::: moniker-end

## <a name="identity-components"></a>身分識別元件

::: moniker range=">= aspnetcore-2.1"

所有身分識別相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。

::: moniker-end

身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/) 此套件包含 ASP.NET Core 身分識別的核心介面集，並由 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 所包含。

## <a name="migrating-to-aspnet-core-identity"></a>遷移至 ASP.NET Core 身分識別

如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。

## <a name="setting-password-strength"></a>設定密碼強度

如需設定最小密碼[需求的範例](#pw)，請參閱設定。

## <a name="next-steps"></a>後續步驟

* [設定身分識別](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
