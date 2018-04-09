---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: 顯示資料表的資料庫資料 (C#) |Microsoft 文件
author: microsoft
description: 在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。 我會顯示兩種格式的 HTML 中的資料庫記錄的一組 ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d5dc9dd4a82e4577c6c1a3b124d45fef0b0f67c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="58cc5-104">顯示資料表的資料庫資料 (C#)</span><span class="sxs-lookup"><span data-stu-id="58cc5-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="58cc5-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="58cc5-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="58cc5-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="58cc5-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="58cc5-107">在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="58cc5-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="58cc5-108">我會顯示兩種格式的 HTML 表格中的資料庫記錄的一組方法。</span><span class="sxs-lookup"><span data-stu-id="58cc5-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="58cc5-109">首先，我會顯示如何設定格式直接在檢視中的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="58cc5-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="58cc5-110">接下來，我會示範如何您可以利用 partials 格式化資料庫記錄時。</span><span class="sxs-lookup"><span data-stu-id="58cc5-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="58cc5-111">本教學課程的目標是要說明如何在 ASP.NET MVC 應用程式中顯示 HTML 表格的資料庫資料。</span><span class="sxs-lookup"><span data-stu-id="58cc5-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="58cc5-112">首先，您會了解如何使用 Visual Studio 中所含的 scaffolding 工具產生自動顯示一組記錄的檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="58cc5-113">接下來，您會學習如何格式化資料庫記錄時，使用了部分做為範本。</span><span class="sxs-lookup"><span data-stu-id="58cc5-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="58cc5-114">建立模型類別</span><span class="sxs-lookup"><span data-stu-id="58cc5-114">Create the Model Classes</span></span>

<span data-ttu-id="58cc5-115">我們將會顯示一組的電影資料庫資料表中的記錄。</span><span class="sxs-lookup"><span data-stu-id="58cc5-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="58cc5-116">電影資料庫資料表包含下列資料行：</span><span class="sxs-lookup"><span data-stu-id="58cc5-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="58cc5-117">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="58cc5-117">**Column Name**</span></span> | <span data-ttu-id="58cc5-118">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="58cc5-118">**Data Type**</span></span> | <span data-ttu-id="58cc5-119">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="58cc5-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58cc5-120">ID</span><span class="sxs-lookup"><span data-stu-id="58cc5-120">Id</span></span> | <span data-ttu-id="58cc5-121">Int</span><span class="sxs-lookup"><span data-stu-id="58cc5-121">Int</span></span> | <span data-ttu-id="58cc5-122">False</span><span class="sxs-lookup"><span data-stu-id="58cc5-122">False</span></span> |
| <span data-ttu-id="58cc5-123">標題</span><span class="sxs-lookup"><span data-stu-id="58cc5-123">Title</span></span> | <span data-ttu-id="58cc5-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="58cc5-124">Nvarchar(200)</span></span> | <span data-ttu-id="58cc5-125">False</span><span class="sxs-lookup"><span data-stu-id="58cc5-125">False</span></span> |
| <span data-ttu-id="58cc5-126">導向器</span><span class="sxs-lookup"><span data-stu-id="58cc5-126">Director</span></span> | <span data-ttu-id="58cc5-127">Nvarchar （50)</span><span class="sxs-lookup"><span data-stu-id="58cc5-127">NVarchar(50)</span></span> | <span data-ttu-id="58cc5-128">False</span><span class="sxs-lookup"><span data-stu-id="58cc5-128">False</span></span> |
| <span data-ttu-id="58cc5-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="58cc5-129">DateReleased</span></span> | <span data-ttu-id="58cc5-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="58cc5-130">DateTime</span></span> | <span data-ttu-id="58cc5-131">False</span><span class="sxs-lookup"><span data-stu-id="58cc5-131">False</span></span> |


<span data-ttu-id="58cc5-132">若要代表電影的資料表在 ASP.NET MVC 應用程式中，我們要建立模型類別。</span><span class="sxs-lookup"><span data-stu-id="58cc5-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="58cc5-133">在本教學課程中，我們可以使用 Microsoft Entity Framework 來建立模型類別。</span><span class="sxs-lookup"><span data-stu-id="58cc5-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="58cc5-134">在本教學課程中，我們會使用 Microsoft Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="58cc5-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="58cc5-135">不過，務必了解，您可以使用各種不同的技術與 ASP.NET MVC 應用程式，包括 LINQ to SQL、 NHibernate 或 ADO.NET 的資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="58cc5-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="58cc5-136">請遵循下列步驟來啟動實體資料模型精靈：</span><span class="sxs-lookup"><span data-stu-id="58cc5-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="58cc5-137">以滑鼠右鍵按一下 [模型] 資料夾，在 [方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。</span><span class="sxs-lookup"><span data-stu-id="58cc5-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="58cc5-138">選取**資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。</span><span class="sxs-lookup"><span data-stu-id="58cc5-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="58cc5-139">將您的資料模型命名*MoviesDBModel.edmx*按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="58cc5-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="58cc5-140">按一下 [新增] 按鈕之後，實體資料模型精靈就會出現 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="58cc5-141">請遵循下列步驟來完成精靈：</span><span class="sxs-lookup"><span data-stu-id="58cc5-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="58cc5-142">在**選擇模型內容**步驟中，選取**從資料庫產生**選項。</span><span class="sxs-lookup"><span data-stu-id="58cc5-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="58cc5-143">在**選擇資料連接**步驟，請使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連接設定。</span><span class="sxs-lookup"><span data-stu-id="58cc5-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="58cc5-144">按一下**下一步** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="58cc5-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="58cc5-145">在**選擇您的資料庫物件**步驟中，展開資料表節點，選取 電影資料表。</span><span class="sxs-lookup"><span data-stu-id="58cc5-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="58cc5-146">輸入的命名空間*模型*按一下**完成** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="58cc5-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="58cc5-147">[![建立 LINQ to SQL 類別](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="58cc5-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="58cc5-148">**圖 01**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="58cc5-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="58cc5-149">實體資料模型精靈完成之後，Entity Data Model Designer 隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="58cc5-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="58cc5-150">在設計工具應該會顯示電影實體 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="58cc5-151">[![Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="58cc5-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="58cc5-152">**圖 02**: Entity Data Model Designer ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="58cc5-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="58cc5-153">我們需要進行一項變更，我們繼續執行。</span><span class="sxs-lookup"><span data-stu-id="58cc5-153">We need to make one change before we continue.</span></span> <span data-ttu-id="58cc5-154">實體資料精靈產生模型類別，名為*電影*表示電影資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="58cc5-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="58cc5-155">由於我們將使用電影類別來代表特定的影片，我們需要修改的類別的類別名稱*影片*而不是*電影*（單數而複數）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="58cc5-156">按兩下設計工具介面上的類別名稱，並將電影從類別的名稱，變更 電影。</span><span class="sxs-lookup"><span data-stu-id="58cc5-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="58cc5-157">進行這項變更之後, 按**儲存**按鈕 （磁片圖示），以產生影片類別。</span><span class="sxs-lookup"><span data-stu-id="58cc5-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="58cc5-158">建立電影控制站</span><span class="sxs-lookup"><span data-stu-id="58cc5-158">Create the Movies Controller</span></span>

<span data-ttu-id="58cc5-159">現在，我們已經呈現我們的資料庫記錄的方式，我們可以建立傳回之集合的電影的控制站。</span><span class="sxs-lookup"><span data-stu-id="58cc5-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="58cc5-160">在 Visual Studio 方案總管 視窗中，以滑鼠右鍵按一下 控制器 資料夾，然後選取功能表選項**新增、 控制站**（請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="58cc5-161">[![加入控制器 功能表](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="58cc5-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="58cc5-162">**圖 03**: [加入控制器] 功能表中 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="58cc5-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="58cc5-163">當**加入控制器**對話方塊出現，請輸入控制器名稱 MovieController （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="58cc5-164">按一下**新增**按鈕以加入新的控制器。</span><span class="sxs-lookup"><span data-stu-id="58cc5-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="58cc5-165">[![[加入控制器] 對話方塊](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="58cc5-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="58cc5-166">**圖 04**: 加入控制器 對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="58cc5-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="58cc5-167">我們需要修改，使它傳回一組資料庫記錄，影片控制器所公開的 index （） 動作。</span><span class="sxs-lookup"><span data-stu-id="58cc5-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="58cc5-168">修改控制器，讓它看起來像是列表 1 中的控制站。</span><span class="sxs-lookup"><span data-stu-id="58cc5-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="58cc5-169">**列出 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="58cc5-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="58cc5-170">列出第 1 MoviesDBEntities 類別用來代表 MoviesDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="58cc5-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="58cc5-171">若要使用這個類別，您必須匯入 MvcApplication1.Models 命名空間，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="58cc5-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="58cc5-172">使用 MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="58cc5-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="58cc5-173">運算式*實體。MovieSet.ToList()*電影資料庫資料表中傳回的所有電影的集合。</span><span class="sxs-lookup"><span data-stu-id="58cc5-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="58cc5-174">建立檢視</span><span class="sxs-lookup"><span data-stu-id="58cc5-174">Create the View</span></span>

<span data-ttu-id="58cc5-175">HTML 表格中顯示的一組資料庫記錄的最簡單方式是樣板的利用 Visual Studio 所提供。</span><span class="sxs-lookup"><span data-stu-id="58cc5-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="58cc5-176">功能表選項來建立您的應用程式**建置時，建置方案**。</span><span class="sxs-lookup"><span data-stu-id="58cc5-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="58cc5-177">您必須建置應用程式，再開啟**加入檢視**對話 」 或 「 資料類別不會出現在對話方塊中。</span><span class="sxs-lookup"><span data-stu-id="58cc5-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="58cc5-178">Index （） 動作上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="58cc5-179">[![加入的檢視](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="58cc5-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="58cc5-180">**圖 05**： 加入的檢視 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="58cc5-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="58cc5-181">在**加入檢視** 對話方塊中，選取核取方塊標示為**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="58cc5-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="58cc5-182">選取影片類別，做為**檢視資料類別**。</span><span class="sxs-lookup"><span data-stu-id="58cc5-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="58cc5-183">選取*清單*為**檢視內容**（請參閱圖 6）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="58cc5-184">選取這些選項會產生強型別 檢視顯示的電影清單。</span><span class="sxs-lookup"><span data-stu-id="58cc5-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="58cc5-185">[![新增檢視對話方塊](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="58cc5-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="58cc5-186">**圖 06**: 新增檢視對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="58cc5-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="58cc5-187">按一下 之後**新增**時自動產生按鈕、 列表 2 中的檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="58cc5-188">這個檢視包含逐一查看集合的影片和顯示每個影片屬性所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="58cc5-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="58cc5-189">**列出 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="58cc5-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="58cc5-190">您可以藉由選取功能表選項來執行應用程式**偵錯，開始偵錯**（或按下 F5 鍵）。</span><span class="sxs-lookup"><span data-stu-id="58cc5-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="58cc5-191">執行應用程式會啟動 Internet Explorer。</span><span class="sxs-lookup"><span data-stu-id="58cc5-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="58cc5-192">如果您導覽至 /Movie URL，您會看到圖 7 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="58cc5-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="58cc5-193">[![資料表的電影](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="58cc5-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="58cc5-194">**圖 07**： 電影的資料表 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="58cc5-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="58cc5-195">如果您不喜歡任何有關資料庫中的記錄圖 7 格線的外觀就可以只修改索引檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="58cc5-196">例如，您可以變更*DateReleased*標頭*發行日期*藉由修改索引檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="58cc5-197">使用部分建立範本</span><span class="sxs-lookup"><span data-stu-id="58cc5-197">Create a Template with a Partial</span></span>

<span data-ttu-id="58cc5-198">當檢視表太複雜時，最好先啟動分成 partials 的檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="58cc5-199">使用 partials 可讓您更輕鬆地了解和維護您的檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="58cc5-200">我們將會建立部分我們可以使用當做範本來設定每個影片資料庫記錄的格式。</span><span class="sxs-lookup"><span data-stu-id="58cc5-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="58cc5-201">請遵循下列步驟來建立部分：</span><span class="sxs-lookup"><span data-stu-id="58cc5-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="58cc5-202">Views\Movie 資料夾上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="58cc5-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="58cc5-203">核取方塊標示為*建立部分檢視 (.ascx)*。</span><span class="sxs-lookup"><span data-stu-id="58cc5-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="58cc5-204">命名部分*MovieTemplate*。</span><span class="sxs-lookup"><span data-stu-id="58cc5-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="58cc5-205">核取方塊標示為**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="58cc5-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="58cc5-206">選取將電影當作*檢視資料類別*。</span><span class="sxs-lookup"><span data-stu-id="58cc5-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="58cc5-207">選取空白做為*檢視內容*。</span><span class="sxs-lookup"><span data-stu-id="58cc5-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="58cc5-208">按一下**新增**按鈕，將部分加入至您的專案。</span><span class="sxs-lookup"><span data-stu-id="58cc5-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="58cc5-209">完成這些步驟之後，修改成類似列出 3 部分 MovieTemplate。</span><span class="sxs-lookup"><span data-stu-id="58cc5-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="58cc5-210">**列出 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="58cc5-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="58cc5-211">列出的 3 部分包含單一資料列的記錄的範本。</span><span class="sxs-lookup"><span data-stu-id="58cc5-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="58cc5-212">修改的索引檢視表中列出的 4 使用 MovieTemplate 部分。</span><span class="sxs-lookup"><span data-stu-id="58cc5-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="58cc5-213">**列出 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="58cc5-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="58cc5-214">列出的 4 中的檢視包含 「 foreach 迴圈 」 可逐一查看所有的電影。</span><span class="sxs-lookup"><span data-stu-id="58cc5-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="58cc5-215">針對每個影片中，部分 MovieTemplate 用來格式化影片。</span><span class="sxs-lookup"><span data-stu-id="58cc5-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="58cc5-216">MovieTemplate 會藉由呼叫 RenderPartial() helper 方法轉譯。</span><span class="sxs-lookup"><span data-stu-id="58cc5-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="58cc5-217">修改過的索引檢視呈現資料庫記錄相同的 HTML 的表格。</span><span class="sxs-lookup"><span data-stu-id="58cc5-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="58cc5-218">不過，已大幅簡化檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="58cc5-219">RenderPartial() 方法是比大多數其他 helper 方法不同，因為它不會傳回字串。</span><span class="sxs-lookup"><span data-stu-id="58cc5-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="58cc5-220">因此，您必須呼叫 RenderPartial() 方法使用&lt;%html.renderpartial(); %&gt;而不是&lt;%= Html.RenderPartial(); %&gt;。</span><span class="sxs-lookup"><span data-stu-id="58cc5-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="58cc5-221">總結</span><span class="sxs-lookup"><span data-stu-id="58cc5-221">Summary</span></span>

<span data-ttu-id="58cc5-222">本教學課程的目的是要說明如何顯示一組資料庫記錄，以及在 HTML 表格中。</span><span class="sxs-lookup"><span data-stu-id="58cc5-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="58cc5-223">首先，您學會如何從控制器動作傳回的一組資料庫記錄，藉由運用 Microsoft Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="58cc5-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="58cc5-224">接下來，您學會如何使用 Visual Studio scaffolding 產生檢視自動顯示的項目集合。</span><span class="sxs-lookup"><span data-stu-id="58cc5-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="58cc5-225">最後，您會學到如何利用部分簡化檢視。</span><span class="sxs-lookup"><span data-stu-id="58cc5-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="58cc5-226">您已學習如何使用部分做為範本，以便您可以格式化每個資料庫的記錄。</span><span class="sxs-lookup"><span data-stu-id="58cc5-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="58cc5-227">[上一頁](creating-model-classes-with-linq-to-sql-cs.md)
> [下一頁](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="58cc5-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
