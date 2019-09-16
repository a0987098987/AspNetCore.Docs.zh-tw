---
title: ASP.NET Core 上的身分識別簡介
author: rick-anderson
description: 將身分識別與 ASP.NET Core 應用程式搭配使用。 瞭解如何設定密碼需求（RequireDigit、RequiredLength、RequiredUniqueChars 等）。
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 325a61e6038e79b9a0db72c8360a5cbff2c8ddae
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011211"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="d5c8c-104">ASP.NET Core 上的身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="d5c8c-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="d5c8c-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d5c8c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d5c8c-106">ASP.NET Core 身分識別是將登入功能新增至 ASP.NET Core 應用程式的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="d5c8c-107">使用者可以建立帳戶，其中包含儲存在身分識別中的登入資訊，或可以使用外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="d5c8c-108">支援的外部登入提供者包括[Facebook、Google、Microsoft 帳戶及 Twitter](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d5c8c-109">您可以使用 SQL Server 資料庫來設定身分識別，以儲存使用者名稱、密碼和設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="d5c8c-110">或者，也可以使用另一個持續性存放區，例如 Azure 表格儲存體。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="d5c8c-111">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/)（[如何下載）](xref:index#how-to-download-a-sample)）。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="d5c8c-112">在本主題中，您將瞭解如何使用身分識別來註冊、登入和登出使用者。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="d5c8c-113">如需有關建立使用身分識別之應用程式的詳細指示，請參閱本文結尾的後續步驟一節。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="d5c8c-114">AddDefaultIdentity 和 AddIdentity</span><span class="sxs-lookup"><span data-stu-id="d5c8c-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="d5c8c-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)是在 ASP.NET Core 2.1 中引進。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="d5c8c-116">呼叫`AddDefaultIdentity`類似于呼叫下列內容：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="d5c8c-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="d5c8c-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="d5c8c-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="d5c8c-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="d5c8c-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="d5c8c-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="d5c8c-120">如需詳細資訊，請參閱[AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) 。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="d5c8c-121">建立具有驗證的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d5c8c-121">Create a Web app with authentication</span></span>

<span data-ttu-id="d5c8c-122">建立具有個別使用者帳戶的 ASP.NET Core Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5c8c-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5c8c-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d5c8c-124">選取 [檔案]  >  [新增]  >  [專案]。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="d5c8c-125">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d5c8c-126">將專案命名為**WebApp1** ，使其命名空間與專案下載相同。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="d5c8c-127">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-127">Click **OK**.</span></span>
* <span data-ttu-id="d5c8c-128">選取 ASP.NET Core **Web 應用程式**，然後選取 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="d5c8c-129">選取 [**個別使用者帳戶**]，然後按一下 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d5c8c-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d5c8c-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="d5c8c-131">產生的專案會提供[ASP.NET Core 身分識別](xref:security/authentication/identity)做為[Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="d5c8c-132">身分識別 Razor 類別庫會公開具有`Identity`區域的端點。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="d5c8c-133">例如：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-133">For example:</span></span>

* <span data-ttu-id="d5c8c-134">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="d5c8c-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="d5c8c-135">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="d5c8c-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="d5c8c-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="d5c8c-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="d5c8c-137">套用移轉</span><span class="sxs-lookup"><span data-stu-id="d5c8c-137">Apply migrations</span></span>

<span data-ttu-id="d5c8c-138">將遷移套用至初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5c8c-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5c8c-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d5c8c-140">在 [套件管理員主控台] （PMC）中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d5c8c-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d5c8c-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="d5c8c-142">測試註冊和登入</span><span class="sxs-lookup"><span data-stu-id="d5c8c-142">Test Register and Login</span></span>

<span data-ttu-id="d5c8c-143">執行應用程式並註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-143">Run the app and register a user.</span></span> <span data-ttu-id="d5c8c-144">視您的螢幕大小而定，您可能需要選取 [流覽] 切換按鈕，才能看到 [**註冊**] 和 **[登**入] 連結。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="d5c8c-145">設定身分識別服務</span><span class="sxs-lookup"><span data-stu-id="d5c8c-145">Configure Identity services</span></span>

<span data-ttu-id="d5c8c-146">服務會在中`ConfigureServices`加入。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="d5c8c-147">典型模式是呼叫所有 `Add{Service}` 方法，然後呼叫 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="d5c8c-148">上述程式碼會使用預設選項值來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="d5c8c-149">服務可透過相依性[插入](xref:fundamentals/dependency-injection)提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="d5c8c-150">藉由呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)來啟用身分識別。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="d5c8c-151">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="d5c8c-152">服務可透過相依性[插入](xref:fundamentals/dependency-injection)來提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="d5c8c-153">藉由`UseAuthentication` `Configure`在方法中呼叫來啟用應用程式的身分識別。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="d5c8c-154">`UseAuthentication`將驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="d5c8c-155">這些服務可透過相依性[插入](xref:fundamentals/dependency-injection)來提供給應用程式。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="d5c8c-156">藉由`UseIdentity` `Configure`在方法中呼叫來啟用應用程式的身分識別。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="d5c8c-157">`UseIdentity`將以 cookie 為基礎的驗證[中介軟體](xref:fundamentals/middleware/index)新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="d5c8c-158">如需詳細資訊，請參閱[IdentityOptions 類別](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)和[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="d5c8c-159">Scaffold 註冊、登入和登出</span><span class="sxs-lookup"><span data-stu-id="d5c8c-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="d5c8c-160">將[Scaffold 身分識別遵循具有授權指示的 Razor 專案](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)，以產生本節所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5c8c-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5c8c-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d5c8c-162">新增註冊、登入和登出檔案。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d5c8c-163">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d5c8c-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d5c8c-164">如果您已建立名為**WebApp1**的專案，請執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="d5c8c-165">否則，請使用正確的命名空間`ApplicationDbContext`：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="d5c8c-166">PowerShell 使用分號做為命令分隔符號。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="d5c8c-167">使用 PowerShell 時，請將檔案清單中的分號 escape，或將檔案清單放在雙引號中，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="d5c8c-168">檢查 Register</span><span class="sxs-lookup"><span data-stu-id="d5c8c-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="d5c8c-169">當使用者按一下 [**註冊**] 連結時， `RegisterModel.OnPostAsync`就會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="d5c8c-170">使用者是由`_userManager`物件上的[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)所建立。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="d5c8c-171">`_userManager`由相依性插入所提供）：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="d5c8c-172">當使用者按一下 [**註冊**] 連結時， `Register`就會在上`AccountController`叫用動作。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="d5c8c-173">動作會藉由`_userManager`在物件上`CreateAsync`呼叫（由相依性插入`AccountController`提供給）來建立使用者： `Register`</span><span class="sxs-lookup"><span data-stu-id="d5c8c-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="d5c8c-174">如果成功建立使用者，則會呼叫來`_signInManager.SignInAsync`登入使用者。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="d5c8c-175">**注意：** 請參閱[帳戶確認](xref:security/authentication/accconfirm#prevent-login-at-registration)以取得在註冊時防止立即登入的步驟。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="d5c8c-176">登錄</span><span class="sxs-lookup"><span data-stu-id="d5c8c-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d5c8c-177">登入表單的顯示時機如下：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="d5c8c-178">已選取 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="d5c8c-179">使用者嘗試存取未獲授權存取的受限制頁面，**或**其未經過系統驗證的情況。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="d5c8c-180">提交登入頁面上的表單時， `OnPostAsync`會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="d5c8c-181">`PasswordSignInAsync`會在`_signInManager`物件上呼叫（由相依性插入所提供）。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="d5c8c-182">基類`Controller` 會`User`公開可從控制器方法存取的屬性。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="d5c8c-183">例如，您可以列舉`User.Claims`並做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="d5c8c-184">如需詳細資訊，請參閱 <xref:security/authorization/introduction>。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d5c8c-185">當使用者選取 [**登入**] 連結時，或在存取需要驗證的網頁時，就會顯示登入表單。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="d5c8c-186">當使用者在登入頁面上提交表單時， `AccountController` `Login`會呼叫動作。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="d5c8c-187">動作會呼叫`PasswordSignInAsync` `_signInManager`物件上的（ `AccountController`由相依性插入所提供）。 `Login`</span><span class="sxs-lookup"><span data-stu-id="d5c8c-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="d5c8c-188">基底（`Controller`或`PageModel`）類別會公開`User`屬性。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="d5c8c-189">例如， `User.Claims`可以列舉以進行授權決策。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="d5c8c-190">登出</span><span class="sxs-lookup"><span data-stu-id="d5c8c-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d5c8c-191">[**登出**] 連結`LogoutModel.OnPost`會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="d5c8c-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync)會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="d5c8c-193">Post 是在*Pages/Shared/_LoginPartial*中指定的：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="d5c8c-194">按一下 [**登出**] 連結會呼叫`LogOut`動作。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="d5c8c-195">上述程式碼會呼叫`_signInManager.SignOutAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="d5c8c-196">`SignOutAsync`方法會清除儲存在 cookie 中的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="d5c8c-197">測試身分識別</span><span class="sxs-lookup"><span data-stu-id="d5c8c-197">Test Identity</span></span>

<span data-ttu-id="d5c8c-198">預設的 Web 專案範本允許匿名存取首頁。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="d5c8c-199">若要測試身分識別[`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) ，請將新增至 [隱私權] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="d5c8c-200">如果您已登入，請登出。執行應用程式並選取 [**隱私權**] 連結。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="d5c8c-201">系統會將您重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="d5c8c-202">探索身分識別</span><span class="sxs-lookup"><span data-stu-id="d5c8c-202">Explore Identity</span></span>

<span data-ttu-id="d5c8c-203">若要更詳細地探索身分識別：</span><span class="sxs-lookup"><span data-stu-id="d5c8c-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="d5c8c-204">建立完整身分識別 UI 來源</span><span class="sxs-lookup"><span data-stu-id="d5c8c-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="d5c8c-205">檢查每個頁面的來源，並逐步執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="d5c8c-206">身分識別元件</span><span class="sxs-lookup"><span data-stu-id="d5c8c-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d5c8c-207">所有身分識別相依的 NuGet 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="d5c8c-208">身分識別的主要套件是[AspNetCore。](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="d5c8c-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="d5c8c-209">此套件包含 ASP.NET Core 身分識別的核心介面集，並且包含`Microsoft.AspNetCore.Identity.EntityFrameworkCore`在中。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="d5c8c-210">遷移至 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="d5c8c-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="d5c8c-211">如需有關遷移現有身分識別存放區的詳細資訊和指引，請參閱[遷移驗證和身分識別](xref:migration/identity)。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="d5c8c-212">設定密碼強度</span><span class="sxs-lookup"><span data-stu-id="d5c8c-212">Setting password strength</span></span>

<span data-ttu-id="d5c8c-213">如需設定最小密碼[需求的範例](#pw)，請參閱設定。</span><span class="sxs-lookup"><span data-stu-id="d5c8c-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5c8c-214">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d5c8c-214">Next Steps</span></span>

* [<span data-ttu-id="d5c8c-215">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="d5c8c-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
