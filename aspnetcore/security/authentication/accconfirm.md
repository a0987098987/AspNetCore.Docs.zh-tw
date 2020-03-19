---
title: ASP.NET Core 中的帳戶確認和密碼復原
author: rick-anderson
description: 瞭解如何使用電子郵件確認和密碼重設來建立 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 3a6b0501d507929c9929207a7bb871b3b81b7cb8
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511622"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>ASP.NET Core 中的帳戶確認和密碼復原

依[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Ponant](https://github.com/Ponant)和[Joe Audette](https://twitter.com/joeaudette)

本教學課程說明如何使用電子郵件確認和密碼重設來建立 ASP.NET Core 應用程式。 本教學課程**不**是開始主題。 您應該已熟悉：

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [驗證](xref:security/authentication/identity)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

請參閱[此 PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf)檔案以取得 ASP.NET Core 1.1 版本。

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a>Prerequisites

[.NET Core 3.0 SDK 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a>使用驗證來建立及測試 web 應用程式

執行下列命令來建立具有驗證的 web 應用程式。

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

執行應用程式，選取 [**註冊**] 連結，然後註冊使用者。 註冊之後，系統會將您重新導向至 [到 `/Identity/Account/RegisterConfirmation`] 頁面，其中包含用來模擬電子郵件確認的連結：

* 選取 [`Click here to confirm your account`] 連結。
* 選取 [**登**入] 連結，並使用相同的認證進行登入。
* 選取 [`Hello YourEmail@provider.com!`] 連結，將您重新導向至 [`/Identity/Account/Manage/PersonalData`] 頁面。
* 選取左側的 [**個人資料**] 索引標籤，然後選取 [**刪除**]。

### <a name="configure-an-email-provider"></a>設定電子郵件提供者

在本教學課程中， [SendGrid](https://sendgrid.com)是用來傳送電子郵件。 您需要 SendGrid 帳戶和金鑰，才能傳送電子郵件。 您可以使用其他電子郵件提供者。 建議您使用 SendGrid 或其他電子郵件服務來傳送電子郵件。 SMTP 很容易保護和設定正確。

建立類別來提取安全的電子郵件金鑰。 針對此範例，請建立*Services/AuthMessageSenderOptions .cs*：

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>設定 SendGrid 使用者秘密

使用 [[密碼管理員] 工具](xref:security/app-secrets)來設定 `SendGridUser` 和 `SendGridKey`。 例如：

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

在 Windows 中，秘密管理員會將金鑰/值組儲存在 `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` 目錄的*秘密 json*檔案中。

不會加密*秘密 json*檔案的內容。 下列標記顯示秘密的*json*檔案。 `SendGridKey` 值已移除。

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

如需詳細資訊，請參閱[選項模式](xref:fundamentals/configuration/options)[和設定](xref:fundamentals/configuration/index)。

### <a name="install-sendgrid"></a>安裝 SendGrid

本教學課程說明如何透過[SendGrid](https://sendgrid.com/)新增電子郵件通知，但您可以使用 SMTP 和其他機制來傳送電子郵件。

安裝 `SendGrid` NuGet 套件：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

從 [套件管理員主控台] 中，輸入下列命令：

```powershell
Install-Package SendGrid
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從主控台，輸入下列命令：

```dotnetcli
dotnet add package SendGrid
```

---

請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)來註冊免費的 SendGrid 帳戶。

### <a name="implement-iemailsender"></a>執行 IEmailSender

若要執行 `IEmailSender`，請使用與下列類似的程式碼來建立*服務/EmailSender* ：

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>設定啟動以支援電子郵件

將下列程式碼新增至*Startup.cs*檔案中的 `ConfigureServices` 方法：

* 將 `EmailSender` 新增為暫時性服務。
* 註冊 `AuthMessageSenderOptions` 設定實例。

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a>註冊、確認電子郵件和重設密碼

執行 web 應用程式，並測試帳戶確認和密碼復原流程。

* 執行應用程式並註冊新的使用者
* 檢查您的電子郵件以取得帳戶確認連結。 如果您沒有收到電子郵件，請參閱[Debug email](#debug) 。
* 按一下連結以確認您的電子郵件。
* 使用您的電子郵件和密碼登入。
* 登出。

### <a name="test-password-reset"></a>測試密碼重設

* 如果您已登入，請選取 [**登出**]。
* 選取 [**登入**] 連結，然後選取 [**忘記密碼？** ] 連結。
* 輸入您用來註冊帳戶的電子郵件。
* 系統會傳送一封電子郵件，其中包含重設密碼的連結。 檢查您的電子郵件，然後按一下連結以重設密碼。 成功重設密碼之後，您就可以使用您的電子郵件和新密碼來登入。

## <a name="change-email-and-activity-timeout"></a>變更電子郵件和活動超時

預設的無活動超時時間為14天。 下列程式碼會將非啟用時間設定為5天：

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a>變更所有資料保護權杖壽命

下列程式碼會將所有資料保護權杖超時時間變更為3小時：

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

內建身分識別使用者權杖（請參閱[AspNetCore/src/Identity/Extensions. Core/src/TokenOptions](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ）具有[一天的超時時間](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。

### <a name="change-the-email-token-lifespan"></a>變更電子郵件權杖生命週期

身分[識別使用者權杖](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)的預設權杖存留期為[一天](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。 本節說明如何變更電子郵件權杖的生命週期。

新增自訂[DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和 <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>：

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

將自訂提供者新增至服務容器：

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a>重新傳送電子郵件確認

請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/5410)。

<a name="debug"></a>

### <a name="debug-email"></a>Debug 電子郵件

如果您無法讓電子郵件正常運作：

* 在 `EmailSender.Execute` 中設定中斷點，以確認呼叫 `SendGridClient.SendEmailAsync`。
* 建立[主控台應用程式，以](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼將電子郵件傳送至 `EmailSender.Execute`。
* 查看[電子郵件活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。
* 檢查您的垃圾郵件資料夾。
* 在不同的電子郵件提供者（Microsoft、Yahoo、Gmail 等）上嘗試另一個電子郵件別名
* 嘗試傳送至不同的電子郵件帳戶。

**安全性最佳做法**是**不要**在測試和開發中使用生產秘密。 如果您將應用程式發佈至 Azure，請在 Azure Web 應用程式入口網站中將 SendGrid 秘密設定為應用程式設定。 設定系統已設定為從環境變數讀取金鑰。

## <a name="combine-social-and-local-login-accounts"></a>合併社交和本機登入帳戶

若要完成這一節，您必須先啟用外部驗證提供者。 請參閱[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)。

您可以按一下電子郵件連結，以合併本機和社交帳戶。 在下列順序中，會先將 "RickAndMSFT@gmail.com" 建立為本機登入;不過，您可以先將帳戶建立為社交登入，然後再新增本機登入。

![Web 應用程式： RickAndMSFT@gmail.com 使用者已驗證](accconfirm/_static/rick.png)

按一下 [**管理**] 連結。 請注意與此帳戶相關聯的0外部（社交登入）。

![管理檢視](accconfirm/_static/manage.png)

按一下另一個登入服務的連結，並接受應用程式要求。 在下圖中，Facebook 是外部驗證提供者：

![管理您的外部登入視圖清單 Facebook](accconfirm/_static/fb.png)

已合併這兩個帳戶。 您可以使用任一帳戶登入。 您可能想要讓使用者新增本機帳戶，以防其社交登入驗證服務關閉，或更有可能失去其社交帳戶的存取權。

## <a name="enable-account-confirmation-after-a-site-has-users"></a>在網站有使用者之後啟用帳戶確認

在網站上啟用帳戶確認，使用者會鎖定所有現有的使用者。 現有的使用者會被鎖定，因為他們的帳戶不會確認。 若要解決現有的使用者鎖定，請使用下列其中一種方法：

* 更新資料庫，將所有現有的使用者標示為已確認。
* 確認現有的使用者。 例如，batch-傳送含有確認連結的電子郵件。

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a>Prerequisites

[.NET Core 2.2 SDK 或更新版本](https://dotnet.microsoft.com/download/dotnet-core)

## <a name="create-a-web--app-and-scaffold-identity"></a>建立 web 應用程式和 scaffold 身分識別

執行下列命令來建立具有驗證的 web 應用程式。

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a>測試新的使用者註冊

執行應用程式，選取 [**註冊**] 連結，然後註冊使用者。 此時，電子郵件上的唯一驗證是使用[`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。 提交註冊之後，您會登入應用程式。 稍後在本教學課程中，會更新程式碼，讓新使用者在驗證電子郵件之前無法登入。

[!INCLUDE[](~/includes/view-identity-db.md)]

請注意，資料表的 [`EmailConfirmed`] 欄位是 `False`。

當應用程式傳送確認電子郵件時，您可能會想要在下一個步驟中再次使用此電子郵件。 以滑鼠右鍵按一下資料列，然後選取 [**刪除**]。 刪除電子郵件別名可讓您更輕鬆地進行下列步驟。

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a>需要電子郵件確認

最佳做法是確認新使用者註冊的電子郵件。 電子郵件確認有助於確認他們不會模擬他人（也就是他們尚未向他人的電子郵件登錄）。 假設您有討論論壇，而您想要防止「yli@example.com」註冊為「nolivetto@contoso.com」。 若未確認電子郵件，「nolivetto@contoso.com」可能會從您的應用程式收到不必要的電子郵件。 假設使用者不小心註冊為「ylo@example.com」，而且未注意到 "yli" 拼錯。 因為應用程式沒有正確的電子郵件，所以無法使用密碼復原。 電子郵件確認為 bot 提供了有限的保護。 電子郵件確認無法保護具有許多電子郵件帳戶的惡意使用者。

您通常會想要防止新的使用者將任何資料張貼到您的網站，然後才會有已確認的電子郵件。

更新 `Startup.ConfigureServices` 以要求已確認的電子郵件：

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

`config.SignIn.RequireConfirmedEmail = true;` 可防止已註冊的使用者登入，直到確認其電子郵件為止。

### <a name="configure-email-provider"></a>設定電子郵件提供者

在本教學課程中， [SendGrid](https://sendgrid.com)是用來傳送電子郵件。 您需要 SendGrid 帳戶和金鑰，才能傳送電子郵件。 您可以使用其他電子郵件提供者。 ASP.NET Core 2.x 包含 `System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。 建議您使用 SendGrid 或其他電子郵件服務來傳送電子郵件。 SMTP 很容易保護和設定正確。

建立類別來提取安全的電子郵件金鑰。 針對此範例，請建立*Services/AuthMessageSenderOptions .cs*：

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>設定 SendGrid 使用者秘密

使用 [[密碼管理員] 工具](xref:security/app-secrets)來設定 `SendGridUser` 和 `SendGridKey`。 例如：

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

在 Windows 中，秘密管理員會將金鑰/值組儲存在 `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` 目錄的*秘密 json*檔案中。

不會加密*秘密 json*檔案的內容。 下列標記顯示秘密的*json*檔案。 `SendGridKey` 值已移除。

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

如需詳細資訊，請參閱[選項模式](xref:fundamentals/configuration/options)[和設定](xref:fundamentals/configuration/index)。

### <a name="install-sendgrid"></a>安裝 SendGrid

本教學課程說明如何透過[SendGrid](https://sendgrid.com/)新增電子郵件通知，但您可以使用 SMTP 和其他機制來傳送電子郵件。

安裝 `SendGrid` NuGet 套件：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

從 [套件管理員主控台] 中，輸入下列命令：

```powershell
Install-Package SendGrid
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從主控台，輸入下列命令：

```dotnetcli
dotnet add package SendGrid
```

---

請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)來註冊免費的 SendGrid 帳戶。

### <a name="implement-iemailsender"></a>執行 IEmailSender

若要執行 `IEmailSender`，請使用與下列類似的程式碼來建立*服務/EmailSender* ：

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>設定啟動以支援電子郵件

將下列程式碼新增至*Startup.cs*檔案中的 `ConfigureServices` 方法：

* 將 `EmailSender` 新增為暫時性服務。
* 註冊 `AuthMessageSenderOptions` 設定實例。

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>啟用帳戶確認和密碼復原

此範本具有帳戶確認和密碼復原的程式碼。 尋找 [*區域/身分識別/頁面/帳戶/* 暫存器 .cs] 中的 `OnPostAsync` 方法。

將下列這行批註化，以防止新註冊的使用者自動登入：

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

會顯示完整的方法，並反白顯示已變更的行：

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>註冊、確認電子郵件和重設密碼

執行 web 應用程式，並測試帳戶確認和密碼復原流程。

* 執行應用程式並註冊新的使用者
* 檢查您的電子郵件以取得帳戶確認連結。 如果您沒有收到電子郵件，請參閱[Debug email](#debug) 。
* 按一下連結以確認您的電子郵件。
* 使用您的電子郵件和密碼登入。
* 登出。

### <a name="view-the-manage-page"></a>查看 [管理] 頁面

在瀏覽器中選取您的使用者名稱，](accconfirm/_static/un.png) 使用者名稱 ![瀏覽器視窗。

[管理] 頁面隨即顯示，並選取 [**設定檔**] 索引標籤。 此**電子郵件**會顯示一個核取方塊，指出已確認電子郵件。

### <a name="test-password-reset"></a>測試密碼重設

* 如果您已登入，請選取 [**登出**]。
* 選取 [**登入**] 連結，然後選取 [**忘記密碼？** ] 連結。
* 輸入您用來註冊帳戶的電子郵件。
* 系統會傳送一封電子郵件，其中包含重設密碼的連結。 檢查您的電子郵件，然後按一下連結以重設密碼。 成功重設密碼之後，您就可以使用您的電子郵件和新密碼來登入。

## <a name="change-email-and-activity-timeout"></a>變更電子郵件和活動超時

預設的無活動超時時間為14天。 下列程式碼會將非啟用時間設定為5天：

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a>變更所有資料保護權杖壽命

下列程式碼會將所有資料保護權杖超時時間變更為3小時：

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

內建身分識別使用者權杖（請參閱[AspNetCore/src/Identity/Extensions. Core/src/TokenOptions](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ）具有[一天的超時時間](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。

### <a name="change-the-email-token-lifespan"></a>變更電子郵件權杖生命週期

身分[識別使用者權杖](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)的預設權杖存留期為[一天](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。 本節說明如何變更電子郵件權杖的生命週期。

新增自訂[DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和 <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>：

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

將自訂提供者新增至服務容器：

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a>重新傳送電子郵件確認

請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/5410)。

<a name="debug"></a>

### <a name="debug-email"></a>Debug 電子郵件

如果您無法讓電子郵件正常運作：

* 在 `EmailSender.Execute` 中設定中斷點，以確認呼叫 `SendGridClient.SendEmailAsync`。
* 建立[主控台應用程式，以](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼將電子郵件傳送至 `EmailSender.Execute`。
* 查看[電子郵件活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。
* 檢查您的垃圾郵件資料夾。
* 在不同的電子郵件提供者（Microsoft、Yahoo、Gmail 等）上嘗試另一個電子郵件別名
* 嘗試傳送至不同的電子郵件帳戶。

**安全性最佳做法**是**不要**在測試和開發中使用生產秘密。 如果您將應用程式發行至 Azure，您可以在 Azure Web 應用程式入口網站中將 SendGrid 秘密設定為應用程式設定。 設定系統已設定為從環境變數讀取金鑰。

## <a name="combine-social-and-local-login-accounts"></a>合併社交和本機登入帳戶

若要完成這一節，您必須先啟用外部驗證提供者。 請參閱[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)。

您可以按一下電子郵件連結，以合併本機和社交帳戶。 在下列順序中，會先將 "RickAndMSFT@gmail.com" 建立為本機登入;不過，您可以先將帳戶建立為社交登入，然後再新增本機登入。

![Web 應用程式： RickAndMSFT@gmail.com 使用者已驗證](accconfirm/_static/rick.png)

按一下 [**管理**] 連結。 請注意與此帳戶相關聯的0外部（社交登入）。

![管理檢視](accconfirm/_static/manage.png)

按一下另一個登入服務的連結，並接受應用程式要求。 在下圖中，Facebook 是外部驗證提供者：

![管理您的外部登入視圖清單 Facebook](accconfirm/_static/fb.png)

已合併這兩個帳戶。 您可以使用任一帳戶登入。 您可能想要讓使用者新增本機帳戶，以防其社交登入驗證服務關閉，或更有可能失去其社交帳戶的存取權。

## <a name="enable-account-confirmation-after-a-site-has-users"></a>在網站有使用者之後啟用帳戶確認

在網站上啟用帳戶確認，使用者會鎖定所有現有的使用者。 現有的使用者會被鎖定，因為他們的帳戶不會確認。 若要解決現有的使用者鎖定，請使用下列其中一種方法：

* 更新資料庫，將所有現有的使用者標示為已確認。
* 確認現有的使用者。 例如，batch-傳送含有確認連結的電子郵件。

::: moniker-end
