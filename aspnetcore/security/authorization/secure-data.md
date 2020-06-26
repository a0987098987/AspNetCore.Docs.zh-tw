---
title: 建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式
author: rick-anderson
description: 瞭解如何 Razor 使用受授權保護的使用者資料來建立頁面應用程式。 包括 HTTPS、驗證、安全性 ASP.NET Core Identity 。
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/secure-data
ms.openlocfilehash: f50015af864a4a62abd5e2eab508aac915cb6370
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404713"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="c82f5-104">建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="c82f5-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c82f5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="c82f5-106">如需 ASP.NET Core MVC 版本，請參閱[此 PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="c82f5-107">本教學課程的 ASP.NET Core 1.1 版本位於[此](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data)資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c82f5-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="c82f5-108">1.1 ASP.NET Core 範例位於[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)中。</span><span class="sxs-lookup"><span data-stu-id="c82f5-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c82f5-109">請參閱[此 pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="c82f5-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c82f5-110">本教學課程示範如何建立 ASP.NET Core web 應用程式，其中包含由授權保護的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c82f5-111">它會顯示已驗證（已註冊）使用者建立的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="c82f5-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c82f5-112">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="c82f5-112">There are three security groups:</span></span>

* <span data-ttu-id="c82f5-113">**已註冊的使用者**可以查看所有核准的資料，而且可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c82f5-114">**管理員**可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c82f5-115">使用者只能看到核准的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c82f5-116">系統**管理員**可以核准/拒絕和編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c82f5-117">本檔中的影像不完全符合最新的範本。</span><span class="sxs-lookup"><span data-stu-id="c82f5-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="c82f5-118">在下圖中，已登入使用者 Rick （ `rick@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c82f5-119">Rick 只能查看已核准的連絡人**Edit**，並 / **Delete** / 針對他的連絡人編輯 [刪除]**建立新**的連結。</span><span class="sxs-lookup"><span data-stu-id="c82f5-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c82f5-120">只有 [Rick] 所建立的最後一筆記錄會顯示 [**編輯**] 和 [**刪除**] 連結。</span><span class="sxs-lookup"><span data-stu-id="c82f5-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c82f5-121">在管理員或系統管理員將狀態變更為「已核准」之前，其他使用者將不會看到最後一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="c82f5-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![顯示 Rick 已登入的螢幕擷取畫面](secure-data/_static/rick.png)

<span data-ttu-id="c82f5-123">在下圖中， `manager@contoso.com` 已登入並以管理員的角色執行：</span><span class="sxs-lookup"><span data-stu-id="c82f5-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![顯示 manager@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/manager1.png)

<span data-ttu-id="c82f5-125">下圖顯示連絡人的 [管理員] 詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="c82f5-125">The following image shows the managers details view of a contact:</span></span>

![經理的連絡人觀點](secure-data/_static/manager.png)

<span data-ttu-id="c82f5-127">[**核准**] 和 [**拒絕**] 按鈕只會針對管理員和系統管理員顯示。</span><span class="sxs-lookup"><span data-stu-id="c82f5-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c82f5-128">在下圖中， `admin@contoso.com` 已登入，並以系統管理員角色：</span><span class="sxs-lookup"><span data-stu-id="c82f5-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![顯示 admin@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/admin.png)

<span data-ttu-id="c82f5-130">系統管理員具有擁有權限。</span><span class="sxs-lookup"><span data-stu-id="c82f5-130">The administrator has all privileges.</span></span> <span data-ttu-id="c82f5-131">她可以讀取/編輯/刪除任何連絡人，並變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="c82f5-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c82f5-132">應用程式[是由下列](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)模型所建立 `Contact` ：</span><span class="sxs-lookup"><span data-stu-id="c82f5-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c82f5-133">此範例包含下列授權處理常式：</span><span class="sxs-lookup"><span data-stu-id="c82f5-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c82f5-134">`ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c82f5-135">`ContactManagerAuthorizationHandler`：允許管理員核准或拒絕連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c82f5-136">`ContactAdministratorsAuthorizationHandler`：可讓系統管理員核准或拒絕連絡人，以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c82f5-137">必要條件</span><span class="sxs-lookup"><span data-stu-id="c82f5-137">Prerequisites</span></span>

<span data-ttu-id="c82f5-138">本教學課程是先進的。</span><span class="sxs-lookup"><span data-stu-id="c82f5-138">This tutorial is advanced.</span></span> <span data-ttu-id="c82f5-139">您應該已熟悉：</span><span class="sxs-lookup"><span data-stu-id="c82f5-139">You should be familiar with:</span></span>

* [<span data-ttu-id="c82f5-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c82f5-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c82f5-141">驗證</span><span class="sxs-lookup"><span data-stu-id="c82f5-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c82f5-142">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="c82f5-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c82f5-143">授權</span><span class="sxs-lookup"><span data-stu-id="c82f5-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="c82f5-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c82f5-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c82f5-145">入門和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-145">The starter and completed app</span></span>

<span data-ttu-id="c82f5-146">[下載](xref:index#how-to-download-a-sample)[已完成](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="c82f5-147">[測試](#test-the-completed-app)已完成的應用程式，以熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="c82f5-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c82f5-148">入門應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-148">The starter app</span></span>

<span data-ttu-id="c82f5-149">[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="c82f5-150">執行應用程式，並按一下 [ **ContactManager** ] 連結，並確認您可以建立、編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c82f5-151">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="c82f5-151">Secure user data</span></span>

<span data-ttu-id="c82f5-152">下列各節包含建立安全使用者資料應用程式的所有主要步驟。</span><span class="sxs-lookup"><span data-stu-id="c82f5-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c82f5-153">您可能會發現參考已完成的專案很有説明。</span><span class="sxs-lookup"><span data-stu-id="c82f5-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c82f5-154">將連絡人資料與使用者結合</span><span class="sxs-lookup"><span data-stu-id="c82f5-154">Tie the contact data to the user</span></span>

<span data-ttu-id="c82f5-155">使用 ASP.NET [Identity](xref:security/authentication/identity) 使用者識別碼，以確保使用者可以編輯其資料，而不是其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c82f5-156">將 `OwnerID` 和新增 `ContactStatus` 至 `Contact` 模型：</span><span class="sxs-lookup"><span data-stu-id="c82f5-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c82f5-157">`OwnerID`這是 `AspNetUser` 資料庫中資料表的使用者識別碼 [Identity](xref:security/authentication/identity) 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c82f5-158">`Status`欄位會決定一般使用者是否可以查看連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c82f5-159">建立新的遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c82f5-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c82f5-160">將角色服務新增至Identity</span><span class="sxs-lookup"><span data-stu-id="c82f5-160">Add Role services to Identity</span></span>

<span data-ttu-id="c82f5-161">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="c82f5-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c82f5-162">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="c82f5-162">Require authenticated users</span></span>

<span data-ttu-id="c82f5-163">將 [預設驗證原則] 設定為 [要求使用者進行驗證]：</span><span class="sxs-lookup"><span data-stu-id="c82f5-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="c82f5-164">您可以 Razor 使用屬性，在頁面、控制器或動作方法層級選擇不進行驗證 `[AllowAnonymous]` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c82f5-165">將預設驗證原則設定為 [要求使用者進行驗證] 可保護新新增的 Razor 頁面和控制器。</span><span class="sxs-lookup"><span data-stu-id="c82f5-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c82f5-166">預設需要驗證，比依賴新的控制器和 Razor 頁面來包含屬性更為安全 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c82f5-167">將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 [索引] 和 [隱私權] 頁面，讓匿名使用者可以在註冊之前取得網站的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="c82f5-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c82f5-168">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="c82f5-168">Configure the test account</span></span>

<span data-ttu-id="c82f5-169">`SeedData`類別會建立兩個帳戶：系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="c82f5-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c82f5-170">使用[秘密管理員工具](xref:security/app-secrets)來設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="c82f5-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c82f5-171">從專案目錄（包含*Program.cs*的目錄）設定密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c82f5-172">如果未指定強式密碼，則會在呼叫時擲回例外狀況 `SeedData.Initialize` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c82f5-173">更新 `Main` 以使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c82f5-174">建立測試帳戶並更新連絡人</span><span class="sxs-lookup"><span data-stu-id="c82f5-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c82f5-175">更新 `Initialize` 類別中的方法 `SeedData` ，以建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="c82f5-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c82f5-176">將系統管理員使用者識別碼和新增 `ContactStatus` 至連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c82f5-177">讓其中一個連絡人「已提交」和一個「已拒絕」。</span><span class="sxs-lookup"><span data-stu-id="c82f5-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c82f5-178">將 [使用者識別碼] 和 [狀態] 新增至所有連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c82f5-179">只會顯示一個連絡人：</span><span class="sxs-lookup"><span data-stu-id="c82f5-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c82f5-180">建立擁有者、管理員和系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c82f5-181">`ContactIsOwnerAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c82f5-182">`ContactIsOwnerAuthorizationHandler`會驗證對資源進行的使用者擁有資源。</span><span class="sxs-lookup"><span data-stu-id="c82f5-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c82f5-183">`ContactIsOwnerAuthorizationHandler`呼叫[內容。](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人擁有者，則會成功。</span><span class="sxs-lookup"><span data-stu-id="c82f5-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c82f5-184">授權處理常式一般：</span><span class="sxs-lookup"><span data-stu-id="c82f5-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="c82f5-185">`context.Succeed`符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="c82f5-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c82f5-186">`Task.CompletedTask`當不符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="c82f5-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c82f5-187">`Task.CompletedTask`不是成功或失敗， &mdash; 它允許其他授權處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="c82f5-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c82f5-188">如果您需要明確失敗，請傳回[coNtext。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="c82f5-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c82f5-189">應用程式可讓連絡人擁有者編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c82f5-190">`ContactIsOwnerAuthorizationHandler`不需要檢查在需求參數中傳遞的作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c82f5-191">建立管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-191">Create a manager authorization handler</span></span>

<span data-ttu-id="c82f5-192">`ContactManagerAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c82f5-193">`ContactManagerAuthorizationHandler`會驗證對資源的使用者是否為管理員。</span><span class="sxs-lookup"><span data-stu-id="c82f5-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c82f5-194">只有管理員可以核准或拒絕內容變更（新的或已變更）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c82f5-195">建立系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="c82f5-196">`ContactAdministratorsAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c82f5-197">`ContactAdministratorsAuthorizationHandler`會驗證對資源的使用者是否為系統管理員。</span><span class="sxs-lookup"><span data-stu-id="c82f5-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c82f5-198">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c82f5-199">註冊授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-199">Register the authorization handlers</span></span>

<span data-ttu-id="c82f5-200">使用 Entity Framework Core 的服務必須使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)註冊相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="c82f5-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c82f5-201">`ContactIsOwnerAuthorizationHandler` [Identity](xref:security/authentication/identity) 會使用以 Entity Framework Core 為基礎的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="c82f5-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c82f5-202">向服務集合註冊處理常式，以便透過相依性插入來使用它們 `ContactsController` 。 [dependency injection](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="c82f5-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c82f5-203">在結尾新增下列程式碼 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="c82f5-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="c82f5-204">`ContactAdministratorsAuthorizationHandler`和 `ContactManagerAuthorizationHandler` 會新增為單次個體。</span><span class="sxs-lookup"><span data-stu-id="c82f5-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c82f5-205">它們是單次個體的，因為它們不使用 EF，而所需的所有資訊都是在 `Context` 方法的參數中 `HandleRequirementAsync` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c82f5-206">支援授權</span><span class="sxs-lookup"><span data-stu-id="c82f5-206">Support authorization</span></span>

<span data-ttu-id="c82f5-207">在本節中，您會更新 Razor 頁面並加入作業需求類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c82f5-208">審查連絡人作業需求類別</span><span class="sxs-lookup"><span data-stu-id="c82f5-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="c82f5-209">檢查 `ContactOperations` 類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c82f5-210">此類別包含應用程式支援的需求：</span><span class="sxs-lookup"><span data-stu-id="c82f5-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c82f5-211">建立 [連絡人] 頁面的基類 Razor</span><span class="sxs-lookup"><span data-stu-id="c82f5-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c82f5-212">建立基類，其中包含 [連絡人] 頁面中使用的服務 Razor 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c82f5-213">基底類別會將初始化程式碼放在一個位置：</span><span class="sxs-lookup"><span data-stu-id="c82f5-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c82f5-214">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-214">The preceding code:</span></span>

* <span data-ttu-id="c82f5-215">新增 `IAuthorizationService` 服務以存取授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c82f5-216">新增 Identity `UserManager` 服務。</span><span class="sxs-lookup"><span data-stu-id="c82f5-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c82f5-217">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="c82f5-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c82f5-218">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-218">Update the CreateModel</span></span>

<span data-ttu-id="c82f5-219">更新 [建立] 頁面模型的函式，以使用 `DI_BasePageModel` 基類：</span><span class="sxs-lookup"><span data-stu-id="c82f5-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c82f5-220">將 `CreateModel.OnPostAsync` 方法更新為：</span><span class="sxs-lookup"><span data-stu-id="c82f5-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c82f5-221">將使用者識別碼新增至 `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="c82f5-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c82f5-222">呼叫授權處理常式，以確認使用者擁有建立連絡人的許可權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c82f5-223">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-223">Update the IndexModel</span></span>

<span data-ttu-id="c82f5-224">更新 `OnGetAsync` 方法，讓一般使用者只會看到已核准的連絡人：</span><span class="sxs-lookup"><span data-stu-id="c82f5-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c82f5-225">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-225">Update the EditModel</span></span>

<span data-ttu-id="c82f5-226">新增授權處理常式，以確認使用者擁有該連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c82f5-227">因為正在驗證資源授權，所以 `[Authorize]` 屬性不夠。</span><span class="sxs-lookup"><span data-stu-id="c82f5-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c82f5-228">評估屬性時，應用程式沒有資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c82f5-229">以資源為基礎的授權必須是必要的。</span><span class="sxs-lookup"><span data-stu-id="c82f5-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c82f5-230">一旦應用程式可存取資源，就必須執行檢查，方法是將它載入頁面模型中，或在處理常式本身內載入。</span><span class="sxs-lookup"><span data-stu-id="c82f5-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c82f5-231">您經常會藉由傳入資源金鑰來存取資源。</span><span class="sxs-lookup"><span data-stu-id="c82f5-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c82f5-232">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-232">Update the DeleteModel</span></span>

<span data-ttu-id="c82f5-233">更新 [刪除] 頁面模型，以使用授權處理常式來驗證使用者是否擁有連絡人的 [刪除] 許可權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c82f5-234">將授權服務插入至 views</span><span class="sxs-lookup"><span data-stu-id="c82f5-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="c82f5-235">目前，UI 會顯示使用者無法修改之連絡人的 [編輯] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="c82f5-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c82f5-236">在*Pages/_ViewImports. cshtml*檔案中插入授權服務，以供所有視圖使用：</span><span class="sxs-lookup"><span data-stu-id="c82f5-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c82f5-237">上述標記會加入數個 `using` 語句。</span><span class="sxs-lookup"><span data-stu-id="c82f5-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c82f5-238">更新*Pages/Contacts/Index*中的 [**編輯**] 和 [**刪除**] 連結，使其僅針對具有適當許可權的使用者呈現：</span><span class="sxs-lookup"><span data-stu-id="c82f5-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c82f5-239">隱藏不具有變更資料許可權之使用者的連結，並不會保護應用程式的安全。</span><span class="sxs-lookup"><span data-stu-id="c82f5-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c82f5-240">隱藏連結可只顯示有效的連結，讓應用程式更容易使用。</span><span class="sxs-lookup"><span data-stu-id="c82f5-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c82f5-241">使用者可以攻擊產生的 Url，以叫用其未擁有之資料的編輯和刪除作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c82f5-242">Razor頁面或控制器必須強制執行存取檢查，以保護資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c82f5-243">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="c82f5-243">Update Details</span></span>

<span data-ttu-id="c82f5-244">更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="c82f5-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c82f5-245">更新 [詳細資料] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="c82f5-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="c82f5-246">新增或移除角色的使用者</span><span class="sxs-lookup"><span data-stu-id="c82f5-246">Add or remove a user to a role</span></span>

<span data-ttu-id="c82f5-247">如需相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8502)：</span><span class="sxs-lookup"><span data-stu-id="c82f5-247">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="c82f5-248">移除使用者的許可權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-248">Removing privileges from a user.</span></span> <span data-ttu-id="c82f5-249">例如，將聊天應用程式中的使用者靜音。</span><span class="sxs-lookup"><span data-stu-id="c82f5-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="c82f5-250">將許可權新增至使用者。</span><span class="sxs-lookup"><span data-stu-id="c82f5-250">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="c82f5-251">挑戰與禁止之間的差異</span><span class="sxs-lookup"><span data-stu-id="c82f5-251">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="c82f5-252">此應用程式會將預設原則設定為[要求已驗證的使用者](#require-authenticated-users)。</span><span class="sxs-lookup"><span data-stu-id="c82f5-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="c82f5-253">下列程式碼允許匿名使用者。</span><span class="sxs-lookup"><span data-stu-id="c82f5-253">The following code allows anonymous users.</span></span> <span data-ttu-id="c82f5-254">允許匿名使用者顯示挑戰與禁止之間的差異。</span><span class="sxs-lookup"><span data-stu-id="c82f5-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="c82f5-255">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="c82f5-255">In the preceding code:</span></span>

* <span data-ttu-id="c82f5-256">當使用者**未**經過驗證時， `ChallengeResult` 會傳回。</span><span class="sxs-lookup"><span data-stu-id="c82f5-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="c82f5-257">當傳回時，會將 `ChallengeResult` 使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="c82f5-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="c82f5-258">當使用者經過驗證但未獲授權時， `ForbidResult` 會傳回。</span><span class="sxs-lookup"><span data-stu-id="c82f5-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="c82f5-259">當 `ForbidResult` 傳回時，使用者會被重新導向至 [拒絕存取] 頁面。</span><span class="sxs-lookup"><span data-stu-id="c82f5-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="c82f5-260">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-260">Test the completed app</span></span>

<span data-ttu-id="c82f5-261">如果您尚未設定已植入之使用者帳戶的密碼，請使用[秘密管理員工具](xref:security/app-secrets#secret-manager)來設定密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="c82f5-262">選擇強式密碼：使用八個或更多個字元，且至少要有一個大寫字元、數位和符號。</span><span class="sxs-lookup"><span data-stu-id="c82f5-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="c82f5-263">例如， `Passw0rd!` 符合強式密碼需求。</span><span class="sxs-lookup"><span data-stu-id="c82f5-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="c82f5-264">從專案的資料夾執行下列命令，其中 `<PW>` 是密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="c82f5-265">如果應用程式有連絡人：</span><span class="sxs-lookup"><span data-stu-id="c82f5-265">If the app has contacts:</span></span>

* <span data-ttu-id="c82f5-266">刪除資料表中的所有記錄 `Contact` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="c82f5-267">重新開機應用程式以植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c82f5-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="c82f5-268">測試已完成之應用程式的簡單方法，是啟動三個不同的瀏覽器（或 incognito/InPrivate 會話）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="c82f5-269">在一個瀏覽器中，註冊新的使用者（例如 `test@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c82f5-270">使用不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c82f5-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c82f5-271">確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="c82f5-271">Verify the following operations:</span></span>

* <span data-ttu-id="c82f5-272">已註冊的使用者可以查看所有已核准的連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="c82f5-273">已註冊的使用者可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c82f5-274">管理員可以核准/拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="c82f5-275">此 `Details` 視圖會顯示 [**核准**] 和 [**拒絕**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c82f5-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c82f5-276">系統管理員可以核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="c82f5-277">User</span><span class="sxs-lookup"><span data-stu-id="c82f5-277">User</span></span>                | <span data-ttu-id="c82f5-278">由應用程式植入</span><span class="sxs-lookup"><span data-stu-id="c82f5-278">Seeded by the app</span></span> | <span data-ttu-id="c82f5-279">選項</span><span class="sxs-lookup"><span data-stu-id="c82f5-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="c82f5-280">否</span><span class="sxs-lookup"><span data-stu-id="c82f5-280">No</span></span>                | <span data-ttu-id="c82f5-281">編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="c82f5-282">是</span><span class="sxs-lookup"><span data-stu-id="c82f5-282">Yes</span></span>               | <span data-ttu-id="c82f5-283">核准/拒絕和編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="c82f5-284">是</span><span class="sxs-lookup"><span data-stu-id="c82f5-284">Yes</span></span>               | <span data-ttu-id="c82f5-285">核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="c82f5-286">在系統管理員的瀏覽器中建立連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c82f5-287">從系統管理員連絡人中複製 [刪除] 和 [編輯] 的 URL。</span><span class="sxs-lookup"><span data-stu-id="c82f5-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c82f5-288">將這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c82f5-289">建立入門應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-289">Create the starter app</span></span>

* <span data-ttu-id="c82f5-290">建立 Razor 名為 "ContactManager" 的頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="c82f5-291">建立具有**個別使用者帳戶**的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c82f5-292">將它命名為 "ContactManager"，讓命名空間符合範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="c82f5-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="c82f5-293">`-uld`指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="c82f5-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c82f5-294">新增*模型/連絡人 .cs*：</span><span class="sxs-lookup"><span data-stu-id="c82f5-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c82f5-295">Scaffold `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="c82f5-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c82f5-296">建立初始遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c82f5-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="c82f5-297">如果您遇到命令的錯誤 `dotnet aspnet-codegenerator razorpage` ，請參閱[此 GitHub 問題](https://github.com/aspnet/Scaffolding/issues/984)。</span><span class="sxs-lookup"><span data-stu-id="c82f5-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="c82f5-298">更新*Pages/Shared/_Layout. cshtml*檔案中的**ContactManager**錨點：</span><span class="sxs-lookup"><span data-stu-id="c82f5-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="c82f5-299">藉由建立、編輯和刪除連絡人來測試應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c82f5-300">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="c82f5-300">Seed the database</span></span>

<span data-ttu-id="c82f5-301">將[SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs)類別新增至 [*資料*] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="c82f5-301">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="c82f5-302">呼叫 `SeedData.Initialize` 來源 `Main` ：</span><span class="sxs-lookup"><span data-stu-id="c82f5-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="c82f5-303">測試應用程式是否植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c82f5-303">Test that the app seeded the database.</span></span> <span data-ttu-id="c82f5-304">如果 contact DB 中有任何資料列，則不會執行種子方法。</span><span class="sxs-lookup"><span data-stu-id="c82f5-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="c82f5-305">本教學課程示範如何建立 ASP.NET Core web 應用程式，其中包含由授權保護的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c82f5-306">它會顯示已驗證（已註冊）使用者建立的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="c82f5-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c82f5-307">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="c82f5-307">There are three security groups:</span></span>

* <span data-ttu-id="c82f5-308">**已註冊的使用者**可以查看所有核准的資料，而且可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c82f5-309">**管理員**可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c82f5-310">使用者只能看到核准的連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c82f5-311">系統**管理員**可以核准/拒絕和編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c82f5-312">在下圖中，已登入使用者 Rick （ `rick@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c82f5-313">Rick 只能查看已核准的連絡人**Edit**，並 / **Delete** / 針對他的連絡人編輯 [刪除]**建立新**的連結。</span><span class="sxs-lookup"><span data-stu-id="c82f5-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c82f5-314">只有 [Rick] 所建立的最後一筆記錄會顯示 [**編輯**] 和 [**刪除**] 連結。</span><span class="sxs-lookup"><span data-stu-id="c82f5-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c82f5-315">在管理員或系統管理員將狀態變更為「已核准」之前，其他使用者將不會看到最後一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="c82f5-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![顯示 Rick 已登入的螢幕擷取畫面](secure-data/_static/rick.png)

<span data-ttu-id="c82f5-317">在下圖中， `manager@contoso.com` 已登入並以管理員的角色執行：</span><span class="sxs-lookup"><span data-stu-id="c82f5-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![顯示 manager@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/manager1.png)

<span data-ttu-id="c82f5-319">下圖顯示連絡人的 [管理員] 詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="c82f5-319">The following image shows the managers details view of a contact:</span></span>

![經理的連絡人觀點](secure-data/_static/manager.png)

<span data-ttu-id="c82f5-321">[**核准**] 和 [**拒絕**] 按鈕只會針對管理員和系統管理員顯示。</span><span class="sxs-lookup"><span data-stu-id="c82f5-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c82f5-322">在下圖中， `admin@contoso.com` 已登入，並以系統管理員角色：</span><span class="sxs-lookup"><span data-stu-id="c82f5-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![顯示 admin@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/admin.png)

<span data-ttu-id="c82f5-324">系統管理員具有擁有權限。</span><span class="sxs-lookup"><span data-stu-id="c82f5-324">The administrator has all privileges.</span></span> <span data-ttu-id="c82f5-325">她可以讀取/編輯/刪除任何連絡人，並變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="c82f5-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c82f5-326">應用程式[是由下列](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)模型所建立 `Contact` ：</span><span class="sxs-lookup"><span data-stu-id="c82f5-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c82f5-327">此範例包含下列授權處理常式：</span><span class="sxs-lookup"><span data-stu-id="c82f5-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c82f5-328">`ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c82f5-329">`ContactManagerAuthorizationHandler`：允許管理員核准或拒絕連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c82f5-330">`ContactAdministratorsAuthorizationHandler`：可讓系統管理員核准或拒絕連絡人，以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c82f5-331">必要條件</span><span class="sxs-lookup"><span data-stu-id="c82f5-331">Prerequisites</span></span>

<span data-ttu-id="c82f5-332">本教學課程是先進的。</span><span class="sxs-lookup"><span data-stu-id="c82f5-332">This tutorial is advanced.</span></span> <span data-ttu-id="c82f5-333">您應該已熟悉：</span><span class="sxs-lookup"><span data-stu-id="c82f5-333">You should be familiar with:</span></span>

* [<span data-ttu-id="c82f5-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c82f5-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c82f5-335">驗證</span><span class="sxs-lookup"><span data-stu-id="c82f5-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c82f5-336">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="c82f5-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c82f5-337">授權</span><span class="sxs-lookup"><span data-stu-id="c82f5-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="c82f5-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c82f5-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c82f5-339">入門和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-339">The starter and completed app</span></span>

<span data-ttu-id="c82f5-340">[下載](xref:index#how-to-download-a-sample)[已完成](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="c82f5-341">[測試](#test-the-completed-app)已完成的應用程式，以熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="c82f5-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c82f5-342">入門應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-342">The starter app</span></span>

<span data-ttu-id="c82f5-343">[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="c82f5-344">執行應用程式，並按一下 [ **ContactManager** ] 連結，並確認您可以建立、編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c82f5-345">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="c82f5-345">Secure user data</span></span>

<span data-ttu-id="c82f5-346">下列各節包含建立安全使用者資料應用程式的所有主要步驟。</span><span class="sxs-lookup"><span data-stu-id="c82f5-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c82f5-347">您可能會發現參考已完成的專案很有説明。</span><span class="sxs-lookup"><span data-stu-id="c82f5-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c82f5-348">將連絡人資料與使用者結合</span><span class="sxs-lookup"><span data-stu-id="c82f5-348">Tie the contact data to the user</span></span>

<span data-ttu-id="c82f5-349">使用 ASP.NET [Identity](xref:security/authentication/identity) 使用者識別碼，以確保使用者可以編輯其資料，而不是其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c82f5-350">將 `OwnerID` 和新增 `ContactStatus` 至 `Contact` 模型：</span><span class="sxs-lookup"><span data-stu-id="c82f5-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c82f5-351">`OwnerID`這是 `AspNetUser` 資料庫中資料表的使用者識別碼 [Identity](xref:security/authentication/identity) 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c82f5-352">`Status`欄位會決定一般使用者是否可以查看連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c82f5-353">建立新的遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c82f5-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c82f5-354">將角色服務新增至Identity</span><span class="sxs-lookup"><span data-stu-id="c82f5-354">Add Role services to Identity</span></span>

<span data-ttu-id="c82f5-355">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="c82f5-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c82f5-356">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="c82f5-356">Require authenticated users</span></span>

<span data-ttu-id="c82f5-357">將 [預設驗證原則] 設定為 [要求使用者進行驗證]：</span><span class="sxs-lookup"><span data-stu-id="c82f5-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="c82f5-358">您可以 Razor 使用屬性，在頁面、控制器或動作方法層級選擇不進行驗證 `[AllowAnonymous]` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c82f5-359">將預設驗證原則設定為 [要求使用者進行驗證] 可保護新新增的 Razor 頁面和控制器。</span><span class="sxs-lookup"><span data-stu-id="c82f5-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c82f5-360">預設需要驗證，比依賴新的控制器和 Razor 頁面來包含屬性更為安全 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c82f5-361">將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 [索引]、[關於] 和 [連絡人] 頁面，讓匿名使用者可以在註冊之前取得網站的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="c82f5-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c82f5-362">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="c82f5-362">Configure the test account</span></span>

<span data-ttu-id="c82f5-363">`SeedData`類別會建立兩個帳戶：系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="c82f5-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c82f5-364">使用[秘密管理員工具](xref:security/app-secrets)來設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="c82f5-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c82f5-365">從專案目錄（包含*Program.cs*的目錄）設定密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c82f5-366">如果未指定強式密碼，則會在呼叫時擲回例外狀況 `SeedData.Initialize` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c82f5-367">更新 `Main` 以使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c82f5-368">建立測試帳戶並更新連絡人</span><span class="sxs-lookup"><span data-stu-id="c82f5-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c82f5-369">更新 `Initialize` 類別中的方法 `SeedData` ，以建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="c82f5-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c82f5-370">將系統管理員使用者識別碼和新增 `ContactStatus` 至連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c82f5-371">讓其中一個連絡人「已提交」和一個「已拒絕」。</span><span class="sxs-lookup"><span data-stu-id="c82f5-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c82f5-372">將 [使用者識別碼] 和 [狀態] 新增至所有連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c82f5-373">只會顯示一個連絡人：</span><span class="sxs-lookup"><span data-stu-id="c82f5-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c82f5-374">建立擁有者、管理員和系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c82f5-375">建立*授權*資料夾，並 `ContactIsOwnerAuthorizationHandler` 在其中建立類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="c82f5-376">`ContactIsOwnerAuthorizationHandler`會驗證對資源進行的使用者擁有資源。</span><span class="sxs-lookup"><span data-stu-id="c82f5-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c82f5-377">`ContactIsOwnerAuthorizationHandler`呼叫[內容。](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人擁有者，則會成功。</span><span class="sxs-lookup"><span data-stu-id="c82f5-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c82f5-378">授權處理常式一般：</span><span class="sxs-lookup"><span data-stu-id="c82f5-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="c82f5-379">`context.Succeed`符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="c82f5-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c82f5-380">`Task.CompletedTask`當不符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="c82f5-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c82f5-381">`Task.CompletedTask`不是成功或失敗， &mdash; 它允許其他授權處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="c82f5-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c82f5-382">如果您需要明確失敗，請傳回[coNtext。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="c82f5-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c82f5-383">應用程式可讓連絡人擁有者編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c82f5-384">`ContactIsOwnerAuthorizationHandler`不需要檢查在需求參數中傳遞的作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c82f5-385">建立管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-385">Create a manager authorization handler</span></span>

<span data-ttu-id="c82f5-386">`ContactManagerAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c82f5-387">`ContactManagerAuthorizationHandler`會驗證對資源的使用者是否為管理員。</span><span class="sxs-lookup"><span data-stu-id="c82f5-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c82f5-388">只有管理員可以核准或拒絕內容變更（新的或已變更）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c82f5-389">建立系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="c82f5-390">`ContactAdministratorsAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c82f5-391">`ContactAdministratorsAuthorizationHandler`會驗證對資源的使用者是否為系統管理員。</span><span class="sxs-lookup"><span data-stu-id="c82f5-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c82f5-392">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c82f5-393">註冊授權處理常式</span><span class="sxs-lookup"><span data-stu-id="c82f5-393">Register the authorization handlers</span></span>

<span data-ttu-id="c82f5-394">使用 Entity Framework Core 的服務必須使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)註冊相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="c82f5-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c82f5-395">`ContactIsOwnerAuthorizationHandler` [Identity](xref:security/authentication/identity) 會使用以 Entity Framework Core 為基礎的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="c82f5-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c82f5-396">向服務集合註冊處理常式，以便透過相依性插入來使用它們 `ContactsController` 。 [dependency injection](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="c82f5-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c82f5-397">在結尾新增下列程式碼 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="c82f5-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="c82f5-398">`ContactAdministratorsAuthorizationHandler`和 `ContactManagerAuthorizationHandler` 會新增為單次個體。</span><span class="sxs-lookup"><span data-stu-id="c82f5-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c82f5-399">它們是單次個體的，因為它們不使用 EF，而所需的所有資訊都是在 `Context` 方法的參數中 `HandleRequirementAsync` 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c82f5-400">支援授權</span><span class="sxs-lookup"><span data-stu-id="c82f5-400">Support authorization</span></span>

<span data-ttu-id="c82f5-401">在本節中，您會更新 Razor 頁面並加入作業需求類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c82f5-402">審查連絡人作業需求類別</span><span class="sxs-lookup"><span data-stu-id="c82f5-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="c82f5-403">檢查 `ContactOperations` 類別。</span><span class="sxs-lookup"><span data-stu-id="c82f5-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c82f5-404">此類別包含應用程式支援的需求：</span><span class="sxs-lookup"><span data-stu-id="c82f5-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c82f5-405">建立 [連絡人] 頁面的基類 Razor</span><span class="sxs-lookup"><span data-stu-id="c82f5-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c82f5-406">建立基類，其中包含 [連絡人] 頁面中使用的服務 Razor 。</span><span class="sxs-lookup"><span data-stu-id="c82f5-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c82f5-407">基底類別會將初始化程式碼放在一個位置：</span><span class="sxs-lookup"><span data-stu-id="c82f5-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c82f5-408">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-408">The preceding code:</span></span>

* <span data-ttu-id="c82f5-409">新增 `IAuthorizationService` 服務以存取授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c82f5-410">新增 Identity `UserManager` 服務。</span><span class="sxs-lookup"><span data-stu-id="c82f5-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c82f5-411">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="c82f5-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c82f5-412">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-412">Update the CreateModel</span></span>

<span data-ttu-id="c82f5-413">更新 [建立] 頁面模型的函式，以使用 `DI_BasePageModel` 基類：</span><span class="sxs-lookup"><span data-stu-id="c82f5-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c82f5-414">將 `CreateModel.OnPostAsync` 方法更新為：</span><span class="sxs-lookup"><span data-stu-id="c82f5-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c82f5-415">將使用者識別碼新增至 `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="c82f5-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c82f5-416">呼叫授權處理常式，以確認使用者擁有建立連絡人的許可權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c82f5-417">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-417">Update the IndexModel</span></span>

<span data-ttu-id="c82f5-418">更新 `OnGetAsync` 方法，讓一般使用者只會看到已核准的連絡人：</span><span class="sxs-lookup"><span data-stu-id="c82f5-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c82f5-419">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-419">Update the EditModel</span></span>

<span data-ttu-id="c82f5-420">新增授權處理常式，以確認使用者擁有該連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c82f5-421">因為正在驗證資源授權，所以 `[Authorize]` 屬性不夠。</span><span class="sxs-lookup"><span data-stu-id="c82f5-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c82f5-422">評估屬性時，應用程式沒有資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c82f5-423">以資源為基礎的授權必須是必要的。</span><span class="sxs-lookup"><span data-stu-id="c82f5-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c82f5-424">一旦應用程式可存取資源，就必須執行檢查，方法是將它載入頁面模型中，或在處理常式本身內載入。</span><span class="sxs-lookup"><span data-stu-id="c82f5-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c82f5-425">您經常會藉由傳入資源金鑰來存取資源。</span><span class="sxs-lookup"><span data-stu-id="c82f5-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c82f5-426">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c82f5-426">Update the DeleteModel</span></span>

<span data-ttu-id="c82f5-427">更新 [刪除] 頁面模型，以使用授權處理常式來驗證使用者是否擁有連絡人的 [刪除] 許可權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c82f5-428">將授權服務插入至 views</span><span class="sxs-lookup"><span data-stu-id="c82f5-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="c82f5-429">目前，UI 會顯示使用者無法修改之連絡人的 [編輯] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="c82f5-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c82f5-430">將授權服務插入*views/_ViewImports. cshtml*檔案中，以供所有視圖使用：</span><span class="sxs-lookup"><span data-stu-id="c82f5-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c82f5-431">上述標記會加入數個 `using` 語句。</span><span class="sxs-lookup"><span data-stu-id="c82f5-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c82f5-432">更新*Pages/Contacts/Index*中的 [**編輯**] 和 [**刪除**] 連結，使其僅針對具有適當許可權的使用者呈現：</span><span class="sxs-lookup"><span data-stu-id="c82f5-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c82f5-433">隱藏不具有變更資料許可權之使用者的連結，並不會保護應用程式的安全。</span><span class="sxs-lookup"><span data-stu-id="c82f5-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c82f5-434">隱藏連結可只顯示有效的連結，讓應用程式更容易使用。</span><span class="sxs-lookup"><span data-stu-id="c82f5-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c82f5-435">使用者可以攻擊產生的 Url，以叫用其未擁有之資料的編輯和刪除作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c82f5-436">Razor頁面或控制器必須強制執行存取檢查，以保護資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c82f5-437">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="c82f5-437">Update Details</span></span>

<span data-ttu-id="c82f5-438">更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="c82f5-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c82f5-439">更新 [詳細資料] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="c82f5-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="c82f5-440">新增或移除角色的使用者</span><span class="sxs-lookup"><span data-stu-id="c82f5-440">Add or remove a user to a role</span></span>

<span data-ttu-id="c82f5-441">如需相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8502)：</span><span class="sxs-lookup"><span data-stu-id="c82f5-441">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="c82f5-442">移除使用者的許可權。</span><span class="sxs-lookup"><span data-stu-id="c82f5-442">Removing privileges from a user.</span></span> <span data-ttu-id="c82f5-443">例如，將聊天應用程式中的使用者靜音。</span><span class="sxs-lookup"><span data-stu-id="c82f5-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="c82f5-444">將許可權新增至使用者。</span><span class="sxs-lookup"><span data-stu-id="c82f5-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="c82f5-445">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-445">Test the completed app</span></span>

<span data-ttu-id="c82f5-446">如果您尚未設定已植入之使用者帳戶的密碼，請使用[秘密管理員工具](xref:security/app-secrets#secret-manager)來設定密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="c82f5-447">選擇強式密碼：使用八個或更多個字元，且至少要有一個大寫字元、數位和符號。</span><span class="sxs-lookup"><span data-stu-id="c82f5-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="c82f5-448">例如， `Passw0rd!` 符合強式密碼需求。</span><span class="sxs-lookup"><span data-stu-id="c82f5-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="c82f5-449">從專案的資料夾執行下列命令，其中 `<PW>` 是密碼：</span><span class="sxs-lookup"><span data-stu-id="c82f5-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="c82f5-450">卸載並更新資料庫</span><span class="sxs-lookup"><span data-stu-id="c82f5-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="c82f5-451">重新開機應用程式以植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c82f5-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="c82f5-452">測試已完成之應用程式的簡單方法，是啟動三個不同的瀏覽器（或 incognito/InPrivate 會話）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="c82f5-453">在一個瀏覽器中，註冊新的使用者（例如 `test@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="c82f5-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c82f5-454">使用不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c82f5-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c82f5-455">確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="c82f5-455">Verify the following operations:</span></span>

* <span data-ttu-id="c82f5-456">已註冊的使用者可以查看所有已核准的連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="c82f5-457">已註冊的使用者可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c82f5-458">管理員可以核准/拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="c82f5-459">此 `Details` 視圖會顯示 [**核准**] 和 [**拒絕**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c82f5-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c82f5-460">系統管理員可以核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="c82f5-461">User</span><span class="sxs-lookup"><span data-stu-id="c82f5-461">User</span></span>                | <span data-ttu-id="c82f5-462">由應用程式植入</span><span class="sxs-lookup"><span data-stu-id="c82f5-462">Seeded by the app</span></span> | <span data-ttu-id="c82f5-463">選項</span><span class="sxs-lookup"><span data-stu-id="c82f5-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="c82f5-464">否</span><span class="sxs-lookup"><span data-stu-id="c82f5-464">No</span></span>                | <span data-ttu-id="c82f5-465">編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="c82f5-466">是</span><span class="sxs-lookup"><span data-stu-id="c82f5-466">Yes</span></span>               | <span data-ttu-id="c82f5-467">核准/拒絕和編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="c82f5-468">是</span><span class="sxs-lookup"><span data-stu-id="c82f5-468">Yes</span></span>               | <span data-ttu-id="c82f5-469">核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="c82f5-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="c82f5-470">在系統管理員的瀏覽器中建立連絡人。</span><span class="sxs-lookup"><span data-stu-id="c82f5-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c82f5-471">從系統管理員連絡人中複製 [刪除] 和 [編輯] 的 URL。</span><span class="sxs-lookup"><span data-stu-id="c82f5-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c82f5-472">將這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="c82f5-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c82f5-473">建立入門應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-473">Create the starter app</span></span>

* <span data-ttu-id="c82f5-474">建立 Razor 名為 "ContactManager" 的頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="c82f5-475">建立具有**個別使用者帳戶**的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c82f5-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c82f5-476">將它命名為 "ContactManager"，讓命名空間符合範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="c82f5-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="c82f5-477">`-uld`指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="c82f5-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c82f5-478">新增*模型/連絡人 .cs*：</span><span class="sxs-lookup"><span data-stu-id="c82f5-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c82f5-479">Scaffold `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="c82f5-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c82f5-480">建立初始遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c82f5-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="c82f5-481">更新*Pages/_Layout. cshtml*檔案中的**ContactManager**錨點：</span><span class="sxs-lookup"><span data-stu-id="c82f5-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="c82f5-482">藉由建立、編輯和刪除連絡人來測試應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c82f5-483">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="c82f5-483">Seed the database</span></span>

<span data-ttu-id="c82f5-484">將[SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs)類別新增至 [ *Data* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c82f5-484">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="c82f5-485">呼叫 `SeedData.Initialize` 來源 `Main` ：</span><span class="sxs-lookup"><span data-stu-id="c82f5-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="c82f5-486">測試應用程式是否植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c82f5-486">Test that the app seeded the database.</span></span> <span data-ttu-id="c82f5-487">如果 contact DB 中有任何資料列，則不會執行種子方法。</span><span class="sxs-lookup"><span data-stu-id="c82f5-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="c82f5-488">其他資源</span><span class="sxs-lookup"><span data-stu-id="c82f5-488">Additional resources</span></span>

* [<span data-ttu-id="c82f5-489">在 Azure App Service 中建置 .NET Core 和 SQL Database Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c82f5-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="c82f5-490">[ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。</span><span class="sxs-lookup"><span data-stu-id="c82f5-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="c82f5-491">這個實驗室會詳細說明本教學課程中引進的安全性功能。</span><span class="sxs-lookup"><span data-stu-id="c82f5-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="c82f5-492">以自訂原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="c82f5-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
