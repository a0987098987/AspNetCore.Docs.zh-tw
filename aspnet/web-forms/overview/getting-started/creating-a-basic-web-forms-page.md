---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: 建立基本的 ASP.NET 4.5 Web Form 頁面中的 Visual Studio 2013 |Microsoft 文件
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 0d44a8f607df3a45ef312820f85f269c7a2c9c1e
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/18/2018
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a><span data-ttu-id="cb282-102">建立基本的 ASP.NET 4.5 Web Form 頁面中的 Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="cb282-102">Creating a Basic ASP.NET 4.5 Web Forms Page in Visual Studio 2013</span></span>
====================
<span data-ttu-id="cb282-103">由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="cb282-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="cb282-104">本逐步解說為您提供的簡介中的 Web 開發環境[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)和[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="cb282-104">This walkthrough provides you with an introduction to the Web development environment in [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) and in [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="cb282-105">本逐步解說會引導您完成建立簡單的 ASP.NET Web Form 頁面，並說明如何建立新的頁面中，加入控制項，並撰寫程式碼的基本技巧。</span><span class="sxs-lookup"><span data-stu-id="cb282-105">This walkthrough guides you through creating a simple ASP.NET Web Forms page and illustrates the basic techniques of creating a new page, adding controls, and writing code.</span></span>

<span data-ttu-id="cb282-106">這個逐步解說中所述的工作包括：</span><span class="sxs-lookup"><span data-stu-id="cb282-106">Tasks illustrated in this walkthrough include:</span></span>

- <span data-ttu-id="cb282-107">建立檔案系統 Web Form 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="cb282-107">Creating a file system Web Forms application project.</span></span>
- <span data-ttu-id="cb282-108">熟悉 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="cb282-108">Familiarizing yourself with Visual Studio.</span></span>
- <span data-ttu-id="cb282-109">建立 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="cb282-109">Creating an ASP.NET page.</span></span>
- <span data-ttu-id="cb282-110">加入控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-110">Adding controls.</span></span>
- <span data-ttu-id="cb282-111">加入事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="cb282-111">Adding event handlers.</span></span>
- <span data-ttu-id="cb282-112">執行和測試的頁面上，從 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="cb282-112">Running and testing a page from Visual Studio.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb282-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="cb282-113">Prerequisites</span></span>


<span data-ttu-id="cb282-114">若要完成這個逐步解說，您將需要：</span><span class="sxs-lookup"><span data-stu-id="cb282-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="cb282-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。</span><span class="sxs-lookup"><span data-stu-id="cb282-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="cb282-116">會自動安裝.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="cb282-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="cb282-117">Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常被稱為 Visual Studio 在此教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="cb282-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="cb282-118">如果您使用 Visual Studio，本逐步解說假設您已經選擇**Web 程式開發**設定的集合，您會啟動 Visual Studio 的第一次。</span><span class="sxs-lookup"><span data-stu-id="cb282-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="cb282-119">如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。</span><span class="sxs-lookup"><span data-stu-id="cb282-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="cb282-120">建立 Web 應用程式專案和頁面</span><span class="sxs-lookup"><span data-stu-id="cb282-120">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="cb282-121">在這部分的逐步解說中，您將建立 Web 應用程式專案，並加入新的頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-121">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span> <span data-ttu-id="cb282-122">您也會加入 HTML 文字，以及執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="cb282-122">You will also add HTML text and run the page in your browser.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="cb282-123">若要建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="cb282-123">To create a Web application project</span></span>

1. <span data-ttu-id="cb282-124">開啟 Microsoft Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="cb282-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="cb282-125">在 [檔案] 功能表上，選取 [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="cb282-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="cb282-126">![檔案功能表](creating-a-basic-web-forms-page/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cb282-126">![File Menu](creating-a-basic-web-forms-page/_static/image1.png)</span></span>

    <span data-ttu-id="cb282-127">[ **新增專案** ] 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="cb282-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="cb282-128">選取**範本** - &gt; **Visual C#**  - &gt; **Web**左側的 [範本] 群組。</span><span class="sxs-lookup"><span data-stu-id="cb282-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="cb282-129">選擇**ASP.NET Web 應用程式**中心資料行中的範本。</span><span class="sxs-lookup"><span data-stu-id="cb282-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="cb282-130">命名您的專案***BasicWebApp***按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cb282-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="cb282-131">![新增專案對話方塊](creating-a-basic-web-forms-page/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="cb282-131">![New Project dialog box](creating-a-basic-web-forms-page/_static/image2.png)</span></span>
6. <span data-ttu-id="cb282-132">接下來，選取**Web Form**範本，然後按一下**確定**按鈕以建立專案。</span><span class="sxs-lookup"><span data-stu-id="cb282-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="cb282-133">![新增 ASP.NET 專案對話方塊](creating-a-basic-web-forms-page/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="cb282-133">![New ASP.NET Project dialog box](creating-a-basic-web-forms-page/_static/image3.png)</span></span>  

    <span data-ttu-id="cb282-134">Visual Studio 建立新的專案，其中包含預先建置的 Web Form 範本為基礎的功能。</span><span class="sxs-lookup"><span data-stu-id="cb282-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span> <span data-ttu-id="cb282-135">它不只提供您*Home.aspx*  頁面上， *about.aspx 的網頁* 頁面上， *Contact.aspx*頁面上，但也包含註冊使用者，並將儲存的成員資格功能他們的認證，讓他們可以登入您的網站。</span><span class="sxs-lookup"><span data-stu-id="cb282-135">It not only provides you with a *Home.aspx* page, an *About.aspx* page, a *Contact.aspx* page, but also includes membership functionality that registers users and saves their credentials so that they can log in to your website.</span></span> <span data-ttu-id="cb282-136">建立新的頁面時，預設 Visual Studio 會顯示在頁面**來源** 檢視中，您可以在其中檢視網頁的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-136">When a new page is created, by default Visual Studio displays the page in **Source** view, where you can see the page's HTML elements.</span></span> <span data-ttu-id="cb282-137">下圖顯示您會看到在**來源**如果您建立名為新的網頁檢視*BasicWebApp.aspx*。</span><span class="sxs-lookup"><span data-stu-id="cb282-137">The following illustration shows what you would see in **Source** view if you created a new Web page named *BasicWebApp.aspx*.</span></span>  
    <span data-ttu-id="cb282-138">![原始碼檢視](creating-a-basic-web-forms-page/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="cb282-138">![Source View](creating-a-basic-web-forms-page/_static/image4.png)</span></span>


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a><span data-ttu-id="cb282-139">Visual Studio 網頁開發環境的教學課程</span><span class="sxs-lookup"><span data-stu-id="cb282-139">A Tour of the Visual Studio Web Development Environment</span></span>


<span data-ttu-id="cb282-140">您所修改頁面繼續進行之前，很有用，讓自己熟悉如何使用 Visual Studio 開發環境。</span><span class="sxs-lookup"><span data-stu-id="cb282-140">Before you proceed by modifying the page, it is useful to familiarize yourself with the Visual Studio development environment.</span></span> <span data-ttu-id="cb282-141">下圖顯示您在 windows 和 Visual Studio 與 Visual Studio Express for Web 中可用的工具。</span><span class="sxs-lookup"><span data-stu-id="cb282-141">The following illustration shows you the windows and tools that are available in Visual Studio and Visual Studio Express for Web.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="cb282-142">這個圖表可顯示的預設視窗和視窗的位置。</span><span class="sxs-lookup"><span data-stu-id="cb282-142">This diagram shows default windows and window locations.</span></span> <span data-ttu-id="cb282-143">**檢視**功能表可讓您顯示其他視窗，以及重新排列和視窗大小調整為符合您的偏好。</span><span class="sxs-lookup"><span data-stu-id="cb282-143">The **View** menu allows you to display additional windows, and to rearrange and resize windows to suit your preferences.</span></span> <span data-ttu-id="cb282-144">如果已經到視窗的排列方式進行變更，您所看到的內容不會符合圖。</span><span class="sxs-lookup"><span data-stu-id="cb282-144">If changes have already been made to the window arrangement, what you see will not match the illustration.</span></span>


 <span data-ttu-id="cb282-145">Visual Studio 環境</span><span class="sxs-lookup"><span data-stu-id="cb282-145">The Visual Studio environment</span></span>

![Visual Studio 環境](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a><span data-ttu-id="cb282-147">讓自己熟悉如何使用 Web 設計工具</span><span class="sxs-lookup"><span data-stu-id="cb282-147">Familiarize yourself with the Web designer</span></span>

<span data-ttu-id="cb282-148">檢查上圖中，並比對的文字至下列清單中，用來描述最常使用的視窗和工具。</span><span class="sxs-lookup"><span data-stu-id="cb282-148">Examine the above illustration and match the text to the following list, which describes the most commonly used windows and tools.</span></span> <span data-ttu-id="cb282-149">（並非所有的視窗和工具，您會看到此處所列，只有標記在上圖中）。</span><span class="sxs-lookup"><span data-stu-id="cb282-149">(Not all windows and tools that you see are listed here, only those marked in the preceding illustration.)</span></span>

- <span data-ttu-id="cb282-150">工具列。</span><span class="sxs-lookup"><span data-stu-id="cb282-150">Toolbars.</span></span> <span data-ttu-id="cb282-151">提供命令的格式化文字，尋找文字等等。</span><span class="sxs-lookup"><span data-stu-id="cb282-151">Provide commands for formatting text, finding text, and so on.</span></span> <span data-ttu-id="cb282-152">某些工具列是可用的只有當您使用**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-152">Some toolbars are available only when you are working in **Design** view.</span></span>
- <span data-ttu-id="cb282-153">**方案總管 中**視窗。</span><span class="sxs-lookup"><span data-stu-id="cb282-153">**Solution Explorer** window.</span></span> <span data-ttu-id="cb282-154">Web 應用程式中顯示的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb282-154">Displays the files and folders in your Web application.</span></span>
- <span data-ttu-id="cb282-155">文件視窗。</span><span class="sxs-lookup"><span data-stu-id="cb282-155">Document window.</span></span> <span data-ttu-id="cb282-156">顯示您在索引標籤式視窗中處理的文件。</span><span class="sxs-lookup"><span data-stu-id="cb282-156">Displays the documents you are working on in tabbed windows.</span></span> <span data-ttu-id="cb282-157">您可以透過按一下索引標籤的文件之間切換。</span><span class="sxs-lookup"><span data-stu-id="cb282-157">You can switch between documents by clicking tabs.</span></span>
- <span data-ttu-id="cb282-158">**屬性**視窗。</span><span class="sxs-lookup"><span data-stu-id="cb282-158">**Properties** window.</span></span> <span data-ttu-id="cb282-159">可讓您變更設定 頁面、 HTML 項目、 控制項和其他物件。</span><span class="sxs-lookup"><span data-stu-id="cb282-159">Allows you to change settings for the page, HTML elements, controls, and other objects.</span></span>
- <span data-ttu-id="cb282-160">檢視索引標籤。</span><span class="sxs-lookup"><span data-stu-id="cb282-160">View tabs.</span></span> <span data-ttu-id="cb282-161">為您提供相同的文件的不同檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-161">Present you with different views of the same document.</span></span> <span data-ttu-id="cb282-162">**設計**檢視是 WYSIWYG 編輯的介面。</span><span class="sxs-lookup"><span data-stu-id="cb282-162">**Design** view is a near-WYSIWYG editing surface.</span></span> <span data-ttu-id="cb282-163">**來源**檢視為頁面的 HTML 編輯器。</span><span class="sxs-lookup"><span data-stu-id="cb282-163">**Source** view is the HTML editor for the page.</span></span> <span data-ttu-id="cb282-164">**分割**檢視同時顯示**設計**檢視和**來源**文件的檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-164">**Split** view displays both the **Design** view and the **Source** view for the document.</span></span> <span data-ttu-id="cb282-165">您將使用**設計**和**來源**稍後在本逐步解說的檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-165">You will work with the **Design** and **Source** views later in this walkthrough.</span></span> <span data-ttu-id="cb282-166">如果您想要開啟中的網頁**設計**檢視，請在**工具**功能表上，按一下 **選項**，選取**HTML 設計工具**節點，然後變更**頁面起始位置**選項。</span><span class="sxs-lookup"><span data-stu-id="cb282-166">If you prefer to open Web pages in **Design** view, on the **Tools** menu, click **Options**, select the **HTML Designer** node, and change the **Start Pages In** option.</span></span>
- <span data-ttu-id="cb282-167">**工具箱**。</span><span class="sxs-lookup"><span data-stu-id="cb282-167">**ToolBox**.</span></span> <span data-ttu-id="cb282-168">提供控制項，您可以將它拖曳到頁面的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-168">Provides controls and HTML elements that you can drag onto your page.</span></span> <span data-ttu-id="cb282-169">**工具箱**項目會由一般函式分組。</span><span class="sxs-lookup"><span data-stu-id="cb282-169">**Toolbox** elements are grouped by common function.</span></span>
- <span data-ttu-id="cb282-170">S **erver 總管**。</span><span class="sxs-lookup"><span data-stu-id="cb282-170">S **erver Explorer**.</span></span> <span data-ttu-id="cb282-171">顯示資料庫連接。</span><span class="sxs-lookup"><span data-stu-id="cb282-171">Displays database connections.</span></span> <span data-ttu-id="cb282-172">如果看不到 伺服器總管，在 檢視 功能表中，按一下 伺服器總管。</span><span class="sxs-lookup"><span data-stu-id="cb282-172">If Server Explorer is not visible, on the View menu, click Server Explorer.</span></span>


### <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="cb282-173">建立新的 ASP.NET Web Form 網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-173">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="cb282-174">當您建立新的 Web Form 應用程式使用**ASP.NET Web 應用程式**專案範本，Visual Studio 加入 ASP.NET 網頁 （Web Form 頁面） 名為*Default.aspx*，以及以多個其他檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb282-174">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="cb282-175">您可以使用*Default.aspx*與 Web 應用程式的 [首頁] 頁面的頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-175">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="cb282-176">不過，對於此逐步解說中，您會建立和使用新的頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-176">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="cb282-177">若要將頁面加入至 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="cb282-177">To add a page to the Web application</span></span>


1. <span data-ttu-id="cb282-178">關閉*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-178">Close the *Default.aspx* page.</span></span> <span data-ttu-id="cb282-179">若要這樣做，請按一下顯示的檔案名稱的索引標籤，然後按一下 [關閉] 選項。</span><span class="sxs-lookup"><span data-stu-id="cb282-179">To do this, click the tab that displays the file name and then click the close option.</span></span>
2. <span data-ttu-id="cb282-180">在**方案總管 中**，以滑鼠右鍵按一下 Web 應用程式名稱 (在此教學課程中的應用程式名稱是**BasicWebSite**)，然後按一下**新增** - &gt;**新項目**。</span><span class="sxs-lookup"><span data-stu-id="cb282-180">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="cb282-181">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="cb282-181">The **Add New Item** dialog box is displayed.</span></span>
3. <span data-ttu-id="cb282-182">選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。</span><span class="sxs-lookup"><span data-stu-id="cb282-182">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="cb282-183">然後，選取**Web Form**中間清單並將其命名*FirstWebPage.aspx*。</span><span class="sxs-lookup"><span data-stu-id="cb282-183">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="cb282-184">![加入新項目對話方塊](creating-a-basic-web-forms-page/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="cb282-184">![Add New Item dialog box](creating-a-basic-web-forms-page/_static/image6.png)</span></span>
4. <span data-ttu-id="cb282-185">按一下**新增**將網頁加入您的專案。</span><span class="sxs-lookup"><span data-stu-id="cb282-185">Click **Add** to add the web page to your project.</span></span>  
<span data-ttu-id="cb282-186">Visual Studio 會建立新的頁面，並開啟它。</span><span class="sxs-lookup"><span data-stu-id="cb282-186">Visual Studio creates the new page and opens it.</span></span>


### <a name="adding-html-to-the-page"></a><span data-ttu-id="cb282-187">將 HTML 加入至網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-187">Adding HTML to the Page</span></span>


<span data-ttu-id="cb282-188">在這個部分的逐步解說中，您將一些靜態文字頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-188">In this part of the walkthrough, you will add some static text to the page.</span></span>

### <a name="to-add-text-to-the-page"></a><span data-ttu-id="cb282-189">將文字加入網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-189">To add text to the page</span></span>


1. <span data-ttu-id="cb282-190">在文件視窗的底部，按一下 **設計**索引標籤，以切換至**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-190">At the bottom of the document window, click the **Design** tab to switch to **Design** view.</span></span>

    <span data-ttu-id="cb282-191">設計檢視會顯示目前的頁面，以類似於 WYSIWYG 的方式。</span><span class="sxs-lookup"><span data-stu-id="cb282-191">Design view displays the current page in a WYSIWYG-like way.</span></span> <span data-ttu-id="cb282-192">此時，您沒有任何文字或控制項在頁面上，因此網頁為空白，但是概述矩形的虛線。</span><span class="sxs-lookup"><span data-stu-id="cb282-192">At this point, you do not have any text or controls on the page, so the page is blank except for a dashed line that outlines a rectangle.</span></span> <span data-ttu-id="cb282-193">代表這個矩形**div**頁面上的元素。</span><span class="sxs-lookup"><span data-stu-id="cb282-193">This rectangle represents a **div** element on the page.</span></span>
2. <span data-ttu-id="cb282-194">按一下以虛線框起來的矩形內部。</span><span class="sxs-lookup"><span data-stu-id="cb282-194">Click inside the rectangle that is outlined by a dashed line.</span></span>
3. <span data-ttu-id="cb282-195">型別**歡迎使用 Visual Web Developer**按**ENTER**兩次。</span><span class="sxs-lookup"><span data-stu-id="cb282-195">Type **Welcome to Visual Web Developer** and press **ENTER** twice.</span></span>

    <span data-ttu-id="cb282-196">下圖顯示在您輸入的文字**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-196">The following illustration shows the text you typed in **Design** view.</span></span>

    <span data-ttu-id="cb282-197">![在 [設計] 檢視中的歡迎文字](creating-a-basic-web-forms-page/_static/image7.png "設計 檢視中的歡迎文字")</span><span class="sxs-lookup"><span data-stu-id="cb282-197">![Welcome text in Design view](creating-a-basic-web-forms-page/_static/image7.png "Welcome text in Design view")</span></span>
4. <span data-ttu-id="cb282-198">切換至**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-198">Switch to **Source** view.</span></span>

    <span data-ttu-id="cb282-199">您可以查看在 HTML**來源**您在輸入時所建立的檢視**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-199">You can see the HTML in **Source** view that you created when you typed in **Design** view.</span></span>  
    <span data-ttu-id="cb282-200">![包含靜態文字的網頁](creating-a-basic-web-forms-page/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="cb282-200">![Web Page with Static Text](creating-a-basic-web-forms-page/_static/image8.png)</span></span>


### <a name="running-the-page"></a><span data-ttu-id="cb282-201">執行網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-201">Running the Page</span></span>


<span data-ttu-id="cb282-202">將控制項加入至頁面在繼續之前，您可以先執行。</span><span class="sxs-lookup"><span data-stu-id="cb282-202">Before you proceed by adding controls to the page, you can first run it.</span></span>

### <a name="to-run-the-page"></a><span data-ttu-id="cb282-203">執行網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-203">To run the page</span></span>


1. <span data-ttu-id="cb282-204">在**方案總管 中**，以滑鼠右鍵按一下*FirstWebPage.aspx*選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="cb282-204">In **Solution Explorer**, right-click *FirstWebPage.aspx* and select **Set as Start Page**.</span></span>
2. <span data-ttu-id="cb282-205">按**CTRL + F5**執行網頁。</span><span class="sxs-lookup"><span data-stu-id="cb282-205">Press **CTRL+F5** to run the page.</span></span>

    <span data-ttu-id="cb282-206">頁面會顯示在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cb282-206">The page is displayed in the browser.</span></span> <span data-ttu-id="cb282-207">雖然您建立的網頁具有的檔案名稱副檔名 *.aspx*，目前執行的任何 HTML 頁面類似。</span><span class="sxs-lookup"><span data-stu-id="cb282-207">Although the page you created has a file-name extension of *.aspx*, it currently runs like any HTML page.</span></span>

    <span data-ttu-id="cb282-208">在瀏覽器中顯示的頁面您可以也以滑鼠右鍵按一下中的頁面**方案總管 中**選取**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="cb282-208">To display a page in the browser you can also right-click the page in **Solution Explorer** and select **View in Browser**.</span></span>
3. <span data-ttu-id="cb282-209">關閉瀏覽器停止 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb282-209">Close the browser to stop the Web application.</span></span>


## <a name="adding-and-programming-controls"></a><span data-ttu-id="cb282-210">加入和程式設計控制項</span><span class="sxs-lookup"><span data-stu-id="cb282-210">Adding and Programming Controls</span></span>


<a id="sectionToggle1"></a>

<span data-ttu-id="cb282-211">您現在要加入伺服器控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-211">You will now add server controls to the page.</span></span> <span data-ttu-id="cb282-212">伺服器控制項，例如按鈕、 標籤、 文字方塊和其他熟悉的控制項，提供一般的形式處理功能，為您的 Web Form 網頁。</span><span class="sxs-lookup"><span data-stu-id="cb282-212">Server controls, such as buttons, labels, text boxes, and other familiar controls, provide typical form-processing capabilities for your Web Forms pages.</span></span> <span data-ttu-id="cb282-213">不過，您也可以程式執行的伺服器，而非用戶端程式碼的控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-213">However, you can program the controls with code that runs on the server, rather than the client.</span></span>

<span data-ttu-id="cb282-214">您將加入[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項，[文字方塊](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項，與[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項加入網頁，並撰寫程式碼來處理[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-214">You will add a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control, a [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control, and a [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control to the page and write code to handle the [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event for the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.</span></span>

### <a name="to-add-controls-to-the-page"></a><span data-ttu-id="cb282-215">將控制項加入頁面</span><span class="sxs-lookup"><span data-stu-id="cb282-215">To add controls to the page</span></span>


1. <span data-ttu-id="cb282-216">按一下**設計**索引標籤，以切換至**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-216">Click the **Design** tab to switch to **Design** view.</span></span>
2. <span data-ttu-id="cb282-217">將插入點放在結尾**歡迎使用 Visual Web Developer**文字，然後按**ENTER**五個以上的時間，進行一些空間在**div**項目方塊。</span><span class="sxs-lookup"><span data-stu-id="cb282-217">Put the insertion point at the end of the **Welcome to Visual Web Developer** text and press **ENTER** five or more times to make some room in the **div** element box.</span></span>
3. <span data-ttu-id="cb282-218">在**工具箱**，依序展開**標準**如果尚未展開的群組。</span><span class="sxs-lookup"><span data-stu-id="cb282-218">In the **Toolbox**, expand the **Standard** group if it is not already expanded.</span></span>  
<span data-ttu-id="cb282-219">請注意，您可能需要展開**工具箱**視窗以檢視它。</span><span class="sxs-lookup"><span data-stu-id="cb282-219">Note that you may need to expand the **Toolbox** window on the left to view it.</span></span>
4. <span data-ttu-id="cb282-220">拖曳[文字方塊](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項拖曳至頁面，並將它中放**div**具有項目方塊**歡迎使用 Visual Web Developer**的第一行。</span><span class="sxs-lookup"><span data-stu-id="cb282-220">Drag a [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control onto the page and drop it in the middle of the **div** element box that has **Welcome to Visual Web Developer** in the first line.</span></span>
5. <span data-ttu-id="cb282-221">拖曳[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項拖曳至頁面，並置放到右邊的[文字方塊](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-221">Drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page and drop it to the right of the [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control.</span></span>
6. <span data-ttu-id="cb282-222">拖曳[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項拖曳至頁面，並將它放在不同行下方[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-222">Drag a [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control onto the page and drop it on a separate line below the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.</span></span>
7. <span data-ttu-id="cb282-223">將放在上面的插入點[文字方塊](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項，然後再輸入**輸入您的名稱：** 。</span><span class="sxs-lookup"><span data-stu-id="cb282-223">Put the insertion point above the [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control, and then type **Enter your name:** .</span></span>

    <span data-ttu-id="cb282-224">此靜態的 HTML 文字是標題[文字方塊](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-224">This static HTML text is the caption for the [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control.</span></span> <span data-ttu-id="cb282-225">您可以混合靜態 HTML 和相同的頁面上的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-225">You can mix static HTML and server controls on the same page.</span></span> <span data-ttu-id="cb282-226">下圖顯示三個控制項如何出現在**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-226">The following illustration shows how the three controls appear in **Design** view.</span></span>

    <span data-ttu-id="cb282-227">![在 [設計] 檢視中的三個控制項](creating-a-basic-web-forms-page/_static/image9.png "設計 檢視中的三個控制項")</span><span class="sxs-lookup"><span data-stu-id="cb282-227">![Three controls in Design view](creating-a-basic-web-forms-page/_static/image9.png "Three controls in Design view")</span></span>


### <a name="setting-control-properties"></a><span data-ttu-id="cb282-228">設定控制項屬性</span><span class="sxs-lookup"><span data-stu-id="cb282-228">Setting Control Properties</span></span>


<span data-ttu-id="cb282-229">Visual Studio 提供各種方式來設定頁面上的控制項屬性。</span><span class="sxs-lookup"><span data-stu-id="cb282-229">Visual Studio offers you various ways to set the properties of controls on the page.</span></span> <span data-ttu-id="cb282-230">在這部分的逐步解說中，您將設定屬性，在這兩**設計**檢視和**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-230">In this part of the walkthrough, you will set properties in both **Design** view and **Source** view.</span></span>

### <a name="to-set-control-properties"></a><span data-ttu-id="cb282-231">若要設定控制項屬性</span><span class="sxs-lookup"><span data-stu-id="cb282-231">To set control properties</span></span>


1. <span data-ttu-id="cb282-232">第一次，顯示**屬性**從選取的 windows**檢視**功能表-&gt; **其他視窗** - &gt; **屬性視窗**。</span><span class="sxs-lookup"><span data-stu-id="cb282-232">First, display the **Properties** windows by selecting from the **View** menu -&gt; **Other Windows** -&gt; **Properies Window**.</span></span> <span data-ttu-id="cb282-233">您也可以選取**F4**顯示**屬性**視窗。</span><span class="sxs-lookup"><span data-stu-id="cb282-233">You could alternatively select **F4** to display the **Properties** window.</span></span>
2. <span data-ttu-id="cb282-234">選取[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項，然後在**屬性**視窗中，設定的值**文字**至**顯示名稱**。</span><span class="sxs-lookup"><span data-stu-id="cb282-234">Select the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control, and then in the **Properties** window, set the value of **Text** to **Display Name**.</span></span> <span data-ttu-id="cb282-235">下圖所示，您所輸入的文字會出現在設計工具中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="cb282-235">The text you entered appears on the button in the designer, as shown in the following illustration.</span></span>

    <span data-ttu-id="cb282-236">![設定按鈕文字](creating-a-basic-web-forms-page/_static/image10.png "設定按鈕文字")</span><span class="sxs-lookup"><span data-stu-id="cb282-236">![Set Button text](creating-a-basic-web-forms-page/_static/image10.png "Set Button text")</span></span>
3. <span data-ttu-id="cb282-237">切換至**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-237">Switch to **Source** view.</span></span>

    <span data-ttu-id="cb282-238">**來源**檢視會顯示在 HTML 頁面上，包括 Visual Studio 已經建立的伺服器控制項的項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-238">**Source** view displays the HTML for the page, including the elements that Visual Studio has created for the server controls.</span></span> <span data-ttu-id="cb282-239">控制項使用宣告 HTML 相似的語法，不同之處在於標記使用前置詞**asp:** 和包含屬性**runat =&quot;伺服器&quot;**。</span><span class="sxs-lookup"><span data-stu-id="cb282-239">Controls are declared using HTML-like syntax, except that the tags use the prefix **asp:** and include the attribute **runat=&quot;server&quot;**.</span></span>

    <span data-ttu-id="cb282-240">控制項屬性會宣告為屬性。</span><span class="sxs-lookup"><span data-stu-id="cb282-240">Control properties are declared as attributes.</span></span> <span data-ttu-id="cb282-241">例如，當您設定[文字](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx)屬性[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項，在步驟 1 中，您已實際設定**文字**中控制項的標記屬性。</span><span class="sxs-lookup"><span data-stu-id="cb282-241">For example, when you set the [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) property for the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control, in step 1, you were actually setting the **Text** attribute in the control's markup.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="cb282-242">所有控制項都位於**表單**項目也具有屬性**runat =&quot;伺服器&quot;**。</span><span class="sxs-lookup"><span data-stu-id="cb282-242">All the controls are inside a **form** element, which also has the attribute **runat=&quot;server&quot;**.</span></span> <span data-ttu-id="cb282-243">**Runat =&quot;伺服器&quot;** 屬性和**asp:** 的控制標記以便處理 ASP.NET 伺服器上網頁執行時，標記控制項的前置詞。</span><span class="sxs-lookup"><span data-stu-id="cb282-243">The **runat=&quot;server&quot;** attribute and the **asp:** prefix for control tags mark the controls so that they are processed by ASP.NET on the server when the page runs.</span></span> <span data-ttu-id="cb282-244">外部程式碼**&lt;形成 runat =&quot;伺服器&quot;&gt;** 和**&lt;指令碼 runat =&quot;伺服器&quot;&gt;** 項目會傳送至瀏覽器，這就是為什麼 ASP.NET 程式碼必須在其開頭的標記包含的項目內不變**runat =&quot;伺服器&quot;** 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb282-244">Code outside of **&lt;form runat=&quot;server&quot;&gt;** and **&lt;script runat=&quot;server&quot;&gt;** elements is sent unchanged to the browser, which is why the ASP.NET code must be inside an element whose opening tag contains the **runat=&quot;server&quot;** attribute.</span></span>
4. <span data-ttu-id="cb282-245">接下來，您將在其中加入一個額外的屬性，以[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-245">Next, you will add an additional property to the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)  control.</span></span> <span data-ttu-id="cb282-246">Put 之後直接在插入點**asp： 標籤**中 **&lt;asp： 標籤&gt;** 標記，並再按**空格鍵**。</span><span class="sxs-lookup"><span data-stu-id="cb282-246">Put the insertion point directly after **asp:Label** in the **&lt;asp:Label&gt;** tag, and then press **SPACEBAR**.</span></span>

    <span data-ttu-id="cb282-247">下拉式清單隨即出現，顯示您可以設定的可用屬性的清單[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-247">A drop-down list appears that displays the list of available properties you can set for a [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span> <span data-ttu-id="cb282-248">這項功能，稱為**IntelliSense**，可協助您在**來源**語法的伺服器控制項、 HTML 項目，以及其他項目 頁面上的檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-248">This feature, referred to as **IntelliSense**, helps you in **Source** view with the syntax of server controls, HTML elements, and other items on the page.</span></span> <span data-ttu-id="cb282-249">下圖顯示**IntelliSense**下拉式選單，如[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-249">The following illustration shows the **IntelliSense** drop-down list for the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span>

    <span data-ttu-id="cb282-250">![IntelliSense 屬性](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense 屬性")</span><span class="sxs-lookup"><span data-stu-id="cb282-250">![IntelliSense attributes](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense attributes")</span></span>
5. <span data-ttu-id="cb282-251">選取**ForeColor** ，然後輸入等號。</span><span class="sxs-lookup"><span data-stu-id="cb282-251">Select **ForeColor** and then type an equal sign.</span></span>

    <span data-ttu-id="cb282-252">IntelliSense 會顯示色彩的清單。</span><span class="sxs-lookup"><span data-stu-id="cb282-252">IntelliSense displays a list of colors.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="cb282-253">您可以顯示**IntelliSense**隨時按下下拉式清單**CTRL + J**檢視程式碼時。</span><span class="sxs-lookup"><span data-stu-id="cb282-253">You can display an **IntelliSense** drop-down list at any time by pressing **CTRL+J** when viewing code.</span></span>
6. <span data-ttu-id="cb282-254">選取的色彩**[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** 控制項的文字。</span><span class="sxs-lookup"><span data-stu-id="cb282-254">Select a color for the **[Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** control's text.</span></span> <span data-ttu-id="cb282-255">請確定您選取濃，讀取在白色背景的色彩。</span><span class="sxs-lookup"><span data-stu-id="cb282-255">Make sure you select a color that is dark enough to read against a white background.</span></span>

    <span data-ttu-id="cb282-256">**ForeColor**屬性已完成，但您已選取，包括引號的色彩。</span><span class="sxs-lookup"><span data-stu-id="cb282-256">The **ForeColor** attribute is completed with the color that you have selected, including the closing quotation mark.</span></span>


### <a name="programming-the-button-control"></a><span data-ttu-id="cb282-257">程式設計按鈕控制項</span><span class="sxs-lookup"><span data-stu-id="cb282-257">Programming the Button Control</span></span>


<span data-ttu-id="cb282-258">對於此逐步解說中，您將撰寫程式碼，可讀取的名稱，使用者在文字方塊中輸入，並接著會顯示在 名稱[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-258">For this walkthrough, you will write code that reads the name that the user enters into the text box and then displays the name in the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span>

### <a name="add-a-default-button-event-handler"></a><span data-ttu-id="cb282-259">新增預設按鈕事件處理常式</span><span class="sxs-lookup"><span data-stu-id="cb282-259">Add a default button event handler</span></span>


1. <span data-ttu-id="cb282-260">切換至**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-260">Switch to **Design** view.</span></span>
2. <span data-ttu-id="cb282-261">按兩下[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-261">Double-click the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.</span></span>

    <span data-ttu-id="cb282-262">根據預設，Visual Studio 會切換成程式碼後置檔案並建立的基本架構的事件處理常式[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項的預設事件，[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件。</span><span class="sxs-lookup"><span data-stu-id="cb282-262">By default, Visual Studio switches to a code-behind file and creates a skeleton event handler for the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control's default event, the [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event.</span></span> <span data-ttu-id="cb282-263">程式碼後置檔案 （例如 C# 中) 的伺服器程式碼可將您 UI 的標記 （例如 HTML)。</span><span class="sxs-lookup"><span data-stu-id="cb282-263">The code-behind file separates your UI markup (such as HTML) from your server code (such as C#).</span></span>   
   <span data-ttu-id="cb282-264">游標會加入此事件處理常式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cb282-264">The cursor is positioned to added code for this event handler.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="cb282-265">按兩下控制項，以在**設計**檢視只是其中幾種方法可以建立事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="cb282-265">Double-clicking a control in **Design** view is just one of several ways you can create event handlers.</span></span>
3. <span data-ttu-id="cb282-266">內部**Button1\_按一下**事件處理常式中，輸入**Label1**後面接句號 (**。**)。</span><span class="sxs-lookup"><span data-stu-id="cb282-266">Inside the **Button1\_Click** event handler, type **Label1** followed by a period (**.**).</span></span>

    <span data-ttu-id="cb282-267">當您輸入超過此時限後的**識別碼**的標籤 (**Label1**)，Visual Studio 會顯示一份可用的成員[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制，如下列所示圖例。</span><span class="sxs-lookup"><span data-stu-id="cb282-267">When you type the period after the **ID** of the label (**Label1**), Visual Studio displays a list of available members for the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control, as shown in the following illustration.</span></span> <span data-ttu-id="cb282-268">成員通常是屬性、 方法或事件。</span><span class="sxs-lookup"><span data-stu-id="cb282-268">A member commonly a property, method, or event.</span></span>

    <span data-ttu-id="cb282-269">![程式碼檢視中的 IntelliSense](creating-a-basic-web-forms-page/_static/image12.png "程式碼檢視中的 IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="cb282-269">![IntelliSense in Code view](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense in Code view")</span></span>
4. <span data-ttu-id="cb282-270">完成**按一下**按鈕，如下列程式碼範例所示，它會讀取事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="cb282-270">Finish the **Click** event handler for the button so that it reads as shown in the following code example.</span></span>

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. <span data-ttu-id="cb282-271">切換回 [檢視**來源**檢視您的 HTML 標記，以滑鼠右鍵按一下*FirstWebPage.aspx*中**方案總管] 中**，然後選取**檢視標記**。</span><span class="sxs-lookup"><span data-stu-id="cb282-271">Switch back to viewing the **Source** view of your HTML markup by right-clicking *FirstWebPage.aspx* in the **Solution Explorer** and selecting **View Markup**.</span></span>
6. <span data-ttu-id="cb282-272">捲動到 **&lt;asp： 按鈕&gt;** 項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-272">Scroll to the **&lt;asp:Button&gt;** element.</span></span> <span data-ttu-id="cb282-273">請注意，  **&lt;asp： 按鈕&gt;** 項目現在具有屬性**onclick =&quot;Button1\_按一下&quot;**。</span><span class="sxs-lookup"><span data-stu-id="cb282-273">Note that the **&lt;asp:Button&gt;** element now has the attribute **onclick=&quot;Button1\_Click&quot;**.</span></span>

    <span data-ttu-id="cb282-274">這個屬性會繫結的按鈕[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)您在上一個步驟中已編碼的處理常式方法的事件。</span><span class="sxs-lookup"><span data-stu-id="cb282-274">This attribute binds the button's [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event to the handler method you coded in the previous step.</span></span>

    <span data-ttu-id="cb282-275">事件處理常式方法可以有任何名稱。您所看到的名稱是由 Visual Studio 建立的預設名稱。</span><span class="sxs-lookup"><span data-stu-id="cb282-275">Event handler methods can have any name; the name you see is the default name created by Visual Studio.</span></span> <span data-ttu-id="cb282-276">很重要的一點是的名稱用於**OnClick** HTML 中的屬性必須符合的程式碼後置中定義的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="cb282-276">The important point is that the name used for the **OnClick** attribute in the HTML must match the name of a method defined in the code-behind.</span></span>


### <a name="running-the-page"></a><span data-ttu-id="cb282-277">執行網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-277">Running the Page</span></span>


<span data-ttu-id="cb282-278">您現在可以測試伺服器上的控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-278">You can now test the server controls on the page.</span></span>

### <a name="to-run-the-page"></a><span data-ttu-id="cb282-279">執行網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-279">To run the page</span></span>


1. <span data-ttu-id="cb282-280">按**CTRL + F5**執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="cb282-280">Press **CTRL+F5** to run the page in the browser.</span></span> <span data-ttu-id="cb282-281">如果發生錯誤，重新檢查前面的步驟。</span><span class="sxs-lookup"><span data-stu-id="cb282-281">If an error occurs, recheck the steps above.</span></span>
2. <span data-ttu-id="cb282-282">在文字方塊中輸入名稱，然後按一下**顯示名稱** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cb282-282">Enter a name into the text box and click the **Display Name** button.</span></span>

    <span data-ttu-id="cb282-283">您輸入的名稱會顯示在[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-283">The name you entered is displayed in the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span> <span data-ttu-id="cb282-284">請注意，當您按一下按鈕時，網頁回傳至 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="cb282-284">Note that when you click the button, the page is posted to the Web server.</span></span> <span data-ttu-id="cb282-285">ASP.NET 然後重新建立頁面時，便會執行您的程式碼 (在此情況下，[按鈕](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)控制項的[按一下](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)事件處理常式的執行)，然後傳送至瀏覽器的 新頁面。</span><span class="sxs-lookup"><span data-stu-id="cb282-285">ASP.NET then recreates the page, runs your code (in this case, the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control's [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event handler runs), and then sends the new page to the browser.</span></span> <span data-ttu-id="cb282-286">如果您觀察瀏覽器中的 [狀態] 列，您可以看到的頁面正在進行往返到 Web 伺服器每次您按一下按鈕。</span><span class="sxs-lookup"><span data-stu-id="cb282-286">If you watch the status bar in the browser, you can see that the page is making a round trip to the Web server each time you click the button.</span></span>
3. <span data-ttu-id="cb282-287">在瀏覽器中檢視您正在執行的頁面上按一下滑鼠右鍵，然後選取網頁的原始檔**檢視原始檔**。</span><span class="sxs-lookup"><span data-stu-id="cb282-287">In the browser, view the source of the page you are running by right-clicking on the page and selecting **View source**.</span></span>

    <span data-ttu-id="cb282-288">頁面在原始程式碼中，您會看到 HTML 沒有任何伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="cb282-288">In the page source code, you see HTML without any server code.</span></span> <span data-ttu-id="cb282-289">具體來說，看不到**&lt;asp:&gt;** 中所使用的項目**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-289">Specifically, you do not see the **&lt;asp:&gt;** elements that you were working with in **Source** view.</span></span> <span data-ttu-id="cb282-290">頁面執行時，ASP.NET 處理伺服器控制項，並會呈現 HTML 項目頁面執行代表控制項的功能。</span><span class="sxs-lookup"><span data-stu-id="cb282-290">When the page runs, ASP.NET processes the server controls and renders HTML elements to the page that perform the functions that represent the control.</span></span> <span data-ttu-id="cb282-291">例如，  **&lt;asp： 按鈕&gt;** 控制項會轉譯為 HTML **&lt;輸入類型 =&quot;提交&quot;&gt;** 項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-291">For example, the **&lt;asp:Button&gt;** control is rendered as the HTML **&lt;input type=&quot;submit&quot;&gt;** element.</span></span>
4. <span data-ttu-id="cb282-292">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cb282-292">Close the browser.</span></span>


## <a name="working-with-additional-controls"></a><span data-ttu-id="cb282-293">使用其他控制項</span><span class="sxs-lookup"><span data-stu-id="cb282-293">Working with Additional Controls</span></span>

<a id="sectionToggle2"></a>

<span data-ttu-id="cb282-294">在這個部分的逐步解說中，您將使用[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項，一次顯示一個月的日期。</span><span class="sxs-lookup"><span data-stu-id="cb282-294">In this part of the walkthrough, you will work with the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control, which displays dates a month at a time.</span></span> <span data-ttu-id="cb282-295">[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項是比您已使用的按鈕、 文字方塊和標籤的複雜控制項，並說明伺服器控制項的一些其他功能。</span><span class="sxs-lookup"><span data-stu-id="cb282-295">The [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control is a more complex control than the button, text box, and label you have been working with and illustrates some further capabilities of server controls.</span></span>

<span data-ttu-id="cb282-296">在本節中，您將加入[System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項加入網頁並將其格式化。</span><span class="sxs-lookup"><span data-stu-id="cb282-296">In this section, you will add a [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control to the page and format it.</span></span>

### <a name="to-add-a-calendar-control"></a><span data-ttu-id="cb282-297">若要新增日曆控制項</span><span class="sxs-lookup"><span data-stu-id="cb282-297">To add a Calendar control</span></span>


1. <span data-ttu-id="cb282-298">在 Visual Studio 中，切換至**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-298">In Visual Studio, switch to **Design** view.</span></span>
2. <span data-ttu-id="cb282-299">從**標準**區段**工具箱**，拖曳[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項拖曳至頁面，並將它放在下面的**div**項目，包含其他控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-299">From the **Standard** section of the **Toolbox**, drag a [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control onto the page and drop it below the **div** element that contains the other controls.</span></span>

    <span data-ttu-id="cb282-300">行事曆的智慧標籤面板隨即出現。</span><span class="sxs-lookup"><span data-stu-id="cb282-300">The calendar's smart tag panel is displayed.</span></span> <span data-ttu-id="cb282-301">此面板會顯示命令可讓您輕鬆地執行所選控制項的最常見的工作。</span><span class="sxs-lookup"><span data-stu-id="cb282-301">The panel displays commands that make it easy for you to perform the most common tasks for the selected control.</span></span> <span data-ttu-id="cb282-302">下圖顯示[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制中轉譯**設計**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-302">The following illustration shows the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control as rendered in **Design** view.</span></span>

    <span data-ttu-id="cb282-303">![月曆控制項在設計檢視中的](creating-a-basic-web-forms-page/_static/image13.png "月曆控制項在設計檢視")</span><span class="sxs-lookup"><span data-stu-id="cb282-303">![Calendar control in Design view](creating-a-basic-web-forms-page/_static/image13.png "Calendar control in Design view")</span></span>
3. <span data-ttu-id="cb282-304">在智慧標籤面板中，選擇 **自動格式化**。</span><span class="sxs-lookup"><span data-stu-id="cb282-304">In the smart tag panel, choose **Auto Format**.</span></span>

    <span data-ttu-id="cb282-305">**自動格式化**對話方塊隨即出現，可讓您選取行事曆的格式化配置。</span><span class="sxs-lookup"><span data-stu-id="cb282-305">The **Auto Format** dialog box is displayed, which allows you to select a formatting scheme for the calendar.</span></span> <span data-ttu-id="cb282-306">下圖顯示**自動格式化**對話方塊的 [行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-306">The following illustration shows the **Auto Format** dialog box for the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control.</span></span>

    <span data-ttu-id="cb282-307">![自動格式化對話方塊 （Calendar 控制項）](creating-a-basic-web-forms-page/_static/image14.png "自動格式化對話方塊 （Calendar 控制項）")</span><span class="sxs-lookup"><span data-stu-id="cb282-307">![Auto Format dialog box (Calendar control)](creating-a-basic-web-forms-page/_static/image14.png "Auto Format dialog box (Calendar control)")</span></span>
4. <span data-ttu-id="cb282-308">從**選取配置**清單中，選取**簡單**，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="cb282-308">From the **Select a scheme** list, select **Simple** and then click **OK**.</span></span>
5. <span data-ttu-id="cb282-309">切換至**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-309">Switch to **Source** view.</span></span>

    <span data-ttu-id="cb282-310">您可以看到 **&lt;asp： 行事曆&gt;** 項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-310">You can see the **&lt;asp:Calendar&gt;** element.</span></span> <span data-ttu-id="cb282-311">此元素會遠超過您稍早建立的簡單控制項的項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-311">This element is much longer than the elements for the simple controls you created earlier.</span></span> <span data-ttu-id="cb282-312">它也包含子元素，例如 **&lt;WeekEndDayStyle&gt;**，代表不同的格式設定。</span><span class="sxs-lookup"><span data-stu-id="cb282-312">It also includes subelements, such as **&lt;WeekEndDayStyle&gt;**, which represent various formatting settings.</span></span> <span data-ttu-id="cb282-313">下圖顯示[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="cb282-313">The following illustration shows the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control in **Source** view.</span></span> <span data-ttu-id="cb282-314">(您在中看到的確切標記**來源**檢視可能會稍有不同的圖例。)</span><span class="sxs-lookup"><span data-stu-id="cb282-314">(The exact markup that you see in **Source** view might differ slightly from the illustration.)</span></span>

    <span data-ttu-id="cb282-315">![月曆控制項在原始碼檢視](creating-a-basic-web-forms-page/_static/image15.png "月曆控制項在原始碼檢視")</span><span class="sxs-lookup"><span data-stu-id="cb282-315">![Calendar control in Source view](creating-a-basic-web-forms-page/_static/image15.png "Calendar control in Source view")</span></span>


### <a name="programming-the-calendar-control"></a><span data-ttu-id="cb282-316">程式設計月曆控制項</span><span class="sxs-lookup"><span data-stu-id="cb282-316">Programming the Calendar Control</span></span>


<span data-ttu-id="cb282-317">在本節中，您將程式[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項來顯示目前選取的日期。</span><span class="sxs-lookup"><span data-stu-id="cb282-317">In this section, you will program the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control to display the currently selected date.</span></span>

### <a name="to-program-the-calendar-control"></a><span data-ttu-id="cb282-318">若要撰寫月曆控制項</span><span class="sxs-lookup"><span data-stu-id="cb282-318">To program the Calendar control</span></span>


1. <span data-ttu-id="cb282-319">在**設計**檢視中，按兩下[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-319">In **Design** view, double-click the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control.</span></span>

    <span data-ttu-id="cb282-320">新的事件處理常式會建立並顯示在名為的程式碼後置檔案*FirstWebPage.aspx.cs*。</span><span class="sxs-lookup"><span data-stu-id="cb282-320">A new event handler is created and displayed in the code-behind file named *FirstWebPage.aspx.cs*.</span></span>
2. <span data-ttu-id="cb282-321">完成[SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx)為下列程式碼的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="cb282-321">Finish the [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) event handler with the following code.</span></span>

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    <span data-ttu-id="cb282-322">上述程式碼會設定標籤控制項的文字選取日期的日曆控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-322">The above code sets the text of the label control to the selected date of the calendar control.</span></span>


### <a name="running-the-page"></a><span data-ttu-id="cb282-323">執行網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-323">Running the Page</span></span>


<span data-ttu-id="cb282-324">您現在可以測試行事曆。</span><span class="sxs-lookup"><span data-stu-id="cb282-324">You can now test the calendar.</span></span>

### <a name="to-run-the-page"></a><span data-ttu-id="cb282-325">執行網頁</span><span class="sxs-lookup"><span data-stu-id="cb282-325">To run the page</span></span>


1. <span data-ttu-id="cb282-326">按**CTRL + F5**執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="cb282-326">Press **CTRL+F5** to run the page in the browser.</span></span>
2. <span data-ttu-id="cb282-327">按一下行事曆中的日期。</span><span class="sxs-lookup"><span data-stu-id="cb282-327">Click a date in the calendar.</span></span>

    <span data-ttu-id="cb282-328">您已按下的日期會顯示在[標籤](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)控制項。</span><span class="sxs-lookup"><span data-stu-id="cb282-328">The date you clicked is displayed in the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span>
3. <span data-ttu-id="cb282-329">在瀏覽器中檢視網頁原始碼。</span><span class="sxs-lookup"><span data-stu-id="cb282-329">In the browser, view the source code for the page.</span></span>

    <span data-ttu-id="cb282-330">請注意，[行事曆](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)控制項的呈現與頁面**資料表**，與為每一天**td**項目。</span><span class="sxs-lookup"><span data-stu-id="cb282-330">Note that the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control has been rendered to the page as a **table**, with each day as a **td** element.</span></span>
4. <span data-ttu-id="cb282-331">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cb282-331">Close the browser.</span></span>


## <a name="next-steps"></a><span data-ttu-id="cb282-332">後續步驟</span><span class="sxs-lookup"><span data-stu-id="cb282-332">Next Steps</span></span>


<a id="nextStepsToggle"></a>

<span data-ttu-id="cb282-333">本逐步解說，說明了 Visual Studio 網頁設計工具的基本功能。</span><span class="sxs-lookup"><span data-stu-id="cb282-333">This walkthrough has illustrated the basic features of the Visual Studio page designer.</span></span> <span data-ttu-id="cb282-334">既然您了解如何建立和編輯 Visual Studio 中的 Web Form 頁面，您可以瀏覽其他功能。</span><span class="sxs-lookup"><span data-stu-id="cb282-334">Now that you understand how to create and edit a Web Forms page in Visual Studio, you might want to explore other features.</span></span> <span data-ttu-id="cb282-335">例如，您可能想要執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="cb282-335">For example, you might want to do the following:</span></span>

- <span data-ttu-id="cb282-336">深入了解 ASP.NET Web Form 遵循逐步教學課程系列[開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="cb282-336">Learn more about ASP.NET Web Forms by following the step-by-step tutorial series [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).</span></span>
- <span data-ttu-id="cb282-337">深入了解 Cascading 樣式表 (CSS)。</span><span class="sxs-lookup"><span data-stu-id="cb282-337">Learn more about Cascading style sheets (CSS).</span></span> <span data-ttu-id="cb282-338">如需詳細資訊，請參閱[使用 CSS 概觀](https://msdn.microsoft.com/library/bb398931.aspx)。</span><span class="sxs-lookup"><span data-stu-id="cb282-338">For details, see [Working with CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).</span></span>
