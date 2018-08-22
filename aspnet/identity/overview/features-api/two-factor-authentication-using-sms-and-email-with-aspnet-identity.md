---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: 使用 SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證 |Microsoft Docs
author: HaoK
description: 本教學課程會示範如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。 撰寫本文時已由 Rick Anderson ( @RickAndMSFT )、 Pr....
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4454f011960f74c20ac590c3d034028cfc867e7d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825166"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>使用 SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證
====================
藉由[Hao 是一隻](https://github.com/HaoK)，[請參閱 Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 本教學課程會示範如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。
> 
> 撰寫本文時已由 Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))，請參閱 Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Hao 是一隻和 Suhas Joshi。 NuGet 範例已寫入主要是由 Hao 是一隻。


本主題涵蓋下列資訊：

- [建立身分識別範例](#build)
- [設定 SMS 的雙因素驗證](#SMS)
- [啟用雙因素驗證](#enable2)
- [如何註冊雙因素驗證提供者](#reg)
- [結合社交和本機登入帳戶](#combine)
- [帳戶鎖定的暴力密碼破解攻擊](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>建立身分識別範例

在本節中，您會使用 NuGet 來下載的範例，我們將使用。 開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。

> [!NOTE]
> 警告： 您必須安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)完成本教學課程。


1. 建立新***空***ASP.NET Web 專案。
2. 在套件管理員主控台中，輸入下列命令的下列命令：  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   在本教學課程中，我們將使用[SendGrid](http://sendgrid.com/)傳送電子郵件並[Twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms 傳簡訊到的。 `Identity.Samples`套件會安裝我們將使用的程式碼。
3. 設定[專案，以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. *選擇性*： 依照我[教學課程中確認電子郵件](account-confirmation-and-password-recovery-with-aspnet-identity.md)SendGrid 連結然後執行應用程式並註冊的電子郵件帳戶。
5. * 選擇性: * 移除此範例示範電子郵件連結確認程式碼 (`ViewBag.Link`帳戶控制器中的程式碼。 請參閱`DisplayEmail`和`ForgotPasswordConfirmation`動作方法和 razor 檢視)。
6. <em>選擇性: * 移除`ViewBag.Status`程式碼從管理和帳戶控制器和 *Views\Account\VerifyCode.cshtml</em>並<em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor 檢視。 或者，您可以保留`ViewBag.Status`顯示畫面來測試此應用程式在本機而不需要將連結，並傳送電子郵件和 SMS 訊息的運作方式。

> [!NOTE]
> 警告： 如果您變更任何安全性設定，在此範例中，生產應用程式必須進行的變更會明確呼叫的安全性稽核。


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>設定 SMS 的雙因素驗證

本教學課程提供使用 Twilio 或 ASPSMS 的指示，但您可以使用任何其他 SMS 提供者。

1. **使用 SMS 提供者建立使用者帳戶**  
  
   建立[Twilio](https://www.twilio.com/try-twilio)該[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)帳戶。
2. **安裝其他套件或新增服務參考**  
  
   Twilio:  
   在套件管理員主控台中，輸入下列命令：  
    `Install-Package Twilio`  
  
   ASPSMS:  
   下列服務參考必須加入：  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   位址:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   命名空間:  
    `ASPSMSX2`
3. **找出 SMS 提供者使用者認證**  
  
   Twilio:  
   從**儀表板**索引標籤上的 Twilio 帳戶，複製**Account SID**並**驗證權杖**。  
  
   ASPSMS:  
   從您的帳戶設定，瀏覽至**Userkey**並將它複製以及您自行定義**密碼**。  
  
   我們將稍後將這些值儲存在變數`SMSAccountIdentification`和`SMSAccountPassword`。
4. **指定寄件者識別碼 / 建立者**  
  
   Twilio:  
   從**數字**索引標籤上，複製您的 Twilio 電話號碼。  
  
   ASPSMS:  
   內**解除鎖定的建立者**功能表上，解除鎖定一或多個建立者，或選擇 英數字元的建立者 （不支援所有的網路）。  
  
   稍後，我們會將這個值儲存在變數中`SMSAccountFrom`。
5. **將 SMS 提供者認證傳送到應用程式**  
  
   提供的認證與寄件者電話號碼給應用程式：

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > 安全性-機密資料絕不儲存在原始程式碼中。 上述保持簡單的範例程式碼中加入的帳戶和認證。 請參閱 Jon Atten [ASP.NET MVC： 保留原始檔控制的私用設定現成](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。
6. **資料傳輸至 SMS 提供者的實作**  
  
   設定`SmsService`類別內*應用程式\_Start\IdentityConfig.cs*檔案。  
  
   根據使用的 SMS 提供者啟用其中一個**Twilio**或**ASPSMS**區段： 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. 執行應用程式，以及您先前註冊的帳戶登入。
8. 按一下您的使用者識別碼，就會啟動`Index`中的動作方法`Manage`控制站。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. 按一下 新增。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. 在幾秒鐘的時間，您會使用驗證碼的簡訊。 請輸入並按下**送出**。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. [管理] 檢視會顯示已加入您的電話號碼。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>檢查程式碼

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index`中的動作方法`Manage`控制器設定根據您的上一個動作的狀態訊息，並提供變更您的本機密碼，或新增本機帳戶的連結。 `Index`方法也會顯示狀態，或您的 2FA 電話號碼、 外部登入、 啟用 2FA，並記住 2FA 方法，這個瀏覽器 （稍後說明）。 按一下您的使用者識別碼 （電子郵件），標題列中，未通過一則訊息。 按一下**電話號碼： 移除**連結傳遞`Message=RemovePhoneSuccess`作為查詢字串。  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber`動作方法會顯示一個對話方塊，輸入可以接收簡訊的電話號碼。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

按一下**傳送驗證碼** 按鈕會將電話號碼公佈 HTTP POST`AddPhoneNumber`動作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync`方法會產生將在簡訊中的安全性權杖。 如果 SMS 服務已設定，權杖會傳送為字串&quot;您的安全性程式碼&lt;語彙基元&gt;&quot;。 `SmsService.SendAsync`方法會以非同步方式呼叫，然後應用程式會重新導向至`VerifyPhoneNumber`動作方法 （這會顯示下列對話方塊），您可以在其中輸入驗證碼。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

一旦您輸入程式碼，並按一下 [提交]，程式碼會張貼到 HTTP POST`VerifyPhoneNumber`動作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync`方法會檢查已張貼的安全性驗證碼。 如果程式碼正確無誤，請加入的電話號碼`PhoneNumber`欄位`AspNetUsers`資料表。 如果成功，該呼叫`SignInAsync`方法呼叫：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent`參數會設定是否在多個要求之間保存驗證工作階段。

當您變更您的安全性設定檔時，會產生新的安全性戳記，並儲存在`SecurityStamp`欄位*AspNetUsers*資料表。 請注意，`SecurityStamp`欄位是不同的安全性 cookie。 安全性 cookie 不會儲存在`AspNetUsers`資料表 （或任何其他身分識別資料庫中的位置）。 使用自我簽署的安全性 cookie 語彙基元[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)並建立不含`UserId, SecurityStamp`和到期時間資訊。

Cookie 中介軟體會檢查每個要求的 cookie。 `SecurityStampValidator`方法中的`Startup`類別叫用的資料庫，並定期檢查安全性戳記依照`validateInterval`。 這只會每隔 30 分鐘 （在我們的範例），除非您變更您的安全性設定檔。 在 30 分鐘的間隔已選擇將存取資料庫的次數降至最低。

`SignInAsync`方法需要的安全性設定檔進行任何變更時呼叫。 時的安全性設定檔變更時，資料庫會更新`SecurityStamp`欄位中，而不呼叫`SignInAsync`方法會保持登入*只*直到下一次 OWIN 管線叫用的資料庫 ( `validateInterval`). 您可以藉由變更進行測試`SignInAsync`方法來立即傳回，並設定 cookie`validateInterval`從 30 分鐘的時間為 5 秒的屬性：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

以上述的程式碼變更，您可以變更您的安全性設定檔 (比方說，是藉由變更的狀態**已啟用的兩個因素**)，而且您會在 5 秒內記錄時`SecurityStampValidator.OnValidateIdentity`方法失敗。 移除在的返回行`SignInAsync`方法，讓另一個變更的安全性設定檔，並將無法將您登出。`SignInAsync`方法會產生新的安全性 cookie。

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>啟用雙因素驗證

在範例應用程式中，您需要使用 UI 來啟用雙因素驗證 (2FA)。 若要啟用 2FA，請按一下您在導覽列中的使用者識別碼 （電子郵件別名）。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
按一下 啟用 2FA。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) 登出後再重新登入。 如果您已啟用電子郵件 (請參閱我[先前的教學課程](account-confirmation-and-password-recovery-with-aspnet-identity.md))，您可以選取 SMS 或 2FA 的電子郵件。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) 確認字碼頁會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) 按一下**記住此瀏覽器**核取方塊將會豁免不必使用 2FA 登入具有該電腦和瀏覽器。 啟用 2FA，然後按一下**記住此瀏覽器**會為您提供強式 2FA 保護避免惡意使用者嘗試存取您的帳戶，只要它們不能存取您的電腦。 您經常使用的任何私用電腦上，您可以執行這項操作。 藉由設定**記住此瀏覽器**不定期使用，您的電腦中獲得 2FA 的額外的安全性，取得上不需要經歷 2FA，在您自己的電腦上的便利性。 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>如何註冊雙因素驗證提供者

當您建立新的 MVC 專案中， *IdentityConfig.cs*檔案包含下列的程式碼，以註冊雙因素驗證提供者：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>新增 2FA 的電話號碼

`AddPhoneNumber`中的動作方法`Manage`控制站產生安全性權杖，並傳送到電話號碼您所提供。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

之後傳送權杖，它將重新導向至`VerifyPhoneNumber`動作方法，您可以在其中輸入程式碼以註冊 2FA SMS。 除非您已經驗證的電話號碼，不會使用 SMS 2FA。

## <a name="enabling-2fa"></a>啟用 2FA

`EnableTFA`動作方法會啟用 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

請注意`SignInAsync`必須呼叫，因為啟用 2FA 已變更的安全性設定檔。 啟用 2FA 之後，使用者必須使用 2FA 登入，使用已註冊 （SMS 和電子郵件中的範例） 2FA 方法。

您可以新增更多的 2FA 提供者，例如 QR 程式碼產生器，或者您可以撰寫您自己的 (請參閱[使用 Google 驗證器使用 ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。

> [!NOTE]
> 2FA 的程式碼使用產生[時間為基礎的單次密碼演算法](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)和代碼六分鐘內是否有效。 如果您採用多個輸入的代碼六分鐘內，您會收到無效的程式碼錯誤訊息。


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>結合社交和本機登入帳戶

您可以結合本機和社交帳戶，按一下您的電子郵件連結。 依照以下順序&quot; RickAndMSFT@gmail.com &quot;首先會建立為本機登入，但您可以為社交登入第一，建立帳戶，然後新增本機登入。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

按一下 **管理**連結。 請注意 0 外部 （社交登入） 與此帳戶相關聯。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

按一下連結以在服務中的另一個記錄檔，並接受應用程式要求。 已合併兩個帳戶，您可以使用任一個帳戶登入。 您可能想要新增本機帳戶，以防使用者社交的記錄檔，在 驗證服務已關閉，或是更有可能他們已無法存取其社交帳戶使用者。

在下圖中，Tom 則社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

按一下**挑選密碼**可讓您在新增本機記錄檔相同的帳戶相關聯。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>帳戶鎖定的暴力密碼破解攻擊

您可以啟用使用者鎖定您的應用程式，免於字典攻擊的檔案，以保護帳戶。 下列程式碼中`ApplicationUserManager Create`方法設定鎖定：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

上述程式碼可讓鎖定只雙因素驗證。 雖然您可以藉由變更登入的鎖定`shouldLockout`設為 true`Login`帳戶控制器方法，建議您不啟用鎖定的登入，因為它可讓帳戶容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)登入攻擊。 在範例程式碼中，鎖定已停用的系統管理員帳戶中建立`ApplicationDbInitializer Seed`方法：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>需要有已驗證的電子郵件帳戶使用者

下列程式碼會要求使用者已驗證電子郵件帳戶，他們可以登入前：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>SignInManager 如何檢查 2FA 需求

同時在本機記錄檔並社交登入檢查，以查看是否已啟用 2FA。 如果已啟用 2FA`SignInManager`登入方法會傳回`SignInStatus.RequiresVerification`，使用者將會重新導向至`SendCode`動作方法，他們必須輸入程式碼來完成序列中的記錄檔。 在使用者本機 cookie 上設定如果使用者具有 RememberMe`SignInManager`會傳回`SignInStatus.Success`，他們就不需要經歷 2FA。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

下列程式碼示範`SendCode`動作方法。 A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)建立所有使用者啟用 2FA 方法。 [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)傳遞至[DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx)協助程式，可讓使用者選取的 2FA 方法 （通常是電子郵件和簡訊）。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

使用者張貼 2FA 的方法，一旦`HTTP POST SendCode`呼叫動作方法時， `SignInManager` 2FA 的程式碼和使用者會重新導向至傳送`VerifyCode`他們可以在其中輸入程式碼來完成的記錄檔中的動作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA 鎖定

雖然您可以設定帳戶鎖定的密碼登入嘗試失敗，該方法可讓您的登入容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)鎖定。 我們建議您只適用於帳戶鎖定 2FA。 當`ApplicationUserManager`會建立，範例程式碼設定 2FA 鎖定和`MaxFailedAccessAttemptsBeforeLockout`五個。 一旦使用者登入 （透過本機帳戶或社交帳戶），儲存 2FA 在每次失敗的嘗試，而且使用者如果達到嘗試次數上限，則會被封鎖五分鐘的時間 (您可以設定使用的時間鎖定`DefaultAccountLockoutTimeSpan`)。

<a id="addRes"></a>

## <a name="additional-resources"></a>其他資源

- [ASP.NET Identity 建議資源](../getting-started/aspnet-identity-recommended-resources.md)因此連結的身分識別部落格、 影片、 教學課程和絕佳的完整清單。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)也會示範如何將使用者資料表中的設定檔資訊。
- [ASP.NET MVC 和身分識別 2.0： 了解基本概念](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)由 John Atten。
- [帳戶確認和密碼復原，使用 ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity 簡介](../getting-started/introduction-to-aspnet-identity.md)
- [宣佈 ASP.NET 身分識別 2.0.0 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)請參閱 Pranav rastogi。
- [ASP.NET Identity 2.0： 設定帳戶驗證和雙因素授權](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)由 John Atten。
