---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: 使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何建置使用雙因素驗證的 ASP.NET MVC 5 web 應用程式。 您應該先完成建立安全的 ASP.NET MVC 5 web 應用程式...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 97b73c5ae18a528d33b44a4de4afc434ac46af48
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577596"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 本教學課程會示範如何建置使用雙因素驗證的 ASP.NET MVC 5 web 應用程式。 您應該先完成[登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)後再繼續。 您可以下載完成的應用程式[此處](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 此下載包含可讓您測試而不需設定電子郵件或 SMS 提供者的電子郵件確認和 SMS 的偵錯協助程式。
> 
> 本教學課程以寫入[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


- [建立 ASP.NET MVC 應用程式](#createMvc)
- [設定 SMS 的雙因素驗證](#SMS)
- [啟用雙因素驗證](#enable2)
- [其他資源](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>建立 ASP.NET MVC 應用程式

開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。

> [!NOTE]
> 警告： 您應該先完成[登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)後再繼續。 您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更新版本，才能完成本教學課程。


1. 建立新的 ASP.NET Web 專案，然後選取 [MVC] 範本。 Web Form 也支援 ASP.NET 身分識別，因此您可以依照類似的步驟，在 web form 應用程式。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. 保留為預設的驗證**個別使用者帳戶**。 如果您想要裝載應用程式在 Azure 中的，核取方塊保持勾選。 稍後在本教學課程中，我們將會部署到 Azure。 您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。
3. 設定[專案，以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   位址:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   命名空間:  
    `ASPSMSX2`
3. **找出 SMS 提供者使用者認證**  
  
   Twilio:  
   從**儀表板**索引標籤上的 Twilio 帳戶，複製**Account SID**並**驗證權杖**。  
  
   ASPSMS:  
   從您的帳戶設定，瀏覽至**Userkey**並將它複製以及您自行定義**密碼**。  
  
   我們稍後會儲存這些值*web.config*檔案內的索引鍵`"SMSAccountIdentification"`和`"SMSAccountPassword"`。
4. **指定寄件者識別碼 / 建立者**  
  
   Twilio:  
   從**數字**索引標籤上，複製您的 Twilio 電話號碼。  
  
   ASPSMS:  
   內**解除鎖定的建立者**功能表上，解除鎖定一或多個建立者，或選擇 英數字元的建立者 （不支援所有的網路）。  
  
   我們稍後會儲存此值*web.config*機碼內的檔案`"SMSAccountFrom"`。
5. **將 SMS 提供者認證傳送到應用程式**  
  
   認證和寄件者電話號碼可讓應用程式。 為了簡單起見，我們將儲存在這些值*web.config*檔案。 當我們部署至 Azure 時，我們可以儲存在安全值**應用程式設定**網站上的一節設定 索引標籤。 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > 安全性-機密資料絕不儲存在原始程式碼中。 上述保持簡單的範例程式碼中加入的帳戶和認證。 請參閱[最佳做法將密碼和其他機密資料部署到 ASP.NET 和 Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
6. **資料傳輸至 SMS 提供者的實作**  
  
   設定`SmsService`類別內*應用程式\_Start\IdentityConfig.cs*檔案。  
  
   根據使用的 SMS 提供者啟用其中一個**Twilio**或**ASPSMS**區段： 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. 更新*Views\Manage\Index.cshtml* Razor 檢視: (注意： 不只是移除現有的程式碼中的註解，請使用下列程式碼。)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. 請確認`EnableTwoFactorAuthentication`並`DisableTwoFactorAuthentication`中的動作方法`ManageController`有[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx)屬性：  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. 執行應用程式，以及您先前註冊的帳戶登入。
10. 按一下您的使用者識別碼，就會啟動`Index`中的動作方法`Manage`控制站。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. 按一下 新增。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber`動作方法會顯示一個對話方塊，輸入可以接收簡訊的電話號碼。

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. 在幾秒鐘的時間，您會使用驗證碼的簡訊。 請輸入並按下**送出**。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. [管理] 檢視會顯示已加入您的電話號碼。

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>啟用雙因素驗證

在範本產生應用程式中，您需要使用 UI 來啟用雙因素驗證 (2FA)。 若要啟用 2FA，請按一下您在導覽列中的使用者識別碼 （電子郵件別名）。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

按一下 啟用 2FA。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

登出，然後記錄回。 如果您已啟用電子郵件 (請參閱我[先前的教學課程](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md))，您可以選取 SMS 或 2FA 的電子郵件。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

確認字碼頁會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

按一下**記住此瀏覽器**核取方塊將會豁免不必使用 2FA 登入，當您核取方塊使用的瀏覽器和裝置。 只要惡意使用者無法存取您的裝置，啟用 2FA，然後按一下**記住此瀏覽器**會為您提供便利的一個步驟密碼存取，同時保留所有存取的強式 2FA 保護從非信任的裝置。 您可以在任何您經常使用的私用裝置上執行這項操作。

本教學課程提供啟用 2FA，新的 ASP.NET MVC 應用程式上的快速簡介。 我的教學課程[SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)會詳述範例背後的程式碼。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

- [使用 SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)進入 雙因素驗證的詳細資訊
- [連結至 ASP.NET Identity 建議資源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入 確認的密碼復原和帳戶的其他詳細資料。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 授權。 它也會示範如何新增額外的資料來識別資料庫。
- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 本教學課程中新增 Azure 部署中，如何保護您的應用程式角色，如何使用成員資格 API 來新增使用者和角色，以及額外的安全性功能。
- [建立 Google app for OAuth 2 和應用程式連線至專案](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [在 Facebook 中建立應用程式和應用程式連線至專案](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [設定專案中的 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
