---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: 建立電影資料庫應用程式在 15 分鐘內，搭配 ASP.NET MVC (VB) |Microsoft 文件
author: StephenWalther
description: 作者： Stephen Walther 建置整個資料庫驅動 ASP.NET MVC 應用程式從開始到完成。 本教學課程是不錯的介紹人士新 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: ecd5892457af5bc14a939672c64eed85fc05ec22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a><span data-ttu-id="22013-104">建立電影資料庫應用程式在 15 分鐘內，搭配 ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="22013-104">Create a Movie Database Application in 15 Minutes with ASP.NET MVC (VB)</span></span>
====================
<span data-ttu-id="22013-105">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="22013-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="22013-106">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="22013-106">Download Code</span></span>](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> <span data-ttu-id="22013-107">作者： Stephen Walther 建置整個資料庫驅動 ASP.NET MVC 應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="22013-107">Stephen Walther builds an entire database-driven ASP.NET MVC application from start to finish.</span></span> <span data-ttu-id="22013-108">本教學課程是不錯的介紹人熟悉 ASP.NET MVC Framework，並想要了解建立 ASP.NET MVC 應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="22013-108">This tutorial is a great introduction for people who are new to the ASP.NET MVC Framework and who want to get a sense of the process of building an ASP.NET MVC application.</span></span>


<span data-ttu-id="22013-109">本教學課程的目的是要讓您了解它是什麼像 」 來建立 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="22013-109">The purpose of this tutorial is to give you a sense of "what it is like" to build an ASP.NET MVC application.</span></span> <span data-ttu-id="22013-110">在本教學課程中，我炸毀透過建置整個 ASP.NET MVC 應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="22013-110">In this tutorial, I blast through building an entire ASP.NET MVC application from start to finish.</span></span> <span data-ttu-id="22013-111">我將示範您如何建置簡單資料庫驅動的應用程式如何清單、 建立和編輯資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="22013-111">I show you how to build a simple database-driven application that illustrates how you can list, create, and edit database records.</span></span>

<span data-ttu-id="22013-112">若要簡化建立我們的應用程式的程序，我們 Visual Studio 2008 的 scaffolding 的功能。</span><span class="sxs-lookup"><span data-stu-id="22013-112">To simplify the process of building our application, we'll take advantage of the scaffolding features of Visual Studio 2008.</span></span> <span data-ttu-id="22013-113">我們會讓 Visual Studio 產生的初始程式碼和我們的控制器、 模型和檢視表的內容。</span><span class="sxs-lookup"><span data-stu-id="22013-113">We'll let Visual Studio generate the initial code and content for our controllers, models, and views.</span></span>

<span data-ttu-id="22013-114">如果您已使用 Active Server Pages 或 ASP.NET，然後您應該會發現 ASP.NET MVC 非常熟悉。</span><span class="sxs-lookup"><span data-stu-id="22013-114">If you have worked with Active Server Pages or ASP.NET, then you should find ASP.NET MVC very familiar.</span></span> <span data-ttu-id="22013-115">ASP.NET MVC 檢視很像是 Active Server Pages 應用程式中的頁面。</span><span class="sxs-lookup"><span data-stu-id="22013-115">ASP.NET MVC views are very much like the pages in an Active Server Pages application.</span></span> <span data-ttu-id="22013-116">此外，就像傳統的 ASP.NET Web Form 應用程式中，ASP.NET MVC 為您提供的完整存取權的一組豐富的語言和.NET framework 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="22013-116">And, just like a traditional ASP.NET Web Forms application, ASP.NET MVC provides you with full access to the rich set of languages and classes provided by the .NET framework.</span></span>

<span data-ttu-id="22013-117">我希望是本教學課程會提供了解如何建立 ASP.NET MVC 應用程式的經驗是類似，不同於建置 Active Server Pages 或 ASP.NET Web Form 應用程式的體驗。</span><span class="sxs-lookup"><span data-stu-id="22013-117">My hope is that this tutorial will give you a sense of how the experience of building an ASP.NET MVC application is both similar and different than the experience of building an Active Server Pages or ASP.NET Web Forms application.</span></span>

## <a name="overview-of-the-movie-database-application"></a><span data-ttu-id="22013-118">電影資料庫應用程式的概觀</span><span class="sxs-lookup"><span data-stu-id="22013-118">Overview of the Movie Database Application</span></span>

<span data-ttu-id="22013-119">因為我們的目標是為了簡單起見，我們將建置簡單的電影資料庫應用程式。</span><span class="sxs-lookup"><span data-stu-id="22013-119">Because our goal is to keep things simple, we'll build a very simple Movie Database application.</span></span> <span data-ttu-id="22013-120">簡單的電影資料庫應用程式可讓我們進行三項作業：</span><span class="sxs-lookup"><span data-stu-id="22013-120">Our simple Movie Database application will allow us to do three things:</span></span>

1. <span data-ttu-id="22013-121">列出一組影片資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="22013-121">List a set of movie database records</span></span>
2. <span data-ttu-id="22013-122">建立新的電影資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="22013-122">Create a new movie database record</span></span>
3. <span data-ttu-id="22013-123">編輯現有的電影資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="22013-123">Edit an existing movie database record</span></span>

<span data-ttu-id="22013-124">同樣地，由於我們想要讓事情變簡單，我們將利用 ASP.NET MVC 架構建置應用程式所需功能的最小數目。</span><span class="sxs-lookup"><span data-stu-id="22013-124">Again, because we want to keep things simple, we'll take advantage of the minimum number of features of the ASP.NET MVC framework needed to build our application.</span></span> <span data-ttu-id="22013-125">例如，我們將不會利用有如開發。</span><span class="sxs-lookup"><span data-stu-id="22013-125">For example, we won't be taking advantage of Test-Driven Development.</span></span>

<span data-ttu-id="22013-126">若要建立應用程式，我們需要完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="22013-126">In order to create our application, we need to complete each of the following steps:</span></span>

1. <span data-ttu-id="22013-127">建立 ASP.NET MVC Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="22013-127">Create the ASP.NET MVC Web Application Project</span></span>
2. <span data-ttu-id="22013-128">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="22013-128">Create the database</span></span>
3. <span data-ttu-id="22013-129">建立資料庫模型</span><span class="sxs-lookup"><span data-stu-id="22013-129">Create the database model</span></span>
4. <span data-ttu-id="22013-130">建立 ASP.NET MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="22013-130">Create the ASP.NET MVC controller</span></span>
5. <span data-ttu-id="22013-131">建立 ASP.NET MVC 檢視</span><span class="sxs-lookup"><span data-stu-id="22013-131">Create the ASP.NET MVC views</span></span>

## <a name="preliminaries"></a><span data-ttu-id="22013-132">準備工作</span><span class="sxs-lookup"><span data-stu-id="22013-132">Preliminaries</span></span>

<span data-ttu-id="22013-133">您將需要 Visual Studio 2008 或 Visual Web Developer 2008 Express 建置 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="22013-133">You'll need either Visual Studio 2008 or Visual Web Developer 2008 Express to build an ASP.NET MVC application.</span></span> <span data-ttu-id="22013-134">您也需要下載 ASP.NET MVC framework。</span><span class="sxs-lookup"><span data-stu-id="22013-134">You also need to download the ASP.NET MVC framework.</span></span>

<span data-ttu-id="22013-135">如果您沒有 Visual Studio 2008，您可以從此網站下載 Visual Studio 2008 的 90 天試用的版：</span><span class="sxs-lookup"><span data-stu-id="22013-135">If you don't own Visual Studio 2008, then you can download a 90 day trial version of Visual Studio 2008 from this website:</span></span>

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

<span data-ttu-id="22013-136">或者，您可以建立 ASP.NET MVC 應用程式與 Visual Web Developer Express 2008。</span><span class="sxs-lookup"><span data-stu-id="22013-136">Alternatively, you can create ASP.NET MVC applications with Visual Web Developer Express 2008.</span></span> <span data-ttu-id="22013-137">如果您決定要使用 Visual Web Developer Express，您必須已安裝 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="22013-137">If you decide to use Visual Web Developer Express then you must have Service Pack 1 installed.</span></span> <span data-ttu-id="22013-138">您可以從此網站下載 Visual Web Developer 2008 Express Service Pack 1:</span><span class="sxs-lookup"><span data-stu-id="22013-138">You can download Visual Web Developer 2008 Express with Service Pack 1 from this website:</span></span>

[<span data-ttu-id="22013-139">https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en</span><span class="sxs-lookup"><span data-stu-id="22013-139">https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

<span data-ttu-id="22013-140">安裝 Visual Studio 2008 或 Visual Web Developer 2008 之後，您需要安裝 ASP.NET MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="22013-140">After you install either Visual Studio 2008 or Visual Web Developer 2008, you need to install the ASP.NET MVC framework.</span></span> <span data-ttu-id="22013-141">您可以從下列網站下載 ASP.NET MVC 架構：</span><span class="sxs-lookup"><span data-stu-id="22013-141">You can download the ASP.NET MVC framework from the following website:</span></span>

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> <span data-ttu-id="22013-142">而不是個別下載 ASP.NET framework 和 ASP.NET MVC 架構，您可以利用 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="22013-142">Instead of downloading the ASP.NET framework and the ASP.NET MVC framework individually, you can take advantage of the Web Platform Installer.</span></span> <span data-ttu-id="22013-143">Web Platform Installer 是可讓您輕鬆地管理已安裝的應用程式的應用程式是您的電腦：</span><span class="sxs-lookup"><span data-stu-id="22013-143">The Web Platform Installer is an application that enables you to easily manage the installed applications are your computer:</span></span>
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a><span data-ttu-id="22013-144">建立 ASP.NET MVC Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="22013-144">Creating an ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="22013-145">讓我們開始在 Visual Studio 2008 中建立新的 ASP.NET MVC Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="22013-145">Let's start by creating a new ASP.NET MVC Web Application project in Visual Studio 2008.</span></span> <span data-ttu-id="22013-146">選取功能表選項**檔案、 新的專案**，您會看到在圖 1 中新增專案 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="22013-146">Select the menu option **File, New Project** and you will see the New Project dialog box in Figure 1.</span></span> <span data-ttu-id="22013-147">選取 Visual Basic 程式設計語言，然後選取 ASP.NET MVC Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="22013-147">Select Visual Basic as the programming language and select the ASP.NET MVC Web Application project template.</span></span> <span data-ttu-id="22013-148">為您的專案名稱 MovieApp，然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="22013-148">Give your project the name MovieApp and click the OK button.</span></span>


<span data-ttu-id="22013-149">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="22013-149">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)</span></span>

<span data-ttu-id="22013-150">**圖 01**: 新的專案 對話方塊 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="22013-150">**Figure 01**: The New Project dialog box ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))</span></span>


<span data-ttu-id="22013-151">請確定您從 新增專案 對話方塊頂端的下拉式清單中選取 .NET Framework 3.5 或 ASP.NET MVC Web 應用程式專案範本就不會出現。</span><span class="sxs-lookup"><span data-stu-id="22013-151">Make sure that you select .NET Framework 3.5 from the dropdown list at the top of the New Project dialog or the ASP.NET MVC Web Application project template won't appear.</span></span>


<span data-ttu-id="22013-152">每當您建立新的 MVC Web 應用程式專案，Visual Studio 會提示您建立個別的單元測試專案。</span><span class="sxs-lookup"><span data-stu-id="22013-152">Whenever you create a new MVC Web Application project, Visual Studio prompts you to create a separate unit test project.</span></span> <span data-ttu-id="22013-153">圖 2 中的對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="22013-153">The dialog in Figure 2 appears.</span></span> <span data-ttu-id="22013-154">因為我們不會建立測試本教學課程中因為時間條件約束 （甚至是，我們應該覺得有點連載有關此） 選取**否**選項，然後按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="22013-154">Because we won't be creating tests in this tutorial because of time constraints (and, yes, we should feel a little guilty about this) select the **No** option and click the **OK** button.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="22013-155">Visual Web Developer 不支援測試專案。</span><span class="sxs-lookup"><span data-stu-id="22013-155">Visual Web Developer does not support test projects.</span></span>


<span data-ttu-id="22013-156">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="22013-156">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)</span></span>

<span data-ttu-id="22013-157">**圖 02**: 建立單元測試專案 對話方塊 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="22013-157">**Figure 02**: The Create Unit Test Project dialog ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))</span></span>


<span data-ttu-id="22013-158">ASP.NET MVC 應用程式有一組標準的資料夾： 模型、 檢視和控制器的資料夾。</span><span class="sxs-lookup"><span data-stu-id="22013-158">An ASP.NET MVC application has a standard set of folders: a Models, Views, and Controllers folder.</span></span> <span data-ttu-id="22013-159">您可以查看這組標準的 [方案總管] 視窗中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="22013-159">You can see this standard set of folders in the Solution Explorer window.</span></span> <span data-ttu-id="22013-160">我們需要將檔案新增至每個模型、 檢視和控制器的資料夾，才能建立電影資料庫應用程式。</span><span class="sxs-lookup"><span data-stu-id="22013-160">We'll need to add files to each of the Models, Views, and Controllers folders in order to build our Movie Database application.</span></span>

<span data-ttu-id="22013-161">當您使用 Visual Studio 建立新的 MVC 應用程式時，您會取得範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="22013-161">When you create a new MVC application with Visual Studio, you get a sample application.</span></span> <span data-ttu-id="22013-162">由於我們想要從頭開始，我們需要刪除此範例應用程式的內容。</span><span class="sxs-lookup"><span data-stu-id="22013-162">Because we want to start from scratch, we need to delete the content for this sample application.</span></span> <span data-ttu-id="22013-163">您必須刪除下列檔案和下列資料夾：</span><span class="sxs-lookup"><span data-stu-id="22013-163">You need to delete the following file and the following folder:</span></span>

- <span data-ttu-id="22013-164">Controllers\HomeController.vb</span><span class="sxs-lookup"><span data-stu-id="22013-164">Controllers\HomeController.vb</span></span>
- <span data-ttu-id="22013-165">Views\Home</span><span class="sxs-lookup"><span data-stu-id="22013-165">Views\Home</span></span>

## <a name="creating-the-database"></a><span data-ttu-id="22013-166">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="22013-166">Creating the Database</span></span>

<span data-ttu-id="22013-167">我們需要建立資料庫來保存我們影片的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="22013-167">We need to create a database to hold our movie database records.</span></span> <span data-ttu-id="22013-168">幸運的是，Visual Studio 會包含名為 SQL Server Express 免費的資料庫。</span><span class="sxs-lookup"><span data-stu-id="22013-168">Luckily, Visual Studio includes a free database named SQL Server Express.</span></span> <span data-ttu-id="22013-169">請遵循下列步驟來建立資料庫：</span><span class="sxs-lookup"><span data-stu-id="22013-169">Follow these steps to create the database:</span></span>

1. <span data-ttu-id="22013-170">以滑鼠右鍵按一下應用程式\_Data 資料夾中的方案總管 視窗，然後選取功能表選項**新增、 新項目**。</span><span class="sxs-lookup"><span data-stu-id="22013-170">Right-click the App\_Data folder in the Solution Explorer window and select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="22013-171">選取**資料**類別目錄，然後選取**SQL Server 資料庫**範本 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="22013-171">Select the **Data** category and select the **SQL Server Database** template (see Figure 3).</span></span>
3. <span data-ttu-id="22013-172">將您的新資料庫命名*MoviesDB.mdf*按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="22013-172">Name your new database *MoviesDB.mdf* and click the **Add** button.</span></span>

<span data-ttu-id="22013-173">建立資料庫之後，您可以按兩下 MoviesDB.mdf 檔案位於 應用程式連接到資料庫\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="22013-173">After you create your database, you can connect to the database by double-clicking the MoviesDB.mdf file located in the App\_Data folder.</span></span> <span data-ttu-id="22013-174">按兩下 MoviesDB.mdf 檔案會開啟 [伺服器總管] 視窗。</span><span class="sxs-lookup"><span data-stu-id="22013-174">Double-clicking the MoviesDB.mdf file opens the Server Explorer window.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="22013-175">在 Visual Web Developer 的情況下的 [資料庫總管] 視窗名為 [伺服器總管] 視窗。</span><span class="sxs-lookup"><span data-stu-id="22013-175">The Server Explorer window is named the Database Explorer window in the case of Visual Web Developer.</span></span>


<span data-ttu-id="22013-176">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="22013-176">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)</span></span>

<span data-ttu-id="22013-177">**圖 03**： 建立 Microsoft SQL Server 資料庫 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="22013-177">**Figure 03**: Creating a Microsoft SQL Server Database ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))</span></span>


<span data-ttu-id="22013-178">接下來，我們需要建立新的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="22013-178">Next, we need to create a new database table.</span></span> <span data-ttu-id="22013-179">從 [伺服器總管] 視窗中以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="22013-179">From within the Sever Explorer window, right-click the Tables folder and select the menu option **Add New Table**.</span></span> <span data-ttu-id="22013-180">選取此功能表選項會開啟資料庫資料表設計工具。</span><span class="sxs-lookup"><span data-stu-id="22013-180">Selecting this menu option opens the database table designer.</span></span> <span data-ttu-id="22013-181">建立下列的資料庫資料行：</span><span class="sxs-lookup"><span data-stu-id="22013-181">Create the following database columns:</span></span>

<a id="0.2_table01"></a>


| <span data-ttu-id="22013-182">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="22013-182">**Column Name**</span></span> | <span data-ttu-id="22013-183">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="22013-183">**Data Type**</span></span> | <span data-ttu-id="22013-184">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="22013-184">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="22013-185">ID</span><span class="sxs-lookup"><span data-stu-id="22013-185">Id</span></span> | <span data-ttu-id="22013-186">Int</span><span class="sxs-lookup"><span data-stu-id="22013-186">Int</span></span> | <span data-ttu-id="22013-187">False</span><span class="sxs-lookup"><span data-stu-id="22013-187">False</span></span> |
| <span data-ttu-id="22013-188">標題</span><span class="sxs-lookup"><span data-stu-id="22013-188">Title</span></span> | <span data-ttu-id="22013-189">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="22013-189">Nvarchar(100)</span></span> | <span data-ttu-id="22013-190">False</span><span class="sxs-lookup"><span data-stu-id="22013-190">False</span></span> |
| <span data-ttu-id="22013-191">導向器</span><span class="sxs-lookup"><span data-stu-id="22013-191">Director</span></span> | <span data-ttu-id="22013-192">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="22013-192">Nvarchar(100)</span></span> | <span data-ttu-id="22013-193">False</span><span class="sxs-lookup"><span data-stu-id="22013-193">False</span></span> |
| <span data-ttu-id="22013-194">DateReleased</span><span class="sxs-lookup"><span data-stu-id="22013-194">DateReleased</span></span> | <span data-ttu-id="22013-195">DateTime</span><span class="sxs-lookup"><span data-stu-id="22013-195">DateTime</span></span> | <span data-ttu-id="22013-196">False</span><span class="sxs-lookup"><span data-stu-id="22013-196">False</span></span> |


<span data-ttu-id="22013-197">第一個資料行的識別碼資料行中，有兩個特殊的屬性。</span><span class="sxs-lookup"><span data-stu-id="22013-197">The first column, the Id column, has two special properties.</span></span> <span data-ttu-id="22013-198">首先，您需要識別碼資料行標示為主要索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="22013-198">First, you need to mark the Id column as the primary key column.</span></span> <span data-ttu-id="22013-199">選取 [Id] 資料行之後, 按**設定主索引鍵**（它是機碼看起來的圖示） 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="22013-199">After selecting the Id column, click the **Set Primary Key** button (it is the icon that looks like a key).</span></span> <span data-ttu-id="22013-200">第二，您需要識別碼資料行標示為識別資料行。</span><span class="sxs-lookup"><span data-stu-id="22013-200">Second, you need to mark the Id column as an Identity column.</span></span> <span data-ttu-id="22013-201">在 [資料行屬性] 視窗中向下捲動至 [識別規格] 區段，然後展開它。</span><span class="sxs-lookup"><span data-stu-id="22013-201">In the Column Properties window, scroll down to the Identity Specification section and expand it.</span></span> <span data-ttu-id="22013-202">變更**為識別**屬性設為值**是**。</span><span class="sxs-lookup"><span data-stu-id="22013-202">Change the **Is Identity** property to the value **Yes**.</span></span> <span data-ttu-id="22013-203">當您完成時，資料表看起來應該像圖 4。</span><span class="sxs-lookup"><span data-stu-id="22013-203">When you are finished, the table should look like Figure 4.</span></span>


<span data-ttu-id="22013-204">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="22013-204">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)</span></span>

<span data-ttu-id="22013-205">**圖 04**: 電影資料庫資料表 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="22013-205">**Figure 04**: The Movies database table ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))</span></span>


<span data-ttu-id="22013-206">最後一個步驟是要儲存新的資料表。</span><span class="sxs-lookup"><span data-stu-id="22013-206">The final step is to save the new table.</span></span> <span data-ttu-id="22013-207">按一下 [儲存] 按鈕 （磁片圖示），並提供新的資料表名稱電影。</span><span class="sxs-lookup"><span data-stu-id="22013-207">Click the Save button (the icon of the floppy) and give the new table the name Movies.</span></span>

<span data-ttu-id="22013-208">完成 建立資料表之後，某些影片將記錄新增至資料表。</span><span class="sxs-lookup"><span data-stu-id="22013-208">After you finish creating the table, add some movie records to the table.</span></span> <span data-ttu-id="22013-209">以滑鼠右鍵按一下 [伺服器總管] 視窗中的電影資料表，然後選取功能表選項**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="22013-209">Right-click the Movies table in the Server Explorer window and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="22013-210">輸入 （請參閱圖 5） 您最愛的電影清單。</span><span class="sxs-lookup"><span data-stu-id="22013-210">Enter a list of your favorite movies (see Figure 5).</span></span>


<span data-ttu-id="22013-211">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="22013-211">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)</span></span>

<span data-ttu-id="22013-212">**圖 05**： 輸入電影的記錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="22013-212">**Figure 05**: Entering movie records ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))</span></span>


## <a name="creating-the-model"></a><span data-ttu-id="22013-213">建立模型</span><span class="sxs-lookup"><span data-stu-id="22013-213">Creating the Model</span></span>

<span data-ttu-id="22013-214">接下來，我們必須建立一組類別來代表我們的資料庫。</span><span class="sxs-lookup"><span data-stu-id="22013-214">We next need to create a set of classes to represent our database.</span></span> <span data-ttu-id="22013-215">我們需要建立資料庫模型。</span><span class="sxs-lookup"><span data-stu-id="22013-215">We need to create a database model.</span></span> <span data-ttu-id="22013-216">我們會利用 Microsoft Entity Framework 自動產生的類別為我們的資料庫模型。</span><span class="sxs-lookup"><span data-stu-id="22013-216">We'll take advantage of the Microsoft Entity Framework to generate the classes for our database model automatically.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="22013-217">ASP.NET MVC 架構未繫結至 Microsoft 的 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="22013-217">The ASP.NET MVC framework is not tied to the Microsoft Entity Framework.</span></span> <span data-ttu-id="22013-218">您可以建立您的資料庫模型類別藉由運用各種不同的物件關聯式對應 (或者 / M) 工具，包括 LINQ to SQL、 Subsonic 和 NHibernate。</span><span class="sxs-lookup"><span data-stu-id="22013-218">You can create your database model classes by taking advantage of a variety of Object Relational Mapping (OR/M) tools including LINQ to SQL, Subsonic, and NHibernate.</span></span>


<span data-ttu-id="22013-219">請遵循下列步驟來啟動實體資料模型精靈：</span><span class="sxs-lookup"><span data-stu-id="22013-219">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="22013-220">以滑鼠右鍵按一下 [模型] 資料夾，在 [方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。</span><span class="sxs-lookup"><span data-stu-id="22013-220">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="22013-221">選取**資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。</span><span class="sxs-lookup"><span data-stu-id="22013-221">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="22013-222">將您的資料模型命名*MoviesDBModel.edmx*按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="22013-222">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="22013-223">按一下 [新增] 按鈕之後，實體資料模型精靈就會出現 （請參閱圖 6）。</span><span class="sxs-lookup"><span data-stu-id="22013-223">After you click the Add button, the Entity Data Model Wizard appears (see Figure 6).</span></span> <span data-ttu-id="22013-224">請遵循下列步驟來完成精靈：</span><span class="sxs-lookup"><span data-stu-id="22013-224">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="22013-225">在**選擇模型內容**步驟中，選取**從資料庫產生**選項。</span><span class="sxs-lookup"><span data-stu-id="22013-225">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="22013-226">在**選擇資料連接**步驟，請使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連接設定。</span><span class="sxs-lookup"><span data-stu-id="22013-226">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="22013-227">按一下**下一步** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="22013-227">Click the **Next** button.</span></span>
3. <span data-ttu-id="22013-228">在**選擇您的資料庫物件**步驟中，展開資料表節點，選取 電影資料表。</span><span class="sxs-lookup"><span data-stu-id="22013-228">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="22013-229">輸入的命名空間*MovieApp.Models*按一下**完成** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="22013-229">Enter the namespace *MovieApp.Models* and click the **Finish** button.</span></span>


<span data-ttu-id="22013-230">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="22013-230">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)</span></span>

<span data-ttu-id="22013-231">**圖 06**： 產生的資料庫模型，使用實體資料模型精靈 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="22013-231">**Figure 06**: Generating a database model with the Entity Data Model Wizard ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))</span></span>


<span data-ttu-id="22013-232">實體資料模型精靈完成之後，Entity Data Model Designer 隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="22013-232">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="22013-233">在設計工具應該會顯示電影資料庫資料表 （請參閱圖 7）。</span><span class="sxs-lookup"><span data-stu-id="22013-233">The Designer should display the Movies database table (see Figure 7).</span></span>


<span data-ttu-id="22013-234">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="22013-234">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)</span></span>

<span data-ttu-id="22013-235">**圖 07**: Entity Data Model Designer ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="22013-235">**Figure 07**: The Entity Data Model Designer ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))</span></span>


<span data-ttu-id="22013-236">我們需要進行一項變更，我們繼續執行。</span><span class="sxs-lookup"><span data-stu-id="22013-236">We need to make one change before we continue.</span></span> <span data-ttu-id="22013-237">實體資料精靈會產生代表電影資料庫資料表的模型類別命名為電影。</span><span class="sxs-lookup"><span data-stu-id="22013-237">The Entity Data Wizard generates a model class named Movies that represents the Movies database table.</span></span> <span data-ttu-id="22013-238">由於我們將使用電影類別來代表特定的影片，我們需要修改的類別的類別名稱*影片*而不是*電影*（單數而複數）。</span><span class="sxs-lookup"><span data-stu-id="22013-238">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="22013-239">按兩下設計工具介面上的類別名稱，並將電影從類別的名稱，變更 電影。</span><span class="sxs-lookup"><span data-stu-id="22013-239">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="22013-240">進行這項變更之後, 按**儲存**按鈕 （磁片圖示），以產生影片類別。</span><span class="sxs-lookup"><span data-stu-id="22013-240">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="creating-the-aspnet-mvc-controller"></a><span data-ttu-id="22013-241">建立 ASP.NET MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="22013-241">Creating the ASP.NET MVC Controller</span></span>

<span data-ttu-id="22013-242">下一個步驟是建立 ASP.NET MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="22013-242">The next step is to create the ASP.NET MVC controller.</span></span> <span data-ttu-id="22013-243">控制器負責控制使用者如何與 ASP.NET MVC 應用程式互動。</span><span class="sxs-lookup"><span data-stu-id="22013-243">A controller is responsible for controlling how a user interacts with an ASP.NET MVC application.</span></span>

<span data-ttu-id="22013-244">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="22013-244">Follow these steps:</span></span>

1. <span data-ttu-id="22013-245">在 [方案總管] 視窗中，以滑鼠右鍵按一下 [控制器] 資料夾並選取功能表選項**新增]、 [控制站**。</span><span class="sxs-lookup"><span data-stu-id="22013-245">In the Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller**.</span></span>
2. <span data-ttu-id="22013-246">在 [加入控制器] 對話方塊中，輸入名稱*HomeController*並標示核取**將動作方法，如 Create、 Update 和 Details 案例加入**（請參閱圖 8）。</span><span class="sxs-lookup"><span data-stu-id="22013-246">In the Add Controller dialog, enter the name *HomeController* and check the checkbox labeled **Add action methods for Create, Update, and Details scenarios** (see Figure 8).</span></span>
3. <span data-ttu-id="22013-247">按一下**新增** 按鈕，將新的控制站新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="22013-247">Click the **Add** button to add the new controller to your project.</span></span>

<span data-ttu-id="22013-248">完成這些步驟之後，會建立程式碼範例 1 中的控制站。</span><span class="sxs-lookup"><span data-stu-id="22013-248">After you complete these steps, the controller in Listing 1 is created.</span></span> <span data-ttu-id="22013-249">請注意，它包含方法，名為索引的詳細資訊，建立，並編輯。</span><span class="sxs-lookup"><span data-stu-id="22013-249">Notice that it contains methods named Index, Details, Create, and Edit.</span></span> <span data-ttu-id="22013-250">在下列章節中，我們會將新增所需的程式碼，讓這些方法才能運作。</span><span class="sxs-lookup"><span data-stu-id="22013-250">In the following sections, we'll add the necessary code to get these methods to work.</span></span>


<span data-ttu-id="22013-251">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="22013-251">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)</span></span>

<span data-ttu-id="22013-252">**圖 08**： 加入新的 ASP.NET MVC 控制器 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="22013-252">**Figure 08**: Adding a new ASP.NET MVC Controller ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))</span></span>


<span data-ttu-id="22013-253">**列出 1 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="22013-253">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a><span data-ttu-id="22013-254">列出資料庫中的記錄</span><span class="sxs-lookup"><span data-stu-id="22013-254">Listing Database Records</span></span>

<span data-ttu-id="22013-255">主控制器的 index （） 方法是 ASP.NET MVC 應用程式的預設方法。</span><span class="sxs-lookup"><span data-stu-id="22013-255">The Index() method of the Home controller is the default method for an ASP.NET MVC application.</span></span> <span data-ttu-id="22013-256">當您執行 ASP.NET MVC 應用程式時，index （） 方法是呼叫第一個控制器方法。</span><span class="sxs-lookup"><span data-stu-id="22013-256">When you run an ASP.NET MVC application, the Index() method is the first controller method that is called.</span></span>

<span data-ttu-id="22013-257">我們會使用 index （） 方法來顯示的電影資料庫資料表中的記錄清單。</span><span class="sxs-lookup"><span data-stu-id="22013-257">We'll use the Index() method to display the list of records from the Movies database table.</span></span> <span data-ttu-id="22013-258">我們會利用資料庫我們稍早建立的 index （） 方法擷取影片資料庫記錄的模型類別。</span><span class="sxs-lookup"><span data-stu-id="22013-258">We'll take advantage of the database model classes that we created earlier to retrieve the movie database records with the Index() method.</span></span>

<span data-ttu-id="22013-259">我已經修改 HomeController 類別清單 2 中的，使其包含名為的新私用欄位\_db。</span><span class="sxs-lookup"><span data-stu-id="22013-259">I've modified the HomeController class in Listing 2 so that it contains a new private field named \_db.</span></span> <span data-ttu-id="22013-260">MoviesDBEntities 類別代表我們的資料庫模型中，我們將使用此類別與資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="22013-260">The MoviesDBEntities class represents our database model and we'll use this class to communicate with our database.</span></span>

<span data-ttu-id="22013-261">我也修改了 index （） 方法，在列出 2。</span><span class="sxs-lookup"><span data-stu-id="22013-261">I've also modified the Index() method in Listing 2.</span></span> <span data-ttu-id="22013-262">Index （） 方法使用 MoviesDBEntities 類別來擷取所有的電影記錄電影資料庫資料表中。</span><span class="sxs-lookup"><span data-stu-id="22013-262">The Index() method uses the MoviesDBEntities class to retrieve all of the movie records from the Movies database table.</span></span> <span data-ttu-id="22013-263">運算式 *\_db。MovieSet.ToList()*電影資料庫資料表中傳回的所有電影記錄清單。</span><span class="sxs-lookup"><span data-stu-id="22013-263">The expression *\_db.MovieSet.ToList()* returns a list of all of the movie records from the Movies database table.</span></span>

<span data-ttu-id="22013-264">電影清單會傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="22013-264">The list of movies is passed to the view.</span></span> <span data-ttu-id="22013-265">取得傳遞給 View() 方法的任何項目取得傳遞至檢視，以檢視資料。</span><span class="sxs-lookup"><span data-stu-id="22013-265">Anything that gets passed to the View() method gets passed to the view as view data.</span></span>

<span data-ttu-id="22013-266">**列出 2 – Controllers/HomeController.vb （修改索引的方法）**</span><span class="sxs-lookup"><span data-stu-id="22013-266">**Listing 2 – Controllers/HomeController.vb (modified Index method)**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

<span data-ttu-id="22013-267">Index （） 方法會傳回名為索引的檢視。</span><span class="sxs-lookup"><span data-stu-id="22013-267">The Index() method returns a view named Index.</span></span> <span data-ttu-id="22013-268">我們需要建立此檢視來顯示影片資料庫記錄的清單。</span><span class="sxs-lookup"><span data-stu-id="22013-268">We need to create this view to display the list of movie database records.</span></span> <span data-ttu-id="22013-269">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="22013-269">Follow these steps:</span></span>


<span data-ttu-id="22013-270">您應該建置專案時 (選取功能表選項**建置時，建置方案**) 才能開啟**加入檢視**對話 」 或 「 無類別會出現在**檢視資料類別**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="22013-270">You should build your project (select the menu option **Build, Build Solution**) before opening the **Add View** dialog or no classes will appear in the **View data class** dropdown list.</span></span>


1. <span data-ttu-id="22013-271">以滑鼠右鍵按一下程式碼編輯器中的 index （） 方法，然後選取功能表選項**加入檢視**（請參閱圖 9）。</span><span class="sxs-lookup"><span data-stu-id="22013-271">Right-click the Index() method in the code editor and select the menu option **Add View** (see Figure 9).</span></span>
2. <span data-ttu-id="22013-272">在 新增檢視 對話方塊中，確認 核取方塊標示為**建立強型別檢視**已核取。</span><span class="sxs-lookup"><span data-stu-id="22013-272">In the Add View dialog, verify that the checkbox labeled **Create a strongly-typed view** is checked.</span></span>
3. <span data-ttu-id="22013-273">從**檢視內容**下拉式清單中，選取值*清單*。</span><span class="sxs-lookup"><span data-stu-id="22013-273">From the **View content** dropdown list, select the value *List*.</span></span>
4. <span data-ttu-id="22013-274">從**檢視資料類別**下拉式清單中，選取值*MovieApp.Movie*。</span><span class="sxs-lookup"><span data-stu-id="22013-274">From the **View data class** dropdown list, select the value *MovieApp.Movie*.</span></span>
5. <span data-ttu-id="22013-275">按一下 [新增] 按鈕來建立新檢視 （請參閱圖 10）。</span><span class="sxs-lookup"><span data-stu-id="22013-275">Click the Add button to create the new view (see Figure 10).</span></span>

<span data-ttu-id="22013-276">完成這些步驟之後，新的檢視，名為 Index.aspx 便加入 Views\Home 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="22013-276">After you complete these steps, a new view named Index.aspx is added to the Views\Home folder.</span></span> <span data-ttu-id="22013-277">列出的 3 會包含索引檢視的內容。</span><span class="sxs-lookup"><span data-stu-id="22013-277">The contents of the Index view are included in Listing 3.</span></span>


<span data-ttu-id="22013-278">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="22013-278">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)</span></span>

<span data-ttu-id="22013-279">**圖 09**： 從控制器動作中加入的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="22013-279">**Figure 09**: Adding a view from a controller action ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))</span></span>


<span data-ttu-id="22013-280">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="22013-280">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)</span></span>

<span data-ttu-id="22013-281">**圖 10**： 使用 [新增檢視] 對話方塊建立新的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="22013-281">**Figure 10**: Creating a new view with the Add View dialog ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))</span></span>


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

<span data-ttu-id="22013-282">索引檢視會顯示所有電影記錄的 HTML 資料表中的電影資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="22013-282">The Index view displays all of the movie records from the Movies database table within an HTML table.</span></span> <span data-ttu-id="22013-283">此檢視包含 For each 逐一查看每個影片 ViewData.Model 屬性所代表。</span><span class="sxs-lookup"><span data-stu-id="22013-283">The view contains a For Each loop that iterates through each movie represented by the ViewData.Model property.</span></span> <span data-ttu-id="22013-284">如果您按下 F5 鍵執行應用程式，您會看到圖 11 網頁。</span><span class="sxs-lookup"><span data-stu-id="22013-284">If you run your application by hitting the F5 key, then you'll see the web page in Figure 11.</span></span>


<span data-ttu-id="22013-285">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="22013-285">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)</span></span>

<span data-ttu-id="22013-286">**圖 11**: 索引檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))</span><span class="sxs-lookup"><span data-stu-id="22013-286">**Figure 11**: The Index view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))</span></span>


## <a name="creating-new-database-records"></a><span data-ttu-id="22013-287">建立新的資料庫資料錄</span><span class="sxs-lookup"><span data-stu-id="22013-287">Creating New Database Records</span></span>

<span data-ttu-id="22013-288">我們在上一節中所建立的索引檢視包含用於建立新的資料庫記錄的連結。</span><span class="sxs-lookup"><span data-stu-id="22013-288">The Index view that we created in the previous section includes a link for creating new database records.</span></span> <span data-ttu-id="22013-289">現在讓我們實作的邏輯和建立檢視建立新的電影資料庫記錄所需。</span><span class="sxs-lookup"><span data-stu-id="22013-289">Let's go ahead and implement the logic and create the view necessary for creating new movie database records.</span></span>

<span data-ttu-id="22013-290">主控制器包含兩個方法，名為 create （）。</span><span class="sxs-lookup"><span data-stu-id="22013-290">The Home controller contains two methods named Create().</span></span> <span data-ttu-id="22013-291">第一個 create （） 方法沒有任何參數。</span><span class="sxs-lookup"><span data-stu-id="22013-291">The first Create() method has no parameters.</span></span> <span data-ttu-id="22013-292">這個 create （） 方法的多載用來顯示的 HTML 表單建立新的電影資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="22013-292">This overload of the Create() method is used to display the HTML form for creating a new movie database record.</span></span>

<span data-ttu-id="22013-293">第二個 create （） 方法具有 FormCollection 參數。</span><span class="sxs-lookup"><span data-stu-id="22013-293">The second Create() method has a FormCollection parameter.</span></span> <span data-ttu-id="22013-294">建立新的電影的 HTML 表單張貼至伺服器時，會呼叫這個 create （） 方法的多載。</span><span class="sxs-lookup"><span data-stu-id="22013-294">This overload of the Create() method is called when the HTML form for creating a new movie is posted to the server.</span></span> <span data-ttu-id="22013-295">請注意這個第二個 create （） 方法有 AcceptVerbs 屬性，使方法呼叫除非 HTTP Post 作業。</span><span class="sxs-lookup"><span data-stu-id="22013-295">Notice that this second Create() method has an AcceptVerbs attribute that prevents the method from being called unless an HTTP Post operation is performed.</span></span>

<span data-ttu-id="22013-296">更新 HomeController 類別中列出的 4 中已修改這個第二個 create （） 方法。</span><span class="sxs-lookup"><span data-stu-id="22013-296">This second Create() method has been modified in the updated HomeController class in Listing 4.</span></span> <span data-ttu-id="22013-297">新版本的 create （） 方法會接受影片參數，並包含電影資料庫資料表中插入新的電影的邏輯。</span><span class="sxs-lookup"><span data-stu-id="22013-297">The new version of the Create() method accepts a Movie parameter and contains the logic for inserting a new movie into the Movies database table.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="22013-298">請注意繫結屬性。</span><span class="sxs-lookup"><span data-stu-id="22013-298">Notice the Bind attribute.</span></span> <span data-ttu-id="22013-299">因為我們不想要更新的影片識別碼屬性，從 HTML 表單，我們需要明確地排除這個屬性。</span><span class="sxs-lookup"><span data-stu-id="22013-299">Because we don't want to update the Movie Id property from HTML form, we need to explicitly exclude this property.</span></span>


<span data-ttu-id="22013-300">**列出 4 – Controllers\HomeController.vb （已修改的建立方式）**</span><span class="sxs-lookup"><span data-stu-id="22013-300">**Listing 4 – Controllers\HomeController.vb (modified Create method)**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

<span data-ttu-id="22013-301">Visual Studio 可讓您輕鬆建立的表單建立新的電影資料庫記錄 （請參閱圖 12）。</span><span class="sxs-lookup"><span data-stu-id="22013-301">Visual Studio makes it easy to create the form for creating a new movie database record (see Figure 12).</span></span> <span data-ttu-id="22013-302">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="22013-302">Follow these steps:</span></span>

1. <span data-ttu-id="22013-303">以滑鼠右鍵按一下程式碼編輯器中的 create （） 方法，然後選取功能表選項**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="22013-303">Right-click the Create() method in the code editor and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="22013-304">請確認此核取方塊標示為**建立強型別檢視**已核取。</span><span class="sxs-lookup"><span data-stu-id="22013-304">Verify that the checkbox labeled **Create a strongly-typed view** is checked.</span></span>
3. <span data-ttu-id="22013-305">從**檢視內容**下拉式清單中，選取值*建立*。</span><span class="sxs-lookup"><span data-stu-id="22013-305">From the **View content** dropdown list, select the value *Create*.</span></span>
4. <span data-ttu-id="22013-306">從**檢視資料類別**下拉式清單中，選取值*MovieApp.Movie*。</span><span class="sxs-lookup"><span data-stu-id="22013-306">From the **View data class** dropdown list, select the value *MovieApp.Movie*.</span></span>
5. <span data-ttu-id="22013-307">按一下**新增**按鈕以建立新的檢視。</span><span class="sxs-lookup"><span data-stu-id="22013-307">Click the **Add** button to create the new view.</span></span>


<span data-ttu-id="22013-308">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="22013-308">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)</span></span>

<span data-ttu-id="22013-309">**圖 12**： 加入建立檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="22013-309">**Figure 12**: Adding the Create view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))</span></span>


<span data-ttu-id="22013-310">Visual Studio 檢視列出 5 中會自動產生。</span><span class="sxs-lookup"><span data-stu-id="22013-310">Visual Studio generates the view in Listing 5 automatically.</span></span> <span data-ttu-id="22013-311">這個檢視包含 HTML 表單，其中包含欄位對應至每個影片類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="22013-311">This view contains an HTML form that includes fields that correspond to each of the properties of the Movie class.</span></span>

<span data-ttu-id="22013-312">**列出 5 – Views\Home\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="22013-312">**Listing 5 – Views\Home\Create.aspx**</span></span>

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> <span data-ttu-id="22013-313">[新增檢視] 對話方塊所產生的 HTML 格式產生 Id 的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="22013-313">The HTML form generated by the Add View dialog generates an Id form field.</span></span> <span data-ttu-id="22013-314">因為識別碼資料行是識別資料行，所以我們不需要這個表單欄位，您可以安全地將它移除。</span><span class="sxs-lookup"><span data-stu-id="22013-314">Because the Id column is an Identity column, we don't need this form field and you can safely remove it.</span></span>


<span data-ttu-id="22013-315">加入建立檢視之後，您可以新增電影記錄至資料庫。</span><span class="sxs-lookup"><span data-stu-id="22013-315">After you add the Create view, you can add new Movie records to the database.</span></span> <span data-ttu-id="22013-316">按 F5 鍵執行應用程式，然後按一下 建立新的連結，以查看圖 13 中的表單。</span><span class="sxs-lookup"><span data-stu-id="22013-316">Run your application by pressing the F5 key and click the Create New link to see the form in Figure 13.</span></span> <span data-ttu-id="22013-317">如果您填寫並提交表單時，會建立新的電影資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="22013-317">If you complete and submit the form, a new movie database record is created.</span></span>

<span data-ttu-id="22013-318">請注意，自動取得表單驗證。</span><span class="sxs-lookup"><span data-stu-id="22013-318">Notice that you get form validation automatically.</span></span> <span data-ttu-id="22013-319">如果沒有輸入電影，發行日期，或是您輸入無效的發行日期，然後會重新顯示表單，並發行日期 欄位會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="22013-319">If you neglect to enter a release date for a movie, or you enter an invalid release date, then the form is redisplayed and the release date field is highlighted.</span></span>


<span data-ttu-id="22013-320">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="22013-320">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)</span></span>

<span data-ttu-id="22013-321">**圖 13**： 建立新的電影資料庫記錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="22013-321">**Figure 13**: Creating a new movie database record ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))</span></span>


## <a name="editing-existing-database-records"></a><span data-ttu-id="22013-322">編輯現有的資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="22013-322">Editing Existing Database Records</span></span>

<span data-ttu-id="22013-323">上一節中，我們將討論如何列出，並建立新的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="22013-323">In the previous sections, we discussed how you can list and create new database records.</span></span> <span data-ttu-id="22013-324">在這最後一節中，我們會討論如何編輯現有的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="22013-324">In this final section, we discuss how you can edit existing database records.</span></span>

<span data-ttu-id="22013-325">首先，我們需要產生的編輯表單。</span><span class="sxs-lookup"><span data-stu-id="22013-325">First, we need to generate the Edit form.</span></span> <span data-ttu-id="22013-326">這個步驟很簡單，因為 Visual Studio 會產生的編輯表單為我們自動。</span><span class="sxs-lookup"><span data-stu-id="22013-326">This step is easy since Visual Studio will generate the Edit form for us automatically.</span></span> <span data-ttu-id="22013-327">在 Visual Studio 程式碼編輯器中開啟 HomeController.vb 類別，並遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="22013-327">Open the HomeController.vb class in the Visual Studio code editor and follow these steps:</span></span>

1. <span data-ttu-id="22013-328">在程式碼編輯器的 Edit() 方法上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱圖 14）。</span><span class="sxs-lookup"><span data-stu-id="22013-328">Right-click the Edit() method in the code editor and select the menu option **Add View** (see Figure 14).</span></span>
2. <span data-ttu-id="22013-329">核取方塊標示為**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="22013-329">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
3. <span data-ttu-id="22013-330">從**檢視內容**下拉式清單中，選取值*編輯*。</span><span class="sxs-lookup"><span data-stu-id="22013-330">From the **View content** dropdown list, select the value *Edit*.</span></span>
4. <span data-ttu-id="22013-331">從**檢視資料類別**下拉式清單中，選取值*MovieApp.Movie*。</span><span class="sxs-lookup"><span data-stu-id="22013-331">From the **View data class** dropdown list, select the value *MovieApp.Movie*.</span></span>
5. <span data-ttu-id="22013-332">按一下**新增**按鈕以建立新的檢視。</span><span class="sxs-lookup"><span data-stu-id="22013-332">Click the **Add** button to create the new view.</span></span>

<span data-ttu-id="22013-333">完成這些步驟，新增名為 Edit.aspx Views\Home 資料夾檢視。</span><span class="sxs-lookup"><span data-stu-id="22013-333">Completing these steps adds a new view named Edit.aspx to the Views\Home folder.</span></span> <span data-ttu-id="22013-334">這個檢視包含編輯電影錄製之 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="22013-334">This view contains an HTML form for editing a movie record.</span></span>


<span data-ttu-id="22013-335">[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="22013-335">[![The New Project dialog box](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)</span></span>

<span data-ttu-id="22013-336">**圖 14**： 加入編輯檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))</span><span class="sxs-lookup"><span data-stu-id="22013-336">**Figure 14**: Adding the Edit view ([Click to view full-size image](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="22013-337">編輯檢視包含 HTML 表單欄位對應到電影識別碼屬性。</span><span class="sxs-lookup"><span data-stu-id="22013-337">The Edit view contains an HTML form field that corresponds to the Movie Id property.</span></span> <span data-ttu-id="22013-338">您不想讓使用者編輯 Id 屬性的值，您應該移除此表單欄位。</span><span class="sxs-lookup"><span data-stu-id="22013-338">Because you don't want people editing the value of the Id property, you should remove this form field.</span></span>


<span data-ttu-id="22013-339">最後，我們需要修改主控制器，讓它支援 編輯資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="22013-339">Finally, we need to modify the Home controller so that it supports editing a database record.</span></span> <span data-ttu-id="22013-340">更新的 HomeController 類別被包含在程式碼範例 6。</span><span class="sxs-lookup"><span data-stu-id="22013-340">The updated HomeController class is contained in Listing 6.</span></span>

<span data-ttu-id="22013-341">**列出 6 – Controllers\HomeController.vb （編輯方法）**</span><span class="sxs-lookup"><span data-stu-id="22013-341">**Listing 6 – Controllers\HomeController.vb (Edit methods)**</span></span>

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

<span data-ttu-id="22013-342">在列出 6 中，我已將額外的邏輯加入到 Edit() 方法的兩個多載。</span><span class="sxs-lookup"><span data-stu-id="22013-342">In Listing 6, I've added additional logic to both overloads of the Edit() method.</span></span> <span data-ttu-id="22013-343">第一個 Edit() 方法會傳回對應的電影資料庫記錄，Id 參數傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="22013-343">The first Edit() method returns the movie database record that corresponds to the Id parameter passed to the method.</span></span> <span data-ttu-id="22013-344">第二個多載會在資料庫中執行電影錄製到的更新。</span><span class="sxs-lookup"><span data-stu-id="22013-344">The second overload performs the updates to a movie record in the database.</span></span>

<span data-ttu-id="22013-345">請注意，您必須擷取原始的影片，然後呼叫 ApplyPropertyChanges()，若要更新資料庫中現有的電影。</span><span class="sxs-lookup"><span data-stu-id="22013-345">Notice that you must retrieve the original movie, and then call ApplyPropertyChanges(), to update the existing movie in the database.</span></span>

## <a name="summary"></a><span data-ttu-id="22013-346">總結</span><span class="sxs-lookup"><span data-stu-id="22013-346">Summary</span></span>

<span data-ttu-id="22013-347">本教學課程的目的是經驗的要提供您的建置 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="22013-347">The purpose of this tutorial was to give you a sense of the experience of building an ASP.NET MVC application.</span></span> <span data-ttu-id="22013-348">希望您會發現建置 ASP.NET MVC web 應用程式是非常類似於建置 Active Server Pages 或 ASP.NET 應用程式的體驗。</span><span class="sxs-lookup"><span data-stu-id="22013-348">I hope that you discovered that building an ASP.NET MVC web application is very similar to the experience of building an Active Server Pages or ASP.NET application.</span></span>

<span data-ttu-id="22013-349">在本教學課程中，我們會檢查 ASP.NET MVC 架構的最基本功能。</span><span class="sxs-lookup"><span data-stu-id="22013-349">In this tutorial, we examined only the most basic features of the ASP.NET MVC framework.</span></span> <span data-ttu-id="22013-350">在未來的教學課程中，我們深入主題，例如控制器、 控制器的動作、 檢視、 檢視資料和 HTML helper。</span><span class="sxs-lookup"><span data-stu-id="22013-350">In future tutorials, we dive deeper into topics such as controllers, controller actions, views, view data, and HTML helpers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="22013-351">上一步</span><span class="sxs-lookup"><span data-stu-id="22013-351">Previous</span></span>](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
