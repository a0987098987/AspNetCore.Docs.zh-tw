---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: "在 ASP.NET 和 Visual Studio 2012 中的 Web 程式開發中最新消息 |Microsoft 文件"
author: rick-anderson
description: "新版的 Visual Studio 導入了一些增強功能的重點在於提升體驗和效能，當使用 Web 技術..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f0818cce2a82ede80556b3471cec9d965c3e987f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="359cb-103">在 ASP.NET 和 Visual Studio 2012 中的 Web 程式開發中最新消息</span><span class="sxs-lookup"><span data-stu-id="359cb-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="359cb-104">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="359cb-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="359cb-105">新版的 Visual Studio 導入了一些增強功能的重點在於提升使用 Web 技術時的體驗和效能。</span><span class="sxs-lookup"><span data-stu-id="359cb-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="359cb-106">CSS、 JavaScript 和 HTML 的 visual Studio 編輯器已完全改寫包含許多最要求中的程式碼的輔助，IntelliSense 和自動縮排等。</span><span class="sxs-lookup"><span data-stu-id="359cb-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="359cb-107">關於效能，統合及縮製現在已整合為內建功能，以便輕鬆地減少頁面載入時間。</span><span class="sxs-lookup"><span data-stu-id="359cb-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="359cb-108">Visual Studio 可讓您能夠使用最新的網站技術。</span><span class="sxs-lookup"><span data-stu-id="359cb-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="359cb-109">您可以使用跨瀏覽器 CSS3 程式碼片段，並確定您的網站同時也利用新的 HTML5 項目和功能的運作無論用戶端平台。</span><span class="sxs-lookup"><span data-stu-id="359cb-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="359cb-110">撰寫和分析 JavaScript 程式碼應該容易，因為這個 Visual Studio 版本。</span><span class="sxs-lookup"><span data-stu-id="359cb-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="359cb-111">IntelliSense 清單整合式的 XML 文件集和導覽功能，現在也適用於 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="359cb-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="359cb-112">現在，您會有 JavaScript 型錄垂手可得。</span><span class="sxs-lookup"><span data-stu-id="359cb-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="359cb-113">此外，您可以檢查 ECMAScript5 相容性與您的指令碼，而且在早期階段偵測出語法錯誤。</span><span class="sxs-lookup"><span data-stu-id="359cb-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="359cb-114">上一次，但並非最不重要，內建統合及縮製，也會實作這個 Visual Studio 版本。</span><span class="sxs-lookup"><span data-stu-id="359cb-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="359cb-115">將封裝您的指令碼檔案和樣式表，並壓縮使站台執行的速度快。</span><span class="sxs-lookup"><span data-stu-id="359cb-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="359cb-116">這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="359cb-117">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="359cb-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="359cb-118">目標</span><span class="sxs-lookup"><span data-stu-id="359cb-118">Objectives</span></span>

<span data-ttu-id="359cb-119">在此實習課程，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="359cb-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="359cb-120">CSS 編輯器中使用的新功能與增強功能</span><span class="sxs-lookup"><span data-stu-id="359cb-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="359cb-121">HTML 編輯器中使用的新功能與增強功能</span><span class="sxs-lookup"><span data-stu-id="359cb-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="359cb-122">在 JavaScript 編輯器中使用的新功能與增強功能</span><span class="sxs-lookup"><span data-stu-id="359cb-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="359cb-123">設定及使用統合及縮製</span><span class="sxs-lookup"><span data-stu-id="359cb-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="359cb-124">必要條件</span><span class="sxs-lookup"><span data-stu-id="359cb-124">Prerequisites</span></span>

- <span data-ttu-id="359cb-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="359cb-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="359cb-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) （適用於安裝指令碼-已安裝 Windows 8 和 Windows Server 2008 R2）</span><span class="sxs-lookup"><span data-stu-id="359cb-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="359cb-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) -或 HTML5 相容的瀏覽器</span><span class="sxs-lookup"><span data-stu-id="359cb-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="359cb-128">練習</span><span class="sxs-lookup"><span data-stu-id="359cb-128">Exercises</span></span>

<span data-ttu-id="359cb-129">此實習課程包括下列練習：</span><span class="sxs-lookup"><span data-stu-id="359cb-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="359cb-130">練習 1： 的新 CSS 編輯器</span><span class="sxs-lookup"><span data-stu-id="359cb-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="359cb-131">練習 2： 什麼是新的 HTML 編輯器</span><span class="sxs-lookup"><span data-stu-id="359cb-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="359cb-132">練習 3： 的新功能的 JavaScript 編輯器</span><span class="sxs-lookup"><span data-stu-id="359cb-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="359cb-133">練習 4： 組合和縮製</span><span class="sxs-lookup"><span data-stu-id="359cb-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="359cb-134">完成本實驗室估計時間： **60 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="359cb-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="359cb-135">練習 1： 的新 CSS 編輯器</span><span class="sxs-lookup"><span data-stu-id="359cb-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="359cb-136">Web 開發人員應該要熟悉許多 CSS 編輯相關問題。</span><span class="sxs-lookup"><span data-stu-id="359cb-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="359cb-137">跨瀏覽器相容性的嚴重問題的 CSS 樣式的其中一個。</span><span class="sxs-lookup"><span data-stu-id="359cb-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="359cb-138">它通常會發生之後將樣式套用至您的網站，, 看起來不同，如果您發現您開啟另一個瀏覽器或裝置中。</span><span class="sxs-lookup"><span data-stu-id="359cb-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="359cb-139">因此，您可能會花相當長的時間進行修正這些 visual 的問題，若要了解，當您最後讓它在瀏覽器中運作，它會折在其他。</span><span class="sxs-lookup"><span data-stu-id="359cb-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="359cb-140">Visual Studio 現在包括功能，可協助開發人員存取、 工作及有效地組織 CSS 樣式表。</span><span class="sxs-lookup"><span data-stu-id="359cb-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="359cb-141">在這個練習中，您將會符合有效的組織和版本，新的功能以及跨瀏覽器相容性 CSS3 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="359cb-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="359cb-142">工作 1-新編輯器功能</span><span class="sxs-lookup"><span data-stu-id="359cb-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="359cb-143">在這項工作，您會發現新的 CSS 編輯器的功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="359cb-144">這個新的編輯器，可協助您提升產能藉由運用新的智慧型縮排、 提升程式碼註解和增強的 IntelliSense 清單。</span><span class="sxs-lookup"><span data-stu-id="359cb-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="359cb-145">啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="359cb-146">在 [方案總管] 中，開啟**Site.css**檔案位於**樣式**資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="359cb-147">請確定**文字編輯器**工具會顯示在工具列上。</span><span class="sxs-lookup"><span data-stu-id="359cb-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="359cb-148">若要這樣做，請選取**檢視** | **工具列**功能表選項和核取**文字編輯器**選項。</span><span class="sxs-lookup"><span data-stu-id="359cb-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="359cb-149">您會注意到，因為新的版本，**註解**按鈕 (![註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) 和**取消註解**按鈕 (![取消註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) 也可供 CSS 編輯器。</span><span class="sxs-lookup"><span data-stu-id="359cb-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="359cb-150">![啟用編輯器和 CSS 工具](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "編輯器和 CSS 工具，啟用")</span><span class="sxs-lookup"><span data-stu-id="359cb-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="359cb-151">*啟用編輯器和 CSS 工具*</span><span class="sxs-lookup"><span data-stu-id="359cb-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="359cb-152">捲動程式碼，然後選取任何的 CSS 類別定義。</span><span class="sxs-lookup"><span data-stu-id="359cb-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="359cb-153">按一下**註解**(![註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) 按鈕來註解選取的行。</span><span class="sxs-lookup"><span data-stu-id="359cb-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="359cb-154">然後，按一下 **取消註解**(![取消註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)) 按鈕，以復原變更。</span><span class="sxs-lookup"><span data-stu-id="359cb-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="359cb-155">按一下**摺疊**(![摺疊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) 和**展開**(![展開](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) 按鈕位於左邊界的文字。</span><span class="sxs-lookup"><span data-stu-id="359cb-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="359cb-156">請注意，您現在可以隱藏您不使用具有更清楚地檢視的樣式。</span><span class="sxs-lookup"><span data-stu-id="359cb-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="359cb-157">![摺疊的 CSS 類別](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "摺疊的 CSS 類別")</span><span class="sxs-lookup"><span data-stu-id="359cb-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="359cb-158">*摺疊的 CSS 類別*</span><span class="sxs-lookup"><span data-stu-id="359cb-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="359cb-159">請確定已啟用智慧型縮排功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="359cb-160">選取**工具** | **選項**功能表選項，然後選取**文字編輯器** | **CSS**  | **格式**螢幕的左窗格中的頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="359cb-161">請檢查**階層式縮排**選項。</span><span class="sxs-lookup"><span data-stu-id="359cb-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="359cb-162">![啟用階層式縮排](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "啟用階層式縮排")</span><span class="sxs-lookup"><span data-stu-id="359cb-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="359cb-163">*啟用階層式縮排*</span><span class="sxs-lookup"><span data-stu-id="359cb-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="359cb-164">找出主要類別定義 (.main)，並附加至 div 項目的樣式。</span><span class="sxs-lookup"><span data-stu-id="359cb-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="359cb-165">您會注意到程式碼將會自動對齊協助使用者快速尋找父類別。</span><span class="sxs-lookup"><span data-stu-id="359cb-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="359cb-166">CSS</span><span class="sxs-lookup"><span data-stu-id="359cb-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="359cb-167">![在 CSS 中的階層式對齊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "在 CSS 中的階層式對齊方式")</span><span class="sxs-lookup"><span data-stu-id="359cb-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="359cb-168">*在 CSS 中的階層式對齊方式*</span><span class="sxs-lookup"><span data-stu-id="359cb-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="359cb-169">內部**.main div**類別中，尋找資料指標的結尾**框線： 0px;**按**Enter**顯示 IntelliSense 清單。</span><span class="sxs-lookup"><span data-stu-id="359cb-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="359cb-170">開始輸入**頂端**，並注意當您輸入要篩選清單的方式。</span><span class="sxs-lookup"><span data-stu-id="359cb-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="359cb-171">清單會顯示包含的項目**頂端**在這個字的任何部分 (在舊版的 Visual Studio 中，由項目篩選清單，*開始*具有該詞彙)。</span><span class="sxs-lookup"><span data-stu-id="359cb-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="359cb-172">![在 CSS 中的 IntelliSense 增強功能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "在 CSS 中的 IntelliSense 增強功能")</span><span class="sxs-lookup"><span data-stu-id="359cb-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="359cb-173">*在 CSS 中的 IntelliSense 增強功能*</span><span class="sxs-lookup"><span data-stu-id="359cb-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="359cb-174">工作 2-色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="359cb-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="359cb-175">在這項工作，您會發現新的 CSS 色彩選擇器整合到 Visual Studio IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="359cb-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="359cb-176">在**Site.css，**找出標頭類別定義 (.header)，並將游標放在旁邊**背景色彩**屬性之間&quot;:&quot;和&quot; #&quot;該程式碼行上的字元**。**</span><span class="sxs-lookup"><span data-stu-id="359cb-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="359cb-177">![尋找游標](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "尋找資料指標")</span><span class="sxs-lookup"><span data-stu-id="359cb-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="359cb-178">*尋找資料指標*</span><span class="sxs-lookup"><span data-stu-id="359cb-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="359cb-179">刪除**冒號**（:） 和寫入一次，以顯示色彩選擇器。</span><span class="sxs-lookup"><span data-stu-id="359cb-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="359cb-180">請注意，第一個色彩，您會看到您的站台的最常見的色彩。</span><span class="sxs-lookup"><span data-stu-id="359cb-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="359cb-181">如果您按一下白色色彩，其 HTML 色彩代碼 (#fff) 將會取代目前的程式碼色彩樣式表中。</span><span class="sxs-lookup"><span data-stu-id="359cb-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="359cb-182">![色彩選擇器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "色彩選擇器")</span><span class="sxs-lookup"><span data-stu-id="359cb-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="359cb-183">*色彩選擇器*</span><span class="sxs-lookup"><span data-stu-id="359cb-183">*Color picker*</span></span>
3. <span data-ttu-id="359cb-184">按**展開**(![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) 上顯示的色彩漸層，，然後拖曳以選取不同的色彩漸層的資料指標與色彩選擇器 按鈕。</span><span class="sxs-lookup"><span data-stu-id="359cb-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="359cb-185">在這之後，按一下 **滴管**按鈕，然後從畫面中選取任何色彩。</span><span class="sxs-lookup"><span data-stu-id="359cb-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="359cb-186">請注意，背景色彩值以動態方式變更您移動游標時。</span><span class="sxs-lookup"><span data-stu-id="359cb-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="359cb-187">![漸層色彩選擇器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "漸層色彩選擇器")</span><span class="sxs-lookup"><span data-stu-id="359cb-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="359cb-188">*漸層色彩選擇器*</span><span class="sxs-lookup"><span data-stu-id="359cb-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="359cb-189">在**不透明度**滑桿，將選取器移到中央的列，以減少不透明度。</span><span class="sxs-lookup"><span data-stu-id="359cb-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="359cb-190">請注意，背景色彩值現在會變更其小數位數為 RGBA。</span><span class="sxs-lookup"><span data-stu-id="359cb-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="359cb-191">![色彩選擇器的不透明度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "色彩選擇器的不透明度")</span><span class="sxs-lookup"><span data-stu-id="359cb-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="359cb-192">*色彩選擇器的不透明度*</span><span class="sxs-lookup"><span data-stu-id="359cb-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-193">CSS3 中增加 RGBA （紅、 綠、 藍色、 Alpha） 色彩定義可讓您定義色彩的不透明度值單一項目。</span><span class="sxs-lookup"><span data-stu-id="359cb-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="359cb-194">不同於**不透明度-**類似的 CSS 屬性 **-**  RGBA 色彩也會與最新的瀏覽器相容。</span><span class="sxs-lookup"><span data-stu-id="359cb-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="359cb-195">工作 3-CSS 相容的程式碼片段</span><span class="sxs-lookup"><span data-stu-id="359cb-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="359cb-196">在這項工作，您將學習如何使用跨瀏覽器相容 CSS3 程式碼片段，才能在您的網站中實作某些功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="359cb-197">在**Site.css**檔案中，找出**標頭**CSS 類別定義 (.header)，並將下列游標 **/\*框線 radius\* /**要加入新的程式碼片段的預留位置。</span><span class="sxs-lookup"><span data-stu-id="359cb-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="359cb-198">按**Enter**顯示 IntelliSense 清單和型別**radius**來篩選清單。</span><span class="sxs-lookup"><span data-stu-id="359cb-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="359cb-199">選取**框線 radius**從清單中按兩下，與選項，然後再按 **索引標籤**插入程式碼片段的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="359cb-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="359cb-200">然後，輸入在像素並按下的 radius 大小**Enter**。</span><span class="sxs-lookup"><span data-stu-id="359cb-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="359cb-201">例如，輸入**15px**。</span><span class="sxs-lookup"><span data-stu-id="359cb-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="359cb-202">新增的程式碼片段 CSS3 屬性會呈現大部分 HTML5 的相容性瀏覽器，包括 Mozilla 和 WebKit 瀏覽器中的圓角的邊框。</span><span class="sxs-lookup"><span data-stu-id="359cb-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="359cb-203">![使用框線 radius 片段](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "使用框線 radius 程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="359cb-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="359cb-204">*使用框線 radius 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="359cb-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="359cb-205">適用於相同**框線**頁面樣式 (.page) 中的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="359cb-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="359cb-206">CSS</span><span class="sxs-lookup"><span data-stu-id="359cb-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="359cb-207">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="359cb-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="359cb-208">請注意，每個頁面現在具有圓框線。</span><span class="sxs-lookup"><span data-stu-id="359cb-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="359cb-209">![圓角](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "圓角")</span><span class="sxs-lookup"><span data-stu-id="359cb-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="359cb-210">*圓的角*</span><span class="sxs-lookup"><span data-stu-id="359cb-210">*Rounded corners*</span></span>
4. <span data-ttu-id="359cb-211">關閉瀏覽器，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="359cb-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="359cb-212">開啟**Custom.css**檔案位於**樣式**資料夾，並將游標放**div.images u l l i m g**類別定義。</span><span class="sxs-lookup"><span data-stu-id="359cb-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="359cb-213">按 enter 鍵以顯示 IntelliSense 清單中，型別**方塊陰影**按 **索引標籤**鍵兩次以插入類別定義中的預設陰影程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="359cb-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="359cb-214">若要設定陰影值**10px 10px 5px #888**。</span><span class="sxs-lookup"><span data-stu-id="359cb-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="359cb-215">然後，輸入**框線 radius**和插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="359cb-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="359cb-216">型別**15px**設定 radius 大小及按**ENTER**。</span><span class="sxs-lookup"><span data-stu-id="359cb-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="359cb-217">![圓角以陰影](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "圓角以陰影")</span><span class="sxs-lookup"><span data-stu-id="359cb-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="359cb-218">*有陰影的圓的角*</span><span class="sxs-lookup"><span data-stu-id="359cb-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-219">目前，陰影屬性會插入對應的前置詞 moz、 webkit （o） 以支援 Mozilla 與 (Chrome 或 Safari，Konkeror) Webkit 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="359cb-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="359cb-220">建立新的類別**div.images u l l i img:hover**下方**div.images u l l i m g**類別定義，並將放在括號內的資料指標**。**</span><span class="sxs-lookup"><span data-stu-id="359cb-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="359cb-221">CSS</span><span class="sxs-lookup"><span data-stu-id="359cb-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="359cb-222">型別**轉換**按 **索引標籤**兩次，以插入轉換程式碼片段的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="359cb-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="359cb-223">然後，輸入**rotate(-15deg)**時暫留在映像變更旋轉角度值。</span><span class="sxs-lookup"><span data-stu-id="359cb-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="359cb-224">CSS</span><span class="sxs-lookup"><span data-stu-id="359cb-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="359cb-225">按**F5**執行方案，然後瀏覽至 CSS3 頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="359cb-226">請注意，映像有圓角方塊陰影。</span><span class="sxs-lookup"><span data-stu-id="359cb-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="359cb-227">將滑鼠游標影像，並觀察他們旋轉。</span><span class="sxs-lookup"><span data-stu-id="359cb-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="359cb-228">![轉換程式碼片段旋轉影像](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "轉換程式碼片段旋轉影像")</span><span class="sxs-lookup"><span data-stu-id="359cb-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="359cb-229">*轉換程式碼片段旋轉影像*</span><span class="sxs-lookup"><span data-stu-id="359cb-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-230">如果您使用 Internet Explorer 10，且看不到陰影，請確定文件模式設定為 IE10 標準。</span><span class="sxs-lookup"><span data-stu-id="359cb-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="359cb-231">按**F12**開啟 Internet Explorer 開發人員工具，然後按一下 **文件模式**變更為 IE10 標準。</span><span class="sxs-lookup"><span data-stu-id="359cb-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="359cb-233">練習 2： 什麼是新的 HTML 編輯器</span><span class="sxs-lookup"><span data-stu-id="359cb-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="359cb-234">Visual Studio 的改進的 HTML 編輯器。</span><span class="sxs-lookup"><span data-stu-id="359cb-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="359cb-235">部分增強功能包含在這個版本會在 HTML 文件、 HTML5 的程式碼片段、 HTML 開始和結束標記比對，以及 HTML 驗證智慧縮排。</span><span class="sxs-lookup"><span data-stu-id="359cb-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="359cb-236">在這個練習中，您會看到這些變更如何改善您的流暢性，網站標記中使用時。</span><span class="sxs-lookup"><span data-stu-id="359cb-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="359cb-237">CSS 編輯器，例如 HTML 編輯器也已獲得改善。</span><span class="sxs-lookup"><span data-stu-id="359cb-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="359cb-238">這些改良功能的大部分都是小型的讓 Web 開發人員的工作更容易。</span><span class="sxs-lookup"><span data-stu-id="359cb-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="359cb-239">需要更多的程式碼片段 html5，智慧型縮排，比對開始和結束標記時編輯和驗證目標 HTML 文件 DOCTYPE 是這些增強功能的一些。</span><span class="sxs-lookup"><span data-stu-id="359cb-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="359cb-240">工作 1-改善的 DOCTYPE 驗證</span><span class="sxs-lookup"><span data-stu-id="359cb-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="359cb-241">HTML 編輯器現在能夠檢查的頁面，DOCTYPE 即使定義可能主版頁面中。</span><span class="sxs-lookup"><span data-stu-id="359cb-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="359cb-242">根據您的頁面文件類型，HTML 編輯器將會驗證正確的規則集，並會篩選 IntelliSense 清單考慮 DOCTYPE 項目。</span><span class="sxs-lookup"><span data-stu-id="359cb-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="359cb-243">在這個工作中，您將變更頁面的 DOCTYPE，若要查看如何 HTML 編輯器行為會隨之變更。</span><span class="sxs-lookup"><span data-stu-id="359cb-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="359cb-244">如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="359cb-245">開啟**Site.Master**頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="359cb-246">請注意，目標結構描述驗證工具列。</span><span class="sxs-lookup"><span data-stu-id="359cb-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="359cb-247">HTML 編輯器 （驗證、 IntelliSense、 等等） 的行為的方式會正確地變更以符合選取的文件類型。</span><span class="sxs-lookup"><span data-stu-id="359cb-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="359cb-248">![使用 HTML 原始檔編輯工具列中的 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "使用 HTML 原始檔編輯工具列中的 Doctype")</span><span class="sxs-lookup"><span data-stu-id="359cb-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="359cb-249">*使用 HTML 原始檔編輯工具列中的 Doctype*</span><span class="sxs-lookup"><span data-stu-id="359cb-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="359cb-250">Html 4.01 變更目標結構描述。</span><span class="sxs-lookup"><span data-stu-id="359cb-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="359cb-251">![在 HTML 原始檔編輯工具列中變更 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "變更 HTML 原始檔編輯工具列中的 Doctype")</span><span class="sxs-lookup"><span data-stu-id="359cb-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="359cb-252">*在 HTML 原始檔編輯工具列中變更文件類型*</span><span class="sxs-lookup"><span data-stu-id="359cb-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="359cb-253">在游標放在**主體**項目，並開始輸入 HTML5 項目的名稱 (例如，**視訊**)。</span><span class="sxs-lookup"><span data-stu-id="359cb-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="359cb-254">請注意，項目不可以使用 IntelliSense 清單中。</span><span class="sxs-lookup"><span data-stu-id="359cb-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="359cb-255">![HTML5 未列出的項目](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 未列出的項目")</span><span class="sxs-lookup"><span data-stu-id="359cb-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="359cb-256">*HTML5 未列出的項目*</span><span class="sxs-lookup"><span data-stu-id="359cb-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="359cb-257">復原到目標結構描述變更驗證工具列，挑選 DOCTYPE: XHTML5 從下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="359cb-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="359cb-258">![使用 HTML 原始檔編輯工具列中的 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "使用 HTML 原始檔編輯工具列中的 Doctype")</span><span class="sxs-lookup"><span data-stu-id="359cb-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="359cb-259">*在 HTML 原始檔編輯工具列中重設文件類型*</span><span class="sxs-lookup"><span data-stu-id="359cb-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="359cb-260">在游標放在**主體**項目和開始再次輸入 HTML5 項目 (例如 like**視訊**)。</span><span class="sxs-lookup"><span data-stu-id="359cb-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="359cb-261">請注意 HTML5 項目現在會出現在 IntelliSense 清單。</span><span class="sxs-lookup"><span data-stu-id="359cb-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="359cb-262">![HTML5 所列出的項目](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 所列出的項目")</span><span class="sxs-lookup"><span data-stu-id="359cb-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="359cb-263">*HTML5 所列出的項目*</span><span class="sxs-lookup"><span data-stu-id="359cb-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="359cb-264">工作 2-開始/結束標記的自動更新</span><span class="sxs-lookup"><span data-stu-id="359cb-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="359cb-265">Visual Studio 現在更新開啟或關閉您所要編輯成互相匹配項目的標記的 HTML。</span><span class="sxs-lookup"><span data-stu-id="359cb-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="359cb-266">編輯的 HTML 標記時，這項新功能會提高生產力。</span><span class="sxs-lookup"><span data-stu-id="359cb-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="359cb-267">在**Default.aspx**頁面上，新增**H3**標題 （例如，Visual Studio 2012 岩石 ！） 的項目。</span><span class="sxs-lookup"><span data-stu-id="359cb-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="359cb-268">變更**H3**標記和型別**H2**或**H1。**</span><span class="sxs-lookup"><span data-stu-id="359cb-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="359cb-269">請注意，結束標記會自動更新。</span><span class="sxs-lookup"><span data-stu-id="359cb-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="359cb-270">您也可以修改若要查看的開始標記會據以更新過的結束標記。</span><span class="sxs-lookup"><span data-stu-id="359cb-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="359cb-271">![結束標記的自動更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "的結束標記的自動更新")</span><span class="sxs-lookup"><span data-stu-id="359cb-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="359cb-272">*結束標記的自動更新*</span><span class="sxs-lookup"><span data-stu-id="359cb-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="359cb-273">工作 3-新的 HTML5 的程式碼片段</span><span class="sxs-lookup"><span data-stu-id="359cb-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="359cb-274">Visual Studio 現在包含數個 HTML5 的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="359cb-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="359cb-275">在這個工作中，您將使用這些程式碼片段部分。</span><span class="sxs-lookup"><span data-stu-id="359cb-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="359cb-276">加入新的資料夾，名為**音訊**網站資料夾的根目錄。</span><span class="sxs-lookup"><span data-stu-id="359cb-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="359cb-277">開啟 Windows 檔案總管，並將複製到任何音訊檔**音訊**資料夾**WhatsNewASPNET.sln**方案。</span><span class="sxs-lookup"><span data-stu-id="359cb-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="359cb-278">在**Default.aspx**頁面上，找出下 Web11 岩石游標!!</span><span class="sxs-lookup"><span data-stu-id="359cb-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="359cb-279">標頭。</span><span class="sxs-lookup"><span data-stu-id="359cb-279">Header.</span></span> <span data-ttu-id="359cb-280">型別**音訊**按 TAB 鍵。</span><span class="sxs-lookup"><span data-stu-id="359cb-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="359cb-281">新的 HTML 編輯器包含 HTML5 內容的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="359cb-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="359cb-282">請記得使用正確的文件類型定義啟用 HTML5 的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="359cb-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="359cb-283">![將 HTML5 的程式碼片段](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "插入 HTML5 的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="359cb-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="359cb-284">*將 HTML5 的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="359cb-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="359cb-285">更新為指向現有的音訊檔案的音訊的來源。</span><span class="sxs-lookup"><span data-stu-id="359cb-285">Update the audio source to point to an existing audio file.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="359cb-286">您必須將音訊檔加入至方案。</span><span class="sxs-lookup"><span data-stu-id="359cb-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="359cb-287">按**F5**執行站台，並播放音訊。</span><span class="sxs-lookup"><span data-stu-id="359cb-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="359cb-288">![執行音訊控制項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "執行音訊控制項")</span><span class="sxs-lookup"><span data-stu-id="359cb-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="359cb-289">*執行音訊控制項*</span><span class="sxs-lookup"><span data-stu-id="359cb-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-290">您也可以嘗試更多的程式碼片段包含在 Visual Studio 中，例如視訊、 圖等。</span><span class="sxs-lookup"><span data-stu-id="359cb-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="359cb-291">現在，請嘗試以插入控制項中頁面的某些部分。</span><span class="sxs-lookup"><span data-stu-id="359cb-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="359cb-292">例如，嘗試插入**GridView**控制項，而不是輸入，但 **&lt;Gri，**開始輸入 **&lt;GV**。</span><span class="sxs-lookup"><span data-stu-id="359cb-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="359cb-293">請注意，IntelliSense 清單顯示**asp: GridView**控制項。</span><span class="sxs-lookup"><span data-stu-id="359cb-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="359cb-294">HTML 編輯器中 IntelliSense 現在提供標題大小寫搜尋，以及部分相符 （擷取包含詞彙的所有項目）。</span><span class="sxs-lookup"><span data-stu-id="359cb-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="359cb-295">![插入 IntelliSense 清單的 GridView](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "插入 IntelliSense 清單的 GridView")</span><span class="sxs-lookup"><span data-stu-id="359cb-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="359cb-296">*插入 IntelliSense 清單的 GridView*</span><span class="sxs-lookup"><span data-stu-id="359cb-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="359cb-297">如果您輸入**&lt;方格**就會符合詞彙的所有項目，但 Visual Studio 將會建議**gridview**控制項：</span><span class="sxs-lookup"><span data-stu-id="359cb-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="359cb-298">![插入 IntelliSense 清單和部分比對的 GridView](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "插入 IntelliSense 清單和部分比對的 GridView")</span><span class="sxs-lookup"><span data-stu-id="359cb-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="359cb-299">*插入 IntelliSense 清單和部分比對的 GridView*</span><span class="sxs-lookup"><span data-stu-id="359cb-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="359cb-300">工作 4-HTML 編輯器智慧標籤</span><span class="sxs-lookup"><span data-stu-id="359cb-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="359cb-301">HTML 編輯器中的另一個改善的幅度智慧標籤功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="359cb-302">智慧標籤，讓您輕鬆執行每個控制項為基礎的通用或重複的開發工作。</span><span class="sxs-lookup"><span data-stu-id="359cb-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="359cb-303">這項功能已可供使用在 HTML 設計工具中，但不是在 HTML 編輯器。</span><span class="sxs-lookup"><span data-stu-id="359cb-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="359cb-304">開啟**Site.Master**並找出**asp： 功能表**項目。</span><span class="sxs-lookup"><span data-stu-id="359cb-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="359cb-305">將游標置於開始標記與小型的圖像顯示在下方的項目-按一下以開啟 [智慧工作提示] 功能表的注意事項。</span><span class="sxs-lookup"><span data-stu-id="359cb-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="359cb-306">請注意，您可以快速存取功能表控制項相關的一些工作。</span><span class="sxs-lookup"><span data-stu-id="359cb-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="359cb-307">![此功能表控制項的工作 智慧](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "智慧工作的功能表控制項")</span><span class="sxs-lookup"><span data-stu-id="359cb-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="359cb-308">*功能表控制項的智慧工作*</span><span class="sxs-lookup"><span data-stu-id="359cb-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="359cb-309">工作 5-智慧型縮排</span><span class="sxs-lookup"><span data-stu-id="359cb-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="359cb-310">其中一個 HTML 中的最佳作法縮排巢狀項目，以便讓程式碼更容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="359cb-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="359cb-311">在 Visual Studio 2012 中，您會注意到，編輯器會自動縮排項目在撰寫程式碼時。</span><span class="sxs-lookup"><span data-stu-id="359cb-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="359cb-312">在舊版的 Visual Studio 中，可用的智慧縮排的 XML 編輯器中，但不是會在 HTML 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="359cb-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="359cb-313">請確定縮排設定 HTML 編輯器上，設定為智慧型縮排。</span><span class="sxs-lookup"><span data-stu-id="359cb-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="359cb-314">若要這樣做，請選取**工具 |選項**功能表選項，然後選取**文字編輯器 |HTML |索引標籤**螢幕的左窗格中的頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="359cb-315">選取智慧縮排選項。</span><span class="sxs-lookup"><span data-stu-id="359cb-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="359cb-316">![HTML 編輯器設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML 編輯器設定")</span><span class="sxs-lookup"><span data-stu-id="359cb-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="359cb-317">*HTML 編輯器設定*</span><span class="sxs-lookup"><span data-stu-id="359cb-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="359cb-318">在**Default.aspx**頁面上，移除 audio 項目底下的所有內容。</span><span class="sxs-lookup"><span data-stu-id="359cb-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="359cb-319">將游標放在開頭結尾**音訊**項目，然後按**ENTER**。</span><span class="sxs-lookup"><span data-stu-id="359cb-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="359cb-320">請注意，資料指標的新位置會有額外的縮排層級。</span><span class="sxs-lookup"><span data-stu-id="359cb-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="359cb-321">![智慧型縮排在 HTML 編輯器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "智慧型縮排在 HTML 編輯器")</span><span class="sxs-lookup"><span data-stu-id="359cb-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="359cb-322">*智慧型縮排在 HTML 編輯器*</span><span class="sxs-lookup"><span data-stu-id="359cb-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="359cb-323">還原已移除，或關閉的內容與音訊的標記**Default.aspx**而不儲存變更。</span><span class="sxs-lookup"><span data-stu-id="359cb-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="359cb-324">工作 6-擷取至使用者控制項</span><span class="sxs-lookup"><span data-stu-id="359cb-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="359cb-325">包含在 Visual Studio 中，例如擷取部份函式的程式碼的重構工具是很棒的功能，以便改善和重構現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="359cb-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="359cb-326">適用於 ASP.NET 網頁的對應項目會擷取至使用者控制項的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="359cb-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="359cb-327">手動進行牽涉到幾個步驟，例如建立新的使用者控制項、 移至使用者控制項的程式碼區段，登錄使用者控制項標記前置詞和，最後，具現化的頁面上的使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="359cb-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="359cb-328">現在，新*擷取至使用者控制項*工具會自動為您執行所有這些步驟。</span><span class="sxs-lookup"><span data-stu-id="359cb-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="359cb-329">在這項工作，您將使用新的擷取至使用者控制項內容的作業，從選取的程式碼中產生新的使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="359cb-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="359cb-330">在**Default.aspx**頁面上，選取**H2**和**音訊**項目。</span><span class="sxs-lookup"><span data-stu-id="359cb-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="359cb-331">以滑鼠右鍵按一下，然後選取**擷取至使用者控制項**。</span><span class="sxs-lookup"><span data-stu-id="359cb-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="359cb-332">![擷取至使用者控制項功能表選項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "擷取至使用者控制項功能表選項")</span><span class="sxs-lookup"><span data-stu-id="359cb-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="359cb-333">*擷取至使用者控制項功能表選項*</span><span class="sxs-lookup"><span data-stu-id="359cb-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="359cb-334">輸入新的使用者控制項的名稱。</span><span class="sxs-lookup"><span data-stu-id="359cb-334">Type a name for the new user control.</span></span> <span data-ttu-id="359cb-335">比方說， **Jukebox.ascx**，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="359cb-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="359cb-336">![儲存擷取的使用者控制項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "儲存擷取的使用者控制項")</span><span class="sxs-lookup"><span data-stu-id="359cb-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="359cb-337">*儲存擷取的使用者控制項*</span><span class="sxs-lookup"><span data-stu-id="359cb-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="359cb-338">請注意，選取的程式碼擷取至使用者控制項選取的程式碼的原始位置取代為新的使用者控制項的執行個體。</span><span class="sxs-lookup"><span data-stu-id="359cb-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="359cb-339">![頁面會自動更新，以使用新的使用者控制項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "頁面自動更新，以使用新的使用者控制項")</span><span class="sxs-lookup"><span data-stu-id="359cb-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="359cb-340">*頁面會自動更新，以使用新的使用者控制項*</span><span class="sxs-lookup"><span data-stu-id="359cb-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="359cb-341">按**F5**執行頁面，並確認此控制項的作用。</span><span class="sxs-lookup"><span data-stu-id="359cb-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="359cb-342">練習 3： 的新功能的 JavaScript 編輯器</span><span class="sxs-lookup"><span data-stu-id="359cb-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="359cb-343">撰寫或編輯 JavaScript 程式碼不容易的工作，特別是當您的應用程式開始變大，而且您會發現自己處理長檔案及數以百計的函式。</span><span class="sxs-lookup"><span data-stu-id="359cb-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="359cb-344">指令碼開發人員通常需要執行一些額外的工作來維護的程式碼可讀性和檔案之間瀏覽。</span><span class="sxs-lookup"><span data-stu-id="359cb-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="359cb-345">藉由加入像 jQuery JavaScript 程式庫中，指令碼瀏覽已成為一項挑戰本身由於程式碼長度。</span><span class="sxs-lookup"><span data-stu-id="359cb-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="359cb-346">Visual Studio 已更新，讓程式碼模式，存取與組織承諾的 JavaScript 編輯器。</span><span class="sxs-lookup"><span data-stu-id="359cb-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="359cb-347">在 C# 或 VB 編輯器中已存在的許多 Visual Studio 功能現在都實作中的 JavaScript 編輯器： 移至定義、 自動縮排、 文件集和您在撰寫時的驗證。</span><span class="sxs-lookup"><span data-stu-id="359cb-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="359cb-348">更新的 IntelliSense 清單將您手邊有 JavaScript 函式類別目錄。</span><span class="sxs-lookup"><span data-stu-id="359cb-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="359cb-349">在此練習中，您將學習的一些新功能和改良的 JavaScript 編輯器。</span><span class="sxs-lookup"><span data-stu-id="359cb-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="359cb-350">您會瀏覽範例檔案，並探索每個新的特性，可讓您的 JavaScript 程式設計 Visual Studio 2012 中更有效率。</span><span class="sxs-lookup"><span data-stu-id="359cb-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="359cb-351">工作 1-JavaScript 編輯器的新功能</span><span class="sxs-lookup"><span data-stu-id="359cb-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="359cb-352">此工作將介紹一些新的 JavaScript 編輯器功能，專注於組織您的程式碼，並讓較佳使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="359cb-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="359cb-353">如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="359cb-354">按**F5**執行應用程式，然後按一下 瀏覽列中的 JavaScript 連結。</span><span class="sxs-lookup"><span data-stu-id="359cb-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="359cb-355">重新整理頁面數的時間和核取方式時，計數器會遞增。</span><span class="sxs-lookup"><span data-stu-id="359cb-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="359cb-356">![頁面計數器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "頁面計數器")</span><span class="sxs-lookup"><span data-stu-id="359cb-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="359cb-357">*頁面計數器*</span><span class="sxs-lookup"><span data-stu-id="359cb-357">*Page counter*</span></span>
3. <span data-ttu-id="359cb-358">關閉瀏覽器並移回至 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="359cb-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="359cb-359">開啟**JavaScript.aspx**頁面上，並找出**&lt;指令碼&gt;**區塊 （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="359cb-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="359cb-360">下列程式碼會使用 HTML5 本機儲存體來儲存*pageLoadCount*變數儲存目前的使用者已瀏覽頁面的次數。</span><span class="sxs-lookup"><span data-stu-id="359cb-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="359cb-361">本機儲存體是 HTML5 標準導入的用戶端索引鍵-值資料庫。</span><span class="sxs-lookup"><span data-stu-id="359cb-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="359cb-362">資料會儲存在本機電腦、 使用者的瀏覽器內。</span><span class="sxs-lookup"><span data-stu-id="359cb-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="359cb-363">請確定文件類型設為 XHTML5 後再繼續進行下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="359cb-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="359cb-364">編輯程式碼，請注意，javascript IntelliSense 包含 HTML5 功能，例如本機儲存體，以及其內部的方法。</span><span class="sxs-lookup"><span data-stu-id="359cb-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="359cb-365">![在 JavaScript 中的 HTML5 JavaScript 功能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript HTML5 JavaScript 功能")</span><span class="sxs-lookup"><span data-stu-id="359cb-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="359cb-366">*在 JavaScript 中的 HTML5 JavaScript 功能*</span><span class="sxs-lookup"><span data-stu-id="359cb-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="359cb-367">按一下任何左括號 (**{**) 的指令碼從程式碼，並請注意括號會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="359cb-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="359cb-368">![方括號會反白顯示](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "方括號會反白顯示")</span><span class="sxs-lookup"><span data-stu-id="359cb-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="359cb-369">*方括號會反白顯示*</span><span class="sxs-lookup"><span data-stu-id="359cb-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="359cb-370">請取消註解函式**testAutoAlign()** (選取的三行，而且您可以使用**CTRL** + **K**;**CTRL** + **U**) 並找出函式程式碼的內部資料指標。</span><span class="sxs-lookup"><span data-stu-id="359cb-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="359cb-371">按 enter 鍵新增第二行。</span><span class="sxs-lookup"><span data-stu-id="359cb-371">Press enter to append a second line.</span></span> <span data-ttu-id="359cb-372">請注意，程式碼現在**對齊**和**自動縮排**。</span><span class="sxs-lookup"><span data-stu-id="359cb-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="359cb-373">![JavaScript 程式碼會自動對齊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript 程式碼會自動對齊")</span><span class="sxs-lookup"><span data-stu-id="359cb-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="359cb-374">*JavaScript 程式碼會自動對齊*</span><span class="sxs-lookup"><span data-stu-id="359cb-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="359cb-375">工作 2-JavaScript 驗證</span><span class="sxs-lookup"><span data-stu-id="359cb-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="359cb-376">在這項工作，您會發現新的 JavaScript 驗證 ECMAScript5 標準。</span><span class="sxs-lookup"><span data-stu-id="359cb-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="359cb-377">此功能將會協助您撰寫相容的 JavaScript 程式碼，同時防止指令碼的問題，站台部署之前。</span><span class="sxs-lookup"><span data-stu-id="359cb-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="359cb-378">Visual Studio 2010 實作 ECMAStript3 相容性，而 Visual Studio 2012 提供 ECMAScript5 相容性。</span><span class="sxs-lookup"><span data-stu-id="359cb-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="359cb-379">開啟**ECMA5script5.js**位於**Scripts\custom**專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="359cb-380">現在，您將測試 ECMAScript5 標準的驗證。</span><span class="sxs-lookup"><span data-stu-id="359cb-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="359cb-381">您可以簽出&quot;**使用嚴格**&quot;在第一行中的檔案，可讓 ECMAScript5 方向**strict 模式**。</span><span class="sxs-lookup"><span data-stu-id="359cb-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="359cb-382">此模式包含在將釐清模稜兩可從過去的版本，並且物件屬性上加入一些新功能，例如 getter 和 setter、 JSON 和更完整的反映的程式庫支援的語言的子集。</span><span class="sxs-lookup"><span data-stu-id="359cb-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="359cb-383">開啟**錯誤清單**如果尚未開啟 (**檢視**功能表 |**錯誤清單**)。</span><span class="sxs-lookup"><span data-stu-id="359cb-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="359cb-384">請注意**函式**宣告會加上底線。</span><span class="sxs-lookup"><span data-stu-id="359cb-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="359cb-385">這是因為在 ECMA5 標準函式不能放在語言結構。</span><span class="sxs-lookup"><span data-stu-id="359cb-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="359cb-386">在錯誤清單低於您會看到警告詳細資料。</span><span class="sxs-lookup"><span data-stu-id="359cb-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="359cb-387">![JavaScript 驗證錯誤訊息](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript 驗證錯誤訊息")</span><span class="sxs-lookup"><span data-stu-id="359cb-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="359cb-388">*JavaScript 驗證錯誤訊息*</span><span class="sxs-lookup"><span data-stu-id="359cb-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="359cb-389">標記為註解**&quot;使用嚴格&quot;**方向，請注意，錯誤會消失，但警告保留。</span><span class="sxs-lookup"><span data-stu-id="359cb-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="359cb-390">檔案的最後一行，在寫入任何字串 like **&quot;測試&quot;** （加上引號，以指出它是以字串形式）。</span><span class="sxs-lookup"><span data-stu-id="359cb-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="359cb-391">寫入期間來顯示 [IntelliSense] 清單中，選取字串旁的**修剪**選項。</span><span class="sxs-lookup"><span data-stu-id="359cb-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="359cb-392">ECMAScript5 標準字串值和變數也會有定義，例如修剪、 大寫、 搜尋和取代字串方法。</span><span class="sxs-lookup"><span data-stu-id="359cb-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="359cb-393">![在 JavaScript 中的 IntelliSense 清單](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "在 JavaScript 中的 IntelliSense 清單")</span><span class="sxs-lookup"><span data-stu-id="359cb-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="359cb-394">*在 JavaScript 中的 IntelliSense 清單*</span><span class="sxs-lookup"><span data-stu-id="359cb-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="359cb-395">工作 3-for JavaScript XML 文件</span><span class="sxs-lookup"><span data-stu-id="359cb-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="359cb-396">在這個工作中，您將探索在 JavaScript 中的 XML 文件的 Visual Studio 功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="359cb-397">您會看到 JavaScript IntelliSense 清單現在會顯示每個函式的 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="359cb-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="359cb-398">此外，您會發現在 JavaScript 中的巡覽功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="359cb-399">開啟**XMLDoc.js**檔案位於**指令碼/自訂**專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="359cb-400">此檔案包含 JavaScript 函式的每個 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="359cb-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="359cb-401">![JavaScript XML 文件整合 intellisense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML 文件整合 intellisense")</span><span class="sxs-lookup"><span data-stu-id="359cb-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="359cb-402">*JavaScript XML 文件整合 intellisense*</span><span class="sxs-lookup"><span data-stu-id="359cb-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="359cb-403">下面**新增**函式在**XMLDoc.js**檔案中，建立新的函式，名為**測試**。</span><span class="sxs-lookup"><span data-stu-id="359cb-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="359cb-404">在**測試**函式，呼叫**乘**接收兩個參數的函式。</span><span class="sxs-lookup"><span data-stu-id="359cb-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="359cb-405">請注意顯示工具提示方塊**乘**函式文件。</span><span class="sxs-lookup"><span data-stu-id="359cb-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="359cb-406">![對於 JavaScript 函式的 XML 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript 函式的 XML 文件")</span><span class="sxs-lookup"><span data-stu-id="359cb-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="359cb-407">*對於 JavaScript 函式的 XML 文件*</span><span class="sxs-lookup"><span data-stu-id="359cb-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="359cb-408">完成的函式呼叫的陳述式和類型*點*開啟 IntelliSense 清單上傳回的值。</span><span class="sxs-lookup"><span data-stu-id="359cb-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="359cb-409">請注意，Visual Studio 會偵測**傳回**文件，視為數字的值中的值。</span><span class="sxs-lookup"><span data-stu-id="359cb-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="359cb-410">![傳回類型的 XML 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "傳回類型的 XML 文件")</span><span class="sxs-lookup"><span data-stu-id="359cb-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="359cb-411">*傳回類型的 XML 文件*</span><span class="sxs-lookup"><span data-stu-id="359cb-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="359cb-412">現在，插入對加入函式的呼叫。</span><span class="sxs-lookup"><span data-stu-id="359cb-412">Now, insert a call to add function.</span></span> <span data-ttu-id="359cb-413">請注意 JavaScript 編輯器現在支援函式多載。</span><span class="sxs-lookup"><span data-stu-id="359cb-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="359cb-414">當您撰寫函式名稱時，您可以選取任何可用的多載指定文件中。</span><span class="sxs-lookup"><span data-stu-id="359cb-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="359cb-415">![XML 文件的多載](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "多載的 XML 文件")</span><span class="sxs-lookup"><span data-stu-id="359cb-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="359cb-416">*多載的 XML 文件*</span><span class="sxs-lookup"><span data-stu-id="359cb-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="359cb-417">開啟**GotoDefinition.js**檔案，並尋找**$().html()**函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="359cb-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="359cb-418">將游標置於**html**。</span><span class="sxs-lookup"><span data-stu-id="359cb-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="359cb-419">按**F12**並巡覽至定義。</span><span class="sxs-lookup"><span data-stu-id="359cb-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="359cb-420">請注意，您現在可以存取，並瀏覽您的 JavaScript 程式碼，而不使用**尋找**工具。</span><span class="sxs-lookup"><span data-stu-id="359cb-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="359cb-421">在底部的 程式碼檔案的簽章區塊之前的 jQuery 指令中找到資料指標。</span><span class="sxs-lookup"><span data-stu-id="359cb-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="359cb-422">按**F12**。</span><span class="sxs-lookup"><span data-stu-id="359cb-422">Press **F12**.</span></span> <span data-ttu-id="359cb-423">您會瀏覽至 jQuery 程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="359cb-424">請注意，您也可以使用的 jQuery 檔案之間瀏覽**F12**。</span><span class="sxs-lookup"><span data-stu-id="359cb-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="359cb-425">![巡覽至 jQuery 定義](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "巡覽至 jQuery 定義")</span><span class="sxs-lookup"><span data-stu-id="359cb-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="359cb-426">*巡覽至 jQuery 定義*</span><span class="sxs-lookup"><span data-stu-id="359cb-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="359cb-427">確定 GotoDefinition.js 儲存檔案之前有沒有語法錯誤。</span><span class="sxs-lookup"><span data-stu-id="359cb-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="359cb-428">練習 4： 組合和縮製</span><span class="sxs-lookup"><span data-stu-id="359cb-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="359cb-429">多少次，您的網站，並包含一個以上的 JavaScript 或 CSS 檔案？</span><span class="sxs-lookup"><span data-stu-id="359cb-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="359cb-430">這是很常見的案例，其中統合及縮製有助於減少檔案大小，並讓執行得更快的站台。</span><span class="sxs-lookup"><span data-stu-id="359cb-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="359cb-431">ASP.NET 4.5 的新功能將一組 JS 或 CSS 檔案封裝成單一元素，並縮短內容 （也就是移除不必要的空白，移除註解，減少識別項） 時，減少其大小。</span><span class="sxs-lookup"><span data-stu-id="359cb-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="359cb-432">統合及縮製 ASP.NET 4.5 中的被執行在執行階段，好讓處理程序可以識別出使用者代理程式 （例如 IE、 Mozilla 等等），並因此，將目標設為使用者瀏覽器 （比方說，移除項目是特定 Mozilla 改善壓縮當要求來自 IE）。</span><span class="sxs-lookup"><span data-stu-id="359cb-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="359cb-433">在此練習中，您將學習如何啟用和 ASP.NET 4.5 中使用不同類型的統合及縮製。</span><span class="sxs-lookup"><span data-stu-id="359cb-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="359cb-434">工作 1-安裝組合和縮製 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="359cb-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="359cb-435">如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="359cb-436">開啟 NuGet 封裝管理員主控台。</span><span class="sxs-lookup"><span data-stu-id="359cb-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="359cb-437">若要這樣做，請使用功能表**檢視** | **其他視窗** | **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="359cb-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="359cb-438">![開啟 封裝管理員 file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "開啟封裝管理員主控台")</span><span class="sxs-lookup"><span data-stu-id="359cb-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="359cb-439">*開啟 封裝管理員主控台*</span><span class="sxs-lookup"><span data-stu-id="359cb-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="359cb-440">在**Package Manager Console**類型**Install-package Microsoft.Web.Optimization**按**ENTER**。</span><span class="sxs-lookup"><span data-stu-id="359cb-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="359cb-441">工作 2-預設組合</span><span class="sxs-lookup"><span data-stu-id="359cb-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="359cb-442">使用統合及縮製的最簡單方式是啟用預設組合。</span><span class="sxs-lookup"><span data-stu-id="359cb-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="359cb-443">這個方法會使用慣例，讓您參考配套並縮短 JS 和 CSS 資料夾中的檔案版本。</span><span class="sxs-lookup"><span data-stu-id="359cb-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="359cb-444">在這項工作，您將學習如何啟用和參考配套並縮短 JS 和 CSS 檔案，並檢視所產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="359cb-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="359cb-445">如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="359cb-446">在**方案總管 中**，依序展開**樣式**， **Scripts\custom**和**Scripts\bundle**資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="359cb-447">請注意，應用程式使用多個 CSS 與 JS 檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="359cb-448">![應用程式中的多個樣式表和 JavaScript 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "應用程式中的多個樣式表與 JavaScript 檔案")</span><span class="sxs-lookup"><span data-stu-id="359cb-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="359cb-449">*應用程式中的多個樣式表和 JavaScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="359cb-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="359cb-450">開啟**Global.asax.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="359cb-451">請注意，新**Microsoft.Web.Optimization**命名空間已標記為註解檔案的開頭。</span><span class="sxs-lookup"><span data-stu-id="359cb-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="359cb-452">取消註解 using 指示詞包含統合及縮製的功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-452">Uncomment the using directive to include the bundling and minification features.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="359cb-453">找出**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="359cb-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="359cb-454">在此方法中，請取消註解 EnableDefaultBundles 呼叫以下程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="359cb-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="359cb-455">這可讓我們來參考搭售的 CSS 資料夾中的檔案集合使用的路徑，該資料夾，再加上&quot;CSS&quot;或&quot;JS&quot;後置詞。</span><span class="sxs-lookup"><span data-stu-id="359cb-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="359cb-456">開啟**Optimization.aspx**檔案，並尋找的內容控制項**HeadContent**。</span><span class="sxs-lookup"><span data-stu-id="359cb-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="359cb-457">請注意 CSS 檔案和 JS 檔案有一個參考的標記。</span><span class="sxs-lookup"><span data-stu-id="359cb-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="359cb-458">此程式碼會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="359cb-458">This code is for demo purposes.</span></span> <span data-ttu-id="359cb-459">在理想情況下，您將會參考在 Site.Master 檔組合。</span><span class="sxs-lookup"><span data-stu-id="359cb-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="359cb-460">在此範例程式碼中，您會發現，部分配套檔案所要參照的 Site.Master 檔中，進行多餘的這個最後一個參考。</span><span class="sxs-lookup"><span data-stu-id="359cb-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="359cb-461">請注意，使用連結中的將慣例**href**屬性以取得所有 CSS 或 JS 檔案樣式和 Scripts\custom 資料夾分別。</span><span class="sxs-lookup"><span data-stu-id="359cb-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="359cb-462">您可以使用路徑**指令碼/自訂/JS**配套並縮短內的所有 JS 檔案，如下所示**指令碼/自訂**資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="359cb-463">這是預設組合的預設行為。</span><span class="sxs-lookup"><span data-stu-id="359cb-463">This is the default behavior with the default bundles.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="359cb-464">開啟**Styles\Site.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="359cb-465">請注意，原始的 CSS 檔案包含縮排程式碼、 空格和增加檔案大小的註解。</span><span class="sxs-lookup"><span data-stu-id="359cb-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="359cb-466">（也 JavaScript 檔案包含空格和註解）。</span><span class="sxs-lookup"><span data-stu-id="359cb-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="359cb-467">![其中一個原始 CSS 檔案中的指令碼資料夾](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "其中一個原始 CSS 檔案中的指令碼資料夾")</span><span class="sxs-lookup"><span data-stu-id="359cb-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="359cb-468">*其中一個原始 CSS 檔案中的指令碼資料夾*</span><span class="sxs-lookup"><span data-stu-id="359cb-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="359cb-469">按**F5**執行應用程式，並瀏覽到**最佳化**頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="359cb-470">按一下**CSS 配套**連結來下載並開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="359cb-471">簽出縮短配套的檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-471">Check out the minified bundled file.</span></span> <span data-ttu-id="359cb-472">您會發現的所有空格、 註解和縮排的字元都已經都移除，正在產生較小的檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="359cb-473">![配套 CSS 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "配套 CSS 檔案")</span><span class="sxs-lookup"><span data-stu-id="359cb-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="359cb-474">*配套的 CSS 檔案*</span><span class="sxs-lookup"><span data-stu-id="359cb-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="359cb-475">現在按一下**JS 組合**開啟配套的 JavaScript 檔案連結。</span><span class="sxs-lookup"><span data-stu-id="359cb-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="359cb-476">您可以放心地忽略警告 [總管]。</span><span class="sxs-lookup"><span data-stu-id="359cb-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="359cb-477">請注意下的 JavaScript 檔案**自訂**資料夾也會搭配並縮短。</span><span class="sxs-lookup"><span data-stu-id="359cb-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="359cb-478">![配套的 JavaScript 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "配套 JavaScript 檔案")</span><span class="sxs-lookup"><span data-stu-id="359cb-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="359cb-479">*配套的 JavaScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="359cb-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="359cb-480">啟用壓縮 CSS 或 JS 檔案是較為複雜舊版 ASP.NET 中。</span><span class="sxs-lookup"><span data-stu-id="359cb-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="359cb-481">現在，如您所見，您只需要加入這一行中的*Global.asax*檔案以便結合在一起，並再參考配套的檔案從您的網站。</span><span class="sxs-lookup"><span data-stu-id="359cb-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="359cb-482">工作 3-靜態組合</span><span class="sxs-lookup"><span data-stu-id="359cb-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="359cb-483">靜態套件組合方法可讓您自訂的組合、 參考和縮製方法將使用的檔案集。</span><span class="sxs-lookup"><span data-stu-id="359cb-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="359cb-484">在這項工作，您將設定來定義一組特定的配套並縮短檔案的靜態套件組合。</span><span class="sxs-lookup"><span data-stu-id="359cb-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="359cb-485">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="359cb-485">Close the browser.</span></span>
2. <span data-ttu-id="359cb-486">開啟**Global.asax.cs**檔案，並尋找**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="359cb-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="359cb-487">取消註解的靜態配套程式碼，如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="359cb-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="359cb-488">您正在定義會參考具有靜態配套&quot; **~/StaticBundle** &quot;虛擬路徑及使用**JsMinify**具有所有指定檔案的縮製的**AddFile**方法。</span><span class="sxs-lookup"><span data-stu-id="359cb-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="359cb-489">最後，您要在其中加入靜態套件組合， **BundleTable**及啟用它。</span><span class="sxs-lookup"><span data-stu-id="359cb-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="359cb-490">請注意，檔案不在相同的位置。這是另一個預設結合在一起的優勢。</span><span class="sxs-lookup"><span data-stu-id="359cb-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="359cb-491">開啟**Optimization.aspx**檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="359cb-492">請注意，連結至**靜態 JS 組合**正在使用您已經宣告 Global.asax.cs 檔案中設定靜態套件組合時的路徑： **/StaticBundle**。</span><span class="sxs-lookup"><span data-stu-id="359cb-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="359cb-493">按**F5**執行應用程式，並瀏覽至**最佳化**頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="359cb-494">按一下**靜態 JS 組合**連結來開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="359cb-495">請注意縮短配套 JavaScript 檔案已設定靜態組合檔案的路徑下的所有 JavaScript 檔案的輸出&quot;/StaticBundle&quot;。</span><span class="sxs-lookup"><span data-stu-id="359cb-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="359cb-496">![靜態的 JavaScript 檔案配套](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "靜態 JavaScript 檔案的套件組合")</span><span class="sxs-lookup"><span data-stu-id="359cb-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="359cb-497">*靜態的 JavaScript 檔案的配套*</span><span class="sxs-lookup"><span data-stu-id="359cb-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="359cb-498">關閉瀏覽器，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="359cb-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="359cb-499">工作 4-動態資料夾組合</span><span class="sxs-lookup"><span data-stu-id="359cb-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="359cb-500">在這項工作，您將學習如何設定動態資料夾組合。</span><span class="sxs-lookup"><span data-stu-id="359cb-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="359cb-501">動態結合在一起的功能是可以包含靜態 JavaScript，以及其他檔案中會編譯成 JavaScript 的語言，因此，需要一些處理，結合在一起會在執行之前。</span><span class="sxs-lookup"><span data-stu-id="359cb-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="359cb-502">在此範例中，您將學習如何使用**DynamicFolderBundle**類別來建立動態套件組合 CofeeScript 中撰寫的檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="359cb-503">CofeeScript 是編譯成 JavaScript，並提供更簡單的語法撰寫 JavaScript 程式碼、 增強 JavaScript 的保持簡潔和可讀性的程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="359cb-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="359cb-504">開啟**Global.asax.cs**檔案，並尋找**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="359cb-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="359cb-505">取消註解的動態套件組合程式碼，如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="359cb-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="359cb-506">您會定義將會使用動態資料夾組合**CoffeeMinify**只會套用到具有檔案的自訂縮製處理器&quot; **.coffee** &quot;延伸模組 (CoffeeScript 檔案）。</span><span class="sxs-lookup"><span data-stu-id="359cb-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="359cb-507">請注意，您可以選取要像配套資料夾內檔案使用搜尋模式 '\*.coffee'。</span><span class="sxs-lookup"><span data-stu-id="359cb-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="359cb-508">開啟 NuGet 封裝管理員主控台。</span><span class="sxs-lookup"><span data-stu-id="359cb-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="359cb-509">若要這樣做，請使用功能表**檢視** | **其他視窗** | **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="359cb-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="359cb-510">在**Package Manager Console**類型**Install-package CoffeeSharp**按**ENTER**。</span><span class="sxs-lookup"><span data-stu-id="359cb-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="359cb-511">按一下**顯示所有檔案**按鈕**方案總管 中**視窗</span><span class="sxs-lookup"><span data-stu-id="359cb-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="359cb-512">![顯示所有檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "都顯示所有檔案")</span><span class="sxs-lookup"><span data-stu-id="359cb-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="359cb-513">*顯示所有檔案*</span><span class="sxs-lookup"><span data-stu-id="359cb-513">*Showing all files*</span></span>
6. <span data-ttu-id="359cb-514">以滑鼠右鍵按一下**CoffeeMinify.cs**檔案**方案總管 中**選取**加入至專案**</span><span class="sxs-lookup"><span data-stu-id="359cb-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="359cb-515">![在專案中包含 CoffeeMinify.cs 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs 檔案包含在專案")</span><span class="sxs-lookup"><span data-stu-id="359cb-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="359cb-516">*在專案中包含 CoffeeMinify.cs 檔案*</span><span class="sxs-lookup"><span data-stu-id="359cb-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="359cb-517">開啟**CoffeeMinify.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="359cb-518">此類別繼承自 JsMinify 来縮短 CoffeeScript 程式碼編譯所產生的 JavaScript 輸出中。</span><span class="sxs-lookup"><span data-stu-id="359cb-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="359cb-519">它會呼叫 CoffeeScript 編譯器在第一次，產生的 JavaScript 程式碼，然後它傳送它至 JsMinify.Process 方法，以縮短產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="359cb-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="359cb-520">開啟**Script1.coffee**和**Script2.coffee**檔案從**指令碼/配套**資料夾。</span><span class="sxs-lookup"><span data-stu-id="359cb-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="359cb-521">這些檔案會包含 CoffeScript 程式碼結合在一起以 CoffeeMinify 類別的執行時編譯。</span><span class="sxs-lookup"><span data-stu-id="359cb-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="359cb-522">為了簡單起見，提供的 CoffeeScript 檔案只包含 CoffeeScript 範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="359cb-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="359cb-523">註解會排除 JsMinify 程序。</span><span class="sxs-lookup"><span data-stu-id="359cb-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="359cb-524">![CoffeeScript 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript 檔案")</span><span class="sxs-lookup"><span data-stu-id="359cb-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="359cb-525">*CoffeeScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="359cb-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-526">[CofeeScript](https://github.com/jashkenas/coffeescript/)撰寫 JavaScript 程式碼、 增強 JavaScript 的保持簡潔和可讀性，以及加入其他功能，例如陣列理解與模式比對會提供更簡單的語法。</span><span class="sxs-lookup"><span data-stu-id="359cb-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="359cb-527">開啟**Optimization.aspx**檔案，並尋找配套連結。</span><span class="sxs-lookup"><span data-stu-id="359cb-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="359cb-528">請注意，連結至**動態 JS 組合**參考**指令碼/配套**資料夾使用**/咖啡**您設定動態資料夾組合的後置詞。</span><span class="sxs-lookup"><span data-stu-id="359cb-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="359cb-529">按**F5**執行應用程式，並瀏覽至**最佳化**頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="359cb-530">按一下**動態 JS 組合**連結來開啟產生的檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="359cb-531">請注意，此組合中所包含的內容僅包含**.coffee**檔案。</span><span class="sxs-lookup"><span data-stu-id="359cb-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="359cb-532">您也可以查看 CoffeeScript 程式碼編譯至 JavaScript，並已移除的標記為註解的線條。</span><span class="sxs-lookup"><span data-stu-id="359cb-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="359cb-533">![動態 JS 檔案配套](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "動態 JS 檔案配套")</span><span class="sxs-lookup"><span data-stu-id="359cb-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="359cb-534">*動態 JS 檔案配套*</span><span class="sxs-lookup"><span data-stu-id="359cb-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="359cb-535">此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。</span><span class="sxs-lookup"><span data-stu-id="359cb-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="359cb-536">總結</span><span class="sxs-lookup"><span data-stu-id="359cb-536">Summary</span></span>

<span data-ttu-id="359cb-537">這個實驗室可協助您了解新 ASP.NET 和 Visual Studio 2012 中的 Web 開發是什麼，以及如何利用 Visual Studio 2012 中的各種增強功能。</span><span class="sxs-lookup"><span data-stu-id="359cb-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="359cb-538">藉由完成這個實際操作實驗室，您已學會如何在 Visual Studio 2012 編輯器中使用的新功能與增強功能，CSS、 JavaScript 和 HTML。</span><span class="sxs-lookup"><span data-stu-id="359cb-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="359cb-539">此外，您已學會如何 Visual Studio 2012 實作內建統合及縮製的。</span><span class="sxs-lookup"><span data-stu-id="359cb-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="359cb-540">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="359cb-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="359cb-541">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="359cb-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="359cb-542">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="359cb-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="359cb-543">移至[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="359cb-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="359cb-544">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; *Visual Studio Express 2012 for Web 與 Windows Azure SDK*&quot;。</span><span class="sxs-lookup"><span data-stu-id="359cb-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="359cb-545">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="359cb-545">Click on **Install Now**.</span></span> <span data-ttu-id="359cb-546">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="359cb-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="359cb-547">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="359cb-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="359cb-548">![安裝 Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="359cb-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="359cb-549">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="359cb-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="359cb-550">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="359cb-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="359cb-552">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="359cb-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="359cb-553">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="359cb-553">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="359cb-555">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="359cb-555">*Installation progress*</span></span>
6. <span data-ttu-id="359cb-556">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="359cb-556">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="359cb-558">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="359cb-558">*Installation completed*</span></span>
7. <span data-ttu-id="359cb-559">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="359cb-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="359cb-560">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="359cb-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="359cb-562">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="359cb-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="359cb-563">附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="359cb-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="359cb-564">此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="359cb-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="359cb-565">工作 1-建立新的網站，從 Windows Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="359cb-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="359cb-566">移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="359cb-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-567">與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。</span><span class="sxs-lookup"><span data-stu-id="359cb-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="359cb-568">您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="359cb-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="359cb-569">![登入 Windows Azure 入口網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="359cb-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="359cb-570">*登入 Windows Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="359cb-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="359cb-571">按一下**新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="359cb-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="359cb-572">![建立新的網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="359cb-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="359cb-573">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="359cb-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="359cb-574">按一下**計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="359cb-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="359cb-575">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="359cb-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="359cb-576">新的網站上提供可用的 URL，然後按一下**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="359cb-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-577">Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="359cb-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="359cb-578">快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。</span><span class="sxs-lookup"><span data-stu-id="359cb-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="359cb-579">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="359cb-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="359cb-580">![建立新的網站使用 快速建立](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "建立新的網站使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="359cb-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="359cb-581">*建立新的網站使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="359cb-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="359cb-582">等候新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="359cb-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="359cb-583">建立網站之後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="359cb-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="359cb-584">請檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="359cb-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="359cb-585">![瀏覽至新的網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="359cb-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="359cb-586">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="359cb-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="359cb-587">![執行網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="359cb-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="359cb-588">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="359cb-588">*Web site running*</span></span>
6. <span data-ttu-id="359cb-589">返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="359cb-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="359cb-590">![開啟網站管理頁面](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "開啟網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="359cb-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="359cb-591">*開啟網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="359cb-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="359cb-592">在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="359cb-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-593">*發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="359cb-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="359cb-594">發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。</span><span class="sxs-lookup"><span data-stu-id="359cb-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="359cb-595">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="359cb-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="359cb-596">![下載網站發行設定檔](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="359cb-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="359cb-597">*下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="359cb-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="359cb-598">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="359cb-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="359cb-599">進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。</span><span class="sxs-lookup"><span data-stu-id="359cb-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="359cb-600">![儲存發行設定檔](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "儲存發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="359cb-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="359cb-601">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="359cb-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="359cb-602">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="359cb-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="359cb-603">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="359cb-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="359cb-604">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="359cb-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="359cb-605">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="359cb-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="359cb-606">您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。</span><span class="sxs-lookup"><span data-stu-id="359cb-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="359cb-607">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="359cb-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="359cb-608">請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="359cb-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="359cb-609">並未建立資料庫，因為它會在後續階段中建立。</span><span class="sxs-lookup"><span data-stu-id="359cb-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="359cb-610">![SQL Database 伺服器儀表板](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="359cb-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="359cb-611">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="359cb-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="359cb-612">下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="359cb-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="359cb-613">若要這樣做，請按一下**設定**，選取 IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊。</span><span class="sxs-lookup"><span data-stu-id="359cb-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="359cb-614">輸入規則的名稱，然後按一下![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png)  按鈕。</span><span class="sxs-lookup"><span data-stu-id="359cb-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![加入用戶端 IP 位址](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="359cb-616">*加入用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="359cb-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="359cb-617">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="359cb-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="359cb-619">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="359cb-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="359cb-620">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="359cb-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="359cb-621">返回至 ASP.NET MVC 4 方案。</span><span class="sxs-lookup"><span data-stu-id="359cb-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="359cb-622">在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="359cb-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="359cb-623">![發行應用程式](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="359cb-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="359cb-624">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="359cb-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="359cb-625">匯入發行設定檔儲存在第一項工作中。</span><span class="sxs-lookup"><span data-stu-id="359cb-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="359cb-626">![匯入發行設定檔](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="359cb-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="359cb-627">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="359cb-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="359cb-628">按一下**驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="359cb-628">Click **Validate Connection**.</span></span> <span data-ttu-id="359cb-629">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="359cb-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="359cb-630">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="359cb-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="359cb-631">![驗證連線](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="359cb-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="359cb-632">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="359cb-632">*Validating connection*</span></span>
4. <span data-ttu-id="359cb-633">在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="359cb-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="359cb-634">![Web 部署設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="359cb-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="359cb-635">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="359cb-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="359cb-636">設定資料庫連接，如下所示：</span><span class="sxs-lookup"><span data-stu-id="359cb-636">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="359cb-637">在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:*前置詞。</span><span class="sxs-lookup"><span data-stu-id="359cb-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="359cb-638">在**使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="359cb-638">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="359cb-639">在**密碼**輸入伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="359cb-639">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="359cb-640">輸入新的資料庫名稱，例如： *MVC4SampleDB*。</span><span class="sxs-lookup"><span data-stu-id="359cb-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="359cb-641">![設定目的地連接字串](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="359cb-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="359cb-642">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="359cb-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="359cb-643">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="359cb-643">Then click **OK**.</span></span> <span data-ttu-id="359cb-644">當系統提示您建立資料庫依序按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="359cb-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="359cb-645">![建立資料庫](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="359cb-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="359cb-646">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="359cb-646">*Creating the database*</span></span>
7. <span data-ttu-id="359cb-647">您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="359cb-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="359cb-648">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="359cb-648">Then click **Next**.</span></span>

    <span data-ttu-id="359cb-649">![連接字串指向 SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "連接字串指向 SQL 資料庫")</span><span class="sxs-lookup"><span data-stu-id="359cb-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="359cb-650">*連接字串指向 SQL 資料庫*</span><span class="sxs-lookup"><span data-stu-id="359cb-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="359cb-651">在**預覽**頁面上，按一下**發行**。</span><span class="sxs-lookup"><span data-stu-id="359cb-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="359cb-652">![Web 應用程式發行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="359cb-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="359cb-653">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="359cb-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="359cb-654">一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="359cb-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="359cb-655">![應用程式發行至 Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "應用程式發行至 Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="359cb-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="359cb-656">*應用程式發行至 Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="359cb-656">*Application published to Windows Azure*</span></span>
