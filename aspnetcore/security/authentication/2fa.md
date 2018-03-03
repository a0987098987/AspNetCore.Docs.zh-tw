---
title: "使用 SMS 雙因素驗證"
author: rick-anderson
description: "示範如何設定 ASP.NET Core 的雙因素驗證 (2FA)"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 721c4c20234c7232b509a0cff444538c2cfeb166
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="two-factor-authentication-with-sms"></a>使用 SMS 雙因素驗證

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[瑞士開發人員](https://github.com/Swiss-Devs)

此教學課程適用於 ASP.NET Core 只 1.x。 請參閱[啟用 QR 代碼產生的驗證器應用程式中 ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 和更新版本。

本教學課程會示範如何設定使用 SMS 進行雙因素驗證 (2FA)。 針對給定指示[twilio](https://www.twilio.com/)和[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)，但是您可以使用任何其他 SMS 提供者。 我們建議您完成[帳戶確認和密碼復原](accconfirm.md)再開始本教學課程。

檢視[完成的範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)。 [如何下載](xref:tutorials/index#how-to-download-a-sample)。

## <a name="create-a-new-aspnet-core-project"></a>建立新的 ASP.NET Core 專案

建立新的 ASP.NET Core web app，名為`Web2FA`與個別使用者帳戶。 請依照下列中的指示[強制執行的 ASP.NET Core 應用程式中的 SSL](xref:security/enforcing-ssl)設定，並要求使用 SSL。

### <a name="create-an-sms-account"></a>建立 SMS 帳戶

SMS 帳戶，例如，建立從[twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)。 記錄的驗證認證 (如 twilio: accountSid 和 authToken，如 ASPSMS： 使用者金鑰和密碼)。

#### <a name="figuring-out-sms-provider-credentials"></a>找出 SMS 提供者認證

**Twilio:**  
從您的 Twilio 帳戶的 [儀表板] 索引標籤中，複製**帳戶 SID**和**驗證語彙基元**。

**ASPSMS:**  
從您的帳戶設定，瀏覽至**使用者金鑰**並將它連同複製您**密碼**。

我們稍後會儲存密碼管理員工具內的金鑰以這些值`SMSAccountIdentification`和`SMSAccountPassword`。

#### <a name="specifying-senderid--originator"></a>指定寄件者識別碼 / 訂閱者

**Twilio:**  
從 [數字] 索引標籤中，複製到您的 Twilio**電話號碼**。 

**ASPSMS:**  
在解除鎖定發送者功能表上，解除鎖定一或多個發送者或選擇英數字元的建立者 （所有網路不都支援）。 

我們稍後將儲存這個值與密碼管理員工具，在機碼`SMSAccountFrom`。


### <a name="provide-credentials-for-the-sms-service"></a>SMS 服務提供的認證

我們將使用[選項模式](xref:fundamentals/configuration/options)存取使用者帳戶和金鑰設定。 

   * 建立類別以擷取安全 SMS 索引鍵。 此範例中，`SMSoptions`中建立類別*Services/SMSoptions.cs*檔案。

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

設定`SMSAccountIdentification`，`SMSAccountPassword`和`SMSAccountFrom`與[密碼管理員工具](xref:security/app-secrets)。 例如: 

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* 新增 SMS 提供者 NuGet 封裝。 從封裝管理員主控台 (PMC) 執行：

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* 將程式碼中的加入*Services/MessageServices.cs*檔案，以啟用 SMS。 使用 Twilio 或 ASPSMS 區段：


**Twilio:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>設定要使用的啟動 `SMSoptions`

新增`SMSoptions`中的服務容器`ConfigureServices`方法中的*Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>啟用雙因素驗證

開啟*Views/Manage/Index.cshtml* Razor 檢視檔案和移除註解字元 （因此沒有標記為元 out）。

## <a name="log-in-with-two-factor-authentication"></a>登入雙因素驗證

* 執行應用程式並註冊新的使用者

![Web 應用程式在 Microsoft Edge 中開啟檢視的暫存器](2fa/_static/login2fa1.png)

* 點選 您的使用者名稱，就會啟動`Index`管理控制器中的動作方法。 然後點選 電話號碼**新增**連結。

![管理檢視](2fa/_static/login2fa2.png)

* 新增電話號碼，將會收到驗證碼，並點選**傳送驗證碼**。

![新增電話號碼 頁面](2fa/_static/login2fa3.png)

* 您會取得驗證碼的簡訊。 請輸入並點選**送出**

![確認電話號碼 頁面](2fa/_static/login2fa4.png)

如果您未收到簡訊，請參閱 twilio 記錄 頁面。

* 管理檢視會顯示已成功新增您的電話號碼。

![管理檢視](2fa/_static/login2fa5.png)

* 點選**啟用**啟用雙因素驗證。

![管理檢視](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>測試兩個雙因素驗證

* 登出。

* 登入。

* 使用者帳戶已啟用雙因素驗證，所以您不必提供第二個驗證因素。 在本教學課程中，您已啟用電話驗證。 內建的範本也可讓您設定電子郵件做為第二個因素。 您可以設定其他的第二個因素，例如 QR 代碼驗證。 點選**提交**。

![傳送驗證碼 檢視](2fa/_static/login2fa7.png)

* 輸入您在簡訊中收到的程式碼。

* 按一下**記住此瀏覽器**核取方塊將會豁免需要使用 2FA 時使用相同的裝置和瀏覽器登入。 啟用 2FA，並按一下**記住此瀏覽器**會為您提供強式 2FA 保護惡意使用者嘗試存取您的帳戶，只要它們不能存取您的裝置。 您可以經常使用的任何私用裝置上執行這項操作。 藉由設定**記住此瀏覽器**、 2FA 的額外的安全性收到您不定期使用的裝置，且您在不需要經過您自己的裝置上的 2FA 方便。

![確認檢視](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>帳戶鎖定，防止暴力攻擊

我們建議您在使用 2FA 帳戶鎖定。 一旦在使用者登入 （透過社交帳戶或本機帳戶），儲存每個在 2FA 的嘗試失敗，而且達到最大嘗試次數 （預設值為 5） 時，如果使用者遭到鎖定五分鐘的時間 (您可以設定時間內無鎖定`DefaultAccountLockoutTimeSpan`)。 下列設定帳戶鎖定的嘗試失敗 10 後 10 分鐘。

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
