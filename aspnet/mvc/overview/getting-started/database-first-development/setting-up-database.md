---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 教學課程：開始使用 EF Database First 使用 MVC 5
description: 本文說明如何開始使用的現有資料庫，並快速建立 web 應用程式，可讓使用者與資料互動。
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 8b094b7c334eaad510c46b55a99ec727b9c381c2
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889921"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a><span data-ttu-id="535a8-103">教學課程：開始使用 EF Database First 使用 MVC 5</span><span class="sxs-lookup"><span data-stu-id="535a8-103">Tutorial: Get started with EF Database First using MVC 5</span></span>

<span data-ttu-id="535a8-104">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="535a8-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="535a8-105">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="535a8-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="535a8-106">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="535a8-106">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="535a8-107">在系列的最後一個部分中，您將部署至 Azure 的站台和資料庫。</span><span class="sxs-lookup"><span data-stu-id="535a8-107">In the last part of the series, you will deploy the site and database to Azure.</span></span>

<span data-ttu-id="535a8-108">本文說明如何開始使用的現有資料庫，並快速建立 web 應用程式，可讓使用者與資料互動。</span><span class="sxs-lookup"><span data-stu-id="535a8-108">This article shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="535a8-109">它會使用 Entity Framework 6 和 MVC 5 建置 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="535a8-109">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="535a8-110">ASP.NET 樣板功能可讓您自動產生程式碼顯示、 更新、 建立和刪除資料。</span><span class="sxs-lookup"><span data-stu-id="535a8-110">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="535a8-111">使用 Visual Studio 內發行的工具，您可以輕鬆地部署站台和資料庫至 Azure。</span><span class="sxs-lookup"><span data-stu-id="535a8-111">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="535a8-112">此系列的一部分，著重於建立資料庫，並填入資料。</span><span class="sxs-lookup"><span data-stu-id="535a8-112">This part of the series focuses on creating a database and populating it with data.</span></span>

<span data-ttu-id="535a8-113">此系列是以貢獻寫入，Tom Dykstra 和 Rick Anderson。</span><span class="sxs-lookup"><span data-stu-id="535a8-113">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="535a8-114">它已改善根據意見反應的註解區段中的使用者。</span><span class="sxs-lookup"><span data-stu-id="535a8-114">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="535a8-115">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="535a8-115">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="535a8-116">將資料庫設定</span><span class="sxs-lookup"><span data-stu-id="535a8-116">Set up the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="535a8-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="535a8-117">Prerequisites</span></span>

* [<span data-ttu-id="535a8-118">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="535a8-118">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="introduction"></a><span data-ttu-id="535a8-119">簡介</span><span class="sxs-lookup"><span data-stu-id="535a8-119">Introduction</span></span>

<span data-ttu-id="535a8-120">本文會說明這種情況您有一個資料庫並想要產生程式碼，該資料庫的欄位為基礎的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="535a8-120">This article addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="535a8-121">這個方法會呼叫第一個資料庫的開發。</span><span class="sxs-lookup"><span data-stu-id="535a8-121">This approach is called Database First development.</span></span> <span data-ttu-id="535a8-122">如果您還沒有現有的資料庫，您可以改為使用此方法稱為 Code First 開發這牽涉到定義的資料類別和類別屬性從產生的資料庫。</span><span class="sxs-lookup"><span data-stu-id="535a8-122">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="535a8-123">將資料庫設定</span><span class="sxs-lookup"><span data-stu-id="535a8-123">Set up the database</span></span>

<span data-ttu-id="535a8-124">為了模擬的環境需要現有的資料庫，您會先使用一些預先填入的資料，建立資料庫，然後再建立 web 應用程式連接至資料庫。</span><span class="sxs-lookup"><span data-stu-id="535a8-124">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="535a8-125">本教學課程被開發使用 LocalDB。</span><span class="sxs-lookup"><span data-stu-id="535a8-125">This tutorial was developed using LocalDB.</span></span> <span data-ttu-id="535a8-126">您可以使用現有的資料庫伺服器，而不使用 LocalDB，但根據您的 Visual Studio 和您的資料庫類型的版本，所有的 Visual Studio 中的資料工具可能不支援。</span><span class="sxs-lookup"><span data-stu-id="535a8-126">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="535a8-127">如果工具不適用於您的資料庫中，您可能需要針對您的資料庫執行某些管理套件中的特定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="535a8-127">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="535a8-128">如果您的 Visual Studio 版本中有資料庫工具的問題，請確定您已安裝最新版的資料庫工具。</span><span class="sxs-lookup"><span data-stu-id="535a8-128">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="535a8-129">如需更新或安裝資料庫工具的資訊，請參閱[Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027)。</span><span class="sxs-lookup"><span data-stu-id="535a8-129">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="535a8-130">啟動 Visual Studio 並建立**SQL Server 資料庫專案**。</span><span class="sxs-lookup"><span data-stu-id="535a8-130">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="535a8-131">將專案命名為**ContosoUniversityData**。</span><span class="sxs-lookup"><span data-stu-id="535a8-131">Name the project **ContosoUniversityData**.</span></span>

![建立資料庫專案](setting-up-database/_static/image1.png)

<span data-ttu-id="535a8-133">您現在有了空的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="535a8-133">You now have an empty database project.</span></span> <span data-ttu-id="535a8-134">您將此將資料庫部署到 Azure 稍後在本教學課程中，因此您必須將 Azure SQL Database 設為專案的目標平台。</span><span class="sxs-lookup"><span data-stu-id="535a8-134">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="535a8-135">設定目標平台不會實際部署資料庫，例如：它只表示資料庫專案將會確認資料庫設計適用於目標平台。</span><span class="sxs-lookup"><span data-stu-id="535a8-135">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="535a8-136">若要設定目標平台，請開啟**屬性**的專案，然後選取**Microsoft Azure SQL Database**目標平台。</span><span class="sxs-lookup"><span data-stu-id="535a8-136">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

<span data-ttu-id="535a8-137">您可以建立本教學課程需要加上定義的資料表的 SQL 指令碼的資料表。</span><span class="sxs-lookup"><span data-stu-id="535a8-137">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="535a8-138">以滑鼠右鍵按一下您的專案，並加入新項目。</span><span class="sxs-lookup"><span data-stu-id="535a8-138">Right-click your project and add a new item.</span></span> <span data-ttu-id="535a8-139">選取 **資料表和檢視表** > **表格**並將它命名*學生*。</span><span class="sxs-lookup"><span data-stu-id="535a8-139">Select **Tables and Views** > **Table** and name it *Student*.</span></span>

<span data-ttu-id="535a8-140">在資料表的檔案中，取代下列程式碼來建立資料表中的 T-SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="535a8-140">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="535a8-141">請注意，[設計] 視窗會自動同步處理的程式碼。</span><span class="sxs-lookup"><span data-stu-id="535a8-141">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="535a8-142">您可以使用程式碼或設計工具。</span><span class="sxs-lookup"><span data-stu-id="535a8-142">You can work with either the code or designer.</span></span>

![顯示程式碼和設計](setting-up-database/_static/image5.png)

<span data-ttu-id="535a8-144">新增另一個資料表。</span><span class="sxs-lookup"><span data-stu-id="535a8-144">Add another table.</span></span> <span data-ttu-id="535a8-145">這次它命名為課程，並使用下列 T-SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="535a8-145">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="535a8-146">然後，重複一次，建立名為註冊的資料表。</span><span class="sxs-lookup"><span data-stu-id="535a8-146">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="535a8-147">您可以填入您的資料庫，以透過執行指令碼會在部署資料庫之後的資料。</span><span class="sxs-lookup"><span data-stu-id="535a8-147">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="535a8-148">加入專案中的部署後指令碼。</span><span class="sxs-lookup"><span data-stu-id="535a8-148">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="535a8-149">以滑鼠右鍵按一下您的專案，並加入新項目。</span><span class="sxs-lookup"><span data-stu-id="535a8-149">Right-click your project and add a new item.</span></span> <span data-ttu-id="535a8-150">選取 **使用者指令碼** > **部署後指令碼**。</span><span class="sxs-lookup"><span data-stu-id="535a8-150">Select **User Scripts** > **Post-Deployment Script**.</span></span> <span data-ttu-id="535a8-151">您可以使用預設名稱。</span><span class="sxs-lookup"><span data-stu-id="535a8-151">You can use the default name.</span></span>

<span data-ttu-id="535a8-152">將下列 T-SQL 程式碼新增至 部署後指令碼。</span><span class="sxs-lookup"><span data-stu-id="535a8-152">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="535a8-153">此指令碼只會將資料加入至資料庫，當不找到任何相符的記錄。</span><span class="sxs-lookup"><span data-stu-id="535a8-153">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="535a8-154">它不會覆寫或刪除任何您可能會在資料庫中輸入的資料。</span><span class="sxs-lookup"><span data-stu-id="535a8-154">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="535a8-155">請務必注意，在每次部署您的資料庫專案，執行部署後指令碼。</span><span class="sxs-lookup"><span data-stu-id="535a8-155">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="535a8-156">因此，您必須仔細考慮您的需求，撰寫這個指令碼時。</span><span class="sxs-lookup"><span data-stu-id="535a8-156">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="535a8-157">在某些情況下，您可能想要每次部署專案時，從頭開始從一組已知的資料。</span><span class="sxs-lookup"><span data-stu-id="535a8-157">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="535a8-158">在其他情況下，您可能不想變更現有的資料，以任何方式。</span><span class="sxs-lookup"><span data-stu-id="535a8-158">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="535a8-159">根據您的需求，您可以決定您是否需要部署後指令碼，或您要包含在指令碼中。</span><span class="sxs-lookup"><span data-stu-id="535a8-159">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="535a8-160">如需填入您的資料庫部署後指令碼的詳細資訊，請參閱[包括 SQL Server 資料庫專案中的資料](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)。</span><span class="sxs-lookup"><span data-stu-id="535a8-160">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="535a8-161">您現在有 4 個 SQL 指令碼檔案，但沒有任何實際的資料表。</span><span class="sxs-lookup"><span data-stu-id="535a8-161">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="535a8-162">您已準備好部署至 localdb 資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="535a8-162">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="535a8-163">在 Visual Studio 中，按一下 [開始] 按鈕 （或 F5），來建置及部署您的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="535a8-163">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="535a8-164">請檢查**輸出**索引標籤，以確認建置和部署是否成功。</span><span class="sxs-lookup"><span data-stu-id="535a8-164">Check the **Output** tab to verify that the build and deployment succeeded.</span></span>

<span data-ttu-id="535a8-165">若要查看已建立新資料庫，請開啟**SQL Server 物件總管**並尋找正確的本機資料庫伺服器中的專案名稱 (在此情況下 **(localdb) \ProjectsV13**)。</span><span class="sxs-lookup"><span data-stu-id="535a8-165">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV13**).</span></span>

<span data-ttu-id="535a8-166">若要查看資料表會填入資料，以滑鼠右鍵按一下資料表，然後選取**檢視資料**。</span><span class="sxs-lookup"><span data-stu-id="535a8-166">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![顯示資料表資料](setting-up-database/_static/image9.png)

<span data-ttu-id="535a8-168">資料表資料的可編輯檢視隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="535a8-168">An editable view of the table data is displayed.</span></span> <span data-ttu-id="535a8-169">例如，如果您選取**資料表** > **dbo.course** > **檢視資料**，您會看到三個資料行的資料表 (**課程**， **title**，以及**信用額度**) 和四個資料列。</span><span class="sxs-lookup"><span data-stu-id="535a8-169">For example, if you select **Tables** > **dbo.course** > **View Data**, you see a table with three columns (**Course**, **Title**, and **Credits**) and four rows.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="535a8-170">其他資源</span><span class="sxs-lookup"><span data-stu-id="535a8-170">Additional resources</span></span>

<span data-ttu-id="535a8-171">Code First 開發簡介範例，請參閱[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="535a8-171">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="535a8-172">如需更進階的範例，請參閱 <<c0> [ 建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="535a8-172">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="535a8-173">如需選取要使用哪個 Entity Framework 方法的指引，請參閱 < [Entity Framework 開發方式](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。</span><span class="sxs-lookup"><span data-stu-id="535a8-173">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="next-steps"></a><span data-ttu-id="535a8-174">後續步驟</span><span class="sxs-lookup"><span data-stu-id="535a8-174">Next steps</span></span>

<span data-ttu-id="535a8-175">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="535a8-175">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="535a8-176">將資料庫設定</span><span class="sxs-lookup"><span data-stu-id="535a8-176">Set up the database</span></span>

<span data-ttu-id="535a8-177">請前往下一篇文章，以了解如何建立 web 應用程式和資料模型。</span><span class="sxs-lookup"><span data-stu-id="535a8-177">Advance to the next article to learn how to create the web application and data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="535a8-178">建立 web 應用程式和資料模型</span><span class="sxs-lookup"><span data-stu-id="535a8-178">Create the web application and data models</span></span>](creating-the-web-application.md)