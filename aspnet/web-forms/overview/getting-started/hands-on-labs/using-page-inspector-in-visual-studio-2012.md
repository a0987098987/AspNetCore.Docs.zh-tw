---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: "在 Visual Studio 2012 中使用 Page Inspector |Microsoft 文件"
author: rick-anderson
description: "在這個實際操作實驗室中，您會發現新的工具，尋找並修正 Visual Studio-Page Inspector 中的網頁上的問題。 Page Inspector 這項新工具的 b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1a9e093faae2cea1c27c582e22aebc908f78addb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-visual-studio-2012"></a><span data-ttu-id="42cb3-104">Page Inspector 使用 Visual Studio 2012 中</span><span class="sxs-lookup"><span data-stu-id="42cb3-104">Using Page Inspector in Visual Studio 2012</span></span>
====================
<span data-ttu-id="42cb3-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="42cb3-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="42cb3-106">在這個實際操作實驗室中，您會發現新的工具，尋找並修正 Visual Studio-Page Inspector 中的網頁上的問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-106">In this Hands-on Lab, you will discover a new tool to find and fix web page issues in Visual Studio - the Page Inspector.</span></span>
> 
> <span data-ttu-id="42cb3-107">Page Inspector 是一種新的工具，將瀏覽器診斷工具整合到 Visual Studio，並提供在瀏覽器、 ASP.NET 和原始程式碼之間的整合式的經驗。</span><span class="sxs-lookup"><span data-stu-id="42cb3-107">Page Inspector is a new tool that brings browser diagnostics tools to Visual Studio and provides an integrated experience among the browser, ASP.NET, and source code.</span></span> <span data-ttu-id="42cb3-108">它會呈現網頁 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁） 直接在 Visual Studio IDE，並可讓您檢查原始程式碼與產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="42cb3-108">It renders a web page (HTML, Web Forms, ASP.NET MVC, or Web Pages) directly within the Visual Studio IDE and lets you examine both the source code and the resulting output.</span></span> <span data-ttu-id="42cb3-109">Page Inspector 可讓您輕鬆分解網站，快速建置從頭頁面，以及快速診斷問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-109">Page Inspector enables you to easily decompose a website, rapidly build pages from the ground up, and quickly diagnose issues.</span></span>
> 
> <span data-ttu-id="42cb3-110">Screen 鍵我們有個及時，例如 ASP.NET MVC 和 WebForms 建立彈性且可擴充的網站的 Web 架構的數字。</span><span class="sxs-lookup"><span data-stu-id="42cb3-110">Nowadays we have a number of Web frameworks that create flexible and scalable websites in a timely manner, such as ASP.NET MVC and WebForms.</span></span> <span data-ttu-id="42cb3-111">相反地，它會取得很難找到問題的頁面上，因為 IDE 不支援在 template 為基礎的網頁和動態內容中的設計工具檢視。</span><span class="sxs-lookup"><span data-stu-id="42cb3-111">On the other hand, it gets harder to find issues on the pages because the IDE does not support the designer view in template-based pages and dynamic content.</span></span> <span data-ttu-id="42cb3-112">因此，這些網站有要查看它們在使用者的瀏覽器中開啟。</span><span class="sxs-lookup"><span data-stu-id="42cb3-112">Therefore, these websites have to be opened in a browser to see how they appear to a user.</span></span>
> 
> <span data-ttu-id="42cb3-113">Web 開發人員會使用外部工具來尋找定期瀏覽器中執行的問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-113">Web developers use external tools to find issues that regularly run in the browser.</span></span> <span data-ttu-id="42cb3-114">然後，它們會返回 IDE，並開始修正。</span><span class="sxs-lookup"><span data-stu-id="42cb3-114">Then, they return to the IDE and start fixing.</span></span> <span data-ttu-id="42cb3-115">這來回 IDE、 瀏覽器和程式碼剖析工具之間的活動可以效率不佳，而且有時候需要重新部署和快取清除每的次您想要重現問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-115">This back and forth activity among the IDE, the browser and the profiling tools can be inefficient, and sometimes requires a fresh deployment and cache cleaning each time you want to reproduce an issue.</span></span>
> 
> <span data-ttu-id="42cb3-116">Page Inspector 將結合在一起使用的功能集結合這兩個領域的最佳橋接器中 Web 程式開發 （瀏覽器工具） 的用戶端和伺服器 （ASP.NET 和原始程式碼的程式碼） 之間的間距。</span><span class="sxs-lookup"><span data-stu-id="42cb3-116">Page Inspector bridges a gap in Web development between the client (browser tools) and the server (ASP.NET and source code) by bringing together the best of both worlds using a combined set of features.</span></span>
> 
> <span data-ttu-id="42cb3-117">使用 Page Inspector 時，您可以查看哪些項目 （包括伺服器端程式碼） 的原始程式檔中已產生要在瀏覽器中呈現的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="42cb3-117">Using Page Inspector, you can see which elements in the source files (including server-side code) have produced the HTML markup to be rendered in the browser.</span></span> <span data-ttu-id="42cb3-118">Page Inspector 也可讓您修改 CSS 屬性與 DOM 項目屬性，才能看到這些變更會立即反映在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="42cb3-118">Page Inspector also lets you modify CSS properties and DOM element attributes to see the changes reflected immediately in the browser.</span></span>
> 
> <span data-ttu-id="42cb3-119">這個實際操作實驗室將引導您完成 Page Inspector 功能，並示範如何使用它們以 Web 應用程式中修正問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-119">This hands-on lab will walk you through the Page Inspector features and show you how you can use them to fix issues in Web applications.</span></span> <span data-ttu-id="42cb3-120">**這個實驗室中包含兩個練習使用類似的流程，但目標為不同的技術。如果您是 ASP.NET MVC 開發人員，請依照下列練習中，如果您是兩個 WebForms 開發人員依照練習**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-120">**This lab contains two exercises using similar flows but targeting different technologies. If you are an ASP.NET MVC Developer, follow exercise one; if you are a WebForms developer follow exercise two**.</span></span>
> 
> <span data-ttu-id="42cb3-121">這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。</span><span class="sxs-lookup"><span data-stu-id="42cb3-121">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="42cb3-122">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="42cb3-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="42cb3-123">目標</span><span class="sxs-lookup"><span data-stu-id="42cb3-123">Objectives</span></span>

<span data-ttu-id="42cb3-124">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="42cb3-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="42cb3-125">分解使用 Page Inspector 的網站</span><span class="sxs-lookup"><span data-stu-id="42cb3-125">Decompose a Web Site using Page Inspector</span></span>
- <span data-ttu-id="42cb3-126">檢查並預覽 Page inspector 的 CSS 樣式變更</span><span class="sxs-lookup"><span data-stu-id="42cb3-126">Inspect and preview CSS styles changes with Page Inspector</span></span>
- <span data-ttu-id="42cb3-127">偵測及修正您使用 Page Inspector 的網頁中的問題</span><span class="sxs-lookup"><span data-stu-id="42cb3-127">Detect and fix issues in your web pages using Page Inspector</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="42cb3-128">必要條件</span><span class="sxs-lookup"><span data-stu-id="42cb3-128">Prerequisites</span></span>

<span data-ttu-id="42cb3-129">您必須具備下列項目才能完成本實驗室：</span><span class="sxs-lookup"><span data-stu-id="42cb3-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="42cb3-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="42cb3-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="42cb3-131">Internet Explorer 9 或更高版本</span><span class="sxs-lookup"><span data-stu-id="42cb3-131">Internet Explorer 9 or higher</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="42cb3-132">練習</span><span class="sxs-lookup"><span data-stu-id="42cb3-132">Exercises</span></span>

<span data-ttu-id="42cb3-133">這個實際操作實驗室包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="42cb3-133">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="42cb3-134">練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="42cb3-134">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>](#Exercise1)
2. [<span data-ttu-id="42cb3-135">練習 2： 在 WebForms 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="42cb3-135">Exercise 2: Using Page Inspector in WebForms Projects</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="42cb3-136">每個練習會伴隨起始方案，練習，可讓您依照每個練習各自的 Begin 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-136">Each exercise is accompanied by a starting solution, located in the Begin folder of the exercise, that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="42cb3-137">內部練習的原始程式碼，您也可以找到包含在 Visual Studio 方案中使用的程式碼所產生對應的練習中的步驟結束資料夾。</span><span class="sxs-lookup"><span data-stu-id="42cb3-137">Inside the source code for an exercise, you will also find an End folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="42cb3-138">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案做為指導。</span><span class="sxs-lookup"><span data-stu-id="42cb3-138">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


<span data-ttu-id="42cb3-139">完成本實驗室估計時間： **30 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-139">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a><span data-ttu-id="42cb3-140">練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="42cb3-140">Exercise 1: Using Page Inspector in ASP.NET MVC Projects</span></span>

<span data-ttu-id="42cb3-141">在此練習中，您將學習如何預覽和偵錯**ASP.NET MVC 4**方案中使用**Page Inspector**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-141">In this exercise, you will learn how to preview and debug an **ASP.NET MVC 4** solution using **Page Inspector**.</span></span> <span data-ttu-id="42cb3-142">首先，您將會執行一簡短圈周圍工具若要了解促進 Web 偵錯處理序的功能。</span><span class="sxs-lookup"><span data-stu-id="42cb3-142">First, you will perform a brief lap around the tool to learn the features that facilitate the Web debugging process.</span></span> <span data-ttu-id="42cb3-143">然後，您會使用包含樣式問題的網頁。</span><span class="sxs-lookup"><span data-stu-id="42cb3-143">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="42cb3-144">您將學習如何使用 Page Inspector 尋找會產生問題的原始程式碼，並加以修正。</span><span class="sxs-lookup"><span data-stu-id="42cb3-144">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="42cb3-145">工作 1-Page Inspector 瀏覽</span><span class="sxs-lookup"><span data-stu-id="42cb3-145">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="42cb3-146">在這項工作，您將學習如何使用 Page Inspector 顯示相片圖庫的 ASP.NET MVC 4 專案的內容中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-146">In this task, you will learn how to use the Page Inspector in the context of an ASP.NET MVC 4 project that shows a photo gallery.</span></span>

1. <span data-ttu-id="42cb3-147">開啟**開始**方案位於**來源/Ex1MVC4/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="42cb3-147">Open the **Begin** solution located at **Source/Ex1-MVC4/Begin/** folder.</span></span>

    1. <span data-ttu-id="42cb3-148">您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="42cb3-148">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="42cb3-149">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-149">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="42cb3-150">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="42cb3-150">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="42cb3-151">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-151">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-152">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="42cb3-152">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="42cb3-153">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="42cb3-153">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="42cb3-154">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="42cb3-154">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="42cb3-155">在 [方案總管] 中，找出**Index.cshtml**檢視下**/檢視表/首頁**專案資料夾中，以滑鼠右鍵按一下並選取**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-155">In the Solution Explorer, locate **Index.cshtml** view under the **/Views/Home** project folder, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="42cb3-156">![選取要在 Page Inspector 中預覽檔案](using-page-inspector-in-visual-studio-2012/_static/image1.png "選取要在 Page Inspector 中的檔案")</span><span class="sxs-lookup"><span data-stu-id="42cb3-156">![Selecting a file to preview in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "Selecting a file to preview in Page Inspector")</span></span>

    <span data-ttu-id="42cb3-157">*選取要在 Page Inspector 中的檔案*</span><span class="sxs-lookup"><span data-stu-id="42cb3-157">*Selecting a file to preview in Page Inspector*</span></span>
3. <span data-ttu-id="42cb3-158">Page Inspector 視窗會顯示*/Home/Index* URL 對應至來源您選取的檢視。</span><span class="sxs-lookup"><span data-stu-id="42cb3-158">The Page Inspector window will show the */Home/Index* URL mapped to the source View you selected.</span></span>

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    <span data-ttu-id="42cb3-160">*Page inspector 的第一個連絡人*</span><span class="sxs-lookup"><span data-stu-id="42cb3-160">*The first contact with Page Inspector*</span></span>

    <span data-ttu-id="42cb3-161">Page Inspector 工具整合在 Visual Studio 環境中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-161">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="42cb3-162">偵測器包含內嵌的瀏覽器，以及一個功能強大的 HTML 分析工具。</span><span class="sxs-lookup"><span data-stu-id="42cb3-162">The inspector contains an embedded browser, together with a powerful HTML profiler.</span></span> <span data-ttu-id="42cb3-163">請注意，您不需要執行此方案，以查看您的網頁的外觀。</span><span class="sxs-lookup"><span data-stu-id="42cb3-163">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-164">Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。</span><span class="sxs-lookup"><span data-stu-id="42cb3-164">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="42cb3-165">如果發生這種情況，調整 Page Inspector 的寬度。</span><span class="sxs-lookup"><span data-stu-id="42cb3-165">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="42cb3-166">按一下**檔案**Page Inspector 中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="42cb3-166">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="42cb3-167">您會看到所撰寫的索引頁面的所有原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="42cb3-167">You will see all the source files that are composing the Index page.</span></span> <span data-ttu-id="42cb3-168">這項功能有助於識別您一眼的所有項目，特別是當您正在使用的部分檢視和範本。</span><span class="sxs-lookup"><span data-stu-id="42cb3-168">This feature helps to identify all the elements at a glance, especially when you are working with partial views and templates.</span></span> <span data-ttu-id="42cb3-169">請注意，您也可以開啟每個檔案是否按的連結。</span><span class="sxs-lookup"><span data-stu-id="42cb3-169">Notice that you can also open each of the files if you click the links.</span></span>

    ![檔案-索引標籤](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    <span data-ttu-id="42cb3-171">*[檔案] 索引標籤*</span><span class="sxs-lookup"><span data-stu-id="42cb3-171">*The Files tab*</span></span>
5. <span data-ttu-id="42cb3-172">按一下**切換檢查模式**位於索引標籤左邊的按鈕。</span><span class="sxs-lookup"><span data-stu-id="42cb3-172">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="42cb3-173">此工具可讓您選取網頁的任何項目，並查看其 HTML 和 Razor 程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-173">This tool will let you select any element of the page and see its HTML and Razor code.</span></span>

    ![切換檢查-模式按鈕](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    <span data-ttu-id="42cb3-175">*切換檢查模式按鈕*</span><span class="sxs-lookup"><span data-stu-id="42cb3-175">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="42cb3-176">在 Page Inspector 瀏覽器中，滑鼠指標移到頁面項目上。</span><span class="sxs-lookup"><span data-stu-id="42cb3-176">In the Page Inspector browser, move the mouse pointer over the page elements.</span></span> <span data-ttu-id="42cb3-177">當您移動滑鼠指標移到所呈現頁面的任意處時，就會顯示項目類型和對應的來源標記或程式碼會反白顯示在 Visual Studio 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-177">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    <span data-ttu-id="42cb3-179">*檢查模式作用中*</span><span class="sxs-lookup"><span data-stu-id="42cb3-179">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-180">不會最大化 Page Inspector 視窗或您無法看到預覽索引標籤顯示的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-180">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="42cb3-181">如果它最大化時，您可以按一下 Page Inspector 中的項目，顯示選取項目的原始碼，但是它將會隱藏頁面偵測器視窗。</span><span class="sxs-lookup"><span data-stu-id="42cb3-181">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="42cb3-182">如果您注意**Index.cshtml**檔案中，您會注意到產生選取的項目之原始程式碼的一部分會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="42cb3-182">If you pay attention to the **Index.cshtml** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="42cb3-183">這項功能可協助長的來源檔案，提供直接和快速的方式來存取這些程式碼的編輯。</span><span class="sxs-lookup"><span data-stu-id="42cb3-183">This feature facilitates the editing of long source files, providing a direct and fast way to access the code.</span></span>

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    <span data-ttu-id="42cb3-185">*檢查項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-185">*Inspecting elements*</span></span>
7. <span data-ttu-id="42cb3-186">按一下**切換檢查模式**按鈕 (![選取 [HTML] 索引標籤顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。](using-page-inspector-in-visual-studio-2012/_static/image7.png "選取 [HTML] 索引標籤顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。")</span><span class="sxs-lookup"><span data-stu-id="42cb3-186">Click the **Toggle Inspection Mode** button (![Select the HTML tab to display the HTML code rendered in the Page Inspector browser.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Select the HTML tab to display the HTML code rendered in the Page Inspector browser.")</span></span> <span data-ttu-id="42cb3-187">) 可停用資料指標。</span><span class="sxs-lookup"><span data-stu-id="42cb3-187">) to disable the cursor.</span></span>
8. <span data-ttu-id="42cb3-188">選取**HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-188">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="42cb3-189">在 HTML 標記中，找出 Koala 連結的清單項目並選取它。</span><span class="sxs-lookup"><span data-stu-id="42cb3-189">In the HTML markup, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="42cb3-190">請注意，當您選取的程式碼時，對應的輸出自動反白顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-190">Notice that when you select the code, the corresponding output is automatically highlighted in the browser.</span></span> <span data-ttu-id="42cb3-191">此功能十分有用，若要查看如何轉譯頁面上的 HTML 區塊。</span><span class="sxs-lookup"><span data-stu-id="42cb3-191">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="42cb3-192">![在頁面中的選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image8.png "頁面中的選取 HTML 項目")</span><span class="sxs-lookup"><span data-stu-id="42cb3-192">![Selecting HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image8.png "Selecting HTML element in the page")</span></span>

    <span data-ttu-id="42cb3-193">*頁面中選取 HTML 項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-193">*Selecting HTML element in the page*</span></span>
10. <span data-ttu-id="42cb3-194">按一下**切換檢查模式**按鈕以啟用*檢查模式*按一下導覽列。</span><span class="sxs-lookup"><span data-stu-id="42cb3-194">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="42cb3-195">右邊的 [樣式] 窗格中的 HTML 程式碼，您會看到套用至選取的元素的 CSS 樣式清單。</span><span class="sxs-lookup"><span data-stu-id="42cb3-195">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-196">由於標頭是站台版面配置的一部分，也會開啟 Page Inspector \_Layout.cshtml 檔案並反白顯示程式碼的區段會受到影響。</span><span class="sxs-lookup"><span data-stu-id="42cb3-196">Since the header is a part of the site layout, Page Inspector will also open \_Layout.cshtml file and highlight the segment of code affected.</span></span>

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    <span data-ttu-id="42cb3-198">*探索樣式和原始程式檔的選取項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-198">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="42cb3-199">切換檢查指標已啟用，將滑鼠指標在藍色精選列下方然後按一下 半圓形。</span><span class="sxs-lookup"><span data-stu-id="42cb3-199">With the toggle inspection pointer enabled, move the mouse pointer below the blue featured bar and click the half circle.</span></span>

    <span data-ttu-id="42cb3-200">![選取項目](using-page-inspector-in-visual-studio-2012/_static/image10.png "選取項目")</span><span class="sxs-lookup"><span data-stu-id="42cb3-200">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image10.png "Selecting an element")</span></span>

    <span data-ttu-id="42cb3-201">*選取項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-201">*Selecting an element*</span></span>
12. <span data-ttu-id="42cb3-202">在 [樣式] 窗格中，找出**背景影像**項目底下**.main 內容**群組。</span><span class="sxs-lookup"><span data-stu-id="42cb3-202">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="42cb3-203">**取消核取****背景影像**看。</span><span class="sxs-lookup"><span data-stu-id="42cb3-203">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="42cb3-204">您會注意到瀏覽器會立即反映變更，並隱藏圓形。</span><span class="sxs-lookup"><span data-stu-id="42cb3-204">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-205">您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。</span><span class="sxs-lookup"><span data-stu-id="42cb3-205">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="42cb3-206">您可以取消核取樣式，或變更其值依需求多次，但將會還原之後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="42cb3-206">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="42cb3-207">![啟用和停用的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image11.png "啟用和停用的 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="42cb3-207">![Enabling and disabling CSS styles](using-page-inspector-in-visual-studio-2012/_static/image11.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="42cb3-208">*啟用和停用的 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="42cb3-208">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="42cb3-209">現在，按一下 '**您的標誌**' 上使用檢查模式的標頭的文字。</span><span class="sxs-lookup"><span data-stu-id="42cb3-209">Now, click the '**your logo here**' text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="42cb3-210">在**樣式**索引標籤上，找出**字型大小**CSS 屬性下**.site 標題**群組。</span><span class="sxs-lookup"><span data-stu-id="42cb3-210">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="42cb3-211">按兩下屬性值，並取代 2.3 em 值與**3 em**，然後按下**ENTER**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-211">Double-click the attribute value and replace the 2.3 em value with **3 em**, and then press **ENTER**.</span></span> <span data-ttu-id="42cb3-212">請注意，標題起來更大。</span><span class="sxs-lookup"><span data-stu-id="42cb3-212">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="42cb3-213">![變更頁面偵測器中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 中的變更的 CSS 值")</span><span class="sxs-lookup"><span data-stu-id="42cb3-213">![Changing CSS values in the Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="42cb3-214">*Page Inspector 中變更的 CSS 值*</span><span class="sxs-lookup"><span data-stu-id="42cb3-214">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="42cb3-215">按一下**追蹤樣式**索引標籤上，Page Inspector 的右窗格中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-215">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="42cb3-216">這是以查看所有的樣式套用至選取範圍中，依屬性名稱的替代方法。</span><span class="sxs-lookup"><span data-stu-id="42cb3-216">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    <span data-ttu-id="42cb3-218">*CSS 樣式追蹤 選取的項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-218">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="42cb3-219">Page Inspector 的另一個功能是 [配置] 窗格。</span><span class="sxs-lookup"><span data-stu-id="42cb3-219">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="42cb3-220">使用檢查模式時，選取導覽列，然後按一下**配置**在右窗格中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="42cb3-220">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="42cb3-221">您會看到的選取項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。</span><span class="sxs-lookup"><span data-stu-id="42cb3-221">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="42cb3-222">請注意，您也可以修改此檢視中的值。</span><span class="sxs-lookup"><span data-stu-id="42cb3-222">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="42cb3-223">![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 中的項目配置")</span><span class="sxs-lookup"><span data-stu-id="42cb3-223">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="42cb3-224">*Page Inspector 中的項目配置*</span><span class="sxs-lookup"><span data-stu-id="42cb3-224">*Element layout in Page Inspector*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="42cb3-225">工作 2-尋找和修正相片圖庫中的樣式問題</span><span class="sxs-lookup"><span data-stu-id="42cb3-225">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="42cb3-226">您要如何診斷與舊版 Visual Studio 網頁問題？</span><span class="sxs-lookup"><span data-stu-id="42cb3-226">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="42cb3-227">也可能已經熟悉 web 偵錯在 Visual Studio IDE，像是 Internet Explorer 開發人員工具或 firebug 這類外部執行的工具。</span><span class="sxs-lookup"><span data-stu-id="42cb3-227">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="42cb3-228">瀏覽器只了解 HTML、 指令碼和樣式，而基礎架構會產生將呈現的 HTML。</span><span class="sxs-lookup"><span data-stu-id="42cb3-228">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="42cb3-229">因此，您通常需要部署整個站台，以查看網頁外觀如何。</span><span class="sxs-lookup"><span data-stu-id="42cb3-229">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="42cb3-230">當您想要偵測及修正問題，您的網站中，您可能必須遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="42cb3-230">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="42cb3-231">從 Visual Studio 中，執行此方案，或部署在 web 伺服器上的頁面。</span><span class="sxs-lookup"><span data-stu-id="42cb3-231">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="42cb3-232">在瀏覽器中開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="42cb3-232">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="42cb3-233">若要尋找相關檔案，您會使用&quot;搜尋&quot;或&quot;檔案中的搜尋&quot;的樣式類別名稱的功能。</span><span class="sxs-lookup"><span data-stu-id="42cb3-233">To find the files involved, you would have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="42cb3-234">一旦偵測到錯誤時，停止 Web 瀏覽器和伺服器。</span><span class="sxs-lookup"><span data-stu-id="42cb3-234">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="42cb3-235">清除瀏覽器快取。</span><span class="sxs-lookup"><span data-stu-id="42cb3-235">Clear the browser cache.</span></span>
5. <span data-ttu-id="42cb3-236">返回 Visual Studio 套用修正程式。</span><span class="sxs-lookup"><span data-stu-id="42cb3-236">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="42cb3-237">重複執行測試的所有步驟。</span><span class="sxs-lookup"><span data-stu-id="42cb3-237">Repeat all the steps to test.</span></span>

<span data-ttu-id="42cb3-238">由於 ASP.NET MVC 4 中沒有實際 WYSIWYG，大部分的樣式問題上偵測到後續階段中，執行或部署 web 應用程式之後。</span><span class="sxs-lookup"><span data-stu-id="42cb3-238">As there is no real WYSIWYG in ASP.NET MVC 4, most of the style issues are detected on a later stage, after running or deploying the web application.</span></span> <span data-ttu-id="42cb3-239">現在，Page inspector，便可預覽任何頁面而無須執行方案。</span><span class="sxs-lookup"><span data-stu-id="42cb3-239">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="42cb3-240">在這項工作，您將使用 Page inspector，並修正相片圖庫應用程式的一些問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-240">In this task, you will use the Page inspector and fix some issues the Photo Gallery application.</span></span>

1. <span data-ttu-id="42cb3-241">使用 Page Inspector 找出**註冊**和**登入**標頭的左側的連結。</span><span class="sxs-lookup"><span data-stu-id="42cb3-241">Using Page Inspector, locate the **Register** and the **Log in** links at the left side of the header.</span></span>

    <span data-ttu-id="42cb3-242">請注意，連結不會顯示在右側，預期的位置都會顯示在類似項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="42cb3-242">Notice that the links are not displayed at the expected place on the right, and they are shown like a bulleted list.</span></span> <span data-ttu-id="42cb3-243">您現在將會對齊右邊的連結，並據此重新加以設定樣式。</span><span class="sxs-lookup"><span data-stu-id="42cb3-243">You will now align the links to the right and restyle them accordingly.</span></span>

    <span data-ttu-id="42cb3-244">![尋找暫存器和記錄檔中連結](using-page-inspector-in-visual-studio-2012/_static/image15.png "尋找暫存器和記錄檔中的連結")</span><span class="sxs-lookup"><span data-stu-id="42cb3-244">![Locating the Register and Log in links](using-page-inspector-in-visual-studio-2012/_static/image15.png "Locating the Register and Log in links")</span></span>

    <span data-ttu-id="42cb3-245">*尋找暫存器和記錄檔中的連結*</span><span class="sxs-lookup"><span data-stu-id="42cb3-245">*Locating the Register and Log in links*</span></span>
2. <span data-ttu-id="42cb3-246">切換檢查模式中選取，按一下 [關閉] 以，但不是能在註冊連結以開啟其程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-246">With Toggle Inspection Mode selected, click close to, but not on, the Register link to open its code.</span></span>

    <span data-ttu-id="42cb3-247">請注意，連結的原始程式碼位於 **\_LoginPartial.cshtml**檔案，不 Index.cshtml 和\_Layout.cshtml，也就是您可能會在第一個位置中尋找的位置。</span><span class="sxs-lookup"><span data-stu-id="42cb3-247">Notice that the source code of the links is located in the **\_LoginPartial.cshtml** file, not the Index.cshtml nor the \_Layout.cshtml, which are the places you might look in first place.</span></span> <span data-ttu-id="42cb3-248">您在已直接在正確的來源檔案中放置。</span><span class="sxs-lookup"><span data-stu-id="42cb3-248">You have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="42cb3-249">在**樣式**索引標籤上，找出並按一下 **<section> #login</section>** 是這些連結的 HTML 容器項目。</span><span class="sxs-lookup"><span data-stu-id="42cb3-249">In the **Styles** tab, locate and click the **<section> #login</section>** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="42cb3-250">請注意， **#login**樣式自動位於**Site.css**按一下之後。</span><span class="sxs-lookup"><span data-stu-id="42cb3-250">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="42cb3-251">此外，程式碼現在會反白。</span><span class="sxs-lookup"><span data-stu-id="42cb3-251">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="42cb3-252">![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image16.png "選取的 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="42cb3-252">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image16.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="42cb3-253">*選取的 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="42cb3-253">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="42cb3-254">請取消註解**文字對齊**屬性反白顯示的程式碼中藉由移除開頭和結尾字元，並儲存**Site.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="42cb3-254">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="42cb3-255">Page Inspector 可感知撰寫目前的頁面上，所有不同檔案的而且它可以偵測到任何這些檔案的變更時。</span><span class="sxs-lookup"><span data-stu-id="42cb3-255">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="42cb3-256">它會提醒您目前的網頁瀏覽器中不會與原始程式檔同步時。</span><span class="sxs-lookup"><span data-stu-id="42cb3-256">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="42cb3-257">在 Page Inspector 瀏覽器中，按一下下方重新載入該頁面的 [網址] 列的列。</span><span class="sxs-lookup"><span data-stu-id="42cb3-257">In the Page Inspector browser, click the bar located below the address bar to reload the page.</span></span>

    ![重新載入該頁面](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    <span data-ttu-id="42cb3-259">*重新載入該頁面*</span><span class="sxs-lookup"><span data-stu-id="42cb3-259">*Reloading the page*</span></span>

    <span data-ttu-id="42cb3-260">連結現在會在右側，但仍是它們看起來像是分項清單。</span><span class="sxs-lookup"><span data-stu-id="42cb3-260">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="42cb3-261">現在，您將會移除項目符號，水平對齊的連結。</span><span class="sxs-lookup"><span data-stu-id="42cb3-261">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新的頁面](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    <span data-ttu-id="42cb3-263">*更新的頁面*</span><span class="sxs-lookup"><span data-stu-id="42cb3-263">*Updated page*</span></span>
6. <span data-ttu-id="42cb3-264">使用檢查模式中，選取任一 **&lt;li&gt;** 包含的項目&quot;註冊&quot;和&quot;登入&quot;連結。</span><span class="sxs-lookup"><span data-stu-id="42cb3-264">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="42cb3-265">然後，按一下  **&lt;區段&gt;#login**存取項目**Styles.css**程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-265">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="42cb3-266">![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image19.png "尋找樣式")</span><span class="sxs-lookup"><span data-stu-id="42cb3-266">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image19.png "Finding the style")</span></span>

    <span data-ttu-id="42cb3-267">*尋找樣式*</span><span class="sxs-lookup"><span data-stu-id="42cb3-267">*Finding the style*</span></span>
7. <span data-ttu-id="42cb3-268">在**Style.css**，請取消註解的程式碼**#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="42cb3-268">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="42cb3-269">您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。</span><span class="sxs-lookup"><span data-stu-id="42cb3-269">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="42cb3-270">![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image20.png "右側的登入連結")</span><span class="sxs-lookup"><span data-stu-id="42cb3-270">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image20.png "Restyling the login links")</span></span>

    <span data-ttu-id="42cb3-271">*右側的登入連結*</span><span class="sxs-lookup"><span data-stu-id="42cb3-271">*Restyling the login links*</span></span>
8. <span data-ttu-id="42cb3-272">儲存**Style.css**檔案，然後按一下下方重新載入該頁面的位址列的一次。</span><span class="sxs-lookup"><span data-stu-id="42cb3-272">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="42cb3-273">請注意，連結會正確出現。</span><span class="sxs-lookup"><span data-stu-id="42cb3-273">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="42cb3-274">![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image21.png "連結靠右對齊")</span><span class="sxs-lookup"><span data-stu-id="42cb3-274">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="42cb3-275">*靠右對齊的連結*</span><span class="sxs-lookup"><span data-stu-id="42cb3-275">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="42cb3-276">最後，您將變更標頭標題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-276">Finally, you will change the header title.</span></span> <span data-ttu-id="42cb3-277">用於檢查模式按一下**您的標誌**文字和 get 會產生它的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-277">Use the inspection mode to click **your logo here** text and get to the source code that generates it.</span></span>
10. <span data-ttu-id="42cb3-278">現在您已在 **\_Layout.cshtml**，取代 '**您的標誌**'text 與'**相片圖庫**'。</span><span class="sxs-lookup"><span data-stu-id="42cb3-278">Now you are in **\_Layout.cshtml**, replace '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="42cb3-279">儲存並更新 Page Inspector 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="42cb3-279">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="42cb3-280">![指派新的標題](using-page-inspector-in-visual-studio-2012/_static/image22.png "指派新的標題")</span><span class="sxs-lookup"><span data-stu-id="42cb3-280">![Assigning a new title](using-page-inspector-in-visual-studio-2012/_static/image22.png "Assigning a new title")</span></span>

    <span data-ttu-id="42cb3-281">*指派新的標題*</span><span class="sxs-lookup"><span data-stu-id="42cb3-281">*Assigning a new title*</span></span>

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    <span data-ttu-id="42cb3-283">*相片圖庫頁面更新*</span><span class="sxs-lookup"><span data-stu-id="42cb3-283">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="42cb3-284">最後，零件**PhotoGallery**專案並按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="42cb3-284">Finally, selet the **PhotoGallery** project and press **F5** to run the app.</span></span> <span data-ttu-id="42cb3-285">簽出所有如預期般運作所做的變更。</span><span class="sxs-lookup"><span data-stu-id="42cb3-285">Check out all the changes work as expected.</span></span>

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a><span data-ttu-id="42cb3-286">練習 2： 在 WebForms 專案中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="42cb3-286">Exercise 2: Using Page Inspector in WebForms Projects</span></span>

<span data-ttu-id="42cb3-287">在此練習中，您將學習如何預覽並使用 Page Inspector WebForms 方案進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="42cb3-287">In this exercise, you will learn how to preview and debug a WebForms solution using Page Inspector.</span></span> <span data-ttu-id="42cb3-288">您第一次將執行若要了解 Page Inspector 功能來協助偵錯處理序 Web 簡短圈周圍的工具。</span><span class="sxs-lookup"><span data-stu-id="42cb3-288">You will first perform a brief lap around the tool to learn the Page Inspector features that facilitate the Web debugging process.</span></span> <span data-ttu-id="42cb3-289">然後，您會使用包含樣式問題的網頁。</span><span class="sxs-lookup"><span data-stu-id="42cb3-289">Then, you will work in a web page that contains styling issues.</span></span> <span data-ttu-id="42cb3-290">您將學習如何使用 Page Inspector 尋找會產生問題的原始程式碼，並加以修正。</span><span class="sxs-lookup"><span data-stu-id="42cb3-290">You will learn how to use Page Inspector to find the source code that generates the issue and fix it.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a><span data-ttu-id="42cb3-291">工作 1-Page Inspector 瀏覽</span><span class="sxs-lookup"><span data-stu-id="42cb3-291">Task 1 - Exploring Page Inspector</span></span>

<span data-ttu-id="42cb3-292">在這項工作，您將學習如何在相片圖庫會顯示在 WebForms 專案內容中使用 Page Inspector 功能。</span><span class="sxs-lookup"><span data-stu-id="42cb3-292">In this task, you will learn how to use the Page Inspector features in the context of a WebForms project that shows a photo gallery.</span></span>

1. <span data-ttu-id="42cb3-293">開啟**開始**方案位於**來源/Ex2WebForms/開始/**資料夾。</span><span class="sxs-lookup"><span data-stu-id="42cb3-293">Open the **Begin** solution located at **Source/Ex2-WebForms/Begin/** folder.</span></span>

    1. <span data-ttu-id="42cb3-294">您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="42cb3-294">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="42cb3-295">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-295">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="42cb3-296">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="42cb3-296">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="42cb3-297">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-297">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-298">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="42cb3-298">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="42cb3-299">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="42cb3-299">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="42cb3-300">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="42cb3-300">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="42cb3-301">在 [方案總管] 中，找出**Default.aspx**頁面上，以滑鼠右鍵按一下它，然後選取**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-301">In the Solution Explorer, locate **Default.aspx** page, right-click it and select **View in Page Inspector**.</span></span>

    <span data-ttu-id="42cb3-302">![Page inspector 開啟 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "Page inspector 開啟 Default.aspx")</span><span class="sxs-lookup"><span data-stu-id="42cb3-302">![Opening Default.aspx with Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "Opening Default.aspx with Page Inspector")</span></span>

    <span data-ttu-id="42cb3-303">*開啟 Default.aspx，Page inspector*</span><span class="sxs-lookup"><span data-stu-id="42cb3-303">*Opening Default.aspx with Page Inspector*</span></span>
3. <span data-ttu-id="42cb3-304">Page Inspector 視窗會顯示 Default.aspx。</span><span class="sxs-lookup"><span data-stu-id="42cb3-304">The Page Inspector window will show Default.aspx.</span></span>

    <span data-ttu-id="42cb3-305">![在 Page Inspector 中檢視 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 中檢視 Default.aspx")</span><span class="sxs-lookup"><span data-stu-id="42cb3-305">![Viewing Default.aspx in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "Viewing Default.aspx in Page Inspector")</span></span>

    <span data-ttu-id="42cb3-306">*在 Page Inspector 中檢視 Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="42cb3-306">*Viewing Default.aspx in Page Inspector*</span></span>

    <span data-ttu-id="42cb3-307">Page Inspector 工具整合在 Visual Studio 環境中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-307">The Page Inspector tool is integrated in your Visual Studio environment.</span></span> <span data-ttu-id="42cb3-308">偵測器包含內嵌的瀏覽器，以及功能強大的 HTML 分析工具會顯示選取的程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-308">The inspector contains an embedded browser, together with a powerful HTML profiler that will show the selected code.</span></span> <span data-ttu-id="42cb3-309">請注意，您不需要執行此方案，以查看您的網頁的外觀。</span><span class="sxs-lookup"><span data-stu-id="42cb3-309">Notice that you do not have to run the solution to see how your pages look.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-310">Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。</span><span class="sxs-lookup"><span data-stu-id="42cb3-310">When the width of Page Inspector browser is less than the width of the opened page, you will not see the page properly.</span></span> <span data-ttu-id="42cb3-311">如果發生這種情況，調整 Page Inspector 的寬度。</span><span class="sxs-lookup"><span data-stu-id="42cb3-311">If that happens, adjust the width of the Page Inspector.</span></span>
4. <span data-ttu-id="42cb3-312">按一下**檔案**Page Inspector 中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="42cb3-312">Click the **Files** tab in Page Inspector.</span></span>

    <span data-ttu-id="42cb3-313">您會看到正在撰寫呈現的預設網頁的所有原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="42cb3-313">You will see all the source files that are composing the rendered Default page.</span></span> <span data-ttu-id="42cb3-314">這是一個實用的功能，來識別您一眼的所有項目，特別是當您正在使用使用者控制項和主版頁面。</span><span class="sxs-lookup"><span data-stu-id="42cb3-314">This is a useful feature to identify all the elements at a glance, especially when you are working with User Controls and Master Pages.</span></span> <span data-ttu-id="42cb3-315">請注意，您也可以導覽到每個檔案。</span><span class="sxs-lookup"><span data-stu-id="42cb3-315">Notice that you can also navigate to each of the files.</span></span>

    <span data-ttu-id="42cb3-316">![[檔案] 索引標籤](using-page-inspector-in-visual-studio-2012/_static/image26.png "[檔案] 索引標籤")</span><span class="sxs-lookup"><span data-stu-id="42cb3-316">![The Files tab](using-page-inspector-in-visual-studio-2012/_static/image26.png "The Files tab")</span></span>

    <span data-ttu-id="42cb3-317">*[檔案] 索引標籤*</span><span class="sxs-lookup"><span data-stu-id="42cb3-317">*The Files tab*</span></span>
5. <span data-ttu-id="42cb3-318">按一下**切換檢查模式**位於索引標籤左邊的按鈕。</span><span class="sxs-lookup"><span data-stu-id="42cb3-318">Click the **Toggle Inspection Mode** button, located at the left of the tabs.</span></span>

    <span data-ttu-id="42cb3-319">此工具可讓您選取網頁的任何項目，並查看其 HTML 程式碼和.aspx 來源。</span><span class="sxs-lookup"><span data-stu-id="42cb3-319">This tool will let you select any element of the page and see its HTML code and .aspx source.</span></span>

    <span data-ttu-id="42cb3-320">![切換檢查模式按鈕](using-page-inspector-in-visual-studio-2012/_static/image27.png "切換檢查模式按鈕")</span><span class="sxs-lookup"><span data-stu-id="42cb3-320">![Toggle Inspection Mode button](using-page-inspector-in-visual-studio-2012/_static/image27.png "Toggle Inspection Mode button")</span></span>

    <span data-ttu-id="42cb3-321">*切換檢查模式按鈕*</span><span class="sxs-lookup"><span data-stu-id="42cb3-321">*Toggle Inspection Mode button*</span></span>
6. <span data-ttu-id="42cb3-322">在 Page Inspector 瀏覽器中，將滑鼠移頁面項目。</span><span class="sxs-lookup"><span data-stu-id="42cb3-322">In the Page Inspector browser, move the mouse over the page elements.</span></span> <span data-ttu-id="42cb3-323">當您移動滑鼠指標移到所呈現頁面的任意處時，就會顯示項目類型和對應的來源標記或程式碼會反白顯示在 Visual Studio 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-323">While you move the mouse pointer over any part of the rendered page, the element type is displayed and the corresponding source markup or code is highlighted in the Visual Studio editor.</span></span>

    <span data-ttu-id="42cb3-324">![在動作中檢查模式](using-page-inspector-in-visual-studio-2012/_static/image28.png "檢查模式作用中")</span><span class="sxs-lookup"><span data-stu-id="42cb3-324">![Inspection mode in action](using-page-inspector-in-visual-studio-2012/_static/image28.png "Inspection mode in action")</span></span>

    <span data-ttu-id="42cb3-325">*檢查模式作用中*</span><span class="sxs-lookup"><span data-stu-id="42cb3-325">*Inspection mode in action*</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-326">不會最大化 Page Inspector 視窗或您無法看到預覽索引標籤顯示的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-326">Do not maximize the Page Inspector window or you will not be able to see the preview tab showing the source code.</span></span> <span data-ttu-id="42cb3-327">如果它最大化時，您可以按一下 Page Inspector 中的項目，顯示選取項目的原始碼，但是它將會隱藏頁面偵測器視窗。</span><span class="sxs-lookup"><span data-stu-id="42cb3-327">If you click the element in Page Inspector when it is maximized, the source code of the selection will appear but it will hide the Page Inspector window.</span></span>

    <span data-ttu-id="42cb3-328">如果您注意**Default.aspx**檔案中，您會注意到產生選取的項目之原始程式碼的一部分會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="42cb3-328">If you pay attention to **Default.aspx** file, you will notice that the portion of source code that generates the selected element is highlighted.</span></span> <span data-ttu-id="42cb3-329">這項功能可協助長的原始程式檔，提供直接和快速的方式來存取這些程式碼的版本。</span><span class="sxs-lookup"><span data-stu-id="42cb3-329">This feature facilitates the edition of long source files, providing a direct and fast way to access the code.</span></span>

    <span data-ttu-id="42cb3-330">![檢查項目](using-page-inspector-in-visual-studio-2012/_static/image29.png "檢查項目")</span><span class="sxs-lookup"><span data-stu-id="42cb3-330">![Inspecting elements](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspecting elements")</span></span>

    <span data-ttu-id="42cb3-331">*檢查項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-331">*Inspecting elements*</span></span>
7. <span data-ttu-id="42cb3-332">按一下**切換檢查模式**按鈕 (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。")</span><span class="sxs-lookup"><span data-stu-id="42cb3-332">Click the **Toggle Inspection Mode** button (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.")</span></span> <span data-ttu-id="42cb3-333">)，位於 Page Inspector 的索引標籤中，若要停用資料指標。</span><span class="sxs-lookup"><span data-stu-id="42cb3-333">), located in Page Inspector tabs, to disable the cursor.</span></span>
8. <span data-ttu-id="42cb3-334">選取**HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-334">Select the **HTML** tab to display the HTML code rendered in the Page Inspector browser.</span></span>
9. <span data-ttu-id="42cb3-335">中的 HTML 程式碼中，找出 Koala 連結的清單項目並選取它。</span><span class="sxs-lookup"><span data-stu-id="42cb3-335">In the HTML code, locate the list item with the Koala link and select it.</span></span>

    <span data-ttu-id="42cb3-336">請注意，當您選取的程式碼時，對應的輸出會自動反白顯示的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="42cb3-336">Notice that when you select the code, the corresponding output is automatically highlighted browser.</span></span> <span data-ttu-id="42cb3-337">此功能十分有用，若要查看如何轉譯頁面上的 HTML 區塊。</span><span class="sxs-lookup"><span data-stu-id="42cb3-337">This feature is useful to see how an HTML block is rendered on the page.</span></span>

    <span data-ttu-id="42cb3-338">![頁面中選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image31.png "頁面中選取 HTML 項目")</span><span class="sxs-lookup"><span data-stu-id="42cb3-338">![Selecting an HTML element in the page](using-page-inspector-in-visual-studio-2012/_static/image31.png "Selecting an HTML element in the page")</span></span>

    <span data-ttu-id="42cb3-339">*頁面中選取 HTML 項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-339">*Selecting an HTML element in the page*</span></span>
10. <span data-ttu-id="42cb3-340">按一下**切換檢查模式**按鈕以啟用*檢查模式*按一下導覽列。</span><span class="sxs-lookup"><span data-stu-id="42cb3-340">Click the **Toggle Inspection Mode** button to enable *Inspection Mode* and click the navigation bar.</span></span> <span data-ttu-id="42cb3-341">右邊的 [樣式] 窗格中的 HTML 程式碼，您會看到套用至選取的元素的 CSS 樣式清單。</span><span class="sxs-lookup"><span data-stu-id="42cb3-341">On the right of the HTML code, in the Styles pane, you will see a list with the CSS styles applied to the selected element.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-342">由於標頭是站台版面配置的一部分，Page Inspector 也會開啟 Site.Master 檔案，並反白顯示受影響的程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="42cb3-342">since the header is a part of the site layout, Page Inspector will also open Site.Master file and highlight the segment of code affected.</span></span>

    <span data-ttu-id="42cb3-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "探索樣式和原始程式檔的選取項目")</span><span class="sxs-lookup"><span data-stu-id="42cb3-343">![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Discovering styles and source files of a selected element")</span></span>

    <span data-ttu-id="42cb3-344">*探索樣式和原始程式檔的選取項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-344">*Discovering styles and source files of a selected element*</span></span>
11. <span data-ttu-id="42cb3-345">切換檢查指標已啟用，移動滑鼠指標在功能表列下方，按一下空白的一半圓形。</span><span class="sxs-lookup"><span data-stu-id="42cb3-345">With the toggle inspection pointer enabled, move the mouse pointer below the menu bar and click the blank half circle.</span></span>

    <span data-ttu-id="42cb3-346">![選取項目](using-page-inspector-in-visual-studio-2012/_static/image33.png "選取項目")</span><span class="sxs-lookup"><span data-stu-id="42cb3-346">![Selecting an element](using-page-inspector-in-visual-studio-2012/_static/image33.png "Selecting an element")</span></span>

    <span data-ttu-id="42cb3-347">*選取項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-347">*Selecting an element*</span></span>
12. <span data-ttu-id="42cb3-348">在 [樣式] 窗格中，找出**背景影像**項目底下**.main 內容**群組。</span><span class="sxs-lookup"><span data-stu-id="42cb3-348">In the Styles pane, locate the **background-image** item under the **.main-content** group.</span></span> <span data-ttu-id="42cb3-349">**取消核取****背景影像**看。</span><span class="sxs-lookup"><span data-stu-id="42cb3-349">**Uncheck** the **background-image** and see what happens.</span></span> <span data-ttu-id="42cb3-350">您會注意到瀏覽器會立即反映變更，並隱藏圓形。</span><span class="sxs-lookup"><span data-stu-id="42cb3-350">You will notice that the browser will reflect the changes immediately and the circle is hidden.</span></span>

    > [!NOTE]
    > <span data-ttu-id="42cb3-351">您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。</span><span class="sxs-lookup"><span data-stu-id="42cb3-351">The changes you apply on the Page Inspector Styles tab do not affect the original stylesheet.</span></span> <span data-ttu-id="42cb3-352">您可以取消核取樣式，或變更其值依需求多次，但將會還原之後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="42cb3-352">You can uncheck styles or change their values as many times as you want, but they will be restored after refreshing the page.</span></span>

    <span data-ttu-id="42cb3-353">![啟用和停用 CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "啟用和停用的 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="42cb3-353">![Enabling and disabling CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Enabling and disabling CSS styles")</span></span>

    <span data-ttu-id="42cb3-354">*啟用和停用的 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="42cb3-354">*Enabling and disabling CSS styles*</span></span>
13. <span data-ttu-id="42cb3-355">現在，按一下 '**您****的標誌在此'**上使用檢查模式的標頭的文字。</span><span class="sxs-lookup"><span data-stu-id="42cb3-355">Now, click the '**your** **logo here'** text on the header using the inspection mode.</span></span>
14. <span data-ttu-id="42cb3-356">在**樣式**索引標籤上，找出**字型大小**CSS 屬性下**.site 標題**群組。</span><span class="sxs-lookup"><span data-stu-id="42cb3-356">In the **Styles** tab, locate the **font-size** CSS attribute under the **.site-title** group.</span></span> <span data-ttu-id="42cb3-357">按兩下以編輯其值的屬性一次。</span><span class="sxs-lookup"><span data-stu-id="42cb3-357">Double-click the attribute once to edit its value.</span></span> <span data-ttu-id="42cb3-358">值取代 2.3em **3em**，然後按 ENTER 鍵。</span><span class="sxs-lookup"><span data-stu-id="42cb3-358">Replace the 2.3em value with **3em**, and then press ENTER.</span></span> <span data-ttu-id="42cb3-359">請注意，標題起來更大。</span><span class="sxs-lookup"><span data-stu-id="42cb3-359">Notice that the title looks bigger.</span></span>

    <span data-ttu-id="42cb3-360">![變更頁面 Inspector2 中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 中的變更的 CSS 值")</span><span class="sxs-lookup"><span data-stu-id="42cb3-360">![Changing CSS values in the Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Changing CSS values in the Page Inspector")</span></span>

    <span data-ttu-id="42cb3-361">*Page Inspector 中變更的 CSS 值*</span><span class="sxs-lookup"><span data-stu-id="42cb3-361">*Changing CSS values in the Page Inspector*</span></span>
15. <span data-ttu-id="42cb3-362">按一下**追蹤樣式**索引標籤上，Page Inspector 的右窗格中。</span><span class="sxs-lookup"><span data-stu-id="42cb3-362">Click the **Trace Styles** tab, located in the right pane of Page Inspector.</span></span> <span data-ttu-id="42cb3-363">這是以查看所有的樣式套用至選取範圍中，依屬性名稱的替代方法。</span><span class="sxs-lookup"><span data-stu-id="42cb3-363">This is an alternative way to see all the styles applied to the selection, ordered by attribute name.</span></span>

    <span data-ttu-id="42cb3-364">![所選項目的 CSS 樣式追蹤](using-page-inspector-in-visual-studio-2012/_static/image36.png "所選項目的 CSS 樣式追蹤")</span><span class="sxs-lookup"><span data-stu-id="42cb3-364">![CSS styles tracing of the selected element](using-page-inspector-in-visual-studio-2012/_static/image36.png "CSS styles tracing of the selected element")</span></span>

    <span data-ttu-id="42cb3-365">*CSS 樣式追蹤 選取的項目*</span><span class="sxs-lookup"><span data-stu-id="42cb3-365">*CSS styles tracing of the selected element*</span></span>
16. <span data-ttu-id="42cb3-366">Page Inspector 的另一個功能是 [配置] 窗格。</span><span class="sxs-lookup"><span data-stu-id="42cb3-366">Another feature of Page Inspector is the Layout pane.</span></span> <span data-ttu-id="42cb3-367">使用檢查模式時，選取導覽列，然後按一下**配置**在右窗格中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="42cb3-367">Using the inspection mode, select the navigation bar and then click the **Layout** tab on the right pane.</span></span> <span data-ttu-id="42cb3-368">您會看到的選取項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。</span><span class="sxs-lookup"><span data-stu-id="42cb3-368">You will see the exact size of the selected element, as well as its offset, margin, padding and border size.</span></span> <span data-ttu-id="42cb3-369">請注意，您也可以修改此檢視中的值。</span><span class="sxs-lookup"><span data-stu-id="42cb3-369">Notice that you can also modify the values from this view.</span></span>

    <span data-ttu-id="42cb3-370">![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 中的項目配置")</span><span class="sxs-lookup"><span data-stu-id="42cb3-370">![Element layout in Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "Element layout in Page Inspector")</span></span>

    <span data-ttu-id="42cb3-371">*Page Inspector 中的項目配置*</span><span class="sxs-lookup"><span data-stu-id="42cb3-371">*Element layout in Page Inspector*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a><span data-ttu-id="42cb3-372">工作 2-尋找和修正相片圖庫中的樣式問題</span><span class="sxs-lookup"><span data-stu-id="42cb3-372">Task 2 - Finding and Fixing Style Issues in the Photo Gallery</span></span>

<span data-ttu-id="42cb3-373">您要如何診斷與舊版 Visual Studio 網頁問題？</span><span class="sxs-lookup"><span data-stu-id="42cb3-373">How would you diagnose Web pages issues with previous versions of Visual Studio?</span></span> <span data-ttu-id="42cb3-374">也可能已經熟悉 web 偵錯在 Visual Studio IDE，像是 Internet Explorer 開發人員工具或 firebug 這類外部執行的工具。</span><span class="sxs-lookup"><span data-stu-id="42cb3-374">You are likely familiar with web debugging tools that run outside the Visual Studio IDE, like Internet Explorer Developer Tools or Firebug.</span></span> <span data-ttu-id="42cb3-375">瀏覽器只了解 HTML、 指令碼和樣式，而基礎架構會產生將呈現的 HTML。</span><span class="sxs-lookup"><span data-stu-id="42cb3-375">Browsers only understand HTML, scripting and styles, while an underlying framework generates the HTML that will be rendered.</span></span> <span data-ttu-id="42cb3-376">因此，您通常需要部署整個站台，以查看網頁外觀如何。</span><span class="sxs-lookup"><span data-stu-id="42cb3-376">For that reason, you often need to deploy the whole site to see how web pages look like.</span></span>

<span data-ttu-id="42cb3-377">當您想要偵測及修正問題，您的網站中，您可能必須遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="42cb3-377">You had probably followed these steps when you wanted to detect and fix an issue in your web site:</span></span>

1. <span data-ttu-id="42cb3-378">從 Visual Studio 中，執行此方案，或部署在 web 伺服器上的頁面。</span><span class="sxs-lookup"><span data-stu-id="42cb3-378">Run the Solution from Visual Studio, or deploy the page on the web server.</span></span>
2. <span data-ttu-id="42cb3-379">在瀏覽器中開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="42cb3-379">In the browser, open the developer tools you use, or simply open the source code and the styles and try to match the issue.</span></span> <span data-ttu-id="42cb3-380">若要尋找相關檔案，您會使用&quot;搜尋&quot;或&quot;檔案中的搜尋&quot;的樣式類別名稱的功能。</span><span class="sxs-lookup"><span data-stu-id="42cb3-380">To find the files involved, you'd have used the &quot;Search&quot; or &quot;Search in files&quot; features with the name of the style classes.</span></span>
3. <span data-ttu-id="42cb3-381">一旦偵測到錯誤時，停止 Web 瀏覽器和伺服器。</span><span class="sxs-lookup"><span data-stu-id="42cb3-381">Once the error is detected, stop the Web browser and the server.</span></span>
4. <span data-ttu-id="42cb3-382">清除瀏覽器快取。</span><span class="sxs-lookup"><span data-stu-id="42cb3-382">Clear the browser cache.</span></span>
5. <span data-ttu-id="42cb3-383">返回 Visual Studio 套用修正程式。</span><span class="sxs-lookup"><span data-stu-id="42cb3-383">Return to Visual Studio to apply a fix.</span></span> <span data-ttu-id="42cb3-384">重複執行測試的所有步驟。</span><span class="sxs-lookup"><span data-stu-id="42cb3-384">Repeat all the steps to test.</span></span>

<span data-ttu-id="42cb3-385">由於沒有真正 WYSIWYG 中 ASP.NET WebForms，一些樣式問題上偵測到後續階段中，執行或部署之後。</span><span class="sxs-lookup"><span data-stu-id="42cb3-385">As there is no real WYSIWYG in ASP.NET WebForms, some style issues are detected on a later stage, after running or deploying.</span></span> <span data-ttu-id="42cb3-386">現在，Page inspector，便可預覽任何頁面而無須執行方案。</span><span class="sxs-lookup"><span data-stu-id="42cb3-386">Now, with Page Inspector, it is possible to preview any page without running the solution.</span></span>

<span data-ttu-id="42cb3-387">在這項工作，您將使用 Page inspector 修正相片圖庫應用程式的一些問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-387">In this task, you will use the Page inspector for fixing some issues of the Photo Gallery application.</span></span> <span data-ttu-id="42cb3-388">在下列步驟中，您將會偵測並快速修正標頭中的一些簡單樣式問題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-388">In the following steps, you will detect and quickly fix some simple styling issue in the header.</span></span>

1. <span data-ttu-id="42cb3-389">使用頁面檢查，找出**註冊**和**登入**標頭的左側的連結。</span><span class="sxs-lookup"><span data-stu-id="42cb3-389">Using Page Inspection, locate the **Register** and the **Log In** links at the left side of the header.</span></span>

    <span data-ttu-id="42cb3-390">請注意，連結不會顯示在右側預期的位置。</span><span class="sxs-lookup"><span data-stu-id="42cb3-390">Notice that the link is not displayed at the expected place on the right.</span></span> <span data-ttu-id="42cb3-391">您現在將會對齊右邊的連結，並據以重新設定樣式。</span><span class="sxs-lookup"><span data-stu-id="42cb3-391">You will now align the link to the right and restyle it accordingly.</span></span>

    <span data-ttu-id="42cb3-392">![登入位於左側的連結](using-page-inspector-in-visual-studio-2012/_static/image38.png "登入位於左側的連結")</span><span class="sxs-lookup"><span data-stu-id="42cb3-392">![Log in link positioned on the left](using-page-inspector-in-visual-studio-2012/_static/image38.png "Log in link positioned on the left")</span></span>

    <span data-ttu-id="42cb3-393">*位在左側的登入連結*</span><span class="sxs-lookup"><span data-stu-id="42cb3-393">*Log In link positioned on the left*</span></span>
2. <span data-ttu-id="42cb3-394">切換選取的檢查模式下，選取 登入連結以開啟其程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-394">With Toggle Inspection Mode selected, select the Log In link to open its code.</span></span>

    <span data-ttu-id="42cb3-395">請注意，連結來源程式碼位於**Site.Master**檔案，不是 Default.aspx 頁面中您可以查看在第一個位置，則您在直接在正確的來源檔案中放置。</span><span class="sxs-lookup"><span data-stu-id="42cb3-395">Notice that the link source code is located in the **Site.Master** file, not in the Default.aspx page which is the place you might look in first place; you have been placed directly in the correct source file.</span></span>
3. <span data-ttu-id="42cb3-396">在**樣式**索引標籤上，找出並按一下**&lt;區段&gt;#login**是這些連結的 HTML 容器項目。</span><span class="sxs-lookup"><span data-stu-id="42cb3-396">In the **Styles** tab, locate and click the **&lt;section&gt; #login** item, which is the HTML container for these links.</span></span>

    <span data-ttu-id="42cb3-397">請注意， **#login**樣式自動位於**Site.css**按一下之後。</span><span class="sxs-lookup"><span data-stu-id="42cb3-397">Notice that the **#login** style is automatically located in **Site.css** after you click.</span></span> <span data-ttu-id="42cb3-398">此外，程式碼現在會反白。</span><span class="sxs-lookup"><span data-stu-id="42cb3-398">Moreover, the code is now highlighted.</span></span>

    <span data-ttu-id="42cb3-399">![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image39.png "選取的 CSS 樣式")</span><span class="sxs-lookup"><span data-stu-id="42cb3-399">![Selecting the CSS styles](using-page-inspector-in-visual-studio-2012/_static/image39.png "Selecting the CSS styles")</span></span>

    <span data-ttu-id="42cb3-400">*選取的 CSS 樣式*</span><span class="sxs-lookup"><span data-stu-id="42cb3-400">*Selecting the CSS styles*</span></span>
4. <span data-ttu-id="42cb3-401">請取消註解**文字對齊**屬性反白顯示的程式碼中藉由移除開頭和結尾字元，並儲存**Site.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="42cb3-401">Uncomment the **text-align** attribute in the highlighted code by removing the opening and closing characters and save the **Site.css** file.</span></span>

    <span data-ttu-id="42cb3-402">Page Inspector 可感知撰寫目前的頁面上，所有不同檔案的而且它可以偵測到任何這些檔案的變更時。</span><span class="sxs-lookup"><span data-stu-id="42cb3-402">Page Inspector is aware of all the different files that compose the current page, and it can detect when any of these files change.</span></span> <span data-ttu-id="42cb3-403">它會提醒您目前的網頁瀏覽器中不會與原始程式檔同步時。</span><span class="sxs-lookup"><span data-stu-id="42cb3-403">It alerts you whenever the current page in browser is not in sync with the source files.</span></span>
5. <span data-ttu-id="42cb3-404">在 Page Inspector 瀏覽器中，按一下位於 [網址] 列，以儲存變更並重新載入頁面下方的列。</span><span class="sxs-lookup"><span data-stu-id="42cb3-404">In the Page Inspector browser, click the bar located below the address bar to save the changes and reload the page.</span></span>

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    <span data-ttu-id="42cb3-406">*重新載入該頁面*</span><span class="sxs-lookup"><span data-stu-id="42cb3-406">*Reloading the page*</span></span>

    <span data-ttu-id="42cb3-407">連結現在會在右側，但仍是它們看起來像是分項清單。</span><span class="sxs-lookup"><span data-stu-id="42cb3-407">The links are now at the right, but they still look like a bulleted list.</span></span> <span data-ttu-id="42cb3-408">現在，您將會移除項目符號，水平對齊的連結。</span><span class="sxs-lookup"><span data-stu-id="42cb3-408">Now, you will remove the bullets and align the links horizontally.</span></span>

    ![更新的頁面](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    <span data-ttu-id="42cb3-410">*更新的頁面*</span><span class="sxs-lookup"><span data-stu-id="42cb3-410">*Updated page*</span></span>
6. <span data-ttu-id="42cb3-411">使用檢查模式中，選取任一 **&lt;li&gt;** 包含的項目&quot;註冊&quot;和&quot;登入&quot;連結。</span><span class="sxs-lookup"><span data-stu-id="42cb3-411">Using the inspection mode, select any of the **&lt;li&gt;** items that contain the &quot;Register&quot; and &quot;Log in&quot; links.</span></span> <span data-ttu-id="42cb3-412">然後，按一下  **&lt;區段&gt;#login**存取項目**Styles.css**程式碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-412">Then, click the **&lt;section&gt; #login** item to access **Styles.css** code.</span></span>

    <span data-ttu-id="42cb3-413">![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image42.png "尋找樣式")</span><span class="sxs-lookup"><span data-stu-id="42cb3-413">![Finding the style](using-page-inspector-in-visual-studio-2012/_static/image42.png "Finding the style")</span></span>

    <span data-ttu-id="42cb3-414">*尋找樣式*</span><span class="sxs-lookup"><span data-stu-id="42cb3-414">*Finding the style*</span></span>
7. <span data-ttu-id="42cb3-415">在**Style.css**，請取消註解的程式碼**#login li**項目。</span><span class="sxs-lookup"><span data-stu-id="42cb3-415">In **Style.css**, uncomment the code for **#login li** items.</span></span> <span data-ttu-id="42cb3-416">您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。</span><span class="sxs-lookup"><span data-stu-id="42cb3-416">The style you are adding will hide the bullet and display the items horizontally.</span></span>

    <span data-ttu-id="42cb3-417">![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image43.png "右側的登入連結")</span><span class="sxs-lookup"><span data-stu-id="42cb3-417">![Restyling the login links](using-page-inspector-in-visual-studio-2012/_static/image43.png "Restyling the login links")</span></span>

    <span data-ttu-id="42cb3-418">*右側的登入連結*</span><span class="sxs-lookup"><span data-stu-id="42cb3-418">*Restyling the login links*</span></span>
8. <span data-ttu-id="42cb3-419">儲存**Style.css**檔案，然後按一下下方重新載入該頁面的位址列的一次。</span><span class="sxs-lookup"><span data-stu-id="42cb3-419">Save **Style.css** file and click once on the bar located below the address to reload the page.</span></span> <span data-ttu-id="42cb3-420">請注意，連結會正確出現。</span><span class="sxs-lookup"><span data-stu-id="42cb3-420">Notice that the links appear correctly.</span></span>

    <span data-ttu-id="42cb3-421">![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image44.png "連結靠右對齊")</span><span class="sxs-lookup"><span data-stu-id="42cb3-421">![Links aligned to the right side](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links aligned to the right side")</span></span>

    <span data-ttu-id="42cb3-422">*靠右對齊的連結*</span><span class="sxs-lookup"><span data-stu-id="42cb3-422">*Links aligned to the right side*</span></span>
9. <span data-ttu-id="42cb3-423">最後，您將變更標頭標題。</span><span class="sxs-lookup"><span data-stu-id="42cb3-423">Finally, you will change the header title.</span></span> <span data-ttu-id="42cb3-424">而不是搜尋 '**您的標誌'**文字中的所有檔案，使用檢查模式按一下文字，並取得產生它的原始碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-424">Instead of searching for the '**your logo here'** text in all the files, use the inspection mode to click the text and get to source code that generates it.</span></span>

    <span data-ttu-id="42cb3-425">![尋找網站標題](using-page-inspector-in-visual-studio-2012/_static/image45.png "尋找網站標題")</span><span class="sxs-lookup"><span data-stu-id="42cb3-425">![Finding the site title](using-page-inspector-in-visual-studio-2012/_static/image45.png "Finding the site title")</span></span>

    <span data-ttu-id="42cb3-426">*尋找網站標題*</span><span class="sxs-lookup"><span data-stu-id="42cb3-426">*Finding the site title*</span></span>
10. <span data-ttu-id="42cb3-427">現在您已在**Site.Master**，取代 '**您的標誌**'text 與'**相片圖庫**'。</span><span class="sxs-lookup"><span data-stu-id="42cb3-427">Now you are in **Site.Master**, replace the '**your logo here**' text with '**Photo Gallery**'.</span></span> <span data-ttu-id="42cb3-428">儲存並更新 Page Inspector 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="42cb3-428">Save and update the Page Inspector browser.</span></span>

    <span data-ttu-id="42cb3-429">![相片圖庫頁面更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "相片圖庫頁面更新")</span><span class="sxs-lookup"><span data-stu-id="42cb3-429">![Photo Gallery page updated](using-page-inspector-in-visual-studio-2012/_static/image46.png "Photo Gallery page updated")</span></span>

    <span data-ttu-id="42cb3-430">*相片圖庫頁面更新*</span><span class="sxs-lookup"><span data-stu-id="42cb3-430">*Photo Gallery page updated*</span></span>
11. <span data-ttu-id="42cb3-431">最後按**F5**執行應用程式的簽出所有如預期般運作所做的變更。</span><span class="sxs-lookup"><span data-stu-id="42cb3-431">Finally press **F5** to run the app the check out all the changes work as expected.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="42cb3-432">總結</span><span class="sxs-lookup"><span data-stu-id="42cb3-432">Summary</span></span>

<span data-ttu-id="42cb3-433">藉由完成這個實際操作實驗室，您已學會如何使用 Page Inspector 預覽您的 Web 應用程式而不需要重新建置並在瀏覽器中執行的網站。</span><span class="sxs-lookup"><span data-stu-id="42cb3-433">By completing this Hands-On Lab, you have learnt how to use Page Inspector to preview your Web application without having to rebuild and run the Web site in a browser.</span></span> <span data-ttu-id="42cb3-434">此外，您已學會如何快速找出並修正 bug，藉由從轉譯的輸出直接存取原始碼。</span><span class="sxs-lookup"><span data-stu-id="42cb3-434">In addition, you have learnt how to quickly find and fix bugs by accessing directly from the rendered output to the source code.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="42cb3-435">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="42cb3-435">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="42cb3-436">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="42cb3-436">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="42cb3-437">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="42cb3-437">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="42cb3-438">移至[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="42cb3-438">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="42cb3-439">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; *Visual Studio Express 2012 for Web 與 Windows Azure SDK*&quot;。</span><span class="sxs-lookup"><span data-stu-id="42cb3-439">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="42cb3-440">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-440">Click on **Install Now**.</span></span> <span data-ttu-id="42cb3-441">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="42cb3-441">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="42cb3-442">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="42cb3-442">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="42cb3-443">![安裝 Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="42cb3-443">![Install Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="42cb3-444">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="42cb3-444">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="42cb3-445">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="42cb3-445">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    <span data-ttu-id="42cb3-447">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="42cb3-447">*Accepting the license terms*</span></span>
5. <span data-ttu-id="42cb3-448">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="42cb3-448">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    <span data-ttu-id="42cb3-450">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="42cb3-450">*Installation progress*</span></span>
6. <span data-ttu-id="42cb3-451">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="42cb3-451">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    <span data-ttu-id="42cb3-453">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="42cb3-453">*Installation completed*</span></span>
7. <span data-ttu-id="42cb3-454">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="42cb3-454">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="42cb3-455">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="42cb3-455">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    <span data-ttu-id="42cb3-457">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="42cb3-457">*VS Express for Web tile*</span></span>
