---
title: 身分識別模型自訂
author: ajcvickers
description: 本文說明如何自訂 ASP.NET Core 識別為基礎的 Entity Framework Core 資料模型。
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036889"
---
# <a name="identity-model-customization"></a>身分識別模型自訂

由[Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core 身分識別提供架構供管理和儲存 ASP.NET Core 應用程式中的使用者帳戶。 「 個別使用者帳戶 」 選取作為驗證機制時，會識別加入至您的專案。 根據預設值，識別所使用的 Entity Framework (EF) 核心的資料模型。 本文說明如何自訂身分識別模型。

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>身分識別和 EF 核心移轉

先檢查模型，它是有助於您了解如何識別用於[EF 核心移轉](/ef/core/managing-schemas/migrations/)來建立和更新資料庫。 在最上層的程序是：

1. 定義或更新[程式碼中的資料模型](/ef/core/modeling/)。
1. 新增的移轉，將此模型轉譯成可以套用至資料庫的變更。
1. 請檢查移轉正確地表示您要的情況。
1. 適用於更新模型同步資料庫移轉。
1. 重複步驟 1-4，進一步精簡模型，並讓資料庫保持同步。

移轉會新增並使用套用：

* [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) 如果您使用 Visual Studio。
* [Dotnet 命令](/ef/core/miscellaneous/cli/dotnet)命令列上。
* 執行應用程式時，請選取 [錯誤] 頁面上的移轉連結。

ASP.NET Core 有可用來執行應用程式時，套用移轉開發時間錯誤網頁處理常式。 對於生產應用程式，它通常會較適用於從移轉產生 SQL 指令碼，並將這些資料庫部署為受控制的應用程式和資料庫部署的一部分。

建立新的應用程式使用識別時，已完成上述步驟 1 和 2。 也就是說，初始資料模型已存在，而且初始移轉加入專案。 初始移轉仍然需要套用至資料庫。 藉由執行更新資料庫 (PMC)，dotnet ef database 更新 (.NET Core CLI) 命令，或使用 [錯誤] 頁面，執行應用程式時，可以在完成初始的移轉。

必須模型進行的變更會重複執行上述步驟。

<a name="identity-model"></a>

## <a name="the-identity-model"></a>身分識別模型

### <a name="entity-types"></a>實體類型

身分識別模型包含七個實體類型：

* `User` -代表使用者
* `Role` -表示角色
* `UserClaim` -表示使用者必須擁有的宣告
* `UserToken` -代表使用者的驗證權杖
* `UserLogin` -將使用者與登入產生關聯
* `RoleClaim` -表示會授與角色中的所有使用者的宣告
* `UserRole` -加入將使用者和角色相關聯的實體

### <a name="entity-type-relationships"></a>實體類型的關聯性

這些實體型別與彼此相關以下列方式：

* 每個`User`都可以擁有許多 `UserClaims`
* 每個`User`都可以擁有許多 `UserLogins`
* 每個`User`都可以擁有許多 `UserTokens`
* 每個`Role`可以有許多相關聯 `RoleClaims`
* 每個`User`可以有許多相關聯`Roles`，而且每個`Role`可以有許多使用者產生關聯
  * 這是多對多關聯性，需要在資料庫中的聯結資料表。 聯結資料表由`UserRole`實體。

### <a name="default-model-configuration"></a>預設模型組態

識別定義各種不同的 「 內容類別 」 繼承自[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)來設定和使用模式。 此組態完成使用[EF 核心程式碼 First fluent 應用程式開發介面](/ef/core/modeling/)中[OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_)內容類別的方法。 預設組態是：

```CSharp
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

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
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

### <a name="model-generic-types"></a>泛型型別模型

身分識別的每個以上所列的實體類型定義預設 CLR 型別。 這些類型全部前面會加上 「 識別 」: `IdentityUser`， `IdentityRole`， `IdentityUserClaim`， `IdentityUserToken`， `IdentityUserLogin`， `IdentityRoleClaim`，和`IdentityUserRole`。

而不是直接使用這些類型，型別可以用於做為基底類別的應用程式本身的型別。 `DbContext`識別所定義的類別是泛型，不同的 CLR 型別可以用於一或多個模型中的實體類型。 這些泛型型別也允許使用者的主索引鍵的類型變更。

對於角色，使用支援的身分識別時[IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext)應該使用類別：

```CSharp
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
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
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

您也可使用身分識別，而角色 （僅 「 宣告 」） 不在此情況下`IdentityUserContext`應該使用類別：


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a>自訂模型

自訂模型的起點是衍生自適當的內容類型。請參閱上一節。 此內容類型通常稱為`ApplicationDbContext`和 ASP.NET Core 範本所建立。

您可以使用的內容設定兩種方式的模型：
* 藉由提供的泛型類型參數的實體和索引鍵類型。
* 藉由覆寫`OnModelCreating`若要修改這些類型的對應。

在覆寫`OnModelCreating`，`base.OnModelCreating`應該先呼叫，覆寫應呼叫設定下一個。 EF 核心通常會有設定的最後一個 wins 原則。 例如，如果`ToTable`實體型別會是呼叫第一次使用一個資料表名稱和不同的資料表名稱，然後再次更新版本，則第二個呼叫中的資料表名稱是用。

### <a name="using-a-custom-user-type"></a>使用自訂的使用者類型

若要使用自訂的使用者類型，建立類型，並讓它繼承自`IdentityUser`。 按照慣例來命名這個型別`ApplicationUser`。 此類型通常有不在基底類型中的其他屬性，否則會有任何值的建立。 例如: 

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

接著使用這個型別作為泛型引數內容：

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

更新`ConfigureServices`以使用新`ApplicationUser`類別：

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

不需要覆寫`OnModelCreating`這裡因為 EF 核心會對應`CustomTag`依慣例的屬性。 不過，資料庫必須更新，以取得新`CustomTag`資料行。 若要這樣做，請加入移轉，並更新資料庫中所述[身分識別和 EF 核心移轉](#identity-migrations)。

### <a name="changing-the-key-type"></a>變更索引鍵的類型

在建立資料庫之後變更主索引鍵 (PK) 資料行的類型是在許多資料庫系統上有問題。 變更在 PK 通常牽涉到卸除並重新建立資料表。 因此，建議您使用存在的初始移轉指定的索引鍵類型，以便在建立資料庫時，會建立為目標的索引鍵類型。

如果資料庫已建立，然後`Drop-Database`(PMC) 或`dotnet ef database drop`(.NET Core CLI) 可以用來將它刪除。

一旦確認資料庫不存在，移除與移轉初始`Remove-Migration`(PMC) 或`dotnet ef migrations remove`(.NET Core CLI)。

更新`ApplicationDbContext`使用不同的基底類別，指定新索引鍵類型`TKey`。 例如，若要使用`Guid`機碼：

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

請注意，泛型類別`IdentityUser<TKey>`和`IdentityRole<TKey>`也必須指定要使用新的索引鍵類型。 `ConfigureServices` 必須更新為使用一般的使用者：

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

如果自訂`ApplicationUser`是正在使用中，更新它繼承自`IdentityUser<TKey>`。 例如: 

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a>加入導覽屬性

變更關聯性的模型組態可能會比更困難進行其他變更。 小心取代現有的關聯性，而不是建立新的其他關聯性。 特別是，已變更的關聯性必須指定相同的外部索引鍵屬性，與現有的關聯性。 例如，之間的關聯性`Users`和`UserClaims`指定為預設為：

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

此關聯性的外部索引鍵指定為`UserClaim.UserId`屬性。 `HasMany` 和`WithOne`會呼叫沒有引數，若要建立沒有導覽屬性關聯性。

將導覽屬性，加入`ApplicationUser`，這將允許相關聯`UserClaims`參考使用者：

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

請注意，`TKey`如`IdentityUserClaim<TKey>`為指定的使用者-在此情況下的主索引鍵型別`string`因為我們會使用預設值。 它是**不**主索引鍵類型`UserClaim`實體類型。

現在，有導覽屬性必須設定在`OnModelCreating`:

```CSharp
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

請注意關聯性會設定完全相同，呼叫中指定的導覽屬性只能搭配`HasMany`。

導覽屬性只存在於 EF 模型，而不是資料庫中。 未變更外部索引鍵關聯性，因為這種模型變更不需要更新資料庫。 這可以透過加入進行變更後的移轉檢查：`Up`和`Down`方法是空的。

### <a name="adding-all-user-navigation-properties"></a>加入所有的使用者導覽屬性

使用前的一節，做為指南，以下是範例設定單向的導覽屬性，針對所有關聯性上使用者：

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
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

### <a name="adding-user-and-role-navigation-properties"></a>新增使用者和角色的導覽屬性

使用前的一節，做為指南，這裡是使用者和角色設定的所有關聯性的導覽屬性的範例：

```CSharp
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

```CSharp
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

* 這個範例還包括`UserRole`加入實體，才能瀏覽的使用者角色的多對多關聯性。
* 請記得要變更的導覽屬性，以反映類型`ApplicationXxx`類型現在正在使用而不是`IdentityXxx`型別。
* 請務必使用`ApplicationXxx`中泛型`ApplicationContext`定義。

### <a name="adding-all-navigation-properties"></a>加入所有導覽屬性

使用前的一節，做為指南，這裡是所有實體類型設定的所有關聯性的導覽屬性的範例：

```CSharp
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

```CSharp
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

### <a name="using-composite-keys"></a>使用複合索引鍵

如前幾節示範變更身分識別模型中使用的索引鍵的類型。 變更要使用複合索引鍵的識別索引鍵模型不支援或建議。 使用身分識別的複合索引鍵，會包含變更超出本文的範圍內的模型，這是不支援的自訂與身分識別管理程式碼互動的方式。

### <a name="changing-tablecolumn-names-and-facets"></a>變更資料表/資料行名稱和 facet

若要變更的資料表和資料行名稱，請呼叫`base.OnModelCreating`，然後再加入設定，以覆寫任何預設值。 例如，若要變更所有識別資料表的名稱：

```CSharp
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

請注意，這些範例會使用預設身分識別類型。 如果您使用應用程式類型，例如`ApplicationUser`然後設定該類型，而不是預設類型。

變更某些資料行名稱的範例如下：

```CSharp
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

某些類型的資料庫資料行可以使用特定設定_facet_例如允許的最大字串長度。 以下是範例模型中設定數個字串屬性的資料行最大長度：

```CSharp
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

### <a name="mapping-to-a-different-schema"></a>對應至不同的結構描述

結構描述的行為會在不同的資料庫提供者，但 SQL Server 的預設值是以"dbo"結構描述中建立所有的資料表。 這可以變更為改用不同的結構描述中建立的資料表。 例如: 

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>消極式載入

本節中會加入支援身分識別模型中的延遲載入 proxy。 延遲載入適合，因為它可讓沒有第一個確保載入這些要使用的導覽屬性。

實體類型可以進行適用於數種方式的消極式載入中所述[EF 核心文件集](/ef/core/querying/related-data#lazy-loading)。 為了簡單起見，我們將使用消極式載入 proxy，而這會需要：

* 安裝[Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)封裝。
* 呼叫`.UseLazyLoadingProxies()`內`AddDbContext`。
* 具有公用虛擬導覽屬性的公用實體型別。

以下是範例呼叫`.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

回頭參考上述範例中，將加入的實體類型的導覽屬性。
