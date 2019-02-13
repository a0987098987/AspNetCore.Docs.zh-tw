---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 2/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 77d7b209d57f9ee44f158798ff780ce85c87aaf2
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56159404"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="98503-103">帳戶確認和 ASP.NET Core 中的密碼復原</span><span class="sxs-lookup"><span data-stu-id="98503-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="98503-104">請參閱[此 PDF 檔案](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 和 2.1 版。</span><span class="sxs-lookup"><span data-stu-id="98503-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="98503-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="98503-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="98503-106">本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="98503-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="98503-107">本教學課程**不**開頭主題。</span><span class="sxs-lookup"><span data-stu-id="98503-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="98503-108">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="98503-108">You should be familiar with:</span></span>

* [<span data-ttu-id="98503-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98503-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="98503-110">驗證</span><span class="sxs-lookup"><span data-stu-id="98503-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="98503-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="98503-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="98503-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="98503-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="98503-113">建立 web 應用程式，並建立身分識別的結構</span><span class="sxs-lookup"><span data-stu-id="98503-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98503-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98503-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="98503-115">在 Visual Studio 中，建立新**Web 應用程式**專案，命名為**WebPWrecover**。</span><span class="sxs-lookup"><span data-stu-id="98503-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="98503-116">選取 **ASP.NET Core 2.1**。</span><span class="sxs-lookup"><span data-stu-id="98503-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="98503-117">保留預設值**驗證**設為**不需要驗證**。</span><span class="sxs-lookup"><span data-stu-id="98503-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="98503-118">下一個步驟中加入驗證。</span><span class="sxs-lookup"><span data-stu-id="98503-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="98503-119">在下一個步驟：</span><span class="sxs-lookup"><span data-stu-id="98503-119">In the next step:</span></span>

* <span data-ttu-id="98503-120">將版面配置頁面設為 *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="98503-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="98503-121">選取*帳戶/註冊*</span><span class="sxs-lookup"><span data-stu-id="98503-121">Select *Account/Register*</span></span>
* <span data-ttu-id="98503-122">建立新**資料內容類別**</span><span class="sxs-lookup"><span data-stu-id="98503-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="98503-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="98503-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="98503-124">執行`dotnet aspnet-codegenerator identity --help`scaffolding 工具取得說明。</span><span class="sxs-lookup"><span data-stu-id="98503-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="98503-125">請依照下列中的指示[啟用驗證](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="98503-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="98503-126">新增`app.UseAuthentication();`至 `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="98503-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="98503-127">新增`<partial name="_LoginPartial" />`和配置檔案。</span><span class="sxs-lookup"><span data-stu-id="98503-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="98503-128">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="98503-128">Test new user registration</span></span>

<span data-ttu-id="98503-129">執行應用程式，請選取**註冊**連結，並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="98503-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="98503-130">此時，唯一的驗證電子郵件是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="98503-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="98503-131">提交註冊之後, 您登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="98503-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="98503-132">稍後在教學課程中，會將程式碼更新讓新的使用者無法登入，直到其電子郵件會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="98503-132">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="98503-133">請注意，資料表的`EmailConfirmed`欄位是`False`。</span><span class="sxs-lookup"><span data-stu-id="98503-133">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="98503-134">您可能想要這封電子郵件一次的下一個步驟時使用的應用程式會傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-134">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="98503-135">以滑鼠右鍵按一下資料列，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="98503-135">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="98503-136">刪除電子郵件別名更容易在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="98503-136">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="98503-137">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="98503-137">Require email confirmation</span></span>

<span data-ttu-id="98503-138">您最好確認新的使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-138">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="98503-139">電子郵件確認可協助您確認它們無法模擬其他人 （也就是尚未註冊使用其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="98503-139">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="98503-140">假設您有討論論壇，而且您想要防止 「yli@example.com"中註冊為 「nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="98503-140">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="98503-141">而不需要電子郵件確認"nolivetto@contoso.com」 無法從您的應用程式收到不想要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-141">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="98503-142">假設使用者不小心註冊為 「ylo@example.com"並還沒有發現..."yli 」 的拼字錯誤。</span><span class="sxs-lookup"><span data-stu-id="98503-142">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="98503-143">他們將無法使用密碼復原，因為應用程式沒有正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-143">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="98503-144">電子郵件確認 bot 提供有限的保護。</span><span class="sxs-lookup"><span data-stu-id="98503-144">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="98503-145">電子郵件確認不會提供保護，防範惡意使用者與許多電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="98503-145">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="98503-146">您通常想要防止新使用者之前確認電子郵件張貼到您的網站的任何資料。</span><span class="sxs-lookup"><span data-stu-id="98503-146">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="98503-147">更新*Areas/Identity/IdentityHostingStartup.cs*要求確認電子郵件：</span><span class="sxs-lookup"><span data-stu-id="98503-147">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="98503-148">`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認其電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-148">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="98503-149">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="98503-149">Configure email provider</span></span>

<span data-ttu-id="98503-150">在本教學課程中， [SendGrid](https://sendgrid.com)用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-150">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="98503-151">您需要的 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-151">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="98503-152">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="98503-152">You can use other email providers.</span></span> <span data-ttu-id="98503-153">ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-153">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="98503-154">我們建議您可以使用 SendGrid 或另一個電子郵件服務傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-154">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="98503-155">SMTP 是難以保護，並正確設定。</span><span class="sxs-lookup"><span data-stu-id="98503-155">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="98503-156">建立類別來擷取安全的電子郵件的金鑰。</span><span class="sxs-lookup"><span data-stu-id="98503-156">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="98503-157">此範例中，建立*Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="98503-157">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="98503-158">設定 SendGrid 使用者祕密</span><span class="sxs-lookup"><span data-stu-id="98503-158">Configure SendGrid user secrets</span></span>

<span data-ttu-id="98503-159">新增的唯一`<UserSecretsId>`值`<PropertyGroup>`專案檔的項目：</span><span class="sxs-lookup"><span data-stu-id="98503-159">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="98503-160">設定`SendGridUser`並`SendGridKey`具有[secret manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="98503-160">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="98503-161">例如: </span><span class="sxs-lookup"><span data-stu-id="98503-161">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="98503-162">在 Windows、 Secret Manager 儲存中的索引鍵/值組*secrets.json*檔案中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。</span><span class="sxs-lookup"><span data-stu-id="98503-162">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="98503-163">內容*secrets.json*檔案未加密。</span><span class="sxs-lookup"><span data-stu-id="98503-163">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="98503-164">*Secrets.json*如下所示的檔案 (`SendGridKey`已移除值。)</span><span class="sxs-lookup"><span data-stu-id="98503-164">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="98503-165">如需詳細資訊，請參閱 <<c0> [ 選項模式](xref:fundamentals/configuration/options)並[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="98503-165">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="98503-166">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="98503-166">Install SendGrid</span></span>

<span data-ttu-id="98503-167">本教學課程示範如何將透過電子郵件通知[SendGrid](https://sendgrid.com/)，但您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="98503-167">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="98503-168">安裝`SendGrid`NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="98503-168">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98503-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98503-169">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="98503-170">從 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="98503-170">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="98503-171">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="98503-171">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="98503-172">在主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="98503-172">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="98503-173">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)報名免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="98503-173">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="98503-174">實作 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="98503-174">Implement IEmailSender</span></span>

<span data-ttu-id="98503-175">實作`IEmailSender`，建立*Services/EmailSender.cs*與下列類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="98503-175">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="98503-176">設定啟動，以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="98503-176">Configure startup to support email</span></span>

<span data-ttu-id="98503-177">將下列程式碼加入`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="98503-177">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="98503-178">新增`EmailSender`為暫時性的服務。</span><span class="sxs-lookup"><span data-stu-id="98503-178">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="98503-179">註冊`AuthMessageSenderOptions`組態執行個體。</span><span class="sxs-lookup"><span data-stu-id="98503-179">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="98503-180">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="98503-180">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="98503-181">範本會將程式碼進行帳戶確認和密碼復原。</span><span class="sxs-lookup"><span data-stu-id="98503-181">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="98503-182">尋找`OnPostAsync`方法中的*Areas/Identity/Pages/Account/Register.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="98503-182">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="98503-183">防止新註冊的使用者自動記錄的標記為註解下面這一行：</span><span class="sxs-lookup"><span data-stu-id="98503-183">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="98503-184">使用已變更反白顯示的列，會顯示完整的方法：</span><span class="sxs-lookup"><span data-stu-id="98503-184">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="98503-185">註冊、 確認電子郵件，以及重設密碼</span><span class="sxs-lookup"><span data-stu-id="98503-185">Register, confirm email, and reset password</span></span>

<span data-ttu-id="98503-186">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="98503-186">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="98503-187">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="98503-187">Run the app and register a new user</span></span>

  ![Web 應用程式註冊帳戶檢視](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="98503-189">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-189">Check your email for the account confirmation link.</span></span> <span data-ttu-id="98503-190">請參閱[偵錯電子郵件](#debug)如果您沒有收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-190">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="98503-191">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-191">Click the link to confirm your email.</span></span>
* <span data-ttu-id="98503-192">使用您的電子郵件和密碼登入。</span><span class="sxs-lookup"><span data-stu-id="98503-192">Sign in with your email and password.</span></span>
* <span data-ttu-id="98503-193">登出。</span><span class="sxs-lookup"><span data-stu-id="98503-193">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="98503-194">檢視 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="98503-194">View the manage page</span></span>

<span data-ttu-id="98503-195">在瀏覽器中，選取您的使用者名稱：![瀏覽器視窗中的使用使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="98503-195">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="98503-196">您可能需要展開的導覽列，以查看使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="98503-196">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

<span data-ttu-id="98503-198">[管理] 頁面會顯示**設定檔**選取的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="98503-198">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="98503-199">**電子郵件**顯示核取方塊，指出電子郵件已確認。</span><span class="sxs-lookup"><span data-stu-id="98503-199">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="98503-200">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="98503-200">Test password reset</span></span>

* <span data-ttu-id="98503-201">如果您的登入，請選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="98503-201">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="98503-202">選取 **登入**連結，然後選取**忘記密碼？** 連結。</span><span class="sxs-lookup"><span data-stu-id="98503-202">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="98503-203">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-203">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="98503-204">會傳送具有重設密碼連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="98503-204">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="98503-205">請檢查您的電子郵件，然後按一下 重設密碼連結。</span><span class="sxs-lookup"><span data-stu-id="98503-205">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="98503-206">已成功重設您的密碼之後，您可以使用您的電子郵件和新密碼登入。</span><span class="sxs-lookup"><span data-stu-id="98503-206">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="98503-207">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="98503-207">Debug email</span></span>

<span data-ttu-id="98503-208">如果您無法收到電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="98503-208">If you can't get email working:</span></span>

* <span data-ttu-id="98503-209">在 設定中斷點`EmailSender.Execute`若要確認`SendGridClient.SendEmailAsync`呼叫。</span><span class="sxs-lookup"><span data-stu-id="98503-209">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="98503-210">建立[主控台應用程式以傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼以`EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="98503-210">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="98503-211">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="98503-211">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="98503-212">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="98503-212">Check your spam folder.</span></span>
* <span data-ttu-id="98503-213">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="98503-213">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="98503-214">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="98503-214">Try sending to different email accounts.</span></span>

<span data-ttu-id="98503-215">**安全性最佳作法**旨在**不**使用生產環境中開發和測試的祕密。</span><span class="sxs-lookup"><span data-stu-id="98503-215">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="98503-216">如果您將應用程式發佈至 Azure 時，您可以為 Azure Web 應用程式入口網站中的應用程式設定來設定 SendGrid 祕密。</span><span class="sxs-lookup"><span data-stu-id="98503-216">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="98503-217">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="98503-217">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="98503-218">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="98503-218">Combine social and local login accounts</span></span>

<span data-ttu-id="98503-219">若要完成本節中，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="98503-219">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="98503-220">請參閱[Facebook、 Google、 與外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="98503-220">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="98503-221">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="98503-221">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="98503-222">依下列順序，"RickAndMSFT@gmail.com」 會先建立做為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後新增的本機登入。</span><span class="sxs-lookup"><span data-stu-id="98503-222">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="98503-224">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="98503-224">Click on the **Manage** link.</span></span> <span data-ttu-id="98503-225">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="98503-225">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="98503-227">按一下連結至另一個登入服務並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="98503-227">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="98503-228">在下圖中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="98503-228">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入檢視，列出 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="98503-230">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="98503-230">The two accounts have been combined.</span></span> <span data-ttu-id="98503-231">您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="98503-231">You are able to sign in with either account.</span></span> <span data-ttu-id="98503-232">您可能想要新增本機帳戶，以免其社交登入驗證服務已關閉，或它們可能比較無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="98503-232">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="98503-233">站台後為使用者啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="98503-233">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="98503-234">在網站上啟用的帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="98503-234">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="98503-235">現有使用者遭到鎖定中，這是因為未確認他們的帳戶。</span><span class="sxs-lookup"><span data-stu-id="98503-235">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="98503-236">若要解決現有的使用者鎖定，請使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="98503-236">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="98503-237">更新將標示為正在確認所有現有的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="98503-237">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="98503-238">請確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="98503-238">Confirm existing users.</span></span> <span data-ttu-id="98503-239">例如，批次傳送電子郵件以確認連結。</span><span class="sxs-lookup"><span data-stu-id="98503-239">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
