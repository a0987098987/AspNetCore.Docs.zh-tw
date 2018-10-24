---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 5cd1e08ac52d352be23a280419d7456f685a03ad
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045597"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="ce454-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="ce454-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ce454-104">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="ce454-104">Add a data model</span></span>

<span data-ttu-id="ce454-105">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ce454-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="ce454-106">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="ce454-106">Name the folder *Models*.</span></span>

<span data-ttu-id="ce454-107">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce454-107">Right click the *Models* folder.</span></span> <span data-ttu-id="ce454-108">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="ce454-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="ce454-109">將類別命名為 **Movie**，並以下列程式碼取代 `Movie` 類別的內容：</span><span class="sxs-lookup"><span data-stu-id="ce454-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="ce454-110">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="ce454-110">Scaffold the movie model</span></span>

<span data-ttu-id="ce454-111">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="ce454-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="ce454-112">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="ce454-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="ce454-113">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="ce454-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="ce454-114">在 [方案總管] 中，以滑鼠右鍵按一下 *Pages* 資料夾 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ce454-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="ce454-115">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="ce454-115">Name the folder *Movies*</span></span>

<span data-ttu-id="ce454-116">在 [方案總管] 中，以滑鼠右鍵按一下 *Pages/Movies* 資料夾 > [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="ce454-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="ce454-118">在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)] > [新增]。</span><span class="sxs-lookup"><span data-stu-id="ce454-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="ce454-120">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="ce454-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="ce454-121">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="ce454-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="ce454-122">在 [資料內容類別] 列中選取 [+] (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="ce454-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="ce454-123">在 [資料內容類別] 下拉式清單中，選取 [RazorPagesMovie.Models.RazorPagesMovieContext]</span><span class="sxs-lookup"><span data-stu-id="ce454-123">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="ce454-124">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="ce454-124">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="ce454-126">隨即建立 Scaffold 處理序並變更下列檔案：</span><span class="sxs-lookup"><span data-stu-id="ce454-126">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="ce454-127">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="ce454-127">Files created</span></span>

* <span data-ttu-id="ce454-128">*Pages/Movies*：建立、刪除、詳細資料、編輯、索引。</span><span class="sxs-lookup"><span data-stu-id="ce454-128">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="ce454-129">下一個教學課程會詳述這些頁面。</span><span class="sxs-lookup"><span data-stu-id="ce454-129">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="ce454-130">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="ce454-130">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="ce454-131">檔案更新</span><span class="sxs-lookup"><span data-stu-id="ce454-131">File updates</span></span>

* <span data-ttu-id="ce454-132">*Startup.cs*：這個檔案的變更會於下一節中詳述。</span><span class="sxs-lookup"><span data-stu-id="ce454-132">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="ce454-133">*appsettings.json*：已新增用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="ce454-133">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="ce454-134">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="ce454-134">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="ce454-135">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="ce454-135">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ce454-136">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="ce454-136">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ce454-137">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="ce454-137">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="ce454-138">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="ce454-138">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="ce454-139">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="ce454-139">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="ce454-140">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="ce454-140">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ce454-141">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="ce454-141">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="ce454-142">為給定的資料模型協調 EF Core 功能的主要類別是資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="ce454-142">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="ce454-143">資料內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="ce454-143">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="ce454-144">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="ce454-144">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="ce454-145">在此專案中，類別命名為 `RazorPagesMovieContext`。</span><span class="sxs-lookup"><span data-stu-id="ce454-145">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="ce454-146">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="ce454-146">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="ce454-147">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="ce454-147">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="ce454-148">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="ce454-148">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="ce454-149">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="ce454-149">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="ce454-150">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="ce454-150">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="ce454-151">執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="ce454-151">Perform initial migration</span></span>

<span data-ttu-id="ce454-152">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="ce454-152">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="ce454-153">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="ce454-153">Add an initial migration.</span></span>
* <span data-ttu-id="ce454-154">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="ce454-154">Update the database with the initial migration.</span></span>

<span data-ttu-id="ce454-155">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="ce454-155">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="ce454-157">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ce454-157">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="ce454-158">或者，您可以從專案資料夾使用下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="ce454-158">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="ce454-159">請略過下列警告訊息，您將在稍後的教學課程中修正該問題：</span><span class="sxs-lookup"><span data-stu-id="ce454-159">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="ce454-160">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="ce454-160">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="ce454-161">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="ce454-161">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="ce454-162">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="ce454-162">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="ce454-163">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="ce454-163">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="ce454-164">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="ce454-164">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="ce454-165">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="ce454-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="ce454-166">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="ce454-166">If you get the error:</span></span>

`SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.`

<span data-ttu-id="ce454-167">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="ce454-167">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ce454-168">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="ce454-168">Add a data model</span></span>

<span data-ttu-id="ce454-169">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ce454-169">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="ce454-170">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="ce454-170">Name the folder *Models*.</span></span>

<span data-ttu-id="ce454-171">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce454-171">Right click the *Models* folder.</span></span> <span data-ttu-id="ce454-172">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="ce454-172">Select **Add** > **Class**.</span></span> <span data-ttu-id="ce454-173">將類別命名為 **Movie** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="ce454-173">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="ce454-174">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="ce454-174">Add a database connection string</span></span>

<span data-ttu-id="ce454-175">將連線字串新增到 *appsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="ce454-175">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="ce454-176">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="ce454-176">Register the database context</span></span>

<span data-ttu-id="ce454-177">用 [Startup 類別的 ConfigureServices 方法](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) 向[相依性插入](xref:fundamentals/dependency-injection)容器註冊資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="ce454-177">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="ce454-178">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="ce454-178">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="ce454-179">新增 Scaffold 工具並執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="ce454-179">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="ce454-180">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="ce454-180">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="ce454-181">新增 Visual Studio Web 程式碼產生套件。</span><span class="sxs-lookup"><span data-stu-id="ce454-181">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="ce454-182">執行 Scaffolding 引擎需要此套件。</span><span class="sxs-lookup"><span data-stu-id="ce454-182">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="ce454-183">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="ce454-183">Add an initial migration.</span></span>
* <span data-ttu-id="ce454-184">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="ce454-184">Update the database with the initial migration.</span></span>

<span data-ttu-id="ce454-185">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="ce454-185">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="ce454-187">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ce454-187">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="ce454-188">或者，您可以使用下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="ce454-188">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="ce454-189">請略過下列訊息：</span><span class="sxs-lookup"><span data-stu-id="ce454-189">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="ce454-190">您將在接下來的教學課程中修正該問題。</span><span class="sxs-lookup"><span data-stu-id="ce454-190">You fix that in the next tutorial.</span></span>

<span data-ttu-id="ce454-191">`Install-Package` 命令會安裝執行 Scaffolding 引擎所需的工具。</span><span class="sxs-lookup"><span data-stu-id="ce454-191">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="ce454-192">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="ce454-192">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="ce454-193">結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="ce454-193">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="ce454-194">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="ce454-194">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="ce454-195">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="ce454-195">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="ce454-196">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="ce454-196">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="ce454-197">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="ce454-197">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="ce454-198">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="ce454-198">Test the app</span></span>

* <span data-ttu-id="ce454-199">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="ce454-199">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="ce454-200">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="ce454-200">Test the **Create** link.</span></span>

  ![建立頁面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="ce454-202">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="ce454-202">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ce454-203">如果您收到 SQL 例外狀況，請確認您已執行移轉並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="ce454-203">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="ce454-204">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="ce454-204">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ce454-205">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="ce454-205">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
