---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: 建立安全的 ASP.NET Web Forms 應用程式與使用者註冊、 電子郵件確認和密碼重設 (C#) |Microsoft Docs
author: Erikre
description: 本教學課程會示範如何建置 ASP.NET Web Forms 應用程式與使用者註冊、 電子郵件確認和密碼重設使用 ASP.NET Identity 成員...
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1c230c3f33bd8261312485e9d77f6f88adb49e9e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812765"
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>建立安全的 ASP.NET Web Forms 應用程式與使用者註冊、 電子郵件確認和密碼重設 (C#)
====================
藉由[Erik Reitan](https://github.com/Erikre)

> 本教學課程會示範如何建置 ASP.NET Web Forms 應用程式與使用者註冊、 電子郵件確認和密碼重設使用 ASP.NET 身分識別的成員資格系統。 本教學課程已根據 Rick Anderson [MVC 教學課程](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。


## <a name="introduction"></a>簡介

本教學課程會引導您完成建立 ASP.NET Web Forms 應用程式使用 Visual Studio 和 ASP.NET 4.5 來建立安全的 Web Form 應用程式以使用者註冊、 電子郵件確認和密碼重設所需的步驟。

### <a name="tutorial-tasks-and-information"></a>教學課程的工作和資訊：

- [建立 ASP.NET Web Forms 應用程式](#createWebForms)
- [將 SendGrid 連結](#SG)
- [需要先登入的電子郵件確認](#require)
- [密碼復原和重設](#reset)
- [重新傳送電子郵件確認連結](#rsend)
- [疑難排解應用程式](#dbg)
- [其他資源](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>建立 ASP.NET Web Forms 應用程式

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以及。

> [!NOTE]
> 警告： 您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更新版本，才能完成本教學課程。


1. 建立新的專案 (**檔案** - &gt; **新專案**)，然後選取**ASP.NET Web 應用程式**範本和最新的.NET Framework從版本**新的專案** 對話方塊。
2. 從**新的 ASP.NET 專案**對話方塊中，選取**Web Form**範本。 保留為預設的驗證**個別使用者帳戶**。 如果您想要裝載在 Azure 中的應用程式，將保留**雲端中的主機**勾選核取方塊。   
 然後，按一下**確定**建立新的專案。  
    ![[新增 ASP.NET 專案] 對話方塊](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. 啟用 Secure Sockets Layer (SSL) 專案。 請遵循中提供的步驟**啟用專案的 SSL**一節[開始使用 Web Form 教學課程系列](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)。
4. 執行應用程式，請按一下**註冊**連結，並註冊新的使用者。 唯一的驗證電子郵件根據到目前為止， [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)以確保電子郵件地址的格式正確的屬性。 您將修改程式碼來新增電子郵件確認。 關閉瀏覽器視窗。
5. 在 [**伺服器總管**的 Visual Studio (**檢視** - &gt; **伺服器總管]**)，瀏覽至**資料 Connections\DefaultConnection\Tables\AspNetUsers**，以滑鼠右鍵按一下並選取**開啟資料表定義**。 

    下圖顯示`AspNetUsers`資料表結構描述：

    ![AspNetUsers 資料表結構描述](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. 在 **伺服器總管**，以滑鼠右鍵按一下**AspNetUsers**資料表，然後選取**顯示資料表資料**。  
  
    ![AspNetUsers 資料表資料](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 此時已不確認的註冊使用者的電子郵件。
7. 按一下資料列，然後選取刪除要刪除的使用者。 您將在下一個步驟中再次新增這封電子郵件，並將確認訊息傳送至電子郵件地址。

## <a name="email-confirmation"></a>電子郵件確認

最好確認電子郵件以確認它們不會模擬其他人的新使用者的註冊期間 （也就是尚未註冊使用其他人的電子郵件）。 假設您有討論論壇，您會想要防止`"bob@cpandl.com"`註冊為`"joe@contoso.com"`。 而不需要電子郵件確認`"joe@contoso.com"`無法從您的應用程式收到不想要的電子郵件。 假設 Bob 不小心註冊為`"bib@cpandl.com"`並沒注意到，他便無法使用密碼復原，因為應用程式不需要其正確的電子郵件。 電子郵件確認從 bot 提供有限的保護，並不提供從決定濫發垃圾郵件者的保護。

您通常想要防止新使用者張貼至您的網站的任何資料之前確認透過其中一個電子郵件、 SMS 文字訊息或其他機制。 在下列章節中，我們將啟用電子郵件確認和修改程式碼以防止新註冊的使用者登入，直到已確認其電子郵件。 您將在本教學課程中使用 SendGrid 電子郵件服務。

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>將 SendGrid 連結

雖然本教學課程中只會顯示如何將透過電子郵件通知[SendGrid](http://sendgrid.com/)，您可以傳送電子郵件使用 SMTP 和其他機制 (請參閱[其他資源](#addRes))。

1. 在 Visual Studio 中開啟**Package Manager Console** (**工具** - &gt; **NuGet 封裝管理員** - &gt;**Package Manager Console**)，並輸入下列命令：  
    `Install-Package SendGrid`
2. 移至[Azure SendGrid 註冊頁面](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)並註冊免費的 SendGrid 帳戶。 您可以也註冊免費的 SendGrid 帳戶直接依據[SendGrid 的站台](http://www.sendgrid.com)。
3. 從**方案總管**開啟*IdentityConfig.cs*中的檔案*應用程式\_啟動*資料夾，並新增下列程式碼的黃色反白顯示`EmailService`類別，以設定**SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. 此外，新增下列`using`開頭的陳述式*IdentityConfig.cs*檔案： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. 為了簡化此範例中，您將儲存中的電子郵件服務帳戶值`appSettings`一節*web.config*檔案。 新增下列 XML 以黃色反白顯示*web.config*您的專案根目錄的檔案：

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > 安全性-機密資料絕不儲存在原始程式碼中。 在此範例中，帳戶和認證會儲存在**appSetting**一節*Web.config*檔案。 在 Azure 上，您可以安全地儲存這些值在**[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** 在 Azure 入口網站中的索引標籤。 如需相關資訊，請參閱 Rick Anderson 主題[最佳做法將密碼和其他機密資料部署到 ASP.NET 和 Azure](https://go.microsoft.com/fwlink/?LinkId=513141)。
6. 新增電子郵件服務值，以反映您的 SendGrid 驗證值 （使用者名稱和密碼），讓您可以成功傳送電子郵件從您的應用程式。 請務必使用您的 SendGrid 帳戶名稱，而不是您提供 SendGrid 電子郵件地址。

### <a name="enable-email-confirmation"></a>啟用電子郵件確認

 若要啟用電子郵件確認，您將修改的登錄程式碼使用下列步驟。  
 

1. 在 *帳戶*資料夾中，開啟*Register.aspx.cs*程式碼後置，並更新`CreateUser_Click`方法，以啟用下列醒目提示的變更： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. 在 [**方案總管] 中**，以滑鼠右鍵按一下*Default.aspx* ，然後選取**設定為起始頁**。
3. 執行應用程式藉由按下**F5。** 頁面會顯示之後，請按一下**註冊**連結可以顯示 [註冊] 頁面。
4. 輸入您的電子郵件和密碼，然後按一下 [**註冊**] 按鈕，透過 SendGrid 電子郵件訊息傳送。  
   您的專案和程式碼的目前狀態可讓使用者登入後他們完成註冊表單中，即使它們尚未確認他們的帳戶。
5. 檢查您的電子郵件帳戶，然後按一下連結，以確認您的電子郵件。  
   一旦您提交註冊表單，您將登入。  
    ![範例網站-登入](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>需要先登入的電子郵件確認

雖然您已確認的電子郵件帳戶，此時您就不需要完整登入的驗證電子郵件中所包含的連結上按一下。 在下一節中，您將修改要求新的使用者將確認電子郵件之前在登入時 （驗證） 的程式碼。

1. 在 [**方案總管] 中**的 Visual Studio 中，更新`CreateUser_Click`中的事件*Register.aspx.cs*程式碼後置中包含*帳戶*資料夾中的，使用下列醒目提示的變更： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. 更新`LogIn`方法中的*Login.aspx.cs*程式碼後置與下列醒目提示的變更： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>執行應用程式

 既然您已實作的程式碼，以檢查是否已確認使用者的電子郵件地址，您可以檢查的功能，兩者**註冊**並**登入**頁面。 

1. 刪除中的任何帳戶**AspNetUsers**包含您想要測試的電子郵件別名的資料表。
2. 執行應用程式 (**F5**)，並確認您已確認您的電子郵件地址之前，無法註冊為使用者。
3. 確認新的帳戶透過剛被傳送的電子郵件之前, 嘗試新的帳戶登入。  
 您會看到您已無法登入進行，您必須確認電子郵件帳戶。
4. 一旦您確認您的電子郵件地址，請登入應用程式。

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>密碼復原和重設

1. 在 Visual Studio 中，移除註解字元`Forgot`方法中的*Forgot.aspx.cs*程式碼後置內*帳戶*資料夾中，因此，此方法會顯示為如下所示： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. 開啟*Login.aspx*頁面。 將標記結尾附近**loginForm**區段以反白顯示如下： 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. 開啟*Login.aspx.cs*程式碼後置和下列從黃色反白顯示的程式碼行取消註解`Page_Load`事件處理常式： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. 執行應用程式藉由按下**F5。** 頁面會顯示之後，請按一下**登入**連結。
5. 按一下 **忘記密碼？** 連結，以顯示**忘記密碼**頁面。
6. 輸入您的電子郵件地址，然後按一下**送出**傳送電子郵件給您的地址才可重設密碼 按鈕。   
   檢查您的電子郵件帳戶，然後按一下連結以顯示**重設密碼**頁面。
7. 在 **重設密碼**頁面上，輸入您的電子郵件、 密碼和確認的密碼。 然後按**重設** 按鈕。  
   當您已成功重設密碼，請**變更密碼**將會顯示頁面。 現在您可以登入您的新密碼。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>重新傳送電子郵件確認連結

一旦使用者建立新的本機帳戶時，它們是電子郵件確認連結，他們才能使用之前，他們可以登入。 如果使用者不小心刪除的確認電子郵件，或永遠不會收到電子郵件，他們必須重新傳送確認連結。 下列程式碼變更會示範如何啟用此功能。

1. 在 Visual Studio 中開啟**Login.aspx.cs**程式碼後置，並新增下列事件處理常式之後`LogIn`事件處理常式：   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. 修改`LogIn`中的事件處理常式*Login.aspx.cs*程式碼後置藉由變更黃色反白顯示，如下所示的程式碼： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. 更新*Login.aspx*頁面加上黃色反白顯示，如下所示的程式碼： 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. 刪除中的任何帳戶**AspNetUsers**包含您想要測試的電子郵件別名的資料表。
5. 執行應用程式 (**F5**)，並註冊您的電子郵件地址。
6. 確認新的帳戶透過剛被傳送的電子郵件之前, 嘗試新的帳戶登入。  
   您會看到您已無法登入進行，您必須確認電子郵件帳戶。 此外，您現在可以重新確認訊息傳送至您的電子郵件帳戶。
7. 輸入您的電子郵件地址和密碼，然後按**重新傳送確認** 按鈕。
8. 一旦您確認您根據新傳送的電子郵件訊息的電子郵件地址，請登入應用程式。

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>疑難排解應用程式

如果您未收到電子郵件包含此連結來確認您的認證：

- 請檢查垃圾郵件或垃圾郵件資料夾。
- 登入您的 SendGrid 帳戶，然後按一下[電子郵件 」 活動連結](https://sendgrid.com/logs/index)。
- 確定您使用您的 SendGrid 使用者帳戶名稱，作為*Web.config*值而不是您的 SendGrid 帳戶電子郵件地址。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

- [連結至 ASP.NET Identity 建議資源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Web Form 教學課程系列-新增 OAuth 2.0 提供者](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Form 教學課程系列-啟用專案的 SSL](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
