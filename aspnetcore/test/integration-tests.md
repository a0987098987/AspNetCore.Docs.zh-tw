---
<span data-ttu-id="c6dbd-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-102">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-103">'Identity'</span></span>
- <span data-ttu-id="c6dbd-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-105">'Razor'</span></span>
- <span data-ttu-id="c6dbd-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-106">'SignalR' uid:</span></span> 

---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="c6dbd-107">ASP.NET Core 中的整合測試</span><span class="sxs-lookup"><span data-stu-id="c6dbd-107">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="c6dbd-108">作者： [Javier Calvarro Nelson](https://github.com/javiercn)、 [Steve Smith](https://ardalis.com/)和[Jos van der Til](https://jvandertil.nl)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-108">By [Javier Calvarro Nelson](https://github.com/javiercn), [Steve Smith](https://ardalis.com/), and [Jos van der Til](https://jvandertil.nl)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c6dbd-109">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-109">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="c6dbd-110">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-110">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="c6dbd-111">本主題假設對單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-111">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="c6dbd-112">如果不熟悉測試概念，請參閱[.Net Core 中的單元測試和 .NET Standard](/dotnet/core/testing/)主題及其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-112">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="c6dbd-113">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c6dbd-114">範例應用程式是 Razor 頁面應用程式，並假設有對頁面的基本瞭解 Razor 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-114">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="c6dbd-115">如果不熟悉 Razor 頁面，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-115">If unfamiliar with Razor Pages, see the following topics:</span></span>

* <span data-ttu-id="c6dbd-116">[頁面簡介 Razor](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-116">[Introduction to Razor Pages](xref:razor-pages/index)</span></span>
* <span data-ttu-id="c6dbd-117">[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-117">[Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start)</span></span>
* <span data-ttu-id="c6dbd-118">[Razor頁面單元測試](xref:test/razor-pages-tests)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-118">[Razor Pages unit tests](xref:test/razor-pages-tests)</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-119">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-119">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="c6dbd-120">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="c6dbd-120">Introduction to integration tests</span></span>

<span data-ttu-id="c6dbd-121">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-121">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="c6dbd-122">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-122">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="c6dbd-123">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-123">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="c6dbd-124">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-124">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="c6dbd-125">資料庫</span><span class="sxs-lookup"><span data-stu-id="c6dbd-125">Database</span></span>
* <span data-ttu-id="c6dbd-126">檔案系統</span><span class="sxs-lookup"><span data-stu-id="c6dbd-126">File system</span></span>
* <span data-ttu-id="c6dbd-127">網路設備</span><span class="sxs-lookup"><span data-stu-id="c6dbd-127">Network appliances</span></span>
* <span data-ttu-id="c6dbd-128">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="c6dbd-128">Request-response pipeline</span></span>

<span data-ttu-id="c6dbd-129">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-129">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="c6dbd-130">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-130">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="c6dbd-131">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-131">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="c6dbd-132">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-132">Require more code and data processing.</span></span>
* <span data-ttu-id="c6dbd-133">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-133">Take longer to run.</span></span>

<span data-ttu-id="c6dbd-134">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-134">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="c6dbd-135">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-135">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="c6dbd-136">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-136">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="c6dbd-137">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-137">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="c6dbd-138">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-138">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="c6dbd-139">在單元測試中，使用基礎結構 fakes/模擬會產生更快速的測試執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-139">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-140">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-140">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="c6dbd-141">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="c6dbd-141">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="c6dbd-142">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="c6dbd-142">ASP.NET Core integration tests</span></span>

<span data-ttu-id="c6dbd-143">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-143">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="c6dbd-144">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-144">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="c6dbd-145">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-145">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="c6dbd-146">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-146">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="c6dbd-147">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-147">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="c6dbd-148">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-148">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="c6dbd-149">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-149">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="c6dbd-150">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-150">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="c6dbd-151">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-151">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="c6dbd-152">執行*Act*測試步驟：用戶端提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-152">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="c6dbd-153">執行判斷*提示測試步驟*：*實際*的回應會根據*預期*的回應驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-153">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="c6dbd-154">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-154">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="c6dbd-155">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-155">The test results are reported.</span></span>

<span data-ttu-id="c6dbd-156">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-156">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="c6dbd-157">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-157">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="c6dbd-158">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-158">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="c6dbd-159">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-159">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="c6dbd-160">此 `Microsoft.AspNetCore.Mvc.Testing` 套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-160">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="c6dbd-161">將相依性檔案（*. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-161">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="c6dbd-162">將[內容根目錄](xref:fundamentals/index#content-root)設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-162">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="c6dbd-163">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用來啟動的 SUT `TestServer` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-163">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="c6dbd-164">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-164">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-165">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-165">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="c6dbd-166">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-166">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="c6dbd-167">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-167">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="c6dbd-168">Razor頁面應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-168">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="c6dbd-169">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-169">The only difference is in how the tests are named.</span></span> <span data-ttu-id="c6dbd-170">在 Razor 頁面應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如，用 `IndexPageTests` 來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-170">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="c6dbd-171">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名（例如， `HomeControllerTests` 測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-171">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="c6dbd-172">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="c6dbd-172">Test app prerequisites</span></span>

<span data-ttu-id="c6dbd-173">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-173">The test project must:</span></span>

* <span data-ttu-id="c6dbd-174">參考[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)的封裝。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-174">Reference the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span>
* <span data-ttu-id="c6dbd-175">在專案檔中指定 Web SDK （ `<Project Sdk="Microsoft.NET.Sdk.Web">` ）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-175">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span>

<span data-ttu-id="c6dbd-176">這些必要條件可在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-176">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="c6dbd-177">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-177">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="c6dbd-178">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-178">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="c6dbd-179">xunit</span><span class="sxs-lookup"><span data-stu-id="c6dbd-179">xunit</span></span>](https://www.nuget.org/packages/xunit)
* [<span data-ttu-id="c6dbd-180">xunit。 visualstudio</span><span class="sxs-lookup"><span data-stu-id="c6dbd-180">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [<span data-ttu-id="c6dbd-181">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="c6dbd-181">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp)

<span data-ttu-id="c6dbd-182">Entity Framework Core 也會用於測試中。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-182">Entity Framework Core is also used in the tests.</span></span> <span data-ttu-id="c6dbd-183">應用程式參考：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-183">The app references:</span></span>

* [<span data-ttu-id="c6dbd-184">AspNetCore. Microsoft.entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="c6dbd-184">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* <span data-ttu-id="c6dbd-185">[AspNetCore Identity 。Microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-185">[Microsoft.AspNetCore.Identity.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)</span></span>
* [<span data-ttu-id="c6dbd-186">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c6dbd-186">Microsoft.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [<span data-ttu-id="c6dbd-187">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="c6dbd-187">Microsoft.EntityFrameworkCore.InMemory</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [<span data-ttu-id="c6dbd-188">Microsoft.entityframeworkcore 工具</span><span class="sxs-lookup"><span data-stu-id="c6dbd-188">Microsoft.EntityFrameworkCore.Tools</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a><span data-ttu-id="c6dbd-189">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="c6dbd-189">SUT environment</span></span>

<span data-ttu-id="c6dbd-190">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-190">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="c6dbd-191">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="c6dbd-191">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="c6dbd-192">[WebApplicationFactory \<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)是用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-192">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="c6dbd-193">`TEntryPoint`是 SUT 的進入點類別，通常是 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-193">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="c6dbd-194">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-194">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="c6dbd-195">下列測試類別會 `BasicTests` 使用 `WebApplicationFactory` 來啟動 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法 `Get_EndpointsReturnSuccessAndCorrectContentType` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-195">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="c6dbd-196">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而 `Content-Type` 標頭則 `text/html; charset=utf-8` 適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-196">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="c6dbd-197">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) `HttpClient` 會建立自動遵循重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-197">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="c6dbd-198">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-198">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="c6dbd-199">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-199">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="c6dbd-200">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-200">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="c6dbd-201">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="c6dbd-201">Customize WebApplicationFactory</span></span>

<span data-ttu-id="c6dbd-202">藉由繼承自 `WebApplicationFactory` 來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-202">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="c6dbd-203">繼承自 `WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-203">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="c6dbd-204">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-204">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="c6dbd-205">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的資料庫植入是由 `InitializeDbForTests` 方法執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-205">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="c6dbd-206">此方法會在[整合測試範例：測試應用程式組織](#test-app-organization)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-206">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

   <span data-ttu-id="c6dbd-207">SUT 的資料庫內容會在其方法中註冊 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-207">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="c6dbd-208">測試應用程式的 `builder.ConfigureServices` 回呼會在*after*應用程式的程式 `Startup.ConfigureServices` 代碼執行之後執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-208">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="c6dbd-209">執行順序是 ASP.NET Core 3.0 版本之[泛型主機](xref:fundamentals/host/generic-host)的重大變更。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-209">The execution order is a breaking change for the [Generic Host](xref:fundamentals/host/generic-host) with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="c6dbd-210">若要針對測試使用不同于應用程式資料庫的資料庫，則必須在中取代應用程式的資料庫內容 `builder.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-210">To use a different database for the tests than the app's database, the app's database context must be replaced in `builder.ConfigureServices`.</span></span>

   <span data-ttu-id="c6dbd-211">對於仍然使用 [Web 主機} （x：基礎/主機/Web 主機）的 SUTs，測試應用程式的 `builder.ConfigureServices` 回呼會在 SUT 的程式碼*之前*執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-211">For SUTs that still use the [Web Host}(xref:fundamentals/host/web-host), the test app's `builder.ConfigureServices` callback is executed *before* the SUT's `Startup.ConfigureServices` code.</span></span> <span data-ttu-id="c6dbd-212">測試應用程式的 `builder.ConfigureTestServices` 回呼會*在之後*執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-212">The test app's `builder.ConfigureTestServices` callback is executed *after*.</span></span>

   <span data-ttu-id="c6dbd-213">範例應用程式會尋找資料庫內容的服務描述元，並使用描述項來移除服務註冊。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-213">The sample app finds the service descriptor for the database context and uses the descriptor to remove the service registration.</span></span> <span data-ttu-id="c6dbd-214">接下來，factory 會加入新 `ApplicationDbContext` 的，其會使用記憶體內部資料庫進行測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-214">Next, the factory adds a new `ApplicationDbContext` that uses an in-memory database for the tests.</span></span>

   <span data-ttu-id="c6dbd-215">若要連接到與記憶體中資料庫不同的資料庫，請將 `UseInMemoryDatabase` 連接內容的呼叫變更為不同的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-215">To connect to a different database than the in-memory database, change the `UseInMemoryDatabase` call to connect the context to a different database.</span></span> <span data-ttu-id="c6dbd-216">若要使用 SQL Server 測試資料庫：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-216">To use a SQL Server test database:</span></span>

   * <span data-ttu-id="c6dbd-217">參考專案檔中的[Microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-217">Reference the [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet package in the project file.</span></span>
   * <span data-ttu-id="c6dbd-218">`UseSqlServer`使用資料庫的連接字串呼叫。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-218">Call `UseSqlServer` with a connection string to the database.</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>((options, context) => 
   {
       context.UseSqlServer(
           Configuration.GetConnectionString("TestingDbConnectionString"));
   });
   ```

2. <span data-ttu-id="c6dbd-219">使用 `CustomWebApplicationFactory` 測試類別中的自訂。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-219">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="c6dbd-220">下列範例會在類別中使用 factory `IndexPageTests` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-220">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="c6dbd-221">範例應用程式的用戶端設定為防止 `HttpClient` 下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-221">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="c6dbd-222">如稍後的模擬[驗證](#mock-authentication)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-222">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="c6dbd-223">第一個回應是其中許多測試中具有標頭的重新導向 `Location` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-223">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="c6dbd-224">一般測試會使用 `HttpClient` 和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-224">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="c6dbd-225">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-225">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="c6dbd-226">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-226">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="c6dbd-227">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-227">Make a request for the page.</span></span>
1. <span data-ttu-id="c6dbd-228">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-228">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="c6dbd-229">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-229">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="c6dbd-230">`SendAsync`範例應用程式中的協助程式擴充方法（helper */HttpClientExtensions*）和 `GetDocumentAsync` helper 方法（helper */HtmlHelpers*）會使用[sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) [AngleSharp](https://anglesharp.github.io/)剖析器，利用下列方法來處理 antiforgery 檢查：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-230">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="c6dbd-231">`GetDocumentAsync`&ndash;接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回 `IHtmlDocument` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-231">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="c6dbd-232">`GetDocumentAsync`使用可根據原始的來準備*虛擬回應*的 factory `HttpResponseMessage` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-232">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="c6dbd-233">如需詳細資訊，請參閱[AngleSharp 檔](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-233">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="c6dbd-234">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-234">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="c6dbd-235">的多載會 `SendAsync` 接受 HTML 表單（ `IHtmlFormElement` ）和下列內容：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-235">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="c6dbd-236">表單的 [提交] 按鈕（ `IHtmlElement` ）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-236">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="c6dbd-237">表單值集合（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-237">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="c6dbd-238">提交按鈕（ `IHtmlElement` ）和表單值（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-238">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-239">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-239">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="c6dbd-240">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-240">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="c6dbd-241">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-241">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="c6dbd-242">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-242">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="c6dbd-243">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="c6dbd-243">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="c6dbd-244">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) `WebApplicationFactory` 會使用設定進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-244">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="c6dbd-245">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會示範的用法 `WithWebHostBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-245">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="c6dbd-246">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-246">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="c6dbd-247">因為類別中的另一項測試 `IndexPageTests` 會執行刪除資料庫中所有記錄的作業，而且可能會在此方法之前執行，所以會 `Post_DeleteMessageHandler_ReturnsRedirectToRoot` 在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-247">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="c6dbd-248">在 sut 的要求中，選取該表單的第一個 [刪除] 按鈕 `messages` 會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-248">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="c6dbd-249">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="c6dbd-249">Client options</span></span>

<span data-ttu-id="c6dbd-250">下表顯示建立實例時[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)可用的預設 WebApplicationFactoryClientOptions `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-250">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="c6dbd-251">選項</span><span class="sxs-lookup"><span data-stu-id="c6dbd-251">Option</span></span> | <span data-ttu-id="c6dbd-252">描述</span><span class="sxs-lookup"><span data-stu-id="c6dbd-252">Description</span></span> | <span data-ttu-id="c6dbd-253">預設</span><span class="sxs-lookup"><span data-stu-id="c6dbd-253">Default</span></span> |
| ---
<span data-ttu-id="c6dbd-254">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-254">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-255">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-255">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-256">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-256">'Identity'</span></span>
- <span data-ttu-id="c6dbd-257">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-257">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-258">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-258">'Razor'</span></span>
- <span data-ttu-id="c6dbd-259">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-259">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-260">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-260">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-261">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-261">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-262">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-262">'Identity'</span></span>
- <span data-ttu-id="c6dbd-263">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-263">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-264">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-264">'Razor'</span></span>
- <span data-ttu-id="c6dbd-265">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-265">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-266">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-266">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-267">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-267">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-268">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-268">'Identity'</span></span>
- <span data-ttu-id="c6dbd-269">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-269">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-270">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-270">'Razor'</span></span>
- <span data-ttu-id="c6dbd-271">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-271">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-272">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-272">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-273">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-273">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-274">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-274">'Identity'</span></span>
- <span data-ttu-id="c6dbd-275">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-275">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-276">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-276">'Razor'</span></span>
- <span data-ttu-id="c6dbd-277">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-277">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-278">------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-278">------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-279">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-279">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-280">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-280">'Identity'</span></span>
- <span data-ttu-id="c6dbd-281">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-281">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-282">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-282">'Razor'</span></span>
- <span data-ttu-id="c6dbd-283">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-283">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-284">---- | |[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) |取得或設定 `HttpClient` 實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-284">---- | | [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span><span data-ttu-id="c6dbd-285"> | `true`| |[BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) |取得或設定實例的基底位址 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-285"> | `true` | | [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Gets or sets the base address of `HttpClient` instances.</span></span><span data-ttu-id="c6dbd-286"> | `http://localhost`| |[HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) |取得或設定 `HttpClient` 實例是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-286"> | `http://localhost` | | [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Gets or sets whether `HttpClient` instances should handle cookies.</span></span><span data-ttu-id="c6dbd-287"> | `true`| |[MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) |取得或設定實例應遵循的重新導向回應數目上限 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-287"> | `true` | | [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> <span data-ttu-id="c6dbd-288">|7 |</span><span class="sxs-lookup"><span data-stu-id="c6dbd-288">| 7 |</span></span>

<span data-ttu-id="c6dbd-289">建立 `WebApplicationFactoryClientOptions` 類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法（程式碼範例中會顯示預設值）：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-289">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="c6dbd-290">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="c6dbd-290">Inject mock services</span></span>

<span data-ttu-id="c6dbd-291">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-291">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="c6dbd-292">**若要插入模擬服務，SUT 必須具有 `Startup` 具有方法的類別 `Startup.ConfigureServices` 。**</span><span class="sxs-lookup"><span data-stu-id="c6dbd-292">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="c6dbd-293">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-293">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="c6dbd-294">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-294">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="c6dbd-295">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-295">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="c6dbd-296">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-296">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="c6dbd-297">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-297">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="c6dbd-298">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-298">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="c6dbd-299">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-299">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="c6dbd-300">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-300">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="c6dbd-301">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-301">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="c6dbd-302">Mock 服務會將應用程式的取代為 `QuoteService` 測試應用程式所提供的服務，稱為 `TestQuoteService` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-302">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="c6dbd-303">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-303">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="c6dbd-304">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-304">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="c6dbd-305">在測試執行期間產生的標記會反映所提供的引號文字 `TestQuoteService` ，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-305">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="c6dbd-306">Mock 驗證</span><span class="sxs-lookup"><span data-stu-id="c6dbd-306">Mock authentication</span></span>

<span data-ttu-id="c6dbd-307">類別中的測試 `AuthTests` 會檢查安全的端點：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-307">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="c6dbd-308">將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-308">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="c6dbd-309">傳回已驗證使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-309">Returns content for an authenticated user.</span></span>

<span data-ttu-id="c6dbd-310">在 SUT 中， `/SecurePage` 頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-310">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="c6dbd-311">如需詳細資訊，請參閱[ Razor 頁面授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-311">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="c6dbd-312">在 `Get_SecurePageRedirectsAnUnauthenticatedUser` 測試中， [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向，方法是將[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)設為 `false` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-312">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="c6dbd-313">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-313">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="c6dbd-314">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-314">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="c6dbd-315">`Location`系統會檢查回應標頭中的標頭值，以確認其開頭為 `http://localhost/Identity/Account/Login` ，而不是最終的登入頁面回應，其中 `Location` 標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-315">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="c6dbd-316">測試應用程式可以 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 在[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中模擬，以便測試驗證和授權的層面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-316">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="c6dbd-317">最小案例會傳回[AuthenticateResult。成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*)：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-317">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="c6dbd-318">`TestAuthHandler`當驗證配置設定為 `Test` （其中已註冊的）時，會呼叫來驗證使用者 `AddAuthentication` `ConfigureTestServices` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-318">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="c6dbd-319">如需的詳細資訊 `WebApplicationFactoryClientOptions` ，請參閱[用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-319">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="c6dbd-320">設定環境</span><span class="sxs-lookup"><span data-stu-id="c6dbd-320">Set the environment</span></span>

<span data-ttu-id="c6dbd-321">根據預設，會將 SUT 的主機和應用程式環境設定為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-321">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="c6dbd-322">若要在使用時覆寫 SUT 的環境 `IHostBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-322">To override the SUT's environment when using `IHostBuilder`:</span></span>

* <span data-ttu-id="c6dbd-323">設定 `ASPNETCORE_ENVIRONMENT` 環境變數（例如，、 `Staging` `Production` 或其他自訂值，例如 `Testing` ）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-323">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="c6dbd-324">覆寫 `CreateHostBuilder` 測試應用程式中的，以讀取前面加上的環境變數 `ASPNETCORE` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-324">Override `CreateHostBuilder` in the test app to read environment variables prefixed with `ASPNETCORE`.</span></span>

```csharp
protected override IHostBuilder CreateHostBuilder() =>
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

<span data-ttu-id="c6dbd-325">如果 SUT 使用 Web 主機（ `IWebHostBuilder` ），請覆寫 `CreateWebHostBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-325">If the SUT uses the Web Host (`IWebHostBuilder`), override `CreateWebHostBuilder`:</span></span>

```csharp
protected override IWebHostBuilder CreateWebHostBuilder() =>
    base.CreateWebHostBuilder().UseEnvironment("Testing");
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="c6dbd-326">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="c6dbd-326">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="c6dbd-327">此函式會藉 `WebApplicationFactory` 由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式[內容根](xref:fundamentals/index#content-root)路徑，其中包含的索引鍵等於元件的整合測試 `TEntryPoint` `System.Reflection.Assembly.FullName` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-327">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="c6dbd-328">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory` 就會回到搜尋方案檔（*.sln*），並將 `TEntryPoint` 元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-328">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="c6dbd-329">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-329">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="c6dbd-330">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="c6dbd-330">Disable shadow copying</span></span>

<span data-ttu-id="c6dbd-331">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-331">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="c6dbd-332">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-332">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="c6dbd-333">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-333">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="c6dbd-334">如需詳細資訊，請參閱[使用 JSON 設定 xUnit](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-334">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="c6dbd-335">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-335">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a><span data-ttu-id="c6dbd-336">物件的處置</span><span class="sxs-lookup"><span data-stu-id="c6dbd-336">Disposal of objects</span></span>

<span data-ttu-id="c6dbd-337">執行此實作為測試之後 `IClassFixture` ，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-337">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="c6dbd-338">如果開發人員所具現化的物件需要處置，請在執行時處置它們 `IClassFixture` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-338">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="c6dbd-339">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-339">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="c6dbd-340">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="c6dbd-340">Integration tests sample</span></span>

<span data-ttu-id="c6dbd-341">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-341">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="c6dbd-342">App</span><span class="sxs-lookup"><span data-stu-id="c6dbd-342">App</span></span> | <span data-ttu-id="c6dbd-343">專案目錄</span><span class="sxs-lookup"><span data-stu-id="c6dbd-343">Project directory</span></span> | <span data-ttu-id="c6dbd-344">說明</span><span class="sxs-lookup"><span data-stu-id="c6dbd-344">Description</span></span> |
| --- | ---
<span data-ttu-id="c6dbd-345">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-345">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-346">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-346">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-347">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-347">'Identity'</span></span>
- <span data-ttu-id="c6dbd-348">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-348">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-349">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-349">'Razor'</span></span>
- <span data-ttu-id="c6dbd-350">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-350">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-351">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-351">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-352">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-353">'Identity'</span></span>
- <span data-ttu-id="c6dbd-354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-354">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-355">'Razor'</span></span>
- <span data-ttu-id="c6dbd-356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-356">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-357">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-357">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-358">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-358">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-359">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-359">'Identity'</span></span>
- <span data-ttu-id="c6dbd-360">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-360">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-361">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-361">'Razor'</span></span>
- <span data-ttu-id="c6dbd-362">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-362">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-363">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-363">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-364">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-364">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-365">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-365">'Identity'</span></span>
- <span data-ttu-id="c6dbd-366">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-366">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-367">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-367">'Razor'</span></span>
- <span data-ttu-id="c6dbd-368">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-368">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-369">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-369">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-370">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-370">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-371">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-371">'Identity'</span></span>
- <span data-ttu-id="c6dbd-372">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-372">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-373">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-373">'Razor'</span></span>
- <span data-ttu-id="c6dbd-374">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-374">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-375">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-375">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-376">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-376">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-377">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-377">'Identity'</span></span>
- <span data-ttu-id="c6dbd-378">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-378">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-379">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-379">'Razor'</span></span>
- <span data-ttu-id="c6dbd-380">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-380">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-381">--------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-381">--------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-382">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-382">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-383">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-383">'Identity'</span></span>
- <span data-ttu-id="c6dbd-384">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-384">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-385">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-385">'Razor'</span></span>
- <span data-ttu-id="c6dbd-386">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-386">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-387">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-387">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-388">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-388">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-389">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-389">'Identity'</span></span>
- <span data-ttu-id="c6dbd-390">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-390">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-391">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-391">'Razor'</span></span>
- <span data-ttu-id="c6dbd-392">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-392">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-393">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-393">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-394">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-394">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-395">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-395">'Identity'</span></span>
- <span data-ttu-id="c6dbd-396">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-396">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-397">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-397">'Razor'</span></span>
- <span data-ttu-id="c6dbd-398">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-398">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-399">------ | |訊息應用程式（SUT） |*src/RazorPagesProject* |可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-399">------ | | Message app (the SUT) | *src/RazorPagesProject* | Allows a user to add, delete one, delete all, and analyze messages.</span></span> <span data-ttu-id="c6dbd-400">| |測試應用程式 |*測試/RazorPagesProject。測試*|用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-400">| | Test app | *tests/RazorPagesProject.Tests* | Used to integration test the SUT.</span></span> |

<span data-ttu-id="c6dbd-401">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-401">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="c6dbd-402">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-402">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```console
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="c6dbd-403">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="c6dbd-403">Message app (SUT) organization</span></span>

<span data-ttu-id="c6dbd-404">SUT 是 Razor 具有下列特性的頁面訊息系統：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-404">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="c6dbd-405">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-405">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="c6dbd-406">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-406">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="c6dbd-407">`Text`屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-407">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="c6dbd-408">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-408">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="c6dbd-409">應用程式在其資料庫內容類別（ `AppDbContext` *Data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-409">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="c6dbd-410">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-410">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="c6dbd-411">應用程式包含 `/SecurePage` 只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-411">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="c6dbd-412">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-412">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="c6dbd-413">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-413">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="c6dbd-414">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-414">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="c6dbd-415">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，但 Razor 頁面支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-415">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="c6dbd-416">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-416">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="c6dbd-417">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="c6dbd-417">Test app organization</span></span>

<span data-ttu-id="c6dbd-418">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-418">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="c6dbd-419">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="c6dbd-419">Test app directory</span></span> | <span data-ttu-id="c6dbd-420">說明</span><span class="sxs-lookup"><span data-stu-id="c6dbd-420">Description</span></span> |
| ---
<span data-ttu-id="c6dbd-421">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-421">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-422">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-422">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-423">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-423">'Identity'</span></span>
- <span data-ttu-id="c6dbd-424">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-424">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-425">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-425">'Razor'</span></span>
- <span data-ttu-id="c6dbd-426">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-426">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-427">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-427">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-428">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-428">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-429">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-429">'Identity'</span></span>
- <span data-ttu-id="c6dbd-430">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-430">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-431">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-431">'Razor'</span></span>
- <span data-ttu-id="c6dbd-432">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-432">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-433">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-433">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-434">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-434">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-435">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-435">'Identity'</span></span>
- <span data-ttu-id="c6dbd-436">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-436">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-437">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-437">'Razor'</span></span>
- <span data-ttu-id="c6dbd-438">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-438">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-439">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-439">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-440">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-440">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-441">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-441">'Identity'</span></span>
- <span data-ttu-id="c6dbd-442">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-442">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-443">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-443">'Razor'</span></span>
- <span data-ttu-id="c6dbd-444">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-444">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-445">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-445">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-446">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-446">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-447">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-447">'Identity'</span></span>
- <span data-ttu-id="c6dbd-448">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-448">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-449">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-449">'Razor'</span></span>
- <span data-ttu-id="c6dbd-450">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-450">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-451">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-451">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-452">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-452">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-453">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-453">'Identity'</span></span>
- <span data-ttu-id="c6dbd-454">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-454">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-455">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-455">'Razor'</span></span>
- <span data-ttu-id="c6dbd-456">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-456">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-457">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-457">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-458">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-458">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-459">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-459">'Identity'</span></span>
- <span data-ttu-id="c6dbd-460">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-460">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-461">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-461">'Razor'</span></span>
- <span data-ttu-id="c6dbd-462">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-462">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-463">--------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-463">--------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-464">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-464">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-465">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-465">'Identity'</span></span>
- <span data-ttu-id="c6dbd-466">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-466">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-467">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-467">'Razor'</span></span>
- <span data-ttu-id="c6dbd-468">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-468">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-469">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-469">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-470">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-470">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-471">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-471">'Identity'</span></span>
- <span data-ttu-id="c6dbd-472">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-472">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-473">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-473">'Razor'</span></span>
- <span data-ttu-id="c6dbd-474">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-474">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-475">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-475">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-476">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-476">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-477">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-477">'Identity'</span></span>
- <span data-ttu-id="c6dbd-478">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-478">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-479">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-479">'Razor'</span></span>
- <span data-ttu-id="c6dbd-480">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-480">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-481">------ | |*AuthTests* |包含的測試方法：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-481">------ | | *AuthTests* | Contains test methods for:</span></span><ul><li><span data-ttu-id="c6dbd-482">以未驗證的使用者存取安全頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-482">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="c6dbd-483">使用 mock 存取已驗證使用者的安全頁面 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-483">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="c6dbd-484">取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-484">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> <span data-ttu-id="c6dbd-485">| |*BasicTests* |包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-485">| | *BasicTests* | Contains a test method for routing and content type.</span></span> <span data-ttu-id="c6dbd-486">| |*IntegrationTests* |包含使用自訂類別之索引頁面的整合測試 `WebApplicationFactory` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-486">| | *IntegrationTests* | Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> <span data-ttu-id="c6dbd-487">| |Helper */公用程式* | </span><span class="sxs-lookup"><span data-stu-id="c6dbd-487">| | *Helpers/Utilities* | </span></span><ul><li><span data-ttu-id="c6dbd-488">*Utilities.cs*包含 `InitializeDbForTests` 用來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-488">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="c6dbd-489">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument` 供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-489">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="c6dbd-490">*HttpClientExtensions.cs*提供的多載， `SendAsync` 以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-490">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="c6dbd-491">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-491">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="c6dbd-492">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-492">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="c6dbd-493">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器，因此 `TestHost` 和封裝不需要測試應用程式的 `TestServer` 專案檔或測試應用程式中的開發人員設定中的直接套件參考。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-493">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="c6dbd-494">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="c6dbd-494">**Seeding the database for testing**</span></span>

<span data-ttu-id="c6dbd-495">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-495">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="c6dbd-496">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-496">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="c6dbd-497">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-497">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

<span data-ttu-id="c6dbd-498">SUT 的資料庫內容會在其方法中註冊 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-498">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="c6dbd-499">測試應用程式的 `builder.ConfigureServices` 回呼會在*after*應用程式的程式 `Startup.ConfigureServices` 代碼執行之後執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-499">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="c6dbd-500">若要針對測試使用不同的資料庫，則必須在中取代應用程式的資料庫內容 `builder.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-500">To use a different database for the tests, the app's database context must be replaced in `builder.ConfigureServices`.</span></span> <span data-ttu-id="c6dbd-501">如需詳細資訊，請參閱[自訂 WebApplicationFactory](#customize-webapplicationfactory)一節。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-501">For more information, see the [Customize WebApplicationFactory](#customize-webapplicationfactory) section.</span></span>

<span data-ttu-id="c6dbd-502">對於仍然使用 [Web 主機} （x：基礎/主機/Web 主機）的 SUTs，測試應用程式的 `builder.ConfigureServices` 回呼會在 SUT 的程式碼*之前*執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-502">For SUTs that still use the [Web Host}(xref:fundamentals/host/web-host), the test app's `builder.ConfigureServices` callback is executed *before* the SUT's `Startup.ConfigureServices` code.</span></span> <span data-ttu-id="c6dbd-503">測試應用程式的 `builder.ConfigureTestServices` 回呼會*在之後*執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-503">The test app's `builder.ConfigureTestServices` callback is executed *after*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c6dbd-504">整合測試可確保應用程式的元件在包含應用程式支援基礎結構的層級上正確運作，例如資料庫、檔案系統和網路。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-504">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="c6dbd-505">ASP.NET Core 支援使用單元測試架構搭配測試 web 主機和記憶體內部測試伺服器的整合測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-505">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="c6dbd-506">本主題假設對單元測試有基本瞭解。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-506">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="c6dbd-507">如果不熟悉測試概念，請參閱[.Net Core 中的單元測試和 .NET Standard](/dotnet/core/testing/)主題及其連結的內容。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-507">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="c6dbd-508">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-508">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c6dbd-509">範例應用程式是 Razor 頁面應用程式，並假設有對頁面的基本瞭解 Razor 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-509">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="c6dbd-510">如果不熟悉 Razor 頁面，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-510">If unfamiliar with Razor Pages, see the following topics:</span></span>

* <span data-ttu-id="c6dbd-511">[頁面簡介 Razor](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-511">[Introduction to Razor Pages](xref:razor-pages/index)</span></span>
* <span data-ttu-id="c6dbd-512">[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-512">[Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start)</span></span>
* <span data-ttu-id="c6dbd-513">[Razor頁面單元測試](xref:test/razor-pages-tests)</span><span class="sxs-lookup"><span data-stu-id="c6dbd-513">[Razor Pages unit tests](xref:test/razor-pages-tests)</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-514">針對測試 Spa，建議使用[Selenium](https://www.seleniumhq.org/)這類工具，這可將瀏覽器自動化。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-514">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="c6dbd-515">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="c6dbd-515">Introduction to integration tests</span></span>

<span data-ttu-id="c6dbd-516">相較于[單元測試](/dotnet/core/testing/)，整合測試會在更廣泛的層級上評估應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-516">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="c6dbd-517">單元測試是用來測試隔離的軟體元件，例如個別的類別方法。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-517">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="c6dbd-518">整合測試會確認兩個或多個應用程式元件一起運作，以產生預期的結果，可能包括完整處理要求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-518">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="c6dbd-519">這些較廣泛的測試可用來測試應用程式的基礎結構和整個架構，通常包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-519">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="c6dbd-520">資料庫</span><span class="sxs-lookup"><span data-stu-id="c6dbd-520">Database</span></span>
* <span data-ttu-id="c6dbd-521">檔案系統</span><span class="sxs-lookup"><span data-stu-id="c6dbd-521">File system</span></span>
* <span data-ttu-id="c6dbd-522">網路設備</span><span class="sxs-lookup"><span data-stu-id="c6dbd-522">Network appliances</span></span>
* <span data-ttu-id="c6dbd-523">要求-回應管線</span><span class="sxs-lookup"><span data-stu-id="c6dbd-523">Request-response pipeline</span></span>

<span data-ttu-id="c6dbd-524">單元測試會使用製造的元件（稱為*fakes*或模擬*物件*）來取代基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-524">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="c6dbd-525">相對於單元測試，整合測試：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-525">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="c6dbd-526">使用應用程式在生產環境中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-526">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="c6dbd-527">需要更多程式碼和資料處理。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-527">Require more code and data processing.</span></span>
* <span data-ttu-id="c6dbd-528">執行較長的時間。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-528">Take longer to run.</span></span>

<span data-ttu-id="c6dbd-529">因此，請將整合測試的使用限制為最重要的基礎結構案例。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-529">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="c6dbd-530">如果可以使用單元測試或整合測試來測試行為，請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-530">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="c6dbd-531">請勿針對每個可能的資料和檔案存取，使用資料庫與檔案系統來撰寫整合測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-531">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="c6dbd-532">無論應用程式之間的多少個位置與資料庫和檔案系統互動，一組專門的讀取、寫入、更新和刪除整合測試，通常都能夠適當地測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-532">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="c6dbd-533">針對與這些元件互動的方法邏輯，使用單元測試進行常式測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-533">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="c6dbd-534">在單元測試中，使用基礎結構 fakes/模擬會產生更快速的測試執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-534">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-535">在整合測試的討論中，測試過的專案經常稱為受測*系統*，或簡稱「SUT」。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-535">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="c6dbd-536">*本主題中會使用「SUT」來參考已測試的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="c6dbd-536">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="c6dbd-537">ASP.NET Core 整合測試</span><span class="sxs-lookup"><span data-stu-id="c6dbd-537">ASP.NET Core integration tests</span></span>

<span data-ttu-id="c6dbd-538">ASP.NET Core 中的整合測試需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-538">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="c6dbd-539">測試專案會用來包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-539">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="c6dbd-540">測試專案具有該 SUT 的參考。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-540">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="c6dbd-541">測試專案會建立適用于 SUT 的測試 web 主機，並使用測試伺服器用戶端來處理與 SUT 的要求和回應。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-541">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="c6dbd-542">測試執行器是用來執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-542">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="c6dbd-543">整合測試會遵循包含一般*排列*、 *Act*和判斷*提示測試步驟*的一連串事件：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-543">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="c6dbd-544">已設定了 SUT 的 web 主機。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-544">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="c6dbd-545">建立測試伺服器用戶端，以提交要求給應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-545">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="c6dbd-546">執行*排列*測試步驟：測試應用程式會準備要求。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-546">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="c6dbd-547">執行*Act*測試步驟：用戶端提交要求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-547">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="c6dbd-548">執行判斷*提示測試步驟*：*實際*的回應會根據*預期*的回應驗證為*通過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-548">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="c6dbd-549">程式會繼續進行，直到執行所有測試為止。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-549">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="c6dbd-550">測試結果會報告。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-550">The test results are reported.</span></span>

<span data-ttu-id="c6dbd-551">測試 web 主機的設定通常與測試回合的應用程式一般 web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-551">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="c6dbd-552">例如，測試可能會使用不同的資料庫或不同的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-552">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="c6dbd-553">基礎結構元件（例如測試 web 主機和記憶體內部測試伺服器（[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)））是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)所提供或管理。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-553">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="c6dbd-554">使用此封裝會簡化建立和執行的測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-554">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="c6dbd-555">此 `Microsoft.AspNetCore.Mvc.Testing` 套件會處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-555">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="c6dbd-556">將相依性檔案（*. .deps.json*）從 SUT 複製到測試專案的*bin*目錄。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-556">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="c6dbd-557">將[內容根目錄](xref:fundamentals/index#content-root)設定為 SUT 的專案根目錄，以便在執行測試時找到靜態檔案和頁面/瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-557">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="c6dbd-558">提供[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類別，以簡化使用來啟動的 SUT `TestServer` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-558">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="c6dbd-559">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)檔描述如何設定測試專案和測試執行器，以及如何執行測試和建議如何命名測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-559">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-560">建立應用程式的測試專案時，請將單元測試從整合測試分成不同的專案。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-560">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="c6dbd-561">這有助於確保基礎結構測試元件不會不小心包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-561">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="c6dbd-562">區隔單元和整合測試也可以控制要執行哪一組測試。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-562">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="c6dbd-563">Razor頁面應用程式和 MVC 應用程式的測試設定之間幾乎沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-563">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="c6dbd-564">唯一的差異在於測試的命名方式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-564">The only difference is in how the tests are named.</span></span> <span data-ttu-id="c6dbd-565">在 Razor 頁面應用程式中，頁面端點的測試通常是在頁面模型類別之後命名（例如，用 `IndexPageTests` 來測試索引頁面的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-565">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="c6dbd-566">在 MVC 應用程式中，通常會以控制器類別來組織測試，並在其測試的控制器之後命名（例如， `HomeControllerTests` 測試主控制器的元件整合）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-566">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="c6dbd-567">測試應用程式必要條件</span><span class="sxs-lookup"><span data-stu-id="c6dbd-567">Test app prerequisites</span></span>

<span data-ttu-id="c6dbd-568">測試專案必須：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-568">The test project must:</span></span>

* <span data-ttu-id="c6dbd-569">參考下列套件：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-569">Reference the following packages:</span></span>
  * [<span data-ttu-id="c6dbd-570">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="c6dbd-570">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [<span data-ttu-id="c6dbd-571">AspNetCore 測試</span><span class="sxs-lookup"><span data-stu-id="c6dbd-571">Microsoft.AspNetCore.Mvc.Testing</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* <span data-ttu-id="c6dbd-572">在專案檔中指定 Web SDK （ `<Project Sdk="Microsoft.NET.Sdk.Web">` ）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-572">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span> <span data-ttu-id="c6dbd-573">參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)時，需要 Web SDK。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-573">The Web SDK is required when referencing the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c6dbd-574">這些必要條件可在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)中看到。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-574">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="c6dbd-575">檢查 [*測試]/[RazorPagesProject]/* [測試]/[RazorPagesProject]。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-575">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="c6dbd-576">範例應用程式會使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)剖析器程式庫，因此範例應用程式也會參考：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-576">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="c6dbd-577">xunit</span><span class="sxs-lookup"><span data-stu-id="c6dbd-577">xunit</span></span>](https://www.nuget.org/packages/xunit/)
* [<span data-ttu-id="c6dbd-578">xunit。 visualstudio</span><span class="sxs-lookup"><span data-stu-id="c6dbd-578">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [<span data-ttu-id="c6dbd-579">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="c6dbd-579">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a><span data-ttu-id="c6dbd-580">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="c6dbd-580">SUT environment</span></span>

<span data-ttu-id="c6dbd-581">如果未設定了 SUT 的[環境](xref:fundamentals/environments)，則環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-581">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="c6dbd-582">使用預設 WebApplicationFactory 的基本測試</span><span class="sxs-lookup"><span data-stu-id="c6dbd-582">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="c6dbd-583">[WebApplicationFactory \<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)是用來建立整合測試的[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-583">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="c6dbd-584">`TEntryPoint`是 SUT 的進入點類別，通常是 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-584">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="c6dbd-585">測試類別會實作為類別*裝置*介面（[IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)），以指示類別包含測試，並在類別中的所有測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-585">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="c6dbd-586">下列測試類別會 `BasicTests` 使用 `WebApplicationFactory` 來啟動 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)給測試方法 `Get_EndpointsReturnSuccessAndCorrectContentType` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-586">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="c6dbd-587">方法會檢查回應狀態碼是否成功（狀態碼的範圍是200-299），而 `Content-Type` 標頭則 `text/html; charset=utf-8` 適用于數個應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-587">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="c6dbd-588">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) `HttpClient` 會建立自動遵循重新導向和處理 cookie 的實例。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-588">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="c6dbd-589">根據預設，當啟用[GDPR 同意原則](xref:security/gdpr)時，不會在要求之間保留非必要的 cookie。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-589">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="c6dbd-590">若要保留非必要的 cookie （例如 TempData 提供者所使用的 cookie），請在您的測試中將它們標示為必要。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-590">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="c6dbd-591">如需將 cookie 標示為必要的指示，請參閱[基本 cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-591">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="c6dbd-592">自訂 WebApplicationFactory</span><span class="sxs-lookup"><span data-stu-id="c6dbd-592">Customize WebApplicationFactory</span></span>

<span data-ttu-id="c6dbd-593">藉由繼承自 `WebApplicationFactory` 來建立一或多個自訂處理站，可以獨立于測試類別建立 Web 主機設定：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-593">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="c6dbd-594">繼承自 `WebApplicationFactory` ，並覆寫[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-594">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="c6dbd-595">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)來設定服務集合，這會在應用程式的之前執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-595">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices), which is executed before the app's `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6dbd-596">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)覆寫和修改服務集合中的已註冊服務：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-596">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows overriding and modifying registered services in the service collection with [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices):</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="c6dbd-597">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)中的資料庫植入是由 `InitializeDbForTests` 方法執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-597">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="c6dbd-598">此方法會在[整合測試範例：測試應用程式組織](#test-app-organization)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-598">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="c6dbd-599">使用 `CustomWebApplicationFactory` 測試類別中的自訂。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-599">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="c6dbd-600">下列範例會在類別中使用 factory `IndexPageTests` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-600">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="c6dbd-601">範例應用程式的用戶端設定為防止 `HttpClient` 下列重新導向。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-601">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="c6dbd-602">如稍後的模擬[驗證](#mock-authentication)一節中所述，這會允許測試檢查應用程式第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-602">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="c6dbd-603">第一個回應是其中許多測試中具有標頭的重新導向 `Location` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-603">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="c6dbd-604">一般測試會使用 `HttpClient` 和 helper 方法來處理要求和回應：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-604">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="c6dbd-605">任何對 SUT 的 POST 要求都必須滿足應用程式的[資料保護 antiforgery 系統](xref:security/data-protection/introduction)自動進行的 antiforgery 檢查。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-605">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="c6dbd-606">為了安排測試的 POST 要求，測試應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-606">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="c6dbd-607">提出頁面的要求。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-607">Make a request for the page.</span></span>
1. <span data-ttu-id="c6dbd-608">剖析 antiforgery cookie，並從回應要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-608">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="c6dbd-609">提出具有 antiforgery cookie 的 POST 要求，並要求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-609">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="c6dbd-610">`SendAsync`範例應用程式中的協助程式擴充方法（helper */HttpClientExtensions*）和 `GetDocumentAsync` helper 方法（helper */HtmlHelpers*）會使用[sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) [AngleSharp](https://anglesharp.github.io/)剖析器，利用下列方法來處理 antiforgery 檢查：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-610">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="c6dbd-611">`GetDocumentAsync`&ndash;接收[HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) ，並傳回 `IHtmlDocument` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-611">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="c6dbd-612">`GetDocumentAsync`使用可根據原始的來準備*虛擬回應*的 factory `HttpResponseMessage` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-612">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="c6dbd-613">如需詳細資訊，請參閱[AngleSharp 檔](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-613">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="c6dbd-614">`SendAsync``HttpClient`撰寫[HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)和呼叫[SendAsync （HttpRequestMessage）](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法，以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-614">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="c6dbd-615">的多載會 `SendAsync` 接受 HTML 表單（ `IHtmlFormElement` ）和下列內容：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-615">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="c6dbd-616">表單的 [提交] 按鈕（ `IHtmlElement` ）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-616">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="c6dbd-617">表單值集合（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-617">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="c6dbd-618">提交按鈕（ `IHtmlElement` ）和表單值（ `IEnumerable<KeyValuePair<string, string>>` ）</span><span class="sxs-lookup"><span data-stu-id="c6dbd-618">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="c6dbd-619">[AngleSharp](https://anglesharp.github.io/)是協力廠商剖析程式庫，用於本主題和範例應用程式中的示範用途。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-619">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="c6dbd-620">ASP.NET Core 應用程式的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-620">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="c6dbd-621">您可以使用其他剖析器，例如[Html 靈活性套件（HAP）](https://html-agility-pack.net/)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-621">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="c6dbd-622">另一種方法是撰寫程式碼，直接處理 antiforgery 系統的要求驗證權杖和 antiforgery cookie。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-622">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="c6dbd-623">使用 WithWebHostBuilder 自訂用戶端</span><span class="sxs-lookup"><span data-stu-id="c6dbd-623">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="c6dbd-624">當測試方法中需要額外的設定時， [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) `WebApplicationFactory` 會使用設定進一步自訂的[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ，建立新的。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-624">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="c6dbd-625">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)的測試方法會示範的用法 `WithWebHostBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-625">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="c6dbd-626">這項測試會藉由觸發在 SUT 中提交表單的方式，在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-626">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="c6dbd-627">因為類別中的另一項測試 `IndexPageTests` 會執行刪除資料庫中所有記錄的作業，而且可能會在此方法之前執行，所以會 `Post_DeleteMessageHandler_ReturnsRedirectToRoot` 在此測試方法中重新植入資料庫，以確保有記錄存在，供 SUT 刪除。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-627">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="c6dbd-628">在 sut 的要求中，選取該表單的第一個 [刪除] 按鈕 `messages` 會進行模擬：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-628">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="c6dbd-629">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="c6dbd-629">Client options</span></span>

<span data-ttu-id="c6dbd-630">下表顯示建立實例時[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)可用的預設 WebApplicationFactoryClientOptions `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-630">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="c6dbd-631">選項</span><span class="sxs-lookup"><span data-stu-id="c6dbd-631">Option</span></span> | <span data-ttu-id="c6dbd-632">描述</span><span class="sxs-lookup"><span data-stu-id="c6dbd-632">Description</span></span> | <span data-ttu-id="c6dbd-633">預設</span><span class="sxs-lookup"><span data-stu-id="c6dbd-633">Default</span></span> |
| ---
<span data-ttu-id="c6dbd-634">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-634">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-635">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-635">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-636">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-636">'Identity'</span></span>
- <span data-ttu-id="c6dbd-637">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-637">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-638">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-638">'Razor'</span></span>
- <span data-ttu-id="c6dbd-639">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-639">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-640">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-640">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-641">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-641">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-642">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-642">'Identity'</span></span>
- <span data-ttu-id="c6dbd-643">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-643">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-644">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-644">'Razor'</span></span>
- <span data-ttu-id="c6dbd-645">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-645">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-646">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-646">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-647">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-647">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-648">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-648">'Identity'</span></span>
- <span data-ttu-id="c6dbd-649">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-649">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-650">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-650">'Razor'</span></span>
- <span data-ttu-id="c6dbd-651">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-651">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-652">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-652">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-653">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-653">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-654">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-654">'Identity'</span></span>
- <span data-ttu-id="c6dbd-655">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-655">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-656">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-656">'Razor'</span></span>
- <span data-ttu-id="c6dbd-657">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-657">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-658">------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-658">------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-659">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-659">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-660">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-660">'Identity'</span></span>
- <span data-ttu-id="c6dbd-661">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-661">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-662">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-662">'Razor'</span></span>
- <span data-ttu-id="c6dbd-663">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-663">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-664">---- | |[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) |取得或設定 `HttpClient` 實例是否應該自動遵循重新導向回應。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-664">---- | | [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span><span data-ttu-id="c6dbd-665"> | `true`| |[BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) |取得或設定實例的基底位址 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-665"> | `true` | | [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Gets or sets the base address of `HttpClient` instances.</span></span><span data-ttu-id="c6dbd-666"> | `http://localhost`| |[HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) |取得或設定 `HttpClient` 實例是否應處理 cookie。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-666"> | `http://localhost` | | [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Gets or sets whether `HttpClient` instances should handle cookies.</span></span><span data-ttu-id="c6dbd-667"> | `true`| |[MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) |取得或設定實例應遵循的重新導向回應數目上限 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-667"> | `true` | | [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> <span data-ttu-id="c6dbd-668">|7 |</span><span class="sxs-lookup"><span data-stu-id="c6dbd-668">| 7 |</span></span>

<span data-ttu-id="c6dbd-669">建立 `WebApplicationFactoryClientOptions` 類別，並將它傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法（程式碼範例中會顯示預設值）：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-669">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="c6dbd-670">插入 mock 服務</span><span class="sxs-lookup"><span data-stu-id="c6dbd-670">Inject mock services</span></span>

<span data-ttu-id="c6dbd-671">您可以在主機建立器上呼叫[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) ，以在測試中覆寫服務。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-671">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="c6dbd-672">**若要插入模擬服務，SUT 必須具有 `Startup` 具有方法的類別 `Startup.ConfigureServices` 。**</span><span class="sxs-lookup"><span data-stu-id="c6dbd-672">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="c6dbd-673">範例 SUT 包含會傳回報價的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-673">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="c6dbd-674">當要求索引頁時，引號會內嵌在 [索引] 頁面上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-674">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="c6dbd-675">*Services/IQuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-675">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="c6dbd-676">*Services/QuoteService .cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-676">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="c6dbd-677">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-677">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="c6dbd-678">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-678">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="c6dbd-679">*Pages/Index .cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-679">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="c6dbd-680">執行 SUT 應用程式時，會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-680">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="c6dbd-681">若要測試服務並在整合測試中加上引號，則測試會將模擬服務插入至 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-681">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="c6dbd-682">Mock 服務會將應用程式的取代為 `QuoteService` 測試應用程式所提供的服務，稱為 `TestQuoteService` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-682">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="c6dbd-683">*IntegrationTests.IndexPageTests.cs*：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-683">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="c6dbd-684">`ConfigureTestServices`會呼叫，並註冊已設定範圍的服務：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-684">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="c6dbd-685">在測試執行期間產生的標記會反映所提供的引號文字 `TestQuoteService` ，因此判斷提示會通過：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-685">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="c6dbd-686">Mock 驗證</span><span class="sxs-lookup"><span data-stu-id="c6dbd-686">Mock authentication</span></span>

<span data-ttu-id="c6dbd-687">類別中的測試 `AuthTests` 會檢查安全的端點：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-687">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="c6dbd-688">將未驗證的使用者重新導向至應用程式的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-688">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="c6dbd-689">傳回已驗證使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-689">Returns content for an authenticated user.</span></span>

<span data-ttu-id="c6dbd-690">在 SUT 中， `/SecurePage` 頁面會使用[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)慣例將[AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-690">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="c6dbd-691">如需詳細資訊，請參閱[ Razor 頁面授權慣例](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-691">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="c6dbd-692">在 `Get_SecurePageRedirectsAnUnauthenticatedUser` 測試中， [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)會設定為不允許重新導向，方法是將[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)設為 `false` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-692">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="c6dbd-693">藉由不允許用戶端遵循重新導向，您可以進行下列檢查：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-693">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="c6dbd-694">您可以根據預期的 HttpStatusCode 檢查由 SUT 所傳回的狀態碼。重新導向至登入頁面（可能是[HttpStatusCode](/dotnet/api/system.net.httpstatuscode)）之後，重新[導向](/dotnet/api/system.net.httpstatuscode)結果，而不是最終狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-694">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="c6dbd-695">`Location`系統會檢查回應標頭中的標頭值，以確認其開頭為 `http://localhost/Identity/Account/Login` ，而不是最終的登入頁面回應，其中 `Location` 標頭不存在。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-695">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="c6dbd-696">測試應用程式可以 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 在[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中模擬，以便測試驗證和授權的層面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-696">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="c6dbd-697">最小案例會傳回[AuthenticateResult。成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*)：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-697">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="c6dbd-698">`TestAuthHandler`當驗證配置設定為 `Test` （其中已註冊的）時，會呼叫來驗證使用者 `AddAuthentication` `ConfigureTestServices` ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-698">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="c6dbd-699">如需的詳細資訊 `WebApplicationFactoryClientOptions` ，請參閱[用戶端選項](#client-options)一節。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-699">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="c6dbd-700">設定環境</span><span class="sxs-lookup"><span data-stu-id="c6dbd-700">Set the environment</span></span>

<span data-ttu-id="c6dbd-701">根據預設，會將 SUT 的主機和應用程式環境設定為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-701">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="c6dbd-702">若要覆寫 SUT 的環境：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-702">To override the SUT's environment:</span></span>

* <span data-ttu-id="c6dbd-703">設定 `ASPNETCORE_ENVIRONMENT` 環境變數（例如，、 `Staging` `Production` 或其他自訂值，例如 `Testing` ）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-703">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="c6dbd-704">`CreateWebHostBuilder`在測試應用程式中覆寫以讀取 `ASPNETCORE_ENVIRONMENT` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-704">Override `CreateWebHostBuilder` in the test app to read the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span>

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

<span data-ttu-id="c6dbd-705">環境也可以直接在自訂的主機產生器上設定 <xref:Microsoft.AspNetCore.Mvc.Testing.WebApplicationFactory%601> ：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-705">The environment can also be set directly on the host builder in a custom <xref:Microsoft.AspNetCore.Mvc.Testing.WebApplicationFactory%601>:</span></span>

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

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="c6dbd-706">測試基礎結構如何推斷應用程式內容根路徑</span><span class="sxs-lookup"><span data-stu-id="c6dbd-706">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="c6dbd-707">此函式會藉 `WebApplicationFactory` 由搜尋元件上的[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用程式[內容根](xref:fundamentals/index#content-root)路徑，其中包含的索引鍵等於元件的整合測試 `TEntryPoint` `System.Reflection.Assembly.FullName` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-707">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="c6dbd-708">如果找不到具有正確索引鍵的屬性， `WebApplicationFactory` 就會回到搜尋方案檔（*.sln*），並將 `TEntryPoint` 元件名稱附加至方案目錄。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-708">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="c6dbd-709">應用程式根目錄（內容根路徑）是用來探索 views 和內容檔案。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-709">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="c6dbd-710">停用陰影複製</span><span class="sxs-lookup"><span data-stu-id="c6dbd-710">Disable shadow copying</span></span>

<span data-ttu-id="c6dbd-711">陰影複製會使測試在與輸出目錄不同的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-711">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="c6dbd-712">若要讓測試正常運作，必須停用陰影複製。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-712">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="c6dbd-713">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)會使用 xUnit，並藉由包含具有正確設定的*xUnit*檔案，來停用 xUnit 的陰影複製。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-713">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="c6dbd-714">如需詳細資訊，請參閱[使用 JSON 設定 xUnit](https://xunit.github.io/docs/configuring-with-json.html)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-714">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="c6dbd-715">將*xunit*檔案新增至測試專案的根目錄，並包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-715">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

<span data-ttu-id="c6dbd-716">如果使用 Visual Studio，請將檔案的 [**複製到輸出目錄**] 屬性設定為 [**永遠複製**]。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-716">If using Visual Studio, set the file's **Copy to Output Directory** property to **Copy always**.</span></span> <span data-ttu-id="c6dbd-717">如果未使用 Visual Studio，請將 `Content` 目標新增至測試應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-717">If not using Visual Studio, add a `Content` target to the test app's project file:</span></span>

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a><span data-ttu-id="c6dbd-718">物件的處置</span><span class="sxs-lookup"><span data-stu-id="c6dbd-718">Disposal of objects</span></span>

<span data-ttu-id="c6dbd-719">執行此實作為測試之後 `IClassFixture` ，當 xUnit 處置[WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時，會處置[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HttpClient](/dotnet/api/system.net.http.httpclient) 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-719">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="c6dbd-720">如果開發人員所具現化的物件需要處置，請在執行時處置它們 `IClassFixture` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-720">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="c6dbd-721">如需詳細資訊，請參閱[執行 Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-721">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="c6dbd-722">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="c6dbd-722">Integration tests sample</span></span>

<span data-ttu-id="c6dbd-723">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)是由兩個應用程式所組成：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-723">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="c6dbd-724">App</span><span class="sxs-lookup"><span data-stu-id="c6dbd-724">App</span></span> | <span data-ttu-id="c6dbd-725">專案目錄</span><span class="sxs-lookup"><span data-stu-id="c6dbd-725">Project directory</span></span> | <span data-ttu-id="c6dbd-726">說明</span><span class="sxs-lookup"><span data-stu-id="c6dbd-726">Description</span></span> |
| --- | ---
<span data-ttu-id="c6dbd-727">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-727">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-728">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-728">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-729">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-729">'Identity'</span></span>
- <span data-ttu-id="c6dbd-730">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-730">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-731">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-731">'Razor'</span></span>
- <span data-ttu-id="c6dbd-732">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-732">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-733">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-733">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-734">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-734">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-735">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-735">'Identity'</span></span>
- <span data-ttu-id="c6dbd-736">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-736">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-737">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-737">'Razor'</span></span>
- <span data-ttu-id="c6dbd-738">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-738">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-739">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-739">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-740">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-740">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-741">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-741">'Identity'</span></span>
- <span data-ttu-id="c6dbd-742">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-742">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-743">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-743">'Razor'</span></span>
- <span data-ttu-id="c6dbd-744">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-744">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-745">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-745">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-746">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-746">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-747">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-747">'Identity'</span></span>
- <span data-ttu-id="c6dbd-748">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-748">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-749">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-749">'Razor'</span></span>
- <span data-ttu-id="c6dbd-750">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-750">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-751">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-751">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-752">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-752">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-753">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-753">'Identity'</span></span>
- <span data-ttu-id="c6dbd-754">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-754">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-755">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-755">'Razor'</span></span>
- <span data-ttu-id="c6dbd-756">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-756">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-757">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-757">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-758">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-758">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-759">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-759">'Identity'</span></span>
- <span data-ttu-id="c6dbd-760">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-760">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-761">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-761">'Razor'</span></span>
- <span data-ttu-id="c6dbd-762">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-762">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-763">--------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-763">--------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-764">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-764">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-765">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-765">'Identity'</span></span>
- <span data-ttu-id="c6dbd-766">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-766">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-767">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-767">'Razor'</span></span>
- <span data-ttu-id="c6dbd-768">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-768">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-769">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-769">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-770">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-770">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-771">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-771">'Identity'</span></span>
- <span data-ttu-id="c6dbd-772">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-772">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-773">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-773">'Razor'</span></span>
- <span data-ttu-id="c6dbd-774">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-774">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-775">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-775">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-776">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-776">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-777">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-777">'Identity'</span></span>
- <span data-ttu-id="c6dbd-778">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-778">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-779">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-779">'Razor'</span></span>
- <span data-ttu-id="c6dbd-780">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-780">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-781">------ | |訊息應用程式（SUT） |*src/RazorPagesProject* |可讓使用者加入、刪除一個、刪除全部及分析訊息。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-781">------ | | Message app (the SUT) | *src/RazorPagesProject* | Allows a user to add, delete one, delete all, and analyze messages.</span></span> <span data-ttu-id="c6dbd-782">| |測試應用程式 |*測試/RazorPagesProject。測試*|用來整合測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-782">| | Test app | *tests/RazorPagesProject.Tests* | Used to integration test the SUT.</span></span> |

<span data-ttu-id="c6dbd-783">測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](https://visualstudio.microsoft.com)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-783">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="c6dbd-784">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [測試]/[RazorPagesProject] 的命令提示字元中執行下列命令 *。測試*目錄：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-784">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="c6dbd-785">訊息應用程式（SUT）組織</span><span class="sxs-lookup"><span data-stu-id="c6dbd-785">Message app (SUT) organization</span></span>

<span data-ttu-id="c6dbd-786">SUT 是 Razor 具有下列特性的頁面訊息系統：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-786">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="c6dbd-787">應用程式的 [索引] 頁面（*pages/index. cshtml*和*pages/Index. CSHTML*）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（每個訊息的平均單字）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-787">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="c6dbd-788">訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （key）和 `Text` （message）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-788">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="c6dbd-789">`Text`屬性是必要的，而且限制為200個字元。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-789">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="c6dbd-790">訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-790">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="c6dbd-791">應用程式在其資料庫內容類別（ `AppDbContext` *Data/AppDbCoNtext .cs*）中包含資料存取層（DAL）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-791">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="c6dbd-792">如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-792">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="c6dbd-793">應用程式包含 `/SecurePage` 只能由已驗證的使用者存取的。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-793">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="c6dbd-794">&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-794">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="c6dbd-795">本主題使用[xUnit](https://xunit.github.io/)測試架構。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-795">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="c6dbd-796">跨不同測試架構的測試概念和測試執行類似，但不完全相同。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-796">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="c6dbd-797">雖然應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，但 Razor 頁面支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-797">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="c6dbd-798">如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)（範例會執行存放庫模式）。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-798">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="c6dbd-799">測試應用程式組織</span><span class="sxs-lookup"><span data-stu-id="c6dbd-799">Test app organization</span></span>

<span data-ttu-id="c6dbd-800">測試應用程式是 [*測試/RazorPagesProject* ] 目錄中的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-800">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="c6dbd-801">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="c6dbd-801">Test app directory</span></span> | <span data-ttu-id="c6dbd-802">說明</span><span class="sxs-lookup"><span data-stu-id="c6dbd-802">Description</span></span> |
| ---
<span data-ttu-id="c6dbd-803">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-803">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-804">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-804">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-805">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-805">'Identity'</span></span>
- <span data-ttu-id="c6dbd-806">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-806">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-807">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-807">'Razor'</span></span>
- <span data-ttu-id="c6dbd-808">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-808">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-809">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-809">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-810">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-810">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-811">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-811">'Identity'</span></span>
- <span data-ttu-id="c6dbd-812">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-812">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-813">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-813">'Razor'</span></span>
- <span data-ttu-id="c6dbd-814">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-814">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-815">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-815">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-816">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-816">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-817">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-817">'Identity'</span></span>
- <span data-ttu-id="c6dbd-818">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-818">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-819">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-819">'Razor'</span></span>
- <span data-ttu-id="c6dbd-820">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-820">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-821">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-821">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-822">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-822">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-823">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-823">'Identity'</span></span>
- <span data-ttu-id="c6dbd-824">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-824">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-825">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-825">'Razor'</span></span>
- <span data-ttu-id="c6dbd-826">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-826">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-827">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-827">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-828">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-828">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-829">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-829">'Identity'</span></span>
- <span data-ttu-id="c6dbd-830">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-830">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-831">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-831">'Razor'</span></span>
- <span data-ttu-id="c6dbd-832">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-832">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-833">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-833">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-834">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-834">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-835">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-835">'Identity'</span></span>
- <span data-ttu-id="c6dbd-836">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-836">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-837">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-837">'Razor'</span></span>
- <span data-ttu-id="c6dbd-838">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-838">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-839">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-839">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-840">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-840">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-841">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-841">'Identity'</span></span>
- <span data-ttu-id="c6dbd-842">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-842">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-843">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-843">'Razor'</span></span>
- <span data-ttu-id="c6dbd-844">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-844">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-845">--------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-845">--------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-846">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-846">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-847">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-847">'Identity'</span></span>
- <span data-ttu-id="c6dbd-848">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-848">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-849">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-849">'Razor'</span></span>
- <span data-ttu-id="c6dbd-850">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-850">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-851">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-851">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-852">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-852">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-853">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-853">'Identity'</span></span>
- <span data-ttu-id="c6dbd-854">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-854">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-855">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-855">'Razor'</span></span>
- <span data-ttu-id="c6dbd-856">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-856">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c6dbd-857">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-857">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c6dbd-858">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-858">'Blazor'</span></span>
- <span data-ttu-id="c6dbd-859">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-859">'Identity'</span></span>
- <span data-ttu-id="c6dbd-860">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-860">'Let's Encrypt'</span></span>
- <span data-ttu-id="c6dbd-861">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c6dbd-861">'Razor'</span></span>
- <span data-ttu-id="c6dbd-862">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-862">'SignalR' uid:</span></span> 

<span data-ttu-id="c6dbd-863">------ | |*AuthTests* |包含的測試方法：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-863">------ | | *AuthTests* | Contains test methods for:</span></span><ul><li><span data-ttu-id="c6dbd-864">以未驗證的使用者存取安全頁面。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-864">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="c6dbd-865">使用 mock 存取已驗證使用者的安全頁面 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-865">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="c6dbd-866">取得 GitHub 使用者設定檔，並檢查設定檔的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-866">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> <span data-ttu-id="c6dbd-867">| |*BasicTests* |包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-867">| | *BasicTests* | Contains a test method for routing and content type.</span></span> <span data-ttu-id="c6dbd-868">| |*IntegrationTests* |包含使用自訂類別之索引頁面的整合測試 `WebApplicationFactory` 。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-868">| | *IntegrationTests* | Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> <span data-ttu-id="c6dbd-869">| |Helper */公用程式* | </span><span class="sxs-lookup"><span data-stu-id="c6dbd-869">| | *Helpers/Utilities* | </span></span><ul><li><span data-ttu-id="c6dbd-870">*Utilities.cs*包含 `InitializeDbForTests` 用來植入具有測試資料之資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-870">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="c6dbd-871">*HtmlHelpers.cs*提供方法來傳回 AngleSharp `IHtmlDocument` 供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-871">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="c6dbd-872">*HttpClientExtensions.cs*提供的多載， `SendAsync` 以將要求提交至 SUT。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-872">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="c6dbd-873">測試架構為[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-873">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="c6dbd-874">整合測試會使用[AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行，其中包含[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-874">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="c6dbd-875">由於[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)會使用測試封裝來設定測試主機和測試伺服器，因此 `TestHost` 和封裝不需要測試應用程式的 `TestServer` 專案檔或測試應用程式中的開發人員設定中的直接套件參考。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-875">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="c6dbd-876">**植入資料庫進行測試**</span><span class="sxs-lookup"><span data-stu-id="c6dbd-876">**Seeding the database for testing**</span></span>

<span data-ttu-id="c6dbd-877">在測試執行之前，整合測試通常需要資料庫中的小型資料集。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-877">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="c6dbd-878">例如，刪除測試會呼叫資料庫記錄，因此資料庫必須至少有一筆記錄，刪除要求才會成功。</span><span class="sxs-lookup"><span data-stu-id="c6dbd-878">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="c6dbd-879">範例應用程式會在*Utilities.cs*中植入具有三個訊息的資料庫，測試可以在執行時使用：</span><span class="sxs-lookup"><span data-stu-id="c6dbd-879">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c6dbd-880">其他資源</span><span class="sxs-lookup"><span data-stu-id="c6dbd-880">Additional resources</span></span>

* [<span data-ttu-id="c6dbd-881">單元測試</span><span class="sxs-lookup"><span data-stu-id="c6dbd-881">Unit tests</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:test/razor-pages-tests>
* <xref:fundamentals/middleware/index>
* <xref:mvc/controllers/testing>
