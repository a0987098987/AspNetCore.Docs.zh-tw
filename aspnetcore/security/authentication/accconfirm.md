---
title: "帳戶確認和 ASP.NET Core 中的密碼復原"
author: rick-anderson
description: "示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。"
keywords: "ASP.NET Core 重設密碼，電子郵件確認時，安全性"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: aaed75c78a99e59954add959a76a2fd68ea5f3fc
ms.sourcegitcommit: f2fb0b45284e4f8c4a9c422bec790aede7c1f0ac
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2017
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="94074-104">帳戶確認和 ASP.NET Core 中的密碼復原</span><span class="sxs-lookup"><span data-stu-id="94074-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="94074-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="94074-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="94074-106">本教學課程會示範如何建置使用電子郵件確認和密碼重設的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="94074-106">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="94074-107">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="94074-107">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94074-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94074-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="94074-109">這個步驟適用於在 Windows 上的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="94074-109">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="94074-110">請參閱下一節的 CLI 指示。</span><span class="sxs-lookup"><span data-stu-id="94074-110">See the next section for CLI instructions.</span></span>

<span data-ttu-id="94074-111">此教學課程需要 Visual Studio 2017 Preview 2 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="94074-111">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="94074-112">在 Visual Studio 中建立新的 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="94074-112">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="94074-113">選取**ASP.NET Core 2.0**。</span><span class="sxs-lookup"><span data-stu-id="94074-113">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="94074-114">下圖顯示**.NET Core**選取，但您可以選取**.NET Framework**。</span><span class="sxs-lookup"><span data-stu-id="94074-114">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="94074-115">選取**變更驗證**並將設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="94074-115">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="94074-116">保留預設值**儲存使用者帳戶在應用程式**。</span><span class="sxs-lookup"><span data-stu-id="94074-116">Keep the default **Store user accounts in-app**.</span></span>

![顯示選取"個別使用者帳戶 radio"的新 [專案] 對話方塊](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94074-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94074-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="94074-119">此教學課程需要 2017年或更新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="94074-119">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="94074-120">在 Visual Studio 中建立新的 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="94074-120">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="94074-121">選取**變更驗證**並將設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="94074-121">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![顯示選取"個別使用者帳戶 radio"的新 [專案] 對話方塊](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="94074-123">.NET core CLI 專案建立 macOS 和 Linux</span><span class="sxs-lookup"><span data-stu-id="94074-123">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="94074-124">如果您使用 CLI 或 SQLite，在命令視窗執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="94074-124">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="94074-125">`--auth Individual`指定的個別使用者帳戶的範本。</span><span class="sxs-lookup"><span data-stu-id="94074-125">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="94074-126">在 Windows 中，加入`-uld`選項。</span><span class="sxs-lookup"><span data-stu-id="94074-126">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="94074-127">`-uld`選項建立 LocalDB 的連接字串，而不是 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="94074-127">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="94074-128">執行`new mvc --help`取得此命令的說明。</span><span class="sxs-lookup"><span data-stu-id="94074-128">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="94074-129">測試新的使用者註冊</span><span class="sxs-lookup"><span data-stu-id="94074-129">Test new user registration</span></span>

<span data-ttu-id="94074-130">執行應用程式中，選取**註冊**連結，並登錄使用者。</span><span class="sxs-lookup"><span data-stu-id="94074-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="94074-131">請依照下列指示執行 Entity Framework Core 移轉。</span><span class="sxs-lookup"><span data-stu-id="94074-131">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="94074-132">此時，只有在電子郵件時才驗證與[[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="94074-132">At this  point, the only validation on the email is with the [[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span> <span data-ttu-id="94074-133">提交註冊後，您登入的應用程式。</span><span class="sxs-lookup"><span data-stu-id="94074-133">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="94074-134">稍後在教學課程中，我們將會變更這讓新的使用者無法登入，直到他們的電子郵件已通過驗證。</span><span class="sxs-lookup"><span data-stu-id="94074-134">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="94074-135">檢視識別資料庫</span><span class="sxs-lookup"><span data-stu-id="94074-135">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="94074-136">SQL Server</span><span class="sxs-lookup"><span data-stu-id="94074-136">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="94074-137">從**檢視**功能表上，選取**SQL Server 物件總管**(SSOX)。</span><span class="sxs-lookup"><span data-stu-id="94074-137">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="94074-138">瀏覽至**(localdb) (SQL Server 13) MSSQLLocalDB**。</span><span class="sxs-lookup"><span data-stu-id="94074-138">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="94074-139">以滑鼠右鍵按一下**dbo。AspNetUsers** > **檢視資料**:</span><span class="sxs-lookup"><span data-stu-id="94074-139">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server 物件總管 中的 AspNetUsers 資料表上的內容功能表](accconfirm/_static/ssox.png)

<span data-ttu-id="94074-141">請注意`EmailConfirmed`欄位是`False`。</span><span class="sxs-lookup"><span data-stu-id="94074-141">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="94074-142">您可以在這封電子郵件一次在下一個步驟時使用應用程式會傳送確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-142">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="94074-143">以滑鼠右鍵按一下資料列，然後選取**刪除**。</span><span class="sxs-lookup"><span data-stu-id="94074-143">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="94074-144">刪除電子郵件別名現在更容易在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="94074-144">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="94074-145">SQLite</span><span class="sxs-lookup"><span data-stu-id="94074-145">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="94074-146">請參閱[在 ASP.NET Core MVC 專案中使用的 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)如需有關如何檢視 SQLite DB 指示。</span><span class="sxs-lookup"><span data-stu-id="94074-146">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="94074-147">需要 SSL 和 SSL 設定 IIS Express</span><span class="sxs-lookup"><span data-stu-id="94074-147">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="94074-148">請參閱[強制 SSL](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="94074-148">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="94074-149">需要電子郵件確認</span><span class="sxs-lookup"><span data-stu-id="94074-149">Require email confirmation</span></span>

<span data-ttu-id="94074-150">若要確認新的使用者註冊，以確認它們不會模擬其他人的電子郵件的最佳作法是 （也就是它們未向其他人的電子郵件）。</span><span class="sxs-lookup"><span data-stu-id="94074-150">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="94074-151">假設您有討論論壇，而且您想要防止 「yli@example.com"從登錄為"nolivetto@contoso.com。 」</span><span class="sxs-lookup"><span data-stu-id="94074-151">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="94074-152">電子郵件確認沒有 「nolivetto@contoso.com"無法從您的應用程式取得垃圾電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-152">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="94074-153">假設使用者不小心註冊為 「ylo@example.com"而且未注意到拼字錯誤的 「 yli 」，它們就無法使用密碼復原，因為應用程式沒有正確的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-153">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="94074-154">電子郵件確認從 bot 提供有限的保護，並不會從擁有多的工作電子郵件別名，他們可以使用登錄來決定垃圾提供保護。</span><span class="sxs-lookup"><span data-stu-id="94074-154">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="94074-155">您通常想要讓新的使用者張貼到您的網站的任何資料之前確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="94074-156">更新`ConfigureServices`要求確認電子郵件：</span><span class="sxs-lookup"><span data-stu-id="94074-156">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94074-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94074-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="94074-158">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span><span class="sxs-lookup"><span data-stu-id="94074-158">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94074-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94074-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="94074-160">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span><span class="sxs-lookup"><span data-stu-id="94074-160">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span></span>

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="94074-161">前一行會防止之前確認他們的電子郵件登入已註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="94074-161">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="94074-162">但是，該行不新使用者無法從它們在註冊後登入。</span><span class="sxs-lookup"><span data-stu-id="94074-162">However, that line does not prevent new users from being logged in after they register.</span></span> <span data-ttu-id="94074-163">它們在註冊後，預設程式碼會登入使用者。</span><span class="sxs-lookup"><span data-stu-id="94074-163">The default code logs in a user after they register.</span></span> <span data-ttu-id="94074-164">一旦他們登出他們將無法再次登入之前他們註冊。</span><span class="sxs-lookup"><span data-stu-id="94074-164">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="94074-165">稍後在教學課程中我們將會變更程式碼，因此新註冊的使用者是**不**登入。</span><span class="sxs-lookup"><span data-stu-id="94074-165">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="94074-166">設定電子郵件提供者</span><span class="sxs-lookup"><span data-stu-id="94074-166">Configure email provider</span></span>

<span data-ttu-id="94074-167">在本教學課程，SendGrid 用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-167">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="94074-168">您需要 SendGrid 帳戶和金鑰來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-168">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="94074-169">您可以使用其他電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="94074-169">You can use other email providers.</span></span> <span data-ttu-id="94074-170">ASP.NET Core 2.x 包含`System.Net.Mail`，可讓您從您的應用程式傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-170">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="94074-171">我們建議您傳送電子郵件使用 SendGrid 或另一個電子郵件服務。</span><span class="sxs-lookup"><span data-stu-id="94074-171">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="94074-172">[選項模式](xref:fundamentals/configuration#options-config-objects)用來存取使用者帳戶和金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="94074-172">The [Options pattern](xref:fundamentals/configuration#options-config-objects) is used to access the user account and key settings.</span></span> <span data-ttu-id="94074-173">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration#fundamentals-configuration)。</span><span class="sxs-lookup"><span data-stu-id="94074-173">For more information, see [configuration](xref:fundamentals/configuration#fundamentals-configuration).</span></span>

<span data-ttu-id="94074-174">建立可擷取保護電子郵件的索引鍵的類別。</span><span class="sxs-lookup"><span data-stu-id="94074-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="94074-175">此範例中，`AuthMessageSenderOptions`中建立類別*Services/AuthMessageSenderOptions.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="94074-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

<span data-ttu-id="94074-176">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="94074-176">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span></span>

<span data-ttu-id="94074-177">設定`SendGridUser`和`SendGridKey`與[密碼管理員工具](../app-secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="94074-177">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="94074-178">例如: </span><span class="sxs-lookup"><span data-stu-id="94074-178">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="94074-179">在 Windows 中，密碼管理員會儲存您的索引鍵/值組*secrets.json* %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId > 目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="94074-179">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="94074-180">內容*secrets.json*檔案不會加密。</span><span class="sxs-lookup"><span data-stu-id="94074-180">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="94074-181">*Secrets.json*檔案如下所示 (`SendGridKey`已移除值。)</span><span class="sxs-lookup"><span data-stu-id="94074-181">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="94074-182">設定要使用 AuthMessageSenderOptions 啟動</span><span class="sxs-lookup"><span data-stu-id="94074-182">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="94074-183">新增`AuthMessageSenderOptions`至結尾處的服務容器`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="94074-183">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94074-184">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94074-184">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="94074-185">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span><span class="sxs-lookup"><span data-stu-id="94074-185">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94074-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94074-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
<span data-ttu-id="94074-187">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span><span class="sxs-lookup"><span data-stu-id="94074-187">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span></span>

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="94074-188">設定 AuthMessageSender 類別</span><span class="sxs-lookup"><span data-stu-id="94074-188">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="94074-189">本教學課程示範如何透過電子郵件通知加入[SendGrid](https://sendgrid.com/)，不過您可以傳送電子郵件使用 SMTP 和其他機制。</span><span class="sxs-lookup"><span data-stu-id="94074-189">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="94074-190">安裝`SendGrid`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="94074-190">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="94074-191">從 [封裝管理員] 主控台中，輸入下列下列命令：</span><span class="sxs-lookup"><span data-stu-id="94074-191">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="94074-192">請參閱[免費開始使用 SendGrid](https://sendgrid.com/free/)註冊免費的 SendGrid 帳戶。</span><span class="sxs-lookup"><span data-stu-id="94074-192">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="94074-193">設定 SendGrid</span><span class="sxs-lookup"><span data-stu-id="94074-193">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94074-194">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94074-194">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="94074-195">將程式碼中的加入*Services/EmailSender.cs*如下所示設定 SendGrid:</span><span class="sxs-lookup"><span data-stu-id="94074-195">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="94074-196">[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span><span class="sxs-lookup"><span data-stu-id="94074-196">[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94074-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94074-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="94074-198">將程式碼中的加入*Services/MessageServices.cs*如下所示設定 SendGrid:</span><span class="sxs-lookup"><span data-stu-id="94074-198">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="94074-199">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span><span class="sxs-lookup"><span data-stu-id="94074-199">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span></span>

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="94074-200">啟用帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="94074-200">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="94074-201">範本的帳戶確認和密碼復原程式碼。</span><span class="sxs-lookup"><span data-stu-id="94074-201">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="94074-202">尋找`[HttpPost] Register`方法中的*AccountController.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="94074-202">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94074-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94074-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="94074-204">防止新註冊的使用者自動登入註解下列一行：</span><span class="sxs-lookup"><span data-stu-id="94074-204">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="94074-205">反白顯示的已變更的程式行顯示完整的方法：</span><span class="sxs-lookup"><span data-stu-id="94074-205">The complete method is shown with the changed line highlighted:</span></span>

<span data-ttu-id="94074-206">[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="94074-206">[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94074-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94074-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="94074-208">取消註解的程式碼，若要啟用帳戶確認。</span><span class="sxs-lookup"><span data-stu-id="94074-208">Uncomment the code to enable account confirmation.</span></span>

<span data-ttu-id="94074-209">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="94074-209">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span></span>

<span data-ttu-id="94074-210">注意： 我們正在也防止新註冊的使用者自動登入註解下列一行：</span><span class="sxs-lookup"><span data-stu-id="94074-210">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="94074-211">取消註解中的程式碼中啟用密碼復原`ForgotPassword`中的動作*Controllers/AccountController.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="94074-211">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

<span data-ttu-id="94074-212">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span><span class="sxs-lookup"><span data-stu-id="94074-212">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span></span>

<span data-ttu-id="94074-213">請取消註解中的表單項目*Views/Account/ForgotPassword.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="94074-213">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="94074-214">您可能想要移除`<p> For more information on how to enable reset password ... </p>`項目，其中包含這篇文章的連結。</span><span class="sxs-lookup"><span data-stu-id="94074-214">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

<span data-ttu-id="94074-215">[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span><span class="sxs-lookup"><span data-stu-id="94074-215">[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span></span>

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="94074-216">註冊、 確認電子郵件，及重設密碼</span><span class="sxs-lookup"><span data-stu-id="94074-216">Register, confirm email, and reset password</span></span>

<span data-ttu-id="94074-217">執行 web 應用程式，並測試的帳戶確認和密碼復原流程。</span><span class="sxs-lookup"><span data-stu-id="94074-217">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="94074-218">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="94074-218">Run the app and register a new user</span></span>

 ![Web 應用程式註冊帳戶檢視](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="94074-220">請檢查您的帳戶確認連結的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-220">Check your email for the account confirmation link.</span></span> <span data-ttu-id="94074-221">請參閱[偵錯電子郵件](#debug)如果您未收到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-221">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="94074-222">按一下連結，以確認您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-222">Click the link to confirm your email.</span></span>
* <span data-ttu-id="94074-223">登入您的電子郵件和密碼。</span><span class="sxs-lookup"><span data-stu-id="94074-223">Log in with your email and password.</span></span>
* <span data-ttu-id="94074-224">登出。</span><span class="sxs-lookup"><span data-stu-id="94074-224">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="94074-225">檢視 [管理] 頁面</span><span class="sxs-lookup"><span data-stu-id="94074-225">View the manage page</span></span>

<span data-ttu-id="94074-226">在瀏覽器中選取您的使用者名稱：![瀏覽器視窗中使用的使用者名稱](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="94074-226">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="94074-227">您可能需要展開以查看使用者名稱導覽列。</span><span class="sxs-lookup"><span data-stu-id="94074-227">You might need to expand the navbar to see user name.</span></span>

![導覽列](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94074-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94074-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="94074-230">[管理] 頁面會顯示與**設定檔**選取的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="94074-230">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="94074-231">**電子郵件**顯示核取方塊，指出電子郵件已確認。</span><span class="sxs-lookup"><span data-stu-id="94074-231">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![管理頁面](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94074-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94074-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="94074-234">稍後在本教學課程中，我們將討論此頁面。</span><span class="sxs-lookup"><span data-stu-id="94074-234">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="94074-235">![管理頁面](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="94074-235">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="94074-236">測試密碼重設</span><span class="sxs-lookup"><span data-stu-id="94074-236">Test password reset</span></span>

* <span data-ttu-id="94074-237">如果您登入，選取**登出**。</span><span class="sxs-lookup"><span data-stu-id="94074-237">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="94074-238">選取**登入**連結，並選取**忘記密碼？**連結。</span><span class="sxs-lookup"><span data-stu-id="94074-238">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="94074-239">輸入您用來註冊帳戶的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-239">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="94074-240">將會傳送一封電子郵件以重設密碼的連結。</span><span class="sxs-lookup"><span data-stu-id="94074-240">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="94074-241">請檢查您的電子郵件並按一下連結以重設密碼。</span><span class="sxs-lookup"><span data-stu-id="94074-241">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="94074-242">已成功重設您的密碼之後，您可以使用您的電子郵件和新密碼登入。</span><span class="sxs-lookup"><span data-stu-id="94074-242">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="94074-243">偵錯電子郵件</span><span class="sxs-lookup"><span data-stu-id="94074-243">Debug email</span></span>

<span data-ttu-id="94074-244">如果您無法取得電子郵件工作：</span><span class="sxs-lookup"><span data-stu-id="94074-244">If you can't get email working:</span></span>

* <span data-ttu-id="94074-245">檢閱[電子郵件 」 活動](https://sendgrid.com/docs/User_Guide/email_activity.html)頁面。</span><span class="sxs-lookup"><span data-stu-id="94074-245">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="94074-246">請檢查垃圾郵件資料夾。</span><span class="sxs-lookup"><span data-stu-id="94074-246">Check your spam folder.</span></span>
* <span data-ttu-id="94074-247">請嘗試不同的電子郵件提供者 （Microsoft、 Yahoo、 Gmail 等等） 上的另一個電子郵件別名</span><span class="sxs-lookup"><span data-stu-id="94074-247">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="94074-248">建立[主控台應用程式傳送電子郵件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)。</span><span class="sxs-lookup"><span data-stu-id="94074-248">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="94074-249">請嘗試傳送到不同的電子郵件帳戶。</span><span class="sxs-lookup"><span data-stu-id="94074-249">Try sending to different email accounts.</span></span>

<span data-ttu-id="94074-250">**注意：**安全性最佳作法是不要使用實際執行測試和開發中的機密資料。</span><span class="sxs-lookup"><span data-stu-id="94074-250">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="94074-251">如果您將應用程式發行至 Azure 時，您可以設定 SendGrid 密碼做為 Azure Web 應用程式入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="94074-251">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="94074-252">組態系統是以讀取環境變數中的索引鍵的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="94074-252">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="94074-253">避免在註冊的登入</span><span class="sxs-lookup"><span data-stu-id="94074-253">Prevent login at registration</span></span>

<span data-ttu-id="94074-254">目前的範本，一旦使用者完成註冊表單中，這些會記錄在 （驗證）。</span><span class="sxs-lookup"><span data-stu-id="94074-254">With the current templates, once a user completes the registration form, they are logged in (authenticated).</span></span> <span data-ttu-id="94074-255">您通常想要確認他們的電子郵件才能加以登入。</span><span class="sxs-lookup"><span data-stu-id="94074-255">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="94074-256">下方區段中，我們將會修改程式碼，需要新的使用者登入之前，有確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="94074-256">In the section below, we will modify the code to require new users have a confirmed email before they are logged in.</span></span> <span data-ttu-id="94074-257">更新`[HttpPost] Login`中的動作*AccountController.cs*下列反白顯示變更的檔案。</span><span class="sxs-lookup"><span data-stu-id="94074-257">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

<span data-ttu-id="94074-258">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span><span class="sxs-lookup"><span data-stu-id="94074-258">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span></span>

<span data-ttu-id="94074-259">**注意：**安全性最佳作法是不要使用實際執行測試和開發中的機密資料。</span><span class="sxs-lookup"><span data-stu-id="94074-259">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="94074-260">如果您將應用程式發行至 Azure 時，您可以設定 SendGrid 密碼做為 Azure Web 應用程式入口網站中的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="94074-260">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="94074-261">組態系統是以讀取環境變數中的索引鍵的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="94074-261">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="94074-262">結合社交和本機登入帳戶</span><span class="sxs-lookup"><span data-stu-id="94074-262">Combine social and local login accounts</span></span>

<span data-ttu-id="94074-263">注意： 本節僅適用於 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="94074-263">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="94074-264">適用於 ASP.NET Core 2.x，請參閱[這](https://github.com/aspnet/Docs/issues/3753)問題。</span><span class="sxs-lookup"><span data-stu-id="94074-264">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="94074-265">若要完成本章節，您必須先啟用外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="94074-265">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="94074-266">請參閱[啟用驗證使用 Facebook、 Google 和其他外部提供者](social/index.md)。</span><span class="sxs-lookup"><span data-stu-id="94074-266">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="94074-267">您可以結合本機和社交帳戶按一下電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="94074-267">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="94074-268">以下列順序，在"RickAndMSFT@gmail.com"第一次建立為本機的登入; 不過，您可以為社交登入，請先建立帳戶，然後加入本機的登入。</span><span class="sxs-lookup"><span data-stu-id="94074-268">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 應用程式：RickAndMSFT@gmail.com已驗證的使用者](accconfirm/_static/rick.png)

<span data-ttu-id="94074-270">按一下**管理**連結。</span><span class="sxs-lookup"><span data-stu-id="94074-270">Click on the **Manage** link.</span></span> <span data-ttu-id="94074-271">請注意 0 外部 （社交登入） 與此帳戶相關聯。</span><span class="sxs-lookup"><span data-stu-id="94074-271">Note the 0 external (social logins) associated with this account.</span></span>

![管理檢視](accconfirm/_static/manage.png)

<span data-ttu-id="94074-273">按一下連結以另一個登入服務，並接受要求的應用程式。</span><span class="sxs-lookup"><span data-stu-id="94074-273">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="94074-274">在下面的影像中，Facebook 會是外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="94074-274">In the image below, Facebook is the external authentication provider:</span></span>

![管理您列出 Facebook 的外部登入檢視](accconfirm/_static/fb.png)

<span data-ttu-id="94074-276">已合併兩個帳戶。</span><span class="sxs-lookup"><span data-stu-id="94074-276">The two accounts have been combined.</span></span> <span data-ttu-id="94074-277">您可以使用任一個帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="94074-277">You will be able to log on with either account.</span></span> <span data-ttu-id="94074-278">您可能會想讓使用者加入本機帳戶，以防使用者社交的記錄檔中驗證服務已關閉，或可能比較他們已經失去其社交帳戶的存取。</span><span class="sxs-lookup"><span data-stu-id="94074-278">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>
