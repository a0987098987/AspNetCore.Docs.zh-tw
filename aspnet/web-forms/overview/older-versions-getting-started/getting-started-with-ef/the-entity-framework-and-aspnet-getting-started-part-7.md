---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: 開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-部分 7 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: cb84f4f3e130fedb3e2f1a17d630767ff65bfa05
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a><span data-ttu-id="c33e4-104">開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 一部分 7</span><span class="sxs-lookup"><span data-stu-id="c33e4-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 7</span></span>
====================
<span data-ttu-id="c33e4-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c33e4-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="c33e4-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="c33e4-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="c33e4-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="c33e4-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-stored-procedures"></a><span data-ttu-id="c33e4-108">使用預存程序</span><span class="sxs-lookup"><span data-stu-id="c33e4-108">Using Stored Procedures</span></span>

<span data-ttu-id="c33e4-109">在上一個教學課程中，您會實作資料表每個階層繼承模式。</span><span class="sxs-lookup"><span data-stu-id="c33e4-109">In the previous tutorial you implemented a table-per-hierarchy inheritance pattern.</span></span> <span data-ttu-id="c33e4-110">本教學課程將說明如何使用預存程序更充分掌控資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="c33e4-110">This tutorial will show you how to use stored procedures to gain more control over database access.</span></span>

<span data-ttu-id="c33e4-111">Entity Framework 可讓您指定它應該使用預存程序進行資料庫存取。</span><span class="sxs-lookup"><span data-stu-id="c33e4-111">The Entity Framework lets you specify that it should use stored procedures for database access.</span></span> <span data-ttu-id="c33e4-112">任何實體型別，您可以指定要用於建立、 更新或刪除實體，該類型的預存程序。</span><span class="sxs-lookup"><span data-stu-id="c33e4-112">For any entity type, you can specify a stored procedure to use for creating, updating, or deleting entities of that type.</span></span> <span data-ttu-id="c33e4-113">然後資料模型中，您可以加入預存程序可讓您執行工作，例如擷取的實體集的參考。</span><span class="sxs-lookup"><span data-stu-id="c33e4-113">Then in the data model you can add references to stored procedures that you can use to perform tasks such as retrieving sets of entities.</span></span>

<span data-ttu-id="c33e4-114">使用預存程序是常見的需求進行資料庫存取。</span><span class="sxs-lookup"><span data-stu-id="c33e4-114">Using stored procedures is a common requirement for database access.</span></span> <span data-ttu-id="c33e4-115">在某些情況下的資料庫系統管理員可能需要的所有資料庫存取都經過基於安全性考量的預存程序。</span><span class="sxs-lookup"><span data-stu-id="c33e4-115">In some cases a database administrator may require that all database access go through stored procedures for security reasons.</span></span> <span data-ttu-id="c33e4-116">在其他情況下，您可能要部分更新資料庫時，會使用 Entity Framework 處理序中建立商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="c33e4-116">In other cases you may want to build business logic into some of the processes that the Entity Framework uses when it updates the database.</span></span> <span data-ttu-id="c33e4-117">例如，每當刪除實體時您可以將它複製到封存資料庫。</span><span class="sxs-lookup"><span data-stu-id="c33e4-117">For example, whenever an entity is deleted you might want to copy it to an archive database.</span></span> <span data-ttu-id="c33e4-118">或者，每次更新一個資料列時您可能想要寫入記錄到記錄資料表何人進行變更所記錄的資料列。</span><span class="sxs-lookup"><span data-stu-id="c33e4-118">Or whenever a row is updated you might want to write a row to a logging table that records who made the change.</span></span> <span data-ttu-id="c33e4-119">您可以在每當 Entity Framework 會刪除實體，或更新實體，就會呼叫預存程序中執行這些種類的工作。</span><span class="sxs-lookup"><span data-stu-id="c33e4-119">You can perform these kinds of tasks in a stored procedure that's called whenever the Entity Framework deletes an entity or updates an entity.</span></span>

<span data-ttu-id="c33e4-120">如同上一個教學課程中，您將不建立任何新頁面。</span><span class="sxs-lookup"><span data-stu-id="c33e4-120">As in the previous tutorial, you'll not create any new pages.</span></span> <span data-ttu-id="c33e4-121">相反地，您要變更 Entity Framework 部分已建立的頁面存取資料庫的方式。</span><span class="sxs-lookup"><span data-stu-id="c33e4-121">Instead, you'll change the way the Entity Framework accesses the database for some of the pages you already created.</span></span>

<span data-ttu-id="c33e4-122">在本教學課程中，您將建立插入資料庫中的預存程序`Student`和`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="c33e4-122">In this tutorial you'll create stored procedures in the database for inserting `Student` and `Instructor` entities.</span></span> <span data-ttu-id="c33e4-123">您會將它們加入至資料模型，並指定，Entity Framework 應該加入使用`Student`和`Instructor`實體資料庫。</span><span class="sxs-lookup"><span data-stu-id="c33e4-123">You'll add them to the data model, and you'll specify that the Entity Framework should use them for adding `Student` and `Instructor` entities to the database.</span></span> <span data-ttu-id="c33e4-124">您也會建立預存程序可讓您擷取`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="c33e4-124">You'll also create a stored procedure that you can use to retrieve `Course` entities.</span></span>

## <a name="creating-stored-procedures-in-the-database"></a><span data-ttu-id="c33e4-125">在資料庫中建立預存程序</span><span class="sxs-lookup"><span data-stu-id="c33e4-125">Creating Stored Procedures in the Database</span></span>

<span data-ttu-id="c33e4-126">(如果您使用*School.mdf*檔案從下載與本教學課程專案中，您可以略過此區段因為已經存在的預存程序。)</span><span class="sxs-lookup"><span data-stu-id="c33e4-126">(If you're using the *School.mdf* file from the project available for download with this tutorial, you can skip this section because the stored procedures already exist.)</span></span>

<span data-ttu-id="c33e4-127">在**伺服器總管**，依序展開*School.mdf*，以滑鼠右鍵按一下**預存程序**，然後選取**加入新的預存程序**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-127">In **Server Explorer**, expand *School.mdf*, right-click **Stored Procedures**, and select **Add New Stored Procedure**.</span></span>

<span data-ttu-id="c33e4-128">[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-128">[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)</span></span>

<span data-ttu-id="c33e4-129">複製下列 SQL 陳述式，並將它們貼至 [預存程序] 視窗中，取代基本架構預存程序。</span><span class="sxs-lookup"><span data-stu-id="c33e4-129">Copy the following SQL statements and paste them into the stored procedure window, replacing the skeleton stored procedure.</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

<span data-ttu-id="c33e4-130">[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-130">[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)</span></span>

<span data-ttu-id="c33e4-131">`Student` 實體有四個屬性： `PersonID`， `LastName`， `FirstName`，和`EnrollmentDate`。</span><span class="sxs-lookup"><span data-stu-id="c33e4-131">`Student` entities have four properties: `PersonID`, `LastName`, `FirstName`, and `EnrollmentDate`.</span></span> <span data-ttu-id="c33e4-132">此資料庫會自動產生的識別碼值和預存程序會接受其他三個參數。</span><span class="sxs-lookup"><span data-stu-id="c33e4-132">The database generates the ID value automatically, and the stored procedure accepts parameters for the other three.</span></span> <span data-ttu-id="c33e4-133">預存程序會傳回新的資料列記錄索引鍵的值，讓 Entity Framework 可以追蹤的它會保存在記憶體中的實體版本。</span><span class="sxs-lookup"><span data-stu-id="c33e4-133">The stored procedure returns the value of the new row's record key so that the Entity Framework can keep track of that in the version of the entity it keeps in memory.</span></span>

<span data-ttu-id="c33e4-134">儲存並關閉 [預存程序] 視窗。</span><span class="sxs-lookup"><span data-stu-id="c33e4-134">Save and close the stored procedure window.</span></span>

<span data-ttu-id="c33e4-135">建立`InsertInstructor`預存程序相同的方式，使用下列 SQL 陳述式：</span><span class="sxs-lookup"><span data-stu-id="c33e4-135">Create an `InsertInstructor` stored procedure in the same manner, using the following SQL statements:</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

<span data-ttu-id="c33e4-136">建立`Update`預存程序`Student`和`Instructor`實體也。</span><span class="sxs-lookup"><span data-stu-id="c33e4-136">Create `Update` stored procedures for `Student` and `Instructor` entities also.</span></span> <span data-ttu-id="c33e4-137">(資料庫中已經有`DeletePerson`預存程序同時運作`Instructor`和`Student`實體。)</span><span class="sxs-lookup"><span data-stu-id="c33e4-137">(The database already has a `DeletePerson` stored procedure which will work for both `Instructor` and `Student` entities.)</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

<span data-ttu-id="c33e4-138">在本教學課程中，您將對應所有三個函式-insert、 update 和 delete-每個實體類型。</span><span class="sxs-lookup"><span data-stu-id="c33e4-138">In this tutorial you'll map all three functions -- insert, update, and delete -- for each entity type.</span></span> <span data-ttu-id="c33e4-139">Entity Framework 第 4 版可讓您對應其中一個或兩個函式預存程序不含對應其他人，但有一個例外： 如果您將對應更新函式，但不是刪除函式，Entity Framework 將會擲回例外狀況時您嘗試刪除實體。</span><span class="sxs-lookup"><span data-stu-id="c33e4-139">The Entity Framework version 4 allows you to map just one or two of these functions to stored procedures without mapping the others, with one exception: if you map the update function but not the delete function, the Entity Framework will throw an exception when you attempt to delete an entity.</span></span> <span data-ttu-id="c33e4-140">在 Entity Framework 3.5 版中，您不需要這麼多的彈性對應預存程序： 如果您對應一個函式您都需要對應這三個。</span><span class="sxs-lookup"><span data-stu-id="c33e4-140">In the Entity Framework version 3.5, you did not have this much flexibility in mapping stored procedures: if you mapped one function you were required to map all three.</span></span>

<span data-ttu-id="c33e4-141">若要建立的預存程序讀取，而不是更新資料，建立一個會選取所有`Course`實體，使用下列 SQL 陳述式：</span><span class="sxs-lookup"><span data-stu-id="c33e4-141">To create a stored procedure that reads rather than updates data, create one that selects all `Course` entities, using the following SQL statements:</span></span>

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a><span data-ttu-id="c33e4-142">加入資料模型中的預存程序</span><span class="sxs-lookup"><span data-stu-id="c33e4-142">Adding the Stored Procedures to the Data Model</span></span>

<span data-ttu-id="c33e4-143">預存程序現在已定義在資料庫中，但是必須將它們加入至資料模型，以供使用，Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="c33e4-143">The stored procedures are now defined in the database, but they must be added to the data model to make them available to the Entity Framework.</span></span> <span data-ttu-id="c33e4-144">開啟*SchoolModel.edmx*，以滑鼠右鍵按一下設計介面，並選取**從資料庫更新模型**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-144">Open *SchoolModel.edmx*, right-click the design surface, and select **Update Model from Database**.</span></span> <span data-ttu-id="c33e4-145">在**新增** 索引標籤**選擇您的資料庫物件**對話方塊方塊中，展開 **預存程序**，選取新建立的預存程序和`DeletePerson`預存程序，，然後按一下 **完成**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-145">In the **Add** tab of the **Choose Your Database Objects** dialog box, expand **Stored Procedures**, select the newly created stored procedures and the `DeletePerson` stored procedure, and then click **Finish**.</span></span>

<span data-ttu-id="c33e4-146">[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-146">[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)</span></span>

## <a name="mapping-the-stored-procedures"></a><span data-ttu-id="c33e4-147">對應的預存程序</span><span class="sxs-lookup"><span data-stu-id="c33e4-147">Mapping the Stored Procedures</span></span>

<span data-ttu-id="c33e4-148">在資料模型設計師中，以滑鼠右鍵按一下`Student`實體，然後選取**預存程序對應**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-148">In the data model designer, right-click the `Student` entity and select **Stored Procedure Mapping**.</span></span>

<span data-ttu-id="c33e4-149">[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-149">[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)</span></span>

<span data-ttu-id="c33e4-150">**對應詳細資料**視窗隨即出現，您可以在其中指定 Entity Framework 應該用於插入、 更新及刪除實體，此類型的預存程序。</span><span class="sxs-lookup"><span data-stu-id="c33e4-150">The **Mapping Details** window appears, in which you can specify stored procedures that the Entity Framework should use for inserting, updating, and deleting entities of this type.</span></span>

<span data-ttu-id="c33e4-151">[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-151">[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)</span></span>

<span data-ttu-id="c33e4-152">設定**插入**函式以**InsertStudent**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-152">Set the **Insert** function to **InsertStudent**.</span></span> <span data-ttu-id="c33e4-153">視窗會顯示預存程序參數，其中每一個都必須對應至實體屬性的清單。</span><span class="sxs-lookup"><span data-stu-id="c33e4-153">The window shows a list of stored procedure parameters, each of which must be mapped to an entity property.</span></span> <span data-ttu-id="c33e4-154">因為名稱相同，其中兩個都會自動對應。</span><span class="sxs-lookup"><span data-stu-id="c33e4-154">Two of these are mapped automatically because the names are the same.</span></span> <span data-ttu-id="c33e4-155">沒有名為的實體屬性`FirstName`，因此您必須手動選取`FirstMidName`從下拉式清單會顯示可用的實體屬性。</span><span class="sxs-lookup"><span data-stu-id="c33e4-155">There's no entity property named `FirstName`, so you must manually select `FirstMidName` from a drop-down list that shows available entity properties.</span></span> <span data-ttu-id="c33e4-156">(這是因為變更名稱`FirstName`屬性`FirstMidName`第一個教學課程中。)</span><span class="sxs-lookup"><span data-stu-id="c33e4-156">(This is because you changed the name of the `FirstName` property to `FirstMidName` in the first tutorial.)</span></span>

<span data-ttu-id="c33e4-157">[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-157">[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)</span></span>

<span data-ttu-id="c33e4-158">在同一個**對應詳細資料**視窗中，對應`Update`函式以`UpdateStudent`預存程序 (請確定您指定`FirstMidName`做為參數值`FirstName`、 與您`Insert`預存程序） 和`Delete`函式以`DeletePerson`預存程序。</span><span class="sxs-lookup"><span data-stu-id="c33e4-158">In the same **Mapping Details** window, map the `Update` function to the `UpdateStudent` stored procedure (make sure you specify `FirstMidName` as the parameter value for `FirstName`, as you did for the `Insert` stored procedure) and the `Delete` function to the `DeletePerson` stored procedure.</span></span>

<span data-ttu-id="c33e4-159">[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-159">[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)</span></span>

<span data-ttu-id="c33e4-160">遵循相同的程序對應 insert、 update 和 delete 預存程序對講師至`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="c33e4-160">Follow the same procedure to map the insert, update, and delete stored procedures for instructors to the `Instructor` entity.</span></span>

<span data-ttu-id="c33e4-161">[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-161">[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)</span></span>

<span data-ttu-id="c33e4-162">讀取而不是更新資料的預存程序，您使用**模型瀏覽器**視窗，將預存程序對應至實體類型就會傳回。</span><span class="sxs-lookup"><span data-stu-id="c33e4-162">For stored procedures that read rather than update data, you use the **Model Browser** window to map the stored procedure to the entity type it returns.</span></span> <span data-ttu-id="c33e4-163">在資料模型設計師中，以滑鼠右鍵按一下設計介面，並選取**模型瀏覽器**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-163">In the data model designer, right-click the design surface and select **Model Browser**.</span></span> <span data-ttu-id="c33e4-164">開啟**SchoolModel.Store**節點然後再開啟**預存程序**節點。</span><span class="sxs-lookup"><span data-stu-id="c33e4-164">Open the **SchoolModel.Store** node and then open the **Stored Procedures** node.</span></span> <span data-ttu-id="c33e4-165">以滑鼠右鍵按一下`GetCourses`預存程序，然後選取**新增函式匯入**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-165">Then right-click the `GetCourses` stored procedure and select **Add Function Import**.</span></span>

<span data-ttu-id="c33e4-166">[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-166">[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)</span></span>

<span data-ttu-id="c33e4-167">在**新增函式匯入**對話方塊的 **傳回集合的**選取**實體**，然後選取`Course`作為實體類型傳回。</span><span class="sxs-lookup"><span data-stu-id="c33e4-167">In the **Add Function Import** dialog box, under **Returns a Collection Of** select **Entities**, and then select `Course` as the entity type returned.</span></span> <span data-ttu-id="c33e4-168">當您完成時，按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="c33e4-168">When you're done, click **OK**.</span></span> <span data-ttu-id="c33e4-169">儲存並關閉*.edmx*檔案。</span><span class="sxs-lookup"><span data-stu-id="c33e4-169">Save and close the *.edmx* file.</span></span>

<span data-ttu-id="c33e4-170">[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-170">[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)</span></span>

## <a name="using-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="c33e4-171">使用 Insert、 Update 和 Delete 預存程序</span><span class="sxs-lookup"><span data-stu-id="c33e4-171">Using Insert, Update, and Delete Stored Procedures</span></span>

<span data-ttu-id="c33e4-172">預存程序插入、 更新和刪除的資料由 Entity Framework 自動加入資料模型及它們對應到適當的實體之後。</span><span class="sxs-lookup"><span data-stu-id="c33e4-172">Stored procedures to insert, update, and delete data are used by the Entity Framework automatically after you've added them to the data model and mapped them to the appropriate entities.</span></span> <span data-ttu-id="c33e4-173">您現在可以執行*StudentsAdd.aspx*  頁面上，且每次您建立新的學生，將會使用 Entity Framework`InsertStudent`預存程序加入新的資料列`Student`資料表。</span><span class="sxs-lookup"><span data-stu-id="c33e4-173">You can now run the *StudentsAdd.aspx* page, and every time you create a new student, the Entity Framework will use the `InsertStudent` stored procedure to add the new row to the `Student` table.</span></span>

<span data-ttu-id="c33e4-174">[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-174">[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)</span></span>

<span data-ttu-id="c33e4-175">執行*Students.aspx*頁面和新的學生會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="c33e4-175">Run the *Students.aspx* page and the new student appears in the list.</span></span>

<span data-ttu-id="c33e4-176">[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-176">[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)</span></span>

<span data-ttu-id="c33e4-177">變更名稱以確認更新函式可運作，然後再刪除 學生確認 delete 函式可正常運作。</span><span class="sxs-lookup"><span data-stu-id="c33e4-177">Change the name to verify that the update function works, and then delete the student to verify that the delete function works.</span></span>

<span data-ttu-id="c33e4-178">[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="c33e4-178">[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)</span></span>

## <a name="using-select-stored-procedures"></a><span data-ttu-id="c33e4-179">使用選取的預存程序</span><span class="sxs-lookup"><span data-stu-id="c33e4-179">Using Select Stored Procedures</span></span>

<span data-ttu-id="c33e4-180">Entity Framework 不會自動執行預存程序這類`GetCourses`，而且您無法使用它們與`EntityDataSource`控制項。</span><span class="sxs-lookup"><span data-stu-id="c33e4-180">The Entity Framework does not automatically run stored procedures such as `GetCourses`, and you cannot use them with the `EntityDataSource` control.</span></span> <span data-ttu-id="c33e4-181">若要使用它們，您呼叫它們從程式碼。</span><span class="sxs-lookup"><span data-stu-id="c33e4-181">To use them, you call them from code.</span></span>

<span data-ttu-id="c33e4-182">開啟*InstructorsCourses.aspx.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="c33e4-182">Open the *InstructorsCourses.aspx.cs* file.</span></span> <span data-ttu-id="c33e4-183">`PopulateDropDownLists`方法使用 LINQ-實體查詢以擷取所有課程實體，讓它可以循環清單，並判斷的哪些講師指派給，哪些是未指定：</span><span class="sxs-lookup"><span data-stu-id="c33e4-183">The `PopulateDropDownLists` method uses a LINQ-to-Entities query to retrieve all course entities so that it can loop through the list and determine which ones an instructor is assigned to and which ones are unassigned:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

<span data-ttu-id="c33e4-184">下列程式碼取代此項：</span><span class="sxs-lookup"><span data-stu-id="c33e4-184">Replace this with the following code:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

<span data-ttu-id="c33e4-185">頁面現在會使用`GetCourses`預存程序擷取所有課程的清單。</span><span class="sxs-lookup"><span data-stu-id="c33e4-185">The page now uses the `GetCourses` stored procedure to retrieve the list of all courses.</span></span> <span data-ttu-id="c33e4-186">執行頁面，以驗證它的運作與以前一樣。</span><span class="sxs-lookup"><span data-stu-id="c33e4-186">Run the page to verify that it works as it did before.</span></span>

<span data-ttu-id="c33e4-187">(預存程序所擷取的實體的導覽屬性可能不會自動填入這些實體，取決於與相關的資料`ObjectContext`預設設定。</span><span class="sxs-lookup"><span data-stu-id="c33e4-187">(Navigation properties of entities retrieved by a stored procedure might not be automatically populated with the data related to those entities, depending on `ObjectContext` default settings.</span></span> <span data-ttu-id="c33e4-188">如需詳細資訊，請參閱[載入相關物件](https://msdn.microsoft.com/library/bb896272.aspx)MSDN Library 中。)</span><span class="sxs-lookup"><span data-stu-id="c33e4-188">For more information, see [Loading Related Objects](https://msdn.microsoft.com/library/bb896272.aspx) in the MSDN Library.)</span></span>

<span data-ttu-id="c33e4-189">在下一個教學課程中，您將學習如何使用動態資料功能，更輕鬆地程式和測試資料格式和驗證規則。</span><span class="sxs-lookup"><span data-stu-id="c33e4-189">In the next tutorial, you'll learn how to use Dynamic Data functionality to make it easier to program and test data formatting and validation rules.</span></span> <span data-ttu-id="c33e4-190">而不是指定每個網頁規則，例如資料格式字串，以及為必要欄位，您可以在資料模型中繼資料中指定這類規則，它們會自動套用每一頁上。</span><span class="sxs-lookup"><span data-stu-id="c33e4-190">Instead of specifying on each web page rules such as data format strings and whether or not a field is required, you can specify such rules in data model metadata and they're automatically applied on every page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c33e4-191">[上一頁](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="c33e4-191">[Previous](the-entity-framework-and-aspnet-getting-started-part-6.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-8.md)</span></span>
