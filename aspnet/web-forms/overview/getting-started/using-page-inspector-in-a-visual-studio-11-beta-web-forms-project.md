---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: 使用 Page Inspector，Visual Studio 2012，在 ASP.NET Web form |Microsoft Docs
author: rick-anderson
description: Page Inspector for Visual Studio 2012 是 web 開發工具與整合式瀏覽器。 在 Page Inspector 的整合式瀏覽器中，選取任何項目...
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e914b87305fa729659822ec1166e9d1947e59cb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806270"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="a42b8-104">在 ASP.NET Web Form 中的 Visual Studio 2012 中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="a42b8-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="a42b8-105">由 Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="a42b8-105">by Tim Ammann</span></span>

> <span data-ttu-id="a42b8-106">Page Inspector for Visual Studio 2012 是 web 開發工具與整合式瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a42b8-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="a42b8-107">在 整合式瀏覽器中，選取任何項目和 Page Inspector 立即會反白顯示項目的來源和 CSS。</span><span class="sxs-lookup"><span data-stu-id="a42b8-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="a42b8-108">您可以瀏覽您的應用程式中的任何頁面、 快速尋找呈現的標記的來源並使用瀏覽器工具，直接在 Visual Studio 環境中。</span><span class="sxs-lookup"><span data-stu-id="a42b8-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="a42b8-109">本教學課程的 shwos 如何啟用檢查模式中，然後快速找出和編輯 CSS 規則與您的 web 專案內的文字。</span><span class="sxs-lookup"><span data-stu-id="a42b8-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="a42b8-110">本教學課程會使用 Web Forms 應用程式專案，但您也可以使用 Page Inspector 適用於網站專案和[MVC](https://go.microsoft.com/?linkid=9802002)應用程式。</span><span class="sxs-lookup"><span data-stu-id="a42b8-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="a42b8-111">本教學課程有下列各節：</span><span class="sxs-lookup"><span data-stu-id="a42b8-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="a42b8-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="a42b8-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="a42b8-113">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a42b8-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="a42b8-114">若要檢視應用程式中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="a42b8-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="a42b8-115">啟用檢查模式</span><span class="sxs-lookup"><span data-stu-id="a42b8-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="a42b8-116">若要變更標記中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="a42b8-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> <span data-ttu-id="a42b8-117">[檢查模式和 [HTML] 視窗](#_6_inspection_mode)</span><span class="sxs-lookup"><span data-stu-id="a42b8-117">[Inspection Mode and the HTML Window](#_6_inspection_mode)</span></span>
> 
> <span data-ttu-id="a42b8-118">[在 [樣式] 視窗中預覽 CSS 變更](#_7_previewing_css)</span><span class="sxs-lookup"><span data-stu-id="a42b8-118">[Preview CSS Changes in the Styles Window](#_7_previewing_css)</span></span>
> 
> [<span data-ttu-id="a42b8-119">CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="a42b8-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="a42b8-120">使用 CSS 色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="a42b8-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="a42b8-121">必要條件</span><span class="sxs-lookup"><span data-stu-id="a42b8-121">Prerequisites</span></span>

- <span data-ttu-id="a42b8-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)或是[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="a42b8-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="a42b8-123">若要取得最新版的 Page Inspector，請使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386)來安裝 Azure SDK for.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="a42b8-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="a42b8-124">Page Inspector 隨附於 Microsoft Web Developer Tools。</span><span class="sxs-lookup"><span data-stu-id="a42b8-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="a42b8-125">1.3 為最新版本。</span><span class="sxs-lookup"><span data-stu-id="a42b8-125">The latest version is 1.3.</span></span> <span data-ttu-id="a42b8-126">若要檢查哪些版本您有執行 Visual Studio，選取**關於 Microsoft Visual Studio**從**協助**功能表。</span><span class="sxs-lookup"><span data-stu-id="a42b8-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="a42b8-127">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a42b8-127">Create a Web Application</span></span>

<span data-ttu-id="a42b8-128">首先，您將建立您將會使用 Page Inspector 與 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a42b8-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="a42b8-129">在 Visual Studio 中，選擇**檔案** &gt; **新專案**。</span><span class="sxs-lookup"><span data-stu-id="a42b8-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="a42b8-130">在左側，展開**Visual C#**，選取**Web**，然後選取**ASP.NET Web Forms 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="a42b8-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![新 Web Forms 應用程式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="a42b8-132">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="a42b8-132">Click **OK**.</span></span>

<span data-ttu-id="a42b8-133">在應用程式中開啟**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="a42b8-133">The application opens in **Source** view.</span></span>

![原始碼檢視中的新 Web Forms 應用程式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="a42b8-135">既然您已使用的應用程式時，您可以使用 Page Inspector 檢查和修改它。</span><span class="sxs-lookup"><span data-stu-id="a42b8-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="a42b8-136">若要檢視應用程式中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="a42b8-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="a42b8-137">接下來，您將檢視 Page inspector 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a42b8-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="a42b8-138">在 **方案總管**，以滑鼠右鍵按一下專案，然後選擇**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="a42b8-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Page Inspector 中的檢視](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="a42b8-140">根據預設，第一次，啟動的 Page Inspector 時停駐為窄視窗左側的 Visual Studio 環境。</span><span class="sxs-lookup"><span data-stu-id="a42b8-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="a42b8-141">將它停駐在左邊，並將它設定為適合您，或使它其中一個工具區域中停駐在頂端、 底部或右邊的寬度：</span><span class="sxs-lookup"><span data-stu-id="a42b8-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Page Inspector 停駐位置](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="a42b8-143">如果您取消停駐的頁面偵測器視窗，您可以將它放 Visual Studio 外部，或甚至是在第二個監視器上如果有的話。</span><span class="sxs-lookup"><span data-stu-id="a42b8-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="a42b8-144">不過，為了 ALT + TAB，Page Inspector 和 Visual Studio 之間的頁面偵測器視窗停駐時，移至**工具** &gt; **選項** &gt; **環境** &gt; **索引標籤和 Windows**，然後在**索引標籤也**，清除核取方塊稱為**浮動工具視窗一律保持最上層的主視窗**:</span><span class="sxs-lookup"><span data-stu-id="a42b8-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![清除 浮動工具視窗核取方塊，按 ALT + TAB 鍵會與 Visual Studio 未停駐的視窗中，Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="a42b8-146">Page Inspector 視窗的上方窗格會顯示目前的網頁瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="a42b8-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="a42b8-147">下方窗格則顯示頁面的左側的 HTML 標記中，並在右側，可讓您的某些索引標籤檢查頁面的不同層面。</span><span class="sxs-lookup"><span data-stu-id="a42b8-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="a42b8-148">下方窗格大致[F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)Internet Explorer 中。</span><span class="sxs-lookup"><span data-stu-id="a42b8-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="a42b8-149">（不過，不同於開發人員工具中，您可以使用 Page Inspector 在 Visual Studio 內的權限。）</span><span class="sxs-lookup"><span data-stu-id="a42b8-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="a42b8-151">在本教學課程中，您會使用 Page Inspector 瀏覽器 窗格中，而**HTML**並**樣式**索引標籤，以協助您快速瀏覽並對應用程式中的變更。</span><span class="sxs-lookup"><span data-stu-id="a42b8-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="a42b8-152">啟用檢查模式</span><span class="sxs-lookup"><span data-stu-id="a42b8-152">Enable Inspection Mode</span></span>

<span data-ttu-id="a42b8-153">接下來，您會看到 Page Inspector 檢查模式中的運作方式。</span><span class="sxs-lookup"><span data-stu-id="a42b8-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="a42b8-154">在 Page Inspector 視窗中，按一下**檢查** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a42b8-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="a42b8-156">若要查看作用中的檢查模式，將滑鼠移到不同的部分，Page Inspector 瀏覽器視窗內的頁面上。</span><span class="sxs-lookup"><span data-stu-id="a42b8-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="a42b8-157">這麼做，將滑鼠指標變為大型的加號，和下方的項目會反白顯示：</span><span class="sxs-lookup"><span data-stu-id="a42b8-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="a42b8-159">當您移動滑鼠指標時，請注意，</span><span class="sxs-lookup"><span data-stu-id="a42b8-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="a42b8-160">中的內容**來源**檢視變更為顯示對應至所選的項目頁面上的標記。</span><span class="sxs-lookup"><span data-stu-id="a42b8-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="a42b8-161">相關的標記會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="a42b8-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="a42b8-162">如果來源是在另一個檔案，該檔案會開啟原始碼檢視中，以反白顯示相關的標記。</span><span class="sxs-lookup"><span data-stu-id="a42b8-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="a42b8-163">中所顯示的標記**HTML** Page Inspector 中的索引標籤也會變更為對應至頁面上選取的元素。</span><span class="sxs-lookup"><span data-stu-id="a42b8-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="a42b8-164">在 [ **HTML** ] 索引標籤，相關的標記會簡要說明。</span><span class="sxs-lookup"><span data-stu-id="a42b8-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="a42b8-165">**樣式**索引標籤會顯示 CSS 規則與目前的選取範圍。</span><span class="sxs-lookup"><span data-stu-id="a42b8-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="a42b8-166">若要變更標記中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="a42b8-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="a42b8-167">現在您會看到如何使用 Page Inspector 尋找及變更標記或可能無法立即看出其位置的文字。</span><span class="sxs-lookup"><span data-stu-id="a42b8-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="a42b8-168">Page Inspector 進入檢查模式，然後捲動至底部的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="a42b8-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="a42b8-169">只要您輸入的頁尾區域時，Page Inspector 隨即開啟*Site.Master*版面配置中的檔案**來源**暫時右邊的其他索引標籤中檢視索引標籤，並反白顯示的主要區段頁面上，您已選取。</span><span class="sxs-lookup"><span data-stu-id="a42b8-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="a42b8-170">這會顯示如何尋找並顯示實際上可能來自不同的檔案比原本所開啟的頁面上的內容 Page Inspector。</span><span class="sxs-lookup"><span data-stu-id="a42b8-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![頁尾會反白顯示在檢查模式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="a42b8-172">在 Page Inspector 瀏覽器視窗中，將滑鼠指標移著作權那一行<a id="a"></a>注意到。</span><span class="sxs-lookup"><span data-stu-id="a42b8-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="a42b8-173">在 [ *Site.Master* ] 頁面上，對應的線條會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="a42b8-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![頁尾的著作權列反白顯示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="a42b8-175">加入一些文字中的行結尾*Site.Master*檔案。</span><span class="sxs-lookup"><span data-stu-id="a42b8-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="a42b8-176">&lt;p&gt;&amp;複製;&lt;%: DateTime.Now.Year %&gt; -我的 ASP.NET 應用程式 Rocks ！&lt;/ p&gt;</span><span class="sxs-lookup"><span data-stu-id="a42b8-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="a42b8-177">現在，按 Ctrl + Alt + Enter 或按一下 更新列，以查看 Page Inspector 瀏覽器視窗中的結果。</span><span class="sxs-lookup"><span data-stu-id="a42b8-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![我的 ASP.NET 應用程式 Rocks ！](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="a42b8-179">您或許以為頁尾所在*Default.aspx*  頁面上，但它原來是在主要的版面配置頁面中，和 Page Inspector 則會為您找到它。</span><span class="sxs-lookup"><span data-stu-id="a42b8-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="a42b8-180">檢查模式和 [HTML] 視窗</span><span class="sxs-lookup"><span data-stu-id="a42b8-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="a42b8-181">接下來，您必須快速瀏覽 [HTML] 視窗，以及它如何為您對應項目。</span><span class="sxs-lookup"><span data-stu-id="a42b8-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="a42b8-182">將 Page Inspector 放在檢查模式中。</span><span class="sxs-lookup"><span data-stu-id="a42b8-182">Put Page Inspector in Inspection Mode.</span></span>

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="a42b8-184">按一下 顯示 「 您標誌的位置 」 頁面的上半部。</span><span class="sxs-lookup"><span data-stu-id="a42b8-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="a42b8-185">您正在檢查詳細資料，因此不再會在瀏覽器視窗中的顯示變更，您將滑鼠游標移特定項的目。</span><span class="sxs-lookup"><span data-stu-id="a42b8-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="a42b8-186">現在將滑鼠指標移到**HTML**視窗。</span><span class="sxs-lookup"><span data-stu-id="a42b8-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="a42b8-187">當您移動滑鼠指標時，Page Inspector 概要說明中的項目**HTML**視窗並反白顯示對應的項目，在瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="a42b8-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML 視窗](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="a42b8-189">如往常一般，Page Inspector 隨即開啟*Site.Master*您暫時索引標籤中的檔案。Site.Master 索引標籤，並以反白顯示對應的標記&lt;標頭&gt;區段：</span><span class="sxs-lookup"><span data-stu-id="a42b8-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![反白顯示的標記](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="a42b8-191">在 [樣式] 視窗中預覽 CSS 變更</span><span class="sxs-lookup"><span data-stu-id="a42b8-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="a42b8-192">接下來，您會看到如何使用 Page Inspector**樣式**預覽 CSS 變更的視窗。</span><span class="sxs-lookup"><span data-stu-id="a42b8-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="a42b8-193">按一下 **檢查**Page Inspector 進入檢查模式的按鈕。</span><span class="sxs-lookup"><span data-stu-id="a42b8-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="a42b8-194">在 Page Inspector 瀏覽器視窗中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="a42b8-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![將滑鼠停留項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="a42b8-196">按一下 div.content 包裝函式區段內，，然後將滑鼠指標移到**樣式**視窗。</span><span class="sxs-lookup"><span data-stu-id="a42b8-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="a42b8-197">底下的.featured.content 包裝函式類別選取器中，清除，然後選取背景色彩屬性的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a42b8-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![清除的背景色彩](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="a42b8-199">請注意變更預覽立即的 Page Inspector 瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="a42b8-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="a42b8-200">再次選取核取方塊，然後按兩下屬性值，並將它變更為`red`。</span><span class="sxs-lookup"><span data-stu-id="a42b8-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="a42b8-201">變更會立即顯示：</span><span class="sxs-lookup"><span data-stu-id="a42b8-201">The change shows immediately:</span></span>

![紅色的背景色彩](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="a42b8-203">**樣式**視窗可讓您輕鬆地測試並預覽 CSS 變更之前，您將變更認可到樣式表本身。</span><span class="sxs-lookup"><span data-stu-id="a42b8-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="a42b8-204">CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="a42b8-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="a42b8-205">這項功能需要 Page Inspector 1.3 版。</span><span class="sxs-lookup"><span data-stu-id="a42b8-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="a42b8-206">CSS 自動同步處理功能可讓您直接編輯 CSS 檔案並查看立即在 Page Inspector 瀏覽器中的變更。</span><span class="sxs-lookup"><span data-stu-id="a42b8-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="a42b8-207">按一下 **檢查**至 Page Inspector 進入檢查模式。</span><span class="sxs-lookup"><span data-stu-id="a42b8-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="a42b8-208">在 Page Inspector 瀏覽器中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="a42b8-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="a42b8-209">再按一次就會選取這個項目。</span><span class="sxs-lookup"><span data-stu-id="a42b8-209">Click once to select this element.</span></span>

<span data-ttu-id="a42b8-210">**樣式**視窗會顯示所有此項目的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="a42b8-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="a42b8-211">請向下捲動尋找.featured.content 包裝函式類別選取器。</span><span class="sxs-lookup"><span data-stu-id="a42b8-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="a42b8-212">按一下 「.featured.content-包裝函式 」。</span><span class="sxs-lookup"><span data-stu-id="a42b8-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="a42b8-213">Page Inspector 隨即開啟，定義此樣式 (Site.css)，並反白顯示對應的 CSS 樣式的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="a42b8-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS 檔案](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="a42b8-215">現在的值變更`background-color`為"red"。</span><span class="sxs-lookup"><span data-stu-id="a42b8-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="a42b8-216">變更會立即出現在 Page Inspector 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a42b8-216">The change appears immediately in the Page Inspector browser.</span></span>

![Page Inspector 瀏覽器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="a42b8-218">使用 CSS 色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="a42b8-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="a42b8-219">接下來，您將了解如何快速地尋找和變更 CSS 來反白顯示的文字，在預設應用程式中使用 Page Inspector。</span><span class="sxs-lookup"><span data-stu-id="a42b8-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="a42b8-220">在此範例中，您已決定，您不喜歡藍色反白顯示，且想要將它變更為另一種色彩。</span><span class="sxs-lookup"><span data-stu-id="a42b8-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="a42b8-221">按一下 [**檢查**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a42b8-221">Click the **Inspect** button.</span></span>

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="a42b8-223">在 Page Inspector 瀏覽器視窗中，將滑鼠指標移反白顯示 」 影片、 教學課程和範例 」 文字，讓 CSS 的 「 標記 」 標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="a42b8-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![將滑鼠游標停留在標記項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="a42b8-225">按一下以選取它的文字。</span><span class="sxs-lookup"><span data-stu-id="a42b8-225">Click the text to select it.</span></span> <span data-ttu-id="a42b8-226">對應的 CSS 標記選取器出現在底部**樣式**視窗。</span><span class="sxs-lookup"><span data-stu-id="a42b8-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![在 [樣式] 視窗中的標記選取器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="a42b8-228">按一下標記選取器。</span><span class="sxs-lookup"><span data-stu-id="a42b8-228">Click the mark selector.</span></span> <span data-ttu-id="a42b8-229">這會開啟*Site.css* web 應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="a42b8-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="a42b8-230">Site.css 索引標籤，並反白顯示對應的 CSS 選取器：</span><span class="sxs-lookup"><span data-stu-id="a42b8-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![標記樣式表中的選取器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="a42b8-232">選取並移除該程式行使用的背景色彩屬性。</span><span class="sxs-lookup"><span data-stu-id="a42b8-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="a42b8-233">您現在會使用新的 Visual Studio 2012 CSS 色彩選擇器來選擇的新色彩**標示**背景色彩屬性。</span><span class="sxs-lookup"><span data-stu-id="a42b8-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="a42b8-234">使用 Visual Studio 2012 CSS 色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="a42b8-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="a42b8-235">在 Visual Studio 2012 CSS 編輯器有色彩選擇器，可讓您輕鬆地選擇並插入色彩。</span><span class="sxs-lookup"><span data-stu-id="a42b8-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="a42b8-236">它具有簡單的色軸和 「 pop 停擺 」 選擇器，可提供較細微的控制。</span><span class="sxs-lookup"><span data-stu-id="a42b8-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="a42b8-237">色彩選擇器包含標準的調色盤的色彩、 支援標準的色彩名稱、 雜湊程式碼、 RGB、 RGBA、 HSL 和 HSLA 色彩，並維護一份您已使用最新文件中的色彩。</span><span class="sxs-lookup"><span data-stu-id="a42b8-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="a42b8-238">在一行中的背景色彩屬性，輸入"bc"然後按向下箭號一次。</span><span class="sxs-lookup"><span data-stu-id="a42b8-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="a42b8-239">當您輸入連字號分隔的屬性，例如 [背景色彩] 中的每個單字的第一個字元時，則 IntelliSense 會篩選清單，為您顯示符合的屬性：</span><span class="sxs-lookup"><span data-stu-id="a42b8-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Intellisense 篩選值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="a42b8-241">現在，請輸入一個冒號。</span><span class="sxs-lookup"><span data-stu-id="a42b8-241">Now type a colon.</span></span> <span data-ttu-id="a42b8-242">當您這樣做時，則會插入完整的背景色彩屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="a42b8-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="a42b8-243">型別**#** 或是**rgb (**，並會出現色彩選擇器列：</span><span class="sxs-lookup"><span data-stu-id="a42b8-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![CSS 色彩選擇器列](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="a42b8-245">若要查看色彩選擇器列的運作方式，按一下滑鼠指標，其色彩或按向下鍵，然後使用左邊和右邊的方向鍵來周遊的色彩。</span><span class="sxs-lookup"><span data-stu-id="a42b8-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="a42b8-246">當您造訪的色彩時，可預覽的背景色彩屬性的對應值：</span><span class="sxs-lookup"><span data-stu-id="a42b8-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![預覽的背景色彩屬性值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="a42b8-248">此時，您可以按 Enter，以選取值，然後完成 CSS 項目以分號 （;）。</span><span class="sxs-lookup"><span data-stu-id="a42b8-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="a42b8-249">現在，請繼續下一節，讓您可以看到色彩選擇器快顯清單中的運作方式。</span><span class="sxs-lookup"><span data-stu-id="a42b8-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="a42b8-250">使用 色彩選擇器快顯清單</span><span class="sxs-lookup"><span data-stu-id="a42b8-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="a42b8-251">時的色軸沒有您要尋找的確切色彩，您可以使用色彩選擇器快顯清單。</span><span class="sxs-lookup"><span data-stu-id="a42b8-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="a42b8-252">若要開啟它，請按一下雙 > 形箭號，最右邊的 [色彩] 列中，或一兩次按鍵盤上的向下箭號。</span><span class="sxs-lookup"><span data-stu-id="a42b8-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS 色彩選擇器快顯清單](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="a42b8-254">按一下右側的垂直列的色彩。</span><span class="sxs-lookup"><span data-stu-id="a42b8-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="a42b8-255">這會在主視窗中顯示該色彩的漸層。</span><span class="sxs-lookup"><span data-stu-id="a42b8-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="a42b8-256">藉由按下 Enter，選擇直接從直條的色彩，或按一下 選擇更精確的主視窗中的任何時間點。</span><span class="sxs-lookup"><span data-stu-id="a42b8-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="a42b8-257">如果您想要使用的電腦螢幕上沒有色彩 （它不一定要在 Visual Studio 使用者介面），您可以使用滴管工具右下角來擷取其值。</span><span class="sxs-lookup"><span data-stu-id="a42b8-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="a42b8-258">您也可以移動滑桿底端的色彩選擇器來變更色彩的不透明度。</span><span class="sxs-lookup"><span data-stu-id="a42b8-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="a42b8-259">這樣變更色彩的 RGBA 值的值，因為 RGBA 格式可以代表不透明度。</span><span class="sxs-lookup"><span data-stu-id="a42b8-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="a42b8-260">選取色彩之後，按下 Enter、，然後輸入 完成中的背景色彩項目分號*Site.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="a42b8-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="a42b8-261">頁面偵測器更新列</span><span class="sxs-lookup"><span data-stu-id="a42b8-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="a42b8-262">Page Inspector 立即偵測到的變更*Site.css*檔案 （或應用程式中的任何檔案） 並在 更新列中顯示警示。</span><span class="sxs-lookup"><span data-stu-id="a42b8-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![更新列](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="a42b8-264">若要儲存所有檔案，並重新整理 Page Inspector 瀏覽器，請按 Ctrl + Alt + Enter 或按一下更新列。</span><span class="sxs-lookup"><span data-stu-id="a42b8-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="a42b8-265">反白顯示色彩的變更會出現在瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="a42b8-265">The change in the highlight color appears in the browser:</span></span>

![反白顯示色彩變更](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="a42b8-267">請注意您方便地重新整理 Page Inspector 瀏覽器直接從 Visual Studio 環境內。</span><span class="sxs-lookup"><span data-stu-id="a42b8-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="a42b8-268">而不外部瀏覽器中使用 Page Inspector 可讓您保持在編輯器中，當您開發您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a42b8-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="a42b8-269">另請參閱</span><span class="sxs-lookup"><span data-stu-id="a42b8-269">See Also</span></span>

<span data-ttu-id="a42b8-270">[簡介 Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) （Channel 9 影片）</span><span class="sxs-lookup"><span data-stu-id="a42b8-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="a42b8-271">[Page Inspector 錯誤訊息](https://go.microsoft.com/?linkid=9813062)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="a42b8-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
