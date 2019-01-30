---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: 在 ASP.NET MVC 中使用 Page Inspector |Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 中的 Page Inspector 是整合式瀏覽器的 web 開發工具。 在 Page Inspector i 與整合式瀏覽器中，選取任何項目...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 0dea8b077878139a3f513cb51447b86a93fe55b8
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236532"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="c2033-104">在 ASP.NET MVC 中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c2033-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="c2033-105">由 Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="c2033-105">by Tim Ammann</span></span>

> <span data-ttu-id="c2033-106">Visual Studio 2012 中的 Page Inspector 是整合式瀏覽器的 web 開發工具。</span><span class="sxs-lookup"><span data-stu-id="c2033-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="c2033-107">在 整合式瀏覽器中，選取任何項目和 Page Inspector 立即會反白顯示項目的來源和 CSS。</span><span class="sxs-lookup"><span data-stu-id="c2033-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="c2033-108">您可以瀏覽任何的 MVC 檢視，快速尋找呈現的標記的來源並使用瀏覽器工具，直接在 Visual Studio 環境中。</span><span class="sxs-lookup"><span data-stu-id="c2033-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="c2033-109">觀看影片</span><span class="sxs-lookup"><span data-stu-id="c2033-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="c2033-110">本教學課程會示範如何啟用檢查模式中，然後快速找出並編輯您的 web 專案內的標記和 CSS。</span><span class="sxs-lookup"><span data-stu-id="c2033-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="c2033-111">本教學課程使用 MVC 專案中，但您也可以使用的 Page Inspector [Web Form](https://go.microsoft.com/?linkid=9802001)和其他 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c2033-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="c2033-112">本教學課程有下列各節：</span><span class="sxs-lookup"><span data-stu-id="c2033-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="c2033-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="c2033-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="c2033-114">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c2033-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="c2033-115">使用 Page Inspector 瀏覽至檢視</span><span class="sxs-lookup"><span data-stu-id="c2033-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="c2033-116">啟用檢查模式</span><span class="sxs-lookup"><span data-stu-id="c2033-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="c2033-117">若要變更標記中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c2033-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - <span data-ttu-id="c2033-118">[檢查模式和 [HTML] 視窗](#_6_inspection_mode)</span><span class="sxs-lookup"><span data-stu-id="c2033-118">[Inspection Mode and the HTML Window](#_6_inspection_mode)</span></span>
> - <span data-ttu-id="c2033-119">[在 [樣式] 視窗中預覽 CSS 變更](#_7_previewing_css)</span><span class="sxs-lookup"><span data-stu-id="c2033-119">[Preview CSS Changes in the Styles window](#_7_previewing_css)</span></span>
> - [<span data-ttu-id="c2033-120">CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="c2033-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="c2033-121">使用 CSS 色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="c2033-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="c2033-122">將動態頁面項目對應至 JavaScript</span><span class="sxs-lookup"><span data-stu-id="c2033-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="c2033-123">必要條件</span><span class="sxs-lookup"><span data-stu-id="c2033-123">Prerequisites</span></span>

- <span data-ttu-id="c2033-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)或是[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="c2033-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="c2033-125">若要取得最新版的 Page Inspector，請使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386)安裝 Windows Azure SDK for.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="c2033-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="c2033-126">Page Inspector 隨附於 Microsoft Web Developer Tools。</span><span class="sxs-lookup"><span data-stu-id="c2033-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="c2033-127">1.3 為最新版本。</span><span class="sxs-lookup"><span data-stu-id="c2033-127">The latest version is 1.3.</span></span> <span data-ttu-id="c2033-128">若要檢查哪些版本您有執行 Visual Studio，選取**關於 Microsoft Visual Studio**從**協助**功能表。</span><span class="sxs-lookup"><span data-stu-id="c2033-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="c2033-129">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c2033-129">Create a Web Application</span></span>

<span data-ttu-id="c2033-130">首先，建立您將會使用 Page Inspector 與 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c2033-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="c2033-131">在 Visual Studio 中，選擇**檔案** &gt; **新專案**。</span><span class="sxs-lookup"><span data-stu-id="c2033-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="c2033-132">在左側，展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c2033-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![新的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="c2033-134">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="c2033-134">Click **OK**.</span></span>

<span data-ttu-id="c2033-135">在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c2033-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="c2033-136">離開**Razor**做為預設檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="c2033-136">Leave **Razor** as the default view engine.</span></span>

![新的 ASP.NET MVC 專案-網際網路應用程式](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="c2033-138">在應用程式中開啟**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="c2033-138">The application opens in **Source** view.</span></span>

![原始碼檢視中的新 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="c2033-140">既然您已使用的應用程式時，您可以使用 Page Inspector 檢查和修改它。</span><span class="sxs-lookup"><span data-stu-id="c2033-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="c2033-141">使用 Page Inspector 瀏覽至檢視</span><span class="sxs-lookup"><span data-stu-id="c2033-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="c2033-142">在 Visual Studio 2012 中，您可以以滑鼠右鍵按一下任何檢視在您專案中，選取**Page Inspector 中的檢視**，Page Inspector 會找出路由，並且顯示頁面。</span><span class="sxs-lookup"><span data-stu-id="c2033-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="c2033-143">在 [**方案總管] 中**，展開**檢視**資料夾，然後**首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="c2033-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="c2033-144">以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="c2033-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Page Inspector 中檢視 Index.cshtml](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="c2033-146">根據預設，Page Inspector 停駐成為視窗左側的 Visual Studio 環境。</span><span class="sxs-lookup"><span data-stu-id="c2033-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="c2033-147">如果想要的話，您可以將它固定到其他位置，或取消停駐視窗。</span><span class="sxs-lookup"><span data-stu-id="c2033-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="c2033-148">請參閱[如何：排列和固定視窗](https://msdn.microsoft.com/library/z4y0hsax.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c2033-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="c2033-149">Page Inspector 視窗的上方窗格會顯示目前的網頁瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="c2033-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="c2033-150">下方窗格會顯示 HTML 標記，以及可讓您檢查頁面的不同層面的某些索引標籤中的頁面。</span><span class="sxs-lookup"><span data-stu-id="c2033-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="c2033-151">下方窗格大致[F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)Internet Explorer 中。</span><span class="sxs-lookup"><span data-stu-id="c2033-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Page Inspector 中的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="c2033-153">在本教學課程中，您將使用**HTML**並**樣式**索引標籤，以快速瀏覽並對應用程式進行變更。</span><span class="sxs-lookup"><span data-stu-id="c2033-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="c2033-154">EnableInspection 模式</span><span class="sxs-lookup"><span data-stu-id="c2033-154">EnableInspection Mode</span></span>

<span data-ttu-id="c2033-155">若要讓 Page Inspector 進入檢查模式，按一下**檢查** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c2033-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="c2033-156">在檢查模式中，當滑鼠指標停留在轉譯的頁面上的任何部分的對應的來源標記或程式碼會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="c2033-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![切換檢查模式](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="c2033-158">現在將滑鼠移 Page Inspector 中頁面的不同部分。</span><span class="sxs-lookup"><span data-stu-id="c2033-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="c2033-159">這麼做，將滑鼠指標變為大型的加號，和下方的項目會反白顯示：</span><span class="sxs-lookup"><span data-stu-id="c2033-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="c2033-161">當您移動滑鼠指標時，Visual Studio 就會反白顯示對應的 Razor 語法，在原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="c2033-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="c2033-162">如果 HTML 項目來自於另一個原始程式檔，Visual Studio 會自動開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="c2033-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="c2033-163">在 Page Inspector **HTML**索引標籤會顯示所產生的 Razor 語法的 HTML。</span><span class="sxs-lookup"><span data-stu-id="c2033-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="c2033-164">當您移動滑鼠指標時，會反白顯示的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="c2033-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="c2033-165">**樣式**索引標籤會顯示項目的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="c2033-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="c2033-166">若要變更標記中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="c2033-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="c2033-167">Page Inspector 可讓您尋找其位置可能不明顯的標記。</span><span class="sxs-lookup"><span data-stu-id="c2033-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="c2033-168">然後您可以修改標記，並查看產生的變更。</span><span class="sxs-lookup"><span data-stu-id="c2033-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="c2033-169">若要查看這種情況，請按一下**檢查**，然後捲動至頁面底部的 [Page Inspector] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="c2033-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="c2033-170">當您將滑鼠指標移到 [頁尾] 區域時，Page Inspector 隨即開啟\_Layout.cshtml 檔案並反白顯示您所選取的 [配置] 頁面的區段。</span><span class="sxs-lookup"><span data-stu-id="c2033-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="c2033-171">如您所見，頁尾會定義於版面配置檔，並不是檢視本身。</span><span class="sxs-lookup"><span data-stu-id="c2033-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![頁尾](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="c2033-173">現在將您的滑鼠指標移入線條 copyright <a id="a"></a>注意到。</span><span class="sxs-lookup"><span data-stu-id="c2033-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="c2033-174">在 [ \_Layout.cshtml] 頁面上，對應的線條會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="c2033-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![頁尾的著作權列反白顯示](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="c2033-176">在行結尾加入一些文字\_Layout.cshtml 檔案。</span><span class="sxs-lookup"><span data-stu-id="c2033-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="c2033-177">&lt;p&gt;&amp;複製;@DateTime.Now.Year -撼動我的 ASP.NET MVC 應用程式 ！ &lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="c2033-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="c2033-178">現在，按 Ctrl + Alt + Enter 或按一下 更新列，以查看 Page Inspector 瀏覽器視窗中的結果。</span><span class="sxs-lookup"><span data-stu-id="c2033-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![我的 ASP.NET 應用程式 Rocks ！](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="c2033-180">您或許以為 Index.cshtml 中的頁尾定義，但它原來是在\_Layout.cshtml，並找到您的 Page Inspector。</span><span class="sxs-lookup"><span data-stu-id="c2033-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="c2033-181">檢查模式和 [HTML] 視窗</span><span class="sxs-lookup"><span data-stu-id="c2033-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="c2033-182">接下來，您必須快速瀏覽 [HTML] 視窗，以及它如何為您對應項目。</span><span class="sxs-lookup"><span data-stu-id="c2033-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="c2033-183">按一下 **檢查**至 Page Inspector 進入檢查模式。</span><span class="sxs-lookup"><span data-stu-id="c2033-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="c2033-184">按一下 顯示 「 您 logohere 」 頁面的上半部。</span><span class="sxs-lookup"><span data-stu-id="c2033-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="c2033-185">您正在檢查詳細資料，因此不再會在瀏覽器視窗中的顯示變更，您將滑鼠游標移特定項的目。</span><span class="sxs-lookup"><span data-stu-id="c2033-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="c2033-186">現在將滑鼠指標移到**HTML**視窗。</span><span class="sxs-lookup"><span data-stu-id="c2033-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="c2033-187">當您移動滑鼠指標時，Page Inspector 概要說明中的項目**HTML**視窗並反白顯示對應的項目，在瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="c2033-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML 視窗](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="c2033-189">如往常一般，Page Inspector 隨即開啟\_Layout.cshtml 檔案供您在暫存的索引標籤中。按一下  \_Layout.cshtml 暫時索引標籤，以及對應的標記會反白顯示&lt;標頭&gt;為您的區段：</span><span class="sxs-lookup"><span data-stu-id="c2033-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![反白顯示的標記](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="c2033-191">在 [樣式] 視窗中預覽 CSS 變更</span><span class="sxs-lookup"><span data-stu-id="c2033-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="c2033-192">接下來，您會使用 Page Inspector**樣式**預覽 CSS 變更的視窗。</span><span class="sxs-lookup"><span data-stu-id="c2033-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="c2033-193">按一下 **檢查**至 Page Inspector 進入檢查模式。</span><span class="sxs-lookup"><span data-stu-id="c2033-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="c2033-194">在 Page Inspector 瀏覽器視窗中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="c2033-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="c2033-196">按一下 div.content 包裝函式區段內，，然後將滑鼠指標移到**樣式**視窗。</span><span class="sxs-lookup"><span data-stu-id="c2033-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="c2033-197">**樣式**視窗會顯示所有此項目的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="c2033-197">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="c2033-198">請向下捲動尋找.featured.content 包裝函式類別選取器。</span><span class="sxs-lookup"><span data-stu-id="c2033-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="c2033-199">現在，請清除的背景色彩屬性的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="c2033-199">Now clear the checkbox for the background-color property.</span></span>

![清除的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="c2033-201">請注意變更預覽立即的 Page Inspector 瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="c2033-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="c2033-202">再次選取核取方塊，然後按兩下屬性值，並將它變更為紅色。</span><span class="sxs-lookup"><span data-stu-id="c2033-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="c2033-203">變更會立即顯示：</span><span class="sxs-lookup"><span data-stu-id="c2033-203">The change shows immediately:</span></span>

![紅色的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="c2033-205">**樣式**視窗可讓您輕鬆地測試並預覽 CSS 變更之前，您將變更認可到樣式表本身。</span><span class="sxs-lookup"><span data-stu-id="c2033-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="c2033-206">CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="c2033-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="c2033-207">這項功能需要 Page Inspector 1.3 版。</span><span class="sxs-lookup"><span data-stu-id="c2033-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="c2033-208">CSS 自動同步處理功能可讓您直接編輯 CSS 檔案並查看立即在 Page Inspector 瀏覽器中的變更。</span><span class="sxs-lookup"><span data-stu-id="c2033-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="c2033-209">按一下 **檢查**至 Page Inspector 進入檢查模式。</span><span class="sxs-lookup"><span data-stu-id="c2033-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="c2033-210">在 Page Inspector 瀏覽器中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="c2033-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="c2033-211">再按一次就會選取這個項目。</span><span class="sxs-lookup"><span data-stu-id="c2033-211">Click once to select this element.</span></span>

<span data-ttu-id="c2033-212">**樣式**視窗會顯示所有此項目的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="c2033-212">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="c2033-213">請向下捲動尋找.featured.content 包裝函式類別選取器。</span><span class="sxs-lookup"><span data-stu-id="c2033-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="c2033-214">按一下 「.featured.content-包裝函式 」。</span><span class="sxs-lookup"><span data-stu-id="c2033-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="c2033-215">Page Inspector 隨即開啟，定義此樣式 (Site.css)，並反白顯示對應的 CSS 樣式的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="c2033-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="c2033-216">現在的值變更`background-color`為"red"。</span><span class="sxs-lookup"><span data-stu-id="c2033-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="c2033-217">變更會立即出現在 Page Inspector 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c2033-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="c2033-218">使用 CSS 色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="c2033-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="c2033-219">在 Visual Studio 2012 CSS 編輯器有色彩選擇器，可讓您輕鬆地選擇並插入色彩。</span><span class="sxs-lookup"><span data-stu-id="c2033-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="c2033-220">色彩選擇器包含標準的調色盤的色彩、 支援標準的色彩名稱、 雜湊程式碼、 RGB、 RGBA、 HSL 和 HSLA 色彩，並維護一份您已使用最新文件中的色彩。</span><span class="sxs-lookup"><span data-stu-id="c2033-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="c2033-221">在上一個區段中，您可以變更的值`background-color`屬性。</span><span class="sxs-lookup"><span data-stu-id="c2033-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="c2033-222">要叫用色彩選擇器，將插入點之後的屬性名稱和類型**#** 或是**rgb (**。</span><span class="sxs-lookup"><span data-stu-id="c2033-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![CSS 色彩選擇器列](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="c2033-224">按一下色彩，以選取它，或按向下鍵，然後使用左邊和右邊的方向鍵來周遊的色彩。</span><span class="sxs-lookup"><span data-stu-id="c2033-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="c2033-225">當您造訪的色彩時，可預覽對應的十六進位值：</span><span class="sxs-lookup"><span data-stu-id="c2033-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![預覽的背景色彩屬性值](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="c2033-227">如果在色軸沒有您想要的確切色彩，您可以使用色彩選擇器快顯清單。</span><span class="sxs-lookup"><span data-stu-id="c2033-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="c2033-228">若要開啟它，請按一下雙 > 形箭號，最右邊的 [色彩] 列中，或一兩次按鍵盤上的向下箭號。</span><span class="sxs-lookup"><span data-stu-id="c2033-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS 色彩選擇器快顯清單](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="c2033-230">按一下右側的垂直列的色彩。</span><span class="sxs-lookup"><span data-stu-id="c2033-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="c2033-231">這會在主視窗中顯示該色彩的漸層。</span><span class="sxs-lookup"><span data-stu-id="c2033-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="c2033-232">藉由按下 Enter，選擇直接從直條的色彩，或按一下 選擇更精確的主視窗中的任何時間點。</span><span class="sxs-lookup"><span data-stu-id="c2033-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="c2033-233">如果您想要使用的電腦螢幕上沒有色彩 （它不一定要在 Visual Studio 使用者介面），您可以使用滴管工具右下角來擷取其值。</span><span class="sxs-lookup"><span data-stu-id="c2033-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="c2033-234">您也可以移動滑桿底端的色彩選擇器來變更色彩的不透明度。</span><span class="sxs-lookup"><span data-stu-id="c2033-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="c2033-235">這樣做的變更色彩值的 RGBA 值，因為 RGBA 格式可以代表不透明度。</span><span class="sxs-lookup"><span data-stu-id="c2033-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="c2033-236">選取色彩之後，按下 Enter、，然後輸入 完成中的背景色彩項目分號*Site.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="c2033-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="c2033-237">頁面偵測器更新列</span><span class="sxs-lookup"><span data-stu-id="c2033-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="c2033-238">Page Inspector 立即偵測到的變更*Site.css*檔案，並在 更新列會顯示警示。</span><span class="sxs-lookup"><span data-stu-id="c2033-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![更新列](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="c2033-240">若要儲存所有檔案，並重新整理 Page Inspector 瀏覽器，請按 Ctrl + Alt + Enter 或按一下更新列。</span><span class="sxs-lookup"><span data-stu-id="c2033-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="c2033-241">反白顯示色彩的變更會出現在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c2033-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="c2033-242">將動態頁面項目對應至 JavaScript</span><span class="sxs-lookup"><span data-stu-id="c2033-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="c2033-243">現代化 web 應用程式，在網頁中的項目通常動態產生使用 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="c2033-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="c2033-244">這表示沒有靜態標記 （HTML 或 Razor） 對應至這些頁面項目。</span><span class="sxs-lookup"><span data-stu-id="c2033-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="c2033-245">1.3 版中，使用 Page Inspector 可以現在對應的頁面回傳至相對應的 JavaScript 程式碼以動態方式新增的項目。</span><span class="sxs-lookup"><span data-stu-id="c2033-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="c2033-246">為了示範這項功能，我們將使用[單一頁面應用程式 (SPA) 範本](../../../single-page-application/overview/introduction/knockoutjs-template.md)。</span><span class="sxs-lookup"><span data-stu-id="c2033-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c2033-247">SPA 範本需要[ASP.NET 和 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)更新。</span><span class="sxs-lookup"><span data-stu-id="c2033-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="c2033-248">在 Visual Studio 中，選擇**檔案** &gt; **新專案**。</span><span class="sxs-lookup"><span data-stu-id="c2033-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="c2033-249">在左側，展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c2033-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="c2033-250">按一下 [確定] 。</span><span class="sxs-lookup"><span data-stu-id="c2033-250">Click **OK**.</span></span>

<span data-ttu-id="c2033-251">在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**單一頁面應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c2033-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="c2033-252">在 [方案總管] 中，展開**檢視**資料夾，然後**首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="c2033-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="c2033-253">以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="c2033-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="c2033-254">在 Page Inspector 瀏覽器中第一件事也就是顯示為登入頁面。</span><span class="sxs-lookup"><span data-stu-id="c2033-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="c2033-255">按一下 「 註冊 」，並建立使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="c2033-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="c2033-256">一旦您註冊，應用程式登入，並建立一些範例項目待辦事項清單。</span><span class="sxs-lookup"><span data-stu-id="c2033-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="c2033-257">按一下 **檢查**至 Page Inspector 進入檢查模式。</span><span class="sxs-lookup"><span data-stu-id="c2033-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="c2033-258">在 Page Inspector 瀏覽器中，按一下 待辦事項項目。</span><span class="sxs-lookup"><span data-stu-id="c2033-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="c2033-259">請注意，而不是正在以藍色強調顯示，項目以橙色醒目提示，使用"JS"的項目名稱旁邊。</span><span class="sxs-lookup"><span data-stu-id="c2033-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="c2033-260">這表示透過指令碼建立的項目是以動態方式。</span><span class="sxs-lookup"><span data-stu-id="c2033-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="c2033-261">此外，橙色底線出現在**呼叫堆疊** 索引標籤。這表示**呼叫堆疊**窗格就會有項目相關的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="c2033-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="c2033-262">按一下 **呼叫堆疊** 索引標籤。**呼叫堆疊** 窗格會顯示建立項目的 JavaScript 呼叫的呼叫堆疊。</span><span class="sxs-lookup"><span data-stu-id="c2033-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="c2033-263">呼叫外部程式庫例如 jQuery 會摺疊，以便您可以輕易看到您的應用程式指令碼的呼叫。</span><span class="sxs-lookup"><span data-stu-id="c2033-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="c2033-264">若要查看完整的堆疊，包括呼叫外部程式庫，您可以展開標示為 [外部程式庫] 的節點：</span><span class="sxs-lookup"><span data-stu-id="c2033-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="c2033-265">如果您按一下呼叫堆疊中的項目時，Visual Studio 開啟程式碼檔，並反白顯示對應的指令碼。</span><span class="sxs-lookup"><span data-stu-id="c2033-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="c2033-266">另請參閱</span><span class="sxs-lookup"><span data-stu-id="c2033-266">See Also</span></span>

<span data-ttu-id="c2033-267">[使用 Visual Studio 的 ASP.NET MVC 4 簡介](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)（ASP.net 網站）</span><span class="sxs-lookup"><span data-stu-id="c2033-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="c2033-268">[簡介 Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) （Channel 9 影片）</span><span class="sxs-lookup"><span data-stu-id="c2033-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="c2033-269">[Page Inspector 錯誤訊息](https://go.microsoft.com/?linkid=9813062)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="c2033-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
