---
title: 簡介 Identity ASP.NET Core
author: rick-anderson
description: 搭配 Identity ASP.NET Core 應用程式使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
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
ms.openlocfilehash: 31970bd2b52ad83c116067d258aa9dca2d9b3b66
ms.sourcegitcommit: 895e952aec11c91d703fbdd3640a979307b8cc67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85793582"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="8d8a5-104">簡介 Identity ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d8a5-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8d8a5-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8d8a5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8d8a5-106">ASP.NET Core Identity ：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="8d8a5-107">是支援使用者介面（UI）登入功能的 API。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="8d8a5-108">管理使用者、密碼、設定檔資料、角色、宣告、權杖、電子郵件確認等等。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="8d8a5-109">使用者可以建立具有儲存在中之登入資訊的帳戶， Identity 或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="8d8a5-110">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="8d8a5-111">[ Identity 原始程式碼](https://github.com/dotnet/AspNetCore/tree/master/src/Identity)可在 GitHub 上取得。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="8d8a5-112">[Scaffold Identity ](xref:security/authentication/scaffold-identity)和會查看產生的檔案，以檢查與的範本互動 Identity 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

Identity<span data-ttu-id="8d8a5-113">通常會使用 SQL Server 資料庫來設定，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-113"> is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="8d8a5-114">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="8d8a5-115">在本主題中，您將瞭解如何使用 Identity 來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="8d8a5-116">注意：範本會將使用者名稱和電子郵件視為相同。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-116">Note: the templates treat username and email as the same for users.</span></span> <span data-ttu-id="8d8a5-117">如需有關建立使用之應用程式的詳細指示 Identity ，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-117">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="8d8a5-118">[Microsoft 身分識別平臺](/azure/active-directory/develop/)是：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-118">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="8d8a5-119">Azure Active Directory （Azure AD）開發人員平臺的演進。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-119">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="8d8a5-120">與 ASP.NET Core 無關 Identity 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-120">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="8d8a5-121">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-121">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="8d8a5-122">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8d8a5-122">Create a Web app with authentication</span></span>

<span data-ttu-id="8d8a5-123">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8d8a5-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d8a5-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8d8a5-125">選取 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-125">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="8d8a5-126">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8d8a5-127">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="8d8a5-128">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-128">Click **OK**.</span></span>
* <span data-ttu-id="8d8a5-129">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-129">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="8d8a5-130">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="8d8a5-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8d8a5-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="8d8a5-132">上述命令會 Razor 使用 SQLite 建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-132">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="8d8a5-133">若要使用 LocalDB 建立 web 應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-133">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="8d8a5-134">產生的專案會[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-134">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="8d8a5-135">Identity Razor 類別庫會以區域公開端點 `Identity` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-135">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="8d8a5-136">例如：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-136">For example:</span></span>

* <span data-ttu-id="8d8a5-137">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="8d8a5-137">/Identity/Account/Login</span></span>
* <span data-ttu-id="8d8a5-138">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="8d8a5-138">/Identity/Account/Logout</span></span>
* <span data-ttu-id="8d8a5-139">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="8d8a5-139">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="8d8a5-140">套用移轉</span><span class="sxs-lookup"><span data-stu-id="8d8a5-140">Apply migrations</span></span>

<span data-ttu-id="8d8a5-141">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-141">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8d8a5-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d8a5-142">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8d8a5-143">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-143">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-cli"></a>[<span data-ttu-id="8d8a5-144">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8d8a5-144">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8d8a5-145">使用 SQLite 時，在此步驟中不需要進行遷移。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-145">Migrations are not necessary at this step when using SQLite.</span></span>

[!INCLUDE [more information on the CLI for EF Core](~/includes/ef-cli.md)]

<span data-ttu-id="8d8a5-146">若是 LocalDB，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-146">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="8d8a5-147">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="8d8a5-147">Test Register and Login</span></span>

<span data-ttu-id="8d8a5-148">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-148">Run the app and register a user.</span></span> <span data-ttu-id="8d8a5-149">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-149">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="8d8a5-150">設定 Identity 服務</span><span class="sxs-lookup"><span data-stu-id="8d8a5-150">Configure Identity services</span></span>

<span data-ttu-id="8d8a5-151">服務會在中加入 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-151">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="8d8a5-152">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-152">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="8d8a5-153">上述的反白顯示程式碼會 Identity 使用預設選項值進行設定。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-153">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="8d8a5-154">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-154">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="8d8a5-155">藉由呼叫來啟用 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-155"> is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="8d8a5-156">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-156">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="8d8a5-157">範本產生的應用程式不會使用[授權](xref:security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-157">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="8d8a5-158">`app.UseAuthorization`包含，以確保在應用程式新增授權時，會以正確的順序新增。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-158">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="8d8a5-159">`UseRouting``UseAuthentication` `UseAuthorization` `UseEndpoints` 必須以上述程式碼中所示的順序來呼叫、、和。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-159">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="8d8a5-160">如需和的詳細資訊 `IdentityOptions` `Startup` ，請參閱 <xref:Microsoft.AspNetCore.Identity.IdentityOptions> 和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-160">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="8d8a5-161">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="8d8a5-161">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8d8a5-162">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d8a5-162">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8d8a5-163">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-163">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="8d8a5-164">遵循[Scaffold 身分識別， Razor 並提供授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示來產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-164">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="8d8a5-165">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8d8a5-165">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8d8a5-166">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-166">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="8d8a5-167">否則，請使用正確的命名空間 `ApplicationDbContext` ：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-167">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="8d8a5-168">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-168">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="8d8a5-169">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-169">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="8d8a5-170">如需有關樣板的詳細資訊 Identity ，請參閱[ Razor 使用授權將身分識別 Scaffold 至專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-170">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="8d8a5-171">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="8d8a5-171">Examine Register</span></span>

<span data-ttu-id="8d8a5-172">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync` 就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-172">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="8d8a5-173">[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)會在物件上建立使用者 `_userManager` ：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-173">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="8d8a5-174">如果成功建立使用者，則會呼叫來登入使用者 `_signInManager.SignInAsync` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="8d8a5-175">請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="8d8a5-176">登入</span><span class="sxs-lookup"><span data-stu-id="8d8a5-176">Log in</span></span>

<span data-ttu-id="8d8a5-177">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="8d8a5-178">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="8d8a5-179">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="8d8a5-180">提交登入頁面上的表單時， `OnPostAsync` 會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="8d8a5-181">`PasswordSignInAsync`會在物件上呼叫 `_signInManager` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-181">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="8d8a5-182">如需有關如何進行授權決策的詳細資訊，請參閱 <xref:security/authorization/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-182">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="8d8a5-183">登出</span><span class="sxs-lookup"><span data-stu-id="8d8a5-183">Log out</span></span>

<span data-ttu-id="8d8a5-184">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="8d8a5-185">在上述程式碼中，程式碼必須是重新導向，才能 `return RedirectToPage();` 讓瀏覽器執行新的要求，並更新使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-185">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="8d8a5-186">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-186">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="8d8a5-187">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="8d8a5-188">測驗Identity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-188">Test Identity</span></span>

<span data-ttu-id="8d8a5-189">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-189">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="8d8a5-190">若要測試 Identity ，請新增 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) ：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-190">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="8d8a5-191">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-191">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="8d8a5-192">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-192">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="8d8a5-193">看Identity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-193">Explore Identity</span></span>

<span data-ttu-id="8d8a5-194">若要 Identity 更詳細地探索：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-194">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="8d8a5-195">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="8d8a5-195">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="8d8a5-196">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-196">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="8d8a5-197">要素</span><span class="sxs-lookup"><span data-stu-id="8d8a5-197"> Components</span></span>

<span data-ttu-id="8d8a5-198">所有 Identity 相依的 NuGet 套件都包含在[ASP.NET Core 共用架構](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)中。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-198">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="8d8a5-199">的主要封裝 Identity 是[AspNetCore。 Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="8d8a5-199">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="8d8a5-200">此套件包含 ASP.NET Core 的核心介面集 Identity ，由所包含 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-200">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="8d8a5-201">遷移至 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-201">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="8d8a5-202">如需有關遷移現有存放區的詳細資訊和指引 Identity ，請參閱[遷移驗證和 Identity ](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-202">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="8d8a5-203">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="8d8a5-203">Setting password strength</span></span>

<span data-ttu-id="8d8a5-204">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-204">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="8d8a5-205">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-205">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="8d8a5-206"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-206"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="8d8a5-207">呼叫 `AddDefaultIdentity` 類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-207">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="8d8a5-208">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-208">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="8d8a5-209">防止發行靜態 Identity 資產</span><span class="sxs-lookup"><span data-stu-id="8d8a5-209">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="8d8a5-210">若要防止將靜態 Identity 資產（適用于 UI 的樣式表單和 JavaScript 檔案 Identity ）發行至 web 根目錄，請將下列 `ResolveStaticWebAssetsInputsDependsOn` 屬性和 `RemoveIdentityAssets` 目標新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-210">To prevent publishing static Identity assets (stylesheets and JavaScript files for Identity UI) to the web root, add the following `ResolveStaticWebAssetsInputsDependsOn` property and `RemoveIdentityAssets` target to the app's project file:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8d8a5-211">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8d8a5-211">Next Steps</span></span>

* <span data-ttu-id="8d8a5-212">[ASP.NET Core Identity 原始碼](https://github.com/dotnet/aspnetcore/tree/master/src/Identity)</span><span class="sxs-lookup"><span data-stu-id="8d8a5-212">[ASP.NET Core Identity source code](https://github.com/dotnet/aspnetcore/tree/master/src/Identity)</span></span>
* <span data-ttu-id="8d8a5-213">如需使用 SQLite 進行設定的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-213">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="8d8a5-214">[配置Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="8d8a5-214">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8d8a5-215">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8d8a5-215">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8d8a5-216">ASP.NET Core Identity 是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-216">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="8d8a5-217">使用者可以建立具有儲存在中之登入資訊的帳戶， Identity 或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-217">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="8d8a5-218">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-218">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

Identity<span data-ttu-id="8d8a5-219">可以使用 SQL Server 資料庫來設定，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-219"> can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="8d8a5-220">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-220">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="8d8a5-221">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-221">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="8d8a5-222">在本主題中，您將瞭解如何使用 Identity 來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-222">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="8d8a5-223">如需有關建立使用之應用程式的詳細指示 Identity ，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-223">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="8d8a5-224">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-224">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="8d8a5-225"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>已在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-225"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="8d8a5-226">呼叫 `AddDefaultIdentity` 類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-226">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="8d8a5-227">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-227">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="8d8a5-228">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8d8a5-228">Create a Web app with authentication</span></span>

<span data-ttu-id="8d8a5-229">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-229">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8d8a5-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d8a5-230">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8d8a5-231">選取 **[** 檔案] [ > **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-231">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="8d8a5-232">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-232">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8d8a5-233">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-233">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="8d8a5-234">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-234">Click **OK**.</span></span>
* <span data-ttu-id="8d8a5-235">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-235">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="8d8a5-236">選取 [**個別使用者帳戶**]，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-236">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="8d8a5-237">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8d8a5-237">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="8d8a5-238">產生的專案會[提供 Identity ASP.NET Core](xref:security/authentication/identity)做為[ Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-238">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="8d8a5-239">Identity Razor 類別庫會以區域公開端點 `Identity` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-239">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="8d8a5-240">例如：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-240">For example:</span></span>

* <span data-ttu-id="8d8a5-241">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="8d8a5-241">/Identity/Account/Login</span></span>
* <span data-ttu-id="8d8a5-242">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="8d8a5-242">/Identity/Account/Logout</span></span>
* <span data-ttu-id="8d8a5-243">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="8d8a5-243">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="8d8a5-244">套用移轉</span><span class="sxs-lookup"><span data-stu-id="8d8a5-244">Apply migrations</span></span>

<span data-ttu-id="8d8a5-245">套用遷移來初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-245">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8d8a5-246">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d8a5-246">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8d8a5-247">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-247">Run the following command in the Package Manager Console (PMC):</span></span>

```powershell
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="8d8a5-248">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8d8a5-248">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="8d8a5-249">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="8d8a5-249">Test Register and Login</span></span>

<span data-ttu-id="8d8a5-250">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-250">Run the app and register a user.</span></span> <span data-ttu-id="8d8a5-251">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-251">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="8d8a5-252">設定 Identity 服務</span><span class="sxs-lookup"><span data-stu-id="8d8a5-252">Configure Identity services</span></span>

<span data-ttu-id="8d8a5-253">服務會在中加入 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-253">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="8d8a5-254">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-254">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="8d8a5-255">上述程式碼會 Identity 使用預設選項值進行設定。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-255">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="8d8a5-256">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-256">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="8d8a5-257">會藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-257"> is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="8d8a5-258">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-258">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="8d8a5-259">如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-259">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="8d8a5-260">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="8d8a5-260">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="8d8a5-261">遵循[Scaffold 身分識別， Razor 並提供授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示來產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-261">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8d8a5-262">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d8a5-262">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8d8a5-263">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-263">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="8d8a5-264">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8d8a5-264">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8d8a5-265">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-265">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="8d8a5-266">否則，請使用正確的命名空間 `ApplicationDbContext` ：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-266">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="8d8a5-267">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-267">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="8d8a5-268">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-268">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="8d8a5-269">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="8d8a5-269">Examine Register</span></span>

<span data-ttu-id="8d8a5-270">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync` 就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-270">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="8d8a5-271">[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)會在物件上建立使用者 `_userManager` ：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-271">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="8d8a5-272">如果成功建立使用者，則會呼叫來登入使用者 `_signInManager.SignInAsync` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-272">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="8d8a5-273">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-273">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="8d8a5-274">登入</span><span class="sxs-lookup"><span data-stu-id="8d8a5-274">Log in</span></span>

<span data-ttu-id="8d8a5-275">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-275">The Login form is displayed when:</span></span>

* <span data-ttu-id="8d8a5-276">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-276">The **Log in** link is selected.</span></span>
* <span data-ttu-id="8d8a5-277">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-277">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="8d8a5-278">提交登入頁面上的表單時， `OnPostAsync` 會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-278">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="8d8a5-279">`PasswordSignInAsync`會在物件上呼叫 `_signInManager` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-279">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="8d8a5-280">如需有關如何進行授權決策的詳細資訊，請參閱 <xref:security/authorization/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-280">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="8d8a5-281">登出</span><span class="sxs-lookup"><span data-stu-id="8d8a5-281">Log out</span></span>

<span data-ttu-id="8d8a5-282">[**登出**] 連結會叫用 `LogoutModel.OnPost` 動作。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-282">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="8d8a5-283">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-283">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="8d8a5-284">Post 是在*Pages/Shared/_LoginPartial*中指定的。 cshtml：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-284">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="8d8a5-285">測驗Identity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-285">Test Identity</span></span>

<span data-ttu-id="8d8a5-286">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-286">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="8d8a5-287">若要進行測試 Identity ，請將新增 [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) 至 [隱私權] 頁面。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-287">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="8d8a5-288">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-288">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="8d8a5-289">系統會將您重新導向至 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-289">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="8d8a5-290">看Identity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-290">Explore Identity</span></span>

<span data-ttu-id="8d8a5-291">若要 Identity 更詳細地探索：</span><span class="sxs-lookup"><span data-stu-id="8d8a5-291">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="8d8a5-292">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="8d8a5-292">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="8d8a5-293">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-293">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="8d8a5-294">要素</span><span class="sxs-lookup"><span data-stu-id="8d8a5-294"> Components</span></span>

<span data-ttu-id="8d8a5-295">所有 Identity 相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-295">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="8d8a5-296">的主要封裝 Identity 是[AspNetCore。 Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="8d8a5-296">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="8d8a5-297">此套件包含 ASP.NET Core 的核心介面集 Identity ，由所包含 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-297">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="8d8a5-298">遷移至 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="8d8a5-298">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="8d8a5-299">如需有關遷移現有存放區的詳細資訊和指引 Identity ，請參閱[遷移驗證和 Identity ](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-299">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="8d8a5-300">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="8d8a5-300">Setting password strength</span></span>

<span data-ttu-id="8d8a5-301">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-301">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d8a5-302">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8d8a5-302">Next Steps</span></span>

* <span data-ttu-id="8d8a5-303">如需使用 SQLite 進行設定的相關資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity 。</span><span class="sxs-lookup"><span data-stu-id="8d8a5-303">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="8d8a5-304">[配置Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="8d8a5-304">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
