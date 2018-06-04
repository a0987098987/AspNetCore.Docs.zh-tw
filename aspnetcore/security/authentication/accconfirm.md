---
title: 帳戶確認和 ASP.NET Core 中的密碼復原
author: rick-anderson
description: 了解如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b6dbe234973431448c18d3cc82a6ac98d4f53a3b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730447"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="3f868-103">帳戶確認和 ASP.NET Core 中的密碼復原</span><span class="sxs-lookup"><span data-stu-id="3f868-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="3f868-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3f868-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="3f868-105">本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f868-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="3f868-106">這個教學課程**不**開頭主題。</span><span class="sxs-lookup"><span data-stu-id="3f868-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="3f868-107">您應該熟悉：</span><span class="sxs-lookup"><span data-stu-id="3f868-107">You should be familiar with:</span></span>

* [<span data-ttu-id="3f868-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f868-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="3f868-109">驗證</span><span class="sxs-lookup"><span data-stu-id="3f868-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="3f868-110">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="3f868-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="3f868-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3f868-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="3f868-112">請參閱[此 PDF 檔案](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC 1.1 和 2.x 版。</span><span class="sxs-lookup"><span data-stu-id="3f868-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f868-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="3f868-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="3f868-114">建立新的 ASP.NET Core 專案，而.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3f868-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3f868-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3f868-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="3f868-116">`--auth Individual` 指定個別的使用者帳戶的專案範本。</span><span class="sxs-lookup"><span data-stu-id="3f868-116">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="3f868-117">在 Windows 中，加入`-uld`選項。</span><span class="sxs-lookup"><span data-stu-id="3f868-117">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="3f868-118">它會指定應該使用 LocalDB，而不是 SQLite。</span><span class="sxs-lookup"><span data-stu-id="3f868-118">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="3f868-119">執行`new mvc --help`取得此命令的說明。</span><span class="sxs-lookup"><span data-stu-id="3f868-119">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3f868-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3f868-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3f868-121">如果您使用 CLI 或 SQLite，在命令視窗執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f868-121">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="3f868-122">`--auth Individual` 指定個別的使用者帳戶的專案範本。</span><span class="sxs-lookup"><span data-stu-id="3f868-122">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="3f868-123">在 Windows 中，加入`-uld`選項。</span><span class="sxs-lookup"><span data-stu-id="3f868-123">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="3f868-124">它會指定應該使用 LocalDB，而不是 SQLite。</span><span class="sxs-lookup"><span data-stu-id="3f868-124">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="3f868-125">執行`new mvc --help`取得此命令的說明。</span><span class="sxs-lookup"><span data-stu-id="3f868-125">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="3f868-126">或者，您可以使用 Visual Studio 建立新的 ASP.NET Core 專案：</span><span class="sxs-lookup"><span data-stu-id="3f868-126">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="3f868-127">在 Visual Studio 中，建立新**Web 應用程式**專案。</span><span class="sxs-lookup"><span data-stu-id="3f868-127">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="3f868-128">選取**ASP.NET Core 2.0**。</span><span class="sxs-lookup"><span data-stu-id="3f868-128">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="3f868-129">**.NET core**選取在下列影像中，但您可以選取 **.NET Framework**。</span><span class="sxs-lookup"><span data-stu-id="3f868-129">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="3f868-130">選取**變更驗證**並將設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="3f868-130">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="3f868-131">保留預設值**儲存使用者帳戶在應用程式**。</span><span class="sxs-lookup"><span data-stu-id="3f868-131">Keep the default **Store user accounts in-app**.</span></span>

![顯示選取"個別使用者帳戶 radio"的新 [專案] 對話方塊](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="3f868-133">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="3f868-133">Test new user registration</span></span>

<span data-ttu-id="3f868-134">執行應用程式中，選取**註冊**連結，並登錄使用者。</span><span class="sxs-lookup"><span data-stu-id="3f868-134">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="3f868-135">請依照下列指示執行 Entity Framework Core 移轉。</span><span class="sxs-lookup"><span data-stu-id="3f868-135">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="3f868-136">此時，只有在電子郵件時才驗證與[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="3f868-136">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="3f868-137">在提交註冊之後, 您登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f868-137">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="3f868-138">稍後在教學課程中，會更新程式碼，所以新的使用者無法登入，直到他們的電子郵件已通過驗證。</span><span class="sxs-lookup"><span data-stu-id="3f868-138">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="3f868-139">檢視識別資料庫</span><span class="sxs-lookup"><span data-stu-id="3f868-139">View the Identity database</span></span>

<span data-ttu-id="3f868-140">請參閱[ASP.NET Core MVC 專案中使用 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)如需如何檢視 SQLite 資料庫的指示。</span><span class="sxs-lookup"><span data-stu-id="3f868-140">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="3f868-141">Visual studio:</span><span class="sxs-lookup"><span data-stu-id="3f868-141">For Visual Studio:</span></span>

* <span data-ttu-id="3f868-142">從**檢視**功能表上，選取**SQL Server 物件總管**(SSOX)。</span><span class="sxs-lookup"><span data-stu-id="3f868-142">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="3f868-143">瀏覽至 **(localdb) (SQL Server 13) MSSQLLocalDB**。</span><span class="sxs-lookup"><span data-stu-id="3f868-143">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="3f868-144">以滑鼠右鍵按一下**dbo。AspNetUsers** > **檢視資料**:</span><span class="sxs-lookup"><span data-stu-id="3f868-144">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server 物件總管 中的 AspNetUsers 資料表上的內容功能表](accconfirm/_static/ssox.png)

<span data-ttu-id="3f868-146">請注意，資料表的`EmailConfirmed`欄位是`False`。</span><span class="sxs-lookup"><span data-stu-id="3f868-146">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="3f868-147">您可以在這封電子郵件一次在下一個步驟時使用應用程式會傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-147">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="3f868-148">以滑鼠右鍵按一下資料列，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="3f868-148">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="3f868-149">刪除電子郵件別名容易在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="3f868-149">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="3f868-150">需要 HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f868-150">Require HTTPS</span></span>

<span data-ttu-id="3f868-151">請參閱[需要 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="3f868-151">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="3f868-152">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="3f868-152">Require email confirmation</span></span>

<span data-ttu-id="3f868-153">最好確認新的使用者註冊的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-153">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="3f868-154">確認可協助確認它們不模擬其他人的電子郵件 （也就是它們未向其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="3f868-154">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="3f868-155">假設您有討論論壇，而且您想要防止 「yli@example.com"從登錄為"nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="3f868-155">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="3f868-156">電子郵件確認沒有 「nolivetto@contoso.com"無法從您的應用程式接收垃圾電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-156">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="3f868-157">假設使用者不小心註冊為 「ylo@example.com"而且未注意到的"yli 」 的拼字錯誤。</span><span class="sxs-lookup"><span data-stu-id="3f868-157">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="3f868-158">它們就無法使用密碼復原，因為應用程式沒有正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-158">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="3f868-159">電子郵件確認從 bot 提供有限的保護。</span><span class="sxs-lookup"><span data-stu-id="3f868-159">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="3f868-160">電子郵件確認不提供保護，防範惡意使用者與多個電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f868-160">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="3f868-161">您通常想要讓新的使用者張貼到您的網站的任何資料之前確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-161">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="3f868-162">更新`ConfigureServices`要求確認電子郵件：</span><span class="sxs-lookup"><span data-stu-id="3f868-162">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="3f868-163">`config.SignIn.RequireConfirmedEmail = true;` 防止已註冊的使用者登入，直到確認他們的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-163">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="3f868-164">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="3f868-164">Configure email provider</span></span>

<span data-ttu-id="3f868-165">在本教學課程，SendGrid 用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-165">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="3f868-166">您需要 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-166">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="3f868-167">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="3f868-167">You can use other email providers.</span></span> <span data-ttu-id="3f868-168">ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-168">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="3f868-169">我們建議您傳送電子郵件使用 SendGrid 或另一個電子郵件服務。</span><span class="sxs-lookup"><span data-stu-id="3f868-169">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="3f868-170">SMTP 是難以保護，並正確設定。</span><span class="sxs-lookup"><span data-stu-id="3f868-170">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="3f868-171">[選項模式](xref:fundamentals/configuration/options)用來存取使用者帳戶和金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="3f868-171">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="3f868-172">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="3f868-172">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="3f868-173">建立可擷取保護電子郵件的索引鍵的類別。</span><span class="sxs-lookup"><span data-stu-id="3f868-173">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="3f868-174">此範例中，`AuthMessageSenderOptions`中建立類別*Services/AuthMessageSenderOptions.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="3f868-174">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="3f868-175">設定`SendGridUser`和`SendGridKey`與[密碼管理員工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="3f868-175">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="3f868-176">例如: </span><span class="sxs-lookup"><span data-stu-id="3f868-176">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="3f868-177">在 Windows 中，密碼管理員存放中的索引鍵/值組*secrets.json*檔案`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目錄。</span><span class="sxs-lookup"><span data-stu-id="3f868-177">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="3f868-178">內容*secrets.json*檔案未加密。</span><span class="sxs-lookup"><span data-stu-id="3f868-178">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="3f868-179">*Secrets.json*檔案如下所示 (`SendGridKey`已移除值。)</span><span class="sxs-lookup"><span data-stu-id="3f868-179">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="3f868-180">設定要使用 AuthMessageSenderOptions 啟動</span><span class="sxs-lookup"><span data-stu-id="3f868-180">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="3f868-181">新增`AuthMessageSenderOptions`至結尾處的服務容器`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="3f868-181">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3f868-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3f868-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3f868-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3f868-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="3f868-184">設定 AuthMessageSender 類別</span><span class="sxs-lookup"><span data-stu-id="3f868-184">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="3f868-185">本教學課程示範如何透過電子郵件通知加入[SendGrid](https://sendgrid.com/)，不過您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="3f868-185">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="3f868-186">安裝`SendGrid`NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="3f868-186">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="3f868-187">從命令列：</span><span class="sxs-lookup"><span data-stu-id="3f868-187">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="3f868-188">從 [封裝管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f868-188">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="3f868-189">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f868-189">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="3f868-190">設定 SendGrid</span><span class="sxs-lookup"><span data-stu-id="3f868-190">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3f868-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3f868-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3f868-192">若要設定 SendGrid，將類似下列的程式碼加入*Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f868-192">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3f868-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3f868-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="3f868-194">將程式碼中的加入*Services/MessageServices.cs*如下所示設定 SendGrid:</span><span class="sxs-lookup"><span data-stu-id="3f868-194">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="3f868-195">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="3f868-195">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="3f868-196">範本的帳戶確認和密碼復原程式碼。</span><span class="sxs-lookup"><span data-stu-id="3f868-196">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="3f868-197">尋找`OnPostAsync`方法中的*Pages/Account/Register.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="3f868-197">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3f868-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3f868-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3f868-199">防止新註冊的使用者自動登入註解下列一行：</span><span class="sxs-lookup"><span data-stu-id="3f868-199">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3f868-200">反白顯示的已變更的程式行顯示完整的方法：</span><span class="sxs-lookup"><span data-stu-id="3f868-200">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3f868-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3f868-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3f868-202">若要啟用帳戶確認，請取消註解下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="3f868-202">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="3f868-203">**注意：** 程式碼會使得新註冊的使用者無法自動登入註解下列一行：</span><span class="sxs-lookup"><span data-stu-id="3f868-203">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3f868-204">取消註解中的程式碼中啟用密碼復原`ForgotPassword`動作*Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f868-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="3f868-205">請取消註解中的表單項目*Views/Account/ForgotPassword.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="3f868-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="3f868-206">您可能想要移除`<p> For more information on how to enable reset password ... </p>`元素，其中包含這篇文章的連結。</span><span class="sxs-lookup"><span data-stu-id="3f868-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="3f868-207">註冊、 確認電子郵件，及重設密碼</span><span class="sxs-lookup"><span data-stu-id="3f868-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="3f868-208">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="3f868-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="3f868-209">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="3f868-209">Run the app and register a new user</span></span>

  ![Web 應用程式註冊帳戶檢視](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="3f868-211">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="3f868-212">請參閱[偵錯電子郵件](#debug)如果您未收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="3f868-213">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="3f868-214">登入您的電子郵件和密碼。</span><span class="sxs-lookup"><span data-stu-id="3f868-214">Log in with your email and password.</span></span>
* <span data-ttu-id="3f868-215">登出。</span><span class="sxs-lookup"><span data-stu-id="3f868-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="3f868-216">檢視 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="3f868-216">View the manage page</span></span>

<span data-ttu-id="3f868-217">在瀏覽器中選取您的使用者名稱：![瀏覽器視窗中使用的使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="3f868-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="3f868-218">您可能需要展開以查看使用者名稱導覽列。</span><span class="sxs-lookup"><span data-stu-id="3f868-218">You might need to expand the navbar to see user name.</span></span>

![導覽列](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3f868-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3f868-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3f868-221">[管理] 頁面會顯示與**設定檔**選取的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="3f868-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="3f868-222">**電子郵件**顯示核取方塊，指出電子郵件已確認。</span><span class="sxs-lookup"><span data-stu-id="3f868-222">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![管理頁面](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3f868-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3f868-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3f868-225">這是稍後在本教學課程中所述。</span><span class="sxs-lookup"><span data-stu-id="3f868-225">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="3f868-226">![管理頁面](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="3f868-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="3f868-227">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="3f868-227">Test password reset</span></span>

* <span data-ttu-id="3f868-228">如果您登入，選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="3f868-228">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="3f868-229">選取**登入**連結，並選取**忘記密碼？** 連結。</span><span class="sxs-lookup"><span data-stu-id="3f868-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="3f868-230">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="3f868-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="3f868-231">會傳送一封電子郵件以重設密碼的連結。</span><span class="sxs-lookup"><span data-stu-id="3f868-231">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="3f868-232">請檢查您的電子郵件並按一下連結以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="3f868-232">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="3f868-233">已成功重設您的密碼之後，您可以登入您的電子郵件和新密碼。</span><span class="sxs-lookup"><span data-stu-id="3f868-233">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="3f868-234">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="3f868-234">Debug email</span></span>

<span data-ttu-id="3f868-235">如果您無法取得電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="3f868-235">If you can't get email working:</span></span>

* <span data-ttu-id="3f868-236">建立[主控台應用程式傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)。</span><span class="sxs-lookup"><span data-stu-id="3f868-236">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="3f868-237">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="3f868-237">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="3f868-238">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f868-238">Check your spam folder.</span></span>
* <span data-ttu-id="3f868-239">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="3f868-239">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="3f868-240">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f868-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="3f868-241">**安全性最佳作法**是**不**使用生產環境中測試和開發的密碼。</span><span class="sxs-lookup"><span data-stu-id="3f868-241">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="3f868-242">如果您將應用程式發行至 Azure 時，您可以設定 SendGrid 密碼做為 Azure Web 應用程式入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="3f868-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3f868-243">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="3f868-243">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3f868-244">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="3f868-244">Combine social and local login accounts</span></span>

<span data-ttu-id="3f868-245">若要完成本章節，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="3f868-245">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="3f868-246">請參閱[Facebook、 Google、 和外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="3f868-246">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="3f868-247">您可以結合本機和社交帳戶按一下電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="3f868-247">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3f868-248">以下列順序，在"RickAndMSFT@gmail.com"第一次建立為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後加入本機的登入。</span><span class="sxs-lookup"><span data-stu-id="3f868-248">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="3f868-250">按一下**管理**連結。</span><span class="sxs-lookup"><span data-stu-id="3f868-250">Click on the **Manage** link.</span></span> <span data-ttu-id="3f868-251">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="3f868-251">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="3f868-253">按一下連結以另一個登入服務，並接受要求的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f868-253">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="3f868-254">在下圖中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="3f868-254">In the following image, Facebook is the external authentication provider:</span></span>

![管理您列出 Facebook 的外部登入檢視](accconfirm/_static/fb.png)

<span data-ttu-id="3f868-256">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f868-256">The two accounts have been combined.</span></span> <span data-ttu-id="3f868-257">也可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="3f868-257">You are able to log on with either account.</span></span> <span data-ttu-id="3f868-258">您可能想要其社交登入驗證服務已關閉，或可能比較他們遺失了存取其社交帳戶加入本機帳戶使用者。</span><span class="sxs-lookup"><span data-stu-id="3f868-258">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="3f868-259">啟用帳戶確認之後某個網站的使用者</span><span class="sxs-lookup"><span data-stu-id="3f868-259">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="3f868-260">啟用帳戶確認站台的使用者與鎖定所有現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="3f868-260">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="3f868-261">現有的使用者遭到鎖定，因為未確認他們的帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f868-261">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="3f868-262">若要解決現有的使用者鎖定，請使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="3f868-262">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="3f868-263">若要將標示為正在確認的所有現有使用者資料庫更新。</span><span class="sxs-lookup"><span data-stu-id="3f868-263">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="3f868-264">請確認現有的使用者。</span><span class="sxs-lookup"><span data-stu-id="3f868-264">Confirm exiting users.</span></span> <span data-ttu-id="3f868-265">例如，批次傳送電子郵件以確認連結。</span><span class="sxs-lookup"><span data-stu-id="3f868-265">For example, batch-send emails with confirmation links.</span></span>
