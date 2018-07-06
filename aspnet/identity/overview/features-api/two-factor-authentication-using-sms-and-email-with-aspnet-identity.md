---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: 使用 SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證 |Microsoft Docs
author: HaoK
description: 本教學課程會示範如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。 撰寫本文時已由 Rick Anderson ( @RickAndMSFT )、 Pr....
ms.author: aspnetcontent
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c1f4bd4a3f65d4b7fccd86214fd0ba45c891e390
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807618"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="fa2a3-104">使用 SMS 和電子郵件使用 ASP.NET Identity 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="fa2a3-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="fa2a3-105">藉由[Hao 是一隻](https://github.com/HaoK)，[請參閱 Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="fa2a3-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="fa2a3-106">本教學課程會示範如何設定雙因素驗證 (2FA) 使用 SMS 和電子郵件。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="fa2a3-107">撰寫本文時已由 Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))，請參閱 Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Hao 是一隻和 Suhas Joshi。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="fa2a3-108">NuGet 範例已寫入主要是由 Hao 是一隻。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="fa2a3-109">本主題涵蓋下列資訊：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-109">This topic covers the following:</span></span>

- [<span data-ttu-id="fa2a3-110">建立身分識別範例</span><span class="sxs-lookup"><span data-stu-id="fa2a3-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="fa2a3-111">設定 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="fa2a3-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="fa2a3-112">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="fa2a3-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="fa2a3-113">如何註冊雙因素驗證提供者</span><span class="sxs-lookup"><span data-stu-id="fa2a3-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="fa2a3-114">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="fa2a3-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="fa2a3-115">帳戶鎖定的暴力密碼破解攻擊</span><span class="sxs-lookup"><span data-stu-id="fa2a3-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="fa2a3-116">建立身分識別範例</span><span class="sxs-lookup"><span data-stu-id="fa2a3-116">Building the Identity sample</span></span>

<span data-ttu-id="fa2a3-117">在本節中，您會使用 NuGet 來下載的範例，我們將使用。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="fa2a3-118">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="fa2a3-119">安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="fa2a3-120">警告： 您必須安裝 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="fa2a3-121">建立新***空***ASP.NET Web 專案。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="fa2a3-122">在套件管理員主控台中，輸入下列命令的下列命令：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="fa2a3-123">在本教學課程中，我們將使用[SendGrid](http://sendgrid.com/)傳送電子郵件並[Twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms 傳簡訊到的。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="fa2a3-124">`Identity.Samples`套件會安裝我們將使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="fa2a3-125">設定[專案，以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="fa2a3-126">*選擇性*： 依照我[教學課程中確認電子郵件](account-confirmation-and-password-recovery-with-aspnet-identity.md)SendGrid 連結然後執行應用程式並註冊的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="fa2a3-127">* 選擇性: * 移除此範例示範電子郵件連結確認程式碼 (`ViewBag.Link`帳戶控制器中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="fa2a3-128">請參閱`DisplayEmail`和`ForgotPasswordConfirmation`動作方法和 razor 檢視)。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="fa2a3-129"><em>選擇性: * 移除`ViewBag.Status`程式碼從管理和帳戶控制器和 *Views\Account\VerifyCode.cshtml</em>並<em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="fa2a3-130">或者，您可以保留`ViewBag.Status`顯示畫面來測試此應用程式在本機而不需要將連結，並傳送電子郵件和 SMS 訊息的運作方式。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="fa2a3-131">警告： 如果您變更任何安全性設定，在此範例中，生產應用程式必須進行的變更會明確呼叫的安全性稽核。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="fa2a3-132">設定 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="fa2a3-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="fa2a3-133">本教學課程提供使用 Twilio 或 ASPSMS 的指示，但您可以使用任何其他 SMS 提供者。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="fa2a3-134">**使用 SMS 提供者建立使用者帳戶**</span><span class="sxs-lookup"><span data-stu-id="fa2a3-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="fa2a3-135">建立[Twilio](https://www.twilio.com/try-twilio)該[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)帳戶。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="fa2a3-136">**安裝其他套件或新增服務參考**</span><span class="sxs-lookup"><span data-stu-id="fa2a3-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="fa2a3-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-137">Twilio:</span></span>  
   <span data-ttu-id="fa2a3-138">在套件管理員主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="fa2a3-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-139">ASPSMS:</span></span>  
   <span data-ttu-id="fa2a3-140">下列服務參考必須加入：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="fa2a3-141">位址:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="fa2a3-142">命名空間:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="fa2a3-143">**找出 SMS 提供者使用者認證**</span><span class="sxs-lookup"><span data-stu-id="fa2a3-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="fa2a3-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-144">Twilio:</span></span>  
   <span data-ttu-id="fa2a3-145">從**儀表板**索引標籤上的 Twilio 帳戶，複製**Account SID**並**驗證權杖**。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="fa2a3-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-146">ASPSMS:</span></span>  
   <span data-ttu-id="fa2a3-147">從您的帳戶設定，瀏覽至**Userkey**並將它複製以及您自行定義**密碼**。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="fa2a3-148">我們將稍後將這些值儲存在變數`SMSAccountIdentification`和`SMSAccountPassword`。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="fa2a3-149">**指定寄件者識別碼 / 建立者**</span><span class="sxs-lookup"><span data-stu-id="fa2a3-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="fa2a3-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-150">Twilio:</span></span>  
   <span data-ttu-id="fa2a3-151">從**數字**索引標籤上，複製您的 Twilio 電話號碼。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="fa2a3-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-152">ASPSMS:</span></span>  
   <span data-ttu-id="fa2a3-153">內**解除鎖定的建立者**功能表上，解除鎖定一或多個建立者，或選擇 英數字元的建立者 （不支援所有的網路）。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="fa2a3-154">稍後，我們會將這個值儲存在變數中`SMSAccountFrom`。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="fa2a3-155">**將 SMS 提供者認證傳送到應用程式**</span><span class="sxs-lookup"><span data-stu-id="fa2a3-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="fa2a3-156">提供的認證與寄件者電話號碼給應用程式：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="fa2a3-157">安全性-機密資料絕不儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="fa2a3-158">上述保持簡單的範例程式碼中加入的帳戶和認證。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="fa2a3-159">請參閱 Jon Atten [ASP.NET MVC： 保留原始檔控制的私用設定現成](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="fa2a3-160">**資料傳輸至 SMS 提供者的實作**</span><span class="sxs-lookup"><span data-stu-id="fa2a3-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="fa2a3-161">設定`SmsService`類別內*應用程式\_Start\IdentityConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="fa2a3-162">根據使用的 SMS 提供者啟用其中一個**Twilio**或**ASPSMS**區段：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="fa2a3-163">執行應用程式，以及您先前註冊的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="fa2a3-164">按一下您的使用者識別碼，就會啟動`Index`中的動作方法`Manage`控制站。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="fa2a3-165">按一下 新增。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="fa2a3-166">在幾秒鐘的時間，您會使用驗證碼的簡訊。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="fa2a3-167">請輸入並按下**送出**。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="fa2a3-168">[管理] 檢視會顯示已加入您的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="fa2a3-169">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="fa2a3-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="fa2a3-170">`Index`中的動作方法`Manage`控制器設定根據您的上一個動作的狀態訊息，並提供變更您的本機密碼，或新增本機帳戶的連結。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="fa2a3-171">`Index`方法也會顯示狀態，或您的 2FA 電話號碼、 外部登入、 啟用 2FA，並記住 2FA 方法，這個瀏覽器 （稍後說明）。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="fa2a3-172">按一下您的使用者識別碼 （電子郵件），標題列中，未通過一則訊息。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="fa2a3-173">按一下**電話號碼： 移除**連結傳遞`Message=RemovePhoneSuccess`作為查詢字串。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="fa2a3-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="fa2a3-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="fa2a3-175">`AddPhoneNumber`動作方法會顯示一個對話方塊，輸入可以接收簡訊的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="fa2a3-176">按一下**傳送驗證碼** 按鈕會將電話號碼公佈 HTTP POST`AddPhoneNumber`動作方法。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="fa2a3-177">`GenerateChangePhoneNumberTokenAsync`方法會產生將在簡訊中的安全性權杖。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="fa2a3-178">如果 SMS 服務已設定，權杖會傳送為字串&quot;您的安全性程式碼&lt;語彙基元&gt;&quot;。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="fa2a3-179">`SmsService.SendAsync`方法會以非同步方式呼叫，然後應用程式會重新導向至`VerifyPhoneNumber`動作方法 （這會顯示下列對話方塊），您可以在其中輸入驗證碼。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="fa2a3-180">一旦您輸入程式碼，並按一下 [提交]，程式碼會張貼到 HTTP POST`VerifyPhoneNumber`動作方法。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="fa2a3-181">`ChangePhoneNumberAsync`方法會檢查已張貼的安全性驗證碼。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="fa2a3-182">如果程式碼正確無誤，請加入的電話號碼`PhoneNumber`欄位`AspNetUsers`資料表。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="fa2a3-183">如果成功，該呼叫`SignInAsync`方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="fa2a3-184">`isPersistent`參數會設定是否在多個要求之間保存驗證工作階段。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="fa2a3-185">當您變更您的安全性設定檔時，會產生新的安全性戳記，並儲存在`SecurityStamp`欄位*AspNetUsers*資料表。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="fa2a3-186">請注意，`SecurityStamp`欄位是不同的安全性 cookie。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="fa2a3-187">安全性 cookie 不會儲存在`AspNetUsers`資料表 （或任何其他身分識別資料庫中的位置）。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="fa2a3-188">使用自我簽署的安全性 cookie 語彙基元[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)並建立不含`UserId, SecurityStamp`和到期時間資訊。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="fa2a3-189">Cookie 中介軟體會檢查每個要求的 cookie。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="fa2a3-190">`SecurityStampValidator`方法中的`Startup`類別叫用的資料庫，並定期檢查安全性戳記依照`validateInterval`。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="fa2a3-191">這只會每隔 30 分鐘 （在我們的範例），除非您變更您的安全性設定檔。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="fa2a3-192">在 30 分鐘的間隔已選擇將存取資料庫的次數降至最低。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="fa2a3-193">`SignInAsync`方法需要的安全性設定檔進行任何變更時呼叫。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="fa2a3-194">時的安全性設定檔變更時，資料庫會更新`SecurityStamp`欄位中，而不呼叫`SignInAsync`方法會保持登入*只*直到下一次 OWIN 管線叫用的資料庫 ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="fa2a3-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="fa2a3-195">您可以藉由變更進行測試`SignInAsync`方法來立即傳回，並設定 cookie`validateInterval`從 30 分鐘的時間為 5 秒的屬性：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="fa2a3-196">以上述的程式碼變更，您可以變更您的安全性設定檔 (比方說，是藉由變更的狀態**已啟用的兩個因素**)，而且您會在 5 秒內記錄時`SecurityStampValidator.OnValidateIdentity`方法失敗。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="fa2a3-197">移除在的返回行`SignInAsync`方法，讓另一個變更的安全性設定檔，並將無法將您登出。`SignInAsync`方法會產生新的安全性 cookie。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="fa2a3-198">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="fa2a3-198">Enable two-factor authentication</span></span>

<span data-ttu-id="fa2a3-199">在範例應用程式中，您需要使用 UI 來啟用雙因素驗證 (2FA)。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="fa2a3-200">若要啟用 2FA，請按一下您在導覽列中的使用者識別碼 （電子郵件別名）。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="fa2a3-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="fa2a3-201">按一下 啟用 2FA。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="fa2a3-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="fa2a3-202">登出後再重新登入。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-202">Log out, then log back in.</span></span> <span data-ttu-id="fa2a3-203">如果您已啟用電子郵件 (請參閱我[先前的教學課程](account-confirmation-and-password-recovery-with-aspnet-identity.md))，您可以選取 SMS 或 2FA 的電子郵件。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="fa2a3-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="fa2a3-204">確認字碼頁會顯示您可以在其中輸入 （來自 SMS 或電子郵件） 的程式碼。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="fa2a3-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="fa2a3-205">按一下**記住此瀏覽器**核取方塊將會豁免不必使用 2FA 登入具有該電腦和瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="fa2a3-206">啟用 2FA，然後按一下**記住此瀏覽器**會為您提供強式 2FA 保護避免惡意使用者嘗試存取您的帳戶，只要它們不能存取您的電腦。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="fa2a3-207">您經常使用的任何私用電腦上，您可以執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="fa2a3-208">藉由設定**記住此瀏覽器**不定期使用，您的電腦中獲得 2FA 的額外的安全性，取得上不需要經歷 2FA，在您自己的電腦上的便利性。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="fa2a3-209">如何註冊雙因素驗證提供者</span><span class="sxs-lookup"><span data-stu-id="fa2a3-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="fa2a3-210">當您建立新的 MVC 專案中， *IdentityConfig.cs*檔案包含下列的程式碼，以註冊雙因素驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="fa2a3-211">新增 2FA 的電話號碼</span><span class="sxs-lookup"><span data-stu-id="fa2a3-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="fa2a3-212">`AddPhoneNumber`中的動作方法`Manage`控制站產生安全性權杖，並傳送到電話號碼您所提供。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="fa2a3-213">之後傳送權杖，它將重新導向至`VerifyPhoneNumber`動作方法，您可以在其中輸入程式碼以註冊 2FA SMS。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="fa2a3-214">除非您已經驗證的電話號碼，不會使用 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="fa2a3-215">啟用 2FA</span><span class="sxs-lookup"><span data-stu-id="fa2a3-215">Enabling 2FA</span></span>

<span data-ttu-id="fa2a3-216">`EnableTFA`動作方法會啟用 2FA:</span><span class="sxs-lookup"><span data-stu-id="fa2a3-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="fa2a3-217">請注意`SignInAsync`必須呼叫，因為啟用 2FA 已變更的安全性設定檔。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="fa2a3-218">啟用 2FA 之後，使用者必須使用 2FA 登入，使用已註冊 （SMS 和電子郵件中的範例） 2FA 方法。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="fa2a3-219">您可以新增更多的 2FA 提供者，例如 QR 程式碼產生器，或者您可以撰寫您自己的 (請參閱[使用 Google 驗證器使用 ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="fa2a3-220">2FA 的程式碼使用產生[時間為基礎的單次密碼演算法](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)和代碼六分鐘內是否有效。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="fa2a3-221">如果您採用多個輸入的代碼六分鐘內，您會收到無效的程式碼錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="fa2a3-222">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="fa2a3-222">Combine social and local login accounts</span></span>

<span data-ttu-id="fa2a3-223">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="fa2a3-224">依照以下順序&quot; RickAndMSFT@gmail.com &quot;首先會建立為本機登入，但您可以為社交登入第一，建立帳戶，然後新增本機登入。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="fa2a3-225">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-225">Click on the **Manage** link.</span></span> <span data-ttu-id="fa2a3-226">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="fa2a3-227">按一下連結以在服務中的另一個記錄檔，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="fa2a3-228">已合併兩個帳戶，您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="fa2a3-229">您可能想要新增本機帳戶，以防使用者社交的記錄檔，在 驗證服務已關閉，或是更有可能他們已無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="fa2a3-230">在下圖中，Tom 則社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="fa2a3-231">按一下**挑選密碼**可讓您在新增本機記錄檔相同的帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="fa2a3-232">帳戶鎖定的暴力密碼破解攻擊</span><span class="sxs-lookup"><span data-stu-id="fa2a3-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="fa2a3-233">您可以啟用使用者鎖定您的應用程式，免於字典攻擊的檔案，以保護帳戶。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="fa2a3-234">下列程式碼中`ApplicationUserManager Create`方法設定鎖定：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="fa2a3-235">上述程式碼可讓鎖定只雙因素驗證。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="fa2a3-236">雖然您可以藉由變更登入的鎖定`shouldLockout`設為 true`Login`帳戶控制器方法，建議您不啟用鎖定的登入，因為它可讓帳戶容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)登入攻擊。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="fa2a3-237">在範例程式碼中，鎖定已停用的系統管理員帳戶中建立`ApplicationDbInitializer Seed`方法：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="fa2a3-238">需要有已驗證的電子郵件帳戶使用者</span><span class="sxs-lookup"><span data-stu-id="fa2a3-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="fa2a3-239">下列程式碼會要求使用者已驗證電子郵件帳戶，他們可以登入前：</span><span class="sxs-lookup"><span data-stu-id="fa2a3-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="fa2a3-240">SignInManager 如何檢查 2FA 需求</span><span class="sxs-lookup"><span data-stu-id="fa2a3-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="fa2a3-241">同時在本機記錄檔並社交登入檢查，以查看是否已啟用 2FA。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="fa2a3-242">如果已啟用 2FA`SignInManager`登入方法會傳回`SignInStatus.RequiresVerification`，使用者將會重新導向至`SendCode`動作方法，他們必須輸入程式碼來完成序列中的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="fa2a3-243">在使用者本機 cookie 上設定如果使用者具有 RememberMe`SignInManager`會傳回`SignInStatus.Success`，他們就不需要經歷 2FA。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="fa2a3-244">下列程式碼示範`SendCode`動作方法。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="fa2a3-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)建立所有使用者啟用 2FA 方法。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="fa2a3-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)傳遞至[DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx)協助程式，可讓使用者選取的 2FA 方法 （通常是電子郵件和簡訊）。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="fa2a3-247">使用者張貼 2FA 的方法，一旦`HTTP POST SendCode`呼叫動作方法時， `SignInManager` 2FA 的程式碼和使用者會重新導向至傳送`VerifyCode`他們可以在其中輸入程式碼來完成的記錄檔中的動作方法。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="fa2a3-248">2FA 鎖定</span><span class="sxs-lookup"><span data-stu-id="fa2a3-248">2FA Lockout</span></span>

<span data-ttu-id="fa2a3-249">雖然您可以設定帳戶鎖定的密碼登入嘗試失敗，該方法可讓您的登入容易[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)鎖定。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="fa2a3-250">我們建議您只適用於帳戶鎖定 2FA。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="fa2a3-251">當`ApplicationUserManager`會建立，範例程式碼設定 2FA 鎖定和`MaxFailedAccessAttemptsBeforeLockout`五個。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="fa2a3-252">一旦使用者登入 （透過本機帳戶或社交帳戶），儲存 2FA 在每次失敗的嘗試，而且使用者如果達到嘗試次數上限，則會被封鎖五分鐘的時間 (您可以設定使用的時間鎖定`DefaultAccountLockoutTimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="fa2a3-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="fa2a3-253">Additional Resources</span></span>

- <span data-ttu-id="fa2a3-254">[ASP.NET Identity 建議資源](../getting-started/aspnet-identity-recommended-resources.md)因此連結的身分識別部落格、 影片、 教學課程和絕佳的完整清單。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="fa2a3-255">[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)也會示範如何將使用者資料表中的設定檔資訊。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="fa2a3-256">[ASP.NET MVC 和身分識別 2.0： 了解基本概念](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)由 John Atten。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="fa2a3-257">帳戶確認和密碼復原，使用 ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="fa2a3-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="fa2a3-258">ASP.NET Identity 簡介</span><span class="sxs-lookup"><span data-stu-id="fa2a3-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="fa2a3-259">[宣佈 ASP.NET 身分識別 2.0.0 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)請參閱 Pranav rastogi。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="fa2a3-260">[ASP.NET Identity 2.0： 設定帳戶驗證和雙因素授權](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)由 John Atten。</span><span class="sxs-lookup"><span data-stu-id="fa2a3-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
