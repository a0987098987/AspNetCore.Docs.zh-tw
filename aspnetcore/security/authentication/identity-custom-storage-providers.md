---
title: "自訂的儲存提供者，ASP.NET Core 識別 |Microsoft 文件"
author: ardalis
description: "如何設定自訂儲存 ASP.NET Core 身分識別提供者。"
keywords: "ASP.NET Core，身分識別，自訂的存放裝置提供者"
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.assetid: b2ace545-ecf6-4664-b31e-b65bd4a6b025
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 214343c9b964baca3c436966d39caede1a58aba2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a><span data-ttu-id="e501c-104">ASP.NET Core 身分識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="e501c-104">Custom storage providers for ASP.NET Core Identity</span></span>

<span data-ttu-id="e501c-105">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e501c-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e501c-106">ASP.NET Core 身分識別是可擴充的系統可讓您建立自訂的儲存提供者，然後連接到您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e501c-106">ASP.NET Core Identity is an extensible system which enables you to create a custom storage provider and connect it to your app.</span></span> <span data-ttu-id="e501c-107">本主題描述如何建立 ASP.NET Core 身分識別的自訂儲存體提供者。</span><span class="sxs-lookup"><span data-stu-id="e501c-107">This topic describes how to create a customized storage provider for ASP.NET Core Identity.</span></span> <span data-ttu-id="e501c-108">它涵蓋建立自己的儲存體提供者的重要概念，但不是會逐步解說。</span><span class="sxs-lookup"><span data-stu-id="e501c-108">It covers the important concepts for creating your own storage provider, but is not a step-by-step walkthrough.</span></span>

<span data-ttu-id="e501c-109">[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)。</span><span class="sxs-lookup"><span data-stu-id="e501c-109">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).</span></span>

## <a name="introduction"></a><span data-ttu-id="e501c-110">簡介</span><span class="sxs-lookup"><span data-stu-id="e501c-110">Introduction</span></span>

<span data-ttu-id="e501c-111">根據預設，ASP.NET Core 識別系統會將使用者資訊儲存在 SQL Server 資料庫中使用 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="e501c-111">By default, the ASP.NET Core Identity system stores user information in a SQL Server database using Entity Framework Core.</span></span> <span data-ttu-id="e501c-112">這個方法對於許多應用程式，非常適合。</span><span class="sxs-lookup"><span data-stu-id="e501c-112">For many apps, this approach works well.</span></span> <span data-ttu-id="e501c-113">不過，您可能想要使用不同的持續性機制或資料結構描述。</span><span class="sxs-lookup"><span data-stu-id="e501c-113">However, you may prefer to use a different persistence mechanism or data schema.</span></span> <span data-ttu-id="e501c-114">例如: </span><span class="sxs-lookup"><span data-stu-id="e501c-114">For example:</span></span>

* <span data-ttu-id="e501c-115">您使用[Azure 資料表儲存體](https://docs.microsoft.com/azure/storage/)或另一個資料存放區。</span><span class="sxs-lookup"><span data-stu-id="e501c-115">You use [Azure Table Storage](https://docs.microsoft.com/azure/storage/) or another data store.</span></span>
* <span data-ttu-id="e501c-116">資料庫資料表有不同的結構。</span><span class="sxs-lookup"><span data-stu-id="e501c-116">Your database tables have a different structure.</span></span> 
* <span data-ttu-id="e501c-117">您可能想要使用不同的資料存取方法，例如[Dapper](https://github.com/StackExchange/Dapper)。</span><span class="sxs-lookup"><span data-stu-id="e501c-117">You may wish to use a different data access approach, such as [Dapper](https://github.com/StackExchange/Dapper).</span></span> 

<span data-ttu-id="e501c-118">在每個這種情況下，您可以撰寫自訂提供者的儲存機制，並插入您的應用程式中的該提供者。</span><span class="sxs-lookup"><span data-stu-id="e501c-118">In each of these cases, you can write a customized provider for your storage mechanism and plug that provider into your app.</span></span>

<span data-ttu-id="e501c-119">ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。</span><span class="sxs-lookup"><span data-stu-id="e501c-119">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="e501c-120">當使用.NET 核心 CLI，新增`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="e501c-120">When using the .NET Core CLI, add `-au Individual`:</span></span>

```
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a><span data-ttu-id="e501c-121">ASP.NET Core 識別架構</span><span class="sxs-lookup"><span data-stu-id="e501c-121">The ASP.NET Core Identity architecture</span></span>

<span data-ttu-id="e501c-122">ASP.NET Core 識別類別，稱為管理員和存放區所組成。</span><span class="sxs-lookup"><span data-stu-id="e501c-122">ASP.NET Core Identity consists of classes called managers and stores.</span></span> <span data-ttu-id="e501c-123">*管理員*是高階應用程式開發人員用來執行作業，例如建立身分識別使用者的類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-123">*Managers* are high-level classes which an app developer uses to perform operations, such as creating an Identity user.</span></span> <span data-ttu-id="e501c-124">*存放區*是較低層級的類別，指定如何保存實體，例如使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="e501c-124">*Stores* are lower-level classes that specify how entities, such as users and roles, are persisted.</span></span> <span data-ttu-id="e501c-125">請遵循存放區[儲存機制模式](http://deviq.com/repository-pattern/)和緊密結合的持續性機制。</span><span class="sxs-lookup"><span data-stu-id="e501c-125">Stores follow the [repository pattern](http://deviq.com/repository-pattern/) and are closely coupled with the persistence mechanism.</span></span> <span data-ttu-id="e501c-126">管理員會分開存放區，這表示您可以取代持續性機制，而不需要變更您的應用程式程式碼 （除了組態）。</span><span class="sxs-lookup"><span data-stu-id="e501c-126">Managers are decoupled from stores, which means you can replace the persistence mechanism without changing your application code (except for configuration).</span></span>

<span data-ttu-id="e501c-127">下圖顯示 web 應用程式的互動方式經理，而與資料存取層的存放區互動。</span><span class="sxs-lookup"><span data-stu-id="e501c-127">The following diagram shows how a web app interacts with the managers, while stores interact with the data access layer.</span></span>

![ASP.NET Core 應用程式使用 （例如，'UserManager'、 'RoleManager'） 的管理員。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

<span data-ttu-id="e501c-130">若要建立自訂的儲存提供者，建立資料來源、 資料存取層級和與此資料存取層 （綠色和灰色方塊上圖所示） 互動的存放區類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-130">To create a custom storage provider, create the data source, the data access layer, and the store classes that interact with this data access layer (the green and grey boxes in the diagram above).</span></span> <span data-ttu-id="e501c-131">您不需要自訂的管理員或應用程式程式碼互動 （藍色方塊上面）。</span><span class="sxs-lookup"><span data-stu-id="e501c-131">You don't need to customize the managers or your app code that interacts with them (the blue boxes above).</span></span>

<span data-ttu-id="e501c-132">建立的新執行個體時`UserManager`或`RoleManager`您提供使用者類別的型別，並將存放區類別的執行個體傳遞做為引數。</span><span class="sxs-lookup"><span data-stu-id="e501c-132">When creating a new instance of `UserManager` or `RoleManager` you provide the type of the user class and pass an instance of the store class as an argument.</span></span> <span data-ttu-id="e501c-133">這個方法可讓您將 ASP.NET Core 插入您的自訂的類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-133">This approach enables you to plug your customized classes into ASP.NET Core.</span></span> 

<span data-ttu-id="e501c-134">[重新設定應用程式使用新的存放裝置提供者](#reconfigure-app-to-use-new-storage-provider)示範如何具現化`UserManager`和`RoleManager`與自訂存放區。</span><span class="sxs-lookup"><span data-stu-id="e501c-134">[Reconfigure app to use new storage provider](#reconfigure-app-to-use-new-storage-provider) shows how to instantiate `UserManager` and `RoleManager` with a customized store.</span></span>

## <a name="aspnet-core-identity-stores-data-types"></a><span data-ttu-id="e501c-135">ASP.NET Core 身分識別儲存的資料類型</span><span class="sxs-lookup"><span data-stu-id="e501c-135">ASP.NET Core Identity stores data types</span></span>

<span data-ttu-id="e501c-136">[ASP.NET Core 識別](https://github.com/aspnet/identity)資料類型詳述於以下各節：</span><span class="sxs-lookup"><span data-stu-id="e501c-136">[ASP.NET Core Identity](https://github.com/aspnet/identity) data types are detailed in the following sections:</span></span>

### <a name="users"></a><span data-ttu-id="e501c-137">使用者</span><span class="sxs-lookup"><span data-stu-id="e501c-137">Users</span></span>

<span data-ttu-id="e501c-138">註冊您的網站的使用者。</span><span class="sxs-lookup"><span data-stu-id="e501c-138">Registered users of your web site.</span></span> <span data-ttu-id="e501c-139">[IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)類型可能會擴充，或使用您自己的自訂類型的範例。</span><span class="sxs-lookup"><span data-stu-id="e501c-139">The [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) type may be extended or used as an example for your own custom type.</span></span> <span data-ttu-id="e501c-140">您不需要繼承自特定的型別，以實作您自己的自訂身分識別儲存體解決方案。</span><span class="sxs-lookup"><span data-stu-id="e501c-140">You do not need to inherit from a particular type to implement your own custom identity storage solution.</span></span>

### <a name="user-claims"></a><span data-ttu-id="e501c-141">使用者宣告</span><span class="sxs-lookup"><span data-stu-id="e501c-141">User Claims</span></span>

<span data-ttu-id="e501c-142">一組陳述式 (或[宣告](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)代表使用者的身分識別之使用者的相關。</span><span class="sxs-lookup"><span data-stu-id="e501c-142">A set of statements (or [Claims](https://docs.microsoft.com//dotnet/api/system.security.claims.claim) about the user that represent the user's identity.</span></span> <span data-ttu-id="e501c-143">可以啟用更高的使用者身分識別與透過角色能達到之效果的運算式。</span><span class="sxs-lookup"><span data-stu-id="e501c-143">Can enable greater expression of the user's identity than can be achieved through roles.</span></span>

### <a name="user-logins"></a><span data-ttu-id="e501c-144">使用者登入</span><span class="sxs-lookup"><span data-stu-id="e501c-144">User Logins</span></span>

<span data-ttu-id="e501c-145">外部驗證提供者 （例如 Facebook 或 Microsoft 帳戶） 的相關資訊時登入使用者使用。</span><span class="sxs-lookup"><span data-stu-id="e501c-145">Information about the external authentication provider (like Facebook or a Microsoft account) to use when logging in a user.</span></span> [<span data-ttu-id="e501c-146">範例</span><span class="sxs-lookup"><span data-stu-id="e501c-146">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a><span data-ttu-id="e501c-147">角色</span><span class="sxs-lookup"><span data-stu-id="e501c-147">Roles</span></span>

<span data-ttu-id="e501c-148">您的網站的授權群組。</span><span class="sxs-lookup"><span data-stu-id="e501c-148">Authorization groups for your site.</span></span> <span data-ttu-id="e501c-149">包含的角色識別碼和角色名稱 （例如"Admin"或"Employee"）。</span><span class="sxs-lookup"><span data-stu-id="e501c-149">Includes the role Id and role name (like "Admin" or "Employee").</span></span> [<span data-ttu-id="e501c-150">範例</span><span class="sxs-lookup"><span data-stu-id="e501c-150">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a><span data-ttu-id="e501c-151">資料存取層</span><span class="sxs-lookup"><span data-stu-id="e501c-151">The data access layer</span></span>

<span data-ttu-id="e501c-152">本主題假設您熟悉您要使用的持續性機制，以及如何建立實體，該機制。</span><span class="sxs-lookup"><span data-stu-id="e501c-152">This topic assumes you are familiar with the persistence mechanism that you are going to use and how to create entities for that mechanism.</span></span> <span data-ttu-id="e501c-153">本主題不提供如何建立儲存機制或資料存取類別; 詳細資料使用 ASP.NET Core 識別時，它會提供有關設計決策的一些建議。</span><span class="sxs-lookup"><span data-stu-id="e501c-153">This topic does not provide details about how to create the repositories or data access classes; it provides some suggestions about design decisions when working with ASP.NET Core Identity.</span></span>

<span data-ttu-id="e501c-154">設計自訂存放區提供者的資料存取層時，您會有很多的自由。</span><span class="sxs-lookup"><span data-stu-id="e501c-154">You have a lot of freedom when designing the data access layer for a customized store provider.</span></span> <span data-ttu-id="e501c-155">您只需要建立持續性機制，可用於您想要使用您的應用程式中的功能。</span><span class="sxs-lookup"><span data-stu-id="e501c-155">You only need to create persistence mechanisms for features that you intend to use in your app.</span></span> <span data-ttu-id="e501c-156">例如，如果您不會在您的應用程式中使用角色，您不需要建立的角色或使用者角色關聯的儲存體。</span><span class="sxs-lookup"><span data-stu-id="e501c-156">For example, if you are not using roles in your app, you do not need to create storage for roles or user role associations.</span></span> <span data-ttu-id="e501c-157">您的技術和現有的基礎結構，可能需要與 ASP.NET Core 身分識別的預設實作非常不同的結構。</span><span class="sxs-lookup"><span data-stu-id="e501c-157">Your technology and existing infrastructure may require a structure that is very different from the default implementation of ASP.NET Core Identity.</span></span> <span data-ttu-id="e501c-158">資料存取層，您提供的邏輯來處理您的儲存體實作的結構。</span><span class="sxs-lookup"><span data-stu-id="e501c-158">In your data access layer, you provide the logic to work with the structure of your storage implementation.</span></span>

<span data-ttu-id="e501c-159">資料存取層提供邏輯以從 ASP.NET Core 身分識別的資料儲存至資料來源。</span><span class="sxs-lookup"><span data-stu-id="e501c-159">The data access layer provides the logic to save the data from ASP.NET Core Identity to a data source.</span></span> <span data-ttu-id="e501c-160">資料存取層，您自訂的存放裝置提供者可能會包含下列類別，以儲存使用者和角色的資訊。</span><span class="sxs-lookup"><span data-stu-id="e501c-160">The data access layer for your customized storage provider might include the following classes to store user and role information.</span></span>

### <a name="context-class"></a><span data-ttu-id="e501c-161">Context 類別</span><span class="sxs-lookup"><span data-stu-id="e501c-161">Context class</span></span>

<span data-ttu-id="e501c-162">封裝連接到您的持續性機制，並執行查詢的資訊。</span><span class="sxs-lookup"><span data-stu-id="e501c-162">Encapsulates the information to connect to your persistence mechanism and execute queries.</span></span> <span data-ttu-id="e501c-163">數個資料類別需要這個類別，通常透過相依性插入所提供的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e501c-163">Several data classes require an instance of this class, typically provided through dependency injection.</span></span> <span data-ttu-id="e501c-164">[範例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)。</span><span class="sxs-lookup"><span data-stu-id="e501c-164">[Example](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).</span></span>

### <a name="user-storage"></a><span data-ttu-id="e501c-165">使用者的儲存體</span><span class="sxs-lookup"><span data-stu-id="e501c-165">User Storage</span></span>

<span data-ttu-id="e501c-166">儲存和擷取使用者資訊 （例如使用者名稱和密碼雜湊）。</span><span class="sxs-lookup"><span data-stu-id="e501c-166">Stores and retrieves user information (such as user name and password hash).</span></span> [<span data-ttu-id="e501c-167">範例</span><span class="sxs-lookup"><span data-stu-id="e501c-167">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a><span data-ttu-id="e501c-168">角色存放裝置</span><span class="sxs-lookup"><span data-stu-id="e501c-168">Role Storage</span></span>

<span data-ttu-id="e501c-169">儲存和擷取角色資訊 （例如角色名稱）。</span><span class="sxs-lookup"><span data-stu-id="e501c-169">Stores and retrieves role information (such as the role name).</span></span> [<span data-ttu-id="e501c-170">範例</span><span class="sxs-lookup"><span data-stu-id="e501c-170">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a><span data-ttu-id="e501c-171">UserClaims 的儲存體</span><span class="sxs-lookup"><span data-stu-id="e501c-171">UserClaims Storage</span></span>

<span data-ttu-id="e501c-172">儲存和擷取使用者宣告資訊 （例如宣告類型和值）。</span><span class="sxs-lookup"><span data-stu-id="e501c-172">Stores and retrieves user claim information (such as the claim type and value).</span></span> [<span data-ttu-id="e501c-173">範例</span><span class="sxs-lookup"><span data-stu-id="e501c-173">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a><span data-ttu-id="e501c-174">Serlogins 的儲存體</span><span class="sxs-lookup"><span data-stu-id="e501c-174">UserLogins Storage</span></span>

<span data-ttu-id="e501c-175">儲存和擷取使用者登入資訊 （例如外部驗證提供者）。</span><span class="sxs-lookup"><span data-stu-id="e501c-175">Stores and retrieves user login information (such as an external authentication provider).</span></span> [<span data-ttu-id="e501c-176">範例</span><span class="sxs-lookup"><span data-stu-id="e501c-176">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a><span data-ttu-id="e501c-177">使用者角色存放裝置</span><span class="sxs-lookup"><span data-stu-id="e501c-177">UserRole Storage</span></span>

<span data-ttu-id="e501c-178">儲存和擷取哪些角色指派給使用者。</span><span class="sxs-lookup"><span data-stu-id="e501c-178">Stores and retrieves which roles are assigned to which users.</span></span> [<span data-ttu-id="e501c-179">範例</span><span class="sxs-lookup"><span data-stu-id="e501c-179">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

<span data-ttu-id="e501c-180">**提示：**只實作您想要使用您的應用程式中的類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-180">**TIP:** Only implement the classes you intend to use in your app.</span></span>

<span data-ttu-id="e501c-181">在資料存取類別中，提供程式碼以執行資料作業的持續性機制。</span><span class="sxs-lookup"><span data-stu-id="e501c-181">In the data access classes, provide code to perform data operations for your persistence mechanism.</span></span> <span data-ttu-id="e501c-182">比方說，在自訂的提供者，您可能必須建立新的使用者，在下列程式碼*儲存*類別：</span><span class="sxs-lookup"><span data-stu-id="e501c-182">For example, within a custom provider, you might have the following code to create a new user in the *store* class:</span></span>

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

<span data-ttu-id="e501c-183">建立使用者的實作邏輯處於``_usersTable.CreateAsync``方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e501c-183">The implementation logic for creating the user is in the ``_usersTable.CreateAsync`` method, shown below.</span></span>

## <a name="customize-the-user-class"></a><span data-ttu-id="e501c-184">自訂使用者類別</span><span class="sxs-lookup"><span data-stu-id="e501c-184">Customize the user class</span></span>

<span data-ttu-id="e501c-185">當實作的儲存提供者，建立使用者類別，其相當於[`IdentityUser`類別](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)。</span><span class="sxs-lookup"><span data-stu-id="e501c-185">When implementing a storage provider, create a user class which is equivalent to the [`IdentityUser` class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).</span></span>

<span data-ttu-id="e501c-186">至少必須包含在使用者類別`Id`和`UserName`屬性。</span><span class="sxs-lookup"><span data-stu-id="e501c-186">At a minimum, your user class must include an `Id` and a `UserName` property.</span></span>

<span data-ttu-id="e501c-187">`IdentityUser`類別定義的內容，``UserManager``呼叫時執行要求的作業。</span><span class="sxs-lookup"><span data-stu-id="e501c-187">The `IdentityUser` class defines the properties that the ``UserManager`` calls when performing requested operations.</span></span> <span data-ttu-id="e501c-188">預設類型`Id`屬性是一個字串，但您可以繼承自`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`並指定不同的類型。</span><span class="sxs-lookup"><span data-stu-id="e501c-188">The default type of the `Id` property is a string, but you can inherit from `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` and specify a different type.</span></span> <span data-ttu-id="e501c-189">架構會預期要處理的資料類型轉換的儲存體實作。</span><span class="sxs-lookup"><span data-stu-id="e501c-189">The framework expects the storage implementation to handle data type conversions.</span></span>

## <a name="customize-the-user-store"></a><span data-ttu-id="e501c-190">自訂使用者存放區</span><span class="sxs-lookup"><span data-stu-id="e501c-190">Customize the user store</span></span>

<span data-ttu-id="e501c-191">建立`UserStore`提供的所有使用者資料作業方式的類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-191">Create a `UserStore` class that provides the methods for all data operations on the user.</span></span> <span data-ttu-id="e501c-192">這個類別就相當於[UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-192">This class is equivalent to the [UserStore<TUser>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) class.</span></span> <span data-ttu-id="e501c-193">在您`UserStore`類別，實作`IUserStore<TUser>`和所需的選用介面。</span><span class="sxs-lookup"><span data-stu-id="e501c-193">In your `UserStore` class, implement `IUserStore<TUser>` and the optional interfaces required.</span></span> <span data-ttu-id="e501c-194">您選取要實作的選擇性介面上根據您的應用程式中提供的功能。</span><span class="sxs-lookup"><span data-stu-id="e501c-194">You select which optional interfaces to implement based on on the functionality provided in your app.</span></span>

### <a name="optional-interfaces"></a><span data-ttu-id="e501c-195">選擇性介面</span><span class="sxs-lookup"><span data-stu-id="e501c-195">Optional interfaces</span></span>

- <span data-ttu-id="e501c-196">IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1</span><span class="sxs-lookup"><span data-stu-id="e501c-196">IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1</span></span>
- <span data-ttu-id="e501c-197">IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span><span class="sxs-lookup"><span data-stu-id="e501c-197">IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span></span>
- <span data-ttu-id="e501c-198">IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span><span class="sxs-lookup"><span data-stu-id="e501c-198">IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span></span>
- <span data-ttu-id="e501c-199">IUserSecurityStampStore<!-- make these all links and remove / --></span><span class="sxs-lookup"><span data-stu-id="e501c-199">IUserSecurityStampStore <!-- make these all links and remove / --></span></span>
- <span data-ttu-id="e501c-200">IUserEmailStore</span><span class="sxs-lookup"><span data-stu-id="e501c-200">IUserEmailStore</span></span>
- <span data-ttu-id="e501c-201">IPhoneNumberStore</span><span class="sxs-lookup"><span data-stu-id="e501c-201">IPhoneNumberStore</span></span>
- <span data-ttu-id="e501c-202">IQueryableUserStore</span><span class="sxs-lookup"><span data-stu-id="e501c-202">IQueryableUserStore</span></span>
- <span data-ttu-id="e501c-203">IUserLoginStore</span><span class="sxs-lookup"><span data-stu-id="e501c-203">IUserLoginStore</span></span>
- <span data-ttu-id="e501c-204">IUserTwoFactorStore</span><span class="sxs-lookup"><span data-stu-id="e501c-204">IUserTwoFactorStore</span></span>
- <span data-ttu-id="e501c-205">IUserLockoutStore</span><span class="sxs-lookup"><span data-stu-id="e501c-205">IUserLockoutStore</span></span>

<span data-ttu-id="e501c-206">選擇性的介面繼承自`IUserStore`。</span><span class="sxs-lookup"><span data-stu-id="e501c-206">The optional interfaces inherit from `IUserStore`.</span></span> <span data-ttu-id="e501c-207">您可以查看儲存部分實作的範例使用者[這裡](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)。</span><span class="sxs-lookup"><span data-stu-id="e501c-207">You can see a partially implemented sample user store [here](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).</span></span>

<span data-ttu-id="e501c-208">內`UserStore`類別，使用您建立用來執行作業的資料存取類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-208">Within the `UserStore` class, you use the data access classes that you created to perform operations.</span></span> <span data-ttu-id="e501c-209">這些會在使用相依性插入傳遞。</span><span class="sxs-lookup"><span data-stu-id="e501c-209">These are passed in using dependency injection.</span></span> <span data-ttu-id="e501c-210">例如，在 SQL Server Dapper 實作，`UserStore`類別具有`CreateAsync`方法使用的執行個體`DapperUsersTable`來插入新的記錄：</span><span class="sxs-lookup"><span data-stu-id="e501c-210">For example, in the SQL Server with Dapper implementation, the `UserStore` class has the `CreateAsync` method which uses an instance of `DapperUsersTable` to insert a new record:</span></span>

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a><span data-ttu-id="e501c-211">若要實作自訂使用者存放區時的介面</span><span class="sxs-lookup"><span data-stu-id="e501c-211">Interfaces to implement when customizing user store</span></span>

- <span data-ttu-id="e501c-212">**IUserStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-212">**IUserStore**</span></span>  
 <span data-ttu-id="e501c-213">[IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1)介面是只有在使用者存放區中，您必須實作的介面。</span><span class="sxs-lookup"><span data-stu-id="e501c-213">The [IUserStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1) interface is the only interface you must implement in the user store.</span></span> <span data-ttu-id="e501c-214">它會定義方法來建立、 更新、 刪除和擷取使用者。</span><span class="sxs-lookup"><span data-stu-id="e501c-214">It defines methods for creating, updating, deleting, and retrieving users.</span></span>
- <span data-ttu-id="e501c-215">**IUserClaimStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-215">**IUserClaimStore**</span></span>  
 <span data-ttu-id="e501c-216">[IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1)介面會定義您要啟用使用者宣告實作的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-216">The [IUserClaimStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface defines the methods you implement to enable user claims.</span></span> <span data-ttu-id="e501c-217">它包含用於加入、 移除和擷取使用者宣告的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-217">It contains methods for adding, removing and retrieving user claims.</span></span>
- <span data-ttu-id="e501c-218">**IUserLoginStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-218">**IUserLoginStore**</span></span>  
 <span data-ttu-id="e501c-219">[IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1)定義您要啟用外部驗證提供者實作的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-219">The [IUserLoginStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1) defines the methods you implement to enable external authentication providers.</span></span> <span data-ttu-id="e501c-220">它包含加入、 移除和擷取使用者登入和擷取使用者為基礎的登入資訊方法的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-220">It contains methods for adding, removing and retrieving user logins, and a method for retrieving a user based on the login information.</span></span>
- <span data-ttu-id="e501c-221">**IUserRoleStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-221">**IUserRoleStore**</span></span>  
 <span data-ttu-id="e501c-222">[IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1)介面會定義您將使用者對應至角色實作的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-222">The [IUserRoleStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface defines the methods you implement to map a user to a role.</span></span> <span data-ttu-id="e501c-223">它包含方法，以新增、 移除和擷取使用者的角色，並檢查使用者是否指派給角色的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-223">It contains methods to add, remove, and retrieve a user's roles, and a method to check if a user is assigned to a role.</span></span>
- <span data-ttu-id="e501c-224">**IUserPasswordStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-224">**IUserPasswordStore**</span></span>  
 <span data-ttu-id="e501c-225">[IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)介面會定義您實作來保存雜湊的密碼的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-225">The [IUserPasswordStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface defines the methods you implement to persist hashed passwords.</span></span> <span data-ttu-id="e501c-226">它包含方法來取得和設定雜湊的密碼，並指出使用者是否已設定密碼的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-226">It contains methods for getting and setting the hashed password, and a method that indicates whether the user has set a password.</span></span>
- <span data-ttu-id="e501c-227">**IUserSecurityStampStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-227">**IUserSecurityStampStore**</span></span>  
 <span data-ttu-id="e501c-228">[IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)介面會定義您實作要用於安全性戳記，指出是否已變更的使用者帳戶資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-228">The [IUserSecurityStampStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface defines the methods you implement to use a security stamp for indicating whether the user's account information has changed.</span></span> <span data-ttu-id="e501c-229">當使用者變更密碼，或加入或移除登入，則會更新這個戳記。</span><span class="sxs-lookup"><span data-stu-id="e501c-229">This stamp is updated when a user changes the password, or adds or removes logins.</span></span> <span data-ttu-id="e501c-230">它包含方法來取得和設定的安全性戳記。</span><span class="sxs-lookup"><span data-stu-id="e501c-230">It contains methods for getting and setting the security stamp.</span></span>
- <span data-ttu-id="e501c-231">**IUserTwoFactorStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-231">**IUserTwoFactorStore**</span></span>  
 <span data-ttu-id="e501c-232">[IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)介面會定義您實作以支援雙因素驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-232">The [IUserTwoFactorStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface defines the methods you implement to support two factor authentication.</span></span> <span data-ttu-id="e501c-233">它包含對取得和設定是否針對使用者啟用雙因素驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-233">It contains methods for getting and setting whether two factor authentication is enabled for a user.</span></span>
- <span data-ttu-id="e501c-234">**IUserPhoneNumberStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-234">**IUserPhoneNumberStore**</span></span>  
 <span data-ttu-id="e501c-235">[IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)介面會定義您實作以儲存使用者電話號碼的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-235">The [IUserPhoneNumberStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface defines the methods you implement to store user phone numbers.</span></span> <span data-ttu-id="e501c-236">它包含對取得和設定的電話號碼和電話號碼是否已確認的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-236">It contains methods for getting and setting the phone number and whether the phone number is confirmed.</span></span>
- <span data-ttu-id="e501c-237">**IUserEmailStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-237">**IUserEmailStore**</span></span>  
 <span data-ttu-id="e501c-238">[IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1)介面會定義您實作以儲存使用者的電子郵件地址的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-238">The [IUserEmailStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface defines the methods you implement to store user email addresses.</span></span> <span data-ttu-id="e501c-239">它包含對取得和設定電子郵件地址和電子郵件是否已確認的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-239">It contains methods for getting and setting the email address and whether the email is confirmed.</span></span>
- <span data-ttu-id="e501c-240">**IUserLockoutStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-240">**IUserLockoutStore**</span></span>  
 <span data-ttu-id="e501c-241">[IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)介面會定義您實作以儲存有關鎖定的帳戶資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-241">The [IUserLockoutStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface defines the methods you implement to store information about locking an account.</span></span> <span data-ttu-id="e501c-242">它包含追蹤失敗的存取嘗試以及鎖定的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-242">It contains methods for tracking failed access attempts and lockouts.</span></span>
- <span data-ttu-id="e501c-243">**IQueryableUserStore**</span><span class="sxs-lookup"><span data-stu-id="e501c-243">**IQueryableUserStore**</span></span>  
 <span data-ttu-id="e501c-244">[IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)介面會定義提供可查詢使用者存放區的成員實作。</span><span class="sxs-lookup"><span data-stu-id="e501c-244">The [IQueryableUserStore&lt;TUser&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface defines the members implement to provide a queryable user store.</span></span>

<span data-ttu-id="e501c-245">您在應用程式中實作所需的介面。</span><span class="sxs-lookup"><span data-stu-id="e501c-245">You implement only the interfaces that are needed in your app.</span></span> <span data-ttu-id="e501c-246">例如: </span><span class="sxs-lookup"><span data-stu-id="e501c-246">For example:</span></span>

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a><span data-ttu-id="e501c-247">IdentityUserClaim、 IdentityUserLogin 和 IdentityUserRole</span><span class="sxs-lookup"><span data-stu-id="e501c-247">IdentityUserClaim, IdentityUserLogin, and IdentityUserRole</span></span>

<span data-ttu-id="e501c-248">``Microsoft.AspNet.Identity.EntityFramework``命名空間包含的實作[IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)， [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)，和[IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-248">The ``Microsoft.AspNet.Identity.EntityFramework`` namespace contains implementations of the [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), and [IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes.</span></span> <span data-ttu-id="e501c-249">如果您使用這些功能，您可能想要建立您自己的版本，這些類別，以及定義您的應用程式的屬性。</span><span class="sxs-lookup"><span data-stu-id="e501c-249">If you are using these features, you may want to create your own versions of these classes and define the properties for your app.</span></span> <span data-ttu-id="e501c-250">不過，有時候很不載入這些實體記憶體時執行 （例如加入或移除使用者的宣告） 的基本作業更有效率。</span><span class="sxs-lookup"><span data-stu-id="e501c-250">However, sometimes it is more efficient to not load these entities into memory when performing basic operations (such as adding or removing a user's claim).</span></span> <span data-ttu-id="e501c-251">請改為後端存放區類別可以執行這些作業，直接針對資料來源。</span><span class="sxs-lookup"><span data-stu-id="e501c-251">Instead, the backend store classes can execute these operations directly on the data source.</span></span> <span data-ttu-id="e501c-252">例如，``UserStore.GetClaimsAsync``方法可以呼叫``userClaimTable.FindByUserId(user.Id)``方法上執行查詢，直接資料表，並傳回清單的宣告。</span><span class="sxs-lookup"><span data-stu-id="e501c-252">For example, the ``UserStore.GetClaimsAsync`` method can call the ``userClaimTable.FindByUserId(user.Id)`` method to execute a query on that table directly and return a list of claims.</span></span>

## <a name="customize-the-role-class"></a><span data-ttu-id="e501c-253">自訂角色類別</span><span class="sxs-lookup"><span data-stu-id="e501c-253">Customize the role class</span></span>

<span data-ttu-id="e501c-254">當實作角色存放裝置提供者，您可以建立自訂角色型別。</span><span class="sxs-lookup"><span data-stu-id="e501c-254">When implementing a role storage provider, you can create a custom role type.</span></span> <span data-ttu-id="e501c-255">它不需要實作特定介面，但是它必須有`Id`而且通常會有`Name`屬性。</span><span class="sxs-lookup"><span data-stu-id="e501c-255">It need not implement a particular interface, but it must have an `Id` and typically it will have a `Name` property.</span></span>

<span data-ttu-id="e501c-256">以下是範例角色類別：</span><span class="sxs-lookup"><span data-stu-id="e501c-256">The following is an example role class:</span></span>

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a><span data-ttu-id="e501c-257">自訂角色存放區</span><span class="sxs-lookup"><span data-stu-id="e501c-257">Customize the role store</span></span>

<span data-ttu-id="e501c-258">您可以建立``RoleStore``提供的角色上的所有資料作業方式的類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-258">You can create a ``RoleStore`` class that provides the methods for all data operations on roles.</span></span> <span data-ttu-id="e501c-259">這個類別就相當於[RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-259">This class is equivalent to the [RoleStore<TRole>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) class.</span></span> <span data-ttu-id="e501c-260">在`RoleStore`類別時，實作``IRoleStore<TRole>``並選擇性地``IQueryableRoleStore<TRole>``介面。</span><span class="sxs-lookup"><span data-stu-id="e501c-260">In the `RoleStore` class, you implement the ``IRoleStore<TRole>`` and optionally the ``IQueryableRoleStore<TRole>`` interface.</span></span>

- <span data-ttu-id="e501c-261">**IRoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="e501c-261">**IRoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="e501c-262">[IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1)介面會定義角色存放區類別中實作的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-262">The [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1) interface defines the methods to implement in the role store class.</span></span> <span data-ttu-id="e501c-263">它包含建立、 更新、 刪除及擷取角色的方法。</span><span class="sxs-lookup"><span data-stu-id="e501c-263">It contains methods for creating, updating, deleting and retrieving roles.</span></span>
- <span data-ttu-id="e501c-264">**RoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="e501c-264">**RoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="e501c-265">若要自訂`RoleStore`，建立類別，實作`IRoleStore`介面。</span><span class="sxs-lookup"><span data-stu-id="e501c-265">To customize `RoleStore`, create a class that implements the `IRoleStore` interface.</span></span> 

## <a name="reconfigure-app-to-use-new-storage-provider"></a><span data-ttu-id="e501c-266">重新設定應用程式使用新的存放裝置提供者</span><span class="sxs-lookup"><span data-stu-id="e501c-266">Reconfigure app to use new storage provider</span></span>

<span data-ttu-id="e501c-267">一旦您已實作的儲存提供者，您會設定您的應用程式使用它。</span><span class="sxs-lookup"><span data-stu-id="e501c-267">Once you have implemented a storage provider, you configure your app to use it.</span></span> <span data-ttu-id="e501c-268">如果您的應用程式使用預設提供者，請將它取代您的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="e501c-268">If your app used the default provider, replace it with your custom provider.</span></span>

1. <span data-ttu-id="e501c-269">移除`Microsoft.AspNetCore.EntityFramework.Identity`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="e501c-269">Remove the `Microsoft.AspNetCore.EntityFramework.Identity` NuGet package.</span></span>
1. <span data-ttu-id="e501c-270">如果存放裝置提供者位於不同的專案或封裝中，加入的參考。</span><span class="sxs-lookup"><span data-stu-id="e501c-270">If the storage provider resides in a separate project or package, add a reference to it.</span></span>
1. <span data-ttu-id="e501c-271">取代所有參考`Microsoft.AspNetCore.EntityFramework.Identity`使用存放裝置提供者的命名空間陳述式。</span><span class="sxs-lookup"><span data-stu-id="e501c-271">Replace all references to `Microsoft.AspNetCore.EntityFramework.Identity` with a using statement for the namespace of your storage provider.</span></span>
1. <span data-ttu-id="e501c-272">在``ConfigureServices``方法，變更`AddIdentity`方法，以使用您的自訂型別。</span><span class="sxs-lookup"><span data-stu-id="e501c-272">In the ``ConfigureServices`` method, change the `AddIdentity` method to use your custom types.</span></span> <span data-ttu-id="e501c-273">您可以建立您自己的擴充方法，針對此目的。</span><span class="sxs-lookup"><span data-stu-id="e501c-273">You can create your own extension methods for this purpose.</span></span> <span data-ttu-id="e501c-274">請參閱[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)的範例。</span><span class="sxs-lookup"><span data-stu-id="e501c-274">See [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) for an example.</span></span>
1. <span data-ttu-id="e501c-275">如果您使用角色時，更新`RoleManager`使用您`RoleStore`類別。</span><span class="sxs-lookup"><span data-stu-id="e501c-275">If you are using Roles, update the `RoleManager` to use your `RoleStore` class.</span></span>
1. <span data-ttu-id="e501c-276">更新您的應用程式設定的連接字串和認證。</span><span class="sxs-lookup"><span data-stu-id="e501c-276">Update the connection string and credentials to your app's configuration.</span></span>

<span data-ttu-id="e501c-277">範例：</span><span class="sxs-lookup"><span data-stu-id="e501c-277">Example:</span></span>

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

## <a name="references"></a><span data-ttu-id="e501c-278">參考</span><span class="sxs-lookup"><span data-stu-id="e501c-278">References</span></span>

- [<span data-ttu-id="e501c-279">ASP.NET 識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="e501c-279">Custom Storage Providers for ASP.NET Identity</span></span>](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- <span data-ttu-id="e501c-280">[ASP.NET Core 識別](https://github.com/aspnet/identity)-這個儲存機制包含社群維護存放區提供者的連結。</span><span class="sxs-lookup"><span data-stu-id="e501c-280">[ASP.NET Core Identity](https://github.com/aspnet/identity) - This repository includes links to community maintained store providers.</span></span>
