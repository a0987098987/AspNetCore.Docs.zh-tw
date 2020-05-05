---
title: Identity簡介 ASP.NET Core
author: rick-anderson
description: 搭配Identity ASP.NET Core 應用程式使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
ms.author: riande
ms.date: 01/15/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity
ms.openlocfilehash: d596a8357c5c812b94950809eedf35718328747c
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777003"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="51602-104">ASP.NET Core 上的身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="51602-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="51602-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51602-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="51602-106">ASP.NET Core 身分識別：</span><span class="sxs-lookup"><span data-stu-id="51602-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="51602-107">是支援使用者介面（UI）登入功能的 API。</span><span class="sxs-lookup"><span data-stu-id="51602-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="51602-108">管理使用者、密碼、設定檔資料、角色、宣告、權杖、電子郵件確認等等。</span><span class="sxs-lookup"><span data-stu-id="51602-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="51602-109">使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="51602-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="51602-110">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="51602-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="51602-111">您可以在 GitHub 上取得身分[識別來來源程式代碼](https://github.com/dotnet/AspNetCore/tree/master/src/Identity)。</span><span class="sxs-lookup"><span data-stu-id="51602-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="51602-112">[Scaffold 身分識別](xref:security/authentication/scaffold-identity)並查看產生的檔案，以檢查與身分識別的範本互動。</span><span class="sxs-lookup"><span data-stu-id="51602-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="51602-113">通常會使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="51602-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="51602-114">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="51602-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="51602-115">在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="51602-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="51602-116">注意：範本會將使用者名稱和電子郵件視為相同。</span><span class="sxs-lookup"><span data-stu-id="51602-116">Note: the templates treat username and email as the same for users.</span></span> <span data-ttu-id="51602-117">如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="51602-117">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="51602-118">[Microsoft 身分識別平臺](/azure/active-directory/develop/)是：</span><span class="sxs-lookup"><span data-stu-id="51602-118">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="51602-119">Azure Active Directory （Azure AD）開發人員平臺的演進。</span><span class="sxs-lookup"><span data-stu-id="51602-119">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="51602-120">與 ASP.NET Core 身分識別無關。</span><span class="sxs-lookup"><span data-stu-id="51602-120">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="51602-121">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="51602-121">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="51602-122">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="51602-122">Create a Web app with authentication</span></span>

<span data-ttu-id="51602-123">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="51602-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="51602-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51602-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="51602-125">>選取 **[** 檔案] [**新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="51602-125">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="51602-126">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="51602-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="51602-127">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="51602-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="51602-128">按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="51602-128">Click **OK**.</span></span>
* <span data-ttu-id="51602-129">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="51602-129">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="51602-130">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="51602-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="51602-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="51602-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="51602-132">上述命令會使用 SQLite 建立 Razor web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="51602-132">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="51602-133">若要使用 LocalDB 建立 web 應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="51602-133">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="51602-134">產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="51602-134">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="51602-135">身分識別 Razor 類別庫會公開具有`Identity`區域的端點。</span><span class="sxs-lookup"><span data-stu-id="51602-135">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="51602-136">例如：</span><span class="sxs-lookup"><span data-stu-id="51602-136">For example:</span></span>

* <span data-ttu-id="51602-137">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="51602-137">/Identity/Account/Login</span></span>
* <span data-ttu-id="51602-138">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="51602-138">/Identity/Account/Logout</span></span>
* <span data-ttu-id="51602-139">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="51602-139">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="51602-140">套用移轉</span><span class="sxs-lookup"><span data-stu-id="51602-140">Apply migrations</span></span>

<span data-ttu-id="51602-141">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="51602-141">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="51602-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51602-142">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="51602-143">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="51602-143">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-cli"></a>[<span data-ttu-id="51602-144">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="51602-144">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="51602-145">使用 SQLite 時，在此步驟中不需要進行遷移。</span><span class="sxs-lookup"><span data-stu-id="51602-145">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="51602-146">若是 LocalDB，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="51602-146">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="51602-147">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="51602-147">Test Register and Login</span></span>

<span data-ttu-id="51602-148">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="51602-148">Run the app and register a user.</span></span> <span data-ttu-id="51602-149">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="51602-149">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="51602-150">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="51602-150">Configure Identity services</span></span>

<span data-ttu-id="51602-151">服務會在中`ConfigureServices`加入。</span><span class="sxs-lookup"><span data-stu-id="51602-151">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="51602-152">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="51602-152">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="51602-153">上述的反白顯示程式碼會使用預設選項值來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="51602-153">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="51602-154">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="51602-154">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="51602-155">藉由呼叫<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>來啟用身分識別。</span><span class="sxs-lookup"><span data-stu-id="51602-155">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="51602-156">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="51602-156">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="51602-157">範本產生的應用程式不會使用[授權](xref:security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="51602-157">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="51602-158">`app.UseAuthorization`包含，以確保在應用程式新增授權時，會以正確的順序新增。</span><span class="sxs-lookup"><span data-stu-id="51602-158">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="51602-159">`UseRouting`必須`UseAuthentication`以`UseAuthorization`上述程式`UseEndpoints`代碼中所示的順序來呼叫、、和。</span><span class="sxs-lookup"><span data-stu-id="51602-159">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="51602-160">如需`IdentityOptions`和`Startup`的詳細資訊， <xref:Microsoft.AspNetCore.Identity.IdentityOptions>請參閱和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="51602-160">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="51602-161">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="51602-161">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="51602-162">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51602-162">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="51602-163">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="51602-163">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="51602-164">將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="51602-164">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="51602-165">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="51602-165">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="51602-166">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="51602-166">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="51602-167">否則，請使用正確的命名空間`ApplicationDbContext`：</span><span class="sxs-lookup"><span data-stu-id="51602-167">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="51602-168">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="51602-168">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="51602-169">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="51602-169">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="51602-170">如需有關樣板識別的詳細資訊，請參閱[使用授權將身分識別 Scaffold 至 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="51602-170">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="51602-171">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="51602-171">Examine Register</span></span>

<span data-ttu-id="51602-172">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync`就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="51602-172">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="51602-173">使用者是由`_userManager`物件上的[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)所建立。</span><span class="sxs-lookup"><span data-stu-id="51602-173">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="51602-174">`_userManager`由相依性插入所提供）：</span><span class="sxs-lookup"><span data-stu-id="51602-174">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="51602-175">如果成功建立使用者，則會呼叫來`_signInManager.SignInAsync`登入使用者。</span><span class="sxs-lookup"><span data-stu-id="51602-175">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="51602-176">請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="51602-176">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="51602-177">登入</span><span class="sxs-lookup"><span data-stu-id="51602-177">Log in</span></span>

<span data-ttu-id="51602-178">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="51602-178">The Login form is displayed when:</span></span>

* <span data-ttu-id="51602-179">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="51602-179">The **Log in** link is selected.</span></span>
* <span data-ttu-id="51602-180">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="51602-180">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="51602-181">提交登入頁面上的表單時，會`OnPostAsync`呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="51602-181">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="51602-182">`PasswordSignInAsync`會在`_signInManager`物件上呼叫（由相依性插入所提供）。</span><span class="sxs-lookup"><span data-stu-id="51602-182">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="51602-183">基底`Controller`類別會公開`User`可從控制器方法存取的屬性。</span><span class="sxs-lookup"><span data-stu-id="51602-183">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="51602-184">例如，您可以列舉`User.Claims`並做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="51602-184">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="51602-185">如需詳細資訊，請參閱<xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="51602-185">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="51602-186">登出</span><span class="sxs-lookup"><span data-stu-id="51602-186">Log out</span></span>

<span data-ttu-id="51602-187">[**登出**] 連結會叫`LogoutModel.OnPost`用動作。</span><span class="sxs-lookup"><span data-stu-id="51602-187">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="51602-188">在上述程式碼中，程式`return RedirectToPage();`代碼必須是重新導向，才能讓瀏覽器執行新的要求，並更新使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="51602-188">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="51602-189">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="51602-189">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="51602-190">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="51602-190">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="51602-191">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="51602-191">Test Identity</span></span>

<span data-ttu-id="51602-192">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="51602-192">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="51602-193">若要測試身分識別[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)，請新增：</span><span class="sxs-lookup"><span data-stu-id="51602-193">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="51602-194">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="51602-194">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="51602-195">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="51602-195">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="51602-196">探索身分識別</span><span class="sxs-lookup"><span data-stu-id="51602-196">Explore Identity</span></span>

<span data-ttu-id="51602-197">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="51602-197">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="51602-198">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="51602-198">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="51602-199">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="51602-199">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="51602-200">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="51602-200">Identity Components</span></span>

<span data-ttu-id="51602-201">所有與身分識別相關的 NuGet 套件都包含在[ASP.NET Core 共用架構](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)中。</span><span class="sxs-lookup"><span data-stu-id="51602-201">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="51602-202">身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="51602-202">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="51602-203">此套件包含 ASP.NET Core 身分識別的核心介面集，並且包含在中`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。</span><span class="sxs-lookup"><span data-stu-id="51602-203">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="51602-204">遷移至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="51602-204">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="51602-205">如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="51602-205">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="51602-206">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="51602-206">Setting password strength</span></span>

<span data-ttu-id="51602-207">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="51602-207">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="51602-208">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="51602-208">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="51602-209"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="51602-209"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="51602-210">呼叫`AddDefaultIdentity`類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="51602-210">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="51602-211">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="51602-211">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="51602-212">防止發佈靜態身分識別資產</span><span class="sxs-lookup"><span data-stu-id="51602-212">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="51602-213">若要防止將靜態身分識別資產（身分識別 UI 的樣式表單和 JavaScript 檔案）發行至 web 根目錄`ResolveStaticWebAssetsInputsDependsOn` ，請`RemoveIdentityAssets`將下列屬性和目標新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="51602-213">To prevent publishing static Identity assets (stylesheets and JavaScript files for Identity UI) to the web root, add the following `ResolveStaticWebAssetsInputsDependsOn` property and `RemoveIdentityAssets` target to the app's project file:</span></span>

```xml
<PropertyGroup>
  <ResolveStaticWebAssetsInputsDependsOn>RemoveIdentityAssets</ResolveStaticWebAssetsInputsDependsOn>
</PropertyGroup>

<Target Name="RemoveIdentityAssets">
  <ItemGroup>
    <StaticWebAsset Remove="@(StaticWebAsset)" Condition="%(SourceId) == 'Microsoft.AspNetCore.Identity.UI'" />
  </ItemGroup>
</Target>
```

## <a name="next-steps"></a><span data-ttu-id="51602-214">後續步驟</span><span class="sxs-lookup"><span data-stu-id="51602-214">Next Steps</span></span>

* <span data-ttu-id="51602-215">如需使用 SQLite 設定身分識別的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131)。</span><span class="sxs-lookup"><span data-stu-id="51602-215">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="51602-216">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="51602-216">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="51602-217">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51602-217">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="51602-218">ASP.NET Core 身分識別是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="51602-218">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="51602-219">使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="51602-219">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="51602-220">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="51602-220">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="51602-221">您可以使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="51602-221">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="51602-222">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="51602-222">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="51602-223">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="51602-223">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="51602-224">在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="51602-224">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="51602-225">如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="51602-225">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="51602-226">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="51602-226">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="51602-227"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="51602-227"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="51602-228">呼叫`AddDefaultIdentity`類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="51602-228">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="51602-229">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="51602-229">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="51602-230">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="51602-230">Create a Web app with authentication</span></span>

<span data-ttu-id="51602-231">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="51602-231">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="51602-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51602-232">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="51602-233">>選取 **[** 檔案] [**新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="51602-233">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="51602-234">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="51602-234">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="51602-235">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="51602-235">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="51602-236">按一下 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="51602-236">Click **OK**.</span></span>
* <span data-ttu-id="51602-237">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="51602-237">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="51602-238">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="51602-238">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="51602-239">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="51602-239">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="51602-240">產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="51602-240">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="51602-241">身分識別 Razor 類別庫會公開具有`Identity`區域的端點。</span><span class="sxs-lookup"><span data-stu-id="51602-241">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="51602-242">例如：</span><span class="sxs-lookup"><span data-stu-id="51602-242">For example:</span></span>

* <span data-ttu-id="51602-243">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="51602-243">/Identity/Account/Login</span></span>
* <span data-ttu-id="51602-244">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="51602-244">/Identity/Account/Logout</span></span>
* <span data-ttu-id="51602-245">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="51602-245">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="51602-246">套用移轉</span><span class="sxs-lookup"><span data-stu-id="51602-246">Apply migrations</span></span>

<span data-ttu-id="51602-247">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="51602-247">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="51602-248">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51602-248">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="51602-249">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="51602-249">Run the following command in the Package Manager Console (PMC):</span></span>

```powershell
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="51602-250">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="51602-250">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="51602-251">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="51602-251">Test Register and Login</span></span>

<span data-ttu-id="51602-252">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="51602-252">Run the app and register a user.</span></span> <span data-ttu-id="51602-253">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="51602-253">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="51602-254">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="51602-254">Configure Identity services</span></span>

<span data-ttu-id="51602-255">服務會在中`ConfigureServices`加入。</span><span class="sxs-lookup"><span data-stu-id="51602-255">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="51602-256">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="51602-256">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="51602-257">上述程式碼會使用預設選項值來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="51602-257">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="51602-258">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="51602-258">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="51602-259">藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用身分識別。</span><span class="sxs-lookup"><span data-stu-id="51602-259">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="51602-260">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="51602-260">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="51602-261">如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="51602-261">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="51602-262">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="51602-262">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="51602-263">將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="51602-263">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="51602-264">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51602-264">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="51602-265">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="51602-265">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="51602-266">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="51602-266">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="51602-267">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="51602-267">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="51602-268">否則，請使用正確的命名空間`ApplicationDbContext`：</span><span class="sxs-lookup"><span data-stu-id="51602-268">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="51602-269">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="51602-269">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="51602-270">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="51602-270">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="51602-271">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="51602-271">Examine Register</span></span>

<span data-ttu-id="51602-272">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync`就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="51602-272">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="51602-273">使用者是由`_userManager`物件上的[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)所建立。</span><span class="sxs-lookup"><span data-stu-id="51602-273">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="51602-274">`_userManager`由相依性插入所提供）：</span><span class="sxs-lookup"><span data-stu-id="51602-274">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="51602-275">如果成功建立使用者，則會呼叫來`_signInManager.SignInAsync`登入使用者。</span><span class="sxs-lookup"><span data-stu-id="51602-275">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="51602-276">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="51602-276">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="51602-277">登入</span><span class="sxs-lookup"><span data-stu-id="51602-277">Log in</span></span>

<span data-ttu-id="51602-278">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="51602-278">The Login form is displayed when:</span></span>

* <span data-ttu-id="51602-279">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="51602-279">The **Log in** link is selected.</span></span>
* <span data-ttu-id="51602-280">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="51602-280">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="51602-281">提交登入頁面上的表單時，會`OnPostAsync`呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="51602-281">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="51602-282">`PasswordSignInAsync`會在`_signInManager`物件上呼叫（由相依性插入所提供）。</span><span class="sxs-lookup"><span data-stu-id="51602-282">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="51602-283">基類`Controller`會公開可從`User`控制器方法存取的屬性。</span><span class="sxs-lookup"><span data-stu-id="51602-283">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="51602-284">例如，您可以列舉`User.Claims`並做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="51602-284">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="51602-285">如需詳細資訊，請參閱<xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="51602-285">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="51602-286">登出</span><span class="sxs-lookup"><span data-stu-id="51602-286">Log out</span></span>

<span data-ttu-id="51602-287">[**登出**] 連結會叫`LogoutModel.OnPost`用動作。</span><span class="sxs-lookup"><span data-stu-id="51602-287">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="51602-288">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="51602-288">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="51602-289">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="51602-289">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="51602-290">測驗Identity</span><span class="sxs-lookup"><span data-stu-id="51602-290">Test Identity</span></span>

<span data-ttu-id="51602-291">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="51602-291">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="51602-292">若要Identity進行測試[`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) ，請將新增至 [隱私權] 頁面。</span><span class="sxs-lookup"><span data-stu-id="51602-292">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="51602-293">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="51602-293">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="51602-294">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="51602-294">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="51602-295">看Identity</span><span class="sxs-lookup"><span data-stu-id="51602-295">Explore Identity</span></span>

<span data-ttu-id="51602-296">若要Identity更詳細地探索：</span><span class="sxs-lookup"><span data-stu-id="51602-296">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="51602-297">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="51602-297">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="51602-298">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="51602-298">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="51602-299">要素</span><span class="sxs-lookup"><span data-stu-id="51602-299"> Components</span></span>

<span data-ttu-id="51602-300">所有相依Identity的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="51602-300">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="51602-301">的主要封裝Identity是[AspNetCore。Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="51602-301">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="51602-302">此套件包含 ASP.NET Core Identity的核心介面集，由`Microsoft.AspNetCore.Identity.EntityFrameworkCore`所包含。</span><span class="sxs-lookup"><span data-stu-id="51602-302">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="51602-303">遷移至 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="51602-303">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="51602-304">如需有關遷移現有Identity存放區的詳細資訊和指引，請參閱[遷移驗證和Identity ](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="51602-304">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="51602-305">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="51602-305">Setting password strength</span></span>

<span data-ttu-id="51602-306">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="51602-306">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51602-307">後續步驟</span><span class="sxs-lookup"><span data-stu-id="51602-307">Next Steps</span></span>

* <span data-ttu-id="51602-308">如需Identity使用 SQLite 進行設定的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131)。</span><span class="sxs-lookup"><span data-stu-id="51602-308">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="51602-309">[配置Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="51602-309">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
