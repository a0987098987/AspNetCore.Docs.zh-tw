---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 擷取和顯示資料與模型繫結和 web form |Microsoft Docs
author: tfitzmac
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 8153642762cf7032f335d21c8c67eac9b52ed2f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823919"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="01757-104">擷取和顯示資料與模型繫結和 web form</span><span class="sxs-lookup"><span data-stu-id="01757-104">Retrieving and displaying data with model binding and web forms</span></span>
====================
<span data-ttu-id="01757-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="01757-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="01757-106">本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="01757-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="01757-107">模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="01757-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="01757-108">此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。</span><span class="sxs-lookup"><span data-stu-id="01757-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="01757-109">模型繫結模式適用於任何資料存取技術。</span><span class="sxs-lookup"><span data-stu-id="01757-109">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="01757-110">在本教學課程中，您將使用 Entity Framework，但您可以使用最熟悉的資料存取技術。</span><span class="sxs-lookup"><span data-stu-id="01757-110">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="01757-111">從資料繫結的伺服器控制項，例如 GridView、 ListView、 DetailsView 或 FormView 控制項，您可以指定要用於選取、 更新、 刪除和建立資料方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="01757-111">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="01757-112">在本教學課程中，您會指定 SelectMethod 值。</span><span class="sxs-lookup"><span data-stu-id="01757-112">In this tutorial, you will specify a value for the SelectMethod.</span></span>
> 
> <span data-ttu-id="01757-113">在該方法中，您提供的邏輯擷取資料。</span><span class="sxs-lookup"><span data-stu-id="01757-113">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="01757-114">在下一個教學課程中，您將 UpdateMethod、 DeleteMethod 和 InsertMethod 設定值。</span><span class="sxs-lookup"><span data-stu-id="01757-114">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
> 
> <span data-ttu-id="01757-115">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。</span><span class="sxs-lookup"><span data-stu-id="01757-115">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="01757-116">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="01757-116">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="01757-117">它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。</span><span class="sxs-lookup"><span data-stu-id="01757-117">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="01757-118">在本教學課程中，您可以執行應用程式在 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="01757-118">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="01757-119">您也可以提供應用程式透過網際網路將它部署至主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="01757-119">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="01757-120">Microsoft 提供免費的 web 裝載中的最多 10 個 web sites</span><span class="sxs-lookup"><span data-stu-id="01757-120">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="01757-121">[免費 Azure 試用帳戶](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="01757-121">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="01757-122">如需有關如何將 Visual Studio web 專案部署至 Azure App Service Web Apps 的資訊，請參閱[使用 Visual Studio 的 ASP.NET Web 部署](../../deployment/visual-studio-web-deployment/introduction.md)系列。</span><span class="sxs-lookup"><span data-stu-id="01757-122">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="01757-123">該教學課程也會示範如何使用 Entity Framework Code First Migrations，將 SQL Server 資料庫部署到 Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="01757-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="01757-124">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="01757-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="01757-125">Microsoft Visual Studio 2013 或 Microsoft Visual Studio Express 2013 for Web</span><span class="sxs-lookup"><span data-stu-id="01757-125">Microsoft Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web</span></span>
>   
> 
> <span data-ttu-id="01757-126">本教學課程也適用於使用 Visual Studio 2012，但會在使用者介面和專案範本中的一些差異。</span><span class="sxs-lookup"><span data-stu-id="01757-126">This tutorial also works with Visual Studio 2012 but there will be some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="01757-127">您將建置</span><span class="sxs-lookup"><span data-stu-id="01757-127">What you'll build</span></span>

<span data-ttu-id="01757-128">在本教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="01757-128">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="01757-129">建立與註冊課程的學生反映一所大學的資料物件</span><span class="sxs-lookup"><span data-stu-id="01757-129">Build data objects that reflect a university with students enrolled in courses</span></span>
2. <span data-ttu-id="01757-130">建置從物件的資料庫資料表</span><span class="sxs-lookup"><span data-stu-id="01757-130">Build database tables from the objects</span></span>
3. <span data-ttu-id="01757-131">填入測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="01757-131">Populate the database with test data</span></span>
4. <span data-ttu-id="01757-132">在 web 表單中顯示資料</span><span class="sxs-lookup"><span data-stu-id="01757-132">Display data in a web form</span></span>

## <a name="set-up-project"></a><span data-ttu-id="01757-133">設定專案</span><span class="sxs-lookup"><span data-stu-id="01757-133">Set up project</span></span>

<span data-ttu-id="01757-134">在 Visual Studio 2013 中，建立新**ASP.NET Web 應用程式**稱為**ContosoUniversityModelBinding**。</span><span class="sxs-lookup"><span data-stu-id="01757-134">In Visual Studio 2013, create a new **ASP.NET Web Application** called **ContosoUniversityModelBinding**.</span></span>

![建立專案](retrieving-data/_static/image2.png)

<span data-ttu-id="01757-136">選取 Web Form 範本，並保留其他預設選項。</span><span class="sxs-lookup"><span data-stu-id="01757-136">Select the Web Forms template, and leave the other default options.</span></span> <span data-ttu-id="01757-137">按一下 [確定] 以安裝專案。</span><span class="sxs-lookup"><span data-stu-id="01757-137">Click OK to setup the project.</span></span>

![選取 web form](retrieving-data/_static/image3.png)

<span data-ttu-id="01757-139">首先，您將幾個小變更，以自訂網站的外觀。</span><span class="sxs-lookup"><span data-stu-id="01757-139">First, you will make a couple of small changes to customize the appearance of the site.</span></span> <span data-ttu-id="01757-140">開啟**Site.Master**檔案，並將標題變更為包含 Contoso 大學，而不是我的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="01757-140">Open the **Site.Master** file, and change the title to include Contoso University instead of My ASP.NET Application.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

<span data-ttu-id="01757-141">然後，將變更從標頭文字**應用程式名稱**要**Contoso 大學**。</span><span class="sxs-lookup"><span data-stu-id="01757-141">Then, change the header text from **Application name** to **Contoso University**.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

<span data-ttu-id="01757-142">也在 Site.Master，變更會出現在標頭可反映與此站台相關的頁面導覽連結。</span><span class="sxs-lookup"><span data-stu-id="01757-142">Also in Site.Master, change the navigation links that appear in the header to reflect the pages that are relevant to this site.</span></span> <span data-ttu-id="01757-143">您不需要其中一個**有關**頁面或**連絡人**頁面上，因此可以移除那些連結。</span><span class="sxs-lookup"><span data-stu-id="01757-143">You will not need either the **About** page or the **Contact** page so those links can be removed.</span></span> <span data-ttu-id="01757-144">相反地，您必須呼叫頁面的連結**學生**。</span><span class="sxs-lookup"><span data-stu-id="01757-144">Instead, you will need a link to a page called **Students**.</span></span> <span data-ttu-id="01757-145">尚未建立此頁面。</span><span class="sxs-lookup"><span data-stu-id="01757-145">This page has not been created yet.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

<span data-ttu-id="01757-146">儲存並關閉 Site.Master。</span><span class="sxs-lookup"><span data-stu-id="01757-146">Save and close Site.Master.</span></span>

<span data-ttu-id="01757-147">現在，您將建立顯示學生資料的 web 表單。</span><span class="sxs-lookup"><span data-stu-id="01757-147">Now, you'll create the web form for displaying student data.</span></span> <span data-ttu-id="01757-148">以滑鼠右鍵按一下您的專案，並**新增****新項目**。</span><span class="sxs-lookup"><span data-stu-id="01757-148">Right-click your project, and **Add** a **New Item**.</span></span> <span data-ttu-id="01757-149">選取 **使用主版頁面的 Web Form**範本，並將它命名**Students.aspx**。</span><span class="sxs-lookup"><span data-stu-id="01757-149">Select the **Web Form with Master Page** template, and name it **Students.aspx**.</span></span>

![建立頁面](retrieving-data/_static/image5.png)

<span data-ttu-id="01757-151">選取  **Site.Master**為新的 web form 主版頁面。</span><span class="sxs-lookup"><span data-stu-id="01757-151">Select **Site.Master** as the master page for the new web form.</span></span>

## <a name="create-the-data-models-and-database"></a><span data-ttu-id="01757-152">建立資料模型和資料庫</span><span class="sxs-lookup"><span data-stu-id="01757-152">Create the data models and database</span></span>

<span data-ttu-id="01757-153">您將使用 Code First 移轉來建立物件和對應的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="01757-153">You will use Code First Migrations to create objects and the corresponding database tables.</span></span> <span data-ttu-id="01757-154">這些資料表會儲存有關學生和其課程的資訊。</span><span class="sxs-lookup"><span data-stu-id="01757-154">These tables will store information about the students and their courses.</span></span>

<span data-ttu-id="01757-155">在 [模型] 資料夾中，新增名為的新類別**UniversityModels.cs**。</span><span class="sxs-lookup"><span data-stu-id="01757-155">In the Models folder, add a new class named **UniversityModels.cs**.</span></span>

![建立模型類別](retrieving-data/_static/image7.png)

<span data-ttu-id="01757-157">在此檔案中，定義 SchoolContext、 學生、 註冊和課程類別，如下所示：</span><span class="sxs-lookup"><span data-stu-id="01757-157">In this file, define the SchoolContext, Student, Enrollment, and Course classes as follows:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

<span data-ttu-id="01757-158">SchoolContext 類別衍生自 DbContext，管理資料庫連接和資料變更。</span><span class="sxs-lookup"><span data-stu-id="01757-158">The SchoolContext class derives from DbContext, which manages the database connection and changes in the data.</span></span>

<span data-ttu-id="01757-159">在學生類別中，請注意已套用至的屬性**FirstName**， **LastName**，並**年**屬性。</span><span class="sxs-lookup"><span data-stu-id="01757-159">In the Student class, notice the attributes that have been applied to the **FirstName**, **LastName**, and **Year** properties.</span></span> <span data-ttu-id="01757-160">這些屬性會用於本教學課程中的資料驗證。</span><span class="sxs-lookup"><span data-stu-id="01757-160">These attributes will be used for data validation in this tutorial.</span></span> <span data-ttu-id="01757-161">若要簡化此示範專案的程式碼，只有這些屬性同時標示為資料驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="01757-161">To simplify the code for this demonstration project, only these properties were marked with data-validation attributes.</span></span> <span data-ttu-id="01757-162">在實際的專案中，您會將驗證屬性套用至需要驗證的資料，例如註冊和課程類別中屬性的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="01757-162">In a real project, you would apply validation attributes to all properties that need validated data, such as properties in the Enrollment and Course classes.</span></span>

<span data-ttu-id="01757-163">儲存 UniversityModels.cs。</span><span class="sxs-lookup"><span data-stu-id="01757-163">Save UniversityModels.cs.</span></span>

<span data-ttu-id="01757-164">若要設定資料庫，根據這些類別，您將使用 Code First 移轉的工具。</span><span class="sxs-lookup"><span data-stu-id="01757-164">You will use the tools for Code First Migrations to set up a database based on these classes.</span></span>

<span data-ttu-id="01757-165">在  **Package Manager Console**，執行命令：</span><span class="sxs-lookup"><span data-stu-id="01757-165">In **Package Manager Console**, run the command:</span></span>  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

<span data-ttu-id="01757-166">如果命令成功完成，您就會收到一則訊息指出已啟用移轉，</span><span class="sxs-lookup"><span data-stu-id="01757-166">If the command completes successfully you will receive a message stating migrations have been enabled,</span></span>

![啟用移轉](retrieving-data/_static/image8.png)

<span data-ttu-id="01757-168">請注意，新的檔案命名為**Configuration.cs**已建立。</span><span class="sxs-lookup"><span data-stu-id="01757-168">Notice that a new file named **Configuration.cs** has been created.</span></span> <span data-ttu-id="01757-169">在 Visual Studio 中，此檔案會自動開啟它建立之後。</span><span class="sxs-lookup"><span data-stu-id="01757-169">In Visual Studio, this file is automatically opened after it is created.</span></span> <span data-ttu-id="01757-170">設定類別包含**種子**方法可讓您預先填入測試資料的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="01757-170">The Configuration class contains a **Seed** method which enables you to pre-populate the database tables with test data.</span></span>

<span data-ttu-id="01757-171">種子方法中加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="01757-171">Add the following code to the Seed method.</span></span> <span data-ttu-id="01757-172">您必須新增**使用**陳述式**ContosoUniversityModelBinding.Models**命名空間。</span><span class="sxs-lookup"><span data-stu-id="01757-172">You'll need to add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

<span data-ttu-id="01757-173">儲存 Configuration.cs。</span><span class="sxs-lookup"><span data-stu-id="01757-173">Save Configuration.cs.</span></span>

<span data-ttu-id="01757-174">在 [套件管理員] 主控台中，執行命令`add-migration initial`。</span><span class="sxs-lookup"><span data-stu-id="01757-174">In the Package Manager Console, run the command `add-migration initial`.</span></span>

<span data-ttu-id="01757-175">然後，執行命令`update-database`。</span><span class="sxs-lookup"><span data-stu-id="01757-175">Then, run the command `update-database`.</span></span>

<span data-ttu-id="01757-176">如果執行此命令時，您會收到例外狀況，，就可以 StudentID 和 CourseID 的值具有不同的種子方法中的值。</span><span class="sxs-lookup"><span data-stu-id="01757-176">If you receive an exception when running this command, it is possible that the values for StudentID and CourseID have varied from the values in the Seed method.</span></span> <span data-ttu-id="01757-177">開啟資料庫中的這些資料表和 StudentID 和 CourseID 尋找現有的值。</span><span class="sxs-lookup"><span data-stu-id="01757-177">Open those tables in the database and find existing values for StudentID and CourseID.</span></span> <span data-ttu-id="01757-178">將這些值加入程式碼植入的註冊項目資料表中。</span><span class="sxs-lookup"><span data-stu-id="01757-178">Add those values in the code for seeding the Enrollments table.</span></span>

<span data-ttu-id="01757-179">已加入的資料庫檔案，但它目前隱藏在專案中。</span><span class="sxs-lookup"><span data-stu-id="01757-179">The database file has been added, but it is currently hidden in the project.</span></span> <span data-ttu-id="01757-180">按一下 **顯示所有檔案**顯示的檔案。</span><span class="sxs-lookup"><span data-stu-id="01757-180">Click **Show All Files** to display the file.</span></span>

![顯示所有檔案](retrieving-data/_static/image10.png)

<span data-ttu-id="01757-182">請注意.mdf 檔案現在會出現在應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="01757-182">Notice the .mdf file now appears in the App\_Data folder.</span></span>

![資料庫檔案](retrieving-data/_static/image12.png)

<span data-ttu-id="01757-184">按兩下.mdf 檔案，然後開啟 [伺服器總管] 中。</span><span class="sxs-lookup"><span data-stu-id="01757-184">Double-click the .mdf file and open the Server Explorer.</span></span> <span data-ttu-id="01757-185">資料表現在會存在，並會填入資料。</span><span class="sxs-lookup"><span data-stu-id="01757-185">The tables now exist and are populated with data.</span></span>

![資料庫資料表](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a><span data-ttu-id="01757-187">顯示學生和相關的資料表的資料</span><span class="sxs-lookup"><span data-stu-id="01757-187">Display data from Students and related tables</span></span>

<span data-ttu-id="01757-188">在資料庫中的資料，您現在已準備好擷取該資料，並顯示在網頁中。</span><span class="sxs-lookup"><span data-stu-id="01757-188">With data in the database, you are now ready to retrieve that data and display it in a web page.</span></span> <span data-ttu-id="01757-189">您將使用**GridView**控制項來顯示資料行和資料列中的資料。</span><span class="sxs-lookup"><span data-stu-id="01757-189">You will use a **GridView** control to display the data in columns and rows.</span></span>

<span data-ttu-id="01757-190">開啟 Students.aspx，並找出**MainContent**預留位置。</span><span class="sxs-lookup"><span data-stu-id="01757-190">Open Students.aspx, and locate the **MainContent** placeholder.</span></span> <span data-ttu-id="01757-191">在該預留位置中，新增**GridView**控制，以包含下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="01757-191">Within that placeholder, add a **GridView** control that includes the following code.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

<span data-ttu-id="01757-192">有幾個重要的概念，您會注意到這個標記程式碼中。</span><span class="sxs-lookup"><span data-stu-id="01757-192">There are a couple of important concepts in this markup code for you to notice.</span></span> <span data-ttu-id="01757-193">首先，請注意值會設定為**SelectMethod** GridView 項目中的屬性。</span><span class="sxs-lookup"><span data-stu-id="01757-193">First, notice that a value is set for the **SelectMethod** property in the GridView element.</span></span> <span data-ttu-id="01757-194">這個值會指定用於擷取資料的 GridView 方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="01757-194">This value specifies the name of the method that is used for retrieving data for the GridView.</span></span> <span data-ttu-id="01757-195">您將在下一個步驟中建立這個方法。</span><span class="sxs-lookup"><span data-stu-id="01757-195">You will create this method in the next step.</span></span> <span data-ttu-id="01757-196">其次，注意**ItemType**屬性設定為您稍早建立的 Student 類別。</span><span class="sxs-lookup"><span data-stu-id="01757-196">Second, notice that the **ItemType** property is set to the Student class that you created earlier.</span></span> <span data-ttu-id="01757-197">藉由設定此值，您可以參考該類別標記程式碼中的屬性。</span><span class="sxs-lookup"><span data-stu-id="01757-197">By setting this value, you can refer to properties of that class in the markup code.</span></span> <span data-ttu-id="01757-198">比方說，Student 類別會包含名為註冊的集合。</span><span class="sxs-lookup"><span data-stu-id="01757-198">For example, the Student class contains a collection named Enrollments.</span></span> <span data-ttu-id="01757-199">您可以使用**Item.Enrollments**來擷取該集合，然後使用 LINQ 語法擷取每個學生的已註冊的點數總和。</span><span class="sxs-lookup"><span data-stu-id="01757-199">You can use **Item.Enrollments** to retrieve that collection and then use LINQ syntax to retrieve the sum of enrolled credits for each student.</span></span>

<span data-ttu-id="01757-200">在程式碼後置檔案中，您要新增指定的方法**SelectMethod**值。</span><span class="sxs-lookup"><span data-stu-id="01757-200">In the code-behind file, you need to add the method that is specified for the **SelectMethod** value.</span></span> <span data-ttu-id="01757-201">開啟**Students.aspx.cs**，並新增**使用**陳述式**ContosoUniversityModelBinding.Models**並**System.Data.Entity**命名空間。</span><span class="sxs-lookup"><span data-stu-id="01757-201">Open **Students.aspx.cs**, and add **using** statements for the **ContosoUniversityModelBinding.Models** and **System.Data.Entity** namespaces.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

<span data-ttu-id="01757-202">然後，新增下列方法。</span><span class="sxs-lookup"><span data-stu-id="01757-202">Then, add the following method.</span></span> <span data-ttu-id="01757-203">請注意，這個方法的名稱會符合 SelectMethod 您提供的名稱。</span><span class="sxs-lookup"><span data-stu-id="01757-203">Notice that the name of this method matches the name you provided for SelectMethod.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

<span data-ttu-id="01757-204">**Include**子句可以改善此查詢的效能，但不是不可或缺的查詢運作。</span><span class="sxs-lookup"><span data-stu-id="01757-204">The **Include** clause improves the performance of this query but is not essential for the query to work.</span></span> <span data-ttu-id="01757-205">不使用 Include 子句中，擷取的資料會使用消極式載入，這牽涉到將傳送到資料庫的個別查詢每次擷取資料的相關。</span><span class="sxs-lookup"><span data-stu-id="01757-205">Without the Include clause, the data would be retrieved using lazy loading, which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="01757-206">藉由提供的 Include 子句，會使用積極式載入，這表示透過單一查詢的資料庫擷取的所有相關資料的擷取資料。</span><span class="sxs-lookup"><span data-stu-id="01757-206">By providing the Include clause, the data is retrieved using eager loading, which means all of the related data is retrieved through a single query of the database.</span></span> <span data-ttu-id="01757-207">在大部分的相關資料則不會的情況下，積極式載入可能會比較沒有效率，因為擷取詳細資料。</span><span class="sxs-lookup"><span data-stu-id="01757-207">In cases where most of the related data is not be used, eager loading can be less efficient because more data is retrieved.</span></span> <span data-ttu-id="01757-208">不過，在此情況下，積極式載入可提供最佳效能因為相關的資料會顯示每一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="01757-208">However, in this case, eager loading provides the best performance because the related data is displayed for each record.</span></span>

<span data-ttu-id="01757-209">如需載入時的效能考量的詳細資訊的相關資料，請參閱節 **「 延遲 」、 「 Eager 和 「 明確載入相關資料的**中[與 Entity Framework 在 ASP 中的讀取相關資料.NET MVC 應用程式](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)主題。</span><span class="sxs-lookup"><span data-stu-id="01757-209">For more information about performance consideration when loading related data, see the section titled **Lazy, Eager, and Explicit Loading of Related Data** in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) topic.</span></span>

<span data-ttu-id="01757-210">根據預設，資料會依照標示為索引鍵屬性的值而定。</span><span class="sxs-lookup"><span data-stu-id="01757-210">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="01757-211">您可以新增的 OrderBy 子句，以指定不同的值進行排序。</span><span class="sxs-lookup"><span data-stu-id="01757-211">You can add an OrderBy clause to specify a different value for sorting.</span></span> <span data-ttu-id="01757-212">在此範例中，預設 StudentID 屬性用於排序。</span><span class="sxs-lookup"><span data-stu-id="01757-212">In this example, the default StudentID property is used for sorting.</span></span> <span data-ttu-id="01757-213">在 [排序、 分頁和篩選資料](sorting-paging-and-filtering-data.md)主題，可讓使用者選取排序的資料行。</span><span class="sxs-lookup"><span data-stu-id="01757-213">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) topic, you will enable the user to select a column for sorting.</span></span>

<span data-ttu-id="01757-214">執行您的 web 應用程式，並巡覽至 Students 頁面。</span><span class="sxs-lookup"><span data-stu-id="01757-214">Run your web application, and navigate to the Students page.</span></span> <span data-ttu-id="01757-215">學生頁面會顯示下列的學生資訊。</span><span class="sxs-lookup"><span data-stu-id="01757-215">The Students page displays the following student information.</span></span>

![顯示資料](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="01757-217">自動產生的模型繫結方法</span><span class="sxs-lookup"><span data-stu-id="01757-217">Automatic generation of model binding methods</span></span>

<span data-ttu-id="01757-218">在逐步進行本教學課程系列，只要到您的專案複製程式碼從本教學課程。</span><span class="sxs-lookup"><span data-stu-id="01757-218">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="01757-219">不過，這種方法的缺點是功能的，您可能不會察覺到自動產生的模型繫結方法的程式碼的 Visual Studio 所提供。</span><span class="sxs-lookup"><span data-stu-id="01757-219">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="01757-220">您自己的專案上工作時，自動產生程式碼可以節省時間，並且有助於您深入了解如何實作的作業。</span><span class="sxs-lookup"><span data-stu-id="01757-220">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="01757-221">本章節描述的自動程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="01757-221">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="01757-222">本章節僅供參考，並不包含任何您需要在您的專案中實作的程式碼。</span><span class="sxs-lookup"><span data-stu-id="01757-222">This section is only informational and does not contain any code you need to implement in your project.</span></span>

<span data-ttu-id="01757-223">設定的值時**SelectMethod**， **UpdateMethod**， **InsertMethod**，或**DeleteMethod**標記程式碼中的屬性您可以選取**建立新的方法**選項。</span><span class="sxs-lookup"><span data-stu-id="01757-223">When setting a value for the **SelectMethod**, **UpdateMethod**, **InsertMethod**, or **DeleteMethod** properties in the markup code, you can select the **Create New Method** option.</span></span>

![建立新的方法](retrieving-data/_static/image18.png)

<span data-ttu-id="01757-225">Visual Studio 不僅會建立具有適當簽章中，程式碼後置中的方法，也會產生可協助您執行作業的實作程式碼。</span><span class="sxs-lookup"><span data-stu-id="01757-225">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to assist you with performing the operation.</span></span> <span data-ttu-id="01757-226">如果您先設定**ItemType**屬性，才能使用自動程式碼產生功能，產生的程式碼會使用該類型的作業。</span><span class="sxs-lookup"><span data-stu-id="01757-226">If you first set the **ItemType** property before using the automatic code generation feature, the generated code will use that type for the operations.</span></span> <span data-ttu-id="01757-227">例如，設定 UpdateMethod 屬性時，下列程式碼就會自動產生：</span><span class="sxs-lookup"><span data-stu-id="01757-227">For example, when setting the UpdateMethod property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="01757-228">同樣地，上述程式碼不需要加入至專案。</span><span class="sxs-lookup"><span data-stu-id="01757-228">Again, the above code does not need to be added to your project.</span></span> <span data-ttu-id="01757-229">在下一個教學課程中，您將實作更新、 刪除和加入新資料的方法。</span><span class="sxs-lookup"><span data-stu-id="01757-229">In the next tutorial, you will implement methods for updating, deleting and adding new data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="01757-230">結論</span><span class="sxs-lookup"><span data-stu-id="01757-230">Conclusion</span></span>

<span data-ttu-id="01757-231">在本教學課程中，您可以建立資料模型類別，並從那些類別產生資料庫。</span><span class="sxs-lookup"><span data-stu-id="01757-231">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="01757-232">您會將資料庫資料表中填入測試資料。</span><span class="sxs-lookup"><span data-stu-id="01757-232">You filled the database tables with test data.</span></span> <span data-ttu-id="01757-233">您用來從資料庫擷取資料的模型繫結，然後 GridView 中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="01757-233">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="01757-234">在接下來[教學課程](updating-deleting-and-creating-data.md)在本系列中，您將啟用更新、 刪除和建立資料。</span><span class="sxs-lookup"><span data-stu-id="01757-234">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you will enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="01757-235">下一步</span><span class="sxs-lookup"><span data-stu-id="01757-235">Next</span></span>](updating-deleting-and-creating-data.md)
