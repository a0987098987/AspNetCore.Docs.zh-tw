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
ms.openlocfilehash: afad542a18a357a77f4542511a3d2c3108dbfb31
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86059769"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>從 ASP.NET 成員資格驗證遷移至 ASP.NET Core 2。0Identity

作者：[Isaac Levin](https://isaaclevin.com)

本文示範如何使用成員資格驗證，將 ASP.NET apps 的資料庫架構遷移至 ASP.NET Core 2.0 Identity 。

> [!NOTE]
> 本檔提供將 ASP.NET 成員資格型應用程式的資料庫架構，遷移至用於 ASP.NET Core 的資料庫架構所需的步驟 Identity 。 如需從 ASP.NET 成員資格型驗證遷移至 ASP.NET 的詳細資訊 Identity ，請參閱將[現有的應用程式從 SQL Identity 成員資格遷移至 ASP.NET ](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)。 如需 ASP.NET Core 的詳細資訊 Identity ，請參閱[ Identity ASP.NET Core 簡介](xref:security/authentication/identity)。

## <a name="review-of-membership-schema"></a>成員資格架構的審查

在 ASP.NET 2.0 之前，開發人員負責為其應用程式建立整個驗證和授權流程。 有了 ASP.NET 2.0，就引進了成員資格，以提供可在 ASP.NET apps 中處理安全性的重複使用解決方案。 開發人員現在可以使用[aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx)命令，將架構啟動至 SQL Server 資料庫。 執行此命令之後，會在資料庫中建立下列資料表。

  ![成員資格資料表](identity/_static/membership-tables.png)

若要將現有的應用程式遷移至 ASP.NET Core 2.0 Identity ，這些資料表中的資料必須遷移至新架構所使用的資料表 Identity 。

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identity 2.0 架構

ASP.NET Core 2.0 遵循 [Identity](/aspnet/identity/index) ASP.NET 4.5 中引進的原則。 雖然原則是共用的，但架構之間的執行不同，甚至是 ASP.NET Core 的版本（請參閱[遷移驗證和 Identity ASP.NET Core 2.0](xref:migration/1x-to-2x/index)）。

若要查看 ASP.NET Core 2.0 的架構，最快的方式 Identity 就是建立新的 ASP.NET Core 2.0 應用程式。 請遵循 Visual Studio 2017 中的下列步驟：

1. 選取 [File] \(檔案\) >  [New] \(新增\) >  [Project] \(專案\)。
1. 建立名為*CoreIdentitySample*的新**ASP.NET Core Web 應用程式**專案。
1. 選取下拉式清單中的 [ **ASP.NET Core 2.0** ]，然後選取 [ **Web 應用程式**]。 此範本會產生[ Razor 頁面](xref:razor-pages/index)應用程式。 在按一下 **[確定]** 之前，請按一下 [**變更驗證**]。
1. 選擇範本的**個別使用者帳戶** Identity 。 最後，依序按一下 **[確定]** 和 **[確定]**。 Visual Studio 會使用 ASP.NET Core 範本來建立專案 Identity 。
1. 選取 [**工具**] [  >  **NuGet 套件管理員**  >  ] [**套件管理員主控台**]，以開啟 [**套件管理員主控台**] （PMC）視窗。
1. 流覽至 PMC 中的專案根目錄，然後執行[Entity Framework （EF） Core](/ef/core) `Update-Database` 命令。

    ASP.NET Core 2.0 Identity 使用 EF Core 來與儲存驗證資料的資料庫互動。 為了讓新建立的應用程式能夠正常執行，必須要有資料庫來儲存這項資料。 建立新的應用程式之後，在資料庫環境中檢查架構的最快方式就是使用[EF Core 遷移](/ef/core/managing-schemas/migrations/)來建立資料庫。 此程式會在本機或其他位置建立資料庫，以模仿該架構。 如需詳細資訊，請參閱上述檔。

    EF Core 命令會針對在*appsettings.js*中指定的資料庫使用連接字串。 下列連接字串會以名為*asp-net-identity*的*localhost*上的資料庫為目標。 在此設定中，EF Core 設定為使用 `DefaultConnection` 連接字串。

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. 選取 [ **View**  >  **SQL Server 物件總管**]。 展開對應至appsettings.js的屬性中指定之資料庫名稱的節點 `ConnectionStrings:DefaultConnection` 。 *appsettings.json*

    此 `Update-Database` 命令會建立以架構指定的資料庫，以及應用程式初始化所需的任何資料。 下圖描述使用上述步驟建立的資料表結構。

    ![Identity多](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>移轉結構描述

資料表結構和欄位中的成員資格和 ASP.NET Core 有些許差異 Identity 。 此模式已大幅變更 ASP.NET 和 ASP.NET Core 應用程式的驗證/授權。 仍然使用的主要物件 Identity 是*使用者*和*角色*。 以下是*使用者*、*角色*和*UserRoles*的對應資料表。

### <a name="users"></a>使用者

|Identity<br>（ `dbo.AspNetUsers` ）資料行  |類型     |成員資格<br>（ `dbo.aspnet_Users`  /  `dbo.aspnet_Membership` ）資料行|類型      |
|-------------------------------------------|-----------------------------------------------------------------------|
| `Id`                            | `string`| `aspnet_Users.UserId`                                      | `string` |
| `UserName`                      | `string`| `aspnet_Users.UserName`                                    | `string` |
| `Email`                         | `string`| `aspnet_Membership.Email`                                  | `string` |
| `NormalizedUserName`            | `string`| `aspnet_Users.LoweredUserName`                             | `string` |
| `NormalizedEmail`               | `string`| `aspnet_Membership.LoweredEmail`                           | `string` |
| `PhoneNumber`                   | `string`| `aspnet_Users.MobileAlias`                                 | `string` |
| `LockoutEnabled`                | `bit`   | `aspnet_Membership.IsLockedOut`                            | `bit`    |

> [!NOTE]
> 並非所有欄位對應都與從成員資格到 ASP.NET Core 的一對一關聯性類似 Identity 。 上表會採用預設成員資格使用者架構，並將它對應至 ASP.NET Core Identity 架構。 任何其他用於成員資格的自訂欄位都必須手動對應。 在此對應中，沒有任何密碼對應，因為密碼條件和密碼 salts 都不會在兩者之間遷移。 **建議您將密碼保留為 null，並要求使用者重設其密碼。** 在 ASP.NET Core 中 Identity ， `LockoutEnd` 如果使用者遭到鎖定，則應該設定為未來的某個日期。這會顯示在遷移腳本中。

### <a name="roles"></a>角色

|Identity<br>（ `dbo.AspNetRoles` ）資料行|類型|成員資格<br>（ `dbo.aspnet_Roles` ）資料行|類型|
|----------------------------------------|-----------------------------------|
|`Id`                           |`string`|`RoleId`         | `string`        |
|`Name`                         |`string`|`RoleName`       | `string`        |
|`NormalizedName`               |`string`|`LoweredRoleName`| `string`        |

### <a name="user-roles"></a>使用者角色

|Identity<br>（ `dbo.AspNetUserRoles` ）資料行|類型|成員資格<br>（ `dbo.aspnet_UsersInRoles` ）資料行|類型|
|-------------------------|----------|--------------|---------------------------|
|`RoleId`                 |`string`  |`RoleId`      |`string`                   |
|`UserId`                 |`string`  |`UserId`      |`string`                   |

建立*使用者*和*角色*的遷移腳本時，請參考上述對應表。 下列範例假設您在資料庫伺服器上有兩個資料庫。 一個資料庫包含現有的 ASP.NET 成員資格架構和資料。 另一個*CoreIdentitySample*資料庫是使用稍早所述的步驟所建立。 批註包含內嵌，以提供更多詳細資料。

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

完成上述腳本之後，稍 Identity 早建立的 ASP.NET Core 應用程式會填入成員資格使用者。 使用者必須在登入之前變更其密碼。

> [!NOTE]
> 如果成員資格系統的使用者名稱不符合其電子郵件地址，則需要對稍早建立的應用程式進行變更以配合此位置。 預設範本預期 `UserName` 和 `Email` 都是相同的。 在不同的情況下，登入程式必須修改為使用， `UserName` 而不是 `Email` 。

在登入頁面的（ `PageModel` 位於*Pages\Account\Login.cshtml.cs*）中， `[EmailAddress]` 從 [*電子郵件*] 屬性移除屬性。 將它重新命名為*UserName*。 這需要 `EmailAddress` 在*View*和*PageModel*中提及的任何位置進行變更。 結果看起來如下：

 ![已修正登入](identity/_static/fixed-login.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已瞭解如何將 SQL 成員資格的使用者移植到 ASP.NET Core 2.0 Identity 。 如需有關 ASP.NET Core 的詳細資訊 Identity ，請參閱[簡介 Identity ](xref:security/authentication/identity)。
