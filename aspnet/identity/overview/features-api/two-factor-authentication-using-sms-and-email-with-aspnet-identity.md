---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證 |Microsoft 文件
author: HaoK
description: 本教學課程將說明如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。 由 Rick anderson 發表撰寫本文時 ( @RickAndMSFT )、 Pr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876133"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="dad0a-104">SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="dad0a-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="dad0a-105">由[Hao 是一隻](https://github.com/HaoK)， [Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="dad0a-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="dad0a-106">本教學課程將說明如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。</span><span class="sxs-lookup"><span data-stu-id="dad0a-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="dad0a-107">由 Rick anderson 發表撰寫本文時 ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))，Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Hao 是一隻和 Suhas Joshi。</span><span class="sxs-lookup"><span data-stu-id="dad0a-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="dad0a-108">NuGet 範例主要 Hao 是一隻寫入。</span><span class="sxs-lookup"><span data-stu-id="dad0a-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="dad0a-109">本主題涵蓋下列資訊：</span><span class="sxs-lookup"><span data-stu-id="dad0a-109">This topic covers the following:</span></span>

- [<span data-ttu-id="dad0a-110">建立身分識別範例</span><span class="sxs-lookup"><span data-stu-id="dad0a-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="dad0a-111">SMS 進行雙因素驗證設定</span><span class="sxs-lookup"><span data-stu-id="dad0a-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="dad0a-112">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="dad0a-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="dad0a-113">如何註冊雙因素驗證提供者</span><span class="sxs-lookup"><span data-stu-id="dad0a-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="dad0a-114">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="dad0a-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="dad0a-115">帳戶鎖定的暴力攻擊</span><span class="sxs-lookup"><span data-stu-id="dad0a-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="dad0a-116">建立身分識別範例</span><span class="sxs-lookup"><span data-stu-id="dad0a-116">Building the Identity sample</span></span>

<span data-ttu-id="dad0a-117">在本節中，您將使用 NuGet，若要下載的範例，我們將使用。</span><span class="sxs-lookup"><span data-stu-id="dad0a-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="dad0a-118">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="dad0a-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="dad0a-119">安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="dad0a-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="dad0a-120">警告： 您必須安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="dad0a-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="dad0a-121">建立新***空***ASP.NET Web 專案。</span><span class="sxs-lookup"><span data-stu-id="dad0a-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="dad0a-122">在 Package Manager Console 中，輸入下列下列命令：</span><span class="sxs-lookup"><span data-stu-id="dad0a-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="dad0a-123">在此教學課程中，我們將使用[SendGrid](http://sendgrid.com/)傳送電子郵件和[Twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms 文字行動的。</span><span class="sxs-lookup"><span data-stu-id="dad0a-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="dad0a-124">`Identity.Samples`套件會安裝我們即將使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="dad0a-125">設定[專案以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="dad0a-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="dad0a-126">*選擇性*： 依照我[電子郵件確認教學課程](account-confirmation-and-password-recovery-with-aspnet-identity.md)連結 SendGrid 然後執行應用程式並註冊電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="dad0a-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="dad0a-127">* 選擇性: * 移除此範例的示範電子郵件連結確認程式碼 (`ViewBag.Link`帳戶控制器中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="dad0a-128">請參閱`DisplayEmail`和`ForgotPasswordConfirmation`動作方法及 razor 檢視)。</span><span class="sxs-lookup"><span data-stu-id="dad0a-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="dad0a-129"><em>選擇性: * 移除`ViewBag.Status`程式碼從管理和帳戶控制器和 *Views\Account\VerifyCode.cshtml</em>和<em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="dad0a-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="dad0a-130">或者，您可以保留`ViewBag.Status`顯示畫面來測試此應用程式在本機，而不必相連結，並傳送電子郵件和簡訊的運作方式。</span><span class="sxs-lookup"><span data-stu-id="dad0a-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="dad0a-131">警告： 如果您變更任何安全性設定，在此範例中，實際執行應用程式必須經過所做的變更會明確呼叫的安全性稽核。</span><span class="sxs-lookup"><span data-stu-id="dad0a-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="dad0a-132">SMS 進行雙因素驗證設定</span><span class="sxs-lookup"><span data-stu-id="dad0a-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="dad0a-133">本教學課程提供使用 Twilio 或 ASPSMS 的指示，但是您可以使用任何其他 SMS 提供者。</span><span class="sxs-lookup"><span data-stu-id="dad0a-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="dad0a-134">**建立使用者帳戶與 SMS 提供者**</span><span class="sxs-lookup"><span data-stu-id="dad0a-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="dad0a-135">建立[Twilio](https://www.twilio.com/try-twilio)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)帳戶。</span><span class="sxs-lookup"><span data-stu-id="dad0a-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="dad0a-136">**安裝其他套件，或加入服務參考**</span><span class="sxs-lookup"><span data-stu-id="dad0a-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="dad0a-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="dad0a-137">Twilio:</span></span>  
   <span data-ttu-id="dad0a-138">在 Package Manager Console 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="dad0a-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="dad0a-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="dad0a-139">ASPSMS:</span></span>  
   <span data-ttu-id="dad0a-140">必須加入下列服務參考：</span><span class="sxs-lookup"><span data-stu-id="dad0a-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="dad0a-141">位址:</span><span class="sxs-lookup"><span data-stu-id="dad0a-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="dad0a-142">命名空間:</span><span class="sxs-lookup"><span data-stu-id="dad0a-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="dad0a-143">**找出 SMS 提供者使用者認證**</span><span class="sxs-lookup"><span data-stu-id="dad0a-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="dad0a-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="dad0a-144">Twilio:</span></span>  
   <span data-ttu-id="dad0a-145">從**儀表板**Twilio 帳戶、 複製的索引標籤**帳戶 SID**和**驗證語彙基元**。</span><span class="sxs-lookup"><span data-stu-id="dad0a-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="dad0a-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="dad0a-146">ASPSMS:</span></span>  
   <span data-ttu-id="dad0a-147">從您的帳戶設定，瀏覽至**使用者金鑰**並將它複製以及您自行定義**密碼**。</span><span class="sxs-lookup"><span data-stu-id="dad0a-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="dad0a-148">我們將稍後將這些值儲存在變數`SMSAccountIdentification`和`SMSAccountPassword`。</span><span class="sxs-lookup"><span data-stu-id="dad0a-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="dad0a-149">**指定寄件者識別碼 / 訂閱者**</span><span class="sxs-lookup"><span data-stu-id="dad0a-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="dad0a-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="dad0a-150">Twilio:</span></span>  
   <span data-ttu-id="dad0a-151">從**數字**索引標籤上，複製您的 Twilio 電話號碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="dad0a-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="dad0a-152">ASPSMS:</span></span>  
   <span data-ttu-id="dad0a-153">內**解除鎖定發送者**功能表上，解除鎖定一或多個的建立者，或選擇英數字元的建立者 （所有網路不都支援）。</span><span class="sxs-lookup"><span data-stu-id="dad0a-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="dad0a-154">我們稍後會將這個值儲存在變數中`SMSAccountFrom`。</span><span class="sxs-lookup"><span data-stu-id="dad0a-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="dad0a-155">**將 SMS 提供者認證傳送到應用程式**</span><span class="sxs-lookup"><span data-stu-id="dad0a-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="dad0a-156">提供的認證和寄件者電話號碼給應用程式：</span><span class="sxs-lookup"><span data-stu-id="dad0a-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="dad0a-157">安全性-永遠不會儲存敏感的資料來源上的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="dad0a-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="dad0a-158">為了簡化範例上述程式碼中加入的帳戶和認證。</span><span class="sxs-lookup"><span data-stu-id="dad0a-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="dad0a-159">請參閱 Jon Atten [ASP.NET MVC： 保留私用設定不足的原始檔控制](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。</span><span class="sxs-lookup"><span data-stu-id="dad0a-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="dad0a-160">**資料傳輸至 SMS 提供者的實作**</span><span class="sxs-lookup"><span data-stu-id="dad0a-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="dad0a-161">設定`SmsService`類別*應用程式\_Start\IdentityConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="dad0a-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="dad0a-162">根據使用的 SMS 提供者啟用  **Twilio**或**ASPSMS** > 一節：</span><span class="sxs-lookup"><span data-stu-id="dad0a-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="dad0a-163">執行應用程式，您先前註冊的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="dad0a-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="dad0a-164">按一下您的使用者識別碼，就會啟動`Index`中的動作方法`Manage`控制站。</span><span class="sxs-lookup"><span data-stu-id="dad0a-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="dad0a-165">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="dad0a-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="dad0a-166">在幾秒鐘的時間就會看到文字訊息，並將驗證碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="dad0a-167">請輸入，並按下**送出**。</span><span class="sxs-lookup"><span data-stu-id="dad0a-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="dad0a-168">管理檢視會顯示已加入您的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="dad0a-169">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="dad0a-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="dad0a-170">`Index`中的動作方法`Manage`控制器設定根據您的上一個動作的狀態訊息，並提供連結變更您的本機密碼，或是新增本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="dad0a-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="dad0a-171">`Index`方法也會顯示狀態，或您 2FA 電話號碼、 外部登入、 2FA 啟用，並請記得 2FA 方法，針對這個瀏覽器 （稍後說明）。</span><span class="sxs-lookup"><span data-stu-id="dad0a-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="dad0a-172">按一下您的使用者識別碼 （電子郵件），在標題列中不會將訊息傳遞。</span><span class="sxs-lookup"><span data-stu-id="dad0a-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="dad0a-173">按一下**電話號碼： 移除**連結傳遞`Message=RemovePhoneSuccess`做為查詢字串。</span><span class="sxs-lookup"><span data-stu-id="dad0a-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="dad0a-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="dad0a-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="dad0a-175">`AddPhoneNumber`動作方法會顯示一個對話方塊，輸入可以接收簡訊的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="dad0a-176">按一下**傳送驗證碼**按鈕會張貼到 HTTP POST 的電話號碼`AddPhoneNumber`動作方法。</span><span class="sxs-lookup"><span data-stu-id="dad0a-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="dad0a-177">`GenerateChangePhoneNumberTokenAsync`方法會產生此值會設定在簡訊中的安全性權杖。</span><span class="sxs-lookup"><span data-stu-id="dad0a-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="dad0a-178">如果 SMS 服務已設定，權杖會以字串形式傳送&quot;您安全性的程式碼&lt;語彙基元&gt;&quot;。</span><span class="sxs-lookup"><span data-stu-id="dad0a-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="dad0a-179">`SmsService.SendAsync`以非同步方式呼叫方法，則應用程式會重新導向至`VerifyPhoneNumber`動作方法 （這會顯示下列對話方塊），您可以在其中輸入驗證碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="dad0a-180">一旦您輸入程式碼，並按一下 [提交]，程式碼張貼至 HTTP POST`VerifyPhoneNumber`動作方法。</span><span class="sxs-lookup"><span data-stu-id="dad0a-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="dad0a-181">`ChangePhoneNumberAsync`方法會檢查已張貼的安全性驗證碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="dad0a-182">程式碼是否正確，會新增電話號碼`PhoneNumber`欄位`AspNetUsers`資料表。</span><span class="sxs-lookup"><span data-stu-id="dad0a-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="dad0a-183">如果成功，該呼叫`SignInAsync`方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="dad0a-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="dad0a-184">`isPersistent`參數會設定是否驗證工作階段會保存在多個要求。</span><span class="sxs-lookup"><span data-stu-id="dad0a-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="dad0a-185">當您變更您的安全性設定檔時，會產生新的安全性戳記，並儲存在`SecurityStamp`欄位*AspNetUsers*資料表。</span><span class="sxs-lookup"><span data-stu-id="dad0a-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="dad0a-186">請注意，`SecurityStamp`欄位是不同的安全性 cookie。</span><span class="sxs-lookup"><span data-stu-id="dad0a-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="dad0a-187">安全性 cookie 不會儲存在`AspNetUsers`資料表 （或識別資料庫中的其他地方）。</span><span class="sxs-lookup"><span data-stu-id="dad0a-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="dad0a-188">使用自我簽署的安全性 cookie 語彙基元[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)並建立`UserId, SecurityStamp`和到期時間資訊。</span><span class="sxs-lookup"><span data-stu-id="dad0a-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="dad0a-189">Cookie 中介軟體會檢查每個要求的 cookie。</span><span class="sxs-lookup"><span data-stu-id="dad0a-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="dad0a-190">`SecurityStampValidator`方法中的`Startup`類別叫用的資料庫，並定期檢查安全性戳記與所指定`validateInterval`。</span><span class="sxs-lookup"><span data-stu-id="dad0a-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="dad0a-191">這只會每隔 30 分鐘 （在我們的範例），除非您變更您的安全性設定檔。</span><span class="sxs-lookup"><span data-stu-id="dad0a-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="dad0a-192">在 30 分鐘的間隔選擇用來存取資料庫的次數降到最低。</span><span class="sxs-lookup"><span data-stu-id="dad0a-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="dad0a-193">`SignInAsync`方法需要被呼叫時的安全性設定檔會進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="dad0a-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="dad0a-194">資料庫的安全性設定檔變更時，會更新`SecurityStamp`欄位，並沒有呼叫`SignInAsync`您永久登入方法*只*直到下一次 OWIN 管線叫用的資料庫 ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="dad0a-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="dad0a-195">您可以藉由變更測試`SignInAsync`方法來立即傳回，並設定 cookie`validateInterval`屬性從 30 分鐘的時間為 5 秒：</span><span class="sxs-lookup"><span data-stu-id="dad0a-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="dad0a-196">與上述的程式碼變更，您可以變更您的安全性設定檔 (例如，藉由變更的狀態**兩個因素啟用**)，而且您會在 5 秒記錄時`SecurityStampValidator.OnValidateIdentity`方法失敗。</span><span class="sxs-lookup"><span data-stu-id="dad0a-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="dad0a-197">移除中的返回行`SignInAsync`方法，請變更其他安全性設定檔，然後您會登出。`SignInAsync`方法會產生新的安全性 cookie。</span><span class="sxs-lookup"><span data-stu-id="dad0a-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="dad0a-198">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="dad0a-198">Enable two-factor authentication</span></span>

<span data-ttu-id="dad0a-199">範例應用程式，您需要使用 UI 啟用雙因素驗證 (2FA)。</span><span class="sxs-lookup"><span data-stu-id="dad0a-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="dad0a-200">若要啟用 2FA，按一下您在瀏覽列中的使用者識別碼 （電子郵件別名）。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="dad0a-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="dad0a-201">按一下 啟用 2FA 上。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="dad0a-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="dad0a-202">登出，然後重新登入。</span><span class="sxs-lookup"><span data-stu-id="dad0a-202">Log out, then log back in.</span></span> <span data-ttu-id="dad0a-203">如果您已啟用電子郵件 (請參閱我[上一個教學課程](account-confirmation-and-password-recovery-with-aspnet-identity.md))，您可以選取的 SMS 或 2FA 電子郵件。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="dad0a-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="dad0a-204">請確認程式碼頁面會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="dad0a-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="dad0a-205">按一下**記住此瀏覽器**核取方塊將會豁免需要使用 2FA 具有該電腦與瀏覽器登入。</span><span class="sxs-lookup"><span data-stu-id="dad0a-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="dad0a-206">啟用 2FA，並按一下**記住此瀏覽器**會為您提供強式 2FA 保護惡意使用者嘗試存取您的帳戶，只要它們不能存取您的電腦。</span><span class="sxs-lookup"><span data-stu-id="dad0a-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="dad0a-207">您可以經常使用的任何私用電腦上執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="dad0a-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="dad0a-208">藉由設定**記住此瀏覽器**、 您沒有經常使用的電腦中獲得 2FA 提高的安全性，且您方便性，不論在不需透過 2FA 您自己的電腦上。</span><span class="sxs-lookup"><span data-stu-id="dad0a-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="dad0a-209">如何註冊雙因素驗證提供者</span><span class="sxs-lookup"><span data-stu-id="dad0a-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="dad0a-210">當您建立新的 MVC 專案， *IdentityConfig.cs*檔案包含下列程式碼，以註冊雙因素驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="dad0a-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="dad0a-211">新增 2FA 的電話號碼</span><span class="sxs-lookup"><span data-stu-id="dad0a-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="dad0a-212">`AddPhoneNumber`中的動作方法`Manage`控制站會產生安全性權杖，並傳送其電話號碼您所提供。</span><span class="sxs-lookup"><span data-stu-id="dad0a-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="dad0a-213">傳送之後語彙基元，它將重新導向至`VerifyPhoneNumber`動作方法，您可以在其中輸入要註冊 2FA SMS 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="dad0a-214">除非您已驗證的電話號碼，不會使用 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="dad0a-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="dad0a-215">啟用 2FA</span><span class="sxs-lookup"><span data-stu-id="dad0a-215">Enabling 2FA</span></span>

<span data-ttu-id="dad0a-216">`EnableTFA`動作方法可讓 2FA:</span><span class="sxs-lookup"><span data-stu-id="dad0a-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="dad0a-217">請注意`SignInAsync`必須先呼叫，因為啟用 2FA 已變更的安全性設定檔。</span><span class="sxs-lookup"><span data-stu-id="dad0a-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="dad0a-218">啟用 2FA 時，使用者必須使用 2FA 登入，使用 2FA 方法完成註冊 （SMS 和電子郵件範例中的）。</span><span class="sxs-lookup"><span data-stu-id="dad0a-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="dad0a-219">您可以新增更多的 2FA 提供者，例如 QR 程式碼產生器，或者您可以撰寫您自己 (請參閱[使用 Google 驗證器與 ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。</span><span class="sxs-lookup"><span data-stu-id="dad0a-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="dad0a-220">使用產生 2FA 代碼[以時間為基礎的單次密碼演算法](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)和代碼六分鐘內才有效。</span><span class="sxs-lookup"><span data-stu-id="dad0a-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="dad0a-221">如果您需要多個六分鐘的時間來輸入程式碼，您會收到無效的程式碼錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="dad0a-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="dad0a-222">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="dad0a-222">Combine social and local login accounts</span></span>

<span data-ttu-id="dad0a-223">您可以結合本機和社交帳戶按一下電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="dad0a-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="dad0a-224">依照以下順序&quot; RickAndMSFT@gmail.com &quot;首先會建立為本機登入，但您可以建立帳戶與社交的記錄檔中第一個，然後加入本機的登入。</span><span class="sxs-lookup"><span data-stu-id="dad0a-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="dad0a-225">按一下**管理**連結。</span><span class="sxs-lookup"><span data-stu-id="dad0a-225">Click on the **Manage** link.</span></span> <span data-ttu-id="dad0a-226">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="dad0a-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="dad0a-227">按一下此連結服務中的另一個記錄檔，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="dad0a-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="dad0a-228">已結合兩個帳戶，您將無法與任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="dad0a-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="dad0a-229">您可能會想讓使用者加入本機帳戶，以防使用者社交的記錄檔中驗證服務已關閉，或可能比較他們已經失去其社交帳戶的存取。</span><span class="sxs-lookup"><span data-stu-id="dad0a-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="dad0a-230">在下圖中，Tom 是社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。</span><span class="sxs-lookup"><span data-stu-id="dad0a-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="dad0a-231">按一下**挑選密碼**可讓您在上新增本機記錄檔相同的帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="dad0a-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="dad0a-232">帳戶鎖定的暴力攻擊</span><span class="sxs-lookup"><span data-stu-id="dad0a-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="dad0a-233">您可以啟用使用者鎖定，從字典攻擊的應用程式上保護的帳戶。</span><span class="sxs-lookup"><span data-stu-id="dad0a-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="dad0a-234">下列程式碼中`ApplicationUserManager Create`方法會設定鎖定：</span><span class="sxs-lookup"><span data-stu-id="dad0a-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="dad0a-235">上述程式碼可讓進行雙因素驗證的鎖定。</span><span class="sxs-lookup"><span data-stu-id="dad0a-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="dad0a-236">雖然您可以藉由變更啟用鎖定的登入`shouldLockout`為 true 的`Login`帳戶控制器方法，建議您不啟用鎖定的登入，因為它會使帳戶容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)登入攻擊。</span><span class="sxs-lookup"><span data-stu-id="dad0a-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="dad0a-237">範例程式碼中，鎖定已停用系統管理員帳戶中建立`ApplicationDbInitializer Seed`方法：</span><span class="sxs-lookup"><span data-stu-id="dad0a-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="dad0a-238">需要使用者擁有的已驗證的電子郵件帳戶</span><span class="sxs-lookup"><span data-stu-id="dad0a-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="dad0a-239">下列程式碼需要使用者擁有的已驗證的電子郵件帳戶，他們可以登入前：</span><span class="sxs-lookup"><span data-stu-id="dad0a-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="dad0a-240">SignInManager 如何檢查 2FA 需求</span><span class="sxs-lookup"><span data-stu-id="dad0a-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="dad0a-241">在本機記錄檔和中檢查是否已啟用 2FA 社交記錄檔。</span><span class="sxs-lookup"><span data-stu-id="dad0a-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="dad0a-242">如果已啟用 2FA，`SignInManager`登入方法會傳回`SignInStatus.RequiresVerification`，使用者將會重新導向至`SendCode`動作方法，您將必須輸入要完成的記錄順序中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="dad0a-243">如果使用者具有 RememberMe 設定在使用者本機 cookie，`SignInManager`會傳回`SignInStatus.Success`，而且它們沒有經過 2FA。</span><span class="sxs-lookup"><span data-stu-id="dad0a-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="dad0a-244">下列程式碼會示範`SendCode`動作方法。</span><span class="sxs-lookup"><span data-stu-id="dad0a-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="dad0a-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)建立與所有使用者啟用 2FA 方法。</span><span class="sxs-lookup"><span data-stu-id="dad0a-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="dad0a-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)傳遞至[DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper，可讓使用者選取的 2FA 方法 （通常是電子郵件和 SMS）。</span><span class="sxs-lookup"><span data-stu-id="dad0a-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="dad0a-247">一旦使用者張貼 2FA 方法`HTTP POST SendCode`呼叫動作方法時，`SignInManager`傳送 2FA 程式碼中，且使用者已重新導向到`VerifyCode`動作方法可以在其中輸入要完成的記錄檔中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dad0a-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="dad0a-248">2FA 鎖定</span><span class="sxs-lookup"><span data-stu-id="dad0a-248">2FA Lockout</span></span>

<span data-ttu-id="dad0a-249">雖然您可以設定帳戶鎖定的登入的密碼嘗試失敗，該方法可讓您的登入容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)鎖定。</span><span class="sxs-lookup"><span data-stu-id="dad0a-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="dad0a-250">我們建議您只能搭配 2FA 使用帳戶鎖定。</span><span class="sxs-lookup"><span data-stu-id="dad0a-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="dad0a-251">當`ApplicationUserManager`已建立，此範例程式碼設定 2FA 鎖定和`MaxFailedAccessAttemptsBeforeLockout`五個。</span><span class="sxs-lookup"><span data-stu-id="dad0a-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="dad0a-252">一旦使用者登入 （透過本機帳戶或社交帳戶），儲存每個在 2FA 的嘗試失敗，而且使用者如果嘗試次數上限為止，則鎖定五分鐘的時間 (您可以設定時間內無鎖定`DefaultAccountLockoutTimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="dad0a-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="dad0a-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="dad0a-253">Additional Resources</span></span>

- <span data-ttu-id="dad0a-254">[ASP.NET Identity 建議資源](../getting-started/aspnet-identity-recommended-resources.md)因此連結的身分識別部落格、 視訊、 教學課程和優越的完整清單。</span><span class="sxs-lookup"><span data-stu-id="dad0a-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="dad0a-255">[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)也示範如何加入使用者資料表中的設定檔資訊。</span><span class="sxs-lookup"><span data-stu-id="dad0a-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="dad0a-256">[ASP.NET MVC 和身分識別 2.0： 了解基本概念](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)由 John Atten。</span><span class="sxs-lookup"><span data-stu-id="dad0a-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="dad0a-257">帳戶確認和 ASP.NET 識別的密碼復原</span><span class="sxs-lookup"><span data-stu-id="dad0a-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="dad0a-258">ASP.NET Identity 簡介</span><span class="sxs-lookup"><span data-stu-id="dad0a-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="dad0a-259">[宣告 ASP.NET Identity 2.0.0 的 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)由 Pranav Rastogi。</span><span class="sxs-lookup"><span data-stu-id="dad0a-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="dad0a-260">[ASP.NET Identity 2.0： 設定帳戶驗證與授權雙因素](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)由 John Atten。</span><span class="sxs-lookup"><span data-stu-id="dad0a-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
