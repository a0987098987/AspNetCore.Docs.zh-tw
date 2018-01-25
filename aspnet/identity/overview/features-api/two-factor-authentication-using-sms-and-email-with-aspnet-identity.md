---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證 |Microsoft 文件"
author: HaoK
description: "本教學課程將說明如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。 由 Rick anderson 發表撰寫本文時 ( @RickAndMSFT )、 Pr...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0f9ff7cf74048a008b150da1e843ff15333269ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證
====================
由[Hao 是一隻](https://github.com/HaoK)， [Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 本教學課程將說明如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。
> 
> 由 Rick anderson 發表撰寫本文時 ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))，Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Hao 是一隻和 Suhas Joshi。 NuGet 範例主要 Hao 是一隻寫入。


本主題涵蓋下列資訊：

- [建立身分識別範例](#build)
- [SMS 進行雙因素驗證設定](#SMS)
- [啟用雙因素驗證](#enable2)
- [如何註冊雙因素驗證提供者](#reg)
- [結合社交和本機登入帳戶](#combine)
- [帳戶鎖定的暴力攻擊](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>建立身分識別範例

在本節中，您將使用 NuGet，若要下載的範例，我們將使用。 開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。

> [!NOTE]
> 警告： 您必須安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)完成本教學課程。


1. 建立新***空***ASP.NET Web 專案。
2. 在 Package Manager Console 中，輸入下列下列命令：  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 在此教學課程中，我們將使用[SendGrid](http://sendgrid.com/)傳送電子郵件和[Twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms 文字行動的。 `Identity.Samples`套件會安裝我們即將使用的程式碼。
3. 設定[專案以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. *選擇性*： 依照我[電子郵件確認教學課程](account-confirmation-and-password-recovery-with-aspnet-identity.md)連結 SendGrid 然後執行應用程式並註冊電子郵件帳戶。
5. * 選擇性: * 移除此範例的示範電子郵件連結確認程式碼 (`ViewBag.Link`帳戶控制器中的程式碼。 請參閱`DisplayEmail`和`ForgotPasswordConfirmation`動作方法及 razor 檢視)。
6. * 選擇性: * 移除`ViewBag.Status`程式碼從管理和帳戶控制器和*Views\Account\VerifyCode.cshtml*和*Views\Manage\VerifyPhoneNumber.cshtml* razor 檢視。 或者，您可以保留`ViewBag.Status`顯示畫面來測試此應用程式在本機，而不必相連結，並傳送電子郵件和簡訊的運作方式。

> [!NOTE]
> 警告： 如果您變更任何安全性設定，在此範例中，實際執行應用程式必須經過所做的變更會明確呼叫的安全性稽核。


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>SMS 進行雙因素驗證設定

本教學課程提供使用 Twilio 或 ASPSMS 的指示，但是您可以使用任何其他 SMS 提供者。

1. **建立使用者帳戶與 SMS 提供者**  
  
 建立[Twilio](https://www.twilio.com/try-twilio)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)帳戶。
2. **安裝其他套件，或加入服務參考**  
  
 Twilio:  
 在 Package Manager Console 中，輸入下列命令：  
    `Install-Package Twilio`  
  
 ASPSMS:  
 必須加入下列服務參考：  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 位址:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 命名空間:  
    `ASPSMSX2`
3. **找出 SMS 提供者使用者認證**  
  
 Twilio:  
 從**儀表板**Twilio 帳戶、 複製的索引標籤**帳戶 SID**和**驗證語彙基元**。  
  
 ASPSMS:  
 從您的帳戶設定，瀏覽至**使用者金鑰**並將它複製以及您自行定義**密碼**。  
  
 我們將稍後將這些值儲存在變數`SMSAccountIdentification`和`SMSAccountPassword`。
4. **指定寄件者識別碼 / 訂閱者**  
  
 Twilio:  
 從**數字**索引標籤上，複製您的 Twilio 電話號碼。  
  
 ASPSMS:  
 內**解除鎖定發送者**功能表上，解除鎖定一或多個的建立者，或選擇英數字元的建立者 （所有網路不都支援）。  
  
 我們稍後會將這個值儲存在變數中`SMSAccountFrom`。
5. **將 SMS 提供者認證傳送到應用程式**  
  
 提供的認證和寄件者電話號碼給應用程式：

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > 安全性-永遠不會儲存敏感的資料來源上的程式碼中。 為了簡化範例上述程式碼中加入的帳戶和認證。 請參閱 Jon Atten [ASP.NET MVC： 保留私用設定不足的原始檔控制](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。
6. **資料傳輸至 SMS 提供者的實作**  
  
 設定`SmsService`類別*應用程式\_Start\IdentityConfig.cs*檔案。  
  
 根據使用的 SMS 提供者啟用  **Twilio**或**ASPSMS** > 一節： 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. 執行應用程式，您先前註冊的帳戶登入。
8. 按一下您的使用者識別碼，就會啟動`Index`中的動作方法`Manage`控制站。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. 按一下 新增。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. 在幾秒鐘的時間就會看到文字訊息，並將驗證碼。 請輸入，並按下**送出**。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. 管理檢視會顯示已加入您的電話號碼。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>檢查程式碼

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index`中的動作方法`Manage`控制器設定根據您的上一個動作的狀態訊息，並提供連結變更您的本機密碼，或是新增本機帳戶。 `Index`方法也會顯示狀態，或您 2FA 電話號碼、 外部登入、 2FA 啟用，並請記得 2FA 方法，針對這個瀏覽器 （稍後說明）。 按一下您的使用者識別碼 （電子郵件），在標題列中不會將訊息傳遞。 按一下**電話號碼： 移除**連結傳遞`Message=RemovePhoneSuccess`做為查詢字串。  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber`動作方法會顯示一個對話方塊，輸入可以接收簡訊的電話號碼。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

按一下**傳送驗證碼**按鈕會張貼到 HTTP POST 的電話號碼`AddPhoneNumber`動作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync`方法會產生此值會設定在簡訊中的安全性權杖。 如果 SMS 服務已設定，權杖會以字串形式傳送&quot;您安全性的程式碼&lt;語彙基元&gt;&quot;。 `SmsService.SendAsync`以非同步方式呼叫方法，則應用程式會重新導向至`VerifyPhoneNumber`動作方法 （這會顯示下列對話方塊），您可以在其中輸入驗證碼。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

一旦您輸入程式碼，並按一下 [提交]，程式碼張貼至 HTTP POST`VerifyPhoneNumber`動作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync`方法會檢查已張貼的安全性驗證碼。 程式碼是否正確，會新增電話號碼`PhoneNumber`欄位`AspNetUsers`資料表。 如果成功，該呼叫`SignInAsync`方法呼叫：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent`參數會設定是否驗證工作階段會保存在多個要求。

當您變更您的安全性設定檔時，會產生新的安全性戳記，並儲存在`SecurityStamp`欄位*AspNetUsers*資料表。 請注意，`SecurityStamp`欄位是不同的安全性 cookie。 安全性 cookie 不會儲存在`AspNetUsers`資料表 （或識別資料庫中的其他地方）。 使用自我簽署的安全性 cookie 語彙基元[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)並建立`UserId, SecurityStamp`和到期時間資訊。

Cookie 中介軟體會檢查每個要求的 cookie。 `SecurityStampValidator`方法中的`Startup`類別叫用的資料庫，並定期檢查安全性戳記與所指定`validateInterval`。 這只會每隔 30 分鐘 （在我們的範例），除非您變更您的安全性設定檔。 在 30 分鐘的間隔選擇用來存取資料庫的次數降到最低。

`SignInAsync`方法需要被呼叫時的安全性設定檔會進行任何變更。 資料庫的安全性設定檔變更時，會更新`SecurityStamp`欄位，並沒有呼叫`SignInAsync`您永久登入方法*只*直到下一次 OWIN 管線叫用的資料庫 ( `validateInterval`). 您可以藉由變更測試`SignInAsync`方法來立即傳回，並設定 cookie`validateInterval`屬性從 30 分鐘的時間為 5 秒：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

與上述的程式碼變更，您可以變更您的安全性設定檔 (例如，藉由變更的狀態**兩個因素啟用**)，而且您會在 5 秒記錄時`SecurityStampValidator.OnValidateIdentity`方法失敗。 移除中的返回行`SignInAsync`方法，請變更其他安全性設定檔，然後您會登出。`SignInAsync`方法會產生新的安全性 cookie。

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>啟用雙因素驗證

範例應用程式，您需要使用 UI 啟用雙因素驗證 (2FA)。 若要啟用 2FA，按一下您在瀏覽列中的使用者識別碼 （電子郵件別名）。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
按一下 啟用 2FA 上。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) 登出，然後重新登入。 如果您已啟用電子郵件 (請參閱我[上一個教學課程](account-confirmation-and-password-recovery-with-aspnet-identity.md))，您可以選取的 SMS 或 2FA 電子郵件。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) 請確認程式碼頁面會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) 按一下**記住此瀏覽器**核取方塊將會豁免需要使用 2FA 具有該電腦與瀏覽器登入。 啟用 2FA，並按一下**記住此瀏覽器**會為您提供強式 2FA 保護惡意使用者嘗試存取您的帳戶，只要它們不能存取您的電腦。 您可以經常使用的任何私用電腦上執行這項操作。 藉由設定**記住此瀏覽器**、 您沒有經常使用的電腦中獲得 2FA 提高的安全性，且您方便性，不論在不需透過 2FA 您自己的電腦上。 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>如何註冊雙因素驗證提供者

當您建立新的 MVC 專案， *IdentityConfig.cs*檔案包含下列程式碼，以註冊雙因素驗證提供者：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>新增 2FA 的電話號碼

`AddPhoneNumber`中的動作方法`Manage`控制站會產生安全性權杖，並傳送其電話號碼您所提供。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

傳送之後語彙基元，它將重新導向至`VerifyPhoneNumber`動作方法，您可以在其中輸入要註冊 2FA SMS 的程式碼。 除非您已驗證的電話號碼，不會使用 SMS 2FA。

## <a name="enabling-2fa"></a>啟用 2FA

`EnableTFA`動作方法可讓 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

請注意`SignInAsync`必須先呼叫，因為啟用 2FA 已變更的安全性設定檔。 啟用 2FA 時，使用者必須使用 2FA 登入，使用 2FA 方法完成註冊 （SMS 和電子郵件範例中的）。

您可以新增更多的 2FA 提供者，例如 QR 程式碼產生器，或者您可以撰寫您自己 (請參閱[使用 Google 驗證器與 ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。

> [!NOTE]
> 使用產生 2FA 代碼[以時間為基礎的單次密碼演算法](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)和代碼六分鐘內才有效。 如果您需要多個六分鐘的時間來輸入程式碼，您會收到無效的程式碼錯誤訊息。


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>結合社交和本機登入帳戶

您可以結合本機和社交帳戶按一下電子郵件連結。 依照以下順序&quot; RickAndMSFT@gmail.com &quot;首先會建立為本機登入，但您可以建立帳戶與社交的記錄檔中第一個，然後加入本機的登入。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

按一下**管理**連結。 請注意 0 外部 （社交登入） 與此帳戶相關聯。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

按一下此連結服務中的另一個記錄檔，並接受應用程式要求。 已結合兩個帳戶，您將無法與任一個帳戶登入。 您可能會想讓使用者加入本機帳戶，以防使用者社交的記錄檔中驗證服務已關閉，或可能比較他們已經失去其社交帳戶的存取。

在下圖中，Tom 是社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

按一下**挑選密碼**可讓您在上新增本機記錄檔相同的帳戶相關聯。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>帳戶鎖定的暴力攻擊

您可以啟用使用者鎖定，從字典攻擊的應用程式上保護的帳戶。 下列程式碼中`ApplicationUserManager Create`方法會設定鎖定：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

上述程式碼可讓進行雙因素驗證的鎖定。 雖然您可以藉由變更啟用鎖定的登入`shouldLockout`為 true 的`Login`帳戶控制器方法，建議您不啟用鎖定的登入，因為它會使帳戶容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)登入攻擊。 範例程式碼中，鎖定已停用系統管理員帳戶中建立`ApplicationDbInitializer Seed`方法：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>需要使用者擁有的已驗證的電子郵件帳戶

下列程式碼需要使用者擁有的已驗證的電子郵件帳戶，他們可以登入前：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>SignInManager 如何檢查 2FA 需求

在本機記錄檔和中檢查是否已啟用 2FA 社交記錄檔。 如果已啟用 2FA，`SignInManager`登入方法會傳回`SignInStatus.RequiresVerification`，使用者將會重新導向至`SendCode`動作方法，您將必須輸入要完成的記錄順序中的程式碼。 如果使用者具有 RememberMe 設定在使用者本機 cookie，`SignInManager`會傳回`SignInStatus.Success`，而且它們沒有經過 2FA。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

下列程式碼會示範`SendCode`動作方法。 A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)建立與所有使用者啟用 2FA 方法。 [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)傳遞至[DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper，可讓使用者選取的 2FA 方法 （通常是電子郵件和 SMS）。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

一旦使用者張貼 2FA 方法`HTTP POST SendCode`呼叫動作方法時，`SignInManager`傳送 2FA 程式碼中，且使用者已重新導向到`VerifyCode`動作方法可以在其中輸入要完成的記錄檔中的程式碼。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA 鎖定

雖然您可以設定帳戶鎖定的登入的密碼嘗試失敗，該方法可讓您的登入容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)鎖定。 我們建議您只能搭配 2FA 使用帳戶鎖定。 當`ApplicationUserManager`已建立，此範例程式碼設定 2FA 鎖定和`MaxFailedAccessAttemptsBeforeLockout`五個。 一旦使用者登入 （透過本機帳戶或社交帳戶），儲存每個在 2FA 的嘗試失敗，而且使用者如果嘗試次數上限為止，則鎖定五分鐘的時間 (您可以設定時間內無鎖定`DefaultAccountLockoutTimeSpan`)。

<a id="addRes"></a>

## <a name="additional-resources"></a>其他資源

- [ASP.NET Identity 建議資源](../getting-started/aspnet-identity-recommended-resources.md)因此連結的身分識別部落格、 視訊、 教學課程和優越的完整清單。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)也示範如何加入使用者資料表中的設定檔資訊。
- [ASP.NET MVC 和身分識別 2.0： 了解基本概念](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)由 John Atten。
- [帳戶確認和 ASP.NET 識別的密碼復原](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity 簡介](../getting-started/introduction-to-aspnet-identity.md)
- [宣告 ASP.NET Identity 2.0.0 的 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)由 Pranav Rastogi。
- [ASP.NET Identity 2.0： 設定帳戶驗證與授權雙因素](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)由 John Atten。
