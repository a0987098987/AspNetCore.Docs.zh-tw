---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 教學課程：變更資料庫的 EF Database First 與 ASP.NET MVC 應用程式
description: 本教學課程著重於對資料庫結構的更新和傳播該項變更整個 web 應用程式。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667631"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="c4001-103">教學課程：變更資料庫的 EF Database First 與 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="c4001-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="c4001-104">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c4001-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="c4001-105">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="c4001-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="c4001-106">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="c4001-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="c4001-107">本教學課程著重於對資料庫結構的更新和傳播該項變更整個 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c4001-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="c4001-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="c4001-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c4001-109">加入資料行</span><span class="sxs-lookup"><span data-stu-id="c4001-109">Add a column</span></span>
> * <span data-ttu-id="c4001-110">將屬性加入至檢視</span><span class="sxs-lookup"><span data-stu-id="c4001-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4001-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="c4001-111">Prerequisites</span></span>

* [<span data-ttu-id="c4001-112">產生檢視</span><span class="sxs-lookup"><span data-stu-id="c4001-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="c4001-113">加入資料行</span><span class="sxs-lookup"><span data-stu-id="c4001-113">Add a column</span></span>

<span data-ttu-id="c4001-114">如果您在資料庫中更新資料表的結構，您需要確保您的變更會傳播到資料模型、 檢視和控制器。</span><span class="sxs-lookup"><span data-stu-id="c4001-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="c4001-115">本教學課程中，要將新的資料行加入記錄中間名學生的 Student 資料表中。</span><span class="sxs-lookup"><span data-stu-id="c4001-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="c4001-116">若要新增此資料行，請開啟資料庫專案，並開啟 Student.sql 檔案。</span><span class="sxs-lookup"><span data-stu-id="c4001-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="c4001-117">透過設計工具或 T-SQL 程式碼中，新增名為的資料行**MiddleName** NVARCHAR(50) 且允許 NULL 值。</span><span class="sxs-lookup"><span data-stu-id="c4001-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="c4001-118">啟動您的資料庫專案 （或 F5），這項變更部署到您的本機資料庫。</span><span class="sxs-lookup"><span data-stu-id="c4001-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="c4001-119">新的欄位加入至資料表。</span><span class="sxs-lookup"><span data-stu-id="c4001-119">The new field is added to the table.</span></span> <span data-ttu-id="c4001-120">如果您看不到 SQL Server 物件總管 中，按一下窗格中的 重新整理 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c4001-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![顯示新的資料行](changing-the-database/_static/image2.png)

<span data-ttu-id="c4001-122">新的資料行存在於資料庫資料表中，但它目前不存在的資料模型類別中。</span><span class="sxs-lookup"><span data-stu-id="c4001-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="c4001-123">您必須更新模型，以包含新的資料行。</span><span class="sxs-lookup"><span data-stu-id="c4001-123">You must update the model to include your new column.</span></span> <span data-ttu-id="c4001-124">在 **模型**資料夾中，開啟**ContosoModel.edmx**檔案來顯示模型圖表。</span><span class="sxs-lookup"><span data-stu-id="c4001-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="c4001-125">請注意，Student 模型不包含 [MiddleName] 屬性。</span><span class="sxs-lookup"><span data-stu-id="c4001-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="c4001-126">以滑鼠右鍵按一下設計介面上的任何位置，然後選取**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="c4001-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="c4001-127">在 [更新精靈] 中，選取**重新整理**索引標籤，然後選取**資料表** > **dbo** > **學生**。</span><span class="sxs-lookup"><span data-stu-id="c4001-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="c4001-128">按一下 [ **完成**]。</span><span class="sxs-lookup"><span data-stu-id="c4001-128">Click **Finish**.</span></span>

<span data-ttu-id="c4001-129">在更新程序完成之後，資料庫圖表包含新**MiddleName**屬性。</span><span class="sxs-lookup"><span data-stu-id="c4001-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="c4001-130">儲存**ContosoModel.edmx**檔案。</span><span class="sxs-lookup"><span data-stu-id="c4001-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="c4001-131">您必須儲存此檔案才能傳播至新的屬性**Student.cs**類別。</span><span class="sxs-lookup"><span data-stu-id="c4001-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="c4001-132">您現在已更新的資料庫和模型。</span><span class="sxs-lookup"><span data-stu-id="c4001-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="c4001-133">建置方案。</span><span class="sxs-lookup"><span data-stu-id="c4001-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="c4001-134">將屬性加入至檢視</span><span class="sxs-lookup"><span data-stu-id="c4001-134">Add the property to the views</span></span>

<span data-ttu-id="c4001-135">不幸的是，在檢視仍然不包含新的屬性。</span><span class="sxs-lookup"><span data-stu-id="c4001-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="c4001-136">若要更新的檢視有兩個選項-您可以重新產生檢視藉由再次新增 scaffold Student 類別，或您可以手動將新屬性新增至您現有的檢視。</span><span class="sxs-lookup"><span data-stu-id="c4001-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="c4001-137">在本教學課程中，您會新增 scaffolding 再次因為您無法對任何自訂的變更自動產生的檢視。</span><span class="sxs-lookup"><span data-stu-id="c4001-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="c4001-138">您可以考慮以手動方式加入的屬性，當您檢視已變更，並不想要捨棄這些變更。</span><span class="sxs-lookup"><span data-stu-id="c4001-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="c4001-139">若要確保檢視時都會重新建立**學生**下方的資料夾**檢視**，並刪除**StudentsController**。</span><span class="sxs-lookup"><span data-stu-id="c4001-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="c4001-140">然後，以滑鼠右鍵按一下**控制器**資料夾，並新增樣板**學生**模型。</span><span class="sxs-lookup"><span data-stu-id="c4001-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="c4001-141">同樣地，將控制器命名**StudentsController**。</span><span class="sxs-lookup"><span data-stu-id="c4001-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="c4001-142">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="c4001-142">Select **Add**.</span></span>

<span data-ttu-id="c4001-143">重新建置方案。</span><span class="sxs-lookup"><span data-stu-id="c4001-143">Build the solution again.</span></span> <span data-ttu-id="c4001-144">檢視現在會包含 [MiddleName] 屬性。</span><span class="sxs-lookup"><span data-stu-id="c4001-144">The views now contain the MiddleName property.</span></span>

![顯示中間名](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="c4001-146">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c4001-146">Next steps</span></span>

<span data-ttu-id="c4001-147">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="c4001-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c4001-148">加入資料行</span><span class="sxs-lookup"><span data-stu-id="c4001-148">Added a column</span></span>
> * <span data-ttu-id="c4001-149">加入檢視中的屬性</span><span class="sxs-lookup"><span data-stu-id="c4001-149">Added the property to the views</span></span>

<span data-ttu-id="c4001-150">請前進到下一個教學課程，以了解如何自訂用於顯示學生記錄的詳細檢視。</span><span class="sxs-lookup"><span data-stu-id="c4001-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c4001-151">自訂檢視</span><span class="sxs-lookup"><span data-stu-id="c4001-151">Customize a view</span></span>](customizing-a-view.md)