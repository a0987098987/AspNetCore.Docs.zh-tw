---
title: Razor Pages ASP.NET Core 中的單元測試
author: rick-anderson
description: 瞭解如何建立 Razor Pages 應用程式的單元測試。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: 0e217b6b7f15519a3da44f5d074cf80fa96a3b3a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664406"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a><span data-ttu-id="1354b-103">Razor Pages ASP.NET Core 中的單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-103">Razor Pages unit tests in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1354b-104">ASP.NET Core 支援 Razor Pages 應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-104">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="1354b-105">資料存取層（DAL）和頁面模型的測試有助於確保：</span><span class="sxs-lookup"><span data-stu-id="1354b-105">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="1354b-106">Razor Pages 應用程式的元件會在應用程式建立期間獨立和一個單位一起工作。</span><span class="sxs-lookup"><span data-stu-id="1354b-106">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="1354b-107">類別和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="1354b-107">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="1354b-108">應用程式的行為方式有其他檔。</span><span class="sxs-lookup"><span data-stu-id="1354b-108">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="1354b-109">自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。</span><span class="sxs-lookup"><span data-stu-id="1354b-109">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="1354b-110">本主題假設您對 Razor Pages 應用程式和單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="1354b-110">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="1354b-111">如果您不熟悉 Razor Pages 應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="1354b-111">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="1354b-112">使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-112">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="1354b-113">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1354b-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1354b-114">範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="1354b-114">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="1354b-115">App</span><span class="sxs-lookup"><span data-stu-id="1354b-115">App</span></span>         | <span data-ttu-id="1354b-116">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="1354b-116">Project folder</span></span>                     | <span data-ttu-id="1354b-117">描述</span><span class="sxs-lookup"><span data-stu-id="1354b-117">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="1354b-118">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="1354b-118">Message app</span></span> | <span data-ttu-id="1354b-119">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="1354b-119">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="1354b-120">可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="1354b-120">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="1354b-121">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="1354b-121">Test app</span></span>    | <span data-ttu-id="1354b-122">*測試/RazorPagesTestSample。測試*</span><span class="sxs-lookup"><span data-stu-id="1354b-122">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="1354b-123">用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-123">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="1354b-124">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="1354b-124">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="1354b-125">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/RazorPagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="1354b-125">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="1354b-126">訊息應用程式組織</span><span class="sxs-lookup"><span data-stu-id="1354b-126">Message app organization</span></span>

<span data-ttu-id="1354b-127">訊息應用程式是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="1354b-127">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="1354b-128">應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="1354b-128">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="1354b-129">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （索引鍵）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="1354b-129">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="1354b-130">`Text` 屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="1354b-130">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="1354b-131">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。</span><span class="sxs-lookup"><span data-stu-id="1354b-131">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="1354b-132">應用程式在其資料庫內容類別中包含 DAL，`AppDbContext` （*Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="1354b-132">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="1354b-133">DAL 方法會標示 `virtual`，這可讓您模擬要在測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="1354b-133">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="1354b-134">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="1354b-134">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="1354b-135">這些*植*入的訊息也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="1354b-135">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="1354b-136">&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-136">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="1354b-137">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="1354b-137">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="1354b-138">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="1354b-138">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="1354b-139">雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="1354b-139">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="1354b-140">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和 <xref:mvc/controllers/testing> （範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="1354b-140">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="1354b-141">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="1354b-141">Test app organization</span></span>

<span data-ttu-id="1354b-142">測試應用程式是 [測試]/[ *RazorPagesTestSample* ] 資料夾內的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="1354b-142">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="1354b-143">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="1354b-143">Test app folder</span></span> | <span data-ttu-id="1354b-144">描述</span><span class="sxs-lookup"><span data-stu-id="1354b-144">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="1354b-145">*Run-unittests*</span><span class="sxs-lookup"><span data-stu-id="1354b-145">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="1354b-146">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-146">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="1354b-147">*IndexPageTests.cs*包含索引頁面模型的單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-147">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="1354b-148">*公用程式*</span><span class="sxs-lookup"><span data-stu-id="1354b-148">*Utilities*</span></span>     | <span data-ttu-id="1354b-149">包含用來為每個 DAL 單元測試建立新資料庫內容選項的 `TestDbContextOptions` 方法，以便將資料庫重設為每個測試的基準條件。</span><span class="sxs-lookup"><span data-stu-id="1354b-149">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="1354b-150">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="1354b-150">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="1354b-151">物件模擬架構為[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="1354b-151">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="1354b-152">資料存取層（DAL）的單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-152">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="1354b-153">訊息應用程式的 DAL 具有四個包含在 `AppDbContext` 類別中的方法（*src/RazorPagesTestSample/Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="1354b-153">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="1354b-154">每個方法在測試應用程式中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-154">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="1354b-155">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="1354b-155">DAL method</span></span>               | <span data-ttu-id="1354b-156">函式</span><span class="sxs-lookup"><span data-stu-id="1354b-156">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="1354b-157">從以 `Text` 屬性排序的資料庫取得 `List<Message>`。</span><span class="sxs-lookup"><span data-stu-id="1354b-157">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="1354b-158">將 `Message` 加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-158">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="1354b-159">刪除資料庫中的所有 `Message` 專案。</span><span class="sxs-lookup"><span data-stu-id="1354b-159">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="1354b-160">藉由 `Id`，從資料庫中刪除單一 `Message`。</span><span class="sxs-lookup"><span data-stu-id="1354b-160">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="1354b-161">為每個測試建立新的 `AppDbContext` 時，DAL 的單元測試需要 <xref:Microsoft.EntityFrameworkCore.DbContextOptions>。</span><span class="sxs-lookup"><span data-stu-id="1354b-161">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="1354b-162">建立每個測試 `DbContextOptions` 的一種方法是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>：</span><span class="sxs-lookup"><span data-stu-id="1354b-162">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="1354b-163">這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-163">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="1354b-164">嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="1354b-164">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="1354b-165">若要強制 `AppDbContext` 針對每項測試使用新的資料庫內容，請提供以新服務提供者為基礎的 `DbContextOptions` 實例。</span><span class="sxs-lookup"><span data-stu-id="1354b-165">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="1354b-166">測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` （*測試/RazorPagesTestSample 測試/公用程式/公用程式 .cs*）來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="1354b-166">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="1354b-167">使用 DAL 單元測試中的 `DbContextOptions`，可讓每個測試以自動方式與全新的資料庫實例一起執行：</span><span class="sxs-lookup"><span data-stu-id="1354b-167">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="1354b-168">`DataAccessLayerTest` 類別（*run-unittests/DataAccessLayerTest*）中的每個測試方法都遵循類似的相片順序-判斷法模式：</span><span class="sxs-lookup"><span data-stu-id="1354b-168">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="1354b-169">排列：已針對測試設定資料庫，並（或）定義預期的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-169">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="1354b-170">Act：執行測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-170">Act: The test is executed.</span></span>
1. <span data-ttu-id="1354b-171">判斷提示：決定測試結果是否成功的判斷提示。</span><span class="sxs-lookup"><span data-stu-id="1354b-171">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="1354b-172">例如，`DeleteMessageAsync` 方法會負責移除其 `Id` 所識別的單一訊息（*src/RazorPagesTestSample/Data/AppDbCoNtext*）：</span><span class="sxs-lookup"><span data-stu-id="1354b-172">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="1354b-173">這個方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-173">There are two tests for this method.</span></span> <span data-ttu-id="1354b-174">一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-174">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="1354b-175">另一個方法則是測試如果訊息 `Id` 不存在時，資料庫不會變更。</span><span class="sxs-lookup"><span data-stu-id="1354b-175">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="1354b-176">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` 方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="1354b-176">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="1354b-177">首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。</span><span class="sxs-lookup"><span data-stu-id="1354b-177">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="1354b-178">植入訊息會取得並保留在 `seedMessages`中。</span><span class="sxs-lookup"><span data-stu-id="1354b-178">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="1354b-179">植入訊息會儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="1354b-179">The seeding messages are saved into the database.</span></span> <span data-ttu-id="1354b-180">具有 `1` `Id` 的訊息已設定為要刪除。</span><span class="sxs-lookup"><span data-stu-id="1354b-180">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="1354b-181">執行 `DeleteMessageAsync` 方法時，預期的訊息應該會有所有訊息，但是除了具有 `Id` 的 `1`以外。</span><span class="sxs-lookup"><span data-stu-id="1354b-181">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="1354b-182">`expectedMessages` 變數代表此預期的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-182">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="1354b-183">方法的作用： `DeleteMessageAsync` 方法會在 `recId` 的 `1`中執行：</span><span class="sxs-lookup"><span data-stu-id="1354b-183">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="1354b-184">最後，方法會從內容取得 `Messages`，並將其與 `expectedMessages` 判斷提示兩者相等：</span><span class="sxs-lookup"><span data-stu-id="1354b-184">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="1354b-185">為了比較這兩個 `List<Message>` 是相同的：</span><span class="sxs-lookup"><span data-stu-id="1354b-185">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="1354b-186">訊息會依 `Id`排序。</span><span class="sxs-lookup"><span data-stu-id="1354b-186">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="1354b-187">訊息配對會在 `Text` 屬性上進行比較。</span><span class="sxs-lookup"><span data-stu-id="1354b-187">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="1354b-188">類似的測試方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 檢查嘗試刪除不存在之訊息的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-188">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="1354b-189">在此情況下，在執行 `DeleteMessageAsync` 方法之後，資料庫中預期的訊息應該等於實際的訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-189">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="1354b-190">資料庫的內容應該不會有任何變更：</span><span class="sxs-lookup"><span data-stu-id="1354b-190">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="1354b-191">頁面模型方法的單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-191">Unit tests of the page model methods</span></span>

<span data-ttu-id="1354b-192">另一組單元測試負責測試頁面模型方法。</span><span class="sxs-lookup"><span data-stu-id="1354b-192">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="1354b-193">在訊息應用程式中，索引頁面模型位於*src/RazorPagesTestSample/Pages/Index. cshtml .cs*的 `IndexModel` 類別中。</span><span class="sxs-lookup"><span data-stu-id="1354b-193">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="1354b-194">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="1354b-194">Page model method</span></span> | <span data-ttu-id="1354b-195">函式</span><span class="sxs-lookup"><span data-stu-id="1354b-195">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="1354b-196">使用 `GetMessagesAsync` 方法，從 DAL 取得適用于 UI 的訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-196">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="1354b-197">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-197">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="1354b-198">呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-198">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="1354b-199">執行 `DeleteMessageAsync`，以使用指定的 `Id` 刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-199">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="1354b-200">如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。</span><span class="sxs-lookup"><span data-stu-id="1354b-200">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="1354b-201">頁面模型方法會使用 `IndexPageTests` 類別中的七個測試進行測試（[*測試]/[RazorPagesTestSample]/[run-unittests]/[IndexPageTests*]）。</span><span class="sxs-lookup"><span data-stu-id="1354b-201">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="1354b-202">測試會使用熟悉的「排列-判斷提示-Act」模式。</span><span class="sxs-lookup"><span data-stu-id="1354b-202">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="1354b-203">這些測試著重于：</span><span class="sxs-lookup"><span data-stu-id="1354b-203">These tests focus on:</span></span>

* <span data-ttu-id="1354b-204">判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="1354b-204">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="1354b-205">確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult>。</span><span class="sxs-lookup"><span data-stu-id="1354b-205">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="1354b-206">檢查是否已正確設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="1354b-206">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="1354b-207">這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。</span><span class="sxs-lookup"><span data-stu-id="1354b-207">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="1354b-208">例如，`AppDbContext` 的 `GetMessagesAsync` 方法是模擬來產生輸出。</span><span class="sxs-lookup"><span data-stu-id="1354b-208">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="1354b-209">當頁面模型方法執行這個方法時，mock 會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-209">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="1354b-210">資料不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-210">The data doesn't come from the database.</span></span> <span data-ttu-id="1354b-211">這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。</span><span class="sxs-lookup"><span data-stu-id="1354b-211">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="1354b-212">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何模擬頁面模型的 `GetMessagesAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="1354b-212">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="1354b-213">當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="1354b-213">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="1354b-214">單元測試 Act 步驟（test */RazorPagesTestSample/run-unittests/IndexPageTests*）：</span><span class="sxs-lookup"><span data-stu-id="1354b-214">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="1354b-215">`IndexPage` 頁面模型的 `OnGetAsync` 方法（*src/RazorPagesTestSample/Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="1354b-215">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="1354b-216">DAL 中的 `GetMessagesAsync` 方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-216">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="1354b-217">模擬版本的方法會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-217">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="1354b-218">在 `Assert` 步驟中，會從頁面模型的 `Messages` 屬性指派實際的訊息（`actualMessages`）。</span><span class="sxs-lookup"><span data-stu-id="1354b-218">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="1354b-219">指派訊息時也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="1354b-219">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="1354b-220">預期和實際的訊息會依其 `Text` 屬性進行比較。</span><span class="sxs-lookup"><span data-stu-id="1354b-220">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="1354b-221">測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-221">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="1354b-222">此群組中的其他測試會建立頁面模型物件，其中包括 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>、<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>、建立 `PageContext`的 <xref:Microsoft.AspNetCore.Mvc.ActionContext>、`ViewDataDictionary`和 `PageContext`。</span><span class="sxs-lookup"><span data-stu-id="1354b-222">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="1354b-223">這些適用于進行測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-223">These are useful in conducting tests.</span></span> <span data-ttu-id="1354b-224">例如，訊息應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 建立 `ModelState` 錯誤，以確認執行 `OnPostAddMessageAsync` 時傳回的是有效的 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>：</span><span class="sxs-lookup"><span data-stu-id="1354b-224">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="1354b-225">其他資源</span><span class="sxs-lookup"><span data-stu-id="1354b-225">Additional resources</span></span>

* [<span data-ttu-id="1354b-226">使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-226">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="1354b-227">[對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）</span><span class="sxs-lookup"><span data-stu-id="1354b-227">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="1354b-228">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="1354b-228">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="1354b-229">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="1354b-229">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="1354b-230">開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core</span><span class="sxs-lookup"><span data-stu-id="1354b-230">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="1354b-231">Moq</span><span class="sxs-lookup"><span data-stu-id="1354b-231">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="1354b-232">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="1354b-232">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1354b-233">ASP.NET Core 支援 Razor Pages 應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-233">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="1354b-234">資料存取層（DAL）和頁面模型的測試有助於確保：</span><span class="sxs-lookup"><span data-stu-id="1354b-234">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="1354b-235">Razor Pages 應用程式的元件會在應用程式建立期間獨立和一個單位一起工作。</span><span class="sxs-lookup"><span data-stu-id="1354b-235">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="1354b-236">類別和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="1354b-236">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="1354b-237">應用程式的行為方式有其他檔。</span><span class="sxs-lookup"><span data-stu-id="1354b-237">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="1354b-238">自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。</span><span class="sxs-lookup"><span data-stu-id="1354b-238">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="1354b-239">本主題假設您對 Razor Pages 應用程式和單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="1354b-239">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="1354b-240">如果您不熟悉 Razor Pages 應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="1354b-240">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="1354b-241">使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-241">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="1354b-242">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1354b-242">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1354b-243">範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="1354b-243">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="1354b-244">App</span><span class="sxs-lookup"><span data-stu-id="1354b-244">App</span></span>         | <span data-ttu-id="1354b-245">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="1354b-245">Project folder</span></span>                     | <span data-ttu-id="1354b-246">描述</span><span class="sxs-lookup"><span data-stu-id="1354b-246">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="1354b-247">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="1354b-247">Message app</span></span> | <span data-ttu-id="1354b-248">*src/RazorPagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="1354b-248">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="1354b-249">可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="1354b-249">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="1354b-250">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="1354b-250">Test app</span></span>    | <span data-ttu-id="1354b-251">*測試/RazorPagesTestSample。測試*</span><span class="sxs-lookup"><span data-stu-id="1354b-251">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="1354b-252">用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-252">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="1354b-253">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="1354b-253">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="1354b-254">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/RazorPagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="1354b-254">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="1354b-255">訊息應用程式組織</span><span class="sxs-lookup"><span data-stu-id="1354b-255">Message app organization</span></span>

<span data-ttu-id="1354b-256">訊息應用程式是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="1354b-256">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="1354b-257">應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="1354b-257">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="1354b-258">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （索引鍵）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="1354b-258">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="1354b-259">`Text` 屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="1354b-259">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="1354b-260">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。</span><span class="sxs-lookup"><span data-stu-id="1354b-260">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="1354b-261">應用程式在其資料庫內容類別中包含 DAL，`AppDbContext` （*Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="1354b-261">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="1354b-262">DAL 方法會標示 `virtual`，這可讓您模擬要在測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="1354b-262">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="1354b-263">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="1354b-263">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="1354b-264">這些*植*入的訊息也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="1354b-264">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="1354b-265">&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-265">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="1354b-266">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="1354b-266">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="1354b-267">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="1354b-267">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="1354b-268">雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="1354b-268">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="1354b-269">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和 <xref:mvc/controllers/testing> （範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="1354b-269">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="1354b-270">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="1354b-270">Test app organization</span></span>

<span data-ttu-id="1354b-271">測試應用程式是 [測試]/[ *RazorPagesTestSample* ] 資料夾內的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="1354b-271">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="1354b-272">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="1354b-272">Test app folder</span></span> | <span data-ttu-id="1354b-273">描述</span><span class="sxs-lookup"><span data-stu-id="1354b-273">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="1354b-274">*Run-unittests*</span><span class="sxs-lookup"><span data-stu-id="1354b-274">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="1354b-275">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-275">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="1354b-276">*IndexPageTests.cs*包含索引頁面模型的單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-276">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="1354b-277">*公用程式*</span><span class="sxs-lookup"><span data-stu-id="1354b-277">*Utilities*</span></span>     | <span data-ttu-id="1354b-278">包含用來為每個 DAL 單元測試建立新資料庫內容選項的 `TestDbContextOptions` 方法，以便將資料庫重設為每個測試的基準條件。</span><span class="sxs-lookup"><span data-stu-id="1354b-278">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="1354b-279">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="1354b-279">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="1354b-280">物件模擬架構為[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="1354b-280">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="1354b-281">資料存取層（DAL）的單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-281">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="1354b-282">訊息應用程式的 DAL 具有四個包含在 `AppDbContext` 類別中的方法（*src/RazorPagesTestSample/Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="1354b-282">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="1354b-283">每個方法在測試應用程式中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-283">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="1354b-284">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="1354b-284">DAL method</span></span>               | <span data-ttu-id="1354b-285">函式</span><span class="sxs-lookup"><span data-stu-id="1354b-285">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="1354b-286">從以 `Text` 屬性排序的資料庫取得 `List<Message>`。</span><span class="sxs-lookup"><span data-stu-id="1354b-286">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="1354b-287">將 `Message` 加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-287">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="1354b-288">刪除資料庫中的所有 `Message` 專案。</span><span class="sxs-lookup"><span data-stu-id="1354b-288">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="1354b-289">藉由 `Id`，從資料庫中刪除單一 `Message`。</span><span class="sxs-lookup"><span data-stu-id="1354b-289">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="1354b-290">為每個測試建立新的 `AppDbContext` 時，DAL 的單元測試需要 <xref:Microsoft.EntityFrameworkCore.DbContextOptions>。</span><span class="sxs-lookup"><span data-stu-id="1354b-290">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="1354b-291">建立每個測試 `DbContextOptions` 的一種方法是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>：</span><span class="sxs-lookup"><span data-stu-id="1354b-291">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="1354b-292">這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-292">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="1354b-293">嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="1354b-293">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="1354b-294">若要強制 `AppDbContext` 針對每項測試使用新的資料庫內容，請提供以新服務提供者為基礎的 `DbContextOptions` 實例。</span><span class="sxs-lookup"><span data-stu-id="1354b-294">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="1354b-295">測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` （*測試/RazorPagesTestSample 測試/公用程式/公用程式 .cs*）來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="1354b-295">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="1354b-296">使用 DAL 單元測試中的 `DbContextOptions`，可讓每個測試以自動方式與全新的資料庫實例一起執行：</span><span class="sxs-lookup"><span data-stu-id="1354b-296">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="1354b-297">`DataAccessLayerTest` 類別（*run-unittests/DataAccessLayerTest*）中的每個測試方法都遵循類似的相片順序-判斷法模式：</span><span class="sxs-lookup"><span data-stu-id="1354b-297">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="1354b-298">排列：已針對測試設定資料庫，並（或）定義預期的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-298">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="1354b-299">Act：執行測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-299">Act: The test is executed.</span></span>
1. <span data-ttu-id="1354b-300">判斷提示：決定測試結果是否成功的判斷提示。</span><span class="sxs-lookup"><span data-stu-id="1354b-300">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="1354b-301">例如，`DeleteMessageAsync` 方法會負責移除其 `Id` 所識別的單一訊息（*src/RazorPagesTestSample/Data/AppDbCoNtext*）：</span><span class="sxs-lookup"><span data-stu-id="1354b-301">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="1354b-302">這個方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-302">There are two tests for this method.</span></span> <span data-ttu-id="1354b-303">一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-303">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="1354b-304">另一個方法則是測試如果訊息 `Id` 不存在時，資料庫不會變更。</span><span class="sxs-lookup"><span data-stu-id="1354b-304">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="1354b-305">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` 方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="1354b-305">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="1354b-306">首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。</span><span class="sxs-lookup"><span data-stu-id="1354b-306">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="1354b-307">植入訊息會取得並保留在 `seedMessages`中。</span><span class="sxs-lookup"><span data-stu-id="1354b-307">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="1354b-308">植入訊息會儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="1354b-308">The seeding messages are saved into the database.</span></span> <span data-ttu-id="1354b-309">具有 `1` `Id` 的訊息已設定為要刪除。</span><span class="sxs-lookup"><span data-stu-id="1354b-309">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="1354b-310">執行 `DeleteMessageAsync` 方法時，預期的訊息應該會有所有訊息，但是除了具有 `Id` 的 `1`以外。</span><span class="sxs-lookup"><span data-stu-id="1354b-310">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="1354b-311">`expectedMessages` 變數代表此預期的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-311">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="1354b-312">方法的作用： `DeleteMessageAsync` 方法會在 `recId` 的 `1`中執行：</span><span class="sxs-lookup"><span data-stu-id="1354b-312">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="1354b-313">最後，方法會從內容取得 `Messages`，並將其與 `expectedMessages` 判斷提示兩者相等：</span><span class="sxs-lookup"><span data-stu-id="1354b-313">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="1354b-314">為了比較這兩個 `List<Message>` 是相同的：</span><span class="sxs-lookup"><span data-stu-id="1354b-314">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="1354b-315">訊息會依 `Id`排序。</span><span class="sxs-lookup"><span data-stu-id="1354b-315">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="1354b-316">訊息配對會在 `Text` 屬性上進行比較。</span><span class="sxs-lookup"><span data-stu-id="1354b-316">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="1354b-317">類似的測試方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 檢查嘗試刪除不存在之訊息的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-317">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="1354b-318">在此情況下，在執行 `DeleteMessageAsync` 方法之後，資料庫中預期的訊息應該等於實際的訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-318">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="1354b-319">資料庫的內容應該不會有任何變更：</span><span class="sxs-lookup"><span data-stu-id="1354b-319">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="1354b-320">頁面模型方法的單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-320">Unit tests of the page model methods</span></span>

<span data-ttu-id="1354b-321">另一組單元測試負責測試頁面模型方法。</span><span class="sxs-lookup"><span data-stu-id="1354b-321">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="1354b-322">在訊息應用程式中，索引頁面模型位於*src/RazorPagesTestSample/Pages/Index. cshtml .cs*的 `IndexModel` 類別中。</span><span class="sxs-lookup"><span data-stu-id="1354b-322">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="1354b-323">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="1354b-323">Page model method</span></span> | <span data-ttu-id="1354b-324">函式</span><span class="sxs-lookup"><span data-stu-id="1354b-324">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="1354b-325">使用 `GetMessagesAsync` 方法，從 DAL 取得適用于 UI 的訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-325">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="1354b-326">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-326">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="1354b-327">呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-327">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="1354b-328">執行 `DeleteMessageAsync`，以使用指定的 `Id` 刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-328">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="1354b-329">如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。</span><span class="sxs-lookup"><span data-stu-id="1354b-329">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="1354b-330">頁面模型方法會使用 `IndexPageTests` 類別中的七個測試進行測試（[*測試]/[RazorPagesTestSample]/[run-unittests]/[IndexPageTests*]）。</span><span class="sxs-lookup"><span data-stu-id="1354b-330">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="1354b-331">測試會使用熟悉的「排列-判斷提示-Act」模式。</span><span class="sxs-lookup"><span data-stu-id="1354b-331">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="1354b-332">這些測試著重于：</span><span class="sxs-lookup"><span data-stu-id="1354b-332">These tests focus on:</span></span>

* <span data-ttu-id="1354b-333">判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="1354b-333">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="1354b-334">確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult>。</span><span class="sxs-lookup"><span data-stu-id="1354b-334">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="1354b-335">檢查是否已正確設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="1354b-335">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="1354b-336">這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。</span><span class="sxs-lookup"><span data-stu-id="1354b-336">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="1354b-337">例如，`AppDbContext` 的 `GetMessagesAsync` 方法是模擬來產生輸出。</span><span class="sxs-lookup"><span data-stu-id="1354b-337">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="1354b-338">當頁面模型方法執行這個方法時，mock 會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-338">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="1354b-339">資料不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="1354b-339">The data doesn't come from the database.</span></span> <span data-ttu-id="1354b-340">這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。</span><span class="sxs-lookup"><span data-stu-id="1354b-340">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="1354b-341">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何模擬頁面模型的 `GetMessagesAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="1354b-341">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="1354b-342">當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="1354b-342">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="1354b-343">單元測試 Act 步驟（test */RazorPagesTestSample/run-unittests/IndexPageTests*）：</span><span class="sxs-lookup"><span data-stu-id="1354b-343">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="1354b-344">`IndexPage` 頁面模型的 `OnGetAsync` 方法（*src/RazorPagesTestSample/Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="1354b-344">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="1354b-345">DAL 中的 `GetMessagesAsync` 方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-345">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="1354b-346">模擬版本的方法會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="1354b-346">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="1354b-347">在 `Assert` 步驟中，會從頁面模型的 `Messages` 屬性指派實際的訊息（`actualMessages`）。</span><span class="sxs-lookup"><span data-stu-id="1354b-347">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="1354b-348">指派訊息時也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="1354b-348">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="1354b-349">預期和實際的訊息會依其 `Text` 屬性進行比較。</span><span class="sxs-lookup"><span data-stu-id="1354b-349">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="1354b-350">測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="1354b-350">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="1354b-351">此群組中的其他測試會建立頁面模型物件，其中包括 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>、<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>、建立 `PageContext`的 <xref:Microsoft.AspNetCore.Mvc.ActionContext>、`ViewDataDictionary`和 `PageContext`。</span><span class="sxs-lookup"><span data-stu-id="1354b-351">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="1354b-352">這些適用于進行測試。</span><span class="sxs-lookup"><span data-stu-id="1354b-352">These are useful in conducting tests.</span></span> <span data-ttu-id="1354b-353">例如，訊息應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 建立 `ModelState` 錯誤，以確認執行 `OnPostAddMessageAsync` 時傳回的是有效的 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>：</span><span class="sxs-lookup"><span data-stu-id="1354b-353">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="1354b-354">其他資源</span><span class="sxs-lookup"><span data-stu-id="1354b-354">Additional resources</span></span>

* [<span data-ttu-id="1354b-355">使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試</span><span class="sxs-lookup"><span data-stu-id="1354b-355">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="1354b-356">[對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）</span><span class="sxs-lookup"><span data-stu-id="1354b-356">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="1354b-357">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="1354b-357">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="1354b-358">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="1354b-358">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="1354b-359">開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core</span><span class="sxs-lookup"><span data-stu-id="1354b-359">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="1354b-360">Moq</span><span class="sxs-lookup"><span data-stu-id="1354b-360">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="1354b-361">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="1354b-361">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
