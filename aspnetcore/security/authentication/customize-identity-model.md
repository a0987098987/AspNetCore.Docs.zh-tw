---
title: IdentityASP.NET Core 中的模型自訂
author: ajcvickers
description: 本文說明如何自訂 ASP.NET Core 的基礎 Entity Framework Core 資料模型 Identity 。
ms.author: avickers
ms.date: 07/01/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 3a5bac0e3e34602b1f8a85a7bcde1ba92b372607
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399162"
---
# <a name="identity-model-customization-in-aspnet-core"></a>IdentityASP.NET Core 中的模型自訂

依[Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core Identity 提供在 ASP.NET Core 應用程式中管理及儲存使用者帳戶的架構。 Identity當您選取**個別使用者帳戶**做為驗證機制時，會新增至您的專案。 根據預設， Identity 會使用 Entity Framework （EF）核心資料模型。 本文說明如何自訂 Identity 模型。

## <a name="identity-and-ef-core-migrations"></a>Identity和 EF Core 遷移

在檢查模型之前，請先瞭解如何 Identity 搭配[EF Core 遷移](/ef/core/managing-schemas/migrations/)來建立和更新資料庫。 在最上層，此程式為：

1. [在程式碼中](/ef/core/modeling/)定義或更新資料模型。
1. 新增遷移，將此模型轉譯成可套用至資料庫的變更。
1. 檢查此遷移是否正確地代表您的意圖。
1. 套用「遷移」，以更新要與模型同步的資料庫。
1. 重複步驟1到4以進一步精簡模型，並讓資料庫保持同步。

使用下列其中一種方法來新增和套用遷移：

* [**套件管理員主控台**] （PMC）視窗（如果使用 Visual Studio）。 如需詳細資訊，請參閱[EF CORE PMC 工具](/ef/core/miscellaneous/cli/powershell)。
* 如果使用命令列，則為 .NET Core CLI。 如需詳細資訊，請參閱[EF Core .net 命令列工具](/ef/core/miscellaneous/cli/dotnet)。
* 在應用程式執行時，按一下 [錯誤] 頁面上的 [套用**遷移**] 按鈕。

ASP.NET Core 有開發階段錯誤頁面處理常式。 處理常式可以在應用程式執行時套用遷移。 生產應用程式通常會從遷移產生 SQL 腳本，並將資料庫變更部署為受控制應用程式和資料庫部署的一部分。

當使用建立新的應用程式時 Identity ，上述步驟1和2已完成。 也就是說，初始資料模型已經存在，而且初始遷移已加入至專案。 初始遷移仍然必須套用至資料庫。 初始遷移可透過下列其中一種方法來套用：

* `Update-Database`在 PMC 中執行。
* `dotnet ef database update`在命令 shell 中執行。
* 當應用程式執行時，按一下 [錯誤] 頁面上的 [套用**遷移**] 按鈕。

請重複上述步驟，因為對模型進行了變更。

## <a name="the-identity-model"></a>Identity模型

### <a name="entity-types"></a>實體類型

此 Identity 模型是由下列實體類型所組成。

|實體類型|說明                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |代表使用者。                                         |
|`Role`     |代表角色。                                           |
|`UserClaim`|表示使用者擁有的宣告。                    |
|`UserToken`|表示使用者的驗證權杖。               |
|`UserLogin`|將使用者與登入產生關聯。                              |
|`RoleClaim`|表示授與給角色中所有使用者的宣告。|
|`UserRole` |關聯使用者和角色的聯結實體。               |

### <a name="entity-type-relationships"></a>實體類型關聯性

[實體類型](#entity-types)會以下列方式彼此相關：

* 每個都 `User` 可以有多個 `UserClaims` 。
* 每個都 `User` 可以有多個 `UserLogins` 。
* 每個都 `User` 可以有多個 `UserTokens` 。
* 每個都 `Role` 可以有多個相關聯 `RoleClaims` 的。
* 每個都 `User` 可以有多個相關聯的 `Roles` ，而且每個都 `Role` 可以與多個相關聯 `Users` 。 這是多對多關聯性，需要資料庫中的聯結資料表。 聯結資料表是以 `UserRole` 實體表示。

### <a name="default-model-configuration"></a>預設模型設定

Identity定義許多繼承自[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)的*內容類別*，以設定和使用模型。 這項設定是在 coNtext 類別的[OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating)方法中，使用[EF CORE Code First 流暢 API](/ef/core/modeling/)來完成。 預設設定為：

```csharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a>模型泛型型別

Identity針對以上所列的每個實體類型，定義預設的[Common Language Runtime](/dotnet/standard/glossary#clr) （CLR）類型。 這些類型的前面都會加上 *Identity* ：

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

類型可以當做應用程式本身類型的基類使用，而不是直接使用這些類型。 所 `DbContext` 定義的類別 Identity 是泛型，因此可以將不同的 CLR 類型用於模型中的一或多個實體類型。 這些泛型型別也允許 `User` 變更主鍵（PK）資料類型。

搭配角色的支援使用時 Identity ， <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> 應該使用類別。 例如：

```csharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey> : IdentityDbContext<
    TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>,
    IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<
    TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>
```

您也可以使用 Identity 沒有角色的（僅限宣告），在此情況下 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> 應該使用類別：

```csharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey> : IdentityUserContext<
    TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>,
    IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<
    TUser, TKey, TUserClaim, TUserLogin, TUserToken> : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customize-the-model"></a>自訂模型

模型自訂的起點是衍生自適當的內容型別。 請參閱[模型泛型型別](#model-generic-types)一節。 此內容類型通常會被呼叫 `ApplicationDbContext` ，而且是由 ASP.NET Core 範本所建立。

內容用來以兩種方式設定模型：

* 提供泛型型別參數的實體和索引鍵類型。
* 覆寫 `OnModelCreating` 以修改這些類型的對應。

覆寫時 `OnModelCreating` ， `base.OnModelCreating` 應該先呼叫，然後再呼叫覆寫設定。 EF Core 通常會有設定的最後一次 wins 原則。 例如，如果 `ToTable` 實體類型的方法先以一個資料表名稱呼叫，然後在稍後使用不同的資料表名稱，則會使用第二個呼叫中的資料表名稱。

### <a name="custom-user-data"></a>自訂使用者資料

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

[自訂使用者資料](xref:security/authentication/add-user-data)是藉由繼承自來支援 `IdentityUser` 。 建議您將此類型命名為 `ApplicationUser` ：

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

使用 `ApplicationUser` 類型做為內容的泛型引數：

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
    }
}
```

不需要覆寫 `OnModelCreating` 類別中的 `ApplicationDbContext` 。 EF Core `CustomTag` 依慣例對應屬性。 不過，必須更新資料庫，才能建立新的資料 `CustomTag` 行。 若要建立資料行，請新增遷移，然後更新資料庫，如[ Identity 和 EF Core 遷移](#identity-and-ef-core-migrations)中所述。

更新*Pages/Shared/_LoginPartial. cshtml* ，並將取代 `IdentityUser` 為 `ApplicationUser` ：

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

更新*Areas/ Identity /IdentityHostingStartup.cs*或 `Startup.ConfigureServices` ，並將取代 `IdentityUser` 為 `ApplicationUser` 。

```csharp
services.AddIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

在 ASP.NET Core 2.1 或更新版本中， Identity 是以 Razor 類別庫的形式提供。 如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity> 。 因此，上述程式碼需要呼叫 <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*> 。 如果 Identity scaffolder 是用來將檔案加入至 Identity 專案，請移除對的呼叫 `AddDefaultUI` 。 如需詳細資訊，請參閱：

* [ScaffoldIdentity](xref:security/authentication/scaffold-identity)
* [新增、下載及刪除自訂使用者資料至Identity](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>變更主要金鑰類型

在建立資料庫之後，對 PK 資料行的資料類型所做的變更在許多資料庫系統上都有問題。 變更 PK 通常牽涉到卸載並重新建立資料表。 因此，在建立資料庫時，應該在初始遷移中指定金鑰類型。

請遵循下列步驟來變更 PK 類型：

1. 如果資料庫是在 PK 變更之前建立的，請執行 `Drop-Database` （PMC）或 `dotnet ef database drop` （.NET Core CLI）將它刪除。
2. 確認刪除資料庫之後，請使用 `Remove-Migration` （PMC）或 `dotnet ef migrations remove` （.NET Core CLI）移除初始遷移。
3. 將 `ApplicationDbContext` 類別更新為衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603> 。 為指定新的金鑰類型 `TKey` 。 例如，若要使用 `Guid` 金鑰類型：

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    ::: moniker range=">= aspnetcore-2.0"

    在上述程式碼中， <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> 必須指定泛型類別和， <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> 才能使用新的索引鍵類型。

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    在上述程式碼中， <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> 必須指定泛型類別和， <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> 才能使用新的索引鍵類型。

    ::: moniker-end

    `Startup.ConfigureServices`必須更新才能使用一般使用者：

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

4. 如果 `ApplicationUser` 正在使用自訂類別，請更新類別以繼承自 `IdentityUser` 。 例如：

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    更新 `ApplicationDbContext` 以參考自訂 `ApplicationUser` 類別：

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。

    在 ASP.NET Core 2.1 或更新版本中， Identity 是以 Razor 類別庫的形式提供。 如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity> 。 因此，上述程式碼需要呼叫 <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*> 。 如果 Identity scaffolder 是用來將檔案加入至 Identity 專案，請移除對的呼叫 `AddDefaultUI` 。

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受 `TKey` 表示主鍵之資料類型的類型。

    ::: moniker-end

5. 如果 `ApplicationRole` 正在使用自訂類別，請更新類別以繼承自 `IdentityRole<TKey>` 。 例如：

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    更新 `ApplicationDbContext` 以參考自訂 `ApplicationRole` 類別。 例如，下列類別會參考自訂 `ApplicationUser` 和自訂 `ApplicationRole` ：

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。

    在 ASP.NET Core 2.1 或更新版本中， Identity 是以 Razor 類別庫的形式提供。 如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity> 。 因此，上述程式碼需要呼叫 <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*> 。 如果 Identity scaffolder 是用來將檔案加入至 Identity 專案，請移除對的呼叫 `AddDefaultUI` 。

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受 `TKey` 表示主鍵之資料類型的類型。

    ::: moniker-end

### <a name="add-navigation-properties"></a>新增導覽屬性

變更關聯性的模型設定可能會比進行其他變更更棘手。 必須小心取代現有的關聯性，而不是建立新的其他關聯性。 特別是，已變更的關聯性必須指定與現有關聯性相同的外鍵（FK）屬性。 例如，和之間的關聯 `Users` 性 `UserClaims` 預設會依照下列方式指定：

```csharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

此關聯性的 FK 會指定為 `UserClaim.UserId` 屬性。 `HasMany`和 `WithOne` 會在沒有引數的情況下呼叫，以建立沒有導覽屬性的關聯性。

將導覽屬性加入至 `ApplicationUser` ，讓 `UserClaims` 使用者可以參考相關聯的：

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

的 `TKey` `IdentityUserClaim<TKey>` 是為使用者的 PK 指定的類型。 在此情況下， `TKey` 是， `string` 因為正在使用預設值。 這**不**是實體類型的 PK 類型 `UserClaim` 。

現在導覽屬性已經存在，必須在中設定 `OnModelCreating` ：

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

請注意，關聯性的設定方式與之前完全相同，只是在呼叫中指定的導覽屬性 `HasMany` 。

導覽屬性只存在於 EF 模型中，而非資料庫中。 因為關聯性的 FK 尚未變更，所以這種模型變更不需要更新資料庫。 這可以藉由在進行變更後新增遷移來加以檢查。 `Up`和 `Down` 方法是空的。

### <a name="add-all-user-navigation-properties"></a>新增所有使用者導覽屬性

使用上述章節做為指導方針，下列範例會為使用者的所有關聯性設定單向導覽屬性：

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="add-user-and-role-navigation-properties"></a>新增使用者和角色導覽屬性

使用上述章節做為指導方針，下列範例會設定使用者和角色上所有關聯性的導覽屬性：

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

注意：

* 此範例也包含 `UserRole` join 實體，這是將多對多關聯性從使用者導覽至角色時所需的專案。
* 請記得變更導覽屬性的類型，以反映目前使用的類型， `ApplicationXxx` 而不是 `IdentityXxx` 類型。
* 請記得 `ApplicationXxx` 在一般定義中使用 `ApplicationContext` 。

### <a name="add-all-navigation-properties"></a>新增所有導覽屬性

使用上述章節做為指引，下列範例會設定所有實體類型上所有關聯性的導覽屬性：

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });
    }
}
```

### <a name="use-composite-keys"></a>使用複合索引鍵

前面幾節會示範如何變更模型中所使用的金鑰類型 Identity 。 Identity不支援或不建議將金鑰模型變更為使用複合索引鍵。 使用複合索引鍵時， Identity 牽涉到變更 Identity 管理員程式碼與模型互動的方式。 此自訂已超出本檔的範圍。

### <a name="change-tablecolumn-names-and-facets"></a>變更資料表/資料行的名稱和 facet

若要變更資料表和資料行的名稱，請呼叫 `base.OnModelCreating` 。 然後，新增設定以覆寫任何預設值。 例如，若要變更所有資料表的名稱 Identity ：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

這些範例會使用預設 Identity 類型。 如果使用之類的應用程式類型 `ApplicationUser` ，請設定該類型，而不是預設類型。

下列範例會變更一些資料行名稱：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

某些類型的資料庫資料行可以使用特定*facet*來設定（例如，允許的最大 `string` 長度）。 下列範例會在模型中設定數個屬性的資料行最大長度 `string` ：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="map-to-a-different-schema"></a>對應至不同的架構

架構在資料庫提供者之間的行為可能不同。 針對 SQL Server，預設值是在*dbo*架構中建立所有資料表。 資料表可以在不同的架構中建立。 例如：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>消極式載入

在本節中，會加入模型中的消極式載入 proxy 支援 Identity 。 消極式載入很有用，因為它允許直接使用導覽屬性，而不需要先確定它們已載入。

實體類型可用於以數種方式進行消極式載入，如[EF Core 檔](/ef/core/querying/related-data#lazy-loading)中所述。 為了簡單起見，請使用消極式載入 proxy，這需要：

* 安裝[microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)封裝。
* 在 <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> [AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)內呼叫。
* 具有導覽屬性的公用實體類型 `public virtual` 。

下列範例示範如何 `UseLazyLoadingProxies` 在中呼叫 `Startup.ConfigureServices` ：

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

請參閱上述範例，以取得將導覽屬性加入至實體類型的指引。

## <a name="additional-resources"></a>其他資源

* <xref:security/authentication/scaffold-identity>

::: moniker-end
