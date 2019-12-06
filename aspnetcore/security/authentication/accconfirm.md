---
title: ASP.NET Core 中的帳戶確認和密碼復原
author: rick-anderson
description: 瞭解如何使用電子郵件確認和密碼重設來建立 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: a4ecc2d91fb72915703dfaa146260f0c1360bded
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880760"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="994fc-103">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="994fc-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="994fc-104">依[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Ponant](https://github.com/Ponant)和[Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="994fc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="994fc-105">本教學課程說明如何使用電子郵件確認和密碼重設來建立 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="994fc-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="994fc-106">本教學課程**不**是開始主題。</span><span class="sxs-lookup"><span data-stu-id="994fc-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="994fc-107">您應該已熟悉：</span><span class="sxs-lookup"><span data-stu-id="994fc-107">You should be familiar with:</span></span>

* [<span data-ttu-id="994fc-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="994fc-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="994fc-109">驗證</span><span class="sxs-lookup"><span data-stu-id="994fc-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="994fc-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="994fc-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="994fc-111">請參閱[此 PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf)檔案以取得 ASP.NET Core 1.1 版本。</span><span class="sxs-lookup"><span data-stu-id="994fc-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="994fc-112">必要條件：</span><span class="sxs-lookup"><span data-stu-id="994fc-112">Prerequisites</span></span>

[<span data-ttu-id="994fc-113">.NET Core 3.0 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="994fc-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="994fc-114">使用驗證來建立及測試 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="994fc-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="994fc-115">執行下列命令來建立具有驗證的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="994fc-115">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="994fc-116">執行應用程式，選取 [**註冊**] 連結，然後註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="994fc-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="994fc-117">註冊之後，系統會將您重新導向至 [到 `/Identity/Account/RegisterConfirmation`] 頁面，其中包含用來模擬電子郵件確認的連結：</span><span class="sxs-lookup"><span data-stu-id="994fc-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="994fc-118">選取 [`Click here to confirm your account`] 連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="994fc-119">選取 [**登**入] 連結，並使用相同的認證進行登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="994fc-120">選取 [`Hello YourEmail@provider.com!`] 連結，將您重新導向至 [`/Identity/Account/Manage/PersonalData`] 頁面。</span><span class="sxs-lookup"><span data-stu-id="994fc-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="994fc-121">選取左側的 [**個人資料**] 索引標籤，然後選取 [**刪除**]。</span><span class="sxs-lookup"><span data-stu-id="994fc-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="994fc-122">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="994fc-122">Configure an email provider</span></span>

<span data-ttu-id="994fc-123">在本教學課程中， [SendGrid](https://sendgrid.com)是用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="994fc-124">您需要 SendGrid 帳戶和金鑰，才能傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="994fc-125">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="994fc-125">You can use other email providers.</span></span> <span data-ttu-id="994fc-126">建議您使用 SendGrid 或其他電子郵件服務來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="994fc-127">SMTP 很容易保護和設定正確。</span><span class="sxs-lookup"><span data-stu-id="994fc-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="994fc-128">建立類別來提取安全的電子郵件金鑰。</span><span class="sxs-lookup"><span data-stu-id="994fc-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="994fc-129">針對此範例，請建立*Services/AuthMessageSenderOptions .cs*：</span><span class="sxs-lookup"><span data-stu-id="994fc-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="994fc-130">設定 SendGrid 使用者秘密</span><span class="sxs-lookup"><span data-stu-id="994fc-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="994fc-131">使用 [[密碼管理員] 工具](xref:security/app-secrets)來設定 `SendGridUser` 和 `SendGridKey`。</span><span class="sxs-lookup"><span data-stu-id="994fc-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="994fc-132">例如：</span><span class="sxs-lookup"><span data-stu-id="994fc-132">For example:</span></span>

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="994fc-133">在 Windows 中，秘密管理員會將金鑰/值組儲存在 `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` 目錄的*秘密 json*檔案中。</span><span class="sxs-lookup"><span data-stu-id="994fc-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="994fc-134">不會加密*秘密 json*檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="994fc-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="994fc-135">下列標記顯示秘密的*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="994fc-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="994fc-136">`SendGridKey` 值已移除。</span><span class="sxs-lookup"><span data-stu-id="994fc-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="994fc-137">如需詳細資訊，請參閱[選項模式](xref:fundamentals/configuration/options)[和設定](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="994fc-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="994fc-138">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="994fc-138">Install SendGrid</span></span>

<span data-ttu-id="994fc-139">本教學課程說明如何透過[SendGrid](https://sendgrid.com/)新增電子郵件通知，但您可以使用 SMTP 和其他機制來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="994fc-140">安裝 `SendGrid` NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="994fc-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="994fc-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="994fc-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="994fc-142">從 [套件管理員主控台] 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="994fc-142">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="994fc-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="994fc-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="994fc-144">從主控台，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="994fc-144">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="994fc-145">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)來註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="994fc-146">執行 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="994fc-146">Implement IEmailSender</span></span>

<span data-ttu-id="994fc-147">若要執行 `IEmailSender`，請使用與下列類似的程式碼來建立*服務/EmailSender* ：</span><span class="sxs-lookup"><span data-stu-id="994fc-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="994fc-148">設定啟動以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="994fc-148">Configure startup to support email</span></span>

<span data-ttu-id="994fc-149">將下列程式碼新增至*Startup.cs*檔案中的 `ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="994fc-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="994fc-150">將 `EmailSender` 新增為暫時性服務。</span><span class="sxs-lookup"><span data-stu-id="994fc-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="994fc-151">註冊 `AuthMessageSenderOptions` 設定實例。</span><span class="sxs-lookup"><span data-stu-id="994fc-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="994fc-152">註冊、確認電子郵件和重設密碼</span><span class="sxs-lookup"><span data-stu-id="994fc-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="994fc-153">執行 web 應用程式，並測試帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="994fc-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="994fc-154">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="994fc-154">Run the app and register a new user</span></span>
* <span data-ttu-id="994fc-155">檢查您的電子郵件以取得帳戶確認連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="994fc-156">如果您沒有收到電子郵件，請參閱[Debug email](#debug) 。</span><span class="sxs-lookup"><span data-stu-id="994fc-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="994fc-157">按一下連結以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="994fc-158">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="994fc-159">登出。</span><span class="sxs-lookup"><span data-stu-id="994fc-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="994fc-160">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="994fc-160">Test password reset</span></span>

* <span data-ttu-id="994fc-161">如果您已登入，請選取 [**登出**]。</span><span class="sxs-lookup"><span data-stu-id="994fc-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="994fc-162">選取 [**登入**] 連結，然後選取 [**忘記密碼？** ] 連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="994fc-163">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="994fc-164">系統會傳送一封電子郵件，其中包含重設密碼的連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="994fc-165">檢查您的電子郵件，然後按一下連結以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="994fc-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="994fc-166">成功重設密碼之後，您就可以使用您的電子郵件和新密碼來登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="994fc-167">變更電子郵件和活動超時</span><span class="sxs-lookup"><span data-stu-id="994fc-167">Change email and activity timeout</span></span>

<span data-ttu-id="994fc-168">預設的無活動超時時間為14天。</span><span class="sxs-lookup"><span data-stu-id="994fc-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="994fc-169">下列程式碼會將非啟用時間設定為5天：</span><span class="sxs-lookup"><span data-stu-id="994fc-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="994fc-170">變更所有資料保護權杖壽命</span><span class="sxs-lookup"><span data-stu-id="994fc-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="994fc-171">下列程式碼會將所有資料保護權杖超時時間變更為3小時：</span><span class="sxs-lookup"><span data-stu-id="994fc-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="994fc-172">內建身分識別使用者權杖（請參閱[AspNetCore/src/Identity/Extensions. Core/src/TokenOptions](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ）具有[一天的超時時間](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="994fc-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="994fc-173">變更電子郵件權杖生命週期</span><span class="sxs-lookup"><span data-stu-id="994fc-173">Change the email token lifespan</span></span>

<span data-ttu-id="994fc-174">身分[識別使用者權杖](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)的預設權杖存留期為[一天](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="994fc-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="994fc-175">本節說明如何變更電子郵件權杖的生命週期。</span><span class="sxs-lookup"><span data-stu-id="994fc-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="994fc-176">新增自訂[DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和 <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>：</span><span class="sxs-lookup"><span data-stu-id="994fc-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="994fc-177">將自訂提供者新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="994fc-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="994fc-178">重新傳送電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="994fc-178">Resend email confirmation</span></span>

<span data-ttu-id="994fc-179">請參閱[這個 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5410)。</span><span class="sxs-lookup"><span data-stu-id="994fc-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="994fc-180">Debug 電子郵件</span><span class="sxs-lookup"><span data-stu-id="994fc-180">Debug email</span></span>

<span data-ttu-id="994fc-181">如果您無法讓電子郵件正常運作：</span><span class="sxs-lookup"><span data-stu-id="994fc-181">If you can't get email working:</span></span>

* <span data-ttu-id="994fc-182">在 `EmailSender.Execute` 中設定中斷點，以確認呼叫 `SendGridClient.SendEmailAsync`。</span><span class="sxs-lookup"><span data-stu-id="994fc-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="994fc-183">建立[主控台應用程式，以](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼將電子郵件傳送至 `EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="994fc-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="994fc-184">查看[電子郵件活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="994fc-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="994fc-185">檢查您的垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="994fc-185">Check your spam folder.</span></span>
* <span data-ttu-id="994fc-186">在不同的電子郵件提供者（Microsoft、Yahoo、Gmail 等）上嘗試另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="994fc-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="994fc-187">嘗試傳送至不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="994fc-188">**安全性最佳做法**是**不要**在測試和開發中使用生產秘密。</span><span class="sxs-lookup"><span data-stu-id="994fc-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="994fc-189">如果您將應用程式發佈至 Azure，請在 Azure Web 應用程式入口網站中將 SendGrid 秘密設定為應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="994fc-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="994fc-190">設定系統已設定為從環境變數讀取金鑰。</span><span class="sxs-lookup"><span data-stu-id="994fc-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="994fc-191">合併社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="994fc-191">Combine social and local login accounts</span></span>

<span data-ttu-id="994fc-192">若要完成這一節，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="994fc-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="994fc-193">請參閱[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="994fc-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="994fc-194">您可以按一下電子郵件連結，以合併本機和社交帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="994fc-195">在下列順序中，會先將 "RickAndMSFT@gmail.com" 建立為本機登入;不過，您可以先將帳戶建立為社交登入，然後再新增本機登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式： RickAndMSFT@gmail.com 使用者已驗證](accconfirm/_static/rick.png)

<span data-ttu-id="994fc-197">按一下 [**管理**] 連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-197">Click on the **Manage** link.</span></span> <span data-ttu-id="994fc-198">請注意與此帳戶相關聯的0外部（社交登入）。</span><span class="sxs-lookup"><span data-stu-id="994fc-198">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="994fc-200">按一下另一個登入服務的連結，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="994fc-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="994fc-201">在下圖中，Facebook 是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="994fc-201">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入視圖清單 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="994fc-203">已合併這兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-203">The two accounts have been combined.</span></span> <span data-ttu-id="994fc-204">您可以使用任一帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-204">You are able to sign in with either account.</span></span> <span data-ttu-id="994fc-205">您可能想要讓使用者新增本機帳戶，以防其社交登入驗證服務關閉，或更有可能失去其社交帳戶的存取權。</span><span class="sxs-lookup"><span data-stu-id="994fc-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="994fc-206">在網站有使用者之後啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="994fc-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="994fc-207">在網站上啟用帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="994fc-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="994fc-208">現有的使用者會被鎖定，因為他們的帳戶不會確認。</span><span class="sxs-lookup"><span data-stu-id="994fc-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="994fc-209">若要解決現有的使用者鎖定，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="994fc-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="994fc-210">更新資料庫，將所有現有的使用者標示為已確認。</span><span class="sxs-lookup"><span data-stu-id="994fc-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="994fc-211">確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="994fc-211">Confirm existing users.</span></span> <span data-ttu-id="994fc-212">例如，batch-傳送含有確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="994fc-213">必要條件：</span><span class="sxs-lookup"><span data-stu-id="994fc-213">Prerequisites</span></span>

[<span data-ttu-id="994fc-214">.NET Core 2.2 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="994fc-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="994fc-215">建立 web 應用程式和 scaffold 身分識別</span><span class="sxs-lookup"><span data-stu-id="994fc-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="994fc-216">執行下列命令來建立具有驗證的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="994fc-216">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="994fc-217">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="994fc-217">Test new user registration</span></span>

<span data-ttu-id="994fc-218">執行應用程式，選取 [**註冊**] 連結，然後註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="994fc-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="994fc-219">此時，電子郵件上的唯一驗證是使用[`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="994fc-219">At this point, the only validation on the email is with the [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="994fc-220">提交註冊之後，您會登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="994fc-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="994fc-221">稍後在本教學課程中，會更新程式碼，讓新使用者在驗證電子郵件之前無法登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="994fc-222">請注意，資料表的 [`EmailConfirmed`] 欄位是 `False`。</span><span class="sxs-lookup"><span data-stu-id="994fc-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="994fc-223">當應用程式傳送確認電子郵件時，您可能會想要在下一個步驟中再次使用此電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="994fc-224">以滑鼠右鍵按一下資料列，然後選取 [**刪除**]。</span><span class="sxs-lookup"><span data-stu-id="994fc-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="994fc-225">刪除電子郵件別名可讓您更輕鬆地進行下列步驟。</span><span class="sxs-lookup"><span data-stu-id="994fc-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="994fc-226">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="994fc-226">Require email confirmation</span></span>

<span data-ttu-id="994fc-227">最佳做法是確認新使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="994fc-228">電子郵件確認有助於確認他們不會模擬他人（也就是他們尚未向他人的電子郵件登錄）。</span><span class="sxs-lookup"><span data-stu-id="994fc-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="994fc-229">假設您有討論論壇，而您想要防止「yli@example.com」註冊為「nolivetto@contoso.com」。</span><span class="sxs-lookup"><span data-stu-id="994fc-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="994fc-230">若未確認電子郵件，「nolivetto@contoso.com」可能會從您的應用程式收到不必要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="994fc-231">假設使用者不小心註冊為「ylo@example.com」，而且未注意到 "yli" 拼錯。</span><span class="sxs-lookup"><span data-stu-id="994fc-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="994fc-232">因為應用程式沒有正確的電子郵件，所以無法使用密碼復原。</span><span class="sxs-lookup"><span data-stu-id="994fc-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="994fc-233">電子郵件確認為 bot 提供了有限的保護。</span><span class="sxs-lookup"><span data-stu-id="994fc-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="994fc-234">電子郵件確認無法保護具有許多電子郵件帳戶的惡意使用者。</span><span class="sxs-lookup"><span data-stu-id="994fc-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="994fc-235">您通常會想要防止新的使用者將任何資料張貼到您的網站，然後才會有已確認的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="994fc-236">更新 `Startup.ConfigureServices` 以要求已確認的電子郵件：</span><span class="sxs-lookup"><span data-stu-id="994fc-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="994fc-237">`config.SignIn.RequireConfirmedEmail = true;` 可防止已註冊的使用者登入，直到確認其電子郵件為止。</span><span class="sxs-lookup"><span data-stu-id="994fc-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="994fc-238">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="994fc-238">Configure email provider</span></span>

<span data-ttu-id="994fc-239">在本教學課程中， [SendGrid](https://sendgrid.com)是用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="994fc-240">您需要 SendGrid 帳戶和金鑰，才能傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="994fc-241">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="994fc-241">You can use other email providers.</span></span> <span data-ttu-id="994fc-242">ASP.NET Core 2.x 包含 `System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="994fc-243">建議您使用 SendGrid 或其他電子郵件服務來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="994fc-244">SMTP 很容易保護和設定正確。</span><span class="sxs-lookup"><span data-stu-id="994fc-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="994fc-245">建立類別來提取安全的電子郵件金鑰。</span><span class="sxs-lookup"><span data-stu-id="994fc-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="994fc-246">針對此範例，請建立*Services/AuthMessageSenderOptions .cs*：</span><span class="sxs-lookup"><span data-stu-id="994fc-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="994fc-247">設定 SendGrid 使用者秘密</span><span class="sxs-lookup"><span data-stu-id="994fc-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="994fc-248">使用 [[密碼管理員] 工具](xref:security/app-secrets)來設定 `SendGridUser` 和 `SendGridKey`。</span><span class="sxs-lookup"><span data-stu-id="994fc-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="994fc-249">例如：</span><span class="sxs-lookup"><span data-stu-id="994fc-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="994fc-250">在 Windows 中，秘密管理員會將金鑰/值組儲存在 `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` 目錄的*秘密 json*檔案中。</span><span class="sxs-lookup"><span data-stu-id="994fc-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="994fc-251">不會加密*秘密 json*檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="994fc-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="994fc-252">下列標記顯示秘密的*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="994fc-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="994fc-253">`SendGridKey` 值已移除。</span><span class="sxs-lookup"><span data-stu-id="994fc-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="994fc-254">如需詳細資訊，請參閱[選項模式](xref:fundamentals/configuration/options)[和設定](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="994fc-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="994fc-255">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="994fc-255">Install SendGrid</span></span>

<span data-ttu-id="994fc-256">本教學課程說明如何透過[SendGrid](https://sendgrid.com/)新增電子郵件通知，但您可以使用 SMTP 和其他機制來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="994fc-257">安裝 `SendGrid` NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="994fc-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="994fc-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="994fc-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="994fc-259">從 [套件管理員主控台] 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="994fc-259">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="994fc-260">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="994fc-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="994fc-261">從主控台，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="994fc-261">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="994fc-262">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)來註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="994fc-263">執行 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="994fc-263">Implement IEmailSender</span></span>

<span data-ttu-id="994fc-264">若要執行 `IEmailSender`，請使用與下列類似的程式碼來建立*服務/EmailSender* ：</span><span class="sxs-lookup"><span data-stu-id="994fc-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="994fc-265">設定啟動以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="994fc-265">Configure startup to support email</span></span>

<span data-ttu-id="994fc-266">將下列程式碼新增至*Startup.cs*檔案中的 `ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="994fc-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="994fc-267">將 `EmailSender` 新增為暫時性服務。</span><span class="sxs-lookup"><span data-stu-id="994fc-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="994fc-268">註冊 `AuthMessageSenderOptions` 設定實例。</span><span class="sxs-lookup"><span data-stu-id="994fc-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="994fc-269">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="994fc-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="994fc-270">此範本具有帳戶確認和密碼復原的程式碼。</span><span class="sxs-lookup"><span data-stu-id="994fc-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="994fc-271">尋找 [*區域/身分識別/頁面/帳戶/* 暫存器 .cs] 中的 `OnPostAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="994fc-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="994fc-272">將下列這行批註化，以防止新註冊的使用者自動登入：</span><span class="sxs-lookup"><span data-stu-id="994fc-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="994fc-273">會顯示完整的方法，並反白顯示已變更的行：</span><span class="sxs-lookup"><span data-stu-id="994fc-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="994fc-274">註冊、確認電子郵件和重設密碼</span><span class="sxs-lookup"><span data-stu-id="994fc-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="994fc-275">執行 web 應用程式，並測試帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="994fc-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="994fc-276">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="994fc-276">Run the app and register a new user</span></span>
* <span data-ttu-id="994fc-277">檢查您的電子郵件以取得帳戶確認連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="994fc-278">如果您沒有收到電子郵件，請參閱[Debug email](#debug) 。</span><span class="sxs-lookup"><span data-stu-id="994fc-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="994fc-279">按一下連結以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="994fc-280">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="994fc-281">登出。</span><span class="sxs-lookup"><span data-stu-id="994fc-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="994fc-282">查看 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="994fc-282">View the manage page</span></span>

<span data-ttu-id="994fc-283">在瀏覽器中選取您的使用者名稱，](accconfirm/_static/un.png) 使用者名稱 ![瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="994fc-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="994fc-284">[管理] 頁面隨即顯示，並選取 [**設定檔**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="994fc-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="994fc-285">此**電子郵件**會顯示一個核取方塊，指出已確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="994fc-286">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="994fc-286">Test password reset</span></span>

* <span data-ttu-id="994fc-287">如果您已登入，請選取 [**登出**]。</span><span class="sxs-lookup"><span data-stu-id="994fc-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="994fc-288">選取 [**登入**] 連結，然後選取 [**忘記密碼？** ] 連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="994fc-289">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="994fc-290">系統會傳送一封電子郵件，其中包含重設密碼的連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="994fc-291">檢查您的電子郵件，然後按一下連結以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="994fc-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="994fc-292">成功重設密碼之後，您就可以使用您的電子郵件和新密碼來登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="994fc-293">變更電子郵件和活動超時</span><span class="sxs-lookup"><span data-stu-id="994fc-293">Change email and activity timeout</span></span>

<span data-ttu-id="994fc-294">預設的無活動超時時間為14天。</span><span class="sxs-lookup"><span data-stu-id="994fc-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="994fc-295">下列程式碼會將非啟用時間設定為5天：</span><span class="sxs-lookup"><span data-stu-id="994fc-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="994fc-296">變更所有資料保護權杖壽命</span><span class="sxs-lookup"><span data-stu-id="994fc-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="994fc-297">下列程式碼會將所有資料保護權杖超時時間變更為3小時：</span><span class="sxs-lookup"><span data-stu-id="994fc-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="994fc-298">內建身分識別使用者權杖（請參閱[AspNetCore/src/Identity/Extensions. Core/src/TokenOptions](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ）具有[一天的超時時間](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="994fc-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="994fc-299">變更電子郵件權杖生命週期</span><span class="sxs-lookup"><span data-stu-id="994fc-299">Change the email token lifespan</span></span>

<span data-ttu-id="994fc-300">身分[識別使用者權杖](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)的預設權杖存留期為[一天](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="994fc-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="994fc-301">本節說明如何變更電子郵件權杖的生命週期。</span><span class="sxs-lookup"><span data-stu-id="994fc-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="994fc-302">新增自訂[DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和 <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>：</span><span class="sxs-lookup"><span data-stu-id="994fc-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="994fc-303">將自訂提供者新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="994fc-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="994fc-304">重新傳送電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="994fc-304">Resend email confirmation</span></span>

<span data-ttu-id="994fc-305">請參閱[這個 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5410)。</span><span class="sxs-lookup"><span data-stu-id="994fc-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="994fc-306">Debug 電子郵件</span><span class="sxs-lookup"><span data-stu-id="994fc-306">Debug email</span></span>

<span data-ttu-id="994fc-307">如果您無法讓電子郵件正常運作：</span><span class="sxs-lookup"><span data-stu-id="994fc-307">If you can't get email working:</span></span>

* <span data-ttu-id="994fc-308">在 `EmailSender.Execute` 中設定中斷點，以確認呼叫 `SendGridClient.SendEmailAsync`。</span><span class="sxs-lookup"><span data-stu-id="994fc-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="994fc-309">建立[主控台應用程式，以](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼將電子郵件傳送至 `EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="994fc-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="994fc-310">查看[電子郵件活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="994fc-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="994fc-311">檢查您的垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="994fc-311">Check your spam folder.</span></span>
* <span data-ttu-id="994fc-312">在不同的電子郵件提供者（Microsoft、Yahoo、Gmail 等）上嘗試另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="994fc-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="994fc-313">嘗試傳送至不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="994fc-314">**安全性最佳做法**是**不要**在測試和開發中使用生產秘密。</span><span class="sxs-lookup"><span data-stu-id="994fc-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="994fc-315">如果您將應用程式發行至 Azure，您可以在 Azure Web 應用程式入口網站中將 SendGrid 秘密設定為應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="994fc-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="994fc-316">設定系統已設定為從環境變數讀取金鑰。</span><span class="sxs-lookup"><span data-stu-id="994fc-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="994fc-317">合併社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="994fc-317">Combine social and local login accounts</span></span>

<span data-ttu-id="994fc-318">若要完成這一節，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="994fc-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="994fc-319">請參閱[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="994fc-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="994fc-320">您可以按一下電子郵件連結，以合併本機和社交帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="994fc-321">在下列順序中，會先將 "RickAndMSFT@gmail.com" 建立為本機登入;不過，您可以先將帳戶建立為社交登入，然後再新增本機登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式： RickAndMSFT@gmail.com 使用者已驗證](accconfirm/_static/rick.png)

<span data-ttu-id="994fc-323">按一下 [**管理**] 連結。</span><span class="sxs-lookup"><span data-stu-id="994fc-323">Click on the **Manage** link.</span></span> <span data-ttu-id="994fc-324">請注意與此帳戶相關聯的0外部（社交登入）。</span><span class="sxs-lookup"><span data-stu-id="994fc-324">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="994fc-326">按一下另一個登入服務的連結，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="994fc-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="994fc-327">在下圖中，Facebook 是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="994fc-327">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入視圖清單 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="994fc-329">已合併這兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="994fc-329">The two accounts have been combined.</span></span> <span data-ttu-id="994fc-330">您可以使用任一帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="994fc-330">You are able to sign in with either account.</span></span> <span data-ttu-id="994fc-331">您可能想要讓使用者新增本機帳戶，以防其社交登入驗證服務關閉，或更有可能失去其社交帳戶的存取權。</span><span class="sxs-lookup"><span data-stu-id="994fc-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="994fc-332">在網站有使用者之後啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="994fc-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="994fc-333">在網站上啟用帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="994fc-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="994fc-334">現有的使用者會被鎖定，因為他們的帳戶不會確認。</span><span class="sxs-lookup"><span data-stu-id="994fc-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="994fc-335">若要解決現有的使用者鎖定，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="994fc-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="994fc-336">更新資料庫，將所有現有的使用者標示為已確認。</span><span class="sxs-lookup"><span data-stu-id="994fc-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="994fc-337">確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="994fc-337">Confirm existing users.</span></span> <span data-ttu-id="994fc-338">例如，batch-傳送含有確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="994fc-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
