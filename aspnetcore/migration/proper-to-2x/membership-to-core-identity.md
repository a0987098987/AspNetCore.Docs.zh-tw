---
title: 從 ASP.NET 成員資格驗證遷移至 ASP.NET Core 2。0Identity
author: isaac2004
description: 瞭解如何使用成員資格驗證將現有的 ASP.NET 應用程式遷移至 ASP.NET Core 2.0 Identity 。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f039772f4276d0e8bcec2629350eba2ec0e7418c
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399682"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="5fa16-103">從 ASP.NET 成員資格驗證遷移至 ASP.NET Core 2。0Identity</span><span class="sxs-lookup"><span data-stu-id="5fa16-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="5fa16-104">作者：[Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="5fa16-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="5fa16-105">本文示範如何使用成員資格驗證，將 ASP.NET apps 的資料庫架構遷移至 ASP.NET Core 2.0 Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="5fa16-106">本檔提供將 ASP.NET 成員資格型應用程式的資料庫架構，遷移至用於 ASP.NET Core 的資料庫架構所需的步驟 Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="5fa16-107">如需從 ASP.NET 成員資格型驗證遷移至 ASP.NET 的詳細資訊 Identity ，請參閱將[現有的應用程式從 SQL Identity 成員資格遷移至 ASP.NET ](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)。</span><span class="sxs-lookup"><span data-stu-id="5fa16-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="5fa16-108">如需 ASP.NET Core 的詳細資訊 Identity ，請參閱[ Identity ASP.NET Core 簡介](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="5fa16-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="5fa16-109">成員資格架構的審查</span><span class="sxs-lookup"><span data-stu-id="5fa16-109">Review of Membership schema</span></span>

<span data-ttu-id="5fa16-110">在 ASP.NET 2.0 之前，開發人員負責為其應用程式建立整個驗證和授權流程。</span><span class="sxs-lookup"><span data-stu-id="5fa16-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="5fa16-111">有了 ASP.NET 2.0，就引進了成員資格，以提供可在 ASP.NET apps 中處理安全性的重複使用解決方案。</span><span class="sxs-lookup"><span data-stu-id="5fa16-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="5fa16-112">開發人員現在可以使用[aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)命令，將架構啟動至 SQL Server 資料庫。</span><span class="sxs-lookup"><span data-stu-id="5fa16-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="5fa16-113">執行此命令之後，會在資料庫中建立下列資料表。</span><span class="sxs-lookup"><span data-stu-id="5fa16-113">After running this command, the following tables were created in the database.</span></span>

  ![成員資格資料表](identity/_static/membership-tables.png)

<span data-ttu-id="5fa16-115">若要將現有的應用程式遷移至 ASP.NET Core 2.0 Identity ，這些資料表中的資料必須遷移至新架構所使用的資料表 Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="5fa16-116">ASP.NET Core Identity 2.0 架構</span><span class="sxs-lookup"><span data-stu-id="5fa16-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="5fa16-117">ASP.NET Core 2.0 遵循 [Identity](/aspnet/identity/index) ASP.NET 4.5 中引進的原則。</span><span class="sxs-lookup"><span data-stu-id="5fa16-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="5fa16-118">雖然原則是共用的，但架構之間的執行不同，甚至是 ASP.NET Core 的版本（請參閱[遷移驗證和 Identity ASP.NET Core 2.0](xref:migration/1x-to-2x/index)）。</span><span class="sxs-lookup"><span data-stu-id="5fa16-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="5fa16-119">若要查看 ASP.NET Core 2.0 的架構，最快的方式 Identity 就是建立新的 ASP.NET Core 2.0 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5fa16-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="5fa16-120">請遵循 Visual Studio 2017 中的下列步驟：</span><span class="sxs-lookup"><span data-stu-id="5fa16-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="5fa16-121">選取 [File] \(檔案\) >  [New] \(新增\) >  [Project] \(專案\)。</span><span class="sxs-lookup"><span data-stu-id="5fa16-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="5fa16-122">建立名為*CoreIdentitySample*的新**ASP.NET Core Web 應用程式**專案。</span><span class="sxs-lookup"><span data-stu-id="5fa16-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="5fa16-123">選取下拉式清單中的 [ **ASP.NET Core 2.0** ]，然後選取 [ **Web 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="5fa16-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="5fa16-124">此範本會產生[ Razor 頁面](xref:razor-pages/index)應用程式。</span><span class="sxs-lookup"><span data-stu-id="5fa16-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="5fa16-125">在按一下 **[確定]** 之前，請按一下 [**變更驗證**]。</span><span class="sxs-lookup"><span data-stu-id="5fa16-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="5fa16-126">選擇範本的**個別使用者帳戶** Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="5fa16-127">最後，依序按一下 **[確定]** 和 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="5fa16-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="5fa16-128">Visual Studio 會使用 ASP.NET Core 範本來建立專案 Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="5fa16-129">選取 [**工具**] [  >  **NuGet 套件管理員**  >  ] [**套件管理員主控台**]，以開啟 [**套件管理員主控台**] （PMC）視窗。</span><span class="sxs-lookup"><span data-stu-id="5fa16-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="5fa16-130">流覽至 PMC 中的專案根目錄，然後執行[Entity Framework （EF） Core](/ef/core) `Update-Database` 命令。</span><span class="sxs-lookup"><span data-stu-id="5fa16-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="5fa16-131">ASP.NET Core 2.0 Identity 使用 EF Core 來與儲存驗證資料的資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="5fa16-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="5fa16-132">為了讓新建立的應用程式能夠正常執行，必須要有資料庫來儲存這項資料。</span><span class="sxs-lookup"><span data-stu-id="5fa16-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="5fa16-133">建立新的應用程式之後，在資料庫環境中檢查架構的最快方式就是使用[EF Core 遷移](/ef/core/managing-schemas/migrations/)來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="5fa16-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="5fa16-134">此程式會在本機或其他位置建立資料庫，以模仿該架構。</span><span class="sxs-lookup"><span data-stu-id="5fa16-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="5fa16-135">如需詳細資訊，請參閱上述檔。</span><span class="sxs-lookup"><span data-stu-id="5fa16-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="5fa16-136">EF Core 命令會針對在*appsettings.js*中指定的資料庫使用連接字串。</span><span class="sxs-lookup"><span data-stu-id="5fa16-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="5fa16-137">下列連接字串會以名為*asp-net-identity*的*localhost*上的資料庫為目標。</span><span class="sxs-lookup"><span data-stu-id="5fa16-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="5fa16-138">在此設定中，EF Core 設定為使用 `DefaultConnection` 連接字串。</span><span class="sxs-lookup"><span data-stu-id="5fa16-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. <span data-ttu-id="5fa16-139">選取 [ **View**  >  **SQL Server 物件總管**]。</span><span class="sxs-lookup"><span data-stu-id="5fa16-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="5fa16-140">展開對應至appsettings.js的屬性中指定之資料庫名稱的節點 `ConnectionStrings:DefaultConnection` 。 *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="5fa16-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="5fa16-141">此 `Update-Database` 命令會建立以架構指定的資料庫，以及應用程式初始化所需的任何資料。</span><span class="sxs-lookup"><span data-stu-id="5fa16-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="5fa16-142">下圖描述使用上述步驟建立的資料表結構。</span><span class="sxs-lookup"><span data-stu-id="5fa16-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    <span data-ttu-id="5fa16-143">![Identity多](identity/_static/identity-tables.png)</span><span class="sxs-lookup"><span data-stu-id="5fa16-143">![Identity Tables](identity/_static/identity-tables.png)</span></span>

## <a name="migrate-the-schema"></a><span data-ttu-id="5fa16-144">移轉結構描述</span><span class="sxs-lookup"><span data-stu-id="5fa16-144">Migrate the schema</span></span>

<span data-ttu-id="5fa16-145">資料表結構和欄位中的成員資格和 ASP.NET Core 有些許差異 Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="5fa16-146">此模式已大幅變更 ASP.NET 和 ASP.NET Core 應用程式的驗證/授權。</span><span class="sxs-lookup"><span data-stu-id="5fa16-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="5fa16-147">仍然使用的主要物件 Identity 是*使用者*和*角色*。</span><span class="sxs-lookup"><span data-stu-id="5fa16-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="5fa16-148">以下是*使用者*、*角色*和*UserRoles*的對應資料表。</span><span class="sxs-lookup"><span data-stu-id="5fa16-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="5fa16-149">使用者</span><span class="sxs-lookup"><span data-stu-id="5fa16-149">Users</span></span>

|<span data-ttu-id="5fa16-150">*Identity<br>供.AspNetUsers*</span><span class="sxs-lookup"><span data-stu-id="5fa16-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="5fa16-151">*成員資格 <br> （dbo. aspnet_Users/dbo. aspnet_Membership）*</span><span class="sxs-lookup"><span data-stu-id="5fa16-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="5fa16-152">**功能變數名稱**</span><span class="sxs-lookup"><span data-stu-id="5fa16-152">**Field Name**</span></span>                 |<span data-ttu-id="5fa16-153">**型別**</span><span class="sxs-lookup"><span data-stu-id="5fa16-153">**Type**</span></span>|<span data-ttu-id="5fa16-154">**功能變數名稱**</span><span class="sxs-lookup"><span data-stu-id="5fa16-154">**Field Name**</span></span>                                    |<span data-ttu-id="5fa16-155">**型別**</span><span class="sxs-lookup"><span data-stu-id="5fa16-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="5fa16-156">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="5fa16-157">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="5fa16-158">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="5fa16-159">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="5fa16-160">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="5fa16-161">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="5fa16-162">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="5fa16-163">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="5fa16-164">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="5fa16-165">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="5fa16-166">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="5fa16-167">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="5fa16-168">bit</span><span class="sxs-lookup"><span data-stu-id="5fa16-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="5fa16-169">bit</span><span class="sxs-lookup"><span data-stu-id="5fa16-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="5fa16-170">並非所有欄位對應都與從成員資格到 ASP.NET Core 的一對一關聯性類似 Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="5fa16-171">上表會採用預設成員資格使用者架構，並將它對應至 ASP.NET Core Identity 架構。</span><span class="sxs-lookup"><span data-stu-id="5fa16-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="5fa16-172">任何其他用於成員資格的自訂欄位都必須手動對應。</span><span class="sxs-lookup"><span data-stu-id="5fa16-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="5fa16-173">在此對應中，沒有任何密碼對應，因為密碼條件和密碼 salts 都不會在兩者之間遷移。</span><span class="sxs-lookup"><span data-stu-id="5fa16-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="5fa16-174">**建議您將密碼保留為 null，並要求使用者重設其密碼。**</span><span class="sxs-lookup"><span data-stu-id="5fa16-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="5fa16-175">在 ASP.NET Core 中 Identity ， `LockoutEnd` 如果使用者遭到鎖定，則應該設定為未來的某個日期。這會顯示在遷移腳本中。</span><span class="sxs-lookup"><span data-stu-id="5fa16-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="5fa16-176">角色</span><span class="sxs-lookup"><span data-stu-id="5fa16-176">Roles</span></span>

|<span data-ttu-id="5fa16-177">*Identity<br>供.AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="5fa16-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="5fa16-178">*成員資格 <br> （dbo. aspnet_Roles）*</span><span class="sxs-lookup"><span data-stu-id="5fa16-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="5fa16-179">**功能變數名稱**</span><span class="sxs-lookup"><span data-stu-id="5fa16-179">**Field Name**</span></span>                 |<span data-ttu-id="5fa16-180">**型別**</span><span class="sxs-lookup"><span data-stu-id="5fa16-180">**Type**</span></span>|<span data-ttu-id="5fa16-181">**功能變數名稱**</span><span class="sxs-lookup"><span data-stu-id="5fa16-181">**Field Name**</span></span>   |<span data-ttu-id="5fa16-182">**型別**</span><span class="sxs-lookup"><span data-stu-id="5fa16-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="5fa16-183">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-183">string</span></span>  |`RoleId`         | <span data-ttu-id="5fa16-184">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="5fa16-185">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-185">string</span></span>  |`RoleName`       | <span data-ttu-id="5fa16-186">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="5fa16-187">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="5fa16-188">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="5fa16-189">使用者角色</span><span class="sxs-lookup"><span data-stu-id="5fa16-189">User Roles</span></span>

|<span data-ttu-id="5fa16-190">*Identity<br>供.[Aspnetuserroles]*</span><span class="sxs-lookup"><span data-stu-id="5fa16-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="5fa16-191">*成員資格 <br> （dbo. aspnet_UsersInRoles）*</span><span class="sxs-lookup"><span data-stu-id="5fa16-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="5fa16-192">**功能變數名稱**</span><span class="sxs-lookup"><span data-stu-id="5fa16-192">**Field Name**</span></span>           |<span data-ttu-id="5fa16-193">**型別**</span><span class="sxs-lookup"><span data-stu-id="5fa16-193">**Type**</span></span>  |<span data-ttu-id="5fa16-194">**功能變數名稱**</span><span class="sxs-lookup"><span data-stu-id="5fa16-194">**Field Name**</span></span>|<span data-ttu-id="5fa16-195">**型別**</span><span class="sxs-lookup"><span data-stu-id="5fa16-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="5fa16-196">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-196">string</span></span>    |`RoleId`      |<span data-ttu-id="5fa16-197">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="5fa16-198">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-198">string</span></span>    |`UserId`      |<span data-ttu-id="5fa16-199">字串</span><span class="sxs-lookup"><span data-stu-id="5fa16-199">string</span></span>                     |

<span data-ttu-id="5fa16-200">建立*使用者*和*角色*的遷移腳本時，請參考上述對應表。</span><span class="sxs-lookup"><span data-stu-id="5fa16-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="5fa16-201">下列範例假設您在資料庫伺服器上有兩個資料庫。</span><span class="sxs-lookup"><span data-stu-id="5fa16-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="5fa16-202">一個資料庫包含現有的 ASP.NET 成員資格架構和資料。</span><span class="sxs-lookup"><span data-stu-id="5fa16-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="5fa16-203">另一個*CoreIdentitySample*資料庫是使用稍早所述的步驟所建立。</span><span class="sxs-lookup"><span data-stu-id="5fa16-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="5fa16-204">批註包含內嵌，以提供更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="5fa16-204">Comments are included inline for more details.</span></span>

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

<span data-ttu-id="5fa16-205">完成上述腳本之後，稍 Identity 早建立的 ASP.NET Core 應用程式會填入成員資格使用者。</span><span class="sxs-lookup"><span data-stu-id="5fa16-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="5fa16-206">使用者必須在登入之前變更其密碼。</span><span class="sxs-lookup"><span data-stu-id="5fa16-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="5fa16-207">如果成員資格系統的使用者名稱不符合其電子郵件地址，則需要對稍早建立的應用程式進行變更以配合此位置。</span><span class="sxs-lookup"><span data-stu-id="5fa16-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="5fa16-208">預設範本預期 `UserName` 和 `Email` 都是相同的。</span><span class="sxs-lookup"><span data-stu-id="5fa16-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="5fa16-209">在不同的情況下，登入程式必須修改為使用， `UserName` 而不是 `Email` 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="5fa16-210">在登入頁面的（ `PageModel` 位於*Pages\Account\Login.cshtml.cs*）中， `[EmailAddress]` 從 [*電子郵件*] 屬性移除屬性。</span><span class="sxs-lookup"><span data-stu-id="5fa16-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="5fa16-211">將它重新命名為*UserName*。</span><span class="sxs-lookup"><span data-stu-id="5fa16-211">Rename it to *UserName*.</span></span> <span data-ttu-id="5fa16-212">這需要 `EmailAddress` 在*View*和*PageModel*中提及的任何位置進行變更。</span><span class="sxs-lookup"><span data-stu-id="5fa16-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="5fa16-213">結果看起來如下：</span><span class="sxs-lookup"><span data-stu-id="5fa16-213">The result looks like the following:</span></span>

 ![已修正登入](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="5fa16-215">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5fa16-215">Next steps</span></span>

<span data-ttu-id="5fa16-216">在本教學課程中，您已瞭解如何將 SQL 成員資格的使用者移植到 ASP.NET Core 2.0 Identity 。</span><span class="sxs-lookup"><span data-stu-id="5fa16-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="5fa16-217">如需有關 ASP.NET Core 的詳細資訊 Identity ，請參閱[簡介 Identity ](xref:security/authentication/identity)。</span><span class="sxs-lookup"><span data-stu-id="5fa16-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
