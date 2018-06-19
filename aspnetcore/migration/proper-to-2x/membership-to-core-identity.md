---
title: 從 ASP.NET 成員資格驗證移轉至 ASP.NET Core 2.0 身分識別
author: isaac2004
description: 了解如何移轉現有的 ASP.NET 應用程式使用 ASP.NET Core 2.0 身分識別的成員資格驗證。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851540"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>從 ASP.NET 成員資格驗證移轉至 ASP.NET Core 2.0 身分識別

作者：[Isaac Levin](https://isaaclevin.com)

本文示範移轉使用 ASP.NET Core 2.0 身分識別驗證成員資格的 ASP.NET 應用程式的資料庫結構描述。

> [!NOTE]
> 本文件提供將 ASP.NET 成員資格為基礎的應用程式的資料庫結構描述移轉至使用 ASP.NET Core 身分識別的資料庫結構描述所需的步驟。 如需有關如何從 ASP.NET 成員資格為基礎的驗證移轉至 ASP.NET Identity 的詳細資訊，請參閱[將從 SQL 成員資格的現有應用程式移轉至 ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)。 如需 ASP.NET Core 身分識別的詳細資訊，請參閱[ASP.NET Core 上的識別簡介](xref:security/authentication/identity)。

## <a name="review-of-membership-schema"></a>成員資格的結構描述的檢閱

在 ASP.NET 2.0 之前開發人員所負責建立自己的應用程式的整個驗證和授權程序。 與 ASP.NET 2.0 被引進的成員資格，提供處理 ASP.NET 應用程式的安全性與重複使用的解決方案。 開發人員能夠立即啟動結構描述載入到 SQL Server 資料庫與[aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)命令。 執行此命令後，資料庫中建立下列資料表。

  ![成員資格資料表](identity/_static/membership-tables.png)

若要將現有的應用程式移轉至 ASP.NET Core 2.0 身分識別，這些資料表中的資料必須移轉到新的身分識別結構描述所使用的資料表。

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core 識別 2.0 結構描述

ASP.NET Core 2.0 會遵循[識別](/aspnet/identity/index)ASP.NET 4.5 中導入的原則。 雖然共用原則時，架構之間的實作是不同版本的 ASP.NET Core 之間 (請參閱[將驗證和身分識別移轉至 ASP.NET Core 2.0](xref:migration/1x-to-2x/index))。

檢視 ASP.NET Core 2.0 身分識別的結構描述最快速的方式是建立新的 ASP.NET Core 2.0 應用程式。 請遵循下列步驟，在 Visual Studio 2017:

* 選取 [檔案]  >  [新增]  >  [專案]。
* 建立新**ASP.NET Core Web 應用程式**，並將專案命名*CoreIdentitySample*。
* 在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。 此範本會產生[Razor 頁面](xref:mvc/razor-pages/index)應用程式。 再按一下**確定**，按一下 **變更驗證**。
* 選擇**個別使用者帳戶**識別範本。 最後，按一下 **確定**，然後**確定**。 Visual Studio 建立專案，使用 ASP.NET Core 識別範本。

ASP.NET Core 2.0 身分識別使用[Entity Framework Core](/ef/core)與儲存的驗證資料的資料庫互動。 為了讓新建立的應用程式運作，那里必須能夠儲存這項資料的資料庫。 之後建立新的應用程式，請檢查資料庫環境中的結構描述最快速的方式是建立使用 Entity Framework 移轉的資料庫。 此程序建立資料庫，請在本機或其他位置，這會模擬該結構描述。 檢閱先前的文件，如需詳細資訊。

若要建立 ASP.NET Core 識別結構描述資料庫，執行`Update-Database`Visual Studio 中的命令**Package Manager Console** (PMC) 視窗&mdash;位於**工具** > **NuGet 套件管理員** > **Package Manager Console**。 PMC 支援 Entity Framework 執行的命令。

實體架構命令會使用連接字串中指定之資料庫*appsettings.json*。 下列連接字串以資料庫為目標上*localhost*名為*asp net-核心識別*。 在此設定，Entity Framework 會設定為使用`DefaultConnection`連接字串。

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

此命令會建立與結構描述所指定的資料庫與應用程式初始化所需的任何資料。 下圖說明上述的步驟來建立資料表結構。

   ![識別資料表](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>移轉結構描述

有的資料表結構和成員資格和 ASP.NET Core 識別欄位的細微的差異。 驗證/授權與 ASP.NET 及 ASP.NET Core 應用程式已經大幅變更模式。 仍然會用於識別索引鍵物件*使用者*和*角色*。 以下是對應的資料表*使用者*，*角色*，和*UserRoles*。

### <a name="users"></a>使用者

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **欄位名稱** | **Type**  |   **欄位名稱** | **Type**  |
|`Id` | 字串 | `aspnet_Users.UserId` | 字串
|`UserName` | 字串 | `aspnet_Users.UserName` | 字串
|`Email` | 字串 | `aspnet_Membership.Email` | 字串
|`NormalizedUserName` | 字串 | `aspnet_Users.LoweredUserName` | 字串
|`NormalizedEmail` | 字串 | `aspnet_Membership.LoweredEmail` | 字串
|`PhoneNumber` | 字串 | `aspnet_Users.MobileAlias` | 字串
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> 並非所有的欄位對應類似 ASP.NET Core 識別要在從成員資格的一對一關聯性。 上表中預設的成員資格使用者結構描述並將它對應至 ASP.NET Core 識別結構描述。 任何其他自訂欄位所使用的成員資格都需要以手動方式對應。 在此對應中，沒有對應的密碼，因為密碼條件和密碼 salt 移轉兩者之間。 **建議讓密碼為 null，並詢問使用者重設其密碼。** 在 ASP.NET Core 身分識別`LockoutEnd`如果鎖定使用者應該設定為未來日期。移轉指令碼所示。

### <a name="roles"></a>角色

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **欄位名稱** | **Type**  |   **欄位名稱** | **Type**  |
|`Id` | 字串 | `RoleId` | 字串
|`Name` | 字串 | `RoleName` | 字串
|`NormalizedName` | 字串 | `LoweredRoleName` | 字串

### <a name="user-roles"></a>使用者角色

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **欄位名稱** | **Type**  |   **欄位名稱** | **Type**  |
|`RoleId` | 字串 | `RoleId` | 字串
|`UserId` | 字串 | `UserId` | 字串

建立的移轉指令碼時，請參考上述對應資料表*使用者*和*角色*。 下列範例假設您有兩個資料庫的資料庫伺服器上。 一個資料庫包含現有的 ASP.NET 成員資格結構描述和資料。 建立其他資料庫使用稍早所述的步驟。 註解是包含的內嵌如需詳細資訊。

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
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

此指令碼完成後稍早建立的 ASP.NET Core 識別應用程式會填入成員資格使用者。 使用者必須變更密碼才能登入。

> [!NOTE]
> 如果成員資格系統會在使用者與不符合其電子郵件地址的使用者名稱，需要進行變更來做到這一點稍早建立的應用程式。 預設範本必須要有`UserName`和`Email`相同。 它們是不同的情況下，登入程序需要修改成使用`UserName`而不是`Email`。

在`PageModel`的登入頁面上，位於*Pages\Account\Login.cshtml.cs*，移除`[EmailAddress]`屬性從*電子郵件*屬性。 它重新命名為*UserName*。 這需要變更任何地方`EmailAddress`所述，在*檢視*和*PageModel*。 結果看起來如下所示：

 ![固定的登入](identity/_static/fixed-login.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將學會如何移植 SQL 成員資格 ASP.NET Core 2.0 身分識別的使用者。 如需 ASP.NET Core 身分識別的詳細資訊，請參閱[識別簡介](xref:security/authentication/identity)。
