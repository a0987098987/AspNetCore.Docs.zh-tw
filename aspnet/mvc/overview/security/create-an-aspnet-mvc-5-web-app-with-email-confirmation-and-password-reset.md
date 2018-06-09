---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: 建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，確認和密碼重設 (C#) 傳送電子郵件 |Microsoft 文件
author: Rick-Anderson
description: 本教學課程會示範如何建立 ASP.NET MVC 5 web 應用程式，但電子郵件確認和密碼重設使用 ASP.NET 識別的成員資格系統。 您的 ca...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: bfa5d52019be81374c7a544e255ab7ffb301fa7b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "34452564"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設 (C#)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何建立 ASP.NET MVC 5 web 應用程式，但電子郵件確認和密碼重設使用 ASP.NET 識別的成員資格系統。 您可以下載完成的應用程式[這裡](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 此下載包含偵錯的協助程式，可讓您測試而不需設定電子郵件或 SMS 提供者的電子郵件確認與簡訊。
> 
> 本教學課程中所編寫的[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>建立 ASP.NET MVC 應用程式

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。

> [!NOTE]
> 警告： 您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本，才能完成本教學課程。


1. 建立新的 ASP.NET Web 專案，然後選取 MVC 範本。 Web Form 也支援 ASP.NET Identity，因此您可以依照類似的步驟，在 web form 應用程式。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. 保留預設驗證為**個別使用者帳戶**。 如果您想要裝載應用程式在 Azure 中的，將保留勾選核取方塊。 稍後在本教學課程中，我們將部署至 Azure。 您可以[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。
3. 設定[專案以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. 執行應用程式，請按一下**註冊**連結和註冊的使用者。 此時，只有在電子郵件時才驗證與[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)屬性。
5. 在 伺服器總管瀏覽至**資料 Connections\DefaultConnection\Tables\AspNetUsers**，以滑鼠右鍵按一下並選取**開啟資料表定義**。

    下圖顯示`AspNetUsers`結構描述：

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. 以滑鼠右鍵按一下**AspNetUsers**資料表，然後選取**顯示資料表資料**。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 此時不已確認電子郵件。
7. 按一下資料列上，選取 [刪除]。 您將在下一個步驟中，再次新增這封電子郵件並傳送確認電子郵件。

## <a name="email-confirmation"></a>電子郵件確認

若要確認新的使用者註冊，以確認它們不會模擬其他人的電子郵件的最佳作法是 （也就是它們未向其他人的電子郵件）。 假設您有討論論壇，您會想要防止`"bob@example.com"`從註冊為`"joe@contoso.com"`。 沒有電子郵件確認`"joe@contoso.com"`無法從您的應用程式取得垃圾電子郵件。 假設 Bob 意外地註冊為`"bib@example.com"`而且未注意到，他就無法使用密碼復原，因為應用程式沒有他正確的電子郵件。 電子郵件確認從 bot 提供有限的保護，並不提供保護，從決定垃圾郵件，它們有許多的工作電子郵件別名，他們可以使用來註冊。

您通常想要讓新的使用者張貼到您的網站的任何資料之前已經確認透過電子郵件、 簡訊或其他機制。 <a id="build"></a>在下列章節中，我們將啟用電子郵件確認，並修改程式碼以防止新註冊的使用者登入，直到已確認他們的電子郵件。

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid 連結

雖然本教學課程只會顯示如何將電子郵件通知，透過[SendGrid](http://sendgrid.com/)，您可以傳送電子郵件使用 SMTP 和其他機制 (請參閱[其他資源](#addRes))。

1. 在 Package Manager Console 中，輸入下列命令： 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. 移至[Azure SendGrid 登入頁面](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)並註冊免費的 SendGrid 帳戶。 設定 SendGrid 將類似下列的程式碼加入*App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

您必須加入包含下列：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

若要維持此範例的簡單性，我們將會儲存中的應用程式設定*web.config*檔案：

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> 安全性-永遠不會儲存敏感的資料來源上的程式碼中。 帳戶和認證會儲存在 appSetting。 在 Azure 上，您可以安全地儲存這些值在**[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure 入口網站中的索引標籤。 請參閱[ASP.NET 和 Azure 部署的密碼和其他機密資料的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。


### <a name="enable-email-confirmation-in-the-account-controller"></a>啟用帳戶控制器中的電子郵件確認

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

確認*Views\Account\ConfirmEmail.cshtml*檔案具有正確的 razor 語法。 (在第一個字元 @ 列可能會遺失。 )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

執行應用程式，然後按一下 [註冊] 連結。 一旦您提交註冊表單時，您登入。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

檢查您的電子郵件帳戶，然後按一下連結來確認您的電子郵件。

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>電子郵件之前需要確認登入

目前在使用者完成註冊表單，一旦他們登入。 您通常想要確認他們的電子郵件才能加以登入。 在下面的區段中，我們將會修改程式碼以要求新的使用者將確認電子郵件之前登入 （驗證）。 更新`HttpPost Register`下列反白顯示變更的方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

註解`SignInAsync`方法，使用者將未簽署註冊。 `TempData["ViewBagLink"] = callbackUrl;`行可以用來[偵錯應用程式](#dbg)和測試不會傳送電子郵件的註冊。 `ViewBag.Message` 用來顯示的確認指示。 [下載範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)包含程式碼以測試電子郵件確認，而不需設定電子郵件，而且也可用來偵錯應用程式。

建立`Views\Shared\Info.cshtml`檔案，然後加入下列的 razor 標記：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

新增[Authorize 屬性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)至`Contact`主控制器動作方法。 您可以按一下**連絡人**連結，確認匿名使用者沒有存取權和已驗證的使用者沒有存取權。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

您也必須更新`HttpPost Login`動作方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

更新*Views\Shared\Error.cshtml*檢視以顯示錯誤訊息：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

刪除中的任何帳戶**AspNetUsers**包含您想要使用測試的電子郵件別名的資料表。 執行應用程式，並確認在您確認電子郵件地址之前，您無法登入。 一旦您確認電子郵件地址，請按一下**連絡人**連結。

<a id="reset"></a>
## <a name="password-recoveryreset"></a>復原/重設密碼

移除註解字元從`HttpPost ForgotPassword`中帳戶控制器動作方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

移除註解字元從`ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)中*Views\Account\Login.cshtml* razor 檢視檔案：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

登入頁面現在會顯示連結，以重設密碼。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>重新傳送電子郵件確認連結

一旦使用者建立新的本機帳戶，它們是電子郵件確認連結需要它們來登入前使用。 如果使用者意外刪除的確認電子郵件，或永遠不會收到電子郵件，他們必須重新傳送的確認連結。 下列程式碼變更會示範如何啟用此功能。

將下列 helper 方法加入至底部*Controllers\AccountController.cs*檔案：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

更新要使用新的協助程式的註冊方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

更新重新傳送的密碼，如果尚未確認的使用者帳戶登入方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>結合社交和本機登入帳戶

您可以結合本機和社交帳戶按一下電子郵件連結。 依照以下順序**RickAndMSFT@gmail.com**首先會建立為本機登入，但您可以建立帳戶與社交的記錄檔中第一個，然後加入本機的登入。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

按一下**管理**連結。 請注意**外部登入： 0**與此帳戶相關聯。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

按一下此連結服務中的另一個記錄檔，並接受應用程式要求。 已結合兩個帳戶，您將無法與任一個帳戶登入。 您可能會想讓使用者加入本機帳戶，以防使用者社交的記錄檔中驗證服務已關閉，或可能比較他們已經失去其社交帳戶的存取。

在下圖中，Tom 是社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

按一下**挑選密碼**可讓您在上新增本機記錄檔相同的帳戶相關聯。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>更深入的電子郵件確認

我的教學課程[帳戶確認和 ASP.NET 識別的密碼復原](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入這個主題更詳細的資料。

<a id="dbg"></a>
## <a name="debugging-the-app"></a>偵錯應用程式

如果您未收到包含連結的電子郵件：

- 請檢查您的垃圾郵件資料夾。
- 登入您的 SendGrid 帳戶並按一下[電子郵件 」 活動連結](https://sendgrid.com/logs/index)。

若要測試沒有電子郵件驗證連結，下載[完成的範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 確認碼和確認連結將顯示在頁面上。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

- [ASP.NET Identity 的連結，建議使用的資源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [帳戶確認和密碼復原與 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入確認的密碼復原和帳戶的其他詳細資料。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 」 授權。 它也會示範如何加入識別資料庫中的其他資料。
- [將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 本教學課程將加入 Azure 部署時，如何保護您的應用程式角色、 如何使用成員資格應用程式開發介面來新增使用者和角色，以及其他安全性功能。
- [OAuth 2 建立 Google 應用程式和應用程式連接至專案](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [在 Facebook 中建立應用程式和應用程式連接至專案](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [在專案中的 SSL 設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
