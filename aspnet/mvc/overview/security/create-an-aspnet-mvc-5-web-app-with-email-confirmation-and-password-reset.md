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
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="ce44f-104">建立安全的 ASP.NET MVC 5 web 應用程式與記錄檔中，電子郵件確認和密碼重設 (C#)</span><span class="sxs-lookup"><span data-stu-id="ce44f-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="ce44f-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ce44f-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="ce44f-106">本教學課程會示範如何建立 ASP.NET MVC 5 web 應用程式，但電子郵件確認和密碼重設使用 ASP.NET 識別的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="ce44f-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="ce44f-107">您可以下載完成的應用程式[這裡](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="ce44f-108">此下載包含偵錯的協助程式，可讓您測試而不需設定電子郵件或 SMS 提供者的電子郵件確認與簡訊。</span><span class="sxs-lookup"><span data-stu-id="ce44f-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="ce44f-109">本教學課程中所編寫的[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="ce44f-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="ce44f-110">建立 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="ce44f-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="ce44f-111">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="ce44f-112">安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ce44f-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="ce44f-113">警告： 您必須安裝[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本，才能完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="ce44f-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="ce44f-114">建立新的 ASP.NET Web 專案，然後選取 MVC 範本。</span><span class="sxs-lookup"><span data-stu-id="ce44f-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="ce44f-115">Web Form 也支援 ASP.NET Identity，因此您可以依照類似的步驟，在 web form 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ce44f-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="ce44f-116">保留預設驗證為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="ce44f-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="ce44f-117">如果您想要裝載應用程式在 Azure 中的，將保留勾選核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ce44f-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="ce44f-118">稍後在本教學課程中，我們將部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="ce44f-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="ce44f-119">您可以[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="ce44f-120">設定[專案以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="ce44f-121">執行應用程式，請按一下**註冊**連結和註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="ce44f-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="ce44f-122">此時，只有在電子郵件時才驗證與[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="ce44f-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="ce44f-123">在 伺服器總管瀏覽至**資料 Connections\DefaultConnection\Tables\AspNetUsers**，以滑鼠右鍵按一下並選取**開啟資料表定義**。</span><span class="sxs-lookup"><span data-stu-id="ce44f-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="ce44f-124">下圖顯示`AspNetUsers`結構描述：</span><span class="sxs-lookup"><span data-stu-id="ce44f-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="ce44f-125">以滑鼠右鍵按一下**AspNetUsers**資料表，然後選取**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="ce44f-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="ce44f-126">此時不已確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ce44f-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="ce44f-127">按一下資料列上，選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="ce44f-127">Click on the row and select delete.</span></span> <span data-ttu-id="ce44f-128">您將在下一個步驟中，再次新增這封電子郵件並傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ce44f-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="ce44f-129">電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="ce44f-129">Email confirmation</span></span>

<span data-ttu-id="ce44f-130">若要確認新的使用者註冊，以確認它們不會模擬其他人的電子郵件的最佳作法是 （也就是它們未向其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="ce44f-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="ce44f-131">假設您有討論論壇，您會想要防止`"bob@example.com"`從註冊為`"joe@contoso.com"`。</span><span class="sxs-lookup"><span data-stu-id="ce44f-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="ce44f-132">沒有電子郵件確認`"joe@contoso.com"`無法從您的應用程式取得垃圾電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ce44f-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="ce44f-133">假設 Bob 意外地註冊為`"bib@example.com"`而且未注意到，他就無法使用密碼復原，因為應用程式沒有他正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ce44f-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="ce44f-134">電子郵件確認從 bot 提供有限的保護，並不提供保護，從決定垃圾郵件，它們有許多的工作電子郵件別名，他們可以使用來註冊。</span><span class="sxs-lookup"><span data-stu-id="ce44f-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="ce44f-135">您通常想要讓新的使用者張貼到您的網站的任何資料之前已經確認透過電子郵件、 簡訊或其他機制。</span><span class="sxs-lookup"><span data-stu-id="ce44f-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="ce44f-136">在下列章節中，我們將啟用電子郵件確認，並修改程式碼以防止新註冊的使用者登入，直到已確認他們的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ce44f-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="ce44f-137">SendGrid 連結</span><span class="sxs-lookup"><span data-stu-id="ce44f-137">Hook up SendGrid</span></span>

<span data-ttu-id="ce44f-138">雖然本教學課程只會顯示如何將電子郵件通知，透過[SendGrid](http://sendgrid.com/)，您可以傳送電子郵件使用 SMTP 和其他機制 (請參閱[其他資源](#addRes))。</span><span class="sxs-lookup"><span data-stu-id="ce44f-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="ce44f-139">在 Package Manager Console 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ce44f-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="ce44f-140">移至[Azure SendGrid 登入頁面](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)並註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="ce44f-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="ce44f-141">設定 SendGrid 將類似下列的程式碼加入*App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce44f-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="ce44f-142">您必須加入包含下列：</span><span class="sxs-lookup"><span data-stu-id="ce44f-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="ce44f-143">若要維持此範例的簡單性，我們將會儲存中的應用程式設定*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="ce44f-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="ce44f-144">安全性-永遠不會儲存敏感的資料來源上的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="ce44f-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="ce44f-145">帳戶和認證會儲存在 appSetting。</span><span class="sxs-lookup"><span data-stu-id="ce44f-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="ce44f-146">在 Azure 上，您可以安全地儲存這些值在**[設定](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure 入口網站中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ce44f-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="ce44f-147">請參閱[ASP.NET 和 Azure 部署的密碼和其他機密資料的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="ce44f-148">啟用帳戶控制器中的電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="ce44f-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="ce44f-149">確認*Views\Account\ConfirmEmail.cshtml*檔案具有正確的 razor 語法。</span><span class="sxs-lookup"><span data-stu-id="ce44f-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="ce44f-150">(在第一個字元 @ 列可能會遺失。</span><span class="sxs-lookup"><span data-stu-id="ce44f-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="ce44f-151">)</span><span class="sxs-lookup"><span data-stu-id="ce44f-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="ce44f-152">執行應用程式，然後按一下 [註冊] 連結。</span><span class="sxs-lookup"><span data-stu-id="ce44f-152">Run the app and click the Register link.</span></span> <span data-ttu-id="ce44f-153">一旦您提交註冊表單時，您登入。</span><span class="sxs-lookup"><span data-stu-id="ce44f-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="ce44f-154">檢查您的電子郵件帳戶，然後按一下連結來確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ce44f-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="ce44f-155">電子郵件之前需要確認登入</span><span class="sxs-lookup"><span data-stu-id="ce44f-155">Require email confirmation before log in</span></span>

<span data-ttu-id="ce44f-156">目前在使用者完成註冊表單，一旦他們登入。</span><span class="sxs-lookup"><span data-stu-id="ce44f-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="ce44f-157">您通常想要確認他們的電子郵件才能加以登入。</span><span class="sxs-lookup"><span data-stu-id="ce44f-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="ce44f-158">在下面的區段中，我們將會修改程式碼以要求新的使用者將確認電子郵件之前登入 （驗證）。</span><span class="sxs-lookup"><span data-stu-id="ce44f-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="ce44f-159">更新`HttpPost Register`下列反白顯示變更的方法：</span><span class="sxs-lookup"><span data-stu-id="ce44f-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="ce44f-160">註解`SignInAsync`方法，使用者將未簽署註冊。</span><span class="sxs-lookup"><span data-stu-id="ce44f-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="ce44f-161">`TempData["ViewBagLink"] = callbackUrl;`行可以用來[偵錯應用程式](#dbg)和測試不會傳送電子郵件的註冊。</span><span class="sxs-lookup"><span data-stu-id="ce44f-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="ce44f-162">`ViewBag.Message` 用來顯示的確認指示。</span><span class="sxs-lookup"><span data-stu-id="ce44f-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="ce44f-163">[下載範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)包含程式碼以測試電子郵件確認，而不需設定電子郵件，而且也可用來偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="ce44f-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="ce44f-164">建立`Views\Shared\Info.cshtml`檔案，然後加入下列的 razor 標記：</span><span class="sxs-lookup"><span data-stu-id="ce44f-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="ce44f-165">新增[Authorize 屬性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)至`Contact`主控制器動作方法。</span><span class="sxs-lookup"><span data-stu-id="ce44f-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="ce44f-166">您可以按一下**連絡人**連結，確認匿名使用者沒有存取權和已驗證的使用者沒有存取權。</span><span class="sxs-lookup"><span data-stu-id="ce44f-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="ce44f-167">您也必須更新`HttpPost Login`動作方法：</span><span class="sxs-lookup"><span data-stu-id="ce44f-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="ce44f-168">更新*Views\Shared\Error.cshtml*檢視以顯示錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="ce44f-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="ce44f-169">刪除中的任何帳戶**AspNetUsers**包含您想要使用測試的電子郵件別名的資料表。</span><span class="sxs-lookup"><span data-stu-id="ce44f-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="ce44f-170">執行應用程式，並確認在您確認電子郵件地址之前，您無法登入。</span><span class="sxs-lookup"><span data-stu-id="ce44f-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="ce44f-171">一旦您確認電子郵件地址，請按一下**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="ce44f-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="ce44f-172">復原/重設密碼</span><span class="sxs-lookup"><span data-stu-id="ce44f-172">Password recovery/reset</span></span>

<span data-ttu-id="ce44f-173">移除註解字元從`HttpPost ForgotPassword`中帳戶控制器動作方法：</span><span class="sxs-lookup"><span data-stu-id="ce44f-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="ce44f-174">移除註解字元從`ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)中*Views\Account\Login.cshtml* razor 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="ce44f-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="ce44f-175">登入頁面現在會顯示連結，以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="ce44f-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="ce44f-176">重新傳送電子郵件確認連結</span><span class="sxs-lookup"><span data-stu-id="ce44f-176">Resend email confirmation link</span></span>

<span data-ttu-id="ce44f-177">一旦使用者建立新的本機帳戶，它們是電子郵件確認連結需要它們來登入前使用。</span><span class="sxs-lookup"><span data-stu-id="ce44f-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="ce44f-178">如果使用者意外刪除的確認電子郵件，或永遠不會收到電子郵件，他們必須重新傳送的確認連結。</span><span class="sxs-lookup"><span data-stu-id="ce44f-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="ce44f-179">下列程式碼變更會示範如何啟用此功能。</span><span class="sxs-lookup"><span data-stu-id="ce44f-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="ce44f-180">將下列 helper 方法加入至底部*Controllers\AccountController.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="ce44f-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="ce44f-181">更新要使用新的協助程式的註冊方法：</span><span class="sxs-lookup"><span data-stu-id="ce44f-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="ce44f-182">更新重新傳送的密碼，如果尚未確認的使用者帳戶登入方法：</span><span class="sxs-lookup"><span data-stu-id="ce44f-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="ce44f-183">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="ce44f-183">Combine social and local login accounts</span></span>

<span data-ttu-id="ce44f-184">您可以結合本機和社交帳戶按一下電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="ce44f-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="ce44f-185">依照以下順序**RickAndMSFT@gmail.com**首先會建立為本機登入，但您可以建立帳戶與社交的記錄檔中第一個，然後加入本機的登入。</span><span class="sxs-lookup"><span data-stu-id="ce44f-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="ce44f-186">按一下**管理**連結。</span><span class="sxs-lookup"><span data-stu-id="ce44f-186">Click on the **Manage** link.</span></span> <span data-ttu-id="ce44f-187">請注意**外部登入： 0**與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="ce44f-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="ce44f-188">按一下此連結服務中的另一個記錄檔，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="ce44f-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="ce44f-189">已結合兩個帳戶，您將無法與任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="ce44f-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="ce44f-190">您可能會想讓使用者加入本機帳戶，以防使用者社交的記錄檔中驗證服務已關閉，或可能比較他們已經失去其社交帳戶的存取。</span><span class="sxs-lookup"><span data-stu-id="ce44f-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="ce44f-191">在下圖中，Tom 是社交登入 (您可以看到從**外部登入： 1**顯示在頁面上)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="ce44f-192">按一下**挑選密碼**可讓您在上新增本機記錄檔相同的帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="ce44f-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="ce44f-193">更深入的電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="ce44f-193">Email confirmation in more depth</span></span>

<span data-ttu-id="ce44f-194">我的教學課程[帳戶確認和 ASP.NET 識別的密碼復原](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入這個主題更詳細的資料。</span><span class="sxs-lookup"><span data-stu-id="ce44f-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="ce44f-195">偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="ce44f-195">Debugging the app</span></span>

<span data-ttu-id="ce44f-196">如果您未收到包含連結的電子郵件：</span><span class="sxs-lookup"><span data-stu-id="ce44f-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="ce44f-197">請檢查您的垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce44f-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="ce44f-198">登入您的 SendGrid 帳戶並按一下[電子郵件 」 活動連結](https://sendgrid.com/logs/index)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="ce44f-199">若要測試沒有電子郵件驗證連結，下載[完成的範例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="ce44f-200">確認碼和確認連結將顯示在頁面上。</span><span class="sxs-lookup"><span data-stu-id="ce44f-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="ce44f-201">其他資源</span><span class="sxs-lookup"><span data-stu-id="ce44f-201">Additional Resources</span></span>

- [<span data-ttu-id="ce44f-202">ASP.NET Identity 的連結，建議使用的資源</span><span class="sxs-lookup"><span data-stu-id="ce44f-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="ce44f-203">[帳戶確認和密碼復原與 ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)進入確認的密碼復原和帳戶的其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="ce44f-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="ce44f-204">[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入的 MVC 5 應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教學課程會示範如何撰寫 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth 2 」 授權。</span><span class="sxs-lookup"><span data-stu-id="ce44f-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="ce44f-205">它也會示範如何加入識別資料庫中的其他資料。</span><span class="sxs-lookup"><span data-stu-id="ce44f-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="ce44f-206">[將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="ce44f-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="ce44f-207">本教學課程將加入 Azure 部署時，如何保護您的應用程式角色、 如何使用成員資格應用程式開發介面來新增使用者和角色，以及其他安全性功能。</span><span class="sxs-lookup"><span data-stu-id="ce44f-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="ce44f-208">OAuth 2 建立 Google 應用程式和應用程式連接至專案</span><span class="sxs-lookup"><span data-stu-id="ce44f-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="ce44f-209">在 Facebook 中建立應用程式和應用程式連接至專案</span><span class="sxs-lookup"><span data-stu-id="ce44f-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="ce44f-210">在專案中的 SSL 設定</span><span class="sxs-lookup"><span data-stu-id="ce44f-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
