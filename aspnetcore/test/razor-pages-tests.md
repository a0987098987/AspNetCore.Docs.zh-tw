---
title: 剃刀頁單元測試在ASP.NET核心
author: rick-anderson
description: 瞭解如何為 Razor 頁面應用創建單元測試。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: 0e217b6b7f15519a3da44f5d074cf80fa96a3b3a
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664406"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a><span data-ttu-id="9b4b2-103">剃刀頁單元測試在ASP.NET核心</span><span class="sxs-lookup"><span data-stu-id="9b4b2-103">Razor Pages unit tests in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9b4b2-104">ASP.NET核心支援剃刀頁面應用的單位測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-104">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="9b4b2-105">資料存取層 (DAL) 和頁面模型的測試有助於確保:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-105">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="9b4b2-106">Razor Pages 應用的某些部分在應用構建期間作為一個單元獨立地協同工作。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-106">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="9b4b2-107">類和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-107">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="9b4b2-108">有關應用應如何操作的其他文檔。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-108">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="9b4b2-109">回歸是代碼更新引起的錯誤,在自動生成和部署期間找到。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-109">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="9b4b2-110">本主題假定您對 Razor Pages 應用和單元測試有基本的瞭解。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-110">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="9b4b2-111">如果您不熟悉 Razor 頁面應用或測試概念,請參閱以下主題:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-111">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="9b4b2-112">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-112">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="9b4b2-113">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b4b2-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9b4b2-114">範例項目由兩個應用組成:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-114">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="9b4b2-115">App</span><span class="sxs-lookup"><span data-stu-id="9b4b2-115">App</span></span>         | <span data-ttu-id="9b4b2-116">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="9b4b2-116">Project folder</span></span>                     | <span data-ttu-id="9b4b2-117">描述</span><span class="sxs-lookup"><span data-stu-id="9b4b2-117">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="9b4b2-118">訊息應用</span><span class="sxs-lookup"><span data-stu-id="9b4b2-118">Message app</span></span> | <span data-ttu-id="9b4b2-119">*src/剃鬚刀測試樣品*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-119">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="9b4b2-120">允許使用者添加郵件、刪除一條消息、刪除所有郵件和分析郵件(尋找每條消息的平均字數)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-120">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="9b4b2-121">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="9b4b2-121">Test app</span></span>    | <span data-ttu-id="9b4b2-122">*測試/剃鬚刀測試樣本.測試*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-122">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="9b4b2-123">用於單元測試消息應用的 DAL 和索引頁模型。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-123">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="9b4b2-124">測試可以使用 IDE 的內建測試功能執行,例如[Mac 的視覺化工作室](/visualstudio/test/unit-test-your-code)或[視覺工作室](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-124">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="9b4b2-125">如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesTestSample.Test.Test.Test*資料夾中的命令提示符執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-125">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="9b4b2-126">訊息應用組織</span><span class="sxs-lookup"><span data-stu-id="9b4b2-126">Message app organization</span></span>

<span data-ttu-id="9b4b2-127">訊息應用是具有以下特徵的 Razor Pages 訊息系統:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-127">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="9b4b2-128">應用程式的索引頁(*頁面/Index.cshtml*和*頁面/Index.cshtml.cs)* 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(尋找每條消息的平均字數)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-128">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="9b4b2-129">消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-129">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="9b4b2-130">該`Text`屬性是必需的,限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-130">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="9b4b2-131">消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-131">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="9b4b2-132">該應用程式在其資料庫上下文類中包含`AppDbContext`DAL(*資料/AppDbContext.cs)。*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-132">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="9b4b2-133">DAL 方法被標`virtual`記 ,這允許類比用於測試的方法。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-133">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="9b4b2-134">如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-134">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="9b4b2-135">這些*種子消息*也用於測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-135">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="9b4b2-136">&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-136">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="9b4b2-137">本主題使用[xUnit](https://xunit.github.io/)測試框架。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-137">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="9b4b2-138">不同測試框架的測試概念和測試實現相似但不相同。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-138">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="9b4b2-139">儘管示例應用不使用儲存庫模式,並且不是[工作單元 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,但 Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-139">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="9b4b2-140">有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和<xref:mvc/controllers/testing>(示例實現存儲庫模式)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-140">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="9b4b2-141">測試應用組織</span><span class="sxs-lookup"><span data-stu-id="9b4b2-141">Test app organization</span></span>

<span data-ttu-id="9b4b2-142">測試應用是*測試/RazorPagesTestSample.Test.測試*資料夾中的控制台應用。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-142">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="9b4b2-143">測試套用資料夾</span><span class="sxs-lookup"><span data-stu-id="9b4b2-143">Test app folder</span></span> | <span data-ttu-id="9b4b2-144">描述</span><span class="sxs-lookup"><span data-stu-id="9b4b2-144">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="9b4b2-145">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-145">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="9b4b2-146">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-146">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="9b4b2-147">*IndexPageTests.cs*包含索引頁模型的單位測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-147">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="9b4b2-148">*公用事業*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-148">*Utilities*</span></span>     | <span data-ttu-id="9b4b2-149">包含用於為每個`TestDbContextOptions`DAL 單元測試創建新資料庫上下文選項的方法,以便資料庫重置為每個測試的基線條件。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-149">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="9b4b2-150">測試框架是[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-150">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="9b4b2-151">對象類比框架是[Moq。](https://github.com/moq/moq4)</span><span class="sxs-lookup"><span data-stu-id="9b4b2-151">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="9b4b2-152">資料存取層 (DAL) 的單位測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-152">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="9b4b2-153">消息應用具有包含`AppDbContext`類 *(src/RazorPagesTestSample/資料/AppDbContext.cs)* 中的四種方法的 DAL。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-153">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="9b4b2-154">每個方法在測試應用中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-154">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="9b4b2-155">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="9b4b2-155">DAL method</span></span>               | <span data-ttu-id="9b4b2-156">函式</span><span class="sxs-lookup"><span data-stu-id="9b4b2-156">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="9b4b2-157">從順序`Text``List<Message>`的資料庫抓取 。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-157">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="9b4b2-158">新增資料庫`Message`加入資料庫 。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-158">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="9b4b2-159">從資料庫中刪除`Message`所有條目。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-159">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="9b4b2-160">通過從資料庫`Message`中刪除單個。 `Id`</span><span class="sxs-lookup"><span data-stu-id="9b4b2-160">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="9b4b2-161">在為每個測試創建新測試時<xref:Microsoft.EntityFrameworkCore.DbContextOptions>,需要 DAL 的`AppDbContext`單位測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-161">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="9b4b2-162">為每個測試建立`DbContextOptions`的一種方法是使用<xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-162">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="9b4b2-163">此方法的問題在於,每個測試都以上次測試離開的任何狀態接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-163">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="9b4b2-164">當嘗試編寫不相互干擾的原子單元測試時,這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-164">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="9b4b2-165">要強制`AppDbContext`對每個測試使用新的資料庫上下文,請使用基於新服務提供`DbContextOptions`者 的實例。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-165">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="9b4b2-166">測試應用程式展示如何使用其`Utilities`類別`TestDbContextOptions`方法 (*測試/ RazorPagesTestSample. 測試/ 實用程式.cs)* 執行此操作:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-166">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="9b4b2-167">`DbContextOptions`使用 DAL 單元測試允許每個測試使用新的資料庫實體以原子方式執行:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-167">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="9b4b2-168">`DataAccessLayerTest`類別的每個測試方法 (*單元測試 / 資料存取層測試.cs*) 都遵循類似的排列-行為斷言模式:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-168">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="9b4b2-169">排列:為測試配置了資料庫和/或定義了預期結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-169">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="9b4b2-170">操作:執行測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-170">Act: The test is executed.</span></span>
1. <span data-ttu-id="9b4b2-171">斷言:斷言是確定測試結果是否成功。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-171">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="9b4b2-172">例如,`DeleteMessageAsync`該方法負責刪除由`Id`其 *(src/RazorPagesTestSample/Data/AppDbContext.cs)* 識別的單一訊息:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-172">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="9b4b2-173">此方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-173">There are two tests for this method.</span></span> <span data-ttu-id="9b4b2-174">一個測試檢查當消息存在於資料庫中時,該方法是否刪除消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-174">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="9b4b2-175">另一種方法測試,如果刪除消息`Id`不存在,資料庫不會更改。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-175">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="9b4b2-176">該方法`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`如下所示:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-176">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="9b4b2-177">首先,該方法執行"排列"步驟,在其中準備 Act 步驟。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-177">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="9b4b2-178">種子化消息在`seedMessages`中 獲取並持有。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-178">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="9b4b2-179">種子設定消息將保存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-179">The seeding messages are saved into the database.</span></span> <span data-ttu-id="9b4b2-180">具有的`Id``1`已設置為刪除的消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-180">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="9b4b2-181">執行`DeleteMessageAsync`方法時,預期消息應具有除`Id``1`具有的的消息之外的所有消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-181">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="9b4b2-182">變數`expectedMessages`表示此預期結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-182">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="9b4b2-183">方法作用:該方法`DeleteMessageAsync`在`recId`中`1`執行傳遞:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-183">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="9b4b2-184">最後,該方法從上下文中獲取`Messages`,並將其`expectedMessages`與 斷言兩者相等:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-184">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="9b4b2-185">為了比較兩`List<Message>`者是相同的:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-185">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="9b4b2-186">消息按 排序`Id`。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-186">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="9b4b2-187">在`Text`屬性上比較消息對。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-187">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="9b4b2-188">類似的測試方法檢查`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`嘗試刪除不存在的消息的結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-188">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="9b4b2-189">在這種情況下,資料庫中的預期消息應等於執行`DeleteMessageAsync`方法后的實際消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-189">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="9b4b2-190">不要變更資料庫的內容:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-190">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="9b4b2-191">頁面模型方法的單位測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-191">Unit tests of the page model methods</span></span>

<span data-ttu-id="9b4b2-192">另一組單元測試負責頁面模型方法的測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-192">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="9b4b2-193">在消息應用中,索引頁模型在`IndexModel`*src/RazorPagesTestSample/Pages/Index.cs.cs*的類中找到。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-193">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="9b4b2-194">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="9b4b2-194">Page model method</span></span> | <span data-ttu-id="9b4b2-195">函式</span><span class="sxs-lookup"><span data-stu-id="9b4b2-195">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="9b4b2-196">使用`GetMessagesAsync`方法從 UI 的 DAL 獲取消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-196">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="9b4b2-197">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效`AddMessageAsync`,則調用以向資料庫添加消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-197">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="9b4b2-198">呼叫`DeleteAllMessagesAsync`以刪除資料庫中的所有消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-198">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="9b4b2-199">執行`DeleteMessageAsync`以刪除指定`Id`郵件。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-199">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="9b4b2-200">如果資料庫中有一個或多個消息,則計算每條消息的平均單詞數。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-200">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="9b4b2-201">頁面模型方法使用`IndexPageTests`類中的七個測試(*測試/RazorPagesTestSample.測試/單元測試/IndexPageTest.cs)* 進行測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-201">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="9b4b2-202">測試使用熟悉的排列-斷言-行為模式。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-202">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="9b4b2-203">這些測試側重於:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-203">These tests focus on:</span></span>

* <span data-ttu-id="9b4b2-204">確定當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時,這些方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-204">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="9b4b2-205">確認方法會產生正確的<xref:Microsoft.AspNetCore.Mvc.IActionResult>。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-205">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="9b4b2-206">檢查屬性值分配是否正確。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-206">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="9b4b2-207">這組測試通常嘲笑 DAL 的方法,以便為執行頁面模型方法的 Act 步驟生成預期數據。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-207">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="9b4b2-208">例如,`GetMessagesAsync``AppDbContext`將類比方法以生成輸出。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-208">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="9b4b2-209">當頁面模型方法執行此方法時,類比將返回結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-209">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="9b4b2-210">數據不來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-210">The data doesn't come from the database.</span></span> <span data-ttu-id="9b4b2-211">這為在頁面模型測試中使用 DAL 創造了可預測的、可靠的測試條件。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-211">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="9b4b2-212">測試`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`顯示如何對`GetMessagesAsync`頁面 模型模擬該方法:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-212">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="9b4b2-213">在`OnGetAsync`Act 步驟中執行該方法時,它將調用`GetMessagesAsync`頁面模型 的方法。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-213">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="9b4b2-214">單元測試行動步驟(*測試/ RazorPages 測試樣本.測試/ 單元測試/索引頁面測試.cs):*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-214">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="9b4b2-215">`IndexPage`頁面模型`OnGetAsync`的方法 (*src/ RazorPages 測試範例/ 頁面/ 索引.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9b4b2-215">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="9b4b2-216">DAL`GetMessagesAsync`中的方法不會返回此方法調用的結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-216">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="9b4b2-217">方法的類比版本返回結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-217">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="9b4b2-218">在步驟`Assert`中,實際消息`actualMessages`( )`Messages`是從頁面模型的屬性中分配的。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-218">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="9b4b2-219">分配消息時,也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-219">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="9b4b2-220">預期消息和實際消息按其`Text`屬性進行比較。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-220">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="9b4b2-221">測試斷言這兩`List<Message>`個實例包含相同的消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-221">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="9b4b2-222">此群組中的其他測試建立頁面模型<xref:Microsoft.AspNetCore.Http.DefaultHttpContext>物件,其中包括<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary><xref:Microsoft.AspNetCore.Mvc.ActionContext>`PageContext``ViewDataDictionary``PageContext`、 、</span><span class="sxs-lookup"><span data-stu-id="9b4b2-222">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="9b4b2-223">這些在進行測試時非常有用。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-223">These are useful in conducting tests.</span></span> <span data-ttu-id="9b4b2-224">例如,訊息應用建立一`ModelState`個錯誤,<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>用於檢查在執行<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>`OnPostAddMessageAsync`時 是否傳回有效:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-224">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="9b4b2-225">其他資源</span><span class="sxs-lookup"><span data-stu-id="9b4b2-225">Additional resources</span></span>

* [<span data-ttu-id="9b4b2-226">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-226">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="9b4b2-227">[單元測試代碼](/visualstudio/test/unit-test-your-code)(可視化工作室)</span><span class="sxs-lookup"><span data-stu-id="9b4b2-227">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="9b4b2-228">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="9b4b2-228">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="9b4b2-229">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="9b4b2-229">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="9b4b2-230">開始使用xUnit.net:使用 .NET Core 與 .NET SDK 指令列</span><span class="sxs-lookup"><span data-stu-id="9b4b2-230">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="9b4b2-231">莫克</span><span class="sxs-lookup"><span data-stu-id="9b4b2-231">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="9b4b2-232">莫克快速入門</span><span class="sxs-lookup"><span data-stu-id="9b4b2-232">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9b4b2-233">ASP.NET核心支援剃刀頁面應用的單位測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-233">ASP.NET Core supports unit tests of Razor Pages apps.</span></span> <span data-ttu-id="9b4b2-234">資料存取層 (DAL) 和頁面模型的測試有助於確保:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-234">Tests of the data access layer (DAL) and page models help ensure:</span></span>

* <span data-ttu-id="9b4b2-235">Razor Pages 應用的某些部分在應用構建期間作為一個單元獨立地協同工作。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-235">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="9b4b2-236">類和方法的責任範圍有限。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-236">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="9b4b2-237">有關應用應如何操作的其他文檔。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-237">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="9b4b2-238">回歸是代碼更新引起的錯誤,在自動生成和部署期間找到。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-238">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="9b4b2-239">本主題假定您對 Razor Pages 應用和單元測試有基本的瞭解。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-239">This topic assumes that you have a basic understanding of Razor Pages apps and unit tests.</span></span> <span data-ttu-id="9b4b2-240">如果您不熟悉 Razor 頁面應用或測試概念,請參閱以下主題:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-240">If you're unfamiliar with Razor Pages apps or test concepts, see the following topics:</span></span>

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [<span data-ttu-id="9b4b2-241">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-241">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

<span data-ttu-id="9b4b2-242">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b4b2-242">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9b4b2-243">範例項目由兩個應用組成:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-243">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="9b4b2-244">App</span><span class="sxs-lookup"><span data-stu-id="9b4b2-244">App</span></span>         | <span data-ttu-id="9b4b2-245">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="9b4b2-245">Project folder</span></span>                     | <span data-ttu-id="9b4b2-246">描述</span><span class="sxs-lookup"><span data-stu-id="9b4b2-246">Description</span></span> |
| ----------- | ---------------------------------- | ----------- |
| <span data-ttu-id="9b4b2-247">訊息應用</span><span class="sxs-lookup"><span data-stu-id="9b4b2-247">Message app</span></span> | <span data-ttu-id="9b4b2-248">*src/剃鬚刀測試樣品*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-248">*src/RazorPagesTestSample*</span></span>         | <span data-ttu-id="9b4b2-249">允許使用者添加郵件、刪除一條消息、刪除所有郵件和分析郵件(尋找每條消息的平均字數)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-249">Allows a user to add a message, delete one message, delete all messages, and analyze messages (find the average number of words per message).</span></span> |
| <span data-ttu-id="9b4b2-250">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="9b4b2-250">Test app</span></span>    | <span data-ttu-id="9b4b2-251">*測試/剃鬚刀測試樣本.測試*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-251">*tests/RazorPagesTestSample.Tests*</span></span> | <span data-ttu-id="9b4b2-252">用於單元測試消息應用的 DAL 和索引頁模型。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-252">Used to unit test the DAL and Index page model of the message app.</span></span> |

<span data-ttu-id="9b4b2-253">測試可以使用 IDE 的內建測試功能執行,例如[Mac 的視覺化工作室](/visualstudio/test/unit-test-your-code)或[視覺工作室](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-253">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](/visualstudio/test/unit-test-your-code) or [Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution).</span></span> <span data-ttu-id="9b4b2-254">如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesTestSample.Test.Test.Test*資料夾中的命令提示符執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-254">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestSample.Tests* folder:</span></span>

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="9b4b2-255">訊息應用組織</span><span class="sxs-lookup"><span data-stu-id="9b4b2-255">Message app organization</span></span>

<span data-ttu-id="9b4b2-256">訊息應用是具有以下特徵的 Razor Pages 訊息系統:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-256">The message app is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="9b4b2-257">應用程式的索引頁(*頁面/Index.cshtml*和*頁面/Index.cshtml.cs)* 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(尋找每條消息的平均字數)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-257">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (find the average number of words per message).</span></span>
* <span data-ttu-id="9b4b2-258">消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-258">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="9b4b2-259">該`Text`屬性是必需的,限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-259">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="9b4b2-260">消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-260">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="9b4b2-261">該應用程式在其資料庫上下文類中包含`AppDbContext`DAL(*資料/AppDbContext.cs)。*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-261">The app contains a DAL in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="9b4b2-262">DAL 方法被標`virtual`記 ,這允許類比用於測試的方法。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-262">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="9b4b2-263">如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-263">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="9b4b2-264">這些*種子消息*也用於測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-264">These *seeded messages* are also used in tests.</span></span>

<span data-ttu-id="9b4b2-265">&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-265">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="9b4b2-266">本主題使用[xUnit](https://xunit.github.io/)測試框架。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-266">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="9b4b2-267">不同測試框架的測試概念和測試實現相似但不相同。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-267">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="9b4b2-268">儘管示例應用不使用儲存庫模式,並且不是[工作單元 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,但 Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-268">Although the sample app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="9b4b2-269">有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和<xref:mvc/controllers/testing>(示例實現存儲庫模式)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-269">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and <xref:mvc/controllers/testing> (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="9b4b2-270">測試應用組織</span><span class="sxs-lookup"><span data-stu-id="9b4b2-270">Test app organization</span></span>

<span data-ttu-id="9b4b2-271">測試應用是*測試/RazorPagesTestSample.Test.測試*資料夾中的控制台應用。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-271">The test app is a console app inside the *tests/RazorPagesTestSample.Tests* folder.</span></span>

| <span data-ttu-id="9b4b2-272">測試套用資料夾</span><span class="sxs-lookup"><span data-stu-id="9b4b2-272">Test app folder</span></span> | <span data-ttu-id="9b4b2-273">描述</span><span class="sxs-lookup"><span data-stu-id="9b4b2-273">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="9b4b2-274">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-274">*UnitTests*</span></span>     | <ul><li><span data-ttu-id="9b4b2-275">*DataAccessLayerTest.cs*包含 DAL 的單元測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-275">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="9b4b2-276">*IndexPageTests.cs*包含索引頁模型的單位測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-276">*IndexPageTests.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="9b4b2-277">*公用事業*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-277">*Utilities*</span></span>     | <span data-ttu-id="9b4b2-278">包含用於為每個`TestDbContextOptions`DAL 單元測試創建新資料庫上下文選項的方法,以便資料庫重置為每個測試的基線條件。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-278">Contains the `TestDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span> |

<span data-ttu-id="9b4b2-279">測試框架是[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-279">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="9b4b2-280">對象類比框架是[Moq。](https://github.com/moq/moq4)</span><span class="sxs-lookup"><span data-stu-id="9b4b2-280">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span>

## <a name="unit-tests-of-the-data-access-layer-dal"></a><span data-ttu-id="9b4b2-281">資料存取層 (DAL) 的單位測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-281">Unit tests of the data access layer (DAL)</span></span>

<span data-ttu-id="9b4b2-282">消息應用具有包含`AppDbContext`類 *(src/RazorPagesTestSample/資料/AppDbContext.cs)* 中的四種方法的 DAL。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-282">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="9b4b2-283">每個方法在測試應用中都有一個或兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-283">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="9b4b2-284">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="9b4b2-284">DAL method</span></span>               | <span data-ttu-id="9b4b2-285">函式</span><span class="sxs-lookup"><span data-stu-id="9b4b2-285">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="9b4b2-286">從順序`Text``List<Message>`的資料庫抓取 。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-286">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="9b4b2-287">新增資料庫`Message`加入資料庫 。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-287">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="9b4b2-288">從資料庫中刪除`Message`所有條目。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-288">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="9b4b2-289">通過從資料庫`Message`中刪除單個。 `Id`</span><span class="sxs-lookup"><span data-stu-id="9b4b2-289">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="9b4b2-290">在為每個測試創建新測試時<xref:Microsoft.EntityFrameworkCore.DbContextOptions>,需要 DAL 的`AppDbContext`單位測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-290">Unit tests of the DAL require <xref:Microsoft.EntityFrameworkCore.DbContextOptions> when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="9b4b2-291">為每個測試建立`DbContextOptions`的一種方法是使用<xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-291">One approach to creating the `DbContextOptions` for each test is to use a <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="9b4b2-292">此方法的問題在於,每個測試都以上次測試離開的任何狀態接收資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-292">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="9b4b2-293">當嘗試編寫不相互干擾的原子單元測試時,這可能會有問題。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-293">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="9b4b2-294">要強制`AppDbContext`對每個測試使用新的資料庫上下文,請使用基於新服務提供`DbContextOptions`者 的實例。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-294">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="9b4b2-295">測試應用程式展示如何使用其`Utilities`類別`TestDbContextOptions`方法 (*測試/ RazorPagesTestSample. 測試/ 實用程式.cs)* 執行此操作:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-295">The test app shows how to do this using its `Utilities` class method `TestDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="9b4b2-296">`DbContextOptions`使用 DAL 單元測試允許每個測試使用新的資料庫實體以原子方式執行:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-296">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="9b4b2-297">`DataAccessLayerTest`類別的每個測試方法 (*單元測試 / 資料存取層測試.cs*) 都遵循類似的排列-行為斷言模式:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-297">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="9b4b2-298">排列:為測試配置了資料庫和/或定義了預期結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-298">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="9b4b2-299">操作:執行測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-299">Act: The test is executed.</span></span>
1. <span data-ttu-id="9b4b2-300">斷言:斷言是確定測試結果是否成功。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-300">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="9b4b2-301">例如,`DeleteMessageAsync`該方法負責刪除由`Id`其 *(src/RazorPagesTestSample/Data/AppDbContext.cs)* 識別的單一訊息:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-301">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="9b4b2-302">此方法有兩個測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-302">There are two tests for this method.</span></span> <span data-ttu-id="9b4b2-303">一個測試檢查當消息存在於資料庫中時,該方法是否刪除消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-303">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="9b4b2-304">另一種方法測試,如果刪除消息`Id`不存在,資料庫不會更改。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-304">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="9b4b2-305">該方法`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`如下所示:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-305">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="9b4b2-306">首先,該方法執行"排列"步驟,在其中準備 Act 步驟。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-306">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="9b4b2-307">種子化消息在`seedMessages`中 獲取並持有。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-307">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="9b4b2-308">種子設定消息將保存到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-308">The seeding messages are saved into the database.</span></span> <span data-ttu-id="9b4b2-309">具有的`Id``1`已設置為刪除的消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-309">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="9b4b2-310">執行`DeleteMessageAsync`方法時,預期消息應具有除`Id``1`具有的的消息之外的所有消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-310">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="9b4b2-311">變數`expectedMessages`表示此預期結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-311">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="9b4b2-312">方法作用:該方法`DeleteMessageAsync`在`recId`中`1`執行傳遞:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-312">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="9b4b2-313">最後,該方法從上下文中獲取`Messages`,並將其`expectedMessages`與 斷言兩者相等:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-313">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="9b4b2-314">為了比較兩`List<Message>`者是相同的:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-314">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="9b4b2-315">消息按 排序`Id`。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-315">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="9b4b2-316">在`Text`屬性上比較消息對。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-316">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="9b4b2-317">類似的測試方法檢查`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`嘗試刪除不存在的消息的結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-317">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="9b4b2-318">在這種情況下,資料庫中的預期消息應等於執行`DeleteMessageAsync`方法后的實際消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-318">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="9b4b2-319">不要變更資料庫的內容:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-319">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a><span data-ttu-id="9b4b2-320">頁面模型方法的單位測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-320">Unit tests of the page model methods</span></span>

<span data-ttu-id="9b4b2-321">另一組單元測試負責頁面模型方法的測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-321">Another set of unit tests is responsible for tests of page model methods.</span></span> <span data-ttu-id="9b4b2-322">在消息應用中,索引頁模型在`IndexModel`*src/RazorPagesTestSample/Pages/Index.cs.cs*的類中找到。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-322">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="9b4b2-323">頁面模型方法</span><span class="sxs-lookup"><span data-stu-id="9b4b2-323">Page model method</span></span> | <span data-ttu-id="9b4b2-324">函式</span><span class="sxs-lookup"><span data-stu-id="9b4b2-324">Function</span></span> |
| ----------------- | -------- |
| `OnGetAsync` | <span data-ttu-id="9b4b2-325">使用`GetMessagesAsync`方法從 UI 的 DAL 獲取消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-325">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="9b4b2-326">如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效`AddMessageAsync`,則調用以向資料庫添加消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-326">If the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is valid, calls `AddMessageAsync` to add a message to the database.</span></span> |
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="9b4b2-327">呼叫`DeleteAllMessagesAsync`以刪除資料庫中的所有消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-327">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="9b4b2-328">執行`DeleteMessageAsync`以刪除指定`Id`郵件。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-328">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="9b4b2-329">如果資料庫中有一個或多個消息,則計算每條消息的平均單詞數。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-329">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="9b4b2-330">頁面模型方法使用`IndexPageTests`類中的七個測試(*測試/RazorPagesTestSample.測試/單元測試/IndexPageTest.cs)* 進行測試。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-330">The page model methods are tested using seven tests in the `IndexPageTests` class (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*).</span></span> <span data-ttu-id="9b4b2-331">測試使用熟悉的排列-斷言-行為模式。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-331">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="9b4b2-332">這些測試側重於:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-332">These tests focus on:</span></span>

* <span data-ttu-id="9b4b2-333">確定當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時,這些方法是否遵循正確的行為。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-333">Determining if the methods follow the correct behavior when the [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) is invalid.</span></span>
* <span data-ttu-id="9b4b2-334">確認方法會產生正確的<xref:Microsoft.AspNetCore.Mvc.IActionResult>。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-334">Confirming the methods produce the correct <xref:Microsoft.AspNetCore.Mvc.IActionResult>.</span></span>
* <span data-ttu-id="9b4b2-335">檢查屬性值分配是否正確。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-335">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="9b4b2-336">這組測試通常嘲笑 DAL 的方法,以便為執行頁面模型方法的 Act 步驟生成預期數據。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-336">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="9b4b2-337">例如,`GetMessagesAsync``AppDbContext`將類比方法以生成輸出。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-337">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="9b4b2-338">當頁面模型方法執行此方法時,類比將返回結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-338">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="9b4b2-339">數據不來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-339">The data doesn't come from the database.</span></span> <span data-ttu-id="9b4b2-340">這為在頁面模型測試中使用 DAL 創造了可預測的、可靠的測試條件。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-340">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="9b4b2-341">測試`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`顯示如何對`GetMessagesAsync`頁面 模型模擬該方法:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-341">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="9b4b2-342">在`OnGetAsync`Act 步驟中執行該方法時,它將調用`GetMessagesAsync`頁面模型 的方法。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-342">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="9b4b2-343">單元測試行動步驟(*測試/ RazorPages 測試樣本.測試/ 單元測試/索引頁面測試.cs):*</span><span class="sxs-lookup"><span data-stu-id="9b4b2-343">Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="9b4b2-344">`IndexPage`頁面模型`OnGetAsync`的方法 (*src/ RazorPages 測試範例/ 頁面/ 索引.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9b4b2-344">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="9b4b2-345">DAL`GetMessagesAsync`中的方法不會返回此方法調用的結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-345">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="9b4b2-346">方法的類比版本返回結果。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-346">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="9b4b2-347">在步驟`Assert`中,實際消息`actualMessages`( )`Messages`是從頁面模型的屬性中分配的。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-347">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="9b4b2-348">分配消息時,也會執行類型檢查。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-348">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="9b4b2-349">預期消息和實際消息按其`Text`屬性進行比較。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-349">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="9b4b2-350">測試斷言這兩`List<Message>`個實例包含相同的消息。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-350">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

<span data-ttu-id="9b4b2-351">此群組中的其他測試建立頁面模型<xref:Microsoft.AspNetCore.Http.DefaultHttpContext>物件,其中包括<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary><xref:Microsoft.AspNetCore.Mvc.ActionContext>`PageContext``ViewDataDictionary``PageContext`、 、</span><span class="sxs-lookup"><span data-stu-id="9b4b2-351">Other tests in this group create page model objects that include the <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>, the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>, an <xref:Microsoft.AspNetCore.Mvc.ActionContext> to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="9b4b2-352">這些在進行測試時非常有用。</span><span class="sxs-lookup"><span data-stu-id="9b4b2-352">These are useful in conducting tests.</span></span> <span data-ttu-id="9b4b2-353">例如,訊息應用建立一`ModelState`個錯誤,<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>用於檢查在執行<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>`OnPostAddMessageAsync`時 是否傳回有效:</span><span class="sxs-lookup"><span data-stu-id="9b4b2-353">For example, the message app establishes a `ModelState` error with <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> to check that a valid <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a><span data-ttu-id="9b4b2-354">其他資源</span><span class="sxs-lookup"><span data-stu-id="9b4b2-354">Additional resources</span></span>

* [<span data-ttu-id="9b4b2-355">使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試</span><span class="sxs-lookup"><span data-stu-id="9b4b2-355">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* <span data-ttu-id="9b4b2-356">[單元測試代碼](/visualstudio/test/unit-test-your-code)(可視化工作室)</span><span class="sxs-lookup"><span data-stu-id="9b4b2-356">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* <xref:test/integration-tests>
* [<span data-ttu-id="9b4b2-357">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="9b4b2-357">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="9b4b2-358">使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案</span><span class="sxs-lookup"><span data-stu-id="9b4b2-358">Building a complete .NET Core solution on macOS using Visual Studio for Mac</span></span>](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [<span data-ttu-id="9b4b2-359">開始使用xUnit.net:使用 .NET Core 與 .NET SDK 指令列</span><span class="sxs-lookup"><span data-stu-id="9b4b2-359">Getting started with xUnit.net: Using .NET Core with the .NET SDK command line</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="9b4b2-360">莫克</span><span class="sxs-lookup"><span data-stu-id="9b4b2-360">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="9b4b2-361">莫克快速入門</span><span class="sxs-lookup"><span data-stu-id="9b4b2-361">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
