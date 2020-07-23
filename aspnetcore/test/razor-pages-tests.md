---
title: RazorASP.NET Core 中的頁面單元測試
author: rick-anderson
description: 瞭解如何建立 Razor 頁面應用程式的單元測試。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 7/22/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/razor-pages-tests
ms.openlocfilehash: ed048d600b629335b8267b63b3cfd57b525d608e
ms.sourcegitcommit: c86b4e2955dc1724f2eaa7c97894ad8b3bf763fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/22/2020
ms.locfileid: "86949090"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor<span data-ttu-id="12470-103">ASP.NET Core 中的頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-103"> Pages unit tests in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="12470-104">ASP.NET Core 支援 Razor 頁面應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-104">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="12470-105">資料存取層（DAL）和頁面模型的測試有助於確保：</span><span class="sxs-lookup"><span data-stu-id="12470-105">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="12470-106">頁面應用程式的各個部分會 Razor 在應用程式建立期間獨立和一個單位一起工作。</span><span class="sxs-lookup"><span data-stu-id="12470-106">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="12470-107">類別和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="12470-107">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="12470-108">應用程式的行為方式有其他檔。</span><span class="sxs-lookup"><span data-stu-id="12470-108">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="12470-109">自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。</span><span class="sxs-lookup"><span data-stu-id="12470-109">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="12470-110">本主題假設您對 Razor 頁面應用程式和單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="12470-110">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="12470-111">如果您不熟悉 Razor 頁面應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="12470-111">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="12470-112">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-112">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="12470-113">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="12470-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="12470-114">範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="12470-114">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="12470-115">App</span><span class="sxs-lookup"><span data-stu-id="12470-115">App</span></span>         | <span data-ttu-id="12470-116">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="12470-116">Project folder</span></span>                     | <span data-ttu-id="12470-117">描述</span><span class="sxs-lookup"><span data-stu-id="12470-117">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="12470-118">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="12470-118">Message app</span></span> | <span data-ttu-id="12470-119">*src/ Razor PagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="12470-119">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="12470-120">可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="12470-120">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="12470-121">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="12470-121">Test app</span></span>    | <span data-ttu-id="12470-122">*測試/ Razor PagesTestSample。測試*</span><span class="sxs-lookup"><span data-stu-id="12470-122">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="12470-123">用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-123">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="12470-124">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="12470-124">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="12470-125">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/ Razor PagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="12470-125">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="12470-126">訊息應用程式組織</span><span class="sxs-lookup"><span data-stu-id="12470-126">Message app organization</span></span>

<span data-ttu-id="12470-127">訊息應用程式是 Razor 具有下列特性的頁面訊息系統：</span><span class="sxs-lookup"><span data-stu-id="12470-127">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="12470-128">應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="12470-128">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="12470-129">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="12470-129">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="12470-130">`Text`屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="12470-130">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="12470-131">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="12470-131">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="12470-132">應用程式在其資料庫內容類別 `AppDbContext` （*Data/AppDbCoNtext .cs*）中包含 DAL。</span><span class="sxs-lookup"><span data-stu-id="12470-132">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="12470-133">DAL 方法已標記 `virtual` ，可讓您模擬要在測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="12470-133">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="12470-134">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="12470-134">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="12470-135">這些*植*入的訊息也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="12470-135">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="12470-136">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="12470-136">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="12470-137">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="12470-137">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="12470-138">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="12470-138">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="12470-139">雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，但 Razor 頁面支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="12470-139">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="12470-140">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和（範例會執行存放 <xref:mvc/controllers/testing> 庫模式）。</span><span class="sxs-lookup"><span data-stu-id="12470-140">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="12470-141">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="12470-141">Test app organization</span></span>

<span data-ttu-id="12470-142">測試應用程式是 [測試]/[ \* Razor PagesTestSample\* ] 資料夾內的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="12470-142">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="12470-143">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="12470-143">Test app folder</span></span> | <span data-ttu-id="12470-144">描述</span><span class="sxs-lookup"><span data-stu-id="12470-144">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="12470-145">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="12470-145">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="12470-146">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-146">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="12470-147">*IndexPageTests.cs*包含索引頁面模型的單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-147">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="12470-148">*公用程式*</span><span class="sxs-lookup"><span data-stu-id="12470-148">*Utilities*</span></span>     | <span data-ttu-id="12470-149">包含 `TestDbContextOptions` 用來為每個 DAL 單元測試建立新資料庫內容選項的方法，以便將資料庫重設為每個測試的基準條件。</span><span class="sxs-lookup"><span data-stu-id="12470-149">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="12470-150">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="12470-150">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="12470-151">物件模擬架構為[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="12470-151">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="12470-152">資料存取層（DAL）的單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-152">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="12470-153">訊息應用程式的 DAL 具有四個包含在類別中的方法 `AppDbContext` （*src/ Razor PagesTestSample/Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="12470-153">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="12470-154">每個方法在測試應用程式中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-154">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="12470-155">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="12470-155">DAL method</span></span>               | <span data-ttu-id="12470-156">函式</span><span class="sxs-lookup"><span data-stu-id="12470-156">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="12470-157">`List<Message>`從依屬性排序的資料庫取得 `Text` 。</span><span class="sxs-lookup"><span data-stu-id="12470-157">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="12470-158">將加入 `Message` 至資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-158">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="12470-159">刪除 `Message` 資料庫中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="12470-159">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="12470-160">`Message`從資料庫刪除單一 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-160">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="12470-161"><xref:Microsoft.EntityFrameworkCore.DbContextOptions>建立每個測試的新時，DAL 的單元測試需要 `AppDbContext` 。</span><span class="sxs-lookup"><span data-stu-id="12470-161">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="12470-162">為每個測試建立的其中一個方法 `DbContextOptions` 是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder> ：</span><span class="sxs-lookup"><span data-stu-id="12470-162">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="12470-163">這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-163">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="12470-164">嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="12470-164">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="12470-165">若要強制將 `AppDbContext` 新的資料庫內容用於每個測試，請提供以 `DbContextOptions` 新服務提供者為基礎的實例。</span><span class="sxs-lookup"><span data-stu-id="12470-165">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="12470-166">測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` （測試/PagesTestSample）來執行此動作\* Razor 。測試/公用程式/公用程式 .cs\*）：</span><span class="sxs-lookup"><span data-stu-id="12470-166">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="12470-167">`DbContextOptions`在 DAL 單元測試中使用，可讓每個測試以自動方式與全新的資料庫實例一起執行：</span><span class="sxs-lookup"><span data-stu-id="12470-167">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="12470-168">類別中的每個測試方法 `DataAccessLayerTest` （*Run-unittests/DataAccessLayerTest*）遵循類似的相片順序-判斷法模式：</span><span class="sxs-lookup"><span data-stu-id="12470-168">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="12470-169">排列：已針對測試設定資料庫，並（或）定義預期的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-169">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="12470-170">Act：執行測試。</span><span class="sxs-lookup"><span data-stu-id="12470-170">Act: The test is executed.</span></span>
1. <span data-ttu-id="12470-171">判斷提示：決定測試結果是否成功的判斷提示。</span><span class="sxs-lookup"><span data-stu-id="12470-171">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="12470-172">例如， `DeleteMessageAsync` 方法會負責移除其 `Id` （*src/ Razor PagesTestSample/Data/AppDbCoNtext*）所識別的單一訊息：</span><span class="sxs-lookup"><span data-stu-id="12470-172">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="12470-173">這個方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="12470-173">There are two tests for this method.</span></span> <span data-ttu-id="12470-174">一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="12470-174">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="12470-175">另一個方法會測試如果要刪除的訊息不存在，資料庫不會變更 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-175">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="12470-176">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="12470-176">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="12470-177">首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。</span><span class="sxs-lookup"><span data-stu-id="12470-177">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="12470-178">植入訊息會取得並保留在中 `seedMessages` 。</span><span class="sxs-lookup"><span data-stu-id="12470-178">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="12470-179">植入訊息會儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="12470-179">The seeding messages are saved into the database.</span></span> <span data-ttu-id="12470-180">具有 `Id` 之的訊息 `1` 已設定為要刪除。</span><span class="sxs-lookup"><span data-stu-id="12470-180">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="12470-181">當 `DeleteMessageAsync` 執行方法時，預期的訊息應該會有所有訊息，但是除了具有的以外， `Id` `1`</span><span class="sxs-lookup"><span data-stu-id="12470-181">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="12470-182">`expectedMessages`變數代表此預期的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-182">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="12470-183">方法的作用： `DeleteMessageAsync` 方法會以傳遞的來執行 `recId` `1` ：</span><span class="sxs-lookup"><span data-stu-id="12470-183">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="12470-184">最後，方法 `Messages` 會從內容取得，並將它與判斷提示 `expectedMessages` 兩者相等的判斷提示：</span><span class="sxs-lookup"><span data-stu-id="12470-184">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="12470-185">若要比較兩者 `List<Message>` 相同：</span><span class="sxs-lookup"><span data-stu-id="12470-185">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="12470-186">訊息會依排序 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-186">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="12470-187">訊息配對會在屬性上進行比較 `Text` 。</span><span class="sxs-lookup"><span data-stu-id="12470-187">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="12470-188">類似的測試方法， `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 會檢查嘗試刪除不存在之訊息的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-188">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="12470-189">在此情況下，資料庫中預期的訊息應該等於執行方法之後的實際訊息 `DeleteMessageAsync` 。</span><span class="sxs-lookup"><span data-stu-id="12470-189">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="12470-190">資料庫的內容應該不會有任何變更：</span><span class="sxs-lookup"><span data-stu-id="12470-190">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="12470-191">頁面模型方法的單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-191">Unit tests of the page model methods</span></span>

<span data-ttu-id="12470-192">另一組單元測試負責測試頁面模型方法。</span><span class="sxs-lookup"><span data-stu-id="12470-192">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="12470-193">在訊息應用程式中，會在 `IndexModel` 類別中的*src/ Razor PagesTestSample/Pages/index. Cshtml*中找到索引頁面模型。</span><span class="sxs-lookup"><span data-stu-id="12470-193">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="12470-194">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="12470-194">Page model method</span></span> | <span data-ttu-id="12470-195">函式</span><span class="sxs-lookup"><span data-stu-id="12470-195">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="12470-196">使用方法，從 DAL 取得適用于 UI 的訊息 `GetMessagesAsync` 。</span><span class="sxs-lookup"><span data-stu-id="12470-196">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="12470-197">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 以將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-197">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="12470-198">呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。</span><span class="sxs-lookup"><span data-stu-id="12470-198">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="12470-199">執行 `DeleteMessageAsync` 以刪除具有指定之的訊息 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-199">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="12470-200">如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。</span><span class="sxs-lookup"><span data-stu-id="12470-200">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="12470-201">頁面模型方法會使用類別中的七個測試（[測試]/[PagesTestSample]/[測試] `IndexPageTests` /[run-unittests]/[\* Razor IndexPageTests\*]）進行測試。</span><span class="sxs-lookup"><span data-stu-id="12470-201">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="12470-202">測試會使用熟悉的「排列-判斷提示-Act」模式。</span><span class="sxs-lookup"><span data-stu-id="12470-202">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="12470-203">這些測試著重于：</span><span class="sxs-lookup"><span data-stu-id="12470-203">These tests focus on:</span></span>

* <span data-ttu-id="12470-204">判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="12470-204">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="12470-205">確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult> 。</span><span class="sxs-lookup"><span data-stu-id="12470-205">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="12470-206">檢查是否已正確設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="12470-206">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="12470-207">這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。</span><span class="sxs-lookup"><span data-stu-id="12470-207">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="12470-208">例如，的 `GetMessagesAsync` 方法 `AppDbContext` 會模擬以產生輸出。</span><span class="sxs-lookup"><span data-stu-id="12470-208">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="12470-209">當頁面模型方法執行這個方法時，mock 會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="12470-209">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="12470-210">資料不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-210">The data doesn't come from the database.</span></span> <span data-ttu-id="12470-211">這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。</span><span class="sxs-lookup"><span data-stu-id="12470-211">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="12470-212">此 `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何 `GetMessagesAsync` 模擬頁面模型的方法：</span><span class="sxs-lookup"><span data-stu-id="12470-212">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="12470-213">當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="12470-213">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="12470-214">單元測試 Act 步驟（test */ Razor PagesTestSample/Run-unittests/IndexPageTests*）：</span><span class="sxs-lookup"><span data-stu-id="12470-214">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="12470-215">`IndexPage`頁面模型的 `OnGetAsync` 方法（*src/ Razor PagesTestSample/Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="12470-215">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="12470-216">`GetMessagesAsync`DAL 中的方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-216">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="12470-217">模擬版本的方法會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="12470-217">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="12470-218">在此 `Assert` 步驟中， `actualMessages` 會從頁面模型的屬性指派實際的訊息（） `Messages` 。</span><span class="sxs-lookup"><span data-stu-id="12470-218">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="12470-219">指派訊息時也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="12470-219">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="12470-220">預期和實際的訊息會依其屬性進行比較 `Text` 。</span><span class="sxs-lookup"><span data-stu-id="12470-220">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="12470-221">測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="12470-221">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="12470-222">此群組中的其他測試會建立頁面模型物件，其中包含 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext> 、、 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary> 、， <xref:Microsoft.AspNetCore.Mvc.ActionContext> 以建立 `PageContext` 、 `ViewDataDictionary` 和 `PageContext` 。</span><span class="sxs-lookup"><span data-stu-id="12470-222">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="12470-223">這些適用于進行測試。</span><span class="sxs-lookup"><span data-stu-id="12470-223">These are useful in conducting tests.</span></span> <span data-ttu-id="12470-224">例如，訊息應用程式 `ModelState` 會與建立錯誤， <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 以檢查 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> 執行時傳回的有效 `OnPostAddMessageAsync` ：</span><span class="sxs-lookup"><span data-stu-id="12470-224">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="12470-225">其他資源</span><span class="sxs-lookup"><span data-stu-id="12470-225">Additional resources</span></span>

* [<span data-ttu-id="12470-226">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-226">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="12470-227">[對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）</span><span class="sxs-lookup"><span data-stu-id="12470-227">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="12470-228">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="12470-228">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="12470-229">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="12470-229">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="12470-230">開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core</span><span class="sxs-lookup"><span data-stu-id="12470-230">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="12470-231">Moq</span><span class="sxs-lookup"><span data-stu-id="12470-231">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="12470-232">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="12470-232">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="12470-233">ASP.NET Core 支援 Razor 頁面應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-233">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="12470-234">資料存取層（DAL）和頁面模型的測試有助於確保：</span><span class="sxs-lookup"><span data-stu-id="12470-234">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="12470-235">頁面應用程式的各個部分會 Razor 在應用程式建立期間獨立和一個單位一起工作。</span><span class="sxs-lookup"><span data-stu-id="12470-235">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="12470-236">類別和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="12470-236">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="12470-237">應用程式的行為方式有其他檔。</span><span class="sxs-lookup"><span data-stu-id="12470-237">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="12470-238">自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。</span><span class="sxs-lookup"><span data-stu-id="12470-238">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="12470-239">本主題假設您對 Razor 頁面應用程式和單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="12470-239">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="12470-240">如果您不熟悉 Razor 頁面應用程式或測試概念，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="12470-240">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="12470-241">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-241">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="12470-242">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="12470-242">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="12470-243">範例專案是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="12470-243">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="12470-244">App</span><span class="sxs-lookup"><span data-stu-id="12470-244">App</span></span>         | <span data-ttu-id="12470-245">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="12470-245">Project folder</span></span>                     | <span data-ttu-id="12470-246">描述</span><span class="sxs-lookup"><span data-stu-id="12470-246">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="12470-247">訊息應用程式</span><span class="sxs-lookup"><span data-stu-id="12470-247">Message app</span></span> | <span data-ttu-id="12470-248">*src/ Razor PagesTestSample*</span><span class="sxs-lookup"><span data-stu-id="12470-248">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="12470-249">可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="12470-249">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="12470-250">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="12470-250">Test app</span></span>    | <span data-ttu-id="12470-251">*測試/ Razor PagesTestSample。測試*</span><span class="sxs-lookup"><span data-stu-id="12470-251">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="12470-252">用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-252">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="12470-253">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="12470-253">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="12470-254">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/ Razor PagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="12470-254">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="12470-255">訊息應用程式組織</span><span class="sxs-lookup"><span data-stu-id="12470-255">Message app organization</span></span>

<span data-ttu-id="12470-256">訊息應用程式是 Razor 具有下列特性的頁面訊息系統：</span><span class="sxs-lookup"><span data-stu-id="12470-256">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="12470-257">應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。</span><span class="sxs-lookup"><span data-stu-id="12470-257">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="12470-258">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="12470-258">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="12470-259">`Text`屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="12470-259">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="12470-260">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="12470-260">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="12470-261">應用程式在其資料庫內容類別 `AppDbContext` （*Data/AppDbCoNtext .cs*）中包含 DAL。</span><span class="sxs-lookup"><span data-stu-id="12470-261">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="12470-262">DAL 方法已標記 `virtual` ，可讓您模擬要在測試中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="12470-262">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="12470-263">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="12470-263">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="12470-264">這些*植*入的訊息也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="12470-264">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="12470-265">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="12470-265">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="12470-266">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="12470-266">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="12470-267">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="12470-267">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="12470-268">雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，但 Razor 頁面支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="12470-268">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="12470-269">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和（範例會執行存放 <xref:mvc/controllers/testing> 庫模式）。</span><span class="sxs-lookup"><span data-stu-id="12470-269">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="12470-270">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="12470-270">Test app organization</span></span>

<span data-ttu-id="12470-271">測試應用程式是 [測試]/[ \* Razor PagesTestSample\* ] 資料夾內的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="12470-271">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="12470-272">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="12470-272">Test app folder</span></span> | <span data-ttu-id="12470-273">描述</span><span class="sxs-lookup"><span data-stu-id="12470-273">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="12470-274">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="12470-274">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="12470-275">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-275">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="12470-276">*IndexPageTests.cs*包含索引頁面模型的單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-276">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="12470-277">*公用程式*</span><span class="sxs-lookup"><span data-stu-id="12470-277">*Utilities*</span></span>     | <span data-ttu-id="12470-278">包含 `TestDbContextOptions` 用來為每個 DAL 單元測試建立新資料庫內容選項的方法，以便將資料庫重設為每個測試的基準條件。</span><span class="sxs-lookup"><span data-stu-id="12470-278">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="12470-279">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="12470-279">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="12470-280">物件模擬架構為[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="12470-280">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="12470-281">資料存取層（DAL）的單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-281">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="12470-282">訊息應用程式的 DAL 具有四個包含在類別中的方法 `AppDbContext` （*src/ Razor PagesTestSample/Data/AppDbCoNtext .cs*）。</span><span class="sxs-lookup"><span data-stu-id="12470-282">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="12470-283">每個方法在測試應用程式中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="12470-283">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="12470-284">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="12470-284">DAL method</span></span>               | <span data-ttu-id="12470-285">函式</span><span class="sxs-lookup"><span data-stu-id="12470-285">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="12470-286">`List<Message>`從依屬性排序的資料庫取得 `Text` 。</span><span class="sxs-lookup"><span data-stu-id="12470-286">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="12470-287">將加入 `Message` 至資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-287">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="12470-288">刪除 `Message` 資料庫中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="12470-288">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="12470-289">`Message`從資料庫刪除單一 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-289">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="12470-290"><xref:Microsoft.EntityFrameworkCore.DbContextOptions>建立每個測試的新時，DAL 的單元測試需要 `AppDbContext` 。</span><span class="sxs-lookup"><span data-stu-id="12470-290">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="12470-291">為每個測試建立的其中一個方法 `DbContextOptions` 是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder> ：</span><span class="sxs-lookup"><span data-stu-id="12470-291">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="12470-292">這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-292">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="12470-293">嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="12470-293">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="12470-294">若要強制將 `AppDbContext` 新的資料庫內容用於每個測試，請提供以 `DbContextOptions` 新服務提供者為基礎的實例。</span><span class="sxs-lookup"><span data-stu-id="12470-294">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="12470-295">測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` （測試/PagesTestSample）來執行此動作\* Razor 。測試/公用程式/公用程式 .cs\*）：</span><span class="sxs-lookup"><span data-stu-id="12470-295">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="12470-296">`DbContextOptions`在 DAL 單元測試中使用，可讓每個測試以自動方式與全新的資料庫實例一起執行：</span><span class="sxs-lookup"><span data-stu-id="12470-296">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="12470-297">類別中的每個測試方法 `DataAccessLayerTest` （*Run-unittests/DataAccessLayerTest*）遵循類似的相片順序-判斷法模式：</span><span class="sxs-lookup"><span data-stu-id="12470-297">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="12470-298">排列：已針對測試設定資料庫，並（或）定義預期的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-298">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="12470-299">Act：執行測試。</span><span class="sxs-lookup"><span data-stu-id="12470-299">Act: The test is executed.</span></span>
1. <span data-ttu-id="12470-300">判斷提示：決定測試結果是否成功的判斷提示。</span><span class="sxs-lookup"><span data-stu-id="12470-300">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="12470-301">例如， `DeleteMessageAsync` 方法會負責移除其 `Id` （*src/ Razor PagesTestSample/Data/AppDbCoNtext*）所識別的單一訊息：</span><span class="sxs-lookup"><span data-stu-id="12470-301">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="12470-302">這個方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="12470-302">There are two tests for this method.</span></span> <span data-ttu-id="12470-303">一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。</span><span class="sxs-lookup"><span data-stu-id="12470-303">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="12470-304">另一個方法會測試如果要刪除的訊息不存在，資料庫不會變更 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-304">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="12470-305">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="12470-305">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="12470-306">首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。</span><span class="sxs-lookup"><span data-stu-id="12470-306">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="12470-307">植入訊息會取得並保留在中 `seedMessages` 。</span><span class="sxs-lookup"><span data-stu-id="12470-307">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="12470-308">植入訊息會儲存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="12470-308">The seeding messages are saved into the database.</span></span> <span data-ttu-id="12470-309">具有 `Id` 之的訊息 `1` 已設定為要刪除。</span><span class="sxs-lookup"><span data-stu-id="12470-309">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="12470-310">當 `DeleteMessageAsync` 執行方法時，預期的訊息應該會有所有訊息，但是除了具有的以外， `Id` `1`</span><span class="sxs-lookup"><span data-stu-id="12470-310">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="12470-311">`expectedMessages`變數代表此預期的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-311">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="12470-312">方法的作用： `DeleteMessageAsync` 方法會以傳遞的來執行 `recId` `1` ：</span><span class="sxs-lookup"><span data-stu-id="12470-312">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="12470-313">最後，方法 `Messages` 會從內容取得，並將它與判斷提示 `expectedMessages` 兩者相等的判斷提示：</span><span class="sxs-lookup"><span data-stu-id="12470-313">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="12470-314">若要比較兩者 `List<Message>` 相同：</span><span class="sxs-lookup"><span data-stu-id="12470-314">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="12470-315">訊息會依排序 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-315">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="12470-316">訊息配對會在屬性上進行比較 `Text` 。</span><span class="sxs-lookup"><span data-stu-id="12470-316">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="12470-317">類似的測試方法， `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 會檢查嘗試刪除不存在之訊息的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-317">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="12470-318">在此情況下，資料庫中預期的訊息應該等於執行方法之後的實際訊息 `DeleteMessageAsync` 。</span><span class="sxs-lookup"><span data-stu-id="12470-318">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="12470-319">資料庫的內容應該不會有任何變更：</span><span class="sxs-lookup"><span data-stu-id="12470-319">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="12470-320">頁面模型方法的單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-320">Unit tests of the page model methods</span></span>

<span data-ttu-id="12470-321">另一組單元測試負責測試頁面模型方法。</span><span class="sxs-lookup"><span data-stu-id="12470-321">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="12470-322">在訊息應用程式中，會在 `IndexModel` 類別中的*src/ Razor PagesTestSample/Pages/index. Cshtml*中找到索引頁面模型。</span><span class="sxs-lookup"><span data-stu-id="12470-322">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="12470-323">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="12470-323">Page model method</span></span> | <span data-ttu-id="12470-324">函式</span><span class="sxs-lookup"><span data-stu-id="12470-324">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="12470-325">使用方法，從 DAL 取得適用于 UI 的訊息 `GetMessagesAsync` 。</span><span class="sxs-lookup"><span data-stu-id="12470-325">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="12470-326">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 以將訊息新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-326">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="12470-327">呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。</span><span class="sxs-lookup"><span data-stu-id="12470-327">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="12470-328">執行 `DeleteMessageAsync` 以刪除具有指定之的訊息 `Id` 。</span><span class="sxs-lookup"><span data-stu-id="12470-328">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="12470-329">如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。</span><span class="sxs-lookup"><span data-stu-id="12470-329">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="12470-330">頁面模型方法會使用類別中的七個測試（[測試]/[PagesTestSample]/[測試] `IndexPageTests` /[run-unittests]/[\* Razor IndexPageTests\*]）進行測試。</span><span class="sxs-lookup"><span data-stu-id="12470-330">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="12470-331">測試會使用熟悉的「排列-判斷提示-Act」模式。</span><span class="sxs-lookup"><span data-stu-id="12470-331">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="12470-332">這些測試著重于：</span><span class="sxs-lookup"><span data-stu-id="12470-332">These tests focus on:</span></span>

* <span data-ttu-id="12470-333">判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="12470-333">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="12470-334">確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult> 。</span><span class="sxs-lookup"><span data-stu-id="12470-334">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="12470-335">檢查是否已正確設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="12470-335">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="12470-336">這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。</span><span class="sxs-lookup"><span data-stu-id="12470-336">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="12470-337">例如，的 `GetMessagesAsync` 方法 `AppDbContext` 會模擬以產生輸出。</span><span class="sxs-lookup"><span data-stu-id="12470-337">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="12470-338">當頁面模型方法執行這個方法時，mock 會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="12470-338">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="12470-339">資料不是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="12470-339">The data doesn't come from the database.</span></span> <span data-ttu-id="12470-340">這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。</span><span class="sxs-lookup"><span data-stu-id="12470-340">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="12470-341">此 `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何 `GetMessagesAsync` 模擬頁面模型的方法：</span><span class="sxs-lookup"><span data-stu-id="12470-341">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="12470-342">當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="12470-342">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="12470-343">單元測試 Act 步驟（test */ Razor PagesTestSample/Run-unittests/IndexPageTests*）：</span><span class="sxs-lookup"><span data-stu-id="12470-343">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="12470-344">`IndexPage`頁面模型的 `OnGetAsync` 方法（*src/ Razor PagesTestSample/Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="12470-344">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="12470-345">`GetMessagesAsync`DAL 中的方法不會傳回這個方法呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="12470-345">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="12470-346">模擬版本的方法會傳回結果。</span><span class="sxs-lookup"><span data-stu-id="12470-346">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="12470-347">在此 `Assert` 步驟中， `actualMessages` 會從頁面模型的屬性指派實際的訊息（） `Messages` 。</span><span class="sxs-lookup"><span data-stu-id="12470-347">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="12470-348">指派訊息時也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="12470-348">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="12470-349">預期和實際的訊息會依其屬性進行比較 `Text` 。</span><span class="sxs-lookup"><span data-stu-id="12470-349">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="12470-350">測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="12470-350">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="12470-351">此群組中的其他測試會建立頁面模型物件，其中包含 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext> 、、 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary> 、， <xref:Microsoft.AspNetCore.Mvc.ActionContext> 以建立 `PageContext` 、 `ViewDataDictionary` 和 `PageContext` 。</span><span class="sxs-lookup"><span data-stu-id="12470-351">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="12470-352">這些適用于進行測試。</span><span class="sxs-lookup"><span data-stu-id="12470-352">These are useful in conducting tests.</span></span> <span data-ttu-id="12470-353">例如，訊息應用程式 `ModelState` 會與建立錯誤， <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 以檢查 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> 執行時傳回的有效 `OnPostAddMessageAsync` ：</span><span class="sxs-lookup"><span data-stu-id="12470-353">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="12470-354">其他資源</span><span class="sxs-lookup"><span data-stu-id="12470-354">Additional resources</span></span>

* [<span data-ttu-id="12470-355">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="12470-355">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="12470-356">[對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）</span><span class="sxs-lookup"><span data-stu-id="12470-356">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="12470-357">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="12470-357">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="12470-358">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="12470-358">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="12470-359">開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core</span><span class="sxs-lookup"><span data-stu-id="12470-359">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="12470-360">Moq</span><span class="sxs-lookup"><span data-stu-id="12470-360">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="12470-361">Moq 快速入門</span><span class="sxs-lookup"><span data-stu-id="12470-361">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
* <span data-ttu-id="12470-362">[JustMockLite](https://github.com/telerik/JustMockLite)：適用于 .net 開發人員的模擬架構。</span><span class="sxs-lookup"><span data-stu-id="12470-362">[JustMockLite](https://github.com/telerik/JustMockLite): A mocking framework for .NET developers.</span></span> <span data-ttu-id="12470-363">（*不是由 Microsoft 維護或支援）。*</span><span class="sxs-lookup"><span data-stu-id="12470-363">(*Not maintained or supported by Microsoft.*)</span></span>

::: moniker-end
