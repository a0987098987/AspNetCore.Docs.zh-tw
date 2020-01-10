---
title: ASP.NET Core 身分識別的自訂儲存體提供者
author: ardalis
description: 瞭解如何設定 ASP.NET Core 身分識別的自訂儲存體提供者。
ms.author: riande
ms.custom: mvc
ms.date: 07/23/2019
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 70951085474d88fd57f1b1496a41adcda520b91f
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829149"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a><span data-ttu-id="297a2-103">ASP.NET Core 身分識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="297a2-103">Custom storage providers for ASP.NET Core Identity</span></span>

<span data-ttu-id="297a2-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="297a2-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="297a2-105">ASP.NET Core 身分識別是可延伸的系統，可讓您建立自訂的儲存提供者，並將它連線到您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="297a2-105">ASP.NET Core Identity is an extensible system which enables you to create a custom storage provider and connect it to your app.</span></span> <span data-ttu-id="297a2-106">本主題說明如何建立 ASP.NET Core 身分識別的自訂存放裝置提供者。</span><span class="sxs-lookup"><span data-stu-id="297a2-106">This topic describes how to create a customized storage provider for ASP.NET Core Identity.</span></span> <span data-ttu-id="297a2-107">其中涵蓋建立您自己的儲存提供者所需的重要概念，但不是逐步解說。</span><span class="sxs-lookup"><span data-stu-id="297a2-107">It covers the important concepts for creating your own storage provider, but isn't a step-by-step walkthrough.</span></span>

<span data-ttu-id="297a2-108">[從 GitHub 檢視或下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample)。</span><span class="sxs-lookup"><span data-stu-id="297a2-108">[View or download sample from GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample).</span></span>

## <a name="introduction"></a><span data-ttu-id="297a2-109">簡介</span><span class="sxs-lookup"><span data-stu-id="297a2-109">Introduction</span></span>

<span data-ttu-id="297a2-110">根據預設，ASP.NET Core 身分識別系統會使用 Entity Framework Core，將使用者資訊儲存在 SQL Server 資料庫中。</span><span class="sxs-lookup"><span data-stu-id="297a2-110">By default, the ASP.NET Core Identity system stores user information in a SQL Server database using Entity Framework Core.</span></span> <span data-ttu-id="297a2-111">對於許多應用程式而言，這種方法運作良好。</span><span class="sxs-lookup"><span data-stu-id="297a2-111">For many apps, this approach works well.</span></span> <span data-ttu-id="297a2-112">不過，您可能會想要使用不同的持續性機制或資料結構描述。</span><span class="sxs-lookup"><span data-stu-id="297a2-112">However, you may prefer to use a different persistence mechanism or data schema.</span></span> <span data-ttu-id="297a2-113">例如：</span><span class="sxs-lookup"><span data-stu-id="297a2-113">For example:</span></span>

* <span data-ttu-id="297a2-114">您會使用[Azure 表格儲存體](/azure/storage/)或另一個資料存放區。</span><span class="sxs-lookup"><span data-stu-id="297a2-114">You use [Azure Table Storage](/azure/storage/) or another data store.</span></span>
* <span data-ttu-id="297a2-115">您的資料庫資料表具有不同的結構。</span><span class="sxs-lookup"><span data-stu-id="297a2-115">Your database tables have a different structure.</span></span> 
* <span data-ttu-id="297a2-116">您可能想要使用不同的資料存取方法，例如[Dapper](https://github.com/StackExchange/Dapper)。</span><span class="sxs-lookup"><span data-stu-id="297a2-116">You may wish to use a different data access approach, such as [Dapper](https://github.com/StackExchange/Dapper).</span></span> 

<span data-ttu-id="297a2-117">在上述每一種情況下，您都可以為您的儲存機制撰寫自訂的提供者，並將該提供者插入您的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="297a2-117">In each of these cases, you can write a customized provider for your storage mechanism and plug that provider into your app.</span></span>

<span data-ttu-id="297a2-118">ASP.NET Core 身分識別包含在具有 [個別使用者帳戶] 選項之 Visual Studio 的專案範本中。</span><span class="sxs-lookup"><span data-stu-id="297a2-118">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="297a2-119">使用 .NET Core CLI 時，請新增 `-au Individual`：</span><span class="sxs-lookup"><span data-stu-id="297a2-119">When using the .NET Core CLI, add `-au Individual`:</span></span>

```dotnetcli
dotnet new mvc -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a><span data-ttu-id="297a2-120">ASP.NET Core 身分識別架構</span><span class="sxs-lookup"><span data-stu-id="297a2-120">The ASP.NET Core Identity architecture</span></span>

<span data-ttu-id="297a2-121">ASP.NET Core 身分識別包含稱為「管理員」和「存放區」的類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-121">ASP.NET Core Identity consists of classes called managers and stores.</span></span> <span data-ttu-id="297a2-122">*管理員*是應用程式開發人員用來執行作業的高層級類別，例如建立身分識別使用者。</span><span class="sxs-lookup"><span data-stu-id="297a2-122">*Managers* are high-level classes which an app developer uses to perform operations, such as creating an Identity user.</span></span> <span data-ttu-id="297a2-123">存放*區*是較低層級的類別，可指定實體（例如使用者和角色）的保存方式。</span><span class="sxs-lookup"><span data-stu-id="297a2-123">*Stores* are lower-level classes that specify how entities, such as users and roles, are persisted.</span></span> <span data-ttu-id="297a2-124">存放區會遵循存放庫模式，並與持續性機制緊密結合。</span><span class="sxs-lookup"><span data-stu-id="297a2-124">Stores follow the repository pattern and are closely coupled with the persistence mechanism.</span></span> <span data-ttu-id="297a2-125">管理員會與存放區分離，這表示您可以取代持續性機制，而不需要變更應用程式代碼（設定除外）。</span><span class="sxs-lookup"><span data-stu-id="297a2-125">Managers are decoupled from stores, which means you can replace the persistence mechanism without changing your application code (except for configuration).</span></span>

<span data-ttu-id="297a2-126">下圖顯示 web 應用程式如何與管理員互動，同時存放區會與資料存取層互動。</span><span class="sxs-lookup"><span data-stu-id="297a2-126">The following diagram shows how a web app interacts with the managers, while stores interact with the data access layer.</span></span>

![ASP.NET Core 應用程式可與管理員合作（例如，' UserManager '、' RoleManager '）。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

<span data-ttu-id="297a2-129">若要建立自訂存放裝置提供者，請建立資料來源、資料存取層，以及與此資料存取層互動的存放區類別（上圖中的綠色和灰色方塊）。</span><span class="sxs-lookup"><span data-stu-id="297a2-129">To create a custom storage provider, create the data source, the data access layer, and the store classes that interact with this data access layer (the green and grey boxes in the diagram above).</span></span> <span data-ttu-id="297a2-130">您不需要自訂與它們互動的管理員或應用程式程式碼（上面的藍色方塊）。</span><span class="sxs-lookup"><span data-stu-id="297a2-130">You don't need to customize the managers or your app code that interacts with them (the blue boxes above).</span></span>

<span data-ttu-id="297a2-131">建立 `UserManager` 或 `RoleManager` 的新實例時，您會提供使用者類別的類型，並傳遞 store 類別的實例做為引數。</span><span class="sxs-lookup"><span data-stu-id="297a2-131">When creating a new instance of `UserManager` or `RoleManager` you provide the type of the user class and pass an instance of the store class as an argument.</span></span> <span data-ttu-id="297a2-132">這種方法可讓您將自訂類別插入 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="297a2-132">This approach enables you to plug your customized classes into ASP.NET Core.</span></span> 

<span data-ttu-id="297a2-133">[重新設定應用程式以使用新的存放裝置提供者](#reconfigure-app-to-use-a-new-storage-provider)示範如何具現化 `UserManager`，並使用自訂的存放區 `RoleManager`。</span><span class="sxs-lookup"><span data-stu-id="297a2-133">[Reconfigure app to use new storage provider](#reconfigure-app-to-use-a-new-storage-provider) shows how to instantiate `UserManager` and `RoleManager` with a customized store.</span></span>

## <a name="aspnet-core-identity-stores-data-types"></a><span data-ttu-id="297a2-134">ASP.NET Core 身分識別儲存資料類型</span><span class="sxs-lookup"><span data-stu-id="297a2-134">ASP.NET Core Identity stores data types</span></span>

<span data-ttu-id="297a2-135">[ASP.NET Core 識別](https://github.com/aspnet/identity)資料類型會在下列各節中詳細說明：</span><span class="sxs-lookup"><span data-stu-id="297a2-135">[ASP.NET Core Identity](https://github.com/aspnet/identity) data types are detailed in the following sections:</span></span>

### <a name="users"></a><span data-ttu-id="297a2-136">使用者</span><span class="sxs-lookup"><span data-stu-id="297a2-136">Users</span></span>

<span data-ttu-id="297a2-137">網站的已註冊使用者。</span><span class="sxs-lookup"><span data-stu-id="297a2-137">Registered users of your web site.</span></span> <span data-ttu-id="297a2-138">[IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser)類型可以擴充或當做您自己自訂類型的範例使用。</span><span class="sxs-lookup"><span data-stu-id="297a2-138">The [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) type may be extended or used as an example for your own custom type.</span></span> <span data-ttu-id="297a2-139">您不需要從特定類型繼承，即可實作為您自己的自訂身分識別儲存體解決方案。</span><span class="sxs-lookup"><span data-stu-id="297a2-139">You don't need to inherit from a particular type to implement your own custom identity storage solution.</span></span>

### <a name="user-claims"></a><span data-ttu-id="297a2-140">使用者宣告</span><span class="sxs-lookup"><span data-stu-id="297a2-140">User Claims</span></span>

<span data-ttu-id="297a2-141">使用者的一組語句（或[宣告](/dotnet/api/system.security.claims.claim)），代表使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="297a2-141">A set of statements (or [Claims](/dotnet/api/system.security.claims.claim)) about the user that represent the user's identity.</span></span> <span data-ttu-id="297a2-142">可以啟用使用者身分識別的更大運算式，而不是透過角色來達成。</span><span class="sxs-lookup"><span data-stu-id="297a2-142">Can enable greater expression of the user's identity than can be achieved through roles.</span></span>

### <a name="user-logins"></a><span data-ttu-id="297a2-143">使用者登入</span><span class="sxs-lookup"><span data-stu-id="297a2-143">User Logins</span></span>

<span data-ttu-id="297a2-144">要在使用者登入時使用的外部驗證提供者（例如 Facebook 或 Microsoft 帳戶）的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="297a2-144">Information about the external authentication provider (like Facebook or a Microsoft account) to use when logging in a user.</span></span> [<span data-ttu-id="297a2-145">範例</span><span class="sxs-lookup"><span data-stu-id="297a2-145">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a><span data-ttu-id="297a2-146">角色</span><span class="sxs-lookup"><span data-stu-id="297a2-146">Roles</span></span>

<span data-ttu-id="297a2-147">網站的授權群組。</span><span class="sxs-lookup"><span data-stu-id="297a2-147">Authorization groups for your site.</span></span> <span data-ttu-id="297a2-148">包含角色識別碼和角色名稱（例如 "Admin" 或 "Employee"）。</span><span class="sxs-lookup"><span data-stu-id="297a2-148">Includes the role Id and role name (like "Admin" or "Employee").</span></span> [<span data-ttu-id="297a2-149">範例</span><span class="sxs-lookup"><span data-stu-id="297a2-149">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a><span data-ttu-id="297a2-150">資料存取層</span><span class="sxs-lookup"><span data-stu-id="297a2-150">The data access layer</span></span>

<span data-ttu-id="297a2-151">本主題假設您熟悉要使用的持續性機制，以及如何建立該機制的實體。</span><span class="sxs-lookup"><span data-stu-id="297a2-151">This topic assumes you are familiar with the persistence mechanism that you are going to use and how to create entities for that mechanism.</span></span> <span data-ttu-id="297a2-152">本主題不提供如何建立存放庫或資料存取類別的詳細資料;當您使用 ASP.NET Core 身分識別時，它會提供有關設計決策的一些建議。</span><span class="sxs-lookup"><span data-stu-id="297a2-152">This topic doesn't provide details about how to create the repositories or data access classes; it provides some suggestions about design decisions when working with ASP.NET Core Identity.</span></span>

<span data-ttu-id="297a2-153">在設計自訂存放區提供者的資料存取層時，您有很多自由。</span><span class="sxs-lookup"><span data-stu-id="297a2-153">You have a lot of freedom when designing the data access layer for a customized store provider.</span></span> <span data-ttu-id="297a2-154">您只需要為您想要在應用程式中使用的功能建立持續性機制。</span><span class="sxs-lookup"><span data-stu-id="297a2-154">You only need to create persistence mechanisms for features that you intend to use in your app.</span></span> <span data-ttu-id="297a2-155">例如，如果您未在應用程式中使用角色，則不需要建立角色或使用者角色關聯的儲存體。</span><span class="sxs-lookup"><span data-stu-id="297a2-155">For example, if you are not using roles in your app, you don't need to create storage for roles or user role associations.</span></span> <span data-ttu-id="297a2-156">您的技術和現有的基礎結構可能需要與 ASP.NET Core 身分識別的預設執行非常不同的結構。</span><span class="sxs-lookup"><span data-stu-id="297a2-156">Your technology and existing infrastructure may require a structure that's very different from the default implementation of ASP.NET Core Identity.</span></span> <span data-ttu-id="297a2-157">在您的資料存取層中，您會提供邏輯來處理您的儲存體執行結構。</span><span class="sxs-lookup"><span data-stu-id="297a2-157">In your data access layer, you provide the logic to work with the structure of your storage implementation.</span></span>

<span data-ttu-id="297a2-158">資料存取層提供將資料從 ASP.NET Core 身分識別儲存至資料來源的邏輯。</span><span class="sxs-lookup"><span data-stu-id="297a2-158">The data access layer provides the logic to save the data from ASP.NET Core Identity to a data source.</span></span> <span data-ttu-id="297a2-159">您自訂的儲存提供者的資料存取層可能包含下列用來儲存使用者和角色資訊的類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-159">The data access layer for your customized storage provider might include the following classes to store user and role information.</span></span>

### <a name="context-class"></a><span data-ttu-id="297a2-160">Context 類別</span><span class="sxs-lookup"><span data-stu-id="297a2-160">Context class</span></span>

<span data-ttu-id="297a2-161">封裝資訊，以連接到您的持續性機制並執行查詢。</span><span class="sxs-lookup"><span data-stu-id="297a2-161">Encapsulates the information to connect to your persistence mechanism and execute queries.</span></span> <span data-ttu-id="297a2-162">有數個數據類別需要這個類別的實例，通常是透過相依性插入來提供。</span><span class="sxs-lookup"><span data-stu-id="297a2-162">Several data classes require an instance of this class, typically provided through dependency injection.</span></span> <span data-ttu-id="297a2-163">[範例](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)。</span><span class="sxs-lookup"><span data-stu-id="297a2-163">[Example](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).</span></span>

### <a name="user-storage"></a><span data-ttu-id="297a2-164">使用者儲存體</span><span class="sxs-lookup"><span data-stu-id="297a2-164">User Storage</span></span>

<span data-ttu-id="297a2-165">儲存並抓取使用者資訊（例如使用者名稱和密碼雜湊）。</span><span class="sxs-lookup"><span data-stu-id="297a2-165">Stores and retrieves user information (such as user name and password hash).</span></span> [<span data-ttu-id="297a2-166">範例</span><span class="sxs-lookup"><span data-stu-id="297a2-166">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a><span data-ttu-id="297a2-167">角色儲存體</span><span class="sxs-lookup"><span data-stu-id="297a2-167">Role Storage</span></span>

<span data-ttu-id="297a2-168">儲存並抓取角色資訊（例如角色名稱）。</span><span class="sxs-lookup"><span data-stu-id="297a2-168">Stores and retrieves role information (such as the role name).</span></span> [<span data-ttu-id="297a2-169">範例</span><span class="sxs-lookup"><span data-stu-id="297a2-169">Example</span></span>](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a><span data-ttu-id="297a2-170">UserClaims 儲存體</span><span class="sxs-lookup"><span data-stu-id="297a2-170">UserClaims Storage</span></span>

<span data-ttu-id="297a2-171">儲存並抓取使用者宣告資訊（例如宣告類型和值）。</span><span class="sxs-lookup"><span data-stu-id="297a2-171">Stores and retrieves user claim information (such as the claim type and value).</span></span> [<span data-ttu-id="297a2-172">範例</span><span class="sxs-lookup"><span data-stu-id="297a2-172">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a><span data-ttu-id="297a2-173">UserLogins 儲存體</span><span class="sxs-lookup"><span data-stu-id="297a2-173">UserLogins Storage</span></span>

<span data-ttu-id="297a2-174">儲存並抓取使用者登入資訊（例如外部驗證提供者）。</span><span class="sxs-lookup"><span data-stu-id="297a2-174">Stores and retrieves user login information (such as an external authentication provider).</span></span> [<span data-ttu-id="297a2-175">範例</span><span class="sxs-lookup"><span data-stu-id="297a2-175">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a><span data-ttu-id="297a2-176">使用者角色存放裝置</span><span class="sxs-lookup"><span data-stu-id="297a2-176">UserRole Storage</span></span>

<span data-ttu-id="297a2-177">儲存並抓取哪些角色會指派給哪些使用者。</span><span class="sxs-lookup"><span data-stu-id="297a2-177">Stores and retrieves which roles are assigned to which users.</span></span> [<span data-ttu-id="297a2-178">範例</span><span class="sxs-lookup"><span data-stu-id="297a2-178">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

<span data-ttu-id="297a2-179">**秘訣：** 只會執行您想要在應用程式中使用的類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-179">**TIP:** Only implement the classes you intend to use in your app.</span></span>

<span data-ttu-id="297a2-180">在資料存取類別中，提供程式碼來執行持續性機制的資料作業。</span><span class="sxs-lookup"><span data-stu-id="297a2-180">In the data access classes, provide code to perform data operations for your persistence mechanism.</span></span> <span data-ttu-id="297a2-181">例如，在自訂提供者內，您可能會有下列程式碼，在*store*類別中建立新的使用者：</span><span class="sxs-lookup"><span data-stu-id="297a2-181">For example, within a custom provider, you might have the following code to create a new user in the *store* class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

<span data-ttu-id="297a2-182">建立使用者的執行邏輯位於 `_usersTable.CreateAsync` 方法中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="297a2-182">The implementation logic for creating the user is in the `_usersTable.CreateAsync` method, shown below.</span></span>

## <a name="customize-the-user-class"></a><span data-ttu-id="297a2-183">自訂使用者類別</span><span class="sxs-lookup"><span data-stu-id="297a2-183">Customize the user class</span></span>

<span data-ttu-id="297a2-184">在執行儲存區提供者時，建立相當於[IdentityUser 類別](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser)的使用者類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-184">When implementing a storage provider, create a user class which is equivalent to the [IdentityUser class](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).</span></span>

<span data-ttu-id="297a2-185">您的使用者類別至少必須包含 `Id` 和 `UserName` 屬性。</span><span class="sxs-lookup"><span data-stu-id="297a2-185">At a minimum, your user class must include an `Id` and a `UserName` property.</span></span>

<span data-ttu-id="297a2-186">`IdentityUser` 類別會定義在執行要求的作業時，`UserManager` 所呼叫的屬性。</span><span class="sxs-lookup"><span data-stu-id="297a2-186">The `IdentityUser` class defines the properties that the `UserManager` calls when performing requested operations.</span></span> <span data-ttu-id="297a2-187">`Id` 屬性的預設類型為字串，但是您可以從 `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` 繼承並指定不同的類型。</span><span class="sxs-lookup"><span data-stu-id="297a2-187">The default type of the `Id` property is a string, but you can inherit from `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` and specify a different type.</span></span> <span data-ttu-id="297a2-188">架構預期儲存體的執行方式可以處理資料類型轉換。</span><span class="sxs-lookup"><span data-stu-id="297a2-188">The framework expects the storage implementation to handle data type conversions.</span></span>

## <a name="customize-the-user-store"></a><span data-ttu-id="297a2-189">自訂使用者存放區</span><span class="sxs-lookup"><span data-stu-id="297a2-189">Customize the user store</span></span>

<span data-ttu-id="297a2-190">建立 `UserStore` 類別，以提供使用者上所有資料作業的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-190">Create a `UserStore` class that provides the methods for all data operations on the user.</span></span> <span data-ttu-id="297a2-191">這個類別相當於[UserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-191">This class is equivalent to the [UserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) class.</span></span> <span data-ttu-id="297a2-192">在您的 `UserStore` 類別中，執行所需的 `IUserStore<TUser>` 和選擇性介面。</span><span class="sxs-lookup"><span data-stu-id="297a2-192">In your `UserStore` class, implement `IUserStore<TUser>` and the optional interfaces required.</span></span> <span data-ttu-id="297a2-193">您可以根據應用程式中提供的功能，選取要執行的選擇性介面。</span><span class="sxs-lookup"><span data-stu-id="297a2-193">You select which optional interfaces to implement based on the functionality provided in your app.</span></span>

### <a name="optional-interfaces"></a><span data-ttu-id="297a2-194">選擇性介面</span><span class="sxs-lookup"><span data-stu-id="297a2-194">Optional interfaces</span></span>

* [<span data-ttu-id="297a2-195">IUserRoleStore</span><span class="sxs-lookup"><span data-stu-id="297a2-195">IUserRoleStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [<span data-ttu-id="297a2-196">IUserClaimStore</span><span class="sxs-lookup"><span data-stu-id="297a2-196">IUserClaimStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [<span data-ttu-id="297a2-197">IUserPasswordStore</span><span class="sxs-lookup"><span data-stu-id="297a2-197">IUserPasswordStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [<span data-ttu-id="297a2-198">IUserSecurityStampStore</span><span class="sxs-lookup"><span data-stu-id="297a2-198">IUserSecurityStampStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [<span data-ttu-id="297a2-199">IUserEmailStore</span><span class="sxs-lookup"><span data-stu-id="297a2-199">IUserEmailStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [<span data-ttu-id="297a2-200">IUserPhoneNumberStore</span><span class="sxs-lookup"><span data-stu-id="297a2-200">IUserPhoneNumberStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)
* [<span data-ttu-id="297a2-201">IQueryableUserStore</span><span class="sxs-lookup"><span data-stu-id="297a2-201">IQueryableUserStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [<span data-ttu-id="297a2-202">IUserLoginStore</span><span class="sxs-lookup"><span data-stu-id="297a2-202">IUserLoginStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [<span data-ttu-id="297a2-203">IUserTwoFactorStore</span><span class="sxs-lookup"><span data-stu-id="297a2-203">IUserTwoFactorStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [<span data-ttu-id="297a2-204">IUserLockoutStore</span><span class="sxs-lookup"><span data-stu-id="297a2-204">IUserLockoutStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

<span data-ttu-id="297a2-205">選擇性的介面會繼承自 `IUserStore<TUser>`。</span><span class="sxs-lookup"><span data-stu-id="297a2-205">The optional interfaces inherit from `IUserStore<TUser>`.</span></span> <span data-ttu-id="297a2-206">您可以在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)中看到部分實作為範例使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="297a2-206">You can see a partially implemented sample user store in the [sample app](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).</span></span>

<span data-ttu-id="297a2-207">在 `UserStore` 類別中，您可以使用您所建立的資料存取類別來執行作業。</span><span class="sxs-lookup"><span data-stu-id="297a2-207">Within the `UserStore` class, you use the data access classes that you created to perform operations.</span></span> <span data-ttu-id="297a2-208">這些會使用相依性插入來傳入。</span><span class="sxs-lookup"><span data-stu-id="297a2-208">These are passed in using dependency injection.</span></span> <span data-ttu-id="297a2-209">例如，在使用 Dapper 執行的 SQL Server 中，`UserStore` 類別的 `CreateAsync` 方法會使用 `DapperUsersTable` 的實例來插入新的記錄：</span><span class="sxs-lookup"><span data-stu-id="297a2-209">For example, in the SQL Server with Dapper implementation, the `UserStore` class has the `CreateAsync` method which uses an instance of `DapperUsersTable` to insert a new record:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a><span data-ttu-id="297a2-210">自訂使用者存放區時所要執行的介面</span><span class="sxs-lookup"><span data-stu-id="297a2-210">Interfaces to implement when customizing user store</span></span>

* <span data-ttu-id="297a2-211">**IUserStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-211">**IUserStore**</span></span>  
 <span data-ttu-id="297a2-212">[IUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1)介面是您必須在使用者存放區中執行的唯一介面。</span><span class="sxs-lookup"><span data-stu-id="297a2-212">The [IUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) interface is the only interface you must implement in the user store.</span></span> <span data-ttu-id="297a2-213">它會定義用來建立、更新、刪除和抓取使用者的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-213">It defines methods for creating, updating, deleting, and retrieving users.</span></span>
* <span data-ttu-id="297a2-214">**IUserClaimStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-214">**IUserClaimStore**</span></span>  
 <span data-ttu-id="297a2-215">[IUserClaimStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)介面會定義您所執行的方法，以啟用使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="297a2-215">The [IUserClaimStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface defines the methods you implement to enable user claims.</span></span> <span data-ttu-id="297a2-216">其中包含新增、移除和抓取使用者宣告的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-216">It contains methods for adding, removing and retrieving user claims.</span></span>
* <span data-ttu-id="297a2-217">**IUserLoginStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-217">**IUserLoginStore**</span></span>  
 <span data-ttu-id="297a2-218">[IUserLoginStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)定義您為了啟用外部驗證提供者而執行的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-218">The [IUserLoginStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) defines the methods you implement to enable external authentication providers.</span></span> <span data-ttu-id="297a2-219">其中包含加入、移除和抓取使用者登入的方法，以及根據登入資訊來抓取使用者的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-219">It contains methods for adding, removing and retrieving user logins, and a method for retrieving a user based on the login information.</span></span>
* <span data-ttu-id="297a2-220">**IUserRoleStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-220">**IUserRoleStore**</span></span>  
 <span data-ttu-id="297a2-221">[IUserRoleStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)介面會定義您為了將使用者對應至角色而執行的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-221">The [IUserRoleStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface defines the methods you implement to map a user to a role.</span></span> <span data-ttu-id="297a2-222">其中包含新增、移除和抓取使用者角色的方法，以及檢查使用者是否已指派給角色的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-222">It contains methods to add, remove, and retrieve a user's roles, and a method to check if a user is assigned to a role.</span></span>
* <span data-ttu-id="297a2-223">**IUserPasswordStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-223">**IUserPasswordStore**</span></span>  
 <span data-ttu-id="297a2-224">[IUserPasswordStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)介面會定義您所執行的方法，以保存雜湊的密碼。</span><span class="sxs-lookup"><span data-stu-id="297a2-224">The [IUserPasswordStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface defines the methods you implement to persist hashed passwords.</span></span> <span data-ttu-id="297a2-225">其中包含取得和設定雜湊密碼的方法，以及指出使用者是否已設定密碼的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-225">It contains methods for getting and setting the hashed password, and a method that indicates whether the user has set a password.</span></span>
* <span data-ttu-id="297a2-226">**IUserSecurityStampStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-226">**IUserSecurityStampStore**</span></span>  
 <span data-ttu-id="297a2-227">[IUserSecurityStampStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)介面會定義您所執行的方法，以使用安全性戳記來指出使用者的帳戶資訊是否已變更。</span><span class="sxs-lookup"><span data-stu-id="297a2-227">The [IUserSecurityStampStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface defines the methods you implement to use a security stamp for indicating whether the user's account information has changed.</span></span> <span data-ttu-id="297a2-228">當使用者變更密碼或新增或移除登入時，就會更新此戳記。</span><span class="sxs-lookup"><span data-stu-id="297a2-228">This stamp is updated when a user changes the password, or adds or removes logins.</span></span> <span data-ttu-id="297a2-229">其中包含取得和設定安全性戳記的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-229">It contains methods for getting and setting the security stamp.</span></span>
* <span data-ttu-id="297a2-230">**IUserTwoFactorStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-230">**IUserTwoFactorStore**</span></span>  
 <span data-ttu-id="297a2-231">[IUserTwoFactorStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)介面會定義您所執行的方法，以支援雙因素驗證。</span><span class="sxs-lookup"><span data-stu-id="297a2-231">The [IUserTwoFactorStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface defines the methods you implement to support two factor authentication.</span></span> <span data-ttu-id="297a2-232">其中包含取得和設定是否為使用者啟用雙因素驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-232">It contains methods for getting and setting whether two factor authentication is enabled for a user.</span></span>
* <span data-ttu-id="297a2-233">**IUserPhoneNumberStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-233">**IUserPhoneNumberStore**</span></span>  
 <span data-ttu-id="297a2-234">[IUserPhoneNumberStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)介面會定義您要用來儲存使用者電話號碼的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-234">The [IUserPhoneNumberStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface defines the methods you implement to store user phone numbers.</span></span> <span data-ttu-id="297a2-235">其中包含取得和設定電話號碼的方法，以及電話號碼是否已確認。</span><span class="sxs-lookup"><span data-stu-id="297a2-235">It contains methods for getting and setting the phone number and whether the phone number is confirmed.</span></span>
* <span data-ttu-id="297a2-236">**IUserEmailStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-236">**IUserEmailStore**</span></span>  
 <span data-ttu-id="297a2-237">[IUserEmailStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)介面會定義您要用來儲存使用者電子郵件地址的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-237">The [IUserEmailStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface defines the methods you implement to store user email addresses.</span></span> <span data-ttu-id="297a2-238">其中包含取得和設定電子郵件地址的方法，以及是否確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="297a2-238">It contains methods for getting and setting the email address and whether the email is confirmed.</span></span>
* <span data-ttu-id="297a2-239">**IUserLockoutStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-239">**IUserLockoutStore**</span></span>  
 <span data-ttu-id="297a2-240">[IUserLockoutStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)介面會定義您所執行的方法，以儲存鎖定帳戶的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="297a2-240">The [IUserLockoutStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface defines the methods you implement to store information about locking an account.</span></span> <span data-ttu-id="297a2-241">其中包含追蹤失敗的存取嘗試和鎖定的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-241">It contains methods for tracking failed access attempts and lockouts.</span></span>
* <span data-ttu-id="297a2-242">**IQueryableUserStore**</span><span class="sxs-lookup"><span data-stu-id="297a2-242">**IQueryableUserStore**</span></span>  
 <span data-ttu-id="297a2-243">[IQueryableUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)介面會定義您所執行的成員，以提供可查詢的使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="297a2-243">The [IQueryableUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface defines the members you implement to provide a queryable user store.</span></span>

<span data-ttu-id="297a2-244">您只會執行應用程式所需的介面。</span><span class="sxs-lookup"><span data-stu-id="297a2-244">You implement only the interfaces that are needed in your app.</span></span> <span data-ttu-id="297a2-245">例如：</span><span class="sxs-lookup"><span data-stu-id="297a2-245">For example:</span></span>

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a><span data-ttu-id="297a2-246">IdentityUserClaim、IdentityUserLogin 和 IdentityUserRole</span><span class="sxs-lookup"><span data-stu-id="297a2-246">IdentityUserClaim, IdentityUserLogin, and IdentityUserRole</span></span>

<span data-ttu-id="297a2-247">命名空間包含[IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)、[IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin) 和 [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) 類別的實作為。`Microsoft.AspNet.Identity.EntityFramework`</span><span class="sxs-lookup"><span data-stu-id="297a2-247">The `Microsoft.AspNet.Identity.EntityFramework` namespace contains implementations of the [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), and [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes.</span></span> <span data-ttu-id="297a2-248">如果您使用這些功能，您可能會想要建立您自己的類別版本，並定義應用程式的屬性。</span><span class="sxs-lookup"><span data-stu-id="297a2-248">If you are using these features, you may want to create your own versions of these classes and define the properties for your app.</span></span> <span data-ttu-id="297a2-249">不過，有時候在執行基本作業（例如新增或移除使用者的宣告）時，不會將這些實體載入記憶體的效率較高。</span><span class="sxs-lookup"><span data-stu-id="297a2-249">However, sometimes it's more efficient to not load these entities into memory when performing basic operations (such as adding or removing a user's claim).</span></span> <span data-ttu-id="297a2-250">相反地，後端存放區類別可以直接在資料來源上執行這些作業。</span><span class="sxs-lookup"><span data-stu-id="297a2-250">Instead, the backend store classes can execute these operations directly on the data source.</span></span> <span data-ttu-id="297a2-251">例如，`UserStore.GetClaimsAsync` 方法可以呼叫 `userClaimTable.FindByUserId(user.Id)` 方法，直接對該資料表執行查詢，並傳回宣告的清單。</span><span class="sxs-lookup"><span data-stu-id="297a2-251">For example, the `UserStore.GetClaimsAsync` method can call the `userClaimTable.FindByUserId(user.Id)` method to execute a query on that table directly and return a list of claims.</span></span>

## <a name="customize-the-role-class"></a><span data-ttu-id="297a2-252">自訂角色類別</span><span class="sxs-lookup"><span data-stu-id="297a2-252">Customize the role class</span></span>

<span data-ttu-id="297a2-253">在執行角色儲存提供者時，您可以建立自訂角色類型。</span><span class="sxs-lookup"><span data-stu-id="297a2-253">When implementing a role storage provider, you can create a custom role type.</span></span> <span data-ttu-id="297a2-254">它不需要執行特定介面，但必須有 `Id`，而且通常會有 `Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="297a2-254">It need not implement a particular interface, but it must have an `Id` and typically it will have a `Name` property.</span></span>

<span data-ttu-id="297a2-255">以下是範例角色類別：</span><span class="sxs-lookup"><span data-stu-id="297a2-255">The following is an example role class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a><span data-ttu-id="297a2-256">自訂角色存放區</span><span class="sxs-lookup"><span data-stu-id="297a2-256">Customize the role store</span></span>

<span data-ttu-id="297a2-257">您可以建立 `RoleStore` 類別，以提供角色上所有資料作業的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-257">You can create a `RoleStore` class that provides the methods for all data operations on roles.</span></span> <span data-ttu-id="297a2-258">這個類別相當於[RoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-258">This class is equivalent to the [RoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) class.</span></span> <span data-ttu-id="297a2-259">在 `RoleStore` 類別中，您會執行 `IRoleStore<TRole>` 和選擇性的 `IQueryableRoleStore<TRole>` 介面。</span><span class="sxs-lookup"><span data-stu-id="297a2-259">In the `RoleStore` class, you implement the `IRoleStore<TRole>` and optionally the `IQueryableRoleStore<TRole>` interface.</span></span>

* <span data-ttu-id="297a2-260">**IRoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="297a2-260">**IRoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="297a2-261">[IRoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1)介面會定義要在角色存放區類別中執行的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-261">The [IRoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interface defines the methods to implement in the role store class.</span></span> <span data-ttu-id="297a2-262">其中包含建立、更新、刪除和抓取角色的方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-262">It contains methods for creating, updating, deleting, and retrieving roles.</span></span>
* <span data-ttu-id="297a2-263">**RoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="297a2-263">**RoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="297a2-264">若要自訂 `RoleStore`，請建立一個可執行 `IRoleStore<TRole>` 介面的類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-264">To customize `RoleStore`, create a class that implements the `IRoleStore<TRole>` interface.</span></span> 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a><span data-ttu-id="297a2-265">重新設定應用程式以使用新的存放裝置提供者</span><span class="sxs-lookup"><span data-stu-id="297a2-265">Reconfigure app to use a new storage provider</span></span>

<span data-ttu-id="297a2-266">一旦您已執行存放裝置提供者，您可以設定應用程式來使用它。</span><span class="sxs-lookup"><span data-stu-id="297a2-266">Once you have implemented a storage provider, you configure your app to use it.</span></span> <span data-ttu-id="297a2-267">如果您的應用程式使用預設提供者，請將它取代為您的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="297a2-267">If your app used the default provider, replace it with your custom provider.</span></span>

1. <span data-ttu-id="297a2-268">移除 `Microsoft.AspNetCore.EntityFramework.Identity` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="297a2-268">Remove the `Microsoft.AspNetCore.EntityFramework.Identity` NuGet package.</span></span>
1. <span data-ttu-id="297a2-269">如果存放裝置提供者位於不同的專案或封裝中，請新增其參考。</span><span class="sxs-lookup"><span data-stu-id="297a2-269">If the storage provider resides in a separate project or package, add a reference to it.</span></span>
1. <span data-ttu-id="297a2-270">使用儲存提供者命名空間的 using 語句，取代所有 `Microsoft.AspNetCore.EntityFramework.Identity` 的參考。</span><span class="sxs-lookup"><span data-stu-id="297a2-270">Replace all references to `Microsoft.AspNetCore.EntityFramework.Identity` with a using statement for the namespace of your storage provider.</span></span>
1. <span data-ttu-id="297a2-271">在 `ConfigureServices` 方法中，將 `AddIdentity` 方法變更為使用您的自訂類型。</span><span class="sxs-lookup"><span data-stu-id="297a2-271">In the `ConfigureServices` method, change the `AddIdentity` method to use your custom types.</span></span> <span data-ttu-id="297a2-272">您可以針對此目的建立自己的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="297a2-272">You can create your own extension methods for this purpose.</span></span> <span data-ttu-id="297a2-273">如需範例，請參閱[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) 。</span><span class="sxs-lookup"><span data-stu-id="297a2-273">See [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) for an example.</span></span>
1. <span data-ttu-id="297a2-274">如果您使用角色，請更新 `RoleManager`，以使用您的 `RoleStore` 類別。</span><span class="sxs-lookup"><span data-stu-id="297a2-274">If you are using Roles, update the `RoleManager` to use your `RoleStore` class.</span></span>
1. <span data-ttu-id="297a2-275">將連接字串和認證更新為您的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="297a2-275">Update the connection string and credentials to your app's configuration.</span></span>

<span data-ttu-id="297a2-276">範例：</span><span class="sxs-lookup"><span data-stu-id="297a2-276">Example:</span></span>

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

## <a name="references"></a><span data-ttu-id="297a2-277">參考</span><span class="sxs-lookup"><span data-stu-id="297a2-277">References</span></span>

* [<span data-ttu-id="297a2-278">ASP.NET 4.x 身分識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="297a2-278">Custom Storage Providers for ASP.NET 4.x Identity</span></span>](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
* <span data-ttu-id="297a2-279">[ASP.NET Core 身分識別](https://github.com/dotnet/AspNetCore/tree/master/src/Identity)&ndash; 此存放庫包含已維護之商店提供者的社區連結。</span><span class="sxs-lookup"><span data-stu-id="297a2-279">[ASP.NET Core Identity](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) &ndash; This repository includes links to community maintained store providers.</span></span>
