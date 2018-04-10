---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET MVC 簡介 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="d29c1-104">ASP.NET MVC 簡介</span><span class="sxs-lookup"><span data-stu-id="d29c1-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="d29c1-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d29c1-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d29c1-106">如果本教學課程中可用的更新的版本[這裡](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。</span><span class="sxs-lookup"><span data-stu-id="d29c1-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="d29c1-107">新的教學課程會使用 ASP.NET MVC 5，本教學課程提供許多改良。</span><span class="sxs-lookup"><span data-stu-id="d29c1-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="d29c1-108">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="d29c1-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="d29c1-109">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="d29c1-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="d29c1-110">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="d29c1-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="d29c1-111">讓我們進行我們第一個 ASP.NET MVC Web 應用程式使用[Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/)。</span><span class="sxs-lookup"><span data-stu-id="d29c1-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="d29c1-112">我們要極小的電影清單應用程式，讓我們將建立並列出電影。</span><span class="sxs-lookup"><span data-stu-id="d29c1-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="d29c1-113">您將建置</span><span class="sxs-lookup"><span data-stu-id="d29c1-113">What You'll Build</span></span>

<span data-ttu-id="d29c1-114">以下是您將建置的應用程式的兩個螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="d29c1-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="d29c1-115">您必須具有不同的資料行的電影的簡單的資料表。</span><span class="sxs-lookup"><span data-stu-id="d29c1-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="d29c1-116">[![電影清單-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d29c1-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="d29c1-117">而且您必須建立表單，我們可以將電影新增至清單。</span><span class="sxs-lookup"><span data-stu-id="d29c1-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="d29c1-118">[![建立電影-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d29c1-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="d29c1-119">您將學習到的技術</span><span class="sxs-lookup"><span data-stu-id="d29c1-119">Skills You'll Learn</span></span>

<span data-ttu-id="d29c1-120">本教學課程將告訴您建置 ASP.NET MVC Web 應用程式使用 Visual Studio 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="d29c1-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="d29c1-121">您將學習：</span><span class="sxs-lookup"><span data-stu-id="d29c1-121">You'll learn:</span></span>

- <span data-ttu-id="d29c1-122">如何建立新的 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="d29c1-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="d29c1-123">如何建立新的資料庫與 SQL Server</span><span class="sxs-lookup"><span data-stu-id="d29c1-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="d29c1-124">如何建立 ASP.NET MVC 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="d29c1-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="d29c1-125">如何擷取及顯示資料</span><span class="sxs-lookup"><span data-stu-id="d29c1-125">How to retrieve and display data</span></span>
- <span data-ttu-id="d29c1-126">如何編輯資料，並啟用資料驗證</span><span class="sxs-lookup"><span data-stu-id="d29c1-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="d29c1-127">如何更新資料庫結構描述</span><span class="sxs-lookup"><span data-stu-id="d29c1-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="d29c1-128">開始使用</span><span class="sxs-lookup"><span data-stu-id="d29c1-128">Get Started</span></span>

<span data-ttu-id="d29c1-129">執行 Visual Web Developer 2010 Express （我稱它為 「 VWD 」 從現在起），然後選取新的專案，從 [開始] 畫面啟動。</span><span class="sxs-lookup"><span data-stu-id="d29c1-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="d29c1-130">Visual Web Developer 是 IDE 或整合式開發環境。</span><span class="sxs-lookup"><span data-stu-id="d29c1-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="d29c1-131">就像您使用 Microsoft Word 來寫入的文件時，您將使用 IDE 來建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="d29c1-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="d29c1-132">沒有顯示各種可用的選項，以及您可能也使用來選取檔案 功能表頂端的工具列 |新的專案。</span><span class="sxs-lookup"><span data-stu-id="d29c1-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="d29c1-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d29c1-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="d29c1-134">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="d29c1-134">Creating Your First Application</span></span>

<span data-ttu-id="d29c1-135">您可以建立使用 Visual Basic 或 Visual C# 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d29c1-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="d29c1-136">現在，選取 Visual C# 在左邊，然後挑選 「 ASP.NET MVC 2 Web 應用程式 」。</span><span class="sxs-lookup"><span data-stu-id="d29c1-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="d29c1-137">「 電影 」 命名您的專案，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d29c1-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="d29c1-138">[![新的專案](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d29c1-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="d29c1-139">右邊是顯示在您的應用程式中的所有檔案和資料夾的 [方案總管]。</span><span class="sxs-lookup"><span data-stu-id="d29c1-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="d29c1-140">中間的大型視窗是時間的您編輯的程式碼並的支出最多。</span><span class="sxs-lookup"><span data-stu-id="d29c1-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="d29c1-141">Visual Studio 使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有可用的應用程式立即而不做任何事 ！</span><span class="sxs-lookup"><span data-stu-id="d29c1-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="d29c1-142">這是簡單"Hello World ！</span><span class="sxs-lookup"><span data-stu-id="d29c1-142">This is a simple "Hello World!</span></span> <span data-ttu-id="d29c1-143">專案和它是我們的應用程式啟動的好地方。</span><span class="sxs-lookup"><span data-stu-id="d29c1-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="d29c1-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d29c1-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="d29c1-145">選取 「 播放 」 按鈕，在工具列。</span><span class="sxs-lookup"><span data-stu-id="d29c1-145">Select the "play" button to the toolbar.</span></span>

![開始偵錯](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="d29c1-147">它是指向會編譯您的程式，並在網頁瀏覽器中啟動您的應用程式的權限的綠色箭頭。</span><span class="sxs-lookup"><span data-stu-id="d29c1-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="d29c1-148">*注意： 您可以改為在鍵盤上按 F5 或選取偵錯-&gt;從 [偵錯] 功能表開始偵錯。*</span><span class="sxs-lookup"><span data-stu-id="d29c1-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="d29c1-149">這會導致 Visual Web Developer 中啟動開發 web 伺服器和執行 web 應用程式 （沒有任何組態或手動啟用此功能所需的步驟）。</span><span class="sxs-lookup"><span data-stu-id="d29c1-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="d29c1-150">它接著啟動瀏覽器，並將它設定為瀏覽應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="d29c1-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="d29c1-151">請注意下方，瀏覽器的網址列中顯示"localhost"，並不像 example.com。這是因為 localhost 一律會指向您自己的本機電腦-在此情況下執行我們剛才建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d29c1-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="d29c1-152">[![首頁](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="d29c1-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="d29c1-153">現成提供的這個預設範本提供您兩個頁面瀏覽和基本登入頁面。</span><span class="sxs-lookup"><span data-stu-id="d29c1-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="d29c1-154">讓我們來變更這個應用程式的運作方式和程序中更了解 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="d29c1-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="d29c1-155">關閉您的瀏覽器，並可讓您變更一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="d29c1-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d29c1-156">下一步</span><span class="sxs-lookup"><span data-stu-id="d29c1-156">Next</span></span>](getting-started-with-mvc-part2.md)
