---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: 顯示資料表的資料庫資料 (VB) |Microsoft Docs
author: microsoft
description: 在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。 我會示範兩種格式的一組資料庫記錄，在 HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f5d9062d901f28b1d64f400e13ed5eb0ca788ab8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362488"
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="a38aa-104">顯示資料表的資料庫資料 (VB)</span><span class="sxs-lookup"><span data-stu-id="a38aa-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="a38aa-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a38aa-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a38aa-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="a38aa-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="a38aa-107">在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="a38aa-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="a38aa-108">我會示範兩種格式的一組 HTML 表格中的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="a38aa-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="a38aa-109">首先，我會示範如何設定格式直接在檢視內的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="a38aa-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="a38aa-110">接下來，我會示範格式化資料庫記錄時，如何充分善用部分。</span><span class="sxs-lookup"><span data-stu-id="a38aa-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="a38aa-111">本教學課程的目標是要說明如何顯示 HTML 表格的資料庫資料，以及在 ASP.NET MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="a38aa-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="a38aa-112">首先，您會了解如何使用隨附於 Visual Studio scaffolding 工具來產生自動顯示一組記錄的檢視。</span><span class="sxs-lookup"><span data-stu-id="a38aa-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="a38aa-113">接下來，您會了解如何格式化資料庫記錄時，使用部分做為範本。</span><span class="sxs-lookup"><span data-stu-id="a38aa-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="a38aa-114">建立模型類別</span><span class="sxs-lookup"><span data-stu-id="a38aa-114">Create the Model Classes</span></span>

<span data-ttu-id="a38aa-115">我們要顯示的電影資料庫資料表中的記錄集。</span><span class="sxs-lookup"><span data-stu-id="a38aa-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="a38aa-116">電影資料庫資料表包含下列資料行：</span><span class="sxs-lookup"><span data-stu-id="a38aa-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="a38aa-117">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="a38aa-117">**Column Name**</span></span> | <span data-ttu-id="a38aa-118">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="a38aa-118">**Data Type**</span></span> | <span data-ttu-id="a38aa-119">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="a38aa-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a38aa-120">ID</span><span class="sxs-lookup"><span data-stu-id="a38aa-120">Id</span></span> | <span data-ttu-id="a38aa-121">Int</span><span class="sxs-lookup"><span data-stu-id="a38aa-121">Int</span></span> | <span data-ttu-id="a38aa-122">False</span><span class="sxs-lookup"><span data-stu-id="a38aa-122">False</span></span> |
| <span data-ttu-id="a38aa-123">標題</span><span class="sxs-lookup"><span data-stu-id="a38aa-123">Title</span></span> | <span data-ttu-id="a38aa-124">Nvarchar(200</span><span class="sxs-lookup"><span data-stu-id="a38aa-124">Nvarchar(200)</span></span> | <span data-ttu-id="a38aa-125">False</span><span class="sxs-lookup"><span data-stu-id="a38aa-125">False</span></span> |
| <span data-ttu-id="a38aa-126">總監</span><span class="sxs-lookup"><span data-stu-id="a38aa-126">Director</span></span> | <span data-ttu-id="a38aa-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="a38aa-127">NVarchar(50)</span></span> | <span data-ttu-id="a38aa-128">False</span><span class="sxs-lookup"><span data-stu-id="a38aa-128">False</span></span> |
| <span data-ttu-id="a38aa-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="a38aa-129">DateReleased</span></span> | <span data-ttu-id="a38aa-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="a38aa-130">DateTime</span></span> | <span data-ttu-id="a38aa-131">False</span><span class="sxs-lookup"><span data-stu-id="a38aa-131">False</span></span> |


<span data-ttu-id="a38aa-132">若要代表在 ASP.NET MVC 應用程式中的電影資料表，我們要建立模型類別。</span><span class="sxs-lookup"><span data-stu-id="a38aa-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="a38aa-133">本教學課程中，我們會使用 Microsoft Entity Framework 來建立我們的模型類別。</span><span class="sxs-lookup"><span data-stu-id="a38aa-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a38aa-134">在本教學課程中，我們會使用 Microsoft Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="a38aa-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="a38aa-135">不過，務必了解，您可以使用各種不同的技術來包括 LINQ to SQL、 NHibernate 或 ADO.NET 的 ASP.NET MVC 應用程式的資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="a38aa-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="a38aa-136">請遵循下列步驟來啟動 Entity Data Model 精靈：</span><span class="sxs-lookup"><span data-stu-id="a38aa-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="a38aa-137">在 [方案總管] 視窗，然後選取功能表選項中的 [Models] 資料夾上按一下滑鼠右鍵**新增]、 [新項目**。</span><span class="sxs-lookup"><span data-stu-id="a38aa-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="a38aa-138">選取 **資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。</span><span class="sxs-lookup"><span data-stu-id="a38aa-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="a38aa-139">將您的資料模型命名*MoviesDBModel.edmx*然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a38aa-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="a38aa-140">按一下 [新增] 按鈕之後，實體資料模型精靈] 會出現 （請參閱 [圖 1）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="a38aa-141">請遵循下列步驟來完成精靈：</span><span class="sxs-lookup"><span data-stu-id="a38aa-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="a38aa-142">在 **選擇模型內容**步驟中，選取**從資料庫產生**選項。</span><span class="sxs-lookup"><span data-stu-id="a38aa-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="a38aa-143">在 **選擇資料連接**步驟中，使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連線設定。</span><span class="sxs-lookup"><span data-stu-id="a38aa-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="a38aa-144">按一下 **下一步**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="a38aa-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="a38aa-145">在 **選擇您的資料庫物件**步驟中，展開 資料表 節點中，選取 電影資料表。</span><span class="sxs-lookup"><span data-stu-id="a38aa-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="a38aa-146">輸入的命名空間*模型*然後按一下**完成** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a38aa-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="a38aa-147">[![建立 LINQ to SQL 類別](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a38aa-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="a38aa-148">**圖 01**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a38aa-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="a38aa-149">Entity Data Model 精靈完成之後，就會開啟實體資料模型設計工具。</span><span class="sxs-lookup"><span data-stu-id="a38aa-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="a38aa-150">設計工具應該會顯示電影實體 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="a38aa-151">[![實體資料模型設計工具](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a38aa-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="a38aa-152">**圖 02**: 實體資料模型設計工具 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a38aa-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="a38aa-153">我們需要進行一項變更，再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="a38aa-153">We need to make one change before we continue.</span></span> <span data-ttu-id="a38aa-154">實體資料精靈產生模型類別，名為*電影*表示電影資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="a38aa-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="a38aa-155">因為我們將使用電影類別來代表特定的影片，我們需要修改的類別名稱*電影*而不是*電影*（單數而複數）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="a38aa-156">按兩下設計工具介面上類別名稱，並從電影時使用的類別名稱變更為電影。</span><span class="sxs-lookup"><span data-stu-id="a38aa-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="a38aa-157">完成此變更之後，按一下**儲存**按鈕 （磁片圖示） 來產生電影類別。</span><span class="sxs-lookup"><span data-stu-id="a38aa-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="a38aa-158">建立電影控制器</span><span class="sxs-lookup"><span data-stu-id="a38aa-158">Create the Movies Controller</span></span>

<span data-ttu-id="a38aa-159">既然我們已代表我們的資料庫記錄的方式，我們可以建立傳回的電影集合的控制站。</span><span class="sxs-lookup"><span data-stu-id="a38aa-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="a38aa-160">Visual Studio 方案總管 視窗中，以滑鼠右鍵按一下 控制器 資料夾，然後選取功能表選項**新增、 控制站**（請參閱 圖 3）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="a38aa-161">[![[加入控制器] 功能表](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a38aa-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="a38aa-162">**圖 03**: [新增控制器] 功能表中 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a38aa-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="a38aa-163">當**新增控制器**對話方塊隨即出現，請輸入控制器名稱 MovieController （請參閱 圖 4）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="a38aa-164">按一下 **新增**按鈕以新增新的控制器。</span><span class="sxs-lookup"><span data-stu-id="a38aa-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="a38aa-165">[![[新增控制器] 對話方塊](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a38aa-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="a38aa-166">**[圖 04**: 新增控制器] 對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a38aa-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="a38aa-167">我們需要修改，使它傳回的一組資料庫記錄由電影控制器的 index （） 動作。</span><span class="sxs-lookup"><span data-stu-id="a38aa-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="a38aa-168">修改控制器，讓它看起來像 列表 1 中的控制站。</span><span class="sxs-lookup"><span data-stu-id="a38aa-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="a38aa-169">**列表 1 – Controllers\MovieController.vb**</span><span class="sxs-lookup"><span data-stu-id="a38aa-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="a38aa-170">在 [列表 1] MoviesDBEntities 類別用來代表 MoviesDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="a38aa-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="a38aa-171">運算式*實體。MovieSet.ToList()* 電影資料庫資料表中傳回所有影片的集合。</span><span class="sxs-lookup"><span data-stu-id="a38aa-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="a38aa-172">建立檢視</span><span class="sxs-lookup"><span data-stu-id="a38aa-172">Create the View</span></span>

<span data-ttu-id="a38aa-173">顯示 HTML 表格中的一組資料庫記錄的最簡單方式是 scaffolding 的利用 Visual Studio 所提供。</span><span class="sxs-lookup"><span data-stu-id="a38aa-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="a38aa-174">選取功能表選項來建置您的應用程式**建置時，建置方案**。</span><span class="sxs-lookup"><span data-stu-id="a38aa-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="a38aa-175">您必須建置您的應用程式，才能開啟**加入檢視**對話 」 或 「 資料類別就不會出現在對話方塊中。</span><span class="sxs-lookup"><span data-stu-id="a38aa-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="a38aa-176">以滑鼠右鍵按一下 index （） 動作，然後選取功能表選項**加入檢視**（請參閱 圖 5）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="a38aa-177">[![新增檢視](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a38aa-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="a38aa-178">**圖 05**： 新增檢視 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="a38aa-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="a38aa-179">在 [**加入檢視**] 對話方塊中，檢查標示的核取方塊**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="a38aa-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="a38aa-180">選取的電影類別**檢視資料類別**。</span><span class="sxs-lookup"><span data-stu-id="a38aa-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="a38aa-181">選取 *清單*作為**檢視內容**（請參閱 圖 6）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="a38aa-182">選取這些選項會產生強型別 檢視顯示的電影清單。</span><span class="sxs-lookup"><span data-stu-id="a38aa-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="a38aa-183">[![[新增檢視] 對話方塊](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a38aa-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="a38aa-184">**圖 06**: 新增檢視對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="a38aa-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="a38aa-185">按一下 [之後**新增**列表 2 中的檢視] 按鈕，會自動產生。</span><span class="sxs-lookup"><span data-stu-id="a38aa-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="a38aa-186">這個檢視包含程式碼，以逐一查看集合的電影，並顯示每個電影的屬性。</span><span class="sxs-lookup"><span data-stu-id="a38aa-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="a38aa-187">**列表 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a38aa-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="a38aa-188">您可以選取功能表選項來執行應用程式**偵錯，請啟動偵錯**（或按下 F5 鍵）。</span><span class="sxs-lookup"><span data-stu-id="a38aa-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="a38aa-189">執行應用程式會啟動 Internet Explorer。</span><span class="sxs-lookup"><span data-stu-id="a38aa-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="a38aa-190">如果您導覽至 /Movie URL，您會看到 [圖 7] 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="a38aa-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="a38aa-191">[![之電影資料表](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a38aa-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="a38aa-192">**圖 07**： 的電影資料表 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="a38aa-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="a38aa-193">如果您不喜歡方格圖 7 中的資料庫記錄的外觀相關的任何項目然後您就可以直接修改 索引 檢視。</span><span class="sxs-lookup"><span data-stu-id="a38aa-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="a38aa-194">例如，您可以變更*DateReleased*標頭*發行日期*藉由修改 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="a38aa-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="a38aa-195">使用部分建立範本</span><span class="sxs-lookup"><span data-stu-id="a38aa-195">Create a Template with a Partial</span></span>

<span data-ttu-id="a38aa-196">當檢視取得太複雜時，最好先啟動分成部分檢視。</span><span class="sxs-lookup"><span data-stu-id="a38aa-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="a38aa-197">使用部分，可讓您檢視了解和維護變得更加容易。</span><span class="sxs-lookup"><span data-stu-id="a38aa-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="a38aa-198">我們將會建立部分我們可以使用做為範本來設定每個電影資料庫記錄的格式。</span><span class="sxs-lookup"><span data-stu-id="a38aa-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="a38aa-199">請遵循下列步驟來建立部分：</span><span class="sxs-lookup"><span data-stu-id="a38aa-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="a38aa-200">Views\Movie 資料夾上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="a38aa-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="a38aa-201">選取標示的核取方塊*建立部分檢視 (.ascx)*。</span><span class="sxs-lookup"><span data-stu-id="a38aa-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="a38aa-202">命名部分*MovieTemplate*。</span><span class="sxs-lookup"><span data-stu-id="a38aa-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="a38aa-203">選取標示的核取方塊**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="a38aa-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="a38aa-204">選取將電影當作*檢視資料類別*。</span><span class="sxs-lookup"><span data-stu-id="a38aa-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="a38aa-205">選取 為空白*檢視內容*。</span><span class="sxs-lookup"><span data-stu-id="a38aa-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="a38aa-206">按一下 **新增**按鈕，將部分加入至您的專案。</span><span class="sxs-lookup"><span data-stu-id="a38aa-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="a38aa-207">完成這些步驟之後，修改部分 MovieTemplate 看起來像是列表 3。</span><span class="sxs-lookup"><span data-stu-id="a38aa-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="a38aa-208">**列表 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="a38aa-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="a38aa-209">在 列表 3 部分包含記錄的單一資料列的範本。</span><span class="sxs-lookup"><span data-stu-id="a38aa-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="a38aa-210">在 列表 4 中已修改的 索引 檢視會使用 MovieTemplate 部分。</span><span class="sxs-lookup"><span data-stu-id="a38aa-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="a38aa-211">**列表 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a38aa-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="a38aa-212">列表 4 中的檢視包含一個 For Each 迴圈來逐一查看所有的影片。</span><span class="sxs-lookup"><span data-stu-id="a38aa-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="a38aa-213">每一部電影，部分 MovieTemplate 用來格式化影片。</span><span class="sxs-lookup"><span data-stu-id="a38aa-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="a38aa-214">MovieTemplate 會藉由呼叫 RenderPartial() helper 方法轉譯。</span><span class="sxs-lookup"><span data-stu-id="a38aa-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="a38aa-215">修改過的 [索引] 檢視會呈現相同的 HTML 資料表的資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="a38aa-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="a38aa-216">不過，已大幅簡化檢視。</span><span class="sxs-lookup"><span data-stu-id="a38aa-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="a38aa-217">RenderPartial() 方法是不同於大部分其他的 helper 方法，因為它不會傳回字串。</span><span class="sxs-lookup"><span data-stu-id="a38aa-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="a38aa-218">因此，您必須呼叫 RenderPartial() 方法使用&lt;%html.renderpartial()&gt;而不是&lt;%= Html.RenderPartial() %&gt;。</span><span class="sxs-lookup"><span data-stu-id="a38aa-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="a38aa-219">總結</span><span class="sxs-lookup"><span data-stu-id="a38aa-219">Summary</span></span>

<span data-ttu-id="a38aa-220">本教學課程的目標是要說明如何顯示一組資料庫記錄，以及在 HTML 表格中。</span><span class="sxs-lookup"><span data-stu-id="a38aa-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="a38aa-221">首先，您已了解如何利用 Microsoft Entity Framework，從控制器動作傳回一組資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="a38aa-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="a38aa-222">接下來，您已了解如何使用 Visual Studio scaffolding 來產生自動顯示的項目集合的檢視。</span><span class="sxs-lookup"><span data-stu-id="a38aa-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="a38aa-223">最後，您已了解如何利用部分簡化檢視。</span><span class="sxs-lookup"><span data-stu-id="a38aa-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="a38aa-224">您已了解如何使用部分做為範本，以便您可以格式化每個資料庫的記錄。</span><span class="sxs-lookup"><span data-stu-id="a38aa-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a38aa-225">[上一頁](creating-model-classes-with-linq-to-sql-vb.md)
> [下一頁](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a38aa-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>
