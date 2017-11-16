---
title: "將模型加新增至 ASP.NET Core MVC 應用程式"
author: rick-anderson
description: "請將模型新增至簡單的 ASP.NET Core 應用程式。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="b7dea-104">注意：ASP.NET Core 2.0 範本包含 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b7dea-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="b7dea-105">在方案總管中，以滑鼠右鍵按一下 **MvcMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="b7dea-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b7dea-106">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="b7dea-106">Name the folder *Models*.</span></span>

<span data-ttu-id="b7dea-107">以滑鼠右鍵按一下 *Models* 資料夾 > [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="b7dea-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="b7dea-108">將類別命名為 **Movie** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="b7dea-108">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="b7dea-109">`ID` 欄位是資料庫對於主索引鍵的必要欄位。</span><span class="sxs-lookup"><span data-stu-id="b7dea-109">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="b7dea-110">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="b7dea-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="b7dea-111">您目前即會在 **MVC** 應用程式中具有一個**模型**。</span><span class="sxs-lookup"><span data-stu-id="b7dea-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="b7dea-112">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="b7dea-112">Scaffolding a controller</span></span>

<span data-ttu-id="b7dea-113">在**方案總管**中，以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [控制器]。</span><span class="sxs-lookup"><span data-stu-id="b7dea-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![上方步驟的檢視](adding-model/_static/add_controller.png)

<span data-ttu-id="b7dea-115">在 [新增 MVC 相依性] 對話方塊中，選取 [基本相依性]，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="b7dea-115">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![上方步驟的檢視](adding-model/_static/add_depend.png)

<span data-ttu-id="b7dea-117">Visual Studio 會新增 Scaffold 控制器所需的相依性，但不會建立控制器本身。</span><span class="sxs-lookup"><span data-stu-id="b7dea-117">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="b7dea-118">接下來叫用 > [新增] > [控制器]可建立控制器。</span><span class="sxs-lookup"><span data-stu-id="b7dea-118">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="b7dea-119">在**方案總管**中，以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [控制器]。</span><span class="sxs-lookup"><span data-stu-id="b7dea-119">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![上方步驟的檢視](adding-model/_static/add_controller.png)

<span data-ttu-id="b7dea-121">在 [新增 Scaffold] 對話方塊中，點選 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="b7dea-121">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="b7dea-123">完成 [新增控制器] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="b7dea-123">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="b7dea-124">**模型類別：***Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="b7dea-124">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="b7dea-125">**資料內容類別：**選取 **+** 圖示並新增預設的 **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="b7dea-125">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![新增資料內容](adding-model/_static/dc.png)

* <span data-ttu-id="b7dea-127">**檢視：**保持核取預設的每一個選項</span><span class="sxs-lookup"><span data-stu-id="b7dea-127">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="b7dea-128">**控制器名稱：**保留預設值 *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="b7dea-128">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="b7dea-129">點選 [新增]</span><span class="sxs-lookup"><span data-stu-id="b7dea-129">Tap **Add**</span></span>

![[新增控制器] 對話方塊](adding-model/_static/add_controller2.png)

<span data-ttu-id="b7dea-131">Visual Studio 會建立：</span><span class="sxs-lookup"><span data-stu-id="b7dea-131">Visual Studio creates:</span></span>

* <span data-ttu-id="b7dea-132">Entity Framework Core [資料庫內容類別](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="b7dea-132">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="b7dea-133">電影控制器 (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="b7dea-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="b7dea-134">Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (*Views/Movies/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="b7dea-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="b7dea-135">自動建立資料庫內容與 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。</span><span class="sxs-lookup"><span data-stu-id="b7dea-135">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="b7dea-136">您很快就會擁有一個正常運作的 Web 應用程式，可讓您管理電影資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7dea-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="b7dea-137">如果執行應用程式，並按一下 **Mvc Movie** 連結，您將收到如下錯誤：</span><span class="sxs-lookup"><span data-stu-id="b7dea-137">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="b7dea-138">您需要建立資料庫，而且您將使用 EF Core [移轉](xref:data/ef-mvc/migrations)功能來執行此作業。</span><span class="sxs-lookup"><span data-stu-id="b7dea-138">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="b7dea-139">移轉可讓您建立符合資料模型的資料庫，並在資料模型變更時，更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="b7dea-139">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="b7dea-140">新增 EF 工具並執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="b7dea-140">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="b7dea-141">在本節中，您將使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="b7dea-141">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b7dea-142">新增 Entity Framework Core Tools 套件。</span><span class="sxs-lookup"><span data-stu-id="b7dea-142">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="b7dea-143">必須有此套件，才能新增移轉並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7dea-143">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="b7dea-144">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="b7dea-144">Add an initial migration.</span></span>
* <span data-ttu-id="b7dea-145">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7dea-145">Update the database with the initial migration.</span></span>

<span data-ttu-id="b7dea-146">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="b7dea-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC 功能表](adding-model/_static/pmc.png)

<span data-ttu-id="b7dea-148">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7dea-148">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b7dea-149">**注意：**如果使用 `Install-Package` 命令但收到錯誤，請開啟 NuGet 套件管理員，並搜尋 `Microsoft.EntityFrameworkCore.Tools` 套件。</span><span class="sxs-lookup"><span data-stu-id="b7dea-149">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="b7dea-150">這可讓您安裝該套件，或檢查其是否已安裝。</span><span class="sxs-lookup"><span data-stu-id="b7dea-150">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="b7dea-151">此外，若您有 PMC 的問題，可以參閱 [CLI 方法](#cli)。</span><span class="sxs-lookup"><span data-stu-id="b7dea-151">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="b7dea-152">`Add-Migration` 命令會建立程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="b7dea-152">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="b7dea-153">結構描述是以 `DbContext` (位在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="b7dea-153">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="b7dea-154">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="b7dea-154">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b7dea-155">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="b7dea-155">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b7dea-156">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="b7dea-156">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b7dea-157">`Update-Database` 命令會執行 *Migrations/*\<時間戳記>_InitialCreate.cs 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="b7dea-157">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="b7dea-158"><a name="cli"></a> 您可以使用命令列介面 (CLI) 而不是 PMC，來執行先前步驟：</span><span class="sxs-lookup"><span data-stu-id="b7dea-158"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="b7dea-159">將 [EF Core 工具](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)新增至 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b7dea-159">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="b7dea-160">從主控台 (在專案目錄中) 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7dea-160">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![列出識別碼、價格、發行日期及標題等可用屬性的模型項目上 IntelliSense 的操作功能表](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="b7dea-162">其他資源</span><span class="sxs-lookup"><span data-stu-id="b7dea-162">Additional resources</span></span>

* [<span data-ttu-id="b7dea-163">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="b7dea-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="b7dea-164">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="b7dea-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="b7dea-165">[上一步：新增檢視](adding-view.md)
[下一步：使用 SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="b7dea-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
