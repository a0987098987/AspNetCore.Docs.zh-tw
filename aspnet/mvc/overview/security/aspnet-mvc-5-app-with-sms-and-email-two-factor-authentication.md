---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: 使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何建置使用雙因素驗證的 ASP.NET MVC 5 web 應用程式。 您應該先完成建立安全的 ASP.NET MVC 5 web 應用程式...
ms.author: aspnetcontent
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4fd0091effcf2cc0517da91922981e49ef0eef5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803203"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="1c55b-104">使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式</span><span class="sxs-lookup"><span data-stu-id="1c55b-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="1c55b-105">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1c55b-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="1c55b-106">本教學課程會示範如何建置使用雙因素驗證的 ASP.NET MVC 5 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c55b-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="1c55b-107">您應該先完成[登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)後再繼續。</span><span class="sxs-lookup"><span data-stu-id="1c55b-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="1c55b-108">您可以下載完成的應用程式[此處](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="1c55b-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="1c55b-109">此下載包含可讓您測試而不需設定電子郵件或 SMS 提供者的電子郵件確認和 SMS 的偵錯協助程式。</span><span class="sxs-lookup"><span data-stu-id="1c55b-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="1c55b-110">本教學課程以寫入[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="1c55b-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="1c55b-111">建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="1c55b-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="1c55b-112">設定 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="1c55b-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="1c55b-113">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="1c55b-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="1c55b-114">其他資源</span><span class="sxs-lookup"><span data-stu-id="1c55b-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="1c55b-115">建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="1c55b-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="1c55b-116">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="1c55b-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="1c55b-117">安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="1c55b-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="1c55b-118">警告： 您應該先完成[登入、 電子郵件確認和密碼重設建立安全的 ASP.NET MVC 5 web 應用程式](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)後再繼續。</span><span class="sxs-lookup"><span data-stu-id="1c55b-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="1c55b-119">您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更新版本，才能完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="1c55b-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="1c55b-120">建立新的 ASP.NET Web 專案，然後選取 [MVC] 範本。</span><span class="sxs-lookup"><span data-stu-id="1c55b-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="1c55b-121">Web Form 也支援 ASP.NET 身分識別，因此您可以依照類似的步驟，在 web form 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c55b-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="1c55b-122">保留為預設的驗證**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="1c55b-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="1c55b-123">如果您想要裝載應用程式在 Azure 中的，核取方塊保持勾選。</span><span class="sxs-lookup"><span data-stu-id="1c55b-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="1c55b-124">稍後在本教學課程中，我們將會部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="1c55b-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="1c55b-125">您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="1c55b-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="1c55b-126">設定[專案，以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="1c55b-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="1c55b-127">設定 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="1c55b-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="1c55b-128">本教學課程提供使用 Twilio 或 ASPSMS 的指示，但您可以使用任何其他 SMS 提供者。</span><span class="sxs-lookup"><span data-stu-id="1c55b-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="1c55b-129">**使用 SMS 提供者建立使用者帳戶**</span><span class="sxs-lookup"><span data-stu-id="1c55b-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="1c55b-130">建立[Twilio](https://www.twilio.com/try-twilio)該[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)帳戶。</span><span class="sxs-lookup"><span data-stu-id="1c55b-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="1c55b-131">**安裝其他套件或新增服務參考**</span><span class="sxs-lookup"><span data-stu-id="1c55b-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="1c55b-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="1c55b-132">Twilio:</span></span>  
   <span data-ttu-id="1c55b-133">在套件管理員主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="1c55b-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="1c55b-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="1c55b-134">ASPSMS:</span></span>  
   <span data-ttu-id="1c55b-135">下列服務參考必須加入：</span><span class="sxs-lookup"><span data-stu-id="1c55b-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="1c55b-136">位址:</span><span class="sxs-lookup"><span data-stu-id="1c55b-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="1c55b-137">命名空間:</span><span class="sxs-lookup"><span data-stu-id="1c55b-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="1c55b-138">**找出 SMS 提供者使用者認證**</span><span class="sxs-lookup"><span data-stu-id="1c55b-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="1c55b-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="1c55b-139">Twilio:</span></span>  
   <span data-ttu-id="1c55b-140">從**儀表板**索引標籤上的 Twilio 帳戶，複製**Account SID**並**驗證權杖**。</span><span class="sxs-lookup"><span data-stu-id="1c55b-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="1c55b-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="1c55b-141">ASPSMS:</span></span>  
   <span data-ttu-id="1c55b-142">從您的帳戶設定，瀏覽至**Userkey**並將它複製以及您自行定義**密碼**。</span><span class="sxs-lookup"><span data-stu-id="1c55b-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="1c55b-143">我們稍後會儲存這些值*web.config*檔案內的索引鍵`"SMSAccountIdentification"`和`"SMSAccountPassword"`。</span><span class="sxs-lookup"><span data-stu-id="1c55b-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="1c55b-144">**指定寄件者識別碼 / 建立者**</span><span class="sxs-lookup"><span data-stu-id="1c55b-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="1c55b-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="1c55b-145">Twilio:</span></span>  
   <span data-ttu-id="1c55b-146">從**數字**索引標籤上，複製您的 Twilio 電話號碼。</span><span class="sxs-lookup"><span data-stu-id="1c55b-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="1c55b-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="1c55b-147">ASPSMS:</span></span>  
   <span data-ttu-id="1c55b-148">內**解除鎖定的建立者**功能表上，解除鎖定一或多個建立者，或選擇 英數字元的建立者 （不支援所有的網路）。</span><span class="sxs-lookup"><span data-stu-id="1c55b-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="1c55b-149">我們稍後會儲存此值*web.config*機碼內的檔案`"SMSAccountFrom"`。</span><span class="sxs-lookup"><span data-stu-id="1c55b-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="1c55b-150">**將 SMS 提供者認證傳送到應用程式**</span><span class="sxs-lookup"><span data-stu-id="1c55b-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="1c55b-151">認證和寄件者電話號碼可讓應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c55b-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="1c55b-152">為了簡單起見，我們將儲存在這些值*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c55b-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="1c55b-153">當我們部署至 Azure 時，我們可以儲存在安全值**應用程式設定**網站上的一節設定 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="1c55b-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="1c55b-154">安全性-機密資料絕不儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="1c55b-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="1c55b-155">上述保持簡單的範例程式碼中加入的帳戶和認證。</span><span class="sxs-lookup"><span data-stu-id="1c55b-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="1c55b-156">請參閱[最佳做法將密碼和其他機密資料部署到 ASP.NET 和 Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="1c55b-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="1c55b-157">**資料傳輸至 SMS 提供者的實作**</span><span class="sxs-lookup"><span data-stu-id="1c55b-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="1c55b-158">設定`SmsService`類別內*應用程式\_Start\IdentityConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c55b-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="1c55b-159">根據使用的 SMS 提供者啟用其中一個**Twilio**或**ASPSMS**區段：</span><span class="sxs-lookup"><span data-stu-id="1c55b-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="1c55b-160">更新*Views\Manage\Index.cshtml* Razor 檢視: (注意： 不只是移除現有的程式碼中的註解，請使用下列程式碼。)</span><span class="sxs-lookup"><span data-stu-id="1c55b-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="1c55b-161">請確認`EnableTwoFactorAuthentication`並`DisableTwoFactorAuthentication`中的動作方法`ManageController`有[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx)屬性：</span><span class="sxs-lookup"><span data-stu-id="1c55b-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="1c55b-162">執行應用程式，以及您先前註冊的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="1c55b-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="1c55b-163">按一下您的使用者識別碼，就會啟動`Index`中的動作方法`Manage`控制站。</span><span class="sxs-lookup"><span data-stu-id="1c55b-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="1c55b-164">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="1c55b-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="1c55b-165">`AddPhoneNumber`動作方法會顯示一個對話方塊，輸入可以接收簡訊的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="1c55b-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="1c55b-166">在幾秒鐘的時間，您會使用驗證碼的簡訊。</span><span class="sxs-lookup"><span data-stu-id="1c55b-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="1c55b-167">請輸入並按下**送出**。</span><span class="sxs-lookup"><span data-stu-id="1c55b-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="1c55b-168">[管理] 檢視會顯示已加入您的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="1c55b-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="1c55b-169">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="1c55b-169">Enable two-factor authentication</span></span>

<span data-ttu-id="1c55b-170">在範本產生應用程式中，您需要使用 UI 來啟用雙因素驗證 (2FA)。</span><span class="sxs-lookup"><span data-stu-id="1c55b-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="1c55b-171">若要啟用 2FA，請按一下您在導覽列中的使用者識別碼 （電子郵件別名）。</span><span class="sxs-lookup"><span data-stu-id="1c55b-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="1c55b-172">按一下 啟用 2FA。</span><span class="sxs-lookup"><span data-stu-id="1c55b-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="1c55b-173">登出，然後記錄回。</span><span class="sxs-lookup"><span data-stu-id="1c55b-173">Logout, then log back in.</span></span> <span data-ttu-id="1c55b-174">如果您已啟用電子郵件 (請參閱我[先前的教學課程](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md))，您可以選取 SMS 或 2FA 的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="1c55b-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="1c55b-175">確認字碼頁會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1c55b-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="1c55b-176">按一下**記住此瀏覽器**核取方塊將會豁免不必使用 2FA 登入，當您核取方塊使用的瀏覽器和裝置。</span><span class="sxs-lookup"><span data-stu-id="1c55b-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="1c55b-177">只要惡意使用者無法存取您的裝置，啟用 2FA，然後按一下**記住此瀏覽器**會為您提供便利的一個步驟密碼存取，同時保留所有存取的強式 2FA 保護從非信任的裝置。</span><span class="sxs-lookup"><span data-stu-id="1c55b-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="1c55b-178">您可以在任何您經常使用的私用裝置上執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="1c55b-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="1c55b-179">本教學課程提供啟用 2FA，新的 ASP.NET MVC 應用程式上的快速簡介。</span><span class="sxs-lookup"><span data-stu-id="1c55b-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="1c55b-180">我的教學課程[SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)會詳述範例背後的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1c55b-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="1c55b-181">其他資源</span><span class="sxs-lookup"><span data-stu-id="1c55b-181">Additional Resources</span></span>

- <span data-ttu-id="1c55b-182">[使用 SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)進入 雙因素驗證的詳細資訊</span><span class="sxs-lookup"><span data-stu-id="1c55b-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="1c55b-183">連結至 ASP.NET Identity 建議資源</span><span class="sxs-lookup"><span data-stu-id="1c55b-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="1c55b-184">[帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入 確認的密碼復原和帳戶的其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="1c55b-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="1c55b-185">[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 授權。</span><span class="sxs-lookup"><span data-stu-id="1c55b-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="1c55b-186">它也會示範如何新增額外的資料來識別資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c55b-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="1c55b-187">[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="1c55b-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="1c55b-188">本教學課程中新增 Azure 部署中，如何保護您的應用程式角色，如何使用成員資格 API 來新增使用者和角色，以及額外的安全性功能。</span><span class="sxs-lookup"><span data-stu-id="1c55b-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="1c55b-189">建立 Google app for OAuth 2 和應用程式連線至專案</span><span class="sxs-lookup"><span data-stu-id="1c55b-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="1c55b-190">在 Facebook 中建立應用程式和應用程式連線至專案</span><span class="sxs-lookup"><span data-stu-id="1c55b-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="1c55b-191">設定專案中的 SSL</span><span class="sxs-lookup"><span data-stu-id="1c55b-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
