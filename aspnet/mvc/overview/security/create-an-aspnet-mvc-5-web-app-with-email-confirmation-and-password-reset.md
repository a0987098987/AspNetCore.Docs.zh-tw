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
ms.openlocfilehash: 5092476c6cf59bea6fab6fa6f169ff11ec4c9c4a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207481"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="7f6be-104">建立安全的 ASP.NET MVC 5 web 應用程式登入、 電子郵件確認和密碼重設 (C#)</span><span class="sxs-lookup"><span data-stu-id="7f6be-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="7f6be-105">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="7f6be-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="7f6be-106">本教學課程會示範如何建置 ASP.NET MVC 5 web 應用程式使用電子郵件確認和密碼重設使用 ASP.NET 身分識別的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="7f6be-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="7f6be-107">您可以下載完成的應用程式[此處](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="7f6be-108">此下載包含可讓您測試而不需設定電子郵件或 SMS 提供者的電子郵件確認和 SMS 的偵錯協助程式。</span><span class="sxs-lookup"><span data-stu-id="7f6be-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="7f6be-109">本教學課程以寫入[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="7f6be-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="7f6be-110">建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="7f6be-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="7f6be-111">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="7f6be-112">安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="7f6be-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="7f6be-113">警告： 您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更新版本，才能完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="7f6be-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="7f6be-114">建立新的 ASP.NET Web 專案，然後選取 [MVC] 範本。</span><span class="sxs-lookup"><span data-stu-id="7f6be-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="7f6be-115">Web Form 也支援 ASP.NET 身分識別，因此您可以依照類似的步驟，在 web form 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f6be-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="7f6be-116">保留為預設的驗證**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="7f6be-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="7f6be-117">如果您想要裝載應用程式在 Azure 中的，核取方塊保持勾選。</span><span class="sxs-lookup"><span data-stu-id="7f6be-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="7f6be-118">稍後在本教學課程中，我們將會部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="7f6be-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="7f6be-119">您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="7f6be-120">設定[專案，以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="7f6be-121">執行應用程式，請按一下**註冊**連結，並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="7f6be-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="7f6be-122">此時，唯一的驗證電子郵件是使用[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="7f6be-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="7f6be-123">在 [伺服器總管] 中，瀏覽至**資料 Connections\DefaultConnection\Tables\AspNetUsers**，以滑鼠右鍵按一下並選取**開啟資料表定義**。</span><span class="sxs-lookup"><span data-stu-id="7f6be-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="7f6be-124">下圖顯示`AspNetUsers`結構描述：</span><span class="sxs-lookup"><span data-stu-id="7f6be-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="7f6be-125">以滑鼠右鍵按一下**AspNetUsers**資料表，然後選取**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="7f6be-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="7f6be-126">此時已不確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="7f6be-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="7f6be-127">按一下資料列，然後選取 刪除。</span><span class="sxs-lookup"><span data-stu-id="7f6be-127">Click on the row and select delete.</span></span> <span data-ttu-id="7f6be-128">您將在下一個步驟中，再次新增這封電子郵件並傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="7f6be-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="7f6be-129">電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="7f6be-129">Email confirmation</span></span>

<span data-ttu-id="7f6be-130">最好確認新的使用者註冊，以確認它們不會模擬其他人的電子郵件 （亦即，尚未註冊使用其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="7f6be-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7f6be-131">假設您有討論論壇，您會想要防止`"bob@example.com"`註冊為`"joe@contoso.com"`。</span><span class="sxs-lookup"><span data-stu-id="7f6be-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="7f6be-132">而不需要電子郵件確認`"joe@contoso.com"`無法從您的應用程式收到不想要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="7f6be-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="7f6be-133">假設 Bob 不小心註冊為`"bib@example.com"`並沒注意到，他便無法使用密碼復原，因為應用程式不需要其正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="7f6be-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="7f6be-134">電子郵件確認來自 bot 提供有限的保護，並不提供保護，從決定濫發垃圾郵件者，它們有許多的工作電子郵件別名，它們可用來註冊。</span><span class="sxs-lookup"><span data-stu-id="7f6be-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="7f6be-135">您通常想要防止新使用者張貼至您的網站的任何資料之前先確認電子郵件、 SMS 文字訊息或其他機制。</span><span class="sxs-lookup"><span data-stu-id="7f6be-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="7f6be-136">在下列章節中，我們將啟用電子郵件確認和修改程式碼以防止新註冊的使用者登入，直到已確認其電子郵件。</span><span class="sxs-lookup"><span data-stu-id="7f6be-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="7f6be-137">將 SendGrid 連結</span><span class="sxs-lookup"><span data-stu-id="7f6be-137">Hook up SendGrid</span></span>

<span data-ttu-id="7f6be-138">在本節中的指示不是最新的。</span><span class="sxs-lookup"><span data-stu-id="7f6be-138">The instructions in this section are not current.</span></span> <span data-ttu-id="7f6be-139">請參閱[設定 SendGrid 電子郵件提供者](/aspnet/core/security/authentication/accconfirm#configure-email-provider)更新指示進行。</span><span class="sxs-lookup"><span data-stu-id="7f6be-139">See [Configure SendGrid email provider](/aspnet/core/security/authentication/accconfirm#configure-email-provider) for updated instructions.</span></span>

<span data-ttu-id="7f6be-140">雖然本教學課程中只會顯示如何將透過電子郵件通知[SendGrid](http://sendgrid.com/)，您可以傳送電子郵件使用 SMTP 和其他機制 (請參閱[其他資源](#addRes))。</span><span class="sxs-lookup"><span data-stu-id="7f6be-140">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="7f6be-141">在套件管理員主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="7f6be-141">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="7f6be-142">移至[Azure SendGrid 註冊頁面](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)並註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="7f6be-142">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="7f6be-143">將新增類似下列程式碼，以設定 SendGrid *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="7f6be-143">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="7f6be-144">您必須新增下列 include:</span><span class="sxs-lookup"><span data-stu-id="7f6be-144">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="7f6be-145">為了簡化此範例中，我們將儲存在應用程式設定*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="7f6be-145">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="7f6be-146">安全性-機密資料絕不儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="7f6be-146">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="7f6be-147">帳戶和認證會儲存在 appSetting 中。</span><span class="sxs-lookup"><span data-stu-id="7f6be-147">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="7f6be-148">在 Azure 上，您可以安全地儲存這些值在 **[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** 在 Azure 入口網站中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="7f6be-148">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="7f6be-149">請參閱[最佳做法將密碼和其他機密資料部署到 ASP.NET 和 Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-149">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="7f6be-150">啟用帳戶控制器中的電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="7f6be-150">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="7f6be-151">請確認*Views\Account\ConfirmEmail.cshtml*檔案有正確的 razor 語法。</span><span class="sxs-lookup"><span data-stu-id="7f6be-151">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="7f6be-152">(@ 字元在第一個列可能會遺失。</span><span class="sxs-lookup"><span data-stu-id="7f6be-152">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="7f6be-153">)</span><span class="sxs-lookup"><span data-stu-id="7f6be-153">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="7f6be-154">執行應用程式，然後按一下 [註冊] 連結。</span><span class="sxs-lookup"><span data-stu-id="7f6be-154">Run the app and click the Register link.</span></span> <span data-ttu-id="7f6be-155">一旦您提交註冊表單，您會登入。</span><span class="sxs-lookup"><span data-stu-id="7f6be-155">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="7f6be-156">檢查您的電子郵件帳戶，然後按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="7f6be-156">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="7f6be-157">需要先登入的電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="7f6be-157">Require email confirmation before log in</span></span>

<span data-ttu-id="7f6be-158">目前在使用者完成註冊表單，一旦他們登入。</span><span class="sxs-lookup"><span data-stu-id="7f6be-158">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="7f6be-159">您通常想要記錄之前，先確認其電子郵件。</span><span class="sxs-lookup"><span data-stu-id="7f6be-159">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="7f6be-160">在下一節，我們將修改的程式碼，以要求新的使用者將確認電子郵件之前在登入時 （驗證）。</span><span class="sxs-lookup"><span data-stu-id="7f6be-160">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="7f6be-161">更新`HttpPost Register`方法具有下列醒目提示的變更：</span><span class="sxs-lookup"><span data-stu-id="7f6be-161">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="7f6be-162">註解`SignInAsync`方法，使用者將不會簽署註冊。</span><span class="sxs-lookup"><span data-stu-id="7f6be-162">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="7f6be-163">`TempData["ViewBagLink"] = callbackUrl;`行可以用來[偵錯應用程式](#dbg)和測試不會傳送電子郵件的註冊。</span><span class="sxs-lookup"><span data-stu-id="7f6be-163">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="7f6be-164">`ViewBag.Message` 用來顯示的確認指示。</span><span class="sxs-lookup"><span data-stu-id="7f6be-164">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="7f6be-165">[下載範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)包含程式碼來測試而不需設定電子郵件的電子郵件確認和也可用來偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f6be-165">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="7f6be-166">建立`Views\Shared\Info.cshtml`檔案，並新增下列 razor 標記：</span><span class="sxs-lookup"><span data-stu-id="7f6be-166">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="7f6be-167">新增[Authorize 屬性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)到`Contact`首頁控制器的動作方法。</span><span class="sxs-lookup"><span data-stu-id="7f6be-167">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="7f6be-168">您可以按一下**連絡人**連結來確認匿名使用者沒有存取，而且已驗證的使用者確實具備存取權。</span><span class="sxs-lookup"><span data-stu-id="7f6be-168">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="7f6be-169">您也必須更新`HttpPost Login`動作方法：</span><span class="sxs-lookup"><span data-stu-id="7f6be-169">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="7f6be-170">更新*Views\Shared\Error.cshtml*檢視，以顯示錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="7f6be-170">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="7f6be-171">刪除中的任何帳戶**AspNetUsers**包含您想要測試的電子郵件別名的資料表。</span><span class="sxs-lookup"><span data-stu-id="7f6be-171">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="7f6be-172">執行應用程式，並確認您已確認您的電子郵件地址之前，您無法登入。</span><span class="sxs-lookup"><span data-stu-id="7f6be-172">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="7f6be-173">一旦您確認您的電子郵件地址，請按一下**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="7f6be-173">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="7f6be-174">復原/重設密碼</span><span class="sxs-lookup"><span data-stu-id="7f6be-174">Password recovery/reset</span></span>

<span data-ttu-id="7f6be-175">移除註解字元`HttpPost ForgotPassword`帳戶控制器中的動作方法：</span><span class="sxs-lookup"><span data-stu-id="7f6be-175">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="7f6be-176">移除註解字元`ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)中*Views\Account\Login.cshtml* razor 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="7f6be-176">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="7f6be-177">登入頁面現在會顯示連結，以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="7f6be-177">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="7f6be-178">重新傳送電子郵件確認連結</span><span class="sxs-lookup"><span data-stu-id="7f6be-178">Resend email confirmation link</span></span>

<span data-ttu-id="7f6be-179">一旦使用者建立新的本機帳戶時，它們是電子郵件確認連結，他們才能使用之前，他們可以登入。</span><span class="sxs-lookup"><span data-stu-id="7f6be-179">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="7f6be-180">如果使用者不小心刪除的確認電子郵件，或永遠不會收到電子郵件，他們必須重新傳送確認連結。</span><span class="sxs-lookup"><span data-stu-id="7f6be-180">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="7f6be-181">下列程式碼變更會示範如何啟用此功能。</span><span class="sxs-lookup"><span data-stu-id="7f6be-181">The following code changes show how to enable this.</span></span>

<span data-ttu-id="7f6be-182">新增下列 helper 方法的底部*Controllers\AccountController.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="7f6be-182">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="7f6be-183">更新 Register 方法將使用新的協助：</span><span class="sxs-lookup"><span data-stu-id="7f6be-183">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="7f6be-184">更新重新傳送的密碼，如果尚未確認的使用者帳戶登入方法：</span><span class="sxs-lookup"><span data-stu-id="7f6be-184">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7f6be-185">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="7f6be-185">Combine social and local login accounts</span></span>

<span data-ttu-id="7f6be-186">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="7f6be-186">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7f6be-187">依照以下順序**RickAndMSFT@gmail.com**首先會建立為本機登入，但您可以為社交登入第一，建立帳戶，然後新增本機登入。</span><span class="sxs-lookup"><span data-stu-id="7f6be-187">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="7f6be-188">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="7f6be-188">Click on the **Manage** link.</span></span> <span data-ttu-id="7f6be-189">附註**外部登入： 0**與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="7f6be-189">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="7f6be-190">按一下連結以在服務中的另一個記錄檔，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="7f6be-190">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="7f6be-191">已合併兩個帳戶，您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="7f6be-191">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="7f6be-192">您可能想要新增本機帳戶，以防使用者社交的記錄檔，在 驗證服務已關閉，或是更有可能他們已無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="7f6be-192">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="7f6be-193">在下圖中，Tom 則社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-193">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="7f6be-194">按一下**挑選密碼**可讓您在新增本機記錄檔相同的帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="7f6be-194">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="7f6be-195">深入的電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="7f6be-195">Email confirmation in more depth</span></span>

<span data-ttu-id="7f6be-196">我的教學課程[帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入本主題含有詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7f6be-196">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="7f6be-197">偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="7f6be-197">Debugging the app</span></span>

<span data-ttu-id="7f6be-198">如果您未收到包含連結的電子郵件：</span><span class="sxs-lookup"><span data-stu-id="7f6be-198">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="7f6be-199">請檢查垃圾郵件或垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="7f6be-199">Check your junk or spam folder.</span></span>
- <span data-ttu-id="7f6be-200">登入您的 SendGrid 帳戶，然後按一下[電子郵件 」 活動連結](https://sendgrid.com/logs/index)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-200">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="7f6be-201">若要測試沒有電子郵件驗證連結，下載[完整的範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-201">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="7f6be-202">頁面上，將會顯示的確認連結和確認碼。</span><span class="sxs-lookup"><span data-stu-id="7f6be-202">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="7f6be-203">其他資源</span><span class="sxs-lookup"><span data-stu-id="7f6be-203">Additional Resources</span></span>

- [<span data-ttu-id="7f6be-204">連結至 ASP.NET Identity 建議資源</span><span class="sxs-lookup"><span data-stu-id="7f6be-204">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="7f6be-205">[帳戶確認和密碼復原，使用 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入 確認的密碼復原和帳戶的其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7f6be-205">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="7f6be-206">[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 授權。</span><span class="sxs-lookup"><span data-stu-id="7f6be-206">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="7f6be-207">它也會示範如何新增額外的資料來識別資料庫。</span><span class="sxs-lookup"><span data-stu-id="7f6be-207">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="7f6be-208">[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="7f6be-208">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="7f6be-209">本教學課程中新增 Azure 部署中，如何保護您的應用程式角色，如何使用成員資格 API 來新增使用者和角色，以及額外的安全性功能。</span><span class="sxs-lookup"><span data-stu-id="7f6be-209">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="7f6be-210">建立 Google app for OAuth 2 和應用程式連線至專案</span><span class="sxs-lookup"><span data-stu-id="7f6be-210">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="7f6be-211">在 Facebook 中建立應用程式和應用程式連線至專案</span><span class="sxs-lookup"><span data-stu-id="7f6be-211">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="7f6be-212">設定專案中的 SSL</span><span class="sxs-lookup"><span data-stu-id="7f6be-212">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
