---
title: ASP.NET Core 身分識別的自訂儲存體提供者
author: ardalis
description: 瞭解如何設定 ASP.NET Core 身分識別的自訂儲存體提供者。
ms.author: riande
ms.custom: mvc
ms.date: 07/23/2019
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 574e66e4dedaf0bfd01d600c3ded4bfb5d1865cd
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664476"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>ASP.NET Core 身分識別的自訂儲存體提供者

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core 身分識別是可延伸的系統，可讓您建立自訂的儲存提供者，並將它連線到您的應用程式。 本主題說明如何建立 ASP.NET Core 身分識別的自訂存放裝置提供者。 其中涵蓋建立您自己的儲存提供者所需的重要概念，但不是逐步解說。

[從 GitHub 檢視或下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)。

## <a name="introduction"></a>簡介

根據預設，ASP.NET Core 身分識別系統會使用 Entity Framework Core，將使用者資訊儲存在 SQL Server 資料庫中。 對於許多應用程式而言，這種方法運作良好。 不過，您可能會想要使用不同的持續性機制或資料結構描述。 例如：

* 您會使用[Azure 表格儲存體](/azure/storage/)或另一個資料存放區。
* 您的資料庫資料表具有不同的結構。 
* 您可能想要使用不同的資料存取方法，例如[Dapper](https://github.com/StackExchange/Dapper)。 

在上述每一種情況下，您都可以為您的儲存機制撰寫自訂的提供者，並將該提供者插入您的應用程式中。

ASP.NET Core 身分識別包含在具有 [個別使用者帳戶] 選項之 Visual Studio 的專案範本中。

使用 .NET Core CLI 時，請新增 `-au Individual`：

```dotnetcli
dotnet new mvc -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>ASP.NET Core 身分識別架構

ASP.NET Core 身分識別包含稱為「管理員」和「存放區」的類別。 *管理員*是應用程式開發人員用來執行作業的高層級類別，例如建立身分識別使用者。 存放*區*是較低層級的類別，可指定實體（例如使用者和角色）的保存方式。 存放區會遵循存放庫模式，並與持續性機制緊密結合。 管理員會與存放區分離，這表示您可以取代持續性機制，而不需要變更應用程式代碼（設定除外）。

下圖顯示 web 應用程式如何與管理員互動，同時存放區會與資料存取層互動。

![ASP.NET Core 應用程式可與管理員合作（例如，' UserManager '、' RoleManager '）。 管理員可與使用 Entity Framework Core 等程式庫來與資料來源進行通訊的存放區（例如 ' UserStore '）一起使用。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

若要建立自訂存放裝置提供者，請建立資料來源、資料存取層，以及與此資料存取層互動的存放區類別（上圖中的綠色和灰色方塊）。 您不需要自訂與它們互動的管理員或應用程式程式碼（上面的藍色方塊）。

建立 `UserManager` 或 `RoleManager` 的新實例時，您會提供使用者類別的類型，並傳遞 store 類別的實例做為引數。 這種方法可讓您將自訂類別插入 ASP.NET Core。 

[重新設定應用程式以使用新的存放裝置提供者](#reconfigure-app-to-use-a-new-storage-provider)示範如何具現化 `UserManager`，並使用自訂的存放區 `RoleManager`。

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core 身分識別儲存資料類型

[ASP.NET Core 識別](https://github.com/aspnet/identity)資料類型會在下列各節中詳細說明：

### <a name="users"></a>使用者

網站的已註冊使用者。 [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser)類型可以擴充或當做您自己自訂類型的範例使用。 您不需要從特定類型繼承，即可實作為您自己的自訂身分識別儲存體解決方案。

### <a name="user-claims"></a>使用者宣告

使用者的一組語句（或[宣告](/dotnet/api/system.security.claims.claim)），代表使用者的身分識別。 可以啟用使用者身分識別的更大運算式，而不是透過角色來達成。

### <a name="user-logins"></a>使用者登入

要在使用者登入時使用的外部驗證提供者（例如 Facebook 或 Microsoft 帳戶）的相關資訊。 [範例](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>角色

網站的授權群組。 包含角色識別碼和角色名稱（例如 "Admin" 或 "Employee"）。 [範例](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>資料存取層

本主題假設您熟悉要使用的持續性機制，以及如何建立該機制的實體。 本主題不提供如何建立存放庫或資料存取類別的詳細資料;當您使用 ASP.NET Core 身分識別時，它會提供有關設計決策的一些建議。

在設計自訂存放區提供者的資料存取層時，您有很多自由。 您只需要為您想要在應用程式中使用的功能建立持續性機制。 例如，如果您未在應用程式中使用角色，則不需要建立角色或使用者角色關聯的儲存體。 您的技術和現有的基礎結構可能需要與 ASP.NET Core 身分識別的預設執行非常不同的結構。 在您的資料存取層中，您會提供邏輯來處理您的儲存體執行結構。

資料存取層提供將資料從 ASP.NET Core 身分識別儲存至資料來源的邏輯。 您自訂的儲存提供者的資料存取層可能包含下列用來儲存使用者和角色資訊的類別。

### <a name="context-class"></a>Context 類別

封裝資訊，以連接到您的持續性機制並執行查詢。 有數個數據類別需要這個類別的實例，通常是透過相依性插入來提供。 [範例](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)。

### <a name="user-storage"></a>使用者儲存體

儲存並抓取使用者資訊（例如使用者名稱和密碼雜湊）。 [範例](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>角色儲存體

儲存並抓取角色資訊（例如角色名稱）。 [範例](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>UserClaims 儲存體

儲存並抓取使用者宣告資訊（例如宣告類型和值）。 [範例](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>UserLogins 儲存體

儲存並抓取使用者登入資訊（例如外部驗證提供者）。 [範例](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>使用者角色存放裝置

儲存並抓取哪些角色會指派給哪些使用者。 [範例](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

**秘訣：** 只會執行您想要在應用程式中使用的類別。

在資料存取類別中，提供程式碼來執行持續性機制的資料作業。 例如，在自訂提供者內，您可能會有下列程式碼，在*store*類別中建立新的使用者：

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

建立使用者的執行邏輯位於 `_usersTable.CreateAsync` 方法中，如下所示。

## <a name="customize-the-user-class"></a>自訂使用者類別

在執行儲存區提供者時，建立相當於[IdentityUser 類別](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser)的使用者類別。

您的使用者類別至少必須包含 `Id` 和 `UserName` 屬性。

`IdentityUser` 類別會定義在執行要求的作業時，`UserManager` 所呼叫的屬性。 `Id` 屬性的預設類型為字串，但是您可以從 `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` 繼承並指定不同的類型。 架構預期儲存體的執行方式可以處理資料類型轉換。

## <a name="customize-the-user-store"></a>自訂使用者存放區

建立 `UserStore` 類別，以提供使用者上所有資料作業的方法。 這個類別相當於[UserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)類別。 在您的 `UserStore` 類別中，執行所需的 `IUserStore<TUser>` 和選擇性介面。 您可以根據應用程式中提供的功能，選取要執行的選擇性介面。

### <a name="optional-interfaces"></a>選擇性介面

* [IUserRoleStore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [IUserClaimStore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [IUserPasswordStore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [IUserEmailStore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IUserPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)
* [IQueryableUserStore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [IUserLoginStore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [IUserTwoFactorStore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [IUserLockoutStore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

選擇性的介面會繼承自 `IUserStore<TUser>`。 您可以在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)中看到部分實作為範例使用者存放區。

在 `UserStore` 類別中，您可以使用您所建立的資料存取類別來執行作業。 這些會使用相依性插入來傳入。 例如，在使用 Dapper 執行的 SQL Server 中，`UserStore` 類別的 `CreateAsync` 方法會使用 `DapperUsersTable` 的實例來插入新的記錄：

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>自訂使用者存放區時所要執行的介面

* **IUserStore**  
 [IUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1)介面是您必須在使用者存放區中執行的唯一介面。 它會定義用來建立、更新、刪除和抓取使用者的方法。
* **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)介面會定義您所執行的方法，以啟用使用者宣告。 其中包含新增、移除和抓取使用者宣告的方法。
* **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)定義您為了啟用外部驗證提供者而執行的方法。 其中包含加入、移除和抓取使用者登入的方法，以及根據登入資訊來抓取使用者的方法。
* **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)介面會定義您為了將使用者對應至角色而執行的方法。 其中包含新增、移除和抓取使用者角色的方法，以及檢查使用者是否已指派給角色的方法。
* **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)介面會定義您所執行的方法，以保存雜湊的密碼。 其中包含取得和設定雜湊密碼的方法，以及指出使用者是否已設定密碼的方法。
* **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)介面會定義您所執行的方法，以使用安全性戳記來指出使用者的帳戶資訊是否已變更。 當使用者變更密碼或新增或移除登入時，就會更新此戳記。 其中包含取得和設定安全性戳記的方法。
* **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)介面會定義您所執行的方法，以支援雙因素驗證。 其中包含取得和設定是否為使用者啟用雙因素驗證的方法。
* **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)介面會定義您要用來儲存使用者電話號碼的方法。 其中包含取得和設定電話號碼的方法，以及電話號碼是否已確認。
* **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)介面會定義您要用來儲存使用者電子郵件地址的方法。 其中包含取得和設定電子郵件地址的方法，以及是否確認電子郵件。
* **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)介面會定義您所執行的方法，以儲存鎖定帳戶的相關資訊。 其中包含追蹤失敗的存取嘗試和鎖定的方法。
* **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)介面會定義您所執行的成員，以提供可查詢的使用者存放區。

您只會執行應用程式所需的介面。 例如：

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim、IdentityUserLogin 和 IdentityUserRole

`Microsoft.AspNet.Identity.EntityFramework` 命名空間包含[IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)、 [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)和[IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)類別的實作為。 如果您使用這些功能，您可能會想要建立您自己的類別版本，並定義應用程式的屬性。 不過，有時候在執行基本作業（例如新增或移除使用者的宣告）時，不會將這些實體載入記憶體的效率較高。 相反地，後端存放區類別可以直接在資料來源上執行這些作業。 例如，`UserStore.GetClaimsAsync` 方法可以呼叫 `userClaimTable.FindByUserId(user.Id)` 方法，直接對該資料表執行查詢，並傳回宣告的清單。

## <a name="customize-the-role-class"></a>自訂角色類別

在執行角色儲存提供者時，您可以建立自訂角色類型。 它不需要執行特定介面，但必須有 `Id`，而且通常會有 `Name` 屬性。

以下是範例角色類別：

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>自訂角色存放區

您可以建立 `RoleStore` 類別，以提供角色上所有資料作業的方法。 這個類別相當於[RoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)類別。 在 `RoleStore` 類別中，您會執行 `IRoleStore<TRole>` 和選擇性的 `IQueryableRoleStore<TRole>` 介面。

* **IRoleStore&lt;TRole&gt;**  
 [IRoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1)介面會定義要在角色存放區類別中執行的方法。 其中包含建立、更新、刪除和抓取角色的方法。
* **RoleStore&lt;TRole&gt;**  
 若要自訂 `RoleStore`，請建立一個可執行 `IRoleStore<TRole>` 介面的類別。 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>重新設定應用程式以使用新的存放裝置提供者

一旦您已執行存放裝置提供者，您可以設定應用程式來使用它。 如果您的應用程式使用預設提供者，請將它取代為您的自訂提供者。

1. 移除 `Microsoft.AspNetCore.EntityFramework.Identity` NuGet 套件。
1. 如果存放裝置提供者位於不同的專案或封裝中，請新增其參考。
1. 使用儲存提供者命名空間的 using 語句，取代所有 `Microsoft.AspNetCore.EntityFramework.Identity` 的參考。
1. 在 `ConfigureServices` 方法中，將 `AddIdentity` 方法變更為使用您的自訂類型。 您可以針對此目的建立自己的擴充方法。 如需範例，請參閱[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) 。
1. 如果您使用角色，請更新 `RoleManager`，以使用您的 `RoleStore` 類別。
1. 將連接字串和認證更新為您的應用程式設定。

範例：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>參考

* [ASP.NET 4.x 身分識別的自訂儲存體提供者](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
* [ASP.NET Core 身分識別](https://github.com/dotnet/AspNetCore/tree/master/src/Identity)&ndash; 此存放庫包含已維護之商店提供者的社區連結。
