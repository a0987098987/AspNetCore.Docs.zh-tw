---
title: 建立 ASP.NET Core 應用程式與受保護的授權的使用者資料
author: rick-anderson
description: 了解如何使用受保護的授權的使用者資料建立 Razor 頁面應用程式。 包含 HTTPS、 驗證、 安全性、 ASP.NET Core 身分識別。
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 225d0e3aa51745253d03e614b1c8568b3a6ba731
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017488"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="d2c2f-104">建立 ASP.NET Core 應用程式與受保護的授權的使用者資料</span><span class="sxs-lookup"><span data-stu-id="d2c2f-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="d2c2f-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d2c2f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="d2c2f-106">請參閱[此 PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 版本。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="d2c2f-107">本教學課程中的 ASP.NET Core 1.1 版本處於[這](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data)資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="d2c2f-108">範例 ASP.NET Core 1.1[範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d2c2f-109">請參閱[此 pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="d2c2f-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d2c2f-110">本教學課程會示範如何建立 ASP.NET Core web 應用程式與受保護的授權的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="d2c2f-111">它會顯示已驗證 （登錄） 的使用者的連絡人清單中已建立。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="d2c2f-112">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-112">There are three security groups:</span></span>

* <span data-ttu-id="d2c2f-113">**已註冊的使用者**可以檢視所有已認可的資料並可以編輯/刪除他們自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="d2c2f-114">**管理員**可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="d2c2f-115">只有已核准的連絡人會對使用者顯示。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="d2c2f-116">**系統管理員**可以核准/拒絕並編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="d2c2f-117">本檔中的影像不完全符合最新的範本。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="d2c2f-118">在下圖中，使用者 Rick (`rick@example.com`) 登入。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="d2c2f-119">Rick 只能檢視已核准的連絡人和**編輯**/**刪除**/**新建**他連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="d2c2f-120">只有最後一筆記錄，由 Rick，顯示**編輯**並**刪除**連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="d2c2f-121">其他使用者不會看到最後一筆記錄，直到管理員或系統管理員的狀態變更為 「 Approved 」。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![螢幕擷取畫面顯示 Rick 登入](secure-data/_static/rick.png)

<span data-ttu-id="d2c2f-123">在下圖中, `manager@contoso.com`已登入並以管理員的角色執行:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![螢幕擷取畫面顯示manager@contoso.com登入](secure-data/_static/manager1.png)

<span data-ttu-id="d2c2f-125">下圖顯示之管理員的連絡詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-125">The following image shows the managers details view of a contact:</span></span>

![連絡人的管理員的檢視](secure-data/_static/manager.png)

<span data-ttu-id="d2c2f-127">**核准**並**拒絕**按鈕只會顯示為管理員和系統管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="d2c2f-128">在下圖中, `admin@contoso.com`已登入, 並以系統管理員角色:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![螢幕擷取畫面顯示admin@contoso.com登入](secure-data/_static/admin.png)

<span data-ttu-id="d2c2f-130">系統管理員將擁有所有權限。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-130">The administrator has all privileges.</span></span> <span data-ttu-id="d2c2f-131">她可以讀取/編輯/刪除的任何連絡人，並變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="d2c2f-132">藉由建立應用程式[scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)下列`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="d2c2f-133">此範例包含下列 「 授權 」 處理常式：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="d2c2f-134">`ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="d2c2f-135">`ContactManagerAuthorizationHandler`：可讓管理員核准或拒絕連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="d2c2f-136">`ContactAdministratorsAuthorizationHandler`：允許系統管理員核准或拒絕連絡人, 以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2c2f-137">必要條件</span><span class="sxs-lookup"><span data-stu-id="d2c2f-137">Prerequisites</span></span>

<span data-ttu-id="d2c2f-138">本教學課程會前進。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-138">This tutorial is advanced.</span></span> <span data-ttu-id="d2c2f-139">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-139">You should be familiar with:</span></span>

* [<span data-ttu-id="d2c2f-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2c2f-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="d2c2f-141">驗證</span><span class="sxs-lookup"><span data-stu-id="d2c2f-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d2c2f-142">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="d2c2f-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d2c2f-143">授權</span><span class="sxs-lookup"><span data-stu-id="d2c2f-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="d2c2f-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d2c2f-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="d2c2f-145">Starter 和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-145">The starter and completed app</span></span>

<span data-ttu-id="d2c2f-146">[下載](xref:index#how-to-download-a-sample)[完成](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="d2c2f-147">[測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="d2c2f-148">入門應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-148">The starter app</span></span>

<span data-ttu-id="d2c2f-149">[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="d2c2f-150">執行應用程式中，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="d2c2f-151">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="d2c2f-151">Secure user data</span></span>

<span data-ttu-id="d2c2f-152">下列各節會有所有主要的步驟，來建立安全的使用者資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="d2c2f-153">您可能會發現它已完成的專案參考很有幫助。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="d2c2f-154">將繫結至使用者的連絡人資料</span><span class="sxs-lookup"><span data-stu-id="d2c2f-154">Tie the contact data to the user</span></span>

<span data-ttu-id="d2c2f-155">使用 ASP.NET[識別](xref:security/authentication/identity)使用者識別碼，以確保使用者可以編輯其資料，但不是其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="d2c2f-156">新增`OwnerID`並`ContactStatus`到`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="d2c2f-157">`OwnerID` 是來自使用者的識別碼`AspNetUser`資料表中[身分識別](xref:security/authentication/identity)資料庫。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="d2c2f-158">`Status`欄位可讓您判斷是否可供一般使用者檢視連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="d2c2f-159">建立新的移轉，並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="d2c2f-160">將角色服務新增到 身分識別</span><span class="sxs-lookup"><span data-stu-id="d2c2f-160">Add Role services to Identity</span></span>

<span data-ttu-id="d2c2f-161">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)來新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="d2c2f-162">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="d2c2f-162">Require authenticated users</span></span>

<span data-ttu-id="d2c2f-163">設定預設的驗證原則，以要求使用者進行驗證：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="d2c2f-164">您可以選擇退出的 Razor 頁面、 控制器或動作方法層級的驗證`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="d2c2f-165">設定預設的驗證原則，以要求使用者進行驗證，可保護新加入的 Razor 頁面和控制站。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="d2c2f-166">具有預設值所需的驗證會比依賴新的控制器和 Razor 頁面，以包含更安全`[Authorize]`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="d2c2f-167">將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 [索引] 和 [隱私權] 頁面, 讓匿名使用者可以在註冊之前取得網站的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="d2c2f-168">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="d2c2f-168">Configure the test account</span></span>

<span data-ttu-id="d2c2f-169">`SeedData`類別會建立兩個帳戶： 系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="d2c2f-170">使用[Secret Manager 工具](xref:security/app-secrets)設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="d2c2f-171">設定密碼，從專案目錄 (目錄包含*Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="d2c2f-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="d2c2f-172">如果未指定強式密碼，則會擲回例外狀況時`SeedData.Initialize`呼叫。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="d2c2f-173">更新`Main`使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="d2c2f-174">建立測試帳戶和更新連絡人</span><span class="sxs-lookup"><span data-stu-id="d2c2f-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="d2c2f-175">更新`Initialize`方法中的`SeedData`類別來建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="d2c2f-176">新增系統管理員使用者識別碼和`ContactStatus`連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="d2c2f-177">讓其中一個 「 連絡人 」 已提交 」 和一個 「 已拒絕 」。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="d2c2f-178">加入所有連絡人的使用者識別碼和狀態。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="d2c2f-179">只有一個連絡人所示：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="d2c2f-180">建立擁有者 」、 「 管理員 」 和 「 系統管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="d2c2f-181">建立`ContactIsOwnerAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d2c2f-182">`ContactIsOwnerAuthorizationHandler`確認處理資源的使用者擁有的資源。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="d2c2f-183">`ContactIsOwnerAuthorizationHandler`呼叫[內容。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)目前已驗證的使用者是否連絡擁有者。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="d2c2f-184">授權的處理常式通常：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="d2c2f-185">傳回`context.Succeed`時符合的需求。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="d2c2f-186">傳回`Task.CompletedTask`時不符合需求。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="d2c2f-187">`Task.CompletedTask`不是成功或失敗&mdash;, 它允許其他授權處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="d2c2f-188">如果您需要明確地使失敗，傳回[內容。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="d2c2f-189">此應用程式會允許連絡人的擁有者可以編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="d2c2f-190">`ContactIsOwnerAuthorizationHandler` 不需要檢查傳入的要求參數的作業。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="d2c2f-191">建立管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-191">Create a manager authorization handler</span></span>

<span data-ttu-id="d2c2f-192">建立`ContactManagerAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d2c2f-193">`ContactManagerAuthorizationHandler`確認處理資源的使用者是管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="d2c2f-194">只有經理可以核准或拒絕內容變更 （新增或變更）。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="d2c2f-195">建立系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="d2c2f-196">建立`ContactAdministratorsAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d2c2f-197">`ContactAdministratorsAuthorizationHandler`確認處理資源的使用者是系統管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="d2c2f-198">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="d2c2f-199">註冊授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-199">Register the authorization handlers</span></span>

<span data-ttu-id="d2c2f-200">您必須針對註冊服務使用 Entity Framework Core[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="d2c2f-201">`ContactIsOwnerAuthorizationHandler`會使用 ASP.NET Core[身分識別](xref:security/authentication/identity)，這根據 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="d2c2f-202">註冊處理常式的服務集合，讓它們能夠`ContactsController`經由[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d2c2f-203">將下列程式碼新增至結尾`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="d2c2f-204">`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`會新增為單一子句。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="d2c2f-205">它們是單次個體，因為他們不使用 EF 和所需的所有資訊都都`Context`參數`HandleRequirementAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="d2c2f-206">支援的授權</span><span class="sxs-lookup"><span data-stu-id="d2c2f-206">Support authorization</span></span>

<span data-ttu-id="d2c2f-207">在本節中，您可以更新 Razor 頁面，並新增的運算需求類別。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="d2c2f-208">檢閱此連絡人的作業需求類別</span><span class="sxs-lookup"><span data-stu-id="d2c2f-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="d2c2f-209">檢閱`ContactOperations`類別。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="d2c2f-210">這個類別包含的需求，應用程式支援：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="d2c2f-211">建立連絡人 Razor 頁面的基底類別</span><span class="sxs-lookup"><span data-stu-id="d2c2f-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="d2c2f-212">建立基底類別，其中包含連絡人 Razor 頁面中使用的服務。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="d2c2f-213">基底類別會將初始化程式碼置於一個位置：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="d2c2f-214">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-214">The preceding code:</span></span>

* <span data-ttu-id="d2c2f-215">新增`IAuthorizationService`服務存取授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="d2c2f-216">將身分識別新增`UserManager`服務。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="d2c2f-217">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="d2c2f-218">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-218">Update the CreateModel</span></span>

<span data-ttu-id="d2c2f-219">更新 [建立] 頁面模型建構函式使用`DI_BasePageModel`基底類別：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="d2c2f-220">更新`CreateModel.OnPostAsync`方法：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="d2c2f-221">新增的使用者識別碼`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="d2c2f-222">呼叫授權處理常式，以確定使用者有權限來建立連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="d2c2f-223">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-223">Update the IndexModel</span></span>

<span data-ttu-id="d2c2f-224">更新`OnGetAsync`方法，讓只有核准的連絡人會向一般使用者顯示：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="d2c2f-225">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-225">Update the EditModel</span></span>

<span data-ttu-id="d2c2f-226">新增授權處理常式，以確認該使用者所擁有的連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="d2c2f-227">正在驗證資源的授權，因為`[Authorize]`屬性還不夠。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="d2c2f-228">要在評估屬性時，應用程式就不需要資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="d2c2f-229">資源為基礎的授權必須是必要的。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="d2c2f-230">一旦應用程式存取的資源中，載入頁面模型中，或載入處理常式本身內，則必須執行檢查。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="d2c2f-231">您經常存取的資源，藉由傳入的資源索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="d2c2f-232">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-232">Update the DeleteModel</span></span>

<span data-ttu-id="d2c2f-233">更新使用授權處理常式，以確定使用者有刪除權限，在連絡人上的 [刪除] 頁面模型。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="d2c2f-234">插入檢視中的授權服務</span><span class="sxs-lookup"><span data-stu-id="d2c2f-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="d2c2f-235">目前，UI 顯示編輯和刪除的使用者無法修改的連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="d2c2f-236">將授權服務插入*Pages/_ViewImports*檔案中, 以供所有視圖使用:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="d2c2f-237">上述標記會新增數個`using`陳述式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="d2c2f-238">更新**編輯**並**刪除**中的連結*Pages/Contacts/Index.cshtml*讓它們只轉譯為適當的權限的使用者：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="d2c2f-239">隱藏不需要變更資料的權限的使用者的連結不安全的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="d2c2f-240">隱藏的連結，讓應用程式更方便使用顯示唯一有效的連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="d2c2f-241">使用者可以 hack 產生的 Url 以叫用 編輯和刪除作業對他們未擁有的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="d2c2f-242">Razor 頁面或控制站必須強制執行存取檢查，以確保資料的安全。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="d2c2f-243">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="d2c2f-243">Update Details</span></span>

<span data-ttu-id="d2c2f-244">更新詳細資料檢視，讓經理可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="d2c2f-245">更新詳細資料 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="d2c2f-246">新增或移除使用者角色</span><span class="sxs-lookup"><span data-stu-id="d2c2f-246">Add or remove a user to a role</span></span>

<span data-ttu-id="d2c2f-247">請參閱[本期](https://github.com/aspnet/AspNetCore.Docs/issues/8502)有關：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="d2c2f-248">移除使用者的權限。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-248">Removing privileges from a user.</span></span> <span data-ttu-id="d2c2f-249">例如, 將聊天應用程式中的使用者靜音。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="d2c2f-250">若要新增權限。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="d2c2f-251">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-251">Test the completed app</span></span>

<span data-ttu-id="d2c2f-252">如果您尚未設定植入的使用者帳戶的密碼，使用[Secret Manager 工具](xref:security/app-secrets#secret-manager)設定密碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="d2c2f-253">選擇強式密碼:請使用八個或更多個字元, 且至少要有一個大寫字元、數位和符號。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="d2c2f-254">比方說，`Passw0rd!`符合強式密碼需求。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="d2c2f-255">執行下列命令，從專案的資料夾，其中`<PW>`的密碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="d2c2f-256">如果應用程式的連絡人：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-256">If the app has contacts:</span></span>

* <span data-ttu-id="d2c2f-257">刪除的記錄中的所有`Contact`資料表。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="d2c2f-258">重新啟動植入資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="d2c2f-259">測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 工作階段）。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="d2c2f-260">在瀏覽器中註冊新的使用者 (例如`test@example.com`)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="d2c2f-261">使用不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="d2c2f-262">請確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-262">Verify the following operations:</span></span>

* <span data-ttu-id="d2c2f-263">已註冊的使用者可以檢視所有已核准的連絡資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="d2c2f-264">已註冊的使用者可以編輯/刪除他們自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="d2c2f-265">經理可以核准/拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="d2c2f-266">`Details`檢視會顯示**核准**並**拒絕**按鈕。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="d2c2f-267">系統管理員可以核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="d2c2f-268">使用者</span><span class="sxs-lookup"><span data-stu-id="d2c2f-268">User</span></span>                | <span data-ttu-id="d2c2f-269">植入的應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-269">Seeded by the app</span></span> | <span data-ttu-id="d2c2f-270">選項</span><span class="sxs-lookup"><span data-stu-id="d2c2f-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="d2c2f-271">否</span><span class="sxs-lookup"><span data-stu-id="d2c2f-271">No</span></span>                | <span data-ttu-id="d2c2f-272">編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="d2c2f-273">是</span><span class="sxs-lookup"><span data-stu-id="d2c2f-273">Yes</span></span>               | <span data-ttu-id="d2c2f-274">核准/拒絕和編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="d2c2f-275">是</span><span class="sxs-lookup"><span data-stu-id="d2c2f-275">Yes</span></span>               | <span data-ttu-id="d2c2f-276">核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="d2c2f-277">在系統管理員的瀏覽器中建立的連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="d2c2f-278">複製的 URL 刪除和編輯從系統管理員連絡。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="d2c2f-279">這些連結貼入測試使用者的瀏覽器，以確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="d2c2f-280">建立入門應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-280">Create the starter app</span></span>

* <span data-ttu-id="d2c2f-281">建立名為"ContactManager"Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="d2c2f-282">建立應用程式與**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="d2c2f-283">提供名稱"ContactManager"使命名空間符合此範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="d2c2f-284">`-uld` 指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="d2c2f-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="d2c2f-285">新增*模型/連絡人 .cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="d2c2f-286">Scaffold`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="d2c2f-287">建立初始移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="d2c2f-288">如果您遇到`dotnet aspnet-codegenerator razorpage`命令的錯誤, 請參閱[此 GitHub 問題](https://github.com/aspnet/Scaffolding/issues/984)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="d2c2f-289">更新*Pages/Shared/_Layout*檔案中的**ContactManager**錨點:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="d2c2f-290">測試應用程式建立、 編輯和刪除連絡人</span><span class="sxs-lookup"><span data-stu-id="d2c2f-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="d2c2f-291">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="d2c2f-291">Seed the database</span></span>

<span data-ttu-id="d2c2f-292">將[SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs)類別新增至 [*資料*] 資料夾:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="d2c2f-293">呼叫`SeedData.Initialize`從`Main`:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="d2c2f-294">測試應用程式植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-294">Test that the app seeded the database.</span></span> <span data-ttu-id="d2c2f-295">請連絡資料庫中有任何資料列，如果種子方法不會執行。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="d2c2f-296">本教學課程會示範如何建立 ASP.NET Core web 應用程式與受保護的授權的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="d2c2f-297">它會顯示已驗證 （登錄） 的使用者的連絡人清單中已建立。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="d2c2f-298">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-298">There are three security groups:</span></span>

* <span data-ttu-id="d2c2f-299">**已註冊的使用者**可以檢視所有已認可的資料並可以編輯/刪除他們自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="d2c2f-300">**管理員**可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="d2c2f-301">只有已核准的連絡人會對使用者顯示。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="d2c2f-302">**系統管理員**可以核准/拒絕並編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="d2c2f-303">在下圖中，使用者 Rick (`rick@example.com`) 登入。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="d2c2f-304">Rick 只能檢視已核准的連絡人和**編輯**/**刪除**/**新建**他連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="d2c2f-305">只有最後一筆記錄，由 Rick，顯示**編輯**並**刪除**連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="d2c2f-306">其他使用者不會看到最後一筆記錄，直到管理員或系統管理員的狀態變更為 「 Approved 」。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![螢幕擷取畫面顯示 Rick 登入](secure-data/_static/rick.png)

<span data-ttu-id="d2c2f-308">在下圖中, `manager@contoso.com`已登入並以管理員的角色執行:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![螢幕擷取畫面顯示manager@contoso.com登入](secure-data/_static/manager1.png)

<span data-ttu-id="d2c2f-310">下圖顯示之管理員的連絡詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-310">The following image shows the managers details view of a contact:</span></span>

![連絡人的管理員的檢視](secure-data/_static/manager.png)

<span data-ttu-id="d2c2f-312">**核准**並**拒絕**按鈕只會顯示為管理員和系統管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="d2c2f-313">在下圖中, `admin@contoso.com`已登入, 並以系統管理員角色:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![螢幕擷取畫面顯示admin@contoso.com登入](secure-data/_static/admin.png)

<span data-ttu-id="d2c2f-315">系統管理員將擁有所有權限。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-315">The administrator has all privileges.</span></span> <span data-ttu-id="d2c2f-316">她可以讀取/編輯/刪除的任何連絡人，並變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="d2c2f-317">藉由建立應用程式[scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)下列`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="d2c2f-318">此範例包含下列 「 授權 」 處理常式：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="d2c2f-319">`ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="d2c2f-320">`ContactManagerAuthorizationHandler`：可讓管理員核准或拒絕連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="d2c2f-321">`ContactAdministratorsAuthorizationHandler`：允許系統管理員核准或拒絕連絡人, 以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2c2f-322">必要條件</span><span class="sxs-lookup"><span data-stu-id="d2c2f-322">Prerequisites</span></span>

<span data-ttu-id="d2c2f-323">本教學課程會前進。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-323">This tutorial is advanced.</span></span> <span data-ttu-id="d2c2f-324">您應該先熟悉：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-324">You should be familiar with:</span></span>

* [<span data-ttu-id="d2c2f-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2c2f-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="d2c2f-326">驗證</span><span class="sxs-lookup"><span data-stu-id="d2c2f-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d2c2f-327">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="d2c2f-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d2c2f-328">授權</span><span class="sxs-lookup"><span data-stu-id="d2c2f-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="d2c2f-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d2c2f-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="d2c2f-330">Starter 和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-330">The starter and completed app</span></span>

<span data-ttu-id="d2c2f-331">[下載](xref:index#how-to-download-a-sample)[完成](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="d2c2f-332">[測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="d2c2f-333">入門應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-333">The starter app</span></span>

<span data-ttu-id="d2c2f-334">[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="d2c2f-335">執行應用程式中，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="d2c2f-336">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="d2c2f-336">Secure user data</span></span>

<span data-ttu-id="d2c2f-337">下列各節會有所有主要的步驟，來建立安全的使用者資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="d2c2f-338">您可能會發現它已完成的專案參考很有幫助。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="d2c2f-339">將繫結至使用者的連絡人資料</span><span class="sxs-lookup"><span data-stu-id="d2c2f-339">Tie the contact data to the user</span></span>

<span data-ttu-id="d2c2f-340">使用 ASP.NET[識別](xref:security/authentication/identity)使用者識別碼，以確保使用者可以編輯其資料，但不是其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="d2c2f-341">新增`OwnerID`並`ContactStatus`到`Contact`模型：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="d2c2f-342">`OwnerID` 是來自使用者的識別碼`AspNetUser`資料表中[身分識別](xref:security/authentication/identity)資料庫。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="d2c2f-343">`Status`欄位可讓您判斷是否可供一般使用者檢視連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="d2c2f-344">建立新的移轉，並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="d2c2f-345">將角色服務新增到 身分識別</span><span class="sxs-lookup"><span data-stu-id="d2c2f-345">Add Role services to Identity</span></span>

<span data-ttu-id="d2c2f-346">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)來新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="d2c2f-347">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="d2c2f-347">Require authenticated users</span></span>

<span data-ttu-id="d2c2f-348">設定預設的驗證原則，以要求使用者進行驗證：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="d2c2f-349">您可以選擇退出的 Razor 頁面、 控制器或動作方法層級的驗證`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="d2c2f-350">設定預設的驗證原則，以要求使用者進行驗證，可保護新加入的 Razor 頁面和控制站。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="d2c2f-351">具有預設值所需的驗證會比依賴新的控制器和 Razor 頁面，以包含更安全`[Authorize]`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="d2c2f-352">新增[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)索引，因此匿名使用者可以取得站台的相關資訊，才能註冊的相關和連絡人頁面。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="d2c2f-353">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="d2c2f-353">Configure the test account</span></span>

<span data-ttu-id="d2c2f-354">`SeedData`類別會建立兩個帳戶： 系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="d2c2f-355">使用[Secret Manager 工具](xref:security/app-secrets)設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="d2c2f-356">設定密碼，從專案目錄 (目錄包含*Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="d2c2f-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="d2c2f-357">如果未指定強式密碼，則會擲回例外狀況時`SeedData.Initialize`呼叫。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="d2c2f-358">更新`Main`使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="d2c2f-359">建立測試帳戶和更新連絡人</span><span class="sxs-lookup"><span data-stu-id="d2c2f-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="d2c2f-360">更新`Initialize`方法中的`SeedData`類別來建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="d2c2f-361">新增系統管理員使用者識別碼和`ContactStatus`連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="d2c2f-362">讓其中一個 「 連絡人 」 已提交 」 和一個 「 已拒絕 」。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="d2c2f-363">加入所有連絡人的使用者識別碼和狀態。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="d2c2f-364">只有一個連絡人所示：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="d2c2f-365">建立擁有者 」、 「 管理員 」 和 「 系統管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="d2c2f-366">建立`ContactIsOwnerAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-366">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d2c2f-367">`ContactIsOwnerAuthorizationHandler`確認處理資源的使用者擁有的資源。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="d2c2f-368">`ContactIsOwnerAuthorizationHandler`呼叫[內容。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)目前已驗證的使用者是否連絡擁有者。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="d2c2f-369">授權的處理常式通常：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="d2c2f-370">傳回`context.Succeed`時符合的需求。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="d2c2f-371">傳回`Task.CompletedTask`時不符合需求。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="d2c2f-372">`Task.CompletedTask`不是成功或失敗&mdash;, 它允許其他授權處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="d2c2f-373">如果您需要明確地使失敗，傳回[內容。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="d2c2f-374">此應用程式會允許連絡人的擁有者可以編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="d2c2f-375">`ContactIsOwnerAuthorizationHandler` 不需要檢查傳入的要求參數的作業。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="d2c2f-376">建立管理員授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-376">Create a manager authorization handler</span></span>

<span data-ttu-id="d2c2f-377">建立`ContactManagerAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d2c2f-378">`ContactManagerAuthorizationHandler`確認處理資源的使用者是管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="d2c2f-379">只有經理可以核准或拒絕內容變更 （新增或變更）。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="d2c2f-380">建立系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="d2c2f-381">建立`ContactAdministratorsAuthorizationHandler`類別內*授權*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d2c2f-382">`ContactAdministratorsAuthorizationHandler`確認處理資源的使用者是系統管理員。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="d2c2f-383">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="d2c2f-384">註冊授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-384">Register the authorization handlers</span></span>

<span data-ttu-id="d2c2f-385">您必須針對註冊服務使用 Entity Framework Core[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="d2c2f-386">`ContactIsOwnerAuthorizationHandler`會使用 ASP.NET Core[身分識別](xref:security/authentication/identity)，這根據 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="d2c2f-387">註冊處理常式的服務集合，讓它們能夠`ContactsController`經由[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d2c2f-388">將下列程式碼新增至結尾`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="d2c2f-389">`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`會新增為單一子句。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="d2c2f-390">它們是單次個體，因為他們不使用 EF 和所需的所有資訊都都`Context`參數`HandleRequirementAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="d2c2f-391">支援的授權</span><span class="sxs-lookup"><span data-stu-id="d2c2f-391">Support authorization</span></span>

<span data-ttu-id="d2c2f-392">在本節中，您可以更新 Razor 頁面，並新增的運算需求類別。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="d2c2f-393">檢閱此連絡人的作業需求類別</span><span class="sxs-lookup"><span data-stu-id="d2c2f-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="d2c2f-394">檢閱`ContactOperations`類別。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="d2c2f-395">這個類別包含的需求，應用程式支援：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="d2c2f-396">建立連絡人 Razor 頁面的基底類別</span><span class="sxs-lookup"><span data-stu-id="d2c2f-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="d2c2f-397">建立基底類別，其中包含連絡人 Razor 頁面中使用的服務。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="d2c2f-398">基底類別會將初始化程式碼置於一個位置：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="d2c2f-399">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-399">The preceding code:</span></span>

* <span data-ttu-id="d2c2f-400">新增`IAuthorizationService`服務存取授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="d2c2f-401">將身分識別新增`UserManager`服務。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="d2c2f-402">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="d2c2f-403">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-403">Update the CreateModel</span></span>

<span data-ttu-id="d2c2f-404">更新 [建立] 頁面模型建構函式使用`DI_BasePageModel`基底類別：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="d2c2f-405">更新`CreateModel.OnPostAsync`方法：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="d2c2f-406">新增的使用者識別碼`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="d2c2f-407">呼叫授權處理常式，以確定使用者有權限來建立連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="d2c2f-408">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-408">Update the IndexModel</span></span>

<span data-ttu-id="d2c2f-409">更新`OnGetAsync`方法，讓只有核准的連絡人會向一般使用者顯示：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="d2c2f-410">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-410">Update the EditModel</span></span>

<span data-ttu-id="d2c2f-411">新增授權處理常式，以確認該使用者所擁有的連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="d2c2f-412">正在驗證資源的授權，因為`[Authorize]`屬性還不夠。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="d2c2f-413">要在評估屬性時，應用程式就不需要資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="d2c2f-414">資源為基礎的授權必須是必要的。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="d2c2f-415">一旦應用程式存取的資源中，載入頁面模型中，或載入處理常式本身內，則必須執行檢查。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="d2c2f-416">您經常存取的資源，藉由傳入的資源索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="d2c2f-417">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="d2c2f-417">Update the DeleteModel</span></span>

<span data-ttu-id="d2c2f-418">更新使用授權處理常式，以確定使用者有刪除權限，在連絡人上的 [刪除] 頁面模型。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="d2c2f-419">插入檢視中的授權服務</span><span class="sxs-lookup"><span data-stu-id="d2c2f-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="d2c2f-420">目前，UI 顯示編輯和刪除的使用者無法修改的連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="d2c2f-421">插入中的授權服務*views/_viewimports.cshtml*檔案，以便它可供所有檢視：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="d2c2f-422">上述標記會新增數個`using`陳述式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="d2c2f-423">更新**編輯**並**刪除**中的連結*Pages/Contacts/Index.cshtml*讓它們只轉譯為適當的權限的使用者：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="d2c2f-424">隱藏不需要變更資料的權限的使用者的連結不安全的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="d2c2f-425">隱藏的連結，讓應用程式更方便使用顯示唯一有效的連結。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="d2c2f-426">使用者可以 hack 產生的 Url 以叫用 編輯和刪除作業對他們未擁有的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="d2c2f-427">Razor 頁面或控制站必須強制執行存取檢查，以確保資料的安全。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="d2c2f-428">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="d2c2f-428">Update Details</span></span>

<span data-ttu-id="d2c2f-429">更新詳細資料檢視，讓經理可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="d2c2f-430">更新詳細資料 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="d2c2f-431">新增或移除使用者角色</span><span class="sxs-lookup"><span data-stu-id="d2c2f-431">Add or remove a user to a role</span></span>

<span data-ttu-id="d2c2f-432">請參閱[本期](https://github.com/aspnet/AspNetCore.Docs/issues/8502)有關：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="d2c2f-433">移除使用者的權限。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-433">Removing privileges from a user.</span></span> <span data-ttu-id="d2c2f-434">例如, 將聊天應用程式中的使用者靜音。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="d2c2f-435">若要新增權限。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="d2c2f-436">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-436">Test the completed app</span></span>

<span data-ttu-id="d2c2f-437">如果您尚未設定植入的使用者帳戶的密碼，使用[Secret Manager 工具](xref:security/app-secrets#secret-manager)設定密碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="d2c2f-438">選擇強式密碼:請使用八個或更多個字元, 且至少要有一個大寫字元、數位和符號。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="d2c2f-439">比方說，`Passw0rd!`符合強式密碼需求。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="d2c2f-440">執行下列命令，從專案的資料夾，其中`<PW>`的密碼：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="d2c2f-441">卸載並更新資料庫</span><span class="sxs-lookup"><span data-stu-id="d2c2f-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="d2c2f-442">重新啟動植入資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="d2c2f-443">測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 工作階段）。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="d2c2f-444">在瀏覽器中註冊新的使用者 (例如`test@example.com`)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="d2c2f-445">使用不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="d2c2f-446">請確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-446">Verify the following operations:</span></span>

* <span data-ttu-id="d2c2f-447">已註冊的使用者可以檢視所有已核准的連絡資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="d2c2f-448">已註冊的使用者可以編輯/刪除他們自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="d2c2f-449">經理可以核准/拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="d2c2f-450">`Details`檢視會顯示**核准**並**拒絕**按鈕。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="d2c2f-451">系統管理員可以核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="d2c2f-452">使用者</span><span class="sxs-lookup"><span data-stu-id="d2c2f-452">User</span></span>                | <span data-ttu-id="d2c2f-453">植入的應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-453">Seeded by the app</span></span> | <span data-ttu-id="d2c2f-454">選項</span><span class="sxs-lookup"><span data-stu-id="d2c2f-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="d2c2f-455">否</span><span class="sxs-lookup"><span data-stu-id="d2c2f-455">No</span></span>                | <span data-ttu-id="d2c2f-456">編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="d2c2f-457">是</span><span class="sxs-lookup"><span data-stu-id="d2c2f-457">Yes</span></span>               | <span data-ttu-id="d2c2f-458">核准/拒絕和編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="d2c2f-459">是</span><span class="sxs-lookup"><span data-stu-id="d2c2f-459">Yes</span></span>               | <span data-ttu-id="d2c2f-460">核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="d2c2f-461">在系統管理員的瀏覽器中建立的連絡人。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="d2c2f-462">複製的 URL 刪除和編輯從系統管理員連絡。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="d2c2f-463">這些連結貼入測試使用者的瀏覽器，以確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="d2c2f-464">建立入門應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-464">Create the starter app</span></span>

* <span data-ttu-id="d2c2f-465">建立名為"ContactManager"Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="d2c2f-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="d2c2f-466">建立應用程式與**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="d2c2f-467">提供名稱"ContactManager"使命名空間符合此範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="d2c2f-468">`-uld` 指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="d2c2f-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="d2c2f-469">新增*模型/連絡人 .cs*:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="d2c2f-470">Scaffold`Contact`模型。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="d2c2f-471">建立初始移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="d2c2f-472">更新**ContactManager**中的錨點*pages/_layout.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="d2c2f-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="d2c2f-473">測試應用程式建立、 編輯和刪除連絡人</span><span class="sxs-lookup"><span data-stu-id="d2c2f-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="d2c2f-474">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="d2c2f-474">Seed the database</span></span>

<span data-ttu-id="d2c2f-475">新增[SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs)類別，即可*資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="d2c2f-476">呼叫`SeedData.Initialize`從`Main`:</span><span class="sxs-lookup"><span data-stu-id="d2c2f-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="d2c2f-477">測試應用程式植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-477">Test that the app seeded the database.</span></span> <span data-ttu-id="d2c2f-478">請連絡資料庫中有任何資料列，如果種子方法不會執行。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="d2c2f-479">其他資源</span><span class="sxs-lookup"><span data-stu-id="d2c2f-479">Additional resources</span></span>

* [<span data-ttu-id="d2c2f-480">建置.NET Core 和 SQL Database web 應用程式在 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d2c2f-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="d2c2f-481">[ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="d2c2f-482">這個實驗室會進入此教學課程中介紹的安全性功能的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d2c2f-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="d2c2f-483">自訂原則式授權</span><span class="sxs-lookup"><span data-stu-id="d2c2f-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
