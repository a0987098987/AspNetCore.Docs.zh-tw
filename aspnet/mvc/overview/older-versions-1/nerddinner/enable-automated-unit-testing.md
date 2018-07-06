---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: 啟用自動化單元測試 |Microsoft Docs
author: microsoft
description: 步驟 12 示範如何開發自動化的單元測試，以便驗證我們 NerdDinner 的功能，並可將讓我們能夠放心進行變更的套件...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 2247dc2e6d22cc0d5ddba97dfe6c7d2d1b0e49be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819403"
---
<a name="enable-automated-unit-testing"></a><span data-ttu-id="9ff76-103">啟用自動化的單元測試</span><span class="sxs-lookup"><span data-stu-id="9ff76-103">Enable Automated Unit Testing</span></span>
====================
<span data-ttu-id="9ff76-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9ff76-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="9ff76-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="9ff76-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="9ff76-106">這是一套免費的步驟 12 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ff76-106">This is step 12 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="9ff76-107">步驟 12 示範如何開發自動化的單元測試，以便驗證我們 NerdDinner 的功能，並可將讓我們能夠放心進行變更和改進未來的應用程式套件。</span><span class="sxs-lookup"><span data-stu-id="9ff76-107">Step 12 shows how to develop a suite of automated unit tests that verify our NerdDinner functionality, and which will give us the confidence to make changes and improvements to the application in the future.</span></span>
> 
> <span data-ttu-id="9ff76-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="9ff76-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-12-unit-testing"></a><span data-ttu-id="9ff76-109">NerdDinner 步驟 12： 單元測試</span><span class="sxs-lookup"><span data-stu-id="9ff76-109">NerdDinner Step 12: Unit Testing</span></span>

<span data-ttu-id="9ff76-110">讓我們來開發自動化的單元測試，以便驗證我們 NerdDinner 的功能，並可將讓我們能夠放心進行變更和改進未來的應用程式套件。</span><span class="sxs-lookup"><span data-stu-id="9ff76-110">Let's develop a suite of automated unit tests that verify our NerdDinner functionality, and which will give us the confidence to make changes and improvements to the application in the future.</span></span>

### <a name="why-unit-test"></a><span data-ttu-id="9ff76-111">為什麼要進行單元測試？</span><span class="sxs-lookup"><span data-stu-id="9ff76-111">Why Unit Test?</span></span>

<span data-ttu-id="9ff76-112">工作某天早上的磁碟機上，您必須依據您進行應用程式相關的靈感突然 flash。</span><span class="sxs-lookup"><span data-stu-id="9ff76-112">On the drive into work one morning you have a sudden flash of inspiration about an application you are working on.</span></span> <span data-ttu-id="9ff76-113">您了解變更您可以實作可讓應用程式大幅更好。</span><span class="sxs-lookup"><span data-stu-id="9ff76-113">You realize there is a change you can implement that will make the application dramatically better.</span></span> <span data-ttu-id="9ff76-114">它可能可以進行重整，清除程式碼，加入新的功能，或修正 bug。</span><span class="sxs-lookup"><span data-stu-id="9ff76-114">It might be a refactoring that cleans up the code, adds a new feature, or fixes a bug.</span></span>

<span data-ttu-id="9ff76-115">當您抵達您的電腦 confronts 您的問題是 – 「 安全性程度 」，藉此改進功能？</span><span class="sxs-lookup"><span data-stu-id="9ff76-115">The question that confronts you when you arrive at your computer is – "how safe is it to make this improvement?"</span></span> <span data-ttu-id="9ff76-116">如果您進行變更有副作用或中斷的項目嗎？</span><span class="sxs-lookup"><span data-stu-id="9ff76-116">What if making the change has side effects or breaks something?</span></span> <span data-ttu-id="9ff76-117">變更可能很簡單，而且只需要幾分鐘的時間來實作，但如果需要手動測試的應用程式案例的所有時數？</span><span class="sxs-lookup"><span data-stu-id="9ff76-117">The change might be simple and only take a few minutes to implement, but what if it takes hours to manually test out all of the application scenarios?</span></span> <span data-ttu-id="9ff76-118">如果您忘記涵蓋的案例，並中斷的應用程式會進入生產環境嗎？</span><span class="sxs-lookup"><span data-stu-id="9ff76-118">What if you forget to cover a scenario and a broken application goes into production?</span></span> <span data-ttu-id="9ff76-119">進行這項改善，真的值得下所有投入時間？</span><span class="sxs-lookup"><span data-stu-id="9ff76-119">Is making this improvement really worth all the effort?</span></span>

<span data-ttu-id="9ff76-120">自動化的單元測試可以提供防護機制，可讓您持續增強您的應用程式，並可避免您正在使用的程式碼的風險。</span><span class="sxs-lookup"><span data-stu-id="9ff76-120">Automated unit tests can provide a safety net that enables you to continually enhance your applications, and avoid being afraid of the code you are working on.</span></span> <span data-ttu-id="9ff76-121">執行具有自動化測試來快速驗證功能可讓您放心 – 程式碼，並讓您能夠讓您可能會否則不會覺得舒適的改善。</span><span class="sxs-lookup"><span data-stu-id="9ff76-121">Having automated tests that quickly verify functionality enables you to code with confidence – and empower you to make improvements you might otherwise not have felt comfortable doing.</span></span> <span data-ttu-id="9ff76-122">此外，它們也會協助建立更容易維護的解決方案，並具有較長的存留期-投資報酬以更高的潛在客戶。</span><span class="sxs-lookup"><span data-stu-id="9ff76-122">They also help create solutions that are more maintainable and have a longer lifetime - which leads to a much higher return on investment.</span></span>

<span data-ttu-id="9ff76-123">ASP.NET MVC 架構能夠以簡單且自然的單元測試應用程式功能。</span><span class="sxs-lookup"><span data-stu-id="9ff76-123">The ASP.NET MVC Framework makes it easy and natural to unit test application functionality.</span></span> <span data-ttu-id="9ff76-124">它也可讓測試導向開發 (TDD) 工作流程，可讓 「 測試先行 」 開發。</span><span class="sxs-lookup"><span data-stu-id="9ff76-124">It also enables a Test Driven Development (TDD) workflow that enables test-first based development.</span></span>

### <a name="nerddinnertests-project"></a><span data-ttu-id="9ff76-125">NerdDinner.Tests 專案</span><span class="sxs-lookup"><span data-stu-id="9ff76-125">NerdDinner.Tests Project</span></span>

<span data-ttu-id="9ff76-126">當我們建立我們的 NerdDinner 應用程式在本教學課程開頭時，我們已收到提示對話方塊，詢問是否要建立單元測試專案來搭配應用程式專案：</span><span class="sxs-lookup"><span data-stu-id="9ff76-126">When we created our NerdDinner application at the beginning of this tutorial, we were prompted with a dialog asking whether we wanted to create a unit test project to go along with the application project:</span></span>

![](enable-automated-unit-testing/_static/image1.png)

<span data-ttu-id="9ff76-127">我們保留 「 是的建立單元測試專案 」 選項按鈕已選取-，導致 「 NerdDinner.Tests"專案新增至我們的解決方案：</span><span class="sxs-lookup"><span data-stu-id="9ff76-127">We kept the "Yes, create a unit test project" radio button selected – which resulted in a "NerdDinner.Tests" project being added to our solution:</span></span>

![](enable-automated-unit-testing/_static/image2.png)

<span data-ttu-id="9ff76-128">NerdDinner.Tests 專案參考 NerdDinner 應用程式專案組件，且讓我們能夠輕鬆地加入自動化的測試，確認應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="9ff76-128">The NerdDinner.Tests project references the NerdDinner application project assembly, and enables us to easily add automated tests to it that verify the application functionality.</span></span>

### <a name="creating-unit-tests-for-our-dinner-model-class"></a><span data-ttu-id="9ff76-129">建立單元測試 Dinner 模型類別</span><span class="sxs-lookup"><span data-stu-id="9ff76-129">Creating Unit Tests for our Dinner Model Class</span></span>

<span data-ttu-id="9ff76-130">讓我們加入一些測試來確認 Dinner 類別，我們建立我們的模型層時，我們建立我們 NerdDinner.Tests 專案。</span><span class="sxs-lookup"><span data-stu-id="9ff76-130">Let's add some tests to our NerdDinner.Tests project that verify the Dinner class we created when we built our model layer.</span></span>

<span data-ttu-id="9ff76-131">我們先建立新資料夾我們稱為 「 模型 」 的測試專案中我們將在其中放置我們模型相關的測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-131">We'll start by creating a new folder within our test project called "Models" where we'll place our model-related tests.</span></span> <span data-ttu-id="9ff76-132">我們將在資料夾上按一下滑鼠右鍵，然後選擇**Add-&gt;新的測試**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9ff76-132">We'll then right-click on the folder and choose the **Add-&gt;New Test** menu command.</span></span> <span data-ttu-id="9ff76-133">這會顯示 [加入新測試] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9ff76-133">This will bring up the "Add New Test" dialog.</span></span>

<span data-ttu-id="9ff76-134">我們將選擇要建立 「 單元測試 」 並將它命名為"DinnerTest.cs 」:</span><span class="sxs-lookup"><span data-stu-id="9ff76-134">We'll choose to create a "Unit Test" and name it "DinnerTest.cs":</span></span>

![](enable-automated-unit-testing/_static/image3.png)

<span data-ttu-id="9ff76-135">當我們按一下 [確定] 按鈕時 Visual Studio 會加入 （會開啟） DinnerTest.cs 檔案加入專案：</span><span class="sxs-lookup"><span data-stu-id="9ff76-135">When we click the "ok" button Visual Studio will add (and open) a DinnerTest.cs file to the project:</span></span>

![](enable-automated-unit-testing/_static/image4.png)

<span data-ttu-id="9ff76-136">預設 Visual Studio 單元測試範本有一大堆樣板程式碼在其中找到有點繁瑣。</span><span class="sxs-lookup"><span data-stu-id="9ff76-136">The default Visual Studio unit test template has a bunch of boiler-plate code within it that I find a little messy.</span></span> <span data-ttu-id="9ff76-137">讓我們來清除它只包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9ff76-137">Let's clean it up to just contain the code below:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

<span data-ttu-id="9ff76-138">在上述 DinnerTest 類別上的 [TestClass] 屬性將其識別為測試，以及選擇性的測試初始化和清除程式碼將會包含的類別。</span><span class="sxs-lookup"><span data-stu-id="9ff76-138">The [TestClass] attribute on the DinnerTest class above identifies it as a class that will contain tests, as well as optional test initialization and teardown code.</span></span> <span data-ttu-id="9ff76-139">我們可以藉由新增在其有 [TestMethod] 屬性的公用方法，定義其內的測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-139">We can define tests within it by adding public methods that have a [TestMethod] attribute on them.</span></span>

<span data-ttu-id="9ff76-140">以下是兩個測試，我們將新增，以執行我們的 Dinner 類別中的第一個。</span><span class="sxs-lookup"><span data-stu-id="9ff76-140">Below are the first of two tests we'll add that exercise our Dinner class.</span></span> <span data-ttu-id="9ff76-141">第一項測試會驗證我們吃晚餐不正確的是否在建立新的 Dinner 時未正確設定的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="9ff76-141">The first test verifies that our Dinner is invalid if a new Dinner is created without all properties being set correctly.</span></span> <span data-ttu-id="9ff76-142">第二項測試會驗證我們吃晚餐吃晚餐具有有效的值設定的所有屬性時才會有效：</span><span class="sxs-lookup"><span data-stu-id="9ff76-142">The second test verifies that our Dinner is valid when a Dinner has all properties set with valid values:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

<span data-ttu-id="9ff76-143">您會注意到上面，我們的測試名稱都是非常明確 （和比較的詳細資訊）。</span><span class="sxs-lookup"><span data-stu-id="9ff76-143">You'll notice above that our test names are very explicit (and somewhat verbose).</span></span> <span data-ttu-id="9ff76-144">因為我們可能會得到建立數百或數千個小型的測試，而且我們想要讓您輕鬆快速判斷每一個函數的行為與意圖 （特別是當我們會仔細檢查失敗的測試執行器中的清單），我們會這麼做。</span><span class="sxs-lookup"><span data-stu-id="9ff76-144">We are doing this because we might end up creating hundreds or thousands of small tests, and we want to make it easy to quickly determine the intent and behavior of each of them (especially when we are looking through a list of failures in a test runner).</span></span> <span data-ttu-id="9ff76-145">測試名稱應該測試的功能之後命名。</span><span class="sxs-lookup"><span data-stu-id="9ff76-145">The test names should be named after the functionality they are testing.</span></span> <span data-ttu-id="9ff76-146">以上所述，我們會使用 「 名詞\_應該\_動詞"命名模式。</span><span class="sxs-lookup"><span data-stu-id="9ff76-146">Above we are using a "Noun\_Should\_Verb" naming pattern.</span></span>

<span data-ttu-id="9ff76-147">我們會建構使用測試模式 – 代表 「 排列、 Act、 判斷提示 」"AAA"的測試：</span><span class="sxs-lookup"><span data-stu-id="9ff76-147">We are structuring the tests using the "AAA" testing pattern – which stands for "Arrange, Act, Assert":</span></span>

- <span data-ttu-id="9ff76-148">排列： 設定單元外待測試</span><span class="sxs-lookup"><span data-stu-id="9ff76-148">Arrange: Setup the unit being tested</span></span>
- <span data-ttu-id="9ff76-149">Act： 執行單元測試，並擷取結果</span><span class="sxs-lookup"><span data-stu-id="9ff76-149">Act: Exercise the unit under test and capture results</span></span>
- <span data-ttu-id="9ff76-150">判斷提示： 驗證的行為</span><span class="sxs-lookup"><span data-stu-id="9ff76-150">Assert: Verify the behavior</span></span>

<span data-ttu-id="9ff76-151">當我們撰寫我們想要避免個別測試的測試執行太多作業。</span><span class="sxs-lookup"><span data-stu-id="9ff76-151">When we write tests we want to avoid having the individual tests do too much.</span></span> <span data-ttu-id="9ff76-152">改為每個測試應該確認只有一個單一的概念 （這會讓您更容易找出失敗的原因）。</span><span class="sxs-lookup"><span data-stu-id="9ff76-152">Instead each test should verify only a single concept (which will make it much easier to pinpoint the cause of failures).</span></span> <span data-ttu-id="9ff76-153">理想的指導方針，就是嘗試只能有單一 assert 陳述式，針對每個測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-153">A good guideline is to try and only have a single assert statement for each test.</span></span> <span data-ttu-id="9ff76-154">如果您有多個測試方法中的陳述式的判斷提示，請確定它們是所有要用來測試相同的概念。</span><span class="sxs-lookup"><span data-stu-id="9ff76-154">If you have more than one assert statement in a test method, make sure they are all being used to test the same concept.</span></span> <span data-ttu-id="9ff76-155">如有疑問，請另一項測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-155">When in doubt, make another test.</span></span>

### <a name="running-tests"></a><span data-ttu-id="9ff76-156">正在執行測試</span><span class="sxs-lookup"><span data-stu-id="9ff76-156">Running Tests</span></span>

<span data-ttu-id="9ff76-157">Visual Studio 2008 Professional （及更新版本） 包含可用來執行 Visual Studio 單元測試專案，在 IDE 中的內建的測試執行器。</span><span class="sxs-lookup"><span data-stu-id="9ff76-157">Visual Studio 2008 Professional (and higher editions) includes a built-in test runner that can be used to run Visual Studio Unit Test projects within the IDE.</span></span> <span data-ttu-id="9ff76-158">我們可以選取**Test-&gt;執行位&gt;方案中的所有測試**功能表命令 （或類型 Ctrl R、 A） 以執行所有的單元測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-158">We can select the **Test-&gt;Run-&gt;All Tests in Solution** menu command (or type Ctrl R, A) to run all of our unit tests.</span></span> <span data-ttu-id="9ff76-159">或者我們可以在 特定的測試類別或測試方法中我們游標的位置，並使用**Test-&gt;執行位&gt;目前內容中的測試**功能表命令 （或型別 Ctrl R、 T） 來執行單元測試的子集。</span><span class="sxs-lookup"><span data-stu-id="9ff76-159">Or alternatively we can position our cursor within a specific test class or test method and use the **Test-&gt;Run-&gt;Tests in Current Context** menu command (or type Ctrl R, T) to run a subset of the unit tests.</span></span>

<span data-ttu-id="9ff76-160">讓我們我們 DinnerTest 類別內游標的位置，並輸入"Ctrl R、 T"來執行這兩個我們剛才定義的測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-160">Let's position our cursor within the DinnerTest class and type "Ctrl R, T" to run the two tests we just defined.</span></span> <span data-ttu-id="9ff76-161">當我們執行此動作的 [測試結果] 視窗會出現在 Visual Studio 中，我們會看到我們的測試回合中所列的結果：</span><span class="sxs-lookup"><span data-stu-id="9ff76-161">When we do this a "Test Results" window will appear within Visual Studio and we'll see the results of our test run listed within it:</span></span>

![](enable-automated-unit-testing/_static/image5.png)

<span data-ttu-id="9ff76-162">*注意： VS 測試結果 視窗不會預設顯示的類別名稱資料行。您可以將它加入以滑鼠右鍵按一下 測試結果 視窗內使用 新增/移除欄位 功能表命令。*</span><span class="sxs-lookup"><span data-stu-id="9ff76-162">*Note: The VS test results window does not show the Class Name column by default. You can add this by right-clicking within the Test Results window and using the Add/Remove Columns menu command.*</span></span>

<span data-ttu-id="9ff76-163">我們的兩個測試時間只有一小部分的第二個執行 –，並為您可以兩者都傳遞，請參閱。</span><span class="sxs-lookup"><span data-stu-id="9ff76-163">Our two tests took only a fraction of a second to run – and as you can see they both passed.</span></span> <span data-ttu-id="9ff76-164">我們現在可以在 上移，並建立其他的測試，確認特定的規則驗證，以及涵蓋兩個 helper 方法-IsUserHost() 和 IsUserRegisterd() – 我們新增至 Dinner 類別，藉此強化它們。</span><span class="sxs-lookup"><span data-stu-id="9ff76-164">We can now go on and augment them by creating additional tests that verify specific rule validations, as well as cover the two helper methods - IsUserHost() and IsUserRegisterd() – that we added to the Dinner class.</span></span> <span data-ttu-id="9ff76-165">備妥，Dinner 類別的所有這些測試可讓您更容易、 更安全地在未來將新的商務規則和驗證加入至它。</span><span class="sxs-lookup"><span data-stu-id="9ff76-165">Having all these tests in place for the Dinner class will make it much easier and safer to add new business rules and validations to it in the future.</span></span> <span data-ttu-id="9ff76-166">我們可以加入我們新的規則邏輯至 Dinner，，，然後在數秒內確認 它尚未中斷任何我們先前的邏輯功能。</span><span class="sxs-lookup"><span data-stu-id="9ff76-166">We can add our new rule logic to Dinner, and then within seconds verify that it hasn't broken any of our previous logic functionality.</span></span>

<span data-ttu-id="9ff76-167">請注意如何使用描述性的測試名稱容易快速了解每項測試會驗證。</span><span class="sxs-lookup"><span data-stu-id="9ff76-167">Notice how using a descriptive test name makes it easy to quickly understand what each test is verifying.</span></span> <span data-ttu-id="9ff76-168">我建議使用**工具-&gt;選項**功能表命令，開啟 測試工具-&gt;測試執行組態 畫面中，並檢查 「 按兩下失敗或結果不明的單元測試結果顯示在測試中的失敗點 」 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="9ff76-168">I recommend using the **Tools-&gt;Options** menu command, opening the Test Tools-&gt;Test Execution configuration screen, and checking the "Double-clicking a failed or inconclusive unit test result displays the point of failure in the test" checkbox.</span></span> <span data-ttu-id="9ff76-169">這可讓您按兩下測試結果 視窗中的失敗，並立即跳到判斷提示失敗。</span><span class="sxs-lookup"><span data-stu-id="9ff76-169">This will allow you to double-click on a failure in the test results window and jump immediately to the assert failure.</span></span>

### <a name="creating-dinnerscontroller-unit-tests"></a><span data-ttu-id="9ff76-170">建立 DinnersController 單元測試</span><span class="sxs-lookup"><span data-stu-id="9ff76-170">Creating DinnersController Unit Tests</span></span>

<span data-ttu-id="9ff76-171">我們現在來建立單元測試，確認我們 DinnersController 的功能。</span><span class="sxs-lookup"><span data-stu-id="9ff76-171">Let's now create some unit tests that verify our DinnersController functionality.</span></span> <span data-ttu-id="9ff76-172">我們將會啟動我們的測試專案中的 「 控制項 」 資料夾上按一下滑鼠右鍵，然後選擇  **Add-&gt;新的測試**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9ff76-172">We'll start by right-clicking on the "Controllers" folder within our Test project and then choose the **Add-&gt;New Test** menu command.</span></span> <span data-ttu-id="9ff76-173">我們將建立 「 單元測試 」 並將它命名為"DinnersControllerTest.cs 」。</span><span class="sxs-lookup"><span data-stu-id="9ff76-173">We'll create a "Unit Test" and name it "DinnersControllerTest.cs".</span></span>

<span data-ttu-id="9ff76-174">我們將建立確認 DinnersController Details() 動作方法的兩個測試方法。</span><span class="sxs-lookup"><span data-stu-id="9ff76-174">We'll create two test methods that verify the Details() action method on the DinnersController.</span></span> <span data-ttu-id="9ff76-175">第一個會確認現有的 Dinner 要求時，會傳回檢視。</span><span class="sxs-lookup"><span data-stu-id="9ff76-175">The first will verify that a View is returned when an existing Dinner is requested.</span></span> <span data-ttu-id="9ff76-176">第二個會確認不存在 Dinner 要求時，會傳回"NotFound"檢視：</span><span class="sxs-lookup"><span data-stu-id="9ff76-176">The second will verify that a "NotFound" view is returned when a non-existent Dinner is requested:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

<span data-ttu-id="9ff76-177">上述程式碼會清除編譯。</span><span class="sxs-lookup"><span data-stu-id="9ff76-177">The above code compiles clean.</span></span> <span data-ttu-id="9ff76-178">當我們執行測試時，不過，它們都失敗：</span><span class="sxs-lookup"><span data-stu-id="9ff76-178">When we run the tests, though, they both fail:</span></span>

![](enable-automated-unit-testing/_static/image6.png)

<span data-ttu-id="9ff76-179">如果我們查看錯誤訊息時，我們會看到測試失敗的原因是因為我們 DinnersRepository 類別無法連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="9ff76-179">If we look at the error messages, we'll see that the reason the tests failed was because our DinnersRepository class was unable to connect to a database.</span></span> <span data-ttu-id="9ff76-180">我們的 NerdDinner 應用程式連接字串使用本機的 SQL Server Express 檔案下 \App,\_NerdDinner 應用程式專案的資料目錄。</span><span class="sxs-lookup"><span data-stu-id="9ff76-180">Our NerdDinner application is using a connection-string to a local SQL Server Express file which lives under the \App\_Data directory of the NerdDinner application project.</span></span> <span data-ttu-id="9ff76-181">因為我們 NerdDinner.Tests 專案會編譯並執行在不同的目錄然後應用程式專案，我們連接字串的相對路徑位置不正確。</span><span class="sxs-lookup"><span data-stu-id="9ff76-181">Because our NerdDinner.Tests project compiles and runs in a different directory then the application project, the relative path location of our connection-string is incorrect.</span></span>

<span data-ttu-id="9ff76-182">我們*無法*修正此問題，將 SQL Express 資料庫檔案複製到我們的測試專案，然後加入適當的測試連接字串，在我們的測試專案的 App.config 中。</span><span class="sxs-lookup"><span data-stu-id="9ff76-182">We *could* fix this by copying the SQL Express database file to our test project, and then add an appropriate test connection-string to it in the App.config of our test project.</span></span> <span data-ttu-id="9ff76-183">這會取得上述解除鎖定，而且執行測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-183">This would get the above tests unblocked and running.</span></span>

<span data-ttu-id="9ff76-184">單元測試程式碼使用實際的資料庫，不過，帶來一些挑戰。</span><span class="sxs-lookup"><span data-stu-id="9ff76-184">Unit testing code using a real database, though, brings with it a number of challenges.</span></span> <span data-ttu-id="9ff76-185">尤其是：</span><span class="sxs-lookup"><span data-stu-id="9ff76-185">Specifically:</span></span>

- <span data-ttu-id="9ff76-186">大幅降低的單元測試執行時間。</span><span class="sxs-lookup"><span data-stu-id="9ff76-186">It significantly slows down the execution time of unit tests.</span></span> <span data-ttu-id="9ff76-187">時間越長，就能執行測試時，您會經常執行它們比較不可能。</span><span class="sxs-lookup"><span data-stu-id="9ff76-187">The longer it takes to run tests, the less likely you are to execute them frequently.</span></span> <span data-ttu-id="9ff76-188">在理想情況下，您會想要能夠執行的秒數 – 並讓它成為您這麼做當然做為編譯專案單元測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-188">Ideally you want your unit tests to be able to be run in seconds – and have it be something you do as naturally as compiling the project.</span></span>
- <span data-ttu-id="9ff76-189">它在測試設定和清除邏輯變得非常複雜。</span><span class="sxs-lookup"><span data-stu-id="9ff76-189">It complicates the setup and cleanup logic within tests.</span></span> <span data-ttu-id="9ff76-190">您想要做到隔離或獨立於其他 （沒有副作用或相依性） 的每個單元測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-190">You want each unit test to be isolated and independent of others (with no side effects or dependencies).</span></span> <span data-ttu-id="9ff76-191">使用時，針對實際資料庫，您必須留意的狀態，並測試之間進行重設。</span><span class="sxs-lookup"><span data-stu-id="9ff76-191">When working against a real database you have to be mindful of state and reset it between tests.</span></span>

<span data-ttu-id="9ff76-192">讓我們看看設計模式，稱為 「 相依性插入 」 可協助我們解決這些問題，並不需要與我們的測試中使用實際的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9ff76-192">Let's look at a design pattern called "dependency injection" that can help us work around these issues and avoid the need to use a real database with our tests.</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="9ff76-193">相依性插入</span><span class="sxs-lookup"><span data-stu-id="9ff76-193">Dependency Injection</span></span>

<span data-ttu-id="9ff76-194">現在 DinnersController 緊密 「 結合 」 DinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="9ff76-194">Right now DinnersController is tightly "coupled" to the DinnerRepository class.</span></span> <span data-ttu-id="9ff76-195">「 結合 」 指的是其中一個類別明確依賴另一個類別才能運作的情況：</span><span class="sxs-lookup"><span data-stu-id="9ff76-195">"Coupling" refers to a situation where a class explicitly relies on another class in order to work:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

<span data-ttu-id="9ff76-196">DinnerRepository 類別需要資料庫的存取權，因為緊密繫結的相依性就會有 DinnersController 類別具有 DinnerRepository 端點需要我們要測試的 DinnersController 動作方法的順序有一個資料庫中。</span><span class="sxs-lookup"><span data-stu-id="9ff76-196">Because the DinnerRepository class requires access to a database, the tightly coupled dependency the DinnersController class has on the DinnerRepository ends up requiring us to have a database in order for the DinnersController action methods to be tested.</span></span>

<span data-ttu-id="9ff76-197">我們可以採用的設計模式，稱為 「 相依性插入 」 – 這是一種方法 （例如提供資料存取的儲存機制類別） 的相依性會不再隱含建立的內使用它們的類別，取得解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="9ff76-197">We can get around this by employing a design pattern called "dependency injection" – which is an approach where dependencies (like repository classes that provide data access) are no longer implicitly created within classes that use them.</span></span> <span data-ttu-id="9ff76-198">相反地，相依性可以明確地傳遞至使用它們的類別使用建構函式的引數。</span><span class="sxs-lookup"><span data-stu-id="9ff76-198">Instead, dependencies can be explicitly passed to the class that uses them using constructor arguments.</span></span> <span data-ttu-id="9ff76-199">如果使用介面定義的相依性，然後我們將在 「 假 」 的相依性實作中進行單元測試案例的彈性。</span><span class="sxs-lookup"><span data-stu-id="9ff76-199">If the dependencies are defined using interfaces, we then have the flexibility to pass in "fake" dependency implementations for unit test scenarios.</span></span> <span data-ttu-id="9ff76-200">這可讓我們建立測試特定相依性的實作，實際上不需要資料庫的存取權。</span><span class="sxs-lookup"><span data-stu-id="9ff76-200">This enables us to create test-specific dependency implementations that do not actually require access to a database.</span></span>

<span data-ttu-id="9ff76-201">若要查看此動作，請讓我們來實作我們 DinnersController 使用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="9ff76-201">To see this in action, let's implement dependency injection with our DinnersController.</span></span>

#### <a name="extracting-an-idinnerrepository-interface"></a><span data-ttu-id="9ff76-202">擷取 IDinnerRepository 介面</span><span class="sxs-lookup"><span data-stu-id="9ff76-202">Extracting an IDinnerRepository interface</span></span>

<span data-ttu-id="9ff76-203">我們的第一個步驟是建立新封裝存放庫合約我們控制站需要以擷取和更新 Dinners IDinnerRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="9ff76-203">Our first step will be to create a new IDinnerRepository interface that encapsulates the repository contract our controllers require to retrieve and update Dinners.</span></span>

<span data-ttu-id="9ff76-204">我們可以手動定義此介面合約上 \Models 資料夾中，按一下滑鼠右鍵，然後選擇**Add-&gt;新的項目**功能表命令，並建立名為 IDinnerRepository.cs 新介面。</span><span class="sxs-lookup"><span data-stu-id="9ff76-204">We can define this interface contract manually by right-clicking on the \Models folder, and then choosing the **Add-&gt;New Item** menu command and creating a new interface named IDinnerRepository.cs.</span></span>

<span data-ttu-id="9ff76-205">或者我們可以使用重構工具內建於 Visual Studio Professional （及更新版本） 會自動擷取，並為我們建立介面，從現有 DinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="9ff76-205">Alternatively we can use the refactoring tools built-into Visual Studio Professional (and higher editions) to automatically extract and create an interface for us from our existing DinnerRepository class.</span></span> <span data-ttu-id="9ff76-206">若要擷取使用與此介面，只是將游標放在文字編輯器中 DinnerRepository 類別，然後以滑鼠右鍵按一下並選擇**重構-&gt;擷取介面**功能表命令：</span><span class="sxs-lookup"><span data-stu-id="9ff76-206">To extract this interface using VS, simply position the cursor in the text editor on the DinnerRepository class, and then right-click and choose the **Refactor-&gt;Extract Interface** menu command:</span></span>

![](enable-automated-unit-testing/_static/image7.png)

<span data-ttu-id="9ff76-207">這會啟動 「 擷取介面 」 對話方塊，並提示我們輸入要建立的介面名稱。</span><span class="sxs-lookup"><span data-stu-id="9ff76-207">This will launch the "Extract Interface" dialog and prompt us for the name of the interface to create.</span></span> <span data-ttu-id="9ff76-208">它會預設為 IDinnerRepository，並自動選取 現有 DinnerRepository 来加入的類別介面上的所有公用方法：</span><span class="sxs-lookup"><span data-stu-id="9ff76-208">It will default to IDinnerRepository and automatically select all public methods on the existing DinnerRepository class to add to the interface:</span></span>

![](enable-automated-unit-testing/_static/image8.png)

<span data-ttu-id="9ff76-209">當我們按一下 [確定] 按鈕時，Visual Studio 會加入我們的應用程式的新 IDinnerRepository 介面：</span><span class="sxs-lookup"><span data-stu-id="9ff76-209">When we click the "ok" button, Visual Studio will add a new IDinnerRepository interface to our application:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

<span data-ttu-id="9ff76-210">使其實作介面，將會更新現有 DinnerRepository 類別：</span><span class="sxs-lookup"><span data-stu-id="9ff76-210">And our existing DinnerRepository class will be updated so that it implements the interface:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a><span data-ttu-id="9ff76-211">更新以支援建構函式插入 DinnersController</span><span class="sxs-lookup"><span data-stu-id="9ff76-211">Updating DinnersController to support constructor injection</span></span>

<span data-ttu-id="9ff76-212">我們現在將更新 DinnersController 類別，以使用新的介面。</span><span class="sxs-lookup"><span data-stu-id="9ff76-212">We'll now update the DinnersController class to use the new interface.</span></span>

<span data-ttu-id="9ff76-213">目前 DinnersController 是硬式編碼，其 「 dinnerRepository"欄位一律為 DinnerRepository 類別：</span><span class="sxs-lookup"><span data-stu-id="9ff76-213">Currently DinnersController is hard-coded such that its "dinnerRepository" field is always a DinnerRepository class:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

<span data-ttu-id="9ff76-214">使 「 dinnerRepository"欄位的型別而不是 DinnerRepository IDinnerRepository，我們會將它變更。</span><span class="sxs-lookup"><span data-stu-id="9ff76-214">We'll change it so that the "dinnerRepository" field is of type IDinnerRepository instead of DinnerRepository.</span></span> <span data-ttu-id="9ff76-215">然後，我們將新增兩個公用 DinnersController 建構函式。</span><span class="sxs-lookup"><span data-stu-id="9ff76-215">We'll then add two public DinnersController constructors.</span></span> <span data-ttu-id="9ff76-216">其中一個建構函式可讓 IDinnerRepository 傳遞做為引數。</span><span class="sxs-lookup"><span data-stu-id="9ff76-216">One of the constructors allows an IDinnerRepository to be passed as an argument.</span></span> <span data-ttu-id="9ff76-217">另一個則是使用我們現有的 DinnerRepository 實作預設建構函式：</span><span class="sxs-lookup"><span data-stu-id="9ff76-217">The other is a default constructor that uses our existing DinnerRepository implementation:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

<span data-ttu-id="9ff76-218">ASP.NET MVC 預設會建立使用預設建構函式的控制器類別，因為我們 DinnersController 在執行階段將繼續使用 DinnerRepository 類別來執行資料存取。</span><span class="sxs-lookup"><span data-stu-id="9ff76-218">Because ASP.NET MVC by default creates controller classes using default constructors, our DinnersController at runtime will continue to use the DinnerRepository class to perform data access.</span></span>

<span data-ttu-id="9ff76-219">我們現在可以更新我們單元測試，不過，在使用參數的建構函式的 「 假 」 dinner 存放庫實作中傳遞。</span><span class="sxs-lookup"><span data-stu-id="9ff76-219">We can now update our unit tests, though, to pass in a "fake" dinner repository implementation using the parameter constructor.</span></span> <span data-ttu-id="9ff76-220">這個 「 假 」 dinner 存放庫不需要存取實際的資料庫，並改為將使用記憶體中的範例資料。</span><span class="sxs-lookup"><span data-stu-id="9ff76-220">This "fake" dinner repository will not require access to a real database, and instead will use in-memory sample data.</span></span>

#### <a name="creating-the-fakedinnerrepository-class"></a><span data-ttu-id="9ff76-221">建立 FakeDinnerRepository 類別</span><span class="sxs-lookup"><span data-stu-id="9ff76-221">Creating the FakeDinnerRepository class</span></span>

<span data-ttu-id="9ff76-222">讓我們建立 FakeDinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="9ff76-222">Let's create a FakeDinnerRepository class.</span></span>

<span data-ttu-id="9ff76-223">我們將開始先建立我們 NerdDinner.Tests 專案內的"Fakes"目錄，然後加入新的 FakeDinnerRepository 類別，(在資料夾上按一下滑鼠右鍵，然後選擇  **Add-&gt;新的類別**):</span><span class="sxs-lookup"><span data-stu-id="9ff76-223">We'll begin by creating a "Fakes" directory within our NerdDinner.Tests project and then add a new FakeDinnerRepository class to it (right-click on the folder and choose **Add-&gt;New Class**):</span></span>

![](enable-automated-unit-testing/_static/image9.png)

<span data-ttu-id="9ff76-224">我們將更新程式碼，以便 FakeDinnerRepository 類別會實作 IDinnerRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="9ff76-224">We'll update the code so that the FakeDinnerRepository class implements the IDinnerRepository interface.</span></span> <span data-ttu-id="9ff76-225">然後，我們可以在其上按一下滑鼠右鍵，並選擇 實作介面 IDinnerRepository 」 內容功能表命令：</span><span class="sxs-lookup"><span data-stu-id="9ff76-225">We can then right-click on it and choose the "Implement interface IDinnerRepository" context menu command:</span></span>

![](enable-automated-unit-testing/_static/image10.png)

<span data-ttu-id="9ff76-226">這會導致 Visual Studio 自動將所有 IDinnerRepository 介面成員新增至預設 「 虛設常式 」 實作 FakeDinnerRepository 類別：</span><span class="sxs-lookup"><span data-stu-id="9ff76-226">This will cause Visual Studio to automatically add all of the IDinnerRepository interface members to our FakeDinnerRepository class with default "stub out" implementations:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

<span data-ttu-id="9ff76-227">然後，我們可以更新 FakeDinnerRepository 實作，才能從記憶體中清單&lt;Dinner&gt;集合做為建構函式引數傳遞給它：</span><span class="sxs-lookup"><span data-stu-id="9ff76-227">We can then update the FakeDinnerRepository implementation to work off of an in-memory List&lt;Dinner&gt; collection passed to it as a constructor argument:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

<span data-ttu-id="9ff76-228">我們現在有假的 IDinnerRepository 實作不需要使用資料庫，並改為可關閉 Dinner 物件的記憶體中清單。</span><span class="sxs-lookup"><span data-stu-id="9ff76-228">We now have a fake IDinnerRepository implementation that does not require a database, and can instead work off an in-memory list of Dinner objects.</span></span>

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a><span data-ttu-id="9ff76-229">FakeDinnerRepository 使用單元測試</span><span class="sxs-lookup"><span data-stu-id="9ff76-229">Using the FakeDinnerRepository with Unit Tests</span></span>

<span data-ttu-id="9ff76-230">讓我們回到先前失敗，因為資料庫無法使用 DinnersController 單元測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-230">Let's return to the DinnersController unit tests that failed earlier because the database wasn't available.</span></span> <span data-ttu-id="9ff76-231">我們可以更新使用填入範例記憶體 Dinner 資料，以使用下列程式碼 DinnersController FakeDinnerRepository 測試方法：</span><span class="sxs-lookup"><span data-stu-id="9ff76-231">We can update the test methods to use a FakeDinnerRepository populated with sample in-memory Dinner data to the DinnersController using the code below:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

<span data-ttu-id="9ff76-232">然後，現在當我們執行這些測試都通過：</span><span class="sxs-lookup"><span data-stu-id="9ff76-232">And now when we run these tests they both pass:</span></span>

![](enable-automated-unit-testing/_static/image11.png)

<span data-ttu-id="9ff76-233">最棒的是，它們會採取只有一小部分來執行，第二個，而且不需要任何複雜的安裝/清除邏輯。</span><span class="sxs-lookup"><span data-stu-id="9ff76-233">Best of all, they take only a fraction of a second to run, and do not require any complicated setup/cleanup logic.</span></span> <span data-ttu-id="9ff76-234">我們現在可以將單元測試所有我們 DinnersController 動作方法的程式碼 （包括清單中，分頁的詳細資訊，建立、 更新和刪除） 不需要連線到實際的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9ff76-234">We can now unit test all of our DinnersController action method code (including listing, paging, details, create, update and delete) without ever needing to connect to a real database.</span></span>

| <span data-ttu-id="9ff76-235">**側邊的主題內容： 相依性插入架構**</span><span class="sxs-lookup"><span data-stu-id="9ff76-235">**Side Topic: Dependency Injection Frameworks**</span></span> |
| --- |
| <span data-ttu-id="9ff76-236">執行手動的相依性插入 （例如我們都以上） 可正常運作，但會成為難以維護的相依性，並增加應用程式中的元件。</span><span class="sxs-lookup"><span data-stu-id="9ff76-236">Performing manual dependency injection (like we are above) works fine, but does become harder to maintain as the number of dependencies and components in an application increases.</span></span> <span data-ttu-id="9ff76-237">適用於.NET 可協助提供更多的相依性管理彈性，存在數個相依性插入架構。</span><span class="sxs-lookup"><span data-stu-id="9ff76-237">Several dependency injection frameworks exist for .NET that can help provide even more dependency management flexibility.</span></span> <span data-ttu-id="9ff76-238">這些架構，有時也稱為 「 反向控制 」 (IoC) 容器，提供適當機制，讓其他組態的支援層級指定，並將相依性傳遞至物件在執行階段 （通常使用建構函式插入).</span><span class="sxs-lookup"><span data-stu-id="9ff76-238">These frameworks, also sometimes called "Inversion of Control" (IoC) containers, provide mechanisms that enable an additional level of configuration support for specifying and passing dependencies to objects at runtime (most often using constructor injection).</span></span> <span data-ttu-id="9ff76-239">一些較受歡迎的 OSS 相依性插入在.NET 中的 IOC 架構包含： AutoFac、 Ninject、 Spring.NET、 StructureMap 和 Windsor。</span><span class="sxs-lookup"><span data-stu-id="9ff76-239">Some of the more popular OSS Dependency Injection / IOC frameworks in .NET include: AutoFac, Ninject, Spring.NET, StructureMap, and Windsor.</span></span> <span data-ttu-id="9ff76-240">ASP.NET MVC 會公開擴充性 Api 可讓開發人員參與的解析度和具現化的控制站，並讓相依性插入 / IoC 架構，此處理序內完全整合。</span><span class="sxs-lookup"><span data-stu-id="9ff76-240">ASP.NET MVC exposes extensibility APIs that enable developers to participate in the resolution and instantiation of controllers, and which enables Dependency Injection / IoC frameworks to be cleanly integrated within this process.</span></span> <span data-ttu-id="9ff76-241">使用 DI/IOC 架構還能讓我們移除我們 DinnersController – 這會完全移除其與 DinnerRepositorys 結合的預設建構函式。</span><span class="sxs-lookup"><span data-stu-id="9ff76-241">Using a DI/IOC framework would also enable us to remove the default constructor from our DinnersController – which would completely remove the coupling between it and the DinnerRepositorys.</span></span> <span data-ttu-id="9ff76-242">我們將不會使用新的相依性插入 / IOC NerdDinner 應用程式架構。</span><span class="sxs-lookup"><span data-stu-id="9ff76-242">We won't be using a dependency injection / IOC framework with our NerdDinner application.</span></span> <span data-ttu-id="9ff76-243">但如果 NerdDinner 的程式碼基底和功能的成長，我們可以認為未來的某些項目。</span><span class="sxs-lookup"><span data-stu-id="9ff76-243">But it is something we could consider for the future if the NerdDinner code-base and capabilities grew.</span></span> |

### <a name="creating-edit-action-unit-tests"></a><span data-ttu-id="9ff76-244">建立編輯動作的單元測試</span><span class="sxs-lookup"><span data-stu-id="9ff76-244">Creating Edit Action Unit Tests</span></span>

<span data-ttu-id="9ff76-245">我們現在來建立單元測試驗證 DinnersController 的編輯功能。</span><span class="sxs-lookup"><span data-stu-id="9ff76-245">Let's now create some unit tests that verify the Edit functionality of the DinnersController.</span></span> <span data-ttu-id="9ff76-246">首先我們要測試我們的編輯動作的 HTTP GET 版本：</span><span class="sxs-lookup"><span data-stu-id="9ff76-246">We'll start by testing the HTTP-GET version of our Edit action:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

<span data-ttu-id="9ff76-247">我們將建立測試來驗證有效 dinner 要求時，轉譯 DinnerFormViewModel 物件所支援的檢視：</span><span class="sxs-lookup"><span data-stu-id="9ff76-247">We'll create a test that verifies that a View backed by a DinnerFormViewModel object is rendered back when a valid dinner is requested:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

<span data-ttu-id="9ff76-248">當我們執行測試時，不過，我們就會發現，它會失敗，因為編輯方法來存取執行 Dinner.IsHostedBy() 檢查 User.Identity.Name 屬性時，會擲回 null 參考例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9ff76-248">When we run the test, though, we'll find that it fails because a null reference exception is thrown when the Edit method accesses the User.Identity.Name property to perform the Dinner.IsHostedBy() check.</span></span>

<span data-ttu-id="9ff76-249">控制器的基底類別上的使用者物件封裝登入的使用者相關的詳細資訊，並在執行階段建立控制器時，會將 ASP.NET mvc 填入。</span><span class="sxs-lookup"><span data-stu-id="9ff76-249">The User object on the Controller base class encapsulates details about the logged-in user, and is populated by ASP.NET MVC when it creates the controller at runtime.</span></span> <span data-ttu-id="9ff76-250">因為我們要測試 web 伺服器環境之外 DinnersController，未設定的使用者物件 （因此 null 參考例外狀況）。</span><span class="sxs-lookup"><span data-stu-id="9ff76-250">Because we are testing the DinnersController outside of a web-server environment, the User object isn't set (hence the null reference exception).</span></span>

### <a name="mocking-the-useridentityname-property"></a><span data-ttu-id="9ff76-251">模擬 (mock) User.Identity.Name 屬性</span><span class="sxs-lookup"><span data-stu-id="9ff76-251">Mocking the User.Identity.Name property</span></span>

<span data-ttu-id="9ff76-252">（mock） 架構，讓測試更容易讓我們可以動態建立支援我們的測試中的相依性物件的假的版本。</span><span class="sxs-lookup"><span data-stu-id="9ff76-252">Mocking frameworks make testing easier by enabling us to dynamically create fake versions of dependent objects that support our tests.</span></span> <span data-ttu-id="9ff76-253">比方說，我們可以使用在我們的編輯動作測試的模擬架構，以動態方式建立使用者物件，我們 DinnersController 可用來查閱模擬的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="9ff76-253">For example, we can use a mocking framework in our Edit action test to dynamically create a User object that our DinnersController can use to lookup a simulated username.</span></span> <span data-ttu-id="9ff76-254">這可避免當我們執行我們的測試時擲回 null 參考。</span><span class="sxs-lookup"><span data-stu-id="9ff76-254">This will avoid a null reference from being thrown when we run our test.</span></span>

<span data-ttu-id="9ff76-255">有許多模擬 (mock) 架構，可以搭配 ASP.NET MVC 的.NET (您可以看到這些設定的清單如下： [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/))。</span><span class="sxs-lookup"><span data-stu-id="9ff76-255">There are many .NET mocking frameworks that can be used with ASP.NET MVC (you can see a list of them here: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)).</span></span> <span data-ttu-id="9ff76-256">為了測試我們的 NerdDinner 應用程式，我們將使用模擬 (mock) 架構，稱為 「 Moq"的開放原始碼，這可以免費從下載[ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq)。</span><span class="sxs-lookup"><span data-stu-id="9ff76-256">For testing our NerdDinner application we'll use an open source mocking framework called "Moq", which can be downloaded for free from [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).</span></span>

<span data-ttu-id="9ff76-257">下載完成後，我們會參考專案中加入我們 NerdDinner.Tests Moq.dll 組件：</span><span class="sxs-lookup"><span data-stu-id="9ff76-257">Once downloaded, we'll add a reference in our NerdDinner.Tests project to the Moq.dll assembly:</span></span>

![](enable-automated-unit-testing/_static/image12.png)

<span data-ttu-id="9ff76-258">然後我們會將 「 CreateDinnersControllerAs(username)"helper 方法加入我們的測試類別當作參數，以及其中的使用者名稱，然後 「 模擬 」 DinnersController 執行個體上的 User.Identity.Name 屬性：</span><span class="sxs-lookup"><span data-stu-id="9ff76-258">We'll then add a "CreateDinnersControllerAs(username)" helper method to our test class that takes a username as a parameter, and which then "mocks" the User.Identity.Name property on the DinnersController instance:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

<span data-ttu-id="9ff76-259">以上所述，我們會建立 fakes 的 ControllerContext 物件 （這是 ASP.NET MVC 會傳遞至控制器類別，來公開執行階段物件，例如使用者、 要求、 回應和工作階段） 的 Mock 物件使用 Moq。</span><span class="sxs-lookup"><span data-stu-id="9ff76-259">Above we are using Moq to create a Mock object that fakes a ControllerContext object (which is what ASP.NET MVC passes to Controller classes to expose runtime objects like User, Request, Response, and Session).</span></span> <span data-ttu-id="9ff76-260">我們會呼叫將 Mock 切換成指定的 ControllerContext HttpContext.User.Identity.Name 屬性應該會傳回我們傳遞至 helper 方法的使用者名稱字串"SetupGet 」 方法。</span><span class="sxs-lookup"><span data-stu-id="9ff76-260">We are calling the "SetupGet" method on the Mock to indicate that the HttpContext.User.Identity.Name property on ControllerContext should return the username string we passed to the helper method.</span></span>

<span data-ttu-id="9ff76-261">我們可以模擬任意數目的 ControllerContext 屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="9ff76-261">We can mock any number of ControllerContext properties and methods.</span></span> <span data-ttu-id="9ff76-262">為了說明這點我在裡面加入 SetupGet() 呼叫 Request.IsAuthenticated 屬性 （這實際上不需要針對下列測試 – 但幫助說明您可以模擬要求屬性的方式）。</span><span class="sxs-lookup"><span data-stu-id="9ff76-262">To illustrate this I've also added a SetupGet() call for the Request.IsAuthenticated property (which isn't actually needed for the tests below – but which helps illustrate how you can mock Request properties).</span></span> <span data-ttu-id="9ff76-263">當我們完成時我們會指派給我們的 helper 方法會傳回 DinnersController 的 ControllerContext mock 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="9ff76-263">When we are done we assign an instance of the ControllerContext mock to the DinnersController our helper method returns.</span></span>

<span data-ttu-id="9ff76-264">我們現在可以撰寫單元測試來測試編輯案例牽涉到不同的使用者使用這個 helper 方法：</span><span class="sxs-lookup"><span data-stu-id="9ff76-264">We can now write unit tests that use this helper method to test Edit scenarios involving different users:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

<span data-ttu-id="9ff76-265">然後，現在當我們執行測試時所傳遞：</span><span class="sxs-lookup"><span data-stu-id="9ff76-265">And now when we run the tests they pass:</span></span>

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a><span data-ttu-id="9ff76-266">測試 UpdateModel() 案例</span><span class="sxs-lookup"><span data-stu-id="9ff76-266">Testing UpdateModel() scenarios</span></span>

<span data-ttu-id="9ff76-267">我們建立了測試，涵蓋編輯動作的 HTTP GET 版本。</span><span class="sxs-lookup"><span data-stu-id="9ff76-267">We've created tests that cover the HTTP-GET version of the Edit action.</span></span> <span data-ttu-id="9ff76-268">我們現在來建立一些測試來驗證編輯動作的 HTTP POST 版本：</span><span class="sxs-lookup"><span data-stu-id="9ff76-268">Let's now create some tests that verify the HTTP-POST version of the Edit action:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

<span data-ttu-id="9ff76-269">讓我們支援與此動作方法的有趣新測試案例是 UpdateModel() helper 方法，控制站的基底類別上的使用方式。</span><span class="sxs-lookup"><span data-stu-id="9ff76-269">The interesting new testing scenario for us to support with this action method is its usage of the UpdateModel() helper method on the Controller base class.</span></span> <span data-ttu-id="9ff76-270">表單張貼值繫結至我們的 Dinner 物件執行個體，我們會使用這個 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="9ff76-270">We are using this helper method to bind form-post values to our Dinner object instance.</span></span>

<span data-ttu-id="9ff76-271">以下是兩項測試會示範如何，我們可以提供表單發佈 UpdateModel() 協助程式方法，若要使用的值。</span><span class="sxs-lookup"><span data-stu-id="9ff76-271">Below are two tests that demonstrates how we can supply form posted values for the UpdateModel() helper method to use.</span></span> <span data-ttu-id="9ff76-272">我們將建立並擴展 FormCollection 物件，執行這項操作，並再將它指派給 「 ValueProvider"屬性，控制站上。</span><span class="sxs-lookup"><span data-stu-id="9ff76-272">We'll do this by creating and populating a FormCollection object, and then assign it to the "ValueProvider" property on the Controller.</span></span>

<span data-ttu-id="9ff76-273">第一項測試會驗證，於成功儲存瀏覽器重新導向至詳細資料的動作。</span><span class="sxs-lookup"><span data-stu-id="9ff76-273">The first test verifies that on a successful save the browser is redirected to the details action.</span></span> <span data-ttu-id="9ff76-274">第二項測試會驗證，張貼無效的輸入時的動作會重新顯示一次編輯檢視並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="9ff76-274">The second test verifies that when invalid input is posted the action redisplays the edit view again with an error message.</span></span>


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a><span data-ttu-id="9ff76-275">測試總結</span><span class="sxs-lookup"><span data-stu-id="9ff76-275">Testing Wrap-Up</span></span>

<span data-ttu-id="9ff76-276">我們所討論的單元測試的控制器類別中所涉及的核心概念。</span><span class="sxs-lookup"><span data-stu-id="9ff76-276">We've covered the core concepts involved in unit testing controller classes.</span></span> <span data-ttu-id="9ff76-277">我們可以使用這些技術人員輕鬆地建立數百個簡單的測試來驗證我們的應用程式的行為。</span><span class="sxs-lookup"><span data-stu-id="9ff76-277">We can use these techniques to easily create hundreds of simple tests that verify the behavior of our application.</span></span>

<span data-ttu-id="9ff76-278">因為我們的控制器和測試模型不需要實際的資料庫，也就是極度快速且輕鬆地執行。</span><span class="sxs-lookup"><span data-stu-id="9ff76-278">Because our controller and model tests do not require a real database, they are extremely fast and easy to run.</span></span> <span data-ttu-id="9ff76-279">我們可以執行數百個自動化測試，以秒為單位，並立即取得有關我們所做的變更是否中斷內容的意見反應。</span><span class="sxs-lookup"><span data-stu-id="9ff76-279">We'll be able to execute hundreds of automated tests in seconds, and immediately get feedback as to whether a change we made broke something.</span></span> <span data-ttu-id="9ff76-280">這將協助提供我們持續改善，重構，並改進我們的應用程式的信心。</span><span class="sxs-lookup"><span data-stu-id="9ff76-280">This will help provide us the confidence to continually improve, refactor, and refine our application.</span></span>

<span data-ttu-id="9ff76-281">我們涵蓋測試與最後一個主題，在這一章 – 但不是因為測試是您應該在開發程序結束 ！</span><span class="sxs-lookup"><span data-stu-id="9ff76-281">We covered testing as the last topic in this chapter – but not because testing is something you should do at the end of a development process!</span></span> <span data-ttu-id="9ff76-282">相反地，您應該在開發程序中盡早撰寫自動化的測試。</span><span class="sxs-lookup"><span data-stu-id="9ff76-282">On the contrary, you should write automated tests as early as possible in your development process.</span></span> <span data-ttu-id="9ff76-283">這麼做因此可讓您取得立即意見反應開發時，可協助您填寫完成思考您的應用程式使用案例，並引導您設計您的應用程式清除分層，並記住。</span><span class="sxs-lookup"><span data-stu-id="9ff76-283">Doing so enables you to get immediate feedback as you develop, helps you think thoughtfully about your application's use case scenarios, and guides you to design your application with clean layering and coupling in mind.</span></span>

<span data-ttu-id="9ff76-284">活頁簿中的稍後章節會討論測試導向開發 (TDD)，以及如何使用它來搭配 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="9ff76-284">A later chapter in the book will discuss Test Driven Development (TDD), and how to use it with ASP.NET MVC.</span></span> <span data-ttu-id="9ff76-285">TDD 是反覆執行的程式碼撰寫慣例，先撰寫的測試，都能符合您產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ff76-285">TDD is an iterative coding practice where you first write the tests that your resulting code will satisfy.</span></span> <span data-ttu-id="9ff76-286">使用 TDD 中，您會開始每項功能所建立的測試，確認您要實作的功能。</span><span class="sxs-lookup"><span data-stu-id="9ff76-286">With TDD you begin each feature by creating a test that verifies the functionality you are about to implement.</span></span> <span data-ttu-id="9ff76-287">撰寫單元測試前可協助確保您清楚了解此功能，以及應該如何運作。</span><span class="sxs-lookup"><span data-stu-id="9ff76-287">Writing the unit test first helps ensure that you clearly understand the feature and how it is supposed to work.</span></span> <span data-ttu-id="9ff76-288">只寫入測試 （並確認它失敗之後） 執行您，然後實作實際功能測試會驗證。</span><span class="sxs-lookup"><span data-stu-id="9ff76-288">Only after the test is written (and you have verified that it fails) do you then implement the actual functionality the test verifies.</span></span> <span data-ttu-id="9ff76-289">因為您已花時間來思考運作的功能有何的使用案例，您會有進一步了解需求與如何加以實作。</span><span class="sxs-lookup"><span data-stu-id="9ff76-289">Because you've already spent time thinking about the use case of how the feature is supposed to work, you will have a better understanding of the requirements and how best to implement them.</span></span> <span data-ttu-id="9ff76-290">當您完成的實作，您可以重新執行測試工作，並取得立即意見反應，而時是否功能正常運作。</span><span class="sxs-lookup"><span data-stu-id="9ff76-290">When you are done with the implementation you can re-run the test – and get immediate feedback as to whether the feature works correctly.</span></span> <span data-ttu-id="9ff76-291">我們將討論更多在第 10 章 TDD。</span><span class="sxs-lookup"><span data-stu-id="9ff76-291">We'll cover TDD more in Chapter 10.</span></span>

### <a name="next-step"></a><span data-ttu-id="9ff76-292">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="9ff76-292">Next Step</span></span>

<span data-ttu-id="9ff76-293">註解一些最終總結。</span><span class="sxs-lookup"><span data-stu-id="9ff76-293">Some final wrap up comments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9ff76-294">[上一頁](use-ajax-to-implement-mapping-scenarios.md)
> [下一頁](nerddinner-wrap-up.md)</span><span class="sxs-lookup"><span data-stu-id="9ff76-294">[Previous](use-ajax-to-implement-mapping-scenarios.md)
[Next](nerddinner-wrap-up.md)</span></span>
