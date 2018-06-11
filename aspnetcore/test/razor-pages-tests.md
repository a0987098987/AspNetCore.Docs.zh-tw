---
title: 在 ASP.NET Core razor 頁面單元測試
author: guardrex
description: 了解如何建立 Razor 網頁應用程式的單元測試。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/razor-pages-tests
ms.openlocfilehash: df74d8e44b2dff00e76139edba47fd8a30ce33ef
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252301"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a><span data-ttu-id="7d4b5-103">在 ASP.NET Core razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="7d4b5-103">Razor Pages unit tests in ASP.NET Core</span></span>

<span data-ttu-id="7d4b5-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7d4b5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7d4b5-105">ASP.NET Core 支援 Razor 網頁應用程式的單元的測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-105">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="7d4b5-106">測試資料的存取層 (DAL) 和頁面模型可協助您確保：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-106">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="7d4b5-107">Razor 網頁應用程式的組件的應用程式建構期間運作彼此獨立，而且一起當做一個單位。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="7d4b5-108">類別和方法有限範圍的責任。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="7d4b5-109">其他文件存在於應用程式的行為方式。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="7d4b5-110">迴歸，也就是所更新的程式碼的錯誤，會在自動化的建置和部署中找到。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="7d4b5-111">本主題假設您有基本了解 Razor 網頁應用程式和單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-111">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="7d4b5-112">如果您熟悉 Razor 網頁應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-112">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* [<span data-ttu-id="7d4b5-113">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="7d4b5-113">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="7d4b5-114">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7d4b5-114">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="7d4b5-115">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d4b5-115">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="7d4b5-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7d4b5-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7d4b5-117">這個範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-117">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="7d4b5-118">應用程式</span><span class="sxs-lookup"><span data-stu-id="7d4b5-118">App</span></span>         | <span data-ttu-id="7d4b5-119">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="7d4b5-119">Project folder</span></span>                        | <span data-ttu-id="7d4b5-120">描述</span><span class="sxs-lookup"><span data-stu-id="7d4b5-120">Description</span></span> |
| ----------- | ------------------------------------- | ----------- |
| <span data-ttu-id="7d4b5-121">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="7d4b5-121">Message app</span></span> | <span data-ttu-id="7d4b5-122">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="7d4b5-122">*src/RazorPagesTestSample*</span></span>            | <span data-ttu-id="7d4b5-123">允許使用者加入、 刪除其中一個，刪除所有項目，以及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-123">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="7d4b5-124">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="7d4b5-124">Test app</span></span>    | <span data-ttu-id="7d4b5-125">*tests/RazorPagesTestSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="7d4b5-125">*tests/RazorPagesTestSample.Tests*</span></span>    | <span data-ttu-id="7d4b5-126">使用單元測試的訊息應用程式： 資料存取層 (DAL) 和索引頁面模型。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-126">Used to unit test the message app: Data access layer (DAL) and Index page model.</span></span> |

<span data-ttu-id="7d4b5-127">可以執行測試，例如使用內建測試功能的 IDE， [Visual Studio](https://www.visualstudio.com/vs/)。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-127">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="7d4b5-128">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列中，執行下列命令，在命令提示字元中*tests/RazorPagesTestSample.Tests*資料夾：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-128">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```console
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="7d4b5-129">訊息應用程式的組織</span><span class="sxs-lookup"><span data-stu-id="7d4b5-129">Message app organization</span></span>

<span data-ttu-id="7d4b5-130">訊息應用程式是簡單的 Razor 頁面訊息系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-130">The message app is a simple Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="7d4b5-131">索引頁的應用程式 (*Pages/Index.cshtml*和*Pages/Index.cshtml.cs*) 提供 UI 和頁面來控制的新增、 刪除和訊息 （每個訊息的平均字） 的分析模型方法.</span><span class="sxs-lookup"><span data-stu-id="7d4b5-131">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="7d4b5-132">所描述的訊息`Message`類別 (*Data/Message.cs*) 使用兩個屬性： `Id` （金鑰） 和`Text`（訊息）。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-132">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="7d4b5-133">`Text`屬性所需，且限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-133">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="7d4b5-134">訊息會儲存使用[Entity Framework 的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-134">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="7d4b5-135">應用程式在其資料庫內容類別，包含資料存取層 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-135">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="7d4b5-136">DAL 方法標示為`virtual`，可讓模擬測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-136">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="7d4b5-137">如果資料庫是空的應用程式啟動時，訊息存放區會使用三個訊息中初始化。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-137">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="7d4b5-138">這些*植入訊息*也可用於測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-138">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="7d4b5-139">&#8224;EF 主題[測試與 InMemory](/ef/core/miscellaneous/testing/in-memory)，說明如何使用記憶體中資料庫 MSTest 的測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-139">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="7d4b5-140">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-140">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="7d4b5-141">測試概念與測試實作跨不同測試架構很類似的但不是完全相同。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-141">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="7d4b5-142">雖然此應用程式不會使用[儲存機制模式](http://martinfowler.com/eaaCatalog/repository.html)並不是有效的範例[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 頁面支援這些模式開發。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-142">Although the app doesn't use the [repository pattern](http://martinfowler.com/eaaCatalog/repository.html) and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="7d4b5-143">如需詳細資訊，請參閱[設計基礎結構的持續性層級](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)， [ASP.NET MVC 應用程式中實作的儲存機制和工作單元模式](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)，和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（此範例會實作儲存機制模式）。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-143">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="7d4b5-144">測試應用程式的組織</span><span class="sxs-lookup"><span data-stu-id="7d4b5-144">Test app organization</span></span>

<span data-ttu-id="7d4b5-145">測試應用程式是主控台應用程式內*tests/RazorPagesTestSample.Tests*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-145">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="7d4b5-146">測試應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="7d4b5-146">Test app folder</span></span> | <span data-ttu-id="7d4b5-147">描述</span><span class="sxs-lookup"><span data-stu-id="7d4b5-147">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="7d4b5-148">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="7d4b5-148">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="7d4b5-149">*DataAccessLayerTest.cs* DAL 包含單元測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-149">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="7d4b5-150">*IndexPageTests.cs*包含單元測試，索引頁面模型。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-150">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="7d4b5-151">*公用程式*</span><span class="sxs-lookup"><span data-stu-id="7d4b5-151">*Utilities*</span></span>     | <span data-ttu-id="7d4b5-152">包含`TestingDbContextOptions`用來建立新的資料庫內容的每個 DAL 單元測試的選項，使資料庫重設為其基準條件，針對每個測試方法。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-152">Contains the `TestingDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="7d4b5-153">測試架構是[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-153">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="7d4b5-154">模擬架構的物件是[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-154">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="7d4b5-155">單元測試的資料存取層 (DAL)</span><span class="sxs-lookup"><span data-stu-id="7d4b5-155">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="7d4b5-156">訊息應用程式具有四種方法中所包含的 DAL`AppDbContext`類別 (*src/RazorPagesTestSample/Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-156">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="7d4b5-157">每個方法都有一個或兩個單元測試中測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-157">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="7d4b5-158">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="7d4b5-158">DAL method</span></span>               | <span data-ttu-id="7d4b5-159">功能</span><span class="sxs-lookup"><span data-stu-id="7d4b5-159">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="7d4b5-160">取得`List<Message>`從依照資料庫`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-160">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="7d4b5-161">新增`Message`到資料庫。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-161">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="7d4b5-162">刪除所有`Message`資料庫的項目。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-162">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="7d4b5-163">刪除單一`Message`從資料庫`Id`。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-163">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="7d4b5-164">DAL 的單元測試需要[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)時建立新`AppDbContext`針對每個測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-164">Unit tests of the DAL require [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="7d4b5-165">若要建立的其中一個方法`DbContextOptions`每項測試是使用[DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span><span class="sxs-lookup"><span data-stu-id="7d4b5-165">One approach to creating the `DbContextOptions` for each test is to use a [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="7d4b5-166">這個方法的問題是，每項測試會接收資料庫中任何狀態先前測試所保留。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-166">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="7d4b5-167">嘗試寫入，不會干擾彼此不可部分完成的單元測試時，這可能會造成問題。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-167">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="7d4b5-168">若要強制`AppDbContext`若要使用新的資料庫內容，針對每個測試，提供`DbContextOptions`根據新的服務提供者的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-168">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="7d4b5-169">測試應用程式會顯示如何執行此動作使用其`Utilities`類別方法`TestingDbContextOptions`(*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="7d4b5-169">The test app shows how to do this using its `Utilities` class method `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="7d4b5-170">使用`DbContextOptions`DAL 單元測試可讓與全新資料庫的執行個體以不可分割方式執行每個測試：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-170">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="7d4b5-171">在每個測試方法`DataAccessLayerTest`類別 (*UnitTests/DataAccessLayerTest.cs*) 遵循類似的排列 Act Assert 模式：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-171">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="7d4b5-172">排列： 將資料庫設定測試和/或定義獲得預期的結果。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-172">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="7d4b5-173">Act： 執行測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-173">Act: The test is executed.</span></span>
1. <span data-ttu-id="7d4b5-174">判斷提示： 若要判斷是否成功的測試結果進行判斷提示。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-174">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="7d4b5-175">例如，`DeleteMessageAsync`方法負責移除單一訊息，由其`Id`(*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span><span class="sxs-lookup"><span data-stu-id="7d4b5-175">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="7d4b5-176">有兩項測試，此方法。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-176">There are two tests for this method.</span></span> <span data-ttu-id="7d4b5-177">一項測試會檢查方法在資料庫中有訊息時，會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-177">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="7d4b5-178">如果，不會變更資料庫的其他方法測試訊息`Id`刪除不存在。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-178">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="7d4b5-179">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-179">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="7d4b5-180">首先，此方法會執行排列步驟中，準備進行的動作步驟執行所在。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-180">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="7d4b5-181">植入訊息會取得並保存在`seedMessages`。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-181">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="7d4b5-182">植入訊息儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-182">The seeding messages are saved into the database.</span></span> <span data-ttu-id="7d4b5-183">與訊息`Id`的`1`設定為刪除。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-183">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="7d4b5-184">當`DeleteMessageAsync`執行方法、 預期的郵件應具有所有除了有訊息`Id`的`1`。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-184">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="7d4b5-185">`expectedMessages`變數代表這個預期的結果。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-185">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="7d4b5-186">方法會：`DeleteMessageAsync`執行方法中傳遞`recId`的`1`:</span><span class="sxs-lookup"><span data-stu-id="7d4b5-186">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="7d4b5-187">最後，方法會取得`Messages`從內容，並比較它`expectedMessages`判斷提示兩個相等：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-187">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="7d4b5-188">若要比較的兩個`List<Message>`相同：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-188">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="7d4b5-189">訊息都會按照`Id`。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-189">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="7d4b5-190">訊息配對會比較上`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-190">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="7d4b5-191">類似的測試方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`檢查結果的嘗試刪除不存在的訊息。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-191">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="7d4b5-192">在此情況下，在資料庫中的預期的訊息應該會等於真正的訊息之後`DeleteMessageAsync`執行方法。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-192">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="7d4b5-193">應該不會變更資料庫的內容：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-193">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="7d4b5-194">單元測試的頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="7d4b5-194">Unit tests of the page model methods</span></span>

<span data-ttu-id="7d4b5-195">單元測試的另一組負責頁面模型方法的測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-195">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="7d4b5-196">在中找到訊息應用程式時，索引頁面模型`IndexModel`類別*src/RazorPagesTestSample/Pages/Index.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-196">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="7d4b5-197">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="7d4b5-197">Page model method</span></span> | <span data-ttu-id="7d4b5-198">功能</span><span class="sxs-lookup"><span data-stu-id="7d4b5-198">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="7d4b5-199">取得訊息的 UI 使用 DAL`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-199">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="7d4b5-200">如果`ModelState`有效，但呼叫`AddMessageAsync`將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-200">If the `ModelState` is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="7d4b5-201">呼叫`DeleteAllMessagesAsync`刪除所有資料庫中的訊息。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-201">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="7d4b5-202">執行`DeleteMessageAsync`若要刪除的訊息`Id`指定。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-202">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="7d4b5-203">如果一或多個訊息是在資料庫中，會計算平均每個訊息的字組數目。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-203">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="7d4b5-204">使用七個測試中的測試網頁模型方法`IndexPageTests`類別 (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*)。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-204">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="7d4b5-205">測試可以使用熟悉的排列判斷提示動作模式。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-205">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="7d4b5-206">這些測試著重於：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-206">These tests focus on:</span></span>

* <span data-ttu-id="7d4b5-207">判斷方法是否遵循正確的行為時`ModelState`無效。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-207">Determining if the methods follow the correct behavior when the `ModelState` is invalid.</span></span>
* <span data-ttu-id="7d4b5-208">確認的方法產生正確`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-208">Confirming the methods produce the correct `IActionResult`.</span></span>
* <span data-ttu-id="7d4b5-209">檢查屬性值的設定正確進行。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-209">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="7d4b5-210">此群組的測試通常會模擬 DAL 以產生預期的資料頁面模型方法執行所在的動作步驟的方法。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-210">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="7d4b5-211">例如，`GetMessagesAsync`方法`AppDbContext`模仿產生輸出。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-211">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="7d4b5-212">當頁面模型方法執行這個方法時，模擬便會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-212">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="7d4b5-213">資料並不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-213">The data doesn't come from the database.</span></span> <span data-ttu-id="7d4b5-214">這會建立在頁面模型測試中使用 DAL 的可預測、 可靠的測試條件。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-214">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="7d4b5-215">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`測試顯示如何`GetMessagesAsync`方法會模仿頁面模型：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-215">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="7d4b5-216">當`OnGetAsync`方法執行的動作步驟中，它會呼叫頁面模型`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-216">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="7d4b5-217">單元測試的動作步驟 (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span><span class="sxs-lookup"><span data-stu-id="7d4b5-217">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="7d4b5-218">`IndexPage` 頁面模型`OnGetAsync`方法 (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="7d4b5-218">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="7d4b5-219">`GetMessagesAsync` DAL 中的方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-219">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="7d4b5-220">方法的模擬的版本傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-220">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="7d4b5-221">在`Assert`步驟，實際的訊息 (`actualMessages`) 從指派的`Messages`頁面模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-221">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="7d4b5-222">指派給訊息也執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-222">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="7d4b5-223">預期和實際的訊息會比較其`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-223">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="7d4b5-224">測試判斷提示，這兩個`List<Message>`執行個體包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-224">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="7d4b5-225">此群組中的其他測試建立頁面包含模型物件`DefaultHttpContext`、 `ModelStateDictionary`、`ActionContext`建立`PageContext`、 `ViewDataDictionary`，和`PageContext`。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-225">Other tests in this group create page model objects that include the `DefaultHttpContext`, the `ModelStateDictionary`, an `ActionContext` to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="7d4b5-226">這些是適用於執行測試。</span><span class="sxs-lookup"><span data-stu-id="7d4b5-226">These are useful in conducting tests.</span></span> <span data-ttu-id="7d4b5-227">比方說，訊息應用程式會建立`ModelState`錯誤`AddModelError`檢查有效`PageResult`時，會傳回`OnPostAddMessageAsync`執行：</span><span class="sxs-lookup"><span data-stu-id="7d4b5-227">For example, the message app establishes a `ModelState` error with `AddModelError` to check that a valid `PageResult` is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="7d4b5-228">其他資源</span><span class="sxs-lookup"><span data-stu-id="7d4b5-228">Additional resources</span></span>

* [<span data-ttu-id="7d4b5-229">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d4b5-229">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="7d4b5-230">測試控制器</span><span class="sxs-lookup"><span data-stu-id="7d4b5-230">Test controllers</span></span>](xref:mvc/controllers/testing)
* <span data-ttu-id="7d4b5-231">[單元測試程式碼](/visualstudio/test/unit-test-your-code)(Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="7d4b5-231">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* [<span data-ttu-id="7d4b5-232">整合測試</span><span class="sxs-lookup"><span data-stu-id="7d4b5-232">Integration tests</span></span>](xref:test/integration-tests)
* [<span data-ttu-id="7d4b5-233">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="7d4b5-233">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="7d4b5-234">開始使用 xUnit.net （.NET Core/ASP.NET 核心）</span><span class="sxs-lookup"><span data-stu-id="7d4b5-234">Getting started with xUnit.net (.NET Core/ASP.NET Core)</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="7d4b5-235">Moq</span><span class="sxs-lookup"><span data-stu-id="7d4b5-235">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="7d4b5-236">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="7d4b5-236">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
