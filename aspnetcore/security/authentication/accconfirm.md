---
title: "帳戶確認和 ASP.NET Core 中的密碼復原"
author: rick-anderson
description: "示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。"
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 14c7fdfc1ed8b87aac8ca937298c7da6373bf06d
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>帳戶確認和 ASP.NET Core 中的密碼復原

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette) 

本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。

## <a name="create-a-new-aspnet-core-project"></a>建立新的 ASP.NET Core 專案

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

這個步驟適用於在 Windows 上的 Visual Studio。 請參閱下一節的 CLI 指示。

此教學課程需要 Visual Studio 2017 Preview 2 或更新版本。

* 在 Visual Studio 中建立新的 Web 應用程式專案。
* 選取**ASP.NET Core 2.0**。 下圖顯示**.NET Core**選取，但您可以選取**.NET Framework**。
* 選取**變更驗證**並將設定為**個別使用者帳戶**。
* 保留預設值**儲存使用者帳戶在應用程式**。

![顯示選取"個別使用者帳戶 radio"的新 [專案] 對話方塊](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

此教學課程需要 2017年或更新版本的 Visual Studio。

* 在 Visual Studio 中建立新的 Web 應用程式專案。
* 選取**變更驗證**並將設定為**個別使用者帳戶**。

![顯示選取"個別使用者帳戶 radio"的新 [專案] 對話方塊](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>.NET core CLI 專案建立 macOS 和 Linux

如果您使用 CLI 或 SQLite，在命令視窗執行下列命令：

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`指定的個別使用者帳戶的範本。
* 在 Windows 中，加入`-uld`選項。 `-uld`選項建立 LocalDB 的連接字串，而不是 SQLite 資料庫。
* 執行`new mvc --help`取得此命令的說明。

## <a name="test-new-user-registration"></a>測試新的使用者註冊

執行應用程式中，選取**註冊**連結，並登錄使用者。 請依照下列指示執行 Entity Framework Core 移轉。 此時，只有在電子郵件時才驗證與[[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。 提交註冊後，您登入的應用程式。 稍後在教學課程中，我們將會變更這讓新的使用者無法登入，直到他們的電子郵件已通過驗證。

## <a name="view-the-identity-database"></a>檢視識別資料庫

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* 從**檢視**功能表上，選取**SQL Server 物件總管**(SSOX)。 
* 瀏覽至**(localdb) (SQL Server 13) MSSQLLocalDB**。 以滑鼠右鍵按一下**dbo。AspNetUsers** > **檢視資料**:

![SQL Server 物件總管 中的 AspNetUsers 資料表上的內容功能表](accconfirm/_static/ssox.png)

請注意`EmailConfirmed`欄位是`False`。

您可以在這封電子郵件一次在下一個步驟時使用應用程式會傳送確認電子郵件。 以滑鼠右鍵按一下資料列，然後選取**刪除**。 刪除電子郵件別名現在更容易在下列步驟。

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

請參閱[在 ASP.NET Core MVC 專案中使用的 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)如需有關如何檢視 SQLite DB 指示。 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>需要 SSL 和 SSL 設定 IIS Express

請參閱[強制 SSL](xref:security/enforcing-ssl)。

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>需要電子郵件確認

若要確認新的使用者註冊，以確認它們不模擬其他人的電子郵件的最佳作法是 （也就是它們未向其他人的電子郵件）。 假設您有討論論壇，而且您想要防止 「yli@example.com"從登錄為"nolivetto@contoso.com。 」 電子郵件確認沒有 「nolivetto@contoso.com"無法從您的應用程式取得垃圾電子郵件。 假設使用者不小心註冊為 「ylo@example.com"而且未注意到拼字錯誤的 「 yli 」，它們就無法使用密碼復原，因為應用程式沒有正確的電子郵件。 電子郵件確認從 bot 提供有限的保護，並不會從擁有多的工作電子郵件別名，他們可以使用登錄來決定垃圾提供保護。

您通常想要讓新的使用者張貼到您的網站的任何資料之前確認電子郵件。 

更新`ConfigureServices`要求確認電子郵件：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
前一行會防止之前確認他們的電子郵件登入已註冊的使用者。 不過，該行不讓新的使用者從他們在註冊後登入。 它們在註冊後，預設程式碼會登入使用者。 一旦他們登出他們將無法再次登入之前他們註冊。 稍後在教學課程中我們將會變更程式碼，因此新註冊的使用者是**不**登入。

### <a name="configure-email-provider"></a>設定電子郵件提供者

在本教學課程，SendGrid 用來傳送電子郵件。 您需要 SendGrid 帳戶和金鑰來傳送電子郵件。 您可以使用其他電子郵件提供者。 ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。 我們建議您傳送電子郵件使用 SendGrid 或另一個電子郵件服務。 SMTP 是難以保護，並正確設定。

[選項模式](xref:fundamentals/configuration/options)用來存取使用者帳戶和金鑰設定。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

建立可擷取保護電子郵件的索引鍵的類別。 此範例中，`AuthMessageSenderOptions`中建立類別*Services/AuthMessageSenderOptions.cs*檔案。

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

設定`SendGridUser`和`SendGridKey`與[密碼管理員工具](../app-secrets.md)。 例如: 

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

在 Windows 中，密碼管理員會儲存您的索引鍵/值組*secrets.json* %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId > 目錄中的檔案。

內容*secrets.json*檔案不會加密。 *Secrets.json*檔案如下所示 (`SendGridKey`已移除值。)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>設定要使用 AuthMessageSenderOptions 啟動

新增`AuthMessageSenderOptions`至結尾處的服務容器`ConfigureServices`方法中的*Startup.cs*檔案：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>設定 AuthMessageSender 類別

本教學課程示範如何透過電子郵件通知加入[SendGrid](https://sendgrid.com/)，不過您可以傳送電子郵件使用 SMTP 和其他機制。

* 安裝`SendGrid`NuGet 封裝。 從 [封裝管理員] 主控台中，輸入下列下列命令：

  `Install-Package SendGrid`

* 請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)註冊免費的 SendGrid 帳戶。

#### <a name="configure-sendgrid"></a>設定 SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* 將程式碼中的加入*Services/EmailSender.cs*如下所示設定 SendGrid:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* 將程式碼中的加入*Services/MessageServices.cs*如下所示設定 SendGrid:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>啟用帳戶確認和密碼復原

範本的帳戶確認和密碼復原程式碼。 尋找`[HttpPost] Register`方法中的*AccountController.cs*檔案。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

防止新註冊的使用者自動登入註解下列一行：

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

反白顯示的已變更的程式行顯示完整的方法：

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

注意： 先前的程式碼將會失敗如果您實作`IEmailSender`並傳送純文字電子郵件。 請參閱[此問題](https://github.com/aspnet/Home/issues/2152)如需詳細資訊和因應措施。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

取消註解的程式碼，若要啟用帳戶確認。

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

注意： 我們正在也防止新註冊的使用者自動登入註解下列一行：

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

取消註解中的程式碼中啟用密碼復原`ForgotPassword`中的動作*Controllers/AccountController.cs*檔案。

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

請取消註解中的表單項目*Views/Account/ForgotPassword.cshtml*。 您可能想要移除`<p> For more information on how to enable reset password ... </p>`項目，其中包含這篇文章的連結。

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>註冊、 確認電子郵件，及重設密碼

執行 web 應用程式，並測試的帳戶確認和密碼復原流程。

* 執行應用程式並註冊新的使用者

 ![Web 應用程式註冊帳戶檢視](accconfirm/_static/loginaccconfirm1.png)

* 請檢查您的帳戶確認連結的電子郵件。 請參閱[偵錯電子郵件](#debug)如果您未收到電子郵件。
* 按一下連結，以確認您的電子郵件。
* 登入您的電子郵件和密碼。
* 登出。

### <a name="view-the-manage-page"></a>檢視 [管理] 頁面

在瀏覽器中選取您的使用者名稱：![瀏覽器視窗中使用的使用者名稱](accconfirm/_static/un.png)

您可能需要展開以查看使用者名稱導覽列。

![導覽列](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[管理] 頁面會顯示與**設定檔**選取的索引標籤。 **電子郵件**顯示核取方塊，指出電子郵件已確認。 

![管理頁面](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

稍後在本教學課程中，我們將討論此頁面。
![管理頁面](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>測試密碼重設

* 如果您登入，選取**登出**。  
* 選取**登入**連結，並選取**忘記密碼？**連結。
* 輸入您用來註冊帳戶的電子郵件。
* 將會傳送一封電子郵件以重設密碼的連結。 請檢查您的電子郵件並按一下連結以重設密碼。  已成功重設您的密碼之後，您可以使用您的電子郵件和新密碼登入。

<a name="debug"></a>

### <a name="debug-email"></a>偵錯電子郵件

如果您無法取得電子郵件工作：

* 檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。
* 請檢查垃圾郵件資料夾。
* 請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等等） 上的另一個電子郵件別名
* 建立[主控台應用程式傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)。
* 請嘗試傳送到不同的電子郵件帳戶。

**注意：**安全性最佳作法是不要使用實際執行測試和開發中的機密資料。 如果您將應用程式發行至 Azure 時，您可以設定 SendGrid 密碼做為 Azure Web 應用程式入口網站中的應用程式設定。 組態系統是以讀取環境變數中的索引鍵的安裝程式。

## <a name="prevent-login-at-registration"></a>避免在註冊的登入

目前的範本，一旦使用者完成註冊表單中，使用者要登入 （驗證）。 您通常想要確認他們的電子郵件才能加以登入。 下方區段中，我們將會修改程式碼，需要使用者正在登入之前，新的使用者有確認電子郵件。 更新`[HttpPost] Login`中的動作*AccountController.cs*下列反白顯示變更的檔案。

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**注意：**安全性最佳作法是不要使用實際執行測試和開發中的機密資料。 如果您將應用程式發行至 Azure 時，您可以設定 SendGrid 密碼做為 Azure Web 應用程式入口網站中的應用程式設定。 組態系統是以讀取環境變數中的索引鍵的安裝程式。


## <a name="combine-social-and-local-login-accounts"></a>結合社交和本機登入帳戶

注意： 本節僅適用於 ASP.NET Core 1.x。 適用於 ASP.NET Core 2.x，請參閱[這](https://github.com/aspnet/Docs/issues/3753)問題。

若要完成本章節，您必須先啟用外部驗證提供者。 請參閱[啟用驗證使用 Facebook、 Google 和其他外部提供者](social/index.md)。

您可以結合本機和社交帳戶按一下電子郵件連結。 以下列順序，在"RickAndMSFT@gmail.com"第一次建立為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後加入本機的登入。

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

按一下**管理**連結。 請注意 0 外部 （社交登入） 與此帳戶相關聯。

![管理檢視](accconfirm/_static/manage.png)

按一下連結以另一個登入服務，並接受要求的應用程式。 在下面的影像中，Facebook 會是外部驗證提供者：

![管理您列出 Facebook 的外部登入檢視](accconfirm/_static/fb.png)

已合併兩個帳戶。 您可以使用任一個帳戶登入。 您可能會想讓使用者加入本機帳戶，以防使用者社交的記錄檔中驗證服務已關閉，或可能比較他們遺失了存取其社交帳戶。
