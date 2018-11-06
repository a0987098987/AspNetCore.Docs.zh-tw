---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 開始使用 Entity Framework 6 Database First 使用 MVC 5 |Microsoft Docs
author: Rick-Anderson
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 7fcb2b82dfa27ae192e1890c0c771d68658760a4
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021140"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="e54ea-104">開始使用 Entity Framework 6 Database First 使用 MVC 5</span><span class="sxs-lookup"><span data-stu-id="e54ea-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="e54ea-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e54ea-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e54ea-106">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e54ea-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="e54ea-107">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="e54ea-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="e54ea-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="e54ea-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="e54ea-109">在系列的最後一個部分中，您將部署至 Azure 的站台和資料庫。</span><span class="sxs-lookup"><span data-stu-id="e54ea-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="e54ea-110">此系列的一部分，著重於建立資料庫，並填入資料。</span><span class="sxs-lookup"><span data-stu-id="e54ea-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="e54ea-111">此系列是以貢獻寫入，Tom Dykstra 和 Rick Anderson。</span><span class="sxs-lookup"><span data-stu-id="e54ea-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="e54ea-112">它已改善根據意見反應的註解區段中的使用者。</span><span class="sxs-lookup"><span data-stu-id="e54ea-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="e54ea-113">簡介</span><span class="sxs-lookup"><span data-stu-id="e54ea-113">Introduction</span></span>

<span data-ttu-id="e54ea-114">本主題說明如何開始使用的現有資料庫，並快速建立 web 應用程式，可讓使用者與資料互動。</span><span class="sxs-lookup"><span data-stu-id="e54ea-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="e54ea-115">它會使用 Entity Framework 6 和 MVC 5 建置 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e54ea-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="e54ea-116">ASP.NET 樣板功能可讓您自動產生程式碼顯示、 更新、 建立和刪除資料。</span><span class="sxs-lookup"><span data-stu-id="e54ea-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="e54ea-117">使用 Visual Studio 內發行的工具，您可以輕鬆地部署站台和資料庫至 Azure。</span><span class="sxs-lookup"><span data-stu-id="e54ea-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="e54ea-118">本主題說明這種情況您有一個資料庫並想要產生程式碼，該資料庫的欄位為基礎的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e54ea-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="e54ea-119">這個方法會呼叫第一個資料庫的開發。</span><span class="sxs-lookup"><span data-stu-id="e54ea-119">This approach is called Database First development.</span></span> <span data-ttu-id="e54ea-120">如果您還沒有現有的資料庫，您可以改為使用此方法稱為 Code First 開發這牽涉到定義的資料類別和類別屬性從產生的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e54ea-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="e54ea-121">Code First 開發簡介範例，請參閱[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="e54ea-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="e54ea-122">如需更進階的範例，請參閱 <<c0> [ 建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="e54ea-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="e54ea-123">如需選取要使用哪個 Entity Framework 方法的指引，請參閱 < [Entity Framework 開發方式](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。</span><span class="sxs-lookup"><span data-stu-id="e54ea-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e54ea-124">必要條件</span><span class="sxs-lookup"><span data-stu-id="e54ea-124">Prerequisites</span></span>

<span data-ttu-id="e54ea-125">Visual Studio 2013 或 Visual Studio Express 2013 for Web</span><span class="sxs-lookup"><span data-stu-id="e54ea-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="e54ea-126">將資料庫設定</span><span class="sxs-lookup"><span data-stu-id="e54ea-126">Set up the database</span></span>

<span data-ttu-id="e54ea-127">為了模擬的環境需要現有的資料庫，您會先使用一些預先填入的資料，建立資料庫，然後再建立 web 應用程式連接至資料庫。</span><span class="sxs-lookup"><span data-stu-id="e54ea-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="e54ea-128">本教學課程是使用 LocalDB Visual Studio 2013 或 Visual Studio Express 2013 for Web 所開發。</span><span class="sxs-lookup"><span data-stu-id="e54ea-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="e54ea-129">您可以使用現有的資料庫伺服器，而不使用 LocalDB，但根據您的 Visual Studio 和您的資料庫類型的版本，所有的 Visual Studio 中的資料工具可能不支援。</span><span class="sxs-lookup"><span data-stu-id="e54ea-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="e54ea-130">如果工具不適用於您的資料庫中，您可能需要針對您的資料庫執行某些管理套件中的特定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="e54ea-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="e54ea-131">如果您的 Visual Studio 版本中有資料庫工具的問題，請確定您已安裝最新版的資料庫工具。</span><span class="sxs-lookup"><span data-stu-id="e54ea-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="e54ea-132">如需更新或安裝資料庫工具的資訊，請參閱[Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027)。</span><span class="sxs-lookup"><span data-stu-id="e54ea-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="e54ea-133">啟動 Visual Studio 並建立**SQL Server 資料庫專案**。</span><span class="sxs-lookup"><span data-stu-id="e54ea-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="e54ea-134">將專案命名為**ContosoUniversityData**。</span><span class="sxs-lookup"><span data-stu-id="e54ea-134">Name the project **ContosoUniversityData**.</span></span>

![建立資料庫專案](setting-up-database/_static/image1.png)

<span data-ttu-id="e54ea-136">您現在有了空的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="e54ea-136">You now have an empty database project.</span></span> <span data-ttu-id="e54ea-137">您將此將資料庫部署到 Azure 稍後在本教學課程中，因此您必須將 Azure SQL Database 設為專案的目標平台。</span><span class="sxs-lookup"><span data-stu-id="e54ea-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="e54ea-138">設定目標平台不會實際部署資料庫，例如：它只表示資料庫專案將會確認資料庫設計適用於目標平台。</span><span class="sxs-lookup"><span data-stu-id="e54ea-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="e54ea-139">若要設定目標平台，請開啟**屬性**的專案，然後選取**Microsoft Azure SQL Database**目標平台。</span><span class="sxs-lookup"><span data-stu-id="e54ea-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![設定目標平台](setting-up-database/_static/image2.png)

<span data-ttu-id="e54ea-141">您可以建立本教學課程需要加上定義的資料表的 SQL 指令碼的資料表。</span><span class="sxs-lookup"><span data-stu-id="e54ea-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="e54ea-142">以滑鼠右鍵按一下您的專案，並加入新項目。</span><span class="sxs-lookup"><span data-stu-id="e54ea-142">Right-click your project and add a new item.</span></span>

![加入新項目](setting-up-database/_static/image3.png)

<span data-ttu-id="e54ea-144">新增名為學生的新資料表。</span><span class="sxs-lookup"><span data-stu-id="e54ea-144">Add a new table named Student.</span></span>

![將 student 資料表](setting-up-database/_static/image4.png)

<span data-ttu-id="e54ea-146">在資料表的檔案中，取代下列程式碼來建立資料表中的 T-SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="e54ea-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="e54ea-147">請注意，[設計] 視窗會自動同步處理的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e54ea-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="e54ea-148">您可以使用程式碼或設計工具。</span><span class="sxs-lookup"><span data-stu-id="e54ea-148">You can work with either the code or designer.</span></span>

![顯示程式碼和設計](setting-up-database/_static/image5.png)

<span data-ttu-id="e54ea-150">新增另一個資料表。</span><span class="sxs-lookup"><span data-stu-id="e54ea-150">Add another table.</span></span> <span data-ttu-id="e54ea-151">這次它命名為課程，並使用下列 T-SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="e54ea-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="e54ea-152">然後，重複一次，建立名為註冊的資料表。</span><span class="sxs-lookup"><span data-stu-id="e54ea-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="e54ea-153">您可以填入您的資料庫，以透過執行指令碼會在部署資料庫之後的資料。</span><span class="sxs-lookup"><span data-stu-id="e54ea-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="e54ea-154">加入專案中的部署後指令碼。</span><span class="sxs-lookup"><span data-stu-id="e54ea-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="e54ea-155">您可以使用預設名稱。</span><span class="sxs-lookup"><span data-stu-id="e54ea-155">You can use the default name.</span></span>

![加入部署後指令碼](setting-up-database/_static/image6.png)

<span data-ttu-id="e54ea-157">將下列 T-SQL 程式碼新增至 部署後指令碼。</span><span class="sxs-lookup"><span data-stu-id="e54ea-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="e54ea-158">此指令碼只會將資料加入至資料庫，當不找到任何相符的記錄。</span><span class="sxs-lookup"><span data-stu-id="e54ea-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="e54ea-159">它不會覆寫或刪除任何您可能會在資料庫中輸入的資料。</span><span class="sxs-lookup"><span data-stu-id="e54ea-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="e54ea-160">請務必注意，在每次部署您的資料庫專案，執行部署後指令碼。</span><span class="sxs-lookup"><span data-stu-id="e54ea-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="e54ea-161">因此，您必須仔細考慮您的需求，撰寫這個指令碼時。</span><span class="sxs-lookup"><span data-stu-id="e54ea-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="e54ea-162">在某些情況下，您可能想要每次部署專案時，從頭開始從一組已知的資料。</span><span class="sxs-lookup"><span data-stu-id="e54ea-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="e54ea-163">在其他情況下，您可能不想變更現有的資料，以任何方式。</span><span class="sxs-lookup"><span data-stu-id="e54ea-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="e54ea-164">根據您的需求，您可以決定您是否需要部署後指令碼，或您要包含在指令碼中。</span><span class="sxs-lookup"><span data-stu-id="e54ea-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="e54ea-165">如需填入您的資料庫部署後指令碼的詳細資訊，請參閱[包括 SQL Server 資料庫專案中的資料](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e54ea-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="e54ea-166">您現在有 4 個 SQL 指令碼檔案，但沒有任何實際的資料表。</span><span class="sxs-lookup"><span data-stu-id="e54ea-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="e54ea-167">您已準備好部署至 localdb 資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="e54ea-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="e54ea-168">在 Visual Studio 中，按一下 [開始] 按鈕 （或 F5），來建置及部署您的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="e54ea-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="e54ea-169">請檢查 [輸出] 索引標籤，以確認建置和部署成功。</span><span class="sxs-lookup"><span data-stu-id="e54ea-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![顯示輸出](setting-up-database/_static/image7.png)

<span data-ttu-id="e54ea-171">若要查看已建立新資料庫，請開啟**SQL Server 物件總管**並尋找正確的本機資料庫伺服器中的專案名稱 (在此情況下 **(localdb) \ProjectsV12**)。</span><span class="sxs-lookup"><span data-stu-id="e54ea-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![顯示新的資料庫](setting-up-database/_static/image8.png)

<span data-ttu-id="e54ea-173">若要查看資料表會填入資料，以滑鼠右鍵按一下資料表，然後選取**檢視資料**。</span><span class="sxs-lookup"><span data-stu-id="e54ea-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![顯示資料表資料](setting-up-database/_static/image9.png)

<span data-ttu-id="e54ea-175">資料表資料的可編輯檢視隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="e54ea-175">An editable view of the table data is displayed.</span></span>

![顯示資料表資料的結果](setting-up-database/_static/image10.png)

<span data-ttu-id="e54ea-177">您的資料庫現在已設定，並填入資料。</span><span class="sxs-lookup"><span data-stu-id="e54ea-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="e54ea-178">在下一個教學課程中，您將建立資料庫的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e54ea-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e54ea-179">下一步</span><span class="sxs-lookup"><span data-stu-id="e54ea-179">Next</span></span>](creating-the-web-application.md)
