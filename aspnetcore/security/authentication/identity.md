---
title: 簡介 Identity ASP.NET Core
author: rick-anderson
description: 搭配 Identity ASP.NET Core 應用程式使用。 瞭解如何 (RequireDigit、RequiredLength、RequiredUniqueChars 等) 設定密碼需求。
ms.author: riande
ms.date: 01/15/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity
ms.openlocfilehash: 6ac565bfa4862168fa143417ab5a81c51b620f16
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86212454"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="b11ef-104">簡介 Identity ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b11ef-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b11ef-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b11ef-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b11ef-106">ASP.NET Core Identity ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="b11ef-107">是支援使用者介面 (UI) 登入功能的 API。</span><span class="sxs-lookup"><span data-stu-id="b11ef-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="b11ef-108">管理使用者、密碼、設定檔資料、角色、宣告、權杖、電子郵件確認等等。</span><span class="sxs-lookup"><span data-stu-id="b11ef-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="b11ef-109">使用者可以建立具有儲存在中之登入資訊的帳戶， Identity 或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="b11ef-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="b11ef-110">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="b11ef-111">[ Identity 原始程式碼](https://github.com/dotnet/AspNetCore/tree/master/src/Identity)可在 GitHub 上取得。</span><span class="sxs-lookup"><span data-stu-id="b11ef-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="b11ef-112">[Scaffold Identity ](xref:security/authentication/scaffold-identity)和會查看產生的檔案，以檢查與的範本互動 Identity 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

Identity<span data-ttu-id="b11ef-113">通常會使用 SQL Server 資料庫來設定，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="b11ef-113"> is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="b11ef-114">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="b11ef-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="b11ef-115">在本主題中，您將瞭解如何使用 Identity 來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="b11ef-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="b11ef-116">注意：範本會將使用者名稱和電子郵件視為相同。</span><span class="sxs-lookup"><span data-stu-id="b11ef-116">Note: the templates treat username and email as the same for users.</span></span> <span data-ttu-id="b11ef-117">如需有關建立使用之應用程式的詳細指示 Identity ，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="b11ef-117">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="b11ef-118">[Microsoft 身分識別平臺](/azure/active-directory/develop/)是：</span><span class="sxs-lookup"><span data-stu-id="b11ef-118">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="b11ef-119">Azure Active Directory (Azure AD) 開發人員平臺的演進。</span><span class="sxs-lookup"><span data-stu-id="b11ef-119">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="b11ef-120">與 ASP.NET Core 無關 Identity 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-120">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/Identity<span data-ttu-id="b11ef-121">Server4.md) ]</span><span class="sxs-lookup"><span data-stu-id="b11ef-121">Server4.md)]</span></span>

<span data-ttu-id="b11ef-122">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([如何下載) ](xref:index#how-to-download-a-sample)) 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-122">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="b11ef-123">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b11ef-123">Create a Web app with authentication</span></span>

<span data-ttu-id="b11ef-124">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="b11ef-124">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b11ef-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b11ef-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b11ef-126">選取 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="b11ef-126">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="b11ef-127">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="b11ef-127">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b11ef-128">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="b11ef-128">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="b11ef-129">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b11ef-129">Click **OK**.</span></span>
* <span data-ttu-id="b11ef-130">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="b11ef-130">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="b11ef-131">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="b11ef-131">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="b11ef-132">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b11ef-132">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="b11ef-133">上述命令會 Razor 使用 SQLite 建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b11ef-133">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="b11ef-134">若要使用 LocalDB 建立 web 應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b11ef-134">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="b11ef-135">產生的專案會[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-135">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="b11ef-136">Identity Razor 類別庫會以區域公開端點 `Identity` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-136">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="b11ef-137">例如：</span><span class="sxs-lookup"><span data-stu-id="b11ef-137">For example:</span></span>

* <span data-ttu-id="b11ef-138">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="b11ef-138">/Identity/Account/Login</span></span>
* <span data-ttu-id="b11ef-139">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="b11ef-139">/Identity/Account/Logout</span></span>
* <span data-ttu-id="b11ef-140">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="b11ef-140">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="b11ef-141">套用移轉</span><span class="sxs-lookup"><span data-stu-id="b11ef-141">Apply migrations</span></span>

<span data-ttu-id="b11ef-142">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="b11ef-142">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b11ef-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b11ef-143">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b11ef-144">在套件管理員主控台中執行下列命令 (PMC) ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-144">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-cli"></a>[<span data-ttu-id="b11ef-145">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b11ef-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b11ef-146">使用 SQLite 時，在此步驟中不需要進行遷移。</span><span class="sxs-lookup"><span data-stu-id="b11ef-146">Migrations are not necessary at this step when using SQLite.</span></span>

[!INCLUDE [more information on the CLI for EF Core](~/includes/ef-cli.md)]

<span data-ttu-id="b11ef-147">若是 LocalDB，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b11ef-147">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="b11ef-148">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="b11ef-148">Test Register and Login</span></span>

<span data-ttu-id="b11ef-149">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="b11ef-149">Run the app and register a user.</span></span> <span data-ttu-id="b11ef-150">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="b11ef-150">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="b11ef-151">設定 Identity 服務</span><span class="sxs-lookup"><span data-stu-id="b11ef-151">Configure Identity services</span></span>

<span data-ttu-id="b11ef-152">服務會在中加入 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-152">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="b11ef-153">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="b11ef-153">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="b11ef-154">上述的反白顯示程式碼會 Identity 使用預設選項值進行設定。</span><span class="sxs-lookup"><span data-stu-id="b11ef-154">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="b11ef-155">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="b11ef-155">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="b11ef-156">藉由呼叫來啟用 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-156"> is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="b11ef-157">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="b11ef-157">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="b11ef-158">範本產生的應用程式不會使用[授權](xref:security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-158">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="b11ef-159">`app.UseAuthorization`包含，以確保在應用程式新增授權時，會以正確的順序新增。</span><span class="sxs-lookup"><span data-stu-id="b11ef-159">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="b11ef-160">`UseRouting``UseAuthentication` `UseAuthorization` `UseEndpoints` 必須以上述程式碼中所示的順序來呼叫、、和。</span><span class="sxs-lookup"><span data-stu-id="b11ef-160">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="b11ef-161">如需和的詳細資訊 `IdentityOptions` `Startup` ，請參閱 <xref:Microsoft.AspNetCore.Identity.IdentityOptions> 和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-161">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="b11ef-162">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="b11ef-162">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b11ef-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b11ef-163">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b11ef-164">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="b11ef-164">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="b11ef-165">遵循[Scaffold 身分識別， Razor 並提供授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示來產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b11ef-165">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="b11ef-166">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b11ef-166">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b11ef-167">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="b11ef-167">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="b11ef-168">否則，請使用正確的命名空間 `ApplicationDbContext` ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-168">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="b11ef-169">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="b11ef-169">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="b11ef-170">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="b11ef-170">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="b11ef-171">如需有關樣板的詳細資訊 Identity ，請參閱[ Razor 使用授權將身分識別 Scaffold 至專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-171">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="b11ef-172">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="b11ef-172">Examine Register</span></span>

<span data-ttu-id="b11ef-173">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync` 就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="b11ef-173">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="b11ef-174">[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)會在物件上建立使用者 `_userManager` ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-174">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="b11ef-175">如果成功建立使用者，則會呼叫來登入使用者 `_signInManager.SignInAsync` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-175">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="b11ef-176">請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="b11ef-176">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="b11ef-177">登入</span><span class="sxs-lookup"><span data-stu-id="b11ef-177">Log in</span></span>

<span data-ttu-id="b11ef-178">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="b11ef-178">The Login form is displayed when:</span></span>

* <span data-ttu-id="b11ef-179">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="b11ef-179">The **Log in** link is selected.</span></span>
* <span data-ttu-id="b11ef-180">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="b11ef-180">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="b11ef-181">提交登入頁面上的表單時， `OnPostAsync` 會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="b11ef-181">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="b11ef-182">`PasswordSignInAsync`會在物件上呼叫 `_signInManager` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-182">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="b11ef-183">如需有關如何進行授權決策的詳細資訊，請參閱 <xref:security/authorization/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-183">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="b11ef-184">登出</span><span class="sxs-lookup"><span data-stu-id="b11ef-184">Log out</span></span>

<span data-ttu-id="b11ef-185">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="b11ef-185">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="b11ef-186">在上述程式碼中，程式碼必須是重新導向，才能 `return RedirectToPage();` 讓瀏覽器執行新的要求，並更新使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="b11ef-186">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="b11ef-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="b11ef-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="b11ef-188">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="b11ef-188">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-cshtml[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="b11ef-189">測驗Identity</span><span class="sxs-lookup"><span data-stu-id="b11ef-189">Test Identity</span></span>

<span data-ttu-id="b11ef-190">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="b11ef-190">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="b11ef-191">若要測試 Identity ，請新增 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-191">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="b11ef-192">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="b11ef-192">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="b11ef-193">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="b11ef-193">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="b11ef-194">看Identity</span><span class="sxs-lookup"><span data-stu-id="b11ef-194">Explore Identity</span></span>

<span data-ttu-id="b11ef-195">若要 Identity 更詳細地探索：</span><span class="sxs-lookup"><span data-stu-id="b11ef-195">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="b11ef-196">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="b11ef-196">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="b11ef-197">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="b11ef-197">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="b11ef-198">要素</span><span class="sxs-lookup"><span data-stu-id="b11ef-198"> Components</span></span>

<span data-ttu-id="b11ef-199">所有 Identity 相依的 NuGet 套件都包含在[ASP.NET Core 共用架構](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)中。</span><span class="sxs-lookup"><span data-stu-id="b11ef-199">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="b11ef-200">的主要封裝 Identity 是[AspNetCore。 Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="b11ef-200">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="b11ef-201">此套件包含 ASP.NET Core 的核心介面集 Identity ，由所包含 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-201">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="b11ef-202">遷移至 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="b11ef-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="b11ef-203">如需有關遷移現有存放區的詳細資訊和指引 Identity ，請參閱[遷移驗證和 Identity ](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-203">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="b11ef-204">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="b11ef-204">Setting password strength</span></span>

<span data-ttu-id="b11ef-205">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="b11ef-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="b11ef-206">AddDefault Identity 並新增Identity</span><span class="sxs-lookup"><span data-stu-id="b11ef-206">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="b11ef-207"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="b11ef-207"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="b11ef-208">呼叫 `AddDefaultIdentity` 類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="b11ef-208">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="b11ef-209">如需詳細資訊，請參閱[AddDefault Identity source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-209">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="b11ef-210">防止發行靜態 Identity 資產</span><span class="sxs-lookup"><span data-stu-id="b11ef-210">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="b11ef-211">若要防止將靜態 Identity 資產發行 (的樣式表單和 JavaScript 檔案，以供 Identity UI) 至 web 根目錄，請將下列 `ResolveStaticWebAssetsInputsDependsOn` 屬性和 `RemoveIdentityAssets` 目標新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="b11ef-211">To prevent publishing static Identity assets (stylesheets and JavaScript files for Identity UI) to the web root, add the following `ResolveStaticWebAssetsInputsDependsOn` property and `RemoveIdentityAssets` target to the app's project file:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b11ef-212">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b11ef-212">Next Steps</span></span>

* <span data-ttu-id="b11ef-213">[ASP.NET Core Identity 原始碼](https://github.com/dotnet/aspnetcore/tree/master/src/Identity)</span><span class="sxs-lookup"><span data-stu-id="b11ef-213">[ASP.NET Core Identity source code](https://github.com/dotnet/aspnetcore/tree/master/src/Identity)</span></span>
* <span data-ttu-id="b11ef-214">如需使用 SQLite 進行設定的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-214">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="b11ef-215">[配置Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="b11ef-215">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b11ef-216">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b11ef-216">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b11ef-217">ASP.NET Core Identity 是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="b11ef-217">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="b11ef-218">使用者可以建立具有儲存在中之登入資訊的帳戶， Identity 或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="b11ef-218">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="b11ef-219">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-219">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

Identity<span data-ttu-id="b11ef-220">可以使用 SQL Server 資料庫來設定，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="b11ef-220"> can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="b11ef-221">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="b11ef-221">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="b11ef-222">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([如何下載) ](xref:index#how-to-download-a-sample)) 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-222">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="b11ef-223">在本主題中，您將瞭解如何使用 Identity 來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="b11ef-223">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="b11ef-224">如需有關建立使用之應用程式的詳細指示 Identity ，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="b11ef-224">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="b11ef-225">AddDefault Identity 並新增Identity</span><span class="sxs-lookup"><span data-stu-id="b11ef-225">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="b11ef-226"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="b11ef-226"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="b11ef-227">呼叫 `AddDefaultIdentity` 類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="b11ef-227">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="b11ef-228">如需詳細資訊，請參閱[AddDefault Identity source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-228">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="b11ef-229">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b11ef-229">Create a Web app with authentication</span></span>

<span data-ttu-id="b11ef-230">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="b11ef-230">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b11ef-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b11ef-231">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b11ef-232">選取 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="b11ef-232">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="b11ef-233">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="b11ef-233">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b11ef-234">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="b11ef-234">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="b11ef-235">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b11ef-235">Click **OK**.</span></span>
* <span data-ttu-id="b11ef-236">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="b11ef-236">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="b11ef-237">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="b11ef-237">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="b11ef-238">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b11ef-238">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="b11ef-239">產生的專案會[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-239">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="b11ef-240">Identity Razor 類別庫會以區域公開端點 `Identity` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-240">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="b11ef-241">例如：</span><span class="sxs-lookup"><span data-stu-id="b11ef-241">For example:</span></span>

* <span data-ttu-id="b11ef-242">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="b11ef-242">/Identity/Account/Login</span></span>
* <span data-ttu-id="b11ef-243">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="b11ef-243">/Identity/Account/Logout</span></span>
* <span data-ttu-id="b11ef-244">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="b11ef-244">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="b11ef-245">套用移轉</span><span class="sxs-lookup"><span data-stu-id="b11ef-245">Apply migrations</span></span>

<span data-ttu-id="b11ef-246">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="b11ef-246">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b11ef-247">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b11ef-247">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b11ef-248">在套件管理員主控台中執行下列命令 (PMC) ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-248">Run the following command in the Package Manager Console (PMC):</span></span>

```powershell
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="b11ef-249">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b11ef-249">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="b11ef-250">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="b11ef-250">Test Register and Login</span></span>

<span data-ttu-id="b11ef-251">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="b11ef-251">Run the app and register a user.</span></span> <span data-ttu-id="b11ef-252">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="b11ef-252">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="b11ef-253">設定 Identity 服務</span><span class="sxs-lookup"><span data-stu-id="b11ef-253">Configure Identity services</span></span>

<span data-ttu-id="b11ef-254">服務會在中加入 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-254">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="b11ef-255">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="b11ef-255">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="b11ef-256">上述程式碼會 Identity 使用預設選項值進行設定。</span><span class="sxs-lookup"><span data-stu-id="b11ef-256">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="b11ef-257">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="b11ef-257">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="b11ef-258">會藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用。</span><span class="sxs-lookup"><span data-stu-id="b11ef-258"> is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="b11ef-259">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="b11ef-259">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="b11ef-260">如需詳細資訊，請參閱[ Identity Options 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-260">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="b11ef-261">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="b11ef-261">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="b11ef-262">遵循[Scaffold 身分識別， Razor 並提供授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示來產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b11ef-262">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b11ef-263">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b11ef-263">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b11ef-264">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="b11ef-264">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="b11ef-265">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b11ef-265">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b11ef-266">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="b11ef-266">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="b11ef-267">否則，請使用正確的命名空間 `ApplicationDbContext` ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-267">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="b11ef-268">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="b11ef-268">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="b11ef-269">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="b11ef-269">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="b11ef-270">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="b11ef-270">Examine Register</span></span>

<span data-ttu-id="b11ef-271">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync` 就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="b11ef-271">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="b11ef-272">[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)會在物件上建立使用者 `_userManager` ：</span><span class="sxs-lookup"><span data-stu-id="b11ef-272">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="b11ef-273">如果成功建立使用者，則會呼叫來登入使用者 `_signInManager.SignInAsync` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-273">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="b11ef-274">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="b11ef-274">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="b11ef-275">登入</span><span class="sxs-lookup"><span data-stu-id="b11ef-275">Log in</span></span>

<span data-ttu-id="b11ef-276">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="b11ef-276">The Login form is displayed when:</span></span>

* <span data-ttu-id="b11ef-277">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="b11ef-277">The **Log in** link is selected.</span></span>
* <span data-ttu-id="b11ef-278">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="b11ef-278">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="b11ef-279">提交登入頁面上的表單時， `OnPostAsync` 會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="b11ef-279">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="b11ef-280">`PasswordSignInAsync`會在物件上呼叫 `_signInManager` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-280">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="b11ef-281">如需有關如何進行授權決策的詳細資訊，請參閱 <xref:security/authorization/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-281">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="b11ef-282">登出</span><span class="sxs-lookup"><span data-stu-id="b11ef-282">Log out</span></span>

<span data-ttu-id="b11ef-283">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="b11ef-283">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="b11ef-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="b11ef-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="b11ef-285">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="b11ef-285">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-cshtml[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="b11ef-286">測驗Identity</span><span class="sxs-lookup"><span data-stu-id="b11ef-286">Test Identity</span></span>

<span data-ttu-id="b11ef-287">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="b11ef-287">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="b11ef-288">若要進行測試 Identity ，請將新增 [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) 至 [隱私權] 頁面。</span><span class="sxs-lookup"><span data-stu-id="b11ef-288">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="b11ef-289">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="b11ef-289">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="b11ef-290">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="b11ef-290">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="b11ef-291">看Identity</span><span class="sxs-lookup"><span data-stu-id="b11ef-291">Explore Identity</span></span>

<span data-ttu-id="b11ef-292">若要 Identity 更詳細地探索：</span><span class="sxs-lookup"><span data-stu-id="b11ef-292">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="b11ef-293">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="b11ef-293">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="b11ef-294">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="b11ef-294">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="b11ef-295">要素</span><span class="sxs-lookup"><span data-stu-id="b11ef-295"> Components</span></span>

<span data-ttu-id="b11ef-296">所有 Identity 相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="b11ef-296">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b11ef-297">的主要封裝 Identity 是[AspNetCore。 Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="b11ef-297">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="b11ef-298">此套件包含 ASP.NET Core 的核心介面集 Identity ，由所包含 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-298">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="b11ef-299">遷移至 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="b11ef-299">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="b11ef-300">如需有關遷移現有存放區的詳細資訊和指引 Identity ，請參閱[遷移驗證和 Identity ](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="b11ef-300">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="b11ef-301">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="b11ef-301">Setting password strength</span></span>

<span data-ttu-id="b11ef-302">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="b11ef-302">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b11ef-303">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b11ef-303">Next Steps</span></span>

* <span data-ttu-id="b11ef-304">如需使用 SQLite 進行設定的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity 。</span><span class="sxs-lookup"><span data-stu-id="b11ef-304">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="b11ef-305">[配置Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="b11ef-305">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
