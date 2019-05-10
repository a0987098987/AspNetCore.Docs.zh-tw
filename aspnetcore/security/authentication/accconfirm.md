---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 59041bcf11f7deb351a2f0bb075ed80c8af5e12b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891675"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="ad2a8-103">帳戶確認和 ASP.NET Core 中的密碼復原</span><span class="sxs-lookup"><span data-stu-id="ad2a8-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ad2a8-104">請參閱[此 PDF 檔案](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 和 2.1 版。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ad2a8-105">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Ponant](https://github.com/Ponant)，和[Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ad2a8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="ad2a8-106">本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="ad2a8-107">本教學課程**不**開頭主題。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="ad2a8-108">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-108">You should be familiar with:</span></span>

* [<span data-ttu-id="ad2a8-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad2a8-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="ad2a8-110">驗證</span><span class="sxs-lookup"><span data-stu-id="ad2a8-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="ad2a8-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ad2a8-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="ad2a8-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="ad2a8-112">Prerequisites</span></span>

[<span data-ttu-id="ad2a8-113">.NET core 2.2 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ad2a8-113">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="ad2a8-114">建立 web 應用程式，並建立身分識別的結構</span><span class="sxs-lookup"><span data-stu-id="ad2a8-114">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="ad2a8-115">執行下列命令來建立驗證的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="ad2a8-116">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="ad2a8-116">Test new user registration</span></span>

<span data-ttu-id="ad2a8-117">執行應用程式，請選取**註冊**連結，並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-117">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="ad2a8-118">此時，唯一的驗證電子郵件是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-118">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="ad2a8-119">提交註冊之後, 您登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-119">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="ad2a8-120">稍後在教學課程中，會將程式碼更新讓新的使用者無法登入，直到其電子郵件會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-120">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="ad2a8-121">請注意，資料表的`EmailConfirmed`欄位是`False`。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-121">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="ad2a8-122">您可能想要這封電子郵件一次的下一個步驟時使用的應用程式會傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-122">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="ad2a8-123">以滑鼠右鍵按一下資料列，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-123">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="ad2a8-124">刪除電子郵件別名更容易在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-124">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="ad2a8-125">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="ad2a8-125">Require email confirmation</span></span>

<span data-ttu-id="ad2a8-126">您最好確認新的使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-126">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="ad2a8-127">電子郵件確認可協助您確認它們無法模擬其他人 （也就是尚未註冊使用其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-127">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="ad2a8-128">假設您有討論論壇，而且您想要防止 「yli@example.com"中註冊為 「nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-128">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="ad2a8-129">而不需要電子郵件確認"nolivetto@contoso.com」 無法從您的應用程式收到不想要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-129">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="ad2a8-130">假設使用者不小心註冊為 「ylo@example.com"並還沒有發現..."yli 」 的拼字錯誤。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-130">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="ad2a8-131">他們將無法使用密碼復原，因為應用程式沒有正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-131">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="ad2a8-132">電子郵件確認 bot 提供有限的保護。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-132">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="ad2a8-133">電子郵件確認不會提供保護，防範惡意使用者與許多電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-133">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="ad2a8-134">您通常想要防止新使用者之前確認電子郵件張貼到您的網站的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-134">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="ad2a8-135">更新`Startup.ConfigureServices`要求確認電子郵件：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-135">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="ad2a8-136">`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認其電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-136">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="ad2a8-137">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="ad2a8-137">Configure email provider</span></span>

<span data-ttu-id="ad2a8-138">在本教學課程中， [SendGrid](https://sendgrid.com)用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-138">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="ad2a8-139">您需要的 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-139">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="ad2a8-140">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-140">You can use other email providers.</span></span> <span data-ttu-id="ad2a8-141">ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-141">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="ad2a8-142">我們建議您可以使用 SendGrid 或另一個電子郵件服務傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-142">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="ad2a8-143">SMTP 是難以保護，並正確設定。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-143">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="ad2a8-144">建立類別來擷取安全的電子郵件的金鑰。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-144">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="ad2a8-145">此範例中，建立*Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="ad2a8-145">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="ad2a8-146">設定 SendGrid 使用者祕密</span><span class="sxs-lookup"><span data-stu-id="ad2a8-146">Configure SendGrid user secrets</span></span>

<span data-ttu-id="ad2a8-147">設定`SendGridUser`並`SendGridKey`具有[secret manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-147">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="ad2a8-148">例如：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-148">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="ad2a8-149">在 Windows、 Secret Manager 儲存中的索引鍵/值組*secrets.json*檔案中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-149">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="ad2a8-150">內容*secrets.json*檔案未加密。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-150">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="ad2a8-151">下列標記示範*secrets.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-151">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="ad2a8-152">`SendGridKey`已移除值。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-152">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="ad2a8-153">如需詳細資訊，請參閱 <<c0> [ 選項模式](xref:fundamentals/configuration/options)並[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-153">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="ad2a8-154">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="ad2a8-154">Install SendGrid</span></span>

<span data-ttu-id="ad2a8-155">本教學課程示範如何將透過電子郵件通知[SendGrid](https://sendgrid.com/)，但您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-155">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="ad2a8-156">安裝`SendGrid`NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-156">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ad2a8-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad2a8-157">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ad2a8-158">從 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-158">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ad2a8-159">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ad2a8-159">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ad2a8-160">在主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-160">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="ad2a8-161">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)報名免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-161">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="ad2a8-162">實作 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="ad2a8-162">Implement IEmailSender</span></span>

<span data-ttu-id="ad2a8-163">實作`IEmailSender`，建立*Services/EmailSender.cs*與下列類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-163">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="ad2a8-164">設定啟動，以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="ad2a8-164">Configure startup to support email</span></span>

<span data-ttu-id="ad2a8-165">將下列程式碼加入`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-165">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="ad2a8-166">新增`EmailSender`為暫時性的服務。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-166">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="ad2a8-167">註冊`AuthMessageSenderOptions`組態執行個體。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-167">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="ad2a8-168">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="ad2a8-168">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="ad2a8-169">範本會將程式碼進行帳戶確認和密碼復原。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-169">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="ad2a8-170">尋找`OnPostAsync`方法中的*Areas/Identity/Pages/Account/Register.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-170">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="ad2a8-171">防止新註冊的使用者自動登入註解下面這一行：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-171">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ad2a8-172">使用已變更反白顯示的列，會顯示完整的方法：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-172">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="ad2a8-173">註冊、 確認電子郵件，以及重設密碼</span><span class="sxs-lookup"><span data-stu-id="ad2a8-173">Register, confirm email, and reset password</span></span>

<span data-ttu-id="ad2a8-174">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-174">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="ad2a8-175">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="ad2a8-175">Run the app and register a new user</span></span>
* <span data-ttu-id="ad2a8-176">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-176">Check your email for the account confirmation link.</span></span> <span data-ttu-id="ad2a8-177">請參閱[偵錯電子郵件](#debug)如果您沒有收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-177">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="ad2a8-178">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-178">Click the link to confirm your email.</span></span>
* <span data-ttu-id="ad2a8-179">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-179">Sign in with your email and password.</span></span>
* <span data-ttu-id="ad2a8-180">登出。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-180">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="ad2a8-181">檢視 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="ad2a8-181">View the manage page</span></span>

<span data-ttu-id="ad2a8-182">在瀏覽器中，選取您的使用者名稱：![瀏覽器視窗中的使用使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="ad2a8-182">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="ad2a8-183">[管理] 頁面會顯示**設定檔**選取的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-183">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="ad2a8-184">**電子郵件**顯示核取方塊，指出電子郵件已確認。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-184">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="ad2a8-185">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="ad2a8-185">Test password reset</span></span>

* <span data-ttu-id="ad2a8-186">如果您已登入，請選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-186">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="ad2a8-187">選取 **登入**連結，然後選取**忘記密碼？** 連結。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-187">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="ad2a8-188">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-188">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="ad2a8-189">會傳送具有重設密碼連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-189">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="ad2a8-190">請檢查您的電子郵件，然後按一下 重設密碼連結。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-190">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="ad2a8-191">已成功重設您的密碼之後，您可以使用您的電子郵件和新密碼登入。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-191">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="ad2a8-192">變更電子郵件和活動的逾時</span><span class="sxs-lookup"><span data-stu-id="ad2a8-192">Change email and activity timeout</span></span>

<span data-ttu-id="ad2a8-193">預設閒置逾時為 14 天。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-193">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="ad2a8-194">下列程式碼會設定為 5 天的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-194">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="ad2a8-195">變更所有資料保護權杖的期限</span><span class="sxs-lookup"><span data-stu-id="ad2a8-195">Change all data protection token lifespans</span></span>

<span data-ttu-id="ad2a8-196">下列程式碼會變更為 3 小時內的所有資料保護權杖逾時期限：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-196">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="ad2a8-197">內建在身分識別的使用者語彙基元 (請參閱[AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) 已[一天的逾時](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-197">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="ad2a8-198">變更的電子郵件的權杖存留時間</span><span class="sxs-lookup"><span data-stu-id="ad2a8-198">Change the email token lifespan</span></span>

<span data-ttu-id="ad2a8-199">預設權杖存留時間[身分識別的使用者語彙基元](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs)是[有一天](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-199">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="ad2a8-200">本節說明如何變更的電子郵件的權杖存留時間。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-200">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="ad2a8-201">新增自訂[DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1)和<xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="ad2a8-201">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="ad2a8-202">將自訂提供者新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-202">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="ad2a8-203">重新傳送電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="ad2a8-203">Resend email confirmation</span></span>

<span data-ttu-id="ad2a8-204">請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5410)。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-204">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="ad2a8-205">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="ad2a8-205">Debug email</span></span>

<span data-ttu-id="ad2a8-206">如果您無法收到電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-206">If you can't get email working:</span></span>

* <span data-ttu-id="ad2a8-207">在 設定中斷點`EmailSender.Execute`若要確認`SendGridClient.SendEmailAsync`呼叫。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-207">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="ad2a8-208">建立[主控台應用程式以傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼以`EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-208">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="ad2a8-209">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-209">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="ad2a8-210">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-210">Check your spam folder.</span></span>
* <span data-ttu-id="ad2a8-211">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="ad2a8-211">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="ad2a8-212">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-212">Try sending to different email accounts.</span></span>

<span data-ttu-id="ad2a8-213">**安全性最佳作法**旨在**不**使用生產環境中開發和測試的祕密。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-213">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="ad2a8-214">如果您將應用程式發佈至 Azure 時，您可以為 Azure Web 應用程式入口網站中的應用程式設定來設定 SendGrid 祕密。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-214">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="ad2a8-215">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-215">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="ad2a8-216">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="ad2a8-216">Combine social and local login accounts</span></span>

<span data-ttu-id="ad2a8-217">若要完成本節中，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-217">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="ad2a8-218">請參閱[Facebook、 Google、 與外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-218">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="ad2a8-219">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-219">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="ad2a8-220">依下列順序，"RickAndMSFT@gmail.com」 會先建立做為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後新增的本機登入。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-220">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="ad2a8-222">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-222">Click on the **Manage** link.</span></span> <span data-ttu-id="ad2a8-223">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-223">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="ad2a8-225">按一下連結至另一個登入服務並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-225">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="ad2a8-226">在下圖中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-226">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入檢視，列出 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="ad2a8-228">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-228">The two accounts have been combined.</span></span> <span data-ttu-id="ad2a8-229">您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-229">You are able to sign in with either account.</span></span> <span data-ttu-id="ad2a8-230">您可能想要新增本機帳戶，以免其社交登入驗證服務已關閉，或它們可能比較無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-230">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="ad2a8-231">站台後為使用者啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="ad2a8-231">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="ad2a8-232">在網站上啟用的帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-232">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="ad2a8-233">現有使用者遭到鎖定中，這是因為未確認他們的帳戶。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-233">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="ad2a8-234">若要解決現有的使用者鎖定，請使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="ad2a8-234">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="ad2a8-235">更新將標示為正在確認所有現有的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-235">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="ad2a8-236">請確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-236">Confirm existing users.</span></span> <span data-ttu-id="ad2a8-237">例如，批次傳送電子郵件以確認連結。</span><span class="sxs-lookup"><span data-stu-id="ad2a8-237">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
