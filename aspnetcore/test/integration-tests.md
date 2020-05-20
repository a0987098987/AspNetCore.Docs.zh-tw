---
title: ASP.NET Core 中的整合測試
author: rick-anderson
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/integration-tests
ms.openlocfilehash: 5a11fd574baccba32f3224e47c01685c81c6d686
ms.sourcegitcommit: 69e1a79a572b0af17d08e81af12c594b7316f2e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2020
ms.locfileid: "83424207"
---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="8b21f-103">ASP.NET Core 中的整合測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-103">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="8b21f-104">作者： [Javier Calvarro Nelson](https://github.com/javiercn)、 [Steve Smith](https://ardalis.com/)和[Jos van der Til](https://jvandertil.nl)</span><span class="sxs-lookup"><span data-stu-id="8b21f-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Steve Smith](https://ardalis.com/), and [Jos van der Til](https://jvandertil.nl)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8b21f-105">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="8b21f-105">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="8b21f-106">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-106">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="8b21f-107">本主題假設對單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="8b21f-107">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="8b21f-108">如果不熟悉測試概念，請參閱[.Net Core 中的單元測試和 .NET Standard](/dotnet/core/testing/)主題及其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="8b21f-108">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="8b21f-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8b21f-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8b21f-110">範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="8b21f-110">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="8b21f-111">如果不熟悉 Razor Pages，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="8b21f-111">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="8b21f-112">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="8b21f-112">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="8b21f-113">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8b21f-113">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="8b21f-114">Razor Pages 單元測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-114">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="8b21f-115">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="8b21f-115">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="8b21f-116">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="8b21f-116">Introduction to integration tests</span></span>

<span data-ttu-id="8b21f-117">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-117">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="8b21f-118">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="8b21f-118">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="8b21f-119">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-119">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="8b21f-120">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="8b21f-120">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="8b21f-121">資料庫</span><span class="sxs-lookup"><span data-stu-id="8b21f-121">Database</span></span>
* <span data-ttu-id="8b21f-122">檔案系統</span><span class="sxs-lookup"><span data-stu-id="8b21f-122">File system</span></span>
* <span data-ttu-id="8b21f-123">網路設備</span><span class="sxs-lookup"><span data-stu-id="8b21f-123">Network appliances</span></span>
* <span data-ttu-id="8b21f-124">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="8b21f-124">Request-response pipeline</span></span>

<span data-ttu-id="8b21f-125">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-125">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="8b21f-126">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="8b21f-126">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="8b21f-127">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-127">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="8b21f-128">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="8b21f-128">Require more code and data processing.</span></span>
* <span data-ttu-id="8b21f-129">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="8b21f-129">Take longer to run.</span></span>

<span data-ttu-id="8b21f-130">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="8b21f-130">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="8b21f-131">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-131">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="8b21f-132">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-132">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="8b21f-133">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-133">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="8b21f-134">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-134">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="8b21f-135">在單元測試中，使用基礎結構 fakes/模擬會產生更快速的測試執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-135">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="8b21f-136">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="8b21f-136">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="8b21f-137">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="8b21f-137">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="8b21f-138">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-138">ASP.NET Core integration tests</span></span>

<span data-ttu-id="8b21f-139">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="8b21f-139">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="8b21f-140">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-140">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="8b21f-141">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="8b21f-141">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="8b21f-142">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="8b21f-142">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="8b21f-143">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="8b21f-143">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="8b21f-144">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="8b21f-144">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="8b21f-145">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="8b21f-145">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="8b21f-146">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-146">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="8b21f-147">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="8b21f-147">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="8b21f-148">執行*Act*測試步驟：用戶端提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="8b21f-148">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="8b21f-149">執行判斷*提示測試步驟*：*實際*的回應會根據*預期*的回應驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="8b21f-149">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="8b21f-150">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="8b21f-150">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="8b21f-151">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="8b21f-151">The test results are reported.</span></span>

<span data-ttu-id="8b21f-152">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="8b21f-152">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="8b21f-153">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="8b21f-153">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="8b21f-154">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="8b21f-154">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="8b21f-155">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-155">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="8b21f-156">此 `Microsoft.AspNetCore.Mvc.Testing` 套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="8b21f-156">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="8b21f-157">將相依性檔案（*. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="8b21f-157">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="8b21f-158">將[內容根目錄](xref:fundamentals/index#content-root)設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="8b21f-158">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="8b21f-159">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用來啟動的 SUT `TestServer` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-159">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="8b21f-160">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="8b21f-160">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="8b21f-161">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="8b21f-161">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="8b21f-162">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="8b21f-162">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="8b21f-163">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-163">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="8b21f-164">Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="8b21f-164">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="8b21f-165">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-165">The only difference is in how the tests are named.</span></span> <span data-ttu-id="8b21f-166">在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如，用 `IndexPageTests` 來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-166">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="8b21f-167">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名（例如， `HomeControllerTests` 測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-167">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="8b21f-168">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="8b21f-168">Test app prerequisites</span></span>

<span data-ttu-id="8b21f-169">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="8b21f-169">The test project must:</span></span>

* <span data-ttu-id="8b21f-170">參考[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)的封裝。</span><span class="sxs-lookup"><span data-stu-id="8b21f-170">Reference the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span>
* <span data-ttu-id="8b21f-171">在專案檔中指定 Web SDK （ `<Project Sdk="Microsoft.NET.Sdk.Web">` ）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-171">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span>

<span data-ttu-id="8b21f-172">這些必要條件可在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="8b21f-172">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="8b21f-173">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="8b21f-173">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="8b21f-174">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="8b21f-174">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="8b21f-175">xunit</span><span class="sxs-lookup"><span data-stu-id="8b21f-175">xunit</span></span>](https://www.nuget.org/packages/xunit)
* [<span data-ttu-id="8b21f-176">xunit。 visualstudio</span><span class="sxs-lookup"><span data-stu-id="8b21f-176">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [<span data-ttu-id="8b21f-177">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="8b21f-177">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp)

<span data-ttu-id="8b21f-178">Entity Framework Core 也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="8b21f-178">Entity Framework Core is also used in the tests.</span></span> <span data-ttu-id="8b21f-179">應用程式參考：</span><span class="sxs-lookup"><span data-stu-id="8b21f-179">The app references:</span></span>

* [<span data-ttu-id="8b21f-180">AspNetCore. Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="8b21f-180">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [<span data-ttu-id="8b21f-181">AspNetCore. Identity. Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="8b21f-181">Microsoft.AspNetCore.Identity.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [<span data-ttu-id="8b21f-182">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="8b21f-182">Microsoft.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [<span data-ttu-id="8b21f-183">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="8b21f-183">Microsoft.EntityFrameworkCore.InMemory</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [<span data-ttu-id="8b21f-184">Microsoft.entityframeworkcore 工具</span><span class="sxs-lookup"><span data-stu-id="8b21f-184">Microsoft.EntityFrameworkCore.Tools</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a><span data-ttu-id="8b21f-185">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="8b21f-185">SUT environment</span></span>

<span data-ttu-id="8b21f-186">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="8b21f-186">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="8b21f-187">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-187">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="8b21f-188">[WebApplicationFactory \<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)是用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-188">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="8b21f-189">`TEntryPoint`是 SUT 的進入點類別，通常是 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="8b21f-189">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="8b21f-190">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="8b21f-190">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="8b21f-191">下列測試類別會 `BasicTests` 使用 `WebApplicationFactory` 來啟動 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法 `Get_EndpointsReturnSuccessAndCorrectContentType` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-191">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="8b21f-192">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而 `Content-Type` 標頭則 `text/html; charset=utf-8` 適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-192">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="8b21f-193">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) `HttpClient` 會建立自動遵循重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="8b21f-193">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="8b21f-194">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="8b21f-194">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="8b21f-195">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="8b21f-195">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="8b21f-196">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-196">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="8b21f-197">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="8b21f-197">Customize WebApplicationFactory</span></span>

<span data-ttu-id="8b21f-198">藉由繼承自 `WebApplicationFactory` 來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="8b21f-198">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="8b21f-199">繼承自 `WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-199">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="8b21f-200">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：</span><span class="sxs-lookup"><span data-stu-id="8b21f-200">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="8b21f-201">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的資料庫植入是由 `InitializeDbForTests` 方法執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-201">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="8b21f-202">此方法會在[整合測試範例：測試應用程式組織](#test-app-organization)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="8b21f-202">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

   <span data-ttu-id="8b21f-203">SUT 的資料庫內容會在其方法中註冊 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-203">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8b21f-204">測試應用程式的 `builder.ConfigureServices` 回呼會在*after*應用程式的程式 `Startup.ConfigureServices` 代碼執行之後執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-204">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="8b21f-205">執行順序是 ASP.NET Core 3.0 版本之[泛型主機](xref:fundamentals/host/generic-host)的重大變更。</span><span class="sxs-lookup"><span data-stu-id="8b21f-205">The execution order is a breaking change for the [Generic Host](xref:fundamentals/host/generic-host) with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="8b21f-206">若要針對測試使用不同于應用程式資料庫的資料庫，則必須在中取代應用程式的資料庫內容 `builder.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-206">To use a different database for the tests than the app's database, the app's database context must be replaced in `builder.ConfigureServices`.</span></span>

   <span data-ttu-id="8b21f-207">範例應用程式會尋找資料庫內容的服務描述元，並使用描述項來移除服務註冊。</span><span class="sxs-lookup"><span data-stu-id="8b21f-207">The sample app finds the service descriptor for the database context and uses the descriptor to remove the service registration.</span></span> <span data-ttu-id="8b21f-208">接下來，factory 會加入新 `ApplicationDbContext` 的，其會使用記憶體內部資料庫進行測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-208">Next, the factory adds a new `ApplicationDbContext` that uses an in-memory database for the tests.</span></span>

   <span data-ttu-id="8b21f-209">若要連接到與記憶體中資料庫不同的資料庫，請將 `UseInMemoryDatabase` 連接內容的呼叫變更為不同的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8b21f-209">To connect to a different database than the in-memory database, change the `UseInMemoryDatabase` call to connect the context to a different database.</span></span> <span data-ttu-id="8b21f-210">若要使用 SQL Server 測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="8b21f-210">To use a SQL Server test database:</span></span>

   * <span data-ttu-id="8b21f-211">參考專案檔中的[Microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-211">Reference the [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet package in the project file.</span></span>
   * <span data-ttu-id="8b21f-212">`UseSqlServer`使用資料庫的連接字串呼叫。</span><span class="sxs-lookup"><span data-stu-id="8b21f-212">Call `UseSqlServer` with a connection string to the database.</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>((options, context) => 
   {
       context.UseSqlServer(
           Configuration.GetConnectionString("TestingDbConnectionString"));
   });
   ```

2. <span data-ttu-id="8b21f-213">使用 `CustomWebApplicationFactory` 測試類別中的自訂。</span><span class="sxs-lookup"><span data-stu-id="8b21f-213">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="8b21f-214">下列範例會在類別中使用 factory `IndexPageTests` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-214">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="8b21f-215">範例應用程式的用戶端設定為防止 `HttpClient` 下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="8b21f-215">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="8b21f-216">如稍後的模擬[驗證](#mock-authentication)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="8b21f-216">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="8b21f-217">第一個回應是其中許多測試中具有標頭的重新導向 `Location` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-217">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="8b21f-218">一般測試會使用 `HttpClient` 和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="8b21f-218">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="8b21f-219">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="8b21f-219">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="8b21f-220">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="8b21f-220">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="8b21f-221">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="8b21f-221">Make a request for the page.</span></span>
1. <span data-ttu-id="8b21f-222">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="8b21f-222">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="8b21f-223">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="8b21f-223">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="8b21f-224">`SendAsync`範例應用程式中的協助程式擴充方法（helper */HttpClientExtensions*）和 `GetDocumentAsync` helper 方法（helper */HtmlHelpers*）會使用[sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) [AngleSharp](https://anglesharp.github.io/)剖析器，利用下列方法來處理 antiforgery 檢查：</span><span class="sxs-lookup"><span data-stu-id="8b21f-224">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="8b21f-225">`GetDocumentAsync`&ndash;接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回 `IHtmlDocument` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-225">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="8b21f-226">`GetDocumentAsync`使用可根據原始的來準備*虛擬回應*的 factory `HttpResponseMessage` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-226">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="8b21f-227">如需詳細資訊，請參閱[AngleSharp 檔](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-227">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="8b21f-228">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-228">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="8b21f-229">的多載會 `SendAsync` 接受 HTML 表單（ `IHtmlFormElement` ）和下列內容：</span><span class="sxs-lookup"><span data-stu-id="8b21f-229">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="8b21f-230">表單的 [提交] 按鈕（ `IHtmlElement` ）</span><span class="sxs-lookup"><span data-stu-id="8b21f-230">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="8b21f-231">表單值集合（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="8b21f-231">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="8b21f-232">提交按鈕（ `IHtmlElement` ）和表單值（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="8b21f-232">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="8b21f-233">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="8b21f-233">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="8b21f-234">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="8b21f-234">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="8b21f-235">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-235">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="8b21f-236">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="8b21f-236">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="8b21f-237">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="8b21f-237">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="8b21f-238">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) `WebApplicationFactory` 會使用設定進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="8b21f-238">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="8b21f-239">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會示範的用法 `WithWebHostBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-239">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="8b21f-240">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="8b21f-240">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="8b21f-241">因為類別中的另一項測試 `IndexPageTests` 會執行刪除資料庫中所有記錄的作業，而且可能會在此方法之前執行，所以會 `Post_DeleteMessageHandler_ReturnsRedirectToRoot` 在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="8b21f-241">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="8b21f-242">在 sut 的要求中，選取該表單的第一個 [刪除] 按鈕 `messages` 會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="8b21f-242">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="8b21f-243">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="8b21f-243">Client options</span></span>

<span data-ttu-id="8b21f-244">下表顯示建立實例時[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)可用的預設 WebApplicationFactoryClientOptions `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-244">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="8b21f-245">選項</span><span class="sxs-lookup"><span data-stu-id="8b21f-245">Option</span></span> | <span data-ttu-id="8b21f-246">描述</span><span class="sxs-lookup"><span data-stu-id="8b21f-246">Description</span></span> | <span data-ttu-id="8b21f-247">預設</span><span class="sxs-lookup"><span data-stu-id="8b21f-247">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="8b21f-248">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="8b21f-248">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="8b21f-249">取得或設定 `HttpClient` 實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="8b21f-249">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="8b21f-250">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="8b21f-250">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="8b21f-251">取得或設定實例的基底位址 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-251">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="8b21f-252">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="8b21f-252">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="8b21f-253">取得或設定 `HttpClient` 實例是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="8b21f-253">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="8b21f-254">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="8b21f-254">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="8b21f-255">取得或設定實例應遵循的重新導向回應數目上限 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-255">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="8b21f-256">7</span><span class="sxs-lookup"><span data-stu-id="8b21f-256">7</span></span> |

<span data-ttu-id="8b21f-257">建立 `WebApplicationFactoryClientOptions` 類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法（程式碼範例中會顯示預設值）：</span><span class="sxs-lookup"><span data-stu-id="8b21f-257">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="8b21f-258">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="8b21f-258">Inject mock services</span></span>

<span data-ttu-id="8b21f-259">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="8b21f-259">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="8b21f-260">**若要插入模擬服務，SUT 必須具有 `Startup` 具有方法的類別 `Startup.ConfigureServices` 。**</span><span class="sxs-lookup"><span data-stu-id="8b21f-260">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="8b21f-261">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="8b21f-261">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="8b21f-262">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="8b21f-262">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="8b21f-263">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-263">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="8b21f-264">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-264">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="8b21f-265">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-265">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="8b21f-266">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-266">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="8b21f-267">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-267">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="8b21f-268">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="8b21f-268">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="8b21f-269">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-269">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="8b21f-270">Mock 服務會將應用程式的取代為 `QuoteService` 測試應用程式所提供的服務，稱為 `TestQuoteService` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-270">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="8b21f-271">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-271">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="8b21f-272">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="8b21f-272">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="8b21f-273">在測試執行期間產生的標記會反映所提供的引號文字 `TestQuoteService` ，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="8b21f-273">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="8b21f-274">Mock 驗證</span><span class="sxs-lookup"><span data-stu-id="8b21f-274">Mock authentication</span></span>

<span data-ttu-id="8b21f-275">類別中的測試 `AuthTests` 會檢查安全的端點：</span><span class="sxs-lookup"><span data-stu-id="8b21f-275">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="8b21f-276">將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-276">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="8b21f-277">傳回已驗證使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="8b21f-277">Returns content for an authenticated user.</span></span>

<span data-ttu-id="8b21f-278">在 SUT 中， `/SecurePage` 頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-278">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="8b21f-279">如需詳細資訊，請參閱[Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-279">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="8b21f-280">在 `Get_SecurePageRedirectsAnUnauthenticatedUser` 測試中， [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向，方法是將[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)設為 `false` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-280">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="8b21f-281">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="8b21f-281">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="8b21f-282">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="8b21f-282">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="8b21f-283">`Location`系統會檢查回應標頭中的標頭值，以確認其開頭為 `http://localhost/Identity/Account/Login` ，而不是最終的登入頁面回應，其中 `Location` 標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="8b21f-283">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="8b21f-284">測試應用程式可以 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 在[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中模擬，以便測試驗證和授權的層面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-284">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="8b21f-285">最小案例會傳回[AuthenticateResult。成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*)：</span><span class="sxs-lookup"><span data-stu-id="8b21f-285">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="8b21f-286">`TestAuthHandler`當驗證配置設定為 `Test` （其中已註冊的）時，會呼叫來驗證使用者 `AddAuthentication` `ConfigureTestServices` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-286">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="8b21f-287">如需的詳細資訊 `WebApplicationFactoryClientOptions` ，請參閱[用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="8b21f-287">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="8b21f-288">設定環境</span><span class="sxs-lookup"><span data-stu-id="8b21f-288">Set the environment</span></span>

<span data-ttu-id="8b21f-289">根據預設，會將 SUT 的主機和應用程式環境設定為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="8b21f-289">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="8b21f-290">若要在使用時覆寫 SUT 的環境 `IHostBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-290">To override the SUT's environment when using `IHostBuilder`:</span></span>

* <span data-ttu-id="8b21f-291">設定 `ASPNETCORE_ENVIRONMENT` 環境變數（例如，、 `Staging` `Production` 或其他自訂值，例如 `Testing` ）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-291">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="8b21f-292">覆寫 `CreateHostBuilder` 測試應用程式中的，以讀取前面加上的環境變數 `ASPNETCORE` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-292">Override `CreateHostBuilder` in the test app to read environment variables prefixed with `ASPNETCORE`.</span></span>

```csharp
protected override IHostBuilder CreateHostBuilder() =>
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

<span data-ttu-id="8b21f-293">如果 SUT 使用 Web 主機（ `IWebHostBuilder` ），請覆寫 `CreateWebHostBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-293">If the SUT uses the Web Host (`IWebHostBuilder`), override `CreateWebHostBuilder`:</span></span>

```csharp
protected override IWebHostBuilder CreateWebHostBuilder() =>
    base.CreateWebHostBuilder().UseEnvironment("Testing");
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="8b21f-294">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="8b21f-294">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="8b21f-295">此函式會藉 `WebApplicationFactory` 由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式[內容根](xref:fundamentals/index#content-root)路徑，其中包含的索引鍵等於元件的整合測試 `TEntryPoint` `System.Reflection.Assembly.FullName` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-295">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="8b21f-296">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory` 就會回到搜尋方案檔（*.sln*），並將 `TEntryPoint` 元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="8b21f-296">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="8b21f-297">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="8b21f-297">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="8b21f-298">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="8b21f-298">Disable shadow copying</span></span>

<span data-ttu-id="8b21f-299">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-299">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="8b21f-300">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="8b21f-300">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="8b21f-301">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="8b21f-301">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="8b21f-302">如需詳細資訊，請參閱[使用 JSON 設定 xUnit](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-302">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="8b21f-303">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="8b21f-303">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a><span data-ttu-id="8b21f-304">物件的處置</span><span class="sxs-lookup"><span data-stu-id="8b21f-304">Disposal of objects</span></span>

<span data-ttu-id="8b21f-305">執行此實作為測試之後 `IClassFixture` ，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-305">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="8b21f-306">如果開發人員所具現化的物件需要處置，請在執行時處置它們 `IClassFixture` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-306">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="8b21f-307">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-307">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="8b21f-308">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="8b21f-308">Integration tests sample</span></span>

<span data-ttu-id="8b21f-309">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="8b21f-309">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="8b21f-310">App</span><span class="sxs-lookup"><span data-stu-id="8b21f-310">App</span></span> | <span data-ttu-id="8b21f-311">專案目錄</span><span class="sxs-lookup"><span data-stu-id="8b21f-311">Project directory</span></span> | <span data-ttu-id="8b21f-312">說明</span><span class="sxs-lookup"><span data-stu-id="8b21f-312">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="8b21f-313">訊息應用程式（SUT）</span><span class="sxs-lookup"><span data-stu-id="8b21f-313">Message app (the SUT)</span></span> | <span data-ttu-id="8b21f-314">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="8b21f-314">*src/RazorPagesProject*</span></span> | <span data-ttu-id="8b21f-315">可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="8b21f-315">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="8b21f-316">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="8b21f-316">Test app</span></span> | <span data-ttu-id="8b21f-317">*測試/RazorPagesProject。測試*</span><span class="sxs-lookup"><span data-stu-id="8b21f-317">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="8b21f-318">用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-318">Used to integration test the SUT.</span></span> |

<span data-ttu-id="8b21f-319">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-319">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="8b21f-320">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="8b21f-320">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```console
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="8b21f-321">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="8b21f-321">Message app (SUT) organization</span></span>

<span data-ttu-id="8b21f-322">SUT 是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="8b21f-322">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="8b21f-323">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-323">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="8b21f-324">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-324">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="8b21f-325">`Text`屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="8b21f-325">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="8b21f-326">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="8b21f-326">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="8b21f-327">應用程式在其資料庫內容類別（ `AppDbContext` *Data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-327">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="8b21f-328">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="8b21f-328">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="8b21f-329">應用程式包含 `/SecurePage` 只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="8b21f-329">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="8b21f-330">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="8b21f-330">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="8b21f-331">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="8b21f-331">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="8b21f-332">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="8b21f-332">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="8b21f-333">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-333">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="8b21f-334">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-334">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="8b21f-335">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="8b21f-335">Test app organization</span></span>

<span data-ttu-id="8b21f-336">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-336">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="8b21f-337">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="8b21f-337">Test app directory</span></span> | <span data-ttu-id="8b21f-338">說明</span><span class="sxs-lookup"><span data-stu-id="8b21f-338">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="8b21f-339">*AuthTests*</span><span class="sxs-lookup"><span data-stu-id="8b21f-339">*AuthTests*</span></span> | <span data-ttu-id="8b21f-340">包含的測試方法：</span><span class="sxs-lookup"><span data-stu-id="8b21f-340">Contains test methods for:</span></span><ul><li><span data-ttu-id="8b21f-341">以未驗證的使用者存取安全頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-341">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="8b21f-342">使用 mock 存取已驗證使用者的安全頁面 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-342">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="8b21f-343">取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="8b21f-343">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> |
| <span data-ttu-id="8b21f-344">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="8b21f-344">*BasicTests*</span></span> | <span data-ttu-id="8b21f-345">包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="8b21f-345">Contains a test method for routing and content type.</span></span> |
| <span data-ttu-id="8b21f-346">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="8b21f-346">*IntegrationTests*</span></span> | <span data-ttu-id="8b21f-347">包含使用自訂類別之索引頁面的整合測試 `WebApplicationFactory` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-347">Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="8b21f-348">*Helper/公用程式*</span><span class="sxs-lookup"><span data-stu-id="8b21f-348">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="8b21f-349">*Utilities.cs*包含 `InitializeDbForTests` 用來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="8b21f-349">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="8b21f-350">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument` 供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="8b21f-350">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="8b21f-351">*HttpClientExtensions.cs*提供的多載， `SendAsync` 以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-351">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="8b21f-352">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-352">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="8b21f-353">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-353">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="8b21f-354">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器，因此 `TestHost` 和封裝不需要測試應用程式的 `TestServer` 專案檔或測試應用程式中的開發人員設定中的直接套件參考。</span><span class="sxs-lookup"><span data-stu-id="8b21f-354">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="8b21f-355">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="8b21f-355">**Seeding the database for testing**</span></span>

<span data-ttu-id="8b21f-356">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="8b21f-356">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="8b21f-357">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="8b21f-357">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="8b21f-358">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="8b21f-358">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

<span data-ttu-id="8b21f-359">SUT 的資料庫內容會在其方法中註冊 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-359">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8b21f-360">測試應用程式的 `builder.ConfigureServices` 回呼會在*after*應用程式的程式 `Startup.ConfigureServices` 代碼執行之後執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-360">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="8b21f-361">若要針對測試使用不同的資料庫，則必須在中取代應用程式的資料庫內容 `builder.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-361">To use a different database for the tests, the app's database context must be replaced in `builder.ConfigureServices`.</span></span> <span data-ttu-id="8b21f-362">如需詳細資訊，請參閱[自訂 WebApplicationFactory](#customize-webapplicationfactory)一節。</span><span class="sxs-lookup"><span data-stu-id="8b21f-362">For more information, see the [Customize WebApplicationFactory](#customize-webapplicationfactory) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8b21f-363">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="8b21f-363">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="8b21f-364">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-364">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="8b21f-365">本主題假設對單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="8b21f-365">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="8b21f-366">如果不熟悉測試概念，請參閱[.Net Core 中的單元測試和 .NET Standard](/dotnet/core/testing/)主題及其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="8b21f-366">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="8b21f-367">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8b21f-367">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8b21f-368">範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="8b21f-368">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="8b21f-369">如果不熟悉 Razor Pages，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="8b21f-369">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="8b21f-370">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="8b21f-370">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="8b21f-371">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8b21f-371">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="8b21f-372">Razor Pages 單元測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-372">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="8b21f-373">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="8b21f-373">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="8b21f-374">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="8b21f-374">Introduction to integration tests</span></span>

<span data-ttu-id="8b21f-375">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-375">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="8b21f-376">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="8b21f-376">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="8b21f-377">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-377">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="8b21f-378">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="8b21f-378">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="8b21f-379">資料庫</span><span class="sxs-lookup"><span data-stu-id="8b21f-379">Database</span></span>
* <span data-ttu-id="8b21f-380">檔案系統</span><span class="sxs-lookup"><span data-stu-id="8b21f-380">File system</span></span>
* <span data-ttu-id="8b21f-381">網路設備</span><span class="sxs-lookup"><span data-stu-id="8b21f-381">Network appliances</span></span>
* <span data-ttu-id="8b21f-382">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="8b21f-382">Request-response pipeline</span></span>

<span data-ttu-id="8b21f-383">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-383">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="8b21f-384">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="8b21f-384">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="8b21f-385">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-385">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="8b21f-386">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="8b21f-386">Require more code and data processing.</span></span>
* <span data-ttu-id="8b21f-387">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="8b21f-387">Take longer to run.</span></span>

<span data-ttu-id="8b21f-388">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="8b21f-388">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="8b21f-389">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-389">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="8b21f-390">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-390">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="8b21f-391">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="8b21f-391">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="8b21f-392">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-392">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="8b21f-393">在單元測試中，使用基礎結構 fakes/模擬會產生更快速的測試執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-393">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="8b21f-394">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="8b21f-394">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="8b21f-395">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="8b21f-395">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="8b21f-396">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-396">ASP.NET Core integration tests</span></span>

<span data-ttu-id="8b21f-397">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="8b21f-397">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="8b21f-398">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-398">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="8b21f-399">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="8b21f-399">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="8b21f-400">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="8b21f-400">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="8b21f-401">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="8b21f-401">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="8b21f-402">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="8b21f-402">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="8b21f-403">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="8b21f-403">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="8b21f-404">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-404">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="8b21f-405">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="8b21f-405">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="8b21f-406">執行*Act*測試步驟：用戶端提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="8b21f-406">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="8b21f-407">執行判斷*提示測試步驟*：*實際*的回應會根據*預期*的回應驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="8b21f-407">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="8b21f-408">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="8b21f-408">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="8b21f-409">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="8b21f-409">The test results are reported.</span></span>

<span data-ttu-id="8b21f-410">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="8b21f-410">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="8b21f-411">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="8b21f-411">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="8b21f-412">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="8b21f-412">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="8b21f-413">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-413">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="8b21f-414">此 `Microsoft.AspNetCore.Mvc.Testing` 套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="8b21f-414">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="8b21f-415">將相依性檔案（*. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="8b21f-415">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="8b21f-416">將[內容根目錄](xref:fundamentals/index#content-root)設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="8b21f-416">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="8b21f-417">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用來啟動的 SUT `TestServer` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-417">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="8b21f-418">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="8b21f-418">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="8b21f-419">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="8b21f-419">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="8b21f-420">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="8b21f-420">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="8b21f-421">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="8b21f-421">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="8b21f-422">Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="8b21f-422">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="8b21f-423">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-423">The only difference is in how the tests are named.</span></span> <span data-ttu-id="8b21f-424">在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如，用 `IndexPageTests` 來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-424">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="8b21f-425">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名（例如， `HomeControllerTests` 測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-425">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="8b21f-426">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="8b21f-426">Test app prerequisites</span></span>

<span data-ttu-id="8b21f-427">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="8b21f-427">The test project must:</span></span>

* <span data-ttu-id="8b21f-428">參考下列套件：</span><span class="sxs-lookup"><span data-stu-id="8b21f-428">Reference the following packages:</span></span>
  * [<span data-ttu-id="8b21f-429">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="8b21f-429">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [<span data-ttu-id="8b21f-430">AspNetCore 測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-430">Microsoft.AspNetCore.Mvc.Testing</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* <span data-ttu-id="8b21f-431">在專案檔中指定 Web SDK （ `<Project Sdk="Microsoft.NET.Sdk.Web">` ）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-431">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span> <span data-ttu-id="8b21f-432">參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)時，需要 Web SDK。</span><span class="sxs-lookup"><span data-stu-id="8b21f-432">The Web SDK is required when referencing the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="8b21f-433">這些必要條件可在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="8b21f-433">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="8b21f-434">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="8b21f-434">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="8b21f-435">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="8b21f-435">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="8b21f-436">xunit</span><span class="sxs-lookup"><span data-stu-id="8b21f-436">xunit</span></span>](https://www.nuget.org/packages/xunit/)
* [<span data-ttu-id="8b21f-437">xunit。 visualstudio</span><span class="sxs-lookup"><span data-stu-id="8b21f-437">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [<span data-ttu-id="8b21f-438">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="8b21f-438">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a><span data-ttu-id="8b21f-439">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="8b21f-439">SUT environment</span></span>

<span data-ttu-id="8b21f-440">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="8b21f-440">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="8b21f-441">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-441">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="8b21f-442">[WebApplicationFactory \<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)是用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-442">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="8b21f-443">`TEntryPoint`是 SUT 的進入點類別，通常是 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="8b21f-443">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="8b21f-444">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="8b21f-444">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="8b21f-445">下列測試類別會 `BasicTests` 使用 `WebApplicationFactory` 來啟動 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法 `Get_EndpointsReturnSuccessAndCorrectContentType` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-445">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="8b21f-446">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而 `Content-Type` 標頭則 `text/html; charset=utf-8` 適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-446">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="8b21f-447">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) `HttpClient` 會建立自動遵循重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="8b21f-447">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="8b21f-448">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="8b21f-448">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="8b21f-449">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="8b21f-449">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="8b21f-450">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-450">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="8b21f-451">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="8b21f-451">Customize WebApplicationFactory</span></span>

<span data-ttu-id="8b21f-452">藉由繼承自 `WebApplicationFactory` 來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="8b21f-452">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="8b21f-453">繼承自 `WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-453">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="8b21f-454">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：</span><span class="sxs-lookup"><span data-stu-id="8b21f-454">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="8b21f-455">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的資料庫植入是由 `InitializeDbForTests` 方法執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-455">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="8b21f-456">此方法會在[整合測試範例：測試應用程式組織](#test-app-organization)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="8b21f-456">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="8b21f-457">使用 `CustomWebApplicationFactory` 測試類別中的自訂。</span><span class="sxs-lookup"><span data-stu-id="8b21f-457">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="8b21f-458">下列範例會在類別中使用 factory `IndexPageTests` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-458">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="8b21f-459">範例應用程式的用戶端設定為防止 `HttpClient` 下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="8b21f-459">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="8b21f-460">如稍後的模擬[驗證](#mock-authentication)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="8b21f-460">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="8b21f-461">第一個回應是其中許多測試中具有標頭的重新導向 `Location` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-461">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="8b21f-462">一般測試會使用 `HttpClient` 和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="8b21f-462">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="8b21f-463">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="8b21f-463">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="8b21f-464">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="8b21f-464">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="8b21f-465">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="8b21f-465">Make a request for the page.</span></span>
1. <span data-ttu-id="8b21f-466">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="8b21f-466">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="8b21f-467">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="8b21f-467">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="8b21f-468">`SendAsync`範例應用程式中的協助程式擴充方法（helper */HttpClientExtensions*）和 `GetDocumentAsync` helper 方法（helper */HtmlHelpers*）會使用[sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) [AngleSharp](https://anglesharp.github.io/)剖析器，利用下列方法來處理 antiforgery 檢查：</span><span class="sxs-lookup"><span data-stu-id="8b21f-468">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="8b21f-469">`GetDocumentAsync`&ndash;接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回 `IHtmlDocument` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-469">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="8b21f-470">`GetDocumentAsync`使用可根據原始的來準備*虛擬回應*的 factory `HttpResponseMessage` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-470">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="8b21f-471">如需詳細資訊，請參閱[AngleSharp 檔](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-471">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="8b21f-472">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-472">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="8b21f-473">的多載會 `SendAsync` 接受 HTML 表單（ `IHtmlFormElement` ）和下列內容：</span><span class="sxs-lookup"><span data-stu-id="8b21f-473">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="8b21f-474">表單的 [提交] 按鈕（ `IHtmlElement` ）</span><span class="sxs-lookup"><span data-stu-id="8b21f-474">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="8b21f-475">表單值集合（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="8b21f-475">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="8b21f-476">提交按鈕（ `IHtmlElement` ）和表單值（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="8b21f-476">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="8b21f-477">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="8b21f-477">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="8b21f-478">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="8b21f-478">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="8b21f-479">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-479">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="8b21f-480">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="8b21f-480">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="8b21f-481">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="8b21f-481">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="8b21f-482">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) `WebApplicationFactory` 會使用設定進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="8b21f-482">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="8b21f-483">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會示範的用法 `WithWebHostBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-483">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="8b21f-484">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="8b21f-484">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="8b21f-485">因為類別中的另一項測試 `IndexPageTests` 會執行刪除資料庫中所有記錄的作業，而且可能會在此方法之前執行，所以會 `Post_DeleteMessageHandler_ReturnsRedirectToRoot` 在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="8b21f-485">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="8b21f-486">在 sut 的要求中，選取該表單的第一個 [刪除] 按鈕 `messages` 會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="8b21f-486">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="8b21f-487">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="8b21f-487">Client options</span></span>

<span data-ttu-id="8b21f-488">下表顯示建立實例時[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)可用的預設 WebApplicationFactoryClientOptions `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-488">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="8b21f-489">選項</span><span class="sxs-lookup"><span data-stu-id="8b21f-489">Option</span></span> | <span data-ttu-id="8b21f-490">描述</span><span class="sxs-lookup"><span data-stu-id="8b21f-490">Description</span></span> | <span data-ttu-id="8b21f-491">預設</span><span class="sxs-lookup"><span data-stu-id="8b21f-491">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="8b21f-492">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="8b21f-492">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="8b21f-493">取得或設定 `HttpClient` 實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="8b21f-493">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="8b21f-494">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="8b21f-494">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="8b21f-495">取得或設定實例的基底位址 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-495">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="8b21f-496">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="8b21f-496">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="8b21f-497">取得或設定 `HttpClient` 實例是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="8b21f-497">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="8b21f-498">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="8b21f-498">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="8b21f-499">取得或設定實例應遵循的重新導向回應數目上限 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-499">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="8b21f-500">7</span><span class="sxs-lookup"><span data-stu-id="8b21f-500">7</span></span> |

<span data-ttu-id="8b21f-501">建立 `WebApplicationFactoryClientOptions` 類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法（程式碼範例中會顯示預設值）：</span><span class="sxs-lookup"><span data-stu-id="8b21f-501">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="8b21f-502">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="8b21f-502">Inject mock services</span></span>

<span data-ttu-id="8b21f-503">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="8b21f-503">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="8b21f-504">**若要插入模擬服務，SUT 必須具有 `Startup` 具有方法的類別 `Startup.ConfigureServices` 。**</span><span class="sxs-lookup"><span data-stu-id="8b21f-504">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="8b21f-505">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="8b21f-505">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="8b21f-506">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="8b21f-506">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="8b21f-507">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-507">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="8b21f-508">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-508">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="8b21f-509">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-509">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="8b21f-510">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-510">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="8b21f-511">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-511">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="8b21f-512">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="8b21f-512">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="8b21f-513">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-513">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="8b21f-514">Mock 服務會將應用程式的取代為 `QuoteService` 測試應用程式所提供的服務，稱為 `TestQuoteService` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-514">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="8b21f-515">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="8b21f-515">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="8b21f-516">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="8b21f-516">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="8b21f-517">在測試執行期間產生的標記會反映所提供的引號文字 `TestQuoteService` ，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="8b21f-517">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="8b21f-518">Mock 驗證</span><span class="sxs-lookup"><span data-stu-id="8b21f-518">Mock authentication</span></span>

<span data-ttu-id="8b21f-519">類別中的測試 `AuthTests` 會檢查安全的端點：</span><span class="sxs-lookup"><span data-stu-id="8b21f-519">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="8b21f-520">將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-520">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="8b21f-521">傳回已驗證使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="8b21f-521">Returns content for an authenticated user.</span></span>

<span data-ttu-id="8b21f-522">在 SUT 中， `/SecurePage` 頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-522">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="8b21f-523">如需詳細資訊，請參閱[Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-523">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="8b21f-524">在 `Get_SecurePageRedirectsAnUnauthenticatedUser` 測試中， [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向，方法是將[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)設為 `false` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-524">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="8b21f-525">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="8b21f-525">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="8b21f-526">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="8b21f-526">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="8b21f-527">`Location`系統會檢查回應標頭中的標頭值，以確認其開頭為 `http://localhost/Identity/Account/Login` ，而不是最終的登入頁面回應，其中 `Location` 標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="8b21f-527">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="8b21f-528">測試應用程式可以 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 在[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中模擬，以便測試驗證和授權的層面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-528">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="8b21f-529">最小案例會傳回[AuthenticateResult。成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*)：</span><span class="sxs-lookup"><span data-stu-id="8b21f-529">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="8b21f-530">`TestAuthHandler`當驗證配置設定為 `Test` （其中已註冊的）時，會呼叫來驗證使用者 `AddAuthentication` `ConfigureTestServices` ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-530">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="8b21f-531">如需的詳細資訊 `WebApplicationFactoryClientOptions` ，請參閱[用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="8b21f-531">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="8b21f-532">設定環境</span><span class="sxs-lookup"><span data-stu-id="8b21f-532">Set the environment</span></span>

<span data-ttu-id="8b21f-533">根據預設，會將 SUT 的主機和應用程式環境設定為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="8b21f-533">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="8b21f-534">若要覆寫 SUT 的環境：</span><span class="sxs-lookup"><span data-stu-id="8b21f-534">To override the SUT's environment:</span></span>

* <span data-ttu-id="8b21f-535">設定 `ASPNETCORE_ENVIRONMENT` 環境變數（例如，、 `Staging` `Production` 或其他自訂值，例如 `Testing` ）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-535">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="8b21f-536">`CreateWebHostBuilder`在測試應用程式中覆寫以讀取 `ASPNETCORE_ENVIRONMENT` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="8b21f-536">Override `CreateWebHostBuilder` in the test app to read the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span>

```csharp
public class CustomWebApplicationFactory<TStartup> 
    : WebApplicationFactory<TStartup> where TStartup: class
{
    protected override IWebHostBuilder CreateWebHostBuilder()
    {
        return base.CreateWebHostBuilder()
            .UseEnvironment(
                Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT"));
    }

    ...
}
```

<span data-ttu-id="8b21f-537">環境也可以直接在自訂的主機產生器上設定 <xref:Microsoft.AspNetCore.Mvc.Testing.WebApplicationFactory%601> ：</span><span class="sxs-lookup"><span data-stu-id="8b21f-537">The environment can also be set directly on the host builder in a custom <xref:Microsoft.AspNetCore.Mvc.Testing.WebApplicationFactory%601>:</span></span>

```csharp
public class CustomWebApplicationFactory<TStartup> 
    : WebApplicationFactory<TStartup> where TStartup: class
{
    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.UseEnvironment(
            Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT"));
    
        ...
    }

    ...
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="8b21f-538">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="8b21f-538">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="8b21f-539">此函式會藉 `WebApplicationFactory` 由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式[內容根](xref:fundamentals/index#content-root)路徑，其中包含的索引鍵等於元件的整合測試 `TEntryPoint` `System.Reflection.Assembly.FullName` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-539">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="8b21f-540">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory` 就會回到搜尋方案檔（*.sln*），並將 `TEntryPoint` 元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="8b21f-540">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="8b21f-541">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="8b21f-541">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="8b21f-542">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="8b21f-542">Disable shadow copying</span></span>

<span data-ttu-id="8b21f-543">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="8b21f-543">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="8b21f-544">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="8b21f-544">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="8b21f-545">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="8b21f-545">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="8b21f-546">如需詳細資訊，請參閱[使用 JSON 設定 xUnit](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-546">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="8b21f-547">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="8b21f-547">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

<span data-ttu-id="8b21f-548">如果使用 Visual Studio，請將檔案的 [**複製到輸出目錄**] 屬性設定為 [**永遠複製**]。</span><span class="sxs-lookup"><span data-stu-id="8b21f-548">If using Visual Studio, set the file's **Copy to Output Directory** property to **Copy always**.</span></span> <span data-ttu-id="8b21f-549">如果未使用 Visual Studio，請將 `Content` 目標新增至測試應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="8b21f-549">If not using Visual Studio, add a `Content` target to the test app's project file:</span></span>

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a><span data-ttu-id="8b21f-550">物件的處置</span><span class="sxs-lookup"><span data-stu-id="8b21f-550">Disposal of objects</span></span>

<span data-ttu-id="8b21f-551">執行此實作為測試之後 `IClassFixture` ，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-551">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="8b21f-552">如果開發人員所具現化的物件需要處置，請在執行時處置它們 `IClassFixture` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-552">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="8b21f-553">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-553">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="8b21f-554">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="8b21f-554">Integration tests sample</span></span>

<span data-ttu-id="8b21f-555">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="8b21f-555">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="8b21f-556">App</span><span class="sxs-lookup"><span data-stu-id="8b21f-556">App</span></span> | <span data-ttu-id="8b21f-557">專案目錄</span><span class="sxs-lookup"><span data-stu-id="8b21f-557">Project directory</span></span> | <span data-ttu-id="8b21f-558">說明</span><span class="sxs-lookup"><span data-stu-id="8b21f-558">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="8b21f-559">訊息應用程式（SUT）</span><span class="sxs-lookup"><span data-stu-id="8b21f-559">Message app (the SUT)</span></span> | <span data-ttu-id="8b21f-560">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="8b21f-560">*src/RazorPagesProject*</span></span> | <span data-ttu-id="8b21f-561">可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="8b21f-561">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="8b21f-562">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="8b21f-562">Test app</span></span> | <span data-ttu-id="8b21f-563">*測試/RazorPagesProject。測試*</span><span class="sxs-lookup"><span data-stu-id="8b21f-563">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="8b21f-564">用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-564">Used to integration test the SUT.</span></span> |

<span data-ttu-id="8b21f-565">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-565">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="8b21f-566">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="8b21f-566">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="8b21f-567">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="8b21f-567">Message app (SUT) organization</span></span>

<span data-ttu-id="8b21f-568">SUT 是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="8b21f-568">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="8b21f-569">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-569">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="8b21f-570">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-570">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="8b21f-571">`Text`屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="8b21f-571">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="8b21f-572">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="8b21f-572">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="8b21f-573">應用程式在其資料庫內容類別（ `AppDbContext` *Data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-573">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="8b21f-574">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="8b21f-574">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="8b21f-575">應用程式包含 `/SecurePage` 只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="8b21f-575">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="8b21f-576">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="8b21f-576">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="8b21f-577">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="8b21f-577">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="8b21f-578">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="8b21f-578">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="8b21f-579">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-579">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="8b21f-580">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="8b21f-580">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="8b21f-581">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="8b21f-581">Test app organization</span></span>

<span data-ttu-id="8b21f-582">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b21f-582">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="8b21f-583">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="8b21f-583">Test app directory</span></span> | <span data-ttu-id="8b21f-584">說明</span><span class="sxs-lookup"><span data-stu-id="8b21f-584">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="8b21f-585">*AuthTests*</span><span class="sxs-lookup"><span data-stu-id="8b21f-585">*AuthTests*</span></span> | <span data-ttu-id="8b21f-586">包含的測試方法：</span><span class="sxs-lookup"><span data-stu-id="8b21f-586">Contains test methods for:</span></span><ul><li><span data-ttu-id="8b21f-587">以未驗證的使用者存取安全頁面。</span><span class="sxs-lookup"><span data-stu-id="8b21f-587">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="8b21f-588">使用 mock 存取已驗證使用者的安全頁面 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-588">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="8b21f-589">取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="8b21f-589">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> |
| <span data-ttu-id="8b21f-590">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="8b21f-590">*BasicTests*</span></span> | <span data-ttu-id="8b21f-591">包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="8b21f-591">Contains a test method for routing and content type.</span></span> |
| <span data-ttu-id="8b21f-592">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="8b21f-592">*IntegrationTests*</span></span> | <span data-ttu-id="8b21f-593">包含使用自訂類別之索引頁面的整合測試 `WebApplicationFactory` 。</span><span class="sxs-lookup"><span data-stu-id="8b21f-593">Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="8b21f-594">*Helper/公用程式*</span><span class="sxs-lookup"><span data-stu-id="8b21f-594">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="8b21f-595">*Utilities.cs*包含 `InitializeDbForTests` 用來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="8b21f-595">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="8b21f-596">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument` 供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="8b21f-596">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="8b21f-597">*HttpClientExtensions.cs*提供的多載， `SendAsync` 以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="8b21f-597">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="8b21f-598">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-598">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="8b21f-599">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="8b21f-599">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="8b21f-600">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器，因此 `TestHost` 和封裝不需要測試應用程式的 `TestServer` 專案檔或測試應用程式中的開發人員設定中的直接套件參考。</span><span class="sxs-lookup"><span data-stu-id="8b21f-600">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="8b21f-601">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="8b21f-601">**Seeding the database for testing**</span></span>

<span data-ttu-id="8b21f-602">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="8b21f-602">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="8b21f-603">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="8b21f-603">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="8b21f-604">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="8b21f-604">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8b21f-605">其他資源</span><span class="sxs-lookup"><span data-stu-id="8b21f-605">Additional resources</span></span>

* [<span data-ttu-id="8b21f-606">單元測試</span><span class="sxs-lookup"><span data-stu-id="8b21f-606">Unit tests</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:test/razor-pages-tests>
* <xref:fundamentals/middleware/index>
* <xref:mvc/controllers/testing>
