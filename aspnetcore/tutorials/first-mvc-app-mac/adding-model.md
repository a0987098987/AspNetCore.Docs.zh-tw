---
title: "將模型新增至 ASP.NET MVC Core 應用程式"
author: rick-anderson
description: "請將模型新增至簡單的 ASP.NET Core 應用程式。"
keywords: ASP.NET Core, MVC, Scaffold, Scaffolding
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 4a158802a19011cbb45da1b3ca43d67706efe4cd
ms.sourcegitcommit: d9e2c99c837078fcac0e315025f8fbfbd45ea6e8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="12f01-104">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="12f01-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="12f01-105">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="12f01-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="12f01-106">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="12f01-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="12f01-107">在中央窗格中選取 [空類別]。</span><span class="sxs-lookup"><span data-stu-id="12f01-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="12f01-108">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="12f01-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="12f01-109">將下列屬性新增至 `Movie` 類別：</span><span class="sxs-lookup"><span data-stu-id="12f01-109">Add the following properties to the `Movie` class:</span></span>

<span data-ttu-id="12f01-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="12f01-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span></span>

<span data-ttu-id="12f01-111">`ID` 欄位是資料庫對於主索引鍵的必要欄位。</span><span class="sxs-lookup"><span data-stu-id="12f01-111">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="12f01-112">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="12f01-112">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="12f01-113">您目前即會在 **MVC** 應用程式中具有一個**模型**。</span><span class="sxs-lookup"><span data-stu-id="12f01-113">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="12f01-114">準備專案進行 Scaffolding</span><span class="sxs-lookup"><span data-stu-id="12f01-114">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="12f01-115">以滑鼠右鍵按一下專案檔，然後選取 [工具] > [編輯檔案]。</span><span class="sxs-lookup"><span data-stu-id="12f01-115">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![上方步驟的檢視](adding-model/_static/1.png)

- <span data-ttu-id="12f01-117">將下列反白顯示的 NuGet 套件新增至 *MvcMovie.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="12f01-117">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  <span data-ttu-id="12f01-118">[!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span><span class="sxs-lookup"><span data-stu-id="12f01-118">[!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span></span>

- <span data-ttu-id="12f01-119">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="12f01-119">Save the file.</span></span>

- <span data-ttu-id="12f01-120">建立 *Models/MvcMovieContext.cs* 檔案，然後新增下列 `MvcMovieContext` 類別：[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="12f01-120">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="12f01-121">開啟 *Startup.cs* 檔案，然後新增兩個 using：[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="12f01-121">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="12f01-122">將資料庫內容新增至 *Startup.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="12f01-122">Add the database context to the *Startup.cs* file:</span></span>

   <span data-ttu-id="12f01-123">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="12f01-123">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span></span>

  <span data-ttu-id="12f01-124">這會告知 Entity Framework 哪些模型類別包含在資料模型中。</span><span class="sxs-lookup"><span data-stu-id="12f01-124">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="12f01-125">您是在定義電影物件的一個「實體集」，它會在資料庫中以「電影」資料表表示。</span><span class="sxs-lookup"><span data-stu-id="12f01-125">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="12f01-126">建置專案以確認沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="12f01-126">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="12f01-127">Scaffold MovieController</span><span class="sxs-lookup"><span data-stu-id="12f01-127">Scaffold the MovieController</span></span>

<span data-ttu-id="12f01-128">在專案資料夾中開啟終端機視窗，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="12f01-128">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="12f01-129">如果您收到錯誤 `No executable found matching command "dotnet-aspnet-codegenerator", verify`：</span><span class="sxs-lookup"><span data-stu-id="12f01-129">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="12f01-130">您位在專案目錄中。</span><span class="sxs-lookup"><span data-stu-id="12f01-130">You are in the project directory.</span></span> <span data-ttu-id="12f01-131">專案目錄中含有 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="12f01-131">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="12f01-132">Dotnet 版本是 1.1 版 (含) 以上版本。</span><span class="sxs-lookup"><span data-stu-id="12f01-132">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="12f01-133">執行 `dotnet` 以取得版本。</span><span class="sxs-lookup"><span data-stu-id="12f01-133">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="12f01-134">您已將 `<DotNetCliToolReference>` 項目新增至 [MvcMovie.csproj 檔案](#prepare-the-project-for-scaffolding)。</span><span class="sxs-lookup"><span data-stu-id="12f01-134">You have added the `<DotNetCliToolReference>` elment to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="12f01-135">Scaffolding 引擎會建立下列各項：</span><span class="sxs-lookup"><span data-stu-id="12f01-135">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="12f01-136">電影控制器 (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="12f01-136">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="12f01-137">Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="12f01-137">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="12f01-138">自動建立 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。</span><span class="sxs-lookup"><span data-stu-id="12f01-138">The automatic creation of [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="12f01-139">您很快就會擁有一個正常運作的 Web 應用程式，可讓您管理電影資料庫。</span><span class="sxs-lookup"><span data-stu-id="12f01-139">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="12f01-140">將檔案新增至 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12f01-140">Add the files to Visual Studio</span></span>

* <span data-ttu-id="12f01-141">將 *MovieController.cs* 檔案新增至 Visual Studio 專案中：</span><span class="sxs-lookup"><span data-stu-id="12f01-141">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="12f01-142">以滑鼠右鍵按一下 *Controllers* 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="12f01-142">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="12f01-143">選取 *MovieController.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="12f01-143">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="12f01-144">新增 *Movies* 資料夾和檢視：</span><span class="sxs-lookup"><span data-stu-id="12f01-144">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="12f01-145">以滑鼠右鍵按一下 *Views* 資料夾，然後選取 [新增] > [新增現有資料夾]。</span><span class="sxs-lookup"><span data-stu-id="12f01-145">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="12f01-146">巡覽至 *Views* 資料夾中，選取 *Views\Movies*，然後選取 [開啟]。</span><span class="sxs-lookup"><span data-stu-id="12f01-146">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="12f01-147">在 [Select files to add from Movies] (選取要從 Movies 新增的檔案) 對話方塊中，選取 [全部包含]，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="12f01-147">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="12f01-148">您現在擁有一個資料庫和多個頁面，可用來顯示、編輯、更新和刪除資料。</span><span class="sxs-lookup"><span data-stu-id="12f01-148">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="12f01-149">在下一個教學課程中，我們將會使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="12f01-149">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12f01-150">其他資源</span><span class="sxs-lookup"><span data-stu-id="12f01-150">Additional resources</span></span>

* [<span data-ttu-id="12f01-151">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="12f01-151">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="12f01-152">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="12f01-152">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="12f01-153">[上一步：新增檢視](adding-view.md)
[下一步：使用 SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="12f01-153">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
