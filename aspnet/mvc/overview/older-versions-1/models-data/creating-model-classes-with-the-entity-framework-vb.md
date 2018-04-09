---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: 使用 Entity Framework (VB) 建立模型類別 |Microsoft 文件
author: microsoft
description: 在本教學課程中，您可以了解如何使用 ASP.NET MVC 與 Microsoft Entity Framework。 您了解如何使用實體精靈來建立 ADO.NET 實體資料...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: 3442435c7b2b9ce2ce6bd016ba74fe671eb76f62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a><span data-ttu-id="cc7e5-104">使用 Entity Framework (VB) 建立模型類別</span><span class="sxs-lookup"><span data-stu-id="cc7e5-104">Creating Model Classes with the Entity Framework (VB)</span></span>
====================
<span data-ttu-id="cc7e5-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cc7e5-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="cc7e5-106">在本教學課程中，您可以了解如何使用 ASP.NET MVC 與 Microsoft Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-106">In this tutorial, you learn how to use ASP.NET MVC with the Microsoft Entity Framework.</span></span> <span data-ttu-id="cc7e5-107">您了解如何使用實體精靈來建立 ADO.NET 實體資料模型。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-107">You learn how to use the Entity Wizard to create an ADO.NET Entity Data Model.</span></span> <span data-ttu-id="cc7e5-108">在本教學課程的過程中，我們會建置說明如何選取、 插入、 更新和刪除資料庫的資料使用 Entity Framework 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-108">Over the course of this tutorial, we build a web application that illustrates how to select, insert, update, and delete database data by using the Entity Framework.</span></span>


<span data-ttu-id="cc7e5-109">本教學課程的目標是以說明如何建立建置 ASP.NET MVC 應用程式時使用 Microsoft Entity Framework 資料存取的類別。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-109">The goal of this tutorial is to explain how you can create data access classes using the Microsoft Entity Framework when building an ASP.NET MVC application.</span></span> <span data-ttu-id="cc7e5-110">本教學課程會假設 Microsoft Entity Framework 的任何先前的知識。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-110">This tutorial assumes no previous knowledge of the Microsoft Entity Framework.</span></span> <span data-ttu-id="cc7e5-111">本教學課程結束時，您將了解如何使用 Entity Framework 來選取、 插入、 更新和刪除資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-111">By the end of this tutorial, you'll understand how to use the Entity Framework to select, insert, update, and delete database records.</span></span>

<span data-ttu-id="cc7e5-112">Microsoft Entity Framework 是物件關聯式對應 (O/RM) 工具，可讓您從資料庫中自動產生的資料存取層。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-112">The Microsoft Entity Framework is an Object Relational Mapping (O/RM) tool that enables you to generate a data access layer from a database automatically.</span></span> <span data-ttu-id="cc7e5-113">Entity Framework 可讓您避免冗長工作以手動方式建立資料存取類別。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-113">The Entity Framework enables you to avoid the tedious work of building your data access classes by hand.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="cc7e5-114">沒有任何必要的連接之間 ASP.NET MVC 和 Microsoft Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-114">There is no essential connection between ASP.NET MVC and the Microsoft Entity Framework.</span></span> <span data-ttu-id="cc7e5-115">有多個替代方案，您可以搭配 ASP.NET MVC 中使用 Entity framework。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-115">There are several alternatives to the Entity Framework that you can use with ASP.NET MVC.</span></span> <span data-ttu-id="cc7e5-116">例如，您可以建立使用其他的 O/RM 工具，例如 Microsoft LINQ to SQL、 NHibernate 或 SubSonic MVC 模型類別。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-116">For example, you can build your MVC Model classes using other O/RM tools such as Microsoft LINQ to SQL, NHibernate, or SubSonic.</span></span>


<span data-ttu-id="cc7e5-117">若要說明如何使用 Microsoft Entity Framework 搭配 ASP.NET MVC，我們會建置一個簡單的範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-117">In order to illustrate how you can use the Microsoft Entity Framework with ASP.NET MVC, we'll build a simple sample application.</span></span> <span data-ttu-id="cc7e5-118">我們將建立電影資料庫應用程式可讓您顯示和編輯電影資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-118">We'll create a Movie Database application that enables you to display and edit movie database records.</span></span>

<span data-ttu-id="cc7e5-119">本教學課程假設您有 Visual Studio 2008 或 Visual Web Developer 2008 Service Pack 1。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-119">This tutorial assumes that you have Visual Studio 2008 or Visual Web Developer 2008 with Service Pack 1.</span></span> <span data-ttu-id="cc7e5-120">您需要 Service Pack 1，才能使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-120">You need Service Pack 1 in order to use the Entity Framework.</span></span> <span data-ttu-id="cc7e5-121">您可以從下列網址下載 Visual Studio 2008 Service Pack 1 或 Service Pack 1 的 Visual Web Developer:</span><span class="sxs-lookup"><span data-stu-id="cc7e5-121">You can download Visual Studio 2008 Service Pack 1 or Visual Web Developer with Service Pack 1 from the following address:</span></span>

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a><span data-ttu-id="cc7e5-122">建立電影範例資料庫</span><span class="sxs-lookup"><span data-stu-id="cc7e5-122">Creating the Movie Sample Database</span></span>

<span data-ttu-id="cc7e5-123">影片資料庫應用程式會使用資料庫資料表名為電影，其中包含下列資料行：</span><span class="sxs-lookup"><span data-stu-id="cc7e5-123">The Movie Database application uses a database table named Movies that contains the following columns:</span></span>

| <span data-ttu-id="cc7e5-124">資料行名稱</span><span class="sxs-lookup"><span data-stu-id="cc7e5-124">Column Name</span></span> | <span data-ttu-id="cc7e5-125">資料類型</span><span class="sxs-lookup"><span data-stu-id="cc7e5-125">Data Type</span></span> | <span data-ttu-id="cc7e5-126">允許 null 值嗎？</span><span class="sxs-lookup"><span data-stu-id="cc7e5-126">Allow Nulls?</span></span> | <span data-ttu-id="cc7e5-127">是主索引鍵嗎？</span><span class="sxs-lookup"><span data-stu-id="cc7e5-127">Is Primary Key?</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc7e5-128">ID</span><span class="sxs-lookup"><span data-stu-id="cc7e5-128">Id</span></span> | <span data-ttu-id="cc7e5-129">int</span><span class="sxs-lookup"><span data-stu-id="cc7e5-129">int</span></span> | <span data-ttu-id="cc7e5-130">False</span><span class="sxs-lookup"><span data-stu-id="cc7e5-130">False</span></span> | <span data-ttu-id="cc7e5-131">True</span><span class="sxs-lookup"><span data-stu-id="cc7e5-131">True</span></span> |
| <span data-ttu-id="cc7e5-132">標題</span><span class="sxs-lookup"><span data-stu-id="cc7e5-132">Title</span></span> | <span data-ttu-id="cc7e5-133">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="cc7e5-133">nvarchar(100)</span></span> | <span data-ttu-id="cc7e5-134">False</span><span class="sxs-lookup"><span data-stu-id="cc7e5-134">False</span></span> | <span data-ttu-id="cc7e5-135">False</span><span class="sxs-lookup"><span data-stu-id="cc7e5-135">False</span></span> |
| <span data-ttu-id="cc7e5-136">導向器</span><span class="sxs-lookup"><span data-stu-id="cc7e5-136">Director</span></span> | <span data-ttu-id="cc7e5-137">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="cc7e5-137">nvarchar(100)</span></span> | <span data-ttu-id="cc7e5-138">False</span><span class="sxs-lookup"><span data-stu-id="cc7e5-138">False</span></span> | <span data-ttu-id="cc7e5-139">False</span><span class="sxs-lookup"><span data-stu-id="cc7e5-139">False</span></span> |

<span data-ttu-id="cc7e5-140">您可以加入至 ASP.NET MVC 專案的這個資料表，依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="cc7e5-140">You can add this table to an ASP.NET MVC project by following these steps:</span></span>

1. <span data-ttu-id="cc7e5-141">以滑鼠右鍵按一下應用程式\_Data 資料夾中的方案總管 視窗，然後選取功能表選項**新增、 新項目。**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-141">Right-click the App\_Data folder in the Solution Explorer window and select the menu option **Add, New Item.**</span></span>
2. <span data-ttu-id="cc7e5-142">從**加入新項目**對話方塊中，選取**SQL Server 資料庫**，指定資料庫名稱 MoviesDB.mdf，再按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-142">From the **Add New Item** dialog box, select **SQL Server Database**, give the database the name MoviesDB.mdf, and click the **Add** button.</span></span>
3. <span data-ttu-id="cc7e5-143">按兩下要開啟 [伺服器總管/資料庫總管] 視窗的 MoviesDB.mdf 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-143">Double-click the MoviesDB.mdf file to open the Server Explorer/Database Explorer window.</span></span>
4. <span data-ttu-id="cc7e5-144">展開 MoviesDB.mdf 資料庫連接，以滑鼠右鍵按一下 [資料表] 資料夾中，並選取功能表選項**加入新的資料表**。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-144">Expand the MoviesDB.mdf database connection, right-click the Tables folder, and select the menu option **Add New Table**.</span></span>
5. <span data-ttu-id="cc7e5-145">在 資料表設計工具中，加入的識別碼、 標題和導向器的資料行。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-145">In the Table Designer, add the Id, Title, and Director columns.</span></span>
6. <span data-ttu-id="cc7e5-146">按一下**儲存**（具有磁碟的圖示） 的按鈕以儲存新的資料表名稱電影。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-146">Click the **Save** button (it has the icon of the floppy) to save the new table with the name Movies.</span></span>

<span data-ttu-id="cc7e5-147">建立電影資料庫資料表之後，您應該將某些範例資料加入資料表。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-147">After you create the Movies database table, you should add some sample data to the table.</span></span> <span data-ttu-id="cc7e5-148">以滑鼠右鍵按一下電影資料表，然後選取功能表選項**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-148">Right-click the Movies table and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="cc7e5-149">您可以輸入假的電影資料到方格內出現。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-149">You can enter fake movie data into the grid that appears.</span></span>

## <a name="creating-the-adonet-entity-data-model"></a><span data-ttu-id="cc7e5-150">建立 ADO.NET 實體資料模型</span><span class="sxs-lookup"><span data-stu-id="cc7e5-150">Creating the ADO.NET Entity Data Model</span></span>

<span data-ttu-id="cc7e5-151">若要使用 Entity Framework，您要建立實體資料模型。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-151">In order to use the Entity Framework, you need to create an Entity Data Model.</span></span> <span data-ttu-id="cc7e5-152">您可以利用 Visual Studio*實體資料模型精靈*從資料庫中自動產生實體資料模型。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-152">You can take advantage of the Visual Studio *Entity Data Model Wizard* to generate an Entity Data Model from a database automatically.</span></span>

<span data-ttu-id="cc7e5-153">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="cc7e5-153">Follow these steps:</span></span>

1. <span data-ttu-id="cc7e5-154">在 [方案總管] 視窗中的 [模型] 資料夾上按一下滑鼠右鍵，然後選取功能表選項**新增]、 [新項目**。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-154">Right-click the Models folder in the Solution Explorer window and select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="cc7e5-155">在**加入新項目** 對話方塊中，選取的資料類別 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-155">In the **Add New Item** dialog, select the Data category (see Figure 1).</span></span>
3. <span data-ttu-id="cc7e5-156">選取**ADO.NET 實體資料模型**範本，提供實體資料模型名稱 MoviesDBModel.edmx，然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-156">Select the **ADO.NET Entity Data Model** template, give the Entity Data Model the name MoviesDBModel.edmx, and click the **Add** button.</span></span> <span data-ttu-id="cc7e5-157">按一下**新增**按鈕會啟動 [資料模型精靈]。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-157">Clicking the **Add** button launches the Data Model Wizard.</span></span>
4. <span data-ttu-id="cc7e5-158">在**選擇模型內容**步驟中，選擇 **從資料庫產生**選項，然後按一下**下一步**按鈕 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-158">In the **Choose Model Contents** step, choose the **Generate from a database** option and click the **Next** button (see Figure 2).</span></span>
5. <span data-ttu-id="cc7e5-159">在**選擇資料連接**步驟選取 MoviesDB.mdf 資料庫連接，輸入的實體的連線設定命名 MoviesDBEntities，然後按一下**下一步**按鈕 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-159">In the **Choose Your Data Connection** step, select the MoviesDB.mdf database connection, enter the entities connection settings name MoviesDBEntities, and click the **Next** button (see Figure 3).</span></span>
6. <span data-ttu-id="cc7e5-160">在**選擇您的資料庫物件**步驟選取影片資料庫資料表，按一下 **完成**按鈕 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-160">In the **Choose Your Database Objects** step, select the Movie database table and click the **Finish** button (see Figure 4).</span></span>

<span data-ttu-id="cc7e5-161">完成這些步驟後，便會開啟 ADO.NET Entity Data Model Designer （實體設計工具）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-161">After you complete these steps, the ADO.NET Entity Data Model Designer (Entity Designer) opens.</span></span>

<span data-ttu-id="cc7e5-162">**圖 1 – 建立新的實體資料模型**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-162">**Figure 1 – Creating a new Entity Data Model**</span></span>

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

<span data-ttu-id="cc7e5-164">**圖 2 – 選擇模型內容的步驟**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-164">**Figure 2 – Choose Model Contents Step**</span></span>

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

<span data-ttu-id="cc7e5-166">**圖 3 – 選擇資料連線**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-166">**Figure 3 – Choose Your Data Connection**</span></span>

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

<span data-ttu-id="cc7e5-168">**圖 4 – 選擇您的資料庫物件**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-168">**Figure 4 – Choose Your Database Objects**</span></span>

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a><span data-ttu-id="cc7e5-170">修改 ADO.NET 實體資料模型</span><span class="sxs-lookup"><span data-stu-id="cc7e5-170">Modifying the ADO.NET Entity Data Model</span></span>

<span data-ttu-id="cc7e5-171">建立實體資料模型之後，您可以修改模型利用實體設計工具 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-171">After you create an Entity Data Model, you can modify the model by taking advantage of the Entity Designer (see Figure 5).</span></span> <span data-ttu-id="cc7e5-172">您可以隨時開啟 Entity Designer 中，按兩下 [方案總管] 視窗內的 [模型] 資料夾中包含的 MoviesDBModel.edmx 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-172">You can open the Entity Designer at any time by double-clicking the MoviesDBModel.edmx file contained in the Models folder within the Solution Explorer window.</span></span>

<span data-ttu-id="cc7e5-173">**圖 5 – ADO.NET 實體資料模型設計工具**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-173">**Figure 5 – The ADO.NET Entity Data Model Designer**</span></span>

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

<span data-ttu-id="cc7e5-175">例如，您可以使用 Entity Designer 中變更的實體模型資料精靈產生的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-175">For example, you can use the Entity Designer to change the names of the classes that the Entity Model Data Wizard generates.</span></span> <span data-ttu-id="cc7e5-176">此精靈會建立新的資料存取類別，名為電影。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-176">The Wizard created a new data access class named Movies.</span></span> <span data-ttu-id="cc7e5-177">換句話說，精靈會讓類別非常同名的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-177">In other words, the Wizard gave the class the very same name as the database table.</span></span> <span data-ttu-id="cc7e5-178">因為我們將使用這個類別來代表特定的影片執行個體，我們應該重新命名電影影片的類別。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-178">Because we will use this class to represent a particular Movie instance, we should rename the class from Movies to Movie.</span></span>

<span data-ttu-id="cc7e5-179">如果您想要重新命名為實體類別，您可以按兩下 Entity Designer 中的類別名稱，並輸入新名稱 （請參閱圖 6）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-179">If you want to rename an entity class, you can double-click on the class name in the Entity Designer and enter a new name (see Figure 6).</span></span> <span data-ttu-id="cc7e5-180">或者，您可以在 Entity Designer 中選取實體之後變更 [屬性] 視窗中的實體的名稱。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-180">Alternatively, you can change the name of an entity in the Properties window after selecting an entity in the Entity Designer.</span></span>

<span data-ttu-id="cc7e5-181">**圖 6 – 變更實體名稱**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-181">**Figure 6 – Changing an entity name**</span></span>

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

<span data-ttu-id="cc7e5-183">請記得儲存您的實體資料模型之後所做的修改，依序按一下 [儲存] 按鈕 （磁片圖示）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-183">Remember to save your Entity Data Model after making a modification by clicking the Save button (the icon of the floppy disk).</span></span> <span data-ttu-id="cc7e5-184">在幕後，實體設計工具會產生一組 Visual Basic.NET 類別。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-184">Behind the scenes, the Entity Designer generates a set of Visual Basic .NET classes.</span></span> <span data-ttu-id="cc7e5-185">您可以從 [方案總管] 視窗中開啟 MoviesDBModel.Designer.vb 檔案，以檢視這些類別。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-185">You can view these classes by opening the MoviesDBModel.Designer.vb file from the Solution Explorer window.</span></span>


<span data-ttu-id="cc7e5-186">不要修改.designer.vb 檔中的程式碼，因為您的變更將覆寫您使用 Entity Designer 中的下一次。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-186">Don't modify the code in the Designer.vb file since your changes will be overwritten the next time you use the Entity Designer.</span></span> <span data-ttu-id="cc7e5-187">如果您想要擴充.designer.vb 檔中定義的實體類別的功能，則您可以建立*部分類別*在不同的檔案。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-187">If you want to extend the functionality of the entity classes defined in the Designer.vb file then you can create *partial classes* in separate files.</span></span>


#### <a name="selecting-database-records-with-the-entity-framework"></a><span data-ttu-id="cc7e5-188">選取與 Entity Framework 資料庫資料錄</span><span class="sxs-lookup"><span data-stu-id="cc7e5-188">Selecting Database Records with the Entity Framework</span></span>

<span data-ttu-id="cc7e5-189">讓我們開始建置影片資料庫應用程式藉由建立顯示一份影片資料錄的網頁。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-189">Let's start building our Movie Database application by creating a page that displays a list of movie records.</span></span> <span data-ttu-id="cc7e5-190">Home 控制器中列出的 1 會公開名為 index （） 的動作。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-190">The Home controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="cc7e5-191">Index 動作傳回的電影記錄所有電影資料庫資料表中利用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-191">The Index() action returns all of the movie records from the Movie database table by taking advantage of the Entity Framework.</span></span>

<span data-ttu-id="cc7e5-192">**列出 1 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-192">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

<span data-ttu-id="cc7e5-193">請注意程式碼範例 1 中的控制站包含建構函式。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-193">Notice that the controller in Listing 1 includes a constructor.</span></span> <span data-ttu-id="cc7e5-194">建構函式會初始化名為的類別層級欄位\_db。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-194">The constructor initializes a class-level field named \_db.</span></span> <span data-ttu-id="cc7e5-195">\_Db 欄位代表由 Microsoft Entity Framework 產生資料庫實體。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-195">The \_db field represents the database entities generated by the Microsoft Entity Framework.</span></span> <span data-ttu-id="cc7e5-196">\_Db 欄位是由實體設計工具產生 MoviesDBEntities 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-196">The \_db field is an instance of the MoviesDBEntities class that was generated by the Entity Designer.</span></span>

<span data-ttu-id="cc7e5-197">\_Db 欄位 index 動作中用來從電影資料庫資料表中擷取的記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-197">The \_db field is used within the Index() action to retrieve the records from the Movies database table.</span></span> <span data-ttu-id="cc7e5-198">運算式\_db。MovieSet 會代表所有記錄的電影資料庫資料表中。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-198">The expression \_db.MovieSet represents all of the records from the Movies database table.</span></span> <span data-ttu-id="cc7e5-199">Tolist （） 方法用來將電影的集合轉換成影片物件的泛型集合： 清單 （的影片）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-199">The ToList() method is used to convert the set of movies into a generic collection of Movie objects: List( Of Movie).</span></span>

<span data-ttu-id="cc7e5-200">搭配 LINQ to Entities 擷取影片記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-200">The movie records are retrieved with the help of LINQ to Entities.</span></span> <span data-ttu-id="cc7e5-201">Index （） 中的動作清單 1 使用 LINQ*方法語法*擷取的資料庫中的記錄組。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-201">The Index() action in Listing 1 uses LINQ *method syntax* to retrieve the set of database records.</span></span> <span data-ttu-id="cc7e5-202">如果您想要的話，您可以使用 LINQ*查詢語法*改為。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-202">If you prefer, you can use LINQ *query syntax* instead.</span></span> <span data-ttu-id="cc7e5-203">下列兩個陳述式會執行完全相同的動作：</span><span class="sxs-lookup"><span data-stu-id="cc7e5-203">The following two statements do the very same thing:</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

<span data-ttu-id="cc7e5-204">使用您認為最具直覺性無論 LINQ 語法 （方法語法或查詢語法）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-204">Use whichever LINQ syntax – method syntax or query syntax – that you find most intuitive.</span></span> <span data-ttu-id="cc7e5-205">沒有任何差異，在兩種方法的效能而言唯一的差別樣式。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-205">There is no difference in performance between the two approaches – the only difference is style.</span></span>

<span data-ttu-id="cc7e5-206">列出 2 中的檢視用來顯示影片資料錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-206">The view in Listing 2 is used to display the movie records.</span></span>

<span data-ttu-id="cc7e5-207">**列出 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-207">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

<span data-ttu-id="cc7e5-208">列出 2 中的檢視包含**每個**迴圈，逐一查看每個影片記錄，並顯示電影錄製的標題和導向器屬性的值。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-208">The view in Listing 2 contains a **For Each** loop that iterates through each movie record and displays the values of the movie record's Title and Director properties.</span></span> <span data-ttu-id="cc7e5-209">請注意，編輯和刪除的連結會顯示每一筆記錄旁。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-209">Notice that an Edit and Delete link is displayed next to each record.</span></span> <span data-ttu-id="cc7e5-210">此外，新增影片連結會出現在檢視的底端 （請參閱圖 7）。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-210">Furthermore, an Add Movie link appears at the bottom of the view (see Figure 7).</span></span>

<span data-ttu-id="cc7e5-211">**圖 7 – 索引檢視**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-211">**Figure 7 – The Index view**</span></span>

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

<span data-ttu-id="cc7e5-213">索引檢視是*型別檢視*。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-213">The Index view is a *typed view*.</span></span> <span data-ttu-id="cc7e5-214">索引檢視表有&lt;%@ 頁面 %&gt;包含 Inherits 屬性的指示詞。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-214">The Index view has a &lt;%@ Page %&gt; directive that includes an Inherits attribute.</span></span> <span data-ttu-id="cc7e5-215">Inherits 屬性會轉換為強型別清單的泛型集合影片物件 – 清單 （的影片） ViewData.Model 屬性。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-215">The Inherits attribute casts the ViewData.Model property to a strongly typed generic List collection of Movie objects – a List(Of Movie).</span></span>

## <a name="inserting-database-records-with-the-entity-framework"></a><span data-ttu-id="cc7e5-216">插入與 Entity Framework 資料庫記錄</span><span class="sxs-lookup"><span data-stu-id="cc7e5-216">Inserting Database Records with the Entity Framework</span></span>

<span data-ttu-id="cc7e5-217">您可以使用 Entity Framework，以便於新記錄插入資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-217">You can use the Entity Framework to make it easy to insert new records into a database table.</span></span> <span data-ttu-id="cc7e5-218">列出 3 包含兩個新的動作加入至主控制器類別可讓您將新記錄插入至電影資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-218">Listing 3 contains two new actions added to the Home controller class that you can use to insert new records into the Movie database table.</span></span>

<span data-ttu-id="cc7e5-219">**列出 3 – Controllers\HomeController.vb （新增方法）**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-219">**Listing 3 – Controllers\HomeController.vb (Add methods)**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

<span data-ttu-id="cc7e5-220">第一個 add （） 動作只會傳回檢視表。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-220">The first Add() action simply returns a view.</span></span> <span data-ttu-id="cc7e5-221">此檢視包含來加入新的電影資料庫表單 （請參閱圖 8） 的記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-221">The view contains a form for adding a new movie database record (see Figure 8).</span></span> <span data-ttu-id="cc7e5-222">當您提交表單時，會叫用第二個 add （） 動作。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-222">When you submit the form, the second Add() action is invoked.</span></span>

<span data-ttu-id="cc7e5-223">請注意第二個 add （） 動作使用 AcceptVerbs 屬性裝飾。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-223">Notice that the second Add() action is decorated with the AcceptVerbs attribute.</span></span> <span data-ttu-id="cc7e5-224">只有在執行 HTTP POST 作業時，可以叫用這個動作。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-224">This action can be invoked only when performing an HTTP POST operation.</span></span> <span data-ttu-id="cc7e5-225">換句話說，此動作可以只叫用時張貼 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-225">In other words, this action can only be invoked when posting an HTML form.</span></span>

<span data-ttu-id="cc7e5-226">第二個 add （） 動作會建立 ASP.NET MVC TryUpdateModel() 方法搭配 Entity Framework 影片類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-226">The second Add() action creates a new instance of the Entity Framework Movie class with the help of the ASP.NET MVC TryUpdateModel() method.</span></span> <span data-ttu-id="cc7e5-227">TryUpdateModel() 方法會採用傳遞至 add （） 方法 FormCollection 中的欄位，並將這些 HTML 表單欄位的值指派給影片類別。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-227">The TryUpdateModel() method takes the fields in the FormCollection passed to the Add() method and assigns the values of these HTML form fields to the Movie class.</span></span>


<span data-ttu-id="cc7e5-228">使用 Entity Framework 時，您必須提供屬性的 「 技術清單 」 TryUpdateModel 或 UpdateModel 方法來更新實體類別的屬性時。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-228">When using the Entity Framework, you must supply a "white list" of properties when using the TryUpdateModel or UpdateModel methods to update the properties of an entity class.</span></span>


<span data-ttu-id="cc7e5-229">接下來，add （） 動作會執行一些簡單的表單驗證。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-229">Next, the Add() action performs some simple form validation.</span></span> <span data-ttu-id="cc7e5-230">動作可確認標題和導向器的屬性有值。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-230">The action verifies that both the Title and Director properties have values.</span></span> <span data-ttu-id="cc7e5-231">如果有驗證錯誤，驗證錯誤訊息將加入 ModelState。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-231">If there is a validation error, then a validation error message is added to ModelState.</span></span>

<span data-ttu-id="cc7e5-232">如果沒有任何驗證錯誤新的電影錄製將加入電影資料庫資料表，Entity Framework 的協助。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-232">If there are no validation errors then a new movie record is added to the Movies database table with the help of the Entity Framework.</span></span> <span data-ttu-id="cc7e5-233">新的記錄加入到具有下列兩行程式碼的資料庫：</span><span class="sxs-lookup"><span data-stu-id="cc7e5-233">The new record is added to the database with the following two lines of code:</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

<span data-ttu-id="cc7e5-234">第一行程式碼會將新的電影實體加入至電影由 Entity Framework 所追蹤的集合。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-234">The first line of code adds the new Movie entity to the set of movies being tracked by the Entity Framework.</span></span> <span data-ttu-id="cc7e5-235">程式碼的第二行儲存的任何變更已對基礎資料庫正在追蹤的電影。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-235">The second line of code saves whatever changes have been made to the Movies being tracked back to the underlying database.</span></span>

<span data-ttu-id="cc7e5-236">**圖 8 – 新增檢視**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-236">**Figure 8 – The Add view**</span></span>

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a><span data-ttu-id="cc7e5-238">更新資料庫的記錄與 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="cc7e5-238">Updating Database Records with the Entity Framework</span></span>

<span data-ttu-id="cc7e5-239">您可以依照幾乎相同的方法來編輯與 Entity Framework 資料庫記錄為我們剛才後面插入新的資料庫記錄的方法。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-239">You can follow almost the same approach to edit a database record with the Entity Framework as the approach that we just followed to insert a new database record.</span></span> <span data-ttu-id="cc7e5-240">列出 4 包含兩個名為 Edit() 的新控制器動作。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-240">Listing 4 contains two new controller actions named Edit().</span></span> <span data-ttu-id="cc7e5-241">第一個 Edit() 動作將會傳回編輯電影錄製之 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-241">The first Edit() action returns an HTML form for editing a movie record.</span></span> <span data-ttu-id="cc7e5-242">第二個 Edit() 動作會嘗試更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-242">The second Edit() action attempts to update the database.</span></span>

<span data-ttu-id="cc7e5-243">**列出 4 – Controllers\HomeController.vb （編輯方法）**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-243">**Listing 4 – Controllers\HomeController.vb (Edit methods)**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

<span data-ttu-id="cc7e5-244">第二個 Edit() 動作一開始會從符合正在編輯的電影識別碼的資料庫擷取電影錄製。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-244">The second Edit() action starts by retrieving the Movie record from the database that matches the Id of the movie being edited.</span></span> <span data-ttu-id="cc7e5-245">下列 LINQ to Entities 陳述式會抓取符合特定識別碼的第一個資料庫記錄：</span><span class="sxs-lookup"><span data-stu-id="cc7e5-245">The following LINQ to Entities statement grabs the first database record that matches a particular Id:</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

<span data-ttu-id="cc7e5-246">接下來，TryUpdateModel() 方法用來將 HTML 表單欄位的值指派給影片實體的屬性。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-246">Next, the TryUpdateModel() method is used to assign the values of the HTML form fields to the properties of the movie entity.</span></span> <span data-ttu-id="cc7e5-247">請注意的白名單提供給指定要更新的確切屬性。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-247">Notice that a white list is supplied to specify the exact properties to update.</span></span>

<span data-ttu-id="cc7e5-248">接下來，會執行一些簡單的驗證，來確認電影標題和導向器的屬性有值。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-248">Next, some simple validation is performed to verify that both the Movie Title and Director properties have values.</span></span> <span data-ttu-id="cc7e5-249">如果任一個屬性遺漏值，然後驗證錯誤訊息加入 ModelState 並 ModelState.IsValid 傳回其值為 false。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-249">If either property is missing a value, then a validation error message is added to ModelState and ModelState.IsValid returns the value false.</span></span>

<span data-ttu-id="cc7e5-250">最後，如果有任何驗證錯誤，則基礎電影資料庫資料表已更新的任何變更呼叫 savechanges （） 方法。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-250">Finally, if there are no validation errors, then the underlying Movies database table is updated with any changes by calling the SaveChanges() method.</span></span>

<span data-ttu-id="cc7e5-251">當編輯資料庫記錄時，您需要傳遞至控制器的動作執行資料庫更新正在編輯的記錄的識別碼。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-251">When editing database records, you need to pass the Id of the record being edited to the controller action that performs the database update.</span></span> <span data-ttu-id="cc7e5-252">否則，控制器動作將不會知道哪一個要在基礎資料庫中更新記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-252">Otherwise, the controller action will not know which record to update in the underlying database.</span></span> <span data-ttu-id="cc7e5-253">編輯檢視中，包含在列出 5 中，包括隱藏的表單欄位，表示正在編輯的資料庫記錄的識別碼。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-253">The Edit view, contained in Listing 5, includes a hidden form field that represents the Id of the database record being edited.</span></span>

<span data-ttu-id="cc7e5-254">**列出 5 – Views\Home\Edit.aspx**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-254">**Listing 5 – Views\Home\Edit.aspx**</span></span>

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a><span data-ttu-id="cc7e5-255">刪除與 Entity Framework 資料庫資料錄</span><span class="sxs-lookup"><span data-stu-id="cc7e5-255">Deleting Database Records with the Entity Framework</span></span>

<span data-ttu-id="cc7e5-256">最終的資料庫作業，我們需要將解決本教學課程中，正在刪除資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-256">The final database operation, which we need to tackle in this tutorial, is deleting database records.</span></span> <span data-ttu-id="cc7e5-257">您可以使用程式碼範例 6 中的控制器動作刪除特定資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-257">You can use the controller action in Listing 6 to delete a particular database record.</span></span>

<span data-ttu-id="cc7e5-258">**列出 6-\Controllers\HomeController.vb （Delete 動作）**</span><span class="sxs-lookup"><span data-stu-id="cc7e5-258">**Listing 6 -- \Controllers\HomeController.vb (Delete action)**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

<span data-ttu-id="cc7e5-259">Delete 動作會先擷取符合識別碼的實體傳遞給動作的電影。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-259">The Delete() action first retrieves the Movie entity that matches the Id passed to the action.</span></span> <span data-ttu-id="cc7e5-260">接下來，電影會從資料庫刪除藉由呼叫 DeleteObject() 方法接著 savechanges （） 方法。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-260">Next, the movie is deleted from the database by calling the DeleteObject() method followed by the SaveChanges() method.</span></span> <span data-ttu-id="cc7e5-261">最後，使用者重新導向回索引檢視。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-261">Finally, the user is redirected back to the Index view.</span></span>

## <a name="summary"></a><span data-ttu-id="cc7e5-262">總結</span><span class="sxs-lookup"><span data-stu-id="cc7e5-262">Summary</span></span>

<span data-ttu-id="cc7e5-263">本教學課程的用途是示範如何藉由運用 ASP.NET MVC 和 Microsoft Entity Framework 建置資料庫驅動的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-263">The purpose of this tutorial was to demonstrate how you can build database-driven web applications by taking advantage of ASP.NET MVC and the Microsoft Entity Framework.</span></span> <span data-ttu-id="cc7e5-264">您已了解如何建置應用程式，可讓您選取、 插入、 更新和刪除資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-264">You learned how to build an application that enables you to select, insert, update, and delete database records.</span></span>

<span data-ttu-id="cc7e5-265">首先，我們將討論如何使用實體資料模型精靈來產生實體資料模型從 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-265">First, we discussed how you can use the Entity Data Model Wizard to generate an Entity Data Model from within Visual Studio.</span></span> <span data-ttu-id="cc7e5-266">接下來，您會了解如何使用 LINQ to Entities 從資料庫資料表擷取一組資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-266">Next, you learn how to use LINQ to Entities to retrieve a set of database records from a database table.</span></span> <span data-ttu-id="cc7e5-267">最後，我們會使用 Entity Framework 來插入、 更新和刪除資料庫的記錄。</span><span class="sxs-lookup"><span data-stu-id="cc7e5-267">Finally, we used the Entity Framework to insert, update, and delete database records.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc7e5-268">[上一頁](validation-with-the-data-annotation-validators-cs.md)
> [下一頁](creating-model-classes-with-linq-to-sql-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cc7e5-268">[Previous](validation-with-the-data-annotation-validators-cs.md)
[Next](creating-model-classes-with-linq-to-sql-vb.md)</span></span>
