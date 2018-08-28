---
title: 建立 ASP.NET Core 應用程式與受保護的授權的使用者資料
author: rick-anderson
description: 了解如何使用受保護的授權的使用者資料建立 Razor 頁面應用程式。 包含 HTTPS、 驗證、 安全性、 ASP.NET Core 身分識別。
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: ba59e8d6243965188397c4ba7a130eec42acfb91
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055876"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="c2147-104">請參閱[此 PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 版本。</span><span class="sxs-lookup"><span data-stu-id="c2147-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="c2147-105">本教學課程中的 ASP.NET Core 1.1 版本處於[這](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data)資料夾。</span><span class="sxs-lookup"><span data-stu-id="c2147-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="c2147-106">範例 ASP.NET Core 1.1[範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)。</span><span class="sxs-lookup"><span data-stu-id="c2147-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2147-107">請參閱 [此 pdf] (https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="c2147-107">See the [this pdf] (https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="c2147-108">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="c2147-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="c2147-109">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c2147-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="c2147-110">本教學課程會示範如何建立 ASP.NET Core web 應用程式與受保護的授權的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c2147-111">它會顯示已驗證 （登錄） 的使用者的連絡人清單中已建立。</span><span class="sxs-lookup"><span data-stu-id="c2147-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c2147-112">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="c2147-112">There are three security groups:</span></span>

* <span data-ttu-id="c2147-113">**已註冊的使用者**可以檢視所有已認可的資料並可以編輯/刪除他們自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c2147-114">**管理員**可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c2147-115">只有已核准的連絡人會對使用者顯示。</span><span class="sxs-lookup"><span data-stu-id="c2147-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c2147-116">**系統管理員**可以核准/拒絕並編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c2147-117">在下圖中，使用者 Rick (`rick@example.com`) 登入。</span><span class="sxs-lookup"><span data-stu-id="c2147-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c2147-118">Rick 只能檢視已核准的連絡人和**編輯**/**刪除**/**新建**他連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="c2147-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c2147-119">只有最後一筆記錄，由 Rick，顯示**編輯**並**刪除**連結。</span><span class="sxs-lookup"><span data-stu-id="c2147-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c2147-120">其他使用者不會看到最後一筆記錄，直到管理員或系統管理員的狀態變更為 「 Approved 」。</span><span class="sxs-lookup"><span data-stu-id="c2147-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![前面所述的映像](secure-data/_static/rick.png)

<span data-ttu-id="c2147-122">在下圖中，`manager@contoso.com`登入，並在管理員的角色：</span><span class="sxs-lookup"><span data-stu-id="c2147-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![前面所述的映像](secure-data/_static/manager1.png)

<span data-ttu-id="c2147-124">下圖顯示之管理員的連絡詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="c2147-124">The following image shows the managers details view of a contact:</span></span>

![前面所述的映像](secure-data/_static/manager.png)

<span data-ttu-id="c2147-126">**核准**並**拒絕**按鈕只會顯示為管理員和系統管理員。</span><span class="sxs-lookup"><span data-stu-id="c2147-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c2147-127">在下圖中，`admin@contoso.com`登入，並在系統管理員角色：</span><span class="sxs-lookup"><span data-stu-id="c2147-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![前面所述的映像](secure-data/_static/admin.png)

<span data-ttu-id="c2147-129">系統管理員將擁有所有權限。</span><span class="sxs-lookup"><span data-stu-id="c2147-129">The administrator has all privileges.</span></span> <span data-ttu-id="c2147-130">她可以讀取/編輯/刪除的任何連絡人，並變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="c2147-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c2147-131">藉由建立應用程式[scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)下列`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="c2147-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="c2147-132">此範例包含下列 「 授權 」 處理常式：</span><span class="sxs-lookup"><span data-stu-id="c2147-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c2147-133">`ContactIsOwnerAuthorizationHandler`： 可確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c2147-134">`ContactManagerAuthorizationHandler`： 允許管理員核准或拒絕連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c2147-135">`ContactAdministratorsAuthorizationHandler`： 可讓系統管理員核准或拒絕連絡人，以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2147-136">必要條件</span><span class="sxs-lookup"><span data-stu-id="c2147-136">Prerequisites</span></span>

<span data-ttu-id="c2147-137">本教學課程會前進。</span><span class="sxs-lookup"><span data-stu-id="c2147-137">This tutorial is advanced.</span></span> <span data-ttu-id="c2147-138">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="c2147-138">You should be familiar with:</span></span>

* [<span data-ttu-id="c2147-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2147-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c2147-140">驗證</span><span class="sxs-lookup"><span data-stu-id="c2147-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="c2147-141">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="c2147-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c2147-142">授權</span><span class="sxs-lookup"><span data-stu-id="c2147-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="c2147-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c2147-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="c2147-144">下載程式碼與本教學課程需要 ASP.NET Core 2.2 preview 1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="c2147-144">The download code as for this tutorial requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="c2147-145">請參閱[此 GitHub 問題](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909)的因應措施。</span><span class="sxs-lookup"><span data-stu-id="c2147-145">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c2147-146">Starter 和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c2147-146">The starter and completed app</span></span>

<span data-ttu-id="c2147-147">[下載](xref:tutorials/index#how-to-download-a-sample)[完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)應用程式。</span><span class="sxs-lookup"><span data-stu-id="c2147-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="c2147-148">[測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="c2147-148">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c2147-149">入門應用程式</span><span class="sxs-lookup"><span data-stu-id="c2147-149">The starter app</span></span>

<span data-ttu-id="c2147-150">[下載](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2)應用程式。</span><span class="sxs-lookup"><span data-stu-id="c2147-150">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="c2147-151">執行應用程式中，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-151">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c2147-152">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="c2147-152">Secure user data</span></span>

<span data-ttu-id="c2147-153">下列各節會有所有主要的步驟，來建立安全的使用者資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c2147-153">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c2147-154">您可能會發現它已完成的專案參考很有幫助。</span><span class="sxs-lookup"><span data-stu-id="c2147-154">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c2147-155">將繫結至使用者的連絡人資料</span><span class="sxs-lookup"><span data-stu-id="c2147-155">Tie the contact data to the user</span></span>

<span data-ttu-id="c2147-156">使用 ASP.NET[識別](xref:security/authentication/identity)使用者識別碼，以確保使用者可以編輯其資料，但不是其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-156">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c2147-157">新增`OwnerID`並`ContactStatus`到`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="c2147-157">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c2147-158">`OwnerID` 是來自使用者的識別碼`AspNetUser`資料表中[身分識別](xref:security/authentication/identity)資料庫。</span><span class="sxs-lookup"><span data-stu-id="c2147-158">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c2147-159">`Status`欄位可讓您判斷是否可供一般使用者檢視連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-159">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c2147-160">建立新的移轉，並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c2147-160">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c2147-161">將角色服務新增到 身分識別</span><span class="sxs-lookup"><span data-stu-id="c2147-161">Add Role services to Identity</span></span>

<span data-ttu-id="c2147-162">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)來新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="c2147-162">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c2147-163">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="c2147-163">Require authenticated users</span></span>

<span data-ttu-id="c2147-164">設定預設的驗證原則，以要求使用者進行驗證：</span><span class="sxs-lookup"><span data-stu-id="c2147-164">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="c2147-165">您可以選擇退出的 Razor 頁面、 控制器或動作方法層級的驗證`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="c2147-165">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c2147-166">設定預設的驗證原則，以要求使用者進行驗證，可保護新加入的 Razor 頁面和控制站。</span><span class="sxs-lookup"><span data-stu-id="c2147-166">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c2147-167">具有預設值所需的驗證會比依賴新的控制器和 Razor 頁面，以包含更安全`[Authorize]`屬性。</span><span class="sxs-lookup"><span data-stu-id="c2147-167">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c2147-168">新增[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)索引，因此匿名使用者可以取得站台的相關資訊，才能註冊的相關和連絡人頁面。</span><span class="sxs-lookup"><span data-stu-id="c2147-168">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c2147-169">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="c2147-169">Configure the test account</span></span>

<span data-ttu-id="c2147-170">`SeedData`類別會建立兩個帳戶： 系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="c2147-170">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c2147-171">使用[Secret Manager 工具](xref:security/app-secrets)設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="c2147-171">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c2147-172">設定密碼，從專案目錄 (目錄包含*Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="c2147-172">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c2147-173">如果未指定強式密碼，則會擲回例外狀況時`SeedData.Initialize`呼叫。</span><span class="sxs-lookup"><span data-stu-id="c2147-173">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c2147-174">更新`Main`使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="c2147-174">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c2147-175">建立測試帳戶和更新連絡人</span><span class="sxs-lookup"><span data-stu-id="c2147-175">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c2147-176">更新`Initialize`方法中的`SeedData`類別來建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="c2147-176">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c2147-177">新增系統管理員使用者識別碼和`ContactStatus`連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-177">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c2147-178">讓其中一個 「 連絡人 」 已提交 」 和一個 「 已拒絕 」。</span><span class="sxs-lookup"><span data-stu-id="c2147-178">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c2147-179">加入所有連絡人的使用者識別碼和狀態。</span><span class="sxs-lookup"><span data-stu-id="c2147-179">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c2147-180">只有一個連絡人所示：</span><span class="sxs-lookup"><span data-stu-id="c2147-180">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c2147-181">建立擁有者 」、 「 管理員 」 和 「 系統管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="c2147-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c2147-182">建立`ContactIsOwnerAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c2147-182">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c2147-183">`ContactIsOwnerAuthorizationHandler`確認處理資源的使用者擁有的資源。</span><span class="sxs-lookup"><span data-stu-id="c2147-183">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c2147-184">`ContactIsOwnerAuthorizationHandler`呼叫[內容。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)目前已驗證的使用者是否連絡擁有者。</span><span class="sxs-lookup"><span data-stu-id="c2147-184">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c2147-185">授權的處理常式通常：</span><span class="sxs-lookup"><span data-stu-id="c2147-185">Authorization handlers generally:</span></span>

* <span data-ttu-id="c2147-186">傳回`context.Succeed`時符合的需求。</span><span class="sxs-lookup"><span data-stu-id="c2147-186">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c2147-187">傳回`Task.CompletedTask`時不符合需求。</span><span class="sxs-lookup"><span data-stu-id="c2147-187">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c2147-188">`Task.CompletedTask` 未成功或失敗&mdash;它允許執行的其他授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="c2147-188">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c2147-189">如果您需要明確地使失敗，傳回[內容。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="c2147-189">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c2147-190">此應用程式會允許連絡人的擁有者可以編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-190">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c2147-191">`ContactIsOwnerAuthorizationHandler` 不需要檢查傳入的要求參數的作業。</span><span class="sxs-lookup"><span data-stu-id="c2147-191">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c2147-192">建立管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="c2147-192">Create a manager authorization handler</span></span>

<span data-ttu-id="c2147-193">建立`ContactManagerAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c2147-193">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c2147-194">`ContactManagerAuthorizationHandler`確認處理資源的使用者是管理員。</span><span class="sxs-lookup"><span data-stu-id="c2147-194">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c2147-195">只有經理可以核准或拒絕內容變更 （新增或變更）。</span><span class="sxs-lookup"><span data-stu-id="c2147-195">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c2147-196">建立系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c2147-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="c2147-197">建立`ContactAdministratorsAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c2147-197">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c2147-198">`ContactAdministratorsAuthorizationHandler`確認處理資源的使用者是系統管理員。</span><span class="sxs-lookup"><span data-stu-id="c2147-198">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c2147-199">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="c2147-199">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c2147-200">註冊授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c2147-200">Register the authorization handlers</span></span>

<span data-ttu-id="c2147-201">您必須針對註冊服務使用 Entity Framework Core[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。</span><span class="sxs-lookup"><span data-stu-id="c2147-201">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c2147-202">`ContactIsOwnerAuthorizationHandler`會使用 ASP.NET Core[身分識別](xref:security/authentication/identity)，這根據 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="c2147-202">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c2147-203">註冊處理常式的服務集合，讓它們能夠`ContactsController`經由[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="c2147-203">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c2147-204">將下列程式碼新增至結尾`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c2147-204">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="c2147-205">`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`會新增為單一子句。</span><span class="sxs-lookup"><span data-stu-id="c2147-205">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c2147-206">它們是單次個體，因為他們不使用 EF 和所需的所有資訊都都`Context`參數`HandleRequirementAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="c2147-206">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c2147-207">支援的授權</span><span class="sxs-lookup"><span data-stu-id="c2147-207">Support authorization</span></span>

<span data-ttu-id="c2147-208">在本節中，您可以更新 Razor 頁面，並新增的運算需求類別。</span><span class="sxs-lookup"><span data-stu-id="c2147-208">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c2147-209">檢閱此連絡人的作業需求類別</span><span class="sxs-lookup"><span data-stu-id="c2147-209">Review the contact operations requirements class</span></span>

<span data-ttu-id="c2147-210">檢閱`ContactOperations`類別。</span><span class="sxs-lookup"><span data-stu-id="c2147-210">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c2147-211">這個類別包含的需求，應用程式支援：</span><span class="sxs-lookup"><span data-stu-id="c2147-211">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c2147-212">建立連絡人 Razor 頁面的基底類別</span><span class="sxs-lookup"><span data-stu-id="c2147-212">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c2147-213">建立基底類別，其中包含連絡人 Razor 頁面中使用的服務。</span><span class="sxs-lookup"><span data-stu-id="c2147-213">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c2147-214">基底類別會將初始化程式碼置於一個位置：</span><span class="sxs-lookup"><span data-stu-id="c2147-214">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c2147-215">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c2147-215">The preceding code:</span></span>

* <span data-ttu-id="c2147-216">新增`IAuthorizationService`服務存取授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="c2147-216">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c2147-217">將身分識別新增`UserManager`服務。</span><span class="sxs-lookup"><span data-stu-id="c2147-217">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c2147-218">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="c2147-218">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c2147-219">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="c2147-219">Update the CreateModel</span></span>

<span data-ttu-id="c2147-220">更新 [建立] 頁面模型建構函式使用`DI_BasePageModel`基底類別：</span><span class="sxs-lookup"><span data-stu-id="c2147-220">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c2147-221">更新`CreateModel.OnPostAsync`方法：</span><span class="sxs-lookup"><span data-stu-id="c2147-221">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c2147-222">新增的使用者識別碼`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="c2147-222">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c2147-223">呼叫授權處理常式，以確定使用者有權限來建立連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-223">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c2147-224">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="c2147-224">Update the IndexModel</span></span>

<span data-ttu-id="c2147-225">更新`OnGetAsync`方法，讓只有核准的連絡人會向一般使用者顯示：</span><span class="sxs-lookup"><span data-stu-id="c2147-225">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c2147-226">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="c2147-226">Update the EditModel</span></span>

<span data-ttu-id="c2147-227">新增授權處理常式，以確認該使用者所擁有的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-227">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c2147-228">正在驗證資源的授權，因為`[Authorize]`屬性還不夠。</span><span class="sxs-lookup"><span data-stu-id="c2147-228">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c2147-229">要在評估屬性時，應用程式就不需要資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="c2147-229">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c2147-230">資源為基礎的授權必須是必要的。</span><span class="sxs-lookup"><span data-stu-id="c2147-230">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c2147-231">一旦應用程式存取的資源中，載入頁面模型中，或載入處理常式本身內，則必須執行檢查。</span><span class="sxs-lookup"><span data-stu-id="c2147-231">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c2147-232">您經常存取的資源，藉由傳入的資源索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c2147-232">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c2147-233">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c2147-233">Update the DeleteModel</span></span>

<span data-ttu-id="c2147-234">更新使用授權處理常式，以確定使用者有刪除權限，在連絡人上的 [刪除] 頁面模型。</span><span class="sxs-lookup"><span data-stu-id="c2147-234">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c2147-235">插入檢視中的授權服務</span><span class="sxs-lookup"><span data-stu-id="c2147-235">Inject the authorization service into the views</span></span>

<span data-ttu-id="c2147-236">目前，UI 顯示編輯和刪除的使用者無法修改的連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="c2147-236">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c2147-237">插入中的授權服務*views/_viewimports.cshtml*檔案，以便它可供所有檢視：</span><span class="sxs-lookup"><span data-stu-id="c2147-237">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c2147-238">上述標記會新增數個`using`陳述式。</span><span class="sxs-lookup"><span data-stu-id="c2147-238">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c2147-239">更新**編輯**並**刪除**中的連結*Pages/Contacts/Index.cshtml*讓它們只轉譯為適當的權限的使用者：</span><span class="sxs-lookup"><span data-stu-id="c2147-239">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c2147-240">隱藏不需要變更資料的權限的使用者的連結不安全的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c2147-240">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c2147-241">隱藏的連結，讓應用程式更方便使用顯示唯一有效的連結。</span><span class="sxs-lookup"><span data-stu-id="c2147-241">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c2147-242">使用者可以 hack 產生的 Url 以叫用 編輯和刪除作業對他們未擁有的資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-242">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c2147-243">Razor 頁面或控制站必須強制執行存取檢查，以確保資料的安全。</span><span class="sxs-lookup"><span data-stu-id="c2147-243">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c2147-244">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="c2147-244">Update Details</span></span>

<span data-ttu-id="c2147-245">更新詳細資料檢視，讓經理可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="c2147-245">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c2147-246">更新詳細資料 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="c2147-246">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="c2147-247">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c2147-247">Test the completed app</span></span>

<span data-ttu-id="c2147-248">如果應用程式的連絡人：</span><span class="sxs-lookup"><span data-stu-id="c2147-248">If the app has contacts:</span></span>

* <span data-ttu-id="c2147-249">刪除中的所有記錄`Contact`資料表。</span><span class="sxs-lookup"><span data-stu-id="c2147-249">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="c2147-250">重新啟動植入資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c2147-250">Restart the app to seed the database.</span></span>

<span data-ttu-id="c2147-251">註冊使用者瀏覽的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-251">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="c2147-252">測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 版本）。</span><span class="sxs-lookup"><span data-stu-id="c2147-252">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="c2147-253">在瀏覽器中註冊新的使用者 (例如`test@example.com`)。</span><span class="sxs-lookup"><span data-stu-id="c2147-253">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c2147-254">使用不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c2147-254">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c2147-255">請確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="c2147-255">Verify the following operations:</span></span>

* <span data-ttu-id="c2147-256">已註冊的使用者可以檢視所有已核准的連絡資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-256">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="c2147-257">已註冊的使用者可以編輯/刪除他們自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-257">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c2147-258">經理可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-258">Managers can approve or reject contact data.</span></span> <span data-ttu-id="c2147-259">`Details`檢視會顯示**核准**並**拒絕**按鈕。</span><span class="sxs-lookup"><span data-stu-id="c2147-259">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c2147-260">系統管理員可以核准/拒絕和編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-260">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="c2147-261">使用者</span><span class="sxs-lookup"><span data-stu-id="c2147-261">User</span></span>| <span data-ttu-id="c2147-262">選項</span><span class="sxs-lookup"><span data-stu-id="c2147-262">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="c2147-263">可以編輯/刪除自己的資料</span><span class="sxs-lookup"><span data-stu-id="c2147-263">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="c2147-264">可以核准/拒絕和編輯/刪除擁有資料</span><span class="sxs-lookup"><span data-stu-id="c2147-264">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="c2147-265">可以編輯/刪除和核准/拒絕的所有資料</span><span class="sxs-lookup"><span data-stu-id="c2147-265">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="c2147-266">在系統管理員的瀏覽器中建立的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c2147-266">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c2147-267">複製的 URL 刪除和編輯從系統管理員連絡。</span><span class="sxs-lookup"><span data-stu-id="c2147-267">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c2147-268">這些連結貼入測試使用者的瀏覽器，以確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="c2147-268">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c2147-269">建立入門應用程式</span><span class="sxs-lookup"><span data-stu-id="c2147-269">Create the starter app</span></span>

* <span data-ttu-id="c2147-270">建立名為"ContactManager"Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="c2147-270">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="c2147-271">建立應用程式與**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="c2147-271">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="c2147-272">提供名稱"ContactManager"使命名空間符合此範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="c2147-272">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="c2147-273">`-uld` 指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="c2147-273">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c2147-274">新增*Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2147-274">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c2147-275">Scaffold`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="c2147-275">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c2147-276">建立初始移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c2147-276">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="c2147-277">更新**ContactManager**中的錨點*pages/_layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="c2147-277">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="c2147-278">測試應用程式建立、 編輯和刪除連絡人</span><span class="sxs-lookup"><span data-stu-id="c2147-278">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c2147-279">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="c2147-279">Seed the database</span></span>

<span data-ttu-id="c2147-280">新增[SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs)類別，即可*資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c2147-280">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="c2147-281">呼叫`SeedData.Initialize`從`Main`:</span><span class="sxs-lookup"><span data-stu-id="c2147-281">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="c2147-282">測試應用程式植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c2147-282">Test that the app seeded the database.</span></span> <span data-ttu-id="c2147-283">請連絡資料庫中有任何資料列，如果種子方法不會執行。</span><span class="sxs-lookup"><span data-stu-id="c2147-283">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="c2147-284">其他資源</span><span class="sxs-lookup"><span data-stu-id="c2147-284">Additional resources</span></span>

* <span data-ttu-id="c2147-285">[ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。</span><span class="sxs-lookup"><span data-stu-id="c2147-285">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="c2147-286">這個實驗室會進入此教學課程中介紹的安全性功能的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c2147-286">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="c2147-287">ASP.NET Core 的授權： 簡單、 宣告式和自訂的角色</span><span class="sxs-lookup"><span data-stu-id="c2147-287">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="c2147-288">自訂原則式授權</span><span class="sxs-lookup"><span data-stu-id="c2147-288">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end