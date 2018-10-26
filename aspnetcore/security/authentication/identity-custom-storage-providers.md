---
title: ASP.NET Core 身分識別的自訂儲存體提供者
author: ardalis
description: 了解如何設定 ASP.NET Core 身分識別的自訂儲存體提供者。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: b10731261ca0c748548fcba94a229ba055d46eb5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090832"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a><span data-ttu-id="2518d-103">ASP.NET Core 身分識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="2518d-103">Custom storage providers for ASP.NET Core Identity</span></span>

<span data-ttu-id="2518d-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2518d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2518d-105">ASP.NET Core Identity 是可延伸的系統，可讓您建立自訂的儲存體提供者，並將它連接到您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2518d-105">ASP.NET Core Identity is an extensible system which enables you to create a custom storage provider and connect it to your app.</span></span> <span data-ttu-id="2518d-106">本主題描述如何建立 ASP.NET Core 身分識別的自訂儲存體提供者。</span><span class="sxs-lookup"><span data-stu-id="2518d-106">This topic describes how to create a customized storage provider for ASP.NET Core Identity.</span></span> <span data-ttu-id="2518d-107">它涵蓋建立自己的儲存體提供者的重要概念，但不需逐步解說。</span><span class="sxs-lookup"><span data-stu-id="2518d-107">It covers the important concepts for creating your own storage provider, but isn't a step-by-step walkthrough.</span></span>

<span data-ttu-id="2518d-108">[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)。</span><span class="sxs-lookup"><span data-stu-id="2518d-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).</span></span>

## <a name="introduction"></a><span data-ttu-id="2518d-109">簡介</span><span class="sxs-lookup"><span data-stu-id="2518d-109">Introduction</span></span>

<span data-ttu-id="2518d-110">根據預設，ASP.NET Core 身分識別系統會將使用者資訊儲存在 SQL Server 資料庫中使用 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="2518d-110">By default, the ASP.NET Core Identity system stores user information in a SQL Server database using Entity Framework Core.</span></span> <span data-ttu-id="2518d-111">對於許多應用程式，這種方法非常適合。</span><span class="sxs-lookup"><span data-stu-id="2518d-111">For many apps, this approach works well.</span></span> <span data-ttu-id="2518d-112">不過，您也可能會想要使用不同的持續性機制或資料結構描述。</span><span class="sxs-lookup"><span data-stu-id="2518d-112">However, you may prefer to use a different persistence mechanism or data schema.</span></span> <span data-ttu-id="2518d-113">例如: </span><span class="sxs-lookup"><span data-stu-id="2518d-113">For example:</span></span>

* <span data-ttu-id="2518d-114">您使用[Azure 表格儲存體](/azure/storage/)或其他資料存放區。</span><span class="sxs-lookup"><span data-stu-id="2518d-114">You use [Azure Table Storage](/azure/storage/) or another data store.</span></span>
* <span data-ttu-id="2518d-115">資料庫資料表有不同的結構。</span><span class="sxs-lookup"><span data-stu-id="2518d-115">Your database tables have a different structure.</span></span> 
* <span data-ttu-id="2518d-116">您可能想要使用不同的資料存取方法，例如[Dapper](https://github.com/StackExchange/Dapper)。</span><span class="sxs-lookup"><span data-stu-id="2518d-116">You may wish to use a different data access approach, such as [Dapper](https://github.com/StackExchange/Dapper).</span></span> 

<span data-ttu-id="2518d-117">在這些情況下，您可以為您的儲存機制中撰寫自訂提供者，並插入您的應用程式中的該提供者。</span><span class="sxs-lookup"><span data-stu-id="2518d-117">In each of these cases, you can write a customized provider for your storage mechanism and plug that provider into your app.</span></span>

<span data-ttu-id="2518d-118">ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。</span><span class="sxs-lookup"><span data-stu-id="2518d-118">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="2518d-119">當使用.NET Core CLI，新增`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="2518d-119">When using the .NET Core CLI, add `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a><span data-ttu-id="2518d-120">ASP.NET Core 身分識別架構</span><span class="sxs-lookup"><span data-stu-id="2518d-120">The ASP.NET Core Identity architecture</span></span>

<span data-ttu-id="2518d-121">ASP.NET Core Identity 是由名為管理員和存放區的類別所組成。</span><span class="sxs-lookup"><span data-stu-id="2518d-121">ASP.NET Core Identity consists of classes called managers and stores.</span></span> <span data-ttu-id="2518d-122">*管理員*一些高階的類別，其應用程式開發人員用來執行作業，例如建立身分識別使用者。</span><span class="sxs-lookup"><span data-stu-id="2518d-122">*Managers* are high-level classes which an app developer uses to perform operations, such as creating an Identity user.</span></span> <span data-ttu-id="2518d-123">*存放區*是較低層級的類別，指定如何保存實體，例如使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="2518d-123">*Stores* are lower-level classes that specify how entities, such as users and roles, are persisted.</span></span> <span data-ttu-id="2518d-124">存放區會遵循儲存機制模式，並會緊密結合的持續性機制。</span><span class="sxs-lookup"><span data-stu-id="2518d-124">Stores follow the repository pattern and are closely coupled with the persistence mechanism.</span></span> <span data-ttu-id="2518d-125">管理員會分離從存放區，這表示您可以將持續性機制，而不需要變更您的應用程式程式碼 （除了組態）。</span><span class="sxs-lookup"><span data-stu-id="2518d-125">Managers are decoupled from stores, which means you can replace the persistence mechanism without changing your application code (except for configuration).</span></span>

<span data-ttu-id="2518d-126">下圖顯示如何將 web 應用程式與互動管理員，而與資料存取層的存放區互動。</span><span class="sxs-lookup"><span data-stu-id="2518d-126">The following diagram shows how a web app interacts with the managers, while stores interact with the data access layer.</span></span>

![ASP.NET Core 應用程式使用 （例如，'UserManager'、 'RoleManager'） 的管理員。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

<span data-ttu-id="2518d-129">若要建立自訂的儲存體提供者，建立資料來源、 資料存取層級和互動使用的資料存取層級 （在上圖中綠灰色方塊） 的存放區類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-129">To create a custom storage provider, create the data source, the data access layer, and the store classes that interact with this data access layer (the green and grey boxes in the diagram above).</span></span> <span data-ttu-id="2518d-130">您不需要自訂的經理 」 或 「 應用程式程式碼與它們 （上述的藍色方塊） 互動。</span><span class="sxs-lookup"><span data-stu-id="2518d-130">You don't need to customize the managers or your app code that interacts with them (the blue boxes above).</span></span>

<span data-ttu-id="2518d-131">建立的新執行個體時`UserManager`或`RoleManager`您提供使用者類別的型別，並傳遞做為引數的儲存區類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="2518d-131">When creating a new instance of `UserManager` or `RoleManager` you provide the type of the user class and pass an instance of the store class as an argument.</span></span> <span data-ttu-id="2518d-132">這種方法可讓您插入 ASP.NET Core 中的自訂的類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-132">This approach enables you to plug your customized classes into ASP.NET Core.</span></span> 

<span data-ttu-id="2518d-133">[重新設定應用程式以使用新的儲存體提供者](#reconfigure-app-to-use-a-new-storage-provider)示範如何具現化`UserManager`和`RoleManager`與自訂存放區。</span><span class="sxs-lookup"><span data-stu-id="2518d-133">[Reconfigure app to use new storage provider](#reconfigure-app-to-use-a-new-storage-provider) shows how to instantiate `UserManager` and `RoleManager` with a customized store.</span></span>

## <a name="aspnet-core-identity-stores-data-types"></a><span data-ttu-id="2518d-134">ASP.NET Core 身分識別儲存的資料類型</span><span class="sxs-lookup"><span data-stu-id="2518d-134">ASP.NET Core Identity stores data types</span></span>

<span data-ttu-id="2518d-135">[ASP.NET Core Identity](https://github.com/aspnet/identity)資料類型詳述於下列各節：</span><span class="sxs-lookup"><span data-stu-id="2518d-135">[ASP.NET Core Identity](https://github.com/aspnet/identity) data types are detailed in the following sections:</span></span>

### <a name="users"></a><span data-ttu-id="2518d-136">使用者</span><span class="sxs-lookup"><span data-stu-id="2518d-136">Users</span></span>

<span data-ttu-id="2518d-137">註冊您的網站的使用者。</span><span class="sxs-lookup"><span data-stu-id="2518d-137">Registered users of your web site.</span></span> <span data-ttu-id="2518d-138">[IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser)型別可能會擴充，或作為您自己的自訂類型的範例。</span><span class="sxs-lookup"><span data-stu-id="2518d-138">The [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) type may be extended or used as an example for your own custom type.</span></span> <span data-ttu-id="2518d-139">您不需要繼承特定的型別，以實作您自己的自訂身分識別儲存體解決方案。</span><span class="sxs-lookup"><span data-stu-id="2518d-139">You don't need to inherit from a particular type to implement your own custom identity storage solution.</span></span>

### <a name="user-claims"></a><span data-ttu-id="2518d-140">使用者宣告</span><span class="sxs-lookup"><span data-stu-id="2518d-140">User Claims</span></span>

<span data-ttu-id="2518d-141">一組陳述式 (或[宣告](/dotnet/api/system.security.claims.claim)) 代表使用者的身分識別的使用者相關。</span><span class="sxs-lookup"><span data-stu-id="2518d-141">A set of statements (or [Claims](/dotnet/api/system.security.claims.claim)) about the user that represent the user's identity.</span></span> <span data-ttu-id="2518d-142">可以啟用更高運算式的非可達成透過角色的使用者身分識別。</span><span class="sxs-lookup"><span data-stu-id="2518d-142">Can enable greater expression of the user's identity than can be achieved through roles.</span></span>

### <a name="user-logins"></a><span data-ttu-id="2518d-143">使用者登入</span><span class="sxs-lookup"><span data-stu-id="2518d-143">User Logins</span></span>

<span data-ttu-id="2518d-144">外部驗證提供者 （例如 Facebook 或 Microsoft 帳戶） 的相關資訊登入使用者時要使用。</span><span class="sxs-lookup"><span data-stu-id="2518d-144">Information about the external authentication provider (like Facebook or a Microsoft account) to use when logging in a user.</span></span> [<span data-ttu-id="2518d-145">範例</span><span class="sxs-lookup"><span data-stu-id="2518d-145">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a><span data-ttu-id="2518d-146">角色</span><span class="sxs-lookup"><span data-stu-id="2518d-146">Roles</span></span>

<span data-ttu-id="2518d-147">您的網站的授權群組。</span><span class="sxs-lookup"><span data-stu-id="2518d-147">Authorization groups for your site.</span></span> <span data-ttu-id="2518d-148">包含角色識別碼和角色名稱 （例如"Admin"或"Employee"）。</span><span class="sxs-lookup"><span data-stu-id="2518d-148">Includes the role Id and role name (like "Admin" or "Employee").</span></span> [<span data-ttu-id="2518d-149">範例</span><span class="sxs-lookup"><span data-stu-id="2518d-149">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a><span data-ttu-id="2518d-150">資料存取層</span><span class="sxs-lookup"><span data-stu-id="2518d-150">The data access layer</span></span>

<span data-ttu-id="2518d-151">本主題假設您已熟悉您要使用的持續性機制，以及如何建立實體，該機制。</span><span class="sxs-lookup"><span data-stu-id="2518d-151">This topic assumes you are familiar with the persistence mechanism that you are going to use and how to create entities for that mechanism.</span></span> <span data-ttu-id="2518d-152">本主題不提供有關如何建立資料存取類別; 的存放庫的詳細資料使用 ASP.NET Core 身分識別時，它會提供有關設計決策的一些建議。</span><span class="sxs-lookup"><span data-stu-id="2518d-152">This topic doesn't provide details about how to create the repositories or data access classes; it provides some suggestions about design decisions when working with ASP.NET Core Identity.</span></span>

<span data-ttu-id="2518d-153">設計自訂存放區提供者的資料存取層時，您會有很多的彈性。</span><span class="sxs-lookup"><span data-stu-id="2518d-153">You have a lot of freedom when designing the data access layer for a customized store provider.</span></span> <span data-ttu-id="2518d-154">您只需要建立持續性機制，您想要使用您的應用程式中的功能。</span><span class="sxs-lookup"><span data-stu-id="2518d-154">You only need to create persistence mechanisms for features that you intend to use in your app.</span></span> <span data-ttu-id="2518d-155">比方說，如果您不會在您的應用程式中使用角色，您不需要建立的角色或使用者角色關聯的儲存體。</span><span class="sxs-lookup"><span data-stu-id="2518d-155">For example, if you are not using roles in your app, you don't need to create storage for roles or user role associations.</span></span> <span data-ttu-id="2518d-156">您的技術及現有的基礎結構可能需要與 ASP.NET Core 身分識別的預設實作非常不同的結構。</span><span class="sxs-lookup"><span data-stu-id="2518d-156">Your technology and existing infrastructure may require a structure that's very different from the default implementation of ASP.NET Core Identity.</span></span> <span data-ttu-id="2518d-157">在資料存取層，您可以提供的邏輯來處理您的儲存體實作的結構。</span><span class="sxs-lookup"><span data-stu-id="2518d-157">In your data access layer, you provide the logic to work with the structure of your storage implementation.</span></span>

<span data-ttu-id="2518d-158">資料存取層會提供將 ASP.NET Core 身分識別的資料儲存至資料來源的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2518d-158">The data access layer provides the logic to save the data from ASP.NET Core Identity to a data source.</span></span> <span data-ttu-id="2518d-159">您的自訂儲存體提供者的資料存取層可能會包含下列類別來儲存使用者和角色的資訊。</span><span class="sxs-lookup"><span data-stu-id="2518d-159">The data access layer for your customized storage provider might include the following classes to store user and role information.</span></span>

### <a name="context-class"></a><span data-ttu-id="2518d-160">Context 類別</span><span class="sxs-lookup"><span data-stu-id="2518d-160">Context class</span></span>

<span data-ttu-id="2518d-161">封裝連接到您的持續性機制，並執行查詢的資訊。</span><span class="sxs-lookup"><span data-stu-id="2518d-161">Encapsulates the information to connect to your persistence mechanism and execute queries.</span></span> <span data-ttu-id="2518d-162">數個資料類別需要這個類別，通常透過相依性插入提供的執行個體。</span><span class="sxs-lookup"><span data-stu-id="2518d-162">Several data classes require an instance of this class, typically provided through dependency injection.</span></span> <span data-ttu-id="2518d-163">[範例](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)。</span><span class="sxs-lookup"><span data-stu-id="2518d-163">[Example](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).</span></span>

### <a name="user-storage"></a><span data-ttu-id="2518d-164">使用者儲存體</span><span class="sxs-lookup"><span data-stu-id="2518d-164">User Storage</span></span>

<span data-ttu-id="2518d-165">儲存和擷取使用者資訊 （例如使用者名稱和密碼雜湊）。</span><span class="sxs-lookup"><span data-stu-id="2518d-165">Stores and retrieves user information (such as user name and password hash).</span></span> [<span data-ttu-id="2518d-166">範例</span><span class="sxs-lookup"><span data-stu-id="2518d-166">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a><span data-ttu-id="2518d-167">角色存放裝置</span><span class="sxs-lookup"><span data-stu-id="2518d-167">Role Storage</span></span>

<span data-ttu-id="2518d-168">儲存和擷取角色資訊 （例如角色名稱）。</span><span class="sxs-lookup"><span data-stu-id="2518d-168">Stores and retrieves role information (such as the role name).</span></span> [<span data-ttu-id="2518d-169">範例</span><span class="sxs-lookup"><span data-stu-id="2518d-169">Example</span></span>](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a><span data-ttu-id="2518d-170">UserClaims 的儲存體</span><span class="sxs-lookup"><span data-stu-id="2518d-170">UserClaims Storage</span></span>

<span data-ttu-id="2518d-171">儲存和擷取使用者宣告資訊 （例如宣告類型和值）。</span><span class="sxs-lookup"><span data-stu-id="2518d-171">Stores and retrieves user claim information (such as the claim type and value).</span></span> [<span data-ttu-id="2518d-172">範例</span><span class="sxs-lookup"><span data-stu-id="2518d-172">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a><span data-ttu-id="2518d-173">Serlogins 的儲存體</span><span class="sxs-lookup"><span data-stu-id="2518d-173">UserLogins Storage</span></span>

<span data-ttu-id="2518d-174">儲存和擷取使用者登入資訊 （例如外部驗證提供者）。</span><span class="sxs-lookup"><span data-stu-id="2518d-174">Stores and retrieves user login information (such as an external authentication provider).</span></span> [<span data-ttu-id="2518d-175">範例</span><span class="sxs-lookup"><span data-stu-id="2518d-175">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a><span data-ttu-id="2518d-176">使用者角色存放裝置</span><span class="sxs-lookup"><span data-stu-id="2518d-176">UserRole Storage</span></span>

<span data-ttu-id="2518d-177">會儲存及擷取哪些角色指派給哪些使用者。</span><span class="sxs-lookup"><span data-stu-id="2518d-177">Stores and retrieves which roles are assigned to which users.</span></span> [<span data-ttu-id="2518d-178">範例</span><span class="sxs-lookup"><span data-stu-id="2518d-178">Example</span></span>](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

<span data-ttu-id="2518d-179">**提示：** 只實作應用程式中想要使用的類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-179">**TIP:** Only implement the classes you intend to use in your app.</span></span>

<span data-ttu-id="2518d-180">在資料存取類別中，提供程式碼來執行資料作業的持續性機制。</span><span class="sxs-lookup"><span data-stu-id="2518d-180">In the data access classes, provide code to perform data operations for your persistence mechanism.</span></span> <span data-ttu-id="2518d-181">比方說，在自訂提供者，您可能必須建立新的使用者，在下列程式碼*儲存*類別：</span><span class="sxs-lookup"><span data-stu-id="2518d-181">For example, within a custom provider, you might have the following code to create a new user in the *store* class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

<span data-ttu-id="2518d-182">建立使用者的實作邏輯是在`_usersTable.CreateAsync`方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2518d-182">The implementation logic for creating the user is in the `_usersTable.CreateAsync` method, shown below.</span></span>

## <a name="customize-the-user-class"></a><span data-ttu-id="2518d-183">自訂使用者類別</span><span class="sxs-lookup"><span data-stu-id="2518d-183">Customize the user class</span></span>

<span data-ttu-id="2518d-184">當實作的儲存體提供者，建立使用者類別，其相當於[IdentityUser 類別](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser)。</span><span class="sxs-lookup"><span data-stu-id="2518d-184">When implementing a storage provider, create a user class which is equivalent to the [IdentityUser class](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).</span></span>

<span data-ttu-id="2518d-185">您的使用者類別必須包含至少`Id`和`UserName`屬性。</span><span class="sxs-lookup"><span data-stu-id="2518d-185">At a minimum, your user class must include an `Id` and a `UserName` property.</span></span>

<span data-ttu-id="2518d-186">`IdentityUser`類別會定義屬性，`UserManager`呼叫時執行要求的作業。</span><span class="sxs-lookup"><span data-stu-id="2518d-186">The `IdentityUser` class defines the properties that the `UserManager` calls when performing requested operations.</span></span> <span data-ttu-id="2518d-187">預設類型`Id`屬性是字串，但您可以繼承自`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`並指定不同的型別。</span><span class="sxs-lookup"><span data-stu-id="2518d-187">The default type of the `Id` property is a string, but you can inherit from `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` and specify a different type.</span></span> <span data-ttu-id="2518d-188">架構必須要有儲存區實作來處理資料類型轉換。</span><span class="sxs-lookup"><span data-stu-id="2518d-188">The framework expects the storage implementation to handle data type conversions.</span></span>

## <a name="customize-the-user-store"></a><span data-ttu-id="2518d-189">自訂使用者存放區</span><span class="sxs-lookup"><span data-stu-id="2518d-189">Customize the user store</span></span>

<span data-ttu-id="2518d-190">建立`UserStore`類別，可提供使用者的所有資料作業的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-190">Create a `UserStore` class that provides the methods for all data operations on the user.</span></span> <span data-ttu-id="2518d-191">這個類別就相當於[UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-191">This class is equivalent to the [UserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) class.</span></span> <span data-ttu-id="2518d-192">在您`UserStore`類別，請實作`IUserStore<TUser>`和所需的選用介面。</span><span class="sxs-lookup"><span data-stu-id="2518d-192">In your `UserStore` class, implement `IUserStore<TUser>` and the optional interfaces required.</span></span> <span data-ttu-id="2518d-193">您選取哪一個選擇性的介面來實作您的應用程式中提供的功能。</span><span class="sxs-lookup"><span data-stu-id="2518d-193">You select which optional interfaces to implement based on the functionality provided in your app.</span></span>

### <a name="optional-interfaces"></a><span data-ttu-id="2518d-194">選擇性的介面</span><span class="sxs-lookup"><span data-stu-id="2518d-194">Optional interfaces</span></span>

* [<span data-ttu-id="2518d-195">IUserRoleStore</span><span class="sxs-lookup"><span data-stu-id="2518d-195">IUserRoleStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [<span data-ttu-id="2518d-196">IUserClaimStore</span><span class="sxs-lookup"><span data-stu-id="2518d-196">IUserClaimStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [<span data-ttu-id="2518d-197">IUserPasswordStore</span><span class="sxs-lookup"><span data-stu-id="2518d-197">IUserPasswordStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [<span data-ttu-id="2518d-198">IUserSecurityStampStore</span><span class="sxs-lookup"><span data-stu-id="2518d-198">IUserSecurityStampStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [<span data-ttu-id="2518d-199">IUserEmailStore</span><span class="sxs-lookup"><span data-stu-id="2518d-199">IUserEmailStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [<span data-ttu-id="2518d-200">IPhoneNumberStore</span><span class="sxs-lookup"><span data-stu-id="2518d-200">IPhoneNumberStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [<span data-ttu-id="2518d-201">IQueryableUserStore</span><span class="sxs-lookup"><span data-stu-id="2518d-201">IQueryableUserStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [<span data-ttu-id="2518d-202">IUserLoginStore</span><span class="sxs-lookup"><span data-stu-id="2518d-202">IUserLoginStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [<span data-ttu-id="2518d-203">IUserTwoFactorStore</span><span class="sxs-lookup"><span data-stu-id="2518d-203">IUserTwoFactorStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [<span data-ttu-id="2518d-204">IUserLockoutStore</span><span class="sxs-lookup"><span data-stu-id="2518d-204">IUserLockoutStore</span></span>](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

<span data-ttu-id="2518d-205">選擇性的介面繼承自`IUserStore<TUser>`。</span><span class="sxs-lookup"><span data-stu-id="2518d-205">The optional interfaces inherit from `IUserStore<TUser>`.</span></span> <span data-ttu-id="2518d-206">您可以看到部分實作的範例使用者存放[範例應用程式](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)。</span><span class="sxs-lookup"><span data-stu-id="2518d-206">You can see a partially implemented sample user store in the [sample app](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).</span></span>

<span data-ttu-id="2518d-207">內`UserStore`類別，您會使用您建立用來執行作業的資料存取類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-207">Within the `UserStore` class, you use the data access classes that you created to perform operations.</span></span> <span data-ttu-id="2518d-208">這些會傳入使用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="2518d-208">These are passed in using dependency injection.</span></span> <span data-ttu-id="2518d-209">例如，SQL Server 中使用 Dapper 實作時，`UserStore`類別具有`CreateAsync`方法使用的執行個體`DapperUsersTable`插入新的記錄：</span><span class="sxs-lookup"><span data-stu-id="2518d-209">For example, in the SQL Server with Dapper implementation, the `UserStore` class has the `CreateAsync` method which uses an instance of `DapperUsersTable` to insert a new record:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a><span data-ttu-id="2518d-210">若要實作自訂使用者存放區時的介面</span><span class="sxs-lookup"><span data-stu-id="2518d-210">Interfaces to implement when customizing user store</span></span>

* <span data-ttu-id="2518d-211">**IUserStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-211">**IUserStore**</span></span>  
 <span data-ttu-id="2518d-212">[IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1)介面是唯一的介面，您必須在使用者存放區實作。</span><span class="sxs-lookup"><span data-stu-id="2518d-212">The [IUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) interface is the only interface you must implement in the user store.</span></span> <span data-ttu-id="2518d-213">它會定義建立、 更新、 刪除和擷取使用者的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-213">It defines methods for creating, updating, deleting, and retrieving users.</span></span>
* <span data-ttu-id="2518d-214">**IUserClaimStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-214">**IUserClaimStore**</span></span>  
 <span data-ttu-id="2518d-215">[IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)介面會定義您要啟用使用者宣告實作的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-215">The [IUserClaimStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface defines the methods you implement to enable user claims.</span></span> <span data-ttu-id="2518d-216">它包含用於加入、 移除和擷取使用者宣告的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-216">It contains methods for adding, removing and retrieving user claims.</span></span>
* <span data-ttu-id="2518d-217">**IUserLoginStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-217">**IUserLoginStore**</span></span>  
 <span data-ttu-id="2518d-218">[IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)定義您要啟用外部驗證提供者所實作的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-218">The [IUserLoginStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) defines the methods you implement to enable external authentication providers.</span></span> <span data-ttu-id="2518d-219">它包含用於加入、 移除和擷取使用者登入和擷取使用者的登入資訊為基礎的方法的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-219">It contains methods for adding, removing and retrieving user logins, and a method for retrieving a user based on the login information.</span></span>
* <span data-ttu-id="2518d-220">**IUserRoleStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-220">**IUserRoleStore**</span></span>  
 <span data-ttu-id="2518d-221">[IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)介面會定義您要對應至角色的使用者實作的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-221">The [IUserRoleStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface defines the methods you implement to map a user to a role.</span></span> <span data-ttu-id="2518d-222">它包含新增、 移除及擷取使用者的角色和方法來檢查是否要將使用者指派給角色的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-222">It contains methods to add, remove, and retrieve a user's roles, and a method to check if a user is assigned to a role.</span></span>
* <span data-ttu-id="2518d-223">**IUserPasswordStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-223">**IUserPasswordStore**</span></span>  
 <span data-ttu-id="2518d-224">[IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)介面會定義您實作以保存雜湊的密碼的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-224">The [IUserPasswordStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface defines the methods you implement to persist hashed passwords.</span></span> <span data-ttu-id="2518d-225">它包含用於取得和設定雜湊的密碼，並指出使用者是否已設定密碼的方法的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-225">It contains methods for getting and setting the hashed password, and a method that indicates whether the user has set a password.</span></span>
* <span data-ttu-id="2518d-226">**IUserSecurityStampStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-226">**IUserSecurityStampStore**</span></span>  
 <span data-ttu-id="2518d-227">[IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)介面會定義您實作要用於安全性戳記，指出是否已變更的使用者帳戶資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-227">The [IUserSecurityStampStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface defines the methods you implement to use a security stamp for indicating whether the user's account information has changed.</span></span> <span data-ttu-id="2518d-228">當使用者變更密碼，或新增或移除登入，則會更新這個戳記。</span><span class="sxs-lookup"><span data-stu-id="2518d-228">This stamp is updated when a user changes the password, or adds or removes logins.</span></span> <span data-ttu-id="2518d-229">它包含方法來取得和設定的安全性戳記。</span><span class="sxs-lookup"><span data-stu-id="2518d-229">It contains methods for getting and setting the security stamp.</span></span>
* <span data-ttu-id="2518d-230">**IUserTwoFactorStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-230">**IUserTwoFactorStore**</span></span>  
 <span data-ttu-id="2518d-231">[IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)介面會定義您實作以支援雙因素驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-231">The [IUserTwoFactorStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface defines the methods you implement to support two factor authentication.</span></span> <span data-ttu-id="2518d-232">它包含用於取得和設定是否針對使用者啟用雙因素驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-232">It contains methods for getting and setting whether two factor authentication is enabled for a user.</span></span>
* <span data-ttu-id="2518d-233">**IUserPhoneNumberStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-233">**IUserPhoneNumberStore**</span></span>  
 <span data-ttu-id="2518d-234">[IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)介面會定義您實作以儲存使用者的電話號碼的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-234">The [IUserPhoneNumberStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface defines the methods you implement to store user phone numbers.</span></span> <span data-ttu-id="2518d-235">它包含用於取得和設定的電話號碼和是否確認電話號碼的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-235">It contains methods for getting and setting the phone number and whether the phone number is confirmed.</span></span>
* <span data-ttu-id="2518d-236">**IUserEmailStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-236">**IUserEmailStore**</span></span>  
 <span data-ttu-id="2518d-237">[IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)介面會定義您要儲存使用者的電子郵件地址所實作的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-237">The [IUserEmailStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface defines the methods you implement to store user email addresses.</span></span> <span data-ttu-id="2518d-238">它包含用於取得和設定電子郵件地址和電子郵件是否已確認的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-238">It contains methods for getting and setting the email address and whether the email is confirmed.</span></span>
* <span data-ttu-id="2518d-239">**IUserLockoutStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-239">**IUserLockoutStore**</span></span>  
 <span data-ttu-id="2518d-240">[IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)介面會定義您實作以儲存有關鎖定的帳戶資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-240">The [IUserLockoutStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface defines the methods you implement to store information about locking an account.</span></span> <span data-ttu-id="2518d-241">它包含用於追蹤失敗的存取嘗試和鎖定的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-241">It contains methods for tracking failed access attempts and lockouts.</span></span>
* <span data-ttu-id="2518d-242">**IQueryableUserStore**</span><span class="sxs-lookup"><span data-stu-id="2518d-242">**IQueryableUserStore**</span></span>  
 <span data-ttu-id="2518d-243">[IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)介面會定義您實作用來提供的可查詢的使用者存放區的成員。</span><span class="sxs-lookup"><span data-stu-id="2518d-243">The [IQueryableUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface defines the members you implement to provide a queryable user store.</span></span>

<span data-ttu-id="2518d-244">在您的應用程式中，您就會實作所需介面。</span><span class="sxs-lookup"><span data-stu-id="2518d-244">You implement only the interfaces that are needed in your app.</span></span> <span data-ttu-id="2518d-245">例如: </span><span class="sxs-lookup"><span data-stu-id="2518d-245">For example:</span></span>

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a><span data-ttu-id="2518d-246">IdentityUserClaim、 IdentityUserLogin 和 IdentityUserRole</span><span class="sxs-lookup"><span data-stu-id="2518d-246">IdentityUserClaim, IdentityUserLogin, and IdentityUserRole</span></span>

<span data-ttu-id="2518d-247">`Microsoft.AspNet.Identity.EntityFramework`命名空間包含的實作[IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1)， [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)，和[IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1)類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-247">The `Microsoft.AspNet.Identity.EntityFramework` namespace contains implementations of the [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), and [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes.</span></span> <span data-ttu-id="2518d-248">如果您使用這些功能，您可能想要建立您自己的這些類別版本，並定義您的應用程式的屬性。</span><span class="sxs-lookup"><span data-stu-id="2518d-248">If you are using these features, you may want to create your own versions of these classes and define the properties for your app.</span></span> <span data-ttu-id="2518d-249">不過，有時它會更有效率的方式將這些實體載入記憶體時執行基本作業 （例如新增或移除使用者的宣告）。</span><span class="sxs-lookup"><span data-stu-id="2518d-249">However, sometimes it's more efficient to not load these entities into memory when performing basic operations (such as adding or removing a user's claim).</span></span> <span data-ttu-id="2518d-250">請改為後端存放區的類別可以執行這些作業，直接在資料來源上。</span><span class="sxs-lookup"><span data-stu-id="2518d-250">Instead, the backend store classes can execute these operations directly on the data source.</span></span> <span data-ttu-id="2518d-251">例如，`UserStore.GetClaimsAsync`方法可以呼叫`userClaimTable.FindByUserId(user.Id)`方法上執行查詢，直接資料表，並傳回宣告的清單。</span><span class="sxs-lookup"><span data-stu-id="2518d-251">For example, the `UserStore.GetClaimsAsync` method can call the `userClaimTable.FindByUserId(user.Id)` method to execute a query on that table directly and return a list of claims.</span></span>

## <a name="customize-the-role-class"></a><span data-ttu-id="2518d-252">自訂角色類別</span><span class="sxs-lookup"><span data-stu-id="2518d-252">Customize the role class</span></span>

<span data-ttu-id="2518d-253">當實作角色存放裝置提供者，您可以建立自訂的角色類型。</span><span class="sxs-lookup"><span data-stu-id="2518d-253">When implementing a role storage provider, you can create a custom role type.</span></span> <span data-ttu-id="2518d-254">它不需要實作特定介面，但是它必須擁有`Id`而且通常會有`Name`屬性。</span><span class="sxs-lookup"><span data-stu-id="2518d-254">It need not implement a particular interface, but it must have an `Id` and typically it will have a `Name` property.</span></span>

<span data-ttu-id="2518d-255">以下是範例角色類別：</span><span class="sxs-lookup"><span data-stu-id="2518d-255">The following is an example role class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a><span data-ttu-id="2518d-256">自訂角色存放區</span><span class="sxs-lookup"><span data-stu-id="2518d-256">Customize the role store</span></span>

<span data-ttu-id="2518d-257">您可以建立`RoleStore`類別，可提供在角色上的所有資料作業的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-257">You can create a `RoleStore` class that provides the methods for all data operations on roles.</span></span> <span data-ttu-id="2518d-258">這個類別就相當於[RoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-258">This class is equivalent to the [RoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) class.</span></span> <span data-ttu-id="2518d-259">在 `RoleStore`類別，實作`IRoleStore<TRole>`並選擇性地`IQueryableRoleStore<TRole>`介面。</span><span class="sxs-lookup"><span data-stu-id="2518d-259">In the `RoleStore` class, you implement the `IRoleStore<TRole>` and optionally the `IQueryableRoleStore<TRole>` interface.</span></span>

* <span data-ttu-id="2518d-260">**IRoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="2518d-260">**IRoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="2518d-261">[IRoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1)介面會定義角色存放區類別中實作的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-261">The [IRoleStore&lt;TRole&gt;](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interface defines the methods to implement in the role store class.</span></span> <span data-ttu-id="2518d-262">它包含如建立、 更新、 刪除和擷取角色的方法。</span><span class="sxs-lookup"><span data-stu-id="2518d-262">It contains methods for creating, updating, deleting, and retrieving roles.</span></span>
* <span data-ttu-id="2518d-263">**RoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="2518d-263">**RoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="2518d-264">若要自訂`RoleStore`，建立可實作類別`IRoleStore<TRole>`介面。</span><span class="sxs-lookup"><span data-stu-id="2518d-264">To customize `RoleStore`, create a class that implements the `IRoleStore<TRole>` interface.</span></span> 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a><span data-ttu-id="2518d-265">重新設定應用程式以使用新的儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="2518d-265">Reconfigure app to use a new storage provider</span></span>

<span data-ttu-id="2518d-266">一旦您實作的儲存體提供者，您會設定您的應用程式使用它。</span><span class="sxs-lookup"><span data-stu-id="2518d-266">Once you have implemented a storage provider, you configure your app to use it.</span></span> <span data-ttu-id="2518d-267">如果您的應用程式會使用預設提供者，請將它取代您的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="2518d-267">If your app used the default provider, replace it with your custom provider.</span></span>

1. <span data-ttu-id="2518d-268">移除`Microsoft.AspNetCore.EntityFramework.Identity`NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2518d-268">Remove the `Microsoft.AspNetCore.EntityFramework.Identity` NuGet package.</span></span>
1. <span data-ttu-id="2518d-269">如果存放裝置提供者位於不同的專案或封裝中，新增對它的參考。</span><span class="sxs-lookup"><span data-stu-id="2518d-269">If the storage provider resides in a separate project or package, add a reference to it.</span></span>
1. <span data-ttu-id="2518d-270">取代所有參考`Microsoft.AspNetCore.EntityFramework.Identity`使用您的儲存體提供者的命名空間的陳述式。</span><span class="sxs-lookup"><span data-stu-id="2518d-270">Replace all references to `Microsoft.AspNetCore.EntityFramework.Identity` with a using statement for the namespace of your storage provider.</span></span>
1. <span data-ttu-id="2518d-271">在 `ConfigureServices`方法中，變更`AddIdentity`方法，以使用您的自訂型別。</span><span class="sxs-lookup"><span data-stu-id="2518d-271">In the `ConfigureServices` method, change the `AddIdentity` method to use your custom types.</span></span> <span data-ttu-id="2518d-272">您可以建立自己的延伸模組方法，針對此目的。</span><span class="sxs-lookup"><span data-stu-id="2518d-272">You can create your own extension methods for this purpose.</span></span> <span data-ttu-id="2518d-273">請參閱[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)的範例。</span><span class="sxs-lookup"><span data-stu-id="2518d-273">See [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) for an example.</span></span>
1. <span data-ttu-id="2518d-274">如果您使用的角色，更新`RoleManager`若要使用您`RoleStore`類別。</span><span class="sxs-lookup"><span data-stu-id="2518d-274">If you are using Roles, update the `RoleManager` to use your `RoleStore` class.</span></span>
1. <span data-ttu-id="2518d-275">更新您的應用程式設定的連接字串和認證。</span><span class="sxs-lookup"><span data-stu-id="2518d-275">Update the connection string and credentials to your app's configuration.</span></span>

<span data-ttu-id="2518d-276">範例：</span><span class="sxs-lookup"><span data-stu-id="2518d-276">Example:</span></span>

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

## <a name="references"></a><span data-ttu-id="2518d-277">參考</span><span class="sxs-lookup"><span data-stu-id="2518d-277">References</span></span>

* [<span data-ttu-id="2518d-278">ASP.NET 4.x 身分識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="2518d-278">Custom Storage Providers for ASP.NET 4.x Identity</span></span>](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
* <span data-ttu-id="2518d-279">[ASP.NET Core Identity](https://github.com/aspnet/identity) &ndash;此存放庫包含社群維護存放區提供者的連結。</span><span class="sxs-lookup"><span data-stu-id="2518d-279">[ASP.NET Core Identity](https://github.com/aspnet/identity) &ndash; This repository includes links to community maintained store providers.</span></span>
