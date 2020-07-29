---
title: 建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式
author: rick-anderson
description: 瞭解如何使用受授權保護的使用者資料來建立 ASP.NET Core web 應用程式。 包括 HTTPS、驗證、安全性 ASP.NET Core Identity 。
ms.author: riande
ms.date: 7/18/2020
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
ms.openlocfilehash: 7d4c10fa0b1c569179fc3e0a518917ec0185c51f
ms.sourcegitcommit: 1b89fc58114a251926abadfd5c69c120f1ba12d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87160281"
---
# <a name="create-an-aspnet-core-web-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="190fa-104">建立 ASP.NET Core web 應用程式，其中包含由授權保護的使用者資料</span><span class="sxs-lookup"><span data-stu-id="190fa-104">Create an ASP.NET Core web app with user data protected by authorization</span></span>

<span data-ttu-id="190fa-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="190fa-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="190fa-106">請參閱[此 pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="190fa-106">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="190fa-107">本教學課程示範如何建立 ASP.NET Core web 應用程式，其中包含由授權保護的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-107">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="190fa-108">它會顯示已驗證（已註冊）使用者建立的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="190fa-108">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="190fa-109">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="190fa-109">There are three security groups:</span></span>

* <span data-ttu-id="190fa-110">**已註冊的使用者**可以查看所有核准的資料，而且可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-110">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="190fa-111">**管理員**可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-111">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="190fa-112">使用者只能看到核准的連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-112">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="190fa-113">系統**管理員**可以核准/拒絕和編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-113">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="190fa-114">本檔中的影像不完全符合最新的範本。</span><span class="sxs-lookup"><span data-stu-id="190fa-114">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="190fa-115">在下圖中，已登入使用者 Rick （ `rick@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="190fa-115">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="190fa-116">Rick 只能查看已核准的連絡人**Edit**，並 / **Delete** / 針對他的連絡人編輯 [刪除]**建立新**的連結。</span><span class="sxs-lookup"><span data-stu-id="190fa-116">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="190fa-117">只有 [Rick] 所建立的最後一筆記錄會顯示 [**編輯**] 和 [**刪除**] 連結。</span><span class="sxs-lookup"><span data-stu-id="190fa-117">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="190fa-118">在管理員或系統管理員將狀態變更為「已核准」之前，其他使用者將不會看到最後一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="190fa-118">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![顯示 Rick 已登入的螢幕擷取畫面](secure-data/_static/rick.png)

<span data-ttu-id="190fa-120">在下圖中， `manager@contoso.com` 已登入並以管理員的角色執行：</span><span class="sxs-lookup"><span data-stu-id="190fa-120">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![顯示 manager@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/manager1.png)

<span data-ttu-id="190fa-122">下圖顯示連絡人的 [管理員] 詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="190fa-122">The following image shows the managers details view of a contact:</span></span>

![經理的連絡人觀點](secure-data/_static/manager.png)

<span data-ttu-id="190fa-124">[**核准**] 和 [**拒絕**] 按鈕只會針對管理員和系統管理員顯示。</span><span class="sxs-lookup"><span data-stu-id="190fa-124">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="190fa-125">在下圖中， `admin@contoso.com` 已登入，並以系統管理員角色：</span><span class="sxs-lookup"><span data-stu-id="190fa-125">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![顯示 admin@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/admin.png)

<span data-ttu-id="190fa-127">系統管理員具有擁有權限。</span><span class="sxs-lookup"><span data-stu-id="190fa-127">The administrator has all privileges.</span></span> <span data-ttu-id="190fa-128">她可以讀取/編輯/刪除任何連絡人，並變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="190fa-128">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="190fa-129">應用程式[是由下列](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)模型所建立 `Contact` ：</span><span class="sxs-lookup"><span data-stu-id="190fa-129">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="190fa-130">此範例包含下列授權處理常式：</span><span class="sxs-lookup"><span data-stu-id="190fa-130">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="190fa-131">`ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-131">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="190fa-132">`ContactManagerAuthorizationHandler`：允許管理員核准或拒絕連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-132">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="190fa-133">`ContactAdministratorsAuthorizationHandler`：可讓系統管理員核准或拒絕連絡人，以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-133">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="190fa-134">先決條件</span><span class="sxs-lookup"><span data-stu-id="190fa-134">Prerequisites</span></span>

<span data-ttu-id="190fa-135">本教學課程是先進的。</span><span class="sxs-lookup"><span data-stu-id="190fa-135">This tutorial is advanced.</span></span> <span data-ttu-id="190fa-136">您應該已熟悉：</span><span class="sxs-lookup"><span data-stu-id="190fa-136">You should be familiar with:</span></span>

* [<span data-ttu-id="190fa-137">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="190fa-137">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="190fa-138">驗證</span><span class="sxs-lookup"><span data-stu-id="190fa-138">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="190fa-139">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="190fa-139">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="190fa-140">授權</span><span class="sxs-lookup"><span data-stu-id="190fa-140">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="190fa-141">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="190fa-141">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="190fa-142">入門和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-142">The starter and completed app</span></span>

<span data-ttu-id="190fa-143">[下載](xref:index#how-to-download-a-sample)[已完成](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="190fa-143">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="190fa-144">[測試](#test-the-completed-app)已完成的應用程式，以熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="190fa-144">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="190fa-145">入門應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-145">The starter app</span></span>

<span data-ttu-id="190fa-146">[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。</span><span class="sxs-lookup"><span data-stu-id="190fa-146">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="190fa-147">執行應用程式，並按一下 [ **ContactManager** ] 連結，並確認您可以建立、編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-147">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="190fa-148">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="190fa-148">Secure user data</span></span>

<span data-ttu-id="190fa-149">下列各節包含建立安全使用者資料應用程式的所有主要步驟。</span><span class="sxs-lookup"><span data-stu-id="190fa-149">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="190fa-150">您可能會發現參考已完成的專案很有説明。</span><span class="sxs-lookup"><span data-stu-id="190fa-150">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="190fa-151">將連絡人資料與使用者結合</span><span class="sxs-lookup"><span data-stu-id="190fa-151">Tie the contact data to the user</span></span>

<span data-ttu-id="190fa-152">使用 ASP.NET [Identity](xref:security/authentication/identity) 使用者識別碼，以確保使用者可以編輯其資料，而不是其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-152">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="190fa-153">將 `OwnerID` 和新增 `ContactStatus` 至 `Contact` 模型：</span><span class="sxs-lookup"><span data-stu-id="190fa-153">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="190fa-154">`OwnerID`這是 `AspNetUser` 資料庫中資料表的使用者識別碼 [Identity](xref:security/authentication/identity) 。</span><span class="sxs-lookup"><span data-stu-id="190fa-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="190fa-155">`Status`欄位會決定一般使用者是否可以查看連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-155">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="190fa-156">建立新的遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="190fa-156">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-no-locidentity"></a><span data-ttu-id="190fa-157">將角色服務新增至Identity</span><span class="sxs-lookup"><span data-stu-id="190fa-157">Add Role services to Identity</span></span>

<span data-ttu-id="190fa-158">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="190fa-158">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

<a name="rau"></a>

### <a name="require-authenticated-users"></a><span data-ttu-id="190fa-159">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="190fa-159">Require authenticated users</span></span>

<span data-ttu-id="190fa-160">將 [回退驗證原則] 設定為 [要求使用者進行驗證]：</span><span class="sxs-lookup"><span data-stu-id="190fa-160">Set the fallback authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=13-99)]

<span data-ttu-id="190fa-161">上述的反白顯示程式碼會設定回溯[驗證原則](xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.FallbackPolicy)。</span><span class="sxs-lookup"><span data-stu-id="190fa-161">The preceding highlighted code sets the [fallback authentication policy](xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.FallbackPolicy).</span></span> <span data-ttu-id="190fa-162">回溯驗證原則會要求***所有***使用者都必須經過驗證，但 Razor 頁面、控制器或動作方法的驗證屬性除外。</span><span class="sxs-lookup"><span data-stu-id="190fa-162">The fallback authentication policy requires ***all*** users to be authenticated, except for Razor Pages, controllers, or action methods with an authentication attribute.</span></span> <span data-ttu-id="190fa-163">例如， Razor 具有或的頁面、控制器或動作方法，會 `[AllowAnonymous]` 使用套用 `[Authorize(PolicyName="MyPolicy")]` 的驗證屬性，而非 fallback 驗證原則。</span><span class="sxs-lookup"><span data-stu-id="190fa-163">For example, Razor Pages, controllers, or action methods with `[AllowAnonymous]` or `[Authorize(PolicyName="MyPolicy")]` use the applied authentication attribute rather than the fallback authentication policy.</span></span>

<span data-ttu-id="190fa-164">Fallback 驗證原則：</span><span class="sxs-lookup"><span data-stu-id="190fa-164">The fallback authentication policy:</span></span>

* <span data-ttu-id="190fa-165">會套用至未明確指定驗證原則的所有要求。</span><span class="sxs-lookup"><span data-stu-id="190fa-165">Is applied to all requests that do not explicitly specify an authentication policy.</span></span> <span data-ttu-id="190fa-166">針對端點路由所服務的要求，這會包含未指定授權屬性的任何端點。</span><span class="sxs-lookup"><span data-stu-id="190fa-166">For requests served by endpoint routing, this would include any endpoint that does not specify an authorization attribute.</span></span> <span data-ttu-id="190fa-167">對於在授權中介軟體之後由其他中介軟體所提供的要求（例如[靜態](xref:fundamentals/static-files)檔案），這會將原則套用至所有要求。</span><span class="sxs-lookup"><span data-stu-id="190fa-167">For requests served by other middleware after the authorization middleware, such as [static files](xref:fundamentals/static-files), this would apply the policy to all requests.</span></span>

<span data-ttu-id="190fa-168">將 [回溯驗證原則] 設定為 [要求使用者進行驗證] 可保護新新增的 Razor 頁面和控制器。</span><span class="sxs-lookup"><span data-stu-id="190fa-168">Setting the fallback authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="190fa-169">預設需要驗證，比依賴新的控制器和 Razor 頁面來包含屬性更為安全 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-169">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="190fa-170"><xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions>類別也包含 <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.DefaultPolicy?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="190fa-170">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions> class also contains <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.DefaultPolicy?displayProperty=nameWithType>.</span></span> <span data-ttu-id="190fa-171">`DefaultPolicy` `[Authorize]` 當未指定任何原則時，是與屬性搭配使用的原則。</span><span class="sxs-lookup"><span data-stu-id="190fa-171">The `DefaultPolicy` is the policy used with the `[Authorize]` attribute when no policy is specified.</span></span> <span data-ttu-id="190fa-172">`[Authorize]`不包含名稱為的原則，不同于 `[Authorize(PolicyName="MyPolicy")]` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-172">`[Authorize]` doesn't contain a named policy, unlike `[Authorize(PolicyName="MyPolicy")]`.</span></span>

<span data-ttu-id="190fa-173">如需原則的詳細資訊，請參閱 <xref:security/authorization/policies> 。</span><span class="sxs-lookup"><span data-stu-id="190fa-173">For more information on policies, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="190fa-174">若要讓 MVC 控制器和 Razor 頁面要求所有使用者進行驗證，另一種方法是新增授權篩選器：</span><span class="sxs-lookup"><span data-stu-id="190fa-174">An alternative way for MVC controllers and Razor Pages to require all users be authenticated is adding an authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup2.cs?name=snippet&highlight=14-99)]

<span data-ttu-id="190fa-175">上述程式碼會使用授權篩選，設定回溯原則會使用端點路由。</span><span class="sxs-lookup"><span data-stu-id="190fa-175">The preceding code uses an authorization filter, setting the fallback policy uses endpoint routing.</span></span> <span data-ttu-id="190fa-176">設定回退原則是要求所有使用者都必須經過驗證的慣用方式。</span><span class="sxs-lookup"><span data-stu-id="190fa-176">Setting the fallback policy is the preferred way to require all users be authenticated.</span></span>

<span data-ttu-id="190fa-177">將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 `Index` 和 `Privacy` 頁面，讓匿名使用者可以在註冊之前取得網站的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="190fa-177">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the `Index` and `Privacy` pages so anonymous users can get information about the site before they register:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="190fa-178">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="190fa-178">Configure the test account</span></span>

<span data-ttu-id="190fa-179">`SeedData`類別會建立兩個帳戶：系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="190fa-179">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="190fa-180">使用[秘密管理員工具](xref:security/app-secrets)來設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="190fa-180">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="190fa-181">從專案目錄（包含*Program.cs*的目錄）設定密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-181">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="190fa-182">如果未指定強式密碼，則會在呼叫時擲回例外狀況 `SeedData.Initialize` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-182">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="190fa-183">更新 `Main` 以使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-183">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="190fa-184">建立測試帳戶並更新連絡人</span><span class="sxs-lookup"><span data-stu-id="190fa-184">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="190fa-185">更新 `Initialize` 類別中的方法 `SeedData` ，以建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="190fa-185">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="190fa-186">將系統管理員使用者識別碼和新增 `ContactStatus` 至連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-186">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="190fa-187">讓其中一個連絡人「已提交」和一個「已拒絕」。</span><span class="sxs-lookup"><span data-stu-id="190fa-187">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="190fa-188">將 [使用者識別碼] 和 [狀態] 新增至所有連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-188">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="190fa-189">只會顯示一個連絡人：</span><span class="sxs-lookup"><span data-stu-id="190fa-189">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="190fa-190">建立擁有者、管理員和系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-190">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="190fa-191">`ContactIsOwnerAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-191">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="190fa-192">`ContactIsOwnerAuthorizationHandler`會驗證對資源進行的使用者擁有資源。</span><span class="sxs-lookup"><span data-stu-id="190fa-192">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="190fa-193">`ContactIsOwnerAuthorizationHandler`呼叫[內容。](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人擁有者，則會成功。</span><span class="sxs-lookup"><span data-stu-id="190fa-193">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="190fa-194">授權處理常式一般：</span><span class="sxs-lookup"><span data-stu-id="190fa-194">Authorization handlers generally:</span></span>

* <span data-ttu-id="190fa-195">`context.Succeed`符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="190fa-195">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="190fa-196">`Task.CompletedTask`當不符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="190fa-196">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="190fa-197">`Task.CompletedTask`不是成功或失敗， &mdash; 它允許其他授權處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="190fa-197">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="190fa-198">如果您需要明確失敗，請傳回[coNtext。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="190fa-198">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="190fa-199">應用程式可讓連絡人擁有者編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-199">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="190fa-200">`ContactIsOwnerAuthorizationHandler`不需要檢查在需求參數中傳遞的作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-200">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="190fa-201">建立管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-201">Create a manager authorization handler</span></span>

<span data-ttu-id="190fa-202">`ContactManagerAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-202">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="190fa-203">`ContactManagerAuthorizationHandler`會驗證對資源的使用者是否為管理員。</span><span class="sxs-lookup"><span data-stu-id="190fa-203">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="190fa-204">只有管理員可以核准或拒絕內容變更（新的或已變更）。</span><span class="sxs-lookup"><span data-stu-id="190fa-204">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="190fa-205">建立系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-205">Create an administrator authorization handler</span></span>

<span data-ttu-id="190fa-206">`ContactAdministratorsAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-206">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="190fa-207">`ContactAdministratorsAuthorizationHandler`會驗證對資源的使用者是否為系統管理員。</span><span class="sxs-lookup"><span data-stu-id="190fa-207">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="190fa-208">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-208">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="190fa-209">註冊授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-209">Register the authorization handlers</span></span>

<span data-ttu-id="190fa-210">使用 Entity Framework Core 的服務必須使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)註冊相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="190fa-210">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="190fa-211">`ContactIsOwnerAuthorizationHandler` [Identity](xref:security/authentication/identity) 會使用以 Entity Framework Core 為基礎的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="190fa-211">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="190fa-212">向服務集合註冊處理常式，以便透過相依性插入來使用它們 `ContactsController` 。 [dependency injection](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="190fa-212">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="190fa-213">在結尾新增下列程式碼 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="190fa-213">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="190fa-214">`ContactAdministratorsAuthorizationHandler`和 `ContactManagerAuthorizationHandler` 會新增為單次個體。</span><span class="sxs-lookup"><span data-stu-id="190fa-214">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="190fa-215">它們是單次個體的，因為它們不使用 EF，而所需的所有資訊都是在 `Context` 方法的參數中 `HandleRequirementAsync` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-215">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="190fa-216">支援授權</span><span class="sxs-lookup"><span data-stu-id="190fa-216">Support authorization</span></span>

<span data-ttu-id="190fa-217">在本節中，您會更新 Razor 頁面並加入作業需求類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-217">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="190fa-218">審查連絡人作業需求類別</span><span class="sxs-lookup"><span data-stu-id="190fa-218">Review the contact operations requirements class</span></span>

<span data-ttu-id="190fa-219">檢查 `ContactOperations` 類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-219">Review the `ContactOperations` class.</span></span> <span data-ttu-id="190fa-220">此類別包含應用程式支援的需求：</span><span class="sxs-lookup"><span data-stu-id="190fa-220">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-no-locrazor-pages"></a><span data-ttu-id="190fa-221">建立 [連絡人] 頁面的基類 Razor</span><span class="sxs-lookup"><span data-stu-id="190fa-221">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="190fa-222">建立基類，其中包含 [連絡人] 頁面中使用的服務 Razor 。</span><span class="sxs-lookup"><span data-stu-id="190fa-222">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="190fa-223">基底類別會將初始化程式碼放在一個位置：</span><span class="sxs-lookup"><span data-stu-id="190fa-223">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="190fa-224">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-224">The preceding code:</span></span>

* <span data-ttu-id="190fa-225">新增 `IAuthorizationService` 服務以存取授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="190fa-225">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="190fa-226">新增 Identity `UserManager` 服務。</span><span class="sxs-lookup"><span data-stu-id="190fa-226">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="190fa-227">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="190fa-227">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="190fa-228">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="190fa-228">Update the CreateModel</span></span>

<span data-ttu-id="190fa-229">更新 [建立] 頁面模型的函式，以使用 `DI_BasePageModel` 基類：</span><span class="sxs-lookup"><span data-stu-id="190fa-229">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="190fa-230">將 `CreateModel.OnPostAsync` 方法更新為：</span><span class="sxs-lookup"><span data-stu-id="190fa-230">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="190fa-231">將使用者識別碼新增至 `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="190fa-231">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="190fa-232">呼叫授權處理常式，以確認使用者擁有建立連絡人的許可權。</span><span class="sxs-lookup"><span data-stu-id="190fa-232">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="190fa-233">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="190fa-233">Update the IndexModel</span></span>

<span data-ttu-id="190fa-234">更新 `OnGetAsync` 方法，讓一般使用者只會看到已核准的連絡人：</span><span class="sxs-lookup"><span data-stu-id="190fa-234">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="190fa-235">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="190fa-235">Update the EditModel</span></span>

<span data-ttu-id="190fa-236">新增授權處理常式，以確認使用者擁有該連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-236">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="190fa-237">因為正在驗證資源授權，所以 `[Authorize]` 屬性不夠。</span><span class="sxs-lookup"><span data-stu-id="190fa-237">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="190fa-238">評估屬性時，應用程式沒有資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="190fa-238">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="190fa-239">以資源為基礎的授權必須是必要的。</span><span class="sxs-lookup"><span data-stu-id="190fa-239">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="190fa-240">一旦應用程式可存取資源，就必須執行檢查，方法是將它載入頁面模型中，或在處理常式本身內載入。</span><span class="sxs-lookup"><span data-stu-id="190fa-240">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="190fa-241">您經常會藉由傳入資源金鑰來存取資源。</span><span class="sxs-lookup"><span data-stu-id="190fa-241">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="190fa-242">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="190fa-242">Update the DeleteModel</span></span>

<span data-ttu-id="190fa-243">更新 [刪除] 頁面模型，以使用授權處理常式來驗證使用者是否擁有連絡人的 [刪除] 許可權。</span><span class="sxs-lookup"><span data-stu-id="190fa-243">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="190fa-244">將授權服務插入至 views</span><span class="sxs-lookup"><span data-stu-id="190fa-244">Inject the authorization service into the views</span></span>

<span data-ttu-id="190fa-245">目前，UI 會顯示使用者無法修改之連絡人的 [編輯] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="190fa-245">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="190fa-246">在*Pages/_ViewImports. cshtml*檔案中插入授權服務，以供所有視圖使用：</span><span class="sxs-lookup"><span data-stu-id="190fa-246">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="190fa-247">上述標記會加入數個 `using` 語句。</span><span class="sxs-lookup"><span data-stu-id="190fa-247">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="190fa-248">更新*Pages/Contacts/Index*中的 [**編輯**] 和 [**刪除**] 連結，使其僅針對具有適當許可權的使用者呈現：</span><span class="sxs-lookup"><span data-stu-id="190fa-248">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="190fa-249">隱藏不具有變更資料許可權之使用者的連結，並不會保護應用程式的安全。</span><span class="sxs-lookup"><span data-stu-id="190fa-249">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="190fa-250">隱藏連結可只顯示有效的連結，讓應用程式更容易使用。</span><span class="sxs-lookup"><span data-stu-id="190fa-250">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="190fa-251">使用者可以攻擊產生的 Url，以叫用其未擁有之資料的編輯和刪除作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-251">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="190fa-252">Razor頁面或控制器必須強制執行存取檢查，以保護資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-252">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="190fa-253">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="190fa-253">Update Details</span></span>

<span data-ttu-id="190fa-254">更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="190fa-254">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="190fa-255">更新 [詳細資料] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="190fa-255">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="190fa-256">新增或移除角色的使用者</span><span class="sxs-lookup"><span data-stu-id="190fa-256">Add or remove a user to a role</span></span>

<span data-ttu-id="190fa-257">如需相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8502)：</span><span class="sxs-lookup"><span data-stu-id="190fa-257">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="190fa-258">移除使用者的許可權。</span><span class="sxs-lookup"><span data-stu-id="190fa-258">Removing privileges from a user.</span></span> <span data-ttu-id="190fa-259">例如，將聊天應用程式中的使用者靜音。</span><span class="sxs-lookup"><span data-stu-id="190fa-259">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="190fa-260">將許可權新增至使用者。</span><span class="sxs-lookup"><span data-stu-id="190fa-260">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="190fa-261">挑戰與禁止之間的差異</span><span class="sxs-lookup"><span data-stu-id="190fa-261">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="190fa-262">此應用程式會將預設原則設定為[要求已驗證的使用者](#rau)。</span><span class="sxs-lookup"><span data-stu-id="190fa-262">This app sets the default policy to [require authenticated users](#rau).</span></span> <span data-ttu-id="190fa-263">下列程式碼允許匿名使用者。</span><span class="sxs-lookup"><span data-stu-id="190fa-263">The following code allows anonymous users.</span></span> <span data-ttu-id="190fa-264">允許匿名使用者顯示挑戰與禁止之間的差異。</span><span class="sxs-lookup"><span data-stu-id="190fa-264">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="190fa-265">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="190fa-265">In the preceding code:</span></span>

* <span data-ttu-id="190fa-266">當使用者**未**經過驗證時， `ChallengeResult` 會傳回。</span><span class="sxs-lookup"><span data-stu-id="190fa-266">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="190fa-267">當傳回時，會將 `ChallengeResult` 使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="190fa-267">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="190fa-268">當使用者經過驗證但未獲授權時， `ForbidResult` 會傳回。</span><span class="sxs-lookup"><span data-stu-id="190fa-268">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="190fa-269">當 `ForbidResult` 傳回時，使用者會被重新導向至 [拒絕存取] 頁面。</span><span class="sxs-lookup"><span data-stu-id="190fa-269">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="190fa-270">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-270">Test the completed app</span></span>

<span data-ttu-id="190fa-271">如果您尚未設定已植入之使用者帳戶的密碼，請使用[秘密管理員工具](xref:security/app-secrets#secret-manager)來設定密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-271">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="190fa-272">選擇強式密碼：使用八個或更多個字元，且至少要有一個大寫字元、數位和符號。</span><span class="sxs-lookup"><span data-stu-id="190fa-272">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="190fa-273">例如， `Passw0rd!` 符合強式密碼需求。</span><span class="sxs-lookup"><span data-stu-id="190fa-273">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="190fa-274">從專案的資料夾執行下列命令，其中 `<PW>` 是密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-274">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="190fa-275">如果應用程式有連絡人：</span><span class="sxs-lookup"><span data-stu-id="190fa-275">If the app has contacts:</span></span>

* <span data-ttu-id="190fa-276">刪除資料表中的所有記錄 `Contact` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-276">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="190fa-277">重新開機應用程式以植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="190fa-277">Restart the app to seed the database.</span></span>

<span data-ttu-id="190fa-278">測試已完成之應用程式的簡單方法，是啟動三個不同的瀏覽器（或 incognito/InPrivate 會話）。</span><span class="sxs-lookup"><span data-stu-id="190fa-278">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="190fa-279">在一個瀏覽器中，註冊新的使用者（例如 `test@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="190fa-279">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="190fa-280">使用不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="190fa-280">Sign in to each browser with a different user.</span></span> <span data-ttu-id="190fa-281">確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="190fa-281">Verify the following operations:</span></span>

* <span data-ttu-id="190fa-282">已註冊的使用者可以查看所有已核准的連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-282">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="190fa-283">已註冊的使用者可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-283">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="190fa-284">管理員可以核准/拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-284">Managers can approve/reject contact data.</span></span> <span data-ttu-id="190fa-285">此 `Details` 視圖會顯示 [**核准**] 和 [**拒絕**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="190fa-285">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="190fa-286">系統管理員可以核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-286">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="190fa-287">User</span><span class="sxs-lookup"><span data-stu-id="190fa-287">User</span></span>                | <span data-ttu-id="190fa-288">由應用程式植入</span><span class="sxs-lookup"><span data-stu-id="190fa-288">Seeded by the app</span></span> | <span data-ttu-id="190fa-289">選項。</span><span class="sxs-lookup"><span data-stu-id="190fa-289">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="190fa-290">否</span><span class="sxs-lookup"><span data-stu-id="190fa-290">No</span></span>                | <span data-ttu-id="190fa-291">編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-291">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="190fa-292">是</span><span class="sxs-lookup"><span data-stu-id="190fa-292">Yes</span></span>               | <span data-ttu-id="190fa-293">核准/拒絕和編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-293">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="190fa-294">是</span><span class="sxs-lookup"><span data-stu-id="190fa-294">Yes</span></span>               | <span data-ttu-id="190fa-295">核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-295">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="190fa-296">在系統管理員的瀏覽器中建立連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-296">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="190fa-297">從系統管理員連絡人中複製 [刪除] 和 [編輯] 的 URL。</span><span class="sxs-lookup"><span data-stu-id="190fa-297">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="190fa-298">將這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-298">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="190fa-299">建立入門應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-299">Create the starter app</span></span>

* <span data-ttu-id="190fa-300">建立 Razor 名為 "ContactManager" 的頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-300">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="190fa-301">建立具有**個別使用者帳戶**的應用程式。</span><span class="sxs-lookup"><span data-stu-id="190fa-301">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="190fa-302">將它命名為 "ContactManager"，讓命名空間符合範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="190fa-302">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="190fa-303">`-uld`指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="190fa-303">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="190fa-304">新增*模型/連絡人 .cs*：</span><span class="sxs-lookup"><span data-stu-id="190fa-304">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="190fa-305">Scaffold `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="190fa-305">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="190fa-306">建立初始遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="190fa-306">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="190fa-307">如果您遇到命令的錯誤 `dotnet aspnet-codegenerator razorpage` ，請參閱[此 GitHub 問題](https://github.com/aspnet/Scaffolding/issues/984)。</span><span class="sxs-lookup"><span data-stu-id="190fa-307">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="190fa-308">更新*Pages/Shared/_Layout. cshtml*檔案中的**ContactManager**錨點：</span><span class="sxs-lookup"><span data-stu-id="190fa-308">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="190fa-309">藉由建立、編輯和刪除連絡人來測試應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-309">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="190fa-310">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="190fa-310">Seed the database</span></span>

<span data-ttu-id="190fa-311">將[SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs)類別新增至 [*資料*] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="190fa-311">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="190fa-312">呼叫 `SeedData.Initialize` 來源 `Main` ：</span><span class="sxs-lookup"><span data-stu-id="190fa-312">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="190fa-313">測試應用程式是否植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="190fa-313">Test that the app seeded the database.</span></span> <span data-ttu-id="190fa-314">如果 contact DB 中有任何資料列，則不會執行種子方法。</span><span class="sxs-lookup"><span data-stu-id="190fa-314">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="190fa-315">本教學課程示範如何建立 ASP.NET Core web 應用程式，其中包含由授權保護的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-315">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="190fa-316">它會顯示已驗證（已註冊）使用者建立的連絡人清單。</span><span class="sxs-lookup"><span data-stu-id="190fa-316">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="190fa-317">有三個安全性群組：</span><span class="sxs-lookup"><span data-stu-id="190fa-317">There are three security groups:</span></span>

* <span data-ttu-id="190fa-318">**已註冊的使用者**可以查看所有核准的資料，而且可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-318">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="190fa-319">**管理員**可以核准或拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-319">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="190fa-320">使用者只能看到核准的連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-320">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="190fa-321">系統**管理員**可以核准/拒絕和編輯/刪除任何資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-321">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="190fa-322">在下圖中，已登入使用者 Rick （ `rick@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="190fa-322">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="190fa-323">Rick 只能查看已核准的連絡人**Edit**，並 / **Delete** / 針對他的連絡人編輯 [刪除]**建立新**的連結。</span><span class="sxs-lookup"><span data-stu-id="190fa-323">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="190fa-324">只有 [Rick] 所建立的最後一筆記錄會顯示 [**編輯**] 和 [**刪除**] 連結。</span><span class="sxs-lookup"><span data-stu-id="190fa-324">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="190fa-325">在管理員或系統管理員將狀態變更為「已核准」之前，其他使用者將不會看到最後一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="190fa-325">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![顯示 Rick 已登入的螢幕擷取畫面](secure-data/_static/rick.png)

<span data-ttu-id="190fa-327">在下圖中， `manager@contoso.com` 已登入並以管理員的角色執行：</span><span class="sxs-lookup"><span data-stu-id="190fa-327">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![顯示 manager@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/manager1.png)

<span data-ttu-id="190fa-329">下圖顯示連絡人的 [管理員] 詳細資料檢視：</span><span class="sxs-lookup"><span data-stu-id="190fa-329">The following image shows the managers details view of a contact:</span></span>

![經理的連絡人觀點](secure-data/_static/manager.png)

<span data-ttu-id="190fa-331">[**核准**] 和 [**拒絕**] 按鈕只會針對管理員和系統管理員顯示。</span><span class="sxs-lookup"><span data-stu-id="190fa-331">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="190fa-332">在下圖中， `admin@contoso.com` 已登入，並以系統管理員角色：</span><span class="sxs-lookup"><span data-stu-id="190fa-332">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![顯示 admin@contoso.com 已登入的螢幕擷取畫面](secure-data/_static/admin.png)

<span data-ttu-id="190fa-334">系統管理員具有擁有權限。</span><span class="sxs-lookup"><span data-stu-id="190fa-334">The administrator has all privileges.</span></span> <span data-ttu-id="190fa-335">她可以讀取/編輯/刪除任何連絡人，並變更連絡人的狀態。</span><span class="sxs-lookup"><span data-stu-id="190fa-335">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="190fa-336">應用程式[是由下列](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)模型所建立 `Contact` ：</span><span class="sxs-lookup"><span data-stu-id="190fa-336">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="190fa-337">此範例包含下列授權處理常式：</span><span class="sxs-lookup"><span data-stu-id="190fa-337">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="190fa-338">`ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-338">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="190fa-339">`ContactManagerAuthorizationHandler`：允許管理員核准或拒絕連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-339">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="190fa-340">`ContactAdministratorsAuthorizationHandler`：可讓系統管理員核准或拒絕連絡人，以及編輯/刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-340">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="190fa-341">先決條件</span><span class="sxs-lookup"><span data-stu-id="190fa-341">Prerequisites</span></span>

<span data-ttu-id="190fa-342">本教學課程是先進的。</span><span class="sxs-lookup"><span data-stu-id="190fa-342">This tutorial is advanced.</span></span> <span data-ttu-id="190fa-343">您應該已熟悉：</span><span class="sxs-lookup"><span data-stu-id="190fa-343">You should be familiar with:</span></span>

* [<span data-ttu-id="190fa-344">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="190fa-344">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="190fa-345">驗證</span><span class="sxs-lookup"><span data-stu-id="190fa-345">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="190fa-346">帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="190fa-346">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="190fa-347">授權</span><span class="sxs-lookup"><span data-stu-id="190fa-347">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="190fa-348">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="190fa-348">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="190fa-349">入門和已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-349">The starter and completed app</span></span>

<span data-ttu-id="190fa-350">[下載](xref:index#how-to-download-a-sample)[已完成](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="190fa-350">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="190fa-351">[測試](#test-the-completed-app)已完成的應用程式，以熟悉其安全性功能。</span><span class="sxs-lookup"><span data-stu-id="190fa-351">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="190fa-352">入門應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-352">The starter app</span></span>

<span data-ttu-id="190fa-353">[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。</span><span class="sxs-lookup"><span data-stu-id="190fa-353">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="190fa-354">執行應用程式，並按一下 [ **ContactManager** ] 連結，並確認您可以建立、編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-354">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="190fa-355">保護使用者資料</span><span class="sxs-lookup"><span data-stu-id="190fa-355">Secure user data</span></span>

<span data-ttu-id="190fa-356">下列各節包含建立安全使用者資料應用程式的所有主要步驟。</span><span class="sxs-lookup"><span data-stu-id="190fa-356">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="190fa-357">您可能會發現參考已完成的專案很有説明。</span><span class="sxs-lookup"><span data-stu-id="190fa-357">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="190fa-358">將連絡人資料與使用者結合</span><span class="sxs-lookup"><span data-stu-id="190fa-358">Tie the contact data to the user</span></span>

<span data-ttu-id="190fa-359">使用 ASP.NET [Identity](xref:security/authentication/identity) 使用者識別碼，以確保使用者可以編輯其資料，而不是其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-359">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="190fa-360">將 `OwnerID` 和新增 `ContactStatus` 至 `Contact` 模型：</span><span class="sxs-lookup"><span data-stu-id="190fa-360">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="190fa-361">`OwnerID`這是 `AspNetUser` 資料庫中資料表的使用者識別碼 [Identity](xref:security/authentication/identity) 。</span><span class="sxs-lookup"><span data-stu-id="190fa-361">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="190fa-362">`Status`欄位會決定一般使用者是否可以查看連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-362">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="190fa-363">建立新的遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="190fa-363">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-no-locidentity"></a><span data-ttu-id="190fa-364">將角色服務新增至Identity</span><span class="sxs-lookup"><span data-stu-id="190fa-364">Add Role services to Identity</span></span>

<span data-ttu-id="190fa-365">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="190fa-365">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=11)]

### <a name="require-authenticated-users"></a><span data-ttu-id="190fa-366">需要已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="190fa-366">Require authenticated users</span></span>

<span data-ttu-id="190fa-367">將 [預設驗證原則] 設定為 [要求使用者進行驗證]：</span><span class="sxs-lookup"><span data-stu-id="190fa-367">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="190fa-368">您可以 Razor 使用屬性，在頁面、控制器或動作方法層級選擇不進行驗證 `[AllowAnonymous]` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-368">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="190fa-369">將預設驗證原則設定為 [要求使用者進行驗證] 可保護新新增的 Razor 頁面和控制器。</span><span class="sxs-lookup"><span data-stu-id="190fa-369">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="190fa-370">預設需要驗證，比依賴新的控制器和 Razor 頁面來包含屬性更為安全 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-370">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="190fa-371">將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 [索引]、[關於] 和 [連絡人] 頁面，讓匿名使用者可以在註冊之前取得網站的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="190fa-371">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="190fa-372">設定測試帳戶</span><span class="sxs-lookup"><span data-stu-id="190fa-372">Configure the test account</span></span>

<span data-ttu-id="190fa-373">`SeedData`類別會建立兩個帳戶：系統管理員和管理員。</span><span class="sxs-lookup"><span data-stu-id="190fa-373">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="190fa-374">使用[秘密管理員工具](xref:security/app-secrets)來設定這些帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="190fa-374">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="190fa-375">從專案目錄（包含*Program.cs*的目錄）設定密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-375">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="190fa-376">如果未指定強式密碼，則會在呼叫時擲回例外狀況 `SeedData.Initialize` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-376">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="190fa-377">更新 `Main` 以使用測試密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-377">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="190fa-378">建立測試帳戶並更新連絡人</span><span class="sxs-lookup"><span data-stu-id="190fa-378">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="190fa-379">更新 `Initialize` 類別中的方法 `SeedData` ，以建立測試帳戶：</span><span class="sxs-lookup"><span data-stu-id="190fa-379">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="190fa-380">將系統管理員使用者識別碼和新增 `ContactStatus` 至連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-380">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="190fa-381">讓其中一個連絡人「已提交」和一個「已拒絕」。</span><span class="sxs-lookup"><span data-stu-id="190fa-381">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="190fa-382">將 [使用者識別碼] 和 [狀態] 新增至所有連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-382">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="190fa-383">只會顯示一個連絡人：</span><span class="sxs-lookup"><span data-stu-id="190fa-383">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="190fa-384">建立擁有者、管理員和系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-384">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="190fa-385">建立*授權*資料夾，並 `ContactIsOwnerAuthorizationHandler` 在其中建立類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-385">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="190fa-386">`ContactIsOwnerAuthorizationHandler`會驗證對資源進行的使用者擁有資源。</span><span class="sxs-lookup"><span data-stu-id="190fa-386">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="190fa-387">`ContactIsOwnerAuthorizationHandler`呼叫[內容。](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人擁有者，則會成功。</span><span class="sxs-lookup"><span data-stu-id="190fa-387">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="190fa-388">授權處理常式一般：</span><span class="sxs-lookup"><span data-stu-id="190fa-388">Authorization handlers generally:</span></span>

* <span data-ttu-id="190fa-389">`context.Succeed`符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="190fa-389">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="190fa-390">`Task.CompletedTask`當不符合需求時傳回。</span><span class="sxs-lookup"><span data-stu-id="190fa-390">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="190fa-391">`Task.CompletedTask`不是成功或失敗， &mdash; 它允許其他授權處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="190fa-391">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="190fa-392">如果您需要明確失敗，請傳回[coNtext。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。</span><span class="sxs-lookup"><span data-stu-id="190fa-392">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="190fa-393">應用程式可讓連絡人擁有者編輯/刪除/建立自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-393">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="190fa-394">`ContactIsOwnerAuthorizationHandler`不需要檢查在需求參數中傳遞的作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-394">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="190fa-395">建立管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-395">Create a manager authorization handler</span></span>

<span data-ttu-id="190fa-396">`ContactManagerAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-396">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="190fa-397">`ContactManagerAuthorizationHandler`會驗證對資源的使用者是否為管理員。</span><span class="sxs-lookup"><span data-stu-id="190fa-397">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="190fa-398">只有管理員可以核准或拒絕內容變更（新的或已變更）。</span><span class="sxs-lookup"><span data-stu-id="190fa-398">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="190fa-399">建立系統管理員授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-399">Create an administrator authorization handler</span></span>

<span data-ttu-id="190fa-400">`ContactAdministratorsAuthorizationHandler`在 [*授權*] 資料夾中建立類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-400">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="190fa-401">`ContactAdministratorsAuthorizationHandler`會驗證對資源的使用者是否為系統管理員。</span><span class="sxs-lookup"><span data-stu-id="190fa-401">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="190fa-402">系統管理員可以執行所有作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-402">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="190fa-403">註冊授權處理常式</span><span class="sxs-lookup"><span data-stu-id="190fa-403">Register the authorization handlers</span></span>

<span data-ttu-id="190fa-404">使用 Entity Framework Core 的服務必須使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)註冊相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="190fa-404">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="190fa-405">`ContactIsOwnerAuthorizationHandler` [Identity](xref:security/authentication/identity) 會使用以 Entity Framework Core 為基礎的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="190fa-405">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="190fa-406">向服務集合註冊處理常式，以便透過相依性插入來使用它們 `ContactsController` 。 [dependency injection](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="190fa-406">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="190fa-407">在結尾新增下列程式碼 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="190fa-407">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="190fa-408">`ContactAdministratorsAuthorizationHandler`和 `ContactManagerAuthorizationHandler` 會新增為單次個體。</span><span class="sxs-lookup"><span data-stu-id="190fa-408">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="190fa-409">它們是單次個體的，因為它們不使用 EF，而所需的所有資訊都是在 `Context` 方法的參數中 `HandleRequirementAsync` 。</span><span class="sxs-lookup"><span data-stu-id="190fa-409">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="190fa-410">支援授權</span><span class="sxs-lookup"><span data-stu-id="190fa-410">Support authorization</span></span>

<span data-ttu-id="190fa-411">在本節中，您會更新 Razor 頁面並加入作業需求類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-411">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="190fa-412">審查連絡人作業需求類別</span><span class="sxs-lookup"><span data-stu-id="190fa-412">Review the contact operations requirements class</span></span>

<span data-ttu-id="190fa-413">檢查 `ContactOperations` 類別。</span><span class="sxs-lookup"><span data-stu-id="190fa-413">Review the `ContactOperations` class.</span></span> <span data-ttu-id="190fa-414">此類別包含應用程式支援的需求：</span><span class="sxs-lookup"><span data-stu-id="190fa-414">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-no-locrazor-pages"></a><span data-ttu-id="190fa-415">建立 [連絡人] 頁面的基類 Razor</span><span class="sxs-lookup"><span data-stu-id="190fa-415">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="190fa-416">建立基類，其中包含 [連絡人] 頁面中使用的服務 Razor 。</span><span class="sxs-lookup"><span data-stu-id="190fa-416">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="190fa-417">基底類別會將初始化程式碼放在一個位置：</span><span class="sxs-lookup"><span data-stu-id="190fa-417">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="190fa-418">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-418">The preceding code:</span></span>

* <span data-ttu-id="190fa-419">新增 `IAuthorizationService` 服務以存取授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="190fa-419">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="190fa-420">新增 Identity `UserManager` 服務。</span><span class="sxs-lookup"><span data-stu-id="190fa-420">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="190fa-421">加入 `ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="190fa-421">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="190fa-422">更新 CreateModel</span><span class="sxs-lookup"><span data-stu-id="190fa-422">Update the CreateModel</span></span>

<span data-ttu-id="190fa-423">更新 [建立] 頁面模型的函式，以使用 `DI_BasePageModel` 基類：</span><span class="sxs-lookup"><span data-stu-id="190fa-423">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="190fa-424">將 `CreateModel.OnPostAsync` 方法更新為：</span><span class="sxs-lookup"><span data-stu-id="190fa-424">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="190fa-425">將使用者識別碼新增至 `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="190fa-425">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="190fa-426">呼叫授權處理常式，以確認使用者擁有建立連絡人的許可權。</span><span class="sxs-lookup"><span data-stu-id="190fa-426">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="190fa-427">更新 IndexModel</span><span class="sxs-lookup"><span data-stu-id="190fa-427">Update the IndexModel</span></span>

<span data-ttu-id="190fa-428">更新 `OnGetAsync` 方法，讓一般使用者只會看到已核准的連絡人：</span><span class="sxs-lookup"><span data-stu-id="190fa-428">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="190fa-429">更新 EditModel</span><span class="sxs-lookup"><span data-stu-id="190fa-429">Update the EditModel</span></span>

<span data-ttu-id="190fa-430">新增授權處理常式，以確認使用者擁有該連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-430">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="190fa-431">因為正在驗證資源授權，所以 `[Authorize]` 屬性不夠。</span><span class="sxs-lookup"><span data-stu-id="190fa-431">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="190fa-432">評估屬性時，應用程式沒有資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="190fa-432">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="190fa-433">以資源為基礎的授權必須是必要的。</span><span class="sxs-lookup"><span data-stu-id="190fa-433">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="190fa-434">一旦應用程式可存取資源，就必須執行檢查，方法是將它載入頁面模型中，或在處理常式本身內載入。</span><span class="sxs-lookup"><span data-stu-id="190fa-434">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="190fa-435">您經常會藉由傳入資源金鑰來存取資源。</span><span class="sxs-lookup"><span data-stu-id="190fa-435">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="190fa-436">更新 DeleteModel</span><span class="sxs-lookup"><span data-stu-id="190fa-436">Update the DeleteModel</span></span>

<span data-ttu-id="190fa-437">更新 [刪除] 頁面模型，以使用授權處理常式來驗證使用者是否擁有連絡人的 [刪除] 許可權。</span><span class="sxs-lookup"><span data-stu-id="190fa-437">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="190fa-438">將授權服務插入至 views</span><span class="sxs-lookup"><span data-stu-id="190fa-438">Inject the authorization service into the views</span></span>

<span data-ttu-id="190fa-439">目前，UI 會顯示使用者無法修改之連絡人的 [編輯] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="190fa-439">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="190fa-440">將授權服務插入*views/_ViewImports. cshtml*檔案中，以供所有視圖使用：</span><span class="sxs-lookup"><span data-stu-id="190fa-440">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="190fa-441">上述標記會加入數個 `using` 語句。</span><span class="sxs-lookup"><span data-stu-id="190fa-441">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="190fa-442">更新*Pages/Contacts/Index*中的 [**編輯**] 和 [**刪除**] 連結，使其僅針對具有適當許可權的使用者呈現：</span><span class="sxs-lookup"><span data-stu-id="190fa-442">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="190fa-443">隱藏不具有變更資料許可權之使用者的連結，並不會保護應用程式的安全。</span><span class="sxs-lookup"><span data-stu-id="190fa-443">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="190fa-444">隱藏連結可只顯示有效的連結，讓應用程式更容易使用。</span><span class="sxs-lookup"><span data-stu-id="190fa-444">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="190fa-445">使用者可以攻擊產生的 Url，以叫用其未擁有之資料的編輯和刪除作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-445">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="190fa-446">Razor頁面或控制器必須強制執行存取檢查，以保護資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-446">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="190fa-447">更新詳細資料</span><span class="sxs-lookup"><span data-stu-id="190fa-447">Update Details</span></span>

<span data-ttu-id="190fa-448">更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：</span><span class="sxs-lookup"><span data-stu-id="190fa-448">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="190fa-449">更新 [詳細資料] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="190fa-449">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="190fa-450">新增或移除角色的使用者</span><span class="sxs-lookup"><span data-stu-id="190fa-450">Add or remove a user to a role</span></span>

<span data-ttu-id="190fa-451">如需相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8502)：</span><span class="sxs-lookup"><span data-stu-id="190fa-451">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="190fa-452">移除使用者的許可權。</span><span class="sxs-lookup"><span data-stu-id="190fa-452">Removing privileges from a user.</span></span> <span data-ttu-id="190fa-453">例如，將聊天應用程式中的使用者靜音。</span><span class="sxs-lookup"><span data-stu-id="190fa-453">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="190fa-454">將許可權新增至使用者。</span><span class="sxs-lookup"><span data-stu-id="190fa-454">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="190fa-455">測試已完成的應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-455">Test the completed app</span></span>

<span data-ttu-id="190fa-456">如果您尚未設定已植入之使用者帳戶的密碼，請使用[秘密管理員工具](xref:security/app-secrets#secret-manager)來設定密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-456">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="190fa-457">選擇強式密碼：使用八個或更多個字元，且至少要有一個大寫字元、數位和符號。</span><span class="sxs-lookup"><span data-stu-id="190fa-457">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="190fa-458">例如， `Passw0rd!` 符合強式密碼需求。</span><span class="sxs-lookup"><span data-stu-id="190fa-458">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="190fa-459">從專案的資料夾執行下列命令，其中 `<PW>` 是密碼：</span><span class="sxs-lookup"><span data-stu-id="190fa-459">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="190fa-460">卸載並更新資料庫</span><span class="sxs-lookup"><span data-stu-id="190fa-460">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="190fa-461">重新開機應用程式以植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="190fa-461">Restart the app to seed the database.</span></span>

<span data-ttu-id="190fa-462">測試已完成之應用程式的簡單方法，是啟動三個不同的瀏覽器（或 incognito/InPrivate 會話）。</span><span class="sxs-lookup"><span data-stu-id="190fa-462">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="190fa-463">在一個瀏覽器中，註冊新的使用者（例如 `test@example.com` ）。</span><span class="sxs-lookup"><span data-stu-id="190fa-463">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="190fa-464">使用不同的使用者登入每個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="190fa-464">Sign in to each browser with a different user.</span></span> <span data-ttu-id="190fa-465">確認下列作業：</span><span class="sxs-lookup"><span data-stu-id="190fa-465">Verify the following operations:</span></span>

* <span data-ttu-id="190fa-466">已註冊的使用者可以查看所有已核准的連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-466">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="190fa-467">已註冊的使用者可以編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-467">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="190fa-468">管理員可以核准/拒絕連絡人資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-468">Managers can approve/reject contact data.</span></span> <span data-ttu-id="190fa-469">此 `Details` 視圖會顯示 [**核准**] 和 [**拒絕**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="190fa-469">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="190fa-470">系統管理員可以核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-470">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="190fa-471">User</span><span class="sxs-lookup"><span data-stu-id="190fa-471">User</span></span>                | <span data-ttu-id="190fa-472">由應用程式植入</span><span class="sxs-lookup"><span data-stu-id="190fa-472">Seeded by the app</span></span> | <span data-ttu-id="190fa-473">選項。</span><span class="sxs-lookup"><span data-stu-id="190fa-473">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="190fa-474">否</span><span class="sxs-lookup"><span data-stu-id="190fa-474">No</span></span>                | <span data-ttu-id="190fa-475">編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-475">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="190fa-476">是</span><span class="sxs-lookup"><span data-stu-id="190fa-476">Yes</span></span>               | <span data-ttu-id="190fa-477">核准/拒絕和編輯/刪除自己的資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-477">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="190fa-478">是</span><span class="sxs-lookup"><span data-stu-id="190fa-478">Yes</span></span>               | <span data-ttu-id="190fa-479">核准/拒絕和編輯/刪除所有資料。</span><span class="sxs-lookup"><span data-stu-id="190fa-479">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="190fa-480">在系統管理員的瀏覽器中建立連絡人。</span><span class="sxs-lookup"><span data-stu-id="190fa-480">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="190fa-481">從系統管理員連絡人中複製 [刪除] 和 [編輯] 的 URL。</span><span class="sxs-lookup"><span data-stu-id="190fa-481">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="190fa-482">將這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="190fa-482">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="190fa-483">建立入門應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-483">Create the starter app</span></span>

* <span data-ttu-id="190fa-484">建立 Razor 名為 "ContactManager" 的頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-484">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="190fa-485">建立具有**個別使用者帳戶**的應用程式。</span><span class="sxs-lookup"><span data-stu-id="190fa-485">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="190fa-486">將它命名為 "ContactManager"，讓命名空間符合範例中使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="190fa-486">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="190fa-487">`-uld`指定 LocalDB，而不是 SQLite</span><span class="sxs-lookup"><span data-stu-id="190fa-487">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="190fa-488">新增*模型/連絡人 .cs*：</span><span class="sxs-lookup"><span data-stu-id="190fa-488">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="190fa-489">Scaffold `Contact` 模型。</span><span class="sxs-lookup"><span data-stu-id="190fa-489">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="190fa-490">建立初始遷移並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="190fa-490">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="190fa-491">更新*Pages/_Layout. cshtml*檔案中的**ContactManager**錨點：</span><span class="sxs-lookup"><span data-stu-id="190fa-491">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="190fa-492">藉由建立、編輯和刪除連絡人來測試應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-492">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="190fa-493">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="190fa-493">Seed the database</span></span>

<span data-ttu-id="190fa-494">將[SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs)類別新增至 [ *Data* ] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="190fa-494">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="190fa-495">呼叫 `SeedData.Initialize` 來源 `Main` ：</span><span class="sxs-lookup"><span data-stu-id="190fa-495">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="190fa-496">測試應用程式是否植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="190fa-496">Test that the app seeded the database.</span></span> <span data-ttu-id="190fa-497">如果 contact DB 中有任何資料列，則不會執行種子方法。</span><span class="sxs-lookup"><span data-stu-id="190fa-497">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="190fa-498">其他資源</span><span class="sxs-lookup"><span data-stu-id="190fa-498">Additional resources</span></span>

* [<span data-ttu-id="190fa-499">在 Azure App Service 中建置 .NET Core 和 SQL Database Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="190fa-499">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="190fa-500">[ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。</span><span class="sxs-lookup"><span data-stu-id="190fa-500">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="190fa-501">這個實驗室會詳細說明本教學課程中引進的安全性功能。</span><span class="sxs-lookup"><span data-stu-id="190fa-501">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="190fa-502">以自訂原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="190fa-502">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
