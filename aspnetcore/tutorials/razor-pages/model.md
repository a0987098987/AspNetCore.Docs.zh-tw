---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 11/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f2c9c2fc8112ef8a1a5afdbe448de6319c43521d
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73761232"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="ffaaf-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="ffaaf-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="ffaaf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ffaaf-105">在本節中，會加入類別來管理跨平臺[SQLite 資料庫](https://www.sqlite.org/index.html)中的電影。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="ffaaf-106">從 ASP.NET Core 範本建立的應用程式會使用 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="ffaaf-107">應用程式的模型類別會與[Entity Framework Core （EF Core）](/ef/core) （[SQLite EF Core 資料庫提供者](/ef/core/providers/sqlite)）搭配使用，以使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="ffaaf-108">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="ffaaf-109">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="ffaaf-110">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ffaaf-111">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="ffaaf-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ffaaf-113">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="ffaaf-114">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-114">Name the folder *Models*.</span></span>

<span data-ttu-id="ffaaf-115">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-115">Right click the *Models* folder.</span></span> <span data-ttu-id="ffaaf-116">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="ffaaf-117">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ffaaf-119">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="ffaaf-120">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ffaaf-122">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="ffaaf-123">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="ffaaf-124">以滑鼠右鍵按一下 [Models] 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="ffaaf-125">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ffaaf-126">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="ffaaf-127">在中央窗格中選取 [類別是空的]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="ffaaf-128">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="ffaaf-129">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="ffaaf-130">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="ffaaf-130">Scaffold the movie model</span></span>

<span data-ttu-id="ffaaf-131">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="ffaaf-132">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ffaaf-134">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="ffaaf-135">以滑鼠右鍵按一下 [Pages] 資料夾 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="ffaaf-136">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-136">Name the folder *Movies*</span></span>

<span data-ttu-id="ffaaf-137">以滑鼠右鍵按一下 [Pages/Movies] 資料夾 > [新增] > [新增 Scaffolded 項目]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="ffaaf-139">在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="ffaaf-141">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="ffaaf-142">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="ffaaf-143">在 [資料內容類別] 列中選取 [+] \(加號\)，並將產生的名稱 RazorPagesMovie.**Models**.RazorPagesMovieContext 變更為 RazorPagesMovie.**Data**.RazorPagesMovieContext。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="ffaaf-144">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="ffaaf-145">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="ffaaf-146">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-146">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/3/arp.png)

<span data-ttu-id="ffaaf-148">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="ffaaf-150">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ffaaf-151">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="ffaaf-152">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="ffaaf-153">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-154">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ffaaf-155">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ffaaf-156">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="ffaaf-157">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="ffaaf-158">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="ffaaf-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ffaaf-160">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="ffaaf-161">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="ffaaf-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="ffaaf-163">已更新</span><span class="sxs-lookup"><span data-stu-id="ffaaf-163">Updated</span></span>

* <span data-ttu-id="ffaaf-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-164">*Startup.cs*</span></span>

<span data-ttu-id="ffaaf-165">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="ffaaf-166">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="ffaaf-167">Scaffold 處理序會建立下列檔案：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="ffaaf-168">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="ffaaf-169">下一節將說明所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="ffaaf-170">初始移轉</span><span class="sxs-lookup"><span data-stu-id="ffaaf-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ffaaf-172">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="ffaaf-173">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-173">Add an initial migration.</span></span>
* <span data-ttu-id="ffaaf-174">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="ffaaf-175">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="ffaaf-177">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-179">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="ffaaf-180">上述命令會產生下列警告：「實體類型 ' Movie ' 的十進位資料行 ' Price ' 未指定任何類型。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="ffaaf-181">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="ffaaf-182">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="ffaaf-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="ffaaf-183">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="ffaaf-184">[遷移] 命令會產生程式碼來建立初始資料庫架構。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="ffaaf-185">架構是以 `DbContext` 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="ffaaf-186">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="ffaaf-187">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="ffaaf-188">`update` 命令會在尚未套用的遷移中執行 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="ffaaf-189">在此情況下，`update` 會在建立資料庫的「*遷移/\<時間戳記 > _InitialCreate .cs*檔案中執行 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="ffaaf-191">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="ffaaf-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="ffaaf-192">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ffaaf-193">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ffaaf-194">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="ffaaf-195">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="ffaaf-196">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="ffaaf-197">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ffaaf-198">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="ffaaf-199">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="ffaaf-200">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="ffaaf-201">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="ffaaf-202">上述程式碼會建立實體集的 [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-202">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="ffaaf-203">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="ffaaf-204">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="ffaaf-205">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="ffaaf-206">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ffaaf-208">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-209">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ffaaf-210">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="ffaaf-211">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-211">Test the app</span></span>

* <span data-ttu-id="ffaaf-212">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="ffaaf-213">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="ffaaf-214">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="ffaaf-215">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-215">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="ffaaf-217">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="ffaaf-218">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="ffaaf-219">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="ffaaf-220">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ffaaf-221">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ffaaf-222">其他資源</span><span class="sxs-lookup"><span data-stu-id="ffaaf-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ffaaf-223">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="ffaaf-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ffaaf-224">在本節中，會加入類別來管理跨平臺[SQLite 資料庫](https://www.sqlite.org/index.html)中的電影。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="ffaaf-225">從 ASP.NET Core 範本建立的應用程式會使用 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="ffaaf-226">應用程式的模型類別會與[Entity Framework Core （EF Core）](/ef/core) （[SQLite EF Core 資料庫提供者](/ef/core/providers/sqlite)）搭配使用，以使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="ffaaf-227">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="ffaaf-228">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="ffaaf-229">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ffaaf-230">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="ffaaf-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ffaaf-232">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="ffaaf-233">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-233">Name the folder *Models*.</span></span>

<span data-ttu-id="ffaaf-234">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-234">Right click the *Models* folder.</span></span> <span data-ttu-id="ffaaf-235">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="ffaaf-236">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ffaaf-238">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="ffaaf-239">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-240">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ffaaf-241">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="ffaaf-242">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="ffaaf-243">以滑鼠右鍵按一下 [Models] 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="ffaaf-244">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ffaaf-245">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="ffaaf-246">在中央窗格中選取 [類別是空的]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="ffaaf-247">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="ffaaf-248">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="ffaaf-249">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="ffaaf-249">Scaffold the movie model</span></span>

<span data-ttu-id="ffaaf-250">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="ffaaf-251">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ffaaf-253">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="ffaaf-254">以滑鼠右鍵按一下 [Pages] 資料夾 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="ffaaf-255">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-255">Name the folder *Movies*</span></span>

<span data-ttu-id="ffaaf-256">以滑鼠右鍵按一下 [Pages/Movies] 資料夾 > [新增] > [新增 Scaffolded 項目]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="ffaaf-258">在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="ffaaf-260">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="ffaaf-261">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="ffaaf-262">在 [資料內容類別] 列中選取 [+] (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="ffaaf-263">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-263">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="ffaaf-265">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="ffaaf-267">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ffaaf-268">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="ffaaf-269">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="ffaaf-270">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-271">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ffaaf-272">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ffaaf-273">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="ffaaf-274">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="ffaaf-275">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="ffaaf-276">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="ffaaf-276">Files created</span></span>

* <span data-ttu-id="ffaaf-277">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="ffaaf-278">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="ffaaf-279">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="ffaaf-279">File updated</span></span>

* <span data-ttu-id="ffaaf-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-280">*Startup.cs*</span></span>

<span data-ttu-id="ffaaf-281">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="ffaaf-282">初始移轉</span><span class="sxs-lookup"><span data-stu-id="ffaaf-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ffaaf-284">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="ffaaf-285">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-285">Add an initial migration.</span></span>
* <span data-ttu-id="ffaaf-286">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="ffaaf-287">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="ffaaf-289">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="ffaaf-290">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="ffaaf-291">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="ffaaf-292">`InitialCreate` 引數是用來命名遷移。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="ffaaf-293">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="ffaaf-294">如需詳細資訊，請參閱<xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="ffaaf-295">`Update-Database` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="ffaaf-296">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-298">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="ffaaf-299">上述命令會產生下列警告：「*實體類型 ' Movie ' 的十進位資料行 ' Price ' 未指定任何類型。如果值不符合預設的有效位數和小數位數，這會導致以無訊息模式截斷值。使用 ' HasColumnType （） ' 明確指定可容納所有值的 SQL server 資料行類型。* 」您可以忽略該警告，其將在稍後的教學課程中修正。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffaaf-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffaaf-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="ffaaf-301">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="ffaaf-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="ffaaf-302">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ffaaf-303">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ffaaf-304">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="ffaaf-305">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="ffaaf-306">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="ffaaf-307">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ffaaf-308">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="ffaaf-309">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="ffaaf-310">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="ffaaf-311">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="ffaaf-312">上述程式碼會建立實體集的 [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-312">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="ffaaf-313">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="ffaaf-314">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="ffaaf-315">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="ffaaf-316">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ffaaf-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffaaf-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ffaaf-318">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ffaaf-319">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ffaaf-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ffaaf-320">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="ffaaf-321">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-321">Test the app</span></span>

* <span data-ttu-id="ffaaf-322">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="ffaaf-323">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="ffaaf-324">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="ffaaf-325">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-325">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="ffaaf-327">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="ffaaf-328">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="ffaaf-329">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="ffaaf-330">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ffaaf-331">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ffaaf-332">其他資源</span><span class="sxs-lookup"><span data-stu-id="ffaaf-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ffaaf-333">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="ffaaf-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
