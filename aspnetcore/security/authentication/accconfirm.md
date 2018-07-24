---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 84eb3580107572f66f0c3b565b8e76ba401c0ddb
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219403"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="42358-103">請參閱[此 PDF 檔案](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 和 2.1 版。</span><span class="sxs-lookup"><span data-stu-id="42358-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="42358-104">帳戶確認和 ASP.NET Core 中的密碼復原</span><span class="sxs-lookup"><span data-stu-id="42358-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="42358-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="42358-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="42358-106">本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="42358-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="42358-107">本教學課程**不**開頭主題。</span><span class="sxs-lookup"><span data-stu-id="42358-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="42358-108">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="42358-108">You should be familiar with:</span></span>

* [<span data-ttu-id="42358-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42358-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="42358-110">驗證</span><span class="sxs-lookup"><span data-stu-id="42358-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="42358-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="42358-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="42358-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="42358-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="42358-113">建立 web 應用程式，並建立身分識別的結構</span><span class="sxs-lookup"><span data-stu-id="42358-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42358-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42358-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="42358-115">在 Visual Studio 中，建立新**Web 應用程式**專案，命名為**WebPWrecover**。</span><span class="sxs-lookup"><span data-stu-id="42358-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="42358-116">選取  **ASP.NET Core 2.1**。</span><span class="sxs-lookup"><span data-stu-id="42358-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="42358-117">保留預設值**驗證**設為**不需要驗證**。</span><span class="sxs-lookup"><span data-stu-id="42358-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="42358-118">下一個步驟中加入驗證。</span><span class="sxs-lookup"><span data-stu-id="42358-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="42358-119">在下一個步驟：</span><span class="sxs-lookup"><span data-stu-id="42358-119">In the next step:</span></span>

* <span data-ttu-id="42358-120">將版面配置頁面設為 *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="42358-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="42358-121">選取*帳戶/註冊*</span><span class="sxs-lookup"><span data-stu-id="42358-121">Select *Account/Register*</span></span>
* <span data-ttu-id="42358-122">建立新**資料內容類別**</span><span class="sxs-lookup"><span data-stu-id="42358-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="42358-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="42358-123">.NET Core CLI</span></span>](#tab/netcore-cli)

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

<span data-ttu-id="42358-124">執行`dotnet aspnet-codegenerator identity --help`scaffolding 工具取得說明。</span><span class="sxs-lookup"><span data-stu-id="42358-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="42358-125">請依照下列中的指示[啟用驗證](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="42358-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="42358-126">新增`app.UseAuthentication();`至 `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="42358-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="42358-127">新增`<partial name="_LoginPartial" />`和配置檔案。</span><span class="sxs-lookup"><span data-stu-id="42358-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="42358-128">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="42358-128">Test new user registration</span></span>

<span data-ttu-id="42358-129">執行應用程式，請選取**註冊**連結，並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="42358-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="42358-130">此時，唯一的驗證電子郵件是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="42358-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="42358-131">提交註冊之後, 您登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="42358-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="42358-132">稍後在教學課程中，會將程式碼更新讓新的使用者無法登入，直到其電子郵件會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="42358-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="42358-133">檢視識別資料庫</span><span class="sxs-lookup"><span data-stu-id="42358-133">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42358-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42358-134">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="42358-135">從**檢視**功能表上，選取**SQL Server 物件總管**(SSOX)。</span><span class="sxs-lookup"><span data-stu-id="42358-135">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="42358-136">瀏覽至 **(localdb) (SQL Server 13) MSSQLLocalDB**。</span><span class="sxs-lookup"><span data-stu-id="42358-136">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="42358-137">以滑鼠右鍵按一下**dbo。AspNetUsers** > **檢視資料**:</span><span class="sxs-lookup"><span data-stu-id="42358-137">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server 物件總管] 中的 [AspNetUsers 資料表上的操作功能表](accconfirm/_static/ssox.png)

<span data-ttu-id="42358-139">請注意，資料表的`EmailConfirmed`欄位是`False`。</span><span class="sxs-lookup"><span data-stu-id="42358-139">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="42358-140">您可能想要這封電子郵件一次的下一個步驟時使用的應用程式會傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-140">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="42358-141">以滑鼠右鍵按一下資料列，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="42358-141">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="42358-142">刪除電子郵件別名更容易在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="42358-142">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="42358-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="42358-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="42358-144">請參閱[ASP.NET Core MVC 專案中使用 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)如需有關如何檢視 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="42358-144">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="42358-145">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="42358-145">Require email confirmation</span></span>

<span data-ttu-id="42358-146">您最好確認新的使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-146">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="42358-147">電子郵件確認可協助您確認它們無法模擬其他人 （也就是尚未註冊使用其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="42358-147">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="42358-148">假設您有討論論壇，而且您想要防止 「yli@example.com"中註冊為 「nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="42358-148">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="42358-149">而不需要電子郵件確認"nolivetto@contoso.com」 無法從您的應用程式收到不想要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-149">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="42358-150">假設使用者不小心註冊為 「ylo@example.com"並還沒有發現..."yli 」 的拼字錯誤。</span><span class="sxs-lookup"><span data-stu-id="42358-150">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="42358-151">他們將無法使用密碼復原，因為應用程式沒有正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-151">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="42358-152">電子郵件確認 bot 提供有限的保護。</span><span class="sxs-lookup"><span data-stu-id="42358-152">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="42358-153">電子郵件確認不會提供保護，防範惡意使用者與許多電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="42358-153">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="42358-154">您通常想要防止新使用者之前確認電子郵件張貼到您的網站的任何資料。</span><span class="sxs-lookup"><span data-stu-id="42358-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="42358-155">更新*Areas/Identity/IdentityHostingStartup.cs*要求確認電子郵件：</span><span class="sxs-lookup"><span data-stu-id="42358-155">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="42358-156">`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認其電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-156">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="42358-157">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="42358-157">Configure email provider</span></span>

<span data-ttu-id="42358-158">在本教學課程中， [SendGrid](https://sendgrid.com)用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-158">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="42358-159">您需要的 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-159">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="42358-160">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="42358-160">You can use other email providers.</span></span> <span data-ttu-id="42358-161">ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-161">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="42358-162">我們建議您可以使用 SendGrid 或另一個電子郵件服務傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-162">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="42358-163">SMTP 是難以保護，並正確設定。</span><span class="sxs-lookup"><span data-stu-id="42358-163">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="42358-164">[選項模式](xref:fundamentals/configuration/options)用來存取使用者帳戶和金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="42358-164">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="42358-165">如需詳細資訊，請參閱 <<c0> [ 組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="42358-165">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="42358-166">建立類別來擷取安全的電子郵件的金鑰。</span><span class="sxs-lookup"><span data-stu-id="42358-166">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="42358-167">此範例中，建立*Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="42358-167">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="42358-168">設定 SendGrid 使用者祕密</span><span class="sxs-lookup"><span data-stu-id="42358-168">Configure SendGrid user secrets</span></span>

<span data-ttu-id="42358-169">新增的唯一`<UserSecretsId>`值`<PropertyGroup>`專案檔的項目：</span><span class="sxs-lookup"><span data-stu-id="42358-169">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="42358-170">設定`SendGridUser`並`SendGridKey`具有[secret manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="42358-170">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="42358-171">例如: </span><span class="sxs-lookup"><span data-stu-id="42358-171">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="42358-172">在 Windows、 Secret Manager 儲存中的索引鍵/值組*secrets.json*檔案中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。</span><span class="sxs-lookup"><span data-stu-id="42358-172">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="42358-173">內容*secrets.json*檔案未加密。</span><span class="sxs-lookup"><span data-stu-id="42358-173">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="42358-174">*Secrets.json*如下所示的檔案 (`SendGridKey`已移除值。)</span><span class="sxs-lookup"><span data-stu-id="42358-174">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="42358-175">安裝 SendGrid</span><span class="sxs-lookup"><span data-stu-id="42358-175">Install SendGrid</span></span>

<span data-ttu-id="42358-176">本教學課程示範如何將透過電子郵件通知[SendGrid](https://sendgrid.com/)，但您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="42358-176">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="42358-177">安裝`SendGrid`NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="42358-177">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42358-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42358-178">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="42358-179">從 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="42358-179">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="42358-180">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="42358-180">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="42358-181">在主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="42358-181">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="42358-182">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)報名免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="42358-182">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="42358-183">實作 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="42358-183">Implement IEmailSender</span></span>

<span data-ttu-id="42358-184">實作`IEmailSender`，建立*Services/EmailSender.cs*與下列類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="42358-184">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="42358-185">設定啟動，以支援電子郵件</span><span class="sxs-lookup"><span data-stu-id="42358-185">Configure startup to support email</span></span>

<span data-ttu-id="42358-186">將下列程式碼加入`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="42358-186">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="42358-187">新增`EmailSender`作為單一服務。</span><span class="sxs-lookup"><span data-stu-id="42358-187">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="42358-188">註冊`AuthMessageSenderOptions`組態執行個體。</span><span class="sxs-lookup"><span data-stu-id="42358-188">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="42358-189">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="42358-189">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="42358-190">範本會將程式碼進行帳戶確認和密碼復原。</span><span class="sxs-lookup"><span data-stu-id="42358-190">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="42358-191">尋找`OnPostAsync`方法中的*Areas/Identity/Pages/Account/Register.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="42358-191">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="42358-192">防止新註冊的使用者自動記錄的標記為註解下面這一行：</span><span class="sxs-lookup"><span data-stu-id="42358-192">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="42358-193">使用已變更反白顯示的列，會顯示完整的方法：</span><span class="sxs-lookup"><span data-stu-id="42358-193">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="42358-194">註冊、 確認電子郵件，以及重設密碼</span><span class="sxs-lookup"><span data-stu-id="42358-194">Register, confirm email, and reset password</span></span>

<span data-ttu-id="42358-195">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="42358-195">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="42358-196">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="42358-196">Run the app and register a new user</span></span>

  ![Web 應用程式註冊帳戶檢視](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="42358-198">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-198">Check your email for the account confirmation link.</span></span> <span data-ttu-id="42358-199">請參閱[偵錯電子郵件](#debug)如果您沒有收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-199">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="42358-200">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-200">Click the link to confirm your email.</span></span>
* <span data-ttu-id="42358-201">登入您的電子郵件和密碼。</span><span class="sxs-lookup"><span data-stu-id="42358-201">Log in with your email and password.</span></span>
* <span data-ttu-id="42358-202">登出。</span><span class="sxs-lookup"><span data-stu-id="42358-202">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="42358-203">檢視 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="42358-203">View the manage page</span></span>

<span data-ttu-id="42358-204">在瀏覽器中，選取您的使用者名稱：![瀏覽器視窗中的使用使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="42358-204">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="42358-205">您可能需要展開的導覽列，以查看使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="42358-205">You might need to expand the navbar to see user name.</span></span>

![導覽列](accconfirm/_static/x.png)

<span data-ttu-id="42358-207">[管理] 頁面會顯示**設定檔**選取的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="42358-207">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="42358-208">**電子郵件**顯示核取方塊，指出電子郵件已確認。</span><span class="sxs-lookup"><span data-stu-id="42358-208">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="42358-209">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="42358-209">Test password reset</span></span>

* <span data-ttu-id="42358-210">如果您的登入，請選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="42358-210">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="42358-211">選取 **登入**連結，然後選取**忘記密碼？** 連結。</span><span class="sxs-lookup"><span data-stu-id="42358-211">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="42358-212">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-212">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="42358-213">會傳送具有重設密碼連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="42358-213">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="42358-214">請檢查您的電子郵件，然後按一下 重設密碼連結。</span><span class="sxs-lookup"><span data-stu-id="42358-214">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="42358-215">已成功重設您的密碼之後，您可以登入您的電子郵件和新密碼。</span><span class="sxs-lookup"><span data-stu-id="42358-215">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="42358-216">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="42358-216">Debug email</span></span>

<span data-ttu-id="42358-217">如果您無法收到電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="42358-217">If you can't get email working:</span></span>

* <span data-ttu-id="42358-218">在 設定中斷點`EmailSender.Execute`若要確認`SendGridClient.SendEmailAsync`呼叫。</span><span class="sxs-lookup"><span data-stu-id="42358-218">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="42358-219">建立[主控台應用程式以傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用類似的程式碼以`EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="42358-219">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="42358-220">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="42358-220">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="42358-221">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="42358-221">Check your spam folder.</span></span>
* <span data-ttu-id="42358-222">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="42358-222">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="42358-223">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="42358-223">Try sending to different email accounts.</span></span>

<span data-ttu-id="42358-224">**安全性最佳作法**旨在**不**使用生產環境中開發和測試的祕密。</span><span class="sxs-lookup"><span data-stu-id="42358-224">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="42358-225">如果您將應用程式發佈至 Azure 時，您可以為 Azure Web 應用程式入口網站中的應用程式設定來設定 SendGrid 祕密。</span><span class="sxs-lookup"><span data-stu-id="42358-225">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="42358-226">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="42358-226">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="42358-227">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="42358-227">Combine social and local login accounts</span></span>

<span data-ttu-id="42358-228">若要完成本節中，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="42358-228">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="42358-229">請參閱[Facebook、 Google、 與外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="42358-229">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="42358-230">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="42358-230">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="42358-231">依下列順序，"RickAndMSFT@gmail.com」 會先建立做為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後新增的本機登入。</span><span class="sxs-lookup"><span data-stu-id="42358-231">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="42358-233">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="42358-233">Click on the **Manage** link.</span></span> <span data-ttu-id="42358-234">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="42358-234">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="42358-236">按一下連結至另一個登入服務並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="42358-236">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="42358-237">在下圖中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="42358-237">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入檢視，列出 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="42358-239">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="42358-239">The two accounts have been combined.</span></span> <span data-ttu-id="42358-240">您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="42358-240">You are able to log on with either account.</span></span> <span data-ttu-id="42358-241">您可能想要新增本機帳戶，以免其社交登入驗證服務已關閉，或它們可能比較無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="42358-241">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="42358-242">站台後為使用者啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="42358-242">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="42358-243">在網站上啟用的帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="42358-243">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="42358-244">現有使用者遭到鎖定中，這是因為未確認他們的帳戶。</span><span class="sxs-lookup"><span data-stu-id="42358-244">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="42358-245">若要解決現有的使用者鎖定，請使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="42358-245">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="42358-246">更新將標示為正在確認所有現有的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="42358-246">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="42358-247">請確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="42358-247">Confirm exiting users.</span></span> <span data-ttu-id="42358-248">例如，批次傳送電子郵件以確認連結。</span><span class="sxs-lookup"><span data-stu-id="42358-248">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
