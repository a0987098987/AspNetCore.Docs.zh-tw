---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "程式設計 ASP.NET Web Pages (Razor) 使用 Visual Studio |Microsoft 文件"
author: tfitzmac
description: "此附錄將解釋如何使用 Visual Studio 2010 或 Visual Web Developer 2010 Express 含有 Razor 語法的 ASP.NET Web Pages 程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="4ef66-103">使用 Visual Studio 程式設計 ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="4ef66-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="4ef66-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4ef66-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4ef66-105">本文說明如何使用 Visual Studio 或 Visual Web Developer Express 程式 ASP.NET Web Pages (Razor) 網站。</span><span class="sxs-lookup"><span data-stu-id="4ef66-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="4ef66-106">您將學習</span><span class="sxs-lookup"><span data-stu-id="4ef66-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="4ef66-107">您必須安裝在您的 Visual Studio 版本中運作以 ASP.NET Web Pages 的 （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="4ef66-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="4ef66-108">如何加入支援的 ASP.NET Web Pages Visual Web Developer 2010 Express。</span><span class="sxs-lookup"><span data-stu-id="4ef66-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="4ef66-109">如何使用 Visual Studio 中的功能，才能使用 ASP.NET Razor 頁面，包括 IntelliSense 和偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="4ef66-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4ef66-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="4ef66-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4ef66-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="4ef66-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="4ef66-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4ef66-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="4ef66-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="4ef66-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="4ef66-114">本教學課程也適用於 ASP.NET Web Pages 2、 Visual Studio 2012、 Visual Studio 2010 和 WebMatrix 2。</span><span class="sxs-lookup"><span data-stu-id="4ef66-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="4ef66-115">您可以使用 WebMatrix 或許多其他程式碼編輯器的 Razor 語法與程式的 ASP.NET Web pages。</span><span class="sxs-lookup"><span data-stu-id="4ef66-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="4ef66-116">您也可以使用 Microsoft Visual Studio 也就是功能完整的整合式的開發環境 (IDE) 提供一組強大的工具來建立許多類型的應用程式 （不只是網站）。</span><span class="sxs-lookup"><span data-stu-id="4ef66-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="4ef66-117">若要使用 ASP.NET Razor 頁面，您也可以使用其中一個 Visual Studio 的完整版本或免費[Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)版本。</span><span class="sxs-lookup"><span data-stu-id="4ef66-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="4ef66-118">Visual Studio 使用 ASP.NET Razor 網頁進行程式設計所提供的兩個特別有用功能如下：</span><span class="sxs-lookup"><span data-stu-id="4ef66-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="4ef66-119">*IntelliSense*。</span><span class="sxs-lookup"><span data-stu-id="4ef66-119">*IntelliSense*.</span></span> <span data-ttu-id="4ef66-120">建置到 Visual Studio 的 IntelliSense 功能會比在 WebMatrix IntelliSense 更廣泛。</span><span class="sxs-lookup"><span data-stu-id="4ef66-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="4ef66-121">*偵錯工具*。</span><span class="sxs-lookup"><span data-stu-id="4ef66-121">*Debugger*.</span></span> <span data-ttu-id="4ef66-122">偵錯工具可讓您針對您的程式碼來停止程式執行、 檢查變數，並逐步執行逐行程式碼時進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="4ef66-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="4ef66-123">使用 Visual Studio 使用不同版本的 ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="4ef66-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="4ef66-124">Visual Studio 2012 和 Visual Studio 2013 包括支援的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="4ef66-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="4ef66-125">（當您安裝 Visual Studio 會安裝之封裝的條件，才能支援 ASP.NET Web Pages）。</span><span class="sxs-lookup"><span data-stu-id="4ef66-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="4ef66-126">Visual Studio 2010 不支援預設的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="4ef66-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="4ef66-127">若要使用 ASP.NET Web Pages Visual Studio 2010，您必須安裝 ASP.NET MVC 封裝。</span><span class="sxs-lookup"><span data-stu-id="4ef66-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="4ef66-128">若要取得 ASP.NET Web Pages 2，您必須安裝 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="4ef66-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="4ef66-129">下表摘要說明支援的 ASP.NET Web Pages 中不同版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="4ef66-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="4ef66-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="4ef66-130">Visual Studio 2010</span></span> | <span data-ttu-id="4ef66-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4ef66-131">Visual Studio 2012</span></span> | <span data-ttu-id="4ef66-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4ef66-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ef66-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="4ef66-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="4ef66-134">安裝 ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="4ef66-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="4ef66-135">（包含）</span><span class="sxs-lookup"><span data-stu-id="4ef66-135">(Included)</span></span> | <span data-ttu-id="4ef66-136">（包含）</span><span class="sxs-lookup"><span data-stu-id="4ef66-136">(Included)</span></span> |
| <span data-ttu-id="4ef66-137">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="4ef66-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="4ef66-138">更新至 ASP.NET Web Pages 3 透過 NuGet</span><span class="sxs-lookup"><span data-stu-id="4ef66-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="4ef66-139">（包含）</span><span class="sxs-lookup"><span data-stu-id="4ef66-139">(Included)</span></span> |

<span data-ttu-id="4ef66-140">若要使用 Visual Studio 2010，請參閱[安裝支援的 Visual Studio 2010 中 ASP.NET Web Pages](#vs2010support)。</span><span class="sxs-lookup"><span data-stu-id="4ef66-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="4ef66-141">啟動 Visual Studio 從 WebMatrix</span><span class="sxs-lookup"><span data-stu-id="4ef66-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="4ef66-142">如果您已經在 WebMatrix 啟動專案，而且想要切換至 Visual Studio，WebMatrix 會提供按鈕，以輕鬆地在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="4ef66-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="4ef66-143">您必須啟用 Visual Studio 安裝在您的電腦，此按鈕上。</span><span class="sxs-lookup"><span data-stu-id="4ef66-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="4ef66-144">下圖顯示的按鈕在 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="4ef66-144">The following image shows the button in WebMatrix.</span></span>

![啟動 Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="4ef66-146">當您按一下按鈕時，Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="4ef66-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="4ef66-147">您可以切換來回 WebMatrix 和 Visual Studio 不會有任何問題。</span><span class="sxs-lookup"><span data-stu-id="4ef66-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="4ef66-148">如果另一個環境中已經變更的任何檔案，而且需要重新載入，才能取得最新的變更，系統會通知您。</span><span class="sxs-lookup"><span data-stu-id="4ef66-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="4ef66-149">Visual Studio 中建立 ASP.NET Razor 網站</span><span class="sxs-lookup"><span data-stu-id="4ef66-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="4ef66-150">若要在 Visual Studio 中建立 ASP.NET Razor 網站：</span><span class="sxs-lookup"><span data-stu-id="4ef66-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="4ef66-151">啟動 Visual Studio 或 Visual Web Developer。</span><span class="sxs-lookup"><span data-stu-id="4ef66-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="4ef66-152">在**檔案**功能表上，按一下 **新網站**。</span><span class="sxs-lookup"><span data-stu-id="4ef66-152">In the **File** menu, click **New Web Site**.</span></span>

    ![建立新的網站](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="4ef66-154">在**新網站**對話方塊方塊中，選取要使用 （Visual C# 或 Visual Basic） 語言。</span><span class="sxs-lookup"><span data-stu-id="4ef66-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="4ef66-155">選取**ASP.NET 網站 (Razor)**範本。</span><span class="sxs-lookup"><span data-stu-id="4ef66-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![razor 的站台](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="4ef66-157">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="4ef66-157">Click **OK**.</span></span>

<span data-ttu-id="4ef66-158">新的專案存在，而且會填入一些預設的網頁以協助您快速入門。</span><span class="sxs-lookup"><span data-stu-id="4ef66-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="4ef66-159">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="4ef66-159">Using IntelliSense</span></span>

<span data-ttu-id="4ef66-160">既然您已建立站台，您可以看到在 Visual Studio 中的 IntelliSense 如何運作。</span><span class="sxs-lookup"><span data-stu-id="4ef66-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="4ef66-161">在您剛才建立的網站，開啟*Default.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="4ef66-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="4ef66-162">之後`<h3>`標記在頁面中，輸入`@ServerInfo.`（包括點）。</span><span class="sxs-lookup"><span data-stu-id="4ef66-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="4ef66-163">請注意如何 IntelliSense 會顯示可用的方法`ServerInfo`下拉式清單中的協助程式。</span><span class="sxs-lookup"><span data-stu-id="4ef66-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="4ef66-165">選取`GetHtml`中方法的清單，然後按 Enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="4ef66-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="4ef66-166">在方法中的 IntelliSense 會自動填入。</span><span class="sxs-lookup"><span data-stu-id="4ef66-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="4ef66-167">(使用 C# 中的任何方法，您必須新增`()`方法之後的字元。)</span><span class="sxs-lookup"><span data-stu-id="4ef66-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
 <span data-ttu-id="4ef66-168">已完成的程式碼，如`GetHtml`方法看起來類似下列的範例：</span><span class="sxs-lookup"><span data-stu-id="4ef66-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="4ef66-169">按 Ctrl + F5 執行頁面。</span><span class="sxs-lookup"><span data-stu-id="4ef66-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="4ef66-170">這是頁面看起來像時顯示在瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="4ef66-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![在瀏覽器中的預設頁面](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="4ef66-172">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="4ef66-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="4ef66-173">使用偵錯工具</span><span class="sxs-lookup"><span data-stu-id="4ef66-173">Using the Debugger</span></span>

1. <span data-ttu-id="4ef66-174">在頂端*Default.cshtml*開頭的該行之後 頁面上， `Page.Title`，加入下列程式碼行：</span><span class="sxs-lookup"><span data-stu-id="4ef66-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="4ef66-175">在編輯器中，以在程式碼左側的灰色邊界，按一下這個新的一行的旁邊才能加入*中斷點*。</span><span class="sxs-lookup"><span data-stu-id="4ef66-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="4ef66-176">中斷點會告知偵錯工具停止在該點執行程式，讓您可以查看發生什麼事的標記。</span><span class="sxs-lookup"><span data-stu-id="4ef66-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![設定中斷點](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="4ef66-178">移除呼叫`ServerInfo.GetHtml`方法，並將呼叫加入`@myTime`變數在它的位置。</span><span class="sxs-lookup"><span data-stu-id="4ef66-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="4ef66-179">此呼叫會顯示新程式碼行所傳回的目前時間值。</span><span class="sxs-lookup"><span data-stu-id="4ef66-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="4ef66-180">按 F5 以偵錯工具中執行網頁。</span><span class="sxs-lookup"><span data-stu-id="4ef66-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="4ef66-181">頁面會在您設定的中斷點上停止。</span><span class="sxs-lookup"><span data-stu-id="4ef66-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="4ef66-182">下圖顯示頁面的外觀在編輯器中使用的中斷點 （黃色）。</span><span class="sxs-lookup"><span data-stu-id="4ef66-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![偵錯中斷點](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="4ef66-184">在 [偵錯] 工具列中，按一下 [**逐步執行**執行下的一行程式碼] 按鈕 （或按下 F11）。</span><span class="sxs-lookup"><span data-stu-id="4ef66-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="4ef66-185">每當您按一下這個按鈕，您會繼續執行至下一行程式碼。</span><span class="sxs-lookup"><span data-stu-id="4ef66-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![逐步執行 按鈕](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="4ef66-187">檢查值`myTime`按住滑鼠指標進入此或藉由檢查中顯示之值的變數**區域變數**和**呼叫堆疊**windows。</span><span class="sxs-lookup"><span data-stu-id="4ef66-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="4ef66-188">Visual Studio 會顯示變數的值。</span><span class="sxs-lookup"><span data-stu-id="4ef66-188">Visual Studio display the value of the variable.</span></span>

    ![顯示的時間值](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="4ef66-190">完成檢查變數及逐步執行程式碼之後，請按 f5 鍵繼續執行而不需要停止每一行程式碼的網頁。</span><span class="sxs-lookup"><span data-stu-id="4ef66-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="4ef66-191">逐步執行所有程式碼完成後，瀏覽器顯示頁面。</span><span class="sxs-lookup"><span data-stu-id="4ef66-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="4ef66-192">若要進一步了解有關偵錯工具以及如何偵錯 Visual Studio 中的程式碼，請參閱[逐步解說： 在 Visual Web Developer 的 偵錯 Web Pages](https://msdn.microsoft.com/library/z9e7w6cs.aspx)。</span><span class="sxs-lookup"><span data-stu-id="4ef66-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="4ef66-193">在 ASP.NET MVC 專案與 Visual Studio 中使用 Razor</span><span class="sxs-lookup"><span data-stu-id="4ef66-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="4ef66-194">Razor 語法也廣泛使用在 ASP.NET MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="4ef66-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="4ef66-195">MVC 是功能強大、 以模式為基礎的方式建置動態網站。</span><span class="sxs-lookup"><span data-stu-id="4ef66-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="4ef66-196">如果您的 ASP.NET Web Pages 網站變得難以維護，您可能要考慮將它轉換成 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ef66-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="4ef66-197">如需建立 MVC 應用程式的範例，請參閱[開始使用 ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="4ef66-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="4ef66-198">安裝 Visual Studio 2010 中的 ASP.NET Web 網頁的支援</span><span class="sxs-lookup"><span data-stu-id="4ef66-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="4ef66-199">本節說明如何安裝 Visual Web Developer Express 2010 和 ASP.NET Web Pages Tools for Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="4ef66-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="4ef66-200">如果您還沒有 Web Platform Installer，請從下列 URL 下載：</span><span class="sxs-lookup"><span data-stu-id="4ef66-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [<span data-ttu-id="4ef66-201">https://www.microsoft.com/web/downloads/platform.aspx</span><span class="sxs-lookup"><span data-stu-id="4ef66-201">https://www.microsoft.com/web/downloads/platform.aspx</span></span>](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="4ef66-202">執行 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="4ef66-202">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="4ef66-203">按一下**產品** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="4ef66-203">Click the **Products** tab.</span></span>

    ![WebPI 產品 索引標籤](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="4ef66-205">搜尋**ASP.NET MVC 4** （適用於 ASP.NET Web Pages 2)，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="4ef66-205">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="4ef66-206">這些產品包含用於建置 ASP.NET Razor 網站的 Visual Studio 工具。</span><span class="sxs-lookup"><span data-stu-id="4ef66-206">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![WebPi 安裝選項](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="4ef66-208">按一下**安裝**以完成安裝。</span><span class="sxs-lookup"><span data-stu-id="4ef66-208">Click **Install** to complete the installation.</span></span>
