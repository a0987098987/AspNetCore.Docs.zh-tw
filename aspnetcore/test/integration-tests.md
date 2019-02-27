---
title: ASP.NET Core 中的整合測試
author: guardrex
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: test/integration-tests
ms.openlocfilehash: 053713e148df70b0be6bb567b55b2381a78d6c3e
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833575"
---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="20c15-103">ASP.NET Core 中的整合測試</span><span class="sxs-lookup"><span data-stu-id="20c15-103">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="20c15-104">藉由[Luke Latham](https://github.com/guardrex)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="20c15-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="20c15-105">整合測試可確保應用程式的元件正確運作，其中包含應用程式的支援基礎結構，例如資料庫、 檔案系統和網路層級。</span><span class="sxs-lookup"><span data-stu-id="20c15-105">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="20c15-106">ASP.NET Core 支援使用測試 web 主機和記憶體測試伺服器中的單元測試架構的整合測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-106">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="20c15-107">本主題假設單元測試的基本知識。</span><span class="sxs-lookup"><span data-stu-id="20c15-107">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="20c15-108">如果熟悉測試概念，請參閱[.NET Core 和.NET Standard 中的單元測試](/dotnet/core/testing/)主題和其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="20c15-108">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="20c15-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="20c15-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="20c15-110">範例應用程式是 Razor 頁面應用程式，並假設 Razor 頁面的基本知識。</span><span class="sxs-lookup"><span data-stu-id="20c15-110">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="20c15-111">如果不熟悉使用 Razor 頁面，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="20c15-111">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="20c15-112">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="20c15-112">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="20c15-113">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20c15-113">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="20c15-114">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="20c15-114">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="20c15-115">進行測試的 Spa，我們建議工具這類[Selenium](https://www.seleniumhq.org/)，它可以自動在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="20c15-115">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="20c15-116">整合測試的簡介</span><span class="sxs-lookup"><span data-stu-id="20c15-116">Introduction to integration tests</span></span>

<span data-ttu-id="20c15-117">整合測試評估比更廣泛的層級上的應用程式的元件[單元測試](/dotnet/core/testing/)。</span><span class="sxs-lookup"><span data-stu-id="20c15-117">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="20c15-118">單元測試用來測試隔離的軟體元件，例如個別類別方法。</span><span class="sxs-lookup"><span data-stu-id="20c15-118">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="20c15-119">整合測試確認兩個或多個應用程式元件一起運作以產生預期的結果，可能包括 完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="20c15-119">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="20c15-120">這些更廣泛的測試用來測試應用程式的基礎結構和整個 framework，通常包括下列元件：</span><span class="sxs-lookup"><span data-stu-id="20c15-120">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="20c15-121">資料庫</span><span class="sxs-lookup"><span data-stu-id="20c15-121">Database</span></span>
* <span data-ttu-id="20c15-122">檔案系統</span><span class="sxs-lookup"><span data-stu-id="20c15-122">File system</span></span>
* <span data-ttu-id="20c15-123">網路設備</span><span class="sxs-lookup"><span data-stu-id="20c15-123">Network appliances</span></span>
* <span data-ttu-id="20c15-124">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="20c15-124">Request-response pipeline</span></span>

<span data-ttu-id="20c15-125">單元測試使用傳遞元件，稱為*fakes*或是*模擬物件*，來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="20c15-125">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="20c15-126">相較於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="20c15-126">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="20c15-127">使用實際的應用程式在生產環境中使用的元件。</span><span class="sxs-lookup"><span data-stu-id="20c15-127">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="20c15-128">需要更多的程式碼及資料處理。</span><span class="sxs-lookup"><span data-stu-id="20c15-128">Require more code and data processing.</span></span>
* <span data-ttu-id="20c15-129">需要較長的時間。</span><span class="sxs-lookup"><span data-stu-id="20c15-129">Take longer to run.</span></span>

<span data-ttu-id="20c15-130">因此，限制使用的最重要的基礎結構案例的整合測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-130">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="20c15-131">如果可以使用單元測試或整合測試，測試行為，請選擇 單元測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-131">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="20c15-132">不要撰寫每個可能的排列的資料與檔案存取資料庫和檔案系統的整合測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-132">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="20c15-133">不論多少位數跨應用程式會與資料庫和檔案系統、 特定的一份讀取、 寫入、 更新和刪除的整合測試通常能夠適當測試資料庫和檔案系統元件互動。</span><span class="sxs-lookup"><span data-stu-id="20c15-133">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="20c15-134">使用單元測試的方法的邏輯與這些元件互動的例行性測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-134">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="20c15-135">在單元測試中的基礎結構使用 fakes/模擬 （mock) 更快速的測試執行中的結果。</span><span class="sxs-lookup"><span data-stu-id="20c15-135">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="20c15-136">在討論中的整合測試，測試的專案時經常會呼叫*待測系統*，或簡稱為 「 SUT"。</span><span class="sxs-lookup"><span data-stu-id="20c15-136">In discussions of integration tests, the tested project is frequently called the *system under test*, or "SUT" for short.</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="20c15-137">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="20c15-137">ASP.NET Core integration tests</span></span>

<span data-ttu-id="20c15-138">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="20c15-138">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="20c15-139">測試專案用來包含及執行測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-139">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="20c15-140">測試專案已測試的 ASP.NET Core 專案，稱為參考*待測系統*(SUT)。</span><span class="sxs-lookup"><span data-stu-id="20c15-140">The test project has a reference to the tested ASP.NET Core project, called the *system under test* (SUT).</span></span> <span data-ttu-id="20c15-141">_「 SUT 」 在本主題中用以參考該測試的應用程式。_</span><span class="sxs-lookup"><span data-stu-id="20c15-141">_"SUT" is used throughout this topic to refer to the tested app._</span></span>
* <span data-ttu-id="20c15-142">測試專案建立 SUT 測試 web 主機，並使用測試伺服器用戶端處理要求和回應 SUT。</span><span class="sxs-lookup"><span data-stu-id="20c15-142">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses to the SUT.</span></span>
* <span data-ttu-id="20c15-143">測試執行器用來執行測試和報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="20c15-143">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="20c15-144">整合測試會遵循包含一般的事件序列*排列*， *Act*，並*Assert*測試步驟：</span><span class="sxs-lookup"><span data-stu-id="20c15-144">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="20c15-145">SUT 的 web 主機已設定。</span><span class="sxs-lookup"><span data-stu-id="20c15-145">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="20c15-146">測試伺服器用戶端會提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="20c15-146">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="20c15-147">*排列*測試步驟執行：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="20c15-147">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="20c15-148">*Act*測試步驟執行：用戶端會提交要求，並接收回應。</span><span class="sxs-lookup"><span data-stu-id="20c15-148">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="20c15-149">*Assert*測試步驟執行：*實際*回應會驗證為*傳遞*或是*失敗*根據*預期*回應。</span><span class="sxs-lookup"><span data-stu-id="20c15-149">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="20c15-150">處理程序會繼續直到所有的測試會執行。</span><span class="sxs-lookup"><span data-stu-id="20c15-150">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="20c15-151">報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="20c15-151">The test results are reported.</span></span>

<span data-ttu-id="20c15-152">通常，測試 web 主機設定以不同的方式比執行測試的應用程式的一般的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="20c15-152">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="20c15-153">比方說，不同的資料庫或不同的應用程式設定可能會用來測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-153">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="20c15-154">基礎結構元件，例如測試 web 主機和記憶體測試伺服器 ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver))、 提供或由[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)封裝。</span><span class="sxs-lookup"><span data-stu-id="20c15-154">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="20c15-155">使用此封裝可以簡化測試建立和執行。</span><span class="sxs-lookup"><span data-stu-id="20c15-155">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="20c15-156">`Microsoft.AspNetCore.Mvc.Testing`封裝處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="20c15-156">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="20c15-157">將相依性檔案複製 (*\*.deps*) 從到測試專案的 SUT *bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c15-157">Copies the dependencies file (*\*.deps*) from the SUT into the test project's *bin* folder.</span></span>
* <span data-ttu-id="20c15-158">設定 SUT 專案根目錄的內容根目錄，因此，靜態檔案和頁面/檢視表位於測試執行時。</span><span class="sxs-lookup"><span data-stu-id="20c15-158">Sets the content root to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="20c15-159">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別來簡化啟動程序與 SUT `TestServer`。</span><span class="sxs-lookup"><span data-stu-id="20c15-159">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="20c15-160">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件說明如何設定測試專案和測試執行器，以及如何執行測試和建議方法，名稱測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="20c15-160">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="20c15-161">建立測試專案的應用程式時，分開的單元測試整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="20c15-161">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="20c15-162">這有助於確保，測試基礎結構的元件不小心包含在單元測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-162">This helps ensure that infrastructure testing components aren't accidently included in the unit tests.</span></span> <span data-ttu-id="20c15-163">區隔單元和整合測試也可讓控制哪些資料集的測試的執行。</span><span class="sxs-lookup"><span data-stu-id="20c15-163">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="20c15-164">沒有幾乎任何的 Razor Pages 應用程式的測試組態和 MVC 應用程式之間的差異。</span><span class="sxs-lookup"><span data-stu-id="20c15-164">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="20c15-165">唯一的差別是在測試命名的方式。</span><span class="sxs-lookup"><span data-stu-id="20c15-165">The only difference is in how the tests are named.</span></span> <span data-ttu-id="20c15-166">在 Razor 頁面應用程式中測試的頁面端點通常命名為之後的頁面模型類別 (例如`IndexPageTests`來測試索引頁面的元件整合)。</span><span class="sxs-lookup"><span data-stu-id="20c15-166">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="20c15-167">在 MVC 應用程式中，測試會通常依控制器類別並命名這些測試的控制站 (例如`HomeControllerTests`測試針對 Home 控制器元件整合)。</span><span class="sxs-lookup"><span data-stu-id="20c15-167">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="20c15-168">測試應用程式的必要條件</span><span class="sxs-lookup"><span data-stu-id="20c15-168">Test app prerequisites</span></span>

<span data-ttu-id="20c15-169">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="20c15-169">The test project must:</span></span>

* <span data-ttu-id="20c15-170">請參考下列套件：</span><span class="sxs-lookup"><span data-stu-id="20c15-170">Reference the following packages:</span></span>
  * [<span data-ttu-id="20c15-171">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="20c15-171">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [<span data-ttu-id="20c15-172">Microsoft.AspNetCore.Mvc.Testing</span><span class="sxs-lookup"><span data-stu-id="20c15-172">Microsoft.AspNetCore.Mvc.Testing</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* <span data-ttu-id="20c15-173">專案檔中指定 Web SDK (`<Project Sdk="Microsoft.NET.Sdk.Web">`)。</span><span class="sxs-lookup"><span data-stu-id="20c15-173">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span> <span data-ttu-id="20c15-174">Web SDK 時，必須參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="20c15-174">The Web SDK is required when referencing the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="20c15-175">這些必要條件中所見[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)。</span><span class="sxs-lookup"><span data-stu-id="20c15-175">These prerequisites can be seen in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="20c15-176">檢查*tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="20c15-176">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="20c15-177">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="20c15-177">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="20c15-178">xunit</span><span class="sxs-lookup"><span data-stu-id="20c15-178">xunit</span></span>](https://www.nuget.org/packages/xunit/)
* [<span data-ttu-id="20c15-179">xunit.runner.visualstudio</span><span class="sxs-lookup"><span data-stu-id="20c15-179">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [<span data-ttu-id="20c15-180">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="20c15-180">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a><span data-ttu-id="20c15-181">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="20c15-181">SUT environment</span></span>

<span data-ttu-id="20c15-182">如果 SUT[環境](xref:fundamentals/environments)未設定，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="20c15-182">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="20c15-183">預設值 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="20c15-183">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="20c15-184">[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)整合測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-184">[WebApplicationFactory&lt;TEntryPoint&gt;](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="20c15-185">`TEntryPoint` 通常是 SUT 的進入點類別`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="20c15-185">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="20c15-186">測試類別會實作*類別的裝置*介面 ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) 以指出包含測試類別，且提供共用的物件執行個體的類別中的測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-186">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

### <a name="basic-test-of-app-endpoints"></a><span data-ttu-id="20c15-187">基本測試的應用程式端點</span><span class="sxs-lookup"><span data-stu-id="20c15-187">Basic test of app endpoints</span></span>

<span data-ttu-id="20c15-188">下列測試類別， `BasicTests`，使用`WebApplicationFactory`SUT 啟動程序，並提供[HttpClient](/dotnet/api/system.net.http.httpclient)至測試方法， `Get_EndpointsReturnSuccessAndCorrectContentType`。</span><span class="sxs-lookup"><span data-stu-id="20c15-188">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="20c15-189">方法會檢查是否成功的回應狀態碼 （在 200-299 的範圍內的狀態碼） 和`Content-Type`標頭是`text/html; charset=utf-8`數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="20c15-189">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="20c15-190">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)建立的執行個體`HttpClient`，會自動遵循重新導向，並會處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="20c15-190">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a><span data-ttu-id="20c15-191">測試安全的端點</span><span class="sxs-lookup"><span data-stu-id="20c15-191">Test a secure endpoint</span></span>

<span data-ttu-id="20c15-192">中的另一個測試`BasicTests`類別會檢查安全端點重新導向未驗證的使用者，應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="20c15-192">Another test in the `BasicTests` class checks that a secure endpoint redirects an unauthenticated user to the app's Login page.</span></span>

<span data-ttu-id="20c15-193">SUT，在`/SecurePage`頁面上使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)來套用慣例[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)至頁面。</span><span class="sxs-lookup"><span data-stu-id="20c15-193">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="20c15-194">如需詳細資訊，請參閱 < [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="20c15-194">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="20c15-195">在 `Get_SecurePageRequiresAnAuthenticatedUser`測試，請[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)設為禁止重新導向設定[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)至`false`:</span><span class="sxs-lookup"><span data-stu-id="20c15-195">In the `Get_SecurePageRequiresAnAuthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

<span data-ttu-id="20c15-196">不允許用戶端以遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="20c15-196">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="20c15-197">SUT 所傳回的狀態碼可以檢查對預期[HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode)結果，不登入頁面，會重新導向後的最終狀態程式碼[HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span><span class="sxs-lookup"><span data-stu-id="20c15-197">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="20c15-198">`Location`回應標頭中的標頭值會檢查以確認它開頭`http://localhost/Identity/Account/Login`，不是最後登入頁面的回應，其中`Location`標頭不會出現。</span><span class="sxs-lookup"><span data-stu-id="20c15-198">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="20c15-199">如需詳細資訊`WebApplicationFactoryClientOptions`，請參閱 <<c2> [ 用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="20c15-199">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="20c15-200">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="20c15-200">Customize WebApplicationFactory</span></span>

<span data-ttu-id="20c15-201">Web 主機組態可以建立獨立的測試類別，藉由繼承自`WebApplicationFactory`建立一或多個自訂的處理站：</span><span class="sxs-lookup"><span data-stu-id="20c15-201">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="20c15-202">繼承自`WebApplicationFactory`，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="20c15-202">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="20c15-203">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)可讓服務集合的組態[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span><span class="sxs-lookup"><span data-stu-id="20c15-203">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="20c15-204">在植入的資料庫[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)由執行`InitializeDbForTests`方法。</span><span class="sxs-lookup"><span data-stu-id="20c15-204">Database seeding in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="20c15-205">中所述的方法是[整合測試的範例：測試應用程式組織](#test-app-organization)一節。</span><span class="sxs-lookup"><span data-stu-id="20c15-205">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="20c15-206">使用自訂`CustomWebApplicationFactory`測試類別中。</span><span class="sxs-lookup"><span data-stu-id="20c15-206">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="20c15-207">下列範例會使用中的處理站`IndexPageTests`類別：</span><span class="sxs-lookup"><span data-stu-id="20c15-207">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="20c15-208">範例應用程式的用戶端設定為防止`HttpClient`從下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="20c15-208">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="20c15-209">中所述[測試安全的端點](#test-a-secure-endpoint) 區段中，這可讓測試，以檢查應用程式的第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="20c15-209">As explained in the [Test a secure endpoint](#test-a-secure-endpoint) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="20c15-210">第一個回應是，有許多使用這些測試中的重新導向`Location`標頭。</span><span class="sxs-lookup"><span data-stu-id="20c15-210">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="20c15-211">一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="20c15-211">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="20c15-212">任何 SUT 的 POST 要求都必須滿足的應用程式時，會自動成為 antiforgery 核取[資料保護防偽系統](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="20c15-212">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="20c15-213">若要排列測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="20c15-213">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="20c15-214">提出要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="20c15-214">Make a request for the page.</span></span>
1. <span data-ttu-id="20c15-215">剖析的防偽 cookie 和回應的要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="20c15-215">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="20c15-216">在位置進行 POST 要求，使用防偽 cookie 和要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="20c15-216">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="20c15-217">`SendAsync`協助程式的擴充方法 (*Helpers/HttpClientExtensions.cs*) 和`GetDocumentAsync`helper 方法 (*Helpers/HtmlHelpers.cs*) 中[的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)使用[AngleSharp](https://anglesharp.github.io/)剖析器來處理 antiforgery 檢查使用下列方法：</span><span class="sxs-lookup"><span data-stu-id="20c15-217">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="20c15-218">`GetDocumentAsync` &ndash; 接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回`IHtmlDocument`。</span><span class="sxs-lookup"><span data-stu-id="20c15-218">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="20c15-219">`GetDocumentAsync` 使用準備的處理站*虛擬回應*根據原始`HttpResponseMessage`。</span><span class="sxs-lookup"><span data-stu-id="20c15-219">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="20c15-220">如需詳細資訊，請參閱 < [AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="20c15-220">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="20c15-221">`SendAsync` 擴充方法`HttpClient`compose [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)然後呼叫[SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)提交要求給 SUT。</span><span class="sxs-lookup"><span data-stu-id="20c15-221">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="20c15-222">多載`SendAsync`接受 HTML 表單 (`IHtmlFormElement`) 和下列：</span><span class="sxs-lookup"><span data-stu-id="20c15-222">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="20c15-223">提交按鈕的表單 (`IHtmlElement`)</span><span class="sxs-lookup"><span data-stu-id="20c15-223">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="20c15-224">表單值集合 (`IEnumerable<KeyValuePair<string, string>>`)</span><span class="sxs-lookup"><span data-stu-id="20c15-224">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="20c15-225">提交按鈕 (`IHtmlElement`)，從而形成值 (`IEnumerable<KeyValuePair<string, string>>`)</span><span class="sxs-lookup"><span data-stu-id="20c15-225">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="20c15-226">[AngleSharp](https://anglesharp.github.io/)第三方剖析供示範之用，本主題中的範例應用程式的程式庫。</span><span class="sxs-lookup"><span data-stu-id="20c15-226">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="20c15-227">AngleSharp 不支援，或整合測試的 ASP.NET Core 應用程式所需。</span><span class="sxs-lookup"><span data-stu-id="20c15-227">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="20c15-228">其他剖析器可以使用，例如[Html 靈活度組件 (HAP)](http://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="20c15-228">Other parsers can be used, such as the [Html Agility Pack (HAP)](http://html-agility-pack.net/).</span></span> <span data-ttu-id="20c15-229">另一種方法是撰寫程式碼來直接處理防偽系統的要求驗證語彙基元和防偽 cookie。</span><span class="sxs-lookup"><span data-stu-id="20c15-229">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="20c15-230">自訂 WithWebHostBuilder 的用戶端</span><span class="sxs-lookup"><span data-stu-id="20c15-230">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="20c15-231">在測試方法中，需要額外的設定時[WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)新建`WebApplicationFactory`具有[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，進一步自訂組態。</span><span class="sxs-lookup"><span data-stu-id="20c15-231">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="20c15-232">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`測試方法[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)示範如何使用`WithWebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="20c15-232">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="20c15-233">這項測試觸發 SUT 中的送出表單，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="20c15-233">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="20c15-234">因為另一個測試中`IndexPageTests`類別執行作業會刪除所有資料庫中的記錄，並可能執行之前`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法中，資料庫就會在這個測試方法，以確保有一筆記錄的刪除 SUT 植入。</span><span class="sxs-lookup"><span data-stu-id="20c15-234">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is seeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="20c15-235">選取`deleteBtn1`按鈕的`messages`SUT 中的表單模擬 SUT 的要求中：</span><span class="sxs-lookup"><span data-stu-id="20c15-235">Selecting the `deleteBtn1` button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="20c15-236">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="20c15-236">Client options</span></span>

<span data-ttu-id="20c15-237">下表顯示的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)建立時可以使用`HttpClient`執行個體。</span><span class="sxs-lookup"><span data-stu-id="20c15-237">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="20c15-238">選項</span><span class="sxs-lookup"><span data-stu-id="20c15-238">Option</span></span> | <span data-ttu-id="20c15-239">描述</span><span class="sxs-lookup"><span data-stu-id="20c15-239">Description</span></span> | <span data-ttu-id="20c15-240">預設</span><span class="sxs-lookup"><span data-stu-id="20c15-240">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="20c15-241">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="20c15-241">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="20c15-242">取得或設定是否`HttpClient`執行個體應該會自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="20c15-242">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="20c15-243">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="20c15-243">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="20c15-244">取得或設定基底位址`HttpClient`執行個體。</span><span class="sxs-lookup"><span data-stu-id="20c15-244">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="20c15-245">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="20c15-245">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="20c15-246">取得或設定是否`HttpClient`執行個體應該處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="20c15-246">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="20c15-247">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="20c15-247">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="20c15-248">取得或設定最大重新導向回應數目`HttpClient`應該遵循執行個體。</span><span class="sxs-lookup"><span data-stu-id="20c15-248">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="20c15-249">7</span><span class="sxs-lookup"><span data-stu-id="20c15-249">7</span></span> |

<span data-ttu-id="20c15-250">建立`WebApplicationFactoryClientOptions`類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法 （預設值在程式碼範例所示）：</span><span class="sxs-lookup"><span data-stu-id="20c15-250">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="20c15-251">插入模擬 （mock） 的服務</span><span class="sxs-lookup"><span data-stu-id="20c15-251">Inject mock services</span></span>

<span data-ttu-id="20c15-252">服務可以藉由呼叫測試中覆寫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)主應用程式產生器。</span><span class="sxs-lookup"><span data-stu-id="20c15-252">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="20c15-253">**若要插入模擬 （mock） 的服務，必須有 SUT`Startup`類別搭配`Startup.ConfigureServices`方法。**</span><span class="sxs-lookup"><span data-stu-id="20c15-253">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="20c15-254">此範例 SUT 包含範圍的服務會傳回報價。</span><span class="sxs-lookup"><span data-stu-id="20c15-254">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="20c15-255">要求 索引 頁面時索引 頁面上的隱藏欄位中內嵌引號。</span><span class="sxs-lookup"><span data-stu-id="20c15-255">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="20c15-256">*Services/IQuoteService.cs*:</span><span class="sxs-lookup"><span data-stu-id="20c15-256">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="20c15-257">*Services/QuoteService.cs*:</span><span class="sxs-lookup"><span data-stu-id="20c15-257">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="20c15-258">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="20c15-258">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="20c15-259">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="20c15-259">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="20c15-260">*Pages/Index.cs*:</span><span class="sxs-lookup"><span data-stu-id="20c15-260">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="20c15-261">SUT 應用程式執行時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="20c15-261">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="20c15-262">若要測試服務和引號插入在整合測試，模擬 （mock） 的服務會插入 SUT 測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-262">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="20c15-263">模擬 （mock） 的服務會取代應用程式的`QuoteService`測試應用程式所提供的服務，使用稱為`TestQuoteService`:</span><span class="sxs-lookup"><span data-stu-id="20c15-263">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="20c15-264">*IntegrationTests.IndexPageTests.cs*:</span><span class="sxs-lookup"><span data-stu-id="20c15-264">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="20c15-265">`ConfigureTestServices` 呼叫時，並已設定領域的服務註冊：</span><span class="sxs-lookup"><span data-stu-id="20c15-265">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="20c15-266">測試執行期間所產生的標記會反映所提供的報價文字`TestQuoteService`，因此判斷提示的階段：</span><span class="sxs-lookup"><span data-stu-id="20c15-266">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="20c15-267">如何測試基礎結構會推斷的應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="20c15-267">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="20c15-268">`WebApplicationFactory`建構函式會藉由搜尋來推斷應用程式內容根路徑[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)等於索引鍵包含整合測試的組件上`TEntryPoint`的組件`System.Reflection.Assembly.FullName`.</span><span class="sxs-lookup"><span data-stu-id="20c15-268">The `WebApplicationFactory` constructor infers the app content root path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="20c15-269">如果找不到具有正確金鑰的屬性，`WebApplicationFactory`回到搜尋解決方案檔案 (*\*.sln*)，並將附加`TEntryPoint`的方案目錄的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="20c15-269">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*\*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="20c15-270">應用程式根目錄 （內容根路徑） 用來探索檢視和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="20c15-270">The app root directory (the content root path) is used to discover views and content files.</span></span>

<span data-ttu-id="20c15-271">在大部分情況下，不需要明確地將應用程式內容根目錄設定，因為通常在執行階段尋找正確的內容根目錄的搜尋邏輯。</span><span class="sxs-lookup"><span data-stu-id="20c15-271">In most cases, it isn't necessary to explicitly set the app content root, as the search logic usually finds the correct content root at runtime.</span></span> <span data-ttu-id="20c15-272">未找到的內容根目錄的特殊案例中使用內建的搜尋演算法，內容可以指定根目錄，明確或使用自訂的邏輯應用程式。</span><span class="sxs-lookup"><span data-stu-id="20c15-272">In special scenarios where the content root isn't found using the built-in search algorithm, the app content root can be specified explicitly or by using custom logic.</span></span> <span data-ttu-id="20c15-273">若要設定應用程式的內容根目錄，在這些情況下，呼叫`UseSolutionRelativeContentRoot`擴充方法，從[Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost)封裝。</span><span class="sxs-lookup"><span data-stu-id="20c15-273">To set the app content root in those scenarios, call the `UseSolutionRelativeContentRoot` extension method from the [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) package.</span></span> <span data-ttu-id="20c15-274">提供方案的相對路徑和選擇性的方案檔案名稱或 glob 模式 (預設 = `*.sln`)。</span><span class="sxs-lookup"><span data-stu-id="20c15-274">Supply the solution's relative path and optional solution file name or glob pattern (default = `*.sln`).</span></span>

<span data-ttu-id="20c15-275">呼叫[UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot)擴充方法使用*一個*下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="20c15-275">Call the [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) extension method using *ONE* of the following approaches:</span></span>

* <span data-ttu-id="20c15-276">設定測試類別時`WebApplicationFactory`，提供的自訂組態[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):</span><span class="sxs-lookup"><span data-stu-id="20c15-276">When configuring test classes with `WebApplicationFactory`, provide a custom configuration with the [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):</span></span>

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* <span data-ttu-id="20c15-277">使用自訂設定測試類別時`WebApplicationFactory`，繼承自`WebApplicationFactory`，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):</span><span class="sxs-lookup"><span data-stu-id="20c15-277">When configuring test classes with a custom `WebApplicationFactory`, inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):</span></span>

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a><span data-ttu-id="20c15-278">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="20c15-278">Disable shadow copying</span></span>

<span data-ttu-id="20c15-279">陰影複製會導致在不同的輸出資料夾的資料夾中執行測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-279">Shadow copying causes the tests to execute in a different folder than the output folder.</span></span> <span data-ttu-id="20c15-280">針對測試才能正常運作，陰影複製必須停用。</span><span class="sxs-lookup"><span data-stu-id="20c15-280">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="20c15-281">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)xunit，並停用所包括的 xUnit 陰影複製*xunit.runner.json*檔案使用的是正確的組態設定。</span><span class="sxs-lookup"><span data-stu-id="20c15-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="20c15-282">如需詳細資訊，請參閱 <<c0> [ 設定使用 JSON 的 xUnit.net](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="20c15-282">For more information, see [Configuring xUnit.net with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="20c15-283">新增*xunit.runner.json*檔案根目錄的測試專案，使用下列內容：</span><span class="sxs-lookup"><span data-stu-id="20c15-283">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a><span data-ttu-id="20c15-284">物件的處置</span><span class="sxs-lookup"><span data-stu-id="20c15-284">Disposal of objects</span></span>

<span data-ttu-id="20c15-285">在測試後`IClassFixture`實作會執行， [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)並[HttpClient](/dotnet/api/system.net.http.httpclient) xUnit 處置時進行處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span><span class="sxs-lookup"><span data-stu-id="20c15-285">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="20c15-286">如果物件具現化開發人員需要處置，處置它們在`IClassFixture`實作。</span><span class="sxs-lookup"><span data-stu-id="20c15-286">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="20c15-287">如需詳細資訊，請參閱 <<c0> [ 實作 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="20c15-287">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="20c15-288">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="20c15-288">Integration tests sample</span></span>

<span data-ttu-id="20c15-289">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)是兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="20c15-289">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="20c15-290">應用程式</span><span class="sxs-lookup"><span data-stu-id="20c15-290">App</span></span> | <span data-ttu-id="20c15-291">專案資料夾</span><span class="sxs-lookup"><span data-stu-id="20c15-291">Project folder</span></span> | <span data-ttu-id="20c15-292">描述</span><span class="sxs-lookup"><span data-stu-id="20c15-292">Description</span></span> |
| --- | -------------- | ----------- |
| <span data-ttu-id="20c15-293">訊息應用程式 (SUT)</span><span class="sxs-lookup"><span data-stu-id="20c15-293">Message app (the SUT)</span></span> | <span data-ttu-id="20c15-294">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="20c15-294">*src/RazorPagesProject*</span></span> | <span data-ttu-id="20c15-295">允許使用者加入、 刪除其中一個、 全部刪除，以及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="20c15-295">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="20c15-296">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="20c15-296">Test app</span></span> | <span data-ttu-id="20c15-297">*tests/RazorPagesProject.Tests*</span><span class="sxs-lookup"><span data-stu-id="20c15-297">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="20c15-298">用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="20c15-298">Used to integration test the SUT.</span></span> |

<span data-ttu-id="20c15-299">可以使用內建的測試功能的 IDE，例如執行測試[Visual Studio](https://www.visualstudio.com/vs/)。</span><span class="sxs-lookup"><span data-stu-id="20c15-299">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="20c15-300">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列中，執行下列命令在命令提示字元中*tests/RazorPagesProject.Tests*資料夾：</span><span class="sxs-lookup"><span data-stu-id="20c15-300">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* folder:</span></span>

```console
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="20c15-301">訊息應用程式 (SUT) 組織</span><span class="sxs-lookup"><span data-stu-id="20c15-301">Message app (SUT) organization</span></span>

<span data-ttu-id="20c15-302">SUT 是 Razor Pages 訊息系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="20c15-302">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="20c15-303">應用程式的 [索引] 頁面 (*pages/Index.cshtml*並*Pages/Index.cshtml.cs*) 提供 UI 和頁面模型的方法，來控制新增、 刪除和分析的訊息 （每個訊息的平均字）.</span><span class="sxs-lookup"><span data-stu-id="20c15-303">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="20c15-304">所描述的訊息`Message`類別 (*Data/Message.cs*) 使用兩個屬性： `Id` （索引鍵） 和`Text`（訊息）。</span><span class="sxs-lookup"><span data-stu-id="20c15-304">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="20c15-305">`Text`屬性是必要的且限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="20c15-305">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="20c15-306">使用儲存的訊息[Entity Framework 的記憶體中的資料庫](/ef/core/providers/in-memory/)&#8224;。</span><span class="sxs-lookup"><span data-stu-id="20c15-306">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="20c15-307">應用程式在其資料庫內容類別中，包含資料存取層 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="20c15-307">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="20c15-308">如果資料庫是空的應用程式啟動時，就會使用三個訊息初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="20c15-308">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="20c15-309">應用程式包含`/SecurePage`，只能存取已驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="20c15-309">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="20c15-310">&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)，說明如何使用記憶體中資料庫的 mstest 執行測試。</span><span class="sxs-lookup"><span data-stu-id="20c15-310">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="20c15-311">本主題會使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="20c15-311">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="20c15-312">測試概念和跨不同的測試架構的測試實作都類似，但不是完全相同。</span><span class="sxs-lookup"><span data-stu-id="20c15-312">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="20c15-313">雖然應用程式不會使用儲存機制模式，而且不是有效的範例[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 頁面支援的開發這些模式。</span><span class="sxs-lookup"><span data-stu-id="20c15-313">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="20c15-314">如需詳細資訊，請參閱 <<c0> [ 設計基礎結構持續層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)並[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（此範例會實作儲存機制模式）。</span><span class="sxs-lookup"><span data-stu-id="20c15-314">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="20c15-315">測試應用程式的組織</span><span class="sxs-lookup"><span data-stu-id="20c15-315">Test app organization</span></span>

<span data-ttu-id="20c15-316">測試應用程式是主控台應用程式內*tests/RazorPagesProject.Tests*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c15-316">The test app is a console app inside the *tests/RazorPagesProject.Tests* folder.</span></span>

| <span data-ttu-id="20c15-317">測試應用程式資料夾</span><span class="sxs-lookup"><span data-stu-id="20c15-317">Test app folder</span></span> | <span data-ttu-id="20c15-318">描述</span><span class="sxs-lookup"><span data-stu-id="20c15-318">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="20c15-319">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="20c15-319">*BasicTests*</span></span> | <span data-ttu-id="20c15-320">*BasicTests.cs*包含路由、 存取安全的頁面未驗證的使用者，並取得 GitHub 使用者設定檔和檢查設定檔的使用者登入的測試方法。</span><span class="sxs-lookup"><span data-stu-id="20c15-320">*BasicTests.cs* contains test methods for routing, accessing a secure page by an unauthenticated user, and obtaining a GitHub user profile and checking the profile's user login.</span></span> |
| <span data-ttu-id="20c15-321">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="20c15-321">*IntegrationTests*</span></span> | <span data-ttu-id="20c15-322">*IndexPageTests.cs*包含 [索引] 頁面使用自訂的整合測試`WebApplicationFactory`類別。</span><span class="sxs-lookup"><span data-stu-id="20c15-322">*IndexPageTests.cs* contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="20c15-323">*協助程式/公用程式*</span><span class="sxs-lookup"><span data-stu-id="20c15-323">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="20c15-324">*Utilities.cs*包含`InitializeDbForTests`方法用來植入測試資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="20c15-324">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="20c15-325">*HtmlHelpers.cs*提供方法來傳回 AngleSharp`IHtmlDocument`以供測試方法。</span><span class="sxs-lookup"><span data-stu-id="20c15-325">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="20c15-326">*HttpClientExtensions.cs*提供的多載`SendAsync`提交要求給 SUT。</span><span class="sxs-lookup"><span data-stu-id="20c15-326">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="20c15-327">測試架構[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="20c15-327">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="20c15-328">使用執行整合測試[Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)，其中包括[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="20c15-328">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="20c15-329">因為[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)封裝用來設定測試主應用程式和測試伺服器`TestHost`和`TestServer`套件不需要在測試應用程式的專案檔中直接將封裝參考或在測試應用程式的開發人員設定。</span><span class="sxs-lookup"><span data-stu-id="20c15-329">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="20c15-330">**植入資料庫中的測試**</span><span class="sxs-lookup"><span data-stu-id="20c15-330">**Seeding the database for testing**</span></span>

<span data-ttu-id="20c15-331">整合測試通常需要在測試執行前的資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="20c15-331">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="20c15-332">比方說，刪除測試會呼叫資料庫記錄的刪除，所以資料庫必須至少一個記錄才會成功的刪除要求。</span><span class="sxs-lookup"><span data-stu-id="20c15-332">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="20c15-333">範例應用程式會植入資料庫中的三個訊息*Utilities.cs*它們執行時，可以使用測試：</span><span class="sxs-lookup"><span data-stu-id="20c15-333">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a><span data-ttu-id="20c15-334">其他資源</span><span class="sxs-lookup"><span data-stu-id="20c15-334">Additional resources</span></span>

* [<span data-ttu-id="20c15-335">單元測試</span><span class="sxs-lookup"><span data-stu-id="20c15-335">Unit tests</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="20c15-336">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="20c15-336">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
* [<span data-ttu-id="20c15-337">中介軟體</span><span class="sxs-lookup"><span data-stu-id="20c15-337">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="20c15-338">測試控制器</span><span class="sxs-lookup"><span data-stu-id="20c15-338">Test controllers</span></span>](xref:mvc/controllers/testing)
