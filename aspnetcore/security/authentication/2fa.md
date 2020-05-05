---
title: 在 ASP.NET Core 中使用 SMS 的雙因素驗證
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 應用程式來設定雙因素驗證（2FA）。
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/2fa
ms.openlocfilehash: e33f22356de983c8c4e0211822d5027a33b48de6
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775826"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>在 ASP.NET Core 中使用 SMS 的雙因素驗證

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[瑞士開發人員](https://github.com/Swiss-Devs)

>[!WARNING]
> 使用以時間為基礎的一次性密碼演算法（TOTP）的雙因素驗證（2FA）驗證器應用程式是2FA 的業界建議方法。 2FA 使用 TOTP 是 SMS 2FA 的慣用選項。 如需詳細資訊，請參閱在 ASP.NET Core for ASP.NET Core 2.0 和更新版本[中啟用 TOTP 驗證器應用程式的 QR 代碼產生](xref:security/authentication/identity-enable-qrcodes)。

本教學課程說明如何使用 SMS 來設定雙因素驗證（2FA）。 系統會提供[twilio](https://www.twilio.com/)和[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)的指示，但是您可以使用任何其他 SMS 提供者。 我們建議您在開始本教學課程之前，先完成[帳戶確認和密碼](xref:security/authentication/accconfirm)復原。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)。 [如何下載](xref:index#how-to-download-a-sample)。

## <a name="create-a-new-aspnet-core-project"></a>建立新的 ASP.NET Core 專案

使用個別使用者帳戶建立名為`Web2FA`的新 ASP.NET Core web 應用程式。 依照中<xref:security/enforcing-ssl>的指示進行設定，並要求 HTTPS。

### <a name="create-an-sms-account"></a>建立 SMS 帳戶

建立 SMS 帳戶，例如，從[twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)。 記錄驗證認證（適用于 twilio： accountSid 和 authToken，適用于 ASPSMS： Userkey 和 Password）。

#### <a name="figuring-out-sms-provider-credentials"></a>找出 SMS 提供者認證

**Twilio**

從 Twilio 帳戶的 [儀表板] 索引標籤中，複製 [**帳戶 SID** ] 和 [**驗證權杖**]。

**ASPSMS:**

從您的帳戶設定中，流覽至**Userkey** ，並將它與您的**密碼**一起複製。

稍後我們會將這些值儲存在中，並使用金鑰`SMSAccountIdentification`和`SMSAccountPassword`中的秘密管理員工具。

#### <a name="specifying-senderid--originator"></a>指定 SenderID/發信者

**Twilio：** 在 [數位] 索引標籤中，複製您的 Twilio**電話號碼**。

**ASPSMS：** 在 [解除鎖定擁有者] 功能表中，解除鎖定一或多個始發者，或選擇英數位元（並非所有網路都支援）。

我們稍後將使用金鑰`SMSAccountFrom`中的「秘密管理員」工具來儲存此值。

### <a name="provide-credentials-for-the-sms-service"></a>提供 SMS 服務的認證

我們將使用 [[選項] 模式](xref:fundamentals/configuration/options)來存取使用者帳戶和金鑰設定。

* 建立類別來提取安全 SMS 金鑰。 針對此範例，會`SMSoptions`在*Services/SMSoptions .cs*檔案中建立類別。

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

`SMSAccountIdentification`使用 [[密碼管理員] 工具](xref:security/app-secrets)設定`SMSAccountPassword`和`SMSAccountFrom` 。 例如：

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* 新增 SMS 提供者的 NuGet 套件。 從 [套件管理員主控台] （PMC）執行：

**Twilio**

`Install-Package Twilio`

**ASPSMS:**

`Install-Package ASPSMS`

* 在*Services/MessageServices .cs*檔案中新增程式碼，以啟用 SMS。 請使用 Twilio 或 ASPSMS 區段：

**Twilio**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>設定要使用的啟動`SMSoptions`

在`SMSoptions` `ConfigureServices` *Startup.cs*的方法中，將新增至服務容器：

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>啟用雙因素驗證

開啟*Views/Manage/Index. cshtml* Razor視圖檔案並移除批註字元（因此不會註解標記）。

## <a name="log-in-with-two-factor-authentication"></a>使用雙因素驗證進行登入

* 執行應用程式並註冊新的使用者

![在 Microsoft Edge 中開啟 Web 應用程式註冊視圖](2fa/_static/login2fa1.png)

* 在您的使用者名稱上按一下，即可`Index`在 [管理控制器] 中啟用動作方法。 然後，按一下 [電話號碼] [**新增**] 連結。

![管理檢視-點一下 [新增] 連結](2fa/_static/login2fa2.png)

* 新增將接收驗證碼的電話號碼，然後按一下 [**傳送驗證碼**]。

![[新增電話號碼] 頁面](2fa/_static/login2fa3.png)

* 您會收到包含驗證碼的文字訊息。 輸入它，然後按 [**提交**]

![驗證電話號碼頁面](2fa/_static/login2fa4.png)

如果您沒有收到文字訊息，請參閱 twilio 記錄頁面。

* [管理] 視圖顯示已成功新增您的電話號碼。

![管理檢視-已成功新增電話號碼](2fa/_static/login2fa5.png)

* 請按 [**啟用**] 以啟用雙因素驗證。

![管理 view-啟用雙因素驗證](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>測試雙因素驗證

* 登出。

* 登入。

* 使用者帳戶已啟用雙因素驗證，因此您必須提供第二個驗證要素。 在本教學課程中，您已啟用電話驗證。 內建範本也可讓您設定電子郵件作為第二個因素。 您可以針對驗證（例如 QR 代碼）設定其他第二個因素。 請按 [**提交**]。

![傳送驗證碼視圖](2fa/_static/login2fa7.png)

* 輸入您在 SMS 訊息中取得的程式碼。

* 按一下 [**記住此瀏覽器**] 核取方塊，可讓您不需要在使用相同的裝置和瀏覽器時，使用2FA 登入。 啟用2FA 並按一下 [**記住此瀏覽器**] 可提供您強式2FA 保護，讓惡意使用者嘗試存取您的帳戶，只要他們無法存取您的裝置即可。 您可以在您經常使用的任何私人裝置上執行此動作。 藉由設定 [**記住此瀏覽器**]，您可以從不常使用的裝置獲得更2FA 的安全性，並讓您不必在自己的裝置上進行2FA。

![確認視圖](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>防止暴力密碼破解攻擊的帳戶鎖定

建議使用2FA 來鎖定帳戶。 使用者透過本機帳戶或社交帳戶登入之後，就會在2FA 中儲存每個失敗的嘗試。 如果達到失敗的存取嘗試次數上限，則使用者會被鎖定（預設值：5分鐘的鎖定嘗試失敗存取次數）。 成功的驗證會重設失敗的存取嘗試計數，並重設時鐘。 失敗的存取嘗試和鎖定時間上限可以使用[MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts)和[DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)來設定。 以下設定在10次失敗的存取嘗試之後10分鐘的帳戶鎖定：

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

確認[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)設定`lockoutOnFailure`為`true`：

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
