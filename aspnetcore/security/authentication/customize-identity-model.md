---
title: ASP.NET Core 中的身分識別模型自訂
author: ajcvickers
description: 本文說明如何自訂 ASP.NET Core 識別為基礎的 Entity Framework Core 資料模型。
ms.author: avickers
ms.date: 07/01/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: f549fdff4a416b5fadcb2b1078b051bbab8e402e
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500481"
---
# <a name="identity-model-customization-in-aspnet-core"></a>ASP.NET Core 中的身分識別模型自訂

藉由[Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core 身分識別可供管理和儲存在 ASP.NET Core 應用程式中的使用者帳戶的架構。 身分識別新增至您的專案時**個別使用者帳戶**選取作為驗證機制。 根據預設，身分識別會使用的 Entity Framework (EF) Core 資料模型。 本文說明如何自訂身分識別模型。

## <a name="identity-and-ef-core-migrations"></a>身分識別和 EF Core 移轉

先檢查模型，它是有助於您了解如何識別用於[EF Core 移轉](/ef/core/managing-schemas/migrations/)來建立和更新資料庫。 在最上層處理程序是：

1. 定義或更新[程式碼中的資料模型](/ef/core/modeling/)。
1. 新增移轉，將此模型轉譯成可以套用至資料庫的變更。
1. 請檢查移轉正確地表示您自己的意願。
1. 適用於移轉至更新資料庫以便與模型保持同步。
1. 重複步驟 1 到 4，以進一步精簡模型，並讓資料庫保持同步。

您可以使用其中一種下列方法來新增和套用移轉：

* **Package Manager Console** (PMC) 視窗，如果使用 Visual Studio。 如需詳細資訊，請參閱 < [EF Core PMC 工具](/ef/core/miscellaneous/cli/powershell)。
* .NET Core CLI 如果使用命令列。 如需詳細資訊，請參閱 < [EF Core.NET 命令列工具](/ef/core/miscellaneous/cli/dotnet)。
* 按一下 **套用移轉**在應用程式執行時，錯誤 頁面上的按鈕。

ASP.NET Core 具有開發階段的錯誤頁面處理常式。 執行應用程式時，處理常式可以套用移轉。 生產環境應用程式通常會產生 SQL 指令碼從移轉和部署資料庫變更為受控制的應用程式和資料庫部署的一部分。

建立新的應用程式，使用身分識別時，已經完成上述步驟 1 和 2。 也就是初始資料模型已存在，並已新增至專案的初始移轉。 初始移轉仍然需要套用至資料庫。 透過下列方法之一，您可以套用初始移轉：

* 執行`Update-Database`在 PMC 中。
* 執行`dotnet ef database update`命令殼層。
* 按一下 **套用移轉**在應用程式執行時，錯誤 頁面上的按鈕。

模型的變更，請重複上述步驟。

## <a name="the-identity-model"></a>身分識別模型

### <a name="entity-types"></a>實體類型

身分識別模型是由下列的實體類型所組成。

|實體類型|描述                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |代表的使用者。                                         |
|`Role`     |表示角色。                                           |
|`UserClaim`|表示使用者擁有的宣告。                    |
|`UserToken`|代表使用者的驗證權杖。               |
|`UserLogin`|將使用者登入產生關聯。                              |
|`RoleClaim`|表示會授與角色中的所有使用者的宣告。|
|`UserRole` |聯結實體產生關聯的使用者和角色。               |

### <a name="entity-type-relationships"></a>實體型別關聯性

[實體類型](#entity-types)透過下列方式與彼此相關：

* 每個`User`可以有多個`UserClaims`。
* 每個`User`可以有多個`UserLogins`。
* 每個`User`可以有多個`UserTokens`。
* 每個`Role`可以有多個相關聯`RoleClaims`。
* 每個`User`可以有多個相關聯`Roles`，而且每個`Role`可以與許多相關聯`Users`。 這是需要在資料庫中的聯結資料表的多對多關聯性。 聯結資料表由`UserRole`實體。

### <a name="default-model-configuration"></a>預設模型組態

識別定義許多*內容類別*繼承自[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)若要設定及使用模型。 此組態完成使用[EF Core 程式碼 First fluent 應用程式開發介面](/ef/core/modeling/)中[OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating)內容類別的方法。 預設設定是：

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

### <a name="model-generic-types"></a>模型的泛型型別

識別定義預設值[Common Language Runtime](/dotnet/standard/glossary#clr)上面所列的每個實體類型 (CLR) 型別。 這些類型都會加上*識別*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

而不是直接使用這些類型，類型可以作為基底類別的應用程式本身的類型。 `DbContext`身分識別所定義的類別是泛型，使不同的 CLR 型別可以用一或多個模型中的實體類型。 這些泛型型別也允許`User`變更主索引鍵 (PK) 資料類型。

針對角色，使用支援的身分識別時<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>應該使用類別。 例如:

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

您也可使用身分識別，而角色 （僅 「 宣告 」） 不在此情況下<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601>應該使用類別：

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

模型自訂的起始點是衍生自適當的內容類型。 請參閱[模型化泛型類型](#model-generic-types)一節。 此內容類型通常稱為`ApplicationDbContext`和 ASP.NET Core 範本所建立。

內容用來設定模型有兩種：

* 提供實體和泛型型別參數的索引鍵類型。
* 覆寫`OnModelCreating`若要修改這些類型的對應。

當覆寫`OnModelCreating`，`base.OnModelCreating`應該先呼叫; 應該接著呼叫覆寫的組態。 EF Core 通常會有設定的最後一個 wins 原則。 例如，如果`ToTable`實體類型的方法稱為第一次一個資料表名稱，然後再次更新版本不同的資料表名稱，第二次呼叫中的資料表名稱會使用。

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

[自訂使用者資料](xref:security/authentication/add-user-data)支援藉由繼承自`IdentityUser`。 按照慣例命名此類型`ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

使用`ApplicationUser`作為內容的泛型引數的型別：

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

不需要覆寫`OnModelCreating`在`ApplicationDbContext`類別。 EF Core 會將對應`CustomTag`依照慣例的屬性。 不過，資料庫必須建立新更新`CustomTag`資料行。 若要建立資料行，新增移轉時，，然後更新資料庫中所述[身分識別和 EF Core 移轉](#identity-and-ef-core-migrations)。

更新*Pages/Shared/_LoginPartial.cshtml* ，並取代`IdentityUser`使用`ApplicationUser`:

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

更新*Areas/Identity/IdentityHostingStartup.cs*或是`Startup.ConfigureServices`，並取代`IdentityUser`使用`ApplicationUser`。

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

在 ASP.NET Core 2.1 或更新版本，Razor 類別庫提供身分識別。 如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity>。 因此，上述程式碼需要呼叫<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>。 如果身分識別 scaffolder 用來識別檔案加入專案，移除呼叫`AddDefaultUI`。 如需詳細資訊，請參閱:

* [Scaffold 身分識別](xref:security/authentication/scaffold-identity)
* [加入、 下載及刪除身分識別的自訂使用者資料](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>變更主索引鍵類型

在建立資料庫後的 PK 資料行的資料類型的變更是在許多資料庫系統上有問題。 變更在 PK 作業通常牽涉到卸除並重新建立資料表。 因此，金鑰類型應該指定在初始移轉時建立資料庫。

請遵循下列步驟來變更 PK 類型：

1. 如果資料庫已建立 PK 變更之前，執行`Drop-Database`(PMC) 或`dotnet ef database drop`(.NET Core CLI) 來刪除它。
2. 確認刪除資料庫之後, 移除與初始移轉`Remove-Migration`(PMC) 或`dotnet ef migrations remove`(.NET Core CLI)。
3. 更新`ApplicationDbContext`類別來衍生自<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>。 指定新的金鑰類型，如`TKey`。 例如，若要使用`Guid`金鑰類型：

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

    在上述程式碼的泛型類別<xref:Microsoft.AspNetCore.Identity.IdentityUser%601>和<xref:Microsoft.AspNetCore.Identity.IdentityRole%601>必須指定要使用新的索引鍵類型。

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    在上述程式碼的泛型類別<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601>和<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601>必須指定要使用新的索引鍵類型。

    ::: moniker-end

    `Startup.ConfigureServices` 必須更新為使用一般的使用者：

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
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

4. 如果自訂`ApplicationUser`類別正在使用中，更新類別繼承自`IdentityUser`。 例如:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    更新`ApplicationDbContext`若要參考自訂`ApplicationUser`類別：

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

    新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。

    在 ASP.NET Core 2.1 或更新版本，Razor 類別庫提供身分識別。 如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity>。 因此，上述程式碼需要呼叫<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>。 如果身分識別 scaffolder 用來識別檔案加入專案，移除呼叫`AddDefaultUI`。

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受`TKey`表示主索引鍵的資料類型的類型。

    ::: moniker-end

5. 如果自訂`ApplicationRole`類別正在使用中，更新類別繼承自`IdentityRole<TKey>`。 例如:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    更新`ApplicationDbContext`若要參考自訂`ApplicationRole`類別。 例如，下列類別會參考自訂`ApplicationUser`和自訂`ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。

    在 ASP.NET Core 2.1 或更新版本，Razor 類別庫提供身分識別。 如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity>。 因此，上述程式碼需要呼叫<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>。 如果身分識別 scaffolder 用來識別檔案加入專案，移除呼叫`AddDefaultUI`。

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受`TKey`表示主索引鍵的資料類型的類型。

    ::: moniker-end

### <a name="add-navigation-properties"></a>新增導覽屬性

變更關聯性的模型組態可能會比其他變更更困難。 小心必須取代現有的關聯性，而不是建立新的其他關聯性。 特別是，已變更的關聯性也必須指定相同的外部索引鍵 (FK) 屬性，為現有的關聯性。 比方說，之間的關係`Users`和`UserClaims`是，根據預設，指定方式如下：

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

FK 此關聯性指定為`UserClaim.UserId`屬性。 `HasMany` 和`WithOne`呼叫不含引數來建立不含導覽屬性的關聯性。

將導覽屬性，加入`ApplicationUser`，可讓相關聯`UserClaims`參考使用者：

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

`TKey`針對`IdentityUserClaim<TKey>`是指定使用者的主索引鍵的類型。 在此情況下，`TKey`是`string`因為正在使用預設值。 它有**未**PK 類型`UserClaim`實體類型。

現在，導覽屬性存在，它必須設定在`OnModelCreating`:

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

請注意關聯性已完全一樣，只與呼叫中指定的導覽屬性`HasMany`。

導覽屬性只存在於 EF 模型，而不是在資料庫中。 由於關聯性的 FK 沒有變更，這種模型變更不需要更新資料庫。 這可以藉由新增移轉，以進行變更之後檢查。 `Up`和`Down`方法是空的。

### <a name="add-all-user-navigation-properties"></a>新增所有的使用者導覽屬性

使用上的一節做為指引，下列範例會設定使用者的所有關聯性的單向導覽屬性：

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

### <a name="add-user-and-role-navigation-properties"></a>新增使用者和角色的導覽屬性

使用上的一節做為指引，下列範例會設定使用者和角色的所有關聯性的導覽屬性：

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

附註：

* 這個範例還包括`UserRole`聯結實體，才能瀏覽的使用者角色的多對多關聯性。
* 請記得要變更類型的導覽屬性反映這一點`ApplicationXxx`類型現在正在使用而不是`IdentityXxx`型別。
* 請務必使用`ApplicationXxx`在泛型`ApplicationContext`定義。

### <a name="add-all-navigation-properties"></a>新增所有導覽屬性

使用上的一節做為指引，下列範例會設定所有的實體類型上的所有關聯性的導覽屬性：

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

前幾節所示範變更身分識別模型中使用的索引鍵的類型。 變更要使用複合索引鍵的識別索引鍵模型不支援或建議。 使用身分識別的複合索引鍵，牽涉到變更身分識別管理員 」 程式碼與模型之間的互動方式。 這項自訂已超出本文的範圍。

### <a name="change-tablecolumn-names-and-facets"></a>變更資料表/資料行名稱和 facet

若要變更的資料表和資料行名稱，請呼叫`base.OnModelCreating`。 然後，新增設定，以覆寫任何預設值。 例如，若要變更的身分識別的所有資料表名稱：

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

這些範例會使用預設的身分識別類型。 如果使用的應用程式類型，例如`ApplicationUser`，設定該型別，而不是預設類型。

下列範例會變更某些資料行名稱：

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

可以使用特定設定某些類型的資料庫資料行*facet* (例如，最大值`string`允許的長度)。 下列範例會設定數個資料行的最大長度`string`模型中的屬性：

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

### <a name="map-to-a-different-schema"></a>對應到不同的結構描述

結構描述的行為會跨資料庫提供者。 SQL Server 的預設值是建立中的所有資料表*dbo*結構描述。 可以在不同的結構描述中建立資料表。 例如:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>消極式載入

在本節中，新增身分識別模型中的消極式載入 proxy 的支援。 「 延遲載入適合，因為它可讓而不用先確保他們載入的導覽屬性。

實體類型可以進行適用於數種方式的消極式載入中所述[EF Core 文件集](/ef/core/querying/related-data#lazy-loading)。 為了簡單起見，使用消極式載入 proxy，而這需要：

* 安裝[Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)封裝。
* 呼叫<xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*>內[AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)。
* 公開的實體類型與`public virtual`導覽屬性。

下列範例示範如何呼叫`UseLazyLoadingProxies`在`Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

請參閱前面的範例，如需導覽屬性加入實體類型的指引。

## <a name="additional-resources"></a>其他資源

* <xref:security/authentication/scaffold-identity>

::: moniker-end
