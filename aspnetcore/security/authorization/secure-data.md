---
title: 建立 ASP.NET Core 應用程式與受保護的授權的使用者資料
author: rick-anderson
description: 了解如何建立 Razor 頁面的應用程式與受保護的授權的使用者資料。 包含 HTTPS、 驗證、 安全性、 ASP.NET Core 身分識別。
ms.author: riande
ms.date: 01/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 32ced054ba559e66de4a300137d56b7c81a4149b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276376"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="c220f-104">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="c220f-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="c220f-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c220f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="c220f-106">本教學課程會示範如何建立 ASP.NET Core web 應用程式與受保護的授權的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c220f-107">它會顯示的驗證 （登錄） 的使用者的連絡人清單已建立。</span><span class="sxs-lookup"><span data-stu-id="c220f-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c220f-108">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="c220f-108">There are three security groups:</span></span>

* <span data-ttu-id="c220f-109">**註冊使用者**就可以檢視所有已認可的資料，而且可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c220f-110">**管理員**可以核准或拒絕連絡資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c220f-111">只有已核准的連絡人會對使用者顯示。</span><span class="sxs-lookup"><span data-stu-id="c220f-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c220f-112">**系統管理員**可以核准/拒絕並編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c220f-113">在下列影像中，使用者 Rick (`rick@example.com`) 登入。</span><span class="sxs-lookup"><span data-stu-id="c220f-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c220f-114">Rick 只能檢視核准的連絡人和**編輯**/**刪除**/**新建**他連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="c220f-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c220f-115">只有最後一筆記錄，建立由 Rick，顯示**編輯**和**刪除**連結。</span><span class="sxs-lookup"><span data-stu-id="c220f-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c220f-116">除非管理員或系統管理員將狀態變更為 「 已核准 」 其他使用者看最後一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="c220f-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![前面所述的映像](secure-data/_static/rick.png)

<span data-ttu-id="c220f-118">在下圖`manager@contoso.com`已登入，並在管理員角色：</span><span class="sxs-lookup"><span data-stu-id="c220f-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![前面所述的映像](secure-data/_static/manager1.png)

<span data-ttu-id="c220f-120">下圖顯示在管理員的連絡人詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="c220f-120">The following image shows the managers details view of a contact:</span></span>

![前面所述的映像](secure-data/_static/manager.png)

<span data-ttu-id="c220f-122">**核准**和**拒絕**按鈕只會顯示管理員和系統管理員。</span><span class="sxs-lookup"><span data-stu-id="c220f-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c220f-123">在下圖`admin@contoso.com`已登入，並在系統管理員角色：</span><span class="sxs-lookup"><span data-stu-id="c220f-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![前面所述的映像](secure-data/_static/admin.png)

<span data-ttu-id="c220f-125">系統管理員將擁有所有權限。</span><span class="sxs-lookup"><span data-stu-id="c220f-125">The administrator has all privileges.</span></span> <span data-ttu-id="c220f-126">她可以讀取/編輯/刪除任何連絡人，變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="c220f-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c220f-127">應用程式所建立[scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)下列`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="c220f-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c220f-128">這個範例包含下列 「 授權 」 處理常式：</span><span class="sxs-lookup"><span data-stu-id="c220f-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c220f-129">`ContactIsOwnerAuthorizationHandler`： 可確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c220f-130">`ContactManagerAuthorizationHandler`： 允許經理核准或拒絕的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c220f-131">`ContactAdministratorsAuthorizationHandler`： 可讓系統管理員核准或拒絕的連絡人，以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c220f-132">必要條件</span><span class="sxs-lookup"><span data-stu-id="c220f-132">Prerequisites</span></span>

<span data-ttu-id="c220f-133">本教學課程會前進。</span><span class="sxs-lookup"><span data-stu-id="c220f-133">This tutorial is advanced.</span></span> <span data-ttu-id="c220f-134">您應該熟悉：</span><span class="sxs-lookup"><span data-stu-id="c220f-134">You should be familiar with:</span></span>

* [<span data-ttu-id="c220f-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c220f-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c220f-136">驗證</span><span class="sxs-lookup"><span data-stu-id="c220f-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="c220f-137">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="c220f-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c220f-138">授權</span><span class="sxs-lookup"><span data-stu-id="c220f-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="c220f-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c220f-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="c220f-140">請參閱[此 PDF 檔案](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC 版本。</span><span class="sxs-lookup"><span data-stu-id="c220f-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="c220f-141">本教學課程中的 ASP.NET Core 1.1 版本處於[這](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data)資料夾。</span><span class="sxs-lookup"><span data-stu-id="c220f-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="c220f-142">ASP.NET Core 範例處於 1.1[範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)。</span><span class="sxs-lookup"><span data-stu-id="c220f-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c220f-143">起始和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c220f-143">The starter and completed app</span></span>

<span data-ttu-id="c220f-144">[下載](xref:tutorials/index#how-to-download-a-sample)[完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)應用程式。</span><span class="sxs-lookup"><span data-stu-id="c220f-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="c220f-145">[測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="c220f-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c220f-146">起始應用程式</span><span class="sxs-lookup"><span data-stu-id="c220f-146">The starter app</span></span>

<span data-ttu-id="c220f-147">[下載](xref:tutorials/index#how-to-download-a-sample)[入門](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2)應用程式。</span><span class="sxs-lookup"><span data-stu-id="c220f-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="c220f-148">執行應用程式，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c220f-149">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="c220f-149">Secure user data</span></span>

<span data-ttu-id="c220f-150">下列各節將有建立安全的使用者資料的應用程式的主要步驟。</span><span class="sxs-lookup"><span data-stu-id="c220f-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c220f-151">您可能會發現很有幫助完成的專案參考。</span><span class="sxs-lookup"><span data-stu-id="c220f-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c220f-152">將繫結到使用者的連絡資料</span><span class="sxs-lookup"><span data-stu-id="c220f-152">Tie the contact data to the user</span></span>

<span data-ttu-id="c220f-153">使用 ASP.NET[識別](xref:security/authentication/identity)的使用者識別碼以確保使用者可以編輯其資料，但沒有其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c220f-154">新增`OwnerID`和`ContactStatus`至`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="c220f-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c220f-155">`OwnerID` 這是使用者的識別碼，從`AspNetUser`資料表中[識別](xref:security/authentication/identity)資料庫。</span><span class="sxs-lookup"><span data-stu-id="c220f-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c220f-156">`Status`欄位可讓您判斷是否為一般使用者可以檢視連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c220f-157">建立新的移轉，並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c220f-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="c220f-158">需要 HTTPS 和已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="c220f-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="c220f-159">新增[IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)至`Startup`:</span><span class="sxs-lookup"><span data-stu-id="c220f-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="c220f-160">在`ConfigureServices`方法*Startup.cs* file、 add [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="c220f-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="c220f-161">如果您使用 Visual Studio，請啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="c220f-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="c220f-162">若要將 HTTP 要求重新導向至 HTTPS，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。</span><span class="sxs-lookup"><span data-stu-id="c220f-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="c220f-163">若您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，為 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="c220f-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="c220f-164">設定`"LocalTest:skipHTTPS": true`中*appsettings。Developement.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="c220f-164">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="c220f-165">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="c220f-165">Require authenticated users</span></span>

<span data-ttu-id="c220f-166">設定為需要驗證使用者的預設驗證原則。</span><span class="sxs-lookup"><span data-stu-id="c220f-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="c220f-167">您可以選擇不使用 Razor 頁面、 控制器或動作的方法層級驗證`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="c220f-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c220f-168">設定為需要驗證使用者的預設驗證原則能保護新加入的 Razor 頁面和控制站。</span><span class="sxs-lookup"><span data-stu-id="c220f-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c220f-169">具有所需的預設驗證比上新的控制站及 Razor 頁面，以包含安全`[Authorize]`屬性。</span><span class="sxs-lookup"><span data-stu-id="c220f-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="c220f-170">與驗證，所有使用者的需求[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)和[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0)呼叫並非必要項。</span><span class="sxs-lookup"><span data-stu-id="c220f-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="c220f-171">更新`ConfigureServices`以下列變更：</span><span class="sxs-lookup"><span data-stu-id="c220f-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="c220f-172">標記為註解`AuthorizeFolder`和`AuthorizePage`。</span><span class="sxs-lookup"><span data-stu-id="c220f-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="c220f-173">設定為需要驗證使用者的預設驗證原則。</span><span class="sxs-lookup"><span data-stu-id="c220f-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="c220f-174">新增[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)索引，因此他們註冊之前，匿名使用者可以取得站台的相關資訊的相關，以及連絡頁面。</span><span class="sxs-lookup"><span data-stu-id="c220f-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="c220f-175">新增`[AllowAnonymous]`至[LoginModel 和 RegisterModel](https://github.com/aspnet/templating/issues/238)。</span><span class="sxs-lookup"><span data-stu-id="c220f-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="c220f-176">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="c220f-176">Configure the test account</span></span>

<span data-ttu-id="c220f-177">`SeedData`類別會建立兩個帳戶： 系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="c220f-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c220f-178">使用[密碼管理員工具](xref:security/app-secrets)設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="c220f-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c220f-179">從專案目錄設定密碼 (目錄包含*Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="c220f-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c220f-180">如果您未使用強式密碼，例外狀況時擲回`SeedData.Initialize`呼叫。</span><span class="sxs-lookup"><span data-stu-id="c220f-180">If you don't use a strong password, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c220f-181">更新`Main`使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="c220f-181">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c220f-182">建立測試帳戶，並更新連絡人</span><span class="sxs-lookup"><span data-stu-id="c220f-182">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c220f-183">更新`Initialize`方法中的`SeedData`類別來建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="c220f-183">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c220f-184">新增系統管理員使用者識別碼和`ContactStatus`至連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-184">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c220f-185">請的連絡人 」 已送出 」 和一個 「 已拒絕 」。</span><span class="sxs-lookup"><span data-stu-id="c220f-185">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c220f-186">加入所有連絡人的使用者識別碼和狀態。</span><span class="sxs-lookup"><span data-stu-id="c220f-186">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c220f-187">只能有一個連絡人所示：</span><span class="sxs-lookup"><span data-stu-id="c220f-187">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c220f-188">建立擁有者、 管理員和系統管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="c220f-188">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c220f-189">建立`ContactIsOwnerAuthorizationHandler`類別*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c220f-189">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c220f-190">`ContactIsOwnerAuthorizationHandler`確認作用於資源的使用者擁有的資源。</span><span class="sxs-lookup"><span data-stu-id="c220f-190">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c220f-191">`ContactIsOwnerAuthorizationHandler`呼叫[內容。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人的擁有者。</span><span class="sxs-lookup"><span data-stu-id="c220f-191">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c220f-192">授權的處理常式通常：</span><span class="sxs-lookup"><span data-stu-id="c220f-192">Authorization handlers generally:</span></span>

* <span data-ttu-id="c220f-193">傳回`context.Succeed`有符合的需求。</span><span class="sxs-lookup"><span data-stu-id="c220f-193">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c220f-194">傳回`Task.CompletedTask`時並不符合需求。</span><span class="sxs-lookup"><span data-stu-id="c220f-194">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c220f-195">`Task.CompletedTask` 都不成功或失敗&mdash;它可讓執行其他授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="c220f-195">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c220f-196">如果您需要明確地使失敗，傳回[內容。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="c220f-196">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c220f-197">應用程式可讓連絡人的擁有者可以編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-197">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c220f-198">`ContactIsOwnerAuthorizationHandler` 不需要檢查需求參數中的作業。</span><span class="sxs-lookup"><span data-stu-id="c220f-198">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c220f-199">建立授權管理員的處理常式</span><span class="sxs-lookup"><span data-stu-id="c220f-199">Create a manager authorization handler</span></span>

<span data-ttu-id="c220f-200">建立`ContactManagerAuthorizationHandler`類別*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c220f-200">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c220f-201">`ContactManagerAuthorizationHandler`確認作用於資源的使用者是管理員。</span><span class="sxs-lookup"><span data-stu-id="c220f-201">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c220f-202">只有經理可以核准或拒絕內容變更 （新增或變更）。</span><span class="sxs-lookup"><span data-stu-id="c220f-202">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c220f-203">建立的系統管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="c220f-203">Create an administrator authorization handler</span></span>

<span data-ttu-id="c220f-204">建立`ContactAdministratorsAuthorizationHandler`類別*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c220f-204">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c220f-205">`ContactAdministratorsAuthorizationHandler`確認作用於資源的使用者是系統管理員。</span><span class="sxs-lookup"><span data-stu-id="c220f-205">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c220f-206">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="c220f-206">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c220f-207">註冊授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="c220f-207">Register the authorization handlers</span></span>

<span data-ttu-id="c220f-208">使用 Entity Framework 的核心服務必須登錄[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。</span><span class="sxs-lookup"><span data-stu-id="c220f-208">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c220f-209">`ContactIsOwnerAuthorizationHandler`使用 ASP.NET Core[識別](xref:security/authentication/identity)，這建置在 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="c220f-209">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c220f-210">登錄處理常式與服務的集合，所以可`ContactsController`透過[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="c220f-210">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c220f-211">將下列程式碼加入至結尾`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c220f-211">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="c220f-212">`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`會新增為 singleton。</span><span class="sxs-lookup"><span data-stu-id="c220f-212">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c220f-213">它們是 singleton，因為它們不使用 EF 和所需的資訊位於`Context`參數`HandleRequirementAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="c220f-213">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c220f-214">支援授權</span><span class="sxs-lookup"><span data-stu-id="c220f-214">Support authorization</span></span>

<span data-ttu-id="c220f-215">在本節中，您可以更新 Razor 頁面，並將作業需求類別加入。</span><span class="sxs-lookup"><span data-stu-id="c220f-215">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c220f-216">檢閱連絡人的作業需求類別</span><span class="sxs-lookup"><span data-stu-id="c220f-216">Review the contact operations requirements class</span></span>

<span data-ttu-id="c220f-217">檢閱`ContactOperations`類別。</span><span class="sxs-lookup"><span data-stu-id="c220f-217">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c220f-218">這個類別包含的需求，應用程式支援：</span><span class="sxs-lookup"><span data-stu-id="c220f-218">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="c220f-219">建立 Razor 頁面的基底類別</span><span class="sxs-lookup"><span data-stu-id="c220f-219">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="c220f-220">建立包含連絡人 Razor 頁面中會使用服務的基底類別。</span><span class="sxs-lookup"><span data-stu-id="c220f-220">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c220f-221">基底類別會將該初始化程式碼在同一個位置：</span><span class="sxs-lookup"><span data-stu-id="c220f-221">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c220f-222">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c220f-222">The preceding code:</span></span>

* <span data-ttu-id="c220f-223">新增`IAuthorizationService`服務存取授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="c220f-223">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c220f-224">新增身分識別`UserManager`服務。</span><span class="sxs-lookup"><span data-stu-id="c220f-224">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c220f-225">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="c220f-225">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c220f-226">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="c220f-226">Update the CreateModel</span></span>

<span data-ttu-id="c220f-227">更新建立頁面模型建構函式使用`DI_BasePageModel`基底類別：</span><span class="sxs-lookup"><span data-stu-id="c220f-227">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c220f-228">更新`CreateModel.OnPostAsync`方法：</span><span class="sxs-lookup"><span data-stu-id="c220f-228">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c220f-229">新增使用者識別碼`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="c220f-229">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c220f-230">呼叫此授權處理常式，以確定使用者有權限來建立的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-230">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c220f-231">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="c220f-231">Update the IndexModel</span></span>

<span data-ttu-id="c220f-232">更新`OnGetAsync`方法，使只有核准的連絡人會向一般使用者顯示：</span><span class="sxs-lookup"><span data-stu-id="c220f-232">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c220f-233">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="c220f-233">Update the EditModel</span></span>

<span data-ttu-id="c220f-234">新增授權處理常式來驗證使用者擁有的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-234">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c220f-235">正在驗證資源授權，因為`[Authorize]`屬性不足夠。</span><span class="sxs-lookup"><span data-stu-id="c220f-235">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c220f-236">應用程式沒有存取資源，要在評估屬性時。</span><span class="sxs-lookup"><span data-stu-id="c220f-236">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c220f-237">必須命令式資源為基礎的授權。</span><span class="sxs-lookup"><span data-stu-id="c220f-237">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c220f-238">一旦應用程式在載入頁面模型或是載入在處理常式本身擁有資源的存取權，就必須執行檢查。</span><span class="sxs-lookup"><span data-stu-id="c220f-238">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c220f-239">您經常存取的資源，藉由傳遞資源索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c220f-239">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c220f-240">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c220f-240">Update the DeleteModel</span></span>

<span data-ttu-id="c220f-241">更新刪除頁面模型，以確認使用者擁有連絡人 delete 權限使用授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="c220f-241">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c220f-242">授權服務插入檢視</span><span class="sxs-lookup"><span data-stu-id="c220f-242">Inject the authorization service into the views</span></span>

<span data-ttu-id="c220f-243">目前，UI 顯示編輯和刪除使用者無法修改的資料的連結。</span><span class="sxs-lookup"><span data-stu-id="c220f-243">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="c220f-244">藉由套用授權的處理常式來檢視，UI 被固定的。</span><span class="sxs-lookup"><span data-stu-id="c220f-244">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="c220f-245">插入中的授權服務*Views/_ViewImports.cshtml*檔案，因此可使用的所有檢視：</span><span class="sxs-lookup"><span data-stu-id="c220f-245">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="c220f-246">上述標記加入了許多`using`陳述式。</span><span class="sxs-lookup"><span data-stu-id="c220f-246">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c220f-247">更新**編輯**和**刪除**中連結*Pages/Contacts/Index.cshtml*讓它們只呈現具有適當的權限的使用者：</span><span class="sxs-lookup"><span data-stu-id="c220f-247">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="c220f-248">隱藏不需要變更資料的權限的使用者連結，並不安全應用程式。</span><span class="sxs-lookup"><span data-stu-id="c220f-248">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c220f-249">隱藏連結，讓應用程式更容易使用顯示唯一有效的連結。</span><span class="sxs-lookup"><span data-stu-id="c220f-249">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c220f-250">使用者可以 hack 叫用 編輯和刪除作業沒有自己的資料產生的 Url。</span><span class="sxs-lookup"><span data-stu-id="c220f-250">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c220f-251">Razor 頁面或控制站必須強制執行存取檢查，以確保資料的安全。</span><span class="sxs-lookup"><span data-stu-id="c220f-251">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c220f-252">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="c220f-252">Update Details</span></span>

<span data-ttu-id="c220f-253">更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="c220f-253">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="c220f-254">更新詳細資料頁面模型：</span><span class="sxs-lookup"><span data-stu-id="c220f-254">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="c220f-255">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c220f-255">Test the completed app</span></span>

<span data-ttu-id="c220f-256">若您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，為 HTTPS:</span><span class="sxs-lookup"><span data-stu-id="c220f-256">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="c220f-257">設定`"LocalTest:skipHTTPS": true`中*appsettings。Developement.json*檔案以略過的 HTTPS 要求。</span><span class="sxs-lookup"><span data-stu-id="c220f-257">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="c220f-258">略過只在開發電腦上的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="c220f-258">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="c220f-259">如果應用程式的連絡人：</span><span class="sxs-lookup"><span data-stu-id="c220f-259">If the app has contacts:</span></span>

* <span data-ttu-id="c220f-260">刪除所有記錄的`Contact`資料表。</span><span class="sxs-lookup"><span data-stu-id="c220f-260">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="c220f-261">重新啟動植入資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c220f-261">Restart the app to seed the database.</span></span>

<span data-ttu-id="c220f-262">註冊使用者瀏覽的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-262">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="c220f-263">若要測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 版本）。</span><span class="sxs-lookup"><span data-stu-id="c220f-263">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="c220f-264">在一個瀏覽器中註冊新的使用者 (例如， `test@example.com`)。</span><span class="sxs-lookup"><span data-stu-id="c220f-264">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c220f-265">以不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c220f-265">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c220f-266">請確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="c220f-266">Verify the following operations:</span></span>

* <span data-ttu-id="c220f-267">已註冊的使用者可以檢視所有已核准的連絡資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-267">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="c220f-268">已註冊的使用者可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-268">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c220f-269">經理可以核准或拒絕連絡資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-269">Managers can approve or reject contact data.</span></span> <span data-ttu-id="c220f-270">`Details`檢視會顯示**核准**和**拒絕**按鈕。</span><span class="sxs-lookup"><span data-stu-id="c220f-270">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c220f-271">系統管理員可以核准/拒絕及編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-271">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="c220f-272">使用者</span><span class="sxs-lookup"><span data-stu-id="c220f-272">User</span></span>| <span data-ttu-id="c220f-273">選項</span><span class="sxs-lookup"><span data-stu-id="c220f-273">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="c220f-274">可以編輯/刪除自己的資料</span><span class="sxs-lookup"><span data-stu-id="c220f-274">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="c220f-275">可以核准/拒絕和編輯/刪除擁有資料</span><span class="sxs-lookup"><span data-stu-id="c220f-275">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="c220f-276">可以編輯/刪除並核准/拒絕的所有資料</span><span class="sxs-lookup"><span data-stu-id="c220f-276">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="c220f-277">系統管理員的瀏覽器中建立的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c220f-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c220f-278">複製的 URL 刪除和編輯從系統管理員連絡。</span><span class="sxs-lookup"><span data-stu-id="c220f-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c220f-279">這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="c220f-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c220f-280">建立起始應用程式</span><span class="sxs-lookup"><span data-stu-id="c220f-280">Create the starter app</span></span>

* <span data-ttu-id="c220f-281">建立名為"ContactManager"Razor 網頁應用程式</span><span class="sxs-lookup"><span data-stu-id="c220f-281">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="c220f-282">建立應用程式與**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="c220f-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c220f-283">將類別命名"ContactManager 」 讓您的命名空間符合範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="c220f-283">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * <span data-ttu-id="c220f-285">`-uld` 指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="c220f-285">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="c220f-286">加入下列`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="c220f-286">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c220f-287">Scaffold`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="c220f-287">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="c220f-288">更新**ContactManager**錨定在*Pages/_Layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="c220f-288">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="c220f-289">為初始移轉建立結構，並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c220f-289">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="c220f-290">測試應用程式所建立、 編輯和刪除連絡人</span><span class="sxs-lookup"><span data-stu-id="c220f-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c220f-291">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="c220f-291">Seed the database</span></span>

<span data-ttu-id="c220f-292">新增`SeedData`類別*資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c220f-292">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="c220f-293">如果您已經下載範例，您可以複製*SeedData.cs*檔案*資料*入門專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c220f-293">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="c220f-294">呼叫`SeedData.Initialize`從`Main`:</span><span class="sxs-lookup"><span data-stu-id="c220f-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="c220f-295">測試應用程式植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c220f-295">Test that the app seeded the database.</span></span> <span data-ttu-id="c220f-296">請連絡資料庫中有任何資料列，如果種子不執行方法。</span><span class="sxs-lookup"><span data-stu-id="c220f-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="c220f-297">其他資源</span><span class="sxs-lookup"><span data-stu-id="c220f-297">Additional resources</span></span>

* <span data-ttu-id="c220f-298">[ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。</span><span class="sxs-lookup"><span data-stu-id="c220f-298">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="c220f-299">這個實驗室會進入這個教學課程中介紹的安全性功能的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c220f-299">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="c220f-300">在 ASP.NET Core 授權： 簡單、 宣告式和自訂的角色</span><span class="sxs-lookup"><span data-stu-id="c220f-300">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="c220f-301">自訂原則式授權</span><span class="sxs-lookup"><span data-stu-id="c220f-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
