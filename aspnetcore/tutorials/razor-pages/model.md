---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 6132f7b907014b4f57bb9ae0300e00b6ecb23f1a
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820076"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="e22a9-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="e22a9-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="e22a9-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e22a9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e22a9-105">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="e22a9-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="e22a9-106">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="e22a9-107">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="e22a9-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="e22a9-108">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="e22a9-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="e22a9-109">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="e22a9-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="e22a9-110">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="e22a9-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e22a9-112">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="e22a9-113">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e22a9-113">Name the folder *Models*.</span></span>

<span data-ttu-id="e22a9-114">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e22a9-114">Right click the *Models* folder.</span></span> <span data-ttu-id="e22a9-115">選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="e22a9-116">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="e22a9-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e22a9-118">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e22a9-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="e22a9-119">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e22a9-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e22a9-121">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="e22a9-122">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e22a9-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="e22a9-123">以滑鼠右鍵按一下 [Models]  資料夾，然後選取 [新增]  > [新增檔案]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="e22a9-124">在 [新增檔案]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="e22a9-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="e22a9-125">在左窗格中選取 [一般]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="e22a9-126">在中央窗格中選取 [類別是空的]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="e22a9-127">將類別命名為 **Movie**，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="e22a9-128">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="e22a9-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="e22a9-129">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="e22a9-129">Scaffold the movie model</span></span>

<span data-ttu-id="e22a9-130">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="e22a9-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="e22a9-131">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="e22a9-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e22a9-133">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e22a9-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="e22a9-134">以滑鼠右鍵按一下 [Pages]  資料夾 > [新增]  > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="e22a9-135">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="e22a9-135">Name the folder *Movies*</span></span>

<span data-ttu-id="e22a9-136">以滑鼠右鍵按一下 [Pages/Movies]  資料夾 > [新增]  > [新增 Scaffolded 項目]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="e22a9-138">在 [新增 Scaffold]  對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]  > [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="e22a9-140">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)  對話方塊：</span><span class="sxs-lookup"><span data-stu-id="e22a9-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="e22a9-141">在 [模型類別]  下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="e22a9-142">在 [資料內容類別]  列中選取 [+]  \(加號\)，並將產生的名稱 RazorPagesMovie.**Models**.RazorPagesMovieContext 變更為 RazorPagesMovie.**Data**.RazorPagesMovieContext。</span><span class="sxs-lookup"><span data-stu-id="e22a9-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="e22a9-143">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="e22a9-144">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="e22a9-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="e22a9-145">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-145">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/3/arp.png)

<span data-ttu-id="e22a9-147">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="e22a9-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="e22a9-149">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="e22a9-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e22a9-150">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="e22a9-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="e22a9-151">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="e22a9-152">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-153">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e22a9-154">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="e22a9-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e22a9-155">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="e22a9-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="e22a9-156">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="e22a9-157">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="e22a9-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="e22a9-158">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="e22a9-158">Files created</span></span>

* <span data-ttu-id="e22a9-159">*Pages/Movies*：Create、Delete、Details、Edit 和 Index。</span><span class="sxs-lookup"><span data-stu-id="e22a9-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="e22a9-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="e22a9-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="e22a9-161">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="e22a9-161">File updated</span></span>

* <span data-ttu-id="e22a9-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="e22a9-162">*Startup.cs*</span></span>

<span data-ttu-id="e22a9-163">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="e22a9-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="e22a9-164">初始移轉</span><span class="sxs-lookup"><span data-stu-id="e22a9-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e22a9-166">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="e22a9-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="e22a9-167">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="e22a9-167">Add an initial migration.</span></span>
* <span data-ttu-id="e22a9-168">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="e22a9-169">從 [工具]  功能表中，選取 [NuGet 套件管理員]  > [套件管理員主控台]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e22a9-171">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-173">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="e22a9-174">上述命令會產生下列警告：「實體類型 'Movie' 上的十進位資料行 'Price' 未指定任何類型。</span><span class="sxs-lookup"><span data-stu-id="e22a9-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="e22a9-175">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="e22a9-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="e22a9-176">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="e22a9-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="e22a9-177">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="e22a9-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="e22a9-178">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e22a9-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e22a9-179">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="e22a9-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e22a9-180">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="e22a9-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="e22a9-181">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="e22a9-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="e22a9-182">`ef database update` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="e22a9-183">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="e22a9-185">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="e22a9-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="e22a9-186">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e22a9-187">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="e22a9-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="e22a9-188">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="e22a9-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e22a9-189">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="e22a9-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="e22a9-190">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e22a9-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="e22a9-191">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e22a9-192">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="e22a9-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="e22a9-193">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="e22a9-194">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="e22a9-195">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="e22a9-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="e22a9-196">上述程式碼會建立實體集的 [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="e22a9-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e22a9-197">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="e22a9-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e22a9-198">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="e22a9-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e22a9-199">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="e22a9-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e22a9-200">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="e22a9-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e22a9-202">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e22a9-204">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="e22a9-205">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e22a9-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e22a9-206">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="e22a9-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e22a9-207">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="e22a9-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e22a9-208">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="e22a9-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="e22a9-209">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="e22a9-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="e22a9-210">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e22a9-211">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e22a9-211">Test the app</span></span>

* <span data-ttu-id="e22a9-212">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="e22a9-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="e22a9-213">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="e22a9-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="e22a9-214">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="e22a9-215">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="e22a9-215">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="e22a9-217">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="e22a9-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e22a9-218">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="e22a9-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e22a9-219">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e22a9-220">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="e22a9-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e22a9-221">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="e22a9-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e22a9-222">其他資源</span><span class="sxs-lookup"><span data-stu-id="e22a9-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e22a9-223">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffolded Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="e22a9-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e22a9-224">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="e22a9-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="e22a9-225">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="e22a9-226">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="e22a9-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="e22a9-227">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="e22a9-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="e22a9-228">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="e22a9-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="e22a9-229">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="e22a9-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e22a9-231">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="e22a9-232">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e22a9-232">Name the folder *Models*.</span></span>

<span data-ttu-id="e22a9-233">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e22a9-233">Right click the *Models* folder.</span></span> <span data-ttu-id="e22a9-234">選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="e22a9-235">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="e22a9-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e22a9-237">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e22a9-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="e22a9-238">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e22a9-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-239">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e22a9-240">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="e22a9-241">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e22a9-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="e22a9-242">以滑鼠右鍵按一下 [Models]  資料夾，然後選取 [新增]  > [新增檔案]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="e22a9-243">在 [新增檔案]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="e22a9-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="e22a9-244">在左窗格中選取 [一般]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="e22a9-245">在中央窗格中選取 [類別是空的]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="e22a9-246">將類別命名為 **Movie**，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="e22a9-247">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="e22a9-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="e22a9-248">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="e22a9-248">Scaffold the movie model</span></span>

<span data-ttu-id="e22a9-249">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="e22a9-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="e22a9-250">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="e22a9-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e22a9-252">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e22a9-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="e22a9-253">以滑鼠右鍵按一下 [Pages]  資料夾 > [新增]  > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="e22a9-254">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="e22a9-254">Name the folder *Movies*</span></span>

<span data-ttu-id="e22a9-255">以滑鼠右鍵按一下 [Pages/Movies]  資料夾 > [新增]  > [新增 Scaffolded 項目]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="e22a9-257">在 [新增 Scaffold]  對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]  > [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="e22a9-259">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)  對話方塊：</span><span class="sxs-lookup"><span data-stu-id="e22a9-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="e22a9-260">在 [模型類別]  下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="e22a9-261">在 [資料內容類別]  列中選取 [+]  (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="e22a9-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="e22a9-262">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-262">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="e22a9-264">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="e22a9-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-265">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="e22a9-266">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="e22a9-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e22a9-267">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="e22a9-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e22a9-268">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="e22a9-269">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-270">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e22a9-271">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="e22a9-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e22a9-272">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="e22a9-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e22a9-273">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="e22a9-274">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="e22a9-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="e22a9-275">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="e22a9-275">Files created</span></span>

* <span data-ttu-id="e22a9-276">*Pages/Movies*：Create、Delete、Details、Edit 和 Index。</span><span class="sxs-lookup"><span data-stu-id="e22a9-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="e22a9-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="e22a9-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="e22a9-278">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="e22a9-278">File updated</span></span>

* <span data-ttu-id="e22a9-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="e22a9-279">*Startup.cs*</span></span>

<span data-ttu-id="e22a9-280">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="e22a9-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="e22a9-281">初始移轉</span><span class="sxs-lookup"><span data-stu-id="e22a9-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e22a9-283">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="e22a9-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="e22a9-284">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="e22a9-284">Add an initial migration.</span></span>
* <span data-ttu-id="e22a9-285">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="e22a9-286">從 [工具]  功能表中，選取 [NuGet 套件管理員]  > [套件管理員主控台]  。</span><span class="sxs-lookup"><span data-stu-id="e22a9-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e22a9-288">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="e22a9-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-289">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-290">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="e22a9-291">上述命令會產生下列警告：「實體類型 'Movie' 上的十進位資料行 'Price' 未指定任何類型。</span><span class="sxs-lookup"><span data-stu-id="e22a9-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="e22a9-292">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="e22a9-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="e22a9-293">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="e22a9-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="e22a9-294">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="e22a9-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="e22a9-295">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e22a9-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e22a9-296">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="e22a9-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e22a9-297">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="e22a9-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="e22a9-298">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="e22a9-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="e22a9-299">`ef database update` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="e22a9-300">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e22a9-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e22a9-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="e22a9-302">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="e22a9-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="e22a9-303">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e22a9-304">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="e22a9-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="e22a9-305">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="e22a9-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e22a9-306">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="e22a9-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="e22a9-307">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e22a9-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="e22a9-308">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e22a9-309">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="e22a9-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="e22a9-310">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="e22a9-311">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="e22a9-312">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="e22a9-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="e22a9-313">上述程式碼會建立實體集的 [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="e22a9-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e22a9-314">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="e22a9-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e22a9-315">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="e22a9-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e22a9-316">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="e22a9-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e22a9-317">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="e22a9-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e22a9-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e22a9-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e22a9-319">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e22a9-320">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e22a9-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e22a9-321">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="e22a9-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="e22a9-322">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e22a9-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e22a9-323">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="e22a9-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e22a9-324">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="e22a9-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e22a9-325">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="e22a9-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="e22a9-326">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="e22a9-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="e22a9-327">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e22a9-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e22a9-328">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e22a9-328">Test the app</span></span>

* <span data-ttu-id="e22a9-329">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="e22a9-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="e22a9-330">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="e22a9-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="e22a9-331">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="e22a9-332">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="e22a9-332">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="e22a9-334">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="e22a9-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e22a9-335">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="e22a9-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e22a9-336">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="e22a9-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e22a9-337">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="e22a9-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e22a9-338">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="e22a9-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e22a9-339">其他資源</span><span class="sxs-lookup"><span data-stu-id="e22a9-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e22a9-340">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffolded Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="e22a9-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
