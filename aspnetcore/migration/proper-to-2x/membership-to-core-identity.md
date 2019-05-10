---
title: 從 ASP.NET 成員資格驗證移轉至 ASP.NET Core 2.0 身分識別
author: isaac2004
description: 了解如何移轉現有的 ASP.NET 應用程式使用 ASP.NET Core 2.0 身分識別的成員資格驗證。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65084869"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="af879-103">從 ASP.NET 成員資格驗證移轉至 ASP.NET Core 2.0 身分識別</span><span class="sxs-lookup"><span data-stu-id="af879-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="af879-104">作者：[Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="af879-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="af879-105">本文示範如何使用 ASP.NET Core 2.0 身分識別的成員資格驗證的 ASP.NET 應用程式的資料庫結構描述移轉。</span><span class="sxs-lookup"><span data-stu-id="af879-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="af879-106">本文件提供將 ASP.NET 成員資格為基礎的應用程式的資料庫結構描述移轉至使用 ASP.NET Core 身分識別的資料庫結構描述所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="af879-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="af879-107">如需有關如何從 ASP.NET 成員資格為基礎的驗證移轉至 ASP.NET Identity 的詳細資訊，請參閱[將現有的應用程式從 SQL 成員資格移轉至 ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)。</span><span class="sxs-lookup"><span data-stu-id="af879-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="af879-108">如需 ASP.NET Core 身分識別的詳細資訊，請參閱[ASP.NET core 身分識別簡介](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="af879-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="af879-109">檢閱的成員資格結構描述</span><span class="sxs-lookup"><span data-stu-id="af879-109">Review of Membership schema</span></span>

<span data-ttu-id="af879-110">在 ASP.NET 2.0 之前開發人員所負責建立整個的驗證和授權程序，為其應用程式。</span><span class="sxs-lookup"><span data-stu-id="af879-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="af879-111">使用 ASP.NET 2.0 成員資格引進，提供處理 ASP.NET 應用程式中的安全性與重複使用的解決方案。</span><span class="sxs-lookup"><span data-stu-id="af879-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="af879-112">開發人員現在便可以啟動結構描述載入到 SQL Server database [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)命令。</span><span class="sxs-lookup"><span data-stu-id="af879-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="af879-113">執行此命令之後，在資料庫中建立下列資料表。</span><span class="sxs-lookup"><span data-stu-id="af879-113">After running this command, the following tables were created in the database.</span></span>

  ![成員資格資料表](identity/_static/membership-tables.png)

<span data-ttu-id="af879-115">若要將現有的應用程式移轉至 ASP.NET Core 2.0 身分識別，這些資料表中的資料必須移轉到新的身分識別結構描述所使用的資料表。</span><span class="sxs-lookup"><span data-stu-id="af879-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="af879-116">ASP.NET Core 身分識別 2.0 結構描述</span><span class="sxs-lookup"><span data-stu-id="af879-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="af879-117">ASP.NET Core 2.0 會遵循[識別](/aspnet/identity/index)ASP.NET 4.5 中導入的原則。</span><span class="sxs-lookup"><span data-stu-id="af879-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="af879-118">雖然共用原則，架構之間的實作是不同版本的 ASP.NET Core 之間 (請參閱[移轉至 ASP.NET Core 2.0 的驗證和身分識別](xref:migration/1x-to-2x/index))。</span><span class="sxs-lookup"><span data-stu-id="af879-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="af879-119">若要檢視的 ASP.NET Core 2.0 身分識別的結構描述的最快方式是建立新的 ASP.NET Core 2.0 應用程式。</span><span class="sxs-lookup"><span data-stu-id="af879-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="af879-120">請遵循 Visual Studio 2017 中的下列步驟：</span><span class="sxs-lookup"><span data-stu-id="af879-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="af879-121">選取 [檔案]  >  [新增]  >  [專案]。</span><span class="sxs-lookup"><span data-stu-id="af879-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="af879-122">建立新**ASP.NET Core Web 應用程式**專案，命名為*CoreIdentitySample*。</span><span class="sxs-lookup"><span data-stu-id="af879-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="af879-123">選取  **ASP.NET Core 2.0**下拉式清單中，然後選取**Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="af879-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="af879-124">此範本會產生[Razor 頁面](xref:razor-pages/index)應用程式。</span><span class="sxs-lookup"><span data-stu-id="af879-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="af879-125">再按一下 **[確定]**，按一下**變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="af879-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="af879-126">選擇**個別使用者帳戶**身分識別的範本。</span><span class="sxs-lookup"><span data-stu-id="af879-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="af879-127">最後，按一下 **[確定]**，然後**確定**。</span><span class="sxs-lookup"><span data-stu-id="af879-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="af879-128">Visual Studio 建立專案，使用 ASP.NET Core 身分識別範本。</span><span class="sxs-lookup"><span data-stu-id="af879-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="af879-129">選取 **工具** > **NuGet 套件管理員** > **Package Manager Console**開啟**Package Manager Console**(PMC) 視窗。</span><span class="sxs-lookup"><span data-stu-id="af879-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="af879-130">巡覽至專案根目錄中 PMC 中，並執行[Entity Framework (EF) Core](/ef/core) `Update-Database`命令。</span><span class="sxs-lookup"><span data-stu-id="af879-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="af879-131">ASP.NET Core 2.0 身分識別會同時使用 EF Core，在與儲存的驗證資料的資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="af879-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="af879-132">為了讓新建立的應用程式運作，那里必須要儲存這項資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="af879-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="af879-133">建立新的應用程式之後, 檢查的資料庫環境中的結構描述的最快方式是建立資料庫使用[EF Core 移轉](/ef/core/managing-schemas/migrations/)。</span><span class="sxs-lookup"><span data-stu-id="af879-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="af879-134">此程序會建立資料庫、 在本機或其他位置，可以模擬該結構描述。</span><span class="sxs-lookup"><span data-stu-id="af879-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="af879-135">檢閱先前的文件，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="af879-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="af879-136">EF Core 命令會使用連接字串中指定的資料庫*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="af879-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="af879-137">下列連接字串為目標資料庫上*localhost*名為*asp net core-識別*。</span><span class="sxs-lookup"><span data-stu-id="af879-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="af879-138">在此設定時，EF Core 會設定為使用`DefaultConnection`連接字串。</span><span class="sxs-lookup"><span data-stu-id="af879-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. <span data-ttu-id="af879-139">選取 **檢視** > **SQL Server 物件總管**。</span><span class="sxs-lookup"><span data-stu-id="af879-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="af879-140">展開對應至指定的資料庫名稱的節點`ConnectionStrings:DefaultConnection`的屬性*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="af879-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="af879-141">`Update-Database`命令所建立的結構描述所指定的資料庫和所需的應用程式初始化任何資料。</span><span class="sxs-lookup"><span data-stu-id="af879-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="af879-142">下圖說明使用上述步驟建立的資料表結構。</span><span class="sxs-lookup"><span data-stu-id="af879-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![身分識別的資料表](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="af879-144">移轉結構描述</span><span class="sxs-lookup"><span data-stu-id="af879-144">Migrate the schema</span></span>

<span data-ttu-id="af879-145">有的資料表結構和成員資格和 ASP.NET Core 身分識別的欄位中的細微差異。</span><span class="sxs-lookup"><span data-stu-id="af879-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="af879-146">使用 ASP.NET 和 ASP.NET Core 應用程式的驗證/授權已大幅變更模式。</span><span class="sxs-lookup"><span data-stu-id="af879-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="af879-147">仍會使用以身分識別的索引鍵物件*使用者*並*角色*。</span><span class="sxs-lookup"><span data-stu-id="af879-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="af879-148">以下是對應的資料表*使用者*，*角色*，並*UserRoles*。</span><span class="sxs-lookup"><span data-stu-id="af879-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="af879-149">使用者</span><span class="sxs-lookup"><span data-stu-id="af879-149">Users</span></span>

|<span data-ttu-id="af879-150">*Identity<br>(dbo.AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="af879-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="af879-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="af879-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="af879-152">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="af879-152">**Field Name**</span></span>                 |<span data-ttu-id="af879-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="af879-153">**Type**</span></span>|<span data-ttu-id="af879-154">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="af879-154">**Field Name**</span></span>                                    |<span data-ttu-id="af879-155">**Type**</span><span class="sxs-lookup"><span data-stu-id="af879-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="af879-156">字串</span><span class="sxs-lookup"><span data-stu-id="af879-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="af879-157">字串</span><span class="sxs-lookup"><span data-stu-id="af879-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="af879-158">字串</span><span class="sxs-lookup"><span data-stu-id="af879-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="af879-159">字串</span><span class="sxs-lookup"><span data-stu-id="af879-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="af879-160">字串</span><span class="sxs-lookup"><span data-stu-id="af879-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="af879-161">字串</span><span class="sxs-lookup"><span data-stu-id="af879-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="af879-162">字串</span><span class="sxs-lookup"><span data-stu-id="af879-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="af879-163">字串</span><span class="sxs-lookup"><span data-stu-id="af879-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="af879-164">字串</span><span class="sxs-lookup"><span data-stu-id="af879-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="af879-165">字串</span><span class="sxs-lookup"><span data-stu-id="af879-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="af879-166">字串</span><span class="sxs-lookup"><span data-stu-id="af879-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="af879-167">字串</span><span class="sxs-lookup"><span data-stu-id="af879-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="af879-168">bit</span><span class="sxs-lookup"><span data-stu-id="af879-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="af879-169">bit</span><span class="sxs-lookup"><span data-stu-id="af879-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="af879-170">並非所有的欄位對應類似於 ASP.NET Core 身分識別在從成員資格的一對一關聯性。</span><span class="sxs-lookup"><span data-stu-id="af879-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="af879-171">上表中的預設成員資格使用者結構描述並將它對應至 ASP.NET Core 身分識別結構描述。</span><span class="sxs-lookup"><span data-stu-id="af879-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="af879-172">任何其他自訂欄位所使用的成員資格，就必須手動對應。</span><span class="sxs-lookup"><span data-stu-id="af879-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="af879-173">在這個對應中，沒有任何對應的密碼，因為密碼 salt 和密碼條件可以兩者之間移轉。</span><span class="sxs-lookup"><span data-stu-id="af879-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="af879-174">**建議您讓密碼為 null，並要求重設其密碼的使用者。**</span><span class="sxs-lookup"><span data-stu-id="af879-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="af879-175">在 ASP.NET Core 身分識別`LockoutEnd`應該設定為在未來的某個日期中，如果使用者遭到鎖定。這是由移轉指令碼所示。</span><span class="sxs-lookup"><span data-stu-id="af879-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="af879-176">角色</span><span class="sxs-lookup"><span data-stu-id="af879-176">Roles</span></span>

|<span data-ttu-id="af879-177">*Identity<br>(dbo.AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="af879-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="af879-178">*Membership<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="af879-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="af879-179">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="af879-179">**Field Name**</span></span>                 |<span data-ttu-id="af879-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="af879-180">**Type**</span></span>|<span data-ttu-id="af879-181">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="af879-181">**Field Name**</span></span>   |<span data-ttu-id="af879-182">**Type**</span><span class="sxs-lookup"><span data-stu-id="af879-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="af879-183">字串</span><span class="sxs-lookup"><span data-stu-id="af879-183">string</span></span>  |`RoleId`         | <span data-ttu-id="af879-184">字串</span><span class="sxs-lookup"><span data-stu-id="af879-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="af879-185">字串</span><span class="sxs-lookup"><span data-stu-id="af879-185">string</span></span>  |`RoleName`       | <span data-ttu-id="af879-186">字串</span><span class="sxs-lookup"><span data-stu-id="af879-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="af879-187">字串</span><span class="sxs-lookup"><span data-stu-id="af879-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="af879-188">字串</span><span class="sxs-lookup"><span data-stu-id="af879-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="af879-189">使用者角色</span><span class="sxs-lookup"><span data-stu-id="af879-189">User Roles</span></span>

|<span data-ttu-id="af879-190">*Identity<br>(dbo.AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="af879-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="af879-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="af879-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="af879-192">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="af879-192">**Field Name**</span></span>           |<span data-ttu-id="af879-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="af879-193">**Type**</span></span>  |<span data-ttu-id="af879-194">**欄位名稱**</span><span class="sxs-lookup"><span data-stu-id="af879-194">**Field Name**</span></span>|<span data-ttu-id="af879-195">**Type**</span><span class="sxs-lookup"><span data-stu-id="af879-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="af879-196">字串</span><span class="sxs-lookup"><span data-stu-id="af879-196">string</span></span>    |`RoleId`      |<span data-ttu-id="af879-197">字串</span><span class="sxs-lookup"><span data-stu-id="af879-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="af879-198">字串</span><span class="sxs-lookup"><span data-stu-id="af879-198">string</span></span>    |`UserId`      |<span data-ttu-id="af879-199">字串</span><span class="sxs-lookup"><span data-stu-id="af879-199">string</span></span>                     |

<span data-ttu-id="af879-200">建立的移轉指令碼時，請參考上述對應表*使用者*並*角色*。</span><span class="sxs-lookup"><span data-stu-id="af879-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="af879-201">下列範例假設您有兩個資料庫的資料庫伺服器上。</span><span class="sxs-lookup"><span data-stu-id="af879-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="af879-202">一個資料庫包含現有的 ASP.NET 成員資格結構描述和資料。</span><span class="sxs-lookup"><span data-stu-id="af879-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="af879-203">另*CoreIdentitySample*使用稍早所述的步驟建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="af879-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="af879-204">註解是包含的內嵌，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="af879-204">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="af879-205">在上述指令碼完成後之後, 稍早建立的 ASP.NET Core 身分識別應用程式會填入成員資格使用者。</span><span class="sxs-lookup"><span data-stu-id="af879-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="af879-206">使用者必須變更其密碼才能登入。</span><span class="sxs-lookup"><span data-stu-id="af879-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="af879-207">如果成員資格系統有不符合其電子郵件地址的使用者名稱的使用者，需要變更就是要做到這一點稍早建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="af879-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="af879-208">預設範本會預期`UserName`和`Email`相同。</span><span class="sxs-lookup"><span data-stu-id="af879-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="af879-209">它們是不同的情況下，登入程序必須使用修改`UserName`而不是`Email`。</span><span class="sxs-lookup"><span data-stu-id="af879-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="af879-210">在 `PageModel`的登入頁面上，位於*Pages\Account\Login.cshtml.cs*，移除`[EmailAddress]`屬性從*電子郵件*屬性。</span><span class="sxs-lookup"><span data-stu-id="af879-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="af879-211">重新命名為*UserName*。</span><span class="sxs-lookup"><span data-stu-id="af879-211">Rename it to *UserName*.</span></span> <span data-ttu-id="af879-212">這需要變更任何地方`EmailAddress`所述，在*檢視*並*PageModel*。</span><span class="sxs-lookup"><span data-stu-id="af879-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="af879-213">結果看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="af879-213">The result looks like the following:</span></span>

 ![固定的登入](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="af879-215">後續步驟</span><span class="sxs-lookup"><span data-stu-id="af879-215">Next steps</span></span>

<span data-ttu-id="af879-216">在本教學課程中，您已了解如何移植到 ASP.NET Core 2.0 身分識別從 SQL 成員資格使用者。</span><span class="sxs-lookup"><span data-stu-id="af879-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="af879-217">如需 ASP.NET Core 身分識別的詳細資訊，請參閱[身分識別簡介](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="af879-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
