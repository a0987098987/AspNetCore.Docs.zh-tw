---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 02/12/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: be9f515178d0169a69487f917c7d39c6f11f1292
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815055"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="fdcd3-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="fdcd3-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="fdcd3-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fdcd3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="fdcd3-105">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="fdcd3-106">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="fdcd3-107">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="fdcd3-108">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="fdcd3-109">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="fdcd3-110">[檢視或下載](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start)範例。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-110">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="fdcd3-111">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="fdcd3-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fdcd3-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdcd3-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fdcd3-113">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="fdcd3-114">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-114">Name the folder *Models*.</span></span>

<span data-ttu-id="fdcd3-115">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-115">Right click the *Models* folder.</span></span> <span data-ttu-id="fdcd3-116">選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="fdcd3-117">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fdcd3-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fdcd3-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fdcd3-119">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="fdcd3-120">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fdcd3-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fdcd3-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fdcd3-122">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fdcd3-123">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="fdcd3-124">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [新增檔案]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fdcd3-125">在 [新增檔案]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fdcd3-126">在左窗格中選取 [一般]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fdcd3-127">在中央窗格中選取 [類別是空的]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="fdcd3-128">將類別命名為 **Movie**，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="fdcd3-129">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="fdcd3-130">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="fdcd3-130">Scaffold the movie model</span></span>

<span data-ttu-id="fdcd3-131">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="fdcd3-132">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fdcd3-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdcd3-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fdcd3-134">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="fdcd3-135">以滑鼠右鍵按一下 *Pages* 資料夾 > [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="fdcd3-136">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="fdcd3-136">Name the folder *Movies*</span></span>

<span data-ttu-id="fdcd3-137">以滑鼠右鍵按一下 *Pages/Movies* 資料夾 > [新增]   > [新增 Scaffolded 項目]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="fdcd3-139">在 [新增 Scaffold]  對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]   > [新增]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="fdcd3-141">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)  對話方塊：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="fdcd3-142">在 [模型類別]  下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="fdcd3-143">在 [資料內容類別]  列中選取 [+]  (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="fdcd3-144">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-144">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="fdcd3-146">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fdcd3-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fdcd3-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="fdcd3-148">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fdcd3-149">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="fdcd3-150">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="fdcd3-151">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fdcd3-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fdcd3-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fdcd3-153">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fdcd3-154">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="fdcd3-155">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="fdcd3-156">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-156">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="fdcd3-157">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="fdcd3-157">Files created</span></span>

* <span data-ttu-id="fdcd3-158">*Pages/Movies*：Create、Delete、Details、Edit 和 Index。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-158">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="fdcd3-159">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="fdcd3-159">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="fdcd3-160">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="fdcd3-160">File updated</span></span>

* <span data-ttu-id="fdcd3-161">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="fdcd3-161">*Startup.cs*</span></span>

<span data-ttu-id="fdcd3-162">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-162">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="fdcd3-163">初始移轉</span><span class="sxs-lookup"><span data-stu-id="fdcd3-163">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fdcd3-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdcd3-164">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fdcd3-165">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-165">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="fdcd3-166">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-166">Add an initial migration.</span></span>
* <span data-ttu-id="fdcd3-167">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-167">Update the database with the initial migration.</span></span>

<span data-ttu-id="fdcd3-168">從 [工具]  功能表中，選取 [NuGet 套件管理員]   > [套件管理員主控台]  。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-168">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="fdcd3-170">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-170">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fdcd3-171">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fdcd3-171">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fdcd3-172">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fdcd3-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="fdcd3-173">上述命令會產生下列警告：「實體類型 'Movie' 上的十進位資料行 'Price' 未指定任何類型。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-173">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="fdcd3-174">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-174">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="fdcd3-175">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="fdcd3-175">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="fdcd3-176">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-176">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="fdcd3-177">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-177">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fdcd3-178">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-178">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fdcd3-179">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-179">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="fdcd3-180">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-180">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="fdcd3-181">`ef database update` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-181">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="fdcd3-182">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-182">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fdcd3-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdcd3-183">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="fdcd3-184">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="fdcd3-184">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="fdcd3-185">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-185">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="fdcd3-186">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-186">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="fdcd3-187">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-187">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="fdcd3-188">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-188">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="fdcd3-189">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-189">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="fdcd3-190">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-190">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fdcd3-191">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-191">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="fdcd3-192">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-192">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="fdcd3-193">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-193">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="fdcd3-194">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-194">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="fdcd3-195">上述程式碼會建立實體集的 [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-195">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="fdcd3-196">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-196">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="fdcd3-197">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-197">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="fdcd3-198">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-198">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="fdcd3-199">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-199">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fdcd3-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fdcd3-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fdcd3-201">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-201">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fdcd3-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fdcd3-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fdcd3-203">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-203">Examine the `Up` method.</span></span>

---

<span data-ttu-id="fdcd3-204">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-204">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fdcd3-205">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-205">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fdcd3-206">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-206">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="fdcd3-207">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-207">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="fdcd3-208">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-208">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="fdcd3-209">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-209">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="fdcd3-210">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="fdcd3-210">Test the app</span></span>

* <span data-ttu-id="fdcd3-211">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-211">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="fdcd3-212">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="fdcd3-212">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="fdcd3-213">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-213">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="fdcd3-214">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-214">Test the **Create** link.</span></span>

  ![建立頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="fdcd3-216">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-216">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="fdcd3-217">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-217">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="fdcd3-218">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-218">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="fdcd3-219">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-219">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="fdcd3-220">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="fdcd3-220">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fdcd3-221">其他資源</span><span class="sxs-lookup"><span data-stu-id="fdcd3-221">Additional resources</span></span>

* [<span data-ttu-id="fdcd3-222">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="fdcd3-222">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=sFVIsdR_RcM)

> [!div class="step-by-step"]
> <span data-ttu-id="fdcd3-223">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffolded Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="fdcd3-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
