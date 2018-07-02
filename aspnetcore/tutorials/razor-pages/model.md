---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327548"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="59fcc-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="59fcc-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="59fcc-104">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="59fcc-104">Add a data model</span></span>

<span data-ttu-id="59fcc-105">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="59fcc-106">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="59fcc-106">Name the folder *Models*.</span></span>

<span data-ttu-id="59fcc-107">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="59fcc-107">Right click the *Models* folder.</span></span> <span data-ttu-id="59fcc-108">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="59fcc-109">將類別命名為 **Movie** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="59fcc-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="59fcc-110">以下列程式碼取代 `Movie` 類別的內容：</span><span class="sxs-lookup"><span data-stu-id="59fcc-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="59fcc-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="59fcc-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="59fcc-112">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="59fcc-112">Scaffold the movie model</span></span>

<span data-ttu-id="59fcc-113">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="59fcc-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="59fcc-114">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="59fcc-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="59fcc-115">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="59fcc-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="59fcc-116">在 [方案總管] 中，以滑鼠右鍵按一下 *Pages* 資料夾 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="59fcc-117">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="59fcc-117">Name the folder *Movies*</span></span>

<span data-ttu-id="59fcc-118">在 [方案總管] 中，以滑鼠右鍵按一下 *Pages/Movies* 資料夾 > [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="59fcc-120">在 [新增 Scaffold] 對話方塊中，選取 [Razor Pages using Entity Framework (CRUD)] \(使用 Entity Framework 的 Razor Pages (CRUD)\) > [新增]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="59fcc-122">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="59fcc-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="59fcc-123">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="59fcc-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="59fcc-124">在 [資料內容類別] 列中選取 [+] (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="59fcc-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="59fcc-125">在 [資料內容類別] 下拉式清單中，選取 [RazorPagesMovie.Models.RazorPagesMovieContext]</span><span class="sxs-lookup"><span data-stu-id="59fcc-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="59fcc-126">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-126">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="59fcc-128">執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="59fcc-128">Perform initial migration</span></span>

<span data-ttu-id="59fcc-129">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="59fcc-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="59fcc-130">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="59fcc-130">Add an initial migration.</span></span>
* <span data-ttu-id="59fcc-131">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="59fcc-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="59fcc-132">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="59fcc-134">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="59fcc-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="59fcc-135">或者，您可以使用下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="59fcc-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="59fcc-136">請略過下列警告訊息，您將在接下來的教學課程中修正該問題：</span><span class="sxs-lookup"><span data-stu-id="59fcc-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="59fcc-137">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="59fcc-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="59fcc-138">結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="59fcc-138">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="59fcc-139">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="59fcc-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="59fcc-140">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="59fcc-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="59fcc-141">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="59fcc-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="59fcc-142">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="59fcc-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="59fcc-143">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="59fcc-143">If you get the error:</span></span>

<span data-ttu-id="59fcc-144">SqlException：無法開啟登入要求的 "RazorPagesMovieContext-GUID" 資料庫。</span><span class="sxs-lookup"><span data-stu-id="59fcc-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="59fcc-145">登入失敗。</span><span class="sxs-lookup"><span data-stu-id="59fcc-145">The login failed.</span></span>
<span data-ttu-id="59fcc-146">使用者 'User-name' 登入失敗。</span><span class="sxs-lookup"><span data-stu-id="59fcc-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="59fcc-147">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="59fcc-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="59fcc-148">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="59fcc-148">Add a data model</span></span>

<span data-ttu-id="59fcc-149">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="59fcc-150">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="59fcc-150">Name the folder *Models*.</span></span>

<span data-ttu-id="59fcc-151">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="59fcc-151">Right click the *Models* folder.</span></span> <span data-ttu-id="59fcc-152">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="59fcc-153">將類別命名為 **Movie** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="59fcc-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="59fcc-154">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="59fcc-154">Add a database connection string</span></span>

<span data-ttu-id="59fcc-155">將連線字串新增到 *appsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="59fcc-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="59fcc-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="59fcc-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="59fcc-157">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="59fcc-157">Register the database context</span></span>

<span data-ttu-id="59fcc-158">用 [Startup 類別的 ConfigureServices 方法](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) 向[相依性插入](xref:fundamentals/dependency-injection)容器註冊資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="59fcc-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="59fcc-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="59fcc-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="59fcc-160">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="59fcc-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="59fcc-161">新增 Scaffold 工具並執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="59fcc-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="59fcc-162">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="59fcc-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="59fcc-163">新增 Visual Studio Web 程式碼產生套件。</span><span class="sxs-lookup"><span data-stu-id="59fcc-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="59fcc-164">執行 Scaffolding 引擎需要此套件。</span><span class="sxs-lookup"><span data-stu-id="59fcc-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="59fcc-165">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="59fcc-165">Add an initial migration.</span></span>
* <span data-ttu-id="59fcc-166">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="59fcc-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="59fcc-167">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="59fcc-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="59fcc-169">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="59fcc-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="59fcc-170">或者，您可以使用下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="59fcc-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="59fcc-171">請略過下列訊息：</span><span class="sxs-lookup"><span data-stu-id="59fcc-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="59fcc-172">您將在接下來的教學課程中修正該問題。</span><span class="sxs-lookup"><span data-stu-id="59fcc-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="59fcc-173">`Install-Package` 命令會安裝執行 Scaffolding 引擎所需的工具。</span><span class="sxs-lookup"><span data-stu-id="59fcc-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="59fcc-174">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="59fcc-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="59fcc-175">結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="59fcc-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="59fcc-176">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="59fcc-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="59fcc-177">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="59fcc-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="59fcc-178">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="59fcc-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="59fcc-179">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="59fcc-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="59fcc-180">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="59fcc-180">Test the app</span></span>

* <span data-ttu-id="59fcc-181">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="59fcc-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="59fcc-182">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="59fcc-182">Test the **Create** link.</span></span>

  ![建立頁面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="59fcc-184">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="59fcc-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="59fcc-185">如果您收到 SQL 例外狀況，請確認您已執行移轉並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="59fcc-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="59fcc-186">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="59fcc-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="59fcc-187">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="59fcc-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
