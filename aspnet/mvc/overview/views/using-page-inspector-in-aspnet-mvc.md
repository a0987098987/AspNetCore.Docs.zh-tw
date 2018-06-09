---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: 在 ASP.NET MVC 中使用 Page Inspector |Microsoft 文件
author: rick-anderson
description: Page Inspector 在 Visual Studio 2012 中是以整合式瀏覽器的 web 開發工具。 在 Page Inspector i 與整合式瀏覽器中，選取任何項目...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034512"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="29451-104">在 ASP.NET MVC 中使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="29451-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="29451-105">由 Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="29451-105">by Tim Ammann</span></span>

> <span data-ttu-id="29451-106">Page Inspector 在 Visual Studio 2012 中是以整合式瀏覽器的 web 開發工具。</span><span class="sxs-lookup"><span data-stu-id="29451-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="29451-107">在整合式瀏覽器中，選取任何項目和 Page Inspector 立即會反白顯示項目的來源和 CSS。</span><span class="sxs-lookup"><span data-stu-id="29451-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="29451-108">您可以瀏覽任何的 MVC 檢視，快速找不到來源呈現標記，並使用瀏覽器工具，Visual Studio 環境內的權限。</span><span class="sxs-lookup"><span data-stu-id="29451-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="29451-109">觀賞視訊</span><span class="sxs-lookup"><span data-stu-id="29451-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="29451-110">本教學課程會示範如何啟用檢查模式中，然後快速找出並編輯您的 web 專案內的標記和 CSS。</span><span class="sxs-lookup"><span data-stu-id="29451-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="29451-111">本教學課程使用 MVC 專案，但您也可以使用 Page Inspector 的[Web Form](https://go.microsoft.com/?linkid=9802001)和其他的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="29451-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="29451-112">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="29451-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="29451-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="29451-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="29451-114">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="29451-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="29451-115">使用 Page Inspector 瀏覽 以檢視</span><span class="sxs-lookup"><span data-stu-id="29451-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="29451-116">啟用檢查模式</span><span class="sxs-lookup"><span data-stu-id="29451-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="29451-117">若要變更標記使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="29451-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - <span data-ttu-id="29451-118">[檢查模式和 [HTML] 視窗](#_6_inspection_mode)</span><span class="sxs-lookup"><span data-stu-id="29451-118">[Inspection Mode and the HTML Window](#_6_inspection_mode)</span></span>
> - <span data-ttu-id="29451-119">[在 [樣式] 視窗中的預覽 CSS 變更](#_7_previewing_css)</span><span class="sxs-lookup"><span data-stu-id="29451-119">[Preview CSS Changes in the Styles window](#_7_previewing_css)</span></span>
> - [<span data-ttu-id="29451-120">CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="29451-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="29451-121">使用 CSS 色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="29451-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="29451-122">對應至 JavaScript 的動態頁面項目</span><span class="sxs-lookup"><span data-stu-id="29451-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="29451-123">必要條件</span><span class="sxs-lookup"><span data-stu-id="29451-123">Prerequisites</span></span>

- <span data-ttu-id="29451-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)或[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="29451-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="29451-125">若要取得 Page Inspector 的最新版本，請使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386)安裝 Windows Azure SDK for.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="29451-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="29451-126">Page Inspector 隨附 Microsoft Web Developer Tools。</span><span class="sxs-lookup"><span data-stu-id="29451-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="29451-127">最新版本是 1.3。</span><span class="sxs-lookup"><span data-stu-id="29451-127">The latest version is 1.3.</span></span> <span data-ttu-id="29451-128">若要檢查的版本中，執行 Visual Studio 也可以選取**關於 Microsoft Visual Studio**從**協助**功能表。</span><span class="sxs-lookup"><span data-stu-id="29451-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="29451-129">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="29451-129">Create a Web Application</span></span>

<span data-ttu-id="29451-130">首先，建立會使用 Page Inspector 與 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="29451-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="29451-131">在 Visual Studio 中，選擇 **檔案** &gt; **新專案**。</span><span class="sxs-lookup"><span data-stu-id="29451-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="29451-132">在左邊展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="29451-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![新的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="29451-134">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="29451-134">Click **OK**.</span></span>

<span data-ttu-id="29451-135">在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="29451-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="29451-136">保留**Razor**做為預設檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="29451-136">Leave **Razor** as the default view engine.</span></span>

![新的 ASP.NET MVC 專案-網際網路應用程式](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="29451-138">應用程式中開啟**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="29451-138">The application opens in **Source** view.</span></span>

![原始碼檢視中的新 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="29451-140">既然您已使用應用程式，您可以使用 Page Inspector 檢查和修改它。</span><span class="sxs-lookup"><span data-stu-id="29451-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="29451-141">使用 Page Inspector 瀏覽 以檢視</span><span class="sxs-lookup"><span data-stu-id="29451-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="29451-142">在 Visual Studio 2012 中，您可以以滑鼠右鍵按一下任何檢視在專案中，選取**Page Inspector 中的檢視**，Page Inspector 會找出路由，並且會顯示頁面。</span><span class="sxs-lookup"><span data-stu-id="29451-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="29451-143">在**方案總管 中**，依序展開**檢視**資料夾，然後**首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="29451-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="29451-144">以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="29451-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![在 Page Inspector 中檢視 Index.cshtml](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="29451-146">根據預設，Page Inspector 停駐成為視窗在 Visual Studio 環境的左半部。</span><span class="sxs-lookup"><span data-stu-id="29451-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="29451-147">如果您想要的話，您可以在其他地方，將它停駐或取消停駐視窗。</span><span class="sxs-lookup"><span data-stu-id="29451-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="29451-148">請參閱[如何： 排列和停駐視窗](https://msdn.microsoft.com/library/z4y0hsax.aspx)。</span><span class="sxs-lookup"><span data-stu-id="29451-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="29451-149">頁面偵測器視窗的上方窗格會顯示目前的網頁瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="29451-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="29451-150">下方窗格會顯示在 HTML 標記中，以及可讓您檢查頁面的不同層面的某些索引標籤頁面。</span><span class="sxs-lookup"><span data-stu-id="29451-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="29451-151">下方窗格會類似於[F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)Internet Explorer 中。</span><span class="sxs-lookup"><span data-stu-id="29451-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Page Inspector 中的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="29451-153">在此教學課程中，您將使用**HTML**和**樣式**索引標籤，以快速巡覽並對應用程式進行變更。</span><span class="sxs-lookup"><span data-stu-id="29451-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="29451-154">EnableInspection 模式</span><span class="sxs-lookup"><span data-stu-id="29451-154">EnableInspection Mode</span></span>

<span data-ttu-id="29451-155">若要讓 Page Inspector 進入檢查模式，按一下**檢查** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="29451-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="29451-156">在檢查模式中，當您滑鼠指標停在呈現的任何的網頁部分的對應的來源標記或程式碼會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="29451-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![切換檢查模式](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="29451-158">現在將滑鼠移 Page Inspector 中頁面的不同部分。</span><span class="sxs-lookup"><span data-stu-id="29451-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="29451-159">當您這樣做，滑鼠指標會變成一個大型的加號和下方的項目會反白顯示：</span><span class="sxs-lookup"><span data-stu-id="29451-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="29451-161">當您移動滑鼠指標時，Visual Studio 會反白顯示對應的 Razor 語法的原始程式檔中。</span><span class="sxs-lookup"><span data-stu-id="29451-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="29451-162">如果 HTML 項目來自於另一個原始程式檔，Visual Studio 會自動開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="29451-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="29451-163">Page Inspector 中**HTML**索引標籤會顯示所產生的 Razor 語法的 HTML。</span><span class="sxs-lookup"><span data-stu-id="29451-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="29451-164">當您移動滑鼠指標時，會反白顯示的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="29451-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="29451-165">**樣式**索引標籤會顯示項目的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="29451-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="29451-166">若要變更標記使用 Page Inspector</span><span class="sxs-lookup"><span data-stu-id="29451-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="29451-167">Page Inspector 可讓您尋找其位置可能不明顯的標記。</span><span class="sxs-lookup"><span data-stu-id="29451-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="29451-168">然後您可以修改標記，以及查看產生的變更。</span><span class="sxs-lookup"><span data-stu-id="29451-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="29451-169">若要查看此，請按一下**檢查**然後捲動至頁面底部的 頁面偵測器視窗中。</span><span class="sxs-lookup"><span data-stu-id="29451-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="29451-170">當您將滑鼠指標移到 [頁尾] 區域時，Page Inspector 會開啟\_Layout.cshtml 檔案並反白顯示您已選取 [配置] 頁面的區段。</span><span class="sxs-lookup"><span data-stu-id="29451-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="29451-171">如您所見，頁尾會配置檔，並不檢視本身中定義。</span><span class="sxs-lookup"><span data-stu-id="29451-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![頁尾](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="29451-173">現在將滑鼠指標移線條 copyright <a id="a"></a>會注意到。</span><span class="sxs-lookup"><span data-stu-id="29451-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="29451-174">在\_Layout.cshtml 頁面上，對應的線條會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="29451-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![頁尾著作權列反白顯示](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="29451-176">中的一行的結尾加入一些文字\_Layout.cshtml 檔案。</span><span class="sxs-lookup"><span data-stu-id="29451-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="29451-177">&lt;p&gt;&amp;複製;@DateTime.Now.Year -撼動我的 ASP.NET MVC 應用程式 ！ &lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="29451-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="29451-178">現在，請按 Ctrl + Alt + Enter 或按一下 [更新] 列，請參閱 Page Inspector 瀏覽器視窗中的結果。</span><span class="sxs-lookup"><span data-stu-id="29451-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![我的 ASP.NET 應用程式岩石 ！](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="29451-180">您想頁尾 Index.cshtml 中, 所定義，但它已在\_Layout.cshtml 和為您找到它的 Page Inspector。</span><span class="sxs-lookup"><span data-stu-id="29451-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="29451-181">檢查模式和 [HTML] 視窗</span><span class="sxs-lookup"><span data-stu-id="29451-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="29451-182">接下來，您必須快速瀏覽 [HTML] 視窗，以及它如何為您對應項目。</span><span class="sxs-lookup"><span data-stu-id="29451-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="29451-183">按一下**檢查**將 Page Inspector 在檢查模式中。</span><span class="sxs-lookup"><span data-stu-id="29451-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="29451-184">按一下網頁顯示 「 您 logohere 」 的上半部。</span><span class="sxs-lookup"><span data-stu-id="29451-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="29451-185">您正在檢查詳細資料，因此，在瀏覽器視窗中的顯示無法再變更當您移動滑鼠指標的特定項的目。</span><span class="sxs-lookup"><span data-stu-id="29451-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="29451-186">現在將滑鼠指標移至**HTML**視窗。</span><span class="sxs-lookup"><span data-stu-id="29451-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="29451-187">當您移動滑鼠指標時，Page Inspector 概要說明中的項目**HTML**視窗並反白顯示瀏覽器視窗中的對應項目。</span><span class="sxs-lookup"><span data-stu-id="29451-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML 視窗](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="29451-189">如往常一般，Page Inspector 隨即開啟\_Layout.cshtml 檔案您在暫時性索引標籤。按一下\_Layout.cshtml 暫存索引標籤上，而對應的標記將會反白顯示在&lt;標頭&gt;> 一節讓您：</span><span class="sxs-lookup"><span data-stu-id="29451-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![反白顯示的標記](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="29451-191">在 [樣式] 視窗中的預覽 CSS 變更</span><span class="sxs-lookup"><span data-stu-id="29451-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="29451-192">接下來，您會使用 Page Inspector**樣式**視窗來預覽 CSS 所做的變更。</span><span class="sxs-lookup"><span data-stu-id="29451-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="29451-193">按一下**檢查**將 Page Inspector 在檢查模式中。</span><span class="sxs-lookup"><span data-stu-id="29451-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="29451-194">在 Page Inspector 瀏覽器視窗中，將滑鼠指標移 「 首頁 」 一節，直到**div.content 包裝函式**標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="29451-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="29451-196">按一下 div.content 包裝函式區段內，，然後將滑鼠指標移至**樣式**視窗。</span><span class="sxs-lookup"><span data-stu-id="29451-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="29451-197">**Syles**  視窗會顯示所有此項目的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="29451-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="29451-198">向下捲動至 尋找.featured.content 包裝函式類別選取器。</span><span class="sxs-lookup"><span data-stu-id="29451-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="29451-199">現在，請清除的背景色彩屬性的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="29451-199">Now clear the checkbox for the background-color property.</span></span>

![清除的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="29451-201">請注意變更預覽立即的 Page Inspector 瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="29451-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="29451-202">再次選取核取方塊，然後按兩下屬性值，並將它變更為紅色。</span><span class="sxs-lookup"><span data-stu-id="29451-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="29451-203">變更會立即顯示：</span><span class="sxs-lookup"><span data-stu-id="29451-203">The change shows immediately:</span></span>

![紅色的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="29451-205">**樣式**視窗讓它易於測試和預覽 CSS 之前變更了您將變更認可到樣式表本身。</span><span class="sxs-lookup"><span data-stu-id="29451-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="29451-206">CSS 自動同步處理</span><span class="sxs-lookup"><span data-stu-id="29451-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="29451-207">此功能需要 Page Inspector 1.3 版。</span><span class="sxs-lookup"><span data-stu-id="29451-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="29451-208">CSS 自動同步處理功能可讓您直接編輯 CSS 檔案，並查看立即在 Page Inspector 瀏覽器中的變更。</span><span class="sxs-lookup"><span data-stu-id="29451-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="29451-209">按一下**檢查**將 Page Inspector 在檢查模式中。</span><span class="sxs-lookup"><span data-stu-id="29451-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="29451-210">Page Inspector 瀏覽器中將滑鼠指標移 「 首頁 」 一節，直到**div.content 包裝函式**標籤會出現。</span><span class="sxs-lookup"><span data-stu-id="29451-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="29451-211">按一下以選取此項目。</span><span class="sxs-lookup"><span data-stu-id="29451-211">Click once to select this element.</span></span>

<span data-ttu-id="29451-212">**Syles**  視窗會顯示所有此項目的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="29451-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="29451-213">向下捲動至 尋找.featured.content 包裝函式類別選取器。</span><span class="sxs-lookup"><span data-stu-id="29451-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="29451-214">按一下 「.featured.content-包裝函式 」。</span><span class="sxs-lookup"><span data-stu-id="29451-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="29451-215">Page Inspector 開啟定義這個樣式 (Site.css)，並反白顯示對應的 CSS 樣式的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="29451-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="29451-216">值現在變更`background-color`至"red"。</span><span class="sxs-lookup"><span data-stu-id="29451-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="29451-217">變更會立即出現在 Page Inspector 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="29451-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="29451-218">使用 CSS 色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="29451-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="29451-219">CSS 編輯器，Visual Studio 2012 中的會有色彩選擇器，可讓您輕鬆地選擇及插入色彩。</span><span class="sxs-lookup"><span data-stu-id="29451-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="29451-220">色彩選擇器包含標準的調色盤的色彩、 支援標準的色彩名稱、 雜湊程式碼、 RGB、 RGBA、 HSL 和 HSLA 色彩，以及維護一份您在文件最近使用的色彩。</span><span class="sxs-lookup"><span data-stu-id="29451-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="29451-221">在前一個區段中，您可以變更的值`background-color`屬性。</span><span class="sxs-lookup"><span data-stu-id="29451-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="29451-222">要叫用色彩選擇器，將插入點的屬性名稱和型別之後**#** 或**rgb (**。</span><span class="sxs-lookup"><span data-stu-id="29451-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![CSS 色彩選擇器列](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="29451-224">按一下色彩選取它，或按向下鍵，然後使用左邊和右邊的方向鍵來周遊色彩。</span><span class="sxs-lookup"><span data-stu-id="29451-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="29451-225">當您瀏覽色彩時，可預覽對應的十六進位值：</span><span class="sxs-lookup"><span data-stu-id="29451-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![預覽的背景色彩屬性值](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="29451-227">如果在色軸沒有完全您想要的色彩，您可以使用色彩選擇器快顯清單。</span><span class="sxs-lookup"><span data-stu-id="29451-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="29451-228">若要開啟它，請按一下色軸，右端 double > 形箭號或次按鍵盤上的向下箭號。</span><span class="sxs-lookup"><span data-stu-id="29451-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS 色彩選擇器快顯清單](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="29451-230">按一下右側的垂直列中的色彩。</span><span class="sxs-lookup"><span data-stu-id="29451-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="29451-231">這會在主視窗中顯示該色彩漸層。</span><span class="sxs-lookup"><span data-stu-id="29451-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="29451-232">按下 Enter，直接從垂直列中選擇的色彩，或按一下任何一點，而精確度卻選擇主視窗中。</span><span class="sxs-lookup"><span data-stu-id="29451-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="29451-233">如果您想要使用您電腦螢幕上沒有的色彩 （它不一定要在 Visual Studio 使用者介面內），您可以使用滴管工具右下方的擷取其值。</span><span class="sxs-lookup"><span data-stu-id="29451-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="29451-234">您也可以移動底部的色彩選擇器的滑桿來變更色彩的不透明度。</span><span class="sxs-lookup"><span data-stu-id="29451-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="29451-235">這麼做會變更色彩 RGBA 值的值因為 RGBA 格式可以代表不透明度。</span><span class="sxs-lookup"><span data-stu-id="29451-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="29451-236">您已選擇色彩之後，按 Enter 鍵，並接著輸入分號以完成中的背景色彩項目*Site.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="29451-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="29451-237">頁面偵測器更新列</span><span class="sxs-lookup"><span data-stu-id="29451-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="29451-238">Page Inspector 可以立即偵測到變更*Site.css*檔並且顯示在 更新列中的警示。</span><span class="sxs-lookup"><span data-stu-id="29451-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![更新列](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="29451-240">若要儲存您的檔案，並重新整理 Page Inspector 瀏覽器，請按 Ctrl + Alt + Enter 或按一下更新列。</span><span class="sxs-lookup"><span data-stu-id="29451-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="29451-241">反白顯示色彩的變更會出現在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="29451-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="29451-242">對應至 JavaScript 的動態頁面項目</span><span class="sxs-lookup"><span data-stu-id="29451-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="29451-243">現代化 web 應用程式，在網頁中的項目通常動態產生的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="29451-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="29451-244">這表示沒有任何靜態標記 （HTML 或 Razor） 對應至這些頁面項目。</span><span class="sxs-lookup"><span data-stu-id="29451-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="29451-245">1.3 版中，使用 Page Inspector 才能現在對應頁面回傳至對應的 JavaScript 程式碼以動態方式加入的項目。</span><span class="sxs-lookup"><span data-stu-id="29451-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="29451-246">若要示範這項功能，我們將使用[單一頁面應用程式 (SPA) 範本](../../../single-page-application/overview/introduction/knockoutjs-template.md)。</span><span class="sxs-lookup"><span data-stu-id="29451-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="29451-247">SPA 範本需要[ASP.NET 和 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)更新。</span><span class="sxs-lookup"><span data-stu-id="29451-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="29451-248">在 Visual Studio 中，選擇 **檔案** &gt; **新專案**。</span><span class="sxs-lookup"><span data-stu-id="29451-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="29451-249">在左邊展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="29451-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="29451-250">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="29451-250">Click **OK**.</span></span>

<span data-ttu-id="29451-251">在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**單一網頁應用程式**。</span><span class="sxs-lookup"><span data-stu-id="29451-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="29451-252">在 [方案總管] 中，展開**檢視**資料夾，然後**首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="29451-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="29451-253">以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="29451-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="29451-254">首先也就是顯示在 Page Inspector 瀏覽器是登入頁面。</span><span class="sxs-lookup"><span data-stu-id="29451-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="29451-255">按一下 「 註冊 」 並建立使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="29451-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="29451-256">一旦您註冊時，應用程式會您登入，並建立一些範例項目待辦事項清單。</span><span class="sxs-lookup"><span data-stu-id="29451-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="29451-257">按一下**檢查**將 Page Inspector 在檢查模式中。</span><span class="sxs-lookup"><span data-stu-id="29451-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="29451-258">在 Page Inspector 瀏覽器中，按一下其中一個待辦項目。</span><span class="sxs-lookup"><span data-stu-id="29451-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="29451-259">請注意，而不是所反白顯示為藍色，項目以橙色醒目提示，與 「 JS"的項目名稱旁邊。</span><span class="sxs-lookup"><span data-stu-id="29451-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="29451-260">這表示透過指令碼建立的項目是以動態方式。</span><span class="sxs-lookup"><span data-stu-id="29451-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="29451-261">此外，橙色會出現底線上**呼叫堆疊** 索引標籤。這表示**呼叫堆疊**窗格就會有多個項目的資訊。</span><span class="sxs-lookup"><span data-stu-id="29451-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="29451-262">按一下**呼叫堆疊** 索引標籤。**呼叫堆疊** 窗格會顯示建立項目的 JavaScript 呼叫的呼叫堆疊。</span><span class="sxs-lookup"><span data-stu-id="29451-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="29451-263">呼叫外部程式庫例如 jQuery 會摺疊，使您可以輕鬆地看到您的應用程式指令碼的呼叫。</span><span class="sxs-lookup"><span data-stu-id="29451-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="29451-264">若要查看完整的堆疊，包括呼叫外部程式庫，您可以展開標示為 [外部程式庫] 的節點：</span><span class="sxs-lookup"><span data-stu-id="29451-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="29451-265">如果您按一下呼叫堆疊中的項目時，Visual Studio 開啟程式碼檔，並反白顯示對應的指令碼。</span><span class="sxs-lookup"><span data-stu-id="29451-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="29451-266">另請參閱</span><span class="sxs-lookup"><span data-stu-id="29451-266">See Also</span></span>

<span data-ttu-id="29451-267">[簡介 Visual Studio 中的 ASP.NET MVC 4](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) （ASP.net 網站）</span><span class="sxs-lookup"><span data-stu-id="29451-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="29451-268">[簡介頁面偵測器](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/)（Channel 9 影片）</span><span class="sxs-lookup"><span data-stu-id="29451-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="29451-269">[Page Inspector 錯誤訊息](https://go.microsoft.com/?linkid=9813062)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="29451-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
