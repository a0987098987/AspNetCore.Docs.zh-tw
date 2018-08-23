---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: 建立影片資料庫應用程式在 15 分鐘內，使用 ASP.NET MVC (C#) |Microsoft Docs
author: StephenWalther
description: Stephen Walther 建置整個資料庫導向 ASP.NET MVC 應用程式從開始到完成。 本教學課程是針對新的 t 的人了解...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 16d62102713b0b86aa93284e39c82d3ae492f892
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830964"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a><span data-ttu-id="18535-104">建立影片資料庫應用程式在 15 分鐘內，使用 ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="18535-104">Create a Movie Database Application in 15 Minutes with ASP.NET MVC (C#)</span></span>
====================
<span data-ttu-id="18535-105">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="18535-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="18535-106">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="18535-106">Download Code</span></span>](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> <span data-ttu-id="18535-107">Stephen Walther 建置整個資料庫導向 ASP.NET MVC 應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="18535-107">Stephen Walther builds an entire database-driven ASP.NET MVC application from start to finish.</span></span> <span data-ttu-id="18535-108">本教學課程是了解誰是 ASP.NET MVC Framework 的新手，而且想要了解建置 ASP.NET MVC 應用程式的程序的人員。</span><span class="sxs-lookup"><span data-stu-id="18535-108">This tutorial is a great introduction for people who are new to the ASP.NET MVC Framework and who want to get a sense of the process of building an ASP.NET MVC application.</span></span>


<span data-ttu-id="18535-109">本教學課程的目的是讓您的 「 功能很類似 」 的意義上建置 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="18535-109">The purpose of this tutorial is to give you a sense of "what it is like" to build an ASP.NET MVC application.</span></span> <span data-ttu-id="18535-110">在本教學課程中，我炸毀透過建置整個 ASP.NET MVC 應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="18535-110">In this tutorial, I blast through building an entire ASP.NET MVC application from start to finish.</span></span> <span data-ttu-id="18535-111">我會告訴您如何建置的簡單資料庫導向應用程式示範如何列出、 建立和編輯資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-111">I show you how to build a simple database-driven application that illustrates how you can list, create, and edit database records.</span></span>

<span data-ttu-id="18535-112">若要簡化建置我們的應用程式的程序，我們將利用 Visual Studio 2008 的 scaffolding 功能。</span><span class="sxs-lookup"><span data-stu-id="18535-112">To simplify the process of building our application, we'll take advantage of the scaffolding features of Visual Studio 2008.</span></span> <span data-ttu-id="18535-113">我們會讓 Visual Studio 產生的初始程式碼和我們的控制器、 模型和檢視表的內容。</span><span class="sxs-lookup"><span data-stu-id="18535-113">We'll let Visual Studio generate the initial code and content for our controllers, models, and views.</span></span>

<span data-ttu-id="18535-114">如果您已使用 Active Server Pages 或 ASP.NET，則您應該會發現 ASP.NET MVC 非常熟悉。</span><span class="sxs-lookup"><span data-stu-id="18535-114">If you have worked with Active Server Pages or ASP.NET, then you should find ASP.NET MVC very familiar.</span></span> <span data-ttu-id="18535-115">ASP.NET MVC 檢視很像是 Active Server Pages 應用程式中的頁面。</span><span class="sxs-lookup"><span data-stu-id="18535-115">ASP.NET MVC views are very much like the pages in an Active Server Pages application.</span></span> <span data-ttu-id="18535-116">而且就像傳統的 ASP.NET Web Forms 應用程式，ASP.NET MVC 為您提供的完整存取權一組豐富的語言和.NET framework 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="18535-116">And, just like a traditional ASP.NET Web Forms application, ASP.NET MVC provides you with full access to the rich set of languages and classes provided by the .NET framework.</span></span>

<span data-ttu-id="18535-117">我希望是，本教學課程會提供您了解如何建置 ASP.NET MVC 應用程式的體驗是類似，不同於建置動態伺服器網頁或 ASP.NET Web Forms 應用程式的體驗。</span><span class="sxs-lookup"><span data-stu-id="18535-117">My hope is that this tutorial will give you a sense of how the experience of building an ASP.NET MVC application is both similar and different than the experience of building an Active Server Pages or ASP.NET Web Forms application.</span></span>

## <a name="overview-of-the-movie-database-application"></a><span data-ttu-id="18535-118">影片資料庫應用程式的概觀</span><span class="sxs-lookup"><span data-stu-id="18535-118">Overview of the Movie Database Application</span></span>

<span data-ttu-id="18535-119">因為我們的目標是為了簡單起見，我們將建置一個非常簡單的電影資料庫應用程式。</span><span class="sxs-lookup"><span data-stu-id="18535-119">Because our goal is to keep things simple, we'll build a very simple Movie Database application.</span></span> <span data-ttu-id="18535-120">我們簡單的電影資料庫應用程式可讓我們將執行三件事：</span><span class="sxs-lookup"><span data-stu-id="18535-120">Our simple Movie Database application will allow us to do three things:</span></span>

1. <span data-ttu-id="18535-121">列出一組影片資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="18535-121">List a set of movie database records</span></span>
2. <span data-ttu-id="18535-122">建立新的電影資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="18535-122">Create a new movie database record</span></span>
3. <span data-ttu-id="18535-123">編輯現有的電影資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="18535-123">Edit an existing movie database record</span></span>

<span data-ttu-id="18535-124">同樣地，因為我們想要簡單起見，我們會利用建置我們的應用程式所需的 ASP.NET MVC framework 功能的最小數目。</span><span class="sxs-lookup"><span data-stu-id="18535-124">Again, because we want to keep things simple, we'll take advantage of the minimum number of features of the ASP.NET MVC framework needed to build our application.</span></span> <span data-ttu-id="18535-125">比方說，我們將不會利用測試導向開發。</span><span class="sxs-lookup"><span data-stu-id="18535-125">For example, we won't be taking advantage of Test-Driven Development.</span></span>

<span data-ttu-id="18535-126">若要建立我們的應用程式，我們需要完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18535-126">In order to create our application, we need to complete each of the following steps:</span></span>

1. <span data-ttu-id="18535-127">建立 ASP.NET MVC Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="18535-127">Create the ASP.NET MVC Web Application Project</span></span>
2. <span data-ttu-id="18535-128">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="18535-128">Create the database</span></span>
3. <span data-ttu-id="18535-129">建立資料庫模型</span><span class="sxs-lookup"><span data-stu-id="18535-129">Create the database model</span></span>
4. <span data-ttu-id="18535-130">建立 ASP.NET MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="18535-130">Create the ASP.NET MVC controller</span></span>
5. <span data-ttu-id="18535-131">建立 ASP.NET MVC 檢視</span><span class="sxs-lookup"><span data-stu-id="18535-131">Create the ASP.NET MVC views</span></span>

## <a name="preliminaries"></a><span data-ttu-id="18535-132">準備工作</span><span class="sxs-lookup"><span data-stu-id="18535-132">Preliminaries</span></span>

<span data-ttu-id="18535-133">您將需要 Visual Studio 2008 或 Visual Web Developer 2008 Express 建置 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="18535-133">You'll need either Visual Studio 2008 or Visual Web Developer 2008 Express to build an ASP.NET MVC application.</span></span> <span data-ttu-id="18535-134">您也需要下載 ASP.NET MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="18535-134">You also need to download the ASP.NET MVC framework.</span></span>

<span data-ttu-id="18535-135">如果您沒有 Visual Studio 2008，您可以從此網站下載 Visual Studio 2008 90 天試用版：</span><span class="sxs-lookup"><span data-stu-id="18535-135">If you don't own Visual Studio 2008, then you can download a 90 day trial version of Visual Studio 2008 from this website:</span></span>

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

<span data-ttu-id="18535-136">或者，您可以建立 ASP.NET MVC 應用程式與 Visual Web Developer Express 2008。</span><span class="sxs-lookup"><span data-stu-id="18535-136">Alternatively, you can create ASP.NET MVC applications with Visual Web Developer Express 2008.</span></span> <span data-ttu-id="18535-137">如果您決定要使用 Visual Web Developer Express，您必須已安裝 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="18535-137">If you decide to use Visual Web Developer Express then you must have Service Pack 1 installed.</span></span> <span data-ttu-id="18535-138">您可以下載 Visual Web Developer 2008 Express 含 Service Pack 1，從這個網站：</span><span class="sxs-lookup"><span data-stu-id="18535-138">You can download Visual Web Developer 2008 Express with Service Pack 1 from this website:</span></span>

[<span data-ttu-id="18535-139">https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&ampdisplaylang = en</span><span class="sxs-lookup"><span data-stu-id="18535-139">https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

<span data-ttu-id="18535-140">安裝 Visual Studio 2008 或 Visual Web Developer 2008 之後，必須先安裝 ASP.NET MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="18535-140">After you install either Visual Studio 2008 or Visual Web Developer 2008, you need to install the ASP.NET MVC framework.</span></span> <span data-ttu-id="18535-141">您可以從下列網站下載 ASP.NET MVC 架構：</span><span class="sxs-lookup"><span data-stu-id="18535-141">You can download the ASP.NET MVC framework from the following website:</span></span>

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> <span data-ttu-id="18535-142">而不是個別下載 ASP.NET framework 和 ASP.NET MVC 架構，您可以利用 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="18535-142">Instead of downloading the ASP.NET framework and the ASP.NET MVC framework individually, you can take advantage of the Web Platform Installer.</span></span> <span data-ttu-id="18535-143">Web Platform Installer 是可讓您輕鬆地管理已安裝的應用程式的應用程式有您的電腦：</span><span class="sxs-lookup"><span data-stu-id="18535-143">The Web Platform Installer is an application that enables you to easily manage the installed applications are your computer:</span></span>
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a><span data-ttu-id="18535-144">建立 ASP.NET MVC Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="18535-144">Creating an ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="18535-145">現在就開始在 Visual Studio 2008 中建立新的 ASP.NET MVC Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="18535-145">Let's start by creating a new ASP.NET MVC Web Application project in Visual Studio 2008.</span></span> <span data-ttu-id="18535-146">選取功能表選項**檔案]、 [新增專案**，您會看到 [圖 1] 中的 [新增專案] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="18535-146">Select the menu option **File, New Project** and you will see the New Project dialog box in Figure 1.</span></span> <span data-ttu-id="18535-147">選取 C# 程式設計語言，然後選取 ASP.NET MVC Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="18535-147">Select C# as the programming language and select the ASP.NET MVC Web Application project template.</span></span> <span data-ttu-id="18535-148">為您的專案名稱 MovieApp，然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18535-148">Give your project the name MovieApp and click the OK button.</span></span>


<span data-ttu-id="18535-149">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="18535-149">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)</span></span>

<span data-ttu-id="18535-150">**[圖 01**: 新的專案] 對話方塊中 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="18535-150">**Figure 01**: The New Project dialog box ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))</span></span>


<span data-ttu-id="18535-151">請確定您從下拉式清單中頂端的 新增專案 對話方塊中選取 .NET Framework 3.5，或 ASP.NET MVC Web 應用程式專案範本不會出現。</span><span class="sxs-lookup"><span data-stu-id="18535-151">Make sure that you select .NET Framework 3.5 from the dropdown list at the top of the New Project dialog or the ASP.NET MVC Web Application project template won't appear.</span></span>


<span data-ttu-id="18535-152">每當您建立新的 MVC Web 應用程式專案，Visual Studio 會提示您建立個別的單元測試專案。</span><span class="sxs-lookup"><span data-stu-id="18535-152">Whenever you create a new MVC Web Application project, Visual Studio prompts you to create a separate unit test project.</span></span> <span data-ttu-id="18535-153">[圖 2] 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="18535-153">The dialog in Figure 2 appears.</span></span> <span data-ttu-id="18535-154">因為我們不會建立測試本教學課程中由於時間限制 （然後是，我們應該會覺得有點怪一點） 選取**No**選項，然後按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18535-154">Because we won't be creating tests in this tutorial because of time constraints (and, yes, we should feel a little guilty about this) select the **No** option and click the **OK** button.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="18535-155">Visual Web Developer 不支援測試專案。</span><span class="sxs-lookup"><span data-stu-id="18535-155">Visual Web Developer does not support test projects.</span></span>


<span data-ttu-id="18535-156">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="18535-156">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)</span></span>

<span data-ttu-id="18535-157">**[圖 02**: 建立單元測試專案] 對話方塊 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="18535-157">**Figure 02**: The Create Unit Test Project dialog ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))</span></span>


<span data-ttu-id="18535-158">ASP.NET MVC 應用程式有一組標準的資料夾： 模型、 檢視和控制器的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18535-158">An ASP.NET MVC application has a standard set of folders: a Models, Views, and Controllers folder.</span></span> <span data-ttu-id="18535-159">您可以看到這一組標準的 [方案總管] 視窗中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18535-159">You can see this standard set of folders in the Solution Explorer window.</span></span> <span data-ttu-id="18535-160">我們必須將檔案新增至每個模型、 檢視和控制器資料夾中，才能建立影片資料庫應用程式。</span><span class="sxs-lookup"><span data-stu-id="18535-160">We'll need to add files to each of the Models, Views, and Controllers folders in order to build our Movie Database application.</span></span>

<span data-ttu-id="18535-161">當您使用 Visual Studio 中建立新的 MVC 應用程式時，您會取得範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="18535-161">When you create a new MVC application with Visual Studio, you get a sample application.</span></span> <span data-ttu-id="18535-162">因為我們想要從頭開始，我們必須刪除此範例應用程式的內容。</span><span class="sxs-lookup"><span data-stu-id="18535-162">Because we want to start from scratch, we need to delete the content for this sample application.</span></span> <span data-ttu-id="18535-163">您必須刪除下列檔案和下列資料夾：</span><span class="sxs-lookup"><span data-stu-id="18535-163">You need to delete the following file and the following folder:</span></span>

- <span data-ttu-id="18535-164">Controllers\HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="18535-164">Controllers\HomeController.cs</span></span>
- <span data-ttu-id="18535-165">Views\Home</span><span class="sxs-lookup"><span data-stu-id="18535-165">Views\Home</span></span>

## <a name="creating-the-database"></a><span data-ttu-id="18535-166">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="18535-166">Creating the Database</span></span>

<span data-ttu-id="18535-167">我們需要建立資料庫來保存我們的電影資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-167">We need to create a database to hold our movie database records.</span></span> <span data-ttu-id="18535-168">幸運的是，Visual Studio 會包含名為 SQL Server Express 的免費資料庫。</span><span class="sxs-lookup"><span data-stu-id="18535-168">Luckily, Visual Studio includes a free database named SQL Server Express.</span></span> <span data-ttu-id="18535-169">請遵循下列步驟來建立資料庫：</span><span class="sxs-lookup"><span data-stu-id="18535-169">Follow these steps to create the database:</span></span>

1. <span data-ttu-id="18535-170">以滑鼠右鍵按一下 [應用程式\_Data 資料夾中的方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。</span><span class="sxs-lookup"><span data-stu-id="18535-170">Right-click the App\_Data folder in the Solution Explorer window and select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="18535-171">選取 **資料**類別目錄，然後選取**SQL Server 資料庫**範本 （見 圖 3）。</span><span class="sxs-lookup"><span data-stu-id="18535-171">Select the **Data** category and select the **SQL Server Database** template (see Figure 3).</span></span>
3. <span data-ttu-id="18535-172">將新資料庫命名*MoviesDB.mdf*然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18535-172">Name your new database *MoviesDB.mdf* and click the **Add** button.</span></span>

<span data-ttu-id="18535-173">建立您的資料庫之後，您可以按兩下 MoviesDB.mdf 檔案位於應用程式連接到資料庫\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18535-173">After you create your database, you can connect to the database by double-clicking the MoviesDB.mdf file located in the App\_Data folder.</span></span> <span data-ttu-id="18535-174">按兩下 MoviesDB.mdf 檔案會開啟 [伺服器總管] 視窗。</span><span class="sxs-lookup"><span data-stu-id="18535-174">Double-clicking the MoviesDB.mdf file opens the Server Explorer window.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="18535-175">資料庫總管 視窗中，在 Visual Web Developer 的情況下名為 伺服器總管 視窗。</span><span class="sxs-lookup"><span data-stu-id="18535-175">The Server Explorer window is named the Database Explorer window in the case of Visual Web Developer.</span></span>


<span data-ttu-id="18535-176">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="18535-176">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)</span></span>

<span data-ttu-id="18535-177">**圖 03**： 建立 Microsoft SQL Server 資料庫 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="18535-177">**Figure 03**: Creating a Microsoft SQL Server Database ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))</span></span>


<span data-ttu-id="18535-178">接下來，我們需要建立新的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="18535-178">Next, we need to create a new database table.</span></span> <span data-ttu-id="18535-179">從 [伺服器總管] 視窗中，在 [資料表] 資料夾上按一下滑鼠右鍵，然後選取功能表選項**加入新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="18535-179">From within the Sever Explorer window, right-click the Tables folder and select the menu option **Add New Table**.</span></span> <span data-ttu-id="18535-180">選取此功能表選項會開啟資料庫資料表設計工具。</span><span class="sxs-lookup"><span data-stu-id="18535-180">Selecting this menu option opens the database table designer.</span></span> <span data-ttu-id="18535-181">建立下列的資料庫資料行：</span><span class="sxs-lookup"><span data-stu-id="18535-181">Create the following database columns:</span></span>

<a id="0.1_table01"></a>


| <span data-ttu-id="18535-182">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="18535-182">**Column Name**</span></span> | <span data-ttu-id="18535-183">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="18535-183">**Data Type**</span></span> | <span data-ttu-id="18535-184">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="18535-184">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="18535-185">ID</span><span class="sxs-lookup"><span data-stu-id="18535-185">Id</span></span> | <span data-ttu-id="18535-186">Int</span><span class="sxs-lookup"><span data-stu-id="18535-186">Int</span></span> | <span data-ttu-id="18535-187">False</span><span class="sxs-lookup"><span data-stu-id="18535-187">False</span></span> |
| <span data-ttu-id="18535-188">標題</span><span class="sxs-lookup"><span data-stu-id="18535-188">Title</span></span> | <span data-ttu-id="18535-189">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="18535-189">Nvarchar(100)</span></span> | <span data-ttu-id="18535-190">False</span><span class="sxs-lookup"><span data-stu-id="18535-190">False</span></span> |
| <span data-ttu-id="18535-191">總監</span><span class="sxs-lookup"><span data-stu-id="18535-191">Director</span></span> | <span data-ttu-id="18535-192">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="18535-192">Nvarchar(100)</span></span> | <span data-ttu-id="18535-193">False</span><span class="sxs-lookup"><span data-stu-id="18535-193">False</span></span> |
| <span data-ttu-id="18535-194">DateReleased</span><span class="sxs-lookup"><span data-stu-id="18535-194">DateReleased</span></span> | <span data-ttu-id="18535-195">DateTime</span><span class="sxs-lookup"><span data-stu-id="18535-195">DateTime</span></span> | <span data-ttu-id="18535-196">False</span><span class="sxs-lookup"><span data-stu-id="18535-196">False</span></span> |


<span data-ttu-id="18535-197">第一個資料行，[識別碼] 欄中，有兩個特殊的屬性。</span><span class="sxs-lookup"><span data-stu-id="18535-197">The first column, the Id column, has two special properties.</span></span> <span data-ttu-id="18535-198">首先，您需要識別碼資料行標示為主要索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="18535-198">First, you need to mark the Id column as the primary key column.</span></span> <span data-ttu-id="18535-199">選取 識別碼 資料行之後, 按一下 **設定主索引鍵**（它是看起來像索引鍵的圖示） 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18535-199">After selecting the Id column, click the **Set Primary Key** button (it is the icon that looks like a key).</span></span> <span data-ttu-id="18535-200">第二，您需要識別碼資料行標示為識別欄位。</span><span class="sxs-lookup"><span data-stu-id="18535-200">Second, you need to mark the Id column as an Identity column.</span></span> <span data-ttu-id="18535-201">在資料行屬性] 視窗中，捲動至 [識別規格 」 區段，並加以展開。</span><span class="sxs-lookup"><span data-stu-id="18535-201">In the Column Properties window, scroll down to the Identity Specification section and expand it.</span></span> <span data-ttu-id="18535-202">變更**是身分識別**屬性設為值**是**。</span><span class="sxs-lookup"><span data-stu-id="18535-202">Change the **Is Identity** property to the value **Yes**.</span></span> <span data-ttu-id="18535-203">當您完成時，資料表看起來應該像 圖 4。</span><span class="sxs-lookup"><span data-stu-id="18535-203">When you are finished, the table should look like Figure 4.</span></span>


<span data-ttu-id="18535-204">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="18535-204">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)</span></span>

<span data-ttu-id="18535-205">**圖 04**: 電影資料庫資料表 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="18535-205">**Figure 04**: The Movies database table ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))</span></span>


<span data-ttu-id="18535-206">最後一個步驟是以儲存新的資料表。</span><span class="sxs-lookup"><span data-stu-id="18535-206">The final step is to save the new table.</span></span> <span data-ttu-id="18535-207">按一下 [儲存] 按鈕 （磁片圖示），並提供新的資料表名稱電影。</span><span class="sxs-lookup"><span data-stu-id="18535-207">Click the Save button (the icon of the floppy) and give the new table the name Movies.</span></span>

<span data-ttu-id="18535-208">完成 建立資料表之後，某些電影將記錄新增至資料表。</span><span class="sxs-lookup"><span data-stu-id="18535-208">After you finish creating the table, add some movie records to the table.</span></span> <span data-ttu-id="18535-209">在 [伺服器總管] 視窗中的電影資料表上按一下滑鼠右鍵，然後選取功能表選項**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="18535-209">Right-click the Movies table in the Server Explorer window and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="18535-210">輸入您最愛的電影，（請參閱 [圖 5]） 的清單。</span><span class="sxs-lookup"><span data-stu-id="18535-210">Enter a list of your favorite movies (see Figure 5).</span></span>


<span data-ttu-id="18535-211">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="18535-211">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)</span></span>

<span data-ttu-id="18535-212">**圖 05**： 輸入影片資料錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="18535-212">**Figure 05**: Entering movie records ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))</span></span>


## <a name="creating-the-model"></a><span data-ttu-id="18535-213">建立模型</span><span class="sxs-lookup"><span data-stu-id="18535-213">Creating the Model</span></span>

<span data-ttu-id="18535-214">接下來，我們需要建立一組類別來代表我們的資料庫。</span><span class="sxs-lookup"><span data-stu-id="18535-214">We next need to create a set of classes to represent our database.</span></span> <span data-ttu-id="18535-215">我們需要建立資料庫模型。</span><span class="sxs-lookup"><span data-stu-id="18535-215">We need to create a database model.</span></span> <span data-ttu-id="18535-216">我們會利用 Microsoft Entity Framework 自動產生我們的資料庫模型的類別。</span><span class="sxs-lookup"><span data-stu-id="18535-216">We'll take advantage of the Microsoft Entity Framework to generate the classes for our database model automatically.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="18535-217">ASP.NET MVC 架構不會繫結至 Microsoft Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="18535-217">The ASP.NET MVC framework is not tied to the Microsoft Entity Framework.</span></span> <span data-ttu-id="18535-218">您可以建立您的資料庫模型類別利用各種不同的物件關聯式對應 (或者 / M) 工具，包括 LINQ to SQL、 Subsonic 和 NHibernate。</span><span class="sxs-lookup"><span data-stu-id="18535-218">You can create your database model classes by taking advantage of a variety of Object Relational Mapping (OR/M) tools including LINQ to SQL, Subsonic, and NHibernate.</span></span>


<span data-ttu-id="18535-219">請遵循下列步驟來啟動 Entity Data Model 精靈：</span><span class="sxs-lookup"><span data-stu-id="18535-219">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="18535-220">在 [方案總管] 視窗，然後選取功能表選項中的 [Models] 資料夾上按一下滑鼠右鍵**新增]、 [新項目**。</span><span class="sxs-lookup"><span data-stu-id="18535-220">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="18535-221">選取 **資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。</span><span class="sxs-lookup"><span data-stu-id="18535-221">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="18535-222">將您的資料模型命名*MoviesDBModel.edmx*然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18535-222">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="18535-223">按一下 [新增] 按鈕之後，實體資料模型精靈] 會出現 （請參閱 [圖 6）。</span><span class="sxs-lookup"><span data-stu-id="18535-223">After you click the Add button, the Entity Data Model Wizard appears (see Figure 6).</span></span> <span data-ttu-id="18535-224">請遵循下列步驟來完成精靈：</span><span class="sxs-lookup"><span data-stu-id="18535-224">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="18535-225">在 **選擇模型內容**步驟中，選取**從資料庫產生**選項。</span><span class="sxs-lookup"><span data-stu-id="18535-225">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="18535-226">在 **選擇資料連接**步驟中，使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連線設定。</span><span class="sxs-lookup"><span data-stu-id="18535-226">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="18535-227">按一下 **下一步**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="18535-227">Click the **Next** button.</span></span>
3. <span data-ttu-id="18535-228">在 **選擇您的資料庫物件**步驟中，展開 資料表 節點中，選取 電影資料表。</span><span class="sxs-lookup"><span data-stu-id="18535-228">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="18535-229">輸入的命名空間*MovieApp.Models*然後按一下**完成** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18535-229">Enter the namespace *MovieApp.Models* and click the **Finish** button.</span></span>


<span data-ttu-id="18535-230">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="18535-230">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)</span></span>

<span data-ttu-id="18535-231">**圖 06**： 產生 Entity Data Model 精靈的資料庫模型 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="18535-231">**Figure 06**: Generating a database model with the Entity Data Model Wizard ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))</span></span>


<span data-ttu-id="18535-232">Entity Data Model 精靈完成之後，就會開啟實體資料模型設計工具。</span><span class="sxs-lookup"><span data-stu-id="18535-232">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="18535-233">設計工具應該會顯示電影資料庫資料表 （請參閱 圖 7）。</span><span class="sxs-lookup"><span data-stu-id="18535-233">The Designer should display the Movies database table (see Figure 7).</span></span>


<span data-ttu-id="18535-234">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="18535-234">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)</span></span>

<span data-ttu-id="18535-235">**圖 07**: 實體資料模型設計工具 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="18535-235">**Figure 07**: The Entity Data Model Designer ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))</span></span>


<span data-ttu-id="18535-236">我們需要進行一項變更，再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="18535-236">We need to make one change before we continue.</span></span> <span data-ttu-id="18535-237">實體資料精靈產生模型類別，名為*電影*表示電影資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="18535-237">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="18535-238">因為我們將使用電影類別來代表特定的影片，我們需要修改的類別名稱*電影*而不是*電影*（單數而複數）。</span><span class="sxs-lookup"><span data-stu-id="18535-238">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="18535-239">按兩下設計工具介面上類別名稱，並從電影時使用的類別名稱變更為電影。</span><span class="sxs-lookup"><span data-stu-id="18535-239">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="18535-240">完成此變更之後，按一下**儲存**按鈕 （磁片圖示） 來產生電影類別。</span><span class="sxs-lookup"><span data-stu-id="18535-240">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="creating-the-aspnet-mvc-controller"></a><span data-ttu-id="18535-241">建立 ASP.NET MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="18535-241">Creating the ASP.NET MVC Controller</span></span>

<span data-ttu-id="18535-242">下一個步驟是建立 ASP.NET MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="18535-242">The next step is to create the ASP.NET MVC controller.</span></span> <span data-ttu-id="18535-243">控制器負責控制使用者與 ASP.NET MVC 應用程式之間的互動方式。</span><span class="sxs-lookup"><span data-stu-id="18535-243">A controller is responsible for controlling how a user interacts with an ASP.NET MVC application.</span></span>

<span data-ttu-id="18535-244">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18535-244">Follow these steps:</span></span>

1. <span data-ttu-id="18535-245">在 [方案總管] 視窗中，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取功能表選項**新增]、 [控制站**。</span><span class="sxs-lookup"><span data-stu-id="18535-245">In the Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller**.</span></span>
2. <span data-ttu-id="18535-246">在 新增控制器 對話方塊中，輸入名稱*HomeController* ，並檢查標示的核取方塊**將動作方法，如 Create、 Update 和 Details 案例加入**（請參閱 圖 8）。</span><span class="sxs-lookup"><span data-stu-id="18535-246">In the Add Controller dialog, enter the name *HomeController* and check the checkbox labeled **Add action methods for Create, Update, and Details scenarios** (see Figure 8).</span></span>
3. <span data-ttu-id="18535-247">按一下 [**新增**] 按鈕，將新的控制器新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="18535-247">Click the **Add** button to add the new controller to your project.</span></span>

<span data-ttu-id="18535-248">完成這些步驟之後，會建立 列表 1 中的控制站。</span><span class="sxs-lookup"><span data-stu-id="18535-248">After you complete these steps, the controller in Listing 1 is created.</span></span> <span data-ttu-id="18535-249">請注意它所包含的方法，名為索引的詳細資訊，建立、 和編輯。</span><span class="sxs-lookup"><span data-stu-id="18535-249">Notice that it contains methods named Index, Details, Create, and Edit.</span></span> <span data-ttu-id="18535-250">在下列章節中，我們將新增必要的程式碼，以取得這些方法才能運作。</span><span class="sxs-lookup"><span data-stu-id="18535-250">In the following sections, we'll add the necessary code to get these methods to work.</span></span>


<span data-ttu-id="18535-251">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="18535-251">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)</span></span>

<span data-ttu-id="18535-252">**圖 08**： 加入新的 ASP.NET MVC 控制站 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="18535-252">**Figure 08**: Adding a new ASP.NET MVC Controller ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))</span></span>


<span data-ttu-id="18535-253">**列表 1 – Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="18535-253">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a><span data-ttu-id="18535-254">列出資料庫中的記錄</span><span class="sxs-lookup"><span data-stu-id="18535-254">Listing Database Records</span></span>

<span data-ttu-id="18535-255">主控制器的 index （） 方法是在 ASP.NET MVC 應用程式的預設方法。</span><span class="sxs-lookup"><span data-stu-id="18535-255">The Index() method of the Home controller is the default method for an ASP.NET MVC application.</span></span> <span data-ttu-id="18535-256">當您執行的 ASP.NET MVC 應用程式時，index （） 方法會是第一個控制器方法所呼叫。</span><span class="sxs-lookup"><span data-stu-id="18535-256">When you run an ASP.NET MVC application, the Index() method is the first controller method that is called.</span></span>

<span data-ttu-id="18535-257">我們將使用 index （） 方法，以顯示電影資料庫資料表中的記錄清單。</span><span class="sxs-lookup"><span data-stu-id="18535-257">We'll use the Index() method to display the list of records from the Movies database table.</span></span> <span data-ttu-id="18535-258">我們會利用資料庫擷取電影資料庫記錄，index （） 方法與稍早建立的模型類別。</span><span class="sxs-lookup"><span data-stu-id="18535-258">We'll take advantage of the database model classes that we created earlier to retrieve the movie database records with the Index() method.</span></span>

<span data-ttu-id="18535-259">我已修改 HomeController 類別，在 列表 2 中的，使其包含名為的新私用欄位\_db。</span><span class="sxs-lookup"><span data-stu-id="18535-259">I've modified the HomeController class in Listing 2 so that it contains a new private field named \_db.</span></span> <span data-ttu-id="18535-260">MoviesDBEntities 類別代表我們的資料庫模型，我們將使用此類別與資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="18535-260">The MoviesDBEntities class represents our database model and we'll use this class to communicate with our database.</span></span>

<span data-ttu-id="18535-261">我也已修改的列表 2 中的 index （） 方法。</span><span class="sxs-lookup"><span data-stu-id="18535-261">I've also modified the Index() method in Listing 2.</span></span> <span data-ttu-id="18535-262">Index （） 方法會使用 MoviesDBEntities 類別來擷取所有電影資料錄的電影資料庫資料表中。</span><span class="sxs-lookup"><span data-stu-id="18535-262">The Index() method uses the MoviesDBEntities class to retrieve all of the movie records from the Movies database table.</span></span> <span data-ttu-id="18535-263">運算式 *\_db。MovieSet.ToList()* 電影資料庫資料表中傳回所有電影資料錄的清單。</span><span class="sxs-lookup"><span data-stu-id="18535-263">The expression *\_db.MovieSet.ToList()* returns a list of all of the movie records from the Movies database table.</span></span>

<span data-ttu-id="18535-264">電影清單會傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="18535-264">The list of movies is passed to the view.</span></span> <span data-ttu-id="18535-265">取得傳遞至 View() 方法的任何項目取得傳遞至檢視，以檢視資料。</span><span class="sxs-lookup"><span data-stu-id="18535-265">Anything that gets passed to the View() method gets passed to the view as view data.</span></span>

<span data-ttu-id="18535-266">**列表 2 – controllers/Homecontroller.cs （已修改索引方法）**</span><span class="sxs-lookup"><span data-stu-id="18535-266">**Listing 2 – Controllers/HomeController.cs (modified Index method)**</span></span>

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

<span data-ttu-id="18535-267">Index （） 方法會傳回名稱為索引的檢視。</span><span class="sxs-lookup"><span data-stu-id="18535-267">The Index() method returns a view named Index.</span></span> <span data-ttu-id="18535-268">我們需要建立此檢視來顯示電影資料庫記錄的清單。</span><span class="sxs-lookup"><span data-stu-id="18535-268">We need to create this view to display the list of movie database records.</span></span> <span data-ttu-id="18535-269">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18535-269">Follow these steps:</span></span>


<span data-ttu-id="18535-270">您應該會建置您的專案 (選取功能表選項**建置時，建置方案**) 開啟之前**加入檢視**對話 」 或 「 無類別會出現在**檢視資料類別**下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="18535-270">You should build your project (select the menu option **Build, Build Solution**) before opening the **Add View** dialog or no classes will appear in the **View data class** dropdown list.</span></span>


1. <span data-ttu-id="18535-271">Index （） 方法，在程式碼編輯器上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱 圖 9）。</span><span class="sxs-lookup"><span data-stu-id="18535-271">Right-click the Index() method in the code editor and select the menu option **Add View** (see Figure 9).</span></span>
2. <span data-ttu-id="18535-272">在 新增檢視 對話方塊中，確認 核取方塊標示**建立強型別檢視**已核取。</span><span class="sxs-lookup"><span data-stu-id="18535-272">In the Add View dialog, verify that the checkbox labeled **Create a strongly-typed view** is checked.</span></span>
3. <span data-ttu-id="18535-273">從**檢視內容**下拉式清單中，選取值*清單*。</span><span class="sxs-lookup"><span data-stu-id="18535-273">From the **View content** dropdown list, select the value *List*.</span></span>
4. <span data-ttu-id="18535-274">從**檢視資料類別**下拉式清單中，選取值*MovieApp.Models.Movie*。</span><span class="sxs-lookup"><span data-stu-id="18535-274">From the **View data class** dropdown list, select the value *MovieApp.Models.Movie*.</span></span>
5. <span data-ttu-id="18535-275">按一下 新增 按鈕來建立新檢視 （請參閱 圖 10）。</span><span class="sxs-lookup"><span data-stu-id="18535-275">Click the Add button to create the new view (see Figure 10).</span></span>

<span data-ttu-id="18535-276">完成這些步驟之後，新的檢視，名為 Index.aspx 會加入 Views\Home 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="18535-276">After you complete these steps, a new view named Index.aspx is added to the Views\Home folder.</span></span> <span data-ttu-id="18535-277">索引檢視的內容會包含在 列表 3。</span><span class="sxs-lookup"><span data-stu-id="18535-277">The contents of the Index view are included in Listing 3.</span></span>


<span data-ttu-id="18535-278">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="18535-278">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)</span></span>

<span data-ttu-id="18535-279">**圖 09**： 從控制器動作中加入的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="18535-279">**Figure 09**: Adding a view from a controller action ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))</span></span>


<span data-ttu-id="18535-280">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="18535-280">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)</span></span>

<span data-ttu-id="18535-281">**圖 10**： 使用 [新增檢視] 對話方塊建立新的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="18535-281">**Figure 10**: Creating a new view with the Add View dialog ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))</span></span>


<span data-ttu-id="18535-282">**列表 3 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="18535-282">**Listing 3 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

<span data-ttu-id="18535-283">[索引] 檢視會顯示所有電影資料錄從 HTML 資料表中的電影資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="18535-283">The Index view displays all of the movie records from the Movies database table within an HTML table.</span></span> <span data-ttu-id="18535-284">此檢視包含的 foreach 迴圈，逐一查看 ViewData.Model 屬性所表示的每個影片。</span><span class="sxs-lookup"><span data-stu-id="18535-284">The view contains a foreach loop that iterates through each movie represented by the ViewData.Model property.</span></span> <span data-ttu-id="18535-285">如果您按下 F5 鍵執行應用程式，您會看到 [圖 11] 中的網頁。</span><span class="sxs-lookup"><span data-stu-id="18535-285">If you run your application by hitting the F5 key, then you'll see the web page in Figure 11.</span></span>


<span data-ttu-id="18535-286">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="18535-286">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)</span></span>

<span data-ttu-id="18535-287">**圖 11**: [索引] 檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))</span><span class="sxs-lookup"><span data-stu-id="18535-287">**Figure 11**: The Index view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))</span></span>


## <a name="creating-new-database-records"></a><span data-ttu-id="18535-288">建立新的資料庫資料錄</span><span class="sxs-lookup"><span data-stu-id="18535-288">Creating New Database Records</span></span>

<span data-ttu-id="18535-289">我們在上一節中建立的 [索引] 檢視包含用於建立新的資料庫記錄的連結。</span><span class="sxs-lookup"><span data-stu-id="18535-289">The Index view that we created in the previous section includes a link for creating new database records.</span></span> <span data-ttu-id="18535-290">我們繼續實作邏輯和建立檢視所需建立新的電影資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-290">Let's go ahead and implement the logic and create the view necessary for creating new movie database records.</span></span>

<span data-ttu-id="18535-291">主控制器包含兩個方法，名為 create （）。</span><span class="sxs-lookup"><span data-stu-id="18535-291">The Home controller contains two methods named Create().</span></span> <span data-ttu-id="18535-292">第一個 create （） 方法沒有任何參數。</span><span class="sxs-lookup"><span data-stu-id="18535-292">The first Create() method has no parameters.</span></span> <span data-ttu-id="18535-293">這個 create （） 方法的多載用來顯示的 HTML 表單建立新的電影資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-293">This overload of the Create() method is used to display the HTML form for creating a new movie database record.</span></span>

<span data-ttu-id="18535-294">第二個 create （） 方法具有 FormCollection 參數。</span><span class="sxs-lookup"><span data-stu-id="18535-294">The second Create() method has a FormCollection parameter.</span></span> <span data-ttu-id="18535-295">建立新的電影的 HTML 表單張貼至伺服器時，會呼叫這個 create （） 方法的多載。</span><span class="sxs-lookup"><span data-stu-id="18535-295">This overload of the Create() method is called when the HTML form for creating a new movie is posted to the server.</span></span> <span data-ttu-id="18535-296">請注意，此第二個 create （） 方法有 AcceptVerbs 屬性，除非執行 HTTP POST 作業呼叫時，避免該方法。</span><span class="sxs-lookup"><span data-stu-id="18535-296">Notice that this second Create() method has an AcceptVerbs attribute that prevents the method from being called unless an HTTP POST operation is performed.</span></span>

<span data-ttu-id="18535-297">更新 HomeController 類別中列表 4 中已修改這個第二個 create （） 方法。</span><span class="sxs-lookup"><span data-stu-id="18535-297">This second Create() method has been modified in the updated HomeController class in Listing 4.</span></span> <span data-ttu-id="18535-298">新版本的 create （） 方法會接受影片參數，並包含電影資料庫資料表中插入新的電影的邏輯。</span><span class="sxs-lookup"><span data-stu-id="18535-298">The new version of the Create() method accepts a Movie parameter and contains the logic for inserting a new movie into the Movies database table.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="18535-299">請注意繫結屬性。</span><span class="sxs-lookup"><span data-stu-id="18535-299">Notice the Bind attribute.</span></span> <span data-ttu-id="18535-300">因為我們不想要更新的影片識別碼屬性，從 HTML 表單，我們需要明確地排除這個屬性。</span><span class="sxs-lookup"><span data-stu-id="18535-300">Because we don't want to update the Movie Id property from HTML form, we need to explicitly exclude this property.</span></span>


<span data-ttu-id="18535-301">**列表 4 – Controllers\HomeController.cs （已修改的建立方法）**</span><span class="sxs-lookup"><span data-stu-id="18535-301">**Listing 4 – Controllers\HomeController.cs (modified Create method)**</span></span>

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

<span data-ttu-id="18535-302">Visual Studio 可讓您輕鬆地建立的表單建立新的電影資料庫記錄 （請參閱 圖 12）。</span><span class="sxs-lookup"><span data-stu-id="18535-302">Visual Studio makes it easy to create the form for creating a new movie database record (see Figure 12).</span></span> <span data-ttu-id="18535-303">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18535-303">Follow these steps:</span></span>

1. <span data-ttu-id="18535-304">以滑鼠右鍵按一下 create （） 方法，在程式碼編輯器，然後選取功能表選項**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="18535-304">Right-click the Create() method in the code editor and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="18535-305">確認核取方塊標示**建立強型別檢視**已核取。</span><span class="sxs-lookup"><span data-stu-id="18535-305">Verify that the checkbox labeled **Create a strongly-typed view** is checked.</span></span>
3. <span data-ttu-id="18535-306">從**檢視內容**下拉式清單中，選取值*建立*。</span><span class="sxs-lookup"><span data-stu-id="18535-306">From the **View content** dropdown list, select the value *Create*.</span></span>
4. <span data-ttu-id="18535-307">從**檢視資料類別**下拉式清單中，選取值*MovieApp.Models.Movie*。</span><span class="sxs-lookup"><span data-stu-id="18535-307">From the **View data class** dropdown list, select the value *MovieApp.Models.Movie*.</span></span>
5. <span data-ttu-id="18535-308">按一下 **新增**按鈕以建立新的檢視。</span><span class="sxs-lookup"><span data-stu-id="18535-308">Click the **Add** button to create the new view.</span></span>


<span data-ttu-id="18535-309">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="18535-309">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)</span></span>

<span data-ttu-id="18535-310">**圖 12**： 加入建立檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="18535-310">**Figure 12**: Adding the Create view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))</span></span>


<span data-ttu-id="18535-311">Visual Studio 的檢視表 5 中會自動產生。</span><span class="sxs-lookup"><span data-stu-id="18535-311">Visual Studio generates the view in Listing 5 automatically.</span></span> <span data-ttu-id="18535-312">這個檢視包含 HTML 表單，其中包含欄位對應至每個電影類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="18535-312">This view contains an HTML form that includes fields that correspond to each of the properties of the Movie class.</span></span>

<span data-ttu-id="18535-313">**列表 5 – Views\Home\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="18535-313">**Listing 5 – Views\Home\Create.aspx**</span></span>

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> <span data-ttu-id="18535-314">[新增檢視] 對話方塊產生的 HTML 格式產生 Id 的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="18535-314">The HTML form generated by the Add View dialog generates an Id form field.</span></span> <span data-ttu-id="18535-315">識別碼資料行是識別欄位，因為我們不需要此表單欄位，而且您可以安全地移除它。</span><span class="sxs-lookup"><span data-stu-id="18535-315">Because the Id column is an Identity column, we don't need this form field and you can safely remove it.</span></span>


<span data-ttu-id="18535-316">加入建立檢視之後，您可以新增影片記錄到資料庫。</span><span class="sxs-lookup"><span data-stu-id="18535-316">After you add the Create view, you can add new Movie records to the database.</span></span> <span data-ttu-id="18535-317">按下 F5 鍵執行應用程式，然後按一下 建立新的連結，以查看 圖 13 中的表單。</span><span class="sxs-lookup"><span data-stu-id="18535-317">Run your application by pressing the F5 key and click the Create New link to see the form in Figure 13.</span></span> <span data-ttu-id="18535-318">如果您完成，並送出表單時，會建立新的電影資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-318">If you complete and submit the form, a new movie database record is created.</span></span>

<span data-ttu-id="18535-319">請注意，您會自動取得表單驗證。</span><span class="sxs-lookup"><span data-stu-id="18535-319">Notice that you get form validation automatically.</span></span> <span data-ttu-id="18535-320">如果您沒有輸入影片，發行日期，或是您輸入無效的發行日期，然後會重新顯示表單，並反白顯示 [發行日期] 欄位。</span><span class="sxs-lookup"><span data-stu-id="18535-320">If you neglect to enter a release date for a movie, or you enter an invalid release date, then the form is redisplayed and the release date field is highlighted.</span></span>


<span data-ttu-id="18535-321">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="18535-321">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)</span></span>

<span data-ttu-id="18535-322">**圖 13**： 建立新的電影資料庫記錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="18535-322">**Figure 13**: Creating a new movie database record ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))</span></span>


## <a name="editing-existing-database-records"></a><span data-ttu-id="18535-323">編輯現有的資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="18535-323">Editing Existing Database Records</span></span>

<span data-ttu-id="18535-324">在先前章節中，我們會討論如何列出，並建立新的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-324">In the previous sections, we discussed how you can list and create new database records.</span></span> <span data-ttu-id="18535-325">在最後一節中，我們會討論如何編輯現有的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-325">In this final section, we discuss how you can edit existing database records.</span></span>

<span data-ttu-id="18535-326">首先，我們需要產生的編輯表單。</span><span class="sxs-lookup"><span data-stu-id="18535-326">First, we need to generate the Edit form.</span></span> <span data-ttu-id="18535-327">這個步驟很容易，因為 Visual Studio 會產生的編輯表單為我們自動。</span><span class="sxs-lookup"><span data-stu-id="18535-327">This step is easy since Visual Studio will generate the Edit form for us automatically.</span></span> <span data-ttu-id="18535-328">Visual Studio 程式碼編輯器中開啟 HomeController.cs 類別，並遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18535-328">Open the HomeController.cs class in the Visual Studio code editor and follow these steps:</span></span>

1. <span data-ttu-id="18535-329">以滑鼠右鍵按一下 edit （） 方法，在程式碼編輯器，然後選取功能表選項**加入檢視**（請參閱 圖 14）。</span><span class="sxs-lookup"><span data-stu-id="18535-329">Right-click the Edit() method in the code editor and select the menu option **Add View** (see Figure 14).</span></span>
2. <span data-ttu-id="18535-330">選取標示的核取方塊**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="18535-330">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
3. <span data-ttu-id="18535-331">從**檢視內容**下拉式清單中，選取值*編輯*。</span><span class="sxs-lookup"><span data-stu-id="18535-331">From the **View content** dropdown list, select the value *Edit*.</span></span>
4. <span data-ttu-id="18535-332">從**檢視資料類別**下拉式清單中，選取值*MovieApp.Models.Movie*。</span><span class="sxs-lookup"><span data-stu-id="18535-332">From the **View data class** dropdown list, select the value *MovieApp.Models.Movie*.</span></span>
5. <span data-ttu-id="18535-333">按一下 **新增**按鈕以建立新的檢視。</span><span class="sxs-lookup"><span data-stu-id="18535-333">Click the **Add** button to create the new view.</span></span>

<span data-ttu-id="18535-334">完成這些步驟，新增名為 Edit.aspx Views\Home 資料夾檢視。</span><span class="sxs-lookup"><span data-stu-id="18535-334">Completing these steps adds a new view named Edit.aspx to the Views\Home folder.</span></span> <span data-ttu-id="18535-335">這個檢視包含編輯電影錄製之 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="18535-335">This view contains an HTML form for editing a movie record.</span></span>


<span data-ttu-id="18535-336">[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="18535-336">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)</span></span>

<span data-ttu-id="18535-337">**圖 14**： 加入編輯檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))</span><span class="sxs-lookup"><span data-stu-id="18535-337">**Figure 14**: Adding the Edit view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="18535-338">[編輯] 檢視包含 HTML 表單欄位對應到影片識別碼屬性。</span><span class="sxs-lookup"><span data-stu-id="18535-338">The Edit view contains an HTML form field that corresponds to the Movie Id property.</span></span> <span data-ttu-id="18535-339">您不想讓使用者編輯 Id 屬性的值，您應該移除此表單欄位。</span><span class="sxs-lookup"><span data-stu-id="18535-339">Because you don't want people editing the value of the Id property, you should remove this form field.</span></span>


<span data-ttu-id="18535-340">最後，我們需要修改主控制器，讓它支援 編輯資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="18535-340">Finally, we need to modify the Home controller so that it supports editing a database record.</span></span> <span data-ttu-id="18535-341">更新的 HomeController 類別都包含在 列表 6。</span><span class="sxs-lookup"><span data-stu-id="18535-341">The updated HomeController class is contained in Listing 6.</span></span>

<span data-ttu-id="18535-342">**列表 6 – Controllers\HomeController.cs （編輯方法）**</span><span class="sxs-lookup"><span data-stu-id="18535-342">**Listing 6 – Controllers\HomeController.cs (Edit methods)**</span></span>

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

<span data-ttu-id="18535-343">在 列表 6，我新增額外的邏輯 edit （） 方法的兩個多載。</span><span class="sxs-lookup"><span data-stu-id="18535-343">In Listing 6, I've added additional logic to both overloads of the Edit() method.</span></span> <span data-ttu-id="18535-344">第一種 edit （） 方法傳回的電影資料庫記錄，其對應至傳遞給方法的 Id 參數。</span><span class="sxs-lookup"><span data-stu-id="18535-344">The first Edit() method returns the movie database record that corresponds to the Id parameter passed to the method.</span></span> <span data-ttu-id="18535-345">第二個多載會執行到電影的記錄更新，在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="18535-345">The second overload performs the updates to a movie record in the database.</span></span>

<span data-ttu-id="18535-346">請注意，您必須擷取原始的影片，然後呼叫 ApplyPropertyChanges()，更新資料庫中現有的影片。</span><span class="sxs-lookup"><span data-stu-id="18535-346">Notice that you must retrieve the original movie, and then call ApplyPropertyChanges(), to update the existing movie in the database.</span></span>

## <a name="summary"></a><span data-ttu-id="18535-347">總結</span><span class="sxs-lookup"><span data-stu-id="18535-347">Summary</span></span>

<span data-ttu-id="18535-348">本教學課程的目的是體驗的要給您的建置 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="18535-348">The purpose of this tutorial was to give you a sense of the experience of building an ASP.NET MVC application.</span></span> <span data-ttu-id="18535-349">我希望您會發現建置 ASP.NET MVC web 應用程式是非常類似於建置動態伺服器網頁或 ASP.NET 應用程式的體驗。</span><span class="sxs-lookup"><span data-stu-id="18535-349">I hope that you discovered that building an ASP.NET MVC web application is very similar to the experience of building an Active Server Pages or ASP.NET application.</span></span>

<span data-ttu-id="18535-350">在本教學課程中，我們會檢查 ASP.NET MVC 架構的最基本功能。</span><span class="sxs-lookup"><span data-stu-id="18535-350">In this tutorial, we examined only the most basic features of the ASP.NET MVC framework.</span></span> <span data-ttu-id="18535-351">在未來的教學課程中，我們更深入地探討的主題，例如控制器、 控制器動作、 檢視、 檢視資料和 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="18535-351">In future tutorials, we dive deeper into topics such as controllers, controller actions, views, view data, and HTML helpers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="18535-352">下一步</span><span class="sxs-lookup"><span data-stu-id="18535-352">Next</span></span>](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
