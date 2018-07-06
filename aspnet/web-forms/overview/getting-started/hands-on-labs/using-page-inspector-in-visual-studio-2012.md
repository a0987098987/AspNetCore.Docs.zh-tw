---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Visual Studio 2012 中使用 Page Inspector |Microsoft Docs
author: rick-anderson
description: 在這個實際操作實驗室中，您會發現新的工具，找出並修正 Visual Studio-Page Inspector 中的網頁上的問題。 Page Inspector 是新工具 b...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: ac945a23dc6ef060340320d047f13c8e81057138
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833668"
---
<a name="using-page-inspector-in-visual-studio-2012"></a><span data-ttu-id="c5155-104">Visual Studio 2012 中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-104">Using Page Inspector in Visual Studio 2012</span></span>
====================
<span data-ttu-id="c5155-105">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c5155-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="c5155-106">在這個實際操作實驗室中，您會發現新的工具，找出並修正 Visual Studio-Page Inspector 中的網頁上的問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-106">In this Hands-on Lab, you will discover a new tool to find and fix web page issues in Visual Studio - the Page Inspector.</span></span>
> 
> <span data-ttu-id="c5155-107">Page Inspector 是一種新的工具，帶入 Visual Studio 中的瀏覽器診斷工具，並提供在瀏覽器、 ASP.NET 和原始程式碼之間的整合式的體驗。</span><span class="sxs-lookup"><span data-stu-id="c5155-107">Page Inspector is a new tool that brings browser diagnostics tools to Visual Studio and provides an integrated experience among the browser, ASP.NET, and source code.</span></span> <span data-ttu-id="c5155-108">它會呈現網頁 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁） 直接在 Visual Studio IDE，並可讓您檢查原始程式碼和產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="c5155-108">It renders a web page (HTML, Web Forms, ASP.NET MVC, or Web Pages) directly within the Visual Studio IDE and lets you examine both the source code and the resulting output.</span></span> <span data-ttu-id="c5155-109">Page Inspector 可讓您輕鬆分解網站，快速建置全新的頁面，並快速診斷問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-109">Page Inspector enables you to easily decompose a website, rapidly build pages from the ground up, and quickly diagnose issues.</span></span>
> 
> <span data-ttu-id="c5155-110">現在我們有幾個即時的方式，例如 ASP.NET MVC 和 WebForms 建立彈性且可調整的網站的 Web 架構。</span><span class="sxs-lookup"><span data-stu-id="c5155-110">Nowadays we have a number of Web frameworks that create flexible and scalable websites in a timely manner, such as ASP.NET MVC and WebForms.</span></span> <span data-ttu-id="c5155-111">相反地，它會取得難以找出頁面上的問題，因為 IDE 不支援範本為基礎的頁面和動態內容中的設計工具檢視。</span><span class="sxs-lookup"><span data-stu-id="c5155-111">On the other hand, it gets harder to find issues on the pages because the IDE does not support the designer view in template-based pages and dynamic content.</span></span> <span data-ttu-id="c5155-112">因此，這些網站有在以查看其顯示給使用者的瀏覽器中開啟。</span><span class="sxs-lookup"><span data-stu-id="c5155-112">Therefore, these websites have to be opened in a browser to see how they appear to a user.</span></span>
> 
> <span data-ttu-id="c5155-113">Web 開發人員會使用外部工具來尋找定期瀏覽器中執行的問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-113">Web developers use external tools to find issues that regularly run in the browser.</span></span> <span data-ttu-id="c5155-114">然後，他們會返回 IDE，並開始修正。</span><span class="sxs-lookup"><span data-stu-id="c5155-114">Then, they return to the IDE and start fixing.</span></span> <span data-ttu-id="c5155-115">這來回 IDE、 瀏覽器與程式碼剖析工具之間的活動可以是效率不佳，而且有時候需要全新的部署和快取清除每的次您想要重現問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-115">This back and forth activity among the IDE, the browser and the profiling tools can be inefficient, and sometimes requires a fresh deployment and cache cleaning each time you want to reproduce an issue.</span></span>
> 
> <span data-ttu-id="c5155-116">Page Inspector 橋接 Web 程式開發，用戶端 （瀏覽器工具） 和伺服器 （ASP.NET 和原始程式碼的程式碼） 之間的間距，藉由雙方使用結合的功能集的優點結合在一起。</span><span class="sxs-lookup"><span data-stu-id="c5155-116">Page Inspector bridges a gap in Web development between the client (browser tools) and the server (ASP.NET and source code) by bringing together the best of both worlds using a combined set of features.</span></span>
> 
> <span data-ttu-id="c5155-117">使用 Page Inspector，您可以看到哪些項目 （包括伺服器端程式碼） 的原始程式檔中產生要在瀏覽器中呈現的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="c5155-117">Using Page Inspector, you can see which elements in the source files (including server-side code) have produced the HTML markup to be rendered in the browser.</span></span> <span data-ttu-id="c5155-118">Page Inspector 也可讓您修改 CSS 屬性，才能看到這些變更會立即反映在瀏覽器的 DOM 項目屬性。</span><span class="sxs-lookup"><span data-stu-id="c5155-118">Page Inspector also lets you modify CSS properties and DOM element attributes to see the changes reflected immediately in the browser.</span></span>
> 
> <span data-ttu-id="c5155-119">這個實際操作實驗室會逐步引導您完成 Page Inspector 功能，並顯示您如何使用它們來修正問題，在 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c5155-119">This hands-on lab will walk you through the Page Inspector features and show you how you can use them to fix issues in Web applications.</span></span> <span data-ttu-id="c5155-120">**這個實驗室中包含兩個練習使用類似的流程，但目標為不同的技術。如果您是 ASP.NET MVC 開發人員，請依照下列練習中其中一個;如果您是兩個 WebForms 開發人員遵循練習**。</span><span class="sxs-lookup"><span data-stu-id="c5155-120">**This lab contains two exercises using similar flows but targeting different technologies. If you are an ASP.NET MVC Developer, follow exercise one; if you are a WebForms developer follow exercise two**.</span></span>
> 
> <span data-ttu-id="c5155-121">這個實驗室會引導您完成先前所述將微幅的變更套用到來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。</span><span class="sxs-lookup"><span data-stu-id="c5155-121">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="c5155-122">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="c5155-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c5155-123">目標</span><span class="sxs-lookup"><span data-stu-id="c5155-123">Objectives</span></span>

<span data-ttu-id="c5155-124">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="c5155-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="c5155-125">分解網站，使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-125">Decompose a Web Site using Page Inspector</span></span>
- <span data-ttu-id="c5155-126">檢查並預覽 Page inspector 的 CSS 樣式變更</span><span class="sxs-lookup"><span data-stu-id="c5155-126">Inspect and preview CSS styles changes with Page Inspector</span></span>
- <span data-ttu-id="c5155-127">偵測並修正問題，在您使用 Page Inspector 的網頁</span><span class="sxs-lookup"><span data-stu-id="c5155-127">Detect and fix issues in your web pages using Page Inspector</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c5155-128">必要條件</span><span class="sxs-lookup"><span data-stu-id="c5155-128">Prerequisites</span></span>

<span data-ttu-id="c5155-129">您必須具備下列項目，即可完成此實驗室：</span><span class="sxs-lookup"><span data-stu-id="c5155-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c5155-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。</span><span class="sxs-lookup"><span data-stu-id="c5155-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="c5155-131">Internet Explorer 9 或更高版本</span><span class="sxs-lookup"><span data-stu-id="c5155-131">Internet Explorer 9 or higher</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c5155-132">練習</span><span class="sxs-lookup"><span data-stu-id="c5155-132">Exercises</span></span>

<span data-ttu-id="c5155-133">這個實際操作實驗室還包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="c5155-133">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="c5155-134">練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-134">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>](#Exercise1)
2. [<span data-ttu-id="c5155-135">練習 2: WebForms 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-135">Exercise 2: Using Page Inspector in WebForms Projects</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="c5155-136">每個練習會伴隨開始的解決方案，可讓您遵循獨立於其他每個練習的練習，開始資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c5155-136">Each exercise is accompanied by a starting solution, located in the Begin folder of the exercise, that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="c5155-137">在練習的原始程式碼，您也可以找到包含 Visual Studio 方案，以產生對應的練習中的步驟的程式碼端資料夾。</span><span class="sxs-lookup"><span data-stu-id="c5155-137">Inside the source code for an exercise, you will also find an End folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="c5155-138">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。</span><span class="sxs-lookup"><span data-stu-id="c5155-138">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


<span data-ttu-id="c5155-139">完成這個實驗室估計時間： **30 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="c5155-139">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a><span data-ttu-id="c5155-140">練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-140">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>

<span data-ttu-id="c5155-141">在此練習中，您將了解如何預覽和偵錯**ASP.NET MVC 4**解決方案使用**Page Inspector**。</span><span class="sxs-lookup"><span data-stu-id="c5155-141">In this exercise, you will learn how to preview and debug an **ASP.NET MVC 4** solution using **Page Inspector**.</span></span> <span data-ttu-id="c5155-142">首先，您將執行工具以了解功能，以協助 Web 偵錯程序簡短巡覽。</span><span class="sxs-lookup"><span data-stu-id="c5155-142">First, you will perform a brief lap around the tool to learn the features that facilitate the Web debugging process.</span></span> <span data-ttu-id="c5155-143">然後，您可在網頁上包含樣式問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-143">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="c5155-144">您將了解如何使用頁面偵測器來尋找會產生問題的原始程式碼，並加以修正。</span><span class="sxs-lookup"><span data-stu-id="c5155-144">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="c5155-145">工作 1-瀏覽 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-145">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="c5155-146">在這個工作中，您將學習如何顯示相片圖庫的 ASP.NET MVC 4 專案的內容中使用 Page Inspector。</span><span class="sxs-lookup"><span data-stu-id="c5155-146">In this task, you will learn how to use the Page Inspector in the context of an ASP.NET MVC 4 project that shows a photo gallery.</span></span>

1. <span data-ttu-id="c5155-147">開啟**開始**解決方案位於**來源/Ex1-MVC4/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c5155-147">Open the **Begin** solution located at **Source/Ex1-MVC4/Begin/** folder.</span></span>

   1. <span data-ttu-id="c5155-148">您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="c5155-148">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c5155-149">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="c5155-149">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c5155-150">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="c5155-150">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c5155-151">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="c5155-151">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c5155-152">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="c5155-152">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c5155-153">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="c5155-153">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c5155-154">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="c5155-154">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c5155-155">在 [方案總管] 中，找出**Index.cshtml**下方檢視 **/views/home**專案資料夾，以滑鼠右鍵按一下它，然後選取**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="c5155-155">In the Solution Explorer, locate **Index.cshtml** view under the **/Views/Home** project folder, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="c5155-156">![選取要在 Page Inspector 中預覽檔案](using-page-inspector-in-visual-studio-2012/_static/image1.png "選取要在 Page Inspector 中預覽檔案")</span><span class="sxs-lookup"><span data-stu-id="c5155-156">![Selecting a file to preview in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "Selecting a file to preview in Page Inspector")</span></span>

    <span data-ttu-id="c5155-157">*選取要在 Page Inspector 中預覽檔案*</span><span class="sxs-lookup"><span data-stu-id="c5155-157">*Selecting a file to preview in Page Inspector*</span></span>
3. <span data-ttu-id="c5155-158">Page Inspector 視窗會顯示 */Home/Index* URL 對應至您所選的檢視的來源。</span><span class="sxs-lookup"><span data-stu-id="c5155-158">The Page Inspector window will show the */Home/Index* URL mapped to the source View you selected.</span></span>

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    <span data-ttu-id="c5155-160">*第一個連絡人，Page inspector*</span><span class="sxs-lookup"><span data-stu-id="c5155-160">*The first contact with Page Inspector*</span></span>

    <span data-ttu-id="c5155-161">Page Inspector 工具已整合在 Visual Studio 環境中。</span><span class="sxs-lookup"><span data-stu-id="c5155-161">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="c5155-162">偵測器會包含內嵌的瀏覽器，以及功能強大的 HTML 剖析工具。</span><span class="sxs-lookup"><span data-stu-id="c5155-162">The inspector contains an embedded browser, together with a powerful HTML profiler.</span></span> <span data-ttu-id="c5155-163">請注意，您就不必在執行的解決方案，以查看您的網頁的外觀。</span><span class="sxs-lookup"><span data-stu-id="c5155-163">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-164">Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。</span><span class="sxs-lookup"><span data-stu-id="c5155-164">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="c5155-165">如果發生這種情況，調整 Page Inspector 的寬度。</span><span class="sxs-lookup"><span data-stu-id="c5155-165">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="c5155-166">按一下 **檔案**Page Inspector 中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c5155-166">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="c5155-167">您會看到正在撰寫 [索引] 頁面的所有原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="c5155-167">You will see all the source files that are composing the Index page.</span></span> <span data-ttu-id="c5155-168">這項功能有助於識別一眼的所有項目，尤其是當您正在使用部分檢視和範本。</span><span class="sxs-lookup"><span data-stu-id="c5155-168">This feature helps to identify all the elements at a glance, especially when you are working with partial views and templates.</span></span> <span data-ttu-id="c5155-169">請注意，您也可以開啟每個檔案是否您按一下連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-169">Notice that you can also open each of the files if you click the links.</span></span>

    ![The-Files-tab](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    <span data-ttu-id="c5155-171">*[檔案] 索引標籤*</span><span class="sxs-lookup"><span data-stu-id="c5155-171">*The Files tab*</span></span>
5. <span data-ttu-id="c5155-172">按一下 **切換檢查模式**位於左側的索引標籤的按鈕。</span><span class="sxs-lookup"><span data-stu-id="c5155-172">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="c5155-173">這項工具可讓您選取頁面的任何項目，並查看其 HTML 和 Razor 程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-173">This tool will let you select any element of the page and see its HTML and Razor code.</span></span>

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    <span data-ttu-id="c5155-175">*切換檢查模式按鈕*</span><span class="sxs-lookup"><span data-stu-id="c5155-175">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="c5155-176">在 Page Inspector 瀏覽器中，將滑鼠指標移到頁面項目上。</span><span class="sxs-lookup"><span data-stu-id="c5155-176">In the Page Inspector browser, move the mouse pointer over the page elements.</span></span> <span data-ttu-id="c5155-177">當您將滑鼠指標移到所呈現任何的頁面部分時，項目類型會顯示，而且對應的來源標記或程式碼會反白顯示 Visual Studio 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="c5155-177">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    <span data-ttu-id="c5155-179">*檢查模式作用中*</span><span class="sxs-lookup"><span data-stu-id="c5155-179">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-180">未最大化的頁面偵測器視窗，或您不能看到預覽索引標籤顯示的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-180">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="c5155-181">如果它最大化時，您可以按一下 Page Inspector 中的項目，原始碼的選取項目會出現，但是它會隱藏頁面偵測器視窗。</span><span class="sxs-lookup"><span data-stu-id="c5155-181">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="c5155-182">如果您注意**Index.cshtml**檔案中，您會注意到的部分的原始程式碼，產生所選的項目會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="c5155-182">If you pay attention to the **Index.cshtml** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="c5155-183">這項功能有助於長的原始程式檔，提供直接和快速的方式來存取這些程式碼的編輯。</span><span class="sxs-lookup"><span data-stu-id="c5155-183">This feature facilitates the editing of long source files, providing a direct and fast way to access the code.</span></span>

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    <span data-ttu-id="c5155-185">*檢查項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-185">*Inspecting elements*</span></span>
7. <span data-ttu-id="c5155-186">按一下 [**檢查模式中切換**] 按鈕 (![選取 [HTML] 索引標籤，以顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。](using-page-inspector-in-visual-studio-2012/_static/image7.png "選取要顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼的 [HTML] 索引標籤。")</span><span class="sxs-lookup"><span data-stu-id="c5155-186">Click the **Toggle Inspection Mode** button (![Select the HTML tab to display the HTML code rendered in the Page Inspector browser.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Select the HTML tab to display the HTML code rendered in the Page Inspector browser.")</span></span> <span data-ttu-id="c5155-187">) 若要停用資料指標。</span><span class="sxs-lookup"><span data-stu-id="c5155-187">) to disable the cursor.</span></span>
8. <span data-ttu-id="c5155-188">選取  **HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-188">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="c5155-189">在 HTML 標記中，找出與 Koala 連結的清單項目，並加以選取。</span><span class="sxs-lookup"><span data-stu-id="c5155-189">In the HTML markup, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="c5155-190">請注意，當您選取的程式碼時，對應的輸出會自動反白顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c5155-190">Notice that when you select the code, the corresponding output is automatically highlighted in the browser.</span></span> <span data-ttu-id="c5155-191">此功能非常有用，若要查看如何呈現網頁上的 HTML 區塊。</span><span class="sxs-lookup"><span data-stu-id="c5155-191">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="c5155-192">![在頁面中的選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image8.png "頁面中的選取 HTML 項目")</span><span class="sxs-lookup"><span data-stu-id="c5155-192">![Selecting HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image8.png "Selecting HTML element in the page")</span></span>

    <span data-ttu-id="c5155-193">*在頁面中選取 HTML 項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-193">*Selecting HTML element in the page*</span></span>
10. <span data-ttu-id="c5155-194">按一下 [**檢查模式中切換**] 按鈕以啟用*檢查模式*，按一下巡覽列。</span><span class="sxs-lookup"><span data-stu-id="c5155-194">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="c5155-195">右側的 [樣式] 窗格中中的 HTML 程式碼，您會看到套用至所選元素的 CSS 樣式的清單。</span><span class="sxs-lookup"><span data-stu-id="c5155-195">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-196">由於標頭是站台版面配置的一部分，也會開啟 Page Inspector \_Layout.cshtml 檔案並反白顯示受影響的程式碼的區段。</span><span class="sxs-lookup"><span data-stu-id="c5155-196">Since the header is a part of the site layout, Page Inspector will also open \_Layout.cshtml file and highlight the segment of code affected.</span></span>

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    <span data-ttu-id="c5155-198">*探索樣式和選取項目的原始程式檔*</span><span class="sxs-lookup"><span data-stu-id="c5155-198">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="c5155-199">啟用切換檢查指標，將滑鼠指標下方的藍色的精選列，然後按一下 一半的圓形。</span><span class="sxs-lookup"><span data-stu-id="c5155-199">With the toggle inspection pointer enabled, move the mouse pointer below the blue featured bar and click the half circle.</span></span>

    <span data-ttu-id="c5155-200">![選取項目](using-page-inspector-in-visual-studio-2012/_static/image10.png "選取項目")</span><span class="sxs-lookup"><span data-stu-id="c5155-200">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image10.png "Selecting an element")</span></span>

    <span data-ttu-id="c5155-201">*選取項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-201">*Selecting an element*</span></span>
12. <span data-ttu-id="c5155-202">在 [樣式] 窗格中，找出**背景影像**項目底下 **.main 內容**群組。</span><span class="sxs-lookup"><span data-stu-id="c5155-202">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="c5155-203">**取消核取****背景影像**，看看結果。</span><span class="sxs-lookup"><span data-stu-id="c5155-203">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="c5155-204">您會發現瀏覽器將會立即反映所做的變更，並隱藏圓形。</span><span class="sxs-lookup"><span data-stu-id="c5155-204">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-205">您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。</span><span class="sxs-lookup"><span data-stu-id="c5155-205">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="c5155-206">您可以取消核取樣式，或變更其值做為許多次，但它們會還原之後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="c5155-206">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="c5155-207">![啟用和停用 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image11.png "啟用和停用 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="c5155-207">![Enabling and disabling CSS styles](using-page-inspector-in-visual-studio-2012/_static/image11.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="c5155-208">*啟用和停用 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="c5155-208">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="c5155-209">現在，按一下 '**您的標誌**' 上使用檢查模式中的標頭的文字。</span><span class="sxs-lookup"><span data-stu-id="c5155-209">Now, click the '**your logo here**' text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="c5155-210">在 **樣式**索引標籤上，找出**字型大小**CSS 屬性底下 **.site 標題**群組。</span><span class="sxs-lookup"><span data-stu-id="c5155-210">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="c5155-211">按兩下屬性值，並將 2.3 em 值取代**3 em**，然後按**ENTER**。</span><span class="sxs-lookup"><span data-stu-id="c5155-211">Double-click the attribute value and replace the 2.3 em value with **3 em**, and then press **ENTER**.</span></span> <span data-ttu-id="c5155-212">請注意，標題看起來更大。</span><span class="sxs-lookup"><span data-stu-id="c5155-212">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="c5155-213">![Page Inspector 中的 CSS 值的變化](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 中的變更的 CSS 值")</span><span class="sxs-lookup"><span data-stu-id="c5155-213">![Changing CSS values in the Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="c5155-214">*Page Inspector 中變更的 CSS 值*</span><span class="sxs-lookup"><span data-stu-id="c5155-214">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="c5155-215">按一下 **追蹤樣式**索引標籤上，Page Inspector 的右窗格中。</span><span class="sxs-lookup"><span data-stu-id="c5155-215">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="c5155-216">這是替代的方式，以查看套用至選取範圍，按照屬性名稱的所有樣式。</span><span class="sxs-lookup"><span data-stu-id="c5155-216">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    <span data-ttu-id="c5155-218">*CSS 樣式追蹤 選取的項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-218">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="c5155-219">Page Inspector 的另一個功能是版面配置 窗格。</span><span class="sxs-lookup"><span data-stu-id="c5155-219">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="c5155-220">使用檢查模式中，選取導覽列，然後再按一下**版面配置**在右窗格中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c5155-220">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="c5155-221">您會看到選取的項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。</span><span class="sxs-lookup"><span data-stu-id="c5155-221">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="c5155-222">請注意，您也可以修改此檢視中的值。</span><span class="sxs-lookup"><span data-stu-id="c5155-222">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="c5155-223">![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 中的項目配置")</span><span class="sxs-lookup"><span data-stu-id="c5155-223">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="c5155-224">*Page Inspector 中的項目配置*</span><span class="sxs-lookup"><span data-stu-id="c5155-224">*Element layout in Page Inspector*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="c5155-225">工作 2-尋找和修正相片圖庫中的樣式問題</span><span class="sxs-lookup"><span data-stu-id="c5155-225">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="c5155-226">如何診斷與舊版 Visual Studio 的 Web 網頁問題？</span><span class="sxs-lookup"><span data-stu-id="c5155-226">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="c5155-227">您就可能已經熟悉的 web 偵錯 Visual Studio IDE，例如 Internet Explorer 開發人員工具或 Firebug 外部執行的工具。</span><span class="sxs-lookup"><span data-stu-id="c5155-227">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="c5155-228">只有瀏覽器了解的 HTML 指令碼和樣式，雖然基礎架構會產生將轉譯的 HTML。</span><span class="sxs-lookup"><span data-stu-id="c5155-228">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="c5155-229">基於這個理由，您通常需要部署整個網站，以查看網頁的外觀。</span><span class="sxs-lookup"><span data-stu-id="c5155-229">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="c5155-230">當您想要偵測及修正的問題，在您的網站時，您可能必須遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="c5155-230">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="c5155-231">從 Visual Studio 中，執行方案，或部署 web 伺服器上的頁面。</span><span class="sxs-lookup"><span data-stu-id="c5155-231">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="c5155-232">在瀏覽器中，開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="c5155-232">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="c5155-233">若要尋找相關的檔案，您會使用&quot;搜尋&quot;或&quot;在檔案中的搜尋&quot;的樣式類別名稱的功能。</span><span class="sxs-lookup"><span data-stu-id="c5155-233">To find the files involved, you would have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="c5155-234">一旦偵測到錯誤時，會停止 Web 瀏覽器和伺服器。</span><span class="sxs-lookup"><span data-stu-id="c5155-234">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="c5155-235">清除瀏覽器快取。</span><span class="sxs-lookup"><span data-stu-id="c5155-235">Clear the browser cache.</span></span>
5. <span data-ttu-id="c5155-236">返回 Visual Studio 來套用修正程式。</span><span class="sxs-lookup"><span data-stu-id="c5155-236">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="c5155-237">重複執行測試的所有步驟。</span><span class="sxs-lookup"><span data-stu-id="c5155-237">Repeat all the steps to test.</span></span>

<span data-ttu-id="c5155-238">因為 ASP.NET MVC 4 中沒有任何實際的 WYSIWYG，大部分的樣式問題上所偵測到稍後的階段之後執行，或部署 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c5155-238">As there is no real WYSIWYG in ASP.NET MVC 4, most of the style issues are detected on a later stage, after running or deploying the web application.</span></span> <span data-ttu-id="c5155-239">現在，Page inspector 就可能需要先預覽任何頁面，而不需執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="c5155-239">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="c5155-240">在這個工作中，您會使用 Page inspector，並修正相片圖庫應用程式的一些問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-240">In this task, you will use the Page inspector and fix some issues the Photo Gallery application.</span></span>

1. <span data-ttu-id="c5155-241">使用 Page Inspector，找出**註冊**並**登入**標頭的左側的連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-241">Using Page Inspector, locate the **Register** and the **Log in** links at the left side of the header.</span></span>

    <span data-ttu-id="c5155-242">請注意連結不會顯示在右側，預期的位置，而且它們會顯示類似於項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="c5155-242">Notice that the links are not displayed at the expected place on the right, and they are shown like a bulleted list.</span></span> <span data-ttu-id="c5155-243">您現在將會對齊右邊的連結，並據以重新加以設定樣式。</span><span class="sxs-lookup"><span data-stu-id="c5155-243">You will now align the links to the right and restyle them accordingly.</span></span>

    <span data-ttu-id="c5155-244">![尋找連結中的暫存器和記錄](using-page-inspector-in-visual-studio-2012/_static/image15.png "尋找暫存器和記錄檔中的連結")</span><span class="sxs-lookup"><span data-stu-id="c5155-244">![Locating the Register and Log in links](using-page-inspector-in-visual-studio-2012/_static/image15.png "Locating the Register and Log in links")</span></span>

    <span data-ttu-id="c5155-245">*尋找連結中的暫存器和記錄檔*</span><span class="sxs-lookup"><span data-stu-id="c5155-245">*Locating the Register and Log in links*</span></span>
2. <span data-ttu-id="c5155-246">切換檢查模式中選取，按一下 [關閉] 以，但不是能在以開啟其程式碼的 [註冊] 連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-246">With Toggle Inspection Mode selected, click close to, but not on, the Register link to open its code.</span></span>

    <span data-ttu-id="c5155-247">請注意，連結的原始程式碼位於 **\_LoginPartial.cshtml**檔案，不 Index.cshtml 和\_Layout.cshtml，也就是您可以看看第一個位置中的位置。</span><span class="sxs-lookup"><span data-stu-id="c5155-247">Notice that the source code of the links is located in the **\_LoginPartial.cshtml** file, not the Index.cshtml nor the \_Layout.cshtml, which are the places you might look in first place.</span></span> <span data-ttu-id="c5155-248">您已經放入正確的原始程式檔直接。</span><span class="sxs-lookup"><span data-stu-id="c5155-248">You have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="c5155-249">在 **樣式**索引標籤上，找出並按一下  **<section> #login</section>** 項目，也就是這些連結的 HTML 容器。</span><span class="sxs-lookup"><span data-stu-id="c5155-249">In the **Styles** tab, locate and click the **<section> #login</section>** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="c5155-250">請注意， **#login**樣式會自動位於**Site.css**按一下之後。</span><span class="sxs-lookup"><span data-stu-id="c5155-250">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="c5155-251">此外，現在會醒目提示程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-251">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="c5155-252">![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image16.png "選取的 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="c5155-252">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image16.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="c5155-253">*選取的 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="c5155-253">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="c5155-254">取消註解**文字對齊**醒目提示的程式碼中移除開頭和結尾字元的屬性，並儲存**Site.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="c5155-254">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="c5155-255">Page Inspector 知道撰寫目前的頁面上，所有不同檔案的存在，而且它可以偵測到任何這些檔案的變更時。</span><span class="sxs-lookup"><span data-stu-id="c5155-255">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="c5155-256">它會警告您只要在瀏覽器中目前的頁面不是與原始程式檔的同步處理。</span><span class="sxs-lookup"><span data-stu-id="c5155-256">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="c5155-257">在 Page Inspector 瀏覽器中，按一下位於 [網址] 列，以重新載入頁面下方的列。</span><span class="sxs-lookup"><span data-stu-id="c5155-257">In the Page Inspector browser, click the bar located below the address bar to reload the page.</span></span>

    ![重新載入頁面](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    <span data-ttu-id="c5155-259">*重新載入頁面*</span><span class="sxs-lookup"><span data-stu-id="c5155-259">*Reloading the page*</span></span>

    <span data-ttu-id="c5155-260">連結現在會在右側，但它們仍然看起來像是項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="c5155-260">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="c5155-261">現在，您將會移除項目符號，並水平對齊的連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-261">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新後的頁面](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    <span data-ttu-id="c5155-263">*更新後的頁面*</span><span class="sxs-lookup"><span data-stu-id="c5155-263">*Updated page*</span></span>
6. <span data-ttu-id="c5155-264">使用檢查模式中，選取任一**&lt;li&gt;** 包含的項目&quot;註冊&quot;並&quot;登入&quot;連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-264">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="c5155-265">然後，按一下 **&lt; 區段&gt;#login**項目來存取**Styles.css**程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-265">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="c5155-266">![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image19.png "尋找樣式")</span><span class="sxs-lookup"><span data-stu-id="c5155-266">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image19.png "Finding the style")</span></span>

    <span data-ttu-id="c5155-267">*尋找樣式*</span><span class="sxs-lookup"><span data-stu-id="c5155-267">*Finding the style*</span></span>
7. <span data-ttu-id="c5155-268">在  **Style.css**，取消註解的程式碼 **#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="c5155-268">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="c5155-269">您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。</span><span class="sxs-lookup"><span data-stu-id="c5155-269">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="c5155-270">![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image20.png "樣式重新設定的登入連結")</span><span class="sxs-lookup"><span data-stu-id="c5155-270">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image20.png "Restyling the login links")</span></span>

    <span data-ttu-id="c5155-271">*右側的登入連結*</span><span class="sxs-lookup"><span data-stu-id="c5155-271">*Restyling the login links*</span></span>
8. <span data-ttu-id="c5155-272">儲存**Style.css**檔案，然後按一下下方 要重新載入頁面的位址列上的一次。</span><span class="sxs-lookup"><span data-stu-id="c5155-272">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="c5155-273">請注意，連結會正確出現。</span><span class="sxs-lookup"><span data-stu-id="c5155-273">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="c5155-274">![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image21.png "連結靠右對齊")</span><span class="sxs-lookup"><span data-stu-id="c5155-274">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="c5155-275">*靠右對齊的連結*</span><span class="sxs-lookup"><span data-stu-id="c5155-275">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="c5155-276">最後，您將變更標頭標題。</span><span class="sxs-lookup"><span data-stu-id="c5155-276">Finally, you will change the header title.</span></span> <span data-ttu-id="c5155-277">使用檢查模式中，按一下**您的標誌**文字並開始產生的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-277">Use the inspection mode to click **your logo here** text and get to the source code that generates it.</span></span>
10. <span data-ttu-id="c5155-278">現在您已在 **\_Layout.cshtml**，取代 '**您的標誌**'與 text'**相片圖庫**'。</span><span class="sxs-lookup"><span data-stu-id="c5155-278">Now you are in **\_Layout.cshtml**, replace '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="c5155-279">儲存並更新 Page Inspector 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c5155-279">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="c5155-280">![指派新的標題](using-page-inspector-in-visual-studio-2012/_static/image22.png "指派新的標題")</span><span class="sxs-lookup"><span data-stu-id="c5155-280">![Assigning a new title](using-page-inspector-in-visual-studio-2012/_static/image22.png "Assigning a new title")</span></span>

    <span data-ttu-id="c5155-281">*指派新的標題*</span><span class="sxs-lookup"><span data-stu-id="c5155-281">*Assigning a new title*</span></span>

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    <span data-ttu-id="c5155-283">*更新的 [相片圖庫] 頁面*</span><span class="sxs-lookup"><span data-stu-id="c5155-283">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="c5155-284">最後，選取**PhotoGallery**專案，然後按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c5155-284">Finally, selet the **PhotoGallery** project and press **F5** to run the app.</span></span> <span data-ttu-id="c5155-285">查看所有變更運作如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="c5155-285">Check out all the changes work as expected.</span></span>

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a><span data-ttu-id="c5155-286">練習 2: WebForms 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-286">Exercise 2: Using Page Inspector in WebForms Projects</span></span>

<span data-ttu-id="c5155-287">在此練習中，您將了解如何預覽，並使用 Page Inspector WebForms 方案進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="c5155-287">In this exercise, you will learn how to preview and debug a WebForms solution using Page Inspector.</span></span> <span data-ttu-id="c5155-288">您會先執行簡短瀏覽工具若要了解 Page Inspector 功能，以協助 Web 偵錯程序。</span><span class="sxs-lookup"><span data-stu-id="c5155-288">You will first perform a brief lap around the tool to learn the Page Inspector features that facilitate the Web debugging process.</span></span> <span data-ttu-id="c5155-289">然後，您可在網頁上包含樣式問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-289">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="c5155-290">您將了解如何使用頁面偵測器來尋找會產生問題的原始程式碼，並加以修正。</span><span class="sxs-lookup"><span data-stu-id="c5155-290">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="c5155-291">工作 1-瀏覽 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c5155-291">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="c5155-292">在這個工作中，您將學習如何在 顯示相片圖庫 WebForms 專案內容中使用 Page Inspector 功能。</span><span class="sxs-lookup"><span data-stu-id="c5155-292">In this task, you will learn how to use the Page Inspector features in the context of a WebForms project that shows a photo gallery.</span></span>

1. <span data-ttu-id="c5155-293">開啟**開始**解決方案位於**來源/Ex2-WebForms/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c5155-293">Open the **Begin** solution located at **Source/Ex2-WebForms/Begin/** folder.</span></span>

   1. <span data-ttu-id="c5155-294">您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="c5155-294">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c5155-295">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="c5155-295">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c5155-296">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="c5155-296">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c5155-297">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="c5155-297">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c5155-298">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="c5155-298">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c5155-299">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="c5155-299">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c5155-300">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="c5155-300">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c5155-301">在 [方案總管] 中，找出**Default.aspx**頁面上，以滑鼠右鍵按一下它，然後選取**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="c5155-301">In the Solution Explorer, locate **Default.aspx** page, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="c5155-302">![Page inspector 中開啟 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "開啟 Default.aspx，Page inspector")</span><span class="sxs-lookup"><span data-stu-id="c5155-302">![Opening Default.aspx with Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "Opening Default.aspx with Page Inspector")</span></span>

    <span data-ttu-id="c5155-303">*開啟 Default.aspx，Page inspector*</span><span class="sxs-lookup"><span data-stu-id="c5155-303">*Opening Default.aspx with Page Inspector*</span></span>
3. <span data-ttu-id="c5155-304">Page Inspector 視窗會顯示 Default.aspx。</span><span class="sxs-lookup"><span data-stu-id="c5155-304">The Page Inspector window will show Default.aspx.</span></span>

    <span data-ttu-id="c5155-305">![Page Inspector 中檢視 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 中檢視 Default.aspx")</span><span class="sxs-lookup"><span data-stu-id="c5155-305">![Viewing Default.aspx in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "Viewing Default.aspx in Page Inspector")</span></span>

    <span data-ttu-id="c5155-306">*Page Inspector 中檢視 Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="c5155-306">*Viewing Default.aspx in Page Inspector*</span></span>

    <span data-ttu-id="c5155-307">Page Inspector 工具已整合在 Visual Studio 環境中。</span><span class="sxs-lookup"><span data-stu-id="c5155-307">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="c5155-308">偵測器會包含內嵌的瀏覽器，以及功能強大的 HTML 程式碼，而該剖析工具會顯示選取的程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-308">The inspector contains an embedded browser, together with a powerful HTML profiler that will show the selected code.</span></span> <span data-ttu-id="c5155-309">請注意，您就不必在執行的解決方案，以查看您的網頁的外觀。</span><span class="sxs-lookup"><span data-stu-id="c5155-309">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-310">Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。</span><span class="sxs-lookup"><span data-stu-id="c5155-310">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="c5155-311">如果發生這種情況，調整 Page Inspector 的寬度。</span><span class="sxs-lookup"><span data-stu-id="c5155-311">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="c5155-312">按一下 **檔案**Page Inspector 中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c5155-312">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="c5155-313">您會看到正在撰寫所呈現的預設網頁的所有原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="c5155-313">You will see all the source files that are composing the rendered Default page.</span></span> <span data-ttu-id="c5155-314">這是很有用的功能，以識別一眼的所有項目，尤其是當您正在使用使用者控制項和主版頁面。</span><span class="sxs-lookup"><span data-stu-id="c5155-314">This is a useful feature to identify all the elements at a glance, especially when you are working with User Controls and Master Pages.</span></span> <span data-ttu-id="c5155-315">請注意，您也可以巡覽至每個檔案。</span><span class="sxs-lookup"><span data-stu-id="c5155-315">Notice that you can also navigate to each of the files.</span></span>

    <span data-ttu-id="c5155-316">![[檔案] 索引標籤](using-page-inspector-in-visual-studio-2012/_static/image26.png "檔案 索引標籤")</span><span class="sxs-lookup"><span data-stu-id="c5155-316">![The Files tab](using-page-inspector-in-visual-studio-2012/_static/image26.png "The Files tab")</span></span>

    <span data-ttu-id="c5155-317">*[檔案] 索引標籤*</span><span class="sxs-lookup"><span data-stu-id="c5155-317">*The Files tab*</span></span>
5. <span data-ttu-id="c5155-318">按一下 **切換檢查模式**位於左側的索引標籤的按鈕。</span><span class="sxs-lookup"><span data-stu-id="c5155-318">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="c5155-319">這項工具可讓您選取頁面的任何項目，並查看它的 HTML 程式碼和.aspx 原始檔。</span><span class="sxs-lookup"><span data-stu-id="c5155-319">This tool will let you select any element of the page and see its HTML code and .aspx source.</span></span>

    <span data-ttu-id="c5155-320">![切換檢查模式按鈕](using-page-inspector-in-visual-studio-2012/_static/image27.png "切換檢查模式按鈕")</span><span class="sxs-lookup"><span data-stu-id="c5155-320">![Toggle Inspection Mode button](using-page-inspector-in-visual-studio-2012/_static/image27.png "Toggle Inspection Mode button")</span></span>

    <span data-ttu-id="c5155-321">*切換檢查模式按鈕*</span><span class="sxs-lookup"><span data-stu-id="c5155-321">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="c5155-322">在 Page Inspector 瀏覽器中，請將滑鼠移頁面項目。</span><span class="sxs-lookup"><span data-stu-id="c5155-322">In the Page Inspector browser, move the mouse over the page elements.</span></span> <span data-ttu-id="c5155-323">當您將滑鼠指標移到所呈現任何的頁面部分時，項目類型會顯示，而且對應的來源標記或程式碼會反白顯示 Visual Studio 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="c5155-323">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    <span data-ttu-id="c5155-324">![檢查模式中運作](using-page-inspector-in-visual-studio-2012/_static/image28.png "檢查模式作用中")</span><span class="sxs-lookup"><span data-stu-id="c5155-324">![Inspection mode in action](using-page-inspector-in-visual-studio-2012/_static/image28.png "Inspection mode in action")</span></span>

    <span data-ttu-id="c5155-325">*檢查模式作用中*</span><span class="sxs-lookup"><span data-stu-id="c5155-325">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-326">未最大化的頁面偵測器視窗，或您不能看到預覽索引標籤顯示的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-326">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="c5155-327">如果它最大化時，您可以按一下 Page Inspector 中的項目，原始碼的選取項目會出現，但是它會隱藏頁面偵測器視窗。</span><span class="sxs-lookup"><span data-stu-id="c5155-327">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="c5155-328">如果您注意**Default.aspx**檔案中，您會注意到的部分的原始程式碼，產生所選的項目會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="c5155-328">If you pay attention to **Default.aspx** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="c5155-329">這項功能有助於長的原始程式檔，提供直接和快速的方式來存取這些程式碼的版本。</span><span class="sxs-lookup"><span data-stu-id="c5155-329">This feature facilitates the edition of long source files, providing a direct and fast way to access the code.</span></span>

    <span data-ttu-id="c5155-330">![檢查項目](using-page-inspector-in-visual-studio-2012/_static/image29.png "檢查項目")</span><span class="sxs-lookup"><span data-stu-id="c5155-330">![Inspecting elements](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspecting elements")</span></span>

    <span data-ttu-id="c5155-331">*檢查項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-331">*Inspecting elements*</span></span>
7. <span data-ttu-id="c5155-332">按一下 [**檢查模式中切換**] 按鈕 (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。")</span><span class="sxs-lookup"><span data-stu-id="c5155-332">Click the **Toggle Inspection Mode** button (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.")</span></span> <span data-ttu-id="c5155-333">)，位於 Page Inspector 的索引標籤中，若要停用資料指標。</span><span class="sxs-lookup"><span data-stu-id="c5155-333">), located in Page Inspector tabs, to disable the cursor.</span></span>
8. <span data-ttu-id="c5155-334">選取  **HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-334">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="c5155-335">中的 HTML 程式碼中，找出與 Koala 連結的清單項目，並加以選取。</span><span class="sxs-lookup"><span data-stu-id="c5155-335">In the HTML code, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="c5155-336">請注意，當您選取的程式碼時，對應的輸出會自動反白顯示的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c5155-336">Notice that when you select the code, the corresponding output is automatically highlighted browser.</span></span> <span data-ttu-id="c5155-337">此功能非常有用，若要查看如何呈現網頁上的 HTML 區塊。</span><span class="sxs-lookup"><span data-stu-id="c5155-337">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="c5155-338">![在頁面中選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image31.png "在頁面中選取 HTML 項目")</span><span class="sxs-lookup"><span data-stu-id="c5155-338">![Selecting an HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image31.png "Selecting an HTML element in the page")</span></span>

    <span data-ttu-id="c5155-339">*在頁面中選取 HTML 項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-339">*Selecting an HTML element in the page*</span></span>
10. <span data-ttu-id="c5155-340">按一下 [**檢查模式中切換**] 按鈕以啟用*檢查模式*，按一下巡覽列。</span><span class="sxs-lookup"><span data-stu-id="c5155-340">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="c5155-341">右側的 [樣式] 窗格中中的 HTML 程式碼，您會看到套用至所選元素的 CSS 樣式的清單。</span><span class="sxs-lookup"><span data-stu-id="c5155-341">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-342">由於標頭是站台版面配置的一部分，Page Inspector 也會開啟 Site.Master 檔案，並反白顯示受影響的程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="c5155-342">since the header is a part of the site layout, Page Inspector will also open Site.Master file and highlight the segment of code affected.</span></span>

    <span data-ttu-id="c5155-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "探索樣式和選取項目的原始程式檔")</span><span class="sxs-lookup"><span data-stu-id="c5155-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Discovering styles and source files of a selected element")</span></span>

    <span data-ttu-id="c5155-344">*探索樣式和選取項目的原始程式檔*</span><span class="sxs-lookup"><span data-stu-id="c5155-344">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="c5155-345">啟用切換檢查指標，將滑鼠指標在功能表列下方，然後按一下 空白的二分之一圓。</span><span class="sxs-lookup"><span data-stu-id="c5155-345">With the toggle inspection pointer enabled, move the mouse pointer below the menu bar and click the blank half circle.</span></span>

    <span data-ttu-id="c5155-346">![選取項目](using-page-inspector-in-visual-studio-2012/_static/image33.png "選取項目")</span><span class="sxs-lookup"><span data-stu-id="c5155-346">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image33.png "Selecting an element")</span></span>

    <span data-ttu-id="c5155-347">*選取項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-347">*Selecting an element*</span></span>
12. <span data-ttu-id="c5155-348">在 [樣式] 窗格中，找出**背景影像**項目底下 **.main 內容**群組。</span><span class="sxs-lookup"><span data-stu-id="c5155-348">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="c5155-349">**取消核取****背景影像**，看看結果。</span><span class="sxs-lookup"><span data-stu-id="c5155-349">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="c5155-350">您會發現瀏覽器將會立即反映所做的變更，並隱藏圓形。</span><span class="sxs-lookup"><span data-stu-id="c5155-350">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5155-351">您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。</span><span class="sxs-lookup"><span data-stu-id="c5155-351">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="c5155-352">您可以取消核取樣式，或變更其值做為許多次，但它們會還原之後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="c5155-352">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="c5155-353">![啟用和停用 CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "啟用和停用 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="c5155-353">![Enabling and disabling CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="c5155-354">*啟用和停用 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="c5155-354">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="c5155-355">現在，按一下 '**您****標誌的位置'** 上使用檢查模式中的標頭的文字。</span><span class="sxs-lookup"><span data-stu-id="c5155-355">Now, click the '**your** **logo here'** text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="c5155-356">在 **樣式**索引標籤上，找出**字型大小**CSS 屬性底下 **.site 標題**群組。</span><span class="sxs-lookup"><span data-stu-id="c5155-356">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="c5155-357">按兩下以編輯其值的屬性一次。</span><span class="sxs-lookup"><span data-stu-id="c5155-357">Double-click the attribute once to edit its value.</span></span> <span data-ttu-id="c5155-358">值取代 2.3em **3em**，然後按 ENTER 鍵。</span><span class="sxs-lookup"><span data-stu-id="c5155-358">Replace the 2.3em value with **3em**, and then press ENTER.</span></span> <span data-ttu-id="c5155-359">請注意，標題看起來更大。</span><span class="sxs-lookup"><span data-stu-id="c5155-359">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="c5155-360">![變更頁面 Inspector2 中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 中的變更的 CSS 值")</span><span class="sxs-lookup"><span data-stu-id="c5155-360">![Changing CSS values in the Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="c5155-361">*Page Inspector 中變更的 CSS 值*</span><span class="sxs-lookup"><span data-stu-id="c5155-361">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="c5155-362">按一下 **追蹤樣式**索引標籤上，Page Inspector 的右窗格中。</span><span class="sxs-lookup"><span data-stu-id="c5155-362">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="c5155-363">這是替代的方式，以查看套用至選取範圍，按照屬性名稱的所有樣式。</span><span class="sxs-lookup"><span data-stu-id="c5155-363">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    <span data-ttu-id="c5155-364">![選取項目的 CSS 樣式追蹤](using-page-inspector-in-visual-studio-2012/_static/image36.png "所選項目的 CSS 樣式追蹤")</span><span class="sxs-lookup"><span data-stu-id="c5155-364">![CSS styles tracing of the selected element](using-page-inspector-in-visual-studio-2012/_static/image36.png "CSS styles tracing of the selected element")</span></span>

    <span data-ttu-id="c5155-365">*CSS 樣式追蹤 選取的項目*</span><span class="sxs-lookup"><span data-stu-id="c5155-365">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="c5155-366">Page Inspector 的另一個功能是版面配置 窗格。</span><span class="sxs-lookup"><span data-stu-id="c5155-366">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="c5155-367">使用檢查模式中，選取導覽列，然後再按一下**版面配置**在右窗格中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c5155-367">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="c5155-368">您會看到選取的項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。</span><span class="sxs-lookup"><span data-stu-id="c5155-368">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="c5155-369">請注意，您也可以修改此檢視中的值。</span><span class="sxs-lookup"><span data-stu-id="c5155-369">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="c5155-370">![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 中的項目配置")</span><span class="sxs-lookup"><span data-stu-id="c5155-370">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="c5155-371">*Page Inspector 中的項目配置*</span><span class="sxs-lookup"><span data-stu-id="c5155-371">*Element layout in Page Inspector*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="c5155-372">工作 2-尋找和修正相片圖庫中的樣式問題</span><span class="sxs-lookup"><span data-stu-id="c5155-372">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="c5155-373">如何診斷與舊版 Visual Studio 的 Web 網頁問題？</span><span class="sxs-lookup"><span data-stu-id="c5155-373">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="c5155-374">您就可能已經熟悉的 web 偵錯 Visual Studio IDE，例如 Internet Explorer 開發人員工具或 Firebug 外部執行的工具。</span><span class="sxs-lookup"><span data-stu-id="c5155-374">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="c5155-375">只有瀏覽器了解的 HTML 指令碼和樣式，雖然基礎架構會產生將轉譯的 HTML。</span><span class="sxs-lookup"><span data-stu-id="c5155-375">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="c5155-376">基於這個理由，您通常需要部署整個網站，以查看網頁的外觀。</span><span class="sxs-lookup"><span data-stu-id="c5155-376">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="c5155-377">當您想要偵測及修正的問題，在您的網站時，您可能必須遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="c5155-377">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="c5155-378">從 Visual Studio 中，執行方案，或部署 web 伺服器上的頁面。</span><span class="sxs-lookup"><span data-stu-id="c5155-378">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="c5155-379">在瀏覽器中，開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="c5155-379">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="c5155-380">若要尋找相關的檔案，您會使用&quot;搜尋&quot;或&quot;在檔案中的搜尋&quot;的樣式類別名稱的功能。</span><span class="sxs-lookup"><span data-stu-id="c5155-380">To find the files involved, you'd have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="c5155-381">一旦偵測到錯誤時，會停止 Web 瀏覽器和伺服器。</span><span class="sxs-lookup"><span data-stu-id="c5155-381">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="c5155-382">清除瀏覽器快取。</span><span class="sxs-lookup"><span data-stu-id="c5155-382">Clear the browser cache.</span></span>
5. <span data-ttu-id="c5155-383">返回 Visual Studio 來套用修正程式。</span><span class="sxs-lookup"><span data-stu-id="c5155-383">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="c5155-384">重複執行測試的所有步驟。</span><span class="sxs-lookup"><span data-stu-id="c5155-384">Repeat all the steps to test.</span></span>

<span data-ttu-id="c5155-385">因為沒有不在 ASP.NET WebForms 真正 WYSIWYG，某些樣式問題上偵測到稍後的階段之後執行，或部署。</span><span class="sxs-lookup"><span data-stu-id="c5155-385">As there is no real WYSIWYG in ASP.NET WebForms, some style issues are detected on a later stage, after running or deploying.</span></span> <span data-ttu-id="c5155-386">現在，Page inspector 就可能需要先預覽任何頁面，而不需執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="c5155-386">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="c5155-387">在這個工作中，您將使用 Page inspector 修正相片圖庫應用程式的一些問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-387">In this task, you will use the Page inspector for fixing some issues of the Photo Gallery application.</span></span> <span data-ttu-id="c5155-388">在下列步驟中，您將會偵測並快速修正標頭中的 簡單樣式的一些問題。</span><span class="sxs-lookup"><span data-stu-id="c5155-388">In the following steps, you will detect and quickly fix some simple styling issue in the header.</span></span>

1. <span data-ttu-id="c5155-389">使用頁面檢查，找出**註冊**並**登入**標頭的左側的連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-389">Using Page Inspection, locate the **Register** and the **Log In** links at the left side of the header.</span></span>

    <span data-ttu-id="c5155-390">請注意，連結不會顯示在右邊的預期位置。</span><span class="sxs-lookup"><span data-stu-id="c5155-390">Notice that the link is not displayed at the expected place on the right.</span></span> <span data-ttu-id="c5155-391">您現在將會對齊右邊的連結，並據以重新設定樣式。</span><span class="sxs-lookup"><span data-stu-id="c5155-391">You will now align the link to the right and restyle it accordingly.</span></span>

    <span data-ttu-id="c5155-392">![登入位於左側的連結](using-page-inspector-in-visual-studio-2012/_static/image38.png "登入位於左側的連結")</span><span class="sxs-lookup"><span data-stu-id="c5155-392">![Log in link positioned on the left](using-page-inspector-in-visual-studio-2012/_static/image38.png "Log in link positioned on the left")</span></span>

    <span data-ttu-id="c5155-393">*位於左側的登入連結*</span><span class="sxs-lookup"><span data-stu-id="c5155-393">*Log In link positioned on the left*</span></span>
2. <span data-ttu-id="c5155-394">切換選取的檢查模式下，選取 登入連結以開啟其程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-394">With Toggle Inspection Mode selected, select the Log In link to open its code.</span></span>

    <span data-ttu-id="c5155-395">請注意，連結來源的程式碼位於**Site.Master**檔案，不是取代 Default.aspx 頁面中您可以看看在第一個位置; 您已放置在正確的原始程式檔直接。</span><span class="sxs-lookup"><span data-stu-id="c5155-395">Notice that the link source code is located in the **Site.Master** file, not in the Default.aspx page which is the place you might look in first place; you have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="c5155-396">在 **樣式**索引標籤上，找出並按一下  **&lt;一節&gt;#login**項目，也就是這些連結的 HTML 容器。</span><span class="sxs-lookup"><span data-stu-id="c5155-396">In the **Styles** tab, locate and click the **&lt;section&gt; #login** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="c5155-397">請注意， **#login**樣式會自動位於**Site.css**按一下之後。</span><span class="sxs-lookup"><span data-stu-id="c5155-397">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="c5155-398">此外，現在會醒目提示程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-398">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="c5155-399">![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image39.png "選取的 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="c5155-399">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image39.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="c5155-400">*選取的 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="c5155-400">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="c5155-401">取消註解**文字對齊**醒目提示的程式碼中移除開頭和結尾字元的屬性，並儲存**Site.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="c5155-401">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="c5155-402">Page Inspector 知道撰寫目前的頁面上，所有不同檔案的存在，而且它可以偵測到任何這些檔案的變更時。</span><span class="sxs-lookup"><span data-stu-id="c5155-402">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="c5155-403">它會警告您只要在瀏覽器中目前的頁面不是與原始程式檔的同步處理。</span><span class="sxs-lookup"><span data-stu-id="c5155-403">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="c5155-404">在 Page Inspector 瀏覽器中，按一下位於 [網址] 列，以儲存變更並重新載入頁面下方的列。</span><span class="sxs-lookup"><span data-stu-id="c5155-404">In the Page Inspector browser, click the bar located below the address bar to save the changes and reload the page.</span></span>

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    <span data-ttu-id="c5155-406">*重新載入頁面*</span><span class="sxs-lookup"><span data-stu-id="c5155-406">*Reloading the page*</span></span>

    <span data-ttu-id="c5155-407">連結現在會在右側，但它們仍然看起來像是項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="c5155-407">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="c5155-408">現在，您將會移除項目符號，並水平對齊的連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-408">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新後的頁面](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    <span data-ttu-id="c5155-410">*更新後的頁面*</span><span class="sxs-lookup"><span data-stu-id="c5155-410">*Updated page*</span></span>
6. <span data-ttu-id="c5155-411">使用檢查模式中，選取任一**&lt;li&gt;** 包含的項目&quot;註冊&quot;並&quot;登入&quot;連結。</span><span class="sxs-lookup"><span data-stu-id="c5155-411">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="c5155-412">然後，按一下 **&lt; 區段&gt;#login**項目來存取**Styles.css**程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-412">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="c5155-413">![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image42.png "尋找樣式")</span><span class="sxs-lookup"><span data-stu-id="c5155-413">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image42.png "Finding the style")</span></span>

    <span data-ttu-id="c5155-414">*尋找樣式*</span><span class="sxs-lookup"><span data-stu-id="c5155-414">*Finding the style*</span></span>
7. <span data-ttu-id="c5155-415">在  **Style.css**，取消註解的程式碼 **#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="c5155-415">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="c5155-416">您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。</span><span class="sxs-lookup"><span data-stu-id="c5155-416">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="c5155-417">![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image43.png "樣式重新設定的登入連結")</span><span class="sxs-lookup"><span data-stu-id="c5155-417">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image43.png "Restyling the login links")</span></span>

    <span data-ttu-id="c5155-418">*右側的登入連結*</span><span class="sxs-lookup"><span data-stu-id="c5155-418">*Restyling the login links*</span></span>
8. <span data-ttu-id="c5155-419">儲存**Style.css**檔案，然後按一下下方 要重新載入頁面的位址列上的一次。</span><span class="sxs-lookup"><span data-stu-id="c5155-419">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="c5155-420">請注意，連結會正確出現。</span><span class="sxs-lookup"><span data-stu-id="c5155-420">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="c5155-421">![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image44.png "連結靠右對齊")</span><span class="sxs-lookup"><span data-stu-id="c5155-421">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="c5155-422">*靠右對齊的連結*</span><span class="sxs-lookup"><span data-stu-id="c5155-422">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="c5155-423">最後，您將變更標頭標題。</span><span class="sxs-lookup"><span data-stu-id="c5155-423">Finally, you will change the header title.</span></span> <span data-ttu-id="c5155-424">而不是搜尋 '**您的標誌'** 文字中的所有檔案，按一下該文字，並取得會產生它的原始碼中使用檢查模式。</span><span class="sxs-lookup"><span data-stu-id="c5155-424">Instead of searching for the '**your logo here'** text in all the files, use the inspection mode to click the text and get to source code that generates it.</span></span>

    <span data-ttu-id="c5155-425">![尋找網站標題](using-page-inspector-in-visual-studio-2012/_static/image45.png "尋找網站標題")</span><span class="sxs-lookup"><span data-stu-id="c5155-425">![Finding the site title](using-page-inspector-in-visual-studio-2012/_static/image45.png "Finding the site title")</span></span>

    <span data-ttu-id="c5155-426">*尋找網站標題*</span><span class="sxs-lookup"><span data-stu-id="c5155-426">*Finding the site title*</span></span>
10. <span data-ttu-id="c5155-427">現在您已在**Site.Master**，取代 '**您的標誌**'與 text'**相片圖庫**'。</span><span class="sxs-lookup"><span data-stu-id="c5155-427">Now you are in **Site.Master**, replace the '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="c5155-428">儲存並更新 Page Inspector 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c5155-428">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="c5155-429">![相片圖庫 頁面更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "相片圖庫 頁面更新")</span><span class="sxs-lookup"><span data-stu-id="c5155-429">![Photo Gallery page updated](using-page-inspector-in-visual-studio-2012/_static/image46.png "Photo Gallery page updated")</span></span>

    <span data-ttu-id="c5155-430">*更新的 [相片圖庫] 頁面*</span><span class="sxs-lookup"><span data-stu-id="c5155-430">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="c5155-431">最後按下**F5**執行應用程式的簽出所有變更運作如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="c5155-431">Finally press **F5** to run the app the check out all the changes work as expected.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c5155-432">總結</span><span class="sxs-lookup"><span data-stu-id="c5155-432">Summary</span></span>

<span data-ttu-id="c5155-433">藉由完成這個實際操作實驗室，您已學會如何使用 Page Inspector 預覽您的 Web 應用程式，而不需要重建和瀏覽器中執行的網站。</span><span class="sxs-lookup"><span data-stu-id="c5155-433">By completing this Hands-On Lab, you have learnt how to use Page Inspector to preview your Web application without having to rebuild and run the Web site in a browser.</span></span> <span data-ttu-id="c5155-434">此外，您已學會如何快速找到並修正 bug，直接從轉譯的輸出存取原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="c5155-434">In addition, you have learnt how to quickly find and fix bugs by accessing directly from the rendered output to the source code.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c5155-435">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="c5155-435">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c5155-436">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="c5155-436">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c5155-437">下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="c5155-437">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c5155-438">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="c5155-438">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c5155-439">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="c5155-439">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c5155-440">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="c5155-440">Click on **Install Now**.</span></span> <span data-ttu-id="c5155-441">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="c5155-441">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c5155-442">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="c5155-442">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c5155-443">![安裝 Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c5155-443">![Install Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c5155-444">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c5155-444">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c5155-445">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="c5155-445">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    <span data-ttu-id="c5155-447">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="c5155-447">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c5155-448">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="c5155-448">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    <span data-ttu-id="c5155-450">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="c5155-450">*Installation progress*</span></span>
6. <span data-ttu-id="c5155-451">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="c5155-451">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    <span data-ttu-id="c5155-453">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="c5155-453">*Installation completed*</span></span>
7. <span data-ttu-id="c5155-454">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="c5155-454">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c5155-455">若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。</span><span class="sxs-lookup"><span data-stu-id="c5155-455">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 圖格](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    <span data-ttu-id="c5155-457">*VS Express for Web 圖格*</span><span class="sxs-lookup"><span data-stu-id="c5155-457">*VS Express for Web tile*</span></span>
