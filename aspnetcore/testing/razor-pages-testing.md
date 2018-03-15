---
title: "Razor 頁面單位和整合測試 ASP.NET Core"
author: guardrex
description: "了解如何建立 Razor 網頁應用程式的單元和整合測試。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/razor-pages-testing
ms.openlocfilehash: e4f87a8151e378717aa9198e4629711c4ea6ef77
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a><span data-ttu-id="ca3b4-103">Razor 頁面單位和整合測試 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca3b4-103">Razor Pages unit and integration testing in ASP.NET Core</span></span>

<span data-ttu-id="ca3b4-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ca3b4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ca3b4-105">ASP.NET Core 支援單位和整合測試的 Razor 網頁應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-105">ASP.NET Core supports unit and integration testing of Razor Pages apps.</span></span> <span data-ttu-id="ca3b4-106">測試資料存取層 (DAL)、 頁面模型和整合式的網頁元件，可協助確保：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-106">Testing the data access layer (DAL), page models, and integrated page components helps ensure:</span></span>

* <span data-ttu-id="ca3b4-107">Razor 網頁應用程式的組件的應用程式建構期間運作彼此獨立，而且一起當做一個單位。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="ca3b4-108">類別和方法有限範圍的責任。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="ca3b4-109">其他文件存在於應用程式的行為方式。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="ca3b4-110">迴歸，也就是所更新的程式碼的錯誤，會在自動化的建置和部署中找到。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="ca3b4-111">本主題假設您有基本了解 Razor 網頁應用程式、 單元測試，以及整合測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-111">This topic assumes that you have a basic understanding of Razor Pages apps, unit testing, and integration testing.</span></span> <span data-ttu-id="ca3b4-112">如果您熟悉 Razor 網頁應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-112">If you're unfamiliar with Razor Pages apps or testing concepts, see the following topics:</span></span>

* [<span data-ttu-id="ca3b4-113">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="ca3b4-113">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="ca3b4-114">開始使用 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="ca3b4-114">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="ca3b4-115">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca3b4-115">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="ca3b4-116">整合測試</span><span class="sxs-lookup"><span data-stu-id="ca3b4-116">Integration testing</span></span>](xref:testing/integration-testing)

<span data-ttu-id="ca3b4-117">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ca3b4-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ca3b4-118">這個範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-118">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="ca3b4-119">應用程式</span><span class="sxs-lookup"><span data-stu-id="ca3b4-119">App</span></span>         | <span data-ttu-id="ca3b4-120">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="ca3b4-120">Project folder</span></span>                        | <span data-ttu-id="ca3b4-121">描述</span><span class="sxs-lookup"><span data-stu-id="ca3b4-121">Description</span></span> |
| ----------- | ------------------------------------- | ----------- |
| <span data-ttu-id="ca3b4-122">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="ca3b4-122">Message app</span></span> | <span data-ttu-id="ca3b4-123">*src/RazorPagesTestingSample*</span><span class="sxs-lookup"><span data-stu-id="ca3b4-123">*src/RazorPagesTestingSample*</span></span>         | <span data-ttu-id="ca3b4-124">允許使用者加入、 刪除其中一個，刪除所有項目，以及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-124">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="ca3b4-125">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="ca3b4-125">Test app</span></span>    | <span data-ttu-id="ca3b4-126">*tests/RazorPagesTestingSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="ca3b4-126">*tests/RazorPagesTestingSample.Tests*</span></span> | <span data-ttu-id="ca3b4-127">用來測試訊息的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-127">Used to test the message app.</span></span><ul><li><span data-ttu-id="ca3b4-128">單元測試： 資料存取層 (DAL)，索引頁面模型</span><span class="sxs-lookup"><span data-stu-id="ca3b4-128">Unit tests: Data access layer (DAL), Index page model</span></span></li><li><span data-ttu-id="ca3b4-129">整合測試： 索引頁</span><span class="sxs-lookup"><span data-stu-id="ca3b4-129">Integration tests: Index page</span></span></li></ul> |

<span data-ttu-id="ca3b4-130">可以使用內建的測試功能的 IDE，例如執行測試[Visual Studio](https://www.visualstudio.com/vs/)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-130">The tests can be run using the built-in testing features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="ca3b4-131">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列中，執行下列命令，在命令提示字元中*tests/RazorPagesTestingSample.Tests*資料夾：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-131">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestingSample.Tests* folder:</span></span>

```console
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="ca3b4-132">訊息應用程式的組織</span><span class="sxs-lookup"><span data-stu-id="ca3b4-132">Message app organization</span></span>

<span data-ttu-id="ca3b4-133">訊息應用程式是簡單的 Razor 頁面訊息系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-133">The message app is a simple Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="ca3b4-134">索引頁的應用程式 (*Pages/Index.cshtml*和*Pages/Index.cshtml.cs*) 提供 UI 和頁面來控制的新增、 刪除和訊息 （每個訊息的平均字） 的分析模型方法.</span><span class="sxs-lookup"><span data-stu-id="ca3b4-134">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="ca3b4-135">所描述的訊息`Message`類別 (*Data/Message.cs*) 使用兩個屬性： `Id` （金鑰） 和`Text`（訊息）。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-135">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="ca3b4-136">`Text`屬性所需，且限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-136">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="ca3b4-137">訊息會儲存使用[Entity Framework 的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-137">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="ca3b4-138">應用程式在其資料庫內容類別，包含資料存取層 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-138">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="ca3b4-139">DAL 方法標示為`virtual`，可讓模擬測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-139">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="ca3b4-140">如果資料庫是空的應用程式啟動時，訊息存放區會使用三個訊息中初始化。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-140">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="ca3b4-141">這些*植入訊息*也會用於測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-141">These *seeded messages* are also used in testing.</span></span>

<span data-ttu-id="ca3b4-142">&#8224;EF 主題[測試 InMemory](/ef/core/miscellaneous/testing/in-memory)，說明如何使用記憶體中的資料庫使用 MSTest 進行測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-142">&#8224;The EF topic, [Testing with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for testing with MSTest.</span></span> <span data-ttu-id="ca3b4-143">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-143">This topic uses the [xUnit](https://xunit.github.io/) testing framework.</span></span> <span data-ttu-id="ca3b4-144">測試概念與測試實作跨不同測試架構很類似的但不是完全相同。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-144">Testing concepts and test implementations across different testing frameworks are similar but not identical.</span></span>

<span data-ttu-id="ca3b4-145">雖然此應用程式不會使用[儲存機制模式](http://martinfowler.com/eaaCatalog/repository.html)並不是有效的範例[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 頁面支援這些模式開發。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-145">Although the app doesn't use the [repository pattern](http://martinfowler.com/eaaCatalog/repository.html) and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="ca3b4-146">如需詳細資訊，請參閱[設計基礎結構的持續性層級](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)， [ASP.NET MVC 應用程式中實作的儲存機制和工作單元模式](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)，和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（此範例會實作儲存機制模式）。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-146">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), and [Testing controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="ca3b4-147">測試應用程式的組織</span><span class="sxs-lookup"><span data-stu-id="ca3b4-147">Test app organization</span></span>

<span data-ttu-id="ca3b4-148">測試應用程式是主控台應用程式內*tests/RazorPagesTestingSample.Tests*資料夾：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-148">The test app is a console app inside the *tests/RazorPagesTestingSample.Tests* folder:</span></span>

| <span data-ttu-id="ca3b4-149">測試應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="ca3b4-149">Test app folder</span></span>    | <span data-ttu-id="ca3b4-150">描述</span><span class="sxs-lookup"><span data-stu-id="ca3b4-150">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="ca3b4-151">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="ca3b4-151">*IntegrationTests*</span></span> | <ul><li><span data-ttu-id="ca3b4-152">*IndexPageTest.cs*包含索引頁的整合測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-152">*IndexPageTest.cs* contains the integration tests for the Index page.</span></span></li><li><span data-ttu-id="ca3b4-153">*TestFixture.cs*建立測試訊息的應用程式的測試主機。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-153">*TestFixture.cs* creates the test host to test the message app.</span></span></li></ul> |
| <span data-ttu-id="ca3b4-154">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="ca3b4-154">*UnitTests*</span></span>        | <ul><li><span data-ttu-id="ca3b4-155">*DataAccessLayerTest.cs* DAL 包含單元測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-155">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="ca3b4-156">*IndexPageTest.cs*包含單元測試，索引頁面模型。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-156">*IndexPageTest.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="ca3b4-157">*公用程式*</span><span class="sxs-lookup"><span data-stu-id="ca3b4-157">*Utilities*</span></span>        | <span data-ttu-id="ca3b4-158">*Utilities.cs*包含:</span><span class="sxs-lookup"><span data-stu-id="ca3b4-158">*Utilities.cs* contains the:</span></span><ul><li><span data-ttu-id="ca3b4-159">`TestingDbContextOptions` 用來建立新的資料庫內容的每個 DAL 單元測試的選項，使資料庫重設為其基準條件，針對每個測試方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-159">`TestingDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span></li><li><span data-ttu-id="ca3b4-160">`GetRequestContentAsync` 方法可用來準備`HttpClient`和整合測試期間傳送至訊息的應用程式要求的內容。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-160">`GetRequestContentAsync` method used to prepare the `HttpClient` and content for requests that are sent to the message app during integration testing.</span></span></li></ul>

<span data-ttu-id="ca3b4-161">測試架構是[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-161">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="ca3b4-162">模擬架構的物件是[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-162">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span> <span data-ttu-id="ca3b4-163">搜尋整合測試都會使用[ASP.NET Core 測試主機](xref:testing/integration-testing#the-test-host)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-163">Integration tests are conducted using the [ASP.NET Core Test Host](xref:testing/integration-testing#the-test-host).</span></span>

## <a name="unit-testing-the-data-access-layer-dal"></a><span data-ttu-id="ca3b4-164">單元測試資料存取層 (DAL)</span><span class="sxs-lookup"><span data-stu-id="ca3b4-164">Unit testing the data access layer (DAL)</span></span>

<span data-ttu-id="ca3b4-165">訊息應用程式具有四種方法中所包含的 DAL`AppDbContext`類別 (*src/RazorPagesTestingSample/Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-165">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestingSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="ca3b4-166">每個方法都有一個或兩個單元測試中測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-166">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="ca3b4-167">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="ca3b4-167">DAL method</span></span>               | <span data-ttu-id="ca3b4-168">功能</span><span class="sxs-lookup"><span data-stu-id="ca3b4-168">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="ca3b4-169">取得`List<Message>`從依照資料庫`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-169">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="ca3b4-170">新增`Message`到資料庫。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-170">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="ca3b4-171">刪除所有`Message`資料庫的項目。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-171">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="ca3b4-172">刪除單一`Message`從資料庫`Id`。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-172">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="ca3b4-173">DAL 的單元測試需要[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)時建立新`AppDbContext`針對每個測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-173">Unit tests of the DAL require [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="ca3b4-174">若要建立的其中一個方法`DbContextOptions`每項測試是使用[DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-174">One approach to creating the `DbContextOptions` for each test is to use a [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="ca3b4-175">這個方法的問題是，每項測試會接收資料庫中任何狀態先前測試所保留。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-175">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="ca3b4-176">嘗試寫入，不會干擾彼此不可部分完成的單元測試時，這可能會造成問題。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-176">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="ca3b4-177">若要強制`AppDbContext`若要使用新的資料庫內容，針對每個測試，提供`DbContextOptions`根據新的服務提供者的執行個體。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-177">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="ca3b4-178">測試應用程式會顯示如何執行此動作使用其`Utilities`類別方法`TestingDbContextOptions`(*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-178">The test app shows how to do this using its `Utilities` class method `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="ca3b4-179">使用`DbContextOptions`DAL 單元測試可讓與全新資料庫的執行個體以不可分割方式執行每個測試：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-179">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="ca3b4-180">在每個測試方法`DataAccessLayerTest`類別 (*UnitTests/DataAccessLayerTest.cs*) 遵循類似的排列 Act Assert 模式：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-180">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="ca3b4-181">排列： 將資料庫設定測試和/或定義獲得預期的結果。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-181">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="ca3b4-182">Act： 執行測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-182">Act: The test is executed.</span></span>
1. <span data-ttu-id="ca3b4-183">判斷提示： 若要判斷是否成功的測試結果進行判斷提示。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-183">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="ca3b4-184">例如，`DeleteMessageAsync`方法負責移除單一訊息，由其`Id`(*src/RazorPagesTestingSample/Data/AppDbContext.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-184">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="ca3b4-185">有兩項測試，此方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-185">There are two tests for this method.</span></span> <span data-ttu-id="ca3b4-186">一項測試會檢查方法在資料庫中有訊息時，會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-186">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="ca3b4-187">如果，不會變更資料庫的其他方法測試訊息`Id`刪除不存在。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-187">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="ca3b4-188">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-188">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="ca3b4-189">首先，此方法會執行排列步驟中，準備進行的動作步驟執行所在。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-189">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="ca3b4-190">植入訊息會取得並保存在`seedMessages`。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-190">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="ca3b4-191">植入訊息儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-191">The seeding messages are saved into the database.</span></span> <span data-ttu-id="ca3b4-192">與訊息`Id`的`1`設定為刪除。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-192">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="ca3b4-193">當`DeleteMessageAsync`執行方法、 預期的郵件應具有所有除了有訊息`Id`的`1`。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-193">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="ca3b4-194">`expectedMessages`變數代表這個預期的結果。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-194">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="ca3b4-195">方法會：`DeleteMessageAsync`執行方法中傳遞`recId`的`1`:</span><span class="sxs-lookup"><span data-stu-id="ca3b4-195">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="ca3b4-196">最後，方法會取得`Messages`從內容，並比較它`expectedMessages`判斷提示兩個相等：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-196">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="ca3b4-197">若要比較的兩個`List<Message>`相同：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-197">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="ca3b4-198">訊息都會按照`Id`。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-198">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="ca3b4-199">訊息配對會比較上`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-199">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="ca3b4-200">類似的測試方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`檢查結果的嘗試刪除不存在的訊息。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-200">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="ca3b4-201">在此情況下，在資料庫中的預期的訊息應該會等於真正的訊息之後`DeleteMessageAsync`執行方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-201">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="ca3b4-202">應該不會變更資料庫的內容：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-202">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a><span data-ttu-id="ca3b4-203">單元測試的頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="ca3b4-203">Unit testing the page model methods</span></span>

<span data-ttu-id="ca3b4-204">單元測試的另一組負責測試頁面模型方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-204">Another set of unit tests is responsible for testing page model methods.</span></span> <span data-ttu-id="ca3b4-205">在中找到訊息應用程式時，索引頁面模型`IndexModel`類別*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-205">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="ca3b4-206">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="ca3b4-206">Page model method</span></span> | <span data-ttu-id="ca3b4-207">功能</span><span class="sxs-lookup"><span data-stu-id="ca3b4-207">Function</span></span> |
| ----------------- | -------- | 
| `OnGetAsync` | <span data-ttu-id="ca3b4-208">取得訊息的 UI 使用 DAL`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-208">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="ca3b4-209">如果`ModelState`有效，但呼叫`AddMessageAsync`將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-209">If the `ModelState` is valid, calls `AddMessageAsync` to add a message to the database.</span></span> | 
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="ca3b4-210">呼叫`DeleteAllMessagesAsync`刪除所有資料庫中的訊息。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-210">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="ca3b4-211">執行`DeleteMessageAsync`若要刪除的訊息`Id`指定。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-211">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="ca3b4-212">如果一或多個訊息是在資料庫中，會計算平均每個訊息的字組數目。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-212">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="ca3b4-213">使用七個測試中的測試網頁模型方法`IndexPageTest`類別 (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-213">The page model methods are tested using seven tests in the `IndexPageTest` class (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*).</span></span> <span data-ttu-id="ca3b4-214">測試可以使用熟悉的排列判斷提示動作模式。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-214">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="ca3b4-215">這些測試著重於：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-215">These tests focus on:</span></span>

* <span data-ttu-id="ca3b4-216">判斷方法是否遵循正確的行為時`ModelState`無效。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-216">Determining if the methods follow the correct behavior when the `ModelState` is invalid.</span></span>
* <span data-ttu-id="ca3b4-217">確認的方法產生正確`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-217">Confirming the methods produce the correct `IActionResult`.</span></span>
* <span data-ttu-id="ca3b4-218">檢查屬性值的設定正確進行。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-218">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="ca3b4-219">此群組的測試通常會模擬 DAL 以產生預期的資料頁面模型方法執行所在的動作步驟的方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-219">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="ca3b4-220">例如，`GetMessagesAsync`方法`AppDbContext`模仿產生輸出。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-220">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="ca3b4-221">當頁面模型方法執行這個方法時，模擬便會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-221">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="ca3b4-222">資料並不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-222">The data doesn't come from the database.</span></span> <span data-ttu-id="ca3b4-223">這會建立在頁面模型測試中使用 DAL 的可預測、 可靠的測試條件。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-223">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="ca3b4-224">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`測試顯示如何`GetMessagesAsync`方法會模仿頁面模型：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-224">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="ca3b4-225">當`OnGetAsync`方法執行的動作步驟中，它會呼叫頁面模型`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-225">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="ca3b4-226">單元測試的動作步驟 (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-226">Unit test Act step (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

<span data-ttu-id="ca3b4-227">`IndexPage` 頁面模型`OnGetAsync`方法 (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-227">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="ca3b4-228">`GetMessagesAsync` DAL 中的方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-228">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="ca3b4-229">方法的模擬的版本傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-229">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="ca3b4-230">在`Assert`步驟，實際的訊息 (`actualMessages`) 從指派的`Messages`頁面模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-230">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="ca3b4-231">指派給訊息也執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-231">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="ca3b4-232">預期和實際的訊息會比較其`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-232">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="ca3b4-233">測試判斷提示，這兩個`List<Message>`執行個體包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-233">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

<span data-ttu-id="ca3b4-234">此群組中的其他測試建立頁面包含模型物件`DefaultHttpContext`、 `ModelStateDictionary`、`ActionContext`建立`PageContext`、 `ViewDataDictionary`，和`PageContext`。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-234">Other tests in this group create page model objects that include the `DefaultHttpContext`, the `ModelStateDictionary`, an `ActionContext` to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="ca3b4-235">這些是適用於執行測試。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-235">These are useful in conducting tests.</span></span> <span data-ttu-id="ca3b4-236">比方說，訊息應用程式會建立`ModelState`錯誤`AddModelError`檢查有效`PageResult`時，會傳回`OnPostAddMessageAsync`執行：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-236">For example, the message app establishes a `ModelState` error with `AddModelError` to check that a valid `PageResult` is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a><span data-ttu-id="ca3b4-237">測試應用程式整合</span><span class="sxs-lookup"><span data-stu-id="ca3b4-237">Integration testing the app</span></span>

<span data-ttu-id="ca3b4-238">整合測試上測試應用程式的元件一起運作的焦點。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-238">The integration tests focus on testing that the app's components work together.</span></span> <span data-ttu-id="ca3b4-239">搜尋整合測試都會使用[ASP.NET Core 測試主機](xref:testing/integration-testing#the-test-host)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-239">Integration tests are conducted using the [ASP.NET Core Test Host](xref:testing/integration-testing#the-test-host).</span></span> <span data-ttu-id="ca3b4-240">這被測試完整的要求-回應生命週期的處理。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-240">Full request-response lifecycle processing is tested.</span></span> <span data-ttu-id="ca3b4-241">這些測試判斷提示的頁面會產生正確的狀態碼和`Location`標頭，如果設定。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-241">These tests assert that the page produces the correct status code and `Location` header, if set.</span></span>

<span data-ttu-id="ca3b4-242">整合測試的範例的範例會檢查要求的訊息應用程式的索引頁面的結果 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-242">An integration testing example from the sample checks the result of requesting the Index page of the message app (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

<span data-ttu-id="ca3b4-243">沒有任何排列步驟。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-243">There's no Arrange step.</span></span> <span data-ttu-id="ca3b4-244">`GetAsync`上呼叫方法`HttpClient`GET 要求傳送至端點。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-244">The `GetAsync` method is called on the `HttpClient` to send a GET request to the endpoint.</span></span> <span data-ttu-id="ca3b4-245">測試判斷提示，結果會是 200 OK 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-245">The test asserts that the result is a 200-OK status code.</span></span>

<span data-ttu-id="ca3b4-246">訊息應用程式的所有 POST 要求都必須都滿足的應用程式會自動成為 antiforgery 核取[資料保護 antiforgery 系統](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-246">Any POST request to the message app must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="ca3b4-247">若要排列測試的 POST 要求，必須測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-247">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="ca3b4-248">提出要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-248">Make a request for the page.</span></span>
1. <span data-ttu-id="ca3b4-249">剖析 antiforgery cookie 和來自回應的要求驗證語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-249">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="ca3b4-250">在位置進行 antiforgery cookie 和要求驗證的 POST 要求語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-250">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="ca3b4-251">`Post_AddMessageHandler_ReturnsRedirectToRoot`測試方法：</span><span class="sxs-lookup"><span data-stu-id="ca3b4-251">The `Post_AddMessageHandler_ReturnsRedirectToRoot` test method:</span></span>

* <span data-ttu-id="ca3b4-252">準備一個訊息和`HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-252">Prepares a message and the `HttpClient`.</span></span>
* <span data-ttu-id="ca3b4-253">對應用程式中的 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-253">Makes a POST request to the app.</span></span>
* <span data-ttu-id="ca3b4-254">檢查回應重新導向至索引頁面。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-254">Checks the response is a redirect back to the Index page.</span></span>

<span data-ttu-id="ca3b4-255">`Post_AddMessageHandler_ReturnsRedirectToRoot ` 方法 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-255">`Post_AddMessageHandler_ReturnsRedirectToRoot ` method (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

<span data-ttu-id="ca3b4-256">`GetRequestContentAsync`公用程式方法會管理準備好要求驗證語彙基元 antiforgery cookie 與用戶端。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-256">The `GetRequestContentAsync` utility method manages preparing the client with the antiforgery cookie and request verification token.</span></span> <span data-ttu-id="ca3b4-257">請注意此方法接收的方式`IDictionary`，允許呼叫測試方法，傳入要編碼以及要求驗證語彙基元要求的資料 (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-257">Note how the method receives an `IDictionary` that permits the calling test method to pass in data for the request to encode along with the request verification token (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

<span data-ttu-id="ca3b4-258">整合測試也可以將應用程式來測試應用程式的回應行為不正確的資料。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-258">Integration tests can also pass bad data to the app to test the app's response behavior.</span></span> <span data-ttu-id="ca3b4-259">訊息應用程式限制為 200 個字元的訊息長度 (*src/RazorPagesTestingSample/Data/Message.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-259">The message app limits message length to 200 characters (*src/RazorPagesTestingSample/Data/Message.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

<span data-ttu-id="ca3b4-260">`Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong`測試`Message`明確傳入 201"X"字元的文字。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-260">The `Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` test `Message` explicitly passes in text with 201 "X" characters.</span></span> <span data-ttu-id="ca3b4-261">這會導致`ModelState`錯誤。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-261">This results in a `ModelState` error.</span></span> <span data-ttu-id="ca3b4-262">Post 要求不會重新導向回索引頁面。</span><span class="sxs-lookup"><span data-stu-id="ca3b4-262">The POST doesn't redirect back to the Index page.</span></span> <span data-ttu-id="ca3b4-263">它會傳回具有 200 OK `null` `Location`標頭 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca3b4-263">It returns a 200-OK with a `null` `Location` header (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a><span data-ttu-id="ca3b4-264">另請參閱</span><span class="sxs-lookup"><span data-stu-id="ca3b4-264">See also</span></span>

* [<span data-ttu-id="ca3b4-265">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca3b4-265">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="ca3b4-266">整合測試</span><span class="sxs-lookup"><span data-stu-id="ca3b4-266">Integration testing</span></span>](xref:testing/integration-testing)
* [<span data-ttu-id="ca3b4-267">測試控制器</span><span class="sxs-lookup"><span data-stu-id="ca3b4-267">Testing controllers</span></span>](xref:mvc/controllers/testing)
* <span data-ttu-id="ca3b4-268">[單元測試程式碼](/visualstudio/test/unit-test-your-code)(Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="ca3b4-268">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* [<span data-ttu-id="ca3b4-269">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="ca3b4-269">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="ca3b4-270">開始使用 xUnit.net （.NET Core/ASP.NET 核心）</span><span class="sxs-lookup"><span data-stu-id="ca3b4-270">Getting started with xUnit.net (.NET Core/ASP.NET Core)</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="ca3b4-271">Moq</span><span class="sxs-lookup"><span data-stu-id="ca3b4-271">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="ca3b4-272">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="ca3b4-272">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
