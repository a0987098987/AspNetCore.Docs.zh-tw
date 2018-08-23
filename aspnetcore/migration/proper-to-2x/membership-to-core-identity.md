---
title: 從 ASP.NET 成員資格驗證移轉至 ASP.NET Core 2.0 身分識別
author: isaac2004
description: 了解如何移轉現有的 ASP.NET 應用程式使用 ASP.NET Core 2.0 身分識別的成員資格驗證。
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2018
ms.locfileid: "41832955"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="0828e-103">從 ASP.NET 成員資格驗證移轉至 ASP.NET Core 2.0 身分識別</span><span class="sxs-lookup"><span data-stu-id="0828e-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="0828e-104">作者：[Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="0828e-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="0828e-105">本文示範如何使用 ASP.NET Core 2.0 身分識別的成員資格驗證的 ASP.NET 應用程式的資料庫結構描述移轉。</span><span class="sxs-lookup"><span data-stu-id="0828e-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="0828e-106">本文件提供將 ASP.NET 成員資格為基礎的應用程式的資料庫結構描述移轉至使用 ASP.NET Core 身分識別的資料庫結構描述所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="0828e-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="0828e-107">如需有關如何從 ASP.NET 成員資格為基礎的驗證移轉至 ASP.NET Identity 的詳細資訊，請參閱[將現有的應用程式從 SQL 成員資格移轉至 ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)。</span><span class="sxs-lookup"><span data-stu-id="0828e-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="0828e-108">如需 ASP.NET Core 身分識別的詳細資訊，請參閱[ASP.NET core 身分識別簡介](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="0828e-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="0828e-109">檢閱的成員資格結構描述</span><span class="sxs-lookup"><span data-stu-id="0828e-109">Review of Membership schema</span></span>

<span data-ttu-id="0828e-110">在 ASP.NET 2.0 之前開發人員所負責建立整個的驗證和授權程序，為其應用程式。</span><span class="sxs-lookup"><span data-stu-id="0828e-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="0828e-111">使用 ASP.NET 2.0 成員資格引進，提供處理 ASP.NET 應用程式中的安全性與重複使用的解決方案。</span><span class="sxs-lookup"><span data-stu-id="0828e-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="0828e-112">開發人員現在便可以啟動結構描述載入到 SQL Server database [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)命令。</span><span class="sxs-lookup"><span data-stu-id="0828e-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="0828e-113">執行此命令之後，在資料庫中建立下列資料表。</span><span class="sxs-lookup"><span data-stu-id="0828e-113">After running this command, the following tables were created in the database.</span></span>

  ![成員資格資料表](identity/_static/membership-tables.png)

<span data-ttu-id="0828e-115">若要將現有的應用程式移轉至 ASP.NET Core 2.0 身分識別，這些資料表中的資料必須移轉到新的身分識別結構描述所使用的資料表。</span><span class="sxs-lookup"><span data-stu-id="0828e-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="0828e-116">ASP.NET Core 身分識別 2.0 結構描述</span><span class="sxs-lookup"><span data-stu-id="0828e-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="0828e-117">ASP.NET Core 2.0 會遵循[識別](/aspnet/identity/index)ASP.NET 4.5 中導入的原則。</span><span class="sxs-lookup"><span data-stu-id="0828e-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="0828e-118">雖然共用原則，架構之間的實作是不同版本的 ASP.NET Core 之間 (請參閱[移轉至 ASP.NET Core 2.0 的驗證和身分識別](xref:migration/1x-to-2x/index))。</span><span class="sxs-lookup"><span data-stu-id="0828e-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="0828e-119">若要檢視的 ASP.NET Core 2.0 身分識別的結構描述的最快方式是建立新的 ASP.NET Core 2.0 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0828e-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="0828e-120">請遵循 Visual Studio 2017 中的下列步驟：</span><span class="sxs-lookup"><span data-stu-id="0828e-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="0828e-121">選取 [檔案]  >  [新增]  >  [專案]。</span><span class="sxs-lookup"><span data-stu-id="0828e-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="0828e-122">建立新**ASP.NET Core Web 應用程式**並將專案命名*CoreIdentitySample*。</span><span class="sxs-lookup"><span data-stu-id="0828e-122">Create a new **ASP.NET Core Web Application** and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="0828e-123">選取  **ASP.NET Core 2.0**下拉式清單中，然後選取**Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="0828e-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="0828e-124">此範本會產生[Razor 頁面](xref:razor-pages/index)應用程式。</span><span class="sxs-lookup"><span data-stu-id="0828e-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="0828e-125">再按一下 **[確定]**，按一下**變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="0828e-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="0828e-126">選擇**個別使用者帳戶**身分識別的範本。</span><span class="sxs-lookup"><span data-stu-id="0828e-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="0828e-127">最後，按一下 **[確定]**，然後**確定**。</span><span class="sxs-lookup"><span data-stu-id="0828e-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="0828e-128">Visual Studio 建立專案，使用 ASP.NET Core 身分識別範本。</span><span class="sxs-lookup"><span data-stu-id="0828e-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="0828e-129">ASP.NET Core 2.0 身分識別會使用[Entity Framework Core](/ef/core)來儲存驗證資料的資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="0828e-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="0828e-130">為了讓新建立的應用程式運作，那里必須要儲存這項資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="0828e-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="0828e-131">之後建立新的應用程式，檢查的資料庫環境中的結構描述的最快方式是建立使用 Entity Framework 移轉的資料庫。</span><span class="sxs-lookup"><span data-stu-id="0828e-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="0828e-132">此程序會建立資料庫、 在本機或其他位置，可以模擬該結構描述。</span><span class="sxs-lookup"><span data-stu-id="0828e-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="0828e-133">檢閱先前的文件，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0828e-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="0828e-134">若要使用 ASP.NET Core 身分識別結構描述中建立資料庫，執行`Update-Database`命令，在 Visual Studio **Package Manager Console** (PMC) 視窗&mdash;位於**工具** > **NuGet 套件管理員** > **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="0828e-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="0828e-135">PMC 支援在執行的 Entity Framework 命令。</span><span class="sxs-lookup"><span data-stu-id="0828e-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="0828e-136">Entity Framework 命令會使用連接字串中指定的資料庫*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="0828e-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="0828e-137">下列連接字串為目標資料庫上*localhost*名為*asp net core-識別*。</span><span class="sxs-lookup"><span data-stu-id="0828e-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="0828e-138">在此設定中，Entity Framework 設定為使用`DefaultConnection`連接字串。</span><span class="sxs-lookup"><span data-stu-id="0828e-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="0828e-139">此命令會建置具有結構描述所指定的資料庫和所需的應用程式初始化任何資料。</span><span class="sxs-lookup"><span data-stu-id="0828e-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="0828e-140">下圖說明使用上述步驟建立的資料表結構。</span><span class="sxs-lookup"><span data-stu-id="0828e-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![身分識別的資料表](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="0828e-142">移轉結構描述</span><span class="sxs-lookup"><span data-stu-id="0828e-142">Migrate the schema</span></span>

<span data-ttu-id="0828e-143">有的資料表結構和成員資格和 ASP.NET Core 身分識別的欄位中的細微差異。</span><span class="sxs-lookup"><span data-stu-id="0828e-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="0828e-144">使用 ASP.NET 和 ASP.NET Core 應用程式的驗證/授權已大幅變更模式。</span><span class="sxs-lookup"><span data-stu-id="0828e-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="0828e-145">仍會使用以身分識別的索引鍵物件*使用者*並*角色*。</span><span class="sxs-lookup"><span data-stu-id="0828e-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="0828e-146">以下是對應的資料表*使用者*，*角色*，並*UserRoles*。</span><span class="sxs-lookup"><span data-stu-id="0828e-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="0828e-147">使用者</span><span class="sxs-lookup"><span data-stu-id="0828e-147">Users</span></span>

| <span data-ttu-id="0828e-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="0828e-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="0828e-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="0828e-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0828e-150">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="0828e-150">**Field Name**</span></span> | <span data-ttu-id="0828e-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="0828e-151">**Type**</span></span>  |   <span data-ttu-id="0828e-152">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="0828e-152">**Field Name**</span></span> | <span data-ttu-id="0828e-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="0828e-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="0828e-154">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="0828e-155">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-155">string</span></span>
|`UserName` | <span data-ttu-id="0828e-156">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="0828e-157">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-157">string</span></span>
|`Email` | <span data-ttu-id="0828e-158">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="0828e-159">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="0828e-160">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="0828e-161">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="0828e-162">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="0828e-163">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="0828e-164">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="0828e-165">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="0828e-166">bit</span><span class="sxs-lookup"><span data-stu-id="0828e-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="0828e-167">bit</span><span class="sxs-lookup"><span data-stu-id="0828e-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="0828e-168">並非所有的欄位對應類似於 ASP.NET Core 身分識別在從成員資格的一對一關聯性。</span><span class="sxs-lookup"><span data-stu-id="0828e-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="0828e-169">上表中的預設成員資格使用者結構描述並將它對應至 ASP.NET Core 身分識別結構描述。</span><span class="sxs-lookup"><span data-stu-id="0828e-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="0828e-170">任何其他自訂欄位所使用的成員資格，就必須手動對應。</span><span class="sxs-lookup"><span data-stu-id="0828e-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="0828e-171">在這個對應中，沒有任何對應的密碼，因為密碼 salt 和密碼條件可以兩者之間移轉。</span><span class="sxs-lookup"><span data-stu-id="0828e-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="0828e-172">**建議您讓密碼為 null，並要求重設其密碼的使用者。**</span><span class="sxs-lookup"><span data-stu-id="0828e-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="0828e-173">在 ASP.NET Core 身分識別`LockoutEnd`應該設定為在未來的某個日期中，如果使用者遭到鎖定。這是由移轉指令碼所示。</span><span class="sxs-lookup"><span data-stu-id="0828e-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="0828e-174">角色</span><span class="sxs-lookup"><span data-stu-id="0828e-174">Roles</span></span>

| <span data-ttu-id="0828e-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="0828e-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="0828e-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="0828e-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0828e-177">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="0828e-177">**Field Name**</span></span> | <span data-ttu-id="0828e-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="0828e-178">**Type**</span></span>  |   <span data-ttu-id="0828e-179">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="0828e-179">**Field Name**</span></span> | <span data-ttu-id="0828e-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="0828e-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="0828e-181">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-181">string</span></span> | `RoleId` | <span data-ttu-id="0828e-182">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-182">string</span></span>
|`Name` | <span data-ttu-id="0828e-183">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-183">string</span></span> | `RoleName` | <span data-ttu-id="0828e-184">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="0828e-185">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="0828e-186">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="0828e-187">使用者角色</span><span class="sxs-lookup"><span data-stu-id="0828e-187">User Roles</span></span>

| <span data-ttu-id="0828e-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="0828e-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="0828e-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="0828e-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0828e-190">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="0828e-190">**Field Name**</span></span> | <span data-ttu-id="0828e-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="0828e-191">**Type**</span></span>  |   <span data-ttu-id="0828e-192">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="0828e-192">**Field Name**</span></span> | <span data-ttu-id="0828e-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="0828e-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="0828e-194">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-194">string</span></span> | `RoleId` | <span data-ttu-id="0828e-195">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-195">string</span></span>
|`UserId` | <span data-ttu-id="0828e-196">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-196">string</span></span> | `UserId` | <span data-ttu-id="0828e-197">字串</span><span class="sxs-lookup"><span data-stu-id="0828e-197">string</span></span>

<span data-ttu-id="0828e-198">建立的移轉指令碼時，請參考上述對應表*使用者*並*角色*。</span><span class="sxs-lookup"><span data-stu-id="0828e-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="0828e-199">下列範例假設您有兩個資料庫的資料庫伺服器上。</span><span class="sxs-lookup"><span data-stu-id="0828e-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="0828e-200">一個資料庫包含現有的 ASP.NET 成員資格結構描述和資料。</span><span class="sxs-lookup"><span data-stu-id="0828e-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="0828e-201">使用稍早所述的步驟建立另一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="0828e-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="0828e-202">註解是包含的內嵌，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0828e-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="0828e-203">此指令碼完成後之後, 稍早建立的 ASP.NET Core 身分識別應用程式會填入成員資格使用者。</span><span class="sxs-lookup"><span data-stu-id="0828e-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="0828e-204">使用者必須變更其密碼才能登入。</span><span class="sxs-lookup"><span data-stu-id="0828e-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="0828e-205">如果成員資格系統有不符合其電子郵件地址的使用者名稱的使用者，需要變更就是要做到這一點稍早建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0828e-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="0828e-206">預設範本會預期`UserName`和`Email`相同。</span><span class="sxs-lookup"><span data-stu-id="0828e-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="0828e-207">它們是不同的情況下，登入程序必須使用修改`UserName`而不是`Email`。</span><span class="sxs-lookup"><span data-stu-id="0828e-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="0828e-208">在 `PageModel`的登入頁面上，位於*Pages\Account\Login.cshtml.cs*，移除`[EmailAddress]`屬性從*電子郵件*屬性。</span><span class="sxs-lookup"><span data-stu-id="0828e-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="0828e-209">重新命名為*UserName*。</span><span class="sxs-lookup"><span data-stu-id="0828e-209">Rename it to *UserName*.</span></span> <span data-ttu-id="0828e-210">這需要變更任何地方`EmailAddress`所述，在*檢視*並*PageModel*。</span><span class="sxs-lookup"><span data-stu-id="0828e-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="0828e-211">結果看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="0828e-211">The result looks like the following:</span></span>

 ![固定的登入](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="0828e-213">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0828e-213">Next steps</span></span>

<span data-ttu-id="0828e-214">在本教學課程中，您已了解如何移植到 ASP.NET Core 2.0 身分識別從 SQL 成員資格使用者。</span><span class="sxs-lookup"><span data-stu-id="0828e-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="0828e-215">如需 ASP.NET Core 身分識別的詳細資訊，請參閱[身分識別簡介](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="0828e-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
