---
title: 身分識別模型自訂
author: ajcvickers
description: 本文說明如何自訂 ASP.NET Core 識別為基礎的 Entity Framework Core 資料模型。
monikerRange: '>= aspnetcore-2.1'
ms.author: avickers
ms.date: 04/12/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 41ea125414c5997ee36f4e312beba4ff318a4a8d
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077671"
---
# <a name="identity-model-customization"></a><span data-ttu-id="bb38e-103">身分識別模型自訂</span><span class="sxs-lookup"><span data-stu-id="bb38e-103">Identity model customization</span></span>

<span data-ttu-id="bb38e-104">由[Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="bb38e-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="bb38e-105">ASP.NET Core 身分識別提供架構供管理和儲存 ASP.NET Core 應用程式中的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="bb38e-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="bb38e-106">「 個別使用者帳戶 」 選取作為驗證機制時，會識別加入至您的專案。</span><span class="sxs-lookup"><span data-stu-id="bb38e-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="bb38e-107">根據預設值，識別所使用的 Entity Framework (EF) 核心的資料模型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="bb38e-108">本文說明如何自訂身分識別模型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="bb38e-109">身分識別和 EF Core 移轉</span><span class="sxs-lookup"><span data-stu-id="bb38e-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="bb38e-110">先檢查模型，它是有助於您了解如何識別用於[EF Core 移轉](/ef/core/managing-schemas/migrations/)來建立和更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="bb38e-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="bb38e-111">在最上層的程序是：</span><span class="sxs-lookup"><span data-stu-id="bb38e-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="bb38e-112">定義或更新[程式碼中的資料模型](/ef/core/modeling/)。</span><span class="sxs-lookup"><span data-stu-id="bb38e-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="bb38e-113">新增的移轉，將此模型轉譯成可以套用至資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="bb38e-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="bb38e-114">請檢查移轉正確地表示您要的情況。</span><span class="sxs-lookup"><span data-stu-id="bb38e-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="bb38e-115">適用於更新模型同步資料庫移轉。</span><span class="sxs-lookup"><span data-stu-id="bb38e-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="bb38e-116">重複步驟 1-4，進一步精簡模型，並讓資料庫保持同步。</span><span class="sxs-lookup"><span data-stu-id="bb38e-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="bb38e-117">移轉會新增並使用套用：</span><span class="sxs-lookup"><span data-stu-id="bb38e-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="bb38e-118">[Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) 如果您使用 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="bb38e-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="bb38e-119">[Dotnet 命令](/ef/core/miscellaneous/cli/dotnet)命令列上。</span><span class="sxs-lookup"><span data-stu-id="bb38e-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="bb38e-120">執行應用程式時，請選取 [錯誤] 頁面上的移轉連結。</span><span class="sxs-lookup"><span data-stu-id="bb38e-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="bb38e-121">ASP.NET Core 有可用來執行應用程式時，套用移轉開發時間錯誤網頁處理常式。</span><span class="sxs-lookup"><span data-stu-id="bb38e-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="bb38e-122">對於生產應用程式，它通常會較適用於從移轉產生 SQL 指令碼，並將這些資料庫部署為受控制的應用程式和資料庫部署的一部分。</span><span class="sxs-lookup"><span data-stu-id="bb38e-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="bb38e-123">建立新的應用程式使用識別時，已完成上述步驟 1 和 2。</span><span class="sxs-lookup"><span data-stu-id="bb38e-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="bb38e-124">也就是說，初始資料模型已存在，而且初始移轉加入專案。</span><span class="sxs-lookup"><span data-stu-id="bb38e-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="bb38e-125">初始移轉仍然需要套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="bb38e-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="bb38e-126">藉由執行更新資料庫 (PMC)，dotnet ef database 更新 (.NET Core CLI) 命令，或使用 [錯誤] 頁面，執行應用程式時，可以在完成初始的移轉。</span><span class="sxs-lookup"><span data-stu-id="bb38e-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="bb38e-127">必須模型進行的變更會重複執行上述步驟。</span><span class="sxs-lookup"><span data-stu-id="bb38e-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="bb38e-128">身分識別模型</span><span class="sxs-lookup"><span data-stu-id="bb38e-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="bb38e-129">實體類型</span><span class="sxs-lookup"><span data-stu-id="bb38e-129">Entity types</span></span>

<span data-ttu-id="bb38e-130">身分識別模型包含七個實體類型：</span><span class="sxs-lookup"><span data-stu-id="bb38e-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="bb38e-131">`User` -代表使用者</span><span class="sxs-lookup"><span data-stu-id="bb38e-131">`User` - represents the user</span></span>
* <span data-ttu-id="bb38e-132">`Role` -表示角色</span><span class="sxs-lookup"><span data-stu-id="bb38e-132">`Role` - represents a role</span></span>
* <span data-ttu-id="bb38e-133">`UserClaim` -表示使用者必須擁有的宣告</span><span class="sxs-lookup"><span data-stu-id="bb38e-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="bb38e-134">`UserToken` -代表使用者的驗證權杖</span><span class="sxs-lookup"><span data-stu-id="bb38e-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="bb38e-135">`UserLogin` -將使用者與登入產生關聯</span><span class="sxs-lookup"><span data-stu-id="bb38e-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="bb38e-136">`RoleClaim` -表示會授與角色中的所有使用者的宣告</span><span class="sxs-lookup"><span data-stu-id="bb38e-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="bb38e-137">`UserRole` -加入將使用者和角色相關聯的實體</span><span class="sxs-lookup"><span data-stu-id="bb38e-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="bb38e-138">實體類型的關聯性</span><span class="sxs-lookup"><span data-stu-id="bb38e-138">Entity type relationships</span></span>

<span data-ttu-id="bb38e-139">這些實體型別與彼此相關以下列方式：</span><span class="sxs-lookup"><span data-stu-id="bb38e-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="bb38e-140">每個`User`都可以擁有許多 `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="bb38e-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="bb38e-141">每個`User`都可以擁有許多 `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="bb38e-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="bb38e-142">每個`User`都可以擁有許多 `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="bb38e-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="bb38e-143">每個`Role`可以有許多相關聯 `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="bb38e-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="bb38e-144">每個`User`可以有許多相關聯`Roles`，而且每個`Role`可以有許多使用者產生關聯</span><span class="sxs-lookup"><span data-stu-id="bb38e-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="bb38e-145">這是多對多關聯性，需要在資料庫中的聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="bb38e-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="bb38e-146">聯結資料表由`UserRole`實體。</span><span class="sxs-lookup"><span data-stu-id="bb38e-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="bb38e-147">預設模型組態</span><span class="sxs-lookup"><span data-stu-id="bb38e-147">Default model configuration</span></span>

<span data-ttu-id="bb38e-148">識別定義各種不同的 「 內容類別 」 繼承自[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)來設定和使用模式。</span><span class="sxs-lookup"><span data-stu-id="bb38e-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="bb38e-149">此組態完成使用[EF Core 程式碼 First fluent 應用程式開發介面](/ef/core/modeling/)中[OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_)內容類別的方法。</span><span class="sxs-lookup"><span data-stu-id="bb38e-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="bb38e-150">預設組態是：</span><span class="sxs-lookup"><span data-stu-id="bb38e-150">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="bb38e-151">泛型型別模型</span><span class="sxs-lookup"><span data-stu-id="bb38e-151">Model generic types</span></span>

<span data-ttu-id="bb38e-152">身分識別的每個以上所列的實體類型定義預設 CLR 型別。</span><span class="sxs-lookup"><span data-stu-id="bb38e-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="bb38e-153">這些類型全部前面會加上 「 識別 」: `IdentityUser`， `IdentityRole`， `IdentityUserClaim`， `IdentityUserToken`， `IdentityUserLogin`， `IdentityRoleClaim`，和`IdentityUserRole`。</span><span class="sxs-lookup"><span data-stu-id="bb38e-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="bb38e-154">而不是直接使用這些類型，型別可以用於做為基底類別的應用程式本身的型別。</span><span class="sxs-lookup"><span data-stu-id="bb38e-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="bb38e-155">`DbContext`識別所定義的類別是泛型，不同的 CLR 型別可以用於一或多個模型中的實體類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="bb38e-156">這些泛型型別也允許使用者的主索引鍵的類型變更。</span><span class="sxs-lookup"><span data-stu-id="bb38e-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="bb38e-157">對於角色，使用支援的身分識別時[IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext)應該使用類別：</span><span class="sxs-lookup"><span data-stu-id="bb38e-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

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

<span data-ttu-id="bb38e-158">您也可使用身分識別，而角色 （僅 「 宣告 」） 不在此情況下`IdentityUserContext`應該使用類別：</span><span class="sxs-lookup"><span data-stu-id="bb38e-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


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

## <a name="customizing-the-model"></a><span data-ttu-id="bb38e-159">自訂模型</span><span class="sxs-lookup"><span data-stu-id="bb38e-159">Customizing the model</span></span>

<span data-ttu-id="bb38e-160">自訂模型的起點是衍生自適當的內容類型。請參閱上一節。</span><span class="sxs-lookup"><span data-stu-id="bb38e-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="bb38e-161">此內容類型通常稱為`ApplicationDbContext`和 ASP.NET Core 範本所建立。</span><span class="sxs-lookup"><span data-stu-id="bb38e-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="bb38e-162">您可以使用的內容設定兩種方式的模型：</span><span class="sxs-lookup"><span data-stu-id="bb38e-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="bb38e-163">藉由提供的泛型類型參數的實體和索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="bb38e-164">藉由覆寫`OnModelCreating`若要修改這些類型的對應。</span><span class="sxs-lookup"><span data-stu-id="bb38e-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="bb38e-165">在覆寫`OnModelCreating`，`base.OnModelCreating`應該先呼叫，覆寫應呼叫設定下一個。</span><span class="sxs-lookup"><span data-stu-id="bb38e-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="bb38e-166">EF Core 通常會有設定的最後一個 wins 原則。</span><span class="sxs-lookup"><span data-stu-id="bb38e-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="bb38e-167">例如，如果`ToTable`實體型別會是呼叫第一次使用一個資料表名稱和不同的資料表名稱，然後再次更新版本，則第二個呼叫中的資料表名稱是用。</span><span class="sxs-lookup"><span data-stu-id="bb38e-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="bb38e-168">使用自訂的使用者類型</span><span class="sxs-lookup"><span data-stu-id="bb38e-168">Using a custom User type</span></span>

<span data-ttu-id="bb38e-169">若要使用自訂的使用者類型，建立類型，並讓它繼承自`IdentityUser`。</span><span class="sxs-lookup"><span data-stu-id="bb38e-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="bb38e-170">按照慣例來命名這個型別`ApplicationUser`。</span><span class="sxs-lookup"><span data-stu-id="bb38e-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="bb38e-171">此類型通常有不在基底類型中的其他屬性，否則會有任何值的建立。</span><span class="sxs-lookup"><span data-stu-id="bb38e-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="bb38e-172">例如: </span><span class="sxs-lookup"><span data-stu-id="bb38e-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="bb38e-173">接著使用這個型別作為泛型引數內容：</span><span class="sxs-lookup"><span data-stu-id="bb38e-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="bb38e-174">更新`ConfigureServices`以使用新`ApplicationUser`類別：</span><span class="sxs-lookup"><span data-stu-id="bb38e-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="bb38e-175">不需要覆寫`OnModelCreating`這裡因為 EF Core 會對應`CustomTag`依慣例的屬性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="bb38e-176">不過，資料庫必須更新，以取得新`CustomTag`資料行。</span><span class="sxs-lookup"><span data-stu-id="bb38e-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="bb38e-177">若要這樣做，請加入移轉，並更新資料庫中所述[身分識別和 EF Core 移轉](#identity-migrations)。</span><span class="sxs-lookup"><span data-stu-id="bb38e-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="bb38e-178">變更索引鍵的類型</span><span class="sxs-lookup"><span data-stu-id="bb38e-178">Changing the key type</span></span>

<span data-ttu-id="bb38e-179">在建立資料庫之後變更主索引鍵 (PK) 資料行的類型是在許多資料庫系統上有問題。</span><span class="sxs-lookup"><span data-stu-id="bb38e-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="bb38e-180">變更在 PK 通常牽涉到卸除並重新建立資料表。</span><span class="sxs-lookup"><span data-stu-id="bb38e-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="bb38e-181">因此，建議您使用存在的初始移轉指定的索引鍵類型，以便在建立資料庫時，會建立為目標的索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="bb38e-182">如果資料庫已建立，然後`Drop-Database`(PMC) 或`dotnet ef database drop`(.NET Core CLI) 可以用來將它刪除。</span><span class="sxs-lookup"><span data-stu-id="bb38e-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="bb38e-183">一旦確認資料庫不存在，移除與移轉初始`Remove-Migration`(PMC) 或`dotnet ef migrations remove`(.NET Core CLI)。</span><span class="sxs-lookup"><span data-stu-id="bb38e-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="bb38e-184">更新`ApplicationDbContext`使用不同的基底類別，指定新索引鍵類型`TKey`。</span><span class="sxs-lookup"><span data-stu-id="bb38e-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="bb38e-185">例如，若要使用`Guid`機碼：</span><span class="sxs-lookup"><span data-stu-id="bb38e-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="bb38e-186">請注意，泛型類別`IdentityUser<TKey>`和`IdentityRole<TKey>`也必須指定要使用新的索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="bb38e-187">`ConfigureServices` 必須更新為使用一般的使用者：</span><span class="sxs-lookup"><span data-stu-id="bb38e-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="bb38e-188">如果自訂`ApplicationUser`是正在使用中，更新它繼承自`IdentityUser<TKey>`。</span><span class="sxs-lookup"><span data-stu-id="bb38e-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="bb38e-189">例如: </span><span class="sxs-lookup"><span data-stu-id="bb38e-189">For example:</span></span>

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

### <a name="adding-navigation-properties"></a><span data-ttu-id="bb38e-190">加入導覽屬性</span><span class="sxs-lookup"><span data-stu-id="bb38e-190">Adding navigation properties</span></span>

<span data-ttu-id="bb38e-191">變更關聯性的模型組態可能會比更困難進行其他變更。</span><span class="sxs-lookup"><span data-stu-id="bb38e-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="bb38e-192">小心取代現有的關聯性，而不是建立新的其他關聯性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="bb38e-193">特別是，已變更的關聯性必須指定相同的外部索引鍵屬性，與現有的關聯性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="bb38e-194">例如，之間的關聯性`Users`和`UserClaims`指定為預設為：</span><span class="sxs-lookup"><span data-stu-id="bb38e-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

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

<span data-ttu-id="bb38e-195">此關聯性的外部索引鍵指定為`UserClaim.UserId`屬性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="bb38e-196">`HasMany` 和`WithOne`會呼叫沒有引數，若要建立沒有導覽屬性關聯性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="bb38e-197">將導覽屬性，加入`ApplicationUser`，這將允許相關聯`UserClaims`參考使用者：</span><span class="sxs-lookup"><span data-stu-id="bb38e-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="bb38e-198">請注意，`TKey`如`IdentityUserClaim<TKey>`為指定的使用者-在此情況下的主索引鍵型別`string`因為我們會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="bb38e-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="bb38e-199">它是**不**主索引鍵類型`UserClaim`實體類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="bb38e-200">現在，有導覽屬性必須設定在`OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="bb38e-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="bb38e-201">請注意關聯性會設定完全相同，呼叫中指定的導覽屬性只能搭配`HasMany`。</span><span class="sxs-lookup"><span data-stu-id="bb38e-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="bb38e-202">導覽屬性只存在於 EF 模型，而不是資料庫中。</span><span class="sxs-lookup"><span data-stu-id="bb38e-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="bb38e-203">未變更外部索引鍵關聯性，因為這種模型變更不需要更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="bb38e-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="bb38e-204">這可以透過加入進行變更後的移轉檢查：`Up`和`Down`方法是空的。</span><span class="sxs-lookup"><span data-stu-id="bb38e-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="bb38e-205">加入所有的使用者導覽屬性</span><span class="sxs-lookup"><span data-stu-id="bb38e-205">Adding all User navigation properties</span></span>

<span data-ttu-id="bb38e-206">使用前的一節，做為指南，以下是範例設定單向的導覽屬性，針對所有關聯性上使用者：</span><span class="sxs-lookup"><span data-stu-id="bb38e-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="bb38e-207">新增使用者和角色的導覽屬性</span><span class="sxs-lookup"><span data-stu-id="bb38e-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="bb38e-208">使用前的一節，做為指南，這裡是使用者和角色設定的所有關聯性的導覽屬性的範例：</span><span class="sxs-lookup"><span data-stu-id="bb38e-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="bb38e-209">附註：</span><span class="sxs-lookup"><span data-stu-id="bb38e-209">Notes:</span></span>

* <span data-ttu-id="bb38e-210">這個範例還包括`UserRole`加入實體，才能瀏覽的使用者角色的多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="bb38e-211">請記得要變更的導覽屬性，以反映類型`ApplicationXxx`類型現在正在使用而不是`IdentityXxx`型別。</span><span class="sxs-lookup"><span data-stu-id="bb38e-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="bb38e-212">請務必使用`ApplicationXxx`中泛型`ApplicationContext`定義。</span><span class="sxs-lookup"><span data-stu-id="bb38e-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="bb38e-213">加入所有導覽屬性</span><span class="sxs-lookup"><span data-stu-id="bb38e-213">Adding all navigation properties</span></span>

<span data-ttu-id="bb38e-214">使用前的一節，做為指南，這裡是所有實體類型設定的所有關聯性的導覽屬性的範例：</span><span class="sxs-lookup"><span data-stu-id="bb38e-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="using-composite-keys"></a><span data-ttu-id="bb38e-215">使用複合索引鍵</span><span class="sxs-lookup"><span data-stu-id="bb38e-215">Using composite keys</span></span>

<span data-ttu-id="bb38e-216">如前幾節示範變更身分識別模型中使用的索引鍵的類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="bb38e-217">變更要使用複合索引鍵的識別索引鍵模型不支援或建議。</span><span class="sxs-lookup"><span data-stu-id="bb38e-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="bb38e-218">使用身分識別的複合索引鍵，會包含變更超出本文的範圍內的模型，這是不支援的自訂與身分識別管理程式碼互動的方式。</span><span class="sxs-lookup"><span data-stu-id="bb38e-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="bb38e-219">變更資料表/資料行名稱和 facet</span><span class="sxs-lookup"><span data-stu-id="bb38e-219">Changing table/column names and facets</span></span>

<span data-ttu-id="bb38e-220">若要變更的資料表和資料行名稱，請呼叫`base.OnModelCreating`，然後再加入設定，以覆寫任何預設值。</span><span class="sxs-lookup"><span data-stu-id="bb38e-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="bb38e-221">例如，若要變更所有識別資料表的名稱：</span><span class="sxs-lookup"><span data-stu-id="bb38e-221">For example, to change the name of all the identity tables:</span></span>

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

<span data-ttu-id="bb38e-222">請注意，這些範例會使用預設身分識別類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="bb38e-223">如果您使用應用程式類型，例如`ApplicationUser`然後設定該類型，而不是預設類型。</span><span class="sxs-lookup"><span data-stu-id="bb38e-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="bb38e-224">變更某些資料行名稱的範例如下：</span><span class="sxs-lookup"><span data-stu-id="bb38e-224">Here is an example that changes some column names:</span></span>

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

<span data-ttu-id="bb38e-225">某些類型的資料庫資料行可以使用特定設定_facet_例如允許的最大字串長度。</span><span class="sxs-lookup"><span data-stu-id="bb38e-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="bb38e-226">以下是範例模型中設定數個字串屬性的資料行最大長度：</span><span class="sxs-lookup"><span data-stu-id="bb38e-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="bb38e-227">對應至不同的結構描述</span><span class="sxs-lookup"><span data-stu-id="bb38e-227">Mapping to a different schema</span></span>

<span data-ttu-id="bb38e-228">結構描述的行為會在不同的資料庫提供者，但 SQL Server 的預設值是以"dbo"結構描述中建立所有的資料表。</span><span class="sxs-lookup"><span data-stu-id="bb38e-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="bb38e-229">這可以變更為改用不同的結構描述中建立的資料表。</span><span class="sxs-lookup"><span data-stu-id="bb38e-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="bb38e-230">例如: </span><span class="sxs-lookup"><span data-stu-id="bb38e-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="bb38e-231">消極式載入</span><span class="sxs-lookup"><span data-stu-id="bb38e-231">Lazy loading</span></span>

<span data-ttu-id="bb38e-232">本節中會加入支援身分識別模型中的延遲載入 proxy。</span><span class="sxs-lookup"><span data-stu-id="bb38e-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="bb38e-233">延遲載入適合，因為它可讓沒有第一個確保載入這些要使用的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="bb38e-234">實體類型可以進行適用於數種方式的消極式載入中所述[EF Core 文件集](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="bb38e-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="bb38e-235">為了簡單起見，我們將使用消極式載入 proxy，而這會需要：</span><span class="sxs-lookup"><span data-stu-id="bb38e-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="bb38e-236">安裝[Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)封裝。</span><span class="sxs-lookup"><span data-stu-id="bb38e-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="bb38e-237">呼叫`.UseLazyLoadingProxies()`內`AddDbContext`。</span><span class="sxs-lookup"><span data-stu-id="bb38e-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="bb38e-238">具有公用虛擬導覽屬性的公用實體型別。</span><span class="sxs-lookup"><span data-stu-id="bb38e-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="bb38e-239">以下是範例呼叫`.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="bb38e-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="bb38e-240">回頭參考上述範例中，將加入的實體類型的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="bb38e-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
