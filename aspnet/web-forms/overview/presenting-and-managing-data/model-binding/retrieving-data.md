---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 擷取和顯示資料與模型繫結和 web form |Microsoft Docs
author: Rick-Anderson
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396281"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="768f0-104">擷取和顯示資料與模型繫結和 web form</span><span class="sxs-lookup"><span data-stu-id="768f0-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="768f0-105">本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="768f0-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="768f0-106">模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="768f0-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="768f0-107">此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。</span><span class="sxs-lookup"><span data-stu-id="768f0-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="768f0-108">模型繫結模式適用於任何資料存取技術。</span><span class="sxs-lookup"><span data-stu-id="768f0-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="768f0-109">在本教學課程中，您將使用 Entity Framework，但您可以使用最熟悉的資料存取技術。</span><span class="sxs-lookup"><span data-stu-id="768f0-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="768f0-110">從資料繫結的伺服器控制項，例如 GridView、 ListView、 DetailsView 或 FormView 控制項，您可以指定要用於選取、 更新、 刪除和建立資料方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="768f0-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="768f0-111">在本教學課程中，您會指定 SelectMethod 值。</span><span class="sxs-lookup"><span data-stu-id="768f0-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="768f0-112">在該方法中，您提供的邏輯擷取資料。</span><span class="sxs-lookup"><span data-stu-id="768f0-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="768f0-113">在下一個教學課程中，您將 UpdateMethod、 DeleteMethod 和 InsertMethod 設定值。</span><span class="sxs-lookup"><span data-stu-id="768f0-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="768f0-114">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，在C#或 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="768f0-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="768f0-115">可下載的程式碼適用於使用 Visual Studio 2012 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="768f0-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="768f0-116">它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2017 範本稍有不同。</span><span class="sxs-lookup"><span data-stu-id="768f0-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="768f0-117">在本教學課程中，您可以執行應用程式在 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="768f0-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="768f0-118">您也可以部署到裝載提供者應用程式，並使其可透過網際網路使用。</span><span class="sxs-lookup"><span data-stu-id="768f0-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="768f0-119">Microsoft 提供免費的 web 裝載中的最多 10 個 web sites</span><span class="sxs-lookup"><span data-stu-id="768f0-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="768f0-120">[免費 Azure 試用帳戶](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="768f0-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="768f0-121">如需有關如何將 Visual Studio web 專案部署至 Azure App Service Web Apps 的資訊，請參閱[使用 Visual Studio 的 ASP.NET Web 部署](../../deployment/visual-studio-web-deployment/introduction.md)系列。</span><span class="sxs-lookup"><span data-stu-id="768f0-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="768f0-122">該教學課程也會示範如何使用 Entity Framework Code First Migrations，將 SQL Server 資料庫部署到 Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="768f0-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="768f0-123">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="768f0-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="768f0-124">Microsoft Visual Studio 2017 或 Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="768f0-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="768f0-125">本教學課程也適用於 Visual Studio 2012 和 Visual Studio 2013，但有一些差異，在使用者介面和專案範本。</span><span class="sxs-lookup"><span data-stu-id="768f0-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="768f0-126">您將建置</span><span class="sxs-lookup"><span data-stu-id="768f0-126">What you'll build</span></span>

<span data-ttu-id="768f0-127">在本教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="768f0-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="768f0-128">建立與註冊課程的學生反映一所大學的資料物件</span><span class="sxs-lookup"><span data-stu-id="768f0-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="768f0-129">建置從物件的資料庫資料表</span><span class="sxs-lookup"><span data-stu-id="768f0-129">Build database tables from the objects</span></span>
* <span data-ttu-id="768f0-130">填入測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="768f0-130">Populate the database with test data</span></span>
* <span data-ttu-id="768f0-131">在 web 表單中顯示資料</span><span class="sxs-lookup"><span data-stu-id="768f0-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="768f0-132">建立專案</span><span class="sxs-lookup"><span data-stu-id="768f0-132">Create the project</span></span>

1. <span data-ttu-id="768f0-133">在 Visual Studio 2017 中，建立**ASP.NET Web 應用程式 (.NET Framework)** 專案，稱為**ContosoUniversityModelBinding**。</span><span class="sxs-lookup"><span data-stu-id="768f0-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![建立專案](retrieving-data/_static/image19.png)

2. <span data-ttu-id="768f0-135">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="768f0-135">Select **OK**.</span></span> <span data-ttu-id="768f0-136">若要選取範本的對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="768f0-136">The dialog box to select a template appears.</span></span>

   ![選取 web form](retrieving-data/_static/image3.png)

3. <span data-ttu-id="768f0-138">選取  **Web Form**範本。</span><span class="sxs-lookup"><span data-stu-id="768f0-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="768f0-139">如有必要，驗證變更為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="768f0-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="768f0-140">選取 [確定] 建立專案。</span><span class="sxs-lookup"><span data-stu-id="768f0-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="768f0-141">修改網站的外觀</span><span class="sxs-lookup"><span data-stu-id="768f0-141">Modify site appearance</span></span>

   <span data-ttu-id="768f0-142">變更幾個自訂網站的外觀。</span><span class="sxs-lookup"><span data-stu-id="768f0-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="768f0-143">開啟 Site.Master 檔。</span><span class="sxs-lookup"><span data-stu-id="768f0-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="768f0-144">變更要顯示的標題**Contoso 大學**而非**My ASP.NET Application**。</span><span class="sxs-lookup"><span data-stu-id="768f0-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="768f0-145">變更中的標頭文字**應用程式名稱**要**Contoso 大學**。</span><span class="sxs-lookup"><span data-stu-id="768f0-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="768f0-146">變更導覽標頭連結至適當的站台。</span><span class="sxs-lookup"><span data-stu-id="768f0-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="768f0-147">移除的連結**關於**並**連絡人**，相反地，連結到**學生**頁面上，您將建立。</span><span class="sxs-lookup"><span data-stu-id="768f0-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="768f0-148">儲存 Site.Master。</span><span class="sxs-lookup"><span data-stu-id="768f0-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="768f0-149">新增 web 表單，以顯示學生資料</span><span class="sxs-lookup"><span data-stu-id="768f0-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="768f0-150">在 [**方案總管] 中**，以滑鼠右鍵按一下您的專案，然後選取**新增**，然後**新項目**。</span><span class="sxs-lookup"><span data-stu-id="768f0-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="768f0-151">在 **加入新項目**對話方塊中，選取**使用主版頁面的 Web Form**範本並將它命名**Students.aspx**。</span><span class="sxs-lookup"><span data-stu-id="768f0-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![建立頁面](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="768f0-153">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="768f0-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="768f0-154">Web form 主版頁面，選取**Site.Master**。</span><span class="sxs-lookup"><span data-stu-id="768f0-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="768f0-155">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="768f0-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="768f0-156">加入資料模型</span><span class="sxs-lookup"><span data-stu-id="768f0-156">Add the data model</span></span>

<span data-ttu-id="768f0-157">在 **模型**資料夾中，加入名為類別**UniversityModels.cs**。</span><span class="sxs-lookup"><span data-stu-id="768f0-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="768f0-158">以滑鼠右鍵按一下**模型**，選取**新增**，然後**新項目**。</span><span class="sxs-lookup"><span data-stu-id="768f0-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="768f0-159">[新增項目] 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="768f0-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="768f0-160">從左側的導覽功能表中，選取**程式碼**，然後**類別**。</span><span class="sxs-lookup"><span data-stu-id="768f0-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![建立模型類別](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="768f0-162">將類別命名為**UniversityModels.cs** ，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="768f0-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="768f0-163">這個檔案中定義`SchoolContext`， `Student`， `Enrollment`，和`Course`類別，如下所示：</span><span class="sxs-lookup"><span data-stu-id="768f0-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="768f0-164">`SchoolContext`類別衍生自`DbContext`，其管理資料庫連接，並變更資料中。</span><span class="sxs-lookup"><span data-stu-id="768f0-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="768f0-165">在 `Student`類別，您會發現要套用的屬性`FirstName`， `LastName`，和`Year`屬性。</span><span class="sxs-lookup"><span data-stu-id="768f0-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="768f0-166">本教學課程會使用這些屬性進行資料驗證。</span><span class="sxs-lookup"><span data-stu-id="768f0-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="768f0-167">若要簡化程式碼，只有這些屬性會標示具有資料驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="768f0-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="768f0-168">在實際的專案中，您會將驗證屬性套用至需要驗證的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="768f0-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="768f0-169">儲存 UniversityModels.cs。</span><span class="sxs-lookup"><span data-stu-id="768f0-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="768f0-170">將類別為基礎的資料庫設定</span><span class="sxs-lookup"><span data-stu-id="768f0-170">Set up the database based on classes</span></span>

<span data-ttu-id="768f0-171">本教學課程會使用[Code First 移轉](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/)建立物件和資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="768f0-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="768f0-172">這些資料表會儲存資訊的學生和他們的課程。</span><span class="sxs-lookup"><span data-stu-id="768f0-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="768f0-173">選取 **工具** > **NuGet 套件管理員** > **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="768f0-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="768f0-174">在  **Package Manager Console**，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="768f0-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="768f0-175">如果命令成功完成時，會出現一則訊息指出已啟用移轉。</span><span class="sxs-lookup"><span data-stu-id="768f0-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![啟用移轉](retrieving-data/_static/image8.png)

      <span data-ttu-id="768f0-177">請注意，名為的檔案*Configuration.cs*已建立。</span><span class="sxs-lookup"><span data-stu-id="768f0-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="768f0-178">`Configuration`類別具有`Seed`方法，它可以預先填入測試資料的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="768f0-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="768f0-179">在資料庫中預先填入</span><span class="sxs-lookup"><span data-stu-id="768f0-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="768f0-180">開啟 Configuration.cs。</span><span class="sxs-lookup"><span data-stu-id="768f0-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="768f0-181">將下列程式碼加入至 `Seed` 方法。</span><span class="sxs-lookup"><span data-stu-id="768f0-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="768f0-182">此外，新增`using`陳述式`ContosoUniversityModelBinding. Models`命名空間。</span><span class="sxs-lookup"><span data-stu-id="768f0-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="768f0-183">儲存 Configuration.cs。</span><span class="sxs-lookup"><span data-stu-id="768f0-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="768f0-184">在 [套件管理員] 主控台中，執行命令**新增移轉初始**。</span><span class="sxs-lookup"><span data-stu-id="768f0-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="768f0-185">執行命令**更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="768f0-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="768f0-186">如果執行此命令中時，您會收到例外狀況`StudentID`並`CourseID`值可能不同於`Seed`方法值。</span><span class="sxs-lookup"><span data-stu-id="768f0-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="768f0-187">開啟這些資料庫資料表，並尋找現有的值，如`StudentID`和`CourseID`。</span><span class="sxs-lookup"><span data-stu-id="768f0-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="768f0-188">將這些值加入的程式碼植入`Enrollments`資料表。</span><span class="sxs-lookup"><span data-stu-id="768f0-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="768f0-189">新增 GridView 控制項</span><span class="sxs-lookup"><span data-stu-id="768f0-189">Add a GridView control</span></span>

<span data-ttu-id="768f0-190">填入的資料庫的資料，您現在準備好擷取該資料並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="768f0-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="768f0-191">開啟 Students.aspx。</span><span class="sxs-lookup"><span data-stu-id="768f0-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="768f0-192">找出`MainContent`預留位置。</span><span class="sxs-lookup"><span data-stu-id="768f0-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="768f0-193">在該預留位置中，新增**GridView**包括此程式碼的控制項。</span><span class="sxs-lookup"><span data-stu-id="768f0-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="768f0-194">要注意的事項：</span><span class="sxs-lookup"><span data-stu-id="768f0-194">Things to note:</span></span>
   * <span data-ttu-id="768f0-195">請注意，設定的值`SelectMethod`GridView 項目中的屬性。</span><span class="sxs-lookup"><span data-stu-id="768f0-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="768f0-196">這個值會指定用來擷取您在下一個步驟中建立的 GridView 資料的方法。</span><span class="sxs-lookup"><span data-stu-id="768f0-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="768f0-197">`ItemType`屬性設定為`Student`稍早建立的類別。</span><span class="sxs-lookup"><span data-stu-id="768f0-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="768f0-198">此設定可讓您參考的標記中的類別屬性。</span><span class="sxs-lookup"><span data-stu-id="768f0-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="768f0-199">例如，`Student`類別具有名為`Enrollments`。</span><span class="sxs-lookup"><span data-stu-id="768f0-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="768f0-200">您可以使用`Item.Enrollments`來擷取該集合，然後使用[LINQ 語法](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq)擷取每個學生的註冊點數總和。</span><span class="sxs-lookup"><span data-stu-id="768f0-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="768f0-201">儲存 Students.aspx。</span><span class="sxs-lookup"><span data-stu-id="768f0-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="768f0-202">加入程式碼來擷取資料</span><span class="sxs-lookup"><span data-stu-id="768f0-202">Add code to retrieve data</span></span>

   <span data-ttu-id="768f0-203">在 Students.aspx 程式碼後置檔案中，新增為指定的方法`SelectMethod`值。</span><span class="sxs-lookup"><span data-stu-id="768f0-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="768f0-204">開啟 Students.aspx.cs。</span><span class="sxs-lookup"><span data-stu-id="768f0-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="768f0-205">新增`using`陳述式`ContosoUniversityModelBinding. Models`和`System.Data.Entity`命名空間。</span><span class="sxs-lookup"><span data-stu-id="768f0-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="768f0-206">新增您指定的方法`SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="768f0-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="768f0-207">`Include`子句可改善查詢效能，但並非必要。</span><span class="sxs-lookup"><span data-stu-id="768f0-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="768f0-208">不含`Include`子句，資料會使用擷取[*消極式載入*](https://en.wikipedia.org/wiki/Lazy_loading)，其中包含每次傳送至資料庫的個別查詢相關的擷取資料。</span><span class="sxs-lookup"><span data-stu-id="768f0-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="768f0-209">具有`Include`子句，資料會使用擷取*積極式載入*，這表示單一資料庫查詢會擷取所有相關的資料。</span><span class="sxs-lookup"><span data-stu-id="768f0-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="768f0-210">如果不使用相關的資料，積極式載入是比較沒有效率因為擷取詳細資料。</span><span class="sxs-lookup"><span data-stu-id="768f0-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="768f0-211">不過，在此情況下，積極式載入可讓您獲得最佳效能因為相關的資料會顯示每一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="768f0-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="768f0-212">如需載入時的效能考量的詳細資訊的相關資料，請參閱 < **「 延遲 」、 「 Eager 和 「 明確載入相關資料的**一節中[讀取相關資料，與 ASP.NET 中的 Entity FrameworkMVC 應用程式](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)文章。</span><span class="sxs-lookup"><span data-stu-id="768f0-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="768f0-213">根據預設，資料會依照標示為索引鍵屬性的值而定。</span><span class="sxs-lookup"><span data-stu-id="768f0-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="768f0-214">您可以新增`OrderBy`子句來指定不同的排序值。</span><span class="sxs-lookup"><span data-stu-id="768f0-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="768f0-215">在此範例中，預設值`StudentID`屬性用來排序。</span><span class="sxs-lookup"><span data-stu-id="768f0-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="768f0-216">在 [排序、 分頁和篩選資料](sorting-paging-and-filtering-data.md)文章中，使用者可選取的資料行排序。</span><span class="sxs-lookup"><span data-stu-id="768f0-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="768f0-217">儲存 Students.aspx.cs。</span><span class="sxs-lookup"><span data-stu-id="768f0-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="768f0-218">執行您的應用程式</span><span class="sxs-lookup"><span data-stu-id="768f0-218">Run your application</span></span> 

<span data-ttu-id="768f0-219">執行您的 web 應用程式 (**F5**) 並瀏覽至**學生**網頁，當中會顯示下列：</span><span class="sxs-lookup"><span data-stu-id="768f0-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![顯示資料](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="768f0-221">自動產生的模型繫結方法</span><span class="sxs-lookup"><span data-stu-id="768f0-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="768f0-222">在逐步進行本教學課程系列，只要到您的專案複製程式碼從本教學課程。</span><span class="sxs-lookup"><span data-stu-id="768f0-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="768f0-223">不過，這種方法的缺點是功能的，您可能不會察覺到自動產生的模型繫結方法的程式碼的 Visual Studio 所提供。</span><span class="sxs-lookup"><span data-stu-id="768f0-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="768f0-224">您自己的專案上工作時，自動產生程式碼可以節省時間，並且有助於您深入了解如何實作的作業。</span><span class="sxs-lookup"><span data-stu-id="768f0-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="768f0-225">本章節描述的自動程式碼產生功能。</span><span class="sxs-lookup"><span data-stu-id="768f0-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="768f0-226">本章節僅供參考，並不包含任何您需要在您的專案中實作的程式碼。</span><span class="sxs-lookup"><span data-stu-id="768f0-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="768f0-227">設定的值時`SelectMethod`， `UpdateMethod`， `InsertMethod`，或`DeleteMethod`屬性的標記程式碼中，您可以選取**建立的新方法**選項。</span><span class="sxs-lookup"><span data-stu-id="768f0-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![建立方法](retrieving-data/_static/image18.png)

<span data-ttu-id="768f0-229">Visual Studio 不僅會建立具有適當簽章中，程式碼後置中的方法，也會產生執行作業的實作程式碼。</span><span class="sxs-lookup"><span data-stu-id="768f0-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="768f0-230">如果您先設定`ItemType`屬性，才能使用自動程式碼產生功能，為作業輸入產生的程式碼使用。</span><span class="sxs-lookup"><span data-stu-id="768f0-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="768f0-231">例如，當設定`UpdateMethod`屬性，下列程式碼會自動產生：</span><span class="sxs-lookup"><span data-stu-id="768f0-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="768f0-232">同樣地，此程式碼不需要加入至專案。</span><span class="sxs-lookup"><span data-stu-id="768f0-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="768f0-233">在下一個教學課程中，您將實作更新、 刪除和加入新資料的方法。</span><span class="sxs-lookup"><span data-stu-id="768f0-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="768f0-234">總結</span><span class="sxs-lookup"><span data-stu-id="768f0-234">Summary</span></span>

<span data-ttu-id="768f0-235">在本教學課程中，您可以建立資料模型類別，並從那些類別產生資料庫。</span><span class="sxs-lookup"><span data-stu-id="768f0-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="768f0-236">您會將資料庫資料表中填入測試資料。</span><span class="sxs-lookup"><span data-stu-id="768f0-236">You filled the database tables with test data.</span></span> <span data-ttu-id="768f0-237">您用來從資料庫擷取資料的模型繫結，然後 GridView 中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="768f0-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="768f0-238">在接下來[教學課程](updating-deleting-and-creating-data.md)在本系列中，您將會讓更新、 刪除和建立資料。</span><span class="sxs-lookup"><span data-stu-id="768f0-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="768f0-239">下一步</span><span class="sxs-lookup"><span data-stu-id="768f0-239">Next</span></span>](updating-deleting-and-creating-data.md)
