---
title: ASP.NET Core 中的整合測試
author: rick-anderson
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/integration-tests
ms.openlocfilehash: a01d2881133f39c1a26e4724ae25336c54746bc5
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82766390"
---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="6fd53-103">ASP.NET Core 中的整合測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-103">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="6fd53-104">作者： [Javier Calvarro Nelson](https://github.com/javiercn)、 [Steve Smith](https://ardalis.com/)和[Jos van der Til](https://jvandertil.nl)</span><span class="sxs-lookup"><span data-stu-id="6fd53-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Steve Smith](https://ardalis.com/), and [Jos van der Til](https://jvandertil.nl)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6fd53-105">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="6fd53-105">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="6fd53-106">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-106">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="6fd53-107">本主題假設對單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="6fd53-107">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="6fd53-108">如果不熟悉測試概念，請參閱[.Net Core 中的單元測試和 .NET Standard](/dotnet/core/testing/)主題及其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="6fd53-108">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="6fd53-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="6fd53-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6fd53-110">範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="6fd53-110">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="6fd53-111">如果不熟悉 Razor Pages，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="6fd53-111">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="6fd53-112">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="6fd53-112">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="6fd53-113">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6fd53-113">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="6fd53-114">Razor Pages 單元測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-114">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="6fd53-115">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="6fd53-115">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="6fd53-116">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="6fd53-116">Introduction to integration tests</span></span>

<span data-ttu-id="6fd53-117">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-117">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="6fd53-118">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="6fd53-118">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="6fd53-119">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-119">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="6fd53-120">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="6fd53-120">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="6fd53-121">資料庫</span><span class="sxs-lookup"><span data-stu-id="6fd53-121">Database</span></span>
* <span data-ttu-id="6fd53-122">檔案系統</span><span class="sxs-lookup"><span data-stu-id="6fd53-122">File system</span></span>
* <span data-ttu-id="6fd53-123">網路設備</span><span class="sxs-lookup"><span data-stu-id="6fd53-123">Network appliances</span></span>
* <span data-ttu-id="6fd53-124">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="6fd53-124">Request-response pipeline</span></span>

<span data-ttu-id="6fd53-125">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-125">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="6fd53-126">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="6fd53-126">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="6fd53-127">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-127">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="6fd53-128">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="6fd53-128">Require more code and data processing.</span></span>
* <span data-ttu-id="6fd53-129">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="6fd53-129">Take longer to run.</span></span>

<span data-ttu-id="6fd53-130">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="6fd53-130">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="6fd53-131">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-131">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="6fd53-132">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-132">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="6fd53-133">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-133">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="6fd53-134">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-134">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="6fd53-135">在單元測試中，使用基礎結構 fakes/模擬會產生更快速的測試執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-135">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="6fd53-136">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="6fd53-136">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="6fd53-137">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="6fd53-137">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="6fd53-138">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-138">ASP.NET Core integration tests</span></span>

<span data-ttu-id="6fd53-139">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="6fd53-139">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="6fd53-140">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-140">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="6fd53-141">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="6fd53-141">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="6fd53-142">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="6fd53-142">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="6fd53-143">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="6fd53-143">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="6fd53-144">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="6fd53-144">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="6fd53-145">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="6fd53-145">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="6fd53-146">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-146">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="6fd53-147">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="6fd53-147">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="6fd53-148">執行*Act*測試步驟：用戶端提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="6fd53-148">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="6fd53-149">執行判斷*提示測試步驟*：*實際*的回應會根據*預期*的回應驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="6fd53-149">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="6fd53-150">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="6fd53-150">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="6fd53-151">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="6fd53-151">The test results are reported.</span></span>

<span data-ttu-id="6fd53-152">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="6fd53-152">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="6fd53-153">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="6fd53-153">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="6fd53-154">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="6fd53-154">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="6fd53-155">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-155">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="6fd53-156">此`Microsoft.AspNetCore.Mvc.Testing`套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="6fd53-156">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="6fd53-157">將相依性檔案（*. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="6fd53-157">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="6fd53-158">將[內容根目錄](xref:fundamentals/index#content-root)設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6fd53-158">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="6fd53-159">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用`TestServer`來啟動的 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-159">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="6fd53-160">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="6fd53-160">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="6fd53-161">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="6fd53-161">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="6fd53-162">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="6fd53-162">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="6fd53-163">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-163">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="6fd53-164">Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="6fd53-164">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="6fd53-165">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-165">The only difference is in how the tests are named.</span></span> <span data-ttu-id="6fd53-166">在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如， `IndexPageTests`用來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-166">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="6fd53-167">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名`HomeControllerTests` （例如，測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-167">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="6fd53-168">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="6fd53-168">Test app prerequisites</span></span>

<span data-ttu-id="6fd53-169">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="6fd53-169">The test project must:</span></span>

* <span data-ttu-id="6fd53-170">參考[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)的封裝。</span><span class="sxs-lookup"><span data-stu-id="6fd53-170">Reference the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span>
* <span data-ttu-id="6fd53-171">在專案檔中指定 Web SDK （`<Project Sdk="Microsoft.NET.Sdk.Web">`）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-171">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span>

<span data-ttu-id="6fd53-172">這些必要條件可在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="6fd53-172">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="6fd53-173">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="6fd53-173">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="6fd53-174">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="6fd53-174">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="6fd53-175">xunit</span><span class="sxs-lookup"><span data-stu-id="6fd53-175">xunit</span></span>](https://www.nuget.org/packages/xunit)
* [<span data-ttu-id="6fd53-176">xunit。 visualstudio</span><span class="sxs-lookup"><span data-stu-id="6fd53-176">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [<span data-ttu-id="6fd53-177">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="6fd53-177">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp)

<span data-ttu-id="6fd53-178">Entity Framework Core 也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="6fd53-178">Entity Framework Core is also used in the tests.</span></span> <span data-ttu-id="6fd53-179">應用程式參考：</span><span class="sxs-lookup"><span data-stu-id="6fd53-179">The app references:</span></span>

* [<span data-ttu-id="6fd53-180">AspNetCore. Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="6fd53-180">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [<span data-ttu-id="6fd53-181">AspNetCore. Identity. Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="6fd53-181">Microsoft.AspNetCore.Identity.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [<span data-ttu-id="6fd53-182">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="6fd53-182">Microsoft.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [<span data-ttu-id="6fd53-183">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="6fd53-183">Microsoft.EntityFrameworkCore.InMemory</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [<span data-ttu-id="6fd53-184">Microsoft.entityframeworkcore 工具</span><span class="sxs-lookup"><span data-stu-id="6fd53-184">Microsoft.EntityFrameworkCore.Tools</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a><span data-ttu-id="6fd53-185">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="6fd53-185">SUT environment</span></span>

<span data-ttu-id="6fd53-186">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="6fd53-186">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="6fd53-187">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-187">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="6fd53-188">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="6fd53-188">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="6fd53-189">`TEntryPoint`是 SUT 的進入點類別，通常是`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="6fd53-189">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="6fd53-190">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="6fd53-190">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="6fd53-191">下列`BasicTests`測試類別會`WebApplicationFactory`使用來啟動 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法。 `Get_EndpointsReturnSuccessAndCorrectContentType`</span><span class="sxs-lookup"><span data-stu-id="6fd53-191">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="6fd53-192">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而`Content-Type`標頭則`text/html; charset=utf-8`適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-192">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="6fd53-193">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)會建立自動遵循`HttpClient`重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="6fd53-193">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="6fd53-194">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="6fd53-194">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="6fd53-195">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="6fd53-195">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="6fd53-196">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-196">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="6fd53-197">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="6fd53-197">Customize WebApplicationFactory</span></span>

<span data-ttu-id="6fd53-198">藉由繼承自`WebApplicationFactory`來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="6fd53-198">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="6fd53-199">繼承自`WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-199">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="6fd53-200">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：</span><span class="sxs-lookup"><span data-stu-id="6fd53-200">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="6fd53-201">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的`InitializeDbForTests`資料庫植入是由方法執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-201">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="6fd53-202">此方法會在[整合測試範例：測試應用程式組織](#test-app-organization)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="6fd53-202">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

   <span data-ttu-id="6fd53-203">SUT 的資料庫內容會在其`Startup.ConfigureServices`方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="6fd53-203">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6fd53-204">測試應用程式的`builder.ConfigureServices`回呼會在應用程式的程式`Startup.ConfigureServices`代碼執行*之後*執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-204">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="6fd53-205">執行順序是 ASP.NET Core 3.0 版本之[泛型主機](xref:fundamentals/host/generic-host)的重大變更。</span><span class="sxs-lookup"><span data-stu-id="6fd53-205">The execution order is a breaking change for the [Generic Host](xref:fundamentals/host/generic-host) with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="6fd53-206">若要針對測試使用不同于應用程式資料庫的資料庫，則必須在中`builder.ConfigureServices`取代應用程式的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="6fd53-206">To use a different database for the tests than the app's database, the app's database context must be replaced in `builder.ConfigureServices`.</span></span>

   <span data-ttu-id="6fd53-207">範例應用程式會尋找資料庫內容的服務描述元，並使用描述項來移除服務註冊。</span><span class="sxs-lookup"><span data-stu-id="6fd53-207">The sample app finds the service descriptor for the database context and uses the descriptor to remove the service registration.</span></span> <span data-ttu-id="6fd53-208">接下來，factory 會加入新`ApplicationDbContext`的，其會使用記憶體內部資料庫進行測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-208">Next, the factory adds a new `ApplicationDbContext` that uses an in-memory database for the tests.</span></span>

   <span data-ttu-id="6fd53-209">若要連接到與記憶體中資料庫不同的資料庫，請將連接`UseInMemoryDatabase`內容的呼叫變更為不同的資料庫。</span><span class="sxs-lookup"><span data-stu-id="6fd53-209">To connect to a different database than the in-memory database, change the `UseInMemoryDatabase` call to connect the context to a different database.</span></span> <span data-ttu-id="6fd53-210">若要使用 SQL Server 測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="6fd53-210">To use a SQL Server test database:</span></span>

   * <span data-ttu-id="6fd53-211">參考專案檔中的[Microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-211">Reference the [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet package in the project file.</span></span>
   * <span data-ttu-id="6fd53-212">使用`UseSqlServer`資料庫的連接字串呼叫。</span><span class="sxs-lookup"><span data-stu-id="6fd53-212">Call `UseSqlServer` with a connection string to the database.</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>((options, context) => 
   {
       context.UseSqlServer(
           Configuration.GetConnectionString("TestingDbConnectionString"));
   });
   ```

2. <span data-ttu-id="6fd53-213">使用測試類別`CustomWebApplicationFactory`中的自訂。</span><span class="sxs-lookup"><span data-stu-id="6fd53-213">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="6fd53-214">下列範例會在`IndexPageTests`類別中使用 factory：</span><span class="sxs-lookup"><span data-stu-id="6fd53-214">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="6fd53-215">範例應用程式的用戶端設定為防止`HttpClient`下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="6fd53-215">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="6fd53-216">如稍後的模擬[驗證](#mock-authentication)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="6fd53-216">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="6fd53-217">第一個回應是其中許多測試中具有`Location`標頭的重新導向。</span><span class="sxs-lookup"><span data-stu-id="6fd53-217">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="6fd53-218">一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="6fd53-218">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="6fd53-219">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="6fd53-219">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="6fd53-220">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="6fd53-220">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="6fd53-221">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="6fd53-221">Make a request for the page.</span></span>
1. <span data-ttu-id="6fd53-222">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="6fd53-222">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="6fd53-223">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="6fd53-223">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="6fd53-224">`SendAsync` [範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中的協助程式擴充方法（helper */HttpClientExtensions*）和`GetDocumentAsync` helper 方法（helper */HtmlHelpers*）會使用[AngleSharp](https://anglesharp.github.io/)剖析器，利用下列方法來處理 antiforgery 檢查：</span><span class="sxs-lookup"><span data-stu-id="6fd53-224">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="6fd53-225">`GetDocumentAsync`&ndash;接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回`IHtmlDocument`。</span><span class="sxs-lookup"><span data-stu-id="6fd53-225">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="6fd53-226">`GetDocumentAsync`使用可根據原始`HttpResponseMessage`的來準備*虛擬回應*的 factory。</span><span class="sxs-lookup"><span data-stu-id="6fd53-226">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="6fd53-227">如需詳細資訊，請參閱[AngleSharp 檔](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-227">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="6fd53-228">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-228">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="6fd53-229">的多`SendAsync`載會接受 HTML 窗`IHtmlFormElement`體（）和下列內容：</span><span class="sxs-lookup"><span data-stu-id="6fd53-229">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="6fd53-230">表單的 [提交]`IHtmlElement`按鈕（）</span><span class="sxs-lookup"><span data-stu-id="6fd53-230">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="6fd53-231">表單值集合`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="6fd53-231">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="6fd53-232">提交按鈕（`IHtmlElement`）和表單值`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="6fd53-232">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="6fd53-233">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="6fd53-233">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="6fd53-234">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="6fd53-234">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="6fd53-235">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-235">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="6fd53-236">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="6fd53-236">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="6fd53-237">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="6fd53-237">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="6fd53-238">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)會使用設定`WebApplicationFactory`進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="6fd53-238">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="6fd53-239">`Post_DeleteMessageHandler_ReturnsRedirectToRoot` [範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會示範的用法`WithWebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="6fd53-239">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="6fd53-240">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="6fd53-240">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="6fd53-241">因為`IndexPageTests`類別中的另一項測試會執行刪除資料庫中所有記錄的作業，而且可能會在此`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法之前執行，所以會在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="6fd53-241">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="6fd53-242">在 sut 的要求中，選取`messages`該表單的第一個 [刪除] 按鈕會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="6fd53-242">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="6fd53-243">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="6fd53-243">Client options</span></span>

<span data-ttu-id="6fd53-244">下表顯示建立`HttpClient`實例時可用的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) 。</span><span class="sxs-lookup"><span data-stu-id="6fd53-244">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="6fd53-245">選項</span><span class="sxs-lookup"><span data-stu-id="6fd53-245">Option</span></span> | <span data-ttu-id="6fd53-246">描述</span><span class="sxs-lookup"><span data-stu-id="6fd53-246">Description</span></span> | <span data-ttu-id="6fd53-247">預設</span><span class="sxs-lookup"><span data-stu-id="6fd53-247">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="6fd53-248">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="6fd53-248">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="6fd53-249">取得或設定`HttpClient`實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="6fd53-249">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="6fd53-250">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="6fd53-250">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="6fd53-251">取得或設定`HttpClient`實例的基底位址。</span><span class="sxs-lookup"><span data-stu-id="6fd53-251">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="6fd53-252">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="6fd53-252">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="6fd53-253">取得或設定實例`HttpClient`是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="6fd53-253">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="6fd53-254">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="6fd53-254">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="6fd53-255">取得或設定`HttpClient`實例應遵循的重新導向回應數目上限。</span><span class="sxs-lookup"><span data-stu-id="6fd53-255">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="6fd53-256">7</span><span class="sxs-lookup"><span data-stu-id="6fd53-256">7</span></span> |

<span data-ttu-id="6fd53-257">建立`WebApplicationFactoryClientOptions`類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法（程式碼範例中會顯示預設值）：</span><span class="sxs-lookup"><span data-stu-id="6fd53-257">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="6fd53-258">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="6fd53-258">Inject mock services</span></span>

<span data-ttu-id="6fd53-259">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="6fd53-259">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="6fd53-260">**若要插入模擬服務，SUT 必須具有具有`Startup` `Startup.ConfigureServices`方法的類別。**</span><span class="sxs-lookup"><span data-stu-id="6fd53-260">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="6fd53-261">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="6fd53-261">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="6fd53-262">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="6fd53-262">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="6fd53-263">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-263">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="6fd53-264">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-264">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="6fd53-265">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-265">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="6fd53-266">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-266">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="6fd53-267">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-267">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="6fd53-268">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="6fd53-268">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="6fd53-269">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-269">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="6fd53-270">Mock 服務會將應用程式的`QuoteService`取代為測試應用程式所提供的服務， `TestQuoteService`稱為：</span><span class="sxs-lookup"><span data-stu-id="6fd53-270">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="6fd53-271">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-271">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="6fd53-272">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="6fd53-272">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="6fd53-273">在測試執行期間產生的標記會反映所提供的引號文字`TestQuoteService`，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="6fd53-273">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="6fd53-274">Mock 驗證</span><span class="sxs-lookup"><span data-stu-id="6fd53-274">Mock authentication</span></span>

<span data-ttu-id="6fd53-275">`AuthTests`類別中的測試會檢查安全的端點：</span><span class="sxs-lookup"><span data-stu-id="6fd53-275">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="6fd53-276">將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-276">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="6fd53-277">傳回已驗證使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="6fd53-277">Returns content for an authenticated user.</span></span>

<span data-ttu-id="6fd53-278">在 SUT 中， `/SecurePage`頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-278">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="6fd53-279">如需詳細資訊，請參閱[Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-279">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="6fd53-280">在`Get_SecurePageRedirectsAnUnauthenticatedUser`測試中， [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向，方法是`false`將[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)設為：</span><span class="sxs-lookup"><span data-stu-id="6fd53-280">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="6fd53-281">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="6fd53-281">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="6fd53-282">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="6fd53-282">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="6fd53-283">系統`Location`會檢查回應標頭中的標頭值`http://localhost/Identity/Account/Login`，以確認其開頭為，而不是最終的登入`Location`頁面回應，其中標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="6fd53-283">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="6fd53-284">測試應用程式可以<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>在[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中模擬，以便測試驗證和授權的層面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-284">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="6fd53-285">最小案例會傳回[AuthenticateResult。成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*)：</span><span class="sxs-lookup"><span data-stu-id="6fd53-285">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="6fd53-286">當`TestAuthHandler`驗證配置設定為`Test` （其中`AddAuthentication`已註冊的`ConfigureTestServices`）時，會呼叫來驗證使用者：</span><span class="sxs-lookup"><span data-stu-id="6fd53-286">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="6fd53-287">如需的詳細`WebApplicationFactoryClientOptions`資訊，請參閱[用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="6fd53-287">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="6fd53-288">設定環境</span><span class="sxs-lookup"><span data-stu-id="6fd53-288">Set the environment</span></span>

<span data-ttu-id="6fd53-289">根據預設，會將 SUT 的主機和應用程式環境設定為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="6fd53-289">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="6fd53-290">若要覆寫 SUT 的環境：</span><span class="sxs-lookup"><span data-stu-id="6fd53-290">To override the SUT's environment:</span></span>

* <span data-ttu-id="6fd53-291">設定`ASPNETCORE_ENVIRONMENT`環境變數（ `Staging` `Production`例如，、或其他自訂值，例如`Testing`）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-291">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="6fd53-292">覆`CreateHostBuilder`寫測試應用程式中的，以讀取前面`ASPNETCORE`加上的環境變數。</span><span class="sxs-lookup"><span data-stu-id="6fd53-292">Override `CreateHostBuilder` in the test app to read environment variables prefixed with `ASPNETCORE`.</span></span>

```csharp
protected override IHostBuilder CreateHostBuilder() => 
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="6fd53-293">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="6fd53-293">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="6fd53-294">此`WebApplicationFactory`函式會藉由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) `TEntryPoint`來推斷應用程式[內容根](xref:fundamentals/index#content-root)路徑，其中包含的索引鍵等於元件`System.Reflection.Assembly.FullName`的整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-294">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="6fd53-295">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory`就會回到搜尋方案檔（*.sln*），並將`TEntryPoint`元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="6fd53-295">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="6fd53-296">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="6fd53-296">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="6fd53-297">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="6fd53-297">Disable shadow copying</span></span>

<span data-ttu-id="6fd53-298">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-298">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="6fd53-299">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="6fd53-299">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="6fd53-300">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="6fd53-300">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="6fd53-301">如需詳細資訊，請參閱[使用 JSON 設定 xUnit](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-301">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="6fd53-302">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="6fd53-302">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a><span data-ttu-id="6fd53-303">物件的處置</span><span class="sxs-lookup"><span data-stu-id="6fd53-303">Disposal of objects</span></span>

<span data-ttu-id="6fd53-304">執行`IClassFixture`此實作為測試之後，當 XUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="6fd53-304">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="6fd53-305">如果開發人員所具現化的物件需要處置，請在`IClassFixture`執行時處置它們。</span><span class="sxs-lookup"><span data-stu-id="6fd53-305">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="6fd53-306">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-306">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="6fd53-307">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="6fd53-307">Integration tests sample</span></span>

<span data-ttu-id="6fd53-308">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="6fd53-308">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="6fd53-309">App</span><span class="sxs-lookup"><span data-stu-id="6fd53-309">App</span></span> | <span data-ttu-id="6fd53-310">專案目錄</span><span class="sxs-lookup"><span data-stu-id="6fd53-310">Project directory</span></span> | <span data-ttu-id="6fd53-311">說明</span><span class="sxs-lookup"><span data-stu-id="6fd53-311">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="6fd53-312">訊息應用程式（SUT）</span><span class="sxs-lookup"><span data-stu-id="6fd53-312">Message app (the SUT)</span></span> | <span data-ttu-id="6fd53-313">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="6fd53-313">*src/RazorPagesProject*</span></span> | <span data-ttu-id="6fd53-314">可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="6fd53-314">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="6fd53-315">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="6fd53-315">Test app</span></span> | <span data-ttu-id="6fd53-316">*測試/RazorPagesProject。測試*</span><span class="sxs-lookup"><span data-stu-id="6fd53-316">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="6fd53-317">用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-317">Used to integration test the SUT.</span></span> |

<span data-ttu-id="6fd53-318">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-318">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="6fd53-319">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="6fd53-319">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```console
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="6fd53-320">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="6fd53-320">Message app (SUT) organization</span></span>

<span data-ttu-id="6fd53-321">SUT 是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="6fd53-321">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="6fd53-322">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-322">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="6fd53-323">訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-323">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="6fd53-324">屬性`Text`是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="6fd53-324">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="6fd53-325">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="6fd53-325">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="6fd53-326">應用程式在其資料庫內容類別`AppDbContext` （*data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-326">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="6fd53-327">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="6fd53-327">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="6fd53-328">應用程式包含`/SecurePage`只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="6fd53-328">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="6fd53-329">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="6fd53-329">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="6fd53-330">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="6fd53-330">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="6fd53-331">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="6fd53-331">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="6fd53-332">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-332">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="6fd53-333">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-333">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="6fd53-334">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="6fd53-334">Test app organization</span></span>

<span data-ttu-id="6fd53-335">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-335">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="6fd53-336">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="6fd53-336">Test app directory</span></span> | <span data-ttu-id="6fd53-337">說明</span><span class="sxs-lookup"><span data-stu-id="6fd53-337">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="6fd53-338">*AuthTests*</span><span class="sxs-lookup"><span data-stu-id="6fd53-338">*AuthTests*</span></span> | <span data-ttu-id="6fd53-339">包含的測試方法：</span><span class="sxs-lookup"><span data-stu-id="6fd53-339">Contains test methods for:</span></span><ul><li><span data-ttu-id="6fd53-340">以未驗證的使用者存取安全頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-340">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="6fd53-341">使用 mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>存取已驗證使用者的安全頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-341">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="6fd53-342">取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="6fd53-342">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> |
| <span data-ttu-id="6fd53-343">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="6fd53-343">*BasicTests*</span></span> | <span data-ttu-id="6fd53-344">包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="6fd53-344">Contains a test method for routing and content type.</span></span> |
| <span data-ttu-id="6fd53-345">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="6fd53-345">*IntegrationTests*</span></span> | <span data-ttu-id="6fd53-346">包含使用自訂`WebApplicationFactory`類別之索引頁面的整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-346">Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="6fd53-347">*Helper/公用程式*</span><span class="sxs-lookup"><span data-stu-id="6fd53-347">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="6fd53-348">*Utilities.cs*包含用`InitializeDbForTests`來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="6fd53-348">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="6fd53-349">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument`供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="6fd53-349">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="6fd53-350">*HttpClientExtensions.cs*提供的`SendAsync`多載，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-350">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="6fd53-351">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-351">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="6fd53-352">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-352">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="6fd53-353">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器，因此`TestHost`和`TestServer`封裝不需要測試應用程式的專案檔或測試應用程式中的開發人員設定中的直接套件參考。</span><span class="sxs-lookup"><span data-stu-id="6fd53-353">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="6fd53-354">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="6fd53-354">**Seeding the database for testing**</span></span>

<span data-ttu-id="6fd53-355">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="6fd53-355">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="6fd53-356">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="6fd53-356">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="6fd53-357">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="6fd53-357">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

<span data-ttu-id="6fd53-358">SUT 的資料庫內容會在其`Startup.ConfigureServices`方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="6fd53-358">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6fd53-359">測試應用程式的`builder.ConfigureServices`回呼會在應用程式的程式`Startup.ConfigureServices`代碼執行*之後*執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-359">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="6fd53-360">若要針對測試使用不同的資料庫，則必須在中`builder.ConfigureServices`取代應用程式的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="6fd53-360">To use a different database for the tests, the app's database context must be replaced in `builder.ConfigureServices`.</span></span> <span data-ttu-id="6fd53-361">如需詳細資訊，請參閱[自訂 WebApplicationFactory](#customize-webapplicationfactory)一節。</span><span class="sxs-lookup"><span data-stu-id="6fd53-361">For more information, see the [Customize WebApplicationFactory](#customize-webapplicationfactory) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6fd53-362">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="6fd53-362">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="6fd53-363">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-363">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="6fd53-364">本主題假設對單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="6fd53-364">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="6fd53-365">如果不熟悉測試概念，請參閱[.Net Core 中的單元測試和 .NET Standard](/dotnet/core/testing/)主題及其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="6fd53-365">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="6fd53-366">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="6fd53-366">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6fd53-367">範例應用程式是 Razor Pages 應用程式，並假設對 Razor Pages 有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="6fd53-367">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="6fd53-368">如果不熟悉 Razor Pages，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="6fd53-368">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="6fd53-369">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="6fd53-369">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="6fd53-370">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6fd53-370">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="6fd53-371">Razor Pages 單元測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-371">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="6fd53-372">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="6fd53-372">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="6fd53-373">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="6fd53-373">Introduction to integration tests</span></span>

<span data-ttu-id="6fd53-374">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-374">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="6fd53-375">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="6fd53-375">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="6fd53-376">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-376">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="6fd53-377">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="6fd53-377">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="6fd53-378">資料庫</span><span class="sxs-lookup"><span data-stu-id="6fd53-378">Database</span></span>
* <span data-ttu-id="6fd53-379">檔案系統</span><span class="sxs-lookup"><span data-stu-id="6fd53-379">File system</span></span>
* <span data-ttu-id="6fd53-380">網路設備</span><span class="sxs-lookup"><span data-stu-id="6fd53-380">Network appliances</span></span>
* <span data-ttu-id="6fd53-381">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="6fd53-381">Request-response pipeline</span></span>

<span data-ttu-id="6fd53-382">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-382">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="6fd53-383">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="6fd53-383">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="6fd53-384">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-384">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="6fd53-385">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="6fd53-385">Require more code and data processing.</span></span>
* <span data-ttu-id="6fd53-386">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="6fd53-386">Take longer to run.</span></span>

<span data-ttu-id="6fd53-387">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="6fd53-387">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="6fd53-388">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-388">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="6fd53-389">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-389">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="6fd53-390">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="6fd53-390">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="6fd53-391">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-391">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="6fd53-392">在單元測試中，使用基礎結構 fakes/模擬會產生更快速的測試執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-392">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="6fd53-393">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="6fd53-393">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="6fd53-394">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="6fd53-394">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="6fd53-395">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-395">ASP.NET Core integration tests</span></span>

<span data-ttu-id="6fd53-396">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="6fd53-396">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="6fd53-397">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-397">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="6fd53-398">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="6fd53-398">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="6fd53-399">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="6fd53-399">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="6fd53-400">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="6fd53-400">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="6fd53-401">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="6fd53-401">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="6fd53-402">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="6fd53-402">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="6fd53-403">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-403">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="6fd53-404">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="6fd53-404">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="6fd53-405">執行*Act*測試步驟：用戶端提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="6fd53-405">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="6fd53-406">執行判斷*提示測試步驟*：*實際*的回應會根據*預期*的回應驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="6fd53-406">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="6fd53-407">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="6fd53-407">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="6fd53-408">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="6fd53-408">The test results are reported.</span></span>

<span data-ttu-id="6fd53-409">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="6fd53-409">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="6fd53-410">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="6fd53-410">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="6fd53-411">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="6fd53-411">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="6fd53-412">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-412">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="6fd53-413">此`Microsoft.AspNetCore.Mvc.Testing`套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="6fd53-413">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="6fd53-414">將相依性檔案（*. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="6fd53-414">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="6fd53-415">將[內容根目錄](xref:fundamentals/index#content-root)設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6fd53-415">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="6fd53-416">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用`TestServer`來啟動的 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-416">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="6fd53-417">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="6fd53-417">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="6fd53-418">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="6fd53-418">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="6fd53-419">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="6fd53-419">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="6fd53-420">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-420">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="6fd53-421">Razor Pages 應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="6fd53-421">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="6fd53-422">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-422">The only difference is in how the tests are named.</span></span> <span data-ttu-id="6fd53-423">在 Razor Pages 應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如， `IndexPageTests`用來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-423">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="6fd53-424">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名`HomeControllerTests` （例如，測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-424">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="6fd53-425">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="6fd53-425">Test app prerequisites</span></span>

<span data-ttu-id="6fd53-426">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="6fd53-426">The test project must:</span></span>

* <span data-ttu-id="6fd53-427">參考下列套件：</span><span class="sxs-lookup"><span data-stu-id="6fd53-427">Reference the following packages:</span></span>
  * [<span data-ttu-id="6fd53-428">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="6fd53-428">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [<span data-ttu-id="6fd53-429">AspNetCore 測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-429">Microsoft.AspNetCore.Mvc.Testing</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* <span data-ttu-id="6fd53-430">在專案檔中指定 Web SDK （`<Project Sdk="Microsoft.NET.Sdk.Web">`）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-430">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span> <span data-ttu-id="6fd53-431">參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)時，需要 Web SDK。</span><span class="sxs-lookup"><span data-stu-id="6fd53-431">The Web SDK is required when referencing the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6fd53-432">這些必要條件可在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="6fd53-432">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="6fd53-433">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="6fd53-433">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="6fd53-434">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="6fd53-434">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="6fd53-435">xunit</span><span class="sxs-lookup"><span data-stu-id="6fd53-435">xunit</span></span>](https://www.nuget.org/packages/xunit/)
* [<span data-ttu-id="6fd53-436">xunit。 visualstudio</span><span class="sxs-lookup"><span data-stu-id="6fd53-436">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [<span data-ttu-id="6fd53-437">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="6fd53-437">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a><span data-ttu-id="6fd53-438">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="6fd53-438">SUT environment</span></span>

<span data-ttu-id="6fd53-439">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="6fd53-439">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="6fd53-440">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-440">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="6fd53-441">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="6fd53-441">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="6fd53-442">`TEntryPoint`是 SUT 的進入點類別，通常是`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="6fd53-442">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="6fd53-443">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="6fd53-443">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="6fd53-444">下列`BasicTests`測試類別會`WebApplicationFactory`使用來啟動 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法。 `Get_EndpointsReturnSuccessAndCorrectContentType`</span><span class="sxs-lookup"><span data-stu-id="6fd53-444">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="6fd53-445">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而`Content-Type`標頭則`text/html; charset=utf-8`適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-445">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="6fd53-446">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)會建立自動遵循`HttpClient`重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="6fd53-446">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="6fd53-447">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="6fd53-447">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="6fd53-448">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="6fd53-448">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="6fd53-449">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-449">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="6fd53-450">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="6fd53-450">Customize WebApplicationFactory</span></span>

<span data-ttu-id="6fd53-451">藉由繼承自`WebApplicationFactory`來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="6fd53-451">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="6fd53-452">繼承自`WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-452">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="6fd53-453">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：</span><span class="sxs-lookup"><span data-stu-id="6fd53-453">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="6fd53-454">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的`InitializeDbForTests`資料庫植入是由方法執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-454">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="6fd53-455">此方法會在[整合測試範例：測試應用程式組織](#test-app-organization)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="6fd53-455">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="6fd53-456">使用測試類別`CustomWebApplicationFactory`中的自訂。</span><span class="sxs-lookup"><span data-stu-id="6fd53-456">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="6fd53-457">下列範例會在`IndexPageTests`類別中使用 factory：</span><span class="sxs-lookup"><span data-stu-id="6fd53-457">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="6fd53-458">範例應用程式的用戶端設定為防止`HttpClient`下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="6fd53-458">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="6fd53-459">如稍後的模擬[驗證](#mock-authentication)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="6fd53-459">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="6fd53-460">第一個回應是其中許多測試中具有`Location`標頭的重新導向。</span><span class="sxs-lookup"><span data-stu-id="6fd53-460">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="6fd53-461">一般測試會使用`HttpClient`和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="6fd53-461">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="6fd53-462">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="6fd53-462">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="6fd53-463">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="6fd53-463">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="6fd53-464">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="6fd53-464">Make a request for the page.</span></span>
1. <span data-ttu-id="6fd53-465">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="6fd53-465">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="6fd53-466">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="6fd53-466">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="6fd53-467">`SendAsync` [範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中的協助程式擴充方法（helper */HttpClientExtensions*）和`GetDocumentAsync` helper 方法（helper */HtmlHelpers*）會使用[AngleSharp](https://anglesharp.github.io/)剖析器，利用下列方法來處理 antiforgery 檢查：</span><span class="sxs-lookup"><span data-stu-id="6fd53-467">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="6fd53-468">`GetDocumentAsync`&ndash;接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回`IHtmlDocument`。</span><span class="sxs-lookup"><span data-stu-id="6fd53-468">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="6fd53-469">`GetDocumentAsync`使用可根據原始`HttpResponseMessage`的來準備*虛擬回應*的 factory。</span><span class="sxs-lookup"><span data-stu-id="6fd53-469">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="6fd53-470">如需詳細資訊，請參閱[AngleSharp 檔](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-470">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="6fd53-471">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-471">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="6fd53-472">的多`SendAsync`載會接受 HTML 窗`IHtmlFormElement`體（）和下列內容：</span><span class="sxs-lookup"><span data-stu-id="6fd53-472">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="6fd53-473">表單的 [提交]`IHtmlElement`按鈕（）</span><span class="sxs-lookup"><span data-stu-id="6fd53-473">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="6fd53-474">表單值集合`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="6fd53-474">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="6fd53-475">提交按鈕（`IHtmlElement`）和表單值`IEnumerable<KeyValuePair<string, string>>`（）</span><span class="sxs-lookup"><span data-stu-id="6fd53-475">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="6fd53-476">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="6fd53-476">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="6fd53-477">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="6fd53-477">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="6fd53-478">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-478">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="6fd53-479">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="6fd53-479">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="6fd53-480">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="6fd53-480">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="6fd53-481">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)會使用設定`WebApplicationFactory`進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="6fd53-481">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="6fd53-482">`Post_DeleteMessageHandler_ReturnsRedirectToRoot` [範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會示範的用法`WithWebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="6fd53-482">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="6fd53-483">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="6fd53-483">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="6fd53-484">因為`IndexPageTests`類別中的另一項測試會執行刪除資料庫中所有記錄的作業，而且可能會在此`Post_DeleteMessageHandler_ReturnsRedirectToRoot`方法之前執行，所以會在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="6fd53-484">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="6fd53-485">在 sut 的要求中，選取`messages`該表單的第一個 [刪除] 按鈕會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="6fd53-485">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="6fd53-486">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="6fd53-486">Client options</span></span>

<span data-ttu-id="6fd53-487">下表顯示建立`HttpClient`實例時可用的預設[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) 。</span><span class="sxs-lookup"><span data-stu-id="6fd53-487">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="6fd53-488">選項</span><span class="sxs-lookup"><span data-stu-id="6fd53-488">Option</span></span> | <span data-ttu-id="6fd53-489">描述</span><span class="sxs-lookup"><span data-stu-id="6fd53-489">Description</span></span> | <span data-ttu-id="6fd53-490">預設</span><span class="sxs-lookup"><span data-stu-id="6fd53-490">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="6fd53-491">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="6fd53-491">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="6fd53-492">取得或設定`HttpClient`實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="6fd53-492">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="6fd53-493">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="6fd53-493">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="6fd53-494">取得或設定`HttpClient`實例的基底位址。</span><span class="sxs-lookup"><span data-stu-id="6fd53-494">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="6fd53-495">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="6fd53-495">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="6fd53-496">取得或設定實例`HttpClient`是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="6fd53-496">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="6fd53-497">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="6fd53-497">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="6fd53-498">取得或設定`HttpClient`實例應遵循的重新導向回應數目上限。</span><span class="sxs-lookup"><span data-stu-id="6fd53-498">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="6fd53-499">7</span><span class="sxs-lookup"><span data-stu-id="6fd53-499">7</span></span> |

<span data-ttu-id="6fd53-500">建立`WebApplicationFactoryClientOptions`類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法（程式碼範例中會顯示預設值）：</span><span class="sxs-lookup"><span data-stu-id="6fd53-500">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="6fd53-501">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="6fd53-501">Inject mock services</span></span>

<span data-ttu-id="6fd53-502">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="6fd53-502">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="6fd53-503">**若要插入模擬服務，SUT 必須具有具有`Startup` `Startup.ConfigureServices`方法的類別。**</span><span class="sxs-lookup"><span data-stu-id="6fd53-503">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="6fd53-504">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="6fd53-504">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="6fd53-505">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="6fd53-505">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="6fd53-506">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-506">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="6fd53-507">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-507">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="6fd53-508">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-508">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="6fd53-509">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-509">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="6fd53-510">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-510">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="6fd53-511">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="6fd53-511">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="6fd53-512">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-512">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="6fd53-513">Mock 服務會將應用程式的`QuoteService`取代為測試應用程式所提供的服務， `TestQuoteService`稱為：</span><span class="sxs-lookup"><span data-stu-id="6fd53-513">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="6fd53-514">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="6fd53-514">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="6fd53-515">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="6fd53-515">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="6fd53-516">在測試執行期間產生的標記會反映所提供的引號文字`TestQuoteService`，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="6fd53-516">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="6fd53-517">Mock 驗證</span><span class="sxs-lookup"><span data-stu-id="6fd53-517">Mock authentication</span></span>

<span data-ttu-id="6fd53-518">`AuthTests`類別中的測試會檢查安全的端點：</span><span class="sxs-lookup"><span data-stu-id="6fd53-518">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="6fd53-519">將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-519">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="6fd53-520">傳回已驗證使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="6fd53-520">Returns content for an authenticated user.</span></span>

<span data-ttu-id="6fd53-521">在 SUT 中， `/SecurePage`頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-521">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="6fd53-522">如需詳細資訊，請參閱[Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-522">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="6fd53-523">在`Get_SecurePageRedirectsAnUnauthenticatedUser`測試中， [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向，方法是`false`將[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)設為：</span><span class="sxs-lookup"><span data-stu-id="6fd53-523">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="6fd53-524">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="6fd53-524">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="6fd53-525">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="6fd53-525">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="6fd53-526">系統`Location`會檢查回應標頭中的標頭值`http://localhost/Identity/Account/Login`，以確認其開頭為，而不是最終的登入`Location`頁面回應，其中標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="6fd53-526">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="6fd53-527">測試應用程式可以<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>在[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中模擬，以便測試驗證和授權的層面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-527">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="6fd53-528">最小案例會傳回[AuthenticateResult。成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*)：</span><span class="sxs-lookup"><span data-stu-id="6fd53-528">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="6fd53-529">當`TestAuthHandler`驗證配置設定為`Test` （其中`AddAuthentication`已註冊的`ConfigureTestServices`）時，會呼叫來驗證使用者：</span><span class="sxs-lookup"><span data-stu-id="6fd53-529">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="6fd53-530">如需的詳細`WebApplicationFactoryClientOptions`資訊，請參閱[用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="6fd53-530">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="6fd53-531">設定環境</span><span class="sxs-lookup"><span data-stu-id="6fd53-531">Set the environment</span></span>

<span data-ttu-id="6fd53-532">根據預設，會將 SUT 的主機和應用程式環境設定為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="6fd53-532">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="6fd53-533">若要覆寫 SUT 的環境：</span><span class="sxs-lookup"><span data-stu-id="6fd53-533">To override the SUT's environment:</span></span>

* <span data-ttu-id="6fd53-534">設定`ASPNETCORE_ENVIRONMENT`環境變數（ `Staging` `Production`例如，、或其他自訂值，例如`Testing`）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-534">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="6fd53-535">覆`CreateHostBuilder`寫測試應用程式中的，以讀取前面`ASPNETCORE`加上的環境變數。</span><span class="sxs-lookup"><span data-stu-id="6fd53-535">Override `CreateHostBuilder` in the test app to read environment variables prefixed with `ASPNETCORE`.</span></span>

```csharp
protected override IHostBuilder CreateHostBuilder() => 
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="6fd53-536">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="6fd53-536">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="6fd53-537">此`WebApplicationFactory`函式會藉由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) `TEntryPoint`來推斷應用程式[內容根](xref:fundamentals/index#content-root)路徑，其中包含的索引鍵等於元件`System.Reflection.Assembly.FullName`的整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-537">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="6fd53-538">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory`就會回到搜尋方案檔（*.sln*），並將`TEntryPoint`元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="6fd53-538">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="6fd53-539">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="6fd53-539">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="6fd53-540">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="6fd53-540">Disable shadow copying</span></span>

<span data-ttu-id="6fd53-541">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="6fd53-541">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="6fd53-542">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="6fd53-542">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="6fd53-543">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="6fd53-543">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="6fd53-544">如需詳細資訊，請參閱[使用 JSON 設定 xUnit](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-544">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="6fd53-545">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="6fd53-545">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

<span data-ttu-id="6fd53-546">如果使用 Visual Studio，請將檔案的 [**複製到輸出目錄**] 屬性設定為 [**永遠複製**]。</span><span class="sxs-lookup"><span data-stu-id="6fd53-546">If using Visual Studio, set the file's **Copy to Output Directory** property to **Copy always**.</span></span> <span data-ttu-id="6fd53-547">如果未使用 Visual Studio，請將`Content`目標新增至測試應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="6fd53-547">If not using Visual Studio, add a `Content` target to the test app's project file:</span></span>

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a><span data-ttu-id="6fd53-548">物件的處置</span><span class="sxs-lookup"><span data-stu-id="6fd53-548">Disposal of objects</span></span>

<span data-ttu-id="6fd53-549">執行`IClassFixture`此實作為測試之後，當 XUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="6fd53-549">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="6fd53-550">如果開發人員所具現化的物件需要處置，請在`IClassFixture`執行時處置它們。</span><span class="sxs-lookup"><span data-stu-id="6fd53-550">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="6fd53-551">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-551">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="6fd53-552">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="6fd53-552">Integration tests sample</span></span>

<span data-ttu-id="6fd53-553">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="6fd53-553">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="6fd53-554">App</span><span class="sxs-lookup"><span data-stu-id="6fd53-554">App</span></span> | <span data-ttu-id="6fd53-555">專案目錄</span><span class="sxs-lookup"><span data-stu-id="6fd53-555">Project directory</span></span> | <span data-ttu-id="6fd53-556">說明</span><span class="sxs-lookup"><span data-stu-id="6fd53-556">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="6fd53-557">訊息應用程式（SUT）</span><span class="sxs-lookup"><span data-stu-id="6fd53-557">Message app (the SUT)</span></span> | <span data-ttu-id="6fd53-558">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="6fd53-558">*src/RazorPagesProject*</span></span> | <span data-ttu-id="6fd53-559">可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="6fd53-559">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="6fd53-560">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="6fd53-560">Test app</span></span> | <span data-ttu-id="6fd53-561">*測試/RazorPagesProject。測試*</span><span class="sxs-lookup"><span data-stu-id="6fd53-561">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="6fd53-562">用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-562">Used to integration test the SUT.</span></span> |

<span data-ttu-id="6fd53-563">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-563">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="6fd53-564">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="6fd53-564">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="6fd53-565">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="6fd53-565">Message app (SUT) organization</span></span>

<span data-ttu-id="6fd53-566">SUT 是具有下列特性的 Razor Pages 訊息系統：</span><span class="sxs-lookup"><span data-stu-id="6fd53-566">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="6fd53-567">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-567">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="6fd53-568">訊息是由`Message`類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和`Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-568">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="6fd53-569">屬性`Text`是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="6fd53-569">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="6fd53-570">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="6fd53-570">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="6fd53-571">應用程式在其資料庫內容類別`AppDbContext` （*data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-571">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="6fd53-572">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="6fd53-572">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="6fd53-573">應用程式包含`/SecurePage`只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="6fd53-573">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="6fd53-574">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="6fd53-574">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="6fd53-575">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="6fd53-575">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="6fd53-576">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="6fd53-576">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="6fd53-577">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-577">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="6fd53-578">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="6fd53-578">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="6fd53-579">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="6fd53-579">Test app organization</span></span>

<span data-ttu-id="6fd53-580">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="6fd53-580">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="6fd53-581">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="6fd53-581">Test app directory</span></span> | <span data-ttu-id="6fd53-582">說明</span><span class="sxs-lookup"><span data-stu-id="6fd53-582">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="6fd53-583">*AuthTests*</span><span class="sxs-lookup"><span data-stu-id="6fd53-583">*AuthTests*</span></span> | <span data-ttu-id="6fd53-584">包含的測試方法：</span><span class="sxs-lookup"><span data-stu-id="6fd53-584">Contains test methods for:</span></span><ul><li><span data-ttu-id="6fd53-585">以未驗證的使用者存取安全頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-585">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="6fd53-586">使用 mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>存取已驗證使用者的安全頁面。</span><span class="sxs-lookup"><span data-stu-id="6fd53-586">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="6fd53-587">取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="6fd53-587">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> |
| <span data-ttu-id="6fd53-588">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="6fd53-588">*BasicTests*</span></span> | <span data-ttu-id="6fd53-589">包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="6fd53-589">Contains a test method for routing and content type.</span></span> |
| <span data-ttu-id="6fd53-590">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="6fd53-590">*IntegrationTests*</span></span> | <span data-ttu-id="6fd53-591">包含使用自訂`WebApplicationFactory`類別之索引頁面的整合測試。</span><span class="sxs-lookup"><span data-stu-id="6fd53-591">Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="6fd53-592">*Helper/公用程式*</span><span class="sxs-lookup"><span data-stu-id="6fd53-592">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="6fd53-593">*Utilities.cs*包含用`InitializeDbForTests`來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="6fd53-593">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="6fd53-594">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument`供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="6fd53-594">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="6fd53-595">*HttpClientExtensions.cs*提供的`SendAsync`多載，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="6fd53-595">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="6fd53-596">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-596">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="6fd53-597">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="6fd53-597">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="6fd53-598">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器，因此`TestHost`和`TestServer`封裝不需要測試應用程式的專案檔或測試應用程式中的開發人員設定中的直接套件參考。</span><span class="sxs-lookup"><span data-stu-id="6fd53-598">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="6fd53-599">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="6fd53-599">**Seeding the database for testing**</span></span>

<span data-ttu-id="6fd53-600">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="6fd53-600">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="6fd53-601">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="6fd53-601">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="6fd53-602">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="6fd53-602">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6fd53-603">其他資源</span><span class="sxs-lookup"><span data-stu-id="6fd53-603">Additional resources</span></span>

* [<span data-ttu-id="6fd53-604">單元測試</span><span class="sxs-lookup"><span data-stu-id="6fd53-604">Unit tests</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:test/razor-pages-tests>
* <xref:fundamentals/middleware/index>
* <xref:mvc/controllers/testing>
