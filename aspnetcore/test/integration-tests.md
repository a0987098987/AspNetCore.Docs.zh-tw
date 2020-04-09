---
title: ASP.NET核心中的整合測試
author: rick-anderson
description: 了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
uid: test/integration-tests
ms.openlocfilehash: 414e47b9c5a1c843755bd79d313f503b554945db
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664693"
---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="a9ea9-103">ASP.NET核心中的整合測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-103">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="a9ea9-104">由[哈威爾·卡爾瓦羅·納爾遜](https://github.com/javiercn)、[史蒂夫·史密斯](https://ardalis.com/)和[喬斯·范德蒂爾](https://jvandertil.nl)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Steve Smith](https://ardalis.com/), and [Jos van der Til](https://jvandertil.nl)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a9ea9-105">集成測試可確保應用元件在包含應用支援基礎結構(如資料庫、檔案系統和網路)的級別上正常運行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-105">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="a9ea9-106">ASP.NET Core 支援使用具有測試 Web 主機和記憶體中測試伺服器的單元測試框架進行集成測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-106">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="a9ea9-107">本主題假定對單元測試有基本的瞭解。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-107">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="a9ea9-108">如果不熟悉測試概念,請參閱[.NET 核心和 .NET 標準主題中的單元測試](/dotnet/core/testing/)及其鏈接內容。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-108">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="a9ea9-109">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9ea9-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a9ea9-110">示例應用是 Razor 頁面應用,並假定對 Razor 頁面有基本的瞭解。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-110">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="a9ea9-111">如果不熟悉 Razor 頁面,請參閱以下主題:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-111">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="a9ea9-112">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="a9ea9-112">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="a9ea9-113">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a9ea9-113">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="a9ea9-114">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-114">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="a9ea9-115">對於測試SpA,我們建議一個工具,如[硒](https://www.seleniumhq.org/),它可以自動化瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-115">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="a9ea9-116">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="a9ea9-116">Introduction to integration tests</span></span>

<span data-ttu-id="a9ea9-117">集成測試比[單元測試](/dotnet/core/testing/)在更廣泛的層面上評估應用的元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-117">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="a9ea9-118">單元測試用於測試隔離的軟體元件,如單個類方法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-118">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="a9ea9-119">集成測試確認兩個或多個應用元件協同工作以產生預期結果,可能包括完全處理請求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-119">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="a9ea9-120">這些更廣泛的測試用於測試應用程式的基礎結構和整個框架,通常包括以下元件:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-120">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="a9ea9-121">資料庫</span><span class="sxs-lookup"><span data-stu-id="a9ea9-121">Database</span></span>
* <span data-ttu-id="a9ea9-122">檔案系統</span><span class="sxs-lookup"><span data-stu-id="a9ea9-122">File system</span></span>
* <span data-ttu-id="a9ea9-123">網路設備</span><span class="sxs-lookup"><span data-stu-id="a9ea9-123">Network appliances</span></span>
* <span data-ttu-id="a9ea9-124">請求-回應導管</span><span class="sxs-lookup"><span data-stu-id="a9ea9-124">Request-response pipeline</span></span>

<span data-ttu-id="a9ea9-125">單元測試使用製造元件(稱為*假*體或*類比物件*)代替基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-125">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="a9ea9-126">與單元測試相比,集成測試:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-126">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="a9ea9-127">使用應用在生產中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-127">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="a9ea9-128">需要更多的代碼和數據處理。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-128">Require more code and data processing.</span></span>
* <span data-ttu-id="a9ea9-129">需要更長的時間才能運行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-129">Take longer to run.</span></span>

<span data-ttu-id="a9ea9-130">因此,將集成測試的使用限制在最重要的基礎結構方案中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-130">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="a9ea9-131">如果可以使用單元測試或集成測試測試行為,請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-131">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="a9ea9-132">不要為資料庫和檔案系統中數據和文件存取的每一種可能排列編寫集成測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-132">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="a9ea9-133">無論應用中有多少位置與資料庫和檔案系統交互,集中讀取、寫入、更新和刪除集成測試集通常能夠充分測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-133">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="a9ea9-134">使用單元測試對與這些元件交互的方法邏輯進行例行測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-134">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="a9ea9-135">在單元測試中,使用基礎結構假/類比會導致更快的測試執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-135">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="a9ea9-136">在討論集成測試時,測試專案通常稱為 *「正在測試的系統*」,簡稱為「SUT」。。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-136">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="a9ea9-137">*本主題中使用"SUT"來參考經過測試的 ASP.NET酷睿應用。*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-137">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="a9ea9-138">ASP.NET核心整合測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-138">ASP.NET Core integration tests</span></span>

<span data-ttu-id="a9ea9-139">ASP.NET核心中的整合測試需要以下要求:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-139">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="a9ea9-140">測試專案用於包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-140">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="a9ea9-141">測試專案具有對 SUT 的引用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-141">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="a9ea9-142">測試專案為 SUT 創建測試 Web 主機,並使用測試伺服器用戶端使用 SUT 處理請求和回應。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-142">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="a9ea9-143">測試運行程式用於執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-143">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="a9ea9-144">整合測試遵循一系列事件,其中包括通常的*排列*、*操作*與*斷言*測試步驟:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-144">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="a9ea9-145">SUT 的 Web 主機已配置。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-145">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="a9ea9-146">將創建測試伺服器用戶端以向應用提交請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-146">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="a9ea9-147">執行 *「排列*」測試步驟:測試應用準備請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-147">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="a9ea9-148">*執行 Act*測試步驟:用戶端提交請求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-148">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="a9ea9-149">*執行 Assert*測試步驟:*實際*回應根據*預期*回應驗證為*透過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-149">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="a9ea9-150">該過程將繼續,直到執行所有測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-150">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="a9ea9-151">報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-151">The test results are reported.</span></span>

<span data-ttu-id="a9ea9-152">通常,測試 Web 主機的配置方式與測試運行應用的正常 Web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-152">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="a9ea9-153">例如,測試可能使用不同的資料庫或不同的應用設置。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-153">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="a9ea9-154">基礎結構元件(如測試 Web 主機和記憶體中測試[伺服器 (TestServer)](/dotnet/api/microsoft.aspnetcore.testhost.testserver)由[Microsoft 提供](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)或管理。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-154">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="a9ea9-155">使用此包可以簡化測試創建和執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-155">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="a9ea9-156">該`Microsoft.AspNetCore.Mvc.Testing`套件處理以下工作:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-156">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="a9ea9-157">將依賴項檔 *(.deps*) 從 SUT 複製到測試專案的*bin*目錄中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-157">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="a9ea9-158">將[內容根目錄](xref:fundamentals/index#content-root)設置到 SUT 的專案根目錄,以便在執行測試時找到靜態檔和頁面/檢視。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-158">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="a9ea9-159">提供[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類以`TestServer`簡化使用的 SUT 引導。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-159">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="a9ea9-160">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件介紹如何設置測試專案和測試運行程式,以及有關如何運行測試的詳細說明以及如何命名測試和測試類的建議。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-160">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="a9ea9-161">為應用創建測試專案時,將單元測試與集成測試分離到不同的專案中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-161">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="a9ea9-162">這有助於確保基礎結構測試元件不會意外包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-162">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="a9ea9-163">單元和集成測試的分離還允許控制運行哪組測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-163">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="a9ea9-164">Razor Pages 應用和 MVC 應用測試的配置之間幾乎沒有區別。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-164">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="a9ea9-165">唯一的區別是如何命名測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-165">The only difference is in how the tests are named.</span></span> <span data-ttu-id="a9ea9-166">在 Razor Pages 應用中,頁面終結點的測試通常以頁面模型類命名`IndexPageTests`(例如, 測試索引頁的元件整合)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-166">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="a9ea9-167">在 MVC 應用中,測試通常由控制器類組織,並基於它們測試的控制器命名(`HomeControllerTests`例如, 測試主控制器的元件整合)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-167">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="a9ea9-168">測試應用先決條件</span><span class="sxs-lookup"><span data-stu-id="a9ea9-168">Test app prerequisites</span></span>

<span data-ttu-id="a9ea9-169">測試項目必須:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-169">The test project must:</span></span>

* <span data-ttu-id="a9ea9-170">參考[微軟.AspNetCore.Mvc.測試](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)包。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-170">Reference the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span>
* <span data-ttu-id="a9ea9-171">在專案檔中指定 Web SDK`<Project Sdk="Microsoft.NET.Sdk.Web">`( 。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-171">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span>

<span data-ttu-id="a9ea9-172">這些先決條件可以在[示例應用中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)看到。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-172">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="a9ea9-173">檢查*測試/RazorPages專案.測試/RazorPages專案.測試.csproj*檔。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-173">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="a9ea9-174">範例應用程式使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)解析器庫,因此範例應用還引用:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-174">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="a9ea9-175">森特</span><span class="sxs-lookup"><span data-stu-id="a9ea9-175">xunit</span></span>](https://www.nuget.org/packages/xunit)
* [<span data-ttu-id="a9ea9-176">xunit.runner.Visualstudio</span><span class="sxs-lookup"><span data-stu-id="a9ea9-176">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [<span data-ttu-id="a9ea9-177">角度夏普</span><span class="sxs-lookup"><span data-stu-id="a9ea9-177">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp)

<span data-ttu-id="a9ea9-178">實體框架核心也用於測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-178">Entity Framework Core is also used in the tests.</span></span> <span data-ttu-id="a9ea9-179">套用:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-179">The app references:</span></span>

* [<span data-ttu-id="a9ea9-180">微軟.AspNetCore.診斷.實體框架核心</span><span class="sxs-lookup"><span data-stu-id="a9ea9-180">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [<span data-ttu-id="a9ea9-181">微軟.AspNetCore.身份.實體框架核心</span><span class="sxs-lookup"><span data-stu-id="a9ea9-181">Microsoft.AspNetCore.Identity.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [<span data-ttu-id="a9ea9-182">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="a9ea9-182">Microsoft.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [<span data-ttu-id="a9ea9-183">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="a9ea9-183">Microsoft.EntityFrameworkCore.InMemory</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [<span data-ttu-id="a9ea9-184">微軟.實體框架核心.工具</span><span class="sxs-lookup"><span data-stu-id="a9ea9-184">Microsoft.EntityFrameworkCore.Tools</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a><span data-ttu-id="a9ea9-185">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="a9ea9-185">SUT environment</span></span>

<span data-ttu-id="a9ea9-186">如果未設置 SUT[的環境](xref:fundamentals/environments),則環境預設為"開發」。。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-186">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="a9ea9-187">使用預設 Web 應用程式工廠的基本測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-187">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="a9ea9-188">[Web\<應用程式 工廠 TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用於為整合測試建立[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-188">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="a9ea9-189">`TEntryPoint`是 SUT 的入口點類別,通常`Startup`是類 。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-189">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="a9ea9-190">測試類實現*類韌體*介面[(IClassFixture)](https://xunit.github.io/docs/shared-context#class-fixture)以指示類包含測試並在類中的測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-190">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="a9ea9-191">以下`BasicTests`測試類`WebApplicationFactory`使用引導 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)`Get_EndpointsReturnSuccessAndCorrectContentType`到測試方法 的 HTTPClient。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-191">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="a9ea9-192">該方法檢查回應狀態代碼是否成功(範圍為 200-299)的狀態代碼,`Content-Type`並且標頭`text/html; charset=utf-8`是 多個應用頁。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-192">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="a9ea9-193">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)創建一`HttpClient`個 實例,該實例會自動遵循重定向並處理 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-193">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="a9ea9-194">默認情況下,啟用[GDPR同意策略](xref:security/gdpr)時,不會跨請求保留非必需Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-194">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="a9ea9-195">要保留非必需的 Cookie(如 TempData 提供者使用的 Cookie),請將其標記為測試中必不可少的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-195">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="a9ea9-196">有關將 Cookie 標記為基本 Cookie 的說明,請參閱[基本 Cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-196">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="a9ea9-197">自訂 Web 應用程式工廠</span><span class="sxs-lookup"><span data-stu-id="a9ea9-197">Customize WebApplicationFactory</span></span>

<span data-ttu-id="a9ea9-198">可以通過繼承`WebApplicationFactory`來 創建一個或多個自定義工廠,從而獨立於測試類創建 Web 主機配置:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-198">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="a9ea9-199">繼承`WebApplicationFactory`並覆蓋[設定 WebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-199">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="a9ea9-200">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[設定服務](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)設定服務集合 :</span><span class="sxs-lookup"><span data-stu-id="a9ea9-200">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="a9ea9-201">[範例應用中的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)資料庫種子設定由`InitializeDbForTests`方法執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-201">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="a9ea9-202">該方法在[集成測試示例:測試應用組織](#test-app-organization)部分中進行了描述。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-202">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

   <span data-ttu-id="a9ea9-203">SUT 的資料庫上下文`Startup.ConfigureServices`在其 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-203">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a9ea9-204">測試應用的`builder.ConfigureServices`回調在執行應用`Startup.ConfigureServices`代碼*後*執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-204">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="a9ea9-205">執行順序是[通用主機](xref:fundamentals/host/generic-host)的一個重大更改,ASP.NET核心 3.0 的發佈。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-205">The execution order is a breaking change for the [Generic Host](xref:fundamentals/host/generic-host) with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="a9ea9-206">要對測試使用不同的資料庫,必須在中`builder.ConfigureServices`替換應用的資料庫上下文。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-206">To use a different database for the tests than the app's database, the app's database context must be replaced in `builder.ConfigureServices`.</span></span>

   <span data-ttu-id="a9ea9-207">範例應用查找資料庫上下文的服務描述符,並使用描述符刪除服務註冊。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-207">The sample app finds the service descriptor for the database context and uses the descriptor to remove the service registration.</span></span> <span data-ttu-id="a9ea9-208">接下來,工廠將添加新的`ApplicationDbContext`測試使用記憶體中資料庫。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-208">Next, the factory adds a new `ApplicationDbContext` that uses an in-memory database for the tests.</span></span>

   <span data-ttu-id="a9ea9-209">要連接到與記憶體中資料庫不同的資料庫,請更改`UseInMemoryDatabase`調用以將上下文連接到其他資料庫。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-209">To connect to a different database than the in-memory database, change the `UseInMemoryDatabase` call to connect the context to a different database.</span></span> <span data-ttu-id="a9ea9-210">要使用 SQL Server 測試資料庫,請使用:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-210">To use a SQL Server test database:</span></span>

   * <span data-ttu-id="a9ea9-211">在專案檔中引用[Microsoft.EntityFramework Core.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-211">Reference the [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) NuGet package in the project file.</span></span>
   * <span data-ttu-id="a9ea9-212">使用`UseSqlServer`與資料庫的連接字串調用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-212">Call `UseSqlServer` with a connection string to the database.</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>((options, context) => 
   {
       context.UseSqlServer(
           Configuration.GetConnectionString("TestingDbConnectionString"));
   });
   ```

2. <span data-ttu-id="a9ea9-213">在測試類`CustomWebApplicationFactory`中使用自定義。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-213">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="a9ea9-214">下面的範例在`IndexPageTests`類別使用工廠:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-214">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="a9ea9-215">示例應用的用戶端配置為防止以下`HttpClient`重定向。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-215">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="a9ea9-216">如稍後在[Mock 身份驗證](#mock-authentication)部分中所述,這允許測試以檢查應用的第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-216">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="a9ea9-217">第一個回應是許多具有`Location`標頭的測試中的重定向。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-217">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="a9ea9-218">典型的測試使用`HttpClient`與 說明器方法來處理請求和回應:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-218">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="a9ea9-219">任何POST請求SUT必須滿足反偽造檢查,這是自動由應用程式的[資料保護反偽造系統](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-219">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a9ea9-220">為了安排測試的開 POST 請求,測試應用必須:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-220">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="a9ea9-221">對頁面發出請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-221">Make a request for the page.</span></span>
1. <span data-ttu-id="a9ea9-222">分析反偽造 Cookie 並從回應請求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-222">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="a9ea9-223">使用防偽造 Cookie 進行 POST 請求,並請求驗證權杖到位。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-223">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="a9ea9-224">`SendAsync`[範例應用程式中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)的協助器擴充方法 (*協助器/HTTPClient 擴充.cs*) 與`GetDocumentAsync`說明器方法 (*協助器/HtmlHelpers.cs*) 使用[AngleSharp](https://anglesharp.github.io/)解析器處理反偽造檢查的方法:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-224">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="a9ea9-225">`GetDocumentAsync`&ndash;接收[HTTPResponse](/dotnet/api/system.net.http.httpresponsemessage)訊息`IHtmlDocument`並傳回 。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-225">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="a9ea9-226">`GetDocumentAsync`使用基於原始`HttpResponseMessage`的準備*虛擬回應*的工廠。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-226">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="a9ea9-227">有關詳細資訊,請參閱[AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-227">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="a9ea9-228">`SendAsync``HttpClient`撰寫[HTTPRequestMessage](/dotnet/api/system.net.http.httprequestmessage)並呼叫[SendAsync(HTTPRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法向 SUT 提交請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-228">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="a9ea9-229">`SendAsync`接受 HTML 窗型`IHtmlFormElement`( ) 的重載與以下內容:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-229">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="a9ea9-230">提交表格按鈕`IHtmlElement`( )</span><span class="sxs-lookup"><span data-stu-id="a9ea9-230">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="a9ea9-231">表單值集合`IEnumerable<KeyValuePair<string, string>>`( )</span><span class="sxs-lookup"><span data-stu-id="a9ea9-231">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="a9ea9-232">提交按鈕`IHtmlElement`( )`IEnumerable<KeyValuePair<string, string>>`與表單 ( )</span><span class="sxs-lookup"><span data-stu-id="a9ea9-232">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="a9ea9-233">[AngleSharp](https://anglesharp.github.io/)是一個第三方解析庫,用於本主題和示例應用中的演示目的。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-233">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="a9ea9-234">ASP.NET核心應用的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-234">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="a9ea9-235">可以使用其他解析器,例如[Html 敏捷包 (HAP)。](https://html-agility-pack.net/)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-235">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="a9ea9-236">另一種方法是編寫代碼,直接處理反偽造系統的請求驗證令牌和反偽造 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-236">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="a9ea9-237">使用 WebHostBuilder 自訂客戶端</span><span class="sxs-lookup"><span data-stu-id="a9ea9-237">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="a9ea9-238">當測試方法中需要其他配置時,[與 WebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)一起`WebApplicationFactory`創建一個新的[IWebHostBuilder,該構建器](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)透過配置進一步自定義。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-238">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="a9ea9-239">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)測試方法演示了`WithWebHostBuilder`的用法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-239">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="a9ea9-240">此測試通過在 SUT 中觸發表單提交來在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-240">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="a9ea9-241">由於`IndexPageTests`類中的另一個測試執行一個操作,該操作將刪除資料庫中的所有記錄,並且可能`Post_DeleteMessageHandler_ReturnsRedirectToRoot`在方法之前運行,因此將在此測試方法中重新設定資料庫的種子,以確保存在要刪除的 SUT 的記錄。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-241">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="a9ea9-242">在 SUT`messages`中選擇 表單的第一個刪除按鈕在 SUT 請求中模擬:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-242">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="a9ea9-243">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="a9ea9-243">Client options</span></span>

<span data-ttu-id="a9ea9-244">下表顯示了建立`HttpClient`實體時可用的預設[Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-244">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="a9ea9-245">選項</span><span class="sxs-lookup"><span data-stu-id="a9ea9-245">Option</span></span> | <span data-ttu-id="a9ea9-246">描述</span><span class="sxs-lookup"><span data-stu-id="a9ea9-246">Description</span></span> | <span data-ttu-id="a9ea9-247">預設</span><span class="sxs-lookup"><span data-stu-id="a9ea9-247">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="a9ea9-248">允許自動重定向</span><span class="sxs-lookup"><span data-stu-id="a9ea9-248">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="a9ea9-249">獲取或設置`HttpClient`實例是否應自動遵循重定向回應。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-249">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="a9ea9-250">基本位址</span><span class="sxs-lookup"><span data-stu-id="a9ea9-250">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="a9ea9-251">獲取或設置實例的基本`HttpClient`位址。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-251">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="a9ea9-252">手柄餅乾</span><span class="sxs-lookup"><span data-stu-id="a9ea9-252">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="a9ea9-253">獲取或設置實例`HttpClient`是否應處理 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-253">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="a9ea9-254">最大自動重定向</span><span class="sxs-lookup"><span data-stu-id="a9ea9-254">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="a9ea9-255">獲取或設置`HttpClient`實例應遵循的最大重定向回應數。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-255">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="a9ea9-256">7</span><span class="sxs-lookup"><span data-stu-id="a9ea9-256">7</span></span> |

<span data-ttu-id="a9ea9-257">建立`WebApplicationFactoryClientOptions`類別並將其傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法(預設值顯示在代碼範例中):</span><span class="sxs-lookup"><span data-stu-id="a9ea9-257">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="a9ea9-258">注入模擬服務</span><span class="sxs-lookup"><span data-stu-id="a9ea9-258">Inject mock services</span></span>

<span data-ttu-id="a9ea9-259">可以通過調用主機生成器上的[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)在測試中覆蓋服務。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-259">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="a9ea9-260">**要注入類比服務,SUT 必須`Startup`具有具有`Startup.ConfigureServices`方法的 類。**</span><span class="sxs-lookup"><span data-stu-id="a9ea9-260">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="a9ea9-261">示例 SUT 包括返回報價表的作用域服務。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-261">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="a9ea9-262">當請求索引頁時,報價嵌入到索引頁上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-262">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="a9ea9-263">*服務/IQuote服務.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-263">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="a9ea9-264">*服務/報價服務.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-264">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="a9ea9-265">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-265">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="a9ea9-266">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="a9ea9-266">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="a9ea9-267">*頁面/索引.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-267">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="a9ea9-268">執行 SUT 應用程式時,將產生以下標記:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-268">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="a9ea9-269">為了在整合測試中測試服務和報價注入,測試將類比服務注入 SUT。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-269">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="a9ea9-270">模擬服務`QuoteService`使用測試應用提供的服務取代應用,該服務`TestQuoteService`稱為 :</span><span class="sxs-lookup"><span data-stu-id="a9ea9-270">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="a9ea9-271">*IntegrationTests.IndexPageTests.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-271">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="a9ea9-272">`ConfigureTestServices`呼叫,並註冊作用域服務:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-272">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="a9ea9-273">在測試執行期間生成的標記反映了`TestQuoteService`提供的報價文本,因此斷言通過:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-273">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="a9ea9-274">類比認證</span><span class="sxs-lookup"><span data-stu-id="a9ea9-274">Mock authentication</span></span>

<span data-ttu-id="a9ea9-275">類別測試`AuthTests`檢查安全終結點:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-275">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="a9ea9-276">將未經身份驗證的用戶重定向到應用的登錄頁面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-276">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="a9ea9-277">返回經過身份驗證的用戶的內容。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-277">Returns content for an authenticated user.</span></span>

<span data-ttu-id="a9ea9-278">在 SUT`/SecurePage`中 ,該頁使用[授權頁](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)約定對頁面應用[授權篩選器](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-278">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="a9ea9-279">有關詳細資訊,請參閱[Razor 頁面授權約定](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-279">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="a9ea9-280">測試`Get_SecurePageRedirectsAnUnauthenticatedUser`中[,Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)設定為設定[「允許自動重定向」](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)來`false`關閉:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-280">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="a9ea9-281">通過不允許用戶端遵循重定向,可以進行以下檢查:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-281">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="a9ea9-282">SUT 傳回的狀態代碼可以根據預期的[HTTPStatusCode.重定向](/dotnet/api/system.net.httpstatuscode)結果進行檢查,而不是重定向到登入頁面後的最終狀態代碼,該頁面將是[HTTPStatusCode.OK](/dotnet/api/system.net.httpstatuscode)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-282">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="a9ea9-283">檢查`Location`回應標頭中的標頭值以確認它`http://localhost/Identity/Account/Login`以 開頭,而不是最終登錄頁回應`Location`,其中標頭將不存在。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-283">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="a9ea9-284">測試應用可以模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中的 ,以便測試身份驗證和授權的方面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-284">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="a9ea9-285">最小專案傳回[身份認證結果.成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span><span class="sxs-lookup"><span data-stu-id="a9ea9-285">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="a9ea9-286">當身份驗證方案設置為`Test`註冊`AddAuthentication`的位置 時,調用 的調用 以對用戶`ConfigureTestServices``TestAuthHandler`進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-286">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="a9ea9-287">有關的詳細資訊`WebApplicationFactoryClientOptions`,請參閱[用戶端選項](#client-options)部分。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-287">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="a9ea9-288">設定環境</span><span class="sxs-lookup"><span data-stu-id="a9ea9-288">Set the environment</span></span>

<span data-ttu-id="a9ea9-289">默認情況下,SUT 的主機和應用環境配置為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-289">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="a9ea9-290">要覆蓋 SUT 的環境,</span><span class="sxs-lookup"><span data-stu-id="a9ea9-290">To override the SUT's environment:</span></span>

* <span data-ttu-id="a9ea9-291">設定`ASPNETCORE_ENVIRONMENT`環境變數(例如`Staging`,`Production`, 或其他自訂`Testing`值, 如 .</span><span class="sxs-lookup"><span data-stu-id="a9ea9-291">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="a9ea9-292">在`CreateHostBuilder`測試應用中重寫以讀取預綴的環境`ASPNETCORE`變數。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-292">Override `CreateHostBuilder` in the test app to read environment variables prefixed with `ASPNETCORE`.</span></span>

```csharp
protected override IHostBuilder CreateHostBuilder() => 
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="a9ea9-293">測試基礎結構如何推斷應用內容根路徑</span><span class="sxs-lookup"><span data-stu-id="a9ea9-293">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="a9ea9-294">建構`WebApplicationFactory`函數透過在包含`TEntryPoint`與程式`System.Reflection.Assembly.FullName`集的整合測試的程式集上搜尋[WebAppFactoryContentRootattribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用[內容根](xref:fundamentals/index#content-root)路徑。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-294">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="a9ea9-295">如果找不到具有正確鍵的屬性,`WebApplicationFactory`請回退到搜尋解決方案檔 *(.sln*) 並將`TEntryPoint`程式集名稱追加到解決方案目錄。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-295">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="a9ea9-296">應用根目錄(內容根路徑)用於發現檢視和內容檔。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-296">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="a9ea9-297">關閉陰影複製</span><span class="sxs-lookup"><span data-stu-id="a9ea9-297">Disable shadow copying</span></span>

<span data-ttu-id="a9ea9-298">捲影複製會導致測試在不同於輸出目錄的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-298">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="a9ea9-299">要使測試正常工作,必須禁用捲影複製。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-299">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="a9ea9-300">[範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)使用 xUnit,並透過將*xunit.runner.json*檔與正確的設定設定包括,來禁用 xUnit 的捲影複製。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-300">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="a9ea9-301">有關詳細資訊,請參閱使用[JSON 配置 xUnit。](https://xunit.github.io/docs/configuring-with-json.html)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-301">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="a9ea9-302">將*xunit.runner.json*檔新增到測試項目的根目錄,內容如下:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-302">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a><span data-ttu-id="a9ea9-303">物品處理</span><span class="sxs-lookup"><span data-stu-id="a9ea9-303">Disposal of objects</span></span>

<span data-ttu-id="a9ea9-304">執行`IClassFixture`實現測試後,當 xUnit 處置[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時,將釋放[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HTTPClient。](/dotnet/api/system.net.http.httpclient)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-304">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="a9ea9-305">如果開發人員實例化的物件需要處理,請在實施中`IClassFixture`釋放它們。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-305">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="a9ea9-306">有關詳細資訊,請參閱實現[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-306">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="a9ea9-307">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="a9ea9-307">Integration tests sample</span></span>

<span data-ttu-id="a9ea9-308">[範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)由兩個應用組成:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-308">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="a9ea9-309">App</span><span class="sxs-lookup"><span data-stu-id="a9ea9-309">App</span></span> | <span data-ttu-id="a9ea9-310">專案目錄</span><span class="sxs-lookup"><span data-stu-id="a9ea9-310">Project directory</span></span> | <span data-ttu-id="a9ea9-311">描述</span><span class="sxs-lookup"><span data-stu-id="a9ea9-311">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="a9ea9-312">訊息應用(SUT)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-312">Message app (the SUT)</span></span> | <span data-ttu-id="a9ea9-313">*src/剃鬚刀專案*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-313">*src/RazorPagesProject*</span></span> | <span data-ttu-id="a9ea9-314">允許使用者添加、刪除一條、刪除所有郵件和分析郵件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-314">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="a9ea9-315">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="a9ea9-315">Test app</span></span> | <span data-ttu-id="a9ea9-316">*測試/剃鬚刀項目測試*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-316">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="a9ea9-317">用於集成測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-317">Used to integration test the SUT.</span></span> |

<span data-ttu-id="a9ea9-318">測試可以使用IDE的內置測試功能(如[Visual Studio)](https://visualstudio.microsoft.com)運行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-318">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="a9ea9-319">如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesProject.測試*目錄中的指令提示符執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-319">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```console
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="a9ea9-320">訊息應用 (SUT) 組織</span><span class="sxs-lookup"><span data-stu-id="a9ea9-320">Message app (SUT) organization</span></span>

<span data-ttu-id="a9ea9-321">SUT 是一個剃刀頁消息系統,具有以下特徵:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-321">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="a9ea9-322">應用程式的索引頁 (*頁面 / Index.cshtml*和*頁面 / Index.cshtml.cs*) 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(每條消息的平均字數)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-322">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="a9ea9-323">消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-323">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="a9ea9-324">該`Text`屬性是必需的,限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-324">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="a9ea9-325">消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-325">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="a9ea9-326">該應用程式在其資料庫上下文類中包含數據訪問層 (DAL) `AppDbContext` (*資料/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-326">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="a9ea9-327">如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-327">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="a9ea9-328">該應用程式包括`/SecurePage`只能由經過身份驗證的使用者訪問的應用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-328">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="a9ea9-329">&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-329">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="a9ea9-330">本主題使用[xUnit](https://xunit.github.io/)測試框架。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-330">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="a9ea9-331">不同測試框架的測試概念和測試實現相似但不相同。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-331">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="a9ea9-332">雖然應用程式不使用儲存庫模式,並且不是[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-332">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="a9ea9-333">有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)(示例實現存儲庫模式)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-333">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="a9ea9-334">測試應用組織</span><span class="sxs-lookup"><span data-stu-id="a9ea9-334">Test app organization</span></span>

<span data-ttu-id="a9ea9-335">測試應用是*測試/RazorPages Project.Test*目錄中的控制台應用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-335">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="a9ea9-336">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="a9ea9-336">Test app directory</span></span> | <span data-ttu-id="a9ea9-337">描述</span><span class="sxs-lookup"><span data-stu-id="a9ea9-337">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="a9ea9-338">*奧斯特測試*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-338">*AuthTests*</span></span> | <span data-ttu-id="a9ea9-339">包含用於:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-339">Contains test methods for:</span></span><ul><li><span data-ttu-id="a9ea9-340">由未經身份驗證的用戶訪問安全頁面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-340">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="a9ea9-341">通過模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>訪問經過身份驗證的使用者的安全頁面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-341">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="a9ea9-342">取得 GitHub 使用者設定檔並檢查設定檔的使用者登錄名。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-342">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> |
| <span data-ttu-id="a9ea9-343">*基本測試*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-343">*BasicTests*</span></span> | <span data-ttu-id="a9ea9-344">包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-344">Contains a test method for routing and content type.</span></span> |
| <span data-ttu-id="a9ea9-345">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-345">*IntegrationTests*</span></span> | <span data-ttu-id="a9ea9-346">包含使用自定義`WebApplicationFactory`類的 Index 頁的集成測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-346">Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="a9ea9-347">*說明者/公用程式*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-347">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="a9ea9-348">*Utilities.cs*包含用於`InitializeDbForTests`使用 測試數據為資料庫設定種子的方法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-348">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="a9ea9-349">*HtmlHelpers.cs*提供了返回 AngleSharp`IHtmlDocument`的方法,供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-349">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="a9ea9-350">*HttpClientExtensions.cs*提供`SendAsync`向 SUT 提交請求的重載。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-350">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="a9ea9-351">測試框架是[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-351">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="a9ea9-352">整合測試使用[Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行,其中包括[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-352">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="a9ea9-353">由於[Microsoft.AspNetCore.Mvc.測試](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)套件用於配置測試主機和測試伺服器,`TestHost``TestServer`因此和 包不需要在測試應用的專案檔或測試應用中的開發人員配置中直接引用包引用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-353">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="a9ea9-354">**設定資料庫進行測試的 torrent**</span><span class="sxs-lookup"><span data-stu-id="a9ea9-354">**Seeding the database for testing**</span></span>

<span data-ttu-id="a9ea9-355">集成測試通常需要在測試執行之前在資料庫中建立小型數據集。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-355">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="a9ea9-356">例如,刪除測試調用資料庫記錄刪除,因此資料庫必須至少有一個記錄才能成功刪除請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-356">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="a9ea9-357">範例應用程式在*資料庫*種子Utilities.cs中包含三條訊息,測試在執行時可以使用這些消息:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-357">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

<span data-ttu-id="a9ea9-358">SUT 的資料庫上下文`Startup.ConfigureServices`在其 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-358">The SUT's database context is registered in its `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a9ea9-359">測試應用的`builder.ConfigureServices`回調在執行應用`Startup.ConfigureServices`代碼*後*執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-359">The test app's `builder.ConfigureServices` callback is executed *after* the app's `Startup.ConfigureServices` code is executed.</span></span> <span data-ttu-id="a9ea9-360">要對測試使用不同的資料庫,必須在中`builder.ConfigureServices`替換應用程式的資料庫上下文。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-360">To use a different database for the tests, the app's database context must be replaced in `builder.ConfigureServices`.</span></span> <span data-ttu-id="a9ea9-361">有關詳細資訊,請參閱[自訂 Web 應用程式工廠](#customize-webapplicationfactory)部分。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-361">For more information, see the [Customize WebApplicationFactory](#customize-webapplicationfactory) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a9ea9-362">集成測試可確保應用元件在包含應用支援基礎結構(如資料庫、檔案系統和網路)的級別上正常運行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-362">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="a9ea9-363">ASP.NET Core 支援使用具有測試 Web 主機和記憶體中測試伺服器的單元測試框架進行集成測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-363">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="a9ea9-364">本主題假定對單元測試有基本的瞭解。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-364">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="a9ea9-365">如果不熟悉測試概念,請參閱[.NET 核心和 .NET 標準主題中的單元測試](/dotnet/core/testing/)及其鏈接內容。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-365">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="a9ea9-366">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9ea9-366">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a9ea9-367">示例應用是 Razor 頁面應用,並假定對 Razor 頁面有基本的瞭解。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-367">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="a9ea9-368">如果不熟悉 Razor 頁面,請參閱以下主題:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-368">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="a9ea9-369">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="a9ea9-369">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="a9ea9-370">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a9ea9-370">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="a9ea9-371">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-371">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="a9ea9-372">對於測試SpA,我們建議一個工具,如[硒](https://www.seleniumhq.org/),它可以自動化瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-372">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="a9ea9-373">整合測試簡介</span><span class="sxs-lookup"><span data-stu-id="a9ea9-373">Introduction to integration tests</span></span>

<span data-ttu-id="a9ea9-374">集成測試比[單元測試](/dotnet/core/testing/)在更廣泛的層面上評估應用的元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-374">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="a9ea9-375">單元測試用於測試隔離的軟體元件,如單個類方法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-375">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="a9ea9-376">集成測試確認兩個或多個應用元件協同工作以產生預期結果,可能包括完全處理請求所需的每個元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-376">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="a9ea9-377">這些更廣泛的測試用於測試應用程式的基礎結構和整個框架,通常包括以下元件:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-377">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="a9ea9-378">資料庫</span><span class="sxs-lookup"><span data-stu-id="a9ea9-378">Database</span></span>
* <span data-ttu-id="a9ea9-379">檔案系統</span><span class="sxs-lookup"><span data-stu-id="a9ea9-379">File system</span></span>
* <span data-ttu-id="a9ea9-380">網路設備</span><span class="sxs-lookup"><span data-stu-id="a9ea9-380">Network appliances</span></span>
* <span data-ttu-id="a9ea9-381">請求-回應導管</span><span class="sxs-lookup"><span data-stu-id="a9ea9-381">Request-response pipeline</span></span>

<span data-ttu-id="a9ea9-382">單元測試使用製造元件(稱為*假*體或*類比物件*)代替基礎結構元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-382">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="a9ea9-383">與單元測試相比,集成測試:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-383">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="a9ea9-384">使用應用在生產中使用的實際元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-384">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="a9ea9-385">需要更多的代碼和數據處理。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-385">Require more code and data processing.</span></span>
* <span data-ttu-id="a9ea9-386">需要更長的時間才能運行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-386">Take longer to run.</span></span>

<span data-ttu-id="a9ea9-387">因此,將集成測試的使用限制在最重要的基礎結構方案中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-387">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="a9ea9-388">如果可以使用單元測試或集成測試測試行為,請選擇單元測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-388">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="a9ea9-389">不要為資料庫和檔案系統中數據和文件存取的每一種可能排列編寫集成測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-389">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="a9ea9-390">無論應用中有多少位置與資料庫和檔案系統交互,集中讀取、寫入、更新和刪除集成測試集通常能夠充分測試資料庫和檔案系統元件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-390">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="a9ea9-391">使用單元測試對與這些元件交互的方法邏輯進行例行測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-391">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="a9ea9-392">在單元測試中,使用基礎結構假/類比會導致更快的測試執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-392">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="a9ea9-393">在討論集成測試時,測試專案通常稱為 *「正在測試的系統*」,簡稱為「SUT」。。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-393">In discussions of integration tests, the tested project is frequently called the *System Under Test*, or "SUT" for short.</span></span>
>
> <span data-ttu-id="a9ea9-394">*本主題中使用"SUT"來參考經過測試的 ASP.NET酷睿應用。*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-394">*"SUT" is used throughout this topic to refer to the tested ASP.NET Core app.*</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="a9ea9-395">ASP.NET核心整合測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-395">ASP.NET Core integration tests</span></span>

<span data-ttu-id="a9ea9-396">ASP.NET核心中的整合測試需要以下要求:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-396">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="a9ea9-397">測試專案用於包含和執行測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-397">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="a9ea9-398">測試專案具有對 SUT 的引用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-398">The test project has a reference to the SUT.</span></span>
* <span data-ttu-id="a9ea9-399">測試專案為 SUT 創建測試 Web 主機,並使用測試伺服器用戶端使用 SUT 處理請求和回應。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-399">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.</span></span>
* <span data-ttu-id="a9ea9-400">測試運行程式用於執行測試並報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-400">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="a9ea9-401">整合測試遵循一系列事件,其中包括通常的*排列*、*操作*與*斷言*測試步驟:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-401">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="a9ea9-402">SUT 的 Web 主機已配置。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-402">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="a9ea9-403">將創建測試伺服器用戶端以向應用提交請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-403">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="a9ea9-404">執行 *「排列*」測試步驟:測試應用準備請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-404">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="a9ea9-405">*執行 Act*測試步驟:用戶端提交請求並接收回應。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-405">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="a9ea9-406">*執行 Assert*測試步驟:*實際*回應根據*預期*回應驗證為*透過*或*失敗*。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-406">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="a9ea9-407">該過程將繼續,直到執行所有測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-407">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="a9ea9-408">報告測試結果。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-408">The test results are reported.</span></span>

<span data-ttu-id="a9ea9-409">通常,測試 Web 主機的配置方式與測試運行應用的正常 Web 主機不同。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-409">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="a9ea9-410">例如,測試可能使用不同的資料庫或不同的應用設置。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-410">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="a9ea9-411">基礎結構元件(如測試 Web 主機和記憶體中測試[伺服器 (TestServer)](/dotnet/api/microsoft.aspnetcore.testhost.testserver)由[Microsoft 提供](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)或管理。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-411">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="a9ea9-412">使用此包可以簡化測試創建和執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-412">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="a9ea9-413">該`Microsoft.AspNetCore.Mvc.Testing`套件處理以下工作:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-413">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="a9ea9-414">將依賴項檔 *(.deps*) 從 SUT 複製到測試專案的*bin*目錄中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-414">Copies the dependencies file (*.deps*) from the SUT into the test project's *bin* directory.</span></span>
* <span data-ttu-id="a9ea9-415">將[內容根目錄](xref:fundamentals/index#content-root)設置到 SUT 的專案根目錄,以便在執行測試時找到靜態檔和頁面/檢視。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-415">Sets the [content root](xref:fundamentals/index#content-root) to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="a9ea9-416">提供[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)類以`TestServer`簡化使用的 SUT 引導。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-416">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="a9ea9-417">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件介紹如何設置測試專案和測試運行程式,以及有關如何運行測試的詳細說明以及如何命名測試和測試類的建議。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-417">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="a9ea9-418">為應用創建測試專案時,將單元測試與集成測試分離到不同的專案中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-418">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="a9ea9-419">這有助於確保基礎結構測試元件不會意外包含在單元測試中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-419">This helps ensure that infrastructure testing components aren't accidentally included in the unit tests.</span></span> <span data-ttu-id="a9ea9-420">單元和集成測試的分離還允許控制運行哪組測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-420">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="a9ea9-421">Razor Pages 應用和 MVC 應用測試的配置之間幾乎沒有區別。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-421">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="a9ea9-422">唯一的區別是如何命名測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-422">The only difference is in how the tests are named.</span></span> <span data-ttu-id="a9ea9-423">在 Razor Pages 應用中,頁面終結點的測試通常以頁面模型類命名`IndexPageTests`(例如, 測試索引頁的元件整合)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-423">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="a9ea9-424">在 MVC 應用中,測試通常由控制器類組織,並基於它們測試的控制器命名(`HomeControllerTests`例如, 測試主控制器的元件整合)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-424">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="a9ea9-425">測試應用先決條件</span><span class="sxs-lookup"><span data-stu-id="a9ea9-425">Test app prerequisites</span></span>

<span data-ttu-id="a9ea9-426">測試項目必須:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-426">The test project must:</span></span>

* <span data-ttu-id="a9ea9-427">參考以下套件:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-427">Reference the following packages:</span></span>
  * [<span data-ttu-id="a9ea9-428">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="a9ea9-428">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [<span data-ttu-id="a9ea9-429">微軟.AspNetCore.Mvc.測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-429">Microsoft.AspNetCore.Mvc.Testing</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* <span data-ttu-id="a9ea9-430">在專案檔中指定 Web SDK`<Project Sdk="Microsoft.NET.Sdk.Web">`( 。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-430">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span> <span data-ttu-id="a9ea9-431">引用[Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)時需要 Web SDK。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-431">The Web SDK is required when referencing the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a9ea9-432">這些先決條件可以在[示例應用中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)看到。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-432">These prerequisites can be seen in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="a9ea9-433">檢查*測試/RazorPages專案.測試/RazorPages專案.測試.csproj*檔。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-433">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="a9ea9-434">範例應用程式使用[xUnit](https://xunit.github.io/)測試架構和[AngleSharp](https://anglesharp.github.io/)解析器庫,因此範例應用還引用:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-434">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="a9ea9-435">森特</span><span class="sxs-lookup"><span data-stu-id="a9ea9-435">xunit</span></span>](https://www.nuget.org/packages/xunit/)
* [<span data-ttu-id="a9ea9-436">xunit.runner.Visualstudio</span><span class="sxs-lookup"><span data-stu-id="a9ea9-436">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [<span data-ttu-id="a9ea9-437">角度夏普</span><span class="sxs-lookup"><span data-stu-id="a9ea9-437">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a><span data-ttu-id="a9ea9-438">SUT 環境</span><span class="sxs-lookup"><span data-stu-id="a9ea9-438">SUT environment</span></span>

<span data-ttu-id="a9ea9-439">如果未設置 SUT[的環境](xref:fundamentals/environments),則環境預設為"開發」。。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-439">If the SUT's [environment](xref:fundamentals/environments) isn't set, the environment defaults to Development.</span></span>

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="a9ea9-440">使用預設 Web 應用程式工廠的基本測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-440">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="a9ea9-441">[Web\<應用程式 工廠 TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)用於為整合測試建立[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-441">[WebApplicationFactory\<TEntryPoint>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="a9ea9-442">`TEntryPoint`是 SUT 的入口點類別,通常`Startup`是類 。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-442">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="a9ea9-443">測試類實現*類韌體*介面[(IClassFixture)](https://xunit.github.io/docs/shared-context#class-fixture)以指示類包含測試並在類中的測試中提供共用物件實例。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-443">Test classes implement a *class fixture* interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

<span data-ttu-id="a9ea9-444">以下`BasicTests`測試類`WebApplicationFactory`使用引導 SUT 並提供[HttpClient](/dotnet/api/system.net.http.httpclient)`Get_EndpointsReturnSuccessAndCorrectContentType`到測試方法 的 HTTPClient。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-444">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="a9ea9-445">該方法檢查回應狀態代碼是否成功(範圍為 200-299)的狀態代碼,`Content-Type`並且標頭`text/html; charset=utf-8`是 多個應用頁。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-445">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="a9ea9-446">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)創建一`HttpClient`個 實例,該實例會自動遵循重定向並處理 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-446">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

<span data-ttu-id="a9ea9-447">默認情況下,啟用[GDPR同意策略](xref:security/gdpr)時,不會跨請求保留非必需Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-447">By default, non-essential cookies aren't preserved across requests when the [GDPR consent policy](xref:security/gdpr) is enabled.</span></span> <span data-ttu-id="a9ea9-448">要保留非必需的 Cookie(如 TempData 提供者使用的 Cookie),請將其標記為測試中必不可少的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-448">To preserve non-essential cookies, such as those used by the TempData provider, mark them as essential in your tests.</span></span> <span data-ttu-id="a9ea9-449">有關將 Cookie 標記為基本 Cookie 的說明,請參閱[基本 Cookie](xref:security/gdpr#essential-cookies)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-449">For instructions on marking a cookie as essential, see [Essential cookies](xref:security/gdpr#essential-cookies).</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="a9ea9-450">自訂 Web 應用程式工廠</span><span class="sxs-lookup"><span data-stu-id="a9ea9-450">Customize WebApplicationFactory</span></span>

<span data-ttu-id="a9ea9-451">可以通過繼承`WebApplicationFactory`來 創建一個或多個自定義工廠,從而獨立於測試類創建 Web 主機配置:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-451">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="a9ea9-452">繼承`WebApplicationFactory`並覆蓋[設定 WebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-452">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="a9ea9-453">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)允許使用[設定服務](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices)設定服務集合 :</span><span class="sxs-lookup"><span data-stu-id="a9ea9-453">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="a9ea9-454">[範例應用中的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)資料庫種子設定由`InitializeDbForTests`方法執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-454">Database seeding in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="a9ea9-455">該方法在[集成測試示例:測試應用組織](#test-app-organization)部分中進行了描述。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-455">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="a9ea9-456">在測試類`CustomWebApplicationFactory`中使用自定義。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-456">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="a9ea9-457">下面的範例在`IndexPageTests`類別使用工廠:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-457">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="a9ea9-458">示例應用的用戶端配置為防止以下`HttpClient`重定向。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-458">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="a9ea9-459">如稍後在[Mock 身份驗證](#mock-authentication)部分中所述,這允許測試以檢查應用的第一個回應的結果。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-459">As explained later in the [Mock authentication](#mock-authentication) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="a9ea9-460">第一個回應是許多具有`Location`標頭的測試中的重定向。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-460">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="a9ea9-461">典型的測試使用`HttpClient`與 說明器方法來處理請求和回應:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-461">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="a9ea9-462">任何POST請求SUT必須滿足反偽造檢查,這是自動由應用程式的[資料保護反偽造系統](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-462">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a9ea9-463">為了安排測試的開 POST 請求,測試應用必須:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-463">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="a9ea9-464">對頁面發出請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-464">Make a request for the page.</span></span>
1. <span data-ttu-id="a9ea9-465">分析反偽造 Cookie 並從回應請求驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-465">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="a9ea9-466">使用防偽造 Cookie 進行 POST 請求,並請求驗證權杖到位。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-466">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="a9ea9-467">`SendAsync`[範例應用程式中](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/)的協助器擴充方法 (*協助器/HTTPClient 擴充.cs*) 與`GetDocumentAsync`說明器方法 (*協助器/HtmlHelpers.cs*) 使用[AngleSharp](https://anglesharp.github.io/)解析器處理反偽造檢查的方法:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-467">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="a9ea9-468">`GetDocumentAsync`&ndash;接收[HTTPResponse](/dotnet/api/system.net.http.httpresponsemessage)訊息`IHtmlDocument`並傳回 。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-468">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="a9ea9-469">`GetDocumentAsync`使用基於原始`HttpResponseMessage`的準備*虛擬回應*的工廠。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-469">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="a9ea9-470">有關詳細資訊,請參閱[AngleSharp 文件](https://github.com/AngleSharp/AngleSharp#documentation)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-470">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="a9ea9-471">`SendAsync``HttpClient`撰寫[HTTPRequestMessage](/dotnet/api/system.net.http.httprequestmessage)並呼叫[SendAsync(HTTPRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_)的擴充方法向 SUT 提交請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-471">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="a9ea9-472">`SendAsync`接受 HTML 窗型`IHtmlFormElement`( ) 的重載與以下內容:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-472">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  * <span data-ttu-id="a9ea9-473">提交表格按鈕`IHtmlElement`( )</span><span class="sxs-lookup"><span data-stu-id="a9ea9-473">Submit button of the form (`IHtmlElement`)</span></span>
  * <span data-ttu-id="a9ea9-474">表單值集合`IEnumerable<KeyValuePair<string, string>>`( )</span><span class="sxs-lookup"><span data-stu-id="a9ea9-474">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  * <span data-ttu-id="a9ea9-475">提交按鈕`IHtmlElement`( )`IEnumerable<KeyValuePair<string, string>>`與表單 ( )</span><span class="sxs-lookup"><span data-stu-id="a9ea9-475">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="a9ea9-476">[AngleSharp](https://anglesharp.github.io/)是一個第三方解析庫,用於本主題和示例應用中的演示目的。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-476">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="a9ea9-477">ASP.NET核心應用的整合測試不支援或不需要 AngleSharp。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-477">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="a9ea9-478">可以使用其他解析器,例如[Html 敏捷包 (HAP)。](https://html-agility-pack.net/)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-478">Other parsers can be used, such as the [Html Agility Pack (HAP)](https://html-agility-pack.net/).</span></span> <span data-ttu-id="a9ea9-479">另一種方法是編寫代碼,直接處理反偽造系統的請求驗證令牌和反偽造 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-479">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="a9ea9-480">使用 WebHostBuilder 自訂客戶端</span><span class="sxs-lookup"><span data-stu-id="a9ea9-480">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="a9ea9-481">當測試方法中需要其他配置時,[與 WebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)一起`WebApplicationFactory`創建一個新的[IWebHostBuilder,該構建器](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)透過配置進一步自定義。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-481">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="a9ea9-482">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`[範例應用程式的](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)測試方法演示了`WithWebHostBuilder`的用法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-482">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="a9ea9-483">此測試通過在 SUT 中觸發表單提交來在資料庫中執行記錄刪除。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-483">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="a9ea9-484">由於`IndexPageTests`類中的另一個測試執行一個操作,該操作將刪除資料庫中的所有記錄,並且可能`Post_DeleteMessageHandler_ReturnsRedirectToRoot`在方法之前運行,因此將在此測試方法中重新設定資料庫的種子,以確保存在要刪除的 SUT 的記錄。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-484">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is reseeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="a9ea9-485">在 SUT`messages`中選擇 表單的第一個刪除按鈕在 SUT 請求中模擬:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-485">Selecting the first delete button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="a9ea9-486">用戶端選項</span><span class="sxs-lookup"><span data-stu-id="a9ea9-486">Client options</span></span>

<span data-ttu-id="a9ea9-487">下表顯示了建立`HttpClient`實體時可用的預設[Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-487">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="a9ea9-488">選項</span><span class="sxs-lookup"><span data-stu-id="a9ea9-488">Option</span></span> | <span data-ttu-id="a9ea9-489">描述</span><span class="sxs-lookup"><span data-stu-id="a9ea9-489">Description</span></span> | <span data-ttu-id="a9ea9-490">預設</span><span class="sxs-lookup"><span data-stu-id="a9ea9-490">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="a9ea9-491">允許自動重定向</span><span class="sxs-lookup"><span data-stu-id="a9ea9-491">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="a9ea9-492">獲取或設置`HttpClient`實例是否應自動遵循重定向回應。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-492">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="a9ea9-493">基本位址</span><span class="sxs-lookup"><span data-stu-id="a9ea9-493">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="a9ea9-494">獲取或設置實例的基本`HttpClient`位址。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-494">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="a9ea9-495">手柄餅乾</span><span class="sxs-lookup"><span data-stu-id="a9ea9-495">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="a9ea9-496">獲取或設置實例`HttpClient`是否應處理 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-496">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="a9ea9-497">最大自動重定向</span><span class="sxs-lookup"><span data-stu-id="a9ea9-497">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="a9ea9-498">獲取或設置`HttpClient`實例應遵循的最大重定向回應數。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-498">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="a9ea9-499">7</span><span class="sxs-lookup"><span data-stu-id="a9ea9-499">7</span></span> |

<span data-ttu-id="a9ea9-500">建立`WebApplicationFactoryClientOptions`類別並將其傳遞給[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)方法(預設值顯示在代碼範例中):</span><span class="sxs-lookup"><span data-stu-id="a9ea9-500">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="a9ea9-501">注入模擬服務</span><span class="sxs-lookup"><span data-stu-id="a9ea9-501">Inject mock services</span></span>

<span data-ttu-id="a9ea9-502">可以通過調用主機生成器上的[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)在測試中覆蓋服務。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-502">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="a9ea9-503">**要注入類比服務,SUT 必須`Startup`具有具有`Startup.ConfigureServices`方法的 類。**</span><span class="sxs-lookup"><span data-stu-id="a9ea9-503">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="a9ea9-504">示例 SUT 包括返回報價表的作用域服務。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-504">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="a9ea9-505">當請求索引頁時,報價嵌入到索引頁上的隱藏欄位中。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-505">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="a9ea9-506">*服務/IQuote服務.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-506">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="a9ea9-507">*服務/報價服務.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-507">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="a9ea9-508">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-508">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="a9ea9-509">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="a9ea9-509">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="a9ea9-510">*頁面/索引.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-510">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="a9ea9-511">執行 SUT 應用程式時,將產生以下標記:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-511">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="a9ea9-512">為了在整合測試中測試服務和報價注入,測試將類比服務注入 SUT。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-512">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="a9ea9-513">模擬服務`QuoteService`使用測試應用提供的服務取代應用,該服務`TestQuoteService`稱為 :</span><span class="sxs-lookup"><span data-stu-id="a9ea9-513">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="a9ea9-514">*IntegrationTests.IndexPageTests.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-514">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="a9ea9-515">`ConfigureTestServices`呼叫,並註冊作用域服務:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-515">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="a9ea9-516">在測試執行期間生成的標記反映了`TestQuoteService`提供的報價文本,因此斷言通過:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-516">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="mock-authentication"></a><span data-ttu-id="a9ea9-517">類比認證</span><span class="sxs-lookup"><span data-stu-id="a9ea9-517">Mock authentication</span></span>

<span data-ttu-id="a9ea9-518">類別測試`AuthTests`檢查安全終結點:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-518">Tests in the `AuthTests` class check that a secure endpoint:</span></span>

* <span data-ttu-id="a9ea9-519">將未經身份驗證的用戶重定向到應用的登錄頁面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-519">Redirects an unauthenticated user to the app's Login page.</span></span>
* <span data-ttu-id="a9ea9-520">返回經過身份驗證的用戶的內容。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-520">Returns content for an authenticated user.</span></span>

<span data-ttu-id="a9ea9-521">在 SUT`/SecurePage`中 ,該頁使用[授權頁](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)約定對頁面應用[授權篩選器](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-521">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="a9ea9-522">有關詳細資訊,請參閱[Razor 頁面授權約定](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-522">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="a9ea9-523">測試`Get_SecurePageRedirectsAnUnauthenticatedUser`中[,Web 應用程式工廠客戶端選項](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)設定為設定[「允許自動重定向」](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)來`false`關閉:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-523">In the `Get_SecurePageRedirectsAnUnauthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet2)]

<span data-ttu-id="a9ea9-524">通過不允許用戶端遵循重定向,可以進行以下檢查:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-524">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="a9ea9-525">SUT 傳回的狀態代碼可以根據預期的[HTTPStatusCode.重定向](/dotnet/api/system.net.httpstatuscode)結果進行檢查,而不是重定向到登入頁面後的最終狀態代碼,該頁面將是[HTTPStatusCode.OK](/dotnet/api/system.net.httpstatuscode)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-525">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="a9ea9-526">檢查`Location`回應標頭中的標頭值以確認它`http://localhost/Identity/Account/Login`以 開頭,而不是最終登錄頁回應`Location`,其中標頭將不存在。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-526">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="a9ea9-527">測試應用可以模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>[配置測試服務](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)中的 ,以便測試身份驗證和授權的方面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-527">The test app can mock an <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> in [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) in order to test aspects of authentication and authorization.</span></span> <span data-ttu-id="a9ea9-528">最小專案傳回[身份認證結果.成功](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span><span class="sxs-lookup"><span data-stu-id="a9ea9-528">A minimal scenario returns an [AuthenticateResult.Success](xref:Microsoft.AspNetCore.Authentication.AuthenticateResult.Success*):</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet4&highlight=11-18)]

<span data-ttu-id="a9ea9-529">當身份驗證方案設置為`Test`註冊`AddAuthentication`的位置 時,調用 的調用 以對用戶`ConfigureTestServices``TestAuthHandler`進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-529">The `TestAuthHandler` is called to authenticate a user when the authentication scheme is set to `Test` where `AddAuthentication` is registered for `ConfigureTestServices`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/AuthTests.cs?name=snippet3&highlight=7-12)]

<span data-ttu-id="a9ea9-530">有關的詳細資訊`WebApplicationFactoryClientOptions`,請參閱[用戶端選項](#client-options)部分。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-530">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="a9ea9-531">設定環境</span><span class="sxs-lookup"><span data-stu-id="a9ea9-531">Set the environment</span></span>

<span data-ttu-id="a9ea9-532">默認情況下,SUT 的主機和應用環境配置為使用開發環境。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-532">By default, the SUT's host and app environment is configured to use the Development environment.</span></span> <span data-ttu-id="a9ea9-533">要覆蓋 SUT 的環境,</span><span class="sxs-lookup"><span data-stu-id="a9ea9-533">To override the SUT's environment:</span></span>

* <span data-ttu-id="a9ea9-534">設定`ASPNETCORE_ENVIRONMENT`環境變數(例如`Staging`,`Production`, 或其他自訂`Testing`值, 如 .</span><span class="sxs-lookup"><span data-stu-id="a9ea9-534">Set the `ASPNETCORE_ENVIRONMENT` environment variable (for example, `Staging`, `Production`, or other custom value, such as `Testing`).</span></span>
* <span data-ttu-id="a9ea9-535">在`CreateHostBuilder`測試應用中重寫以讀取預綴的環境`ASPNETCORE`變數。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-535">Override `CreateHostBuilder` in the test app to read environment variables prefixed with `ASPNETCORE`.</span></span>

```csharp
protected override IHostBuilder CreateHostBuilder() => 
    base.CreateHostBuilder()
        .ConfigureHostConfiguration(
            config => config.AddEnvironmentVariables("ASPNETCORE"));
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="a9ea9-536">測試基礎結構如何推斷應用內容根路徑</span><span class="sxs-lookup"><span data-stu-id="a9ea9-536">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="a9ea9-537">建構`WebApplicationFactory`函數透過在包含`TEntryPoint`與程式`System.Reflection.Assembly.FullName`集的整合測試的程式集上搜尋[WebAppFactoryContentRootattribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)來推斷應用[內容根](xref:fundamentals/index#content-root)路徑。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-537">The `WebApplicationFactory` constructor infers the app [content root](xref:fundamentals/index#content-root) path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="a9ea9-538">如果找不到具有正確鍵的屬性,`WebApplicationFactory`請回退到搜尋解決方案檔 *(.sln*) 並將`TEntryPoint`程式集名稱追加到解決方案目錄。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-538">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="a9ea9-539">應用根目錄(內容根路徑)用於發現檢視和內容檔。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-539">The app root directory (the content root path) is used to discover views and content files.</span></span>

## <a name="disable-shadow-copying"></a><span data-ttu-id="a9ea9-540">關閉陰影複製</span><span class="sxs-lookup"><span data-stu-id="a9ea9-540">Disable shadow copying</span></span>

<span data-ttu-id="a9ea9-541">捲影複製會導致測試在不同於輸出目錄的目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-541">Shadow copying causes the tests to execute in a different directory than the output directory.</span></span> <span data-ttu-id="a9ea9-542">要使測試正常工作,必須禁用捲影複製。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-542">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="a9ea9-543">[範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)使用 xUnit,並透過將*xunit.runner.json*檔與正確的設定設定包括,來禁用 xUnit 的捲影複製。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-543">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="a9ea9-544">有關詳細資訊,請參閱使用[JSON 配置 xUnit。](https://xunit.github.io/docs/configuring-with-json.html)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-544">For more information, see [Configuring xUnit with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="a9ea9-545">將*xunit.runner.json*檔新增到測試項目的根目錄,內容如下:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-545">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

<span data-ttu-id="a9ea9-546">如果使用 Visual Studio,請將檔案的 **「複製到輸出目錄」** 屬性設定為 **「始終複製**」。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-546">If using Visual Studio, set the file's **Copy to Output Directory** property to **Copy always**.</span></span> <span data-ttu-id="a9ea9-547">如果不使用 Visual Studio,則`Content`向測試應用的專案檔新增目標:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-547">If not using Visual Studio, add a `Content` target to the test app's project file:</span></span>

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a><span data-ttu-id="a9ea9-548">物品處理</span><span class="sxs-lookup"><span data-stu-id="a9ea9-548">Disposal of objects</span></span>

<span data-ttu-id="a9ea9-549">執行`IClassFixture`實現測試後,當 xUnit 處置[Web 應用程式工廠](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)時,將釋放[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)和[HTTPClient。](/dotnet/api/system.net.http.httpclient)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-549">After the tests of the `IClassFixture` implementation are executed, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) and [HttpClient](/dotnet/api/system.net.http.httpclient) are disposed when xUnit disposes of the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1).</span></span> <span data-ttu-id="a9ea9-550">如果開發人員實例化的物件需要處理,請在實施中`IClassFixture`釋放它們。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-550">If objects instantiated by the developer require disposal, dispose of them in the `IClassFixture` implementation.</span></span> <span data-ttu-id="a9ea9-551">有關詳細資訊,請參閱實現[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-551">For more information, see [Implementing a Dispose method](/dotnet/standard/garbage-collection/implementing-dispose).</span></span>

## <a name="integration-tests-sample"></a><span data-ttu-id="a9ea9-552">整合測試範例</span><span class="sxs-lookup"><span data-stu-id="a9ea9-552">Integration tests sample</span></span>

<span data-ttu-id="a9ea9-553">[範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples)由兩個應用組成:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-553">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="a9ea9-554">App</span><span class="sxs-lookup"><span data-stu-id="a9ea9-554">App</span></span> | <span data-ttu-id="a9ea9-555">專案目錄</span><span class="sxs-lookup"><span data-stu-id="a9ea9-555">Project directory</span></span> | <span data-ttu-id="a9ea9-556">描述</span><span class="sxs-lookup"><span data-stu-id="a9ea9-556">Description</span></span> |
| --- | ----------------- | ----------- |
| <span data-ttu-id="a9ea9-557">訊息應用(SUT)</span><span class="sxs-lookup"><span data-stu-id="a9ea9-557">Message app (the SUT)</span></span> | <span data-ttu-id="a9ea9-558">*src/剃鬚刀專案*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-558">*src/RazorPagesProject*</span></span> | <span data-ttu-id="a9ea9-559">允許使用者添加、刪除一條、刪除所有郵件和分析郵件。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-559">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="a9ea9-560">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="a9ea9-560">Test app</span></span> | <span data-ttu-id="a9ea9-561">*測試/剃鬚刀項目測試*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-561">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="a9ea9-562">用於集成測試 SUT。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-562">Used to integration test the SUT.</span></span> |

<span data-ttu-id="a9ea9-563">測試可以使用IDE的內置測試功能(如[Visual Studio)](https://visualstudio.microsoft.com)運行。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-563">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://visualstudio.microsoft.com).</span></span> <span data-ttu-id="a9ea9-564">如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesProject.測試*目錄中的指令提示符執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-564">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* directory:</span></span>

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="a9ea9-565">訊息應用 (SUT) 組織</span><span class="sxs-lookup"><span data-stu-id="a9ea9-565">Message app (SUT) organization</span></span>

<span data-ttu-id="a9ea9-566">SUT 是一個剃刀頁消息系統,具有以下特徵:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-566">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="a9ea9-567">應用程式的索引頁 (*頁面 / Index.cshtml*和*頁面 / Index.cshtml.cs*) 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(每條消息的平均字數)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-567">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="a9ea9-568">消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-568">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="a9ea9-569">該`Text`屬性是必需的,限制為 200 個字元。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-569">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="a9ea9-570">消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-570">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="a9ea9-571">該應用程式在其資料庫上下文類中包含數據訪問層 (DAL) `AppDbContext` (*資料/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-571">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="a9ea9-572">如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-572">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="a9ea9-573">該應用程式包括`/SecurePage`只能由經過身份驗證的使用者訪問的應用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-573">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="a9ea9-574">&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-574">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="a9ea9-575">本主題使用[xUnit](https://xunit.github.io/)測試框架。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-575">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="a9ea9-576">不同測試框架的測試概念和測試實現相似但不相同。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-576">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="a9ea9-577">雖然應用程式不使用儲存庫模式,並且不是[工作單位 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,Razor Pages 支援這些開發模式。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-577">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="a9ea9-578">有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和[測試控制器邏輯](/aspnet/core/mvc/controllers/testing)(示例實現存儲庫模式)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-578">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="a9ea9-579">測試應用組織</span><span class="sxs-lookup"><span data-stu-id="a9ea9-579">Test app organization</span></span>

<span data-ttu-id="a9ea9-580">測試應用是*測試/RazorPages Project.Test*目錄中的控制台應用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-580">The test app is a console app inside the *tests/RazorPagesProject.Tests* directory.</span></span>

| <span data-ttu-id="a9ea9-581">測試應用程式目錄</span><span class="sxs-lookup"><span data-stu-id="a9ea9-581">Test app directory</span></span> | <span data-ttu-id="a9ea9-582">描述</span><span class="sxs-lookup"><span data-stu-id="a9ea9-582">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="a9ea9-583">*奧斯特測試*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-583">*AuthTests*</span></span> | <span data-ttu-id="a9ea9-584">包含用於:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-584">Contains test methods for:</span></span><ul><li><span data-ttu-id="a9ea9-585">由未經身份驗證的用戶訪問安全頁面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-585">Accessing a secure page by an unauthenticated user.</span></span></li><li><span data-ttu-id="a9ea9-586">通過模擬<xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>訪問經過身份驗證的使用者的安全頁面。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-586">Accessing a secure page by an authenticated user with a mock <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span></li><li><span data-ttu-id="a9ea9-587">取得 GitHub 使用者設定檔並檢查設定檔的使用者登錄名。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-587">Obtaining a GitHub user profile and checking the profile's user login.</span></span></li></ul> |
| <span data-ttu-id="a9ea9-588">*基本測試*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-588">*BasicTests*</span></span> | <span data-ttu-id="a9ea9-589">包含路由和內容類型的測試方法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-589">Contains a test method for routing and content type.</span></span> |
| <span data-ttu-id="a9ea9-590">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-590">*IntegrationTests*</span></span> | <span data-ttu-id="a9ea9-591">包含使用自定義`WebApplicationFactory`類的 Index 頁的集成測試。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-591">Contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="a9ea9-592">*說明者/公用程式*</span><span class="sxs-lookup"><span data-stu-id="a9ea9-592">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="a9ea9-593">*Utilities.cs*包含用於`InitializeDbForTests`使用 測試數據為資料庫設定種子的方法。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-593">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="a9ea9-594">*HtmlHelpers.cs*提供了返回 AngleSharp`IHtmlDocument`的方法,供測試方法使用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-594">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="a9ea9-595">*HttpClientExtensions.cs*提供`SendAsync`向 SUT 提交請求的重載。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-595">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="a9ea9-596">測試框架是[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-596">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="a9ea9-597">整合測試使用[Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)進行,其中包括[測試伺服器](/dotnet/api/microsoft.aspnetcore.testhost.testserver)。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-597">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="a9ea9-598">由於[Microsoft.AspNetCore.Mvc.測試](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)套件用於配置測試主機和測試伺服器,`TestHost``TestServer`因此和 包不需要在測試應用的專案檔或測試應用中的開發人員配置中直接引用包引用。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-598">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="a9ea9-599">**設定資料庫進行測試的 torrent**</span><span class="sxs-lookup"><span data-stu-id="a9ea9-599">**Seeding the database for testing**</span></span>

<span data-ttu-id="a9ea9-600">集成測試通常需要在測試執行之前在資料庫中建立小型數據集。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-600">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="a9ea9-601">例如,刪除測試調用資料庫記錄刪除,因此資料庫必須至少有一個記錄才能成功刪除請求。</span><span class="sxs-lookup"><span data-stu-id="a9ea9-601">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="a9ea9-602">範例應用程式在*資料庫*種子Utilities.cs中包含三條訊息,測試在執行時可以使用這些消息:</span><span class="sxs-lookup"><span data-stu-id="a9ea9-602">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a9ea9-603">其他資源</span><span class="sxs-lookup"><span data-stu-id="a9ea9-603">Additional resources</span></span>

* [<span data-ttu-id="a9ea9-604">單元測試</span><span class="sxs-lookup"><span data-stu-id="a9ea9-604">Unit tests</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:test/razor-pages-tests>
* <xref:fundamentals/middleware/index>
* <xref:mvc/controllers/testing>
