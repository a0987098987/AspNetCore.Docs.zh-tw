---
title: 簡介 Identity ASP.NET Core
author: rick-anderson
description: 搭配 Identity ASP.NET Core 應用程式使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
ms.author: riande
ms.date: 01/15/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity
ms.openlocfilehash: 6d9532ed8ff89735f41c362b27826436c37c6cb5
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84452131"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="66fb3-104">簡介 Identity ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66fb3-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="66fb3-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="66fb3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="66fb3-106">ASP.NET Core Identity ：</span><span class="sxs-lookup"><span data-stu-id="66fb3-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="66fb3-107">是支援使用者介面（UI）登入功能的 API。</span><span class="sxs-lookup"><span data-stu-id="66fb3-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="66fb3-108">管理使用者、密碼、設定檔資料、角色、宣告、權杖、電子郵件確認等等。</span><span class="sxs-lookup"><span data-stu-id="66fb3-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="66fb3-109">使用者可以建立具有儲存在中之登入資訊的帳戶， Identity 或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="66fb3-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="66fb3-110">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="66fb3-111">[ Identity 原始程式碼](https://github.com/dotnet/AspNetCore/tree/master/src/Identity)可在 GitHub 上取得。</span><span class="sxs-lookup"><span data-stu-id="66fb3-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="66fb3-112">[Scaffold Identity ](xref:security/authentication/scaffold-identity)和會查看產生的檔案，以檢查與的範本互動 Identity 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

Identity<span data-ttu-id="66fb3-113">通常會使用 SQL Server 資料庫來設定，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="66fb3-113"> is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="66fb3-114">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="66fb3-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="66fb3-115">在本主題中，您將瞭解如何使用 Identity 來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="66fb3-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="66fb3-116">注意：範本會將使用者名稱和電子郵件視為相同。</span><span class="sxs-lookup"><span data-stu-id="66fb3-116">Note: the templates treat username and email as the same for users.</span></span> <span data-ttu-id="66fb3-117">如需有關建立使用之應用程式的詳細指示 Identity ，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="66fb3-117">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="66fb3-118">[Microsoft 身分識別平臺](/azure/active-directory/develop/)是：</span><span class="sxs-lookup"><span data-stu-id="66fb3-118">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="66fb3-119">Azure Active Directory （Azure AD）開發人員平臺的演進。</span><span class="sxs-lookup"><span data-stu-id="66fb3-119">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="66fb3-120">與 ASP.NET Core 無關 Identity 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-120">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="66fb3-121">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="66fb3-121">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="66fb3-122">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="66fb3-122">Create a Web app with authentication</span></span>

<span data-ttu-id="66fb3-123">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="66fb3-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="66fb3-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66fb3-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="66fb3-125">選取 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="66fb3-125">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="66fb3-126">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="66fb3-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="66fb3-127">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="66fb3-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="66fb3-128">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="66fb3-128">Click **OK**.</span></span>
* <span data-ttu-id="66fb3-129">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="66fb3-129">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="66fb3-130">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="66fb3-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="66fb3-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="66fb3-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="66fb3-132">上述命令會 Razor 使用 SQLite 建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="66fb3-132">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="66fb3-133">若要使用 LocalDB 建立 web 應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="66fb3-133">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="66fb3-134">產生的專案會[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-134">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="66fb3-135">Identity Razor 類別庫會以區域公開端點 `Identity` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-135">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="66fb3-136">例如：</span><span class="sxs-lookup"><span data-stu-id="66fb3-136">For example:</span></span>

* <span data-ttu-id="66fb3-137">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="66fb3-137">/Identity/Account/Login</span></span>
* <span data-ttu-id="66fb3-138">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="66fb3-138">/Identity/Account/Logout</span></span>
* <span data-ttu-id="66fb3-139">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="66fb3-139">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="66fb3-140">套用移轉</span><span class="sxs-lookup"><span data-stu-id="66fb3-140">Apply migrations</span></span>

<span data-ttu-id="66fb3-141">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="66fb3-141">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="66fb3-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66fb3-142">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="66fb3-143">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="66fb3-143">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-cli"></a>[<span data-ttu-id="66fb3-144">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="66fb3-144">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="66fb3-145">使用 SQLite 時，在此步驟中不需要進行遷移。</span><span class="sxs-lookup"><span data-stu-id="66fb3-145">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="66fb3-146">若是 LocalDB，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="66fb3-146">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="66fb3-147">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="66fb3-147">Test Register and Login</span></span>

<span data-ttu-id="66fb3-148">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="66fb3-148">Run the app and register a user.</span></span> <span data-ttu-id="66fb3-149">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="66fb3-149">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="66fb3-150">設定 Identity 服務</span><span class="sxs-lookup"><span data-stu-id="66fb3-150">Configure Identity services</span></span>

<span data-ttu-id="66fb3-151">服務會在中加入 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-151">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="66fb3-152">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="66fb3-152">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="66fb3-153">上述的反白顯示程式碼會 Identity 使用預設選項值進行設定。</span><span class="sxs-lookup"><span data-stu-id="66fb3-153">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="66fb3-154">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="66fb3-154">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="66fb3-155">藉由呼叫來啟用 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-155"> is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="66fb3-156">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="66fb3-156">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="66fb3-157">範本產生的應用程式不會使用[授權](xref:security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-157">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="66fb3-158">`app.UseAuthorization`包含，以確保在應用程式新增授權時，會以正確的順序新增。</span><span class="sxs-lookup"><span data-stu-id="66fb3-158">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="66fb3-159">`UseRouting``UseAuthentication` `UseAuthorization` `UseEndpoints` 必須以上述程式碼中所示的順序來呼叫、、和。</span><span class="sxs-lookup"><span data-stu-id="66fb3-159">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="66fb3-160">如需和的詳細資訊 `IdentityOptions` `Startup` ，請參閱 <xref:Microsoft.AspNetCore.Identity.IdentityOptions> 和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-160">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="66fb3-161">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="66fb3-161">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="66fb3-162">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66fb3-162">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="66fb3-163">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="66fb3-163">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="66fb3-164">遵循[Scaffold 身分識別， Razor 並提供授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示來產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="66fb3-164">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="66fb3-165">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="66fb3-165">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="66fb3-166">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="66fb3-166">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="66fb3-167">否則，請使用正確的命名空間 `ApplicationDbContext` ：</span><span class="sxs-lookup"><span data-stu-id="66fb3-167">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="66fb3-168">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="66fb3-168">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="66fb3-169">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="66fb3-169">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="66fb3-170">如需有關樣板的詳細資訊 Identity ，請參閱[ Razor 使用授權將身分識別 Scaffold 至專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-170">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="66fb3-171">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="66fb3-171">Examine Register</span></span>

<span data-ttu-id="66fb3-172">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync` 就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="66fb3-172">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="66fb3-173">[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)會在物件上建立使用者 `_userManager` ：</span><span class="sxs-lookup"><span data-stu-id="66fb3-173">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="66fb3-174">如果成功建立使用者，則會呼叫來登入使用者 `_signInManager.SignInAsync` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="66fb3-175">請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="66fb3-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="66fb3-176">登入</span><span class="sxs-lookup"><span data-stu-id="66fb3-176">Log in</span></span>

<span data-ttu-id="66fb3-177">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="66fb3-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="66fb3-178">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="66fb3-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="66fb3-179">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="66fb3-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="66fb3-180">提交登入頁面上的表單時， `OnPostAsync` 會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="66fb3-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="66fb3-181">`PasswordSignInAsync`會在物件上呼叫 `_signInManager` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-181">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="66fb3-182">如需有關如何進行授權決策的詳細資訊，請參閱 <xref:security/authorization/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-182">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="66fb3-183">登出</span><span class="sxs-lookup"><span data-stu-id="66fb3-183">Log out</span></span>

<span data-ttu-id="66fb3-184">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="66fb3-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="66fb3-185">在上述程式碼中，程式碼必須是重新導向，才能 `return RedirectToPage();` 讓瀏覽器執行新的要求，並更新使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="66fb3-185">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="66fb3-186">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="66fb3-186">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="66fb3-187">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="66fb3-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="66fb3-188">測驗Identity</span><span class="sxs-lookup"><span data-stu-id="66fb3-188">Test Identity</span></span>

<span data-ttu-id="66fb3-189">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="66fb3-189">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="66fb3-190">若要測試 Identity ，請新增 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) ：</span><span class="sxs-lookup"><span data-stu-id="66fb3-190">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="66fb3-191">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="66fb3-191">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="66fb3-192">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="66fb3-192">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="66fb3-193">看Identity</span><span class="sxs-lookup"><span data-stu-id="66fb3-193">Explore Identity</span></span>

<span data-ttu-id="66fb3-194">若要 Identity 更詳細地探索：</span><span class="sxs-lookup"><span data-stu-id="66fb3-194">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="66fb3-195">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="66fb3-195">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="66fb3-196">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="66fb3-196">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="66fb3-197">要素</span><span class="sxs-lookup"><span data-stu-id="66fb3-197"> Components</span></span>

<span data-ttu-id="66fb3-198">所有 Identity 相依的 NuGet 套件都包含在[ASP.NET Core 共用架構](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)中。</span><span class="sxs-lookup"><span data-stu-id="66fb3-198">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="66fb3-199">的主要封裝 Identity 是[AspNetCore。 Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="66fb3-199">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="66fb3-200">此套件包含 ASP.NET Core 的核心介面集 Identity ，由所包含 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-200">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="66fb3-201">遷移至 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="66fb3-201">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="66fb3-202">如需有關遷移現有存放區的詳細資訊和指引 Identity ，請參閱[遷移驗證和 Identity ](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-202">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="66fb3-203">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="66fb3-203">Setting password strength</span></span>

<span data-ttu-id="66fb3-204">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="66fb3-204">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="66fb3-205">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="66fb3-205">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="66fb3-206"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="66fb3-206"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="66fb3-207">呼叫 `AddDefaultIdentity` 類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="66fb3-207">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="66fb3-208">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-208">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="66fb3-209">防止發行靜態 Identity 資產</span><span class="sxs-lookup"><span data-stu-id="66fb3-209">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="66fb3-210">若要防止將靜態 Identity 資產（適用于 UI 的樣式表單和 JavaScript 檔案 Identity ）發行至 web 根目錄，請將下列 `ResolveStaticWebAssetsInputsDependsOn` 屬性和 `RemoveIdentityAssets` 目標新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="66fb3-210">To prevent publishing static Identity assets (stylesheets and JavaScript files for Identity UI) to the web root, add the following `ResolveStaticWebAssetsInputsDependsOn` property and `RemoveIdentityAssets` target to the app's project file:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="66fb3-211">後續步驟</span><span class="sxs-lookup"><span data-stu-id="66fb3-211">Next Steps</span></span>

* <span data-ttu-id="66fb3-212">如需使用 SQLite 進行設定的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-212">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="66fb3-213">[配置Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="66fb3-213">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="66fb3-214">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="66fb3-214">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="66fb3-215">ASP.NET Core Identity 是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="66fb3-215">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="66fb3-216">使用者可以建立具有儲存在中之登入資訊的帳戶， Identity 或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="66fb3-216">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="66fb3-217">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-217">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

Identity<span data-ttu-id="66fb3-218">可以使用 SQL Server 資料庫來設定，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="66fb3-218"> can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="66fb3-219">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="66fb3-219">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="66fb3-220">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="66fb3-220">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="66fb3-221">在本主題中，您將瞭解如何使用 Identity 來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="66fb3-221">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="66fb3-222">如需有關建立使用之應用程式的詳細指示 Identity ，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="66fb3-222">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="66fb3-223">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="66fb3-223">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="66fb3-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="66fb3-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="66fb3-225">呼叫 `AddDefaultIdentity` 類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="66fb3-225">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="66fb3-226">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-226">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="66fb3-227">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="66fb3-227">Create a Web app with authentication</span></span>

<span data-ttu-id="66fb3-228">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="66fb3-228">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="66fb3-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66fb3-229">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="66fb3-230">選取 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="66fb3-230">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="66fb3-231">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="66fb3-231">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="66fb3-232">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="66fb3-232">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="66fb3-233">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="66fb3-233">Click **OK**.</span></span>
* <span data-ttu-id="66fb3-234">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="66fb3-234">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="66fb3-235">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="66fb3-235">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="66fb3-236">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="66fb3-236">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="66fb3-237">產生的專案會[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-237">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="66fb3-238">Identity Razor 類別庫會以區域公開端點 `Identity` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-238">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="66fb3-239">例如：</span><span class="sxs-lookup"><span data-stu-id="66fb3-239">For example:</span></span>

* <span data-ttu-id="66fb3-240">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="66fb3-240">/Identity/Account/Login</span></span>
* <span data-ttu-id="66fb3-241">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="66fb3-241">/Identity/Account/Logout</span></span>
* <span data-ttu-id="66fb3-242">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="66fb3-242">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="66fb3-243">套用移轉</span><span class="sxs-lookup"><span data-stu-id="66fb3-243">Apply migrations</span></span>

<span data-ttu-id="66fb3-244">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="66fb3-244">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="66fb3-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66fb3-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="66fb3-246">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="66fb3-246">Run the following command in the Package Manager Console (PMC):</span></span>

```powershell
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="66fb3-247">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="66fb3-247">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="66fb3-248">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="66fb3-248">Test Register and Login</span></span>

<span data-ttu-id="66fb3-249">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="66fb3-249">Run the app and register a user.</span></span> <span data-ttu-id="66fb3-250">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="66fb3-250">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="66fb3-251">設定 Identity 服務</span><span class="sxs-lookup"><span data-stu-id="66fb3-251">Configure Identity services</span></span>

<span data-ttu-id="66fb3-252">服務會在中加入 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-252">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="66fb3-253">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="66fb3-253">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="66fb3-254">上述程式碼會 Identity 使用預設選項值進行設定。</span><span class="sxs-lookup"><span data-stu-id="66fb3-254">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="66fb3-255">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="66fb3-255">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="66fb3-256">會藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用。</span><span class="sxs-lookup"><span data-stu-id="66fb3-256"> is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="66fb3-257">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="66fb3-257">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="66fb3-258">如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-258">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="66fb3-259">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="66fb3-259">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="66fb3-260">遵循[Scaffold 身分識別， Razor 並提供授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示來產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="66fb3-260">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="66fb3-261">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66fb3-261">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="66fb3-262">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="66fb3-262">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="66fb3-263">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="66fb3-263">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="66fb3-264">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="66fb3-264">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="66fb3-265">否則，請使用正確的命名空間 `ApplicationDbContext` ：</span><span class="sxs-lookup"><span data-stu-id="66fb3-265">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="66fb3-266">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="66fb3-266">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="66fb3-267">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="66fb3-267">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="66fb3-268">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="66fb3-268">Examine Register</span></span>

<span data-ttu-id="66fb3-269">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync` 就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="66fb3-269">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="66fb3-270">[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)會在物件上建立使用者 `_userManager` ：</span><span class="sxs-lookup"><span data-stu-id="66fb3-270">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="66fb3-271">如果成功建立使用者，則會呼叫來登入使用者 `_signInManager.SignInAsync` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-271">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="66fb3-272">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="66fb3-272">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="66fb3-273">登入</span><span class="sxs-lookup"><span data-stu-id="66fb3-273">Log in</span></span>

<span data-ttu-id="66fb3-274">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="66fb3-274">The Login form is displayed when:</span></span>

* <span data-ttu-id="66fb3-275">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="66fb3-275">The **Log in** link is selected.</span></span>
* <span data-ttu-id="66fb3-276">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="66fb3-276">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="66fb3-277">提交登入頁面上的表單時， `OnPostAsync` 會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="66fb3-277">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="66fb3-278">`PasswordSignInAsync`會在物件上呼叫 `_signInManager` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-278">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="66fb3-279">如需有關如何進行授權決策的詳細資訊，請參閱 <xref:security/authorization/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-279">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="66fb3-280">登出</span><span class="sxs-lookup"><span data-stu-id="66fb3-280">Log out</span></span>

<span data-ttu-id="66fb3-281">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="66fb3-281">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="66fb3-282">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="66fb3-282">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="66fb3-283">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="66fb3-283">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="66fb3-284">測驗Identity</span><span class="sxs-lookup"><span data-stu-id="66fb3-284">Test Identity</span></span>

<span data-ttu-id="66fb3-285">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="66fb3-285">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="66fb3-286">若要進行測試 Identity ，請將新增 [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) 至 [隱私權] 頁面。</span><span class="sxs-lookup"><span data-stu-id="66fb3-286">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="66fb3-287">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="66fb3-287">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="66fb3-288">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="66fb3-288">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="66fb3-289">看Identity</span><span class="sxs-lookup"><span data-stu-id="66fb3-289">Explore Identity</span></span>

<span data-ttu-id="66fb3-290">若要 Identity 更詳細地探索：</span><span class="sxs-lookup"><span data-stu-id="66fb3-290">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="66fb3-291">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="66fb3-291">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="66fb3-292">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="66fb3-292">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="66fb3-293">要素</span><span class="sxs-lookup"><span data-stu-id="66fb3-293"> Components</span></span>

<span data-ttu-id="66fb3-294">所有 Identity 相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="66fb3-294">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="66fb3-295">的主要封裝 Identity 是[AspNetCore。 Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="66fb3-295">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="66fb3-296">此套件包含 ASP.NET Core 的核心介面集 Identity ，由所包含 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-296">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="66fb3-297">遷移至 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="66fb3-297">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="66fb3-298">如需有關遷移現有存放區的詳細資訊和指引 Identity ，請參閱[遷移驗證和 Identity ](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="66fb3-298">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="66fb3-299">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="66fb3-299">Setting password strength</span></span>

<span data-ttu-id="66fb3-300">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="66fb3-300">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66fb3-301">後續步驟</span><span class="sxs-lookup"><span data-stu-id="66fb3-301">Next Steps</span></span>

* <span data-ttu-id="66fb3-302">如需使用 SQLite 進行設定的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity 。</span><span class="sxs-lookup"><span data-stu-id="66fb3-302">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="66fb3-303">[配置Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="66fb3-303">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
