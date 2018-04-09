---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: 建立新的 ASP.NET MVC 專案 |Microsoft 文件
author: microsoft
description: 步驟 1 會示範如何將基本 NerdDinner 應用程式結構放在位置。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="45440-103">建立新的 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="45440-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="45440-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="45440-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="45440-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="45440-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="45440-106">這是一套免費的步驟 1 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="45440-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="45440-107">步驟 1 會示範如何將基本 NerdDinner 應用程式結構放在位置。</span><span class="sxs-lookup"><span data-stu-id="45440-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="45440-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="45440-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="45440-109">NerdDinner 步驟 1： 檔案-&gt;新專案</span><span class="sxs-lookup"><span data-stu-id="45440-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="45440-110">我們一開始我們 NerdDinner 應用程式會藉由選取**檔案-&gt;新專案**Visual Studio 2008 或免費 Visual Web Developer 2008 Express 中的功能表項目。</span><span class="sxs-lookup"><span data-stu-id="45440-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="45440-111">這會顯示 [新增專案] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="45440-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="45440-112">若要建立新的 ASP.NET MVC 應用程式，我們會選取對話方塊左側的"Web"節點，然後選擇 ["ASP.NET MVC Web 應用程式] 專案範本，在右邊：</span><span class="sxs-lookup"><span data-stu-id="45440-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="45440-113">*重要事項： 請確定您已下載並安裝 ASP.NET MVC-否則它不會顯示在 [新增專案] 對話方塊。您可以使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)如果您尚未尚未安裝 (ASP.NET MVC 內可供使用 」 Web 平台-&gt;架構和執行階段 」 一節)。*</span><span class="sxs-lookup"><span data-stu-id="45440-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="45440-114">我們會將新的專案，我們將建立 「 NerdDinner"，然後按一下 「 確定 」 按鈕來建立它。</span><span class="sxs-lookup"><span data-stu-id="45440-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="45440-115">當我們按一下 [確定] Visual Studio 會顯示其他對話方塊，提示我們選擇建立新應用程式的單元測試專案。</span><span class="sxs-lookup"><span data-stu-id="45440-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="45440-116">此單元測試專案可讓我們來建立自動化的測試來驗證功能和行為的應用程式 (項目仍會詳細說明如何在此教學課程後面的待辦事項)。</span><span class="sxs-lookup"><span data-stu-id="45440-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="45440-117">上述的對話方塊中的 「 測試架構 」 下拉式清單中填入所有使用 ASP.NET MVC 單元測試專案範本在電腦上安裝。</span><span class="sxs-lookup"><span data-stu-id="45440-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="45440-118">您可以下載版本 NUnit、 MBUnit，和 XUnit。</span><span class="sxs-lookup"><span data-stu-id="45440-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="45440-119">也支援內建的 Visual Studio 單元測試架構。</span><span class="sxs-lookup"><span data-stu-id="45440-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="45440-120">*注意： Visual Studio 單元測試架構才可使用 Visual Studio 2008 專業版和更新版本。如果您使用 VS 2008 Standard Edition 或 Visual Web Developer 2008 Express，您必須下載並安裝 ASP.NET MVC 的 NUnit、 MBUnit 或 XUnit 延伸，以便顯示此對話方塊。如果沒有安裝任何測試架構，將不會顯示對話方塊。*</span><span class="sxs-lookup"><span data-stu-id="45440-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="45440-121">我們將使用預設 「 NerdDinner.Tests 」 名稱，我們建立時，測試專案，並使用 「 Visual Studio 單元測試 」 架構選項。</span><span class="sxs-lookup"><span data-stu-id="45440-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="45440-122">當我們按一下 Visual Studio 的 「 確定 」 按鈕將使用它的其中一個 web 應用程式，另一個用於我們的單元測試中的兩個專案就讓我們建立方案：</span><span class="sxs-lookup"><span data-stu-id="45440-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="45440-123">檢查 NerdDinner 目錄結構</span><span class="sxs-lookup"><span data-stu-id="45440-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="45440-124">當您使用 Visual Studio 建立新的 ASP.NET MVC 應用程式時，它會自動將檔案和目錄的數字加入專案：</span><span class="sxs-lookup"><span data-stu-id="45440-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="45440-125">預設的 ASP.NET MVC 專案有六個最上層目錄：</span><span class="sxs-lookup"><span data-stu-id="45440-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="45440-126">**Directory**</span><span class="sxs-lookup"><span data-stu-id="45440-126">**Directory**</span></span> | <span data-ttu-id="45440-127">**目的**</span><span class="sxs-lookup"><span data-stu-id="45440-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="45440-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="45440-128">**/Controllers**</span></span> | <span data-ttu-id="45440-129">您在其中放置處理 URL 要求的控制器類別</span><span class="sxs-lookup"><span data-stu-id="45440-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="45440-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="45440-130">**/Models**</span></span> | <span data-ttu-id="45440-131">您在其中放置代表和操作資料的類別</span><span class="sxs-lookup"><span data-stu-id="45440-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="45440-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="45440-132">**/Views**</span></span> | <span data-ttu-id="45440-133">您在其中放置所負責轉譯輸出 UI 範本檔案</span><span class="sxs-lookup"><span data-stu-id="45440-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="45440-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="45440-134">**/Scripts**</span></span> | <span data-ttu-id="45440-135">您可在此將 JavaScript 程式庫檔案和指令碼 (.js)</span><span class="sxs-lookup"><span data-stu-id="45440-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="45440-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="45440-136">**/Content**</span></span> | <span data-ttu-id="45440-137">您在其中放置 CSS 和映像檔，以及其他非-動態/非-JavaScript 內容</span><span class="sxs-lookup"><span data-stu-id="45440-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="45440-138">**/App\_Data**</span><span class="sxs-lookup"><span data-stu-id="45440-138">**/App\_Data**</span></span> | <span data-ttu-id="45440-139">將資料檔案儲存在您要讀取/寫入。</span><span class="sxs-lookup"><span data-stu-id="45440-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="45440-140">ASP.NET MVC 不需要這個結構。</span><span class="sxs-lookup"><span data-stu-id="45440-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="45440-141">事實上，大型應用程式開發人員將通常分割應用程式設定跨多個專案，使其更易管理 (例如： 資料模型類別通常從移至個別的類別庫專案中的 web 應用程式)。</span><span class="sxs-lookup"><span data-stu-id="45440-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="45440-142">不過，預設專案結構，並提供好的預設目錄慣例我們可以用來保留全新我們的應用程式考量。</span><span class="sxs-lookup"><span data-stu-id="45440-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="45440-143">當我們展開 /Controllers 目錄，我們會發現，Visual Studio 兩個控制器類別 – HomeController 和 AccountController – 依預設加入至專案：</span><span class="sxs-lookup"><span data-stu-id="45440-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="45440-144">當我們展開 /Views 目錄時，我們會發現三個子目錄 – /Home、 /Account 和 /Shared – 以及數個範本，其中檔案預設也加入至專案：</span><span class="sxs-lookup"><span data-stu-id="45440-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="45440-145">當我們展開的 /Content 和 /Scripts 目錄時，我們也可以找到用來設定所有 HTML 都樣式，於網站上的 Site.css 檔案，以及可以啟用 ASP.NET AJAX 和 jQuery 的 JavaScript 程式庫支援應用程式中：</span><span class="sxs-lookup"><span data-stu-id="45440-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="45440-146">當我們展開 NerdDinner.Tests 專案時，我們也可以找到包含我們控制器類別的單元測試的兩個類別：</span><span class="sxs-lookup"><span data-stu-id="45440-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="45440-147">這些加入的 Visual Studio 的預設檔案工作應用程式層完成，但有關頁面、 帳戶登入/登出註冊頁面，以及未處理的錯誤頁面 （所有有線向上和現成提供的工作） 的首頁上，提供我們的基本結構。</span><span class="sxs-lookup"><span data-stu-id="45440-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="45440-148">執行 NerdDinner 應用程式</span><span class="sxs-lookup"><span data-stu-id="45440-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="45440-149">我們可以執行專案，選擇 **偵錯-&gt;開始偵錯**或**偵錯-&gt;啟動但不偵錯**功能表項目：</span><span class="sxs-lookup"><span data-stu-id="45440-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="45440-150">這會啟動內建 ASP.NET Web 伺服器隨附於 Visual Studio 中，並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="45440-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="45440-151">以下是我們的新專案的首頁 (URL:"/") 在執行時：</span><span class="sxs-lookup"><span data-stu-id="45440-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="45440-152">按一下 [關於] 索引標籤會顯示有關頁面 (URL:"/ 家用/關於 」):</span><span class="sxs-lookup"><span data-stu-id="45440-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="45440-153">按一下右上方的 「 登入 」 連結會帶我們前往登入頁面 (URL:"/ 帳戶/登入 」)</span><span class="sxs-lookup"><span data-stu-id="45440-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="45440-154">如果我們沒有登入帳戶，我們可以按一下暫存器連結 (URL:"/ 帳戶/暫存器 」) 來建立一個：</span><span class="sxs-lookup"><span data-stu-id="45440-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="45440-155">程式碼來實作上述的首頁、 關於和登出 / 註冊當我們建立新專案時依預設加入的功能。</span><span class="sxs-lookup"><span data-stu-id="45440-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="45440-156">我們將使用它做為我們的應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="45440-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="45440-157">測試 NerdDinner 應用程式</span><span class="sxs-lookup"><span data-stu-id="45440-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="45440-158">如果我們使用的專業版或更高版本的 Visual Studio 2008，我們可以使用的內建的單元測試 Visual Studio 中的 IDE 支援測試專案：</span><span class="sxs-lookup"><span data-stu-id="45440-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="45440-159">選擇其中一個上述選項會開啟 「 測試結果 」 窗格中的，在 IDE 中，並提供涵蓋的內建功能的 27 單元測試包含在新專案中的成功/失敗狀態：</span><span class="sxs-lookup"><span data-stu-id="45440-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="45440-160">稍後在本教學課程中我們將討論更多關於自動化測試，並新增其他單元測試，其中涵蓋應用程式功能，我們會實作。</span><span class="sxs-lookup"><span data-stu-id="45440-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="45440-161">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="45440-161">Next Step</span></span>

<span data-ttu-id="45440-162">我們現在有基本應用程式結構中的位置。</span><span class="sxs-lookup"><span data-stu-id="45440-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="45440-163">我們現在[建立資料庫來儲存應用程式資料](create-a-database.md)。</span><span class="sxs-lookup"><span data-stu-id="45440-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45440-164">[上一頁](introducing-the-nerddinner-tutorial.md)
> [下一頁](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="45440-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
