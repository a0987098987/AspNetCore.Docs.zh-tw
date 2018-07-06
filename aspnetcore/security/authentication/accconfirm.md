---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803268"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="a6daf-103">帳戶確認和 ASP.NET Core 中的密碼復原</span><span class="sxs-lookup"><span data-stu-id="a6daf-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="a6daf-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="a6daf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="a6daf-105">本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6daf-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="a6daf-106">本教學課程**不**開頭主題。</span><span class="sxs-lookup"><span data-stu-id="a6daf-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="a6daf-107">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="a6daf-107">You should be familiar with:</span></span>

* [<span data-ttu-id="a6daf-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6daf-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="a6daf-109">驗證</span><span class="sxs-lookup"><span data-stu-id="a6daf-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="a6daf-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a6daf-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="a6daf-111">請參閱[此 PDF 檔案](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC 1.1 和 2.x 版。</span><span class="sxs-lookup"><span data-stu-id="a6daf-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6daf-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="a6daf-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="a6daf-113">使用.NET Core CLI 建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="a6daf-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6daf-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="a6daf-115">`--auth Individual` 指定的個別使用者帳戶的專案範本。</span><span class="sxs-lookup"><span data-stu-id="a6daf-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="a6daf-116">在 Windows 中，新增`-uld`選項。</span><span class="sxs-lookup"><span data-stu-id="a6daf-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="a6daf-117">它會指定應該使用 LocalDB，而不是 SQLite。</span><span class="sxs-lookup"><span data-stu-id="a6daf-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="a6daf-118">執行`new mvc --help`來取得此命令的說明。</span><span class="sxs-lookup"><span data-stu-id="a6daf-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6daf-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a6daf-120">如果您使用 CLI 或 SQLite，執行下列命令在命令視窗中：</span><span class="sxs-lookup"><span data-stu-id="a6daf-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="a6daf-121">`--auth Individual` 指定的個別使用者帳戶的專案範本。</span><span class="sxs-lookup"><span data-stu-id="a6daf-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="a6daf-122">在 Windows 中，新增`-uld`選項。</span><span class="sxs-lookup"><span data-stu-id="a6daf-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="a6daf-123">它會指定應該使用 LocalDB，而不是 SQLite。</span><span class="sxs-lookup"><span data-stu-id="a6daf-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="a6daf-124">執行`new mvc --help`來取得此命令的說明。</span><span class="sxs-lookup"><span data-stu-id="a6daf-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="a6daf-125">或者，您也可以使用 Visual Studio 中建立新的 ASP.NET Core 專案：</span><span class="sxs-lookup"><span data-stu-id="a6daf-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="a6daf-126">在 Visual Studio 中，建立新**Web 應用程式**專案。</span><span class="sxs-lookup"><span data-stu-id="a6daf-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="a6daf-127">選取  **ASP.NET Core 2.0**。</span><span class="sxs-lookup"><span data-stu-id="a6daf-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="a6daf-128">**.NET core**選取在下圖中，但您可以選取 **.NET Framework**。</span><span class="sxs-lookup"><span data-stu-id="a6daf-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="a6daf-129">選取 **變更驗證**並將設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="a6daf-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="a6daf-130">保留預設值**存放區的使用者帳戶的應用程式內**。</span><span class="sxs-lookup"><span data-stu-id="a6daf-130">Keep the default **Store user accounts in-app**.</span></span>

![顯示 「 個別使用者帳戶選項 」 選取 [新增專案] 對話方塊](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="a6daf-132">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="a6daf-132">Test new user registration</span></span>

<span data-ttu-id="a6daf-133">執行應用程式，請選取**註冊**連結，並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6daf-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="a6daf-134">請依照下列指示執行 Entity Framework Core 移轉。</span><span class="sxs-lookup"><span data-stu-id="a6daf-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="a6daf-135">此時，唯一的驗證電子郵件是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="a6daf-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="a6daf-136">提交註冊之後, 您登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6daf-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="a6daf-137">稍後在教學課程中，因此新的使用者無法登入，直到已驗證電子郵件，被更新的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a6daf-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="a6daf-138">檢視識別資料庫</span><span class="sxs-lookup"><span data-stu-id="a6daf-138">View the Identity database</span></span>

<span data-ttu-id="a6daf-139">請參閱[ASP.NET Core MVC 專案中使用 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)如需有關如何檢視 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="a6daf-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="a6daf-140">Visual studio:</span><span class="sxs-lookup"><span data-stu-id="a6daf-140">For Visual Studio:</span></span>

* <span data-ttu-id="a6daf-141">從**檢視**功能表上，選取**SQL Server 物件總管**(SSOX)。</span><span class="sxs-lookup"><span data-stu-id="a6daf-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="a6daf-142">瀏覽至 **(localdb) (SQL Server 13) MSSQLLocalDB**。</span><span class="sxs-lookup"><span data-stu-id="a6daf-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="a6daf-143">以滑鼠右鍵按一下**dbo。AspNetUsers** > **檢視資料**:</span><span class="sxs-lookup"><span data-stu-id="a6daf-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server 物件總管] 中的 [AspNetUsers 資料表上的操作功能表](accconfirm/_static/ssox.png)

<span data-ttu-id="a6daf-145">請注意，資料表的`EmailConfirmed`欄位是`False`。</span><span class="sxs-lookup"><span data-stu-id="a6daf-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="a6daf-146">您可能想要這封電子郵件一次的下一個步驟時使用的應用程式會傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="a6daf-147">以滑鼠右鍵按一下資料列，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="a6daf-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="a6daf-148">刪除電子郵件別名更容易在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="a6daf-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="a6daf-149">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="a6daf-149">Require HTTPS</span></span>

<span data-ttu-id="a6daf-150">請參閱[需要 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="a6daf-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="a6daf-151">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="a6daf-151">Require email confirmation</span></span>

<span data-ttu-id="a6daf-152">您最好確認新的使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="a6daf-153">電子郵件確認可協助您確認它們無法模擬其他人 （也就是尚未註冊使用其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="a6daf-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="a6daf-154">假設您有討論論壇，而且您想要防止 「yli@example.com"中註冊為 「nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="a6daf-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="a6daf-155">而不需要電子郵件確認"nolivetto@contoso.com」 無法從您的應用程式收到不想要的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="a6daf-156">假設使用者不小心註冊為 「ylo@example.com"並還沒有發現..."yli 」 的拼字錯誤。</span><span class="sxs-lookup"><span data-stu-id="a6daf-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="a6daf-157">他們將無法使用密碼復原，因為應用程式沒有正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="a6daf-158">電子郵件確認 bot 提供有限的保護。</span><span class="sxs-lookup"><span data-stu-id="a6daf-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="a6daf-159">電子郵件確認不會提供保護，防範惡意使用者與許多電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6daf-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="a6daf-160">您通常想要防止新使用者之前確認電子郵件張貼到您的網站的任何資料。</span><span class="sxs-lookup"><span data-stu-id="a6daf-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="a6daf-161">更新`ConfigureServices`要求確認電子郵件：</span><span class="sxs-lookup"><span data-stu-id="a6daf-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="a6daf-162">`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認其電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="a6daf-163">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="a6daf-163">Configure email provider</span></span>

<span data-ttu-id="a6daf-164">在本教學課程中，用來傳送電子郵件的 SendGrid。</span><span class="sxs-lookup"><span data-stu-id="a6daf-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="a6daf-165">您需要的 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="a6daf-166">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="a6daf-166">You can use other email providers.</span></span> <span data-ttu-id="a6daf-167">ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="a6daf-168">我們建議您可以使用 SendGrid 或另一個電子郵件服務傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="a6daf-169">SMTP 是難以保護，並正確設定。</span><span class="sxs-lookup"><span data-stu-id="a6daf-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="a6daf-170">[選項模式](xref:fundamentals/configuration/options)用來存取使用者帳戶和金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="a6daf-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="a6daf-171">如需詳細資訊，請參閱 <<c0> [ 組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="a6daf-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="a6daf-172">建立類別來擷取安全的電子郵件的金鑰。</span><span class="sxs-lookup"><span data-stu-id="a6daf-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="a6daf-173">此範例中，`AuthMessageSenderOptions`類別中建立*Services/AuthMessageSenderOptions.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="a6daf-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="a6daf-174">設定`SendGridUser`並`SendGridKey`具有[secret manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="a6daf-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="a6daf-175">例如: </span><span class="sxs-lookup"><span data-stu-id="a6daf-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="a6daf-176">在 Windows、 Secret Manager 儲存中的索引鍵/值組*secrets.json*檔案中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。</span><span class="sxs-lookup"><span data-stu-id="a6daf-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="a6daf-177">內容*secrets.json*檔案未加密。</span><span class="sxs-lookup"><span data-stu-id="a6daf-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="a6daf-178">*Secrets.json*如下所示的檔案 (`SendGridKey`已移除值。)</span><span class="sxs-lookup"><span data-stu-id="a6daf-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="a6daf-179">設定啟動，以使用 AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="a6daf-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="a6daf-180">新增`AuthMessageSenderOptions`至服務容器的結尾`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="a6daf-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6daf-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6daf-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="a6daf-183">設定 AuthMessageSender 類別</span><span class="sxs-lookup"><span data-stu-id="a6daf-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="a6daf-184">本教學課程示範如何將透過電子郵件通知[SendGrid](https://sendgrid.com/)，但您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="a6daf-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="a6daf-185">安裝`SendGrid`NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="a6daf-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="a6daf-186">從命令列：</span><span class="sxs-lookup"><span data-stu-id="a6daf-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="a6daf-187">從 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="a6daf-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="a6daf-188">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)報名免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6daf-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="a6daf-189">設定 SendGrid</span><span class="sxs-lookup"><span data-stu-id="a6daf-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6daf-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a6daf-191">若要設定 SendGrid，新增類似下列的程式碼*Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6daf-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6daf-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="a6daf-193">將程式碼中的加入*Services/MessageServices.cs*如下所示設定 SendGrid:</span><span class="sxs-lookup"><span data-stu-id="a6daf-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="a6daf-194">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="a6daf-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="a6daf-195">範本會將程式碼進行帳戶確認和密碼復原。</span><span class="sxs-lookup"><span data-stu-id="a6daf-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="a6daf-196">尋找`OnPostAsync`方法中的*Pages/Account/Register.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="a6daf-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6daf-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a6daf-198">防止新註冊的使用者自動記錄的標記為註解下面這一行：</span><span class="sxs-lookup"><span data-stu-id="a6daf-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="a6daf-199">使用已變更反白顯示的列，會顯示完整的方法：</span><span class="sxs-lookup"><span data-stu-id="a6daf-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6daf-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a6daf-201">若要啟用帳戶確認，取消註解下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="a6daf-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="a6daf-202">**注意：** 程式碼，導致無法自動記錄的標記為註解下面這行的新註冊的使用者：</span><span class="sxs-lookup"><span data-stu-id="a6daf-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="a6daf-203">取消註解中的程式碼中啟用密碼復原`ForgotPassword`動作*controllers/Accountcontroller.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6daf-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="a6daf-204">取消註解中的表單項目*Views/Account/ForgotPassword.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="a6daf-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="a6daf-205">您可能想要移除`<p> For more information on how to enable reset password ... </p>`元素，其中包含這篇文章的連結。</span><span class="sxs-lookup"><span data-stu-id="a6daf-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="a6daf-206">註冊、 確認電子郵件，以及重設密碼</span><span class="sxs-lookup"><span data-stu-id="a6daf-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="a6daf-207">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="a6daf-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="a6daf-208">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="a6daf-208">Run the app and register a new user</span></span>

  ![Web 應用程式註冊帳戶檢視](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="a6daf-210">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="a6daf-211">請參閱[偵錯電子郵件](#debug)如果您沒有收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="a6daf-212">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="a6daf-213">登入您的電子郵件和密碼。</span><span class="sxs-lookup"><span data-stu-id="a6daf-213">Log in with your email and password.</span></span>
* <span data-ttu-id="a6daf-214">登出。</span><span class="sxs-lookup"><span data-stu-id="a6daf-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="a6daf-215">檢視 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="a6daf-215">View the manage page</span></span>

<span data-ttu-id="a6daf-216">在瀏覽器中，選取您的使用者名稱：![瀏覽器視窗中的使用使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="a6daf-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="a6daf-217">您可能需要展開的導覽列，以查看使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="a6daf-217">You might need to expand the navbar to see user name.</span></span>

![導覽列](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6daf-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a6daf-220">[管理] 頁面會顯示**設定檔**選取的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="a6daf-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="a6daf-221">**電子郵件**顯示核取方塊，指出電子郵件已確認。</span><span class="sxs-lookup"><span data-stu-id="a6daf-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![管理頁面](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6daf-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6daf-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a6daf-224">稍後在本教學課程中，這是所述。</span><span class="sxs-lookup"><span data-stu-id="a6daf-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="a6daf-225">![管理頁面](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="a6daf-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="a6daf-226">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="a6daf-226">Test password reset</span></span>

* <span data-ttu-id="a6daf-227">如果您的登入，請選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="a6daf-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="a6daf-228">選取 **登入**連結，然後選取**忘記密碼？** 連結。</span><span class="sxs-lookup"><span data-stu-id="a6daf-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="a6daf-229">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="a6daf-230">會傳送具有重設密碼連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a6daf-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="a6daf-231">請檢查您的電子郵件，然後按一下 重設密碼連結。</span><span class="sxs-lookup"><span data-stu-id="a6daf-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="a6daf-232">已成功重設您的密碼之後，您可以登入您的電子郵件和新密碼。</span><span class="sxs-lookup"><span data-stu-id="a6daf-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="a6daf-233">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="a6daf-233">Debug email</span></span>

<span data-ttu-id="a6daf-234">如果您無法收到電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="a6daf-234">If you can't get email working:</span></span>

* <span data-ttu-id="a6daf-235">建立[主控台應用程式以傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)。</span><span class="sxs-lookup"><span data-stu-id="a6daf-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="a6daf-236">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="a6daf-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="a6daf-237">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="a6daf-237">Check your spam folder.</span></span>
* <span data-ttu-id="a6daf-238">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="a6daf-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="a6daf-239">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6daf-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="a6daf-240">**安全性最佳作法**旨在**不**使用生產環境中開發和測試的祕密。</span><span class="sxs-lookup"><span data-stu-id="a6daf-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="a6daf-241">如果您將應用程式發佈至 Azure 時，您可以為 Azure Web 應用程式入口網站中的應用程式設定來設定 SendGrid 祕密。</span><span class="sxs-lookup"><span data-stu-id="a6daf-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="a6daf-242">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="a6daf-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="a6daf-243">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="a6daf-243">Combine social and local login accounts</span></span>

<span data-ttu-id="a6daf-244">若要完成本節中，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="a6daf-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="a6daf-245">請參閱[Facebook、 Google、 與外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="a6daf-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a6daf-246">您可以結合本機和社交帳戶，按一下您的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="a6daf-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="a6daf-247">依下列順序，"RickAndMSFT@gmail.com」 會先建立做為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後新增的本機登入。</span><span class="sxs-lookup"><span data-stu-id="a6daf-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="a6daf-249">按一下 **管理**連結。</span><span class="sxs-lookup"><span data-stu-id="a6daf-249">Click on the **Manage** link.</span></span> <span data-ttu-id="a6daf-250">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="a6daf-250">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="a6daf-252">按一下連結至另一個登入服務並接受應用程式要求。</span><span class="sxs-lookup"><span data-stu-id="a6daf-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="a6daf-253">在下圖中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="a6daf-253">In the following image, Facebook is the external authentication provider:</span></span>

![管理您的外部登入檢視，列出 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="a6daf-255">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6daf-255">The two accounts have been combined.</span></span> <span data-ttu-id="a6daf-256">您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="a6daf-256">You are able to log on with either account.</span></span> <span data-ttu-id="a6daf-257">您可能想要新增本機帳戶，以免其社交登入驗證服務已關閉，或它們可能比較無法存取其社交帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="a6daf-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="a6daf-258">站台後為使用者啟用帳戶確認</span><span class="sxs-lookup"><span data-stu-id="a6daf-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="a6daf-259">在網站上啟用的帳戶確認，使用者會鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6daf-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="a6daf-260">現有使用者遭到鎖定中，這是因為未確認他們的帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6daf-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="a6daf-261">若要解決現有的使用者鎖定，請使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="a6daf-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="a6daf-262">更新將標示為正在確認所有現有的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="a6daf-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="a6daf-263">請確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="a6daf-263">Confirm exiting users.</span></span> <span data-ttu-id="a6daf-264">例如，批次傳送電子郵件以確認連結。</span><span class="sxs-lookup"><span data-stu-id="a6daf-264">For example, batch-send emails with confirmation links.</span></span>
