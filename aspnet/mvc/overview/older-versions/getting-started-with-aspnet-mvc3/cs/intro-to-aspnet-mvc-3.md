---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (C#) 簡介 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: b0ff8da1911b36e6e74e5c7057f27d891ad57f61
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832285"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="23ef4-103">ASP.NET MVC 3 (C#) 簡介</span><span class="sxs-lookup"><span data-stu-id="23ef4-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="23ef4-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="23ef4-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="23ef4-105">本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="23ef4-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="23ef4-106">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="23ef4-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="23ef4-107">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="23ef4-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="23ef4-108">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="23ef4-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="23ef4-109">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="23ef4-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="23ef4-110">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="23ef4-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="23ef4-111">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="23ef4-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="23ef4-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="23ef4-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="23ef4-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="23ef4-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="23ef4-114">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="23ef4-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="23ef4-115">使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="23ef4-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="23ef4-116">[下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="23ef4-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="23ef4-117">如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="23ef4-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="23ef4-118">您將建置</span><span class="sxs-lookup"><span data-stu-id="23ef4-118">What You'll Build</span></span>

<span data-ttu-id="23ef4-119">您將實作簡單的電影清單應用程式可支援建立、 編輯和列出資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="23ef4-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="23ef4-120">以下是兩個螢幕擷取畫面，您將建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="23ef4-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="23ef4-121">它包含顯示資料庫中的電影清單的頁面：</span><span class="sxs-lookup"><span data-stu-id="23ef4-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="23ef4-123">應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的項目相關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="23ef4-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="23ef4-124">所有的輸入資料的案例包括驗證，以確保儲存在資料庫中的資料正確無誤。</span><span class="sxs-lookup"><span data-stu-id="23ef4-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="23ef4-125">您將學習到的技能</span><span class="sxs-lookup"><span data-stu-id="23ef4-125">Skills You'll Learn</span></span>

<span data-ttu-id="23ef4-126">以下是您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="23ef4-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="23ef4-127">如何建立新的 ASP.NET MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="23ef4-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="23ef4-128">如何建立 ASP.NET MVC 控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="23ef4-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="23ef4-129">如何建立新的資料庫，使用 Entity Framework Code First 開發架構。</span><span class="sxs-lookup"><span data-stu-id="23ef4-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="23ef4-130">如何擷取及顯示資料。</span><span class="sxs-lookup"><span data-stu-id="23ef4-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="23ef4-131">如何編輯資料，並啟用資料驗證。</span><span class="sxs-lookup"><span data-stu-id="23ef4-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="23ef4-132">快速入門</span><span class="sxs-lookup"><span data-stu-id="23ef4-132">Getting Started</span></span>

<span data-ttu-id="23ef4-133">啟動執行 Visual Web Developer 2010 Express ("Visual Web Developer"簡稱)，並選取**新的專案**從**開始**頁面。</span><span class="sxs-lookup"><span data-stu-id="23ef4-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="23ef4-134">Visual Web Developer 是 IDE 或整合式的開發環境。</span><span class="sxs-lookup"><span data-stu-id="23ef4-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="23ef4-135">就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="23ef4-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="23ef4-136">在 Visual Web Developer 中，沒有工具列上方顯示各種可用的選項給您。</span><span class="sxs-lookup"><span data-stu-id="23ef4-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="23ef4-137">另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。</span><span class="sxs-lookup"><span data-stu-id="23ef4-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="23ef4-138">(例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)</span><span class="sxs-lookup"><span data-stu-id="23ef4-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="23ef4-139">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="23ef4-139">Creating Your First Application</span></span>

<span data-ttu-id="23ef4-140">您可以建立使用 Visual Basic 或 Visual C# 程式設計語言的應用程式。</span><span class="sxs-lookup"><span data-stu-id="23ef4-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="23ef4-141">選取 Visual C# 在左邊，然後選取**ASP.NET MVC 3 Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="23ef4-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="23ef4-142">命名您的專案"為 MvcMovie"，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="23ef4-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="23ef4-143">(如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。)</span><span class="sxs-lookup"><span data-stu-id="23ef4-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="23ef4-144">在 **新的 ASP.NET MVC 3 專案**對話方塊中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="23ef4-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="23ef4-145">請檢查**使用 HTML5 標記**並保留**Razor**做為預設檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="23ef4-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="23ef4-146">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="23ef4-146">Click **OK**.</span></span> <span data-ttu-id="23ef4-147">Visual Web Developer 中使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有運作中應用程式現在無須執行任何動作 ！</span><span class="sxs-lookup"><span data-stu-id="23ef4-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="23ef4-148">這是簡單"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="23ef4-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="23ef4-149">專案和它的啟動您的應用程式的好地方。</span><span class="sxs-lookup"><span data-stu-id="23ef4-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="23ef4-150">從 [偵錯] 功能表中，選取 [開始偵錯]。</span><span class="sxs-lookup"><span data-stu-id="23ef4-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="23ef4-151">請注意，若要開始偵錯的鍵盤快速鍵 F5。</span><span class="sxs-lookup"><span data-stu-id="23ef4-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="23ef4-152">F5 會導致 Visual Web Developer，以啟動開發 web 伺服器並執行您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="23ef4-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="23ef4-153">然後 visual Web Developer 會啟動瀏覽器，並開啟應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="23ef4-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="23ef4-154">通知，指出瀏覽器的網址列`localhost`而不是類似`example.com`。</span><span class="sxs-lookup"><span data-stu-id="23ef4-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="23ef4-155">這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="23ef4-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="23ef4-156">Visual Web Developer 中執行時 web 專案，會將隨機的連接埠用於 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="23ef4-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="23ef4-157">下圖中的隨機連接埠號碼會是 43246。</span><span class="sxs-lookup"><span data-stu-id="23ef4-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="23ef4-158">當您執行應用程式時，您可能會看到不同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="23ef4-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="23ef4-159">現成這個預設範本可讓您瀏覽的兩個頁面和基本的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="23ef4-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="23ef4-160">下一個步驟是要變更此應用程式的運作方式，並在程序有點了解 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="23ef4-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="23ef4-161">關閉瀏覽器，並讓我們變更一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="23ef4-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="23ef4-162">下一步</span><span class="sxs-lookup"><span data-stu-id="23ef4-162">Next</span></span>](adding-a-controller.md)
