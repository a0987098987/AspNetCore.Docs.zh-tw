---
title: 在 ASP.NET Core razor 頁面單元測試
author: guardrex
description: 了解如何建立 Razor 頁面應用程式的單元測試。
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: 924908a92eea23fd2dc81a3809e74760d9295e4f
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477406"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a><span data-ttu-id="0f06c-103">在 ASP.NET Core razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="0f06c-103">Razor Pages unit tests in ASP.NET Core</span></span>

<span data-ttu-id="0f06c-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0f06c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0f06c-105">ASP.NET Core 支援 Razor 網頁應用程式的單元的測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-105">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="0f06c-106">測試資料的存取層 (DAL) 和頁面模型可協助您確保：</span><span class="sxs-lookup"><span data-stu-id="0f06c-106">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="0f06c-107">Razor 頁面應用程式的組件的應用程式建構期間運作彼此獨立，而且一起當做一個單位。</span><span class="sxs-lookup"><span data-stu-id="0f06c-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="0f06c-108">類別和方法有限範圍的責任。</span><span class="sxs-lookup"><span data-stu-id="0f06c-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="0f06c-109">其他文件存在於應用程式的行為方式。</span><span class="sxs-lookup"><span data-stu-id="0f06c-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="0f06c-110">迴歸測試，也就是更新的程式碼所帶來的錯誤，會自動的建置和部署期間找到的。</span><span class="sxs-lookup"><span data-stu-id="0f06c-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="0f06c-111">本主題假設您有基本了解 Razor Pages 應用程式和單元測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-111">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="0f06c-112">如果您不熟悉搭配 Razor 頁面應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="0f06c-112">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* [<span data-ttu-id="0f06c-113">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="0f06c-113">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="0f06c-114">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0f06c-114">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="0f06c-115">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f06c-115">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="0f06c-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0f06c-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0f06c-117">範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="0f06c-117">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="0f06c-118">應用程式</span><span class="sxs-lookup"><span data-stu-id="0f06c-118">App</span></span>         | <span data-ttu-id="0f06c-119">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="0f06c-119">Project folder</span></span>                        | <span data-ttu-id="0f06c-120">描述</span><span class="sxs-lookup"><span data-stu-id="0f06c-120">Description</span></span> |
| ----------- | ------------------------------------- | ----------- |
| <span data-ttu-id="0f06c-121">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="0f06c-121">Message app</span></span> | <span data-ttu-id="0f06c-122">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="0f06c-122">*src/RazorPagesTestSample*</span></span>            | <span data-ttu-id="0f06c-123">允許使用者加入、 刪除其中一個、 全部刪除，以及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="0f06c-123">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="0f06c-124">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="0f06c-124">Test app</span></span>    | <span data-ttu-id="0f06c-125">*tests/RazorPagesTestSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="0f06c-125">*tests/RazorPagesTestSample.Tests*</span></span>    | <span data-ttu-id="0f06c-126">使用單元測試的訊息應用程式： 資料存取層 (DAL) 和索引 頁面模型。</span><span class="sxs-lookup"><span data-stu-id="0f06c-126">Used to unit test the message app: Data access layer (DAL) and Index page model.</span></span> |

<span data-ttu-id="0f06c-127">可以使用內建的測試功能的 IDE，例如執行測試[Visual Studio](https://www.visualstudio.com/vs/)。</span><span class="sxs-lookup"><span data-stu-id="0f06c-127">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="0f06c-128">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列中，執行下列命令在命令提示字元中*tests/RazorPagesTestSample.Tests*資料夾：</span><span class="sxs-lookup"><span data-stu-id="0f06c-128">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```console
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="0f06c-129">訊息應用程式的組織</span><span class="sxs-lookup"><span data-stu-id="0f06c-129">Message app organization</span></span>

<span data-ttu-id="0f06c-130">訊息應用程式是簡單的 Razor Pages 訊息系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="0f06c-130">The message app is a simple Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="0f06c-131">應用程式的 [索引] 頁面 (*pages/Index.cshtml*並*Pages/Index.cshtml.cs*) 提供 UI 和頁面模型的方法，來控制新增、 刪除和分析的訊息 （每個訊息的平均字）.</span><span class="sxs-lookup"><span data-stu-id="0f06c-131">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="0f06c-132">所描述的訊息`Message`類別 (*Data/Message.cs*) 使用兩個屬性： `Id` （索引鍵） 和`Text`（訊息）。</span><span class="sxs-lookup"><span data-stu-id="0f06c-132">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="0f06c-133">`Text`屬性是必要的且限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="0f06c-133">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="0f06c-134">使用儲存的訊息[Entity Framework 的記憶體中的資料庫](/ef/core/providers/in-memory/)&#8224;。</span><span class="sxs-lookup"><span data-stu-id="0f06c-134">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="0f06c-135">應用程式在其資料庫內容類別中，包含資料存取層 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="0f06c-135">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="0f06c-136">DAL 方法標示為`virtual`，可讓模擬 (mock) 的測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-136">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="0f06c-137">如果資料庫是空的應用程式啟動時，就會使用三個訊息初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="0f06c-137">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="0f06c-138">這些*植入訊息*也會用於測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-138">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="0f06c-139">&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)，說明如何使用記憶體中資料庫的 mstest 執行測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-139">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="0f06c-140">本主題會使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="0f06c-140">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="0f06c-141">測試概念和跨不同的測試架構的測試實作都類似，但不是完全相同。</span><span class="sxs-lookup"><span data-stu-id="0f06c-141">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="0f06c-142">雖然應用程式不會使用儲存機制模式，而且不是有效的範例[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 頁面支援的開發這些模式。</span><span class="sxs-lookup"><span data-stu-id="0f06c-142">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="0f06c-143">如需詳細資訊，請參閱 <<c0> [ 設計基礎結構持續層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)並[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（此範例會實作儲存機制模式）。</span><span class="sxs-lookup"><span data-stu-id="0f06c-143">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="0f06c-144">測試應用程式的組織</span><span class="sxs-lookup"><span data-stu-id="0f06c-144">Test app organization</span></span>

<span data-ttu-id="0f06c-145">測試應用程式是主控台應用程式內*tests/RazorPagesTestSample.Tests*資料夾。</span><span class="sxs-lookup"><span data-stu-id="0f06c-145">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="0f06c-146">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="0f06c-146">Test app folder</span></span> | <span data-ttu-id="0f06c-147">描述</span><span class="sxs-lookup"><span data-stu-id="0f06c-147">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="0f06c-148">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="0f06c-148">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="0f06c-149">*DataAccessLayerTest.cs* DAL 包含單元測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-149">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="0f06c-150">*IndexPageTests.cs*包含 [索引] 頁面模型的單元測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-150">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="0f06c-151">*公用程式*</span><span class="sxs-lookup"><span data-stu-id="0f06c-151">*Utilities*</span></span>     | <span data-ttu-id="0f06c-152">包含`TestingDbContextOptions`用來建立新資料庫的每個 DAL 單元測試的內容選項，使資料庫重設為其基準條件，針對每個測試方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-152">Contains the `TestingDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="0f06c-153">測試架構[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="0f06c-153">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="0f06c-154">模擬 (mock) 架構的物件是[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="0f06c-154">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="0f06c-155">單元測試的資料存取層 (DAL)</span><span class="sxs-lookup"><span data-stu-id="0f06c-155">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="0f06c-156">訊息應用程式中所包含的四種方法有 DAL`AppDbContext`類別 (*src/RazorPagesTestSample/Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="0f06c-156">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="0f06c-157">每個方法會有一個或兩個單元測試中測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="0f06c-157">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="0f06c-158">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="0f06c-158">DAL method</span></span>               | <span data-ttu-id="0f06c-159">功能</span><span class="sxs-lookup"><span data-stu-id="0f06c-159">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="0f06c-160">取得`List<Message>`從資料庫依排序`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="0f06c-160">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="0f06c-161">新增`Message`到資料庫。</span><span class="sxs-lookup"><span data-stu-id="0f06c-161">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="0f06c-162">刪除所有`Message`資料庫的項目。</span><span class="sxs-lookup"><span data-stu-id="0f06c-162">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="0f06c-163">會刪除單一`Message`從資料庫`Id`。</span><span class="sxs-lookup"><span data-stu-id="0f06c-163">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="0f06c-164">需要單元測試的 DAL [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)時建立新`AppDbContext`針對每個測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-164">Unit tests of the DAL require [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="0f06c-165">若要建立的其中一個方法`DbContextOptions`每項測試是使用[DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span><span class="sxs-lookup"><span data-stu-id="0f06c-165">One approach to creating the `DbContextOptions` for each test is to use a [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="0f06c-166">此方法的問題是，每項測試會接收資料庫中任何狀態之前的測試，維持不變。</span><span class="sxs-lookup"><span data-stu-id="0f06c-166">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="0f06c-167">嘗試寫入不會干擾彼此的不可部分完成的單元測試時，這可能會造成問題。</span><span class="sxs-lookup"><span data-stu-id="0f06c-167">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="0f06c-168">若要強制`AppDbContext`若要針對每個測試中使用新的資料庫內容，提供`DbContextOptions`根據新的服務提供者的執行個體。</span><span class="sxs-lookup"><span data-stu-id="0f06c-168">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="0f06c-169">測試應用程式示範如何執行這項操作使用其`Utilities`類別方法`TestingDbContextOptions`(*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="0f06c-169">The test app shows how to do this using its `Utilities` class method `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="0f06c-170">使用`DbContextOptions`DAL 單元測試可讓每個測試，以使用最新的資料庫執行個體以不可分割方式執行：</span><span class="sxs-lookup"><span data-stu-id="0f06c-170">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="0f06c-171">在每個測試方法`DataAccessLayerTest`類別 (*UnitTests/DataAccessLayerTest.cs*) 都遵循類似的排列 Act Assert 模式：</span><span class="sxs-lookup"><span data-stu-id="0f06c-171">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="0f06c-172">排列： 資料庫已設定測試和/或定義預期的結果。</span><span class="sxs-lookup"><span data-stu-id="0f06c-172">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="0f06c-173">Act： 執行測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-173">Act: The test is executed.</span></span>
1. <span data-ttu-id="0f06c-174">判斷提示： 若要判斷測試結果是否順利進行判斷提示。</span><span class="sxs-lookup"><span data-stu-id="0f06c-174">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="0f06c-175">例如，`DeleteMessageAsync`方法會負責移除單一訊息可由其`Id`(*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span><span class="sxs-lookup"><span data-stu-id="0f06c-175">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="0f06c-176">有兩個測試，此方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-176">There are two tests for this method.</span></span> <span data-ttu-id="0f06c-177">一項測試會檢查方法在資料庫中有訊息時，會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="0f06c-177">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="0f06c-178">如果，不會變更資料庫的其他方法測試訊息`Id`刪除不存在。</span><span class="sxs-lookup"><span data-stu-id="0f06c-178">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="0f06c-179">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="0f06c-179">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="0f06c-180">首先，方法會執行排列步驟中，準備進行 Act 步驟發生的位置。</span><span class="sxs-lookup"><span data-stu-id="0f06c-180">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="0f06c-181">取得和保留在植入訊息`seedMessages`。</span><span class="sxs-lookup"><span data-stu-id="0f06c-181">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="0f06c-182">植入的訊息儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="0f06c-182">The seeding messages are saved into the database.</span></span> <span data-ttu-id="0f06c-183">與訊息`Id`的`1`設定為刪除。</span><span class="sxs-lookup"><span data-stu-id="0f06c-183">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="0f06c-184">當`DeleteMessageAsync`方法執行、 預期的訊息應該有的所有除了與訊息`Id`的`1`。</span><span class="sxs-lookup"><span data-stu-id="0f06c-184">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="0f06c-185">`expectedMessages`變數代表此預期的結果。</span><span class="sxs-lookup"><span data-stu-id="0f06c-185">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="0f06c-186">方法是：`DeleteMessageAsync`方法會執行傳入`recId`的`1`:</span><span class="sxs-lookup"><span data-stu-id="0f06c-186">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="0f06c-187">最後，此方法會取得`Messages`從內容，並比較它`expectedMessages`這兩個相等的判斷提示：</span><span class="sxs-lookup"><span data-stu-id="0f06c-187">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="0f06c-188">若要比較的兩個`List<Message>`相同：</span><span class="sxs-lookup"><span data-stu-id="0f06c-188">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="0f06c-189">訊息會按照`Id`。</span><span class="sxs-lookup"><span data-stu-id="0f06c-189">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="0f06c-190">訊息配對會比較上`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="0f06c-190">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="0f06c-191">類似的測試方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`會檢查嘗試刪除不存在的訊息的結果。</span><span class="sxs-lookup"><span data-stu-id="0f06c-191">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="0f06c-192">在此情況下，在資料庫中的預期的訊息應該等於實際的訊息之後`DeleteMessageAsync`執行方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-192">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="0f06c-193">應該不會變更資料庫的內容：</span><span class="sxs-lookup"><span data-stu-id="0f06c-193">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="0f06c-194">頁面模型方法的單元測試</span><span class="sxs-lookup"><span data-stu-id="0f06c-194">Unit tests of the page model methods</span></span>

<span data-ttu-id="0f06c-195">單元測試的另一組負責測試的頁面模型的方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-195">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="0f06c-196">在中找到的訊息應用程式中，索引頁面模型`IndexModel`類別內*src/RazorPagesTestSample/Pages/Index.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="0f06c-196">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="0f06c-197">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="0f06c-197">Page model method</span></span> | <span data-ttu-id="0f06c-198">功能</span><span class="sxs-lookup"><span data-stu-id="0f06c-198">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="0f06c-199">取得訊息從 UI 使用 DAL`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-199">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="0f06c-200">如果`ModelState`有效，但呼叫`AddMessageAsync`將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="0f06c-200">If the `ModelState` is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="0f06c-201">呼叫`DeleteAllMessagesAsync`刪除所有資料庫中的訊息。</span><span class="sxs-lookup"><span data-stu-id="0f06c-201">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="0f06c-202">執行`DeleteMessageAsync`若要刪除的訊息`Id`指定。</span><span class="sxs-lookup"><span data-stu-id="0f06c-202">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="0f06c-203">如果一或多個訊息都位於資料庫中，會計算每個訊息的字組的平均數目。</span><span class="sxs-lookup"><span data-stu-id="0f06c-203">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="0f06c-204">頁面模型的方法使用中的七個測試進行測試`IndexPageTests`類別 (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*)。</span><span class="sxs-lookup"><span data-stu-id="0f06c-204">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="0f06c-205">測試使用熟悉的排列判斷提示動作模式。</span><span class="sxs-lookup"><span data-stu-id="0f06c-205">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="0f06c-206">這些測試著重於：</span><span class="sxs-lookup"><span data-stu-id="0f06c-206">These tests focus on:</span></span>

* <span data-ttu-id="0f06c-207">決定是否方法均遵循正確的行為時`ModelState`無效。</span><span class="sxs-lookup"><span data-stu-id="0f06c-207">Determining if the methods follow the correct behavior when the `ModelState` is invalid.</span></span>
* <span data-ttu-id="0f06c-208">確認方法會產生正確`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="0f06c-208">Confirming the methods produce the correct `IActionResult`.</span></span>
* <span data-ttu-id="0f06c-209">檢查屬性值的設定正確進行。</span><span class="sxs-lookup"><span data-stu-id="0f06c-209">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="0f06c-210">測試這個群組通常會模擬 DAL 以產生預期的資料，頁面模型方法執行所在的動作步驟的方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-210">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="0f06c-211">例如，`GetMessagesAsync`方法的`AppDbContext`模擬以產生輸出。</span><span class="sxs-lookup"><span data-stu-id="0f06c-211">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="0f06c-212">當頁面模型方法執行這個方法時，mock 就會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="0f06c-212">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="0f06c-213">資料並不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="0f06c-213">The data doesn't come from the database.</span></span> <span data-ttu-id="0f06c-214">這會建立在頁面模型測試中使用 DAL 的可預測、 可靠的測試條件。</span><span class="sxs-lookup"><span data-stu-id="0f06c-214">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="0f06c-215">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`測試顯示如何`GetMessagesAsync`方法模擬頁面模型：</span><span class="sxs-lookup"><span data-stu-id="0f06c-215">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="0f06c-216">當`OnGetAsync`方法執行動作步驟中，它會呼叫頁面模型的`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="0f06c-216">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="0f06c-217">單元測試的動作步驟 (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span><span class="sxs-lookup"><span data-stu-id="0f06c-217">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="0f06c-218">`IndexPage` 頁面模型`OnGetAsync`方法 (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="0f06c-218">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="0f06c-219">`GetMessagesAsync` DAL 中的方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="0f06c-219">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="0f06c-220">模擬 （mock） 的版本的方法傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="0f06c-220">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="0f06c-221">在 `Assert`步驟，實際的訊息 (`actualMessages`) 會從指派`Messages`頁面模型屬性。</span><span class="sxs-lookup"><span data-stu-id="0f06c-221">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="0f06c-222">當指派給訊息時，也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="0f06c-222">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="0f06c-223">預期和實際的訊息會藉由比較其`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="0f06c-223">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="0f06c-224">測試判斷提示，這兩個`List<Message>`執行個體包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="0f06c-224">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="0f06c-225">其他測試，此群組中的建立的頁面模型物件，其中包含`DefaultHttpContext`，則`ModelStateDictionary`，則`ActionContext`來建立`PageContext`、 `ViewDataDictionary`，和`PageContext`。</span><span class="sxs-lookup"><span data-stu-id="0f06c-225">Other tests in this group create page model objects that include the `DefaultHttpContext`, the `ModelStateDictionary`, an `ActionContext` to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="0f06c-226">這些是用於執行測試。</span><span class="sxs-lookup"><span data-stu-id="0f06c-226">These are useful in conducting tests.</span></span> <span data-ttu-id="0f06c-227">例如，訊息應用程式會建立`ModelState`錯誤`AddModelError`以確認有效`PageResult`時，會傳回`OnPostAddMessageAsync`執行：</span><span class="sxs-lookup"><span data-stu-id="0f06c-227">For example, the message app establishes a `ModelState` error with `AddModelError` to check that a valid `PageResult` is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="0f06c-228">其他資源</span><span class="sxs-lookup"><span data-stu-id="0f06c-228">Additional resources</span></span>

* [<span data-ttu-id="0f06c-229">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f06c-229">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="0f06c-230">測試控制器</span><span class="sxs-lookup"><span data-stu-id="0f06c-230">Test controllers</span></span>](xref:mvc/controllers/testing)
* <span data-ttu-id="0f06c-231">[您的程式碼進行單元測試](/visualstudio/test/unit-test-your-code)(Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="0f06c-231">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* [<span data-ttu-id="0f06c-232">整合測試</span><span class="sxs-lookup"><span data-stu-id="0f06c-232">Integration tests</span></span>](xref:test/integration-tests)
* [<span data-ttu-id="0f06c-233">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="0f06c-233">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="0f06c-234">開始使用 xUnit.net （.NET Core/ASP.NET Core）</span><span class="sxs-lookup"><span data-stu-id="0f06c-234">Getting started with xUnit.net (.NET Core/ASP.NET Core)</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="0f06c-235">Moq</span><span class="sxs-lookup"><span data-stu-id="0f06c-235">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="0f06c-236">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="0f06c-236">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
