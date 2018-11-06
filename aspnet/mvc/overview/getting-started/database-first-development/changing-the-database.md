---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: EF Database First 與 ASP.NET MVC： 變更資料庫 |Microsoft Docs
author: Rick-Anderson
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 8d0eff37ced757cd8be74f8171c9aaa430940010
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021270"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="8ec5a-104">EF Database First 與 ASP.NET MVC： 變更資料庫</span><span class="sxs-lookup"><span data-stu-id="8ec5a-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="8ec5a-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8ec5a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8ec5a-106">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8ec5a-107">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8ec5a-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="8ec5a-109">系列的這個部分會著重於對資料庫結構的更新和傳播該項變更整個 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="8ec5a-110">加入資料行</span><span class="sxs-lookup"><span data-stu-id="8ec5a-110">Add a column</span></span>

<span data-ttu-id="8ec5a-111">如果您在資料庫中更新資料表的結構，您需要確保您的變更會傳播到資料模型、 檢視和控制器。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="8ec5a-112">本教學課程中，要將新的資料行加入記錄中間名學生的 Student 資料表中。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="8ec5a-113">若要新增此資料行，請開啟資料庫專案，並開啟 Student.sql 檔案。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="8ec5a-114">透過設計工具或 T-SQL 程式碼中，新增名為的資料行**MiddleName** NVARCHAR(50) 且允許 NULL 值。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![新增中間名](changing-the-database/_static/image1.png)

<span data-ttu-id="8ec5a-116">啟動您的資料庫專案 （或 F5），這項變更部署到您的本機資料庫。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="8ec5a-117">新的欄位加入至資料表。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-117">The new field is added to the table.</span></span> <span data-ttu-id="8ec5a-118">如果您看不到 SQL Server 物件總管 中，按一下窗格中的 重新整理 按鈕。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![顯示新的資料行](changing-the-database/_static/image2.png)

<span data-ttu-id="8ec5a-120">新的資料行存在於資料庫資料表中，但它目前不存在的資料模型類別中。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="8ec5a-121">您必須更新模型，以包含新的資料行。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-121">You must update the model to include your new column.</span></span> <span data-ttu-id="8ec5a-122">在 **模型**資料夾中，開啟**ContosoModel.edmx**檔案來顯示模型圖表。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="8ec5a-123">請注意，Student 模型不包含 [MiddleName] 屬性。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="8ec5a-124">以滑鼠右鍵按一下設計介面上的任何位置，然後選取**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![更新模型](changing-the-database/_static/image3.png)

<span data-ttu-id="8ec5a-126">在 [更新精靈] 中，選取**重新整理**索引標籤和**學生**資料表。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![更新精靈](changing-the-database/_static/image4.png)

<span data-ttu-id="8ec5a-128">按一下 [ **完成**]。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-128">Click **Finish**.</span></span>

<span data-ttu-id="8ec5a-129">在更新程序完成之後，資料庫圖表包含新**MiddleName**屬性。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="8ec5a-130">儲存**ContosoModel.edmx**檔案。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="8ec5a-131">您必須儲存此檔案才能傳播至新的屬性**Student.cs**類別。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="8ec5a-132">您現在已更新的資料庫和模型。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="8ec5a-133">建置方案。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-133">Build the solution.</span></span>

<span data-ttu-id="8ec5a-134">不幸的是，在檢視仍然不包含新的屬性。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="8ec5a-135">若要更新的檢視有兩個選項-您可以重新產生檢視藉由再次新增 scaffold Student 類別，或您可以手動將新屬性新增至您現有的檢視。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="8ec5a-136">在本教學課程中，您會新增 scaffolding 再次因為您無法對任何自訂的變更自動產生的檢視。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="8ec5a-137">您可以考慮以手動方式加入的屬性，當您檢視已變更，並不想要捨棄這些變更。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="8ec5a-138">若要確保檢視時都會重新建立**學生**下方的資料夾**檢視**，並刪除**StudentsController**。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="8ec5a-139">然後，以滑鼠右鍵按一下**控制器**資料夾，並新增樣板**學生**模型。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="8ec5a-140">同樣地，將控制器命名**StudentsController**。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="8ec5a-141">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-141">Select **OK**.</span></span>

<span data-ttu-id="8ec5a-142">檢視現在會包含 [MiddleName] 屬性。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-142">The views now contain the MiddleName property.</span></span>

![顯示中間名](changing-the-database/_static/image5.png)

<span data-ttu-id="8ec5a-144">在下一步 區段中，您將加入程式碼來自訂顯示學生記錄的詳細檢視。</span><span class="sxs-lookup"><span data-stu-id="8ec5a-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ec5a-145">[上一頁](generating-views.md)
> [下一頁](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="8ec5a-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
