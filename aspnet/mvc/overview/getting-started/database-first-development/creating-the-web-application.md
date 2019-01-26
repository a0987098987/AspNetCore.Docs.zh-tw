---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 教學課程：建立 Web 應用程式和 ef 資料模型資料庫的第一個使用 ASP.NET MVC
description: 本文著重於建立 web 應用程式，並產生您的資料庫資料表為基礎的資料模型。
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 095d355866c9ab8fba3759f3e05e2a521992f3d6
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889765"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="fe244-103">教學課程：建立 Web 應用程式和 ef 資料模型資料庫的第一個使用 ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fe244-103">Tutorial: Create the the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="fe244-104">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="fe244-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="fe244-105">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="fe244-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="fe244-106">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="fe244-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="fe244-107">本文著重於建立 web 應用程式，並產生您的資料庫資料表為基礎的資料模型。</span><span class="sxs-lookup"><span data-stu-id="fe244-107">This article focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="fe244-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="fe244-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe244-109">建立 ASP.NET Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fe244-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="fe244-110">產生模型</span><span class="sxs-lookup"><span data-stu-id="fe244-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe244-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="fe244-111">Prerequisites</span></span>

* [<span data-ttu-id="fe244-112">開始使用 Entity Framework 6 Database First 使用 MVC 5</span><span class="sxs-lookup"><span data-stu-id="fe244-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="fe244-113">建立 ASP.NET Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fe244-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="fe244-114">在新的解決方案或與資料庫專案相同的方案中，建立新的專案在 Visual Studio 中，並選取**ASP.NET Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="fe244-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="fe244-115">將專案命名為**ContosoSite**。</span><span class="sxs-lookup"><span data-stu-id="fe244-115">Name the project **ContosoSite**.</span></span>

![建立專案](creating-the-web-application/_static/image1.png)

<span data-ttu-id="fe244-117">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="fe244-117">Click **OK**.</span></span>

<span data-ttu-id="fe244-118">在 [新增 ASP.NET 專案] 視窗中，選取**MVC**範本。</span><span class="sxs-lookup"><span data-stu-id="fe244-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="fe244-119">您可以清除**雲端中的主機**選項現在，因為您將部署更新版本的雲端應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe244-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="fe244-120">按一下 **確定**建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe244-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="fe244-121">建立專案的預設檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="fe244-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="fe244-122">在本教學課程中，您將使用 Entity Framework 6。</span><span class="sxs-lookup"><span data-stu-id="fe244-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="fe244-123">您可以在您的專案，透過 [管理 NuGet 封裝] 視窗中，仔細查看 Entity Framework 的版本。</span><span class="sxs-lookup"><span data-stu-id="fe244-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="fe244-124">如有必要，請更新您的 Entity Framework 的版本。</span><span class="sxs-lookup"><span data-stu-id="fe244-124">If necessary, update your version of Entity Framework.</span></span>

![顯示版本](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="fe244-126">產生模型</span><span class="sxs-lookup"><span data-stu-id="fe244-126">Generate the models</span></span>

<span data-ttu-id="fe244-127">您現在會從資料庫資料表建立 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="fe244-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="fe244-128">這些模型是您將使用的資料搭配使用的類別。</span><span class="sxs-lookup"><span data-stu-id="fe244-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="fe244-129">每個模型鏡像資料庫中的資料表，並包含對應到資料表中資料行的屬性。</span><span class="sxs-lookup"><span data-stu-id="fe244-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="fe244-130">以滑鼠右鍵按一下**模型**資料夾，然後選取**新增**並**新項目**。</span><span class="sxs-lookup"><span data-stu-id="fe244-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="fe244-131">在 [加入新項目] 視窗中，選取**資料**的左窗格中並**ADO.NET 實體資料模型**從中間窗格中的選項。</span><span class="sxs-lookup"><span data-stu-id="fe244-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="fe244-132">將新的模型檔案**ContosoModel**。</span><span class="sxs-lookup"><span data-stu-id="fe244-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="fe244-133">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="fe244-133">Click **Add**.</span></span>

<span data-ttu-id="fe244-134">在 [實體資料模型精靈] 中，選取**資料庫的 EF Designer**。</span><span class="sxs-lookup"><span data-stu-id="fe244-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="fe244-135">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="fe244-135">Click **Next**.</span></span>

<span data-ttu-id="fe244-136">如果您有在您的開發環境內定義的資料庫連接，您可能會看到其中一個預先選取這些連線。</span><span class="sxs-lookup"><span data-stu-id="fe244-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="fe244-137">不過，您會想要建立新的連接到您在本教學課程的第一個部分中建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="fe244-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="fe244-138">按一下 [**新的連接**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fe244-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="fe244-139">在 [連接屬性] 視窗中，提供您的資料庫建立所在的本機伺服器的名稱 (在此情況下 **(localdb) \Projects13**)。</span><span class="sxs-lookup"><span data-stu-id="fe244-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\Projects13**).</span></span> <span data-ttu-id="fe244-140">提供伺服器名稱之後, 請從可用的資料庫選取 ContosoUniversityData。</span><span class="sxs-lookup"><span data-stu-id="fe244-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![設定連接屬性](creating-the-web-application/_static/image8.png)

<span data-ttu-id="fe244-142">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="fe244-142">Click **OK**.</span></span>

<span data-ttu-id="fe244-143">現在會顯示正確的連接屬性。</span><span class="sxs-lookup"><span data-stu-id="fe244-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="fe244-144">您可以在 Web.Config 檔案中使用連接的預設名稱。</span><span class="sxs-lookup"><span data-stu-id="fe244-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="fe244-145">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="fe244-145">Click **Next**.</span></span>

<span data-ttu-id="fe244-146">選取最新版的 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="fe244-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="fe244-147">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="fe244-147">Click **Next**.</span></span>

<span data-ttu-id="fe244-148">選取 **資料表**來產生所有的三個資料表的模型。</span><span class="sxs-lookup"><span data-stu-id="fe244-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="fe244-149">按一下 [ **完成**]。</span><span class="sxs-lookup"><span data-stu-id="fe244-149">Click **Finish**.</span></span>

<span data-ttu-id="fe244-150">如果您收到安全性警告，請選取**確定**繼續執行範本。</span><span class="sxs-lookup"><span data-stu-id="fe244-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="fe244-151">模型從資料庫資料表中，產生，並會顯示一個圖表，顯示的屬性和資料表之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="fe244-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![模型的圖表](creating-the-web-application/_static/image11.png)

<span data-ttu-id="fe244-153">[模型] 資料夾現在包含已從資料庫產生模型與相關的許多新檔案。</span><span class="sxs-lookup"><span data-stu-id="fe244-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="fe244-154">**ContosoModel.Context.cs**檔案包含的類別衍生自**DbContext**類別，並提供屬性，對應至資料庫資料表每個模型類別。</span><span class="sxs-lookup"><span data-stu-id="fe244-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="fe244-155">**Course.cs**， **Enrollment.cs**，並**Student.cs**檔案包含代表資料庫資料表的模型類別。</span><span class="sxs-lookup"><span data-stu-id="fe244-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="fe244-156">使用樣板時，您將使用模型類別和內容類別。</span><span class="sxs-lookup"><span data-stu-id="fe244-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="fe244-157">繼續進行本教學課程之前，建置專案。</span><span class="sxs-lookup"><span data-stu-id="fe244-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="fe244-158">在下一步 區段中，您會產生資料模型為基礎的程式碼，但如果尚未建置專案，將無法運作一節。</span><span class="sxs-lookup"><span data-stu-id="fe244-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe244-159">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fe244-159">Next steps</span></span>

<span data-ttu-id="fe244-160">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="fe244-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe244-161">建立 ASP.NET web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fe244-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="fe244-162">產生模型</span><span class="sxs-lookup"><span data-stu-id="fe244-162">Generated the models</span></span>

<span data-ttu-id="fe244-163">前往下一篇文章，以了解如何建立會產生資料模型為基礎的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fe244-163">Advance to the next article to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="fe244-164">產生檢視</span><span class="sxs-lookup"><span data-stu-id="fe244-164">Generating views</span></span>](generating-views.md)