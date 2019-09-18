---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 18cf9aea930a7989bb844bc6c40dfa1ce84b7b4d
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082617"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="f2907-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="f2907-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="f2907-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f2907-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f2907-105">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="f2907-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="f2907-106">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="f2907-107">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="f2907-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="f2907-108">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="f2907-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="f2907-109">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="f2907-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f2907-110">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="f2907-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2907-112">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f2907-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="f2907-113">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f2907-113">Name the folder *Models*.</span></span>

<span data-ttu-id="f2907-114">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2907-114">Right click the *Models* folder.</span></span> <span data-ttu-id="f2907-115">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f2907-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="f2907-116">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="f2907-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f2907-118">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2907-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="f2907-119">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2907-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f2907-121">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f2907-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="f2907-122">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f2907-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="f2907-123">以滑鼠右鍵按一下 [Models] 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="f2907-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="f2907-124">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f2907-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="f2907-125">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="f2907-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="f2907-126">在中央窗格中選取 [類別是空的]。</span><span class="sxs-lookup"><span data-stu-id="f2907-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="f2907-127">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f2907-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="f2907-128">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="f2907-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="f2907-129">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="f2907-129">Scaffold the movie model</span></span>

<span data-ttu-id="f2907-130">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="f2907-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="f2907-131">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="f2907-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2907-133">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="f2907-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="f2907-134">以滑鼠右鍵按一下 [Pages] 資料夾 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f2907-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="f2907-135">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="f2907-135">Name the folder *Movies*</span></span>

<span data-ttu-id="f2907-136">以滑鼠右鍵按一下 [Pages/Movies] 資料夾 > [新增] > [新增 Scaffolded 項目]。</span><span class="sxs-lookup"><span data-stu-id="f2907-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="f2907-138">在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="f2907-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="f2907-140">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="f2907-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="f2907-141">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="f2907-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="f2907-142">在 [資料內容類別] 列中選取 [+] \(加號\)，並將產生的名稱 RazorPagesMovie.**Models**.RazorPagesMovieContext 變更為 RazorPagesMovie.**Data**.RazorPagesMovieContext。</span><span class="sxs-lookup"><span data-stu-id="f2907-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="f2907-143">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="f2907-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="f2907-144">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="f2907-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="f2907-145">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f2907-145">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/3/arp.png)

<span data-ttu-id="f2907-147">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="f2907-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="f2907-149">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f2907-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f2907-150">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="f2907-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="f2907-151">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="f2907-152">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-153">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f2907-154">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f2907-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f2907-155">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="f2907-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="f2907-156">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="f2907-157">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="f2907-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2907-159">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="f2907-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="f2907-160">*Pages/Movies*：Create、Delete、Details、Edit 和 Index。</span><span class="sxs-lookup"><span data-stu-id="f2907-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="f2907-161">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="f2907-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="f2907-162">已更新</span><span class="sxs-lookup"><span data-stu-id="f2907-162">Updated</span></span>

* <span data-ttu-id="f2907-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="f2907-163">*Startup.cs*</span></span>

<span data-ttu-id="f2907-164">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="f2907-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f2907-165">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f2907-166">Scaffold 處理序會建立下列檔案：</span><span class="sxs-lookup"><span data-stu-id="f2907-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="f2907-167">*Pages/Movies*：Create、Delete、Details、Edit 和 Index。</span><span class="sxs-lookup"><span data-stu-id="f2907-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="f2907-168">下一節將說明所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="f2907-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="f2907-169">初始移轉</span><span class="sxs-lookup"><span data-stu-id="f2907-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2907-171">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="f2907-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="f2907-172">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="f2907-172">Add an initial migration.</span></span>
* <span data-ttu-id="f2907-173">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="f2907-174">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="f2907-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="f2907-176">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-178">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="f2907-179">上述命令會產生下列警告：「實體類型 'Movie' 上的十進位資料行 'Price' 未指定任何類型。</span><span class="sxs-lookup"><span data-stu-id="f2907-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="f2907-180">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="f2907-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="f2907-181">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="f2907-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="f2907-182">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="f2907-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="f2907-183">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="f2907-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="f2907-184">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="f2907-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="f2907-185">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="f2907-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="f2907-186">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="f2907-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="f2907-187">`ef database update` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="f2907-188">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="f2907-190">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="f2907-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="f2907-191">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="f2907-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f2907-192">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="f2907-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="f2907-193">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="f2907-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="f2907-194">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="f2907-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="f2907-195">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="f2907-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="f2907-196">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f2907-197">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="f2907-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="f2907-198">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="f2907-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="f2907-199">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="f2907-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="f2907-200">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="f2907-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="f2907-201">上述程式碼會建立實體集的 [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="f2907-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="f2907-202">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="f2907-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="f2907-203">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="f2907-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="f2907-204">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="f2907-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="f2907-205">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="f2907-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f2907-207">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-208">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f2907-209">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="f2907-210">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="f2907-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="f2907-211">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="f2907-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="f2907-212">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="f2907-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f2907-213">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="f2907-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="f2907-214">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="f2907-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="f2907-215">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="f2907-216">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="f2907-216">Test the app</span></span>

* <span data-ttu-id="f2907-217">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="f2907-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="f2907-218">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="f2907-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="f2907-219">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="f2907-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="f2907-220">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="f2907-220">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="f2907-222">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="f2907-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="f2907-223">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="f2907-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="f2907-224">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="f2907-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="f2907-225">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="f2907-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="f2907-226">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="f2907-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f2907-227">其他資源</span><span class="sxs-lookup"><span data-stu-id="f2907-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f2907-228">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffolded Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f2907-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f2907-229">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="f2907-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="f2907-230">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="f2907-231">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="f2907-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="f2907-232">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="f2907-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="f2907-233">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="f2907-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f2907-234">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="f2907-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2907-236">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f2907-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="f2907-237">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f2907-237">Name the folder *Models*.</span></span>

<span data-ttu-id="f2907-238">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2907-238">Right click the *Models* folder.</span></span> <span data-ttu-id="f2907-239">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f2907-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="f2907-240">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="f2907-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f2907-242">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2907-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="f2907-243">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2907-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-244">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f2907-245">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f2907-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="f2907-246">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f2907-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="f2907-247">以滑鼠右鍵按一下 [Models] 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="f2907-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="f2907-248">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f2907-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="f2907-249">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="f2907-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="f2907-250">在中央窗格中選取 [類別是空的]。</span><span class="sxs-lookup"><span data-stu-id="f2907-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="f2907-251">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f2907-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="f2907-252">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="f2907-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="f2907-253">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="f2907-253">Scaffold the movie model</span></span>

<span data-ttu-id="f2907-254">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="f2907-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="f2907-255">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="f2907-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2907-257">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="f2907-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="f2907-258">以滑鼠右鍵按一下 [Pages] 資料夾 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f2907-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="f2907-259">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="f2907-259">Name the folder *Movies*</span></span>

<span data-ttu-id="f2907-260">以滑鼠右鍵按一下 [Pages/Movies] 資料夾 > [新增] > [新增 Scaffolded 項目]。</span><span class="sxs-lookup"><span data-stu-id="f2907-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="f2907-262">在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="f2907-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="f2907-264">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="f2907-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="f2907-265">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="f2907-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="f2907-266">在 [資料內容類別] 列中選取 [+] (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="f2907-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="f2907-267">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f2907-267">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="f2907-269">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="f2907-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="f2907-271">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f2907-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f2907-272">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="f2907-272">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="f2907-273">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-273">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="f2907-274">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-274">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-275">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f2907-276">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f2907-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f2907-277">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="f2907-277">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="f2907-278">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-278">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="f2907-279">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="f2907-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="f2907-280">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="f2907-280">Files created</span></span>

* <span data-ttu-id="f2907-281">*Pages/Movies*：Create、Delete、Details、Edit 和 Index。</span><span class="sxs-lookup"><span data-stu-id="f2907-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="f2907-282">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="f2907-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="f2907-283">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="f2907-283">File updated</span></span>

* <span data-ttu-id="f2907-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="f2907-284">*Startup.cs*</span></span>

<span data-ttu-id="f2907-285">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="f2907-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="f2907-286">初始移轉</span><span class="sxs-lookup"><span data-stu-id="f2907-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2907-288">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="f2907-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="f2907-289">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="f2907-289">Add an initial migration.</span></span>
* <span data-ttu-id="f2907-290">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="f2907-291">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="f2907-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="f2907-293">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2907-293">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-295">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="f2907-296">上述命令會產生下列警告：「實體類型 'Movie' 上的十進位資料行 'Price' 未指定任何類型。</span><span class="sxs-lookup"><span data-stu-id="f2907-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="f2907-297">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="f2907-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="f2907-298">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="f2907-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="f2907-299">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="f2907-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="f2907-300">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="f2907-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="f2907-301">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="f2907-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="f2907-302">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="f2907-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="f2907-303">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="f2907-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="f2907-304">`ef database update` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="f2907-305">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2907-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2907-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="f2907-307">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="f2907-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="f2907-308">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="f2907-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f2907-309">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="f2907-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="f2907-310">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="f2907-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="f2907-311">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="f2907-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="f2907-312">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="f2907-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="f2907-313">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f2907-314">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="f2907-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="f2907-315">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="f2907-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="f2907-316">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="f2907-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="f2907-317">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="f2907-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="f2907-318">上述程式碼會建立實體集的 [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="f2907-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="f2907-319">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="f2907-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="f2907-320">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="f2907-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="f2907-321">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="f2907-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="f2907-322">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="f2907-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2907-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2907-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f2907-324">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2907-325">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2907-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f2907-326">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2907-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="f2907-327">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="f2907-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="f2907-328">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="f2907-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="f2907-329">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="f2907-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f2907-330">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="f2907-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="f2907-331">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="f2907-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="f2907-332">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2907-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="f2907-333">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="f2907-333">Test the app</span></span>

* <span data-ttu-id="f2907-334">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="f2907-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="f2907-335">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="f2907-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="f2907-336">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="f2907-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="f2907-337">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="f2907-337">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="f2907-339">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="f2907-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="f2907-340">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="f2907-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="f2907-341">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="f2907-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="f2907-342">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="f2907-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="f2907-343">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="f2907-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f2907-344">其他資源</span><span class="sxs-lookup"><span data-stu-id="f2907-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f2907-345">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffolded Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f2907-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
