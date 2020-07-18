---
title: ASP.NET Core 中的帳戶確認和密碼復原
author: rick-anderson
description: 瞭解如何使用電子郵件確認和密碼重設來建立 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 03/11/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/accconfirm
ms.openlocfilehash: 1156ddd2921afbfeccaf077ca29d267f8b1e844a
ms.sourcegitcommit: 3544941682869734ea0113e24e02ed0ec9e1a9ec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2020
ms.locfileid: "86464549"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="72e06-103">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="72e06-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="72e06-104">依[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Ponant](https://github.com/Ponant)和[Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="72e06-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="72e06-105">本教學課程說明如何使用電子郵件確認和密碼重設來建立 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72e06-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="72e06-106">本教學課程**不**是開始主題。</span><span class="sxs-lookup"><span data-stu-id="72e06-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="72e06-107">您應該已熟悉：</span><span class="sxs-lookup"><span data-stu-id="72e06-107">You should be familiar with:</span></span>

* [<span data-ttu-id="72e06-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72e06-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="72e06-109">驗證</span><span class="sxs-lookup"><span data-stu-id="72e06-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="72e06-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="72e06-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="72e06-111">先決條件</span><span class="sxs-lookup"><span data-stu-id="72e06-111">Prerequisites</span></span>

[<span data-ttu-id="72e06-112">.NET Core 3.0 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="72e06-112">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="72e06-113">使用驗證來建立及測試 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="72e06-113">Create and test a web app with authentication</span></span>

<span data-ttu-id="72e06-114">執行下列命令來建立具有驗證的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72e06-114">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="72e06-115">執行應用程式，選取 [**註冊**] 連結，然後註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="72e06-115">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="72e06-116">註冊之後，系統會將您重新導向至 [到] `/Identity/Account/RegisterConfirmation` 頁面，其中包含用來模擬電子郵件確認的連結：</span><span class="sxs-lookup"><span data-stu-id="72e06-116">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="72e06-117">選取 `Click here to confirm your account` 連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-117">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="72e06-118">選取 [**登**入] 連結，並使用相同的認證進行登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-118">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="72e06-119">選取 `Hello YourEmail@provider.com!` 連結，將您重新導向至 `/Identity/Account/Manage/PersonalData` 頁面。</span><span class="sxs-lookup"><span data-stu-id="72e06-119">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="72e06-120">選取左側的 [**個人資料**] 索引標籤，然後選取 [**刪除**]。</span><span class="sxs-lookup"><span data-stu-id="72e06-120">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="72e06-121">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="72e06-121">Configure an email provider</span></span>

<span data-ttu-id="72e06-122">在本教學課程中， [SendGrid](https://sendgrid.com)是用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-122">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="72e06-123">您需要 SendGrid 帳戶和金鑰，才能傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-123">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="72e06-124">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="72e06-124">You can use other email providers.</span></span> <span data-ttu-id="72e06-125">建議您使用 SendGrid 或其他電子郵件服務來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-125">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="72e06-126">SMTP 很容易保護和設定正確。</span><span class="sxs-lookup"><span data-stu-id="72e06-126">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="72e06-127">SendGrid 帳戶可能需要[新增寄件者](https://sendgrid.com/docs/ui/sending-email/senders/)。</span><span class="sxs-lookup"><span data-stu-id="72e06-127">The SendGrid account may require [adding a Sender](https://sendgrid.com/docs/ui/sending-email/senders/).</span></span>

<span data-ttu-id="72e06-128">建立類別來提取安全的電子郵件金鑰。</span><span class="sxs-lookup"><span data-stu-id="72e06-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="72e06-129">針對此範例，請建立*Services/AuthMessageSenderOptions .cs*：</span><span class="sxs-lookup"><span data-stu-id="72e06-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="72e06-130">設定 SendGrid 使用者秘密</span><span class="sxs-lookup"><span data-stu-id="72e06-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="72e06-131">`SendGridUser` `SendGridKey` 使用[秘密管理員工具](xref:security/app-secrets)設定和。</span><span class="sxs-lookup"><span data-stu-id="72e06-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="72e06-132">例如：</span><span class="sxs-lookup"><span data-stu-id="72e06-132">For example:</span></span>

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="72e06-133">在 Windows 中，秘密管理員會將索引鍵/值組儲存在目錄中的檔案*secrets.js* `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` 。</span><span class="sxs-lookup"><span data-stu-id="72e06-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="72e06-134">檔案*上secrets.js*的內容不會加密。</span><span class="sxs-lookup"><span data-stu-id="72e06-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="72e06-135">下列標記顯示檔案*上的secrets.js* 。</span><span class="sxs-lookup"><span data-stu-id="72e06-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="72e06-136">已 `SendGridKey` 移除此值。</span><span class="sxs-lookup"><span data-stu-id="72e06-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="72e06-137">如需詳細資訊，請參閱[選項模式](xref:fundamentals/configuration/options)[和設定](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="72e06-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="72e06-138">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="72e06-138">Install SendGrid</span></span>

<span data-ttu-id="72e06-139">本教學課程說明如何透過[SendGrid](https://sendgrid.com/)新增電子郵件通知，但您可以使用 SMTP 和其他機制來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="72e06-140">安裝 `SendGrid` NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="72e06-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="72e06-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72e06-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="72e06-142">從 [套件管理員主控台] 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="72e06-142">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-cli"></a>[<span data-ttu-id="72e06-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="72e06-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="72e06-144">從主控台，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="72e06-144">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="72e06-145">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)來註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="72e06-146">執行 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="72e06-146">Implement IEmailSender</span></span>

<span data-ttu-id="72e06-147">若要執行 `IEmailSender` ，請使用與下列類似的程式碼來建立*服務/EmailSender* ：</span><span class="sxs-lookup"><span data-stu-id="72e06-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="72e06-148">設定啟動以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="72e06-148">Configure startup to support email</span></span>

<span data-ttu-id="72e06-149">將下列程式碼新增至 `ConfigureServices` *Startup.cs*檔案中的方法：</span><span class="sxs-lookup"><span data-stu-id="72e06-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="72e06-150">新增 `EmailSender` 為暫時性服務。</span><span class="sxs-lookup"><span data-stu-id="72e06-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="72e06-151">註冊設定 `AuthMessageSenderOptions` 實例。</span><span class="sxs-lookup"><span data-stu-id="72e06-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="scaffold-registerconfirmation"></a><span data-ttu-id="72e06-152">Scaffold RegisterConfirmation</span><span class="sxs-lookup"><span data-stu-id="72e06-152">Scaffold RegisterConfirmation</span></span>

<span data-ttu-id="72e06-153">遵循[Scaffold Identity ](xref:security/authentication/scaffold-identity)和 Scaffold 的指示 `RegisterConfirmation` 。</span><span class="sxs-lookup"><span data-stu-id="72e06-153">Follow the instructions for [Scaffold Identity](xref:security/authentication/scaffold-identity) and scaffold `RegisterConfirmation`.</span></span>

<!-- .NET 5 fixes this, see
https://github.com/dotnet/aspnetcore/blob/master/src/Identity/UI/src/Areas/Identity/Pages/V4/Account/RegisterConfirmation.cshtml.cs#L74-L77
-->

[!INCLUDE[](~/includes/disableVer.md)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="72e06-154">註冊、確認電子郵件和重設密碼</span><span class="sxs-lookup"><span data-stu-id="72e06-154">Register, confirm email, and reset password</span></span>

<span data-ttu-id="72e06-155">執行 web 應用程式，並測試帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="72e06-155">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="72e06-156">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="72e06-156">Run the app and register a new user</span></span>
* <span data-ttu-id="72e06-157">檢查您的電子郵件以取得帳戶確認連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-157">Check your email for the account confirmation link.</span></span> <span data-ttu-id="72e06-158">如果您沒有收到電子郵件，請參閱[Debug email](#debug) 。</span><span class="sxs-lookup"><span data-stu-id="72e06-158">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="72e06-159">按一下連結以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-159">Click the link to confirm your email.</span></span>
* <span data-ttu-id="72e06-160">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-160">Sign in with your email and password.</span></span>
* <span data-ttu-id="72e06-161">登出。</span><span class="sxs-lookup"><span data-stu-id="72e06-161">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="72e06-162">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="72e06-162">Test password reset</span></span>

* <span data-ttu-id="72e06-163">如果您已登入，請選取 [**登出**]。</span><span class="sxs-lookup"><span data-stu-id="72e06-163">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="72e06-164">選取 [**登入**] 連結，然後選取 [**忘記密碼？** ] 連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-164">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="72e06-165">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-165">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="72e06-166">系統會傳送一封電子郵件，其中包含重設密碼的連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-166">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="72e06-167">檢查您的電子郵件，然後按一下連結以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="72e06-167">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="72e06-168">成功重設密碼之後，您就可以使用您的電子郵件和新密碼來登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-168">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="72e06-169">變更電子郵件和活動超時</span><span class="sxs-lookup"><span data-stu-id="72e06-169">Change email and activity timeout</span></span>

<span data-ttu-id="72e06-170">預設的無活動超時時間為14天。</span><span class="sxs-lookup"><span data-stu-id="72e06-170">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="72e06-171">下列程式碼會將非啟用時間設定為5天：</span><span class="sxs-lookup"><span data-stu-id="72e06-171">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="72e06-172">變更所有資料保護權杖壽命</span><span class="sxs-lookup"><span data-stu-id="72e06-172">Change all data protection token lifespans</span></span>

<span data-ttu-id="72e06-173">下列程式碼會將所有資料保護權杖超時時間變更為3小時：</span><span class="sxs-lookup"><span data-stu-id="72e06-173">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="72e06-174">內建的 Identity 使用者權杖（請參閱[AspNetCore/src/ Identity /Extensions.Core/src/TokenOptions.cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ）有[一天的超時時間](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="72e06-174">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="72e06-175">變更電子郵件權杖生命週期</span><span class="sxs-lookup"><span data-stu-id="72e06-175">Change the email token lifespan</span></span>

<span data-ttu-id="72e06-176">[ Identity 使用者權杖](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)的預設權杖存留期為[一天](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="72e06-176">The default token lifespan of [the Identity user tokens](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="72e06-177">本節說明如何變更電子郵件權杖的生命週期。</span><span class="sxs-lookup"><span data-stu-id="72e06-177">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="72e06-178">新增自訂[DataProtectorTokenProvider \<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和 <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions> ：</span><span class="sxs-lookup"><span data-stu-id="72e06-178">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="72e06-179">將自訂提供者新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="72e06-179">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="72e06-180">重新傳送電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="72e06-180">Resend email confirmation</span></span>

<span data-ttu-id="72e06-181">請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/5410)。</span><span class="sxs-lookup"><span data-stu-id="72e06-181">See [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="72e06-182">Debug 電子郵件</span><span class="sxs-lookup"><span data-stu-id="72e06-182">Debug email</span></span>

<span data-ttu-id="72e06-183">如果您無法讓電子郵件正常運作：</span><span class="sxs-lookup"><span data-stu-id="72e06-183">If you can't get email working:</span></span>

* <span data-ttu-id="72e06-184">在中設定中斷點 `EmailSender.Execute` ，以驗證 `SendGridClient.SendEmailAsync` 是否已呼叫。</span><span class="sxs-lookup"><span data-stu-id="72e06-184">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="72e06-185">建立[主控台應用程式，以](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼將電子郵件傳送至 `EmailSender.Execute` 。</span><span class="sxs-lookup"><span data-stu-id="72e06-185">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="72e06-186">查看[電子郵件活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="72e06-186">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="72e06-187">檢查您的垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="72e06-187">Check your spam folder.</span></span>
* <span data-ttu-id="72e06-188">在不同的電子郵件提供者（Microsoft、Yahoo、Gmail 等）上嘗試另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="72e06-188">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="72e06-189">嘗試傳送至不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-189">Try sending to different email accounts.</span></span>

<span data-ttu-id="72e06-190">**安全性最佳做法**是**不要**在測試和開發中使用生產秘密。</span><span class="sxs-lookup"><span data-stu-id="72e06-190">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="72e06-191">如果您將應用程式發佈至 Azure，請在 Azure Web 應用程式入口網站中將 SendGrid 秘密設定為應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="72e06-191">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="72e06-192">設定系統已設定為從環境變數讀取金鑰。</span><span class="sxs-lookup"><span data-stu-id="72e06-192">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="72e06-193">合併社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="72e06-193">Combine social and local login accounts</span></span>

<span data-ttu-id="72e06-194">若要完成這一節，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="72e06-194">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="72e06-195">請參閱[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="72e06-195">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="72e06-196">您可以按一下電子郵件連結，以合併本機和社交帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-196">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="72e06-197">在下列順序中， RickAndMSFT@gmail.com 會先將 "" 建立為本機登入; 不過，您可以先將帳戶建立為社交登入，然後再新增本機登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-197">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式： RickAndMSFT@gmail.com 使用者驗證](accconfirm/_static/rick.png)

<span data-ttu-id="72e06-199">按一下 [**管理**] 連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-199">Click on the **Manage** link.</span></span> <span data-ttu-id="72e06-200">請注意與此帳戶相關聯的0外部（社交登入）。</span><span class="sxs-lookup"><span data-stu-id="72e06-200">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="72e06-202">按一下另一個登入服務的連結，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="72e06-202">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="72e06-203">在下圖中，Facebook 是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="72e06-203">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入視圖清單 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="72e06-205">已合併這兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-205">The two accounts have been combined.</span></span> <span data-ttu-id="72e06-206">您可以使用任一帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-206">You are able to sign in with either account.</span></span> <span data-ttu-id="72e06-207">您可能想要讓使用者新增本機帳戶，以防其社交登入驗證服務關閉，或更有可能失去其社交帳戶的存取權。</span><span class="sxs-lookup"><span data-stu-id="72e06-207">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="72e06-208">在網站有使用者之後啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="72e06-208">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="72e06-209">在網站上啟用帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="72e06-209">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="72e06-210">現有的使用者會被鎖定，因為他們的帳戶不會確認。</span><span class="sxs-lookup"><span data-stu-id="72e06-210">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="72e06-211">若要解決現有的使用者鎖定，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="72e06-211">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="72e06-212">更新資料庫，將所有現有的使用者標示為已確認。</span><span class="sxs-lookup"><span data-stu-id="72e06-212">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="72e06-213">確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="72e06-213">Confirm existing users.</span></span> <span data-ttu-id="72e06-214">例如，batch-傳送含有確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-214">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="72e06-215">先決條件</span><span class="sxs-lookup"><span data-stu-id="72e06-215">Prerequisites</span></span>

[<span data-ttu-id="72e06-216">.NET Core 2.2 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="72e06-216">.NET Core 2.2 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="72e06-217">建立 web 應用程式和 scaffoldIdentity</span><span class="sxs-lookup"><span data-stu-id="72e06-217">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="72e06-218">執行下列命令來建立具有驗證的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72e06-218">Run the following commands to create a web app with authentication.</span></span>

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

> [!NOTE]
> <span data-ttu-id="72e06-219">如果 <xref:Microsoft.AspNetCore.Identity.PasswordOptions> 在中設定 `Startup.ConfigureServices` ，則 scaffold 頁面中的屬性可能需要[ `[StringLength]` 屬性](xref:System.ComponentModel.DataAnnotations.StringLengthAttribute)設定 `Password` Identity 。</span><span class="sxs-lookup"><span data-stu-id="72e06-219">If <xref:Microsoft.AspNetCore.Identity.PasswordOptions> are configured in `Startup.ConfigureServices`, [`[StringLength]` attribute](xref:System.ComponentModel.DataAnnotations.StringLengthAttribute) configuration might be required for the `Password` property in scaffolded Identity pages.</span></span> <span data-ttu-id="72e06-220">在檔案中的樣板 `InputModel` `Password` 之後，會在檔案中找到屬性 `Areas/Identity/Pages/Account/Register.cshtml.cs` Identity 。</span><span class="sxs-lookup"><span data-stu-id="72e06-220">An `InputModel` `Password` property is found in the `Areas/Identity/Pages/Account/Register.cshtml.cs` file after scaffolding Identity.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="72e06-221">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="72e06-221">Test new user registration</span></span>

<span data-ttu-id="72e06-222">執行應用程式，選取 [**註冊**] 連結，然後註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="72e06-222">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="72e06-223">此時，電子郵件上的唯一驗證是使用 [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) 屬性。</span><span class="sxs-lookup"><span data-stu-id="72e06-223">At this point, the only validation on the email is with the [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="72e06-224">提交註冊之後，您會登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="72e06-224">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="72e06-225">稍後在本教學課程中，會更新程式碼，讓新使用者在驗證電子郵件之前無法登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-225">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="72e06-226">請注意，資料表的 `EmailConfirmed` 欄位是 `False` 。</span><span class="sxs-lookup"><span data-stu-id="72e06-226">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="72e06-227">當應用程式傳送確認電子郵件時，您可能會想要在下一個步驟中再次使用此電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-227">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="72e06-228">以滑鼠右鍵按一下資料列，然後選取 [**刪除**]。</span><span class="sxs-lookup"><span data-stu-id="72e06-228">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="72e06-229">刪除電子郵件別名可讓您更輕鬆地進行下列步驟。</span><span class="sxs-lookup"><span data-stu-id="72e06-229">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="72e06-230">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="72e06-230">Require email confirmation</span></span>

<span data-ttu-id="72e06-231">最佳做法是確認新使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-231">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="72e06-232">電子郵件確認有助於確認他們不會模擬他人（也就是他們尚未向他人的電子郵件登錄）。</span><span class="sxs-lookup"><span data-stu-id="72e06-232">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="72e06-233">假設您有討論論壇，而您想要防止「」 yli@example.com 註冊為「」 nolivetto@contoso.com 。</span><span class="sxs-lookup"><span data-stu-id="72e06-233">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="72e06-234">若未確認電子郵件，" nolivetto@contoso.com " 可能會從您的應用程式收到不必要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-234">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="72e06-235">假設使用者不小心註冊為 " ylo@example.com "，且未注意到 "yli" 拼錯的錯誤。</span><span class="sxs-lookup"><span data-stu-id="72e06-235">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="72e06-236">因為應用程式沒有正確的電子郵件，所以無法使用密碼復原。</span><span class="sxs-lookup"><span data-stu-id="72e06-236">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="72e06-237">電子郵件確認為 bot 提供了有限的保護。</span><span class="sxs-lookup"><span data-stu-id="72e06-237">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="72e06-238">電子郵件確認無法保護具有許多電子郵件帳戶的惡意使用者。</span><span class="sxs-lookup"><span data-stu-id="72e06-238">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="72e06-239">您通常會想要防止新的使用者將任何資料張貼到您的網站，然後才會有已確認的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-239">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="72e06-240">更新 `Startup.ConfigureServices` 以要求已確認的電子郵件：</span><span class="sxs-lookup"><span data-stu-id="72e06-240">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="72e06-241">`config.SignIn.RequireConfirmedEmail = true;`防止已註冊的使用者登入，直到確認其電子郵件為止。</span><span class="sxs-lookup"><span data-stu-id="72e06-241">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="72e06-242">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="72e06-242">Configure email provider</span></span>

<span data-ttu-id="72e06-243">在本教學課程中， [SendGrid](https://sendgrid.com)是用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-243">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="72e06-244">您需要 SendGrid 帳戶和金鑰，才能傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-244">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="72e06-245">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="72e06-245">You can use other email providers.</span></span> <span data-ttu-id="72e06-246">ASP.NET Core 2.x 包含，可 `System.Net.Mail` 讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-246">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="72e06-247">建議您使用 SendGrid 或其他電子郵件服務來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-247">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="72e06-248">SMTP 很容易保護和設定正確。</span><span class="sxs-lookup"><span data-stu-id="72e06-248">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="72e06-249">建立類別來提取安全的電子郵件金鑰。</span><span class="sxs-lookup"><span data-stu-id="72e06-249">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="72e06-250">針對此範例，請建立*Services/AuthMessageSenderOptions .cs*：</span><span class="sxs-lookup"><span data-stu-id="72e06-250">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="72e06-251">設定 SendGrid 使用者秘密</span><span class="sxs-lookup"><span data-stu-id="72e06-251">Configure SendGrid user secrets</span></span>

<span data-ttu-id="72e06-252">`SendGridUser` `SendGridKey` 使用[秘密管理員工具](xref:security/app-secrets)設定和。</span><span class="sxs-lookup"><span data-stu-id="72e06-252">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="72e06-253">例如：</span><span class="sxs-lookup"><span data-stu-id="72e06-253">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="72e06-254">在 Windows 中，秘密管理員會將索引鍵/值組儲存在目錄中的檔案*secrets.js* `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` 。</span><span class="sxs-lookup"><span data-stu-id="72e06-254">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="72e06-255">檔案*上secrets.js*的內容不會加密。</span><span class="sxs-lookup"><span data-stu-id="72e06-255">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="72e06-256">下列標記顯示檔案*上的secrets.js* 。</span><span class="sxs-lookup"><span data-stu-id="72e06-256">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="72e06-257">已 `SendGridKey` 移除此值。</span><span class="sxs-lookup"><span data-stu-id="72e06-257">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="72e06-258">如需詳細資訊，請參閱[選項模式](xref:fundamentals/configuration/options)[和設定](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="72e06-258">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="72e06-259">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="72e06-259">Install SendGrid</span></span>

<span data-ttu-id="72e06-260">本教學課程說明如何透過[SendGrid](https://sendgrid.com/)新增電子郵件通知，但您可以使用 SMTP 和其他機制來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-260">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="72e06-261">安裝 `SendGrid` NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="72e06-261">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="72e06-262">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72e06-262">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="72e06-263">從 [套件管理員主控台] 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="72e06-263">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-cli"></a>[<span data-ttu-id="72e06-264">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="72e06-264">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="72e06-265">從主控台，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="72e06-265">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="72e06-266">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)來註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-266">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="72e06-267">執行 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="72e06-267">Implement IEmailSender</span></span>

<span data-ttu-id="72e06-268">若要執行 `IEmailSender` ，請使用與下列類似的程式碼來建立*服務/EmailSender* ：</span><span class="sxs-lookup"><span data-stu-id="72e06-268">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="72e06-269">設定啟動以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="72e06-269">Configure startup to support email</span></span>

<span data-ttu-id="72e06-270">將下列程式碼新增至 `ConfigureServices` *Startup.cs*檔案中的方法：</span><span class="sxs-lookup"><span data-stu-id="72e06-270">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="72e06-271">新增 `EmailSender` 為暫時性服務。</span><span class="sxs-lookup"><span data-stu-id="72e06-271">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="72e06-272">註冊設定 `AuthMessageSenderOptions` 實例。</span><span class="sxs-lookup"><span data-stu-id="72e06-272">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="72e06-273">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="72e06-273">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="72e06-274">此範本具有帳戶確認和密碼復原的程式碼。</span><span class="sxs-lookup"><span data-stu-id="72e06-274">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="72e06-275">在 [ `OnPostAsync` *區域/ Identity /Pages/Account/Register.cshtml.cs*] 中尋找方法。</span><span class="sxs-lookup"><span data-stu-id="72e06-275">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="72e06-276">將下列這行批註化，以防止新註冊的使用者自動登入：</span><span class="sxs-lookup"><span data-stu-id="72e06-276">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="72e06-277">會顯示完整的方法，並反白顯示已變更的行：</span><span class="sxs-lookup"><span data-stu-id="72e06-277">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="72e06-278">註冊、確認電子郵件和重設密碼</span><span class="sxs-lookup"><span data-stu-id="72e06-278">Register, confirm email, and reset password</span></span>

<span data-ttu-id="72e06-279">執行 web 應用程式，並測試帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="72e06-279">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="72e06-280">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="72e06-280">Run the app and register a new user</span></span>
* <span data-ttu-id="72e06-281">檢查您的電子郵件以取得帳戶確認連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-281">Check your email for the account confirmation link.</span></span> <span data-ttu-id="72e06-282">如果您沒有收到電子郵件，請參閱[Debug email](#debug) 。</span><span class="sxs-lookup"><span data-stu-id="72e06-282">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="72e06-283">按一下連結以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-283">Click the link to confirm your email.</span></span>
* <span data-ttu-id="72e06-284">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-284">Sign in with your email and password.</span></span>
* <span data-ttu-id="72e06-285">登出。</span><span class="sxs-lookup"><span data-stu-id="72e06-285">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="72e06-286">查看 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="72e06-286">View the manage page</span></span>

<span data-ttu-id="72e06-287">在瀏覽器中選取您的使用者名稱： ![ 瀏覽器視窗，並使用使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="72e06-287">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="72e06-288">[管理] 頁面隨即顯示，並選取 [**設定檔**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="72e06-288">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="72e06-289">此**電子郵件**會顯示一個核取方塊，指出已確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-289">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="72e06-290">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="72e06-290">Test password reset</span></span>

* <span data-ttu-id="72e06-291">如果您已登入，請選取 [**登出**]。</span><span class="sxs-lookup"><span data-stu-id="72e06-291">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="72e06-292">選取 [**登入**] 連結，然後選取 [**忘記密碼？** ] 連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-292">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="72e06-293">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-293">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="72e06-294">系統會傳送一封電子郵件，其中包含重設密碼的連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-294">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="72e06-295">檢查您的電子郵件，然後按一下連結以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="72e06-295">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="72e06-296">成功重設密碼之後，您就可以使用您的電子郵件和新密碼來登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-296">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="72e06-297">變更電子郵件和活動超時</span><span class="sxs-lookup"><span data-stu-id="72e06-297">Change email and activity timeout</span></span>

<span data-ttu-id="72e06-298">預設的無活動超時時間為14天。</span><span class="sxs-lookup"><span data-stu-id="72e06-298">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="72e06-299">下列程式碼會將非啟用時間設定為5天：</span><span class="sxs-lookup"><span data-stu-id="72e06-299">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="72e06-300">變更所有資料保護權杖壽命</span><span class="sxs-lookup"><span data-stu-id="72e06-300">Change all data protection token lifespans</span></span>

<span data-ttu-id="72e06-301">下列程式碼會將所有資料保護權杖超時時間變更為3小時：</span><span class="sxs-lookup"><span data-stu-id="72e06-301">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="72e06-302">內建的 Identity 使用者權杖（請參閱[AspNetCore/src/ Identity /Extensions.Core/src/TokenOptions.cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ）有[一天的超時時間](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="72e06-302">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="72e06-303">變更電子郵件權杖生命週期</span><span class="sxs-lookup"><span data-stu-id="72e06-303">Change the email token lifespan</span></span>

<span data-ttu-id="72e06-304">[ Identity 使用者權杖](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)的預設權杖存留期為[一天](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="72e06-304">The default token lifespan of [the Identity user tokens](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="72e06-305">本節說明如何變更電子郵件權杖的生命週期。</span><span class="sxs-lookup"><span data-stu-id="72e06-305">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="72e06-306">新增自訂[DataProtectorTokenProvider \<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和 <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions> ：</span><span class="sxs-lookup"><span data-stu-id="72e06-306">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="72e06-307">將自訂提供者新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="72e06-307">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="72e06-308">重新傳送電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="72e06-308">Resend email confirmation</span></span>

<span data-ttu-id="72e06-309">請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/5410)。</span><span class="sxs-lookup"><span data-stu-id="72e06-309">See [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="72e06-310">Debug 電子郵件</span><span class="sxs-lookup"><span data-stu-id="72e06-310">Debug email</span></span>

<span data-ttu-id="72e06-311">如果您無法讓電子郵件正常運作：</span><span class="sxs-lookup"><span data-stu-id="72e06-311">If you can't get email working:</span></span>

* <span data-ttu-id="72e06-312">在中設定中斷點 `EmailSender.Execute` ，以驗證 `SendGridClient.SendEmailAsync` 是否已呼叫。</span><span class="sxs-lookup"><span data-stu-id="72e06-312">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="72e06-313">建立[主控台應用程式，以](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼將電子郵件傳送至 `EmailSender.Execute` 。</span><span class="sxs-lookup"><span data-stu-id="72e06-313">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="72e06-314">查看[電子郵件活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="72e06-314">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="72e06-315">檢查您的垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="72e06-315">Check your spam folder.</span></span>
* <span data-ttu-id="72e06-316">在不同的電子郵件提供者（Microsoft、Yahoo、Gmail 等）上嘗試另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="72e06-316">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="72e06-317">嘗試傳送至不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-317">Try sending to different email accounts.</span></span>

<span data-ttu-id="72e06-318">**安全性最佳做法**是**不要**在測試和開發中使用生產秘密。</span><span class="sxs-lookup"><span data-stu-id="72e06-318">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="72e06-319">如果您將應用程式發行至 Azure，您可以在 Azure Web 應用程式入口網站中將 SendGrid 秘密設定為應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="72e06-319">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="72e06-320">設定系統已設定為從環境變數讀取金鑰。</span><span class="sxs-lookup"><span data-stu-id="72e06-320">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="72e06-321">合併社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="72e06-321">Combine social and local login accounts</span></span>

<span data-ttu-id="72e06-322">若要完成這一節，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="72e06-322">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="72e06-323">請參閱[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="72e06-323">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="72e06-324">您可以按一下電子郵件連結，以合併本機和社交帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-324">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="72e06-325">在下列順序中， RickAndMSFT@gmail.com 會先將 "" 建立為本機登入; 不過，您可以先將帳戶建立為社交登入，然後再新增本機登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-325">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式： RickAndMSFT@gmail.com 使用者驗證](accconfirm/_static/rick.png)

<span data-ttu-id="72e06-327">按一下 [**管理**] 連結。</span><span class="sxs-lookup"><span data-stu-id="72e06-327">Click on the **Manage** link.</span></span> <span data-ttu-id="72e06-328">請注意與此帳戶相關聯的0外部（社交登入）。</span><span class="sxs-lookup"><span data-stu-id="72e06-328">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="72e06-330">按一下另一個登入服務的連結，並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="72e06-330">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="72e06-331">在下圖中，Facebook 是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="72e06-331">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入視圖清單 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="72e06-333">已合併這兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="72e06-333">The two accounts have been combined.</span></span> <span data-ttu-id="72e06-334">您可以使用任一帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="72e06-334">You are able to sign in with either account.</span></span> <span data-ttu-id="72e06-335">您可能想要讓使用者新增本機帳戶，以防其社交登入驗證服務關閉，或更有可能失去其社交帳戶的存取權。</span><span class="sxs-lookup"><span data-stu-id="72e06-335">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="72e06-336">在網站有使用者之後啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="72e06-336">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="72e06-337">在網站上啟用帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="72e06-337">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="72e06-338">現有的使用者會被鎖定，因為他們的帳戶不會確認。</span><span class="sxs-lookup"><span data-stu-id="72e06-338">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="72e06-339">若要解決現有的使用者鎖定，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="72e06-339">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="72e06-340">更新資料庫，將所有現有的使用者標示為已確認。</span><span class="sxs-lookup"><span data-stu-id="72e06-340">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="72e06-341">確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="72e06-341">Confirm existing users.</span></span> <span data-ttu-id="72e06-342">例如，batch-傳送含有確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="72e06-342">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
