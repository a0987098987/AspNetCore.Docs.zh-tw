---
title: 新增模型到 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 請將模型新增至簡單的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: ed83ab92c70ea87f3c805787303e24c9ecfc4e12
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265532"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="41239-103">新增模型到 ASP.NET Core MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="41239-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="41239-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="41239-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="41239-105">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="41239-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="41239-106">這些類別是 **MVC** 應用程式的「模型」部分。</span><span class="sxs-lookup"><span data-stu-id="41239-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="41239-107">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="41239-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="41239-108">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化您必須撰寫的資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="41239-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="41239-109">您所建立的模型類別稱為 POCO 類別 (來自「純舊 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="41239-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="41239-110">它們只會定義資料庫將儲存之資料的屬性。</span><span class="sxs-lookup"><span data-stu-id="41239-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="41239-111">在本教學課程中，您首先要撰寫模型類別，而 EF Core 會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="41239-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="41239-112">本文未提及的替代方法是從現有的資料庫產生模型類別。</span><span class="sxs-lookup"><span data-stu-id="41239-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="41239-113">如需該方法的資訊，請參閱 [ASP.NET Core - 現有的資料庫](/ef/core/get-started/aspnetcore/existing-db)。</span><span class="sxs-lookup"><span data-stu-id="41239-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="41239-114">新增資料模型類別</span><span class="sxs-lookup"><span data-stu-id="41239-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41239-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41239-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="41239-116">以滑鼠右鍵按一下 *Models* 資料夾 > [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="41239-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="41239-117">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="41239-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="41239-118">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="41239-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="41239-119">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="41239-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="41239-120">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="41239-120">Scaffold the movie model</span></span>

<span data-ttu-id="41239-121">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="41239-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="41239-122">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="41239-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41239-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41239-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="41239-124">在 [方案總管] 中以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="41239-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![上方步驟的檢視](adding-model/_static/add_controller21.png)

<span data-ttu-id="41239-126">在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="41239-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="41239-128">完成 [新增控制器] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="41239-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="41239-129">**模型類別：** Movie (MvcMovie.Models) </span><span class="sxs-lookup"><span data-stu-id="41239-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="41239-130">**資料內容類別：** 選取 **+** 圖示並新增預設的 **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="41239-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![新增資料內容](adding-model/_static/dc.png)

* <span data-ttu-id="41239-132">**檢視：** 保持核取預設的每一個選項</span><span class="sxs-lookup"><span data-stu-id="41239-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="41239-133">**控制器名稱：** 保留預設 *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="41239-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="41239-134">選取 [新增]</span><span class="sxs-lookup"><span data-stu-id="41239-134">Select **Add**</span></span>

![[新增控制器] 對話方塊](adding-model/_static/add_controller2.png)

<span data-ttu-id="41239-136">Visual Studio 會建立：</span><span class="sxs-lookup"><span data-stu-id="41239-136">Visual Studio creates:</span></span>

* <span data-ttu-id="41239-137">Entity Framework Core [資料庫內容類別](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="41239-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="41239-138">電影控制器 (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="41239-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="41239-139">Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="41239-139">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="41239-140">自動建立資料庫內容與 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。</span><span class="sxs-lookup"><span data-stu-id="41239-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="41239-141">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41239-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="41239-142">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="41239-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="41239-143">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="41239-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="41239-144">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="41239-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="41239-145">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="41239-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="41239-146">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="41239-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="41239-147">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="41239-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="41239-148">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="41239-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="41239-149">如果執行應用程式，然後按一下 **Mvc Movie** 連結，則會收到如下錯誤：</span><span class="sxs-lookup"><span data-stu-id="41239-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="41239-150">您需要建立資料庫，而且您會使用 EF Core [移轉](xref:data/ef-mvc/migrations)功能來執行此作業。</span><span class="sxs-lookup"><span data-stu-id="41239-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="41239-151">移轉可讓您建立符合資料模型的資料庫，並在資料模型變更時，更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="41239-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="41239-152">初始移轉</span><span class="sxs-lookup"><span data-stu-id="41239-152">Initial migration</span></span>

<span data-ttu-id="41239-153">在本節中，將會完成下列作業：</span><span class="sxs-lookup"><span data-stu-id="41239-153">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="41239-154">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="41239-154">Add an initial migration.</span></span>
* <span data-ttu-id="41239-155">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="41239-155">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41239-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41239-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="41239-157">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台] (PMC)。</span><span class="sxs-lookup"><span data-stu-id="41239-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![PMC 功能表](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="41239-159">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="41239-159">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="41239-160">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="41239-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="41239-161">資料庫結構描述會以 `MvcMovieContext` 類別 (在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="41239-161">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="41239-162">`Initial` 引數是移轉名稱。</span><span class="sxs-lookup"><span data-stu-id="41239-162">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="41239-163">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="41239-163">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="41239-164">如需詳細資訊，請參閱<xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="41239-164">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="41239-165">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="41239-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="41239-166">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="41239-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="41239-167">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="41239-167">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="41239-168">資料庫結構描述會以 `MvcMovieContext` 類別 (在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="41239-168">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="41239-169">`InitialCreate` 引數是移轉名稱。</span><span class="sxs-lookup"><span data-stu-id="41239-169">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="41239-170">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="41239-170">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="41239-171">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="41239-171">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="41239-172">ASP.NET Core 內建[相依性插入 (DI)](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="41239-172">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="41239-173">服務 (例如 EF Core DB 內容) 會在應用程式啟動期間使用 DI 來註冊。</span><span class="sxs-lookup"><span data-stu-id="41239-173">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="41239-174">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="41239-174">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="41239-175">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="41239-175">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41239-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41239-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="41239-177">Scaffolding 工具會自動建立 DB 內容，並向 DI 容器註冊該內容。</span><span class="sxs-lookup"><span data-stu-id="41239-177">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="41239-178">請檢查下列 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="41239-178">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="41239-179">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="41239-179">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="41239-180">`MvcMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="41239-180">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="41239-181">資料內容 (`MvcMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="41239-181">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="41239-182">資料內容會指定資料模型包含哪些實體：</span><span class="sxs-lookup"><span data-stu-id="41239-182">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="41239-183">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="41239-183">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="41239-184">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="41239-184">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="41239-185">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="41239-185">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="41239-186">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="41239-186">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="41239-187">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="41239-187">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="41239-188">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="41239-188">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="41239-189">您已建立 DB 內容，並向 DI 容器註冊該內容。</span><span class="sxs-lookup"><span data-stu-id="41239-189">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="41239-190">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="41239-190">Test the app</span></span>

* <span data-ttu-id="41239-191">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="41239-191">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="41239-192">如果您收到類似如下的資料庫例外狀況：</span><span class="sxs-lookup"><span data-stu-id="41239-192">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="41239-193">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="41239-193">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="41239-194">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="41239-194">Test the **Create** link.</span></span>

  > [!NOTE]
  > <span data-ttu-id="41239-195">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="41239-195">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="41239-196">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="41239-196">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="41239-197">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="41239-197">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="41239-198">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="41239-198">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="41239-199">檢查 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="41239-199">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="41239-200">上述醒目提示的程式碼會顯示要新增至[相依性插入](xref:fundamentals/dependency-injection)容器的電影資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="41239-200">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="41239-201">`services.AddDbContext<MvcMovieContext>(options =>` 指定要使用的資料庫和連線字串。</span><span class="sxs-lookup"><span data-stu-id="41239-201">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="41239-202">`=>` 是 [Lambda 運算子](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="41239-202">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="41239-203">開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：</span><span class="sxs-lookup"><span data-stu-id="41239-203">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="41239-204">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext `) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="41239-204">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="41239-205">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="41239-205">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="41239-206">強型別模型和 @model 關鍵字</span><span class="sxs-lookup"><span data-stu-id="41239-206">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="41239-207">稍早在本教學課程中，您已看到控制器如何使用 `ViewData` 字典傳遞資料或物件給檢視。</span><span class="sxs-lookup"><span data-stu-id="41239-207">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="41239-208">`ViewData` 字典是一種動態物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="41239-208">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="41239-209">MVC 也提供將強型別模型物件傳遞至檢視的能力。</span><span class="sxs-lookup"><span data-stu-id="41239-209">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="41239-210">強型別方法可讓程式碼的編譯時期檢查變得更佳。</span><span class="sxs-lookup"><span data-stu-id="41239-210">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="41239-211">Scaffolding 機制在建立方法和檢視時，已使用此方法 (也就是傳遞強型別模型) 來搭配 `MoviesController` 類別和檢視。</span><span class="sxs-lookup"><span data-stu-id="41239-211">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="41239-212">檢查 *Controllers/MoviesController.cs* 檔案中產生的 `Details` 方法：</span><span class="sxs-lookup"><span data-stu-id="41239-212">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="41239-213">`id` 參數通常會傳遞為路由資料。</span><span class="sxs-lookup"><span data-stu-id="41239-213">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="41239-214">例如，`https://localhost:5001/movies/details/1` 設定：</span><span class="sxs-lookup"><span data-stu-id="41239-214">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="41239-215">控制器為 `movies` 控制器 (第一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="41239-215">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="41239-216">動作為 `details` (第二個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="41239-216">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="41239-217">識別碼為 1 (最後一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="41239-217">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="41239-218">您也可以在 `id` 中使用查詢字串傳遞，如下所示：</span><span class="sxs-lookup"><span data-stu-id="41239-218">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="41239-219">若未提供識別碼值，`id` 參數會定義為[可為 Null 的型別](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`)。</span><span class="sxs-lookup"><span data-stu-id="41239-219">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="41239-220">[Lambda 運算式](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)會傳遞至 `FirstOrDefaultAsync`，以選取符合路由資料或查詢字串值的電影實體。</span><span class="sxs-lookup"><span data-stu-id="41239-220">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="41239-221">如果找到電影，則 `Movie` 模型的執行個體會傳遞至 `Details` 檢視：</span><span class="sxs-lookup"><span data-stu-id="41239-221">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="41239-222">檢查 *Views/Movies/Details.cshtml* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="41239-222">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="41239-223">藉由在檢視檔案的最上方包含 `@model` 陳述式，您可以指定檢視預期要有的物件類型。</span><span class="sxs-lookup"><span data-stu-id="41239-223">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="41239-224">當您建立電影控制器時，*Details.cshtml* 檔案的最上方會自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="41239-224">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="41239-225">這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。</span><span class="sxs-lookup"><span data-stu-id="41239-225">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="41239-226">例如，在 *Details.cshtml* 檢視中，程式碼會使用強型別的 `Model` 物件，將每個電影欄位傳遞至 `DisplayNameFor` 和 `DisplayFor` HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="41239-226">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="41239-227">`Create` 與 `Edit` 方法和檢視也會傳遞 `Movie` 模型物件。</span><span class="sxs-lookup"><span data-stu-id="41239-227">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="41239-228">檢查電影控制器中的 *Index.cshtml* 檢視和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="41239-228">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="41239-229">請注意程式碼如何在呼叫 `View` 方法時建立 `List` 物件。</span><span class="sxs-lookup"><span data-stu-id="41239-229">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="41239-230">此程式碼會從 `Index` 動作方法將 `Movies` 清單傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="41239-230">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="41239-231">當您建立電影控制器時，Scaffolding 會在 *Index.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="41239-231">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="41239-232">`@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影清單。</span><span class="sxs-lookup"><span data-stu-id="41239-232">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="41239-233">例如，在 *Index.cshtml* 檢視中，程式碼會透過強型別 `Model` 物件的 `foreach` 陳述式循環存取電影：</span><span class="sxs-lookup"><span data-stu-id="41239-233">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="41239-234">因為 `Model` 物件是強型別 (作為 `IEnumerable<Movie>` 物件)，所以迴圈中每個項目的類型為 `Movie`。</span><span class="sxs-lookup"><span data-stu-id="41239-234">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="41239-235">撇開其他優點，這表示您會進行程式碼編譯時期檢查：</span><span class="sxs-lookup"><span data-stu-id="41239-235">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41239-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="41239-236">Additional resources</span></span>

* [<span data-ttu-id="41239-237">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="41239-237">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="41239-238">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="41239-238">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="41239-239">[上一步：新增檢視](adding-view.md)
> [下一步：使用 SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="41239-239">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>
