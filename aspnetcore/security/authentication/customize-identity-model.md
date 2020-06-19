---
title: IdentityASP.NET Core 中的模型自訂
author: ajcvickers
description: 本文說明如何自訂 ASP.NET Core 的基礎 Entity Framework Core 資料模型 Identity 。
ms.author: avickers
ms.date: 07/01/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 96ee703da4ced69c5d9c703139e33b76b5dcdff1
ms.sourcegitcommit: 4437f4c149f1ef6c28796dcfaa2863b4c088169c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85074147"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Identity<span data-ttu-id="1c921-103">ASP.NET Core 中的模型自訂</span><span class="sxs-lookup"><span data-stu-id="1c921-103"> model customization in ASP.NET Core</span></span>

<span data-ttu-id="1c921-104">依[Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="1c921-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="1c921-105">ASP.NET Core Identity 提供在 ASP.NET Core 應用程式中管理及儲存使用者帳戶的架構。</span><span class="sxs-lookup"><span data-stu-id="1c921-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core apps.</span></span> Identity<span data-ttu-id="1c921-106">當您選取**個別使用者帳戶**做為驗證機制時，會新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="1c921-106"> is added to your project when **Individual User Accounts** is selected as the authentication mechanism.</span></span> <span data-ttu-id="1c921-107">根據預設， Identity 會使用 Entity Framework （EF）核心資料模型。</span><span class="sxs-lookup"><span data-stu-id="1c921-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="1c921-108">本文說明如何自訂 Identity 模型。</span><span class="sxs-lookup"><span data-stu-id="1c921-108">This article describes how to customize the Identity model.</span></span>

## <a name="identity-and-ef-core-migrations"></a>Identity<span data-ttu-id="1c921-109">和 EF Core 遷移</span><span class="sxs-lookup"><span data-stu-id="1c921-109"> and EF Core Migrations</span></span>

<span data-ttu-id="1c921-110">在檢查模型之前，請先瞭解如何 Identity 搭配[EF Core 遷移](/ef/core/managing-schemas/migrations/)來建立和更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c921-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="1c921-111">在最上層，此程式為：</span><span class="sxs-lookup"><span data-stu-id="1c921-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="1c921-112">[在程式碼中](/ef/core/modeling/)定義或更新資料模型。</span><span class="sxs-lookup"><span data-stu-id="1c921-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="1c921-113">新增遷移，將此模型轉譯成可套用至資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="1c921-113">Add a Migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="1c921-114">檢查此遷移是否正確地代表您的意圖。</span><span class="sxs-lookup"><span data-stu-id="1c921-114">Check that the Migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="1c921-115">套用「遷移」，以更新要與模型同步的資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c921-115">Apply the Migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="1c921-116">重複步驟1到4以進一步精簡模型，並讓資料庫保持同步。</span><span class="sxs-lookup"><span data-stu-id="1c921-116">Repeat steps 1 through 4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="1c921-117">使用下列其中一種方法來新增和套用遷移：</span><span class="sxs-lookup"><span data-stu-id="1c921-117">Use one of the following approaches to add and apply Migrations:</span></span>

* <span data-ttu-id="1c921-118">[**套件管理員主控台**] （PMC）視窗（如果使用 Visual Studio）。</span><span class="sxs-lookup"><span data-stu-id="1c921-118">The **Package Manager Console** (PMC) window if using Visual Studio.</span></span> <span data-ttu-id="1c921-119">如需詳細資訊，請參閱[EF CORE PMC 工具](/ef/core/miscellaneous/cli/powershell)。</span><span class="sxs-lookup"><span data-stu-id="1c921-119">For more information, see [EF Core PMC tools](/ef/core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="1c921-120">如果使用命令列，則為 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="1c921-120">The .NET Core CLI if using the command line.</span></span> <span data-ttu-id="1c921-121">如需詳細資訊，請參閱[EF Core .net 命令列工具](/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="1c921-121">For more information, see [EF Core .NET command line tools](/ef/core/miscellaneous/cli/dotnet).</span></span>
* <span data-ttu-id="1c921-122">在應用程式執行時，按一下 [錯誤] 頁面上的 [套用**遷移**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1c921-122">Clicking the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="1c921-123">ASP.NET Core 有開發階段錯誤頁面處理常式。</span><span class="sxs-lookup"><span data-stu-id="1c921-123">ASP.NET Core has a development-time error page handler.</span></span> <span data-ttu-id="1c921-124">處理常式可以在應用程式執行時套用遷移。</span><span class="sxs-lookup"><span data-stu-id="1c921-124">The handler can apply migrations when the app is run.</span></span> <span data-ttu-id="1c921-125">生產應用程式通常會從遷移產生 SQL 腳本，並將資料庫變更部署為受控制應用程式和資料庫部署的一部分。</span><span class="sxs-lookup"><span data-stu-id="1c921-125">Production apps typically generate SQL scripts from the migrations and deploy database changes as part of a controlled app and database deployment.</span></span>

<span data-ttu-id="1c921-126">當使用建立新的應用程式時 Identity ，上述步驟1和2已完成。</span><span class="sxs-lookup"><span data-stu-id="1c921-126">When a new app using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="1c921-127">也就是說，初始資料模型已經存在，而且初始遷移已加入至專案。</span><span class="sxs-lookup"><span data-stu-id="1c921-127">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="1c921-128">初始遷移仍然必須套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c921-128">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="1c921-129">初始遷移可透過下列其中一種方法來套用：</span><span class="sxs-lookup"><span data-stu-id="1c921-129">The initial migration can be applied via one of the following approaches:</span></span>

* <span data-ttu-id="1c921-130">`Update-Database`在 PMC 中執行。</span><span class="sxs-lookup"><span data-stu-id="1c921-130">Run `Update-Database` in PMC.</span></span>
* <span data-ttu-id="1c921-131">`dotnet ef database update`在命令 shell 中執行。</span><span class="sxs-lookup"><span data-stu-id="1c921-131">Run `dotnet ef database update` in a command shell.</span></span>
* <span data-ttu-id="1c921-132">當應用程式執行時，按一下 [錯誤] 頁面上的 [套用**遷移**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1c921-132">Click the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="1c921-133">請重複上述步驟，因為對模型進行了變更。</span><span class="sxs-lookup"><span data-stu-id="1c921-133">Repeat the preceding steps as changes are made to the model.</span></span>

## <a name="the-identity-model"></a><span data-ttu-id="1c921-134">Identity模型</span><span class="sxs-lookup"><span data-stu-id="1c921-134">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="1c921-135">實體類型</span><span class="sxs-lookup"><span data-stu-id="1c921-135">Entity types</span></span>

<span data-ttu-id="1c921-136">此 Identity 模型是由下列實體類型所組成。</span><span class="sxs-lookup"><span data-stu-id="1c921-136">The Identity model consists of the following entity types.</span></span>

|<span data-ttu-id="1c921-137">實體類型</span><span class="sxs-lookup"><span data-stu-id="1c921-137">Entity type</span></span>|<span data-ttu-id="1c921-138">描述</span><span class="sxs-lookup"><span data-stu-id="1c921-138">Description</span></span>                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |<span data-ttu-id="1c921-139">代表使用者。</span><span class="sxs-lookup"><span data-stu-id="1c921-139">Represents the user.</span></span>                                         |
|`Role`     |<span data-ttu-id="1c921-140">代表角色。</span><span class="sxs-lookup"><span data-stu-id="1c921-140">Represents a role.</span></span>                                           |
|`UserClaim`|<span data-ttu-id="1c921-141">表示使用者擁有的宣告。</span><span class="sxs-lookup"><span data-stu-id="1c921-141">Represents a claim that a user possesses.</span></span>                    |
|`UserToken`|<span data-ttu-id="1c921-142">表示使用者的驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="1c921-142">Represents an authentication token for a user.</span></span>               |
|`UserLogin`|<span data-ttu-id="1c921-143">將使用者與登入產生關聯。</span><span class="sxs-lookup"><span data-stu-id="1c921-143">Associates a user with a login.</span></span>                              |
|`RoleClaim`|<span data-ttu-id="1c921-144">表示授與給角色中所有使用者的宣告。</span><span class="sxs-lookup"><span data-stu-id="1c921-144">Represents a claim that's granted to all users within a role.</span></span>|
|`UserRole` |<span data-ttu-id="1c921-145">關聯使用者和角色的聯結實體。</span><span class="sxs-lookup"><span data-stu-id="1c921-145">A join entity that associates users and roles.</span></span>               |

### <a name="entity-type-relationships"></a><span data-ttu-id="1c921-146">實體類型關聯性</span><span class="sxs-lookup"><span data-stu-id="1c921-146">Entity type relationships</span></span>

<span data-ttu-id="1c921-147">[實體類型](#entity-types)會以下列方式彼此相關：</span><span class="sxs-lookup"><span data-stu-id="1c921-147">The [entity types](#entity-types) are related to each other in the following ways:</span></span>

* <span data-ttu-id="1c921-148">每個都 `User` 可以有多個 `UserClaims` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-148">Each `User` can have many `UserClaims`.</span></span>
* <span data-ttu-id="1c921-149">每個都 `User` 可以有多個 `UserLogins` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-149">Each `User` can have many `UserLogins`.</span></span>
* <span data-ttu-id="1c921-150">每個都 `User` 可以有多個 `UserTokens` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-150">Each `User` can have many `UserTokens`.</span></span>
* <span data-ttu-id="1c921-151">每個都 `Role` 可以有多個相關聯 `RoleClaims` 的。</span><span class="sxs-lookup"><span data-stu-id="1c921-151">Each `Role` can have many associated `RoleClaims`.</span></span>
* <span data-ttu-id="1c921-152">每個都 `User` 可以有多個相關聯的 `Roles` ，而且每個都 `Role` 可以與多個相關聯 `Users` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-152">Each `User` can have many associated `Roles`, and each `Role` can be associated with many `Users`.</span></span> <span data-ttu-id="1c921-153">這是多對多關聯性，需要資料庫中的聯結資料表。</span><span class="sxs-lookup"><span data-stu-id="1c921-153">This is a many-to-many relationship that requires a join table in the database.</span></span> <span data-ttu-id="1c921-154">聯結資料表是以 `UserRole` 實體表示。</span><span class="sxs-lookup"><span data-stu-id="1c921-154">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="1c921-155">預設模型設定</span><span class="sxs-lookup"><span data-stu-id="1c921-155">Default model configuration</span></span>

Identity<span data-ttu-id="1c921-156">定義許多繼承自[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)的*內容類別*，以設定和使用模型。</span><span class="sxs-lookup"><span data-stu-id="1c921-156"> defines many *context classes* that inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="1c921-157">這項設定是在 coNtext 類別的[OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating)方法中，使用[EF CORE Code First 流暢 API](/ef/core/modeling/)來完成。</span><span class="sxs-lookup"><span data-stu-id="1c921-157">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) method of the context class.</span></span> <span data-ttu-id="1c921-158">預設設定為：</span><span class="sxs-lookup"><span data-stu-id="1c921-158">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="1c921-159">模型泛型型別</span><span class="sxs-lookup"><span data-stu-id="1c921-159">Model generic types</span></span>

Identity<span data-ttu-id="1c921-160">針對以上所列的每個實體類型，定義預設的[Common Language Runtime](/dotnet/standard/glossary#clr) （CLR）類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-160"> defines default [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) types for each of the entity types listed above.</span></span> <span data-ttu-id="1c921-161">這些類型的前面都會加上 *Identity* ：</span><span class="sxs-lookup"><span data-stu-id="1c921-161">These types are all prefixed with *Identity*:</span></span>

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

<span data-ttu-id="1c921-162">類型可以當做應用程式本身類型的基類使用，而不是直接使用這些類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-162">Rather than using these types directly, the types can be used as base classes for the app's own types.</span></span> <span data-ttu-id="1c921-163">所 `DbContext` 定義的類別 Identity 是泛型，因此可以將不同的 CLR 類型用於模型中的一或多個實體類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-163">The `DbContext` classes defined by Identity are generic, such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="1c921-164">這些泛型型別也允許 `User` 變更主鍵（PK）資料類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-164">These generic types also allow the `User` primary key (PK) data type to be changed.</span></span>

<span data-ttu-id="1c921-165">搭配角色的支援使用時 Identity ， <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> 應該使用類別。</span><span class="sxs-lookup"><span data-stu-id="1c921-165">When using Identity with support for roles, an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> class should be used.</span></span> <span data-ttu-id="1c921-166">例如：</span><span class="sxs-lookup"><span data-stu-id="1c921-166">For example:</span></span>

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

<span data-ttu-id="1c921-167">您也可以使用 Identity 沒有角色的（僅限宣告），在此情況下 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> 應該使用類別：</span><span class="sxs-lookup"><span data-stu-id="1c921-167">It's also possible to use Identity without roles (only claims), in which case an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> class should be used:</span></span>

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

## <a name="customize-the-model"></a><span data-ttu-id="1c921-168">自訂模型</span><span class="sxs-lookup"><span data-stu-id="1c921-168">Customize the model</span></span>

<span data-ttu-id="1c921-169">模型自訂的起點是衍生自適當的內容型別。</span><span class="sxs-lookup"><span data-stu-id="1c921-169">The starting point for model customization is to derive from the appropriate context type.</span></span> <span data-ttu-id="1c921-170">請參閱[模型泛型型別](#model-generic-types)一節。</span><span class="sxs-lookup"><span data-stu-id="1c921-170">See the [Model generic types](#model-generic-types) section.</span></span> <span data-ttu-id="1c921-171">此內容類型通常會被呼叫 `ApplicationDbContext` ，而且是由 ASP.NET Core 範本所建立。</span><span class="sxs-lookup"><span data-stu-id="1c921-171">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="1c921-172">內容用來以兩種方式設定模型：</span><span class="sxs-lookup"><span data-stu-id="1c921-172">The context is used to configure the model in two ways:</span></span>

* <span data-ttu-id="1c921-173">提供泛型型別參數的實體和索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-173">Supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="1c921-174">覆寫 `OnModelCreating` 以修改這些類型的對應。</span><span class="sxs-lookup"><span data-stu-id="1c921-174">Overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="1c921-175">覆寫時 `OnModelCreating` ， `base.OnModelCreating` 應該先呼叫，然後再呼叫覆寫設定。</span><span class="sxs-lookup"><span data-stu-id="1c921-175">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first; the overriding configuration should be called next.</span></span> <span data-ttu-id="1c921-176">EF Core 通常會有設定的最後一次 wins 原則。</span><span class="sxs-lookup"><span data-stu-id="1c921-176">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="1c921-177">例如，如果 `ToTable` 實體類型的方法先以一個資料表名稱呼叫，然後在稍後使用不同的資料表名稱，則會使用第二個呼叫中的資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="1c921-177">For example, if the `ToTable` method for an entity type is called first with one table name and then again later with a different table name, the table name in the second call is used.</span></span>

### <a name="custom-user-data"></a><span data-ttu-id="1c921-178">自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="1c921-178">Custom user data</span></span>

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

<span data-ttu-id="1c921-179">[自訂使用者資料](xref:security/authentication/add-user-data)是藉由繼承自來支援 `IdentityUser` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-179">[Custom user data](xref:security/authentication/add-user-data) is supported by inheriting from `IdentityUser`.</span></span> <span data-ttu-id="1c921-180">建議您將此類型命名為 `ApplicationUser` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-180">It's customary to name this type `ApplicationUser`:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="1c921-181">使用 `ApplicationUser` 類型做為內容的泛型引數：</span><span class="sxs-lookup"><span data-stu-id="1c921-181">Use the `ApplicationUser` type as a generic argument for the context:</span></span>

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

<span data-ttu-id="1c921-182">不需要覆寫 `OnModelCreating` 類別中的 `ApplicationDbContext` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-182">There's no need to override `OnModelCreating` in the `ApplicationDbContext` class.</span></span> <span data-ttu-id="1c921-183">EF Core `CustomTag` 依慣例對應屬性。</span><span class="sxs-lookup"><span data-stu-id="1c921-183">EF Core maps the `CustomTag` property by convention.</span></span> <span data-ttu-id="1c921-184">不過，必須更新資料庫，才能建立新的資料 `CustomTag` 行。</span><span class="sxs-lookup"><span data-stu-id="1c921-184">However, the database needs to be updated to create a new `CustomTag` column.</span></span> <span data-ttu-id="1c921-185">若要建立資料行，請新增遷移，然後更新資料庫，如[ Identity 和 EF Core 遷移](#identity-and-ef-core-migrations)中所述。</span><span class="sxs-lookup"><span data-stu-id="1c921-185">To create the column, add a migration, and then update the database as described in [Identity and EF Core Migrations](#identity-and-ef-core-migrations).</span></span>

<span data-ttu-id="1c921-186">更新*Pages/Shared/_LoginPartial. cshtml* ，並將取代 `IdentityUser` 為 `ApplicationUser` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-186">Update *Pages/Shared/_LoginPartial.cshtml* and replace `IdentityUser` with `ApplicationUser`:</span></span>

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

<span data-ttu-id="1c921-187">更新*Areas/ Identity /IdentityHostingStartup.cs*或 `Startup.ConfigureServices` ，並將取代 `IdentityUser` 為 `ApplicationUser` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-187">Update *Areas/Identity/IdentityHostingStartup.cs*  or `Startup.ConfigureServices` and replace `IdentityUser` with `ApplicationUser`.</span></span>

```csharp
services.AddIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

<span data-ttu-id="1c921-188">在 ASP.NET Core 2.1 或更新版本中， Identity 是以 Razor 類別庫的形式提供。</span><span class="sxs-lookup"><span data-stu-id="1c921-188">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="1c921-189">如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity> 。</span><span class="sxs-lookup"><span data-stu-id="1c921-189">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="1c921-190">因此，上述程式碼需要呼叫 <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*> 。</span><span class="sxs-lookup"><span data-stu-id="1c921-190">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="1c921-191">如果 Identity scaffolder 是用來將檔案加入至 Identity 專案，請移除對的呼叫 `AddDefaultUI` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-191">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span> <span data-ttu-id="1c921-192">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="1c921-192">For more information, see:</span></span>

* <span data-ttu-id="1c921-193">[ScaffoldIdentity](xref:security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="1c921-193">[Scaffold Identity](xref:security/authentication/scaffold-identity)</span></span>
* <span data-ttu-id="1c921-194">[新增、下載及刪除自訂使用者資料至Identity](xref:security/authentication/add-user-data)</span><span class="sxs-lookup"><span data-stu-id="1c921-194">[Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data)</span></span>

### <a name="change-the-primary-key-type"></a><span data-ttu-id="1c921-195">變更主要金鑰類型</span><span class="sxs-lookup"><span data-stu-id="1c921-195">Change the primary key type</span></span>

<span data-ttu-id="1c921-196">在建立資料庫之後，對 PK 資料行的資料類型所做的變更在許多資料庫系統上都有問題。</span><span class="sxs-lookup"><span data-stu-id="1c921-196">A change to the PK column's data type after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="1c921-197">變更 PK 通常牽涉到卸載並重新建立資料表。</span><span class="sxs-lookup"><span data-stu-id="1c921-197">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="1c921-198">因此，在建立資料庫時，應該在初始遷移中指定金鑰類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-198">Therefore, key types should be specified in the initial migration when the database is created.</span></span>

<span data-ttu-id="1c921-199">請遵循下列步驟來變更 PK 類型：</span><span class="sxs-lookup"><span data-stu-id="1c921-199">Follow these steps to change the PK type:</span></span>

1. <span data-ttu-id="1c921-200">如果資料庫是在 PK 變更之前建立的，請執行 `Drop-Database` （PMC）或 `dotnet ef database drop` （.NET Core CLI）將它刪除。</span><span class="sxs-lookup"><span data-stu-id="1c921-200">If the database was created before the PK change, run `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) to delete it.</span></span>
2. <span data-ttu-id="1c921-201">確認刪除資料庫之後，請使用 `Remove-Migration` （PMC）或 `dotnet ef migrations remove` （.NET Core CLI）移除初始遷移。</span><span class="sxs-lookup"><span data-stu-id="1c921-201">After confirming deletion of the database, remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>
3. <span data-ttu-id="1c921-202">將 `ApplicationDbContext` 類別更新為衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603> 。</span><span class="sxs-lookup"><span data-stu-id="1c921-202">Update the `ApplicationDbContext` class to derive from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span></span> <span data-ttu-id="1c921-203">為指定新的金鑰類型 `TKey` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-203">Specify the new key type for `TKey`.</span></span> <span data-ttu-id="1c921-204">例如，若要使用 `Guid` 金鑰類型：</span><span class="sxs-lookup"><span data-stu-id="1c921-204">For example, to use a `Guid` key type:</span></span>

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

    <span data-ttu-id="1c921-205">在上述程式碼中， <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> 必須指定泛型類別和， <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> 才能使用新的索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-205">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    <span data-ttu-id="1c921-206">在上述程式碼中， <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> 必須指定泛型類別和， <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> 才能使用新的索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-206">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    <span data-ttu-id="1c921-207">`Startup.ConfigureServices`必須更新才能使用一般使用者：</span><span class="sxs-lookup"><span data-stu-id="1c921-207">`Startup.ConfigureServices` must be updated to use the generic user:</span></span>

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

4. <span data-ttu-id="1c921-208">如果 `ApplicationUser` 正在使用自訂類別，請更新類別以繼承自 `IdentityUser` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-208">If a custom `ApplicationUser` class is being used, update the class to inherit from `IdentityUser`.</span></span> <span data-ttu-id="1c921-209">例如：</span><span class="sxs-lookup"><span data-stu-id="1c921-209">For example:</span></span>

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    <span data-ttu-id="1c921-210">更新 `ApplicationDbContext` 以參考自訂 `ApplicationUser` 類別：</span><span class="sxs-lookup"><span data-stu-id="1c921-210">Update `ApplicationDbContext` to reference the custom `ApplicationUser` class:</span></span>

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

    <span data-ttu-id="1c921-211">在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-211">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="1c921-212">主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。</span><span class="sxs-lookup"><span data-stu-id="1c921-212">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="1c921-213">在 ASP.NET Core 2.1 或更新版本中， Identity 是以 Razor 類別庫的形式提供。</span><span class="sxs-lookup"><span data-stu-id="1c921-213">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="1c921-214">如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity> 。</span><span class="sxs-lookup"><span data-stu-id="1c921-214">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="1c921-215">因此，上述程式碼需要呼叫 <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*> 。</span><span class="sxs-lookup"><span data-stu-id="1c921-215">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="1c921-216">如果 Identity scaffolder 是用來將檔案加入至 Identity 專案，請移除對的呼叫 `AddDefaultUI` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-216">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="1c921-217">主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。</span><span class="sxs-lookup"><span data-stu-id="1c921-217">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="1c921-218"><xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受 `TKey` 表示主鍵之資料類型的類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-218">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

5. <span data-ttu-id="1c921-219">如果 `ApplicationRole` 正在使用自訂類別，請更新類別以繼承自 `IdentityRole<TKey>` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-219">If a custom `ApplicationRole` class is being used, update the class to inherit from `IdentityRole<TKey>`.</span></span> <span data-ttu-id="1c921-220">例如：</span><span class="sxs-lookup"><span data-stu-id="1c921-220">For example:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    <span data-ttu-id="1c921-221">更新 `ApplicationDbContext` 以參考自訂 `ApplicationRole` 類別。</span><span class="sxs-lookup"><span data-stu-id="1c921-221">Update `ApplicationDbContext` to reference the custom `ApplicationRole` class.</span></span> <span data-ttu-id="1c921-222">例如，下列類別會參考自訂 `ApplicationUser` 和自訂 `ApplicationRole` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-222">For example, the following class references a custom `ApplicationUser` and a custom `ApplicationRole`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="1c921-223">在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-223">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    <span data-ttu-id="1c921-224">主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。</span><span class="sxs-lookup"><span data-stu-id="1c921-224">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="1c921-225">在 ASP.NET Core 2.1 或更新版本中， Identity 是以 Razor 類別庫的形式提供。</span><span class="sxs-lookup"><span data-stu-id="1c921-225">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="1c921-226">如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity> 。</span><span class="sxs-lookup"><span data-stu-id="1c921-226">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="1c921-227">因此，上述程式碼需要呼叫 <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*> 。</span><span class="sxs-lookup"><span data-stu-id="1c921-227">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="1c921-228">如果 Identity scaffolder 是用來將檔案加入至 Identity 專案，請移除對的呼叫 `AddDefaultUI` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-228">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="1c921-229">在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-229">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="1c921-230">主要索引鍵的資料類型是藉由分析[DbCoNtext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件來推斷。</span><span class="sxs-lookup"><span data-stu-id="1c921-230">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="1c921-231">在中加入服務時，註冊自訂資料庫內容類別 Identity `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-231">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="1c921-232"><xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受 `TKey` 表示主鍵之資料類型的類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-232">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

### <a name="add-navigation-properties"></a><span data-ttu-id="1c921-233">新增導覽屬性</span><span class="sxs-lookup"><span data-stu-id="1c921-233">Add navigation properties</span></span>

<span data-ttu-id="1c921-234">變更關聯性的模型設定可能會比進行其他變更更棘手。</span><span class="sxs-lookup"><span data-stu-id="1c921-234">Changing the model configuration for relationships can be more difficult than making other changes.</span></span> <span data-ttu-id="1c921-235">必須小心取代現有的關聯性，而不是建立新的其他關聯性。</span><span class="sxs-lookup"><span data-stu-id="1c921-235">Care must be taken to replace the existing relationships rather than create new, additional relationships.</span></span> <span data-ttu-id="1c921-236">特別是，已變更的關聯性必須指定與現有關聯性相同的外鍵（FK）屬性。</span><span class="sxs-lookup"><span data-stu-id="1c921-236">In particular, the changed relationship must specify the same foreign key (FK) property as the existing relationship.</span></span> <span data-ttu-id="1c921-237">例如，和之間的關聯 `Users` 性 `UserClaims` 預設會依照下列方式指定：</span><span class="sxs-lookup"><span data-stu-id="1c921-237">For example, the relationship between `Users` and `UserClaims` is, by default, specified as follows:</span></span>

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

<span data-ttu-id="1c921-238">此關聯性的 FK 會指定為 `UserClaim.UserId` 屬性。</span><span class="sxs-lookup"><span data-stu-id="1c921-238">The FK for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="1c921-239">`HasMany`和 `WithOne` 會在沒有引數的情況下呼叫，以建立沒有導覽屬性的關聯性。</span><span class="sxs-lookup"><span data-stu-id="1c921-239">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="1c921-240">將導覽屬性加入至 `ApplicationUser` ，讓 `UserClaims` 使用者可以參考相關聯的：</span><span class="sxs-lookup"><span data-stu-id="1c921-240">Add a navigation property to `ApplicationUser` that allows associated `UserClaims` to be referenced from the user:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="1c921-241">的 `TKey` `IdentityUserClaim<TKey>` 是為使用者的 PK 指定的類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-241">The `TKey` for `IdentityUserClaim<TKey>` is the type specified for the PK of users.</span></span> <span data-ttu-id="1c921-242">在此情況下， `TKey` 是， `string` 因為正在使用預設值。</span><span class="sxs-lookup"><span data-stu-id="1c921-242">In this case, `TKey` is `string` because the defaults are being used.</span></span> <span data-ttu-id="1c921-243">這**不**是實體類型的 PK 類型 `UserClaim` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-243">It's **not** the PK type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="1c921-244">現在導覽屬性已經存在，必須在中設定 `OnModelCreating` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-244">Now that the navigation property exists, it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="1c921-245">請注意，關聯性的設定方式與之前完全相同，只是在呼叫中指定的導覽屬性 `HasMany` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-245">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="1c921-246">導覽屬性只存在於 EF 模型中，而非資料庫中。</span><span class="sxs-lookup"><span data-stu-id="1c921-246">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="1c921-247">因為關聯性的 FK 尚未變更，所以這種模型變更不需要更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c921-247">Because the FK for the relationship hasn't changed, this kind of model change doesn't require the database to be updated.</span></span> <span data-ttu-id="1c921-248">這可以藉由在進行變更後新增遷移來加以檢查。</span><span class="sxs-lookup"><span data-stu-id="1c921-248">This can be checked by adding a migration after making the change.</span></span> <span data-ttu-id="1c921-249">`Up`和 `Down` 方法是空的。</span><span class="sxs-lookup"><span data-stu-id="1c921-249">The `Up` and `Down` methods are empty.</span></span>

### <a name="add-all-user-navigation-properties"></a><span data-ttu-id="1c921-250">新增所有使用者導覽屬性</span><span class="sxs-lookup"><span data-stu-id="1c921-250">Add all User navigation properties</span></span>

<span data-ttu-id="1c921-251">使用上述章節做為指導方針，下列範例會為使用者的所有關聯性設定單向導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="1c921-251">Using the section above as guidance, the following example configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="add-user-and-role-navigation-properties"></a><span data-ttu-id="1c921-252">新增使用者和角色導覽屬性</span><span class="sxs-lookup"><span data-stu-id="1c921-252">Add User and Role navigation properties</span></span>

<span data-ttu-id="1c921-253">使用上述章節做為指導方針，下列範例會設定使用者和角色上所有關聯性的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="1c921-253">Using the section above as guidance, the following example configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="1c921-254">注意：</span><span class="sxs-lookup"><span data-stu-id="1c921-254">Notes:</span></span>

* <span data-ttu-id="1c921-255">此範例也包含 `UserRole` join 實體，這是將多對多關聯性從使用者導覽至角色時所需的專案。</span><span class="sxs-lookup"><span data-stu-id="1c921-255">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="1c921-256">請記得變更導覽屬性的類型，以反映目前使用的類型， `ApplicationXxx` 而不是 `IdentityXxx` 類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-256">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="1c921-257">請記得 `ApplicationXxx` 在一般定義中使用 `ApplicationContext` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-257">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="add-all-navigation-properties"></a><span data-ttu-id="1c921-258">新增所有導覽屬性</span><span class="sxs-lookup"><span data-stu-id="1c921-258">Add all navigation properties</span></span>

<span data-ttu-id="1c921-259">使用上述章節做為指引，下列範例會設定所有實體類型上所有關聯性的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="1c921-259">Using the section above as guidance, the following example configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="use-composite-keys"></a><span data-ttu-id="1c921-260">使用複合索引鍵</span><span class="sxs-lookup"><span data-stu-id="1c921-260">Use composite keys</span></span>

<span data-ttu-id="1c921-261">前面幾節會示範如何變更模型中所使用的金鑰類型 Identity 。</span><span class="sxs-lookup"><span data-stu-id="1c921-261">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="1c921-262">Identity不支援或不建議將金鑰模型變更為使用複合索引鍵。</span><span class="sxs-lookup"><span data-stu-id="1c921-262">Changing the Identity key model to use composite keys isn't supported or recommended.</span></span> <span data-ttu-id="1c921-263">使用複合索引鍵時， Identity 牽涉到變更 Identity 管理員程式碼與模型互動的方式。</span><span class="sxs-lookup"><span data-stu-id="1c921-263">Using a composite key with Identity involves changing how the Identity manager code interacts with the model.</span></span> <span data-ttu-id="1c921-264">此自訂已超出本檔的範圍。</span><span class="sxs-lookup"><span data-stu-id="1c921-264">This customization is beyond the scope of this document.</span></span>

### <a name="change-tablecolumn-names-and-facets"></a><span data-ttu-id="1c921-265">變更資料表/資料行的名稱和 facet</span><span class="sxs-lookup"><span data-stu-id="1c921-265">Change table/column names and facets</span></span>

<span data-ttu-id="1c921-266">若要變更資料表和資料行的名稱，請呼叫 `base.OnModelCreating` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-266">To change the names of tables and columns, call `base.OnModelCreating`.</span></span> <span data-ttu-id="1c921-267">然後，新增設定以覆寫任何預設值。</span><span class="sxs-lookup"><span data-stu-id="1c921-267">Then, add configuration to override any of the defaults.</span></span> <span data-ttu-id="1c921-268">例如，若要變更所有資料表的名稱 Identity ：</span><span class="sxs-lookup"><span data-stu-id="1c921-268">For example, to change the name of all the Identity tables:</span></span>

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

<span data-ttu-id="1c921-269">這些範例會使用預設 Identity 類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-269">These examples use the default Identity types.</span></span> <span data-ttu-id="1c921-270">如果使用之類的應用程式類型 `ApplicationUser` ，請設定該類型，而不是預設類型。</span><span class="sxs-lookup"><span data-stu-id="1c921-270">If using an app type such as `ApplicationUser`, configure that type instead of the default type.</span></span>

<span data-ttu-id="1c921-271">下列範例會變更一些資料行名稱：</span><span class="sxs-lookup"><span data-stu-id="1c921-271">The following example changes some column names:</span></span>

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

<span data-ttu-id="1c921-272">某些類型的資料庫資料行可以使用特定*facet*來設定（例如，允許的最大 `string` 長度）。</span><span class="sxs-lookup"><span data-stu-id="1c921-272">Some types of database columns can be configured with certain *facets* (for example, the maximum `string` length allowed).</span></span> <span data-ttu-id="1c921-273">下列範例會在模型中設定數個屬性的資料行最大長度 `string` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-273">The following example sets column maximum lengths for several `string` properties in the model:</span></span>

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

### <a name="map-to-a-different-schema"></a><span data-ttu-id="1c921-274">對應至不同的架構</span><span class="sxs-lookup"><span data-stu-id="1c921-274">Map to a different schema</span></span>

<span data-ttu-id="1c921-275">架構在資料庫提供者之間的行為可能不同。</span><span class="sxs-lookup"><span data-stu-id="1c921-275">Schemas can behave differently across database providers.</span></span> <span data-ttu-id="1c921-276">針對 SQL Server，預設值是在*dbo*架構中建立所有資料表。</span><span class="sxs-lookup"><span data-stu-id="1c921-276">For SQL Server, the default is to create all tables in the *dbo* schema.</span></span> <span data-ttu-id="1c921-277">資料表可以在不同的架構中建立。</span><span class="sxs-lookup"><span data-stu-id="1c921-277">The tables can be created in a different schema.</span></span> <span data-ttu-id="1c921-278">例如：</span><span class="sxs-lookup"><span data-stu-id="1c921-278">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a><span data-ttu-id="1c921-279">消極式載入</span><span class="sxs-lookup"><span data-stu-id="1c921-279">Lazy loading</span></span>

<span data-ttu-id="1c921-280">在本節中，會加入模型中的消極式載入 proxy 支援 Identity 。</span><span class="sxs-lookup"><span data-stu-id="1c921-280">In this section, support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="1c921-281">消極式載入很有用，因為它允許直接使用導覽屬性，而不需要先確定它們已載入。</span><span class="sxs-lookup"><span data-stu-id="1c921-281">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they're loaded.</span></span>

<span data-ttu-id="1c921-282">實體類型可用於以數種方式進行消極式載入，如[EF Core 檔](/ef/core/querying/related-data#lazy-loading)中所述。</span><span class="sxs-lookup"><span data-stu-id="1c921-282">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="1c921-283">為了簡單起見，請使用消極式載入 proxy，這需要：</span><span class="sxs-lookup"><span data-stu-id="1c921-283">For simplicity, use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="1c921-284">安裝[microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)封裝。</span><span class="sxs-lookup"><span data-stu-id="1c921-284">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="1c921-285">在 <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> [AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)內呼叫。</span><span class="sxs-lookup"><span data-stu-id="1c921-285">A call to <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> inside [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span>
* <span data-ttu-id="1c921-286">具有導覽屬性的公用實體類型 `public virtual` 。</span><span class="sxs-lookup"><span data-stu-id="1c921-286">Public entity types with `public virtual` navigation properties.</span></span>

<span data-ttu-id="1c921-287">下列範例示範如何 `UseLazyLoadingProxies` 在中呼叫 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1c921-287">The following example demonstrates calling `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="1c921-288">請參閱上述範例，以取得將導覽屬性加入至實體類型的指引。</span><span class="sxs-lookup"><span data-stu-id="1c921-288">Refer to the preceding examples for guidance on adding navigation properties to the entity types.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c921-289">其他資源</span><span class="sxs-lookup"><span data-stu-id="1c921-289">Additional resources</span></span>

* <xref:security/authentication/scaffold-identity>

::: moniker-end
