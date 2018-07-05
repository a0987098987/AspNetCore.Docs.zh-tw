---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: 建立 ASP.NET Web Forms 應用程式使用 SMS 雙因素驗證 (C#) |Microsoft Docs
author: Erikre
description: 本教學課程會示範如何建置使用雙因素驗證的 ASP.NET Web Forms 應用程式。 本教學課程已設計為來補充本教學課程標題為 Cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1231ed0cb238ad35c12405dd9a264ccfd7332960
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401288"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>建立 ASP.NET Web Forms 應用程式使用 SMS 雙因素驗證 (C#)
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載電子郵件和 SMS 的雙因素驗證的 ASP.NET Web Forms 應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> 本教學課程會示範如何建置使用雙因素驗證的 ASP.NET Web Forms 應用程式。 本教學課程旨在補足標題為本教學課程[建立安全的 ASP.NET Web Forms 應用程式以使用者註冊、 電子郵件確認和密碼重設](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)。 此外，本教學課程已根據 Rick Anderson [MVC 教學課程](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)。


## <a name="introduction"></a>簡介

本教學課程會引導您完成建立 ASP.NET Web Forms 應用程式支援使用 Visual Studio 的雙因素驗證所需的步驟。 雙因素驗證是額外的使用者驗證步驟。 這個額外步驟會在登入期間產生唯一個人識別碼 (PIN)。 Pin 碼通常會傳送給電子郵件或簡訊的使用者。 在登入時，您的應用程式的使用者會將 PIN 然後進入作為額外的驗證措施。

### <a name="tutorial-tasks-and-information"></a>教學課程的工作和資訊：

- [建立 ASP.NET Web Forms 應用程式](#createWebForms)
- [設定 SMS 和雙因素驗證](#SMS)
- [啟用雙因素驗證已註冊的使用者](#use2FA)
- [其他資源](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>建立 ASP.NET Web Forms 應用程式

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以及。 此外，您必須建立[Twilio](https://www.twilio.com/try-twilio)帳戶，如下所述。

> [!NOTE]
> 重要事項： 您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更新版本，才能完成本教學課程。


1. 建立新的專案 (**檔案** - &gt; **新專案**)，然後選取**ASP.NET Web 應用程式**以及.NET Framework 的範本從版本 4.5.2**新的專案** 對話方塊。
2. 從**新的 ASP.NET 專案**對話方塊中，選取**Web Form**範本。 保留為預設的驗證**個別使用者帳戶**。 然後，按一下**確定**建立新的專案。  
    ![[新增 ASP.NET 專案] 對話方塊](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. 啟用 Secure Sockets Layer (SSL) 專案。 請遵循中提供的步驟**啟用專案的 SSL**一節[開始使用 Web Form 教學課程系列](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)。
4. 在 Visual Studio 中開啟**Package Manager Console** (**工具** - &gt; **NuGet 封裝管理員** - &gt;**Package Manager Console**)，並輸入下列命令：  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>設定 SMS 和雙因素驗證

本教學課程中使用 Twilio，但您可以使用任何的 SMS 提供者。

1. 建立[Twilio](https://www.twilio.com/try-twilio)帳戶。
2. 從**儀表板**索引標籤上的 Twilio 帳戶，複製**Account SID**和**驗證權杖。** 您會將它們加入您的應用程式更新版本。
3. 從**數字**索引標籤上，複製您的 Twilio**電話號碼**以及。
4. Twilio**帳戶 SID**，**驗證權杖**並**電話號碼**可用應用程式。 為了簡單起見，您將會儲存這些值*web.config*檔案。 當您部署至 Azure 時，您可以儲存在安全的值**appSettings**網站上的一節設定 索引標籤。此外，當您新增的電話號碼，只使用數字。   
   請注意，您也可以加入 SendGrid 認證。 SendGrid 是電子郵件通知服務。 如需如何啟用 SendGrid 的詳細資訊，請參閱標題為本教學課程的 ' 攔截程序設定 SendGrid' 區段[建立安全 ASP.NET Web Forms 應用程式以使用者註冊、 電子郵件確認和密碼重設。](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > 安全性-機密資料絕不儲存在原始程式碼中。 在此範例中，帳戶和認證會儲存在**appSettings**一節*Web.config*檔案。 在 Azure 上，您可以安全地儲存這些值在**[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** 在 Azure 入口網站中的索引標籤。 如需相關資訊，請參閱 Rick Anderson 主題[最佳做法將密碼和其他機密資料部署到 ASP.NET 和 Azure](https://go.microsoft.com/fwlink/?LinkId=513141)。
5. 設定`SmsService`類別內*應用程式\_Start\IdentityConfig.cs*檔案進行下列變更中反白顯示的黃色： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. 新增下列`using`開頭的陳述式*IdentityConfig.cs*檔案： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. 更新*Account/Manage.aspx*檔案藉由移除以黃色反白顯示的行：  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. 在 `Page_Load`處理常式*Manage.aspx.cs*程式碼後置、 取消註解使它顯示為以黃色醒目提示的程式碼行如下： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. 中的程式碼後置*帳號*/*TwoFactorAuthenticationSignIn.aspx.cs*，更新`Page_Load`處理常式，加入下列程式碼以黃色反白顯示： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   藉由上述的程式碼變更，其中包含的驗證選項的 「 提供者 」 DropDownList 不會重設的第一個值。 這可讓使用者已成功選取所有選項，使用驗證時，不只是第一個。
10. 在 [**方案總管] 中**，以滑鼠右鍵按一下*Default.aspx* ，然後選取**設定為起始頁**。
11. 藉由測試您的應用程式，第一次建置應用程式 (**Ctrl**+**Shift**+**B**)，然後執行應用程式 (**F5**) 和請選取**註冊**來建立新的使用者帳戶或選取**登入**如果已註冊的使用者帳戶。
12. 之後 （如使用者） 登入，按一下 使用者識別碼 （電子郵件地址） 在導覽列中顯示**管理帳戶**頁面 (Manage.aspx)。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. 按一下 **新增**旁**電話號碼**上**管理帳戶**頁面。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. 新增位置 （如使用者） 您想要接收 SMS 訊息 （文字訊息），然後按一下 [電話號碼**送出**] 按鈕。   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    此時，應用程式將使用的認證*Web.config*連絡 Twilio。 SMS 訊息 （文字訊息） 將會傳送至使用者帳戶相關聯的電話。 您可以確認傳送的 Twilio 訊息時，已藉由檢視 Twilio 儀表板。
15. 在幾秒鐘之後，使用者帳戶相關聯的電話會收到包含驗證碼的文字訊息。 輸入驗證碼，然後按**送出**。  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>啟用雙因素驗證已註冊的使用者

此時，您已啟用雙因素驗證應用程式。 若要使用雙因素驗證使用者，他們可以只是變更其設定，使用 UI。 

1. 為您的應用程式的使用者您可以啟用雙因素驗證為您特定的帳戶上的使用者識別碼 （電子郵件別名） 即可在導覽列中顯示**管理帳戶**頁面。然後，按一下**啟用**啟用雙因素驗證帳戶的連結。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. 登出，然後再重新登入。 如果您已啟用電子郵件，您可以選取 SMS 或雙因素驗證的電子郵件。 如果您尚未啟用電子郵件，請參閱標題為本教學課程[建立安全 ASP.NET Web Forms 應用程式與使用者註冊、 電子郵件確認和密碼重設](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. 雙因素驗證 頁面會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 按一下**記住此瀏覽器**核取方塊將會豁免需要使用雙因素驗證來登入，當您核取方塊使用的瀏覽器和裝置。 只要惡意使用者無法存取您的裝置，啟用雙因素驗證，然後按一下**記住此瀏覽器**會為您提供便利的一個步驟密碼存取，同時仍然保留強式從非信任的裝置的所有存取的雙因素驗證保護。 您可以在任何您經常使用的私用裝置上執行這項操作。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

- [使用 SMS 的雙因素驗證和使用 ASP.NET Identity 的電子郵件](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [連結至 ASP.NET Identity 建議資源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure 網站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Form 教學課程系列-新增 OAuth 2.0 提供者](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Form 教學課程系列-啟用專案的 SSL](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [在 Facebook 中建立應用程式和應用程式連線至專案](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
