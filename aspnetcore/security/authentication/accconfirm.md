---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b6dbe234973431448c18d3cc82a6ac98d4f53a3b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730447"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>帳戶確認和 ASP.NET Core 中的密碼復原

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)

本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。 這個教學課程**不**開頭主題。 您應該熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [驗證](xref:security/authentication/index)
* [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

請參閱[此 PDF 檔案](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC 1.1 和 2.x 版。

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>建立新的 ASP.NET Core 專案，而.NET Core CLI

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` 指定個別的使用者帳戶的專案範本。
* 在 Windows 中，加入`-uld`選項。 它會指定應該使用 LocalDB，而不是 SQLite。
* 執行`new mvc --help`取得此命令的說明。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果您使用 CLI 或 SQLite，在命令視窗執行下列命令：

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` 指定個別的使用者帳戶的專案範本。
* 在 Windows 中，加入`-uld`選項。 它會指定應該使用 LocalDB，而不是 SQLite。
* 執行`new mvc --help`取得此命令的說明。

---

或者，您可以使用 Visual Studio 建立新的 ASP.NET Core 專案：

* 在 Visual Studio 中，建立新**Web 應用程式**專案。
* 選取**ASP.NET Core 2.0**。 **.NET core**選取在下列影像中，但您可以選取 **.NET Framework**。
* 選取**變更驗證**並將設定為**個別使用者帳戶**。
* 保留預設值**儲存使用者帳戶在應用程式**。

![顯示選取"個別使用者帳戶 radio"的新 [專案] 對話方塊](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>測試新的使用者註冊

執行應用程式中，選取**註冊**連結，並登錄使用者。 請依照下列指示執行 Entity Framework Core 移轉。 此時，只有在電子郵件時才驗證與[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。 在提交註冊之後, 您登入應用程式。 稍後在教學課程中，會更新程式碼，所以新的使用者無法登入，直到他們的電子郵件已通過驗證。

## <a name="view-the-identity-database"></a>檢視識別資料庫

請參閱[ASP.NET Core MVC 專案中使用 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)如需如何檢視 SQLite 資料庫的指示。

Visual studio:

* 從**檢視**功能表上，選取**SQL Server 物件總管**(SSOX)。
* 瀏覽至 **(localdb) (SQL Server 13) MSSQLLocalDB**。 以滑鼠右鍵按一下**dbo。AspNetUsers** > **檢視資料**:

![SQL Server 物件總管 中的 AspNetUsers 資料表上的內容功能表](accconfirm/_static/ssox.png)

請注意，資料表的`EmailConfirmed`欄位是`False`。

您可以在這封電子郵件一次在下一個步驟時使用應用程式會傳送確認電子郵件。 以滑鼠右鍵按一下資料列，然後選取**刪除**。 刪除電子郵件別名容易在下列步驟。

---

## <a name="require-https"></a>需要 HTTPS

請參閱[需要 HTTPS](xref:security/enforcing-ssl)。

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>需要電子郵件確認

最好確認新的使用者註冊的電子郵件。 確認可協助確認它們不模擬其他人的電子郵件 （也就是它們未向其他人的電子郵件）。 假設您有討論論壇，而且您想要防止 「yli@example.com"從登錄為"nolivetto@contoso.com"。 電子郵件確認沒有 「nolivetto@contoso.com"無法從您的應用程式接收垃圾電子郵件。 假設使用者不小心註冊為 「ylo@example.com"而且未注意到的"yli 」 的拼字錯誤。 它們就無法使用密碼復原，因為應用程式沒有正確的電子郵件。 電子郵件確認從 bot 提供有限的保護。 電子郵件確認不提供保護，防範惡意使用者與多個電子郵件帳戶。

您通常想要讓新的使用者張貼到您的網站的任何資料之前確認電子郵件。

更新`ConfigureServices`要求確認電子郵件：

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認他們的電子郵件。

### <a name="configure-email-provider"></a>設定電子郵件提供者

在本教學課程，SendGrid 用來傳送電子郵件。 您需要 SendGrid 帳戶和金鑰來傳送電子郵件。 您可以使用其他電子郵件提供者。 ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。 我們建議您傳送電子郵件使用 SendGrid 或另一個電子郵件服務。 SMTP 是難以保護，並正確設定。

[選項模式](xref:fundamentals/configuration/options)用來存取使用者帳戶和金鑰設定。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

建立可擷取保護電子郵件的索引鍵的類別。 此範例中，`AuthMessageSenderOptions`中建立類別*Services/AuthMessageSenderOptions.cs*檔案：

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

設定`SendGridUser`和`SendGridKey`與[密碼管理員工具](xref:security/app-secrets)。 例如: 

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

在 Windows 中，密碼管理員存放中的索引鍵/值組*secrets.json*檔案`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。

內容*secrets.json*檔案未加密。 *Secrets.json*檔案如下所示 (`SendGridKey`已移除值。)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>設定要使用 AuthMessageSenderOptions 啟動

新增`AuthMessageSenderOptions`至結尾處的服務容器`ConfigureServices`方法中的*Startup.cs*檔案：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>設定 AuthMessageSender 類別

本教學課程示範如何透過電子郵件通知加入[SendGrid](https://sendgrid.com/)，不過您可以傳送電子郵件使用 SMTP 和其他機制。

安裝`SendGrid`NuGet 封裝：

* 從命令列：

    `dotnet add package SendGrid`

* 從 [封裝管理員] 主控台中，輸入下列命令：

  `Install-Package SendGrid`

請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)註冊免費的 SendGrid 帳戶。

#### <a name="configure-sendgrid"></a>設定 SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

若要設定 SendGrid，將類似下列的程式碼加入*Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* 將程式碼中的加入*Services/MessageServices.cs*如下所示設定 SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>啟用帳戶確認和密碼復原

範本的帳戶確認和密碼復原程式碼。 尋找`OnPostAsync`方法中的*Pages/Account/Register.cshtml.cs*。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

防止新註冊的使用者自動登入註解下列一行：

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

反白顯示的已變更的程式行顯示完整的方法：

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

若要啟用帳戶確認，請取消註解下列程式碼：

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**注意：** 程式碼會使得新註冊的使用者無法自動登入註解下列一行：

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

取消註解中的程式碼中啟用密碼復原`ForgotPassword`動作*Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

請取消註解中的表單項目*Views/Account/ForgotPassword.cshtml*。 您可能想要移除`<p> For more information on how to enable reset password ... </p>`元素，其中包含這篇文章的連結。

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

這是稍後在本教學課程中所述。
![管理頁面](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>測試密碼重設

* 如果您登入，選取**登出**。
* 選取**登入**連結，並選取**忘記密碼？** 連結。
* 輸入您用來註冊帳戶的電子郵件。
* 會傳送一封電子郵件以重設密碼的連結。 請檢查您的電子郵件並按一下連結以重設密碼。 已成功重設您的密碼之後，您可以登入您的電子郵件和新密碼。

<a name="debug"></a>

### <a name="debug-email"></a>偵錯電子郵件

如果您無法取得電子郵件工作：

* 建立[主控台應用程式傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)。
* 檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。
* 請檢查垃圾郵件資料夾。
* 請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等等） 上的另一個電子郵件別名
* 請嘗試傳送到不同的電子郵件帳戶。

**安全性最佳作法**是**不**使用生產環境中測試和開發的密碼。 如果您將應用程式發行至 Azure 時，您可以設定 SendGrid 密碼做為 Azure Web 應用程式入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。

## <a name="combine-social-and-local-login-accounts"></a>結合社交和本機登入帳戶

若要完成本章節，您必須先啟用外部驗證提供者。 請參閱[Facebook、 Google、 和外部提供者驗證](xref:security/authentication/social/index)。

您可以結合本機和社交帳戶按一下電子郵件連結。 以下列順序，在"RickAndMSFT@gmail.com"第一次建立為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後加入本機的登入。

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

按一下**管理**連結。 請注意 0 外部 （社交登入） 與此帳戶相關聯。

![管理檢視](accconfirm/_static/manage.png)

按一下連結以另一個登入服務，並接受要求的應用程式。 在下圖中，Facebook 會是外部驗證提供者：

![管理您列出 Facebook 的外部登入檢視](accconfirm/_static/fb.png)

已合併兩個帳戶。 也可以使用任一個帳戶登入。 您可能想要其社交登入驗證服務已關閉，或可能比較他們遺失了存取其社交帳戶加入本機帳戶使用者。

## <a name="enable-account-confirmation-after-a-site-has-users"></a>啟用帳戶確認之後某個網站的使用者

啟用帳戶確認站台的使用者與鎖定所有現有的使用者。 現有的使用者遭到鎖定，因為未確認他們的帳戶。 若要解決現有的使用者鎖定，請使用下列方法之一：

* 若要將標示為正在確認的所有現有使用者資料庫更新。
* 請確認現有的使用者。 例如，批次傳送電子郵件以確認連結。
