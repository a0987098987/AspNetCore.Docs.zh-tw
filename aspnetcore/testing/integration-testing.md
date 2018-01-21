---
title: "整合測試 ASP.NET Core"
author: ardalis
description: "如何使用 ASP.NET Core 整合測試，以確保應用程式的元件可以正確運作。"
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: 8b0d741c05a723ad80fe812254c9a500a9fd9204
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="67c4a-103">整合測試 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67c4a-103">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="67c4a-104">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="67c4a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="67c4a-105">整合測試，可確保應用程式的元件正確運作時組合在一起。</span><span class="sxs-lookup"><span data-stu-id="67c4a-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="67c4a-106">ASP.NET Core 支援整合測試使用單元測試架構和內建測試 web 主機，可以用來處理要求網路額外負荷。</span><span class="sxs-lookup"><span data-stu-id="67c4a-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="67c4a-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="67c4a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="67c4a-108">整合測試的簡介</span><span class="sxs-lookup"><span data-stu-id="67c4a-108">Introduction to integration testing</span></span>

<span data-ttu-id="67c4a-109">整合測試會驗證，應用程式的不同部分正確一同運作。</span><span class="sxs-lookup"><span data-stu-id="67c4a-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="67c4a-110">不同於[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)，整合測試通常牽涉到的應用程式基礎結構考量，例如資料庫、 檔案系統、 網路資源或 web 要求和回應。</span><span class="sxs-lookup"><span data-stu-id="67c4a-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="67c4a-111">單元測試使用 fakes 或模擬物件來取代這些考量，但整合測試的目的是要確認系統可以與這些系統所正常運作。</span><span class="sxs-lookup"><span data-stu-id="67c4a-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="67c4a-112">整合測試，因為它們執行較大的程式碼區段，而且它們是依賴基礎結構項目，通常的數量級低於單元測試。</span><span class="sxs-lookup"><span data-stu-id="67c4a-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="67c4a-113">因此，最好限制多少的整合測試您所撰寫，尤其是您可以使用單元測試中測試相同的行為。</span><span class="sxs-lookup"><span data-stu-id="67c4a-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="67c4a-114">如果某些行為可使用單元測試或整合測試進行測試，會想要單元測試，因為它將會幾乎一律會比較快。</span><span class="sxs-lookup"><span data-stu-id="67c4a-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="67c4a-115">您可能會有數十份或數百個單元測試，以許多不同的輸入，但只要少數幾個整合測試涵蓋範圍的最重要的案例。</span><span class="sxs-lookup"><span data-stu-id="67c4a-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="67c4a-116">測試您自己的方法內的邏輯通常是網域的單元測試。</span><span class="sxs-lookup"><span data-stu-id="67c4a-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="67c4a-117">測試您的應用程式架構，例如與 ASP.NET Core 或資料庫中的運作方式是整合測試的位置開始執行。</span><span class="sxs-lookup"><span data-stu-id="67c4a-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="67c4a-118">它並不需要太多的整合測試，以確認您能夠寫入資料庫中的資料列和讀回。</span><span class="sxs-lookup"><span data-stu-id="67c4a-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="67c4a-119">您不需要測試每個可能的排列的資料存取程式碼，您只需要測試足以提供您的應用程式正常運作的信心。</span><span class="sxs-lookup"><span data-stu-id="67c4a-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="67c4a-120">整合測試的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67c4a-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="67c4a-121">若要取得設定執行的整合測試，您將需要建立測試專案，請將參考加入 ASP.NET Core web 專案，並安裝的測試執行器。</span><span class="sxs-lookup"><span data-stu-id="67c4a-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="67c4a-122">此程序所述[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件，以及執行測試和建議命名您的測試和測試類別的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="67c4a-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="67c4a-123">將您的單元測試和整合測試使用不同的專案。</span><span class="sxs-lookup"><span data-stu-id="67c4a-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="67c4a-124">這有助於確保您不小心引入基礎結構考量單元測試，並可讓您輕鬆地選擇要執行的測試設定。</span><span class="sxs-lookup"><span data-stu-id="67c4a-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="67c4a-125">測試主機</span><span class="sxs-lookup"><span data-stu-id="67c4a-125">The Test Host</span></span>

<span data-ttu-id="67c4a-126">ASP.NET Core 包含可加入至整合測試專案的測試主機來裝載 ASP.NET Core 應用程式使用，而不需要實際的 web 主機要求的服務測試。</span><span class="sxs-lookup"><span data-stu-id="67c4a-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="67c4a-127">提供的範例包括整合測試專案已設定使用[xUnit](https://xunit.github.io)和測試的主機。</span><span class="sxs-lookup"><span data-stu-id="67c4a-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="67c4a-128">它會使用`Microsoft.AspNetCore.TestHost`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="67c4a-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="67c4a-129">一次`Microsoft.AspNetCore.TestHost`封裝包含在專案中，您便可以建立及設定`TestServer`在您的測試。</span><span class="sxs-lookup"><span data-stu-id="67c4a-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="67c4a-130">下列測試顯示如何以確認站台的根提出的要求會傳回"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="67c4a-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="67c4a-131">和應該成功地對執行預設 Visual Studio 所建立的 ASP.NET Core 空白網站範本。</span><span class="sxs-lookup"><span data-stu-id="67c4a-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="67c4a-132">這項測試會使用排列 Act Assert 模式。</span><span class="sxs-lookup"><span data-stu-id="67c4a-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="67c4a-133">排列步驟會在建構函式，建立的執行個體中完成`TestServer`。</span><span class="sxs-lookup"><span data-stu-id="67c4a-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="67c4a-134">設定`WebHostBuilder`將用來建立`TestHost`; 在此範例中，`Configure`來自待測系統 (SUT) 的方法`Startup`類別會傳遞至`WebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="67c4a-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="67c4a-135">這個方法會用來設定的要求管線`TestServer`相同至 SUT 伺服器會設定方式。</span><span class="sxs-lookup"><span data-stu-id="67c4a-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="67c4a-136">在測試動作部分，請發出要求來`TestServer`"/"路徑，和回應的執行個體重新讀入字串。</span><span class="sxs-lookup"><span data-stu-id="67c4a-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="67c4a-137">與預期的"Hello World ！"的字串，這個字串進行比較。</span><span class="sxs-lookup"><span data-stu-id="67c4a-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="67c4a-138">如果兩者相符，測試就會成功。否則，它會失敗。</span><span class="sxs-lookup"><span data-stu-id="67c4a-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="67c4a-139">現在您可以加入一些其他的整合測試，以確認質數檢查功能，適用於透過 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="67c4a-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="67c4a-140">請注意，您不會真的想要測試正確性的質數檢查程式，而是，但是這些測試與 web 應用程式正在執行您的預期。</span><span class="sxs-lookup"><span data-stu-id="67c4a-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="67c4a-141">您已有單元測試涵蓋範圍，可讓您在中的信心`PrimeService`，您可以在這裡看到如下：</span><span class="sxs-lookup"><span data-stu-id="67c4a-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![測試總管](integration-testing/_static/test-explorer.png)

<span data-ttu-id="67c4a-143">您可以進一步了解中的單元測試[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)發行項。</span><span class="sxs-lookup"><span data-stu-id="67c4a-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="67c4a-144">測試 Mvc/Razor 的整合</span><span class="sxs-lookup"><span data-stu-id="67c4a-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="67c4a-145">含有 Razor 檢視的測試專案需要`<PreserveCompilationContext>`設為 true *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="67c4a-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="67c4a-146">專案遺漏這個項目會產生類似下面的錯誤：</span><span class="sxs-lookup"><span data-stu-id="67c4a-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="67c4a-147">若要使用的中介軟體中重構</span><span class="sxs-lookup"><span data-stu-id="67c4a-147">Refactoring to use middleware</span></span>

<span data-ttu-id="67c4a-148">重構是變更以改善其設計不會變更其行為的應用程式的程式碼的程序。</span><span class="sxs-lookup"><span data-stu-id="67c4a-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="67c4a-149">在理想情況下應該有一套傳遞測試，因為這些協助確保系統行為保持不變之前和之後所做的變更時。</span><span class="sxs-lookup"><span data-stu-id="67c4a-149">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="67c4a-150">查看所在的 web 應用程式中實作檢查邏輯質數方式`Configure`方法，您看到：</span><span class="sxs-lookup"><span data-stu-id="67c4a-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

<span data-ttu-id="67c4a-151">此程式碼可以運作，但很遠低於您希望如何實作 ASP.NET Core 應用程式中，即使一樣簡單因為這是這種功能。</span><span class="sxs-lookup"><span data-stu-id="67c4a-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="67c4a-152">假設有什麼`Configure`方法看起來會像視每次您加入另一個 URL 端點加入這麼多的程式碼 ！</span><span class="sxs-lookup"><span data-stu-id="67c4a-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="67c4a-153">要考慮的其中一個選項將加入[MVC](xref:mvc/overview)應用程式，以及建立控制站來處理的基本檢查。</span><span class="sxs-lookup"><span data-stu-id="67c4a-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="67c4a-154">不過，假設您目前不需要任何其他 MVC 功能，位元 overkill。</span><span class="sxs-lookup"><span data-stu-id="67c4a-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="67c4a-155">您可以不過，利用 ASP.NET Core[中介軟體](xref:fundamentals/middleware)，這將協助我們封裝質數檢查它自己的類別中的邏輯，並達成更好[的重要性分離](http://deviq.com/separation-of-concerns/)中`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="67c4a-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="67c4a-156">您想要允許讓中介軟體類別必須是做為參數，指定用於中介軟體路徑`RequestDelegate`和`PrimeCheckerOptions`其建構函式中的執行個體。</span><span class="sxs-lookup"><span data-stu-id="67c4a-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="67c4a-157">如果要求路徑不符合此中介軟體的是設定為預期，您只需呼叫鏈結中的下一個中介軟體，並且進行進一步動作。</span><span class="sxs-lookup"><span data-stu-id="67c4a-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="67c4a-158">中的實作程式碼的其餘部分`Configure`現在處於`Invoke`方法。</span><span class="sxs-lookup"><span data-stu-id="67c4a-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="67c4a-159">由於中介軟體取決於`PrimeService`服務，您也正在要求的建構函式與此服務執行個體。</span><span class="sxs-lookup"><span data-stu-id="67c4a-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="67c4a-160">架構會提供此服務透過[相依性插入](xref:fundamentals/dependency-injection)，假設它已設定，例如在`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="67c4a-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="67c4a-161">其路徑會比對時，此中介軟體都可做為要求委派鏈結中的端點，因為沒有要呼叫`_next.Invoke`當此中介軟體會處理要求。</span><span class="sxs-lookup"><span data-stu-id="67c4a-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="67c4a-162">與此中介軟體，而且某些很有幫助擴充方法建立，以簡化設定它，重構的`Configure`方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="67c4a-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="67c4a-163">這項重構之後, 您確信，web 應用程式仍能運作，因為所有要通過整合測試。</span><span class="sxs-lookup"><span data-stu-id="67c4a-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="67c4a-164">您最好將變更認可到原始檔控制之後您完成重構和您的測試都成功。</span><span class="sxs-lookup"><span data-stu-id="67c4a-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="67c4a-165">如果您正在練習測試導向開發[認可加入您紅-綠-重構 」 循環，請考慮](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development)。</span><span class="sxs-lookup"><span data-stu-id="67c4a-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="67c4a-166">資源</span><span class="sxs-lookup"><span data-stu-id="67c4a-166">Resources</span></span>

* [<span data-ttu-id="67c4a-167">單元測試</span><span class="sxs-lookup"><span data-stu-id="67c4a-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="67c4a-168">中介軟體</span><span class="sxs-lookup"><span data-stu-id="67c4a-168">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="67c4a-169">測試控制器</span><span class="sxs-lookup"><span data-stu-id="67c4a-169">Testing controllers</span></span>](xref:mvc/controllers/testing)
