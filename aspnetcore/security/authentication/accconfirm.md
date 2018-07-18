---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 8e175cd19ca4a9de1e7cf6b330b3d82f309b6501
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063334"
---
::: moniker range="<= aspnetcore-2.0"

請參閱[此 PDF 檔案](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 和 2.1 版。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>帳戶確認和 ASP.NET Core 中的密碼復原

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)

本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。 本教學課程**不**開頭主題。 您應該先熟悉：

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [驗證](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a>建立 web 應用程式，並建立身分識別的結構

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* 在 Visual Studio 中，建立新**Web 應用程式**專案。
* 選取  **ASP.NET Core 2.1**。
* 保留預設值**驗證**設為**不需要驗證**。 下一個步驟中加入驗證。

在下一個步驟：

* 將版面配置頁面設為 *~/Pages/Shared/_Layout.cshtml*
* 選取*帳戶/註冊*
* 建立新**資料內容類別**

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

執行`dotnet aspnet-codegenerator identity --help`scaffolding 工具取得說明。

------

請依照下列中的指示[啟用驗證](xref:security/authentication/scaffold-identity#useauthentication):

* 新增`app.UseAuthentication();`至 `Startup.Configure`
* 新增`<partial name="_LoginPartial" />`和配置檔案。

## <a name="test-new-user-registration"></a>測試新的使用者註冊

執行應用程式，請選取**註冊**連結，並註冊的使用者。 此時，唯一的驗證電子郵件是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。 提交註冊之後, 您登入應用程式。 稍後在教學課程中，會將程式碼更新讓新的使用者無法登入，直到其電子郵件會進行驗證。

## <a name="view-the-identity-database"></a>檢視識別資料庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* 從**檢視**功能表上，選取**SQL Server 物件總管**(SSOX)。
* 瀏覽至 **(localdb) (SQL Server 13) MSSQLLocalDB**。 以滑鼠右鍵按一下**dbo。AspNetUsers** > **檢視資料**:

![SQL Server 物件總管] 中的 [AspNetUsers 資料表上的操作功能表](accconfirm/_static/ssox.png)

請注意，資料表的`EmailConfirmed`欄位是`False`。

您可能想要這封電子郵件一次的下一個步驟時使用的應用程式會傳送確認電子郵件。 以滑鼠右鍵按一下資料列，然後選取**刪除**。 刪除電子郵件別名更容易在下列步驟。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

請參閱[ASP.NET Core MVC 專案中使用 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)如需有關如何檢視 SQLite 資料庫。

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>需要電子郵件確認

您最好確認新的使用者註冊的電子郵件。 電子郵件確認可協助您確認它們無法模擬其他人 （也就是尚未註冊使用其他人的電子郵件）。 假設您有討論論壇，而且您想要防止 「yli@example.com"中註冊為 「nolivetto@contoso.com"。 而不需要電子郵件確認"nolivetto@contoso.com」 無法從您的應用程式收到不想要的電子郵件。 假設使用者不小心註冊為 「ylo@example.com"並還沒有發現..."yli 」 的拼字錯誤。 他們將無法使用密碼復原，因為應用程式沒有正確的電子郵件。 電子郵件確認 bot 提供有限的保護。 電子郵件確認不會提供保護，防範惡意使用者與許多電子郵件帳戶。

您通常想要防止新使用者之前確認電子郵件張貼到您的網站的任何資料。

更新*Areas/Identity/IdentityHostingStartup.cs*要求確認電子郵件：

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認其電子郵件。

### <a name="configure-email-provider"></a>設定電子郵件提供者

在本教學課程中， [SendGrid](https://sendgrid.com)用來傳送電子郵件。 您需要的 SendGrid 帳戶和金鑰來傳送電子郵件。 您可以使用其他電子郵件提供者。 ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。 我們建議您可以使用 SendGrid 或另一個電子郵件服務傳送電子郵件。 SMTP 是難以保護，並正確設定。

[選項模式](xref:fundamentals/configuration/options)用來存取使用者帳戶和金鑰設定。 如需詳細資訊，請參閱 <<c0> [ 組態](xref:fundamentals/configuration/index)。

建立類別來擷取安全的電子郵件的金鑰。 此範例中，建立*Services/AuthMessageSenderOptions.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>設定 SendGrid 使用者祕密

新增的唯一`<UserSecretsId>`值`<PropertyGroup>`專案檔的項目：

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

設定`SendGridUser`並`SendGridKey`具有[secret manager 工具](xref:security/app-secrets)。 例如: 

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

在 Windows、 Secret Manager 儲存中的索引鍵/值組*secrets.json*檔案中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。

內容*secrets.json*檔案未加密。 *Secrets.json*如下所示的檔案 (`SendGridKey`已移除值。)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a>安裝 SendGrid

本教學課程示範如何將透過電子郵件通知[SendGrid](https://sendgrid.com/)，但您可以傳送電子郵件使用 SMTP 和其他機制。

安裝`SendGrid`NuGet 套件：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

從 [套件管理員] 主控台中，輸入下列命令：

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

在主控台中，輸入下列命令：

```cli
dotnet add package SendGrid
```

------

請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)報名免費的 SendGrid 帳戶。
### <a name="implement-iemailsender"></a>實作 IEmailSender

實作`IEmailSender`，建立*Services/EmailSender.cs*與下列類似的程式碼：

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>設定啟動，以支援電子郵件

將下列程式碼加入`ConfigureServices`方法中的*Startup.cs*檔案：

* 新增`EmailSender`作為單一服務。
* 註冊`AuthMessageSenderOptions`組態執行個體。

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>啟用帳戶確認和密碼復原

範本會將程式碼進行帳戶確認和密碼復原。 尋找`OnPostAsync`方法中的*Areas/Identity/Pages/Account/Register.cshtml.cs*。

防止新註冊的使用者自動記錄的標記為註解下面這一行：

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

使用已變更反白顯示的列，會顯示完整的方法：

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>註冊、 確認電子郵件，以及重設密碼

執行 web 應用程式，並測試的帳戶確認和密碼復原流程。

* 執行應用程式並註冊新的使用者

  ![Web 應用程式註冊帳戶檢視](accconfirm/_static/loginaccconfirm1.png)

* 請檢查您的帳戶確認連結的電子郵件。 請參閱[偵錯電子郵件](#debug)如果您沒有收到電子郵件。
* 按一下連結，以確認您的電子郵件。
* 登入您的電子郵件和密碼。
* 登出。

### <a name="view-the-manage-page"></a>檢視 [管理] 頁面

在瀏覽器中，選取您的使用者名稱：![瀏覽器視窗中的使用使用者名稱](accconfirm/_static/un.png)

您可能需要展開的導覽列，以查看使用者名稱。

![導覽列](accconfirm/_static/x.png)

[管理] 頁面會顯示**設定檔**選取的索引標籤。 **電子郵件**顯示核取方塊，指出電子郵件已確認。

### <a name="test-password-reset"></a>測試密碼重設

* 如果您的登入，請選取**登出**。
* 選取 **登入**連結，然後選取**忘記密碼？** 連結。
* 輸入您用來註冊帳戶的電子郵件。
* 會傳送具有重設密碼連結的電子郵件。 請檢查您的電子郵件，然後按一下 重設密碼連結。 已成功重設您的密碼之後，您可以登入您的電子郵件和新密碼。

<a name="debug"></a>

### <a name="debug-email"></a>偵錯電子郵件

如果您無法收到電子郵件工作：

* 在 設定中斷點`EmailSender.Execute`若要確認`SendGridClient.SendEmailAsync`呼叫。
* 建立[主控台應用程式以傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼以`EmailSender.Execute`。
* 檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。
* 請檢查垃圾郵件資料夾。
* 請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等） 上的另一個電子郵件別名
* 請嘗試傳送到不同的電子郵件帳戶。

**安全性最佳作法**旨在**不**使用生產環境中開發和測試的祕密。 如果您將應用程式發佈至 Azure 時，您可以為 Azure Web 應用程式入口網站中的應用程式設定來設定 SendGrid 祕密。 設定系統設定來讀取環境變數中的索引鍵。

## <a name="combine-social-and-local-login-accounts"></a>結合社交和本機登入帳戶

若要完成本節中，您必須先啟用外部驗證提供者。 請參閱[Facebook、 Google、 與外部提供者驗證](xref:security/authentication/social/index)。

您可以結合本機和社交帳戶，按一下您的電子郵件連結。 依下列順序，"RickAndMSFT@gmail.com」 會先建立做為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後新增的本機登入。

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

按一下 **管理**連結。 請注意 0 外部 （社交登入） 與此帳戶相關聯。

![管理檢視](accconfirm/_static/manage.png)

按一下連結至另一個登入服務並接受應用程式要求。 在下圖中，Facebook 會是外部驗證提供者：

![管理您的外部登入檢視，列出 Facebook](accconfirm/_static/fb.png)

已合併兩個帳戶。 您可以使用任一個帳戶登入。 您可能想要新增本機帳戶，以免其社交登入驗證服務已關閉，或它們可能比較無法存取其社交帳戶使用者。

## <a name="enable-account-confirmation-after-a-site-has-users"></a>站台後為使用者啟用帳戶確認

在網站上啟用的帳戶確認，使用者會鎖定所有現有的使用者。 現有使用者遭到鎖定中，這是因為未確認他們的帳戶。 若要解決現有的使用者鎖定，請使用下列方法之一：

* 更新將標示為正在確認所有現有的使用者資料庫。
* 請確認現有的使用者。 例如，批次傳送電子郵件以確認連結。

::: moniker-end