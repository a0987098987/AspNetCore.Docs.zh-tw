---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 802ba446af04df6a35ac73187ad693b8ec80c654
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814841"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="3d01d-103">帳戶確認和 ASP.NET Core 中的密碼復原</span><span class="sxs-lookup"><span data-stu-id="3d01d-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="3d01d-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Ponant](https://github.com/Ponant)，和[Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3d01d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="3d01d-105">本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3d01d-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="3d01d-106">本教學課程**不**開頭主題。</span><span class="sxs-lookup"><span data-stu-id="3d01d-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="3d01d-107">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="3d01d-107">You should be familiar with:</span></span>

* [<span data-ttu-id="3d01d-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d01d-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="3d01d-109">驗證</span><span class="sxs-lookup"><span data-stu-id="3d01d-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="3d01d-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3d01d-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="3d01d-111">請參閱[此 PDF 檔案](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 版本。</span><span class="sxs-lookup"><span data-stu-id="3d01d-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="3d01d-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="3d01d-112">Prerequisites</span></span>

[<span data-ttu-id="3d01d-113">.NET core 3.0 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3d01d-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="3d01d-114">建立及測試與驗證的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="3d01d-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="3d01d-115">執行下列命令來建立驗證的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3d01d-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="3d01d-116">執行應用程式，請選取**註冊**連結，並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="3d01d-117">註冊之後，將您重新導向至`/Identity/Account/RegisterConfirmation`其中包含模擬電子郵件確認連結的頁面：</span><span class="sxs-lookup"><span data-stu-id="3d01d-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="3d01d-118">選取`Click here to confirm your account`連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="3d01d-119">選取 **登入**連結，並使用相同的認證登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="3d01d-120">選取 `Hello YourEmail@provider.com!`連結，將您重新導向`/Identity/Account/Manage/PersonalData`頁面。</span><span class="sxs-lookup"><span data-stu-id="3d01d-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="3d01d-121">選取 **資**左邊的索引標籤，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="3d01d-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="3d01d-122">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="3d01d-122">Configure an email provider</span></span>

<span data-ttu-id="3d01d-123">在本教學課程中， [SendGrid](https://sendgrid.com)用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="3d01d-124">您需要的 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="3d01d-125">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-125">You can use other email providers.</span></span> <span data-ttu-id="3d01d-126">我們建議您可以使用 SendGrid 或另一個電子郵件服務傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="3d01d-127">SMTP 是難以保護，並正確設定。</span><span class="sxs-lookup"><span data-stu-id="3d01d-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="3d01d-128">建立類別來擷取安全的電子郵件的金鑰。</span><span class="sxs-lookup"><span data-stu-id="3d01d-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="3d01d-129">此範例中，建立*Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d01d-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="3d01d-130">設定 SendGrid 使用者祕密</span><span class="sxs-lookup"><span data-stu-id="3d01d-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="3d01d-131">設定`SendGridUser`並`SendGridKey`具有[secret manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="3d01d-132">例如：</span><span class="sxs-lookup"><span data-stu-id="3d01d-132">For example:</span></span>

```console
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="3d01d-133">在 Windows、 Secret Manager 儲存中的索引鍵/值組*secrets.json*檔案中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。</span><span class="sxs-lookup"><span data-stu-id="3d01d-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="3d01d-134">內容*secrets.json*檔案未加密。</span><span class="sxs-lookup"><span data-stu-id="3d01d-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="3d01d-135">下列標記示範*secrets.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="3d01d-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="3d01d-136">`SendGridKey`已移除值。</span><span class="sxs-lookup"><span data-stu-id="3d01d-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="3d01d-137">如需詳細資訊，請參閱 <<c0> [ 選項模式](xref:fundamentals/configuration/options)並[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="3d01d-138">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="3d01d-138">Install SendGrid</span></span>

<span data-ttu-id="3d01d-139">本教學課程示範如何將透過電子郵件通知[SendGrid](https://sendgrid.com/)，但您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="3d01d-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="3d01d-140">安裝`SendGrid`NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="3d01d-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d01d-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d01d-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3d01d-142">從 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3d01d-142">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3d01d-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3d01d-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3d01d-144">在主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3d01d-144">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="3d01d-145">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)報名免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="3d01d-146">實作 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="3d01d-146">Implement IEmailSender</span></span>

<span data-ttu-id="3d01d-147">實作`IEmailSender`，建立*Services/EmailSender.cs*與下列類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="3d01d-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="3d01d-148">設定啟動，以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="3d01d-148">Configure startup to support email</span></span>

<span data-ttu-id="3d01d-149">將下列程式碼加入`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="3d01d-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="3d01d-150">新增`EmailSender`為暫時性的服務。</span><span class="sxs-lookup"><span data-stu-id="3d01d-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="3d01d-151">註冊`AuthMessageSenderOptions`組態執行個體。</span><span class="sxs-lookup"><span data-stu-id="3d01d-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="3d01d-152">註冊、 確認電子郵件，以及重設密碼</span><span class="sxs-lookup"><span data-stu-id="3d01d-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="3d01d-153">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="3d01d-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="3d01d-154">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="3d01d-154">Run the app and register a new user</span></span>
* <span data-ttu-id="3d01d-155">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="3d01d-156">請參閱[偵錯電子郵件](#debug)如果您沒有收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="3d01d-157">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="3d01d-158">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="3d01d-159">登出。</span><span class="sxs-lookup"><span data-stu-id="3d01d-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="3d01d-160">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="3d01d-160">Test password reset</span></span>

* <span data-ttu-id="3d01d-161">如果您已登入，請選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="3d01d-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="3d01d-162">選取 **登入**連結，然後選取**忘記密碼？** 連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="3d01d-163">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="3d01d-164">會傳送具有重設密碼連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="3d01d-165">請檢查您的電子郵件，然後按一下 重設密碼連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="3d01d-166">已成功重設您的密碼之後，您可以使用您的電子郵件和新密碼登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="3d01d-167">變更電子郵件和活動的逾時</span><span class="sxs-lookup"><span data-stu-id="3d01d-167">Change email and activity timeout</span></span>

<span data-ttu-id="3d01d-168">預設閒置逾時為 14 天。</span><span class="sxs-lookup"><span data-stu-id="3d01d-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="3d01d-169">下列程式碼會設定為 5 天的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="3d01d-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="3d01d-170">變更所有資料保護權杖的期限</span><span class="sxs-lookup"><span data-stu-id="3d01d-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="3d01d-171">下列程式碼會變更為 3 小時內的所有資料保護權杖逾時期限：</span><span class="sxs-lookup"><span data-stu-id="3d01d-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="3d01d-172">內建在身分識別的使用者語彙基元 (請參閱[AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) 已[一天的逾時](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="3d01d-173">變更的電子郵件的權杖存留時間</span><span class="sxs-lookup"><span data-stu-id="3d01d-173">Change the email token lifespan</span></span>

<span data-ttu-id="3d01d-174">預設權杖存留時間[身分識別的使用者語彙基元](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)是[有一天](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="3d01d-175">本節說明如何變更的電子郵件的權杖存留時間。</span><span class="sxs-lookup"><span data-stu-id="3d01d-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="3d01d-176">新增自訂[DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和<xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="3d01d-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="3d01d-177">將自訂提供者新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="3d01d-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="3d01d-178">重新傳送電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="3d01d-178">Resend email confirmation</span></span>

<span data-ttu-id="3d01d-179">請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5410)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="3d01d-180">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="3d01d-180">Debug email</span></span>

<span data-ttu-id="3d01d-181">如果您無法收到電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="3d01d-181">If you can't get email working:</span></span>

* <span data-ttu-id="3d01d-182">在 設定中斷點`EmailSender.Execute`若要確認`SendGridClient.SendEmailAsync`呼叫。</span><span class="sxs-lookup"><span data-stu-id="3d01d-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="3d01d-183">建立[主控台應用程式以傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼以`EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="3d01d-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="3d01d-184">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="3d01d-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="3d01d-185">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="3d01d-185">Check your spam folder.</span></span>
* <span data-ttu-id="3d01d-186">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="3d01d-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="3d01d-187">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="3d01d-188">**安全性最佳作法**旨在**不**使用生產環境中開發和測試的祕密。</span><span class="sxs-lookup"><span data-stu-id="3d01d-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="3d01d-189">如果您將應用程式發佈至 Azure 時，請為 Azure Web 應用程式入口網站中的應用程式設定設定 SendGrid 祕密。</span><span class="sxs-lookup"><span data-stu-id="3d01d-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3d01d-190">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="3d01d-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3d01d-191">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="3d01d-191">Combine social and local login accounts</span></span>

<span data-ttu-id="3d01d-192">若要完成本節中，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="3d01d-193">請參閱[Facebook、 Google、 與外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="3d01d-194">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3d01d-195">依下列順序，"RickAndMSFT@gmail.com」 會先建立做為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後新增的本機登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="3d01d-197">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-197">Click on the **Manage** link.</span></span> <span data-ttu-id="3d01d-198">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="3d01d-198">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="3d01d-200">按一下連結至另一個登入服務並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="3d01d-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="3d01d-201">在下圖中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="3d01d-201">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入檢視，列出 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="3d01d-203">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-203">The two accounts have been combined.</span></span> <span data-ttu-id="3d01d-204">您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-204">You are able to sign in with either account.</span></span> <span data-ttu-id="3d01d-205">您可能想要新增本機帳戶，以免其社交登入驗證服務已關閉，或它們可能比較無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="3d01d-206">站台後為使用者啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="3d01d-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="3d01d-207">在網站上啟用的帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="3d01d-208">現有使用者遭到鎖定中，這是因為未確認他們的帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="3d01d-209">若要解決現有的使用者鎖定，請使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="3d01d-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="3d01d-210">更新將標示為正在確認所有現有的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="3d01d-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="3d01d-211">請確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-211">Confirm existing users.</span></span> <span data-ttu-id="3d01d-212">例如，批次傳送電子郵件以確認連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="3d01d-213">必要條件</span><span class="sxs-lookup"><span data-stu-id="3d01d-213">Prerequisites</span></span>

[<span data-ttu-id="3d01d-214">.NET core 2.2 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3d01d-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="3d01d-215">建立 web 應用程式，並建立身分識別的結構</span><span class="sxs-lookup"><span data-stu-id="3d01d-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="3d01d-216">執行下列命令來建立驗證的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3d01d-216">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="3d01d-217">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="3d01d-217">Test new user registration</span></span>

<span data-ttu-id="3d01d-218">執行應用程式，請選取**註冊**連結，並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="3d01d-219">此時，唯一的驗證電子郵件是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="3d01d-219">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="3d01d-220">提交註冊之後, 您登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="3d01d-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="3d01d-221">稍後在教學課程中，會將程式碼更新讓新的使用者無法登入，直到其電子郵件會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="3d01d-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="3d01d-222">請注意，資料表的`EmailConfirmed`欄位是`False`。</span><span class="sxs-lookup"><span data-stu-id="3d01d-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="3d01d-223">您可能想要這封電子郵件一次的下一個步驟時使用的應用程式會傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="3d01d-224">以滑鼠右鍵按一下資料列，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="3d01d-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="3d01d-225">刪除電子郵件別名更容易在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="3d01d-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="3d01d-226">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="3d01d-226">Require email confirmation</span></span>

<span data-ttu-id="3d01d-227">您最好確認新的使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="3d01d-228">電子郵件確認可協助您確認它們無法模擬其他人 （也就是尚未註冊使用其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="3d01d-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="3d01d-229">假設您有討論論壇，而且您想要防止 「yli@example.com"中註冊為 「nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="3d01d-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="3d01d-230">而不需要電子郵件確認"nolivetto@contoso.com」 無法從您的應用程式收到不想要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="3d01d-231">假設使用者不小心註冊為 「ylo@example.com"並還沒有發現..."yli 」 的拼字錯誤。</span><span class="sxs-lookup"><span data-stu-id="3d01d-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="3d01d-232">他們將無法使用密碼復原，因為應用程式沒有正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="3d01d-233">電子郵件確認 bot 提供有限的保護。</span><span class="sxs-lookup"><span data-stu-id="3d01d-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="3d01d-234">電子郵件確認不會提供保護，防範惡意使用者與許多電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="3d01d-235">您通常想要防止新使用者之前確認電子郵件張貼到您的網站的任何資料。</span><span class="sxs-lookup"><span data-stu-id="3d01d-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="3d01d-236">更新`Startup.ConfigureServices`要求確認電子郵件：</span><span class="sxs-lookup"><span data-stu-id="3d01d-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="3d01d-237">`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認其電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="3d01d-238">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="3d01d-238">Configure email provider</span></span>

<span data-ttu-id="3d01d-239">在本教學課程中， [SendGrid](https://sendgrid.com)用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="3d01d-240">您需要的 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="3d01d-241">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-241">You can use other email providers.</span></span> <span data-ttu-id="3d01d-242">ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="3d01d-243">我們建議您可以使用 SendGrid 或另一個電子郵件服務傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="3d01d-244">SMTP 是難以保護，並正確設定。</span><span class="sxs-lookup"><span data-stu-id="3d01d-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="3d01d-245">建立類別來擷取安全的電子郵件的金鑰。</span><span class="sxs-lookup"><span data-stu-id="3d01d-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="3d01d-246">此範例中，建立*Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d01d-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="3d01d-247">設定 SendGrid 使用者祕密</span><span class="sxs-lookup"><span data-stu-id="3d01d-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="3d01d-248">設定`SendGridUser`並`SendGridKey`具有[secret manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="3d01d-249">例如：</span><span class="sxs-lookup"><span data-stu-id="3d01d-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="3d01d-250">在 Windows、 Secret Manager 儲存中的索引鍵/值組*secrets.json*檔案中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。</span><span class="sxs-lookup"><span data-stu-id="3d01d-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="3d01d-251">內容*secrets.json*檔案未加密。</span><span class="sxs-lookup"><span data-stu-id="3d01d-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="3d01d-252">下列標記示範*secrets.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="3d01d-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="3d01d-253">`SendGridKey`已移除值。</span><span class="sxs-lookup"><span data-stu-id="3d01d-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="3d01d-254">如需詳細資訊，請參閱 <<c0> [ 選項模式](xref:fundamentals/configuration/options)並[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="3d01d-255">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="3d01d-255">Install SendGrid</span></span>

<span data-ttu-id="3d01d-256">本教學課程示範如何將透過電子郵件通知[SendGrid](https://sendgrid.com/)，但您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="3d01d-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="3d01d-257">安裝`SendGrid`NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="3d01d-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d01d-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d01d-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3d01d-259">從 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3d01d-259">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3d01d-260">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3d01d-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3d01d-261">在主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3d01d-261">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="3d01d-262">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)報名免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="3d01d-263">實作 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="3d01d-263">Implement IEmailSender</span></span>

<span data-ttu-id="3d01d-264">實作`IEmailSender`，建立*Services/EmailSender.cs*與下列類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="3d01d-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="3d01d-265">設定啟動，以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="3d01d-265">Configure startup to support email</span></span>

<span data-ttu-id="3d01d-266">將下列程式碼加入`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="3d01d-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="3d01d-267">新增`EmailSender`為暫時性的服務。</span><span class="sxs-lookup"><span data-stu-id="3d01d-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="3d01d-268">註冊`AuthMessageSenderOptions`組態執行個體。</span><span class="sxs-lookup"><span data-stu-id="3d01d-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="3d01d-269">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="3d01d-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="3d01d-270">範本會將程式碼進行帳戶確認和密碼復原。</span><span class="sxs-lookup"><span data-stu-id="3d01d-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="3d01d-271">尋找`OnPostAsync`方法中的*Areas/Identity/Pages/Account/Register.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="3d01d-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="3d01d-272">防止新註冊的使用者自動登入註解下面這一行：</span><span class="sxs-lookup"><span data-stu-id="3d01d-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3d01d-273">使用已變更反白顯示的列，會顯示完整的方法：</span><span class="sxs-lookup"><span data-stu-id="3d01d-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="3d01d-274">註冊、 確認電子郵件，以及重設密碼</span><span class="sxs-lookup"><span data-stu-id="3d01d-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="3d01d-275">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="3d01d-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="3d01d-276">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="3d01d-276">Run the app and register a new user</span></span>
* <span data-ttu-id="3d01d-277">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="3d01d-278">請參閱[偵錯電子郵件](#debug)如果您沒有收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="3d01d-279">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="3d01d-280">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="3d01d-281">登出。</span><span class="sxs-lookup"><span data-stu-id="3d01d-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="3d01d-282">檢視 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="3d01d-282">View the manage page</span></span>

<span data-ttu-id="3d01d-283">在瀏覽器中，選取您的使用者名稱：![瀏覽器視窗中的使用使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="3d01d-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="3d01d-284">[管理] 頁面會顯示**設定檔**選取的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="3d01d-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="3d01d-285">**電子郵件**顯示核取方塊，指出電子郵件已確認。</span><span class="sxs-lookup"><span data-stu-id="3d01d-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="3d01d-286">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="3d01d-286">Test password reset</span></span>

* <span data-ttu-id="3d01d-287">如果您已登入，請選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="3d01d-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="3d01d-288">選取 **登入**連結，然後選取**忘記密碼？** 連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="3d01d-289">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="3d01d-290">會傳送具有重設密碼連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3d01d-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="3d01d-291">請檢查您的電子郵件，然後按一下 重設密碼連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="3d01d-292">已成功重設您的密碼之後，您可以使用您的電子郵件和新密碼登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="3d01d-293">變更電子郵件和活動的逾時</span><span class="sxs-lookup"><span data-stu-id="3d01d-293">Change email and activity timeout</span></span>

<span data-ttu-id="3d01d-294">預設閒置逾時為 14 天。</span><span class="sxs-lookup"><span data-stu-id="3d01d-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="3d01d-295">下列程式碼會設定為 5 天的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="3d01d-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="3d01d-296">變更所有資料保護權杖的期限</span><span class="sxs-lookup"><span data-stu-id="3d01d-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="3d01d-297">下列程式碼會變更為 3 小時內的所有資料保護權杖逾時期限：</span><span class="sxs-lookup"><span data-stu-id="3d01d-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="3d01d-298">內建在身分識別的使用者語彙基元 (請參閱[AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) 已[一天的逾時](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="3d01d-299">變更的電子郵件的權杖存留時間</span><span class="sxs-lookup"><span data-stu-id="3d01d-299">Change the email token lifespan</span></span>

<span data-ttu-id="3d01d-300">預設權杖存留時間[身分識別的使用者語彙基元](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)是[有一天](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="3d01d-301">本節說明如何變更的電子郵件的權杖存留時間。</span><span class="sxs-lookup"><span data-stu-id="3d01d-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="3d01d-302">新增自訂[DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和<xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="3d01d-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="3d01d-303">將自訂提供者新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="3d01d-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="3d01d-304">重新傳送電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="3d01d-304">Resend email confirmation</span></span>

<span data-ttu-id="3d01d-305">請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5410)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="3d01d-306">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="3d01d-306">Debug email</span></span>

<span data-ttu-id="3d01d-307">如果您無法收到電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="3d01d-307">If you can't get email working:</span></span>

* <span data-ttu-id="3d01d-308">在 設定中斷點`EmailSender.Execute`若要確認`SendGridClient.SendEmailAsync`呼叫。</span><span class="sxs-lookup"><span data-stu-id="3d01d-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="3d01d-309">建立[主控台應用程式以傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼以`EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="3d01d-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="3d01d-310">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="3d01d-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="3d01d-311">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="3d01d-311">Check your spam folder.</span></span>
* <span data-ttu-id="3d01d-312">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="3d01d-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="3d01d-313">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="3d01d-314">**安全性最佳作法**旨在**不**使用生產環境中開發和測試的祕密。</span><span class="sxs-lookup"><span data-stu-id="3d01d-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="3d01d-315">如果您將應用程式發佈至 Azure 時，您可以為 Azure Web 應用程式入口網站中的應用程式設定來設定 SendGrid 祕密。</span><span class="sxs-lookup"><span data-stu-id="3d01d-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3d01d-316">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="3d01d-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3d01d-317">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="3d01d-317">Combine social and local login accounts</span></span>

<span data-ttu-id="3d01d-318">若要完成本節中，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="3d01d-319">請參閱[Facebook、 Google、 與外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="3d01d-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="3d01d-320">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3d01d-321">依下列順序，"RickAndMSFT@gmail.com」 會先建立做為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後新增的本機登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="3d01d-323">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-323">Click on the **Manage** link.</span></span> <span data-ttu-id="3d01d-324">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="3d01d-324">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="3d01d-326">按一下連結至另一個登入服務並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="3d01d-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="3d01d-327">在下圖中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="3d01d-327">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入檢視，列出 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="3d01d-329">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-329">The two accounts have been combined.</span></span> <span data-ttu-id="3d01d-330">您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="3d01d-330">You are able to sign in with either account.</span></span> <span data-ttu-id="3d01d-331">您可能想要新增本機帳戶，以免其社交登入驗證服務已關閉，或它們可能比較無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="3d01d-332">站台後為使用者啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="3d01d-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="3d01d-333">在網站上啟用的帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="3d01d-334">現有使用者遭到鎖定中，這是因為未確認他們的帳戶。</span><span class="sxs-lookup"><span data-stu-id="3d01d-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="3d01d-335">若要解決現有的使用者鎖定，請使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="3d01d-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="3d01d-336">更新將標示為正在確認所有現有的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="3d01d-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="3d01d-337">請確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="3d01d-337">Confirm existing users.</span></span> <span data-ttu-id="3d01d-338">例如，批次傳送電子郵件以確認連結。</span><span class="sxs-lookup"><span data-stu-id="3d01d-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
