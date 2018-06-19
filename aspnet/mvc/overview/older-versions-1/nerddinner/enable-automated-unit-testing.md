---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: 啟用自動化單元測試 |Microsoft 文件
author: microsoft
description: 步驟 12 示範如何開發自動化的單元測試可確認我們 NerdDinner 功能，並讓我們進行變更的信心套件...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: fede08be7e06327c6d04fa5d36f7dd818d79b380
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875626"
---
<a name="enable-automated-unit-testing"></a><span data-ttu-id="8d6af-103">啟用自動化的單元測試</span><span class="sxs-lookup"><span data-stu-id="8d6af-103">Enable Automated Unit Testing</span></span>
====================
<span data-ttu-id="8d6af-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8d6af-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8d6af-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="8d6af-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="8d6af-106">這是一套免費的步驟 12 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="8d6af-106">This is step 12 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="8d6af-107">步驟 12 示範如何開發可確認我們 NerdDinner 功能，並讓我們進行的變更與改進未來的應用程式的信心自動化的單元測試套件。</span><span class="sxs-lookup"><span data-stu-id="8d6af-107">Step 12 shows how to develop a suite of automated unit tests that verify our NerdDinner functionality, and which will give us the confidence to make changes and improvements to the application in the future.</span></span>
> 
> <span data-ttu-id="8d6af-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="8d6af-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-12-unit-testing"></a><span data-ttu-id="8d6af-109">NerdDinner 步驟 12： 單元測試</span><span class="sxs-lookup"><span data-stu-id="8d6af-109">NerdDinner Step 12: Unit Testing</span></span>

<span data-ttu-id="8d6af-110">讓我們來開發自動化的單元測試可確認我們 NerdDinner 功能，並讓我們進行的變更與改進未來的應用程式的信心套件。</span><span class="sxs-lookup"><span data-stu-id="8d6af-110">Let's develop a suite of automated unit tests that verify our NerdDinner functionality, and which will give us the confidence to make changes and improvements to the application in the future.</span></span>

### <a name="why-unit-test"></a><span data-ttu-id="8d6af-111">單元測試為何？</span><span class="sxs-lookup"><span data-stu-id="8d6af-111">Why Unit Test?</span></span>

<span data-ttu-id="8d6af-112">一個早上到磁碟機上，您必須依據您進行應用程式的相關的靈感突然 flash。</span><span class="sxs-lookup"><span data-stu-id="8d6af-112">On the drive into work one morning you have a sudden flash of inspiration about an application you are working on.</span></span> <span data-ttu-id="8d6af-113">您了解變更您可以實作，可讓應用程式大幅更好。</span><span class="sxs-lookup"><span data-stu-id="8d6af-113">You realize there is a change you can implement that will make the application dramatically better.</span></span> <span data-ttu-id="8d6af-114">可能是重構的清除程式碼中，加入了新的功能，或修正 bug。</span><span class="sxs-lookup"><span data-stu-id="8d6af-114">It might be a refactoring that cleans up the code, adds a new feature, or fixes a bug.</span></span>

<span data-ttu-id="8d6af-115">Confronts 您，當您抵達電腦的問題是 – 「 安全方式是，使這項改進？ 」</span><span class="sxs-lookup"><span data-stu-id="8d6af-115">The question that confronts you when you arrive at your computer is – "how safe is it to make this improvement?"</span></span> <span data-ttu-id="8d6af-116">如果進行變更的副作用或是中斷的項目嗎？</span><span class="sxs-lookup"><span data-stu-id="8d6af-116">What if making the change has side effects or breaks something?</span></span> <span data-ttu-id="8d6af-117">變更可能會很簡單，而且只需要幾分鐘才能實作，但是如果需要手動測試的所有應用程式案例的時數？</span><span class="sxs-lookup"><span data-stu-id="8d6af-117">The change might be simple and only take a few minutes to implement, but what if it takes hours to manually test out all of the application scenarios?</span></span> <span data-ttu-id="8d6af-118">如果您忘記涵蓋的案例並中斷應用程式進入生產嗎？</span><span class="sxs-lookup"><span data-stu-id="8d6af-118">What if you forget to cover a scenario and a broken application goes into production?</span></span> <span data-ttu-id="8d6af-119">正在進行這項改進真的值得所有投入時間？</span><span class="sxs-lookup"><span data-stu-id="8d6af-119">Is making this improvement really worth all the effort?</span></span>

<span data-ttu-id="8d6af-120">自動化的單元測試可以提供一種安全措施，可讓您持續增強您的應用程式，並避免在您處理的程式碼的。</span><span class="sxs-lookup"><span data-stu-id="8d6af-120">Automated unit tests can provide a safety net that enables you to continually enhance your applications, and avoid being afraid of the code you are working on.</span></span> <span data-ttu-id="8d6af-121">執行具有自動化測試，以快速驗證功能可讓您有信心地-程式碼，並讓您可讓您可能會否則不具有認為舒適的改善。</span><span class="sxs-lookup"><span data-stu-id="8d6af-121">Having automated tests that quickly verify functionality enables you to code with confidence – and empower you to make improvements you might otherwise not have felt comfortable doing.</span></span> <span data-ttu-id="8d6af-122">它們也可以協助建立更容易維護的方案，並以較長的存留期的投資報酬率的將導致更高。</span><span class="sxs-lookup"><span data-stu-id="8d6af-122">They also help create solutions that are more maintainable and have a longer lifetime - which leads to a much higher return on investment.</span></span>

<span data-ttu-id="8d6af-123">ASP.NET MVC 架構可讓您輕鬆自然的單元測試應用程式功能。</span><span class="sxs-lookup"><span data-stu-id="8d6af-123">The ASP.NET MVC Framework makes it easy and natural to unit test application functionality.</span></span> <span data-ttu-id="8d6af-124">它也可讓測試驅動開發 (TDD) 工作流程，可讓 「 測試先行 」 開發。</span><span class="sxs-lookup"><span data-stu-id="8d6af-124">It also enables a Test Driven Development (TDD) workflow that enables test-first based development.</span></span>

### <a name="nerddinnertests-project"></a><span data-ttu-id="8d6af-125">NerdDinner.Tests Project</span><span class="sxs-lookup"><span data-stu-id="8d6af-125">NerdDinner.Tests Project</span></span>

<span data-ttu-id="8d6af-126">當我們在本教學課程的開頭建立我們 NerdDinner 應用程式時，我們已收到提示對話方塊，詢問是否我們想要建立單元測試專案中，移以及應用程式專案：</span><span class="sxs-lookup"><span data-stu-id="8d6af-126">When we created our NerdDinner application at the beginning of this tutorial, we were prompted with a dialog asking whether we wanted to create a unit test project to go along with the application project:</span></span>

![](enable-automated-unit-testing/_static/image1.png)

<span data-ttu-id="8d6af-127">我們保留 「 [是]，建立單元測試專案 」 選取的選項按鈕-而產生了 「 NerdDinner.Tests 」 專案加入我們的解決方案：</span><span class="sxs-lookup"><span data-stu-id="8d6af-127">We kept the "Yes, create a unit test project" radio button selected – which resulted in a "NerdDinner.Tests" project being added to our solution:</span></span>

![](enable-automated-unit-testing/_static/image2.png)

<span data-ttu-id="8d6af-128">NerdDinner.Tests 專案參考 NerdDinner 應用程式專案的組件，並可讓我們以輕鬆地加入自動化的測試，確認應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="8d6af-128">The NerdDinner.Tests project references the NerdDinner application project assembly, and enables us to easily add automated tests to it that verify the application functionality.</span></span>

### <a name="creating-unit-tests-for-our-dinner-model-class"></a><span data-ttu-id="8d6af-129">建立單元測試，我們 Dinner 模型類別</span><span class="sxs-lookup"><span data-stu-id="8d6af-129">Creating Unit Tests for our Dinner Model Class</span></span>

<span data-ttu-id="8d6af-130">讓我們加入一些測試以確認我們建置我們模型層時，我們建立的 Dinner 類別我們 NerdDinner.Tests 專案。</span><span class="sxs-lookup"><span data-stu-id="8d6af-130">Let's add some tests to our NerdDinner.Tests project that verify the Dinner class we created when we built our model layer.</span></span>

<span data-ttu-id="8d6af-131">我們會先建立新資料夾內我們測試專案，稱為 「 模型 」 會放置在我們與模型相關的測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-131">We'll start by creating a new folder within our test project called "Models" where we'll place our model-related tests.</span></span> <span data-ttu-id="8d6af-132">我們將在資料夾上按一下滑鼠右鍵，然後選擇**新增-&gt;新測試**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="8d6af-132">We'll then right-click on the folder and choose the **Add-&gt;New Test** menu command.</span></span> <span data-ttu-id="8d6af-133">這會顯示 「 加入新測試 」 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="8d6af-133">This will bring up the "Add New Test" dialog.</span></span>

<span data-ttu-id="8d6af-134">我們將選擇要建立 「 單元測試 」 並將它命名為"DinnerTest.cs 」:</span><span class="sxs-lookup"><span data-stu-id="8d6af-134">We'll choose to create a "Unit Test" and name it "DinnerTest.cs":</span></span>

![](enable-automated-unit-testing/_static/image3.png)

<span data-ttu-id="8d6af-135">當我們按一下 「 確定 」 按鈕 Visual Studio 會加入 （會開啟） DinnerTest.cs 檔案加入專案中：</span><span class="sxs-lookup"><span data-stu-id="8d6af-135">When we click the "ok" button Visual Studio will add (and open) a DinnerTest.cs file to the project:</span></span>

![](enable-automated-unit-testing/_static/image4.png)

<span data-ttu-id="8d6af-136">預設 Visual Studio 單元測試範本有一大堆鍋爐調色盤中的程式碼它找到有點混亂。</span><span class="sxs-lookup"><span data-stu-id="8d6af-136">The default Visual Studio unit test template has a bunch of boiler-plate code within it that I find a little messy.</span></span> <span data-ttu-id="8d6af-137">讓我們來清除它只包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="8d6af-137">Let's clean it up to just contain the code below:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

<span data-ttu-id="8d6af-138">在上述的 DinnerTest 類別 [由 TestClass] 屬性識別為類別將包含測試，以及選擇性的測試初始化和清除程式碼。</span><span class="sxs-lookup"><span data-stu-id="8d6af-138">The [TestClass] attribute on the DinnerTest class above identifies it as a class that will contain tests, as well as optional test initialization and teardown code.</span></span> <span data-ttu-id="8d6af-139">我們可以透過將在其有 [TestMethod] 屬性的公用方法加入定義內的測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-139">We can define tests within it by adding public methods that have a [TestMethod] attribute on them.</span></span>

<span data-ttu-id="8d6af-140">以下是兩個測試我們會將新增 Dinner 類別中的第一個。</span><span class="sxs-lookup"><span data-stu-id="8d6af-140">Below are the first of two tests we'll add that exercise our Dinner class.</span></span> <span data-ttu-id="8d6af-141">第一項測試會驗證我們 Dinner 不正確的是否沒有正確設定的所有內容建立新的 Dinner。</span><span class="sxs-lookup"><span data-stu-id="8d6af-141">The first test verifies that our Dinner is invalid if a new Dinner is created without all properties being set correctly.</span></span> <span data-ttu-id="8d6af-142">第二個測試確認我們 Dinner 有效 Dinner 擁有的所有屬性都具有有效的值設定時：</span><span class="sxs-lookup"><span data-stu-id="8d6af-142">The second test verifies that our Dinner is valid when a Dinner has all properties set with valid values:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

<span data-ttu-id="8d6af-143">您會發現上方，我們的測試名稱都是非常明確 （和比較的詳細資訊）。</span><span class="sxs-lookup"><span data-stu-id="8d6af-143">You'll notice above that our test names are very explicit (and somewhat verbose).</span></span> <span data-ttu-id="8d6af-144">因為我們最後可能會建立數百或數千個小型測試中，而且我們想要讓您輕鬆快速判斷每一個函數的行為與意圖 （特別是我們會透過一份測試執行器中的失敗） 時，我們會這樣做。</span><span class="sxs-lookup"><span data-stu-id="8d6af-144">We are doing this because we might end up creating hundreds or thousands of small tests, and we want to make it easy to quickly determine the intent and behavior of each of them (especially when we are looking through a list of failures in a test runner).</span></span> <span data-ttu-id="8d6af-145">測試名稱應為名他們所測試的功能。</span><span class="sxs-lookup"><span data-stu-id="8d6af-145">The test names should be named after the functionality they are testing.</span></span> <span data-ttu-id="8d6af-146">我們將使用以上所述"名詞\_應該\_指令動詞"的命名模式。</span><span class="sxs-lookup"><span data-stu-id="8d6af-146">Above we are using a "Noun\_Should\_Verb" naming pattern.</span></span>

<span data-ttu-id="8d6af-147">我們會建構使用"AAA"測試模式 – 代表 「 排列、 動作、 判斷提示 」 的測試：</span><span class="sxs-lookup"><span data-stu-id="8d6af-147">We are structuring the tests using the "AAA" testing pattern – which stands for "Arrange, Act, Assert":</span></span>

- <span data-ttu-id="8d6af-148">排列： 安裝程式的單元測試</span><span class="sxs-lookup"><span data-stu-id="8d6af-148">Arrange: Setup the unit being tested</span></span>
- <span data-ttu-id="8d6af-149">Act： 執行的單元測試和擷取結果</span><span class="sxs-lookup"><span data-stu-id="8d6af-149">Act: Exercise the unit under test and capture results</span></span>
- <span data-ttu-id="8d6af-150">判斷提示： 驗證的行為</span><span class="sxs-lookup"><span data-stu-id="8d6af-150">Assert: Verify the behavior</span></span>

<span data-ttu-id="8d6af-151">當我們撰寫我們想要避免讓個別測試的測試執行太多。</span><span class="sxs-lookup"><span data-stu-id="8d6af-151">When we write tests we want to avoid having the individual tests do too much.</span></span> <span data-ttu-id="8d6af-152">改為每個測試應該確認只有一個單一的概念 （如此可讓您更容易找出失敗的原因）。</span><span class="sxs-lookup"><span data-stu-id="8d6af-152">Instead each test should verify only a single concept (which will make it much easier to pinpoint the cause of failures).</span></span> <span data-ttu-id="8d6af-153">最好是嘗試只能有單一 assert 陳述式，針對每個測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-153">A good guideline is to try and only have a single assert statement for each test.</span></span> <span data-ttu-id="8d6af-154">如果您有一個以上的判斷提示陳述式的測試方法中，確定它們所有要用來測試概念相同。</span><span class="sxs-lookup"><span data-stu-id="8d6af-154">If you have more than one assert statement in a test method, make sure they are all being used to test the same concept.</span></span> <span data-ttu-id="8d6af-155">當不確定，請在另一項測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-155">When in doubt, make another test.</span></span>

### <a name="running-tests"></a><span data-ttu-id="8d6af-156">正在執行測試</span><span class="sxs-lookup"><span data-stu-id="8d6af-156">Running Tests</span></span>

<span data-ttu-id="8d6af-157">Visual Studio 2008 Professional （和更高的版本） 包含可以用來執行 Visual Studio 單元測試專案在 IDE 中的內建的測試執行器。</span><span class="sxs-lookup"><span data-stu-id="8d6af-157">Visual Studio 2008 Professional (and higher editions) includes a built-in test runner that can be used to run Visual Studio Unit Test projects within the IDE.</span></span> <span data-ttu-id="8d6af-158">我們可以從中選取**Test-&gt;執行-&gt;方案中的所有測試**功能表命令 （或類型為 Ctrl R、 A） 執行所有的單元測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-158">We can select the **Test-&gt;Run-&gt;All Tests in Solution** menu command (or type Ctrl R, A) to run all of our unit tests.</span></span> <span data-ttu-id="8d6af-159">或者我們可以在特定的測試類別或測試方法中我們游標的位置，並使用**Test-&gt;執行-&gt;目前內容中的測試**功能表命令 （或型別 Ctrl R、 T） 來執行單元測試的子集。</span><span class="sxs-lookup"><span data-stu-id="8d6af-159">Or alternatively we can position our cursor within a specific test class or test method and use the **Test-&gt;Run-&gt;Tests in Current Context** menu command (or type Ctrl R, T) to run a subset of the unit tests.</span></span>

<span data-ttu-id="8d6af-160">讓我們我們 DinnerTest 類別內游標的位置，然後輸入 「 Ctrl R、 T 「 我們剛才定義的兩個執行測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-160">Let's position our cursor within the DinnerTest class and type "Ctrl R, T" to run the two tests we just defined.</span></span> <span data-ttu-id="8d6af-161">當我們執行這個動作 「 測試結果 」 視窗會出現在 Visual Studio 中，我們會看到我們的測試回合中所列的結果：</span><span class="sxs-lookup"><span data-stu-id="8d6af-161">When we do this a "Test Results" window will appear within Visual Studio and we'll see the results of our test run listed within it:</span></span>

![](enable-automated-unit-testing/_static/image5.png)

<span data-ttu-id="8d6af-162">*注意： VS 測試結果視窗未顯示類別名稱資料行的預設值。您可以將它加入以滑鼠右鍵按一下 [測試結果] 視窗中使用 [新增/移除欄位] 功能表命令。*</span><span class="sxs-lookup"><span data-stu-id="8d6af-162">*Note: The VS test results window does not show the Class Name column by default. You can add this by right-clicking within the Test Results window and using the Add/Remove Columns menu command.*</span></span>

<span data-ttu-id="8d6af-163">我們兩項測試所需的第二個執行 –，並為您可以輕鬆擁有兩者傳遞，請參閱。</span><span class="sxs-lookup"><span data-stu-id="8d6af-163">Our two tests took only a fraction of a second to run – and as you can see they both passed.</span></span> <span data-ttu-id="8d6af-164">我們現在可以繼續，並加強它們藉由建立額外的測試，確認特定的規則驗證，以及涵蓋兩個 helper 方法-IsUserHost() 和 IsUserRegisterd() – 我們新增至 Dinner 類別。</span><span class="sxs-lookup"><span data-stu-id="8d6af-164">We can now go on and augment them by creating additional tests that verify specific rule validations, as well as cover the two helper methods - IsUserHost() and IsUserRegisterd() – that we added to the Dinner class.</span></span> <span data-ttu-id="8d6af-165">就地 Dinner 類別具有所有這些測試會讓更容易，未來對其加入新的商務規則和驗證更安全。</span><span class="sxs-lookup"><span data-stu-id="8d6af-165">Having all these tests in place for the Dinner class will make it much easier and safer to add new business rules and validations to it in the future.</span></span> <span data-ttu-id="8d6af-166">我們可以將我們新的規則邏輯加入至 Dinner，，，然後在數秒內檢查 它尚未中斷任何我們先前的邏輯功能。</span><span class="sxs-lookup"><span data-stu-id="8d6af-166">We can add our new rule logic to Dinner, and then within seconds verify that it hasn't broken any of our previous logic functionality.</span></span>

<span data-ttu-id="8d6af-167">請注意如何使用描述性的測試名稱讓您輕鬆地快速了解每項測試會驗證。</span><span class="sxs-lookup"><span data-stu-id="8d6af-167">Notice how using a descriptive test name makes it easy to quickly understand what each test is verifying.</span></span> <span data-ttu-id="8d6af-168">建議您使用**Tools-&gt;選項**功能表命令，開啟的測試工具-&gt;測試執行組態 畫面中，並檢查 「 按兩下失敗或結果不明的單元測試結果顯示測試失敗的點 」 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="8d6af-168">I recommend using the **Tools-&gt;Options** menu command, opening the Test Tools-&gt;Test Execution configuration screen, and checking the "Double-clicking a failed or inconclusive unit test result displays the point of failure in the test" checkbox.</span></span> <span data-ttu-id="8d6af-169">這可讓您在失敗的測試結果視窗中按兩下，然後立即跳到判斷提示失敗。</span><span class="sxs-lookup"><span data-stu-id="8d6af-169">This will allow you to double-click on a failure in the test results window and jump immediately to the assert failure.</span></span>

### <a name="creating-dinnerscontroller-unit-tests"></a><span data-ttu-id="8d6af-170">建立 DinnersController 單元測試</span><span class="sxs-lookup"><span data-stu-id="8d6af-170">Creating DinnersController Unit Tests</span></span>

<span data-ttu-id="8d6af-171">我們現在來建立驗證我們 DinnersController 功能有單元測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-171">Let's now create some unit tests that verify our DinnersController functionality.</span></span> <span data-ttu-id="8d6af-172">我們將會啟動 「 控制項 」 資料夾，在我們的測試專案上按一下滑鼠右鍵，然後選擇 **新增-&gt;新測試**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="8d6af-172">We'll start by right-clicking on the "Controllers" folder within our Test project and then choose the **Add-&gt;New Test** menu command.</span></span> <span data-ttu-id="8d6af-173">我們會建立 「 單元測試 」 並將它命名為"DinnersControllerTest.cs"。</span><span class="sxs-lookup"><span data-stu-id="8d6af-173">We'll create a "Unit Test" and name it "DinnersControllerTest.cs".</span></span>

<span data-ttu-id="8d6af-174">我們將建立確認 DinnersController Details() 動作方法的兩個測試方法。</span><span class="sxs-lookup"><span data-stu-id="8d6af-174">We'll create two test methods that verify the Details() action method on the DinnersController.</span></span> <span data-ttu-id="8d6af-175">第一個會確認現有的 Dinner 要求時傳回的檢視。</span><span class="sxs-lookup"><span data-stu-id="8d6af-175">The first will verify that a View is returned when an existing Dinner is requested.</span></span> <span data-ttu-id="8d6af-176">第二個會確認不存在 Dinner 要求時，會傳回"NotFound"檢視：</span><span class="sxs-lookup"><span data-stu-id="8d6af-176">The second will verify that a "NotFound" view is returned when a non-existent Dinner is requested:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

<span data-ttu-id="8d6af-177">上述程式碼會編譯初始狀態。</span><span class="sxs-lookup"><span data-stu-id="8d6af-177">The above code compiles clean.</span></span> <span data-ttu-id="8d6af-178">當我們執行測試時，不過，它們都失敗：</span><span class="sxs-lookup"><span data-stu-id="8d6af-178">When we run the tests, though, they both fail:</span></span>

![](enable-automated-unit-testing/_static/image6.png)

<span data-ttu-id="8d6af-179">如果我們查看錯誤訊息時，我們會看到測試失敗的原因，是因為我們 DinnersRepository 類別無法連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="8d6af-179">If we look at the error messages, we'll see that the reason the tests failed was because our DinnersRepository class was unable to connect to a database.</span></span> <span data-ttu-id="8d6af-180">我們 NerdDinner 應用程式使用連接字串的本機 SQL Server Express 檔案存留在 \App\_NerdDinner 應用程式專案的資料目錄。</span><span class="sxs-lookup"><span data-stu-id="8d6af-180">Our NerdDinner application is using a connection-string to a local SQL Server Express file which lives under the \App\_Data directory of the NerdDinner application project.</span></span> <span data-ttu-id="8d6af-181">因為我們 NerdDinner.Tests 專案編譯並執行於不同資料夾然後應用程式專案，我們連接字串的相對路徑位置不正確。</span><span class="sxs-lookup"><span data-stu-id="8d6af-181">Because our NerdDinner.Tests project compiles and runs in a different directory then the application project, the relative path location of our connection-string is incorrect.</span></span>

<span data-ttu-id="8d6af-182">我們*無法*修正此問題，SQL Express 資料庫檔案複製到測試專案中，並將適當的測試連接字串到它在我們的測試專案的 App.config 中。</span><span class="sxs-lookup"><span data-stu-id="8d6af-182">We *could* fix this by copying the SQL Express database file to our test project, and then add an appropriate test connection-string to it in the App.config of our test project.</span></span> <span data-ttu-id="8d6af-183">這會取得解除鎖定，而且執行上述測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-183">This would get the above tests unblocked and running.</span></span>

<span data-ttu-id="8d6af-184">單元測試程式碼使用實際的資料庫，不過，會帶來一些挑戰。</span><span class="sxs-lookup"><span data-stu-id="8d6af-184">Unit testing code using a real database, though, brings with it a number of challenges.</span></span> <span data-ttu-id="8d6af-185">尤其是：</span><span class="sxs-lookup"><span data-stu-id="8d6af-185">Specifically:</span></span>

- <span data-ttu-id="8d6af-186">它會明顯變慢的單元測試執行時間。</span><span class="sxs-lookup"><span data-stu-id="8d6af-186">It significantly slows down the execution time of unit tests.</span></span> <span data-ttu-id="8d6af-187">越長它會執行測試時，您會經常執行它們比較不可能。</span><span class="sxs-lookup"><span data-stu-id="8d6af-187">The longer it takes to run tests, the less likely you are to execute them frequently.</span></span> <span data-ttu-id="8d6af-188">在理想情況下，您會想要能夠執行的秒數 –，並讓它是您這麼做為自然編譯專案為您的單元測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-188">Ideally you want your unit tests to be able to be run in seconds – and have it be something you do as naturally as compiling the project.</span></span>
- <span data-ttu-id="8d6af-189">它的設定和清除的邏輯中測試變得非常複雜。</span><span class="sxs-lookup"><span data-stu-id="8d6af-189">It complicates the setup and cleanup logic within tests.</span></span> <span data-ttu-id="8d6af-190">您想要隔離的 （具有沒有副作用或相依性） 獨立於其他每個單元測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-190">You want each unit test to be isolated and independent of others (with no side effects or dependencies).</span></span> <span data-ttu-id="8d6af-191">針對實際資料庫使用時您必須留意的狀態重設測試之間。</span><span class="sxs-lookup"><span data-stu-id="8d6af-191">When working against a real database you have to be mindful of state and reset it between tests.</span></span>

<span data-ttu-id="8d6af-192">讓我們看看稱為 「 相依性插入 」 可協助我們解決這些問題，並不需要使用我們的測試與實際資料庫設計模式。</span><span class="sxs-lookup"><span data-stu-id="8d6af-192">Let's look at a design pattern called "dependency injection" that can help us work around these issues and avoid the need to use a real database with our tests.</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="8d6af-193">相依性插入</span><span class="sxs-lookup"><span data-stu-id="8d6af-193">Dependency Injection</span></span>

<span data-ttu-id="8d6af-194">現在 DinnersController 緊密 」 結合 「 DinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="8d6af-194">Right now DinnersController is tightly "coupled" to the DinnerRepository class.</span></span> <span data-ttu-id="8d6af-195">「 連接 」 指的是其中一個類別明確依賴另一個類別才能運作的情況下：</span><span class="sxs-lookup"><span data-stu-id="8d6af-195">"Coupling" refers to a situation where a class explicitly relies on another class in order to work:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

<span data-ttu-id="8d6af-196">因為 DinnerRepository 類別需要資料庫的存取權，緊密繫結的相依性 DinnersController 類別對我們要測試的 DinnersController 動作方法的順序有資料庫需要 DinnerRepository 結束。</span><span class="sxs-lookup"><span data-stu-id="8d6af-196">Because the DinnerRepository class requires access to a database, the tightly coupled dependency the DinnersController class has on the DinnerRepository ends up requiring us to have a database in order for the DinnersController action methods to be tested.</span></span>

<span data-ttu-id="8d6af-197">我們可以採用稱為 「 相依性插入 」 – 這是一個方法相依性 （例如提供資料存取的儲存機制類別） 不再隱含建立的位置使用它們的類別內的設計模式，取得解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="8d6af-197">We can get around this by employing a design pattern called "dependency injection" – which is an approach where dependencies (like repository classes that provide data access) are no longer implicitly created within classes that use them.</span></span> <span data-ttu-id="8d6af-198">相反地，相依性可以明確地傳遞給使用它們的類別使用建構函式的引數。</span><span class="sxs-lookup"><span data-stu-id="8d6af-198">Instead, dependencies can be explicitly passed to the class that uses them using constructor arguments.</span></span> <span data-ttu-id="8d6af-199">如果使用介面定義的相依性，然後我們更有彈性地在單元測試案例的 「 詐騙 」 的相依性實作中傳遞。</span><span class="sxs-lookup"><span data-stu-id="8d6af-199">If the dependencies are defined using interfaces, we then have the flexibility to pass in "fake" dependency implementations for unit test scenarios.</span></span> <span data-ttu-id="8d6af-200">這可讓我們來建立測試特定相依性的實作，實際上不需要資料庫的存取權。</span><span class="sxs-lookup"><span data-stu-id="8d6af-200">This enables us to create test-specific dependency implementations that do not actually require access to a database.</span></span>

<span data-ttu-id="8d6af-201">若要查看此動作，讓我們實作使用我們 DinnersController 相依性插入。</span><span class="sxs-lookup"><span data-stu-id="8d6af-201">To see this in action, let's implement dependency injection with our DinnersController.</span></span>

#### <a name="extracting-an-idinnerrepository-interface"></a><span data-ttu-id="8d6af-202">擷取 IDinnerRepository 介面</span><span class="sxs-lookup"><span data-stu-id="8d6af-202">Extracting an IDinnerRepository interface</span></span>

<span data-ttu-id="8d6af-203">我們第一個步驟是建立新封裝儲存機制合約我們控制站需要以擷取和更新 Dinners IDinnerRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="8d6af-203">Our first step will be to create a new IDinnerRepository interface that encapsulates the repository contract our controllers require to retrieve and update Dinners.</span></span>

<span data-ttu-id="8d6af-204">我們可以手動定義此介面合約 \Models 資料夾上按一下滑鼠右鍵，然後選擇**新增-&gt;新項目**功能表命令，並建立名為 IDinnerRepository.cs 新介面。</span><span class="sxs-lookup"><span data-stu-id="8d6af-204">We can define this interface contract manually by right-clicking on the \Models folder, and then choosing the **Add-&gt;New Item** menu command and creating a new interface named IDinnerRepository.cs.</span></span>

<span data-ttu-id="8d6af-205">或者我們可以使用重構作業工具內建 Visual Studio Professional （及更高的版本） 會自動擷取，並為我們建立介面，從現有 DinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="8d6af-205">Alternatively we can use the refactoring tools built-into Visual Studio Professional (and higher editions) to automatically extract and create an interface for us from our existing DinnerRepository class.</span></span> <span data-ttu-id="8d6af-206">若要擷取使用 VS 這個介面，只要將游標放在文字編輯器中 DinnerRepository 類別，然後以滑鼠右鍵按一下並選擇**重構-&gt;擷取介面**功能表命令：</span><span class="sxs-lookup"><span data-stu-id="8d6af-206">To extract this interface using VS, simply position the cursor in the text editor on the DinnerRepository class, and then right-click and choose the **Refactor-&gt;Extract Interface** menu command:</span></span>

![](enable-automated-unit-testing/_static/image7.png)

<span data-ttu-id="8d6af-207">這將會啟動 「 擷取介面 」 對話方塊，並提示我們要建立的介面名稱。</span><span class="sxs-lookup"><span data-stu-id="8d6af-207">This will launch the "Extract Interface" dialog and prompt us for the name of the interface to create.</span></span> <span data-ttu-id="8d6af-208">它會預設為 IDinnerRepository 並自動選取要加入至介面的現有 DinnerRepository 類別上的所有公用方法：</span><span class="sxs-lookup"><span data-stu-id="8d6af-208">It will default to IDinnerRepository and automatically select all public methods on the existing DinnerRepository class to add to the interface:</span></span>

![](enable-automated-unit-testing/_static/image8.png)

<span data-ttu-id="8d6af-209">當我們按一下 「 確定 」 按鈕時，則 Visual Studio 會將新 IDinnerRepository 介面加入我們的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8d6af-209">When we click the "ok" button, Visual Studio will add a new IDinnerRepository interface to our application:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

<span data-ttu-id="8d6af-210">然後，以便實作介面，就會更新現有 DinnerRepository 類別：</span><span class="sxs-lookup"><span data-stu-id="8d6af-210">And our existing DinnerRepository class will be updated so that it implements the interface:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a><span data-ttu-id="8d6af-211">更新以支援建構函式插入 DinnersController</span><span class="sxs-lookup"><span data-stu-id="8d6af-211">Updating DinnersController to support constructor injection</span></span>

<span data-ttu-id="8d6af-212">我們現在會將更新 DinnersController 類別，以使用新的介面。</span><span class="sxs-lookup"><span data-stu-id="8d6af-212">We'll now update the DinnersController class to use the new interface.</span></span>

<span data-ttu-id="8d6af-213">目前 DinnersController 是硬式編碼，例如其"dinnerRepository 」 欄位一律會 DinnerRepository 類別：</span><span class="sxs-lookup"><span data-stu-id="8d6af-213">Currently DinnersController is hard-coded such that its "dinnerRepository" field is always a DinnerRepository class:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

<span data-ttu-id="8d6af-214">我們將會使"dinnerRepository 」 欄位的類型而不是 DinnerRepository IDinnerRepository 變更它。</span><span class="sxs-lookup"><span data-stu-id="8d6af-214">We'll change it so that the "dinnerRepository" field is of type IDinnerRepository instead of DinnerRepository.</span></span> <span data-ttu-id="8d6af-215">然後，我們會新增兩個公用 DinnersController 建構函式。</span><span class="sxs-lookup"><span data-stu-id="8d6af-215">We'll then add two public DinnersController constructors.</span></span> <span data-ttu-id="8d6af-216">其中一個建構函式可讓 IDinnerRepository 傳遞做為引數。</span><span class="sxs-lookup"><span data-stu-id="8d6af-216">One of the constructors allows an IDinnerRepository to be passed as an argument.</span></span> <span data-ttu-id="8d6af-217">另一個方法是使用我們現有的 DinnerRepository 實作預設建構函式：</span><span class="sxs-lookup"><span data-stu-id="8d6af-217">The other is a default constructor that uses our existing DinnerRepository implementation:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

<span data-ttu-id="8d6af-218">因為預設的 ASP.NET MVC 建立控制器類別使用預設建構函式，所以我們 DinnersController 在執行階段將繼續使用 DinnerRepository 類別來執行資料存取。</span><span class="sxs-lookup"><span data-stu-id="8d6af-218">Because ASP.NET MVC by default creates controller classes using default constructors, our DinnersController at runtime will continue to use the DinnerRepository class to perform data access.</span></span>

<span data-ttu-id="8d6af-219">我們現在可以更新我們單元測試，不過，在使用參數的建構函式的 「 假"dinner 儲存機制實作中傳遞。</span><span class="sxs-lookup"><span data-stu-id="8d6af-219">We can now update our unit tests, though, to pass in a "fake" dinner repository implementation using the parameter constructor.</span></span> <span data-ttu-id="8d6af-220">這個 「 假"dinner 儲存機制不需要存取實際的資料庫，並會改為使用記憶體中的範例資料。</span><span class="sxs-lookup"><span data-stu-id="8d6af-220">This "fake" dinner repository will not require access to a real database, and instead will use in-memory sample data.</span></span>

#### <a name="creating-the-fakedinnerrepository-class"></a><span data-ttu-id="8d6af-221">建立 FakeDinnerRepository 類別</span><span class="sxs-lookup"><span data-stu-id="8d6af-221">Creating the FakeDinnerRepository class</span></span>

<span data-ttu-id="8d6af-222">讓我們來建立 FakeDinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="8d6af-222">Let's create a FakeDinnerRepository class.</span></span>

<span data-ttu-id="8d6af-223">我們將開始建立我們 NerdDinner.Tests 專案內的"Fakes"目錄，然後為其新增新的 FakeDinnerRepository 類別 (在資料夾上按一下滑鼠右鍵，然後選擇 **新增-&gt;新類別**):</span><span class="sxs-lookup"><span data-stu-id="8d6af-223">We'll begin by creating a "Fakes" directory within our NerdDinner.Tests project and then add a new FakeDinnerRepository class to it (right-click on the folder and choose **Add-&gt;New Class**):</span></span>

![](enable-automated-unit-testing/_static/image9.png)

<span data-ttu-id="8d6af-224">我們將更新程式碼，以便讓 FakeDinnerRepository 類別實作 IDinnerRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="8d6af-224">We'll update the code so that the FakeDinnerRepository class implements the IDinnerRepository interface.</span></span> <span data-ttu-id="8d6af-225">我們可以在其上按一下滑鼠右鍵，然後選擇 「 實作介面 IDinnerRepository 」 的操作功能表命令：</span><span class="sxs-lookup"><span data-stu-id="8d6af-225">We can then right-click on it and choose the "Implement interface IDinnerRepository" context menu command:</span></span>

![](enable-automated-unit-testing/_static/image10.png)

<span data-ttu-id="8d6af-226">這會導致 Visual Studio 會自動將所有 IDinnerRepository 介面成員新增至預設 「 虛設常式 」 實作我們 FakeDinnerRepository 類別：</span><span class="sxs-lookup"><span data-stu-id="8d6af-226">This will cause Visual Studio to automatically add all of the IDinnerRepository interface members to our FakeDinnerRepository class with default "stub out" implementations:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

<span data-ttu-id="8d6af-227">然後，我們可以更新 FakeDinnerRepository 實作，才能從記憶體中清單&lt;Dinner&gt;集合做為建構函式引數傳遞給它：</span><span class="sxs-lookup"><span data-stu-id="8d6af-227">We can then update the FakeDinnerRepository implementation to work off of an in-memory List&lt;Dinner&gt; collection passed to it as a constructor argument:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

<span data-ttu-id="8d6af-228">我們現在有假的 IDinnerRepository 實作，不需要使用資料庫，並可以改為使用關閉的 Dinner 物件之記憶體中清單。</span><span class="sxs-lookup"><span data-stu-id="8d6af-228">We now have a fake IDinnerRepository implementation that does not require a database, and can instead work off an in-memory list of Dinner objects.</span></span>

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a><span data-ttu-id="8d6af-229">使用單元測試的 FakeDinnerRepository</span><span class="sxs-lookup"><span data-stu-id="8d6af-229">Using the FakeDinnerRepository with Unit Tests</span></span>

<span data-ttu-id="8d6af-230">讓我們回到稍早失敗，因為資料庫無法使用 DinnersController 單元測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-230">Let's return to the DinnersController unit tests that failed earlier because the database wasn't available.</span></span> <span data-ttu-id="8d6af-231">我們可以更新使用填入範例記憶體 Dinner 資料以使用下列程式碼 DinnersController FakeDinnerRepository 的測試方法：</span><span class="sxs-lookup"><span data-stu-id="8d6af-231">We can update the test methods to use a FakeDinnerRepository populated with sample in-memory Dinner data to the DinnersController using the code below:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

<span data-ttu-id="8d6af-232">現在當我們執行這些測試都成功：</span><span class="sxs-lookup"><span data-stu-id="8d6af-232">And now when we run these tests they both pass:</span></span>

![](enable-automated-unit-testing/_static/image11.png)

<span data-ttu-id="8d6af-233">最棒的他們採取只有執行，第二個部分，而且不需要任何複雜的安裝/清除邏輯。</span><span class="sxs-lookup"><span data-stu-id="8d6af-233">Best of all, they take only a fraction of a second to run, and do not require any complicated setup/cleanup logic.</span></span> <span data-ttu-id="8d6af-234">我們現在即可以單元測試所有我們 DinnersController 動作方法的程式碼 （包括清單中，分頁的詳細資訊，建立、 更新及刪除） 不需要連接到實際的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8d6af-234">We can now unit test all of our DinnersController action method code (including listing, paging, details, create, update and delete) without ever needing to connect to a real database.</span></span>

| <span data-ttu-id="8d6af-235">**側邊主題內容： 相依性插入的架構**</span><span class="sxs-lookup"><span data-stu-id="8d6af-235">**Side Topic: Dependency Injection Frameworks**</span></span> |
| --- |
| <span data-ttu-id="8d6af-236">執行手動的相依性插入 （例如我們都以上） 可正常運作，但沒有變得難以維護的相依性數量為和應用程式中的元件會增加。</span><span class="sxs-lookup"><span data-stu-id="8d6af-236">Performing manual dependency injection (like we are above) works fine, but does become harder to maintain as the number of dependencies and components in an application increases.</span></span> <span data-ttu-id="8d6af-237">可協助提供更多的相依性管理彈性的.NET 存在數個相依性插入的架構。</span><span class="sxs-lookup"><span data-stu-id="8d6af-237">Several dependency injection frameworks exist for .NET that can help provide even more dependency management flexibility.</span></span> <span data-ttu-id="8d6af-238">這些架構，有時也稱為 「 的逆轉控制 」 (IoC) 容器提供的一層額外的組態支援指定並傳遞要在執行階段 （最常使用的建構函式插入的物件相依性).</span><span class="sxs-lookup"><span data-stu-id="8d6af-238">These frameworks, also sometimes called "Inversion of Control" (IoC) containers, provide mechanisms that enable an additional level of configuration support for specifying and passing dependencies to objects at runtime (most often using constructor injection).</span></span> <span data-ttu-id="8d6af-239">某些更常用的 OSS 相依性插入 / IOC 架構在.NET 中的包括： AutoFac、 Ninject、 Spring.NET、 StructureMap 和 Windsor。</span><span class="sxs-lookup"><span data-stu-id="8d6af-239">Some of the more popular OSS Dependency Injection / IOC frameworks in .NET include: AutoFac, Ninject, Spring.NET, StructureMap, and Windsor.</span></span> <span data-ttu-id="8d6af-240">ASP.NET MVC 會公開擴充性 Api 可讓開發人員參與解析度和具現化的控制站，並讓相依性插入 / IoC 架構以完全整合，此處理序內。</span><span class="sxs-lookup"><span data-stu-id="8d6af-240">ASP.NET MVC exposes extensibility APIs that enable developers to participate in the resolution and instantiation of controllers, and which enables Dependency Injection / IoC frameworks to be cleanly integrated within this process.</span></span> <span data-ttu-id="8d6af-241">使用 DI/IOC 架構也會讓我們能夠移除我們 DinnersController – 會完全移除它和 DinnerRepositorys 之間的連接中的預設建構函式。</span><span class="sxs-lookup"><span data-stu-id="8d6af-241">Using a DI/IOC framework would also enable us to remove the default constructor from our DinnersController – which would completely remove the coupling between it and the DinnerRepositorys.</span></span> <span data-ttu-id="8d6af-242">我們將不會使用相依性插入 / IOC 架構與我們 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d6af-242">We won't be using a dependency injection / IOC framework with our NerdDinner application.</span></span> <span data-ttu-id="8d6af-243">但是，如果成長 NerdDinner 程式碼基底和功能，我們可以考慮針對未來的某些項目。</span><span class="sxs-lookup"><span data-stu-id="8d6af-243">But it is something we could consider for the future if the NerdDinner code-base and capabilities grew.</span></span> |

### <a name="creating-edit-action-unit-tests"></a><span data-ttu-id="8d6af-244">建立編輯動作的單元測試</span><span class="sxs-lookup"><span data-stu-id="8d6af-244">Creating Edit Action Unit Tests</span></span>

<span data-ttu-id="8d6af-245">我們現在來建立驗證 DinnersController 的編輯功能有單元測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-245">Let's now create some unit tests that verify the Edit functionality of the DinnersController.</span></span> <span data-ttu-id="8d6af-246">首先我們要測試 HTTP-GET 版本我們編輯動作：</span><span class="sxs-lookup"><span data-stu-id="8d6af-246">We'll start by testing the HTTP-GET version of our Edit action:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

<span data-ttu-id="8d6af-247">我們將建立測試，以確認有效 dinner 要求時，會轉譯 DinnerFormViewModel 物件所支援的檢視：</span><span class="sxs-lookup"><span data-stu-id="8d6af-247">We'll create a test that verifies that a View backed by a DinnerFormViewModel object is rendered back when a valid dinner is requested:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

<span data-ttu-id="8d6af-248">當我們執行測試時，不過，我們就會發現，它會失敗，因為編輯方法來存取執行 Dinner.IsHostedBy() 檢查 User.Identity.Name 屬性時，會擲回 null 參考例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8d6af-248">When we run the test, though, we'll find that it fails because a null reference exception is thrown when the Edit method accesses the User.Identity.Name property to perform the Dinner.IsHostedBy() check.</span></span>

<span data-ttu-id="8d6af-249">控制器的基底類別上的使用者物件封裝之登入之使用者的相關詳細資訊，ASP.NET MVC 控制器建立在執行階段時填入。</span><span class="sxs-lookup"><span data-stu-id="8d6af-249">The User object on the Controller base class encapsulates details about the logged-in user, and is populated by ASP.NET MVC when it creates the controller at runtime.</span></span> <span data-ttu-id="8d6af-250">因為我們要測試 web 伺服器環境之外 DinnersController，未設定的使用者物件 （因此 null 參考例外狀況）。</span><span class="sxs-lookup"><span data-stu-id="8d6af-250">Because we are testing the DinnersController outside of a web-server environment, the User object isn't set (hence the null reference exception).</span></span>

### <a name="mocking-the-useridentityname-property"></a><span data-ttu-id="8d6af-251">模擬 User.Identity.Name 屬性</span><span class="sxs-lookup"><span data-stu-id="8d6af-251">Mocking the User.Identity.Name property</span></span>

<span data-ttu-id="8d6af-252">模擬架構會讓測試更為容易讓我們以動態方式建立假支援我們的測試中的相依性物件的版本。</span><span class="sxs-lookup"><span data-stu-id="8d6af-252">Mocking frameworks make testing easier by enabling us to dynamically create fake versions of dependent objects that support our tests.</span></span> <span data-ttu-id="8d6af-253">例如，我們可以在我們的編輯動作測試使用模擬架構，以動態方式建立我們 DinnersController 可以用來查閱模擬的使用者名稱的使用者物件。</span><span class="sxs-lookup"><span data-stu-id="8d6af-253">For example, we can use a mocking framework in our Edit action test to dynamically create a User object that our DinnersController can use to lookup a simulated username.</span></span> <span data-ttu-id="8d6af-254">這樣可避免從我們執行我們的測試時，所擲回的 null 參考。</span><span class="sxs-lookup"><span data-stu-id="8d6af-254">This will avoid a null reference from being thrown when we run our test.</span></span>

<span data-ttu-id="8d6af-255">有許多.NET 模擬可以搭配 ASP.NET MVC 的架構 (您可以看到這些設定的清單這裡： [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/))。</span><span class="sxs-lookup"><span data-stu-id="8d6af-255">There are many .NET mocking frameworks that can be used with ASP.NET MVC (you can see a list of them here: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)).</span></span> <span data-ttu-id="8d6af-256">為了測試，我們會使用模擬架構，稱為 「 Moq"的開放原始碼我們 NerdDinner 應用程式，它可以免費下載從[ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq)。</span><span class="sxs-lookup"><span data-stu-id="8d6af-256">For testing our NerdDinner application we'll use an open source mocking framework called "Moq", which can be downloaded for free from [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).</span></span>

<span data-ttu-id="8d6af-257">在下載後，我們會參考專案中加入我們 NerdDinner.Tests Moq.dll 組件：</span><span class="sxs-lookup"><span data-stu-id="8d6af-257">Once downloaded, we'll add a reference in our NerdDinner.Tests project to the Moq.dll assembly:</span></span>

![](enable-automated-unit-testing/_static/image12.png)

<span data-ttu-id="8d6af-258">然後我們會將"CreateDinnersControllerAs(username)"helper 方法加入我們的測試類別會做為參數，以及其使用者名稱，然後 「 mocks"DinnersController 執行個體上的 User.Identity.Name 屬性：</span><span class="sxs-lookup"><span data-stu-id="8d6af-258">We'll then add a "CreateDinnersControllerAs(username)" helper method to our test class that takes a username as a parameter, and which then "mocks" the User.Identity.Name property on the DinnersController instance:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

<span data-ttu-id="8d6af-259">上方我們使用 Moq 建立 fakes 的 ControllerContext 物件 （這是 ASP.NET MVC 會傳遞至控制器類別公開的執行階段物件，例如使用者、 要求、 回應和工作階段） 模擬物件。</span><span class="sxs-lookup"><span data-stu-id="8d6af-259">Above we are using Moq to create a Mock object that fakes a ControllerContext object (which is what ASP.NET MVC passes to Controller classes to expose runtime objects like User, Request, Response, and Session).</span></span> <span data-ttu-id="8d6af-260">我們正在撥打"SetupGet"方法上表示的 ControllerContext HttpContext.User.Identity.Name 屬性應該傳回我們傳遞到 helper 方法的使用者名稱字串模擬。</span><span class="sxs-lookup"><span data-stu-id="8d6af-260">We are calling the "SetupGet" method on the Mock to indicate that the HttpContext.User.Identity.Name property on ControllerContext should return the username string we passed to the helper method.</span></span>

<span data-ttu-id="8d6af-261">我們可以模擬任何數目的 ControllerContext 屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="8d6af-261">We can mock any number of ControllerContext properties and methods.</span></span> <span data-ttu-id="8d6af-262">為了說明這點我也新增了 SetupGet() 呼叫 Request.IsAuthenticated 屬性 （這不實際需要下列 – 測試，但前者可協助說明您可以模擬要求屬性的方式）。</span><span class="sxs-lookup"><span data-stu-id="8d6af-262">To illustrate this I've also added a SetupGet() call for the Request.IsAuthenticated property (which isn't actually needed for the tests below – but which helps illustrate how you can mock Request properties).</span></span> <span data-ttu-id="8d6af-263">當我們完成我們會指派給我們的 helper 方法會傳回 DinnersController 的 ControllerContext 模擬的執行個體。</span><span class="sxs-lookup"><span data-stu-id="8d6af-263">When we are done we assign an instance of the ControllerContext mock to the DinnersController our helper method returns.</span></span>

<span data-ttu-id="8d6af-264">現在我們可以撰寫單元測試，使用此 helper 方法來測試編輯案例牽涉到不同的使用者：</span><span class="sxs-lookup"><span data-stu-id="8d6af-264">We can now write unit tests that use this helper method to test Edit scenarios involving different users:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

<span data-ttu-id="8d6af-265">現在我們執行測試時傳遞：</span><span class="sxs-lookup"><span data-stu-id="8d6af-265">And now when we run the tests they pass:</span></span>

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a><span data-ttu-id="8d6af-266">測試 UpdateModel() 案例</span><span class="sxs-lookup"><span data-stu-id="8d6af-266">Testing UpdateModel() scenarios</span></span>

<span data-ttu-id="8d6af-267">我們建立了涵蓋編輯動作的 HTTP GET 版本的測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-267">We've created tests that cover the HTTP-GET version of the Edit action.</span></span> <span data-ttu-id="8d6af-268">我們現在來建立一些測試，確認編輯動作的 HTTP POST 版本：</span><span class="sxs-lookup"><span data-stu-id="8d6af-268">Let's now create some tests that verify the HTTP-POST version of the Edit action:</span></span>

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

<span data-ttu-id="8d6af-269">我們支援與此動作方法的有趣新測試案例是 UpdateModel() helper 方法，控制站的基底類別上的使用方式。</span><span class="sxs-lookup"><span data-stu-id="8d6af-269">The interesting new testing scenario for us to support with this action method is its usage of the UpdateModel() helper method on the Controller base class.</span></span> <span data-ttu-id="8d6af-270">使用這個 helper 方法來將表單張貼值繫結至我們 Dinner 物件執行個體。</span><span class="sxs-lookup"><span data-stu-id="8d6af-270">We are using this helper method to bind form-post values to our Dinner object instance.</span></span>

<span data-ttu-id="8d6af-271">以下是示範如何才能提供表單張貼 UpdateModel() helper 方法，若要使用的值的兩個測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-271">Below are two tests that demonstrates how we can supply form posted values for the UpdateModel() helper method to use.</span></span> <span data-ttu-id="8d6af-272">我們會建立並擴展 FormCollection 物件，執行這項操作，然後將它指派給 「 ValueProvider"屬性，控制站上。</span><span class="sxs-lookup"><span data-stu-id="8d6af-272">We'll do this by creating and populating a FormCollection object, and then assign it to the "ValueProvider" property on the Controller.</span></span>

<span data-ttu-id="8d6af-273">第一項測試會驗證，在成功儲存瀏覽器會重新導向至詳細資料的動作。</span><span class="sxs-lookup"><span data-stu-id="8d6af-273">The first test verifies that on a successful save the browser is redirected to the details action.</span></span> <span data-ttu-id="8d6af-274">第二個測試會驗證當張貼無效的輸入時的動作又再次編輯檢視並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8d6af-274">The second test verifies that when invalid input is posted the action redisplays the edit view again with an error message.</span></span>


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a><span data-ttu-id="8d6af-275">測試 Wrap-Up</span><span class="sxs-lookup"><span data-stu-id="8d6af-275">Testing Wrap-Up</span></span>

<span data-ttu-id="8d6af-276">我們已涵蓋參與單元測試控制器類別的的核心概念。</span><span class="sxs-lookup"><span data-stu-id="8d6af-276">We've covered the core concepts involved in unit testing controller classes.</span></span> <span data-ttu-id="8d6af-277">我們可以使用這些技術輕鬆地建立數百個簡單的測試，確認我們的應用程式的行為。</span><span class="sxs-lookup"><span data-stu-id="8d6af-277">We can use these techniques to easily create hundreds of simple tests that verify the behavior of our application.</span></span>

<span data-ttu-id="8d6af-278">我們的控制器和測試模型不需要實際的資料庫，因為它們是非常快速且容易執行。</span><span class="sxs-lookup"><span data-stu-id="8d6af-278">Because our controller and model tests do not require a real database, they are extremely fast and easy to run.</span></span> <span data-ttu-id="8d6af-279">我們將能夠以秒為單位，執行數百個自動化測試，並立即取得回應我們所做的變更是否中斷的項目。</span><span class="sxs-lookup"><span data-stu-id="8d6af-279">We'll be able to execute hundreds of automated tests in seconds, and immediately get feedback as to whether a change we made broke something.</span></span> <span data-ttu-id="8d6af-280">這將協助提供我們以持續改善，重構及改善我們的應用程式的信心。</span><span class="sxs-lookup"><span data-stu-id="8d6af-280">This will help provide us the confidence to continually improve, refactor, and refine our application.</span></span>

<span data-ttu-id="8d6af-281">我們涵蓋測試做為最後一個主題在本章中 – 但不是因為測試是您應該執行結尾的開發程序 ！</span><span class="sxs-lookup"><span data-stu-id="8d6af-281">We covered testing as the last topic in this chapter – but not because testing is something you should do at the end of a development process!</span></span> <span data-ttu-id="8d6af-282">相反地，您應該在開發程序中盡早撰寫自動化的測試。</span><span class="sxs-lookup"><span data-stu-id="8d6af-282">On the contrary, you should write automated tests as early as possible in your development process.</span></span> <span data-ttu-id="8d6af-283">這樣做因此可讓您取得的立即回應您開發，可協助您周全思考應用程式的使用案例，並引導您設計您的應用程式分層和耦合記住初始狀態。</span><span class="sxs-lookup"><span data-stu-id="8d6af-283">Doing so enables you to get immediate feedback as you develop, helps you think thoughtfully about your application's use case scenarios, and guides you to design your application with clean layering and coupling in mind.</span></span>

<span data-ttu-id="8d6af-284">活頁簿中稍後的章節將討論測試驅動開發 (TDD)，以及如何使用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="8d6af-284">A later chapter in the book will discuss Test Driven Development (TDD), and how to use it with ASP.NET MVC.</span></span> <span data-ttu-id="8d6af-285">TDD 反覆編碼作法是先撰寫測試產生的程式碼將滿足。</span><span class="sxs-lookup"><span data-stu-id="8d6af-285">TDD is an iterative coding practice where you first write the tests that your resulting code will satisfy.</span></span> <span data-ttu-id="8d6af-286">使用 TDD 中，您可以開始每項功能藉由建立測試，以確認您要實作的功能。</span><span class="sxs-lookup"><span data-stu-id="8d6af-286">With TDD you begin each feature by creating a test that verifies the functionality you are about to implement.</span></span> <span data-ttu-id="8d6af-287">撰寫單元測試第一次可協助確保您清楚地了解此功能，且它是應該要如何運作。</span><span class="sxs-lookup"><span data-stu-id="8d6af-287">Writing the unit test first helps ensure that you clearly understand the feature and how it is supposed to work.</span></span> <span data-ttu-id="8d6af-288">只會寫入測試 （並確認它失敗之後） 執行您，然後實作測試驗證實際的功能。</span><span class="sxs-lookup"><span data-stu-id="8d6af-288">Only after the test is written (and you have verified that it fails) do you then implement the actual functionality the test verifies.</span></span> <span data-ttu-id="8d6af-289">因為您已經會花費時間思考該如何運作功能的使用案例，您會有更深入了解需求以及如何妥善地加以實作。</span><span class="sxs-lookup"><span data-stu-id="8d6af-289">Because you've already spent time thinking about the use case of how the feature is supposed to work, you will have a better understanding of the requirements and how best to implement them.</span></span> <span data-ttu-id="8d6af-290">當您完成的實作，您可以重新執行測試 – 並取得與立即回應是否功能正常運作。</span><span class="sxs-lookup"><span data-stu-id="8d6af-290">When you are done with the implementation you can re-run the test – and get immediate feedback as to whether the feature works correctly.</span></span> <span data-ttu-id="8d6af-291">我們將討論 TDD 章 10 中的多。</span><span class="sxs-lookup"><span data-stu-id="8d6af-291">We'll cover TDD more in Chapter 10.</span></span>

### <a name="next-step"></a><span data-ttu-id="8d6af-292">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="8d6af-292">Next Step</span></span>

<span data-ttu-id="8d6af-293">註解設定某些最終換行。</span><span class="sxs-lookup"><span data-stu-id="8d6af-293">Some final wrap up comments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8d6af-294">[上一頁](use-ajax-to-implement-mapping-scenarios.md)
> [下一頁](nerddinner-wrap-up.md)</span><span class="sxs-lookup"><span data-stu-id="8d6af-294">[Previous](use-ajax-to-implement-mapping-scenarios.md)
[Next](nerddinner-wrap-up.md)</span></span>
