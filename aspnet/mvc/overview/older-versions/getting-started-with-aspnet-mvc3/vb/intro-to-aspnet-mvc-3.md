---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (VB) 簡介 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f596dbfb534a64169767fb77fb15ecc867466c74
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577180"
---
<a name="intro-to-aspnet-mvc-3-vb"></a><span data-ttu-id="c595c-103">ASP.NET MVC 3 (VB) 簡介</span><span class="sxs-lookup"><span data-stu-id="c595c-103">Intro to ASP.NET MVC 3 (VB)</span></span>
====================
<span data-ttu-id="c595c-104">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="c595c-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="c595c-105">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="c595c-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="c595c-106">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="c595c-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="c595c-107">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="c595c-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="c595c-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="c595c-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="c595c-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="c595c-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="c595c-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="c595c-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="c595c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="c595c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="c595c-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="c595c-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="c595c-113">使用本主題隨附了 VB.NET 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="c595c-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="c595c-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="c595c-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="c595c-115">如果您偏好 C#，切換至[C# 版本](../cs/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="c595c-115">If you prefer C#, switch to the [C# version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="c595c-116">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="c595c-116">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="c595c-117">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="c595c-117">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="c595c-118">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="c595c-118">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="c595c-119">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="c595c-119">Alternatively, you can individually install the prerequisites using the following links:</span></span>

- [<span data-ttu-id="c595c-120">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="c595c-120">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [<span data-ttu-id="c595c-121">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="c595c-121">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- <span data-ttu-id="c595c-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="c595c-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>

<span data-ttu-id="c595c-123">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="c595c-123">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>

<span data-ttu-id="c595c-124">使用本主題隨附了 VB 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="c595c-124">A Visual Web Developer project with VB source code is available to accompany this topic.</span></span> <span data-ttu-id="c595c-125">[下載 VB 版本](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)。</span><span class="sxs-lookup"><span data-stu-id="c595c-125">[Download the VB version here](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824).</span></span> <span data-ttu-id="c595c-126">如果您偏好 CSharp，切換至[CSharp 版本](../cs/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="c595c-126">If you prefer CSharp, switch to the [CSharp version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="c595c-127">您將建置</span><span class="sxs-lookup"><span data-stu-id="c595c-127">What You'll Build</span></span>

<span data-ttu-id="c595c-128">您將實作簡單的電影清單應用程式可支援建立、 編輯和列出資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="c595c-128">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="c595c-129">以下是兩個螢幕擷取畫面，您將建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c595c-129">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="c595c-130">它包含顯示資料庫中的電影清單的頁面：</span><span class="sxs-lookup"><span data-stu-id="c595c-130">It includes a page that displays a list of movies from a database:</span></span>

<span data-ttu-id="c595c-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c595c-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span></span>

<span data-ttu-id="c595c-132">應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的項目相關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c595c-132">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="c595c-133">所有的輸入資料的案例包括驗證，以確保儲存在資料庫中的資料正確無誤。</span><span class="sxs-lookup"><span data-stu-id="c595c-133">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

<span data-ttu-id="c595c-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c595c-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="c595c-135">您將學習到的技能</span><span class="sxs-lookup"><span data-stu-id="c595c-135">Skills You'll Learn</span></span>

<span data-ttu-id="c595c-136">以下是您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="c595c-136">Here's what you'll learn:</span></span>

- <span data-ttu-id="c595c-137">如何建立新的 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="c595c-137">How to create a new ASP.NET MVC project</span></span>
- <span data-ttu-id="c595c-138">如何建立新的資料庫，使用 Entity Framework code first</span><span class="sxs-lookup"><span data-stu-id="c595c-138">How to create a new database using Entity Framework code-first</span></span>
- <span data-ttu-id="c595c-139">如何建立 ASP.NET MVC 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="c595c-139">How to create ASP.NET MVC controllers and views</span></span>
- <span data-ttu-id="c595c-140">如何擷取及顯示資料</span><span class="sxs-lookup"><span data-stu-id="c595c-140">How to retrieve and display data</span></span>
- <span data-ttu-id="c595c-141">如何編輯資料，並啟用資料驗證</span><span class="sxs-lookup"><span data-stu-id="c595c-141">How to edit data and enable data validation</span></span>

## <a name="getting-started"></a><span data-ttu-id="c595c-142">快速入門</span><span class="sxs-lookup"><span data-stu-id="c595c-142">Getting Started</span></span>

<span data-ttu-id="c595c-143">執行 Visual Web Developer 2010 Express (簡稱為 「 VWD") 來啟動，然後選取**新的專案**從**開始**頁面。</span><span class="sxs-lookup"><span data-stu-id="c595c-143">Start by running Visual Web Developer 2010 Express ("VWD" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="c595c-144">Visual Web Developer 是 IDE 或整合式的開發環境。</span><span class="sxs-lookup"><span data-stu-id="c595c-144">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="c595c-145">就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="c595c-145">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="c595c-146">在 Visual Web Developer 中，沒有工具列上方顯示各種可用的選項給您。</span><span class="sxs-lookup"><span data-stu-id="c595c-146">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="c595c-147">另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。</span><span class="sxs-lookup"><span data-stu-id="c595c-147">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="c595c-148">(例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)</span><span class="sxs-lookup"><span data-stu-id="c595c-148">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="c595c-149">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="c595c-149">Creating Your First Application</span></span>

<span data-ttu-id="c595c-150">您可以建立使用您選擇的 Visual Basic 或 Visual C# 程式設計語言的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c595c-150">You can create applications using your choice of either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="c595c-151">本教學課程中，選取 Visual Basic 在左邊，然後選取**ASP.NET MVC 3 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c595c-151">For this tutorial, select Visual Basic on the left, then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="c595c-152">命名您的專案"為 MvcMovie"，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="c595c-152">Name your project "MvcMovie" and then click **OK**.</span></span>

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="c595c-154">在 **新的 ASP.NET MVC 3 專案**對話方塊中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="c595c-154">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="c595c-155">離開**Razor**做為預設檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="c595c-155">Leave **Razor** as the default view engine.</span></span>

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

<span data-ttu-id="c595c-157">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="c595c-157">Click **OK**.</span></span> <span data-ttu-id="c595c-158">Visual Web Developer 中使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有運作中應用程式現在無須執行任何動作 ！</span><span class="sxs-lookup"><span data-stu-id="c595c-158">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="c595c-159">這是簡單"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="c595c-159">This is a simple "Hello World!"</span></span> <span data-ttu-id="c595c-160">專案和它的啟動您的應用程式的好地方。</span><span class="sxs-lookup"><span data-stu-id="c595c-160">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="c595c-161">從 [偵錯] 功能表中，選取 [開始偵錯]。</span><span class="sxs-lookup"><span data-stu-id="c595c-161">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image11.png)

<span data-ttu-id="c595c-162">請注意，若要開始偵錯的鍵盤快速鍵 F5。</span><span class="sxs-lookup"><span data-stu-id="c595c-162">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="c595c-163">F5 會導致 Visual Web Developer，以啟動開發 web 伺服器並執行您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c595c-163">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="c595c-164">VWD 再啟動瀏覽器，並開啟應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="c595c-164">VWD then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="c595c-165">通知，指出瀏覽器的網址列`localhost`而不是類似`example.com`。</span><span class="sxs-lookup"><span data-stu-id="c595c-165">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="c595c-166">這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c595c-166">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="c595c-167">當 VWD 執行 web 專案時，專案使用隨機的連接埠。</span><span class="sxs-lookup"><span data-stu-id="c595c-167">When VWD runs a web project, a random port is used for the project.</span></span> <span data-ttu-id="c595c-168">下圖中的隨機連接埠號碼會是 43246。</span><span class="sxs-lookup"><span data-stu-id="c595c-168">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="c595c-169">您的專案可能會使用不同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="c595c-169">Your project will probably use a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image12.png)

<span data-ttu-id="c595c-170">根據預設，這個預設範本會提供您兩個頁面來瀏覽和基本的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="c595c-170">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="c595c-171">讓我們變更此應用程式的運作方式，並在程序有點了解 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="c595c-171">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="c595c-172">關閉瀏覽器，並讓我們變更一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="c595c-172">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c595c-173">下一步</span><span class="sxs-lookup"><span data-stu-id="c595c-173">Next</span></span>](adding-a-controller.md)
