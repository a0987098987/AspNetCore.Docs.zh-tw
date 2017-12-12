---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: "實習： Visual Studio 2013 Web Tools |Microsoft 文件"
author: rick-anderson
description: "Visual Studio 是絕佳的開發環境。以網路為基礎的 Windows 和 web 專案。 它包括強大的文字編輯器，可以輕鬆地用於..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="5a6e1-104">實習： Visual Studio 2013 Web 工具</span><span class="sxs-lookup"><span data-stu-id="5a6e1-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="5a6e1-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5a6e1-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5a6e1-106">下載 Web 營訓練套件</span><span class="sxs-lookup"><span data-stu-id="5a6e1-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="5a6e1-107">Visual Studio 是絕佳的開發環境。以網路為基礎的 Windows 和 web 專案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="5a6e1-108">它包含一個功能強大的文字編輯器，輕鬆可以用來編輯沒有專案的獨立檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="5a6e1-109">當您編輯每個檔案，visual Studio 會維護一個完整剖析樹狀目錄。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="5a6e1-110">這可讓 Visual Studio 提供前所未有的自動完成和文件為基礎的動作，同時可讓開發經驗更快和更愉快。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="5a6e1-111">這些功能會特別強大，HTML 和 CSS 文件。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="5a6e1-112">所有的這項強大也是可用的擴充功能，可以很簡單地擴充功能強大的新功能與編輯器，以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="5a6e1-113">Web Essentials 是 （大多數） 網頁相關的增強功能加入 Visual Studio 的集合。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="5a6e1-114">它包含許多新的 IntelliSense 完成 （特別是針對 CSS)，新的瀏覽器連結功能，自動 JSHint javascript 檔案、 HTML 和 CSS，以及其他許多功能，是不可或缺的現代 web 開發新的警告。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="5a6e1-115">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="5a6e1-116">概觀</span><span class="sxs-lookup"><span data-stu-id="5a6e1-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5a6e1-117">目標</span><span class="sxs-lookup"><span data-stu-id="5a6e1-117">Objectives</span></span>

<span data-ttu-id="5a6e1-118">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="5a6e1-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="5a6e1-119">使用新的 HTML 編輯器功能，例如豐富 HTML5 的程式碼片段和 Zen 撰寫程式碼併入 Web Essentials</span><span class="sxs-lookup"><span data-stu-id="5a6e1-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="5a6e1-120">使用瀏覽器矩陣工具提示色彩選擇器等 Web Essentials 中包含的新 CSS 編輯器功能</span><span class="sxs-lookup"><span data-stu-id="5a6e1-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="5a6e1-121">使用新的 JavaScript 編輯器功能，例如擷取至檔案和 IntelliSense 的 Web Essentials 中包含的所有 HTML 項目</span><span class="sxs-lookup"><span data-stu-id="5a6e1-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="5a6e1-122">您的瀏覽器與 Visual Studio 中使用瀏覽器連結之間交換資料</span><span class="sxs-lookup"><span data-stu-id="5a6e1-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5a6e1-123">必要條件</span><span class="sxs-lookup"><span data-stu-id="5a6e1-123">Prerequisites</span></span>

<span data-ttu-id="5a6e1-124">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="5a6e1-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="5a6e1-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="5a6e1-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="5a6e1-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="5a6e1-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="5a6e1-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="5a6e1-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5a6e1-128">安裝程式</span><span class="sxs-lookup"><span data-stu-id="5a6e1-128">Setup</span></span>

<span data-ttu-id="5a6e1-129">若要執行這個實際操作實驗室練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="5a6e1-130">開啟 Windows 檔案總管 視窗並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="5a6e1-131">以滑鼠右鍵按一下**Setup.cmd**選取**系統管理員身分執行**啟動安裝程序，將會設定您的環境，並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="5a6e1-132">如果顯示 [使用者帳戶控制] 對話方塊，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="5a6e1-133">請確定執行安裝程式之前已在本實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="5a6e1-134">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="5a6e1-134">Using the Code Snippets</span></span>

<span data-ttu-id="5a6e1-135">在實驗室文件，您會指示要插入的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="5a6e1-136">為了方便起見，大部分的這段程式碼是依現狀 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，不必以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="5a6e1-137">每個練習會伴隨起始方案位於**開始**練習，可讓您依照每個練習各自的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="5a6e1-138">請注意練習期間加入的程式碼片段遺失從中啟動方案，並可能無法運作，直到您已經完成本練習。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="5a6e1-139">內部練習的原始程式碼，您也可以找到**結束**資料夾包含在 Visual Studio 方案中使用的程式碼所產生對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="5a6e1-140">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案做為指導。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5a6e1-141">練習</span><span class="sxs-lookup"><span data-stu-id="5a6e1-141">Exercises</span></span>

<span data-ttu-id="5a6e1-142">這個實際操作實驗室包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="5a6e1-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="5a6e1-143">使用瀏覽器連結和 Web Essentials</span><span class="sxs-lookup"><span data-stu-id="5a6e1-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="5a6e1-144">利用程式碼片段和 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="5a6e1-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="5a6e1-145">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="5a6e1-146">每個預先定義的集合設計為比對特定開發樣式，並判斷視窗配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="5a6e1-147">在這個實驗室的程序說明完成指定的工作，Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="5a6e1-148">如果您選擇不同的設定集合的開發環境，可能是您應該考慮的步驟中的差異。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="5a6e1-149">練習 1： 使用瀏覽器連結和 Web Essentials</span><span class="sxs-lookup"><span data-stu-id="5a6e1-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="5a6e1-150">**Web Essentials**是將各種現代網頁程式開發中，大部分著重於讓 web 開發經驗更快和更愉快的有用功能的 Visual Studio 擴充功能。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="5a6e1-151">您可以從 Visual Studio 中的延伸模組組件庫安裝 Web Essentials。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="5a6e1-152">**瀏覽器連結**是包含在 Visual Studio IDE 和任何開啟的瀏覽器，您的 web 應用程式和 Visual Studio 之間交換資料之間的通道會提供的 Visual Studio 2013 中的新功能。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="5a6e1-153">Web Essentials 會擴充工具，以操作 DOM 物件模型和 CSS 樣式的瀏覽器直接從您網頁瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="5a6e1-154">在此練習中，您將探索所支援的功能部分**Web Essentials**和**瀏覽器連結**增強簡單測驗頁面。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="5a6e1-155">工作 1-在多個瀏覽器中執行專案</span><span class="sxs-lookup"><span data-stu-id="5a6e1-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="5a6e1-156">在這項工作，您將設定 web 應用程式在多個瀏覽器中執行的同時發生，這是適用於跨瀏覽器測試。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="5a6e1-157">開啟**Microsoft Visual Studio**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="5a6e1-158">在**檔案**功能表上，選取**開啟 |專案/方案...**並瀏覽至**Ex1 WorkingwithBrowserLinkandWebEssentials\Begin**中**來源**實驗室 (C:\WebCampsTK\HOL\VSWebTooling\Source) 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="5a6e1-159">選取**Begin.sln**按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="5a6e1-160">在 Visual Studio 工具列中，展開 [瀏覽器] 功能表，然後選取**瀏覽...**.</span><span class="sxs-lookup"><span data-stu-id="5a6e1-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="5a6e1-161">![瀏覽功能表選項](visual-studio-2013-web-tools/_static/image1.png "...功能表瀏覽器中瀏覽")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="5a6e1-162">*瀏覽功能表選項*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="5a6e1-163">在**瀏覽**對話方塊，選取**Google Chrome**和**Internet Explorer**按住**CTRL**鍵，然後按一下**設為預設值**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="5a6e1-164">![瀏覽對話方塊](visual-studio-2013-web-tools/_static/image2.png "瀏覽 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="5a6e1-165">*選取多個預設瀏覽器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="5a6e1-166">Google Chrome 和 Internet Explorer 現在應該顯示為預設瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="5a6e1-167">按一下**取消**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="5a6e1-168">![Google Chrome 和 Internet Explorer 作為預設瀏覽器](visual-studio-2013-web-tools/_static/image3.png "Google Chrome 和 Internet Explorer 的預設瀏覽器")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="5a6e1-169">*Google Chrome 和 Internet Explorer 作為預設瀏覽器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-170">設定預設瀏覽器之後**多個瀏覽器**瀏覽器 功能表中選取選項。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="5a6e1-171">![多個瀏覽器](visual-studio-2013-web-tools/_static/image4.png "多個瀏覽器")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="5a6e1-172">按**CTRL** + **F5**執行應用程式，但不偵錯。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="5a6e1-173">當這兩個瀏覽器視窗開啟時，將其中一個，在彼此上方才能同時在這兩個瀏覽器上檢視更新。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="5a6e1-174">瀏覽器應該會顯示以淺藍色矩形的網頁。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="5a6e1-175">![放置在彼此上方的瀏覽器](visual-studio-2013-web-tools/_static/image5.png "放置在彼此上方的瀏覽器")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="5a6e1-176">*放置在彼此上方的瀏覽器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="5a6e1-177">請勿關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-177">Do not close the browsers.</span></span> <span data-ttu-id="5a6e1-178">您將在下一項工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="5a6e1-179">工作 2-使用 Zen 撰寫程式碼來建立 HTML 元素</span><span class="sxs-lookup"><span data-stu-id="5a6e1-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="5a6e1-180">**撰寫程式碼 Zen**高速的 HTML、 XML、 XSL （或任何其他結構化程式碼格式） 的外掛程式程式碼撰寫和編輯，是編輯器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="5a6e1-181">此外掛程式的核心是功能強大的縮寫引擎可讓您展開將運算式-類似於 CSS 選取器的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="5a6e1-182">Zen 撰寫程式碼是一種撰寫使用 CSS 的 HTML 樣式選取器語法的快速方式。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="5a6e1-183">在此練習中，您將使用 Zen 撰寫程式碼所提供的功能 Web Essentials 所產生的 HTML 按鈕，其代表問題的選項。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="5a6e1-184">切換回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="5a6e1-185">開啟**Index.cshtml**檔案位於**檢視** | **首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="5a6e1-186">取代 **&lt;！-TODO： 加入選項-&gt;** 註解與下列程式碼，然後按 **索引標籤**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="5a6e1-187">程式碼應該擴展成 HTML。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="5a6e1-188">![展開 HTML](visual-studio-2013-web-tools/_static/image6.png "展開 HTML")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="5a6e1-189">*擴充的 HTML*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-190">若要深入了解 Zen 撰寫程式碼的語法，請參閱下列[文章](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="5a6e1-191">按一下**重新整理瀏覽器連結的**按鈕，以更新這兩個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="5a6e1-192">![重新整理瀏覽器連結](visual-studio-2013-web-tools/_static/image7.png "重新整理瀏覽器連結")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="5a6e1-193">*重新整理瀏覽器連結*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="5a6e1-194">![包含四個按鈕的 Internet Explorer-頁面更新](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer 的更新包含四個按鈕的頁面")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="5a6e1-195">*Internet Explorer 的更新包含四個按鈕的頁面*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="5a6e1-196">![包含四個按鈕的 Google Chrome-頁面更新](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-包含四個按鈕更新頁面")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="5a6e1-197">*Google Chrome-包含四個按鈕更新頁面*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="5a6e1-198">切換回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="5a6e1-199">您的按鈕新增至網頁，但您仍然要新增範本問題。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="5a6e1-200">為了這樣做，您將使用的新功能中呼叫 Web Essentials **Lorem Ipsum 產生器**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="5a6e1-201">找出**div**具有項目**類別**屬性**前端**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="5a6e1-202">將下列程式碼新增的第一個子元素為**div**，然後按 **索引標籤**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="5a6e1-203">程式碼應該擴展成 HTML。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="5a6e1-204">![Lorem Ipsum 自動產生](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum 自動產生")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="5a6e1-205">*Lorem Ipsum 自動產生*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-206">Zen 撰寫程式碼的一部分，您現在可以直接在 HTML 編輯器中產生 Lorem Ipsum 程式碼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="5a6e1-207">只要輸入**lorem**並點擊 **索引標籤**及 30 文字 Lorem Ipsum 會插入文字。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="5a6e1-208">例如：</span><span class="sxs-lookup"><span data-stu-id="5a6e1-208">E.g.</span></span> <span data-ttu-id="5a6e1-209">*lorem10*插入 10 Lorem Ipsum 字。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="5a6e1-210">將使用另一項新功能中呼叫 Web Essentials 頂端的問題新增商標**Lorem 像素產生器**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="5a6e1-211">將下列程式碼新增的第一個子元素為**div**具有項目**容器**為**類別**，再按 **索引標籤**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="5a6e1-212">程式碼應該展開為 HTML。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="5a6e1-213">![Lorem 像素自動產生](visual-studio-2013-web-tools/_static/image11.png "Lorem 像素自動產生")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="5a6e1-214">*Lorem 像素自動產生*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-215">Zen 撰寫程式碼的一部分，您也可以直接在 HTML 編輯器中產生 Lorem 像素的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="5a6e1-216">只要輸入**pix-200 x 200-動物**並點擊 **索引標籤**和**img**要插入的代表動物 200 x 200 映像的標籤。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="5a6e1-217">如需詳細資訊，請參閱[Lorem 像素](http://www.lorempixel.com)。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="5a6e1-218">按一下**重新整理瀏覽器連結的**按鈕，以更新這兩個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="5a6e1-219">![Internet Explorer 的自動產生的影像和文字](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer 的自動產生的影像和文字")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="5a6e1-220">*Internet Explorer-自動產生的影像和文字*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="5a6e1-221">![Google Chrome-自動產生的影像和文字](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-自動產生的影像和文字")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="5a6e1-222">*Google Chrome-自動產生的影像和文字*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-223">加入程式碼片段時，會隨機選取映像，因為瀏覽器中顯示的影像可能會不同。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="5a6e1-224">請勿關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-224">Do not close the browsers.</span></span> <span data-ttu-id="5a6e1-225">您將在下一項工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="5a6e1-226">工作 3-更新樣式屬性</span><span class="sxs-lookup"><span data-stu-id="5a6e1-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="5a6e1-227">在這個工作中，您將使用的瀏覽器連結**檢查模式**功能偵測到特定的 DOM 項目，產生的位置的確切位置，然後更新 使用色彩選擇器提供的 Web 該元素的色彩屬性基本功能。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="5a6e1-228">在 Internet Explorer 瀏覽器中，按**CTRL** + **ALT** + **我**若要啟用檢查的模式。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="5a6e1-229">將指標移淺藍色框線，然後按一下。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="5a6e1-230">![將指標移淺藍色框線](visual-studio-2013-web-tools/_static/image14.png "將指標移淺藍色框線")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="5a6e1-231">*將指標移淺藍色框線*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="5a6e1-232">切換回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="5a6e1-233">請注意如何在瀏覽器中選取 HTML 項目也會選取在 Visual Studio HTML 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="5a6e1-234">![在 Visual Studio HTML 編輯器中選取 HTML 項目](visual-studio-2013-web-tools/_static/image15.png "Visual Studio HTML 編輯器中選取 HTML 項目")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="5a6e1-235">*在 Visual Studio HTML 編輯器中選取 HTML 項目*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="5a6e1-236">將立即更新**前端**以變更選取項目的樣式的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="5a6e1-237">若要這樣做，請按**CTRL** + **，**開啟**巡覽至**搜尋 方塊。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="5a6e1-238">型別**site.css**按**ENTER**開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="5a6e1-239">![開啟檔案 Site.css](visual-studio-2013-web-tools/_static/image16.png "Site.css 的開啟檔案")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="5a6e1-240">*開啟檔案 Site.css*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="5a6e1-241">按**CTRL** + **F**和型別**.flip 容器.front**尋找 CSS 選取器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="5a6e1-242">按一下淺藍色方塊中的類別，以開啟 色彩選擇器的框線屬性。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="5a6e1-243">![開啟 色彩選擇器](visual-studio-2013-web-tools/_static/image17.png "開啟 色彩選擇器")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="5a6e1-244">*開啟 色彩選擇器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="5a6e1-245">按一下 > 形箭號按鈕展開色彩選擇器，並選取新的色彩。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="5a6e1-246">![展開 色彩選擇器](visual-studio-2013-web-tools/_static/image18.png "展開色彩選擇器")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="5a6e1-247">*展開 色彩選擇器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="5a6e1-248">按**CTRL** + **ALT** + **ENTER**重新整理瀏覽器連結。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="5a6e1-249">切換至 Internet Explorer，並注意如何變更框線的色彩。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="5a6e1-250">![Internet Explorer-更新的框線色彩](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-更新的框線色彩")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="5a6e1-251">*Internet Explorer-更新的框線色彩*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="5a6e1-252">切換至 Google Chrome，請注意如何變更框線的色彩。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="5a6e1-253">![Google Chrome-更新的框線色彩](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-更新的框線色彩")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="5a6e1-254">*Google Chrome-更新的框線色彩*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="5a6e1-255">切換回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="5a6e1-256">移至結尾**Site.css**檔案並按**CTRL** + **F**找出**.btn**選取器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="5a6e1-257">請注意， **-webkit 框線半徑**屬性會以綠色加上底線。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="5a6e1-258">![-webkit 框線半徑屬性 btn 選取器的](visual-studio-2013-web-tools/_static/image21.png "btn 選擇器-webkit 框線半徑屬性")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="5a6e1-259">*-webkit 框線半徑 btn 選取器 屬性*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="5a6e1-260">將放在插入號**-webkit 框線半徑**屬性。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="5a6e1-261">藍線，應該會出現在屬性的第一個單字的第一個字母。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="5a6e1-262">這是**智慧標籤**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="5a6e1-263">按**CTRL** + **。**</span><span class="sxs-lookup"><span data-stu-id="5a6e1-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="5a6e1-264">若要開啟 [建議] 功能表，然後按一下**遺漏標準屬性 (框線 radius) 新增**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="5a6e1-265">![新增遺失的標準屬性建議](visual-studio-2013-web-tools/_static/image22.png "新增遺失的標準屬性建議")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="5a6e1-266">*加入遺漏標準屬性建議*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="5a6e1-267">**框線 radius**屬性會自動加入至 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="5a6e1-268">![遺漏新增的標準屬性](visual-studio-2013-web-tools/_static/image23.png "遺漏新增的標準屬性")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="5a6e1-269">*遺漏新增的標準屬性*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="5a6e1-270">將指標移**框線 radius**屬性來顯示**瀏覽器矩陣工具提示**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="5a6e1-271">**瀏覽器矩陣工具提示**每個瀏覽器中顯示屬性的可用性。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="5a6e1-272">![瀏覽器矩陣工具提示](visual-studio-2013-web-tools/_static/image24.png "瀏覽器矩陣工具提示")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="5a6e1-273">*瀏覽器矩陣工具提示*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="5a6e1-274">請注意，值**框線 radius**屬性已仍加上底線。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="5a6e1-275">將指標移至看到警告訊息的值。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="5a6e1-276">![框線 radius 屬性值的警告](visual-studio-2013-web-tools/_static/image25.png "border-radius 屬性值的警告")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="5a6e1-277">*框線 radius 屬性值的警告*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="5a6e1-278">移除的單位**框線 radius**為工具提示所建議的屬性值。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="5a6e1-279">做為**框線 radius**是標準的屬性，定義四個端點都是，您可以移除的方式捨入的框線**-webkit 框線半徑**屬性和 CSS 規則的值。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="5a6e1-280">將放在插入號**自動換行**屬性和智慧標籤也會出現下方的注意。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="5a6e1-281">開啟功能表，然後按一下**加入遺漏的廠商細節**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="5a6e1-282">![加入遺漏的廠商特定建議](visual-studio-2013-web-tools/_static/image26.png "加入遺漏的廠商特定建議")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="5a6e1-283">*加入遺漏的廠商特定建議*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="5a6e1-284">**Ms 自動換行**屬性會自動加入至 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="5a6e1-285">![廠商特定屬性加入](visual-studio-2013-web-tools/_static/image27.png "加入廠商特定屬性")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="5a6e1-286">*加入特定廠商屬性*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="5a6e1-287">工作 4-更新瀏覽器從 HTML 程式碼</span><span class="sxs-lookup"><span data-stu-id="5a6e1-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="5a6e1-288">在這個工作中，您將使用的瀏覽器連結**設計模式**編輯 DOM 物件，從瀏覽器，並傳送變更到 HTML 來源檔案，Visual Studio 中的功能。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="5a6e1-289">在 Google Chrome 中，按**CTRL** + **ALT** + **D**啟用設計模式。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="5a6e1-290">將指標移**Lorem Ipsum dolor sit amet**加上標籤，然後按一下。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="5a6e1-291">![編輯問題](visual-studio-2013-web-tools/_static/image28.png "編輯問題")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="5a6e1-292">*編輯問題*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-292">*Editing the question*</span></span>
3. <span data-ttu-id="5a6e1-293">資料指標應該會出現。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-293">A cursor should appear.</span></span> <span data-ttu-id="5a6e1-294">取代原始的文字與*什麼沒有它看起來會像撰寫較長的問題？*，然後按下**ESC**結束設計模式。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="5a6e1-295">![編輯問題](visual-studio-2013-web-tools/_static/image29.png "編輯的問題")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="5a6e1-296">*編輯的問題*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-296">*Question edited*</span></span>
4. <span data-ttu-id="5a6e1-297">切換回 Visual Studio] 和 [開啟**Index.cshtml**，如果尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="5a6e1-298">請注意，內部文字 **&lt;p&gt;** 已更新項目。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="5a6e1-299">![HTML 網頁中的更新問題](visual-studio-2013-web-tools/_static/image30.png "HTML 網頁中的更新問題")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="5a6e1-300">*HTML 網頁中的更新的問題*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="5a6e1-301">工作 5-檢閱 SEO 相關警告</span><span class="sxs-lookup"><span data-stu-id="5a6e1-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="5a6e1-302">**搜尋的搜尋引擎最佳化**(SEO) 是在搜尋引擎結果清單上進行網站順位較高的程序。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="5a6e1-303">站台的排名越高列更一致的方式，詳細的訪客的站台會從該搜尋引擎。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="5a6e1-304">Web Essentials 包含檢查 HTML 分析工具，報告問題找到，並提供協助修正它們。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="5a6e1-305">移至**檢視**功能表，然後按一下**錯誤清單**開啟**錯誤清單**視窗。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="5a6e1-306">![錯誤清單檢視中功能表](visual-studio-2013-web-tools/_static/image31.png "檢視 功能表中的錯誤清單")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="5a6e1-307">*錯誤清單檢視 功能表*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="5a6e1-308">請注意，有通知的 SEO 警告**&lt;中繼&gt;**標記頁面描述遺漏。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="5a6e1-309">按兩下要修正此問題的 SEO 警告項目。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="5a6e1-310">![錯誤清單視窗](visual-studio-2013-web-tools/_static/image32.png "錯誤清單視窗")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="5a6e1-311">*錯誤清單視窗*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-311">*Error List window*</span></span>
3. <span data-ttu-id="5a6e1-312">在**Web Essentials**對話方塊中，按一下 **是**插入描述&lt;中繼&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="5a6e1-313">![Web Essentials 對話方塊](visual-studio-2013-web-tools/_static/image33.png "Web Essentials 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="5a6e1-314">*Web Essentials 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="5a6e1-315">編輯器 **\_Layout.cshtml**開啟和**&lt;中繼&gt;**標記會自動加入至**head**區段HTML 檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="5a6e1-316">![自動新增 _Layout 頁面中的中繼標籤](visual-studio-2013-web-tools/_static/image34.png "Meta 標記，自動新增 _Layout 頁面中")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="5a6e1-317">*自動加入至中繼標籤\_版面配置頁*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="5a6e1-318">值變更**內容**屬性*GeekQuiz*並儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="5a6e1-319">練習 2： 取得程式碼片段和 IntelliSense 的優點</span><span class="sxs-lookup"><span data-stu-id="5a6e1-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="5a6e1-320">透過 Web Essentials，HTML 編輯器中已擴充加上額外的功能。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="5a6e1-321">在此練習中，您會看到一些開發 web 應用程式時很有幫助的新功能。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="5a6e1-322">工作 1-使用 HTML 文件中的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="5a6e1-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="5a6e1-323">您會看到在這項工作的第一個新功能稱為**動態 IntelliSense**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="5a6e1-324">動態 IntelliSense 讀取其他標記和屬性來推斷可能會使用的識別碼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="5a6e1-325">在這項工作，您將建立新的 HTML 表單元素包含標籤和輸入的欄位。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="5a6e1-326">然後您會加入**如**屬性至繫結至輸入、 標籤，您會看到 IntelliSense 建議根據的範圍中的輸入識別碼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="5a6e1-327">開啟**Visual Studio Express 2013 for Web**和**Begin.sln**方案位於**來源/Ex2 TakingAdvantageofCodeSnippetsandIntelliSense/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="5a6e1-328">或者，您可以繼續使用解決方案您在上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="5a6e1-329">在**方案總管 中**，開啟**Index.cshtml**檔案位於**檢視** | **首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="5a6e1-330">新增下列表單內**&lt;區段&gt;**項目。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="5a6e1-331">(程式碼片段- *VisualStudio2013WebTooling* - *Ex2* - *表單*)</span><span class="sxs-lookup"><span data-stu-id="5a6e1-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="5a6e1-332">輸入的標記應該加上標記的某些欄位的說明。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="5a6e1-333">加入下列標籤輸入標記之前。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="5a6e1-334">**如**屬性**&lt;標籤&gt;**指定標籤的格式項目繫結至。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="5a6e1-335">屬性的值應該等於相關項目的 id。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="5a6e1-336">新增**如**屬性**&lt;標籤&gt;**項目。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="5a6e1-337">下圖所示&quot;名稱&quot;值接著在隨後顯示在 IntelliSense 中，根據在相同範圍內項目的 id (封閉式**&lt;表單&gt;**)。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="5a6e1-338">![在 IntelliSense 中顯示識別碼](visual-studio-2013-web-tools/_static/image35.png "在 IntelliSense 中顯示的識別碼")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="5a6e1-339">*在 IntelliSense 中顯示的識別碼*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="5a6e1-340">刪除最近新增**&lt;表單&gt;**項目和其內容。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="5a6e1-341">工作 2-使用 HTML 程式碼片段</span><span class="sxs-lookup"><span data-stu-id="5a6e1-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="5a6e1-342">HTML5 引進了超過 25 個新的語意標記。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="5a6e1-343">已經有 visual Studio 的 IntelliSense 支援，這些標記，但 Visual Studio 2013 可以更快速且更容易撰寫藉由新增新的程式碼片段的標記。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="5a6e1-344">雖然這些標記不是複雜的但是它們會隨附幾個小型微妙的詳細資訊，例如新增的正確轉碼器後援*音訊*標記。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="5a6e1-345">在這項工作，您會看到音訊的標記的 HTML 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="5a6e1-346">在**Index.cshtml**檔案中，輸入**&lt;則**內**&lt;區段&gt;**項目，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="5a6e1-347">![插入音訊項目](visual-studio-2013-web-tools/_static/image36.png "插入音訊項目")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="5a6e1-348">*插入音訊項目*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="5a6e1-349">按 **索引標籤**兩次注意如何在頁面上加入下列程式碼和資料指標置於**src**第一個來源的屬性。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="5a6e1-350">按下 **索引標籤**金鑰兩次，在程式碼片段插入。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="5a6e1-351">音訊的程式碼片段顯示的標準方式使用*音訊*標記，以改善的支援兩個原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="5a6e1-352">刪除第二行，並以下列連結來 WebCampsTV Katana 顯示更新的第一行的來源： [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3)。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="5a6e1-353">產生的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="5a6e1-354">原始程式檔*KatanaProject.mp3*用做為範例。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="5a6e1-355">如果您想要的話，您可以使用另一個來源。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="5a6e1-356">按**CTRL** + **S**儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="5a6e1-357">按**CTRL** + **F5**啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="5a6e1-358">請注意音訊的播放程式已加入至應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="5a6e1-359">![在 Internet Explorer 中的音訊播放器](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer 中的音訊播放程式")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="5a6e1-360">*在 Internet Explorer 中的音訊播放器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="5a6e1-361">![Google Chrome 中的音訊播放器](visual-studio-2013-web-tools/_static/image38.png "Google Chrome 中的音訊播放程式")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="5a6e1-362">*Google Chrome 中的音訊播放器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="5a6e1-363">請勿關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-363">Do not close the browsers.</span></span> <span data-ttu-id="5a6e1-364">您將在下一項工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="5a6e1-365">工作 3-使用 JavaScript 文件中的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="5a6e1-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="5a6e1-366">與 Web Essentials 2013，樣式表和 HTML 頁面產生識別碼和類別名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="5a6e1-367">在這項工作，您將學習如何這些清單改善 Web Essentials 2013 中的 JavaScript IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="5a6e1-368">在**Index.cshtml**檔案中加入下列程式碼，以定義**指令碼**標記的 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="5a6e1-369">加入下列程式碼內**指令碼**標記可定義準備回呼函式。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="5a6e1-370">(程式碼片段- *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="5a6e1-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="5a6e1-371">將放在插入號**指令碼**標記和按**CTRL** + **。**</span><span class="sxs-lookup"><span data-stu-id="5a6e1-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="5a6e1-372">若要開啟 [建議] 功能表。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="5a6e1-373">按一下**擷取至檔案**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="5a6e1-374">![JavaScript 擷取至檔案的建議](visual-studio-2013-web-tools/_static/image39.png "JavaScript 擷取至檔案的建議")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="5a6e1-375">*JavaScript 擷取至檔案的建議*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="5a6e1-376">在**存**視窗中，選取**指令碼**資料夾，將檔案命名**init.js**按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="5a6e1-377">![另存新檔 視窗](visual-studio-2013-web-tools/_static/image40.png "另存新檔 視窗")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="5a6e1-378">*另存新檔 視窗*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-379">**Init.js**建立檔案和指令碼的內容移到檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="5a6e1-380">![Init.js 檔案所包含的內容以建立](visual-studio-2013-web-tools/_static/image41.png "Init.js 以建立檔案，所包含的內容")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="5a6e1-381">*Init.js 以建立檔案，所包含的內容*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="5a6e1-382">開啟**Index.cshtml**檔，並檢查指令碼標記已被取代的參考**init.js**檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="5a6e1-383">![Init.js html 參考](visual-studio-2013-web-tools/_static/image42.png "Init.js html 參考")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="5a6e1-384">*Init.js html 參考*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="5a6e1-385">移至**方案總管 中**並注意**init.js**檔案會自動包含在方案中。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="5a6e1-386">![Init.js 檔案包含在解決方案中](visual-studio-2013-web-tools/_static/image43.png "Init.js 檔案包含在解決方案中")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="5a6e1-387">*Init.js 檔案包含在解決方案中*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="5a6e1-388">切換回**init.js**檔案，以更新**準備**函式回呼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="5a6e1-389">在傳遞至函式回呼定義*準備*，加入下列程式碼，以取得特定的類別屬性的所有項目。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="5a6e1-390">按**CTRL** + **空間**內引號之間**getElementsByClassName**函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="5a6e1-391">![顯示 IntelliSense getElementsByClassName 函式](visual-studio-2013-web-tools/_static/image44.png "顯示 IntelliSense getElementsByClassName 函式")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="5a6e1-392">*顯示 IntelliSense getElementsByClassName 函式*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-393">請注意，IntelliSense 會顯示專案樣式表中定義的類別。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="5a6e1-394">取代為下列程式碼，您已建立的行。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="5a6e1-395">將游標放在之後**au**在引號內**getElementsByTagName**函式，並按**CTRL** + **空間**.</span><span class="sxs-lookup"><span data-stu-id="5a6e1-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="5a6e1-396">![顯示 IntelliSense getElementByTagName 方法](visual-studio-2013-web-tools/_static/image45.png "顯示 IntelliSense getElementByTagName 方法")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="5a6e1-397">*顯示 IntelliSense getElementsByTagName 方法*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="5a6e1-398">選取**&quot;音訊&quot;**清單和按**ENTER**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="5a6e1-399">其結果如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="5a6e1-400">![擷取音訊元素](visual-studio-2013-web-tools/_static/image46.png "擷取音訊的項目")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="5a6e1-401">*擷取音訊的項目*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="5a6e1-402">在**方案總管 中**，以滑鼠右鍵按一下**init.js**檔案**指令碼**資料夾，然後選取**縮短 JavaScript 檔案**從**Web Essentials**功能表。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="5a6e1-403">![縮短 JavaScript 檔案](visual-studio-2013-web-tools/_static/image47.png "縮短 JavaScript 檔案")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="5a6e1-404">*縮短 JavaScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="5a6e1-405">當系統提示時按一下 變更來源檔案啟用自動縮製**是**。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="5a6e1-406">![啟用自動縮製警告](visual-studio-2013-web-tools/_static/image48.png "啟用自動縮製警告")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="5a6e1-407">*啟用自動縮製警告*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5a6e1-408">**Init.min.js**會建立並加入做為相依性的**init.js**檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="5a6e1-409">![建立 Init.min.js 檔案](visual-studio-2013-web-tools/_static/image49.png "Init.min.js 建立的檔案")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="5a6e1-410">*建立 Init.min.js 檔案*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="5a6e1-411">開啟**init.min.js**檔案，並注意會縮短檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="5a6e1-412">![Init.min.js 檔案內容](visual-studio-2013-web-tools/_static/image50.png "Init.min.js 檔案內容")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="5a6e1-413">*Init.min.js 檔案內容*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="5a6e1-414">在**init.js**檔案中，加入下列程式碼下方**getElementsByTagName**函式呼叫來播放音訊的項目。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="5a6e1-415">(程式碼片段- *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="5a6e1-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="5a6e1-416">按一下**CTRL** + **S**儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="5a6e1-417">因為縮短的檔案已經開啟，您會看到對話方塊，指出檔案的原始檔編輯器之外修改。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="5a6e1-418">按一下 [ **是**]。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-418">Click **Yes**.</span></span>

    <span data-ttu-id="5a6e1-419">![Microsoft Visual Studio 警告](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio 警告")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="5a6e1-420">*Microsoft Visual Studio 警告*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="5a6e1-421">切換回**init.min.js**檔案以確認檔案已更新與新的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="5a6e1-422">![更新 Init.min.js 檔案](visual-studio-2013-web-tools/_static/image52.png "Init.min.js 檔案更新")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="5a6e1-423">*更新 Init.min.js 檔案*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="5a6e1-424">按一下**重新整理瀏覽器連結** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="5a6e1-425">一旦兩種瀏覽器會重新整理您在前一項工作中看到音訊播放器會開始自動播放。</span><span class="sxs-lookup"><span data-stu-id="5a6e1-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="5a6e1-426">![在檢視中的音訊播放器](visual-studio-2013-web-tools/_static/image53.png "在檢視中的音訊播放程式")</span><span class="sxs-lookup"><span data-stu-id="5a6e1-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="5a6e1-427">*在檢視中的音訊播放器*</span><span class="sxs-lookup"><span data-stu-id="5a6e1-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5a6e1-428">總結</span><span class="sxs-lookup"><span data-stu-id="5a6e1-428">Summary</span></span>

<span data-ttu-id="5a6e1-429">藉由完成這個實際操作實驗室，您已經學會如何：</span><span class="sxs-lookup"><span data-stu-id="5a6e1-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="5a6e1-430">使用新的 HTML 編輯器功能，例如豐富 HTML5 的程式碼片段和 Zen 撰寫程式碼併入 Web Essentials</span><span class="sxs-lookup"><span data-stu-id="5a6e1-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="5a6e1-431">使用瀏覽器矩陣工具提示色彩選擇器等 Web Essentials 中包含的新 CSS 編輯器功能</span><span class="sxs-lookup"><span data-stu-id="5a6e1-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="5a6e1-432">使用新的 JavaScript 編輯器功能，例如擷取至檔案和 IntelliSense 的 Web Essentials 中包含的所有 HTML 項目</span><span class="sxs-lookup"><span data-stu-id="5a6e1-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="5a6e1-433">您的瀏覽器與 Visual Studio 中使用瀏覽器連結之間交換資料</span><span class="sxs-lookup"><span data-stu-id="5a6e1-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
