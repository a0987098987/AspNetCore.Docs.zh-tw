---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: 帳戶確認和密碼復原，使用 ASP.NET Identity (C#) |Microsoft Docs
author: HaoK
description: 進行本教學課程之前，您應該先完成登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式。 本教學課程...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 47dc2c1044a5964624ba2f8af4f174a2fd99d3e8
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236402"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>帳戶確認和密碼復原，使用 ASP.NET Identity (C#)

> 在進行本教學課程之前您應該先完成[登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。 本教學課程包含更多詳細資料，並將為您示範如何設定本機帳戶的確認電子郵件，並可讓使用者重設其遺忘的密碼，在 ASP.NET 身分識別。

本機使用者帳戶需要使用者建立帳戶的密碼和密碼 （安全） 儲存在 web 應用程式。 ASP.NET Identity 也支援不需要使用者建立應用程式密碼的社交帳戶。 [社交帳戶](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)使用協力廠商 （例如 Google、 Twitter、 Facebook 或 Microsoft） 驗證使用者。 本主題涵蓋下列資訊：

- [建立 ASP.NET MVC 應用程式](#createMvc)並探索 ASP.NET 身分識別功能。
- [建立身分識別範例](#build)
- [設定電子郵件確認](#email)

新的使用者註冊其電子郵件別名，這會建立本機帳戶。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

選取 [Register] 按鈕會傳送確認電子郵件，其中包含電子郵件地址的驗證語彙基元。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

含有確認語彙基元，為其帳戶的電子郵件傳送給使用者。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

選取此連結確認帳戶。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>復原/重設密碼

忘記其密碼的本機使用者可以有安全性權杖傳送給他們的電子郵件帳戶，讓他們能夠重設其密碼。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
使用者很快就會收到一封電子郵件，讓他們能夠重設其密碼的連結。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
選取此連結會移至重設頁面。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
選取**重設**按鈕將會確認重設密碼。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>建立 ASP.NET Web 應用程式

開始安裝並執行[Visual Studio 2017](https://visualstudio.microsoft.com/)。


1. 建立新的 ASP.NET Web 專案，然後選取 [MVC] 範本。 Web Form 也支援 ASP.NET 身分識別，因此您可以依照類似的步驟，在 web form 應用程式。
2. 若要驗證變更**個別使用者帳戶**。
3. 執行應用程式，請選取**註冊**連結，並註冊的使用者。 此時，唯一的驗證電子郵件是使用[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)屬性。
4. 在 [伺服器總管] 中，瀏覽至**資料 Connections\DefaultConnection\Tables\AspNetUsers**，以滑鼠右鍵按一下，然後選取**開啟資料表定義**。

    下圖顯示`AspNetUsers`結構描述：

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. 以滑鼠右鍵按一下**AspNetUsers**資料表，然後選取**顯示資料表資料**。  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   此時已不確認電子郵件。

ASP.NET Identity 的預設資料存放區是 Entity Framework，但您可以設定，以使用其他資料存放區，並新增額外的欄位。 請參閱[額外的資源](#addRes)本教學課程的最後一節。

[OWIN 啟動類別](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)( *Startup.cs* ) 應用程式會啟動，並叫用時會呼叫`ConfigureAuth`方法*應用程式\_Start\Startup.Auth.cs*，這會設定 OWIN 管線，並初始化 ASP.NET 身分識別。 檢查 `ConfigureAuth` 方法。 每個`CreatePerOwinContext`呼叫會註冊回呼 (儲存在`OwinContext`)，每個要求來建立指定型別的執行個體將一次呼叫。 您可以在建構函式中設定中斷點並`Create`每個型別的方法 (`ApplicationDbContext, ApplicationUserManager`)，並確認每個要求上呼叫。 執行個體`ApplicationDbContext`和`ApplicationUserManager`會儲存在 OWIN 內容中，可以存取整個應用程式。 ASP.NET 身分識別連結到 cookie 中介軟體透過 OWIN 管線。 如需詳細資訊，請參閱 <<c0> [ 每個要求存留期管理，在 ASP.NET 身分識別的 UserManager 類別](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。

當您變更您的安全性設定檔時，會產生新的安全性戳記，並儲存在`SecurityStamp`欄位*AspNetUsers*資料表。 請注意，`SecurityStamp`欄位是不同的安全性 cookie。 安全性 cookie 不會儲存在`AspNetUsers`資料表 （或任何其他身分識別資料庫中的位置）。 使用自我簽署的安全性 cookie 語彙基元[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)並建立不含`UserId, SecurityStamp`和到期時間資訊。

Cookie 中介軟體會檢查每個要求的 cookie。 `SecurityStampValidator`方法中的`Startup`類別叫用的資料庫，並定期檢查安全性戳記依照`validateInterval`。 這只會每隔 30 分鐘 （在我們的範例），除非您變更您的安全性設定檔。 在 30 分鐘的間隔已選擇將存取資料庫的次數降至最低。 請參閱我[雙因素驗證的教學課程](index.md)如需詳細資訊。

每個程式碼的註解`UseCookieAuthentication`方法支援 cookie 驗證。 `SecurityStamp`欄位和相關聯的程式碼會提供額外一層的應用程式的安全性，當您變更密碼，您就會記錄從您用以登入瀏覽器。 `SecurityStampValidator.OnValidateIdentity`方法可讓應用程式的使用者登入時，驗證安全性權杖時變更密碼，或使用外部登入使用。 這樣才能確保以舊密碼產生的任何權杖 (cookie) 都會失效。 在範例專案中，如果您變更使用者密碼然後新的權杖會為使用者產生，任何先前的權杖都會失效和`SecurityStamp`欄位會更新。

身分識別系統可讓您設定您的應用程式，使用者的安全性設定檔變更時 (例如，當使用者變更其密碼或變更相關聯的登入 (例如 Facebook、 Google、 Microsoft 帳戶等)，使用者登出所有瀏覽器執行個體。 例如下, 圖顯示[單一登出的範例](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)應用程式，這可讓使用者選取一個按鈕，即可登出所有的瀏覽器執行個體 （在此情況下，在 IE、 Firefox 和 Chrome）。 或者，此範例可讓您只登出的特定瀏覽器執行個體。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[單一登出的範例](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)應用程式會顯示如何 ASP.NET 身分識別可讓您可以重新產生安全性權杖。 這樣才能確保以舊密碼產生的任何權杖 (cookie) 都會失效。 這項功能提供一層額外的安全性，以您的應用程式;當您變更您的密碼，則您會登出您已登入此應用程式。

*應用程式\_Start\IdentityConfig.cs*檔案包含`ApplicationUserManager`，`EmailService`和`SmsService`類別。 `EmailService`並`SmsService`類別會實作每個`IIdentityMessageService`介面，以便您在每個類別，以設定電子郵件和 SMS 常見的方法。 雖然本教學課程中只會顯示如何將透過電子郵件通知[SendGrid](http://sendgrid.com/)，您可以傳送電子郵件使用 SMTP 和其他機制。

`Startup`類別也包含將社交登入 （Facebook、 Twitter 等），請參閱我的教學課程的規範盤子[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)如需詳細資訊。

檢查`ApplicationUserManager`類別，其中包含的使用者身分識別資訊，並設定下列功能：

- 密碼強度需求。
- 使用者被鎖定 （嘗試次數和時間）。
- 雙因素驗證 (2FA)。 在另一個教學課程中，我會說明 2FA 和傳送簡訊。
- 連結的電子郵件和 SMS 服務。 （我會在另一個教學課程中說明 SMS）。

`ApplicationUserManager`類別衍生自泛型`UserManager<ApplicationUser>`類別。 `ApplicationUser` 衍生自[IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx)。 `IdentityUser` 衍生自泛型`IdentityUser`類別：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

上述的屬性中的屬性符合`AspNetUsers`資料表，如上所示。

泛型引數上的`IUser`可讓您在衍生類別，使用不同類型的主索引鍵。 請參閱[ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)範例顯示如何將主索引鍵從字串變更為 int 或 GUID。

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) 中定義*Models\IdentityModels.cs*為：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

上述反白顯示的程式碼會產生[ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)。 ASP.NET 身分識別和 OWIN 的 Cookie 驗證以宣告為基礎，因此架構需要應用程式來產生`ClaimsIdentity`使用者。 `ClaimsIdentity` 資訊的相關使用者的名稱，例如使用者的所有宣告年齡和哪些角色的使用者屬於。 您也可以在這個階段中新增更多的使用者宣告。

OWIN`AuthenticationManager.SignIn`方法會傳入`ClaimsIdentity`並登入使用者：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)顯示如何新增其他屬性，以`ApplicationUser`類別。

## <a name="email-confirmation"></a>電子郵件確認

它是個不錯的主意，以確認新的使用者向以確認它們不會模擬其他人的電子郵件 （亦即，尚未註冊使用其他人的電子郵件）。 假設您有討論論壇，您會想要防止`"bob@example.com"`註冊為`"joe@contoso.com"`。 而不需要電子郵件確認`"joe@contoso.com"`無法從您的應用程式收到不想要的電子郵件。 假設 Bob 不小心註冊為`"bib@example.com"`並沒注意到，他便無法使用密碼復原，因為應用程式不需要其正確的電子郵件。 電子郵件確認來自 bot 提供有限的保護，並不提供保護，從決定濫發垃圾郵件者，它們有許多的工作電子郵件別名，它們可用來註冊。在下列範例中，使用者將無法變更其密碼，直到已確認他們的帳戶 （藉由它們選取他們註冊的電子郵件帳戶收到的確認連結。）您可以將此工作流程套用至其他案例中，例如傳送確認及重設系統管理員，等變更其設定檔的使用者的使用者傳送電子郵件建立的新帳戶密碼的連結。 您通常想要防止新使用者張貼至您的網站的任何資料之前先確認電子郵件、 SMS 文字訊息或其他機制。 <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>建立更完整的範例

在本節中，您會使用 NuGet 來下載更完整範例，我們將使用。

1. 建立新***空***ASP.NET Web 專案。
2. 在套件管理員主控台中，輸入下列命令： 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   在本教學課程中，我們將使用[SendGrid](http://sendgrid.com/)傳送電子郵件。 `Identity.Samples`套件會安裝我們將使用的程式碼。
3. 設定[專案，以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. 測試建立本機帳戶執行應用程式中，選取**註冊**連結，然後張貼之註冊表單。
5. 選取示範電子郵件連結，以模擬電子郵件確認。
6. 移除此範例示範電子郵件連結確認程式碼 (`ViewBag.Link`帳戶控制器中的程式碼。 請參閱`DisplayEmail`和`ForgotPasswordConfirmation`動作方法和 razor 檢視)。

> [!WARNING]
> 如果您變更任何安全性設定，在此範例中，生產應用程式必須進行安全性稽核會明確呼叫所做的變更。


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>檢查應用程式中的程式碼\_Start\IdentityConfig.cs

此範例示範如何建立帳戶，並將它加入*Admin*角色。 您應該更換此範例中的電子郵件，您將使用系統管理員帳戶的電子郵件。 現在若要建立系統管理員帳戶的最簡單方式是以程式設計方式在`Seed`方法。 我們希望有這項工具在未來，可讓您建立及管理使用者和角色。 範例程式碼可以讓您建立和管理使用者和角色，但您必須先具備系統管理員帳戶以執行的角色和使用者管理頁面。 在此範例中，就會植入資料庫時，會建立系統管理員帳戶。

變更密碼，並將名稱變更為您可以在何處接收電子郵件通知的帳戶。

> [!WARNING]
> 安全性-機密資料絕不儲存在原始程式碼中。

如先前所述`app.CreatePerOwinContext`startup 類別中的呼叫加入回呼`Create`DB 的應用程式內容、 使用者管理員和角色管理員類別的方法。 OWIN 管線呼叫`Create`方法，這些類別的每個要求，並將每個類別的內容。 帳戶控制器會公開使用者管理員從 HTTP 內容 （其中包含 OWIN 內容）：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

當使用者註冊本機帳戶，`HTTP Post Register`方法呼叫：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

上述程式碼會使用模型資料來建立新的使用者帳戶使用的電子郵件和輸入的密碼。 資料存放區中的電子郵件別名時，帳戶建立失敗，並重新顯示表單。 `GenerateEmailConfirmationTokenAsync`方法會建立安全的確認語彙基元，並將它儲存在 ASP.NET 身分識別資料存放區。 [其中使用了 Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx)方法會建立一個連結，其中包含`UserId`和確認語彙基元。 此連結，然後傳送電子郵件給使用者，使用者可以選取其電子郵件應用程式中的連結，以確認其帳戶。

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>設定電子郵件確認

移至[Azure SendGrid 註冊頁面](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)並註冊免費帳戶。 加入程式碼來設定 SendGrid 如下所示：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> 電子郵件用戶端經常只接受文字 (沒有 HTML)。 您應該提供中的文字和 HTML 的訊息。 在 SendGrid 上述範例中，做法是使用`myMessage.Text`和`myMessage.Html`如上所示的程式碼。


下列程式碼示範如何使用電子郵件傳送[MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)類別 where`message.Body`傳回只的連結。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> 安全性-機密資料絕不儲存在原始程式碼中。 帳戶和認證會儲存在 appSetting 中。 在 Azure 上，您可以安全地儲存這些值在 **[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** 在 Azure 入口網站中的索引標籤。 請參閱[最佳做法將密碼和其他機密資料部署到 ASP.NET 和 Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。


輸入您的 SendGrid 認證，執行應用程式，註冊的電子郵件別名可以選取 [確認] 連結您的電子郵件中。 若要查看如何執行這項作業您[Outlook.com](http://outlook.com)電子郵件帳戶，請參閱 John Atten [ C# Outlook.Com SMTP 主機的 SMTP 組態](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx)和他[ASP.NET 身分識別 2.0:設定註冊帳戶驗證和雙因素授權](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)文章。

一旦使用者選取**註冊**電子郵件地址會傳送包含驗證語彙基元確認電子郵件 按鈕。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

含有確認語彙基元，為其帳戶的電子郵件傳送給使用者。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>檢查程式碼

下列程式碼顯示 `POST ForgotPassword` 方法。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

如果尚未確認使用者電子郵件，方法就會以無訊息模式失敗。 無效的電子郵件地址張貼錯誤時，如果惡意使用者就可以使用該資訊來尋找有效的使用者識別碼 （電子郵件別名） 攻擊。

下列程式碼示範`ConfirmEmail`在帳戶控制器當使用者在傳送給他們的電子郵件中選取 [確認] 連結時所呼叫的方法：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

一旦已使用遺忘的密碼權杖，它會失效。 下列程式碼變更`Create`方法 (在*應用程式\_Start\IdentityConfig.cs*檔案) 會設定在過去 3 小時內到期的權杖。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

使用上述的程式碼，忘記的密碼和電子郵件確認語彙基元將於過去 3 小時內過期。 預設值`TokenLifespan`是一天。

下列程式碼顯示電子郵件確認方法：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 若要讓您的應用程式更安全，ASP.NET 身分識別支援雙因素驗證 (2FA)。 請參閱[ASP.NET 身分識別 2.0:設定帳戶驗證和雙因素授權](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)由 John Atten。 雖然您可以設定帳戶鎖定的密碼登入嘗試失敗，該方法可讓您的登入容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)鎖定。 我們建議您只適用於帳戶鎖定 2FA。  
<a id="addRes"></a>

## <a name="additional-resources"></a>其他資源

- [ASP.NET Identity 的自訂儲存體提供者概觀](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)也會示範如何將使用者資料表中的設定檔資訊。
- [ASP.NET MVC 和身分識別 2.0:了解基本概念](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)由 John Atten。
- [ASP.NET Identity 簡介](../getting-started/introduction-to-aspnet-identity.md)
- [宣佈 ASP.NET 身分識別 2.0.0 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)請參閱 Pranav rastogi。
