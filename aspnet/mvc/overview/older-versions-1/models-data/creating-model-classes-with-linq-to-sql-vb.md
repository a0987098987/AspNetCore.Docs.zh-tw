---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: 建立模型類別搭配 LINQ to SQL (VB) |Microsoft 文件
author: microsoft
description: 本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。 在本教學課程中，您可以了解如何建置模型 c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 5438838123c40d82afbda191a48878d6dca80736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874807"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a><span data-ttu-id="1af56-104">使用 LINQ to SQL (VB) 建立模型類別</span><span class="sxs-lookup"><span data-stu-id="1af56-104">Creating Model Classes with LINQ to SQL (VB)</span></span>
====================
<span data-ttu-id="1af56-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1af56-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="1af56-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="1af56-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> <span data-ttu-id="1af56-107">本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-107">The goal of this tutorial is to explain one method of creating model classes for an ASP.NET MVC application.</span></span> <span data-ttu-id="1af56-108">在本教學課程中，您可以了解如何建置模型類別，並藉由運用 Microsoft LINQ to SQL 中執行資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="1af56-108">In this tutorial, you learn how to build model classes and perform database access by taking advantage of Microsoft LINQ to SQL.</span></span>


<span data-ttu-id="1af56-109">本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-109">The goal of this tutorial is to explain one method of creating model classes for an ASP.NET MVC application.</span></span> <span data-ttu-id="1af56-110">在本教學課程中，您可以了解如何建置模型類別，並藉由運用 Microsoft LINQ to SQL 中執行資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="1af56-110">In this tutorial, you learn how to build model classes and perform database access by taking advantage of Microsoft LINQ to SQL.</span></span>

<span data-ttu-id="1af56-111">在本教學課程中，我們會建置基本的電影資料庫應用程式。</span><span class="sxs-lookup"><span data-stu-id="1af56-111">In this tutorial, we build a basic Movie database application.</span></span> <span data-ttu-id="1af56-112">我們先建立電影資料庫應用程式中的最快速且最簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="1af56-112">We start by creating the Movie database application in the fastest and easiest way possible.</span></span> <span data-ttu-id="1af56-113">我們會執行所有存取我們的資料直接從我們控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="1af56-113">We perform all of our data access directly from our controller actions.</span></span>

<span data-ttu-id="1af56-114">接下來，您會了解如何使用儲存機制模式。</span><span class="sxs-lookup"><span data-stu-id="1af56-114">Next, you learn how to use the Repository pattern.</span></span> <span data-ttu-id="1af56-115">使用儲存機制模式將需要更多的工作。</span><span class="sxs-lookup"><span data-stu-id="1af56-115">Using the Repository pattern requires a little more work.</span></span> <span data-ttu-id="1af56-116">不過，採用這個模式的優點是它可讓您建置應用程式，則具有可調適性，若要變更，並可以輕鬆地測試。</span><span class="sxs-lookup"><span data-stu-id="1af56-116">However, the advantage of adopting this pattern is that it enables you to build applications that are adaptable to change and can be easily tested.</span></span>

## <a name="what-is-a-model-class"></a><span data-ttu-id="1af56-117">模型類別為何？</span><span class="sxs-lookup"><span data-stu-id="1af56-117">What is a Model Class?</span></span>

<span data-ttu-id="1af56-118">MVC 模型包含的所有未包含在 MVC 檢視或 MVC 控制器中的應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="1af56-118">An MVC model contains all of the application logic that is not contained in an MVC view or MVC controller.</span></span> <span data-ttu-id="1af56-119">特別是，MVC 模型包含的所有應用程式商務和資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="1af56-119">In particular, an MVC model contains all of your application business and data access logic.</span></span>

<span data-ttu-id="1af56-120">您可以使用各種不同的技術來實作資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="1af56-120">You can use a variety of different technologies to implement your data access logic.</span></span> <span data-ttu-id="1af56-121">例如，您可以建立資料存取類別使用 Microsoft Entity Framework、 NHibernate、 Subsonic 或 ADO.NET 類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-121">For example, you can build your data access classes using the Microsoft Entity Framework, NHibernate, Subsonic, or ADO.NET classes.</span></span>

<span data-ttu-id="1af56-122">在本教學課程中，我可以使用 LINQ to SQL 查詢，並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="1af56-122">In this tutorial, I use LINQ to SQL to query and update the database.</span></span> <span data-ttu-id="1af56-123">LINQ to SQL 提供非常簡單的方法，與 Microsoft SQL Server 資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="1af56-123">LINQ to SQL provides you with a very easy method of interacting with a Microsoft SQL Server database.</span></span> <span data-ttu-id="1af56-124">不過，務必了解，ASP.NET MVC 架構未繫結至 LINQ to SQL 以任何方式。</span><span class="sxs-lookup"><span data-stu-id="1af56-124">However, it is important to understand that the ASP.NET MVC framework is not tied to LINQ to SQL in any way.</span></span> <span data-ttu-id="1af56-125">ASP.NET MVC 是與任何資料存取技術相容。</span><span class="sxs-lookup"><span data-stu-id="1af56-125">ASP.NET MVC is compatible with any data access technology.</span></span>

## <a name="create-a-movie-database"></a><span data-ttu-id="1af56-126">建立電影資料庫</span><span class="sxs-lookup"><span data-stu-id="1af56-126">Create a Movie Database</span></span>

<span data-ttu-id="1af56-127">在此教學課程--以說明如何建置模型類別-我們建置簡單的電影資料庫應用程式。</span><span class="sxs-lookup"><span data-stu-id="1af56-127">In this tutorial -- in order to illustrate how you can build model classes -- we build a simple Movie database application.</span></span> <span data-ttu-id="1af56-128">第一個步驟是建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="1af56-128">The first step is to create a new database.</span></span> <span data-ttu-id="1af56-129">以滑鼠右鍵按一下應用程式\_Data 資料夾中的方案總管 視窗，然後選取功能表選項**新增、 新項目**。</span><span class="sxs-lookup"><span data-stu-id="1af56-129">Right-click the App\_Data folder in the Solution Explorer window and select the menu option **Add, New Item**.</span></span> <span data-ttu-id="1af56-130">選取的 SQL Server 資料庫範本，讓它名稱 MoviesDB.mdf，再按一下**新增**按鈕 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="1af56-130">Select the SQL Server Database template, give it the name MoviesDB.mdf, and click the **Add** button (see Figure 1).</span></span>


<span data-ttu-id="1af56-131">[![加入新的 SQL Server 資料庫](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1af56-131">[![Adding a new SQL Server Database](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)</span></span>

<span data-ttu-id="1af56-132">**圖 01**： 加入新的 SQL Server 資料庫 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1af56-132">**Figure 01**: Adding a new SQL Server Database ([Click to view full-size image](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))</span></span>


<span data-ttu-id="1af56-133">建立新的資料庫之後，您可以開啟資料庫應用程式中按兩下 MoviesDB.mdf\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="1af56-133">After you create the new database, you can open the database by double-clicking the MoviesDB.mdf file in the App\_Data folder.</span></span> <span data-ttu-id="1af56-134">按兩下 MoviesDB.mdf 檔案會開啟 [伺服器總管] 視窗 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="1af56-134">Double-clicking the MoviesDB.mdf file opens the Server Explorer window (see Figure 2).</span></span>


|   | <span data-ttu-id="1af56-135">[伺服器總管] 視窗使用 Visual Web Developer 中時呼叫 [資料庫總管] 視窗。</span><span class="sxs-lookup"><span data-stu-id="1af56-135">The Server Explorer window is called the Database Explorer window when using Visual Web Developer.</span></span> |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

<span data-ttu-id="1af56-136">[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1af56-136">[![Using the Server Explorer window](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)</span></span>

<span data-ttu-id="1af56-137">**圖 02**： 使用 [伺服器總管] 視窗 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1af56-137">**Figure 02**: Using the Server Explorer window ([Click to view full-size image](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))</span></span>


<span data-ttu-id="1af56-138">我們需要將一個資料表加入至資料庫，表示我們電影。</span><span class="sxs-lookup"><span data-stu-id="1af56-138">We need to add one table to our database that represents our movies.</span></span> <span data-ttu-id="1af56-139">以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="1af56-139">Right-click the Tables folder and select the menu option **Add New Table**.</span></span> <span data-ttu-id="1af56-140">選取此功能表選項會開啟 資料表設計工具 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="1af56-140">Selecting this menu option opens the Table Designer (see Figure 3).</span></span>


<span data-ttu-id="1af56-141">[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1af56-141">[![Using the Server Explorer window](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)</span></span>

<span data-ttu-id="1af56-142">**圖 03**： 資料表設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="1af56-142">**Figure 03**: The Table Designer ([Click to view full-size image](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))</span></span>


<span data-ttu-id="1af56-143">我們需要將下列資料行加入至資料庫資料表：</span><span class="sxs-lookup"><span data-stu-id="1af56-143">We need to add the following columns to our database table:</span></span>

| <span data-ttu-id="1af56-144">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="1af56-144">**Column Name**</span></span> | <span data-ttu-id="1af56-145">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="1af56-145">**Data Type**</span></span> | <span data-ttu-id="1af56-146">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="1af56-146">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1af56-147">ID</span><span class="sxs-lookup"><span data-stu-id="1af56-147">Id</span></span> | <span data-ttu-id="1af56-148">Int</span><span class="sxs-lookup"><span data-stu-id="1af56-148">Int</span></span> | <span data-ttu-id="1af56-149">False</span><span class="sxs-lookup"><span data-stu-id="1af56-149">False</span></span> |
| <span data-ttu-id="1af56-150">標題</span><span class="sxs-lookup"><span data-stu-id="1af56-150">Title</span></span> | <span data-ttu-id="1af56-151">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="1af56-151">Nvarchar(200)</span></span> | <span data-ttu-id="1af56-152">False</span><span class="sxs-lookup"><span data-stu-id="1af56-152">False</span></span> |
| <span data-ttu-id="1af56-153">導向器</span><span class="sxs-lookup"><span data-stu-id="1af56-153">Director</span></span> | <span data-ttu-id="1af56-154">Nvarchar （50)</span><span class="sxs-lookup"><span data-stu-id="1af56-154">Nvarchar(50)</span></span> | <span data-ttu-id="1af56-155">False</span><span class="sxs-lookup"><span data-stu-id="1af56-155">False</span></span> |

<span data-ttu-id="1af56-156">您需要執行兩個特殊的動作識別碼資料行。</span><span class="sxs-lookup"><span data-stu-id="1af56-156">You need to do two special things to the Id column.</span></span> <span data-ttu-id="1af56-157">首先，您必須將標示為主要索引鍵資料行的識別碼資料行，資料表設計工具中選取資料行，然後按一下 索引鍵的圖示。</span><span class="sxs-lookup"><span data-stu-id="1af56-157">First, you need to mark the Id column as a primary key column by selecting the column in the Table Designer and clicking the icon of a key.</span></span> <span data-ttu-id="1af56-158">LINQ to SQL 會要求您執行插入或更新資料庫時，指定主要的索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="1af56-158">LINQ to SQL requires you to specify your primary key columns when performing inserts or updates against the database.</span></span>

<span data-ttu-id="1af56-159">接下來，您必須將標示為識別資料行的識別碼資料行所指定的值給**為識別**屬性 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="1af56-159">Next, you need to mark the Id column as an Identity column by assigning the value Yes to the **Is Identity** property (see Figure 3).</span></span> <span data-ttu-id="1af56-160">識別資料行是指派新的數字自動每當您將新的資料列加入資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="1af56-160">An Identity column is a column that is assigned a new number automatically whenever you add a new row of data to a table.</span></span>

<span data-ttu-id="1af56-161">進行這些變更之後，請使用名稱 tblMovie 儲存資料表。</span><span class="sxs-lookup"><span data-stu-id="1af56-161">After you make these changes, save the table with the name tblMovie.</span></span> <span data-ttu-id="1af56-162">您可以依序按一下 [儲存] 按鈕儲存資料表。</span><span class="sxs-lookup"><span data-stu-id="1af56-162">You can save the table by clicking the Save button.</span></span>

## <a name="create-linq-to-sql-classes"></a><span data-ttu-id="1af56-163">建立 LINQ to SQL 類別</span><span class="sxs-lookup"><span data-stu-id="1af56-163">Create LINQ to SQL Classes</span></span>

<span data-ttu-id="1af56-164">我們的 MVC 模型會包含 LINQ to SQL 類別代表 tblMovie 資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="1af56-164">Our MVC model will contain LINQ to SQL classes that represent the tblMovie database table.</span></span> <span data-ttu-id="1af56-165">若要建立這些 LINQ to SQL 類別最簡單方式是以滑鼠右鍵按一下 模型 資料夾中，選取**新增、 新項目**，選取 LINQ to SQL 類別範本，將類別命名 Movie.dbml，然後按一下**新增**按鈕 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="1af56-165">The easiest way to create these LINQ to SQL classes is to right-click the Models folder, select **Add, New Item**, select the LINQ to SQL Classes template, give the classes the name Movie.dbml, and click the **Add** button (see Figure 4).</span></span>


<span data-ttu-id="1af56-166">[![建立 LINQ to SQL 類別](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="1af56-166">[![Creating LINQ to SQL classes](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)</span></span>

<span data-ttu-id="1af56-167">**圖 04**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="1af56-167">**Figure 04**: Creating LINQ to SQL classes ([Click to view full-size image](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))</span></span>


<span data-ttu-id="1af56-168">立即建立電影 LINQ to SQL 類別之後，便會顯示物件關聯式設計工具。</span><span class="sxs-lookup"><span data-stu-id="1af56-168">Immediately after you create the Movie LINQ to SQL Classes, the Object Relational Designer appears.</span></span> <span data-ttu-id="1af56-169">您可以從伺服器總管] 視窗拖曳至 [物件關聯式設計工具建立 LINQ to SQL 類別代表特定的資料庫資料表拖曳資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="1af56-169">You can drag database tables from the Server Explorer window onto the Object Relational Designer to create LINQ to SQL Classes that represent particular database tables.</span></span> <span data-ttu-id="1af56-170">我們需要加入 tblMovie 資料庫資料表拖曳至 物件關聯式設計工具 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="1af56-170">We need to add the tblMovie database table onto the Object Relational Designer (see Figure 4).</span></span>


<span data-ttu-id="1af56-171">[![使用物件關聯式設計工具](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="1af56-171">[![Using the Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)</span></span>

<span data-ttu-id="1af56-172">**圖 05**： 使用物件關聯式設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="1af56-172">**Figure 05**: Using the Object Relational Designer ([Click to view full-size image](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))</span></span>


<span data-ttu-id="1af56-173">根據預設，物件關聯式設計工具建立資料庫資料表拖曳至設計工具的非常與同名的類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-173">By default, the Object Relational Designer creates a class with the very same name as the database table that you drag onto the Designer.</span></span> <span data-ttu-id="1af56-174">不過，我們不想要呼叫我們類別 tblMovie。</span><span class="sxs-lookup"><span data-stu-id="1af56-174">However, we don't want to call our class tblMovie.</span></span> <span data-ttu-id="1af56-175">因此，按一下 類別設計工具中的名稱，將類別的名稱變更為影片。</span><span class="sxs-lookup"><span data-stu-id="1af56-175">Therefore, click the name of the class in the Designer and change the name of the class to Movie.</span></span>

<span data-ttu-id="1af56-176">最後，請記得要按一下**儲存**按鈕 （磁片的圖片） 儲存 LINQ to SQL 類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-176">Finally, remember to click the **Save** button (the picture of the floppy) to save the LINQ to SQL Classes.</span></span> <span data-ttu-id="1af56-177">否則，LINQ to SQL 類別不會產生的物件關聯式設計工具。</span><span class="sxs-lookup"><span data-stu-id="1af56-177">Otherwise, the LINQ to SQL Classes won't be generated by the Object Relational Designer.</span></span>

## <a name="using-linq-to-sql-in-a-controller-action"></a><span data-ttu-id="1af56-178">使用 LINQ to SQL 中的控制器動作</span><span class="sxs-lookup"><span data-stu-id="1af56-178">Using LINQ to SQL in a Controller Action</span></span>

<span data-ttu-id="1af56-179">現在，我們已經我們 LINQ to SQL 類別時，我們可以使用這些類別，從資料庫擷取資料。</span><span class="sxs-lookup"><span data-stu-id="1af56-179">Now that we have our LINQ to SQL classes, we can use these classes to retrieve data from the database.</span></span> <span data-ttu-id="1af56-180">在本節中，您可以了解如何使用 LINQ to SQL 類別，直接在控制器動作。</span><span class="sxs-lookup"><span data-stu-id="1af56-180">In this section, you learn how to use LINQ to SQL classes directly within a controller action.</span></span> <span data-ttu-id="1af56-181">我們將 MVC 檢視中顯示 tblMovies 資料庫資料表中的電影的清單。</span><span class="sxs-lookup"><span data-stu-id="1af56-181">We'll display the list of movies from the tblMovies database table in an MVC view.</span></span>

<span data-ttu-id="1af56-182">首先，我們需要修改 HomeController 類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-182">First, we need to modify the HomeController class.</span></span> <span data-ttu-id="1af56-183">這個類別可以找到您的應用程式的 [控制器] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="1af56-183">This class can be found in the Controllers folder of your application.</span></span> <span data-ttu-id="1af56-184">修改類別，讓它看起來像是列表 1 中的類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-184">Modify the class so it looks like the class in Listing 1.</span></span>

<span data-ttu-id="1af56-185">**列出 1 – `Controllers\HomeController.vb`**</span><span class="sxs-lookup"><span data-stu-id="1af56-185">**Listing 1 – `Controllers\HomeController.vb`**</span></span>

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

<span data-ttu-id="1af56-186">Index （） 中的動作清單 1 來表示 MoviesDB 資料庫使用 LINQ to SQL DataContext 類別 (MovieDataContext)。</span><span class="sxs-lookup"><span data-stu-id="1af56-186">The Index() action in Listing 1 uses a LINQ to SQL DataContext class (the MovieDataContext) to represent the MoviesDB database.</span></span> <span data-ttu-id="1af56-187">MoveDataContext 類別是由 Visual Studio 物件關聯式設計工具產生。</span><span class="sxs-lookup"><span data-stu-id="1af56-187">The MoveDataContext class was generated by the Visual Studio Object Relational Designer.</span></span>

<span data-ttu-id="1af56-188">LINQ 查詢會針對從 tblMovies 資料庫資料表擷取所有的電影 DataContext 執行。</span><span class="sxs-lookup"><span data-stu-id="1af56-188">A LINQ query is performed against the DataContext to retrieve all of the movies from the tblMovies database table.</span></span> <span data-ttu-id="1af56-189">電影清單會指派給名為電影的本機變數。</span><span class="sxs-lookup"><span data-stu-id="1af56-189">The list of movies is assigned to a local variable named movies.</span></span> <span data-ttu-id="1af56-190">最後的電影清單會傳遞至檢視檢視資料。</span><span class="sxs-lookup"><span data-stu-id="1af56-190">Finally, the list of movies is passed to the view through view data.</span></span>

<span data-ttu-id="1af56-191">若要顯示電影，我們接下來要修改索引檢視表。</span><span class="sxs-lookup"><span data-stu-id="1af56-191">In order to show the movies, we next need to modify the Index view.</span></span> <span data-ttu-id="1af56-192">您可以找到的索引檢視 Views\Home\ 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="1af56-192">You can find the Index view in the Views\Home\ folder.</span></span> <span data-ttu-id="1af56-193">更新索引檢視，讓它看起來像是列表 2 中的檢視。</span><span class="sxs-lookup"><span data-stu-id="1af56-193">Update the Index view so that it looks like the view in Listing 2.</span></span>

<span data-ttu-id="1af56-194">**列出 2 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="1af56-194">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

<span data-ttu-id="1af56-195">請注意，修改的索引檢視表包含&lt;%@ 匯入命名空間 %&gt;指示詞上方的檢視。</span><span class="sxs-lookup"><span data-stu-id="1af56-195">Notice that the modified Index view includes an &lt;%@ import namespace %&gt; directive at the top of the view.</span></span> <span data-ttu-id="1af56-196">這個指示詞會匯入 MvcApplication1 命名空間。</span><span class="sxs-lookup"><span data-stu-id="1af56-196">This directive imports the MvcApplication1 namespace.</span></span> <span data-ttu-id="1af56-197">我們需要此命名空間，才能使用模型類別，尤其是，在檢視中的影片類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-197">We need this namespace in order to work with the model classes – in particular, the Movie class -- in the view.</span></span>

<span data-ttu-id="1af56-198">列出 2 中的檢視包含 For each 逐一查看所有 ViewData.Model 屬性所代表的項目。</span><span class="sxs-lookup"><span data-stu-id="1af56-198">The view in Listing 2 contains a For Each loop that iterates through all of the items represented by the ViewData.Model property.</span></span> <span data-ttu-id="1af56-199">Title 屬性的值會顯示每個影片。</span><span class="sxs-lookup"><span data-stu-id="1af56-199">The value of the Title property is displayed for each movie.</span></span>

<span data-ttu-id="1af56-200">請注意 ViewData.Model 屬性的值會轉換成 IEnumerable。</span><span class="sxs-lookup"><span data-stu-id="1af56-200">Notice that the value of the ViewData.Model property is cast to an IEnumerable.</span></span> <span data-ttu-id="1af56-201">這是必要的 ViewData.Model 內容執行迴圈。</span><span class="sxs-lookup"><span data-stu-id="1af56-201">This is necessary in order to loop through the contents of ViewData.Model.</span></span> <span data-ttu-id="1af56-202">這裡的另一個選項是建立強型別檢視。</span><span class="sxs-lookup"><span data-stu-id="1af56-202">Another option here is to create a strongly-typed view.</span></span> <span data-ttu-id="1af56-203">建立強型別檢視時，您會轉換至特定類型的 ViewData.Model 屬性檢視的程式碼後置類別中。</span><span class="sxs-lookup"><span data-stu-id="1af56-203">When you create a strongly-typed view, you cast the ViewData.Model property to a particular type in a view's code-behind class.</span></span>

<span data-ttu-id="1af56-204">如果您執行應用程式之後修改 HomeController 類別和索引檢視，然後就會出現空白頁面。</span><span class="sxs-lookup"><span data-stu-id="1af56-204">If you run the application after modifying the HomeController class and the Index view then you will get a blank page.</span></span> <span data-ttu-id="1af56-205">因為沒有影片記錄 tblMovies 資料庫資料表中，您會得到空白頁。</span><span class="sxs-lookup"><span data-stu-id="1af56-205">You'll get a blank page because there are no movie records in the tblMovies database table.</span></span>

<span data-ttu-id="1af56-206">若要將記錄新增至 tblMovies 資料庫資料表，tblMovies 資料庫資料表，在 [伺服器總管] 視窗 （在 Visual Web Developer 的 [資料庫總管] 視窗） 中以滑鼠右鍵按一下並選取功能表選項**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="1af56-206">In order to add records to the tblMovies database table, right-click the tblMovies database table in the Server Explorer window (Database Explorer window in Visual Web Developer) and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="1af56-207">您可以插入影片記錄所使用的方格，會出現 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="1af56-207">You can insert movie records by using the grid that appears (see Figure 5).</span></span>


<span data-ttu-id="1af56-208">[![插入影片](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="1af56-208">[![Inserting movies](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)</span></span>

<span data-ttu-id="1af56-209">**圖 06**： 插入影片 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="1af56-209">**Figure 06**: Inserting movies([Click to view full-size image](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))</span></span>


<span data-ttu-id="1af56-210">您將某些資料庫記錄加入 tblMovies 資料表，並執行應用程式之後，您會看到圖 7 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="1af56-210">After you add some database records to the tblMovies table, and you run the application, you'll see the page in Figure 7.</span></span> <span data-ttu-id="1af56-211">所有的電影資料庫記錄會顯示在項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="1af56-211">All of the movie database records are displayed in a bulleted list.</span></span>


<span data-ttu-id="1af56-212">[![顯示與索引檢視的電影](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="1af56-212">[![Displaying movies with the Index view](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)</span></span>

<span data-ttu-id="1af56-213">**圖 07**： 顯示與索引檢視的影片 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="1af56-213">**Figure 07**: Displaying movies with the Index view([Click to view full-size image](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))</span></span>


## <a name="using-the-repository-pattern"></a><span data-ttu-id="1af56-214">使用儲存機制模式</span><span class="sxs-lookup"><span data-stu-id="1af56-214">Using the Repository Pattern</span></span>

<span data-ttu-id="1af56-215">在上一節中，我們會使用 LINQ to SQL 類別，直接在控制器動作。</span><span class="sxs-lookup"><span data-stu-id="1af56-215">In the previous section, we used LINQ to SQL classes directly within a controller action.</span></span> <span data-ttu-id="1af56-216">我們使用 MovieDataContext 類別，直接從 index （） 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="1af56-216">We used the MovieDataContext class directly from the Index() controller action.</span></span> <span data-ttu-id="1af56-217">並沒有任何在簡單的應用程式的情況下執行這個問題。</span><span class="sxs-lookup"><span data-stu-id="1af56-217">There is nothing wrong with doing this in the case of a simple application.</span></span> <span data-ttu-id="1af56-218">不過，在控制器類別中的直接使用 LINQ to SQL 工作會產生問題時您需要建置更複雜的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1af56-218">However, working directly with LINQ to SQL in a controller class creates problems when you need to build a more complex application.</span></span>

<span data-ttu-id="1af56-219">控制器類別中使用 LINQ to SQL 難以未來切換資料存取技術。</span><span class="sxs-lookup"><span data-stu-id="1af56-219">Using LINQ to SQL within a controller class makes it difficult to switch data access technologies in the future.</span></span> <span data-ttu-id="1af56-220">例如，您可能決定切換使用為您的資料存取技術的 Microsoft Entity Framework 使用 Microsoft LINQ to SQL。</span><span class="sxs-lookup"><span data-stu-id="1af56-220">For example, you might decide to switch from using Microsoft LINQ to SQL to using the Microsoft Entity Framework as your data access technology.</span></span> <span data-ttu-id="1af56-221">在此情況下，您需要重寫存取的資料庫應用程式內的每個控制站。</span><span class="sxs-lookup"><span data-stu-id="1af56-221">In that case, you would need to rewrite every controller that accesses the database within your application.</span></span>

<span data-ttu-id="1af56-222">使用 LINQ to SQL 中控制器類別也很難建置您的應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="1af56-222">Using LINQ to SQL within a controller class also makes it difficult to build unit tests for your application.</span></span> <span data-ttu-id="1af56-223">一般來說，您不想執行單元測試時，與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="1af56-223">Normally, you do not want to interact with a database when performing unit tests.</span></span> <span data-ttu-id="1af56-224">您想要使用您的單元測試來測試應用程式邏輯而非您資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="1af56-224">You want to use your unit tests to test your application logic and not your database server.</span></span>

<span data-ttu-id="1af56-225">若要建立的 MVC 應用程式更適應未來的變更和可以更輕鬆地測試，您應該考慮使用儲存機制模式。</span><span class="sxs-lookup"><span data-stu-id="1af56-225">In order to build an MVC application that is more adaptable to future change and that can be more easily tested, you should consider using the Repository pattern.</span></span> <span data-ttu-id="1af56-226">當您使用儲存機制模式時，您會建立個別的儲存機制類別，其中包含所有您資料庫的存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="1af56-226">When you use the Repository pattern, you create a separate repository class that contains all of your database access logic.</span></span>

<span data-ttu-id="1af56-227">當您建立的儲存機制類別時，您會建立介面，代表所有儲存機制類別所使用的方法。</span><span class="sxs-lookup"><span data-stu-id="1af56-227">When you create the repository class, you create an interface that represents all of the methods used by the repository class.</span></span> <span data-ttu-id="1af56-228">中的控制站，您可以撰寫您的程式碼，而不是儲存機制介面。</span><span class="sxs-lookup"><span data-stu-id="1af56-228">Within your controllers, you write your code against the interface instead of the repository.</span></span> <span data-ttu-id="1af56-229">這樣一來，您可以實作在未來使用不同的資料存取技術的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="1af56-229">That way, you can implement the repository using different data access technologies in the future.</span></span>

<span data-ttu-id="1af56-230">介面中列出的 3 為 IMovieRepository，它代表名為 ListAll() 的單一方法。</span><span class="sxs-lookup"><span data-stu-id="1af56-230">The interface in Listing 3 is named IMovieRepository and it represents a single method named ListAll().</span></span>

<span data-ttu-id="1af56-231">**列出 3 – `Models\IMovieRepository.vb`**</span><span class="sxs-lookup"><span data-stu-id="1af56-231">**Listing 3 – `Models\IMovieRepository.vb`**</span></span>

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

<span data-ttu-id="1af56-232">儲存機制中的類別清單 4 實作 IMovieRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="1af56-232">The repository class in Listing 4 implements the IMovieRepository interface.</span></span> <span data-ttu-id="1af56-233">請注意，它會包含名為 ListAll() 對應至 IMovieRepository 介面所需的方法的方法。</span><span class="sxs-lookup"><span data-stu-id="1af56-233">Notice that it contains a method named ListAll() that corresponds to the method required by the IMovieRepository interface.</span></span>

<span data-ttu-id="1af56-234">**列出 4 – `Models\MovieRepository.vb`**</span><span class="sxs-lookup"><span data-stu-id="1af56-234">**Listing 4 – `Models\MovieRepository.vb`**</span></span>

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

<span data-ttu-id="1af56-235">最後，在列出 5 MoviesController 類別會使用儲存機制模式。</span><span class="sxs-lookup"><span data-stu-id="1af56-235">Finally, the MoviesController class in Listing 5 uses the Repository pattern.</span></span> <span data-ttu-id="1af56-236">它不會再使用 LINQ to SQL 類別直接。</span><span class="sxs-lookup"><span data-stu-id="1af56-236">It no longer uses LINQ to SQL classes directly.</span></span>

<span data-ttu-id="1af56-237">**列出 5 – `Controllers\MoviesController.vb`**</span><span class="sxs-lookup"><span data-stu-id="1af56-237">**Listing 5 – `Controllers\MoviesController.vb`**</span></span>

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

<span data-ttu-id="1af56-238">請注意在列出 5 MoviesController 類別有兩個建構函式。</span><span class="sxs-lookup"><span data-stu-id="1af56-238">Notice that the MoviesController class in Listing 5 has two constructors.</span></span> <span data-ttu-id="1af56-239">應用程式執行時，會呼叫第一個建構函式，無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="1af56-239">The first constructor, the parameterless constructor, is called when your application is running.</span></span> <span data-ttu-id="1af56-240">這個建構函式建立 MovieRepository 類別的執行個體，並將其傳遞給第二個建構函式。</span><span class="sxs-lookup"><span data-stu-id="1af56-240">This constructor creates an instance of the MovieRepository class and passes it to the second constructor.</span></span>

<span data-ttu-id="1af56-241">第二個建構函式具有單一參數： IMovieRepository 參數。</span><span class="sxs-lookup"><span data-stu-id="1af56-241">The second constructor has a single parameter: an IMovieRepository parameter.</span></span> <span data-ttu-id="1af56-242">這個建構函式只會將參數的值指派給名為的類別層級欄位\_儲存機制。</span><span class="sxs-lookup"><span data-stu-id="1af56-242">This constructor simply assigns the value of the parameter to a class-level field named \_repository.</span></span>

<span data-ttu-id="1af56-243">MoviesController 類別利用一種軟體設計模式，呼叫相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="1af56-243">The MoviesController class is taking advantage of a software design pattern called the Dependency Injection pattern.</span></span> <span data-ttu-id="1af56-244">特別是，它使用呼叫建構函式相依性插入的項目。</span><span class="sxs-lookup"><span data-stu-id="1af56-244">In particular, it is using something called Constructor Dependency Injection.</span></span> <span data-ttu-id="1af56-245">閱讀更多有關閱讀下列文章依 Martin Fowler 此模式：</span><span class="sxs-lookup"><span data-stu-id="1af56-245">You can read more about this pattern by reading the following article by Martin Fowler:</span></span>

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

<span data-ttu-id="1af56-246">請注意，所有 MoviesController 類別 （除了第一個建構函式） 中的程式碼與 IMovieRepository 介面，而不是實際 MovieRepository 類別互動。</span><span class="sxs-lookup"><span data-stu-id="1af56-246">Notice that all of the code in the MoviesController class (with the exception of the first constructor) interacts with the IMovieRepository interface instead of the actual MovieRepository class.</span></span> <span data-ttu-id="1af56-247">程式碼互動的抽象介面，而不是介面的具象實作。</span><span class="sxs-lookup"><span data-stu-id="1af56-247">The code interacts with an abstract interface instead of a concrete implementation of the interface.</span></span>

<span data-ttu-id="1af56-248">如果您想要修改應用程式所使用的資料存取技術您可以直接實作的類別，會使用替代的資料庫存取技術的 IMovieRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="1af56-248">If you want to modify the data access technology used by the application then you can simply implement the IMovieRepository interface with a class that uses the alternative database access technology.</span></span> <span data-ttu-id="1af56-249">例如，您可以建立 EntityFrameworkMovieRepository 類別或 SubSonicMovieRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-249">For example, you could create an EntityFrameworkMovieRepository class or a SubSonicMovieRepository class.</span></span> <span data-ttu-id="1af56-250">控制器類別所撰寫的介面，因為您可以將 IMovieRepository 的新實作傳遞至控制器類別，類別會繼續運作。</span><span class="sxs-lookup"><span data-stu-id="1af56-250">Because the controller class is programmed against the interface, you can pass a new implementation of IMovieRepository to the controller class and the class would continue to work.</span></span>

<span data-ttu-id="1af56-251">此外，如果您想要測試 MoviesController 類別，則您可以傳遞假的電影儲存機制的類別來 MoviesController。</span><span class="sxs-lookup"><span data-stu-id="1af56-251">Furthermore, if you want to test the MoviesController class, then you can pass a fake movie repository class to the MoviesController.</span></span> <span data-ttu-id="1af56-252">您可以實作 IMovieRepository 類別實際上不會存取資料庫，但是包含所有必要的 IMovieRepository 介面方法的類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-252">You can implement the IMovieRepository class with a class that does not actually access the database but contains all of the required methods of the IMovieRepository interface.</span></span> <span data-ttu-id="1af56-253">這樣一來，進行單元測試 MoviesController 類別而不需要實際存取實際的資料庫。</span><span class="sxs-lookup"><span data-stu-id="1af56-253">That way, you can unit test the MoviesController class without actually accessing a real database.</span></span>

## <a name="summary"></a><span data-ttu-id="1af56-254">總結</span><span class="sxs-lookup"><span data-stu-id="1af56-254">Summary</span></span>

<span data-ttu-id="1af56-255">本教學課程的目標是為了示範如何建立 MVC 模型類別，藉由運用 Microsoft LINQ to SQL。</span><span class="sxs-lookup"><span data-stu-id="1af56-255">The goal of this tutorial was to demonstrate how you can create MVC model classes by taking advantage of Microsoft LINQ to SQL.</span></span> <span data-ttu-id="1af56-256">我們會檢查兩種策略，在 ASP.NET MVC 應用程式中顯示資料庫的資料。</span><span class="sxs-lookup"><span data-stu-id="1af56-256">We examined two strategies for displaying database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="1af56-257">首先，我們建立 LINQ to SQL 類別，並使用直接內控制器動作的類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-257">First, we created LINQ to SQL classes and used the classes directly within a controller action.</span></span> <span data-ttu-id="1af56-258">使用 LINQ to SQL 類別的控制器內，可讓您快速並輕鬆地在 MVC 應用程式中顯示資料庫的資料。</span><span class="sxs-lookup"><span data-stu-id="1af56-258">Using LINQ to SQL classes within a controller enables you to quickly and easily display database data in an MVC application.</span></span>

<span data-ttu-id="1af56-259">接下來，我們已探索的稍微更困難，但更明確地正向，顯示資料庫資料路徑。</span><span class="sxs-lookup"><span data-stu-id="1af56-259">Next, we explored a slightly more difficult, but definitely more virtuous, path for displaying database data.</span></span> <span data-ttu-id="1af56-260">我們已利用儲存機制模式，所有資料庫存取邏輯放在不同的儲存機制類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-260">We took advantage of the Repository pattern and placed all of our database access logic in a separate repository class.</span></span> <span data-ttu-id="1af56-261">中我們控制站，我們會寫入所有我們的程式碼，根據介面而不是具象類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-261">In our controller, we wrote all of our code against an interface instead of a concrete class.</span></span> <span data-ttu-id="1af56-262">儲存機制模式的優點是它可讓我們來輕鬆地在未來變更資料庫存取技術，可讓我們來輕鬆地測試控制器類別。</span><span class="sxs-lookup"><span data-stu-id="1af56-262">The advantage of the Repository pattern is that it enables us to easily change database access technologies in the future and it enables us to easily test our controller classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1af56-263">[上一頁](creating-model-classes-with-the-entity-framework-vb.md)
> [下一頁](displaying-a-table-of-database-data-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1af56-263">[Previous](creating-model-classes-with-the-entity-framework-vb.md)
[Next](displaying-a-table-of-database-data-vb.md)</span></span>
