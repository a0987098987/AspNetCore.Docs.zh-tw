---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: 使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式 |Microsoft 文件
author: Rick-Anderson
description: 本教學課程會示範如何建置使用雙因素驗證的 ASP.NET MVC 5 web 應用程式。 您應該先完成建立安全的 ASP.NET MVC 5 web 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873608"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="a2bf9-104">使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式</span><span class="sxs-lookup"><span data-stu-id="a2bf9-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="a2bf9-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a2bf9-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="a2bf9-106">本教學課程會示範如何建置使用雙因素驗證的 ASP.NET MVC 5 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="a2bf9-107">您應該先完成[建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)後再繼續。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="a2bf9-108">您可以下載完成的應用程式[這裡](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="a2bf9-109">此下載包含偵錯的協助程式，可讓您測試而不需設定電子郵件或 SMS 提供者的電子郵件確認與簡訊。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="a2bf9-110">本教學課程中所編寫的[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="a2bf9-111">建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="a2bf9-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="a2bf9-112">SMS 進行雙因素驗證設定</span><span class="sxs-lookup"><span data-stu-id="a2bf9-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="a2bf9-113">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="a2bf9-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="a2bf9-114">其他資源</span><span class="sxs-lookup"><span data-stu-id="a2bf9-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="a2bf9-115">建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="a2bf9-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="a2bf9-116">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="a2bf9-117">安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="a2bf9-118">警告： 您應該先完成[建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)後再繼續。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="a2bf9-119">您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本，才能完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="a2bf9-120">建立新的 ASP.NET Web 專案，然後選取 MVC 範本。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="a2bf9-121">Web Form 也支援 ASP.NET Identity，因此您可以依照類似的步驟，在 web form 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="a2bf9-122">保留預設驗證為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="a2bf9-123">如果您想要裝載應用程式在 Azure 中的，將保留勾選核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="a2bf9-124">稍後在本教學課程中，我們將部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="a2bf9-125">您可以[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="a2bf9-126">設定[專案以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="a2bf9-127">SMS 進行雙因素驗證設定</span><span class="sxs-lookup"><span data-stu-id="a2bf9-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="a2bf9-128">本教學課程提供使用 Twilio 或 ASPSMS 的指示，但是您可以使用任何其他 SMS 提供者。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="a2bf9-129">**建立使用者帳戶與 SMS 提供者**</span><span class="sxs-lookup"><span data-stu-id="a2bf9-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="a2bf9-130">建立[Twilio](https://www.twilio.com/try-twilio)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)帳戶。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="a2bf9-131">**安裝其他套件，或加入服務參考**</span><span class="sxs-lookup"><span data-stu-id="a2bf9-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="a2bf9-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-132">Twilio:</span></span>  
   <span data-ttu-id="a2bf9-133">在 Package Manager Console 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="a2bf9-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="a2bf9-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-134">ASPSMS:</span></span>  
   <span data-ttu-id="a2bf9-135">必須加入下列服務參考：</span><span class="sxs-lookup"><span data-stu-id="a2bf9-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="a2bf9-136">位址:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="a2bf9-137">命名空間:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="a2bf9-138">**找出 SMS 提供者使用者認證**</span><span class="sxs-lookup"><span data-stu-id="a2bf9-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="a2bf9-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-139">Twilio:</span></span>  
   <span data-ttu-id="a2bf9-140">從**儀表板**Twilio 帳戶、 複製的索引標籤**帳戶 SID**和**驗證語彙基元**。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="a2bf9-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-141">ASPSMS:</span></span>  
   <span data-ttu-id="a2bf9-142">從您的帳戶設定，瀏覽至**使用者金鑰**並將它複製以及您自行定義**密碼**。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="a2bf9-143">我們稍後會儲存這些值*web.config*檔案內的索引鍵`"SMSAccountIdentification"`和`"SMSAccountPassword"`。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="a2bf9-144">**指定寄件者識別碼 / 訂閱者**</span><span class="sxs-lookup"><span data-stu-id="a2bf9-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="a2bf9-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-145">Twilio:</span></span>  
   <span data-ttu-id="a2bf9-146">從**數字**索引標籤上，複製您的 Twilio 電話號碼。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="a2bf9-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="a2bf9-147">ASPSMS:</span></span>  
   <span data-ttu-id="a2bf9-148">內**解除鎖定發送者**功能表上，解除鎖定一或多個的建立者，或選擇英數字元的建立者 （所有網路不都支援）。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="a2bf9-149">我們稍後將儲存這個值*web.config*機碼內的檔案`"SMSAccountFrom"`。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="a2bf9-150">**將 SMS 提供者認證傳送到應用程式**</span><span class="sxs-lookup"><span data-stu-id="a2bf9-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="a2bf9-151">提供的認證和寄件者電話號碼給應用程式。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="a2bf9-152">為了簡單起見，我們將儲存在這些值*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="a2bf9-153">當我們將部署到 Azure 時，我們可以儲存在安全的值**應用程式設定**區段上的網站設定 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="a2bf9-154">安全性-永遠不會儲存敏感的資料來源上的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="a2bf9-155">為了簡化範例上述程式碼中加入的帳戶和認證。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="a2bf9-156">請參閱[ASP.NET 和 Azure 部署的密碼和其他機密資料的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="a2bf9-157">**資料傳輸至 SMS 提供者的實作**</span><span class="sxs-lookup"><span data-stu-id="a2bf9-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="a2bf9-158">設定`SmsService`類別*應用程式\_Start\IdentityConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="a2bf9-159">根據使用的 SMS 提供者啟用  **Twilio**或**ASPSMS** > 一節：</span><span class="sxs-lookup"><span data-stu-id="a2bf9-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="a2bf9-160">更新*Views\Manage\Index.cshtml* Razor 檢視: (注意： 不要只移除現有的程式碼中的註解，請使用下列程式碼。)</span><span class="sxs-lookup"><span data-stu-id="a2bf9-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="a2bf9-161">確認`EnableTwoFactorAuthentication`和`DisableTwoFactorAuthentication`中的動作方法`ManageController`有[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx)屬性：</span><span class="sxs-lookup"><span data-stu-id="a2bf9-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="a2bf9-162">執行應用程式，您先前註冊的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="a2bf9-163">按一下您的使用者識別碼，就會啟動`Index`中的動作方法`Manage`控制站。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="a2bf9-164">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="a2bf9-165">`AddPhoneNumber`動作方法會顯示一個對話方塊，輸入可以接收簡訊的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="a2bf9-166">在幾秒鐘的時間就會看到文字訊息，並將驗證碼。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="a2bf9-167">請輸入，並按下**送出**。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="a2bf9-168">管理檢視會顯示已加入您的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="a2bf9-169">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="a2bf9-169">Enable two-factor authentication</span></span>

<span data-ttu-id="a2bf9-170">在範本產生應用程式時，您需要使用 UI 啟用雙因素驗證 (2FA)。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="a2bf9-171">若要啟用 2FA，按一下您在瀏覽列中的使用者識別碼 （電子郵件別名）。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="a2bf9-172">按一下 啟用 2FA 上。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="a2bf9-173">登出，然後記錄回。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-173">Logout, then log back in.</span></span> <span data-ttu-id="a2bf9-174">如果您已啟用電子郵件 (請參閱我[上一個教學課程](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md))，您可以選取的 SMS 或 2FA 電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="a2bf9-175">請確認程式碼頁面會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="a2bf9-176">按一下**記住此瀏覽器**核取方塊將會豁免需要使用 2FA 使用瀏覽器及裝置您核取方塊時，登入。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="a2bf9-177">惡意使用者無法存取您的裝置，只要啟用 2FA，並按一下**記住此瀏覽器**會讓您方便一次密碼存取，同時保留所有存取的強式 2FA 保護從非信任的裝置。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="a2bf9-178">您可以經常使用的任何私用裝置上執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="a2bf9-179">本教學課程提供啟用 2FA 新的 ASP.NET MVC 應用程式上的快速簡介。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="a2bf9-180">我的教學課程[ASP.NET Identity 中使用 SMS 和電子郵件的雙因素驗證](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)範例背後的程式碼上便會進入 詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="a2bf9-181">其他資源</span><span class="sxs-lookup"><span data-stu-id="a2bf9-181">Additional Resources</span></span>

- <span data-ttu-id="a2bf9-182">[SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)進入雙因素驗證的詳細資料</span><span class="sxs-lookup"><span data-stu-id="a2bf9-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="a2bf9-183">ASP.NET Identity 的連結，建議使用的資源</span><span class="sxs-lookup"><span data-stu-id="a2bf9-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="a2bf9-184">[帳戶確認和密碼復原與 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入確認的密碼復原和帳戶的其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="a2bf9-185">[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 」 授權。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="a2bf9-186">它也會示範如何加入識別資料庫中的其他資料。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="a2bf9-187">[將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 應用程式部署到 Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="a2bf9-188">本教學課程將加入 Azure 部署時，如何保護您的應用程式角色、 如何使用成員資格應用程式開發介面來新增使用者和角色，以及其他安全性功能。</span><span class="sxs-lookup"><span data-stu-id="a2bf9-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="a2bf9-189">OAuth 2 建立 Google 應用程式和應用程式連接至專案</span><span class="sxs-lookup"><span data-stu-id="a2bf9-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="a2bf9-190">在 Facebook 中建立應用程式和應用程式連接至專案</span><span class="sxs-lookup"><span data-stu-id="a2bf9-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="a2bf9-191">在專案中的 SSL 設定</span><span class="sxs-lookup"><span data-stu-id="a2bf9-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
