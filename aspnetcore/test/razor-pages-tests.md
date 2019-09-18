---
title: 在 ASP.NET Core razor 頁面單元測試
author: guardrex
description: 瞭解如何建立 Razor Pages 應用程式的單元測試。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: afac97d686ef190ebb92d20a55a15dd774b0d1de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081431"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a><span data-ttu-id="e57ec-103">在 ASP.NET Core razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="e57ec-103">Razor Pages unit tests in ASP.NET Core</span></span>

<span data-ttu-id="e57ec-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e57ec-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e57ec-105">ASP.NET Core 支援 Razor 網頁應用程式的單元的測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-105">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="e57ec-106">資料存取層（DAL）和頁面模型的測試有助於確保：</span><span class="sxs-lookup"><span data-stu-id="e57ec-106">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="e57ec-107">Razor Pages 應用程式的元件會在應用程式建立期間獨立和一個單位一起工作。</span><span class="sxs-lookup"><span data-stu-id="e57ec-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="e57ec-108">類別和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="e57ec-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="e57ec-109">應用程式的行為方式有其他檔。</span><span class="sxs-lookup"><span data-stu-id="e57ec-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="e57ec-110">自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="e57ec-111">本主題假設您對 Razor Pages 應用程式和單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="e57ec-111">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="e57ec-112">如果您不熟悉 Razor Pages 應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="e57ec-112">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="e57ec-113">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="e57ec-113">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="e57ec-114">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e57ec-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e57ec-115">範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="e57ec-115">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="e57ec-116">App</span><span class="sxs-lookup"><span data-stu-id="e57ec-116">App</span></span>         | <span data-ttu-id="e57ec-117">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="e57ec-117">Project folder</span></span>                     | <span data-ttu-id="e57ec-118">描述</span><span class="sxs-lookup"><span data-stu-id="e57ec-118">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="e57ec-119">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="e57ec-119">Message app</span></span> | <span data-ttu-id="e57ec-120">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="e57ec-120">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="e57ec-121">可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-121">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="e57ec-122">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e57ec-122">Test app</span></span>    | <span data-ttu-id="e57ec-123">*tests/RazorPagesTestSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="e57ec-123">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="e57ec-124">用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-124">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="e57ec-125">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="e57ec-125">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="e57ec-126">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/RazorPagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e57ec-126">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="e57ec-127">訊息應用程式組織</span><span class="sxs-lookup"><span data-stu-id="e57ec-127">Message app organization</span></span>

<span data-ttu-id="e57ec-128">訊息應用程式是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="e57ec-128">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="e57ec-129">應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-129">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="e57ec-130">訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-130">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="e57ec-131">屬性`Text`是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="e57ec-131">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="e57ec-132">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。</span><span class="sxs-lookup"><span data-stu-id="e57ec-132">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="e57ec-133">應用程式在其資料庫內容類別`AppDbContext` （*Data/AppDbCoNtext .cs*）中包含 DAL。</span><span class="sxs-lookup"><span data-stu-id="e57ec-133">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="e57ec-134">DAL 方法已標記`virtual`，可讓您模擬要在測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="e57ec-134">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="e57ec-135">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="e57ec-135">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="e57ec-136">這些*植*入的訊息也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="e57ec-136">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="e57ec-137">&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-137">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="e57ec-138">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="e57ec-138">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="e57ec-139">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="e57ec-139">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="e57ec-140">雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="e57ec-140">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="e57ec-141">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和<xref:mvc/controllers/testing> （範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-141">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="e57ec-142">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="e57ec-142">Test app organization</span></span>

<span data-ttu-id="e57ec-143">測試應用程式是 [測試]/[ *RazorPagesTestSample* ] 資料夾內的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="e57ec-143">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="e57ec-144">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="e57ec-144">Test app folder</span></span> | <span data-ttu-id="e57ec-145">說明</span><span class="sxs-lookup"><span data-stu-id="e57ec-145">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="e57ec-146">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="e57ec-146">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="e57ec-147">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-147">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="e57ec-148">*IndexPageTests.cs*包含索引頁面模型的單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-148">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="e57ec-149">*多數*</span><span class="sxs-lookup"><span data-stu-id="e57ec-149">*Utilities*</span></span>     | <span data-ttu-id="e57ec-150">包含用來為每個 DAL 單元測試建立新資料庫內容選項的方法，以便將資料庫重設為每個測試的基準條件。`TestDbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="e57ec-150">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="e57ec-151">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="e57ec-151">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="e57ec-152">物件模擬架構為[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="e57ec-152">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="e57ec-153">資料存取層（DAL）的單元測試</span><span class="sxs-lookup"><span data-stu-id="e57ec-153">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="e57ec-154">訊息應用程式的 DAL 具有四個包含在`AppDbContext`類別中的方法（*src/RazorPagesTestSample/Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-154">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="e57ec-155">每個方法在測試應用程式中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-155">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="e57ec-156">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="e57ec-156">DAL method</span></span>               | <span data-ttu-id="e57ec-157">函數</span><span class="sxs-lookup"><span data-stu-id="e57ec-157">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="e57ec-158">`List<Message>` 從`Text`依屬性排序的資料庫取得。</span><span class="sxs-lookup"><span data-stu-id="e57ec-158">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="e57ec-159">將加入`Message`至資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-159">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="e57ec-160">刪除資料庫`Message`中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="e57ec-160">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="e57ec-161">`Message` 從`Id`資料庫刪除單一。</span><span class="sxs-lookup"><span data-stu-id="e57ec-161">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="e57ec-162">建立每個測試的新<xref:Microsoft.EntityFrameworkCore.DbContextOptions> `AppDbContext`時，DAL 的單元測試需要。</span><span class="sxs-lookup"><span data-stu-id="e57ec-162">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="e57ec-163">為每個測試建立`DbContextOptions`的其中一個方法是<xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>使用：</span><span class="sxs-lookup"><span data-stu-id="e57ec-163">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="e57ec-164">這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-164">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="e57ec-165">嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="e57ec-165">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="e57ec-166">若要強制`AppDbContext`將新的資料庫內容用於每個測試，請`DbContextOptions`提供以新服務提供者為基礎的實例。</span><span class="sxs-lookup"><span data-stu-id="e57ec-166">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="e57ec-167">測試應用程式會示範如何使用其`Utilities`類別方法`TestDbContextOptions` （測試/RazorPagesTestSample）來執行此動作 *。測試/公用程式/公用程式 .cs*）：</span><span class="sxs-lookup"><span data-stu-id="e57ec-167">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="e57ec-168">在 DAL `DbContextOptions`單元測試中使用，可讓每個測試以自動方式與全新的資料庫實例一起執行：</span><span class="sxs-lookup"><span data-stu-id="e57ec-168">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="e57ec-169">`DataAccessLayerTest`類別中的每個測試方法（*run-unittests/DataAccessLayerTest*）遵循類似的相片順序-判斷法模式：</span><span class="sxs-lookup"><span data-stu-id="e57ec-169">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="e57ec-170">–已針對測試設定資料庫，並（或）定義預期的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-170">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="e57ec-171">Act測試會執行。</span><span class="sxs-lookup"><span data-stu-id="e57ec-171">Act: The test is executed.</span></span>
1. <span data-ttu-id="e57ec-172">判斷提示判斷測試結果是否成功，會進行判斷提示。</span><span class="sxs-lookup"><span data-stu-id="e57ec-172">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="e57ec-173">例如， `DeleteMessageAsync`方法會負責移除其`Id` （*src/RazorPagesTestSample/Data/AppDbCoNtext*）所識別的單一訊息：</span><span class="sxs-lookup"><span data-stu-id="e57ec-173">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="e57ec-174">這個方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-174">There are two tests for this method.</span></span> <span data-ttu-id="e57ec-175">一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-175">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="e57ec-176">另一個方法會測試如果要刪除的訊息`Id`不存在，資料庫不會變更。</span><span class="sxs-lookup"><span data-stu-id="e57ec-176">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="e57ec-177">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="e57ec-177">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="e57ec-178">首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。</span><span class="sxs-lookup"><span data-stu-id="e57ec-178">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="e57ec-179">植入訊息會取得並保留在`seedMessages`中。</span><span class="sxs-lookup"><span data-stu-id="e57ec-179">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="e57ec-180">植入訊息會儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="e57ec-180">The seeding messages are saved into the database.</span></span> <span data-ttu-id="e57ec-181">具有`Id`之的`1`訊息已設定為要刪除。</span><span class="sxs-lookup"><span data-stu-id="e57ec-181">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="e57ec-182">當執行`Id` `1`方法時，預期的訊息應該會有所有訊息，但是除了具有的以外， `DeleteMessageAsync`</span><span class="sxs-lookup"><span data-stu-id="e57ec-182">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="e57ec-183">`expectedMessages`變數代表此預期的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-183">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="e57ec-184">方法的作用如下：會執行`recId` 方法，`1`並傳入的： `DeleteMessageAsync`</span><span class="sxs-lookup"><span data-stu-id="e57ec-184">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="e57ec-185">最後，方法`Messages`會從內容取得，並將它`expectedMessages`與判斷提示兩者相等的判斷提示：</span><span class="sxs-lookup"><span data-stu-id="e57ec-185">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="e57ec-186">若要比較兩者`List<Message>`相同：</span><span class="sxs-lookup"><span data-stu-id="e57ec-186">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="e57ec-187">訊息會依`Id`排序。</span><span class="sxs-lookup"><span data-stu-id="e57ec-187">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="e57ec-188">訊息配對會在`Text`屬性上進行比較。</span><span class="sxs-lookup"><span data-stu-id="e57ec-188">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="e57ec-189">類似的測試方法， `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`會檢查嘗試刪除不存在之訊息的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-189">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="e57ec-190">在此情況下，資料庫中預期的訊息應該等於執行`DeleteMessageAsync`方法之後的實際訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-190">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="e57ec-191">資料庫的內容應該不會有任何變更：</span><span class="sxs-lookup"><span data-stu-id="e57ec-191">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="e57ec-192">頁面模型方法的單元測試</span><span class="sxs-lookup"><span data-stu-id="e57ec-192">Unit tests of the page model methods</span></span>

<span data-ttu-id="e57ec-193">另一組單元測試負責測試頁面模型方法。</span><span class="sxs-lookup"><span data-stu-id="e57ec-193">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="e57ec-194">在訊息應用程式中，會在`IndexModel`類別中的*src/RazorPagesTestSample/Pages/index. cshtml*中找到索引頁面模型。</span><span class="sxs-lookup"><span data-stu-id="e57ec-194">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="e57ec-195">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="e57ec-195">Page model method</span></span> | <span data-ttu-id="e57ec-196">函數</span><span class="sxs-lookup"><span data-stu-id="e57ec-196">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="e57ec-197">使用`GetMessagesAsync`方法，從 DAL 取得適用于 UI 的訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-197">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="e57ec-198">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫`AddMessageAsync`以將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-198">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="e57ec-199">呼叫`DeleteAllMessagesAsync`以刪除資料庫中的所有訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-199">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="e57ec-200">執行`DeleteMessageAsync`以刪除`Id`具有指定之的訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-200">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="e57ec-201">如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。</span><span class="sxs-lookup"><span data-stu-id="e57ec-201">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="e57ec-202">頁面模型方法會使用類別中的七個測試`IndexPageTests` （[*測試]/[RazorPagesTestSample]/* [測試]/[run-unittests]/[IndexPageTests]）進行測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-202">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="e57ec-203">測試會使用熟悉的「排列-判斷提示-Act」模式。</span><span class="sxs-lookup"><span data-stu-id="e57ec-203">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="e57ec-204">這些測試著重于：</span><span class="sxs-lookup"><span data-stu-id="e57ec-204">These tests focus on:</span></span>

* <span data-ttu-id="e57ec-205">判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="e57ec-205">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="e57ec-206">確認方法會產生正確<xref:Microsoft.AspNetCore.Mvc.IActionResult>的。</span><span class="sxs-lookup"><span data-stu-id="e57ec-206">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="e57ec-207">檢查是否已正確設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="e57ec-207">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="e57ec-208">這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。</span><span class="sxs-lookup"><span data-stu-id="e57ec-208">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="e57ec-209">例如， `GetMessagesAsync`的方法`AppDbContext`會模擬以產生輸出。</span><span class="sxs-lookup"><span data-stu-id="e57ec-209">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="e57ec-210">當頁面模型方法執行這個方法時，mock 會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-210">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="e57ec-211">資料不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-211">The data doesn't come from the database.</span></span> <span data-ttu-id="e57ec-212">這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。</span><span class="sxs-lookup"><span data-stu-id="e57ec-212">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="e57ec-213">此`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`測試會顯示`GetMessagesAsync`如何模擬頁面模型的方法：</span><span class="sxs-lookup"><span data-stu-id="e57ec-213">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="e57ec-214">當方法在 Act 步驟中執行時，它會呼叫頁面模型的`GetMessagesAsync`方法。 `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="e57ec-214">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="e57ec-215">單元測試 Act 步驟（test */RazorPagesTestSample/run-unittests/IndexPageTests*）：</span><span class="sxs-lookup"><span data-stu-id="e57ec-215">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="e57ec-216">`IndexPage`頁面模型的`OnGetAsync`方法（*src/RazorPagesTestSample/Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="e57ec-216">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="e57ec-217">DAL `GetMessagesAsync`中的方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-217">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="e57ec-218">模擬版本的方法會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-218">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="e57ec-219">在此`Assert`步驟中，會從頁面`actualMessages`模型的`Messages`屬性指派實際的訊息（）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-219">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="e57ec-220">指派訊息時也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="e57ec-220">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="e57ec-221">預期和實際的訊息會依其`Text`屬性進行比較。</span><span class="sxs-lookup"><span data-stu-id="e57ec-221">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="e57ec-222">測試判斷提示兩個`List<Message>`實例包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-222">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="e57ec-223">此群組中的其他測試會建立頁面模型物件， <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>其中包含<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>、、 <xref:Microsoft.AspNetCore.Mvc.ActionContext> `PageContext`、， `PageContext`以建立、 `ViewDataDictionary`和。</span><span class="sxs-lookup"><span data-stu-id="e57ec-223">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="e57ec-224">這些適用于進行測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-224">These are useful in conducting tests.</span></span> <span data-ttu-id="e57ec-225">例如，訊息`ModelState`應用程式會與<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>建立錯誤，以檢查執行時`OnPostAddMessageAsync`傳回<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>的有效：</span><span class="sxs-lookup"><span data-stu-id="e57ec-225">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="e57ec-226">其他資源</span><span class="sxs-lookup"><span data-stu-id="e57ec-226">Additional resources</span></span>

* [<span data-ttu-id="e57ec-227">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="e57ec-227">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="e57ec-228">[對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）</span><span class="sxs-lookup"><span data-stu-id="e57ec-228">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="e57ec-229">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="e57ec-229">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="e57ec-230">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="e57ec-230">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="e57ec-231">XUnit.net 入門：搭配 .NET SDK 命令列使用 .NET Core</span><span class="sxs-lookup"><span data-stu-id="e57ec-231">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="e57ec-232">Moq</span><span class="sxs-lookup"><span data-stu-id="e57ec-232">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="e57ec-233">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="e57ec-233">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e57ec-234">ASP.NET Core 支援 Razor 網頁應用程式的單元的測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-234">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="e57ec-235">資料存取層（DAL）和頁面模型的測試有助於確保：</span><span class="sxs-lookup"><span data-stu-id="e57ec-235">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="e57ec-236">Razor Pages 應用程式的元件會在應用程式建立期間獨立和一個單位一起工作。</span><span class="sxs-lookup"><span data-stu-id="e57ec-236">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="e57ec-237">類別和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="e57ec-237">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="e57ec-238">應用程式的行為方式有其他檔。</span><span class="sxs-lookup"><span data-stu-id="e57ec-238">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="e57ec-239">自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-239">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="e57ec-240">本主題假設您對 Razor Pages 應用程式和單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="e57ec-240">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="e57ec-241">如果您不熟悉 Razor Pages 應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="e57ec-241">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="e57ec-242">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="e57ec-242">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="e57ec-243">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e57ec-243">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e57ec-244">範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="e57ec-244">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="e57ec-245">App</span><span class="sxs-lookup"><span data-stu-id="e57ec-245">App</span></span>         | <span data-ttu-id="e57ec-246">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="e57ec-246">Project folder</span></span>                     | <span data-ttu-id="e57ec-247">描述</span><span class="sxs-lookup"><span data-stu-id="e57ec-247">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="e57ec-248">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="e57ec-248">Message app</span></span> | <span data-ttu-id="e57ec-249">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="e57ec-249">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="e57ec-250">可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-250">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="e57ec-251">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e57ec-251">Test app</span></span>    | <span data-ttu-id="e57ec-252">*tests/RazorPagesTestSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="e57ec-252">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="e57ec-253">用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-253">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="e57ec-254">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="e57ec-254">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="e57ec-255">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/RazorPagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e57ec-255">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="e57ec-256">訊息應用程式組織</span><span class="sxs-lookup"><span data-stu-id="e57ec-256">Message app organization</span></span>

<span data-ttu-id="e57ec-257">訊息應用程式是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="e57ec-257">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="e57ec-258">應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-258">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="e57ec-259">訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-259">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="e57ec-260">屬性`Text`是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="e57ec-260">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="e57ec-261">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。</span><span class="sxs-lookup"><span data-stu-id="e57ec-261">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="e57ec-262">應用程式在其資料庫內容類別`AppDbContext` （*Data/AppDbCoNtext .cs*）中包含 DAL。</span><span class="sxs-lookup"><span data-stu-id="e57ec-262">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="e57ec-263">DAL 方法已標記`virtual`，可讓您模擬要在測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="e57ec-263">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="e57ec-264">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="e57ec-264">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="e57ec-265">這些*植*入的訊息也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="e57ec-265">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="e57ec-266">&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-266">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="e57ec-267">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="e57ec-267">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="e57ec-268">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="e57ec-268">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="e57ec-269">雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="e57ec-269">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="e57ec-270">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和<xref:mvc/controllers/testing> （範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-270">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="e57ec-271">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="e57ec-271">Test app organization</span></span>

<span data-ttu-id="e57ec-272">測試應用程式是 [測試]/[ *RazorPagesTestSample* ] 資料夾內的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="e57ec-272">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="e57ec-273">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="e57ec-273">Test app folder</span></span> | <span data-ttu-id="e57ec-274">描述</span><span class="sxs-lookup"><span data-stu-id="e57ec-274">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="e57ec-275">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="e57ec-275">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="e57ec-276">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-276">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="e57ec-277">*IndexPageTests.cs*包含索引頁面模型的單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-277">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="e57ec-278">*多數*</span><span class="sxs-lookup"><span data-stu-id="e57ec-278">*Utilities*</span></span>     | <span data-ttu-id="e57ec-279">包含用來為每個 DAL 單元測試建立新資料庫內容選項的方法，以便將資料庫重設為每個測試的基準條件。`TestDbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="e57ec-279">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="e57ec-280">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="e57ec-280">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="e57ec-281">物件模擬架構為[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="e57ec-281">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="e57ec-282">資料存取層（DAL）的單元測試</span><span class="sxs-lookup"><span data-stu-id="e57ec-282">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="e57ec-283">訊息應用程式的 DAL 具有四個包含在`AppDbContext`類別中的方法（*src/RazorPagesTestSample/Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-283">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="e57ec-284">每個方法在測試應用程式中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-284">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="e57ec-285">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="e57ec-285">DAL method</span></span>               | <span data-ttu-id="e57ec-286">函數</span><span class="sxs-lookup"><span data-stu-id="e57ec-286">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="e57ec-287">`List<Message>` 從`Text`依屬性排序的資料庫取得。</span><span class="sxs-lookup"><span data-stu-id="e57ec-287">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="e57ec-288">將加入`Message`至資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-288">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="e57ec-289">刪除資料庫`Message`中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="e57ec-289">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="e57ec-290">`Message` 從`Id`資料庫刪除單一。</span><span class="sxs-lookup"><span data-stu-id="e57ec-290">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="e57ec-291">建立每個測試的新<xref:Microsoft.EntityFrameworkCore.DbContextOptions> `AppDbContext`時，DAL 的單元測試需要。</span><span class="sxs-lookup"><span data-stu-id="e57ec-291">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="e57ec-292">為每個測試建立`DbContextOptions`的其中一個方法是<xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>使用：</span><span class="sxs-lookup"><span data-stu-id="e57ec-292">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="e57ec-293">這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-293">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="e57ec-294">嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="e57ec-294">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="e57ec-295">若要強制`AppDbContext`將新的資料庫內容用於每個測試，請`DbContextOptions`提供以新服務提供者為基礎的實例。</span><span class="sxs-lookup"><span data-stu-id="e57ec-295">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="e57ec-296">測試應用程式會示範如何使用其`Utilities`類別方法`TestDbContextOptions` （測試/RazorPagesTestSample）來執行此動作 *。測試/公用程式/公用程式 .cs*）：</span><span class="sxs-lookup"><span data-stu-id="e57ec-296">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="e57ec-297">在 DAL `DbContextOptions`單元測試中使用，可讓每個測試以自動方式與全新的資料庫實例一起執行：</span><span class="sxs-lookup"><span data-stu-id="e57ec-297">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="e57ec-298">`DataAccessLayerTest`類別中的每個測試方法（*run-unittests/DataAccessLayerTest*）遵循類似的相片順序-判斷法模式：</span><span class="sxs-lookup"><span data-stu-id="e57ec-298">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="e57ec-299">–已針對測試設定資料庫，並（或）定義預期的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-299">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="e57ec-300">Act測試會執行。</span><span class="sxs-lookup"><span data-stu-id="e57ec-300">Act: The test is executed.</span></span>
1. <span data-ttu-id="e57ec-301">判斷提示判斷測試結果是否成功，會進行判斷提示。</span><span class="sxs-lookup"><span data-stu-id="e57ec-301">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="e57ec-302">例如， `DeleteMessageAsync`方法會負責移除其`Id` （*src/RazorPagesTestSample/Data/AppDbCoNtext*）所識別的單一訊息：</span><span class="sxs-lookup"><span data-stu-id="e57ec-302">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="e57ec-303">這個方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-303">There are two tests for this method.</span></span> <span data-ttu-id="e57ec-304">一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-304">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="e57ec-305">另一個方法會測試如果要刪除的訊息`Id`不存在，資料庫不會變更。</span><span class="sxs-lookup"><span data-stu-id="e57ec-305">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="e57ec-306">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="e57ec-306">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="e57ec-307">首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。</span><span class="sxs-lookup"><span data-stu-id="e57ec-307">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="e57ec-308">植入訊息會取得並保留在`seedMessages`中。</span><span class="sxs-lookup"><span data-stu-id="e57ec-308">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="e57ec-309">植入訊息會儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="e57ec-309">The seeding messages are saved into the database.</span></span> <span data-ttu-id="e57ec-310">具有`Id`之的`1`訊息已設定為要刪除。</span><span class="sxs-lookup"><span data-stu-id="e57ec-310">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="e57ec-311">當執行`Id` `1`方法時，預期的訊息應該會有所有訊息，但是除了具有的以外， `DeleteMessageAsync`</span><span class="sxs-lookup"><span data-stu-id="e57ec-311">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="e57ec-312">`expectedMessages`變數代表此預期的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-312">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="e57ec-313">方法的作用如下：會執行`recId` 方法，`1`並傳入的： `DeleteMessageAsync`</span><span class="sxs-lookup"><span data-stu-id="e57ec-313">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="e57ec-314">最後，方法`Messages`會從內容取得，並將它`expectedMessages`與判斷提示兩者相等的判斷提示：</span><span class="sxs-lookup"><span data-stu-id="e57ec-314">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="e57ec-315">若要比較兩者`List<Message>`相同：</span><span class="sxs-lookup"><span data-stu-id="e57ec-315">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="e57ec-316">訊息會依`Id`排序。</span><span class="sxs-lookup"><span data-stu-id="e57ec-316">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="e57ec-317">訊息配對會在`Text`屬性上進行比較。</span><span class="sxs-lookup"><span data-stu-id="e57ec-317">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="e57ec-318">類似的測試方法， `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`會檢查嘗試刪除不存在之訊息的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-318">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="e57ec-319">在此情況下，資料庫中預期的訊息應該等於執行`DeleteMessageAsync`方法之後的實際訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-319">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="e57ec-320">資料庫的內容應該不會有任何變更：</span><span class="sxs-lookup"><span data-stu-id="e57ec-320">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="e57ec-321">頁面模型方法的單元測試</span><span class="sxs-lookup"><span data-stu-id="e57ec-321">Unit tests of the page model methods</span></span>

<span data-ttu-id="e57ec-322">另一組單元測試負責測試頁面模型方法。</span><span class="sxs-lookup"><span data-stu-id="e57ec-322">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="e57ec-323">在訊息應用程式中，會在`IndexModel`類別中的*src/RazorPagesTestSample/Pages/index. cshtml*中找到索引頁面模型。</span><span class="sxs-lookup"><span data-stu-id="e57ec-323">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="e57ec-324">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="e57ec-324">Page model method</span></span> | <span data-ttu-id="e57ec-325">函數</span><span class="sxs-lookup"><span data-stu-id="e57ec-325">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="e57ec-326">使用`GetMessagesAsync`方法，從 DAL 取得適用于 UI 的訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-326">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="e57ec-327">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫`AddMessageAsync`以將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-327">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="e57ec-328">呼叫`DeleteAllMessagesAsync`以刪除資料庫中的所有訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-328">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="e57ec-329">執行`DeleteMessageAsync`以刪除`Id`具有指定之的訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-329">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="e57ec-330">如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。</span><span class="sxs-lookup"><span data-stu-id="e57ec-330">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="e57ec-331">頁面模型方法會使用類別中的七個測試`IndexPageTests` （[*測試]/[RazorPagesTestSample]/* [測試]/[run-unittests]/[IndexPageTests]）進行測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-331">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="e57ec-332">測試會使用熟悉的「排列-判斷提示-Act」模式。</span><span class="sxs-lookup"><span data-stu-id="e57ec-332">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="e57ec-333">這些測試著重于：</span><span class="sxs-lookup"><span data-stu-id="e57ec-333">These tests focus on:</span></span>

* <span data-ttu-id="e57ec-334">判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="e57ec-334">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="e57ec-335">確認方法會產生正確<xref:Microsoft.AspNetCore.Mvc.IActionResult>的。</span><span class="sxs-lookup"><span data-stu-id="e57ec-335">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="e57ec-336">檢查是否已正確設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="e57ec-336">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="e57ec-337">這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。</span><span class="sxs-lookup"><span data-stu-id="e57ec-337">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="e57ec-338">例如， `GetMessagesAsync`的方法`AppDbContext`會模擬以產生輸出。</span><span class="sxs-lookup"><span data-stu-id="e57ec-338">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="e57ec-339">當頁面模型方法執行這個方法時，mock 會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-339">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="e57ec-340">資料不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="e57ec-340">The data doesn't come from the database.</span></span> <span data-ttu-id="e57ec-341">這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。</span><span class="sxs-lookup"><span data-stu-id="e57ec-341">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="e57ec-342">此`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`測試會顯示`GetMessagesAsync`如何模擬頁面模型的方法：</span><span class="sxs-lookup"><span data-stu-id="e57ec-342">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="e57ec-343">當方法在 Act 步驟中執行時，它會呼叫頁面模型的`GetMessagesAsync`方法。 `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="e57ec-343">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="e57ec-344">單元測試 Act 步驟（test */RazorPagesTestSample/run-unittests/IndexPageTests*）：</span><span class="sxs-lookup"><span data-stu-id="e57ec-344">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="e57ec-345">`IndexPage`頁面模型的`OnGetAsync`方法（*src/RazorPagesTestSample/Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="e57ec-345">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="e57ec-346">DAL `GetMessagesAsync`中的方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-346">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="e57ec-347">模擬版本的方法會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="e57ec-347">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="e57ec-348">在此`Assert`步驟中，會從頁面`actualMessages`模型的`Messages`屬性指派實際的訊息（）。</span><span class="sxs-lookup"><span data-stu-id="e57ec-348">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="e57ec-349">指派訊息時也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="e57ec-349">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="e57ec-350">預期和實際的訊息會依其`Text`屬性進行比較。</span><span class="sxs-lookup"><span data-stu-id="e57ec-350">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="e57ec-351">測試判斷提示兩個`List<Message>`實例包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="e57ec-351">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="e57ec-352">此群組中的其他測試會建立頁面模型物件， <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>其中包含<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>、、 <xref:Microsoft.AspNetCore.Mvc.ActionContext> `PageContext`、， `PageContext`以建立、 `ViewDataDictionary`和。</span><span class="sxs-lookup"><span data-stu-id="e57ec-352">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="e57ec-353">這些適用于進行測試。</span><span class="sxs-lookup"><span data-stu-id="e57ec-353">These are useful in conducting tests.</span></span> <span data-ttu-id="e57ec-354">例如，訊息`ModelState`應用程式會與<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>建立錯誤，以檢查執行時`OnPostAddMessageAsync`傳回<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>的有效：</span><span class="sxs-lookup"><span data-stu-id="e57ec-354">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="e57ec-355">其他資源</span><span class="sxs-lookup"><span data-stu-id="e57ec-355">Additional resources</span></span>

* [<span data-ttu-id="e57ec-356">單元測試 C# 中使用 dotnet 測試和 xUnit.NET Core</span><span class="sxs-lookup"><span data-stu-id="e57ec-356">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="e57ec-357">[對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）</span><span class="sxs-lookup"><span data-stu-id="e57ec-357">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="e57ec-358">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="e57ec-358">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="e57ec-359">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="e57ec-359">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="e57ec-360">XUnit.net 入門：搭配 .NET SDK 命令列使用 .NET Core</span><span class="sxs-lookup"><span data-stu-id="e57ec-360">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="e57ec-361">Moq</span><span class="sxs-lookup"><span data-stu-id="e57ec-361">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="e57ec-362">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="e57ec-362">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
