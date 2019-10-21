---
title: ASP.NET Core 上的身分識別簡介
author: rick-anderson
description: 將身分識別與 ASP.NET Core 應用程式搭配使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
ms.author: riande
ms.date: 10/15/2019
uid: security/authentication/identity
ms.openlocfilehash: 8da13ca5f74a9c829eb8137d33af0684ff88266d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333560"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="0c15c-104">ASP.NET Core 上的身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="0c15c-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c15c-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="0c15c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0c15c-106">ASP.NET Core 身分識別是支援使用者介面（UI）登入功能的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="0c15c-106">ASP.NET Core Identity is a membership system that supports user interface (UI) login functionality.</span></span> <span data-ttu-id="0c15c-107">使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="0c15c-108">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="0c15c-109">您可以使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="0c15c-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="0c15c-110">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="0c15c-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="0c15c-111">在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-111">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="0c15c-112">如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="0c15c-112">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="0c15c-113">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="0c15c-113">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="0c15c-114">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="0c15c-114">Create a Web app with authentication</span></span>

<span data-ttu-id="0c15c-115">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="0c15c-115">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c15c-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c15c-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0c15c-117">選取 **[** 檔案] > [**新增**>**專案**]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-117">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="0c15c-118">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0c15c-119">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="0c15c-119">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="0c15c-120">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-120">Click **OK**.</span></span>
* <span data-ttu-id="0c15c-121">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-121">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="0c15c-122">選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="0c15c-122">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c15c-123">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0c15c-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="0c15c-124">上述命令會使用 SQLite 建立 Razor web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c15c-124">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="0c15c-125">若要使用 LocalDB 建立 web 應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c15c-125">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="0c15c-126">產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-126">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="0c15c-127">身分識別 Razor 類別庫會公開具有 `Identity` 區域的端點。</span><span class="sxs-lookup"><span data-stu-id="0c15c-127">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="0c15c-128">例如:</span><span class="sxs-lookup"><span data-stu-id="0c15c-128">For example:</span></span>

* <span data-ttu-id="0c15c-129">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="0c15c-129">/Identity/Account/Login</span></span>
* <span data-ttu-id="0c15c-130">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="0c15c-130">/Identity/Account/Logout</span></span>
* <span data-ttu-id="0c15c-131">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="0c15c-131">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="0c15c-132">套用移轉</span><span class="sxs-lookup"><span data-stu-id="0c15c-132">Apply migrations</span></span>

<span data-ttu-id="0c15c-133">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="0c15c-133">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c15c-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c15c-134">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c15c-135">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c15c-135">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c15c-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0c15c-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c15c-137">使用 SQLite 時，在此步驟中不需要進行遷移。</span><span class="sxs-lookup"><span data-stu-id="0c15c-137">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="0c15c-138">若是 LocalDB，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c15c-138">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="0c15c-139">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="0c15c-139">Test Register and Login</span></span>

<span data-ttu-id="0c15c-140">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-140">Run the app and register a user.</span></span> <span data-ttu-id="0c15c-141">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="0c15c-141">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="0c15c-142">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="0c15c-142">Configure Identity services</span></span>

<span data-ttu-id="0c15c-143">服務會在 `ConfigureServices` 中新增。</span><span class="sxs-lookup"><span data-stu-id="0c15c-143">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="0c15c-144">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="0c15c-144">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="0c15c-145">上述的反白顯示程式碼會使用預設選項值來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="0c15c-145">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="0c15c-146">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c15c-146">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="0c15c-147">識別是藉由呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 來啟用。</span><span class="sxs-lookup"><span data-stu-id="0c15c-147">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="0c15c-148">`UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="0c15c-148">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="0c15c-149">範本產生的應用程式不會使用[授權](xref:security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-149">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="0c15c-150">包含 `app.UseAuthorization`，以確保在應用程式新增授權時，會以正確的順序加入。</span><span class="sxs-lookup"><span data-stu-id="0c15c-150">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="0c15c-151">`UseRouting`、`UseAuthentication`、`UseAuthorization` 和 `UseEndpoints` 必須以上述程式碼中所示的順序來呼叫。</span><span class="sxs-lookup"><span data-stu-id="0c15c-151">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="0c15c-152">如需 `IdentityOptions` 和 `Startup` 的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Identity.IdentityOptions> 和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-152">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="0c15c-153">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="0c15c-153">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c15c-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c15c-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c15c-155">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="0c15c-155">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="0c15c-156">將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0c15c-156">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c15c-157">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0c15c-157">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c15c-158">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="0c15c-158">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="0c15c-159">否則，請為 `ApplicationDbContext` 使用正確的命名空間：</span><span class="sxs-lookup"><span data-stu-id="0c15c-159">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="0c15c-160">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="0c15c-160">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="0c15c-161">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="0c15c-161">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="0c15c-162">如需有關樣板識別的詳細資訊，請參閱[使用授權將身分識別 Scaffold 至 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-162">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="0c15c-163">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="0c15c-163">Examine Register</span></span>

<span data-ttu-id="0c15c-164">當使用者按一下 [**註冊**] 連結時，就會叫用 `RegisterModel.OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="0c15c-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="0c15c-165">使用者是透過[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)在 `_userManager` 物件上建立的。</span><span class="sxs-lookup"><span data-stu-id="0c15c-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="0c15c-166">`_userManager` 是由相依性插入所提供）：</span><span class="sxs-lookup"><span data-stu-id="0c15c-166">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="0c15c-167">如果成功建立使用者，則會透過呼叫 `_signInManager.SignInAsync` 來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="0c15c-168">請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="0c15c-168">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="0c15c-169">登錄</span><span class="sxs-lookup"><span data-stu-id="0c15c-169">Log in</span></span>

<span data-ttu-id="0c15c-170">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="0c15c-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="0c15c-171">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="0c15c-171">The **Log in** link is selected.</span></span>
* <span data-ttu-id="0c15c-172">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="0c15c-172">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="0c15c-173">當登入頁面上的表單提交時，會呼叫 `OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="0c15c-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="0c15c-174">`PasswordSignInAsync` 是在 `_signInManager` 物件上呼叫（由相依性插入所提供）。</span><span class="sxs-lookup"><span data-stu-id="0c15c-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="0c15c-175">基底 `Controller` 類別會公開可從控制器方法存取的 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="0c15c-175">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="0c15c-176">例如，您可以列舉 `User.Claims` 並做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="0c15c-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="0c15c-177">如需詳細資訊，請參閱<xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="0c15c-177">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="0c15c-178">登出</span><span class="sxs-lookup"><span data-stu-id="0c15c-178">Log out</span></span>

<span data-ttu-id="0c15c-179">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="0c15c-179">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="0c15c-180">在上述程式碼中，程式碼 `return RedirectToPage();` 必須是重新導向，才能讓瀏覽器執行新的要求，並更新使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="0c15c-180">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="0c15c-181">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="0c15c-181">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="0c15c-182">Post 是在*Pages/Shared/_LoginPartial*中指定的：</span><span class="sxs-lookup"><span data-stu-id="0c15c-182">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="0c15c-183">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-183">Test Identity</span></span>

<span data-ttu-id="0c15c-184">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="0c15c-184">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="0c15c-185">若要測試身分識別，請新增[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)：</span><span class="sxs-lookup"><span data-stu-id="0c15c-185">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="0c15c-186">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="0c15c-186">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="0c15c-187">系統會將您重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0c15c-187">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="0c15c-188">探索身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-188">Explore Identity</span></span>

<span data-ttu-id="0c15c-189">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="0c15c-189">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="0c15c-190">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="0c15c-190">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="0c15c-191">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="0c15c-191">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="0c15c-192">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="0c15c-192">Identity Components</span></span>

<span data-ttu-id="0c15c-193">所有與身分識別相關的 NuGet 套件都包含在[ASP.NET Core 共用架構](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)中。</span><span class="sxs-lookup"><span data-stu-id="0c15c-193">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="0c15c-194">身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="0c15c-194">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="0c15c-195">此套件包含 ASP.NET Core 身分識別的核心介面集，而且是由 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 所包含。</span><span class="sxs-lookup"><span data-stu-id="0c15c-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="0c15c-196">遷移至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-196">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="0c15c-197">如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-197">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="0c15c-198">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="0c15c-198">Setting password strength</span></span>

<span data-ttu-id="0c15c-199">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="0c15c-199">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="0c15c-200">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="0c15c-200">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="0c15c-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> 是在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="0c15c-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="0c15c-202">呼叫 `AddDefaultIdentity` 類似于呼叫下列各項：</span><span class="sxs-lookup"><span data-stu-id="0c15c-202">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="0c15c-203">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="0c15c-203">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c15c-204">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0c15c-204">Next Steps</span></span>

* [<span data-ttu-id="0c15c-205">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-205">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0c15c-206">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="0c15c-206">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0c15c-207">ASP.NET Core 身分識別是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="0c15c-207">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="0c15c-208">使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-208">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="0c15c-209">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-209">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="0c15c-210">您可以使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="0c15c-210">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="0c15c-211">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="0c15c-211">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="0c15c-212">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="0c15c-212">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="0c15c-213">在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-213">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="0c15c-214">如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="0c15c-214">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="0c15c-215">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="0c15c-215">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="0c15c-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> 是在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="0c15c-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="0c15c-217">呼叫 `AddDefaultIdentity` 類似于呼叫下列各項：</span><span class="sxs-lookup"><span data-stu-id="0c15c-217">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="0c15c-218">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="0c15c-218">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="0c15c-219">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="0c15c-219">Create a Web app with authentication</span></span>

<span data-ttu-id="0c15c-220">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="0c15c-220">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c15c-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c15c-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0c15c-222">選取 **[** 檔案] > [**新增**>**專案**]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-222">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="0c15c-223">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-223">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0c15c-224">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="0c15c-224">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="0c15c-225">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-225">Click **OK**.</span></span>
* <span data-ttu-id="0c15c-226">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="0c15c-226">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="0c15c-227">選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="0c15c-227">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c15c-228">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0c15c-228">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="0c15c-229">產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-229">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="0c15c-230">身分識別 Razor 類別庫會公開具有 `Identity` 區域的端點。</span><span class="sxs-lookup"><span data-stu-id="0c15c-230">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="0c15c-231">例如:</span><span class="sxs-lookup"><span data-stu-id="0c15c-231">For example:</span></span>

* <span data-ttu-id="0c15c-232">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="0c15c-232">/Identity/Account/Login</span></span>
* <span data-ttu-id="0c15c-233">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="0c15c-233">/Identity/Account/Logout</span></span>
* <span data-ttu-id="0c15c-234">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="0c15c-234">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="0c15c-235">套用移轉</span><span class="sxs-lookup"><span data-stu-id="0c15c-235">Apply migrations</span></span>

<span data-ttu-id="0c15c-236">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="0c15c-236">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c15c-237">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c15c-237">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c15c-238">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c15c-238">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c15c-239">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0c15c-239">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="0c15c-240">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="0c15c-240">Test Register and Login</span></span>

<span data-ttu-id="0c15c-241">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-241">Run the app and register a user.</span></span> <span data-ttu-id="0c15c-242">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="0c15c-242">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="0c15c-243">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="0c15c-243">Configure Identity services</span></span>

<span data-ttu-id="0c15c-244">服務會在 `ConfigureServices` 中新增。</span><span class="sxs-lookup"><span data-stu-id="0c15c-244">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="0c15c-245">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="0c15c-245">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="0c15c-246">上述程式碼會使用預設選項值來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="0c15c-246">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="0c15c-247">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c15c-247">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="0c15c-248">藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用身分識別。</span><span class="sxs-lookup"><span data-stu-id="0c15c-248">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="0c15c-249">`UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="0c15c-249">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="0c15c-250">如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-250">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="0c15c-251">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="0c15c-251">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="0c15c-252">將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0c15c-252">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c15c-253">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c15c-253">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c15c-254">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="0c15c-254">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c15c-255">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0c15c-255">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c15c-256">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="0c15c-256">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="0c15c-257">否則，請為 `ApplicationDbContext` 使用正確的命名空間：</span><span class="sxs-lookup"><span data-stu-id="0c15c-257">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="0c15c-258">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="0c15c-258">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="0c15c-259">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="0c15c-259">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="0c15c-260">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="0c15c-260">Examine Register</span></span>

<span data-ttu-id="0c15c-261">當使用者按一下 [**註冊**] 連結時，就會叫用 `RegisterModel.OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="0c15c-261">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="0c15c-262">使用者是透過[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)在 `_userManager` 物件上建立的。</span><span class="sxs-lookup"><span data-stu-id="0c15c-262">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="0c15c-263">`_userManager` 是由相依性插入所提供）：</span><span class="sxs-lookup"><span data-stu-id="0c15c-263">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="0c15c-264">如果成功建立使用者，則會透過呼叫 `_signInManager.SignInAsync` 來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="0c15c-264">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="0c15c-265">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="0c15c-265">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="0c15c-266">登錄</span><span class="sxs-lookup"><span data-stu-id="0c15c-266">Log in</span></span>

<span data-ttu-id="0c15c-267">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="0c15c-267">The Login form is displayed when:</span></span>

* <span data-ttu-id="0c15c-268">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="0c15c-268">The **Log in** link is selected.</span></span>
* <span data-ttu-id="0c15c-269">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="0c15c-269">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="0c15c-270">當登入頁面上的表單提交時，會呼叫 `OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="0c15c-270">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="0c15c-271">`PasswordSignInAsync` 是在 `_signInManager` 物件上呼叫（由相依性插入所提供）。</span><span class="sxs-lookup"><span data-stu-id="0c15c-271">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="0c15c-272">基底 `Controller` 類別會公開您可以從控制器方法存取的 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="0c15c-272">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="0c15c-273">例如，您可以列舉 `User.Claims` 並做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="0c15c-273">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="0c15c-274">如需詳細資訊，請參閱<xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="0c15c-274">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="0c15c-275">登出</span><span class="sxs-lookup"><span data-stu-id="0c15c-275">Log out</span></span>

<span data-ttu-id="0c15c-276">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="0c15c-276">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="0c15c-277">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="0c15c-277">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="0c15c-278">Post 是在*Pages/Shared/_LoginPartial*中指定的：</span><span class="sxs-lookup"><span data-stu-id="0c15c-278">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="0c15c-279">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-279">Test Identity</span></span>

<span data-ttu-id="0c15c-280">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="0c15c-280">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="0c15c-281">若要測試身分識別，請將[`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)新增至 [隱私權] 頁面。</span><span class="sxs-lookup"><span data-stu-id="0c15c-281">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="0c15c-282">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="0c15c-282">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="0c15c-283">系統會將您重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0c15c-283">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="0c15c-284">探索身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-284">Explore Identity</span></span>

<span data-ttu-id="0c15c-285">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="0c15c-285">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="0c15c-286">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="0c15c-286">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="0c15c-287">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="0c15c-287">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="0c15c-288">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="0c15c-288">Identity Components</span></span>

<span data-ttu-id="0c15c-289">所有身分識別相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="0c15c-289">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="0c15c-290">身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="0c15c-290">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="0c15c-291">此套件包含 ASP.NET Core 身分識別的核心介面集，而且是由 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 所包含。</span><span class="sxs-lookup"><span data-stu-id="0c15c-291">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="0c15c-292">遷移至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-292">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="0c15c-293">如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="0c15c-293">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="0c15c-294">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="0c15c-294">Setting password strength</span></span>

<span data-ttu-id="0c15c-295">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="0c15c-295">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c15c-296">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0c15c-296">Next Steps</span></span>

* [<span data-ttu-id="0c15c-297">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="0c15c-297">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
