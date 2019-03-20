---
title: 使用 ASP.NET Core 中的 SMS 的雙因素驗證
author: rick-anderson
description: 了解如何設定雙因素驗證 (2FA) 與 ASP.NET Core 應用程式。
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 116249a7cd4faebd0c899e383d86f5c5c3c7146a
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265243"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>使用 ASP.NET Core 中的 SMS 的雙因素驗證

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[瑞士開發人員](https://github.com/Swiss-Devs)

>[!WARNING]
> 兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的方法 2FA 的產業。 2FA 使用 TOTP 是慣用的 SMS 2FA。 如需詳細資訊，請參閱 < [TOTP ASP.NET Core 中的驗證器應用程式的啟用 QR 代碼產生](xref:security/authentication/identity-enable-qrcodes)ASP.NET Core 2.0 和更新版本。

本教學課程會示範如何設定使用 SMS 的雙因素驗證 (2FA)。 針對提供的指示[twilio](https://www.twilio.com/)並[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)，但是您可以使用任何其他 SMS 提供者。 我們建議您先完成[帳戶確認和密碼復原](xref:security/authentication/accconfirm)再開始本教學課程。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)。 [如何下載](xref:index#how-to-download-a-sample)。

## <a name="create-a-new-aspnet-core-project"></a>建立新的 ASP.NET Core 專案

建立新的 ASP.NET Core web app，名為`Web2FA`與個別使用者帳戶。 請依照下列中的指示<xref:security/enforcing-ssl>設定，並需要 HTTPS。

### <a name="create-an-sms-account"></a>建立 SMS 帳戶

從建立 SMS 帳戶，例如[twilio](https://www.twilio.com/)或是[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)。 記錄的驗證認證 (twilio: accountSid 和 authToken，如 ASPSMS:使用者金鑰和密碼）。

#### <a name="figuring-out-sms-provider-credentials"></a>找出 SMS 提供者認證

**Twilio:**

從您的 Twilio 帳戶的 [儀表板] 索引標籤中，複製**帳戶 SID**並**驗證權杖**。

**ASPSMS:**

從您的帳戶設定，瀏覽至**Userkey** ，並將其連同複製您**密碼**。

我們稍後會儲存使用中索引鍵的祕密管理員工具的這些值`SMSAccountIdentification`和`SMSAccountPassword`。

#### <a name="specifying-senderid--originator"></a>指定寄件者識別碼 / 建立者

**Twilio:** 從數字] 索引標籤上，複製 [您的 Twilio**電話號碼**。

**ASPSMS:** 在 解除鎖定的建立者 功能表中，解除鎖定一或多個建立者，或選擇 英數字元的建立者 （不支援所有的網路）。

我們稍後會儲存這個值與機碼內的 「 密碼管理員 」 工具`SMSAccountFrom`。

### <a name="provide-credentials-for-the-sms-service"></a>SMS 服務提供的認證

我們將使用[選項模式](xref:fundamentals/configuration/options)存取使用者帳戶和金鑰設定。

* 建立類別來擷取安全的 SMS 金鑰。 此範例中，`SMSoptions`類別中建立*Services/SMSoptions.cs*檔案。

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

設定`SMSAccountIdentification`，`SMSAccountPassword`並`SMSAccountFrom`具有[secret manager 工具](xref:security/app-secrets)。 例如: 

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* 新增 NuGet 套件的 SMS 提供者。 從套件管理員主控台 (PMC) 執行：

**Twilio:**

`Install-Package Twilio`

**ASPSMS:**

`Install-Package ASPSMS`

* 將程式碼中的加入*Services/MessageServices.cs*檔案以啟用 SMS。 使用 Twilio 或 ASPSMS 區段：

**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>設定要使用的啟動 `SMSoptions`

新增`SMSoptions`中的服務容器`ConfigureServices`方法中的*Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>啟用雙因素驗證

開啟*Views/Manage/Index.cshtml* Razor 檢視檔案] 和 [移除註解字元，因此不需要標記的標記為註解）。

## <a name="log-in-with-two-factor-authentication"></a>登入雙重要素驗證

* 執行應用程式並註冊新的使用者

![Web 應用程式在 Microsoft Edge 中開啟檢視的暫存器](2fa/_static/login2fa1.png)

* 點選您的使用者名稱，就會啟動`Index`管理控制器中的動作方法。 然後點選 電話號碼**新增**連結。

![管理檢視： 點選 [新增] 連結](2fa/_static/login2fa2.png)

* 新增電話號碼接收驗證碼，並點選**傳送驗證碼**。

![新增電話號碼 頁面](2fa/_static/login2fa3.png)

* 您會收到驗證碼的簡訊。 請輸入，並點選 **提交**

![確認電話號碼 頁面](2fa/_static/login2fa4.png)

如果您未收到簡訊，請參閱 twilio [記錄] 頁面。

* [管理] 檢視會顯示已成功新增您的電話號碼。

![管理檢視-已成功新增電話號碼](2fa/_static/login2fa5.png)

* 點選**啟用**啟用雙因素驗證。

![管理檢視-啟用雙因素驗證](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>測試兩個雙因素驗證

* 登出。

* 登入。

* 使用者帳戶已啟用雙因素驗證，因此您需要提供第二因素驗證。 在本教學課程中，您已啟用電話驗證。 內建的範本也可讓您設定電子郵件做為第二個因素。 您可以設定其他的第二個因素，例如 QR 代碼的驗證。 點選**提交**。

![傳送驗證碼 檢視](2fa/_static/login2fa7.png)

* 輸入您在簡訊中收到的程式碼。

* 按一下**記住此瀏覽器**核取方塊會排除您不需要使用 2FA 登入，使用相同的裝置和瀏覽器時。 啟用 2FA，然後按一下**記住此瀏覽器**會為您提供強式 2FA 保護避免惡意使用者嘗試存取您的帳戶，只要它們不能存取您的裝置。 您可以在任何您經常使用的私用裝置上執行這項操作。 藉由設定**記住此瀏覽器**2FA 的額外的安全性獲得不定期使用，您的裝置，取得上不需要經歷 2FA，在您自己的裝置上的便利性。

![確認檢視](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>防止暴力密碼破解攻擊的帳戶鎖定

帳戶鎖定被建議使用於 2FA。 一旦使用者登入時透過本機帳戶或社交帳戶，則會儲存 2FA 在每次失敗的嘗試。 如果達到最大失敗的存取嘗試時，使用者會被封鎖 (預設值：5 分鐘後鎖定 5 存取嘗試失敗）。 驗證成功重設失敗的存取嘗試次數和重設時鐘。 存取嘗試失敗的最大值，而且可以使用設定鎖定時間[MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts)並[DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)。 下列組態會設定帳戶鎖定的存取嘗試失敗 10 後 10 分鐘的時間：

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

確認[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)設定`lockoutOnFailure`到`true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
