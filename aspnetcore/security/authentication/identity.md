---
title: ASP.NET core 身分識別簡介
author: rick-anderson
description: 使用 ASP.NET Core 應用程式中使用身分識別。 了解如何設定密碼的需求 （RequireDigit、 RequiredLength、 RequiredUniqueChars，等等）。
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 03f89114b516a37ee1d06934f2e549b4d56ff099
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098765"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="a0b10-104">ASP.NET core 身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="a0b10-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="a0b10-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a0b10-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a0b10-106">ASP.NET Core Identity 是將登入功能加入至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="a0b10-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="a0b10-107">使用者可以建立的帳戶與儲存在身分識別的登入資訊，或者可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="a0b10-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="a0b10-108">支援的外部登入提供者包括[Facebook、 Google、 Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a0b10-109">可以使用 SQL Server 資料庫來儲存使用者名稱、 密碼和設定檔資料，設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="a0b10-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a0b10-110">或者，另一個持續性存放區可用，例如 Azure 資料表儲存體。</span><span class="sxs-lookup"><span data-stu-id="a0b10-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="a0b10-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)([如何下載)](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a0b10-111">[View or download the sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a0b10-112">本主題中，您將了解如何使用身分識別來註冊、 登入，並登出使用者。</span><span class="sxs-lookup"><span data-stu-id="a0b10-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="a0b10-113">如需有關建立使用身分識別的應用程式的詳細指示，請參閱本文結尾 「 後續步驟 」 一節。</span><span class="sxs-lookup"><span data-stu-id="a0b10-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="a0b10-114">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a0b10-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="a0b10-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="a0b10-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a0b10-116">呼叫`AddDefaultIdentity`類似於呼叫下列命令：</span><span class="sxs-lookup"><span data-stu-id="a0b10-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="a0b10-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a0b10-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="a0b10-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="a0b10-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="a0b10-119">對 AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="a0b10-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="a0b10-120">請參閱[AddDefaultIdentity 來源](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a0b10-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="a0b10-121">建立驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a0b10-121">Create a Web app with authentication</span></span>

<span data-ttu-id="a0b10-122">個別使用者帳戶中建立 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="a0b10-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0b10-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0b10-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a0b10-124">選取 [檔案]  >  [新增]  >  [專案]。</span><span class="sxs-lookup"><span data-stu-id="a0b10-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="a0b10-125">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="a0b10-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a0b10-126">將專案命名為**WebApp1**將專案下載相同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="a0b10-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="a0b10-127">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="a0b10-127">Click **OK**.</span></span>
* <span data-ttu-id="a0b10-128">選取 ASP.NET Core **Web 應用程式**ASP.NET Core 2.1 中，然後選取**變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="a0b10-128">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="a0b10-129">選取 **個別使用者帳戶**然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="a0b10-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a0b10-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a0b10-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="a0b10-131">產生的專案提供[ASP.NET Core Identity](xref:security/authentication/identity)作為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="a0b10-132">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="a0b10-132">Test Register and Login</span></span>

<span data-ttu-id="a0b10-133">執行應用程式並註冊的使用者。</span><span class="sxs-lookup"><span data-stu-id="a0b10-133">Run the app and register a user.</span></span> <span data-ttu-id="a0b10-134">根據您的螢幕大小，您可能需要選取瀏覽切換按鈕，以查看**註冊**並**登入**連結。</span><span class="sxs-lookup"><span data-stu-id="a0b10-134">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![切換瀏覽列按鈕](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="a0b10-136">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="a0b10-136">Configure Identity services</span></span>

<span data-ttu-id="a0b10-137">服務會加入`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="a0b10-137">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="a0b10-138">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="a0b10-138">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="a0b10-139">下列程式碼不包含產生的範本`CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="a0b10-139">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="a0b10-140">上述程式碼會使用預設選項值設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="a0b10-140">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="a0b10-141">服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-141">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a0b10-142">藉由呼叫啟用身分識別[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-142">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="a0b10-143">`UseAuthentication` 新增驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0b10-143">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="a0b10-144">服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-144">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a0b10-145">藉由呼叫應用程式啟用身分識別`UseAuthentication`在`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="a0b10-145">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="a0b10-146">`UseAuthentication` 新增驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0b10-146">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="a0b10-147">這些服務可透過應用程式[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-147">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a0b10-148">藉由呼叫應用程式啟用身分識別`UseIdentity`在`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="a0b10-148">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="a0b10-149">`UseIdentity` 新增 cookie 為基礎的驗證[中介軟體](xref:fundamentals/middleware/index)至要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0b10-149">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="a0b10-150">如需詳細資訊，請參閱 < [IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)並[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-150">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="a0b10-151">Scaffold 註冊、 登入和登出</span><span class="sxs-lookup"><span data-stu-id="a0b10-151">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="a0b10-152">請遵循[Scaffold Razor 專案具有授權的身分識別](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示來產生這一節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a0b10-152">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a0b10-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0b10-153">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a0b10-154">新增註冊、 登入和登出的檔案。</span><span class="sxs-lookup"><span data-stu-id="a0b10-154">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a0b10-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a0b10-155">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a0b10-156">如果您建立的專案名稱**WebApp1**，執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="a0b10-156">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="a0b10-157">否則，請使用正確的命名空間，如`ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a0b10-157">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a0b10-158">PowerShell 會使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="a0b10-158">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a0b10-159">使用 PowerShell 時，逸出分號，在檔案清單，或置於雙引號括住，如上述範例所示的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="a0b10-159">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="a0b10-160">檢查暫存器</span><span class="sxs-lookup"><span data-stu-id="a0b10-160">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="a0b10-161">當使用者按一下**註冊**連結，`RegisterModel.OnPostAsync`叫用動作。</span><span class="sxs-lookup"><span data-stu-id="a0b10-161">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="a0b10-162">使用者由[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)上`_userManager`物件。</span><span class="sxs-lookup"><span data-stu-id="a0b10-162">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="a0b10-163">`_userManager` 是由提供相依性插入）：</span><span class="sxs-lookup"><span data-stu-id="a0b10-163">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="a0b10-164">當使用者按一下**註冊**連結`Register`上叫用動作`AccountController`。</span><span class="sxs-lookup"><span data-stu-id="a0b10-164">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="a0b10-165">`Register`動作會建立使用者，藉由呼叫`CreateAsync`上`_userManager`物件 (提供給`AccountController`由相依性插入):</span><span class="sxs-lookup"><span data-stu-id="a0b10-165">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="a0b10-166">如果已成功建立使用者，使用者會登入的呼叫所`_signInManager.SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="a0b10-166">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="a0b10-167">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)的步驟，以避免在註冊的立即登入。</span><span class="sxs-lookup"><span data-stu-id="a0b10-167">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="a0b10-168">登入</span><span class="sxs-lookup"><span data-stu-id="a0b10-168">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a0b10-169">登入表單顯示時：</span><span class="sxs-lookup"><span data-stu-id="a0b10-169">The Login form is displayed when:</span></span>

* <span data-ttu-id="a0b10-170">**登入**選取連結。</span><span class="sxs-lookup"><span data-stu-id="a0b10-170">The **Log in** link is selected.</span></span>
* <span data-ttu-id="a0b10-171">使用者嘗試存取受限制的頁面未獲授權可以存取**或**時在還沒有已驗證系統。</span><span class="sxs-lookup"><span data-stu-id="a0b10-171">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="a0b10-172">登入頁面上的表單提交時，`OnPostAsync`呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="a0b10-172">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="a0b10-173">`PasswordSignInAsync` 呼叫`_signInManager`（由相依性插入提供） 的物件。</span><span class="sxs-lookup"><span data-stu-id="a0b10-173">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="a0b10-174">基底`Controller`類別會公開`User`屬性，您可以從控制器方法存取。</span><span class="sxs-lookup"><span data-stu-id="a0b10-174">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="a0b10-175">比方說，您可以列舉`User.Claims`並進行授權決策。</span><span class="sxs-lookup"><span data-stu-id="a0b10-175">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a0b10-176">如需詳細資訊，請參閱<xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="a0b10-176">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a0b10-177">當使用者選取時，會顯示登入表單**登入**存取需要驗證的網頁時，會重新導向或連結。</span><span class="sxs-lookup"><span data-stu-id="a0b10-177">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="a0b10-178">當使用者提交表單時的登入頁面上， `AccountController` `Login`呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="a0b10-178">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="a0b10-179">`Login`動作會呼叫`PasswordSignInAsync`上`_signInManager`物件 (提供給`AccountController`由相依性插入)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-179">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="a0b10-180">基底 (`Controller`或是`PageModel`) 類別會公開`User`屬性。</span><span class="sxs-lookup"><span data-stu-id="a0b10-180">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="a0b10-181">比方說，`User.Claims`可以列舉來製作授權決策。</span><span class="sxs-lookup"><span data-stu-id="a0b10-181">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="a0b10-182">登出</span><span class="sxs-lookup"><span data-stu-id="a0b10-182">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a0b10-183">**登出** 連結會叫用`LogoutModel.OnPost`動作。</span><span class="sxs-lookup"><span data-stu-id="a0b10-183">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="a0b10-184">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="a0b10-184">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="a0b10-185">不重新導向之後呼叫`SignOutAsync`或使用者將會**不**登出。</span><span class="sxs-lookup"><span data-stu-id="a0b10-185">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="a0b10-186">中所指定的張貼*Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a0b10-186">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="a0b10-187">按一下 **登出**連結呼叫`LogOut`動作。</span><span class="sxs-lookup"><span data-stu-id="a0b10-187">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="a0b10-188">上述程式碼會呼叫`_signInManager.SignOutAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="a0b10-188">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="a0b10-189">`SignOutAsync`方法會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="a0b10-189">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="a0b10-190">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="a0b10-190">Test Identity</span></span>

<span data-ttu-id="a0b10-191">預設的 web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="a0b10-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="a0b10-192">若要測試識別，新增[ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute)至 About 頁面。</span><span class="sxs-lookup"><span data-stu-id="a0b10-192">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="a0b10-193">如果您登入，登出。執行應用程式，然後選取**關於**連結。</span><span class="sxs-lookup"><span data-stu-id="a0b10-193">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="a0b10-194">將您重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="a0b10-194">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="a0b10-195">瀏覽身分識別</span><span class="sxs-lookup"><span data-stu-id="a0b10-195">Explore Identity</span></span>

<span data-ttu-id="a0b10-196">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="a0b10-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="a0b10-197">建立完整的身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="a0b10-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="a0b10-198">檢查每個頁面和逐步執行偵錯工具的來源。</span><span class="sxs-lookup"><span data-stu-id="a0b10-198">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="a0b10-199">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="a0b10-199">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a0b10-200">中包含所有身分識別相依的 NuGet 套件[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-200">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="a0b10-201">身分識別的主要套件[Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="a0b10-202">此封裝包含一組核心介面的 ASP.NET Core 身分識別，並包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`。</span><span class="sxs-lookup"><span data-stu-id="a0b10-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a0b10-203">移轉至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="a0b10-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a0b10-204">如需詳細資訊和移轉您現有的身分識別存放區的指引，請參閱[移轉驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="a0b10-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a0b10-205">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="a0b10-205">Setting password strength</span></span>

<span data-ttu-id="a0b10-206">請參閱[組態](#pw)如需設定的最小的密碼需求的範例。</span><span class="sxs-lookup"><span data-stu-id="a0b10-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0b10-207">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a0b10-207">Next Steps</span></span>

* [<span data-ttu-id="a0b10-208">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="a0b10-208">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
