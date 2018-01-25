---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: "帳戶確認和密碼復原與 ASP.NET Identity (C#) |Microsoft 文件"
author: HaoK
description: "執行本教學課程之前，您應該先完成建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設。 本教學課程中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 548baaaa06980fb793c079b66b6edc34422eb579
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>帳戶確認和密碼復原與 ASP.NET Identity (C#)
====================
由[Hao 是一隻](https://github.com/HaoK)， [Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 執行本教學課程之前，您應該先完成[建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。 本教學課程包含更多詳細資料，並顯示如何設定本機帳戶確認電子郵件，並讓使用者重設其忘記的密碼，在 ASP.NET Identity 中。 由 Rick anderson 發表撰寫本文時 ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))，Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Hao 是一隻和 Suhas Joshi。 NuGet 範例主要 Hao 是一隻寫入。


本機使用者帳戶需要使用者建立帳戶的密碼，而且該密碼 （安全） 儲存在 web 應用程式。 ASP.NET Identity 也支援社交帳戶，不需要使用者建立應用程式的密碼。 [社交帳戶](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)使用第三方 （如 Google、 Twitter、 Facebook 或 Microsoft） 來驗證使用者。 本主題涵蓋下列資訊：

- [建立 ASP.NET MVC 應用程式](#createMvc)和瀏覽 ASP.NET 識別的功能。
- [建立身分識別範例](#build)
- [設定電子郵件確認](#email)

新的使用者註冊他們的電子郵件別名，會建立本機帳戶。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

按一下 [註冊] 按鈕，將傳送確認電子郵件包含電子郵件地址驗證語彙基元。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

使用者會傳送一封電子郵件以確認語彙基元為其帳戶。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

按一下此連結確認帳戶。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>復原/重設密碼

忘記其密碼的本機使用者可以有安全性權杖傳送給他們的電子郵件帳戶，好讓他們能夠重設其密碼。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
使用者很快就會收到一封電子郵件，允許它們重設其密碼的連結。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
按一下此連結會移至重設頁面。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
按一下**重設**按鈕將會確認重設密碼。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>建立 ASP.NET Web 應用程式

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。

> [!NOTE]
> 警告： 您必須安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)完成本教學課程。


1. 建立新的 ASP.NET Web 專案，然後選取 MVC 範本。 Web Form 也支援 ASP.NET Identity，因此您可以依照類似的步驟，在 web form 應用程式。
2. 保留預設驗證為**個別使用者帳戶**。
3. 執行應用程式，請按一下**註冊**連結和註冊的使用者。 此時，只有在電子郵件時才驗證與[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)屬性。
4. 在 伺服器總管瀏覽至**資料 Connections\DefaultConnection\Tables\AspNetUsers**，以滑鼠右鍵按一下並選取**開啟資料表定義**。

    下圖顯示`AspNetUsers`結構描述：

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. 以滑鼠右鍵按一下**AspNetUsers**資料表，然後選取**顯示資料表資料**。  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
 此時不已確認電子郵件。

ASP.NET Identity 的預設資料存放區是 Entity Framework 中，但您可以設定成使用其他資料存放區，並加入其他欄位。 請參閱[其他資源](#addRes)本教學課程的最後一節。

[OWIN 啟動類別](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)( *Startup.cs* ) 時呼叫的應用程式啟動，而且會叫用`ConfigureAuth`方法中的*應用程式\_Start\Startup.Auth.cs*，這會設定 OWIN 管線，並初始化 ASP.NET Identity。 檢查`ConfigureAuth`方法。 每個`CreatePerOwinContext`呼叫註冊的回呼 (儲存在`OwinContext`)，將會呼叫一次針對每個要求來建立指定型別的執行個體。 您可以在建構函式中設定中斷點和`Create`每個型別的方法 (`ApplicationDbContext, ApplicationUserManager`)，並確認每個要求上呼叫。 執行個體`ApplicationDbContext`和`ApplicationUserManager`儲存在 OWIN 環境中，可在整個應用程式存取。 ASP.NET Identity 勾點至 OWIN 管線透過 cookie 中介軟體。 如需詳細資訊，請參閱[每個要求存留期管理 UserManager 在 ASP.NET Identity 中的類別](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。

當您變更您的安全性設定檔時，會產生新的安全性戳記，並儲存在`SecurityStamp`欄位*AspNetUsers*資料表。 請注意，`SecurityStamp`欄位是不同的安全性 cookie。 安全性 cookie 不會儲存在`AspNetUsers`資料表 （或識別資料庫中的其他地方）。 使用自我簽署的安全性 cookie 語彙基元[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)並建立`UserId, SecurityStamp`和到期時間資訊。

Cookie 中介軟體會檢查每個要求的 cookie。 `SecurityStampValidator`方法中的`Startup`類別叫用的資料庫，並定期檢查安全性戳記與所指定`validateInterval`。 這只會每隔 30 分鐘 （在我們的範例），除非您變更您的安全性設定檔。 在 30 分鐘的間隔選擇用來存取資料庫的次數降到最低。 請參閱我[雙因素驗證教學課程](index.md)如需詳細資訊。

每個在程式碼的註解`UseCookieAuthentication`方法支援的 cookie 驗證。 `SecurityStamp`欄位以及相關聯的程式碼提供了額外一層的應用程式的安全性，當您變更密碼，您就會記錄您用以登入瀏覽器外。 `SecurityStampValidator.OnValidateIdentity`方法可讓應用程式的使用者登入時，驗證安全性權杖時變更密碼，或使用外部登入使用。 需要這個動作，以確保以舊密碼產生任何語彙基元 (cookie) 都會變成無效。 在範例專案中，如果您變更使用者密碼然後新的權杖，會產生使用者，都無效的任何先前的語彙基元和`SecurityStamp`欄位會更新。

識別系統可讓您設定您的應用程式，使用者的安全性設定檔變更時 (例如，當使用者變更其密碼或變更相關聯的登入 (例如 Facebook、 Google、 Microsoft 帳戶等)，使用者登出所有瀏覽器執行個體。 例如下, 圖顯示[單一登出的範例](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)應用程式，可讓使用者登出所有瀏覽器執行個體 （在此情況下，在 IE、 Firefox 和 Chrome） 中，按一下其中一個按鈕。 或者，此範例可讓您僅登出的特定瀏覽器執行個體。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[單一登出的範例](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)應用程式示範如何 ASP.NET 識別可讓您重新產生安全性權杖。 需要這個動作，以確保以舊密碼產生任何語彙基元 (cookie) 都會變成無效。 這項功能提供一層額外的安全性，您的應用程式。當您變更您的密碼，則您將會登出您已登入此應用程式。

*應用程式\_Start\IdentityConfig.cs*檔案包含`ApplicationUserManager`，`EmailService`和`SmsService`類別。 `EmailService`和`SmsService`類別每個實作`IIdentityMessageService`介面，以便用來設定電子郵件和 SMS 每個類別中有常見的方法。 雖然本教學課程只會顯示如何將電子郵件通知，透過[SendGrid](http://sendgrid.com/)，您可以傳送電子郵件使用 SMTP 和其他機制。

`Startup`類別也包含鍋爐調色盤新增社交登入 （Facebook、 Twitter、 等等），請參閱我教學課程[MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)如需詳細資訊。

檢查`ApplicationUserManager`類別，其中包含使用者識別資訊，設定下列功能：

- 密碼強度需求。
- 使用者被鎖定 （嘗試次數和時間）。
- 雙因素驗證 (2FA)。 在另一個教學課程中，我將討論 2FA 和 SMS。
- 連結的電子郵件和 SMS 服務。 （我會在另一個教學課程涵蓋 SMS）。

`ApplicationUserManager`類別衍生自泛型`UserManager<ApplicationUser>`類別。 `ApplicationUser`衍生自[IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx)。 `IdentityUser`衍生自泛型`IdentityUser`類別：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

中的屬性與一致的上述屬性`AspNetUsers`資料表，如上所示。

泛型引數上的`IUser`可讓您在衍生類別使用不同類型的主索引鍵。 請參閱[ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)範例說明如何將主索引鍵從字串變更為 int 或 GUID。

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser`(`public class ApplicationUserManager : UserManager<ApplicationUser>`) 中定義*Models\IdentityModels.cs*為：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

上述反白顯示的程式碼會產生[ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)。 ASP.NET Identity 與 OWIN 的 Cookie 驗證以宣告為基礎，因此架構都需要應用程式產生`ClaimsIdentity`使用者。 `ClaimsIdentity`具有資訊有關的使用者名稱，例如使用者的所有宣告年齡和角色的使用者屬於。 您也可以在這個階段中加入更多的使用者宣告。

OWIN`AuthenticationManager.SignIn`方法會傳入`ClaimsIdentity`和登入使用者：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)示範如何將其他屬性，以便`ApplicationUser`類別。

## <a name="email-confirmation"></a>電子郵件確認

最好確認電子郵件以確認它們不會模擬其他人註冊新的使用者 （也就是它們未向其他人的電子郵件）。 假設您有討論論壇，您會想要防止`"bob@example.com"`從註冊為`"joe@contoso.com"`。 沒有電子郵件確認`"joe@contoso.com"`無法從您的應用程式取得垃圾電子郵件。 假設 Bob 意外地註冊為`"bib@example.com"`而且未注意到，他就無法使用密碼復原，因為應用程式沒有他正確的電子郵件。 電子郵件確認從 bot 提供有限的保護，並不提供保護，從決定垃圾郵件，它們有許多的工作電子郵件別名，他們可以使用來註冊。在下列範例中，使用者將無法變更其密碼，直到已確認其帳戶 (由他們按一下確認連結上註冊他們的電子郵件帳戶收到。)您可以將此工作流程套用至其他案例，例如傳送確認及重設系統管理員，會傳送使用者電子郵件，當它們等變更其設定檔建立新的帳戶密碼的連結。 您通常想要讓新的使用者張貼到您的網站的任何資料之前已經確認透過電子郵件、 簡訊或其他機制。 <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>建立更完整的範例

在本節中，您將使用 NuGet 下載更完整的範例，我們將使用。

1. 建立新***空***ASP.NET Web 專案。
2. 在 Package Manager Console 中，輸入下列下列命令： 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

 在此教學課程中，我們將使用[SendGrid](http://sendgrid.com/)傳送電子郵件。 `Identity.Samples`套件會安裝我們即將使用的程式碼。
3. 設定[專案以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. 執行應用程式中，按一下來測試建立本機帳戶**註冊**連結，然後張貼註冊表單。
5. 按一下模擬電子郵件確認示範電子郵件連結。
6. 移除範例的示範電子郵件連結確認程式碼 (`ViewBag.Link`帳戶控制器中的程式碼。 請參閱`DisplayEmail`和`ForgotPasswordConfirmation`動作方法及 razor 檢視)。

> [!NOTE]
> 警告： 如果您變更任何安全性設定，在此範例中，實際執行應用程式必須經過的安全性稽核會明確呼叫所做的變更。


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>檢查應用程式中的程式碼\_Start\IdentityConfig.cs

此範例示範如何建立帳戶，並將它加入*Admin*角色。 您應該以您將使用系統管理員帳戶的電子郵件來取代範例中的電子郵件。 現在來建立系統管理員帳戶最簡單的方式是以程式設計方式在`Seed`方法。 我們希望有一個工具在未來，可讓您建立及管理使用者和角色。 範例程式碼會讓您建立及管理使用者和角色，但您必須先有系統管理員帳戶以執行的角色和使用者管理頁面。 在此範例中，在植入資料庫時，會建立系統管理員帳戶。

變更密碼，並將名稱變更，可接收電子郵件通知的帳戶。

> [!WARNING]
> 安全性-永遠不會儲存敏感的資料來源上的程式碼中。

如前所述，`app.CreatePerOwinContext`呼叫中啟動類別加入回呼`Create`DB 的應用程式內容、 使用者管理員和角色管理員類別的方法。 OWIN 管線呼叫`Create`方法，這些類別為每個要求，以及儲存每個類別內容。 帳戶控制器會公開使用者管理員從 HTTP 內容 （其中包含 OWIN 內容）：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

當使用者註冊本機帳戶，`HTTP Post Register`方法呼叫：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

上述程式碼會使用模型資料建立新的使用者帳戶使用的電子郵件和輸入的密碼。 資料存放區中的電子郵件別名時，帳戶建立失敗，會再次顯示表單。 `GenerateEmailConfirmationTokenAsync`方法會建立安全的確認語彙基元，並將它儲存在 ASP.NET Identity 的資料存放區。 [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx)方法會建立一個連結，其中包含`UserId`和確認語彙基元。 此連結，然後電子郵件傳送給使用者，使用者可以按一下來確認其帳戶其電子郵件應用程式中的連結。

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>設定電子郵件確認

移至[Azure SendGrid 登入頁面](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)並註冊免費帳戶。 加入下列命令來設定 SendGrid 類似的程式碼：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> 電子郵件用戶端經常只接受文字 (無 HTML)。 您應該提供文字和 HTML 中的訊息。 在上述 SendGrid 範例中，做法是使用`myMessage.Text`和`myMessage.Html`如上所示的程式碼。


下列程式碼示範如何將傳送電子郵件使用[MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)類別`message.Body`傳回只有的連結。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> 安全性-永遠不會儲存敏感的資料來源上的程式碼中。 帳戶和認證會儲存在 appSetting。 在 Azure 上，您可以安全地儲存這些值在**[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure 入口網站中的索引標籤。 請參閱[ASP.NET 和 Azure 部署的密碼和其他機密資料的最佳做法](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。


輸入您的 SendGrid 認證，執行應用程式，暫存器使用電子郵件別名可以按一下您的電子郵件中的 [確認] 連結。 若要了解如何執行這與您[Outlook.com](http://outlook.com)電子郵件帳戶，請參閱 John Atten [Outlook.Com SMTP 主機的 C# SMTP 組態](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx)和他[ASP.NET Identity 2.0： 設定註冊帳戶驗證與雙因素授權](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)文章。

當使用者按一下**註冊**包含驗證語彙基元確認電子郵件會傳送電子郵件地址 按鈕。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

使用者會傳送一封電子郵件以確認語彙基元為其帳戶。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>檢查程式碼

下列程式碼顯示 `POST ForgotPassword` 方法。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

如果尚未確認使用者電子郵件，方法就會以無訊息模式失敗。 如果錯誤已發佈無效的電子郵件地址，惡意使用者就可以使用該資訊來尋找有效的使用者識別碼 （電子郵件別名） 攻擊。

下列程式碼會示範`ConfirmEmail`中帳戶控制器使用者傳送給他們的電子郵件中的 [確認] 連結時所呼叫的方法：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

一旦已使用忘記的密碼語彙基元，它會失效。 下列程式碼變更`Create`方法 (在*應用程式\_Start\IdentityConfig.cs*檔案) 設定即將在過去 3 小時內的語彙基元。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

上述程式碼，忘記的密碼和電子郵件確認語彙基元即將於過去 3 小時內。 預設值`TokenLifespan`是一天。

下列程式碼顯示電子郵件確認方法：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 為了讓您的應用程式更安全，ASP.NET Identity 支援雙因素驗證 (2FA)。 請參閱[ASP.NET Identity 2.0： 設定帳戶驗證與授權雙因素](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)由 John Atten。 雖然您可以設定帳戶鎖定的登入的密碼嘗試失敗，該方法可讓您的登入容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)鎖定。 我們建議您只能搭配 2FA 使用帳戶鎖定。  
<a id="addRes"></a>

## <a name="additional-resources"></a>其他資源

- [ASP.NET Identity 的自訂儲存體提供者概觀](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)也示範如何加入使用者資料表中的設定檔資訊。
- [ASP.NET MVC 和身分識別 2.0： 了解基本概念](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)由 John Atten。
- [ASP.NET Identity 簡介](../getting-started/introduction-to-aspnet-identity.md)
- [宣告 ASP.NET Identity 2.0.0 的 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)由 Pranav Rastogi。
