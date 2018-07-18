---
title: ASP.NET core 身分識別簡介
author: rick-anderson
description: 使用 ASP.NET Core 應用程式中使用身分識別。 包括設定密碼的需求 （RequireDigit、 RequiredLength、 RequiredUniqueChars 等等）。
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095635"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET core 身分識別簡介

藉由[請參閱 Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，Jon Galloway [Erik Reitan](https://github.com/Erikre)，和[Steve Smith](https://ardalis.com/)

ASP.NET Core Identity 是可讓您將登入功能加入至您的應用程式的成員資格系統。 使用者可以建立帳戶和登入的使用者名稱和密碼，或它們可以使用外部登入提供者，例如 Facebook、 Google、 Microsoft 帳戶、 Twitter 或其他項目。

您可以設定要用來儲存使用者名稱、 密碼及分析資料的 SQL Server 資料庫的 ASP.NET Core身分識別。 或者，您可以使用您自己的持續性存放區，例如，「 Azure 資料表儲存體。 本文件包含適用於 Visual Studio，並使用 CLI 的指示。

[檢視或下載範例程式碼。](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>身分識別概觀

本主題中，您將了解如何使用 ASP.NET Core Identity，若要將功能新增至註冊、 登入，並登出使用者。 如需有關使用 ASP.NET Core Identity 建立應用程式的詳細指示，請參閱本文結尾 「 後續步驟 」 一節。

1. 個別使用者帳戶中建立 ASP.NET Core Web 應用程式專案。

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   在 Visual Studio 中，選取**檔案** > **新增** > **專案**。 選取  **ASP.NET Core Web 應用程式**然後按一下**確定**。

   ![[新增專案] 對話](identity/_static/01-new-project.png)

   選取 ASP.NET Core **Web 應用程式 （模型-檢視-控制器）** asp.net Core 2.x，然後選取**變更驗證**。

   ![[新增專案] 對話](identity/_static/02-new-project.png)

   供應項目會出現對話方塊驗證選項。 選取 **個別使用者帳戶**，按一下 **確定**返回先前的對話方塊。

   ![[新增專案] 對話](identity/_static/03-new-project-auth.png)

   選取**個別使用者帳戶**會指示 Visual Studio 建立模型、 Viewmodel、 檢視、 控制器及其他專案範本的一部分驗證所需的資產。

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

   如果使用.NET Core CLI，請建立新的專案使用`dotnet new mvc --auth Individual`。 此命令會建立新的專案與 Visual Studio 會建立相同身分識別範本程式碼。

   建立的專案包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`封裝，它會保存身分資料和使用 SQL Server 驗證的結構描述[Entity Framework Core](https://docs.microsoft.com/ef/)。

   ---

2. 設定身分識別服務，並新增中的介軟體`Startup`。

   中的應用程式中加入的身分識別服務`ConfigureServices`方法中的`Startup`類別：

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   這些服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。

   藉由呼叫應用程式啟用身分識別`UseAuthentication`在`Configure`方法。 `UseAuthentication` 新增驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   這些服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。

   藉由呼叫應用程式啟用身分識別`UseIdentity`在`Configure`方法。 `UseIdentity` 新增 cookie 為基礎的驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   如需處理程序的應用程式啟動的詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。

3. 建立的使用者。

   啟動應用程式，然後按一下**註冊**連結。

   如果這是您要執行此動作的第一次，您可能必須執行移轉。 應用程式會提示您**套用移轉**。 如有需要請重新整理頁面。

   ![適用於移轉網頁](identity/_static/apply-migrations.png)

   或者，您可以測試使用您的應用程式，而不需要持續性資料庫中的 ASP.NET Core 身分識別，使用記憶體中資料庫。 若要使用的記憶體中資料庫，請新增`Microsoft.EntityFrameworkCore.InMemory`封裝到您的應用程式，並修改您的應用程式呼叫`AddDbContext`在`ConfigureServices`，如下所示：

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   當使用者按一下**註冊**連結`Register`上叫用動作`AccountController`。 `Register`動作會建立使用者，藉由呼叫`CreateAsync`上`_userManager`物件 (提供給`AccountController`由相依性插入):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   如果已成功建立使用者，使用者會登入的呼叫所`_signInManager.SignInAsync`。

   **注意︰** 請參閱 <<c2> [ 帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)的步驟，以避免在註冊的立即登入。

4. 登入。

   使用者可以登入，依序按一下**登入** 連結，在頂端的站台，或它們可能會瀏覽至登入頁面當他們嘗試存取需要授權的站台的一部分。 當使用者提交表單時的登入頁面上， `AccountController` `Login`呼叫動作。

   `Login`動作會呼叫`PasswordSignInAsync`上`_signInManager`物件 (提供給`AccountController`由相依性插入)。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   基底`Controller`類別會公開`User`屬性，您可以從控制器方法存取。 比方說，您可以列舉`User.Claims`並進行授權決策。 如需詳細資訊，請參閱 <<c0> [ 授權](xref:security/authorization/index)。

5. 登出。

   按一下 **登出**連結呼叫`LogOut`動作。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   上述程式碼中的呼叫上述`_signInManager.SignOutAsync`方法。 `SignOutAsync`方法會清除儲存在 cookie 中的使用者宣告。

<a name="pw"></a>
6. 組態設定。

   身分識別具有可以在應用程式的啟動類別中覆寫一些預設行為。 `IdentityOptions` 不需要使用的預設行為時設定。 下列程式碼會設定數個密碼強度選項：

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   如需如何設定身分識別的詳細資訊，請參閱[設定的身分識別](xref:security/authentication/identity-configuration)。

   您也可以設定主要索引鍵資料類型，請參閱[設定識別主索引鍵的資料型別](xref:security/authentication/identity-primary-key-configuration)。

7. 檢視的資料庫。

   如果您的應用程式使用 SQL Server 資料庫 （預設值在 Windows 上，以及適用於 Visual Studio 使用者），您可以檢視資料庫建立的應用程式。 您可以使用**SQL Server Management Studio**。 或者，從 Visual Studio 中，選取**檢視** > **SQL Server 物件總管**。 連接到 **(localdb) \MSSQLLocalDB**。 資料庫名稱符合`aspnet-<name of your project>-<guid>`隨即出現。

   ![在 AspNetUsers 資料庫資料表上的操作功能表](identity/_static/04-db.png)

   展開的資料庫及其**資料表**，然後以滑鼠右鍵按一下**dbo。AspNetUsers**資料表，然後選取**檢視資料**。

8. 確認身分識別可運作

    預設值*ASP.NET Core Web 應用程式*專案範本可讓使用者存取應用程式中的任何動作，而不需要登入。 若要確認，適用於 ASP.NET 身分識別，請新增`[Authorize]`屬性設定為`About`動作`Home`控制站。

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    執行專案使用**Ctrl** + **F5**並瀏覽至**有關**頁面。 只有已驗證的使用者可以存取**關於**現在頁面，所以 ASP.NET 會將您導向登入頁面，來登入或註冊。

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    開啟命令視窗並瀏覽至該專案的根目錄的目錄包含`.csproj`檔案。 執行[dotnet 執行](/dotnet/core/tools/dotnet-run)執行應用程式的命令：

    ```csharp
    dotnet run 
    ```

    瀏覽的輸出中指定的 URL [dotnet 執行](/dotnet/core/tools/dotnet-run)命令。 此 URL 應該指向`localhost`產生的連接埠號碼。 瀏覽至**關於**頁面。 只有已驗證的使用者可以存取**關於**現在頁面，所以 ASP.NET 會將您導向登入頁面，來登入或註冊。

    ---

## <a name="identity-components"></a>身分識別元件

身分識別系統的主要參考組件是`Microsoft.AspNetCore.Identity`。 此封裝包含一組核心介面的 ASP.NET Core 身分識別，並包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。

若要使用 ASP.NET Core 應用程式中的身分識別系統需要這些相依性：

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -包含必要的型別，以搭配 Entity Framework Core 中的身分識別。

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core 是 Microsoft 建議的資料存取技術，例如 SQL Server 關聯式資料庫。 進行測試，您可以使用`Microsoft.EntityFrameworkCore.InMemory`。

* `Microsoft.AspNetCore.Authentication.Cookies` 中介軟體，可讓應用程式以使用以 cookie 為基礎的驗證。

## <a name="migrating-to-aspnet-core-identity"></a>移轉至 ASP.NET Core 身分識別

如需詳細資訊和指引移轉您現有的身分識別存放區，請參閱[移轉驗證和身分識別](xref:migration/identity)。

## <a name="setting-password-strength"></a>設定密碼強度

請參閱[組態](#pw)如需設定的最小的密碼需求的範例。

## <a name="next-steps"></a>後續步驟

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
