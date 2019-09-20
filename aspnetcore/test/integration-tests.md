---
title: ASP.NET Core 中的整合測試
author: guardrex
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: test/integration-tests
ms.openlocfilehash: eba54b891c4f2d9876f9db5a3d0394990584c146
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149412"
---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="b3eda-103">ASP.NET Core 中的整合測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-103">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="b3eda-104">作者： [Luke Latham](https://github.com/guardrex)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b3eda-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b3eda-105">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="b3eda-105">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="b3eda-106">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-106">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="b3eda-107">本主題假設您對單元測試知識有基本了解。</span><span class="sxs-lookup"><span data-stu-id="b3eda-107">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="b3eda-108">如果不熟悉測試概念，請參閱 [.NET Core 與 .NET Standard 中的單元測試](/dotnet/core/testing/)主題和其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="b3eda-108">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="b3eda-109">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3eda-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b3eda-110">範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="b3eda-110">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="b3eda-111">如果不熟悉 Razor Pages，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="b3eda-111">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="b3eda-112">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="b3eda-112">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="b3eda-113">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="b3eda-113">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="b3eda-114">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-114">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="b3eda-115">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="b3eda-115">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="b3eda-116">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="b3eda-116">Introduction to integration tests</span></span>

<span data-ttu-id="b3eda-117">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-117">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="b3eda-118">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-118">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="b3eda-119">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-119">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="b3eda-120">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="b3eda-120">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="b3eda-121">資料庫</span><span class="sxs-lookup"><span data-stu-id="b3eda-121">Database</span></span>
* <span data-ttu-id="b3eda-122">檔案系統</span><span class="sxs-lookup"><span data-stu-id="b3eda-122">File system</span></span>
* <span data-ttu-id="b3eda-123">網路設備</span><span class="sxs-lookup"><span data-stu-id="b3eda-123">Network appliances</span></span>
* <span data-ttu-id="b3eda-124">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="b3eda-124">Request-response pipeline</span></span>

<span data-ttu-id="b3eda-125">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-125">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="b3eda-126">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="b3eda-126">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="b3eda-127">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-127">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="b3eda-128">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="b3eda-128">Require more code and data processing.</span></span>
* <span data-ttu-id="b3eda-129">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="b3eda-129">Take longer to run.</span></span>

<span data-ttu-id="b3eda-130">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="b3eda-130">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="b3eda-131">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-131">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="b3eda-132">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-132">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="b3eda-133">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-133">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="b3eda-134">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-134">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="b3eda-135">在單元測試中的基礎結構使用 fakes/模擬 (mock) 更快速的測試執行中的結果。</span><span class="sxs-lookup"><span data-stu-id="b3eda-135">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="b3eda-136">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="b3eda-136">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="b3eda-137">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="b3eda-137">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="b3eda-138">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-138">ASP.NET Core integration tests</span></span>

<span data-ttu-id="b3eda-139">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="b3eda-139">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="b3eda-140">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-140">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="b3eda-141">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="b3eda-141">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="b3eda-142">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="b3eda-142">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="b3eda-143">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="b3eda-143">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="b3eda-144">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="b3eda-144">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="b3eda-145">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="b3eda-145">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="b3eda-146">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-146">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="b3eda-147">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="b3eda-147">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="b3eda-148">執行*Act*測試步驟：用戶端會提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="b3eda-148">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="b3eda-149">會執行*Assert*測試步驟：*實際*的回應會根據*預期*的回應而驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="b3eda-149">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="b3eda-150">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="b3eda-150">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="b3eda-151">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="b3eda-151">The test results are reported.</span></span>

<span data-ttu-id="b3eda-152">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="b3eda-152">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="b3eda-153">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="b3eda-153">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="b3eda-154">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="b3eda-154">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="b3eda-155">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-155">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="b3eda-156">此`Microsoft.AspNetCore.Mvc.Testing`套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="b3eda-156">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="b3eda-157">將相依性檔案（ *. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="b3eda-157">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="b3eda-158">將內容根目錄設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="b3eda-158">Sets the content root to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="b3eda-159">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用`TestServer`來啟動的 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-159">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="b3eda-160">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="b3eda-160">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="b3eda-161">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="b3eda-161">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="b3eda-162">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="b3eda-162">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="b3eda-163">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-163">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="b3eda-164">Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="b3eda-164">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="b3eda-165">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-165">The only difference is in how the tests are named.</span></span> <span data-ttu-id="b3eda-166">在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如， `IndexPageTests`用來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-166">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="b3eda-167">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名`HomeControllerTests` （例如，測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-167">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="b3eda-168">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="b3eda-168">Test app prerequisites</span></span>

<span data-ttu-id="b3eda-169">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="b3eda-169">The test project must:</span></span>

* <span data-ttu-id="b3eda-170">參考[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)的封裝。</span><span class="sxs-lookup"><span data-stu-id="b3eda-170">Reference the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span>
* <span data-ttu-id="b3eda-171">在專案檔中指定 Web SDK （`<Project Sdk="Microsoft.NET.Sdk.Web">`）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-171">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span>

<span data-ttu-id="b3eda-172">這些必要條件可在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="b3eda-172">These prerequisites can be seen in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="b3eda-173">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="b3eda-173">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="b3eda-174">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="b3eda-174">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="b3eda-175">xunit</span><span class="sxs-lookup"><span data-stu-id="b3eda-175">xunit</span></span>](https://www.nuget.org/packages/xunit)
* [<span data-ttu-id="b3eda-176">xunit.runner.visualstudio</span><span class="sxs-lookup"><span data-stu-id="b3eda-176">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [<span data-ttu-id="b3eda-177">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="b3eda-177">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp)

<span data-ttu-id="b3eda-178">Entity Framework Core 也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="b3eda-178">Entity Framework Core is also used in the tests.</span></span> <span data-ttu-id="b3eda-179">應用程式參考：</span><span class="sxs-lookup"><span data-stu-id="b3eda-179">The app references:</span></span>

* [<span data-ttu-id="b3eda-180">AspNetCore. Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="b3eda-180">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [<span data-ttu-id="b3eda-181">AspNetCore. Identity. Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="b3eda-181">Microsoft.AspNetCore.Identity.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [<span data-ttu-id="b3eda-182">Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="b3eda-182">Microsoft.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [<span data-ttu-id="b3eda-183">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="b3eda-183">Microsoft.EntityFrameworkCore.InMemory</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [<span data-ttu-id="b3eda-184">Microsoft.entityframeworkcore 工具</span><span class="sxs-lookup"><span data-stu-id="b3eda-184">Microsoft.EntityFrameworkCore.Tools</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a><span data-ttu-id="b3eda-185">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="b3eda-185">SUT environment</span></span>

<span data-ttu-id="b3eda-186">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="b3eda-186">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="b3eda-187">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-187">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="b3eda-188">[WebApplicationFactory\<TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="b3eda-188">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="b3eda-189">`TEntryPoint`是 SUT 的進入點類別，通常`Startup`是類別。</span><span class="sxs-lookup"><span data-stu-id="b3eda-189">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="b3eda-190">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="b3eda-190">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

### <a name="basic-test-of-app-endpoints"></a><span data-ttu-id="b3eda-191">應用程式端點的基本測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-191">Basic test of app endpoints</span></span>

<span data-ttu-id="b3eda-192">下列測試類別`BasicTests`會`WebApplicationFactory`使用來啟動 SUT 並提供`Get_EndpointsReturnSuccessAndCorrectContentType` [HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-192">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="b3eda-193">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而`Content-Type`標頭則`text/html; charset=utf-8`適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="b3eda-193">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="b3eda-194">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)會建立自動遵循`HttpClient`重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="b3eda-194">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="b3eda-195">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="b3eda-195">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="b3eda-196">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="b3eda-196">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="b3eda-197">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-197">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

### <a name="test-a-secure-endpoint"></a><span data-ttu-id="b3eda-198">測試安全端點</span><span class="sxs-lookup"><span data-stu-id="b3eda-198">Test a secure endpoint</span></span>

<span data-ttu-id="b3eda-199">類別中的`BasicTests`另一項測試會檢查安全端點是否將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="b3eda-199">Another test in the `BasicTests` class checks that a secure endpoint redirects an unauthenticated user to the app's Login page.</span></span>

<span data-ttu-id="b3eda-200">在 SUT 中， `/SecurePage`頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="b3eda-200">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="b3eda-201">如需詳細資訊，請參閱  [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-201">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="b3eda-202">在測試中, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向, 方法是`false`將 [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) 設為:  `Get_SecurePageRequiresAnAuthenticatedUser`</span><span class="sxs-lookup"><span data-stu-id="b3eda-202">In the `Get_SecurePageRequiresAnAuthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

<span data-ttu-id="b3eda-203">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="b3eda-203">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="b3eda-204">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="b3eda-204">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="b3eda-205">系統`Location`會檢查回應標頭中的標頭值`http://localhost/Identity/Account/Login`，以確認其開頭為，而不是最終的登入`Location`頁面回應，其中標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="b3eda-205">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="b3eda-206">如需詳細資訊`WebApplicationFactoryClientOptions`，請參閱 <c2> [ 用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="b3eda-206">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="b3eda-207">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="b3eda-207">Customize WebApplicationFactory</span></span>

<span data-ttu-id="b3eda-208">藉由繼承`WebApplicationFactory`自來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="b3eda-208">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="b3eda-209">繼承自`WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-209">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="b3eda-210">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：</span><span class="sxs-lookup"><span data-stu-id="b3eda-210">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="b3eda-211">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的`InitializeDbForTests`資料庫植入是由方法執行。</span><span class="sxs-lookup"><span data-stu-id="b3eda-211">Database seeding in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="b3eda-212">[整合測試範例中會說明方法：測試應用程式](#test-app-organization)組織區段。</span><span class="sxs-lookup"><span data-stu-id="b3eda-212">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="b3eda-213">使用測試類別`CustomWebApplicationFactory`中的自訂。</span><span class="sxs-lookup"><span data-stu-id="b3eda-213">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="b3eda-214">下列範例會在`IndexPageTests`類別中使用 factory：</span><span class="sxs-lookup"><span data-stu-id="b3eda-214">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="b3eda-215">範例應用程式的用戶端設定為防止`HttpClient`下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="b3eda-215">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="b3eda-216">如[測試安全端點](#test-a-secure-endpoint)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="b3eda-216">As explained in the [Test a secure endpoint](#test-a-secure-endpoint) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="b3eda-217">第一個回應是其中許多測試中具有`Location`標頭的重新導向。</span><span class="sxs-lookup"><span data-stu-id="b3eda-217">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="b3eda-218">一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="b3eda-218">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="b3eda-219">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="b3eda-219">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="b3eda-220">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="b3eda-220">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="b3eda-221">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="b3eda-221">Make a request for the page.</span></span>
1. <span data-ttu-id="b3eda-222">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="b3eda-222">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="b3eda-223">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="b3eda-223">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="b3eda-224">`GetDocumentAsync`  在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中, helper擴充方法(helper/HttpClientExtensions)和helper方法(helper/HtmlHelpers)會使用[AngleSharp](https://anglesharp.github.io/)剖析器來`SendAsync`處理 antiforgery請使用下列方法進行檢查:</span><span class="sxs-lookup"><span data-stu-id="b3eda-224">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="b3eda-225">`GetDocumentAsync`接收 [HttpResponseMessage ](/dotnet/api/system.net.http.httpresponsemessage) , 並`IHtmlDocument`傳回&ndash;。</span><span class="sxs-lookup"><span data-stu-id="b3eda-225">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="b3eda-226">`GetDocumentAsync`使用可根據原始`HttpResponseMessage`的來準備*虛擬回應*的 factory。</span><span class="sxs-lookup"><span data-stu-id="b3eda-226">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="b3eda-227">如需詳細資訊，請參閱 [AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-227">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="b3eda-228">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-228">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="b3eda-229">的多載會`IHtmlFormElement`接受HTML表單（）和下列內容：`SendAsync`</span><span class="sxs-lookup"><span data-stu-id="b3eda-229">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="b3eda-230">表單的 [提交]`IHtmlElement`按鈕（）</span><span class="sxs-lookup"><span data-stu-id="b3eda-230">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="b3eda-231">表單值集合`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="b3eda-231">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="b3eda-232">提交按鈕（`IHtmlElement`）和表單值`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="b3eda-232">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="b3eda-233">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="b3eda-233">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="b3eda-234">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="b3eda-234">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="b3eda-235">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-235">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="b3eda-236">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="b3eda-236">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="b3eda-237">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="b3eda-237">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="b3eda-238">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)會使用設定`WebApplicationFactory`進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="b3eda-238">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="b3eda-239">`Post_DeleteMessageHandler_ReturnsRedirectToRoot` [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會`WithWebHostBuilder`示範的用法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-239">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="b3eda-240">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="b3eda-240">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="b3eda-241">因為`IndexPageTests`類別中的另一項測試會執行刪除資料庫中所有記錄的作業，而且可能會在此`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法之前執行，所以會在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="b3eda-241">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="b3eda-242">在 sut 的要求中，選取`messages`該表單的第一個 [刪除] 按鈕會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="b3eda-242">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="b3eda-243">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="b3eda-243">Client options</span></span>

<span data-ttu-id="b3eda-244">下表顯示建立`HttpClient`實例時可用的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) 。</span><span class="sxs-lookup"><span data-stu-id="b3eda-244">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="b3eda-245">選項</span><span class="sxs-lookup"><span data-stu-id="b3eda-245">Option</span></span> | <span data-ttu-id="b3eda-246">描述</span><span class="sxs-lookup"><span data-stu-id="b3eda-246">Description</span></span> | <span data-ttu-id="b3eda-247">預設</span><span class="sxs-lookup"><span data-stu-id="b3eda-247">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="b3eda-248">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="b3eda-248">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="b3eda-249">取得或設定`HttpClient`實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="b3eda-249">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="b3eda-250">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="b3eda-250">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="b3eda-251">取得或設定`HttpClient`實例的基底位址。</span><span class="sxs-lookup"><span data-stu-id="b3eda-251">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="b3eda-252">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="b3eda-252">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="b3eda-253">取得或設定實例`HttpClient`是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="b3eda-253">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="b3eda-254">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="b3eda-254">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="b3eda-255">取得或設定`HttpClient`實例應遵循的重新導向回應數目上限。</span><span class="sxs-lookup"><span data-stu-id="b3eda-255">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="b3eda-256">7</span><span class="sxs-lookup"><span data-stu-id="b3eda-256">7</span></span> |

<span data-ttu-id="b3eda-257">建立類別, 並將它傳遞給 [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) 方法 (程式碼範例中會顯示預設值):  `WebApplicationFactoryClientOptions`</span><span class="sxs-lookup"><span data-stu-id="b3eda-257">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="b3eda-258">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="b3eda-258">Inject mock services</span></span>

<span data-ttu-id="b3eda-259">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="b3eda-259">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="b3eda-260">**若要插入模擬服務，SUT 必須具有`Startup` `Startup.ConfigureServices`具有方法的類別。**</span><span class="sxs-lookup"><span data-stu-id="b3eda-260">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="b3eda-261">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="b3eda-261">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="b3eda-262">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="b3eda-262">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="b3eda-263">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-263">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="b3eda-264">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-264">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="b3eda-265">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-265">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="b3eda-266">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-266">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="b3eda-267">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-267">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="b3eda-268">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="b3eda-268">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="b3eda-269">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-269">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="b3eda-270">Mock 服務會將應用程式的`QuoteService`取代為測試應用程式所提供的服務， `TestQuoteService`稱為：</span><span class="sxs-lookup"><span data-stu-id="b3eda-270">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="b3eda-271">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-271">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="b3eda-272">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="b3eda-272">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="b3eda-273">在測試執行期間產生的標記會反映所提供`TestQuoteService`的引號文字，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="b3eda-273">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="b3eda-274">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="b3eda-274">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="b3eda-275">此`WebApplicationFactory`函式會藉由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式內容根路徑，其中包含的索引`TEntryPoint`鍵等於元件`System.Reflection.Assembly.FullName`的整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-275">The `WebApplicationFactory` constructor infers the app content root path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="b3eda-276">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory`就會回到搜尋方案檔（ *.sln* `TEntryPoint` ），並將元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="b3eda-276">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="b3eda-277">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="b3eda-277">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="b3eda-278">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="b3eda-278">Disable shadow copying</span></span>

<span data-ttu-id="b3eda-279">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="b3eda-279">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="b3eda-280">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="b3eda-280">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="b3eda-281">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="b3eda-281">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="b3eda-282">如需詳細資訊，請參閱 <c0> [ 設定使用 JSON 的 xUnit.net](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-282">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="b3eda-283">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="b3eda-283">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a><span data-ttu-id="b3eda-284">物件的處置</span><span class="sxs-lookup"><span data-stu-id="b3eda-284">Disposal of objects</span></span>

<span data-ttu-id="b3eda-285">執行`IClassFixture`此實作為測試之後，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="b3eda-285">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="b3eda-286">如果開發人員所具現化的物件需要處置，請在`IClassFixture`執行時處置它們。</span><span class="sxs-lookup"><span data-stu-id="b3eda-286">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="b3eda-287">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-287">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="b3eda-288">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="b3eda-288">Integration tests sample</span></span>

<span data-ttu-id="b3eda-289">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="b3eda-289">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="b3eda-290">App</span><span class="sxs-lookup"><span data-stu-id="b3eda-290">App</span></span> | <span data-ttu-id="b3eda-291">專案目錄</span><span class="sxs-lookup"><span data-stu-id="b3eda-291">Project directory</span></span> | <span data-ttu-id="b3eda-292">描述</span><span class="sxs-lookup"><span data-stu-id="b3eda-292">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="b3eda-293">訊息應用程式（SUT）</span><span class="sxs-lookup"><span data-stu-id="b3eda-293">Message app (the SUT)</span></span> | <span data-ttu-id="b3eda-294">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="b3eda-294">*src/RazorPagesProject*</span></span> | <span data-ttu-id="b3eda-295">可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="b3eda-295">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="b3eda-296">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="b3eda-296">Test app</span></span> | <span data-ttu-id="b3eda-297">*tests/RazorPagesProject.Tests*</span><span class="sxs-lookup"><span data-stu-id="b3eda-297">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="b3eda-298">用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-298">Used to integration test the SUT.</span></span> |

<span data-ttu-id="b3eda-299">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-299">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="b3eda-300">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="b3eda-300">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```console
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="b3eda-301">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="b3eda-301">Message app (SUT) organization</span></span>

<span data-ttu-id="b3eda-302">SUT 是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="b3eda-302">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="b3eda-303">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-303">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="b3eda-304">訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-304">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="b3eda-305">屬性`Text`是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="b3eda-305">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="b3eda-306">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。</span><span class="sxs-lookup"><span data-stu-id="b3eda-306">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="b3eda-307">應用程式在其資料庫內容類別`AppDbContext` （*data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-307">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="b3eda-308">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="b3eda-308">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="b3eda-309">應用程式包含`/SecurePage`只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="b3eda-309">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="b3eda-310">&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-310">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="b3eda-311">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="b3eda-311">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="b3eda-312">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="b3eda-312">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="b3eda-313">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-313">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="b3eda-314">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-314">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="b3eda-315">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="b3eda-315">Test app organization</span></span>

<span data-ttu-id="b3eda-316">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-316">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="b3eda-317">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="b3eda-317">Test app directory</span></span> | <span data-ttu-id="b3eda-318">說明</span><span class="sxs-lookup"><span data-stu-id="b3eda-318">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="b3eda-319">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="b3eda-319">*BasicTests*</span></span> | <span data-ttu-id="b3eda-320">*BasicTests.cs*包含用於路由的測試方法、透過未經驗證的使用者存取安全頁面，以及取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="b3eda-320">*BasicTests.cs* contains test methods for routing, accessing a secure page by an unauthenticated user, and obtaining a GitHub user profile and checking the profile's user login.</span></span> |
| <span data-ttu-id="b3eda-321">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="b3eda-321">*IntegrationTests*</span></span> | <span data-ttu-id="b3eda-322">*IndexPageTests.cs*包含使用自訂`WebApplicationFactory`類別之索引頁面的整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-322">*IndexPageTests.cs* contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="b3eda-323">*Helper/公用程式*</span><span class="sxs-lookup"><span data-stu-id="b3eda-323">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="b3eda-324">*Utilities.cs*包含`InitializeDbForTests`用來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-324">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="b3eda-325">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument`供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="b3eda-325">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="b3eda-326">*HttpClientExtensions.cs*提供的`SendAsync`多載，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-326">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="b3eda-327">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-327">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="b3eda-328">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-328">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="b3eda-329">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器， `TestHost`因此和`TestServer`封裝不需要在測試應用程式的專案檔或開發人員中直接封裝參考測試應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="b3eda-329">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="b3eda-330">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="b3eda-330">**Seeding the database for testing**</span></span>

<span data-ttu-id="b3eda-331">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="b3eda-331">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="b3eda-332">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="b3eda-332">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="b3eda-333">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="b3eda-333">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b3eda-334">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="b3eda-334">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="b3eda-335">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-335">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="b3eda-336">本主題假設您對單元測試知識有基本了解。</span><span class="sxs-lookup"><span data-stu-id="b3eda-336">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="b3eda-337">如果不熟悉測試概念，請參閱 [.NET Core 與 .NET Standard 中的單元測試](/dotnet/core/testing/)主題和其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="b3eda-337">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="b3eda-338">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3eda-338">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b3eda-339">範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="b3eda-339">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="b3eda-340">如果不熟悉 Razor Pages，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="b3eda-340">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="b3eda-341">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="b3eda-341">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="b3eda-342">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="b3eda-342">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="b3eda-343">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-343">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="b3eda-344">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="b3eda-344">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="b3eda-345">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="b3eda-345">Introduction to integration tests</span></span>

<span data-ttu-id="b3eda-346">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-346">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="b3eda-347">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-347">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="b3eda-348">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-348">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="b3eda-349">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="b3eda-349">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="b3eda-350">資料庫</span><span class="sxs-lookup"><span data-stu-id="b3eda-350">Database</span></span>
* <span data-ttu-id="b3eda-351">檔案系統</span><span class="sxs-lookup"><span data-stu-id="b3eda-351">File system</span></span>
* <span data-ttu-id="b3eda-352">網路設備</span><span class="sxs-lookup"><span data-stu-id="b3eda-352">Network appliances</span></span>
* <span data-ttu-id="b3eda-353">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="b3eda-353">Request-response pipeline</span></span>

<span data-ttu-id="b3eda-354">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-354">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="b3eda-355">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="b3eda-355">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="b3eda-356">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-356">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="b3eda-357">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="b3eda-357">Require more code and data processing.</span></span>
* <span data-ttu-id="b3eda-358">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="b3eda-358">Take longer to run.</span></span>

<span data-ttu-id="b3eda-359">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="b3eda-359">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="b3eda-360">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-360">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="b3eda-361">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-361">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="b3eda-362">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="b3eda-362">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="b3eda-363">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-363">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="b3eda-364">在單元測試中的基礎結構使用 fakes/模擬 (mock) 更快速的測試執行中的結果。</span><span class="sxs-lookup"><span data-stu-id="b3eda-364">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="b3eda-365">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="b3eda-365">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="b3eda-366">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="b3eda-366">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="b3eda-367">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-367">ASP.NET Core integration tests</span></span>

<span data-ttu-id="b3eda-368">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="b3eda-368">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="b3eda-369">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-369">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="b3eda-370">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="b3eda-370">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="b3eda-371">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="b3eda-371">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="b3eda-372">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="b3eda-372">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="b3eda-373">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="b3eda-373">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="b3eda-374">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="b3eda-374">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="b3eda-375">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-375">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="b3eda-376">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="b3eda-376">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="b3eda-377">執行*Act*測試步驟：用戶端會提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="b3eda-377">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="b3eda-378">會執行*Assert*測試步驟：*實際*的回應會根據*預期*的回應而驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="b3eda-378">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="b3eda-379">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="b3eda-379">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="b3eda-380">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="b3eda-380">The test results are reported.</span></span>

<span data-ttu-id="b3eda-381">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="b3eda-381">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="b3eda-382">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="b3eda-382">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="b3eda-383">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="b3eda-383">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="b3eda-384">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-384">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="b3eda-385">此`Microsoft.AspNetCore.Mvc.Testing`套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="b3eda-385">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="b3eda-386">將相依性檔案（ *. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="b3eda-386">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="b3eda-387">將內容根目錄設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="b3eda-387">Sets the content root to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="b3eda-388">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用`TestServer`來啟動的 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-388">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="b3eda-389">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="b3eda-389">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="b3eda-390">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="b3eda-390">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="b3eda-391">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="b3eda-391">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="b3eda-392">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-392">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="b3eda-393">Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="b3eda-393">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="b3eda-394">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-394">The only difference is in how the tests are named.</span></span> <span data-ttu-id="b3eda-395">在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如， `IndexPageTests`用來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-395">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="b3eda-396">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名`HomeControllerTests` （例如，測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-396">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="b3eda-397">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="b3eda-397">Test app prerequisites</span></span>

<span data-ttu-id="b3eda-398">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="b3eda-398">The test project must:</span></span>

* <span data-ttu-id="b3eda-399">參考下列套件：</span><span class="sxs-lookup"><span data-stu-id="b3eda-399">Reference the following packages:</span></span>
  * [<span data-ttu-id="b3eda-400">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="b3eda-400">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [<span data-ttu-id="b3eda-401">Microsoft.AspNetCore.Mvc.Testing</span><span class="sxs-lookup"><span data-stu-id="b3eda-401">Microsoft.AspNetCore.Mvc.Testing</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* <span data-ttu-id="b3eda-402">在專案檔中指定 Web SDK （`<Project Sdk="Microsoft.NET.Sdk.Web">`）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-402">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span> <span data-ttu-id="b3eda-403">參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)時，需要 Web SDK。</span><span class="sxs-lookup"><span data-stu-id="b3eda-403">The Web SDK is required when referencing the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b3eda-404">這些必要條件可在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="b3eda-404">These prerequisites can be seen in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="b3eda-405">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="b3eda-405">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="b3eda-406">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="b3eda-406">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="b3eda-407">xunit</span><span class="sxs-lookup"><span data-stu-id="b3eda-407">xunit</span></span>](https://www.nuget.org/packages/xunit/)
* [<span data-ttu-id="b3eda-408">xunit.runner.visualstudio</span><span class="sxs-lookup"><span data-stu-id="b3eda-408">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [<span data-ttu-id="b3eda-409">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="b3eda-409">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a><span data-ttu-id="b3eda-410">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="b3eda-410">SUT environment</span></span>

<span data-ttu-id="b3eda-411">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="b3eda-411">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="b3eda-412">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-412">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="b3eda-413">[WebApplicationFactory\<TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="b3eda-413">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="b3eda-414">`TEntryPoint`是 SUT 的進入點類別，通常`Startup`是類別。</span><span class="sxs-lookup"><span data-stu-id="b3eda-414">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="b3eda-415">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="b3eda-415">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

### <a name="basic-test-of-app-endpoints"></a><span data-ttu-id="b3eda-416">應用程式端點的基本測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-416">Basic test of app endpoints</span></span>

<span data-ttu-id="b3eda-417">下列測試類別`BasicTests`會`WebApplicationFactory`使用來啟動 SUT 並提供`Get_EndpointsReturnSuccessAndCorrectContentType` [HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-417">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="b3eda-418">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而`Content-Type`標頭則`text/html; charset=utf-8`適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="b3eda-418">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="b3eda-419">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)會建立自動遵循`HttpClient`重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="b3eda-419">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="b3eda-420">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="b3eda-420">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="b3eda-421">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="b3eda-421">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="b3eda-422">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-422">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

### <a name="test-a-secure-endpoint"></a><span data-ttu-id="b3eda-423">測試安全端點</span><span class="sxs-lookup"><span data-stu-id="b3eda-423">Test a secure endpoint</span></span>

<span data-ttu-id="b3eda-424">類別中的`BasicTests`另一項測試會檢查安全端點是否將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="b3eda-424">Another test in the `BasicTests` class checks that a secure endpoint redirects an unauthenticated user to the app's Login page.</span></span>

<span data-ttu-id="b3eda-425">在 SUT 中， `/SecurePage`頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="b3eda-425">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="b3eda-426">如需詳細資訊，請參閱  [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-426">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="b3eda-427">在測試中, [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向, 方法是`false`將 [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) 設為:  `Get_SecurePageRequiresAnAuthenticatedUser`</span><span class="sxs-lookup"><span data-stu-id="b3eda-427">In the `Get_SecurePageRequiresAnAuthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

<span data-ttu-id="b3eda-428">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="b3eda-428">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="b3eda-429">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="b3eda-429">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="b3eda-430">系統`Location`會檢查回應標頭中的標頭值`http://localhost/Identity/Account/Login`，以確認其開頭為，而不是最終的登入`Location`頁面回應，其中標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="b3eda-430">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="b3eda-431">如需詳細資訊`WebApplicationFactoryClientOptions`，請參閱 <c2> [ 用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="b3eda-431">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="b3eda-432">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="b3eda-432">Customize WebApplicationFactory</span></span>

<span data-ttu-id="b3eda-433">藉由繼承`WebApplicationFactory`自來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="b3eda-433">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="b3eda-434">繼承自`WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-434">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="b3eda-435">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：</span><span class="sxs-lookup"><span data-stu-id="b3eda-435">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="b3eda-436">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的`InitializeDbForTests`資料庫植入是由方法執行。</span><span class="sxs-lookup"><span data-stu-id="b3eda-436">Database seeding in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="b3eda-437">[整合測試範例中會說明方法：測試應用程式](#test-app-organization)組織區段。</span><span class="sxs-lookup"><span data-stu-id="b3eda-437">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="b3eda-438">使用測試類別`CustomWebApplicationFactory`中的自訂。</span><span class="sxs-lookup"><span data-stu-id="b3eda-438">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="b3eda-439">下列範例會在`IndexPageTests`類別中使用 factory：</span><span class="sxs-lookup"><span data-stu-id="b3eda-439">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="b3eda-440">範例應用程式的用戶端設定為防止`HttpClient`下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="b3eda-440">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="b3eda-441">如[測試安全端點](#test-a-secure-endpoint)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="b3eda-441">As explained in the [Test a secure endpoint](#test-a-secure-endpoint) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="b3eda-442">第一個回應是其中許多測試中具有`Location`標頭的重新導向。</span><span class="sxs-lookup"><span data-stu-id="b3eda-442">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="b3eda-443">一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="b3eda-443">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="b3eda-444">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="b3eda-444">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="b3eda-445">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="b3eda-445">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="b3eda-446">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="b3eda-446">Make a request for the page.</span></span>
1. <span data-ttu-id="b3eda-447">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="b3eda-447">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="b3eda-448">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="b3eda-448">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="b3eda-449">`GetDocumentAsync`  在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中, helper擴充方法(helper/HttpClientExtensions)和helper方法(helper/HtmlHelpers)會使用[AngleSharp](https://anglesharp.github.io/)剖析器來`SendAsync`處理 antiforgery請使用下列方法進行檢查:</span><span class="sxs-lookup"><span data-stu-id="b3eda-449">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="b3eda-450">`GetDocumentAsync`接收 [HttpResponseMessage ](/dotnet/api/system.net.http.httpresponsemessage) , 並`IHtmlDocument`傳回&ndash;。</span><span class="sxs-lookup"><span data-stu-id="b3eda-450">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="b3eda-451">`GetDocumentAsync`使用可根據原始`HttpResponseMessage`的來準備*虛擬回應*的 factory。</span><span class="sxs-lookup"><span data-stu-id="b3eda-451">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="b3eda-452">如需詳細資訊，請參閱 [AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-452">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="b3eda-453">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-453">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="b3eda-454">的多載會`IHtmlFormElement`接受HTML表單（）和下列內容：`SendAsync`</span><span class="sxs-lookup"><span data-stu-id="b3eda-454">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="b3eda-455">表單的 [提交]`IHtmlElement`按鈕（）</span><span class="sxs-lookup"><span data-stu-id="b3eda-455">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="b3eda-456">表單值集合`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="b3eda-456">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="b3eda-457">提交按鈕（`IHtmlElement`）和表單值`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="b3eda-457">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="b3eda-458">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="b3eda-458">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="b3eda-459">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="b3eda-459">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="b3eda-460">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-460">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="b3eda-461">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="b3eda-461">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="b3eda-462">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="b3eda-462">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="b3eda-463">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)會使用設定`WebApplicationFactory`進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="b3eda-463">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="b3eda-464">`Post_DeleteMessageHandler_ReturnsRedirectToRoot` [範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會`WithWebHostBuilder`示範的用法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-464">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="b3eda-465">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="b3eda-465">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="b3eda-466">因為`IndexPageTests`類別中的另一項測試會執行刪除資料庫中所有記錄的作業，而且可能會在此`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法之前執行，所以會在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="b3eda-466">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="b3eda-467">在 sut 的要求中，選取`messages`該表單的第一個 [刪除] 按鈕會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="b3eda-467">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="b3eda-468">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="b3eda-468">Client options</span></span>

<span data-ttu-id="b3eda-469">下表顯示建立`HttpClient`實例時可用的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) 。</span><span class="sxs-lookup"><span data-stu-id="b3eda-469">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="b3eda-470">選項</span><span class="sxs-lookup"><span data-stu-id="b3eda-470">Option</span></span> | <span data-ttu-id="b3eda-471">描述</span><span class="sxs-lookup"><span data-stu-id="b3eda-471">Description</span></span> | <span data-ttu-id="b3eda-472">預設</span><span class="sxs-lookup"><span data-stu-id="b3eda-472">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="b3eda-473">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="b3eda-473">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="b3eda-474">取得或設定`HttpClient`實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="b3eda-474">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="b3eda-475">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="b3eda-475">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="b3eda-476">取得或設定`HttpClient`實例的基底位址。</span><span class="sxs-lookup"><span data-stu-id="b3eda-476">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="b3eda-477">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="b3eda-477">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="b3eda-478">取得或設定實例`HttpClient`是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="b3eda-478">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="b3eda-479">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="b3eda-479">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="b3eda-480">取得或設定`HttpClient`實例應遵循的重新導向回應數目上限。</span><span class="sxs-lookup"><span data-stu-id="b3eda-480">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="b3eda-481">7</span><span class="sxs-lookup"><span data-stu-id="b3eda-481">7</span></span> |

<span data-ttu-id="b3eda-482">建立類別, 並將它傳遞給 [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) 方法 (程式碼範例中會顯示預設值):  `WebApplicationFactoryClientOptions`</span><span class="sxs-lookup"><span data-stu-id="b3eda-482">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="b3eda-483">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="b3eda-483">Inject mock services</span></span>

<span data-ttu-id="b3eda-484">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="b3eda-484">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="b3eda-485">**若要插入模擬服務，SUT 必須具有`Startup` `Startup.ConfigureServices`具有方法的類別。**</span><span class="sxs-lookup"><span data-stu-id="b3eda-485">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="b3eda-486">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="b3eda-486">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="b3eda-487">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="b3eda-487">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="b3eda-488">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-488">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="b3eda-489">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-489">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="b3eda-490">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-490">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="b3eda-491">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-491">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="b3eda-492">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-492">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="b3eda-493">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="b3eda-493">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="b3eda-494">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-494">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="b3eda-495">Mock 服務會將應用程式的`QuoteService`取代為測試應用程式所提供的服務， `TestQuoteService`稱為：</span><span class="sxs-lookup"><span data-stu-id="b3eda-495">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="b3eda-496">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="b3eda-496">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="b3eda-497">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="b3eda-497">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="b3eda-498">在測試執行期間產生的標記會反映所提供`TestQuoteService`的引號文字，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="b3eda-498">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="b3eda-499">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="b3eda-499">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="b3eda-500">此`WebApplicationFactory`函式會藉由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式內容根路徑，其中包含的索引`TEntryPoint`鍵等於元件`System.Reflection.Assembly.FullName`的整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-500">The `WebApplicationFactory` constructor infers the app content root path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="b3eda-501">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory`就會回到搜尋方案檔（ *.sln* `TEntryPoint` ），並將元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="b3eda-501">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="b3eda-502">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="b3eda-502">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="b3eda-503">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="b3eda-503">Disable shadow copying</span></span>

<span data-ttu-id="b3eda-504">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="b3eda-504">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="b3eda-505">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="b3eda-505">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="b3eda-506">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="b3eda-506">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="b3eda-507">如需詳細資訊，請參閱 <c0> [ 設定使用 JSON 的 xUnit.net](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-507">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="b3eda-508">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="b3eda-508">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

<span data-ttu-id="b3eda-509">如果使用 Visual Studio，請將檔案的 [**複製到輸出目錄**] 屬性設定為 [**永遠複製**]。</span><span class="sxs-lookup"><span data-stu-id="b3eda-509">If using Visual Studio, set the file's **Copy to Output Directory** property to **Copy always**.</span></span> <span data-ttu-id="b3eda-510">如果未使用 Visual Studio，請將`Content`目標新增至測試應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="b3eda-510">If not using Visual Studio, add a `Content` target to the test app's project file:</span></span>

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a><span data-ttu-id="b3eda-511">物件的處置</span><span class="sxs-lookup"><span data-stu-id="b3eda-511">Disposal of objects</span></span>

<span data-ttu-id="b3eda-512">執行`IClassFixture`此實作為測試之後，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="b3eda-512">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="b3eda-513">如果開發人員所具現化的物件需要處置，請在`IClassFixture`執行時處置它們。</span><span class="sxs-lookup"><span data-stu-id="b3eda-513">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="b3eda-514">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-514">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="b3eda-515">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="b3eda-515">Integration tests sample</span></span>

<span data-ttu-id="b3eda-516">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="b3eda-516">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="b3eda-517">App</span><span class="sxs-lookup"><span data-stu-id="b3eda-517">App</span></span> | <span data-ttu-id="b3eda-518">專案目錄</span><span class="sxs-lookup"><span data-stu-id="b3eda-518">Project directory</span></span> | <span data-ttu-id="b3eda-519">描述</span><span class="sxs-lookup"><span data-stu-id="b3eda-519">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="b3eda-520">訊息應用程式（SUT）</span><span class="sxs-lookup"><span data-stu-id="b3eda-520">Message app (the SUT)</span></span> | <span data-ttu-id="b3eda-521">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="b3eda-521">*src/RazorPagesProject*</span></span> | <span data-ttu-id="b3eda-522">可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="b3eda-522">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="b3eda-523">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="b3eda-523">Test app</span></span> | <span data-ttu-id="b3eda-524">*tests/RazorPagesProject.Tests*</span><span class="sxs-lookup"><span data-stu-id="b3eda-524">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="b3eda-525">用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-525">Used to integration test the SUT.</span></span> |

<span data-ttu-id="b3eda-526">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-526">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="b3eda-527">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="b3eda-527">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="b3eda-528">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="b3eda-528">Message app (SUT) organization</span></span>

<span data-ttu-id="b3eda-529">SUT 是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="b3eda-529">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="b3eda-530">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-530">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="b3eda-531">訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-531">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="b3eda-532">屬性`Text`是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="b3eda-532">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="b3eda-533">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。</span><span class="sxs-lookup"><span data-stu-id="b3eda-533">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="b3eda-534">應用程式在其資料庫內容類別`AppDbContext` （*data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-534">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="b3eda-535">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="b3eda-535">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="b3eda-536">應用程式包含`/SecurePage`只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="b3eda-536">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="b3eda-537">&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-537">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="b3eda-538">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="b3eda-538">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="b3eda-539">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="b3eda-539">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="b3eda-540">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-540">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="b3eda-541">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="b3eda-541">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="b3eda-542">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="b3eda-542">Test app organization</span></span>

<span data-ttu-id="b3eda-543">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3eda-543">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="b3eda-544">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="b3eda-544">Test app directory</span></span> | <span data-ttu-id="b3eda-545">說明</span><span class="sxs-lookup"><span data-stu-id="b3eda-545">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="b3eda-546">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="b3eda-546">*BasicTests*</span></span> | <span data-ttu-id="b3eda-547">*BasicTests.cs*包含用於路由的測試方法、透過未經驗證的使用者存取安全頁面，以及取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="b3eda-547">*BasicTests.cs* contains test methods for routing, accessing a secure page by an unauthenticated user, and obtaining a GitHub user profile and checking the profile's user login.</span></span> |
| <span data-ttu-id="b3eda-548">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="b3eda-548">*IntegrationTests*</span></span> | <span data-ttu-id="b3eda-549">*IndexPageTests.cs*包含使用自訂`WebApplicationFactory`類別之索引頁面的整合測試。</span><span class="sxs-lookup"><span data-stu-id="b3eda-549">*IndexPageTests.cs* contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="b3eda-550">*Helper/公用程式*</span><span class="sxs-lookup"><span data-stu-id="b3eda-550">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="b3eda-551">*Utilities.cs*包含`InitializeDbForTests`用來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="b3eda-551">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="b3eda-552">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument`供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="b3eda-552">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="b3eda-553">*HttpClientExtensions.cs*提供的`SendAsync`多載，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="b3eda-553">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="b3eda-554">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-554">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="b3eda-555">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="b3eda-555">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="b3eda-556">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器， `TestHost`因此和`TestServer`封裝不需要在測試應用程式的專案檔或開發人員中直接封裝參考測試應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="b3eda-556">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="b3eda-557">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="b3eda-557">**Seeding the database for testing**</span></span>

<span data-ttu-id="b3eda-558">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="b3eda-558">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="b3eda-559">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="b3eda-559">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="b3eda-560">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="b3eda-560">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b3eda-561">其他資源</span><span class="sxs-lookup"><span data-stu-id="b3eda-561">Additional resources</span></span>

* [<span data-ttu-id="b3eda-562">單元測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-562">Unit tests</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="b3eda-563">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="b3eda-563">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
* [<span data-ttu-id="b3eda-564">中介軟體</span><span class="sxs-lookup"><span data-stu-id="b3eda-564">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="b3eda-565">測試控制器</span><span class="sxs-lookup"><span data-stu-id="b3eda-565">Test controllers</span></span>](xref:mvc/controllers/testing)
