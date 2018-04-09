---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (VB) 簡介 |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 3be0de6ea6d49f9c0de659398190b71c36ba222a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-vb"></a><span data-ttu-id="3c951-103">ASP.NET MVC 3 (VB) 簡介</span><span class="sxs-lookup"><span data-stu-id="3c951-103">Intro to ASP.NET MVC 3 (VB)</span></span>
====================
<span data-ttu-id="3c951-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3c951-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="3c951-105">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="3c951-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="3c951-106">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="3c951-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="3c951-107">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="3c951-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="3c951-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="3c951-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="3c951-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="3c951-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="3c951-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="3c951-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="3c951-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="3c951-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="3c951-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="3c951-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="3c951-113">使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="3c951-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="3c951-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="3c951-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="3c951-115">如果您偏好 C#，切換至[C# 版本](../cs/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="3c951-115">If you prefer C#, switch to the [C# version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="3c951-116">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="3c951-116">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="3c951-117">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="3c951-117">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="3c951-118">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="3c951-118">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="3c951-119">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="3c951-119">Alternatively, you can individually install the prerequisites using the following links:</span></span>

- [<span data-ttu-id="3c951-120">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="3c951-120">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [<span data-ttu-id="3c951-121">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="3c951-121">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- <span data-ttu-id="3c951-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="3c951-122">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>

<span data-ttu-id="3c951-123">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="3c951-123">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>

<span data-ttu-id="3c951-124">使用本主題隨附 Visual Web Developer 專案 VB 的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="3c951-124">A Visual Web Developer project with VB source code is available to accompany this topic.</span></span> <span data-ttu-id="3c951-125">[下載的 VB 版本](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)。</span><span class="sxs-lookup"><span data-stu-id="3c951-125">[Download the VB version here](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824).</span></span> <span data-ttu-id="3c951-126">如果您偏好 CSharp，切換至[CSharp 版本](../cs/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="3c951-126">If you prefer CSharp, switch to the [CSharp version](../cs/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="3c951-127">您將建置</span><span class="sxs-lookup"><span data-stu-id="3c951-127">What You'll Build</span></span>

<span data-ttu-id="3c951-128">您將實作簡單的電影清單應用程式可支援建立、 編輯，以及列出資料庫中的影片。</span><span class="sxs-lookup"><span data-stu-id="3c951-128">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="3c951-129">以下是您將建置的應用程式的兩個螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="3c951-129">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="3c951-130">它包含顯示之頁面的資料庫中的電影清單：</span><span class="sxs-lookup"><span data-stu-id="3c951-130">It includes a page that displays a list of movies from a database:</span></span>

<span data-ttu-id="3c951-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3c951-131">[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)</span></span>

<span data-ttu-id="3c951-132">應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的有關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="3c951-132">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="3c951-133">所有的資料輸入案例包括驗證，以確保儲存在資料庫中的資料正確無誤。</span><span class="sxs-lookup"><span data-stu-id="3c951-133">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

<span data-ttu-id="3c951-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3c951-134">[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="3c951-135">您將學習到的技術</span><span class="sxs-lookup"><span data-stu-id="3c951-135">Skills You'll Learn</span></span>

<span data-ttu-id="3c951-136">以下是您將學習：</span><span class="sxs-lookup"><span data-stu-id="3c951-136">Here's what you'll learn:</span></span>

- <span data-ttu-id="3c951-137">如何建立新的 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="3c951-137">How to create a new ASP.NET MVC project</span></span>
- <span data-ttu-id="3c951-138">如何建立新的資料庫使用 Entity Framework 程式碼優先 （contract-first）</span><span class="sxs-lookup"><span data-stu-id="3c951-138">How to create a new database using Entity Framework code-first</span></span>
- <span data-ttu-id="3c951-139">如何建立 ASP.NET MVC 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="3c951-139">How to create ASP.NET MVC controllers and views</span></span>
- <span data-ttu-id="3c951-140">如何擷取及顯示資料</span><span class="sxs-lookup"><span data-stu-id="3c951-140">How to retrieve and display data</span></span>
- <span data-ttu-id="3c951-141">如何編輯資料，並啟用資料驗證</span><span class="sxs-lookup"><span data-stu-id="3c951-141">How to edit data and enable data validation</span></span>

## <a name="getting-started"></a><span data-ttu-id="3c951-142">快速入門</span><span class="sxs-lookup"><span data-stu-id="3c951-142">Getting Started</span></span>

<span data-ttu-id="3c951-143">開始執行 Visual Web Developer 2010 Express (簡稱為 「 VWD")，並選取**新專案**從**啟動**頁面。</span><span class="sxs-lookup"><span data-stu-id="3c951-143">Start by running Visual Web Developer 2010 Express ("VWD" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="3c951-144">Visual Web Developer 是 IDE 或整合式的開發環境。</span><span class="sxs-lookup"><span data-stu-id="3c951-144">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="3c951-145">就像您使用 Microsoft Word 來寫入的文件時，您將使用 IDE 來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c951-145">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="3c951-146">在 Visual Web Developer 中，沒有工具列上方顯示各種可用的選項給您。</span><span class="sxs-lookup"><span data-stu-id="3c951-146">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="3c951-147">也會提供另一種方式，在 IDE 中執行工作的功能表。</span><span class="sxs-lookup"><span data-stu-id="3c951-147">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="3c951-148">(例如，而不是選取**新的專案**從**啟動** 頁面上，您可以使用功能表並選取**檔案** &gt; **新專案**.)</span><span class="sxs-lookup"><span data-stu-id="3c951-148">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="3c951-149">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="3c951-149">Creating Your First Application</span></span>

<span data-ttu-id="3c951-150">您可以建立使用您所選擇的 Visual Basic 或 Visual C# 當做程式語言的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c951-150">You can create applications using your choice of either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="3c951-151">此教學課程中，選取 Visual Basic 的左側，然後選取  **ASP.NET MVC 3 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="3c951-151">For this tutorial, select Visual Basic on the left, then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="3c951-152">將您的專案"MvcMovie"，再按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="3c951-152">Name your project "MvcMovie" and then click **OK**.</span></span>

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="3c951-154">在**新增 ASP.NET MVC 3 專案**對話方塊中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="3c951-154">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="3c951-155">保留**Razor**做為預設檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="3c951-155">Leave **Razor** as the default view engine.</span></span>

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

<span data-ttu-id="3c951-157">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="3c951-157">Click **OK**.</span></span> <span data-ttu-id="3c951-158">Visual Web Developer 中使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有可用的應用程式立即而不做任何事 ！</span><span class="sxs-lookup"><span data-stu-id="3c951-158">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="3c951-159">這是簡單"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="3c951-159">This is a simple "Hello World!"</span></span> <span data-ttu-id="3c951-160">專案和它的啟動您的應用程式的好地方。</span><span class="sxs-lookup"><span data-stu-id="3c951-160">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="3c951-161">從 [偵錯] 功能表中，選取 [開始偵錯]。</span><span class="sxs-lookup"><span data-stu-id="3c951-161">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image11.png)

<span data-ttu-id="3c951-162">請注意，開始偵錯的鍵盤快速鍵 F5。</span><span class="sxs-lookup"><span data-stu-id="3c951-162">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="3c951-163">F5 會導致啟動開發 web 伺服器和執行 web 應用程式的 Visual Web Developer。</span><span class="sxs-lookup"><span data-stu-id="3c951-163">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="3c951-164">VWD 然後會啟動瀏覽器，並開啟應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="3c951-164">VWD then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="3c951-165">請注意，該處會指示瀏覽器的網址列`localhost`並不是類似`example.com`。</span><span class="sxs-lookup"><span data-stu-id="3c951-165">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="3c951-166">這是因為`localhost`一律會指向您自己的本機電腦，在此情況下執行您剛才建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c951-166">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="3c951-167">當 VWD 執行 web 專案時，專案使用隨機的連接埠。</span><span class="sxs-lookup"><span data-stu-id="3c951-167">When VWD runs a web project, a random port is used for the project.</span></span> <span data-ttu-id="3c951-168">在下面的影像中的隨機連接埠號碼是 43246。</span><span class="sxs-lookup"><span data-stu-id="3c951-168">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="3c951-169">您的專案可能會使用不同的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="3c951-169">Your project will probably use a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image12.png)

<span data-ttu-id="3c951-170">現成提供的這個預設範本提供您兩個頁面瀏覽和基本登入頁面。</span><span class="sxs-lookup"><span data-stu-id="3c951-170">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="3c951-171">讓我們來變更這個應用程式的運作方式和程序中更了解 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="3c951-171">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="3c951-172">關閉瀏覽器，並讓我們將變更一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="3c951-172">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3c951-173">下一步</span><span class="sxs-lookup"><span data-stu-id="3c951-173">Next</span></span>](adding-a-controller.md)
