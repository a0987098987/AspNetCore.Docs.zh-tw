---
title: 使用 ASP.NET Core 的存放庫模式
author: ardalis
description: 了解如何在 ASP.NET Core 應用程式中實作存放庫應用程式設計模式。
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342594"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="b6fe8-103">使用 ASP.NET Core 的存放庫模式</span><span class="sxs-lookup"><span data-stu-id="b6fe8-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="b6fe8-104">作者：[Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com) 與 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b6fe8-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b6fe8-105">*存放庫模式*是一個將資料存取隔離在介面抽象後方的設計模式。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="b6fe8-106">連線到資料庫並處理資料儲存體物件是透過由介面實作所提供的方法來執行。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="b6fe8-107">因此，不需要呼叫程式碼以處理資料庫問題，例如連線、命令與讀者。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="b6fe8-108">使用 ASP.NET Core 的存放庫模式實作有下列優點：</span><span class="sxs-lookup"><span data-stu-id="b6fe8-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="b6fe8-109">應用程式的組織的複雜性較低，而且在商業與資料存取層之間沒有直接相互依存。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="b6fe8-110">比較容易重複使用資料庫存取程式碼，因為程式碼是由一或多個存放庫集中管理。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="b6fe8-111">可以從資料庫層在不相互依存的情況下針對商業網域進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="b6fe8-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6fe8-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b6fe8-113">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples)使用存放庫模式來初始化及顯示電影角色名稱清單。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="b6fe8-114">應用程式為其資料持續性使用 [Entity Framework Core](/ef/core/) 與 `ApplicationDbContext` 類別，但資料庫基礎結構在資料存取處並不顯而易見。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="b6fe8-115">資料存取與資料庫物件是從[存放庫](https://martinfowler.com/eaaCatalog/repository.html)後方進行抽象化。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="b6fe8-116">存放庫介面</span><span class="sxs-lookup"><span data-stu-id="b6fe8-116">Repository interface</span></span>

<span data-ttu-id="b6fe8-117">存放庫介面定義實作的屬性與方法。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="b6fe8-118">在範例應用程式中，電影角色資料的存放庫介面是 `ICharacterRepository`。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="b6fe8-119">`ICharacterRepository` 定義 `ListAll` 與 `Add` 方法，它們是在應用程式中搭配 `Character` 執行個體使用的必要項目：</span><span class="sxs-lookup"><span data-stu-id="b6fe8-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b6fe8-120">`Character` 會定義為：</span><span class="sxs-lookup"><span data-stu-id="b6fe8-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="b6fe8-121">存放庫具象型別</span><span class="sxs-lookup"><span data-stu-id="b6fe8-121">Repository concrete type</span></span>

<span data-ttu-id="b6fe8-122">介面是由具象型別所實作。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="b6fe8-123">在範例應用程式中，`CharacterRepository` 會管理資料庫內容並實作 `ListAll` 與 `Add` 方法：</span><span class="sxs-lookup"><span data-stu-id="b6fe8-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="b6fe8-124">註冊存放庫服務</span><span class="sxs-lookup"><span data-stu-id="b6fe8-124">Register the repository service</span></span>

<span data-ttu-id="b6fe8-125">存放庫與資料庫內容是使用 `Startup.ConfigureServices` 中的服務容器所註冊。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b6fe8-126">在範例應用程式中，`ApplicationDbContext`是藉由呼叫擴充方法 [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 來設定。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="b6fe8-127">`ICharacterRepository` 是註冊為具範圍服務：</span><span class="sxs-lookup"><span data-stu-id="b6fe8-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="b6fe8-128">插入存放庫的執行個體</span><span class="sxs-lookup"><span data-stu-id="b6fe8-128">Inject an instance of the repository</span></span>

<span data-ttu-id="b6fe8-129">在需要資料庫存取的類別中，會透過建構函式要求存放庫的執行個體，並指派給私人欄位以供類別方法使用。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="b6fe8-130">在範例應用程式中，`ICharacterRepository` 是用來：</span><span class="sxs-lookup"><span data-stu-id="b6fe8-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="b6fe8-131">在沒有任何角色存在的情況下填入資料庫。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="b6fe8-132">取得要顯示的角色清單。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="b6fe8-133">注意，呼叫程式碼只與介面的實作 `CharacterRepository` 互動。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="b6fe8-134">呼叫程式碼未直接使用 `ApplicationDbContext`：</span><span class="sxs-lookup"><span data-stu-id="b6fe8-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="b6fe8-135">一般存放庫介面</span><span class="sxs-lookup"><span data-stu-id="b6fe8-135">Generic repository interface</span></span>

<span data-ttu-id="b6fe8-136">此主題與其範例應用程式示範最簡單的存放庫模式實作，其中會為每個商業物件建立一個存放庫。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="b6fe8-137">若應用程式成長超過一些目標，*一般存放庫介面* 可能可以減少實作存放庫模式所需的程式碼量。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="b6fe8-138">如需詳細資訊，請參閱[DevIQ: 存放庫模式: 一般存放庫介面](http://deviq.com/repository-pattern/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b6fe8-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6fe8-139">其他資源</span><span class="sxs-lookup"><span data-stu-id="b6fe8-139">Additional resources</span></span>

* <span data-ttu-id="b6fe8-140">[DevIQ: 存放庫模式](https://deviq.com/repository-pattern/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b6fe8-140">[DevIQ: Repository Pattern](https://deviq.com/repository-pattern/)</span></span>
* [<span data-ttu-id="b6fe8-141">相依性插入</span><span class="sxs-lookup"><span data-stu-id="b6fe8-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="b6fe8-142">在檢視中插入相依性</span><span class="sxs-lookup"><span data-stu-id="b6fe8-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="b6fe8-143">在控制器中插入相依性</span><span class="sxs-lookup"><span data-stu-id="b6fe8-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="b6fe8-144">要求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="b6fe8-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* <span data-ttu-id="b6fe8-145">[逆轉控制容器和相依性插入模式](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b6fe8-145">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html)</span></span>
