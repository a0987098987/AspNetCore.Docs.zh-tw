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
# <a name="identity-model-customization-in-aspnet-core"></a><span data-ttu-id="6299a-103">ASP.NET Core 中的身分識別模型自訂</span><span class="sxs-lookup"><span data-stu-id="6299a-103">Identity model customization in ASP.NET Core</span></span>

<span data-ttu-id="6299a-104">藉由[Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="6299a-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="6299a-105">ASP.NET Core 身分識別可供管理和儲存在 ASP.NET Core 應用程式中的使用者帳戶的架構。</span><span class="sxs-lookup"><span data-stu-id="6299a-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core apps.</span></span> <span data-ttu-id="6299a-106">身分識別新增至您的專案時**個別使用者帳戶**選取作為驗證機制。</span><span class="sxs-lookup"><span data-stu-id="6299a-106">Identity is added to your project when **Individual User Accounts** is selected as the authentication mechanism.</span></span> <span data-ttu-id="6299a-107">根據預設，身分識別會使用的 Entity Framework (EF) Core 資料模型。</span><span class="sxs-lookup"><span data-stu-id="6299a-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="6299a-108">本文說明如何自訂身分識別模型。</span><span class="sxs-lookup"><span data-stu-id="6299a-108">This article describes how to customize the Identity model.</span></span>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="6299a-109">身分識別和 EF Core 移轉</span><span class="sxs-lookup"><span data-stu-id="6299a-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="6299a-110">先檢查模型，它是有助於您了解如何識別用於[EF Core 移轉](/ef/core/managing-schemas/migrations/)來建立和更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="6299a-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="6299a-111">在最上層處理程序是：</span><span class="sxs-lookup"><span data-stu-id="6299a-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="6299a-112">定義或更新[程式碼中的資料模型](/ef/core/modeling/)。</span><span class="sxs-lookup"><span data-stu-id="6299a-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="6299a-113">新增移轉，將此模型轉譯成可以套用至資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="6299a-113">Add a Migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="6299a-114">請檢查移轉正確地表示您自己的意願。</span><span class="sxs-lookup"><span data-stu-id="6299a-114">Check that the Migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="6299a-115">適用於移轉至更新資料庫以便與模型保持同步。</span><span class="sxs-lookup"><span data-stu-id="6299a-115">Apply the Migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="6299a-116">重複步驟 1 到 4，以進一步精簡模型，並讓資料庫保持同步。</span><span class="sxs-lookup"><span data-stu-id="6299a-116">Repeat steps 1 through 4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="6299a-117">您可以使用其中一種下列方法來新增和套用移轉：</span><span class="sxs-lookup"><span data-stu-id="6299a-117">Use one of the following approaches to add and apply Migrations:</span></span>

* <span data-ttu-id="6299a-118">**Package Manager Console** (PMC) 視窗，如果使用 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="6299a-118">The **Package Manager Console** (PMC) window if using Visual Studio.</span></span> <span data-ttu-id="6299a-119">如需詳細資訊，請參閱 < [EF Core PMC 工具](/ef/core/miscellaneous/cli/powershell)。</span><span class="sxs-lookup"><span data-stu-id="6299a-119">For more information, see [EF Core PMC tools](/ef/core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="6299a-120">.NET Core CLI 如果使用命令列。</span><span class="sxs-lookup"><span data-stu-id="6299a-120">The .NET Core CLI if using the command line.</span></span> <span data-ttu-id="6299a-121">如需詳細資訊，請參閱 < [EF Core.NET 命令列工具](/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="6299a-121">For more information, see [EF Core .NET command line tools](/ef/core/miscellaneous/cli/dotnet).</span></span>
* <span data-ttu-id="6299a-122">按一下 **套用移轉**在應用程式執行時，錯誤 頁面上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="6299a-122">Clicking the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="6299a-123">ASP.NET Core 具有開發階段的錯誤頁面處理常式。</span><span class="sxs-lookup"><span data-stu-id="6299a-123">ASP.NET Core has a development-time error page handler.</span></span> <span data-ttu-id="6299a-124">執行應用程式時，處理常式可以套用移轉。</span><span class="sxs-lookup"><span data-stu-id="6299a-124">The handler can apply migrations when the app is run.</span></span> <span data-ttu-id="6299a-125">生產環境應用程式通常會產生 SQL 指令碼從移轉和部署資料庫變更為受控制的應用程式和資料庫部署的一部分。</span><span class="sxs-lookup"><span data-stu-id="6299a-125">Production apps typically generate SQL scripts from the migrations and deploy database changes as part of a controlled app and database deployment.</span></span>

<span data-ttu-id="6299a-126">建立新的應用程式，使用身分識別時，已經完成上述步驟 1 和 2。</span><span class="sxs-lookup"><span data-stu-id="6299a-126">When a new app using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="6299a-127">也就是初始資料模型已存在，並已新增至專案的初始移轉。</span><span class="sxs-lookup"><span data-stu-id="6299a-127">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="6299a-128">初始移轉仍然需要套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="6299a-128">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="6299a-129">透過下列方法之一，您可以套用初始移轉：</span><span class="sxs-lookup"><span data-stu-id="6299a-129">The initial migration can be applied via one of the following approaches:</span></span>

* <span data-ttu-id="6299a-130">執行`Update-Database`在 PMC 中。</span><span class="sxs-lookup"><span data-stu-id="6299a-130">Run `Update-Database` in PMC.</span></span>
* <span data-ttu-id="6299a-131">執行`dotnet ef database update`命令殼層。</span><span class="sxs-lookup"><span data-stu-id="6299a-131">Run `dotnet ef database update` in a command shell.</span></span>
* <span data-ttu-id="6299a-132">按一下 **套用移轉**在應用程式執行時，錯誤 頁面上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="6299a-132">Click the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="6299a-133">模型的變更，請重複上述步驟。</span><span class="sxs-lookup"><span data-stu-id="6299a-133">Repeat the preceding steps as changes are made to the model.</span></span>

## <a name="the-identity-model"></a><span data-ttu-id="6299a-134">身分識別模型</span><span class="sxs-lookup"><span data-stu-id="6299a-134">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="6299a-135">實體類型</span><span class="sxs-lookup"><span data-stu-id="6299a-135">Entity types</span></span>

<span data-ttu-id="6299a-136">身分識別模型是由下列的實體類型所組成。</span><span class="sxs-lookup"><span data-stu-id="6299a-136">The Identity model consists of the following entity types.</span></span>

|<span data-ttu-id="6299a-137">實體類型</span><span class="sxs-lookup"><span data-stu-id="6299a-137">Entity type</span></span>|<span data-ttu-id="6299a-138">描述</span><span class="sxs-lookup"><span data-stu-id="6299a-138">Description</span></span>                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |<span data-ttu-id="6299a-139">代表的使用者。</span><span class="sxs-lookup"><span data-stu-id="6299a-139">Represents the user.</span></span>                                         |
|`Role`     |<span data-ttu-id="6299a-140">表示角色。</span><span class="sxs-lookup"><span data-stu-id="6299a-140">Represents a role.</span></span>                                           |
|`UserClaim`|<span data-ttu-id="6299a-141">表示使用者擁有的宣告。</span><span class="sxs-lookup"><span data-stu-id="6299a-141">Represents a claim that a user possesses.</span></span>                    |
|`UserToken`|<span data-ttu-id="6299a-142">代表使用者的驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="6299a-142">Represents an authentication token for a user.</span></span>               |
|`UserLogin`|<span data-ttu-id="6299a-143">將使用者登入產生關聯。</span><span class="sxs-lookup"><span data-stu-id="6299a-143">Associates a user with a login.</span></span>                              |
|`RoleClaim`|<span data-ttu-id="6299a-144">表示會授與角色中的所有使用者的宣告。</span><span class="sxs-lookup"><span data-stu-id="6299a-144">Represents a claim that's granted to all users within a role.</span></span>|
|`UserRole` |<span data-ttu-id="6299a-145">聯結實體產生關聯的使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="6299a-145">A join entity that associates users and roles.</span></span>               |

### <a name="entity-type-relationships"></a><span data-ttu-id="6299a-146">實體型別關聯性</span><span class="sxs-lookup"><span data-stu-id="6299a-146">Entity type relationships</span></span>

<span data-ttu-id="6299a-147">[實體類型](#entity-types)透過下列方式與彼此相關：</span><span class="sxs-lookup"><span data-stu-id="6299a-147">The [entity types](#entity-types) are related to each other in the following ways:</span></span>

* <span data-ttu-id="6299a-148">每個`User`可以有多個`UserClaims`。</span><span class="sxs-lookup"><span data-stu-id="6299a-148">Each `User` can have many `UserClaims`.</span></span>
* <span data-ttu-id="6299a-149">每個`User`可以有多個`UserLogins`。</span><span class="sxs-lookup"><span data-stu-id="6299a-149">Each `User` can have many `UserLogins`.</span></span>
* <span data-ttu-id="6299a-150">每個`User`可以有多個`UserTokens`。</span><span class="sxs-lookup"><span data-stu-id="6299a-150">Each `User` can have many `UserTokens`.</span></span>
* <span data-ttu-id="6299a-151">每個`Role`可以有多個相關聯`RoleClaims`。</span><span class="sxs-lookup"><span data-stu-id="6299a-151">Each `Role` can have many associated `RoleClaims`.</span></span>
* <span data-ttu-id="6299a-152">每個`User`可以有多個相關聯`Roles`，而且每個`Role`可以與許多相關聯`Users`。</span><span class="sxs-lookup"><span data-stu-id="6299a-152">Each `User` can have many associated `Roles`, and each `Role` can be associated with many `Users`.</span></span> <span data-ttu-id="6299a-153">這是需要在資料庫中的聯結資料表的多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="6299a-153">This is a many-to-many relationship that requires a join table in the database.</span></span> <span data-ttu-id="6299a-154">聯結資料表由`UserRole`實體。</span><span class="sxs-lookup"><span data-stu-id="6299a-154">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="6299a-155">預設模型組態</span><span class="sxs-lookup"><span data-stu-id="6299a-155">Default model configuration</span></span>

<span data-ttu-id="6299a-156">識別定義許多*內容類別*繼承自[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)若要設定及使用模型。</span><span class="sxs-lookup"><span data-stu-id="6299a-156">Identity defines many *context classes* that inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="6299a-157">此組態完成使用[EF Core 程式碼 First fluent 應用程式開發介面](/ef/core/modeling/)中[OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating)內容類別的方法。</span><span class="sxs-lookup"><span data-stu-id="6299a-157">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) method of the context class.</span></span> <span data-ttu-id="6299a-158">預設設定是：</span><span class="sxs-lookup"><span data-stu-id="6299a-158">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="6299a-159">模型的泛型型別</span><span class="sxs-lookup"><span data-stu-id="6299a-159">Model generic types</span></span>

<span data-ttu-id="6299a-160">識別定義預設值[Common Language Runtime](/dotnet/standard/glossary#clr)上面所列的每個實體類型 (CLR) 型別。</span><span class="sxs-lookup"><span data-stu-id="6299a-160">Identity defines default [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) types for each of the entity types listed above.</span></span> <span data-ttu-id="6299a-161">這些類型都會加上*識別*:</span><span class="sxs-lookup"><span data-stu-id="6299a-161">These types are all prefixed with *Identity*:</span></span>

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

<span data-ttu-id="6299a-162">而不是直接使用這些類型，類型可以作為基底類別的應用程式本身的類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-162">Rather than using these types directly, the types can be used as base classes for the app's own types.</span></span> <span data-ttu-id="6299a-163">`DbContext`身分識別所定義的類別是泛型，使不同的 CLR 型別可以用一或多個模型中的實體類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-163">The `DbContext` classes defined by Identity are generic, such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="6299a-164">這些泛型型別也允許`User`變更主索引鍵 (PK) 資料類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-164">These generic types also allow the `User` primary key (PK) data type to be changed.</span></span>

<span data-ttu-id="6299a-165">針對角色，使用支援的身分識別時<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>應該使用類別。</span><span class="sxs-lookup"><span data-stu-id="6299a-165">When using Identity with support for roles, an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> class should be used.</span></span> <span data-ttu-id="6299a-166">例如:</span><span class="sxs-lookup"><span data-stu-id="6299a-166">For example:</span></span>

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

<span data-ttu-id="6299a-167">您也可使用身分識別，而角色 （僅 「 宣告 」） 不在此情況下<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601>應該使用類別：</span><span class="sxs-lookup"><span data-stu-id="6299a-167">It's also possible to use Identity without roles (only claims), in which case an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> class should be used:</span></span>

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

## <a name="customize-the-model"></a><span data-ttu-id="6299a-168">自訂模型</span><span class="sxs-lookup"><span data-stu-id="6299a-168">Customize the model</span></span>

<span data-ttu-id="6299a-169">模型自訂的起始點是衍生自適當的內容類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-169">The starting point for model customization is to derive from the appropriate context type.</span></span> <span data-ttu-id="6299a-170">請參閱[模型化泛型類型](#model-generic-types)一節。</span><span class="sxs-lookup"><span data-stu-id="6299a-170">See the [Model generic types](#model-generic-types) section.</span></span> <span data-ttu-id="6299a-171">此內容類型通常稱為`ApplicationDbContext`和 ASP.NET Core 範本所建立。</span><span class="sxs-lookup"><span data-stu-id="6299a-171">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="6299a-172">內容用來設定模型有兩種：</span><span class="sxs-lookup"><span data-stu-id="6299a-172">The context is used to configure the model in two ways:</span></span>

* <span data-ttu-id="6299a-173">提供實體和泛型型別參數的索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-173">Supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="6299a-174">覆寫`OnModelCreating`若要修改這些類型的對應。</span><span class="sxs-lookup"><span data-stu-id="6299a-174">Overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="6299a-175">當覆寫`OnModelCreating`，`base.OnModelCreating`應該先呼叫; 應該接著呼叫覆寫的組態。</span><span class="sxs-lookup"><span data-stu-id="6299a-175">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first; the overriding configuration should be called next.</span></span> <span data-ttu-id="6299a-176">EF Core 通常會有設定的最後一個 wins 原則。</span><span class="sxs-lookup"><span data-stu-id="6299a-176">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="6299a-177">例如，如果`ToTable`實體類型的方法稱為第一次一個資料表名稱，然後再次更新版本不同的資料表名稱，第二次呼叫中的資料表名稱會使用。</span><span class="sxs-lookup"><span data-stu-id="6299a-177">For example, if the `ToTable` method for an entity type is called first with one table name and then again later with a different table name, the table name in the second call is used.</span></span>

### <a name="custom-user-data"></a><span data-ttu-id="6299a-178">自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="6299a-178">Custom user data</span></span>

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

<span data-ttu-id="6299a-179">[自訂使用者資料](xref:security/authentication/add-user-data)支援藉由繼承自`IdentityUser`。</span><span class="sxs-lookup"><span data-stu-id="6299a-179">[Custom user data](xref:security/authentication/add-user-data) is supported by inheriting from `IdentityUser`.</span></span> <span data-ttu-id="6299a-180">按照慣例命名此類型`ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="6299a-180">It's customary to name this type `ApplicationUser`:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="6299a-181">使用`ApplicationUser`作為內容的泛型引數的型別：</span><span class="sxs-lookup"><span data-stu-id="6299a-181">Use the `ApplicationUser` type as a generic argument for the context:</span></span>

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

<span data-ttu-id="6299a-182">不需要覆寫`OnModelCreating`在`ApplicationDbContext`類別。</span><span class="sxs-lookup"><span data-stu-id="6299a-182">There's no need to override `OnModelCreating` in the `ApplicationDbContext` class.</span></span> <span data-ttu-id="6299a-183">EF Core 會將對應`CustomTag`依照慣例的屬性。</span><span class="sxs-lookup"><span data-stu-id="6299a-183">EF Core maps the `CustomTag` property by convention.</span></span> <span data-ttu-id="6299a-184">不過，資料庫必須建立新更新`CustomTag`資料行。</span><span class="sxs-lookup"><span data-stu-id="6299a-184">However, the database needs to be updated to create a new `CustomTag` column.</span></span> <span data-ttu-id="6299a-185">若要建立資料行，新增移轉時，，然後更新資料庫中所述[身分識別和 EF Core 移轉](#identity-and-ef-core-migrations)。</span><span class="sxs-lookup"><span data-stu-id="6299a-185">To create the column, add a migration, and then update the database as described in [Identity and EF Core Migrations](#identity-and-ef-core-migrations).</span></span>

<span data-ttu-id="6299a-186">更新*Pages/Shared/_LoginPartial.cshtml* ，並取代`IdentityUser`使用`ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="6299a-186">Update *Pages/Shared/_LoginPartial.cshtml* and replace `IdentityUser` with `ApplicationUser`:</span></span>

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

<span data-ttu-id="6299a-187">更新*Areas/Identity/IdentityHostingStartup.cs*或是`Startup.ConfigureServices`，並取代`IdentityUser`使用`ApplicationUser`。</span><span class="sxs-lookup"><span data-stu-id="6299a-187">Update *Areas/Identity/IdentityHostingStartup.cs*  or `Startup.ConfigureServices` and replace `IdentityUser` with `ApplicationUser`.</span></span>

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

<span data-ttu-id="6299a-188">在 ASP.NET Core 2.1 或更新版本，Razor 類別庫提供身分識別。</span><span class="sxs-lookup"><span data-stu-id="6299a-188">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="6299a-189">如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity>。</span><span class="sxs-lookup"><span data-stu-id="6299a-189">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="6299a-190">因此，上述程式碼需要呼叫<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>。</span><span class="sxs-lookup"><span data-stu-id="6299a-190">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="6299a-191">如果身分識別 scaffolder 用來識別檔案加入專案，移除呼叫`AddDefaultUI`。</span><span class="sxs-lookup"><span data-stu-id="6299a-191">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span> <span data-ttu-id="6299a-192">如需詳細資訊，請參閱:</span><span class="sxs-lookup"><span data-stu-id="6299a-192">For more information, see:</span></span>

* [<span data-ttu-id="6299a-193">Scaffold 身分識別</span><span class="sxs-lookup"><span data-stu-id="6299a-193">Scaffold Identity</span></span>](xref:security/authentication/scaffold-identity)
* [<span data-ttu-id="6299a-194">加入、 下載及刪除身分識別的自訂使用者資料</span><span class="sxs-lookup"><span data-stu-id="6299a-194">Add, download, and delete custom user data to Identity</span></span>](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a><span data-ttu-id="6299a-195">變更主索引鍵類型</span><span class="sxs-lookup"><span data-stu-id="6299a-195">Change the primary key type</span></span>

<span data-ttu-id="6299a-196">在建立資料庫後的 PK 資料行的資料類型的變更是在許多資料庫系統上有問題。</span><span class="sxs-lookup"><span data-stu-id="6299a-196">A change to the PK column's data type after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="6299a-197">變更在 PK 作業通常牽涉到卸除並重新建立資料表。</span><span class="sxs-lookup"><span data-stu-id="6299a-197">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="6299a-198">因此，金鑰類型應該指定在初始移轉時建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="6299a-198">Therefore, key types should be specified in the initial migration when the database is created.</span></span>

<span data-ttu-id="6299a-199">請遵循下列步驟來變更 PK 類型：</span><span class="sxs-lookup"><span data-stu-id="6299a-199">Follow these steps to change the PK type:</span></span>

1. <span data-ttu-id="6299a-200">如果資料庫已建立 PK 變更之前，執行`Drop-Database`(PMC) 或`dotnet ef database drop`(.NET Core CLI) 來刪除它。</span><span class="sxs-lookup"><span data-stu-id="6299a-200">If the database was created before the PK change, run `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) to delete it.</span></span>
2. <span data-ttu-id="6299a-201">確認刪除資料庫之後, 移除與初始移轉`Remove-Migration`(PMC) 或`dotnet ef migrations remove`(.NET Core CLI)。</span><span class="sxs-lookup"><span data-stu-id="6299a-201">After confirming deletion of the database, remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>
3. <span data-ttu-id="6299a-202">更新`ApplicationDbContext`類別來衍生自<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>。</span><span class="sxs-lookup"><span data-stu-id="6299a-202">Update the `ApplicationDbContext` class to derive from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span></span> <span data-ttu-id="6299a-203">指定新的金鑰類型，如`TKey`。</span><span class="sxs-lookup"><span data-stu-id="6299a-203">Specify the new key type for `TKey`.</span></span> <span data-ttu-id="6299a-204">例如，若要使用`Guid`金鑰類型：</span><span class="sxs-lookup"><span data-stu-id="6299a-204">For example, to use a `Guid` key type:</span></span>

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

    <span data-ttu-id="6299a-205">在上述程式碼的泛型類別<xref:Microsoft.AspNetCore.Identity.IdentityUser%601>和<xref:Microsoft.AspNetCore.Identity.IdentityRole%601>必須指定要使用新的索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-205">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    <span data-ttu-id="6299a-206">在上述程式碼的泛型類別<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601>和<xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601>必須指定要使用新的索引鍵類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-206">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    <span data-ttu-id="6299a-207">`Startup.ConfigureServices` 必須更新為使用一般的使用者：</span><span class="sxs-lookup"><span data-stu-id="6299a-207">`Startup.ConfigureServices` must be updated to use the generic user:</span></span>

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

4. <span data-ttu-id="6299a-208">如果自訂`ApplicationUser`類別正在使用中，更新類別繼承自`IdentityUser`。</span><span class="sxs-lookup"><span data-stu-id="6299a-208">If a custom `ApplicationUser` class is being used, update the class to inherit from `IdentityUser`.</span></span> <span data-ttu-id="6299a-209">例如:</span><span class="sxs-lookup"><span data-stu-id="6299a-209">For example:</span></span>

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    <span data-ttu-id="6299a-210">更新`ApplicationDbContext`若要參考自訂`ApplicationUser`類別：</span><span class="sxs-lookup"><span data-stu-id="6299a-210">Update `ApplicationDbContext` to reference the custom `ApplicationUser` class:</span></span>

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

    <span data-ttu-id="6299a-211">新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6299a-211">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="6299a-212">藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。</span><span class="sxs-lookup"><span data-stu-id="6299a-212">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="6299a-213">在 ASP.NET Core 2.1 或更新版本，Razor 類別庫提供身分識別。</span><span class="sxs-lookup"><span data-stu-id="6299a-213">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="6299a-214">如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity>。</span><span class="sxs-lookup"><span data-stu-id="6299a-214">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="6299a-215">因此，上述程式碼需要呼叫<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>。</span><span class="sxs-lookup"><span data-stu-id="6299a-215">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="6299a-216">如果身分識別 scaffolder 用來識別檔案加入專案，移除呼叫`AddDefaultUI`。</span><span class="sxs-lookup"><span data-stu-id="6299a-216">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="6299a-217">藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。</span><span class="sxs-lookup"><span data-stu-id="6299a-217">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="6299a-218"><xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受`TKey`表示主索引鍵的資料類型的類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-218">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

5. <span data-ttu-id="6299a-219">如果自訂`ApplicationRole`類別正在使用中，更新類別繼承自`IdentityRole<TKey>`。</span><span class="sxs-lookup"><span data-stu-id="6299a-219">If a custom `ApplicationRole` class is being used, update the class to inherit from `IdentityRole<TKey>`.</span></span> <span data-ttu-id="6299a-220">例如:</span><span class="sxs-lookup"><span data-stu-id="6299a-220">For example:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    <span data-ttu-id="6299a-221">更新`ApplicationDbContext`若要參考自訂`ApplicationRole`類別。</span><span class="sxs-lookup"><span data-stu-id="6299a-221">Update `ApplicationDbContext` to reference the custom `ApplicationRole` class.</span></span> <span data-ttu-id="6299a-222">例如，下列類別會參考自訂`ApplicationUser`和自訂`ApplicationRole`:</span><span class="sxs-lookup"><span data-stu-id="6299a-222">For example, the following class references a custom `ApplicationUser` and a custom `ApplicationRole`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="6299a-223">新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6299a-223">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    <span data-ttu-id="6299a-224">藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。</span><span class="sxs-lookup"><span data-stu-id="6299a-224">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="6299a-225">在 ASP.NET Core 2.1 或更新版本，Razor 類別庫提供身分識別。</span><span class="sxs-lookup"><span data-stu-id="6299a-225">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="6299a-226">如需詳細資訊，請參閱 <xref:security/authentication/scaffold-identity>。</span><span class="sxs-lookup"><span data-stu-id="6299a-226">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="6299a-227">因此，上述程式碼需要呼叫<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>。</span><span class="sxs-lookup"><span data-stu-id="6299a-227">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="6299a-228">如果身分識別 scaffolder 用來識別檔案加入專案，移除呼叫`AddDefaultUI`。</span><span class="sxs-lookup"><span data-stu-id="6299a-228">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="6299a-229">新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6299a-229">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="6299a-230">藉由分析的主索引鍵的資料型別推斷[DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)物件。</span><span class="sxs-lookup"><span data-stu-id="6299a-230">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="6299a-231">新增身分識別服務中的註冊自訂的資料庫內容類別`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6299a-231">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="6299a-232"><xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*>方法會接受`TKey`表示主索引鍵的資料類型的類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-232">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

### <a name="add-navigation-properties"></a><span data-ttu-id="6299a-233">新增導覽屬性</span><span class="sxs-lookup"><span data-stu-id="6299a-233">Add navigation properties</span></span>

<span data-ttu-id="6299a-234">變更關聯性的模型組態可能會比其他變更更困難。</span><span class="sxs-lookup"><span data-stu-id="6299a-234">Changing the model configuration for relationships can be more difficult than making other changes.</span></span> <span data-ttu-id="6299a-235">小心必須取代現有的關聯性，而不是建立新的其他關聯性。</span><span class="sxs-lookup"><span data-stu-id="6299a-235">Care must be taken to replace the existing relationships rather than create new, additional relationships.</span></span> <span data-ttu-id="6299a-236">特別是，已變更的關聯性也必須指定相同的外部索引鍵 (FK) 屬性，為現有的關聯性。</span><span class="sxs-lookup"><span data-stu-id="6299a-236">In particular, the changed relationship must specify the same foreign key (FK) property as the existing relationship.</span></span> <span data-ttu-id="6299a-237">比方說，之間的關係`Users`和`UserClaims`是，根據預設，指定方式如下：</span><span class="sxs-lookup"><span data-stu-id="6299a-237">For example, the relationship between `Users` and `UserClaims` is, by default, specified as follows:</span></span>

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

<span data-ttu-id="6299a-238">FK 此關聯性指定為`UserClaim.UserId`屬性。</span><span class="sxs-lookup"><span data-stu-id="6299a-238">The FK for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="6299a-239">`HasMany` 和`WithOne`呼叫不含引數來建立不含導覽屬性的關聯性。</span><span class="sxs-lookup"><span data-stu-id="6299a-239">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="6299a-240">將導覽屬性，加入`ApplicationUser`，可讓相關聯`UserClaims`參考使用者：</span><span class="sxs-lookup"><span data-stu-id="6299a-240">Add a navigation property to `ApplicationUser` that allows associated `UserClaims` to be referenced from the user:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="6299a-241">`TKey`針對`IdentityUserClaim<TKey>`是指定使用者的主索引鍵的類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-241">The `TKey` for `IdentityUserClaim<TKey>` is the type specified for the PK of users.</span></span> <span data-ttu-id="6299a-242">在此情況下，`TKey`是`string`因為正在使用預設值。</span><span class="sxs-lookup"><span data-stu-id="6299a-242">In this case, `TKey` is `string` because the defaults are being used.</span></span> <span data-ttu-id="6299a-243">它有**未**PK 類型`UserClaim`實體類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-243">It's **not** the PK type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="6299a-244">現在，導覽屬性存在，它必須設定在`OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="6299a-244">Now that the navigation property exists, it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="6299a-245">請注意關聯性已完全一樣，只與呼叫中指定的導覽屬性`HasMany`。</span><span class="sxs-lookup"><span data-stu-id="6299a-245">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="6299a-246">導覽屬性只存在於 EF 模型，而不是在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="6299a-246">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="6299a-247">由於關聯性的 FK 沒有變更，這種模型變更不需要更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="6299a-247">Because the FK for the relationship hasn't changed, this kind of model change doesn't require the database to be updated.</span></span> <span data-ttu-id="6299a-248">這可以藉由新增移轉，以進行變更之後檢查。</span><span class="sxs-lookup"><span data-stu-id="6299a-248">This can be checked by adding a migration after making the change.</span></span> <span data-ttu-id="6299a-249">`Up`和`Down`方法是空的。</span><span class="sxs-lookup"><span data-stu-id="6299a-249">The `Up` and `Down` methods are empty.</span></span>

### <a name="add-all-user-navigation-properties"></a><span data-ttu-id="6299a-250">新增所有的使用者導覽屬性</span><span class="sxs-lookup"><span data-stu-id="6299a-250">Add all User navigation properties</span></span>

<span data-ttu-id="6299a-251">使用上的一節做為指引，下列範例會設定使用者的所有關聯性的單向導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="6299a-251">Using the section above as guidance, the following example configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="add-user-and-role-navigation-properties"></a><span data-ttu-id="6299a-252">新增使用者和角色的導覽屬性</span><span class="sxs-lookup"><span data-stu-id="6299a-252">Add User and Role navigation properties</span></span>

<span data-ttu-id="6299a-253">使用上的一節做為指引，下列範例會設定使用者和角色的所有關聯性的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="6299a-253">Using the section above as guidance, the following example configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="6299a-254">附註：</span><span class="sxs-lookup"><span data-stu-id="6299a-254">Notes:</span></span>

* <span data-ttu-id="6299a-255">這個範例還包括`UserRole`聯結實體，才能瀏覽的使用者角色的多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="6299a-255">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="6299a-256">請記得要變更類型的導覽屬性反映這一點`ApplicationXxx`類型現在正在使用而不是`IdentityXxx`型別。</span><span class="sxs-lookup"><span data-stu-id="6299a-256">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="6299a-257">請務必使用`ApplicationXxx`在泛型`ApplicationContext`定義。</span><span class="sxs-lookup"><span data-stu-id="6299a-257">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="add-all-navigation-properties"></a><span data-ttu-id="6299a-258">新增所有導覽屬性</span><span class="sxs-lookup"><span data-stu-id="6299a-258">Add all navigation properties</span></span>

<span data-ttu-id="6299a-259">使用上的一節做為指引，下列範例會設定所有的實體類型上的所有關聯性的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="6299a-259">Using the section above as guidance, the following example configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="use-composite-keys"></a><span data-ttu-id="6299a-260">使用複合索引鍵</span><span class="sxs-lookup"><span data-stu-id="6299a-260">Use composite keys</span></span>

<span data-ttu-id="6299a-261">前幾節所示範變更身分識別模型中使用的索引鍵的類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-261">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="6299a-262">變更要使用複合索引鍵的識別索引鍵模型不支援或建議。</span><span class="sxs-lookup"><span data-stu-id="6299a-262">Changing the Identity key model to use composite keys isn't supported or recommended.</span></span> <span data-ttu-id="6299a-263">使用身分識別的複合索引鍵，牽涉到變更身分識別管理員 」 程式碼與模型之間的互動方式。</span><span class="sxs-lookup"><span data-stu-id="6299a-263">Using a composite key with Identity involves changing how the Identity manager code interacts with the model.</span></span> <span data-ttu-id="6299a-264">這項自訂已超出本文的範圍。</span><span class="sxs-lookup"><span data-stu-id="6299a-264">This customization is beyond the scope of this document.</span></span>

### <a name="change-tablecolumn-names-and-facets"></a><span data-ttu-id="6299a-265">變更資料表/資料行名稱和 facet</span><span class="sxs-lookup"><span data-stu-id="6299a-265">Change table/column names and facets</span></span>

<span data-ttu-id="6299a-266">若要變更的資料表和資料行名稱，請呼叫`base.OnModelCreating`。</span><span class="sxs-lookup"><span data-stu-id="6299a-266">To change the names of tables and columns, call `base.OnModelCreating`.</span></span> <span data-ttu-id="6299a-267">然後，新增設定，以覆寫任何預設值。</span><span class="sxs-lookup"><span data-stu-id="6299a-267">Then, add configuration to override any of the defaults.</span></span> <span data-ttu-id="6299a-268">例如，若要變更的身分識別的所有資料表名稱：</span><span class="sxs-lookup"><span data-stu-id="6299a-268">For example, to change the name of all the Identity tables:</span></span>

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

<span data-ttu-id="6299a-269">這些範例會使用預設的身分識別類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-269">These examples use the default Identity types.</span></span> <span data-ttu-id="6299a-270">如果使用的應用程式類型，例如`ApplicationUser`，設定該型別，而不是預設類型。</span><span class="sxs-lookup"><span data-stu-id="6299a-270">If using an app type such as `ApplicationUser`, configure that type instead of the default type.</span></span>

<span data-ttu-id="6299a-271">下列範例會變更某些資料行名稱：</span><span class="sxs-lookup"><span data-stu-id="6299a-271">The following example changes some column names:</span></span>

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

<span data-ttu-id="6299a-272">可以使用特定設定某些類型的資料庫資料行*facet* (例如，最大值`string`允許的長度)。</span><span class="sxs-lookup"><span data-stu-id="6299a-272">Some types of database columns can be configured with certain *facets* (for example, the maximum `string` length allowed).</span></span> <span data-ttu-id="6299a-273">下列範例會設定數個資料行的最大長度`string`模型中的屬性：</span><span class="sxs-lookup"><span data-stu-id="6299a-273">The following example sets column maximum lengths for several `string` properties in the model:</span></span>

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

### <a name="map-to-a-different-schema"></a><span data-ttu-id="6299a-274">對應到不同的結構描述</span><span class="sxs-lookup"><span data-stu-id="6299a-274">Map to a different schema</span></span>

<span data-ttu-id="6299a-275">結構描述的行為會跨資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="6299a-275">Schemas can behave differently across database providers.</span></span> <span data-ttu-id="6299a-276">SQL Server 的預設值是建立中的所有資料表*dbo*結構描述。</span><span class="sxs-lookup"><span data-stu-id="6299a-276">For SQL Server, the default is to create all tables in the *dbo* schema.</span></span> <span data-ttu-id="6299a-277">可以在不同的結構描述中建立資料表。</span><span class="sxs-lookup"><span data-stu-id="6299a-277">The tables can be created in a different schema.</span></span> <span data-ttu-id="6299a-278">例如:</span><span class="sxs-lookup"><span data-stu-id="6299a-278">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a><span data-ttu-id="6299a-279">消極式載入</span><span class="sxs-lookup"><span data-stu-id="6299a-279">Lazy loading</span></span>

<span data-ttu-id="6299a-280">在本節中，新增身分識別模型中的消極式載入 proxy 的支援。</span><span class="sxs-lookup"><span data-stu-id="6299a-280">In this section, support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="6299a-281">「 延遲載入適合，因為它可讓而不用先確保他們載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6299a-281">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they're loaded.</span></span>

<span data-ttu-id="6299a-282">實體類型可以進行適用於數種方式的消極式載入中所述[EF Core 文件集](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="6299a-282">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="6299a-283">為了簡單起見，使用消極式載入 proxy，而這需要：</span><span class="sxs-lookup"><span data-stu-id="6299a-283">For simplicity, use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="6299a-284">安裝[Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)封裝。</span><span class="sxs-lookup"><span data-stu-id="6299a-284">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="6299a-285">呼叫<xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*>內[AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)。</span><span class="sxs-lookup"><span data-stu-id="6299a-285">A call to <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> inside [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span>
* <span data-ttu-id="6299a-286">公開的實體類型與`public virtual`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6299a-286">Public entity types with `public virtual` navigation properties.</span></span>

<span data-ttu-id="6299a-287">下列範例示範如何呼叫`UseLazyLoadingProxies`在`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6299a-287">The following example demonstrates calling `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="6299a-288">請參閱前面的範例，如需導覽屬性加入實體類型的指引。</span><span class="sxs-lookup"><span data-stu-id="6299a-288">Refer to the preceding examples for guidance on adding navigation properties to the entity types.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6299a-289">其他資源</span><span class="sxs-lookup"><span data-stu-id="6299a-289">Additional resources</span></span>

* <xref:security/authentication/scaffold-identity>

::: moniker-end
