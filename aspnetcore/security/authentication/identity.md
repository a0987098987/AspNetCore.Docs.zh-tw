---
title: ASP.NET Core 上的身分識別簡介
author: rick-anderson
description: 將身分識別與 ASP.NET Core 應用程式搭配使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
ms.author: riande
ms.date: 01/15/2020
uid: security/authentication/identity
ms.openlocfilehash: 2e0723d34a09109a034f3375c4e94aedab2a5427
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662341"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET Core 上的身分識別簡介

::: moniker range=">= aspnetcore-3.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

ASP.NET Core 身分識別：

* 是支援使用者介面（UI）登入功能的 API。
* 管理使用者、密碼、設定檔資料、角色、宣告、權杖、電子郵件確認等等。

使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。 支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。

您可以在 GitHub 上取得身分[識別來來源程式代碼](https://github.com/dotnet/AspNetCore/tree/master/src/Identity)。 [Scaffold 身分識別](xref:security/authentication/scaffold-identity)並查看產生的檔案，以檢查與身分識別的範本互動。

通常會使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。 或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。

在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。 如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。

[Microsoft 身分識別平臺](/azure/active-directory/develop/)是：

* Azure Active Directory （Azure AD）開發人員平臺的演進。
* 與 ASP.NET Core 身分識別無關。

[!INCLUDE[](~/includes/IdentityServer4.md)]

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)（[如何下載）](xref:index#how-to-download-a-sample)）。

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a>建立具有驗證的 Web 應用程式

建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 選取 **[** 檔案] > [**新增**>**專案**]。
* 選取 **ASP.NET Core Web 應用程式**。 將專案命名為**WebApp1** ，使其命名空間與專案下載相同。 按一下 [確定]。
* 選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。
* 選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

上述命令會使用 SQLite 建立 Razor web 應用程式。 若要使用 LocalDB 建立 web 應用程式，請執行下列命令：

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。 身分識別 Razor 類別庫會公開具有 `Identity` 區域的端點。 例如：

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>套用移轉

套用遷移來初始化資料庫。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [套件管理員主控台] （PMC）中執行下列命令：

`PM> Update-Database`

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用 SQLite 時，在此步驟中不需要進行遷移。 若是 LocalDB，請執行下列命令：

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>測試註冊和登入

執行應用程式並註冊使用者。 視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>設定身分識別服務

服務會在 `ConfigureServices`中新增。 典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

上述的反白顯示程式碼會使用預設選項值來設定身分識別。 服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。

識別是藉由呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>來啟用。 `UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

範本產生的應用程式不會使用[授權](xref:security/authorization/secure-data)。 包含 `app.UseAuthorization`，以確保在應用程式新增授權時，會以正確的順序加入。 `UseRouting`、`UseAuthentication`、`UseAuthorization`和 `UseEndpoints` 必須以上述程式碼中所示的順序來呼叫。

如需 `IdentityOptions` 和 `Startup`的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Identity.IdentityOptions> 和[應用程式啟動](xref:fundamentals/startup)。

## <a name="scaffold-register-login-and-logout"></a>Scaffold 註冊、登入和登出

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

新增註冊、登入和登出檔案。 將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您已建立名為**WebApp1**的專案，請執行下列命令。 否則，請為 `ApplicationDbContext`使用正確的命名空間：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell 使用分號做為命令分隔符號。 使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。

如需有關樣板識別的詳細資訊，請參閱[使用授權將身分識別 Scaffold 至 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)。

---

### <a name="examine-register"></a>檢查 Register

當使用者按一下 [**註冊**] 連結時，就會叫用 `RegisterModel.OnPostAsync` 動作。 使用者是透過[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)在 `_userManager` 物件上建立的。 `_userManager` 是由相依性插入所提供）：

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

如果成功建立使用者，則會透過呼叫 `_signInManager.SignInAsync`來登入使用者。

請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。

### <a name="log-in"></a>登入

登入表單的顯示時機如下：

* 已選取 [**登入**] 連結。
* 使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。

當登入頁面上的表單提交時，會呼叫 `OnPostAsync` 動作。 `PasswordSignInAsync` 是在 `_signInManager` 物件上呼叫（由相依性插入所提供）。

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

基底 `Controller` 類別會公開可從控制器方法存取的 `User` 屬性。 例如，您可以列舉 `User.Claims` 並做出授權決策。 如需詳細資訊，請參閱 <xref:security/authorization/introduction>。

### <a name="log-out"></a>登出

[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

在上述程式碼中，程式碼 `return RedirectToPage();` 必須是重新導向，才能讓瀏覽器執行新的要求，並更新使用者的身分識別。

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。

Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a>測試身分識別

預設的 Web 專案範本允許匿名存取首頁。 若要測試身分識別，請新增[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)：

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。 系統會將您重新導向至 [登入] 頁面。

### <a name="explore-identity"></a>探索身分識別

若要更詳細地探索身分識別：

* [建立完整身分識別 UI 來源](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* 檢查每個頁面的來源，並逐步執行偵錯工具。

## <a name="identity-components"></a>身分識別元件

所有與身分識別相關的 NuGet 套件都包含在[ASP.NET Core 共用架構](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)中。

身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/) 此套件包含 ASP.NET Core 身分識別的核心介面集，而且是由 `Microsoft.AspNetCore.Identity.EntityFrameworkCore`所包含。

## <a name="migrating-to-aspnet-core-identity"></a>遷移至 ASP.NET Core 身分識別

如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。

## <a name="setting-password-strength"></a>設定密碼強度

如需設定最小密碼[需求的範例](#pw)，請參閱設定。

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity 和 AddIdentity

<xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> 是在 ASP.NET Core 2.1 中引進。 呼叫 `AddDefaultIdentity` 類似于呼叫下列各項：

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。

## <a name="prevent-publish-of-static-identity-assets"></a>防止發佈靜態身分識別資產

若要防止將靜態身分識別資產（身分識別 UI 的樣式表單和 JavaScript 檔案）發行至 web 根目錄，請將下列 `ResolveStaticWebAssetsInputsDependsOn` 屬性和 `RemoveIdentityAssets` 目標新增至應用程式的專案檔：

```xml
<PropertyGroup>
  <ResolveStaticWebAssetsInputsDependsOn>RemoveIdentityAssets</ResolveStaticWebAssetsInputsDependsOn>
</PropertyGroup>

<Target Name="RemoveIdentityAssets">
  <ItemGroup>
    <StaticWebAsset Remove="@(StaticWebAsset)" Condition="%(SourceId) == 'Microsoft.AspNetCore.Identity.UI'" />
  </ItemGroup>
</Target>
```

## <a name="next-steps"></a>後續步驟

* 如需使用 SQLite 設定身分識別的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131)。
* [設定身分識別](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

ASP.NET Core 身分識別是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。 使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。 支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。

您可以使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。 或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。

在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。 如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity 和 AddIdentity

<xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> 是在 ASP.NET Core 2.1 中引進。 呼叫 `AddDefaultIdentity` 類似于呼叫下列各項：

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。

## <a name="create-a-web-app-with-authentication"></a>建立具有驗證的 Web 應用程式

建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 選取 **[** 檔案] > [**新增**>**專案**]。
* 選取 **ASP.NET Core Web 應用程式**。 將專案命名為**WebApp1** ，使其命名空間與專案下載相同。 按一下 [確定]。
* 選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。
* 選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。 身分識別 Razor 類別庫會公開具有 `Identity` 區域的端點。 例如：

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>套用移轉

套用遷移來初始化資料庫。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [套件管理員主控台] （PMC）中執行下列命令：

```powershell
Update-Database
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>測試註冊和登入

執行應用程式並註冊使用者。 視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>設定身分識別服務

服務會在 `ConfigureServices`中新增。 典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

上述程式碼會使用預設選項值來設定身分識別。 服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。

藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用身分識別。 `UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。

## <a name="scaffold-register-login-and-logout"></a>Scaffold 註冊、登入和登出

將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

新增註冊、登入和登出檔案。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您已建立名為**WebApp1**的專案，請執行下列命令。 否則，請為 `ApplicationDbContext`使用正確的命名空間：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell 使用分號做為命令分隔符號。 使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。

---

### <a name="examine-register"></a>檢查 Register

當使用者按一下 [**註冊**] 連結時，就會叫用 `RegisterModel.OnPostAsync` 動作。 使用者是透過[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)在 `_userManager` 物件上建立的。 `_userManager` 是由相依性插入所提供）：

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

如果成功建立使用者，則會透過呼叫 `_signInManager.SignInAsync`來登入使用者。

**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。

### <a name="log-in"></a>登入

登入表單的顯示時機如下：

* 已選取 [**登入**] 連結。
* 使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。

當登入頁面上的表單提交時，會呼叫 `OnPostAsync` 動作。 `PasswordSignInAsync` 是在 `_signInManager` 物件上呼叫（由相依性插入所提供）。

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

基底 `Controller` 類別會公開您可以從控制器方法存取的 `User` 屬性。 例如，您可以列舉 `User.Claims` 並做出授權決策。 如需詳細資訊，請參閱 <xref:security/authorization/introduction>。

### <a name="log-out"></a>登出

[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。

Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a>測試身分識別

預設的 Web 專案範本允許匿名存取首頁。 若要測試身分識別，請將[`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)新增至 [隱私權] 頁面。

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。 系統會將您重新導向至 [登入] 頁面。

### <a name="explore-identity"></a>探索身分識別

若要更詳細地探索身分識別：

* [建立完整身分識別 UI 來源](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* 檢查每個頁面的來源，並逐步執行偵錯工具。

## <a name="identity-components"></a>身分識別元件

所有身分識別相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。

身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/) 此套件包含 ASP.NET Core 身分識別的核心介面集，而且是由 `Microsoft.AspNetCore.Identity.EntityFrameworkCore`所包含。

## <a name="migrating-to-aspnet-core-identity"></a>遷移至 ASP.NET Core 身分識別

如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。

## <a name="setting-password-strength"></a>設定密碼強度

如需設定最小密碼[需求的範例](#pw)，請參閱設定。

## <a name="next-steps"></a>後續步驟

* 如需使用 SQLite 設定身分識別的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131)。
* [設定身分識別](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
