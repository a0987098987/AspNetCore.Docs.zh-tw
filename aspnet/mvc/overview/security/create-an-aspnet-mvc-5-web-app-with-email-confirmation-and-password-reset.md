---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: 建立安全的 ASP.NET MVC 5 web 應用程式登入、 電子郵件確認和密碼重設 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何建置 ASP.NET MVC 5 web 應用程式使用電子郵件確認和密碼重設使用 ASP.NET 身分識別的成員資格系統。 您的 ca...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: e15595cab2d1f51374d4577a67f0f190a531acb5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824340"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>建立安全的 ASP.NET MVC 5 web 應用程式登入、 電子郵件確認和密碼重設 (C#)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何建置 ASP.NET MVC 5 web 應用程式使用電子郵件確認和密碼重設使用 ASP.NET 身分識別的成員資格系統。 您可以下載完成的應用程式[此處](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 此下載包含可讓您測試而不需設定電子郵件或 SMS 提供者的電子郵件確認和 SMS 的偵錯協助程式。
> 
> 本教學課程以寫入[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>建立 ASP.NET MVC 應用程式

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。

> [!NOTE]
> 警告： 您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更新版本，才能完成本教學課程。


1. 建立新的 ASP.NET Web 專案，然後選取 [MVC] 範本。 Web Form 也支援 ASP.NET 身分識別，因此您可以依照類似的步驟，在 web form 應用程式。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. 保留為預設的驗證**個別使用者帳戶**。 如果您想要裝載應用程式在 Azure 中的，核取方塊保持勾選。 稍後在本教學課程中，我們將會部署到 Azure。 您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。
3. 設定[專案，以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. 執行應用程式，請按一下**註冊**連結，並註冊的使用者。 此時，唯一的驗證電子郵件是使用[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)屬性。
5. 在 [伺服器總管] 中，瀏覽至**資料 Connections\DefaultConnection\Tables\AspNetUsers**，以滑鼠右鍵按一下並選取**開啟資料表定義**。

    下圖顯示`AspNetUsers`結構描述：

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. 以滑鼠右鍵按一下**AspNetUsers**資料表，然後選取**顯示資料表資料**。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 此時已不確認電子郵件。
7. 按一下資料列，然後選取 刪除。 您將在下一個步驟中，再次新增這封電子郵件並傳送確認電子郵件。

## <a name="email-confirmation"></a>電子郵件確認

最好確認新的使用者註冊，以確認它們不會模擬其他人的電子郵件 （亦即，尚未註冊使用其他人的電子郵件）。 假設您有討論論壇，您會想要防止`"bob@example.com"`註冊為`"joe@contoso.com"`。 而不需要電子郵件確認`"joe@contoso.com"`無法從您的應用程式收到不想要的電子郵件。 假設 Bob 不小心註冊為`"bib@example.com"`並沒注意到，他便無法使用密碼復原，因為應用程式不需要其正確的電子郵件。 電子郵件確認來自 bot 提供有限的保護，並不提供保護，從決定濫發垃圾郵件者，它們有許多的工作電子郵件別名，它們可用來註冊。

您通常想要防止新使用者張貼至您的網站的任何資料之前先確認電子郵件、 SMS 文字訊息或其他機制。 <a id="build"></a>在下列章節中，我們將啟用電子郵件確認和修改程式碼以防止新註冊的使用者登入，直到已確認其電子郵件。

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>將 SendGrid 連結

雖然本教學課程中只會顯示如何將透過電子郵件通知[SendGrid](http://sendgrid.com/)，您可以傳送電子郵件使用 SMTP 和其他機制 (請參閱[其他資源](#addRes))。

1. 在套件管理員主控台中，輸入下列命令： 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. 移至[Azure SendGrid 註冊頁面](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)並註冊免費的 SendGrid 帳戶。 將新增類似下列程式碼，以設定 SendGrid *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

您必須新增下列 include:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

為了簡化此範例中，我們將儲存在應用程式設定*web.config*檔案：

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> 安全性-機密資料絕不儲存在原始程式碼中。 帳戶和認證會儲存在 appSetting 中。 在 Azure 上，您可以安全地儲存這些值在 **[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** 在 Azure 入口網站中的索引標籤。 請參閱[最佳做法將密碼和其他機密資料部署到 ASP.NET 和 Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。


### <a name="enable-email-confirmation-in-the-account-controller"></a>啟用帳戶控制器中的電子郵件確認

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

請確認*Views\Account\ConfirmEmail.cshtml*檔案有正確的 razor 語法。 (@ 字元在第一個列可能會遺失。 )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

執行應用程式，然後按一下 [註冊] 連結。 一旦您提交註冊表單，您會登入。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

檢查您的電子郵件帳戶，然後按一下連結，以確認您的電子郵件。

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>需要先登入的電子郵件確認

目前在使用者完成註冊表單，一旦他們登入。 您通常想要記錄之前，先確認其電子郵件。 在下一節，我們將修改的程式碼，以要求新的使用者將確認電子郵件之前在登入時 （驗證）。 更新`HttpPost Register`方法具有下列醒目提示的變更：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

註解`SignInAsync`方法，使用者將不會簽署註冊。 `TempData["ViewBagLink"] = callbackUrl;`行可以用來[偵錯應用程式](#dbg)和測試不會傳送電子郵件的註冊。 `ViewBag.Message` 用來顯示的確認指示。 [下載範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)包含程式碼來測試而不需設定電子郵件的電子郵件確認和也可用來偵錯應用程式。

建立`Views\Shared\Info.cshtml`檔案，並新增下列 razor 標記：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

新增[Authorize 屬性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)到`Contact`首頁控制器的動作方法。 您可以按一下**連絡人**連結來確認匿名使用者沒有存取，而且已驗證的使用者確實具備存取權。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

您也必須更新`HttpPost Login`動作方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

更新*Views\Shared\Error.cshtml*檢視，以顯示錯誤訊息：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

刪除中的任何帳戶**AspNetUsers**包含您想要測試的電子郵件別名的資料表。 執行應用程式，並確認您已確認您的電子郵件地址之前，您無法登入。 一旦您確認您的電子郵件地址，請按一下**連絡人**連結。

<a id="reset"></a>
## <a name="password-recoveryreset"></a>復原/重設密碼

移除註解字元`HttpPost ForgotPassword`帳戶控制器中的動作方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

移除註解字元`ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)中*Views\Account\Login.cshtml* razor 檢視檔案：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

登入頁面現在會顯示連結，以重設密碼。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>重新傳送電子郵件確認連結

一旦使用者建立新的本機帳戶時，它們是電子郵件確認連結，他們才能使用之前，他們可以登入。 如果使用者不小心刪除的確認電子郵件，或永遠不會收到電子郵件，他們必須重新傳送確認連結。 下列程式碼變更會示範如何啟用此功能。

新增下列 helper 方法的底部*Controllers\AccountController.cs*檔案：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

更新 Register 方法將使用新的協助：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

更新重新傳送的密碼，如果尚未確認的使用者帳戶登入方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>結合社交和本機登入帳戶

您可以結合本機和社交帳戶，按一下您的電子郵件連結。 依照以下順序**RickAndMSFT@gmail.com**首先會建立為本機登入，但您可以為社交登入第一，建立帳戶，然後新增本機登入。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

按一下 **管理**連結。 附註**外部登入： 0**與此帳戶相關聯。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

按一下連結以在服務中的另一個記錄檔，並接受應用程式要求。 已合併兩個帳戶，您可以使用任一個帳戶登入。 您可能想要新增本機帳戶，以防使用者社交的記錄檔，在 驗證服務已關閉，或是更有可能他們已無法存取其社交帳戶使用者。

在下圖中，Tom 則社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

按一下**挑選密碼**可讓您在新增本機記錄檔相同的帳戶相關聯。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>深入的電子郵件確認

我的教學課程[帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入本主題含有詳細資料。

<a id="dbg"></a>
## <a name="debugging-the-app"></a>偵錯應用程式

如果您未收到包含連結的電子郵件：

- 請檢查垃圾郵件或垃圾郵件資料夾。
- 登入您的 SendGrid 帳戶，然後按一下[電子郵件 」 活動連結](https://sendgrid.com/logs/index)。

若要測試沒有電子郵件驗證連結，下載[完整的範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 頁面上，將會顯示的確認連結和確認碼。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

- [連結至 ASP.NET Identity 建議資源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入 確認的密碼復原和帳戶的其他詳細資料。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 授權。 它也會示範如何新增額外的資料來識別資料庫。
- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 本教學課程中新增 Azure 部署中，如何保護您的應用程式角色，如何使用成員資格 API 來新增使用者和角色，以及額外的安全性功能。
- [建立 Google app for OAuth 2 和應用程式連線至專案](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [在 Facebook 中建立應用程式和應用程式連線至專案](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [設定專案中的 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
