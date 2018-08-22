---
title: ASP.NET core 身分識別簡介
author: rick-anderson
description: 使用 ASP.NET Core 應用程式中使用身分識別。 了解如何設定密碼的需求 （RequireDigit、 RequiredLength、 RequiredUniqueChars，等等）。
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: fd5fa2fd1e069bf10f3baea38b1fe9f951dc4a7d
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2018
ms.locfileid: "41824685"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="6c969-104">ASP.NET core 身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="6c969-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="6c969-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6c969-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6c969-106">ASP.NET Core Identity 是將登入功能加入至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="6c969-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="6c969-107">使用者可以建立的帳戶與儲存在身分識別的登入資訊，或者可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="6c969-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="6c969-108">支援的外部登入提供者包括[Facebook、 Google、 Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="6c969-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="6c969-109">可以使用 SQL Server 資料庫來儲存使用者名稱、 密碼和設定檔資料，設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="6c969-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="6c969-110">或者，另一個持續性存放區可用，例如 Azure 資料表儲存體。</span><span class="sxs-lookup"><span data-stu-id="6c969-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="6c969-111">檢視或下載範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="6c969-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="6c969-112">（如何下載）</span><span class="sxs-lookup"><span data-stu-id="6c969-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="6c969-113">本主題中，您將了解如何使用身分識別來註冊、 登入，並登出使用者。</span><span class="sxs-lookup"><span data-stu-id="6c969-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="6c969-114">如需有關建立使用身分識別的應用程式的詳細指示，請參閱本文結尾 「 後續步驟 」 一節。</span><span class="sxs-lookup"><span data-stu-id="6c969-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="6c969-115">建立驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="6c969-115">Create a Web app with authentication</span></span>

<span data-ttu-id="6c969-116">個別使用者帳戶中建立 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="6c969-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c969-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c969-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6c969-118">選取 [檔案]  >  [新增]  >  [專案]。</span><span class="sxs-lookup"><span data-stu-id="6c969-118">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="6c969-119">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="6c969-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6c969-120">將專案命名為**WebApp1**將專案下載相同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="6c969-120">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="6c969-121">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="6c969-121">Click **OK**.</span></span>
* <span data-ttu-id="6c969-122">選取 ASP.NET Core **Web 應用程式**ASP.NET Core 2.1 中，然後選取**變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="6c969-122">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="6c969-123">選取 **個別使用者帳戶**然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="6c969-123">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c969-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6c969-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="6c969-125">產生的專案提供[ASP.NET Core Identity](xref:security/authentication/identity)作為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="6c969-125">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="6c969-126">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="6c969-126">Test Register and Login</span></span>

<span data-ttu-id="6c969-127">執行應用程式並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="6c969-127">Run the app and register a user.</span></span> <span data-ttu-id="6c969-128">根據您的螢幕大小，您可能需要選取瀏覽切換按鈕，以查看**註冊**並**登入**連結。</span><span class="sxs-lookup"><span data-stu-id="6c969-128">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![切換瀏覽列按鈕](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="6c969-130">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="6c969-130">Configure Identity services</span></span>

<span data-ttu-id="6c969-131">服務會加入`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="6c969-131">Services are added in `ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="6c969-132">上述程式碼會使用預設選項值設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="6c969-132">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="6c969-133">服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="6c969-133">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="6c969-134">藉由呼叫啟用身分識別[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。</span><span class="sxs-lookup"><span data-stu-id="6c969-134">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="6c969-135">`UseAuthentication` 新增驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。</span><span class="sxs-lookup"><span data-stu-id="6c969-135">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="6c969-136">服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="6c969-136">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="6c969-137">藉由呼叫應用程式啟用身分識別`UseAuthentication`在`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="6c969-137">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="6c969-138">`UseAuthentication` 新增驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。</span><span class="sxs-lookup"><span data-stu-id="6c969-138">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="6c969-139">這些服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="6c969-139">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="6c969-140">藉由呼叫應用程式啟用身分識別`UseIdentity`在`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="6c969-140">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="6c969-141">`UseIdentity` 新增 cookie 為基礎的驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。</span><span class="sxs-lookup"><span data-stu-id="6c969-141">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="6c969-142">如需詳細資訊，請參閱 < [IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)並[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="6c969-142">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="6c969-143">Scaffold 註冊、 登入和登出</span><span class="sxs-lookup"><span data-stu-id="6c969-143">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="6c969-144">請遵循[Scaffold Razor 專案具有授權的身分識別](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示。</span><span class="sxs-lookup"><span data-stu-id="6c969-144">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c969-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c969-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6c969-146">新增註冊、 登入和登出的檔案。</span><span class="sxs-lookup"><span data-stu-id="6c969-146">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c969-147">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6c969-147">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6c969-148">如果您建立的專案名稱**WebApp1**，執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="6c969-148">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="6c969-149">否則，請使用正確的命名空間，如`ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="6c969-149">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="6c969-150">PowerShell 會使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="6c969-150">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="6c969-151">使用 PowerShell 時，逸出分號，在檔案清單，或置於雙引號括住，如上述範例所示的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="6c969-151">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="6c969-152">檢查暫存器</span><span class="sxs-lookup"><span data-stu-id="6c969-152">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="6c969-153">當使用者按一下**註冊**連結，`RegisterModel.OnPostAsync`叫用動作。</span><span class="sxs-lookup"><span data-stu-id="6c969-153">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="6c969-154">使用者由[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)上`_userManager`物件。</span><span class="sxs-lookup"><span data-stu-id="6c969-154">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="6c969-155">`_userManager` 是由提供相依性插入）：</span><span class="sxs-lookup"><span data-stu-id="6c969-155">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="6c969-156">當使用者按一下**註冊**連結`Register`上叫用動作`AccountController`。</span><span class="sxs-lookup"><span data-stu-id="6c969-156">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="6c969-157">`Register`動作會建立使用者，藉由呼叫`CreateAsync`上`_userManager`物件 (提供給`AccountController`由相依性插入):</span><span class="sxs-lookup"><span data-stu-id="6c969-157">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="6c969-158">如果已成功建立使用者，使用者會登入的呼叫所`_signInManager.SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="6c969-158">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="6c969-159">**注意︰** 請參閱 <<c2> [ 帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)的步驟，以避免在註冊的立即登入。</span><span class="sxs-lookup"><span data-stu-id="6c969-159">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="6c969-160">登入</span><span class="sxs-lookup"><span data-stu-id="6c969-160">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c969-161">登入表單顯示時：</span><span class="sxs-lookup"><span data-stu-id="6c969-161">The Login form is displayed when:</span></span>

* <span data-ttu-id="6c969-162">**登入**選取連結。</span><span class="sxs-lookup"><span data-stu-id="6c969-162">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="6c969-163">當使用者存取的頁面時，它們未經過驗證**或**獲授權，就會重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="6c969-163">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="6c969-164">登入頁面上的表單提交時，`OnPostAsync`呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="6c969-164">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="6c969-165">`PasswordSignInAsync` 呼叫`_signInManager`（由相依性插入提供） 的物件。</span><span class="sxs-lookup"><span data-stu-id="6c969-165">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="6c969-166">基底`Controller`類別會公開`User`屬性，您可以從控制器方法存取。</span><span class="sxs-lookup"><span data-stu-id="6c969-166">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="6c969-167">比方說，您可以列舉`User.Claims`並進行授權決策。</span><span class="sxs-lookup"><span data-stu-id="6c969-167">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="6c969-168">如需詳細資訊，請參閱 <<c0> [ 授權](xref:security/authorization/index)。</span><span class="sxs-lookup"><span data-stu-id="6c969-168">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c969-169">當使用者選取時，會顯示登入表單**登入**存取需要驗證的網頁時，會重新導向或連結。</span><span class="sxs-lookup"><span data-stu-id="6c969-169">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="6c969-170">當使用者提交表單時的登入頁面上， `AccountController` `Login`呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="6c969-170">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="6c969-171">`Login`動作會呼叫`PasswordSignInAsync`上`_signInManager`物件 (提供給`AccountController`由相依性插入)。</span><span class="sxs-lookup"><span data-stu-id="6c969-171">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="6c969-172">基底 (`Controller`或是`PageModel`) 類別會公開`User`屬性。</span><span class="sxs-lookup"><span data-stu-id="6c969-172">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="6c969-173">比方說，`User.Claims`可以列舉來製作授權決策。</span><span class="sxs-lookup"><span data-stu-id="6c969-173">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="6c969-174">登出</span><span class="sxs-lookup"><span data-stu-id="6c969-174">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c969-175">**登出** 連結會叫用`LogoutModel.OnPost`動作。</span><span class="sxs-lookup"><span data-stu-id="6c969-175">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="6c969-176">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="6c969-176">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="6c969-177">不重新導向之後呼叫`SignOutAsync`或使用者將會**不**登出。</span><span class="sxs-lookup"><span data-stu-id="6c969-177">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="6c969-178">中所指定的張貼*Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c969-178">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   <span data-ttu-id="6c969-179">按一下 **登出**連結呼叫`LogOut`動作。</span><span class="sxs-lookup"><span data-stu-id="6c969-179">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="6c969-180">上述程式碼會呼叫`_signInManager.SignOutAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="6c969-180">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="6c969-181">`SignOutAsync`方法會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="6c969-181">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="6c969-182">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="6c969-182">Test Identity</span></span>

<span data-ttu-id="6c969-183">預設的 web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="6c969-183">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="6c969-184">若要測試識別，新增[ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)至 About 頁面。</span><span class="sxs-lookup"><span data-stu-id="6c969-184">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="6c969-185">如果您登入，登出。執行應用程式，然後選取**關於**連結。</span><span class="sxs-lookup"><span data-stu-id="6c969-185">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="6c969-186">將您重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="6c969-186">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="6c969-187">瀏覽身分識別</span><span class="sxs-lookup"><span data-stu-id="6c969-187">Explore Identity</span></span>

<span data-ttu-id="6c969-188">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="6c969-188">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="6c969-189">建立完整的身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="6c969-189">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="6c969-190">檢查每個頁面和逐步執行偵錯工具的來源。</span><span class="sxs-lookup"><span data-stu-id="6c969-190">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="6c969-191">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="6c969-191">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c969-192">中包含所有身分識別相依的 NuGet 套件[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="6c969-192">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
::: moniker-end

<span data-ttu-id="6c969-193">身分識別的主要套件[Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)。</span><span class="sxs-lookup"><span data-stu-id="6c969-193">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="6c969-194">此封裝包含一組核心介面的 ASP.NET Core 身分識別，並包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。</span><span class="sxs-lookup"><span data-stu-id="6c969-194">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="6c969-195">移轉至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="6c969-195">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="6c969-196">如需詳細資訊和移轉您現有的身分識別存放區的指引，請參閱[移轉驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="6c969-196">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="6c969-197">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="6c969-197">Setting password strength</span></span>

<span data-ttu-id="6c969-198">請參閱[組態](#pw)如需設定的最小的密碼需求的範例。</span><span class="sxs-lookup"><span data-stu-id="6c969-198">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c969-199">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6c969-199">Next Steps</span></span>

* [<span data-ttu-id="6c969-200">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="6c969-200">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="6c969-201">[設定身分識別主索引鍵資料類型](xref:security/authentication/identity-primary-key-configuration)。</span><span class="sxs-lookup"><span data-stu-id="6c969-201">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
