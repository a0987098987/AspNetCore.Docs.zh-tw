---
title: ASP.NET Core 上的身分識別簡介
author: rick-anderson
description: 將身分識別與 ASP.NET Core 應用程式搭配使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
ms.author: riande
ms.date: 12/7/2019
uid: security/authentication/identity
ms.openlocfilehash: 331ebe36eb4bb7fa694de8daa969bcabcab1c974
ms.sourcegitcommit: b3e1e31e5d8bdd94096cf27444594d4a7b065525
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2019
ms.locfileid: "74803392"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="43704-104">ASP.NET Core 上的身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="43704-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43704-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="43704-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="43704-106">ASP.NET Core 身分識別：</span><span class="sxs-lookup"><span data-stu-id="43704-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="43704-107">是支援使用者介面（UI）登入功能的 API。</span><span class="sxs-lookup"><span data-stu-id="43704-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="43704-108">管理使用者、密碼、設定檔資料、角色、宣告、權杖、電子郵件確認等等。</span><span class="sxs-lookup"><span data-stu-id="43704-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="43704-109">使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="43704-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="43704-110">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="43704-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="43704-111">您可以在 GitHub 上取得身分[識別來來源程式代碼](https://github.com/aspnet/AspNetCore/tree/master/src/Identity)。</span><span class="sxs-lookup"><span data-stu-id="43704-111">The [Identity source code](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="43704-112">[Scaffold 身分識別](xref:security/authentication/scaffold-identity)並查看產生的檔案，以檢查與身分識別的範本互動。</span><span class="sxs-lookup"><span data-stu-id="43704-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="43704-113">通常會使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="43704-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="43704-114">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="43704-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="43704-115">在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="43704-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="43704-116">如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="43704-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="43704-117">[Microsoft 身分識別平臺](/azure/active-directory/develop/)是：</span><span class="sxs-lookup"><span data-stu-id="43704-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="43704-118">Azure Active Directory （Azure AD）開發人員平臺的演進。</span><span class="sxs-lookup"><span data-stu-id="43704-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="43704-119">與 ASP.NET Core 身分識別無關。</span><span class="sxs-lookup"><span data-stu-id="43704-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="43704-120">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="43704-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="43704-121">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="43704-121">Create a Web app with authentication</span></span>

<span data-ttu-id="43704-122">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="43704-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43704-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43704-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="43704-124">選取 [File] \(檔案\) > [New] \(新增\) > [Project] \(專案\)。</span><span class="sxs-lookup"><span data-stu-id="43704-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="43704-125">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="43704-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="43704-126">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="43704-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="43704-127">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="43704-127">Click **OK**.</span></span>
* <span data-ttu-id="43704-128">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="43704-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="43704-129">選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="43704-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="43704-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="43704-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="43704-131">上述命令會使用 SQLite 建立 Razor web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="43704-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="43704-132">若要使用 LocalDB 建立 web 應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="43704-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="43704-133">產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="43704-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="43704-134">身分識別 Razor 類別庫會公開具有 `Identity` 區域的端點。</span><span class="sxs-lookup"><span data-stu-id="43704-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="43704-135">例如：</span><span class="sxs-lookup"><span data-stu-id="43704-135">For example:</span></span>

* <span data-ttu-id="43704-136">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="43704-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="43704-137">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="43704-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="43704-138">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="43704-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="43704-139">套用移轉</span><span class="sxs-lookup"><span data-stu-id="43704-139">Apply migrations</span></span>

<span data-ttu-id="43704-140">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="43704-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43704-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43704-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43704-142">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="43704-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="43704-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="43704-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="43704-144">使用 SQLite 時，在此步驟中不需要進行遷移。</span><span class="sxs-lookup"><span data-stu-id="43704-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="43704-145">若是 LocalDB，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="43704-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="43704-146">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="43704-146">Test Register and Login</span></span>

<span data-ttu-id="43704-147">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="43704-147">Run the app and register a user.</span></span> <span data-ttu-id="43704-148">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="43704-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="43704-149">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="43704-149">Configure Identity services</span></span>

<span data-ttu-id="43704-150">服務會在 `ConfigureServices`中新增。</span><span class="sxs-lookup"><span data-stu-id="43704-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="43704-151">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="43704-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="43704-152">上述的反白顯示程式碼會使用預設選項值來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="43704-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="43704-153">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="43704-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="43704-154">識別是藉由呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>來啟用。</span><span class="sxs-lookup"><span data-stu-id="43704-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="43704-155">`UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="43704-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="43704-156">範本產生的應用程式不會使用[授權](xref:security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="43704-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="43704-157">包含 `app.UseAuthorization`，以確保在應用程式新增授權時，會以正確的順序加入。</span><span class="sxs-lookup"><span data-stu-id="43704-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="43704-158">`UseRouting`、`UseAuthentication`、`UseAuthorization`和 `UseEndpoints` 必須以上述程式碼中所示的順序來呼叫。</span><span class="sxs-lookup"><span data-stu-id="43704-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="43704-159">如需 `IdentityOptions` 和 `Startup`的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Identity.IdentityOptions> 和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="43704-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="43704-160">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="43704-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43704-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43704-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43704-162">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="43704-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="43704-163">將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="43704-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="43704-164">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="43704-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="43704-165">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="43704-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="43704-166">否則，請為 `ApplicationDbContext`使用正確的命名空間：</span><span class="sxs-lookup"><span data-stu-id="43704-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="43704-167">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="43704-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="43704-168">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="43704-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="43704-169">如需有關樣板識別的詳細資訊，請參閱[使用授權將身分識別 Scaffold 至 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="43704-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="43704-170">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="43704-170">Examine Register</span></span>

<span data-ttu-id="43704-171">當使用者按一下 [**註冊**] 連結時，就會叫用 `RegisterModel.OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="43704-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="43704-172">使用者是透過[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)在 `_userManager` 物件上建立的。</span><span class="sxs-lookup"><span data-stu-id="43704-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="43704-173">`_userManager` 是由相依性插入所提供）：</span><span class="sxs-lookup"><span data-stu-id="43704-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="43704-174">如果成功建立使用者，則會透過呼叫 `_signInManager.SignInAsync`來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="43704-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="43704-175">請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="43704-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="43704-176">登入</span><span class="sxs-lookup"><span data-stu-id="43704-176">Log in</span></span>

<span data-ttu-id="43704-177">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="43704-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="43704-178">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="43704-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="43704-179">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="43704-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="43704-180">當登入頁面上的表單提交時，會呼叫 `OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="43704-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="43704-181">`PasswordSignInAsync` 是在 `_signInManager` 物件上呼叫（由相依性插入所提供）。</span><span class="sxs-lookup"><span data-stu-id="43704-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="43704-182">基底 `Controller` 類別會公開可從控制器方法存取的 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="43704-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="43704-183">例如，您可以列舉 `User.Claims` 並做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="43704-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="43704-184">如需詳細資訊，請參閱<xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="43704-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="43704-185">登出</span><span class="sxs-lookup"><span data-stu-id="43704-185">Log out</span></span>

<span data-ttu-id="43704-186">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="43704-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="43704-187">在上述程式碼中，程式碼 `return RedirectToPage();` 必須是重新導向，才能讓瀏覽器執行新的要求，並更新使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="43704-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="43704-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="43704-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="43704-189">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="43704-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="43704-190">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-190">Test Identity</span></span>

<span data-ttu-id="43704-191">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="43704-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="43704-192">若要測試身分識別，請新增[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)：</span><span class="sxs-lookup"><span data-stu-id="43704-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="43704-193">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="43704-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="43704-194">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="43704-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="43704-195">探索身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-195">Explore Identity</span></span>

<span data-ttu-id="43704-196">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="43704-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="43704-197">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="43704-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="43704-198">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="43704-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="43704-199">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="43704-199">Identity Components</span></span>

<span data-ttu-id="43704-200">所有與身分識別相關的 NuGet 套件都包含在[ASP.NET Core 共用架構](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)中。</span><span class="sxs-lookup"><span data-stu-id="43704-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="43704-201">身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="43704-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="43704-202">此套件包含 ASP.NET Core 身分識別的核心介面集，而且是由 `Microsoft.AspNetCore.Identity.EntityFrameworkCore`所包含。</span><span class="sxs-lookup"><span data-stu-id="43704-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="43704-203">遷移至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="43704-204">如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="43704-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="43704-205">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="43704-205">Setting password strength</span></span>

<span data-ttu-id="43704-206">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="43704-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="43704-207">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="43704-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="43704-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> 是在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="43704-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="43704-209">呼叫 `AddDefaultIdentity` 類似于呼叫下列各項：</span><span class="sxs-lookup"><span data-stu-id="43704-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="43704-210">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="43704-210">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43704-211">後續步驟</span><span class="sxs-lookup"><span data-stu-id="43704-211">Next Steps</span></span>

* [<span data-ttu-id="43704-212">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-212">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="43704-213">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="43704-213">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="43704-214">ASP.NET Core 身分識別是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="43704-214">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="43704-215">使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="43704-215">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="43704-216">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="43704-216">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="43704-217">您可以使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="43704-217">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="43704-218">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="43704-218">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="43704-219">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="43704-219">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="43704-220">在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="43704-220">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="43704-221">如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="43704-221">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="43704-222">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="43704-222">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="43704-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> 是在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="43704-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="43704-224">呼叫 `AddDefaultIdentity` 類似于呼叫下列各項：</span><span class="sxs-lookup"><span data-stu-id="43704-224">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="43704-225">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="43704-225">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="43704-226">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="43704-226">Create a Web app with authentication</span></span>

<span data-ttu-id="43704-227">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="43704-227">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43704-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43704-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="43704-229">選取 [File] \(檔案\) > [New] \(新增\) > [Project] \(專案\)。</span><span class="sxs-lookup"><span data-stu-id="43704-229">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="43704-230">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="43704-230">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="43704-231">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="43704-231">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="43704-232">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="43704-232">Click **OK**.</span></span>
* <span data-ttu-id="43704-233">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="43704-233">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="43704-234">選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="43704-234">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="43704-235">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="43704-235">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="43704-236">產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="43704-236">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="43704-237">身分識別 Razor 類別庫會公開具有 `Identity` 區域的端點。</span><span class="sxs-lookup"><span data-stu-id="43704-237">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="43704-238">例如：</span><span class="sxs-lookup"><span data-stu-id="43704-238">For example:</span></span>

* <span data-ttu-id="43704-239">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="43704-239">/Identity/Account/Login</span></span>
* <span data-ttu-id="43704-240">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="43704-240">/Identity/Account/Logout</span></span>
* <span data-ttu-id="43704-241">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="43704-241">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="43704-242">套用移轉</span><span class="sxs-lookup"><span data-stu-id="43704-242">Apply migrations</span></span>

<span data-ttu-id="43704-243">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="43704-243">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43704-244">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43704-244">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43704-245">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="43704-245">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="43704-246">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="43704-246">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="43704-247">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="43704-247">Test Register and Login</span></span>

<span data-ttu-id="43704-248">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="43704-248">Run the app and register a user.</span></span> <span data-ttu-id="43704-249">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="43704-249">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="43704-250">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="43704-250">Configure Identity services</span></span>

<span data-ttu-id="43704-251">服務會在 `ConfigureServices`中新增。</span><span class="sxs-lookup"><span data-stu-id="43704-251">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="43704-252">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="43704-252">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="43704-253">上述程式碼會使用預設選項值來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="43704-253">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="43704-254">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="43704-254">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="43704-255">藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用身分識別。</span><span class="sxs-lookup"><span data-stu-id="43704-255">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="43704-256">`UseAuthentication` 會將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="43704-256">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="43704-257">如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="43704-257">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="43704-258">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="43704-258">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="43704-259">將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="43704-259">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43704-260">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43704-260">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43704-261">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="43704-261">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="43704-262">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="43704-262">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="43704-263">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="43704-263">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="43704-264">否則，請為 `ApplicationDbContext`使用正確的命名空間：</span><span class="sxs-lookup"><span data-stu-id="43704-264">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="43704-265">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="43704-265">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="43704-266">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="43704-266">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="43704-267">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="43704-267">Examine Register</span></span>

<span data-ttu-id="43704-268">當使用者按一下 [**註冊**] 連結時，就會叫用 `RegisterModel.OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="43704-268">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="43704-269">使用者是透過[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)在 `_userManager` 物件上建立的。</span><span class="sxs-lookup"><span data-stu-id="43704-269">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="43704-270">`_userManager` 是由相依性插入所提供）：</span><span class="sxs-lookup"><span data-stu-id="43704-270">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="43704-271">如果成功建立使用者，則會透過呼叫 `_signInManager.SignInAsync`來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="43704-271">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="43704-272">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="43704-272">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="43704-273">登入</span><span class="sxs-lookup"><span data-stu-id="43704-273">Log in</span></span>

<span data-ttu-id="43704-274">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="43704-274">The Login form is displayed when:</span></span>

* <span data-ttu-id="43704-275">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="43704-275">The **Log in** link is selected.</span></span>
* <span data-ttu-id="43704-276">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="43704-276">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="43704-277">當登入頁面上的表單提交時，會呼叫 `OnPostAsync` 動作。</span><span class="sxs-lookup"><span data-stu-id="43704-277">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="43704-278">`PasswordSignInAsync` 是在 `_signInManager` 物件上呼叫（由相依性插入所提供）。</span><span class="sxs-lookup"><span data-stu-id="43704-278">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="43704-279">基底 `Controller` 類別會公開您可以從控制器方法存取的 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="43704-279">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="43704-280">例如，您可以列舉 `User.Claims` 並做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="43704-280">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="43704-281">如需詳細資訊，請參閱<xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="43704-281">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="43704-282">登出</span><span class="sxs-lookup"><span data-stu-id="43704-282">Log out</span></span>

<span data-ttu-id="43704-283">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="43704-283">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="43704-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="43704-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="43704-285">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="43704-285">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="43704-286">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-286">Test Identity</span></span>

<span data-ttu-id="43704-287">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="43704-287">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="43704-288">若要測試身分識別，請將[`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)新增至 [隱私權] 頁面。</span><span class="sxs-lookup"><span data-stu-id="43704-288">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="43704-289">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="43704-289">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="43704-290">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="43704-290">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="43704-291">探索身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-291">Explore Identity</span></span>

<span data-ttu-id="43704-292">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="43704-292">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="43704-293">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="43704-293">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="43704-294">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="43704-294">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="43704-295">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="43704-295">Identity Components</span></span>

<span data-ttu-id="43704-296">所有身分識別相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="43704-296">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="43704-297">身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="43704-297">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="43704-298">此套件包含 ASP.NET Core 身分識別的核心介面集，而且是由 `Microsoft.AspNetCore.Identity.EntityFrameworkCore`所包含。</span><span class="sxs-lookup"><span data-stu-id="43704-298">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="43704-299">遷移至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-299">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="43704-300">如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="43704-300">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="43704-301">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="43704-301">Setting password strength</span></span>

<span data-ttu-id="43704-302">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="43704-302">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43704-303">後續步驟</span><span class="sxs-lookup"><span data-stu-id="43704-303">Next Steps</span></span>

* [<span data-ttu-id="43704-304">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="43704-304">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
