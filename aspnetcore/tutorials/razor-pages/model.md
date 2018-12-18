---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 91fee1db820493be671fecaee3cfb4c1b7df8bd3
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121359"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="d21ea-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="d21ea-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="d21ea-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d21ea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="d21ea-105">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="d21ea-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="d21ea-106">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="d21ea-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="d21ea-107">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="d21ea-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="d21ea-108">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="d21ea-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="d21ea-109">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="d21ea-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="d21ea-110">[檢視或下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/)範例。</span><span class="sxs-lookup"><span data-stu-id="d21ea-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="d21ea-111">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="d21ea-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d21ea-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d21ea-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d21ea-113">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="d21ea-114">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="d21ea-114">Name the folder *Models*.</span></span>

<span data-ttu-id="d21ea-115">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d21ea-115">Right click the *Models* folder.</span></span> <span data-ttu-id="d21ea-116">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="d21ea-117">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="d21ea-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d21ea-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d21ea-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d21ea-119">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d21ea-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="d21ea-120">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d21ea-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d21ea-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d21ea-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d21ea-122">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="d21ea-123">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="d21ea-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="d21ea-124">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="d21ea-125">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="d21ea-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="d21ea-126">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="d21ea-127">在中央窗格中選取 [空類別]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-127">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="d21ea-128">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="d21ea-129">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="d21ea-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="d21ea-130">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="d21ea-130">Scaffold the movie model</span></span>

<span data-ttu-id="d21ea-131">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="d21ea-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="d21ea-132">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="d21ea-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d21ea-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d21ea-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d21ea-134">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="d21ea-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="d21ea-135">以滑鼠右鍵按一下 *Pages* 資料夾 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="d21ea-136">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="d21ea-136">Name the folder *Movies*</span></span>

<span data-ttu-id="d21ea-137">以滑鼠右鍵按一下 *Pages/Movies* 資料夾 > [新增] > [新增 Scaffolded 項目]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="d21ea-139">在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="d21ea-141">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="d21ea-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="d21ea-142">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="d21ea-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="d21ea-143">在 [資料內容類別] 列中選取 [+] (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="d21ea-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="d21ea-144">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-144">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="d21ea-146">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="d21ea-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d21ea-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d21ea-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="d21ea-148">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="d21ea-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d21ea-149">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="d21ea-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="d21ea-150">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d21ea-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="d21ea-151">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d21ea-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d21ea-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d21ea-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d21ea-153">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="d21ea-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d21ea-154">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d21ea-154">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="d21ea-155">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="d21ea-155">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="d21ea-156">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="d21ea-156">Files created</span></span>

* <span data-ttu-id="d21ea-157">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="d21ea-157">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="d21ea-158">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="d21ea-158">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="d21ea-159">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="d21ea-159">File updated</span></span>

* <span data-ttu-id="d21ea-160">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="d21ea-160">*Startup.cs*</span></span>

<span data-ttu-id="d21ea-161">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="d21ea-161">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="d21ea-162">初始移轉</span><span class="sxs-lookup"><span data-stu-id="d21ea-162">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d21ea-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d21ea-163">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="d21ea-164">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="d21ea-164">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="d21ea-165">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="d21ea-165">Add an initial migration.</span></span>
* <span data-ttu-id="d21ea-166">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="d21ea-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="d21ea-167">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="d21ea-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="d21ea-169">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="d21ea-169">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d21ea-170">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d21ea-170">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d21ea-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d21ea-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="d21ea-172">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="d21ea-172">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d21ea-173">結構描述是以 `DbContext` (位在 *Models/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="d21ea-173">The schema is based on the model specified in the `DbContext` (In the *Models/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="d21ea-174">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="d21ea-174">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="d21ea-175">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="d21ea-175">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="d21ea-176">`ef database update` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="d21ea-176">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="d21ea-177">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="d21ea-177">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d21ea-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d21ea-178">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="d21ea-179">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="d21ea-179">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="d21ea-180">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d21ea-180">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d21ea-181">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="d21ea-181">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d21ea-182">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="d21ea-182">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d21ea-183">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="d21ea-183">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d21ea-184">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="d21ea-184">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="d21ea-185">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="d21ea-185">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d21ea-186">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="d21ea-186">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="d21ea-187">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="d21ea-187">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="d21ea-188">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="d21ea-188">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="d21ea-189">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="d21ea-189">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="d21ea-190">上述程式碼會建立實體集的 [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="d21ea-190">The preceding code creates a [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="d21ea-191">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="d21ea-191">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="d21ea-192">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="d21ea-192">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d21ea-193">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="d21ea-193">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="d21ea-194">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="d21ea-194">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d21ea-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d21ea-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d21ea-196">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d21ea-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="d21ea-197">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="d21ea-197">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d21ea-198">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="d21ea-198">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="d21ea-199">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="d21ea-199">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="d21ea-200">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="d21ea-200">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="d21ea-201">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="d21ea-201">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="d21ea-202">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="d21ea-202">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="d21ea-203">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="d21ea-203">Test the app</span></span>

* <span data-ttu-id="d21ea-204">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="d21ea-204">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="d21ea-205">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="d21ea-205">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="d21ea-206">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="d21ea-206">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="d21ea-207">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="d21ea-207">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="d21ea-209">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="d21ea-209">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="d21ea-210">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="d21ea-210">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="d21ea-211">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="d21ea-211">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="d21ea-212">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="d21ea-212">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="d21ea-213">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="d21ea-213">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d21ea-214">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="d21ea-214">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
