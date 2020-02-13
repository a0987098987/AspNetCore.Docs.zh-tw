---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: d2f9a64c77d76702004b94cdf36e558b33d7e19a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172568"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="3c6d1-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="3c6d1-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="3c6d1-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3c6d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="3c6d1-105">在本節中，會加入類別來管理跨平臺[SQLite 資料庫](https://www.sqlite.org/index.html)中的電影。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="3c6d1-106">從 ASP.NET Core 範本建立的應用程式會使用 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="3c6d1-107">應用程式的模型類別會與[Entity Framework Core （EF Core）](/ef/core) （[SQLite EF Core 資料庫提供者](/ef/core/providers/sqlite)）搭配使用，以使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="3c6d1-108">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="3c6d1-109">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="3c6d1-110">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="3c6d1-111">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="3c6d1-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c6d1-113">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="3c6d1-114">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-114">Name the folder *Models*.</span></span>

<span data-ttu-id="3c6d1-115">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-115">Right click the *Models* folder.</span></span> <span data-ttu-id="3c6d1-116">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="3c6d1-117">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c6d1-119">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="3c6d1-120">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3c6d1-122">在 Solution Pad 中，以滑鼠右鍵按一下**RazorPagesMovie**專案，然後選取 [**加入**>**新增資料夾**]。將資料夾命名為*模型*。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="3c6d1-123">以滑鼠右鍵按一下 [*模型*] 資料夾，然後選取 [**加入**>**新增**檔案 ...]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="3c6d1-124">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="3c6d1-125">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="3c6d1-126">在中央窗格中選取 [類別是空的]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="3c6d1-127">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="3c6d1-128">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="3c6d1-129">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="3c6d1-129">Scaffold the movie model</span></span>

<span data-ttu-id="3c6d1-130">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="3c6d1-131">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c6d1-133">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c6d1-134">在 *頁面* 資料夾上按一下滑鼠右鍵，>**加入**>**新增資料夾**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c6d1-135">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-135">Name the folder *Movies*</span></span>

<span data-ttu-id="3c6d1-136">以滑鼠右鍵按一下  *Pages/電影* 資料夾 **，> 新增**>**新增 scaffold 專案**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="3c6d1-138">在 [**新增 Scaffold** ] 對話方塊中，選取 [ **Razor Pages 使用 Entity Framework （CRUD）** ] > [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="3c6d1-140">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="3c6d1-141">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="3c6d1-142">在 [資料內容類別] 列中選取 [ **]+** \(加號\)，並將產生的名稱 RazorPagesMovie.**Models**.RazorPagesMovieContext 變更為 RazorPagesMovie.**Data**.RazorPagesMovieContext。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="3c6d1-143">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="3c6d1-144">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="3c6d1-145">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-145">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/3/arp.png)

<span data-ttu-id="3c6d1-147">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="3c6d1-149">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3c6d1-150">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="3c6d1-151">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="3c6d1-152">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-153">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c6d1-154">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c6d1-155">在 *頁面* 資料夾上按一下滑鼠右鍵，>**加入**>**新增資料夾**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c6d1-156">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-156">Name the folder *Movies*</span></span>

<span data-ttu-id="3c6d1-157">以滑鼠右鍵按一下 [ *Pages/電影*] 資料夾 >**加入**>**新**的 [樣板]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![前述指示中的圖片。](model/_static/scaMac.png)

<span data-ttu-id="3c6d1-159">在 **新**的架構 對話方塊中，選取  **Razor Pages 使用 Entity Framework （CRUD）**  **> 下一步**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

<span data-ttu-id="3c6d1-161">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="3c6d1-162">在 [**模型類別**] 下拉式選，選取或輸入**Movie （RazorPagesMovie）** 。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="3c6d1-163">在 [**資料內容類別**] 列中，輸入新類別的名稱 RazorPagesMovie。**資料**。RazorpagesmoviecoNtext-21.</span><span class="sxs-lookup"><span data-stu-id="3c6d1-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="3c6d1-164">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="3c6d1-165">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="3c6d1-166">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-166">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arpMac.png)

<span data-ttu-id="3c6d1-168">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="3c6d1-169">新增 EF 工具</span><span class="sxs-lookup"><span data-stu-id="3c6d1-169">Add EF tools</span></span>

<span data-ttu-id="3c6d1-170">執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="3c6d1-171">上述命令會加入 .NET Core CLI 的 Entity Framework Core 工具。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="3c6d1-172">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="3c6d1-172">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c6d1-174">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="3c6d1-175">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="3c6d1-176">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="3c6d1-177">已更新</span><span class="sxs-lookup"><span data-stu-id="3c6d1-177">Updated</span></span>

* <span data-ttu-id="3c6d1-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-178">*Startup.cs*</span></span>

<span data-ttu-id="3c6d1-179">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-180">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c6d1-181">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="3c6d1-182">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="3c6d1-183">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="3c6d1-184">已更新</span><span class="sxs-lookup"><span data-stu-id="3c6d1-184">Updated</span></span>

* <span data-ttu-id="3c6d1-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-185">*Startup.cs*</span></span>

<span data-ttu-id="3c6d1-186">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c6d1-188">Scaffold 處理序會建立下列檔案：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="3c6d1-189">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="3c6d1-190">下一節將說明所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="3c6d1-191">初始移轉</span><span class="sxs-lookup"><span data-stu-id="3c6d1-191">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c6d1-193">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="3c6d1-194">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-194">Add an initial migration.</span></span>
* <span data-ttu-id="3c6d1-195">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="3c6d1-196">從 [**工具**] 功能表中，選取 [ **NuGet 套件管理員**] > [**套件管理員主控台**]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="3c6d1-198">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-200">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="3c6d1-201">上述命令會產生下列警告：「實體類型 ' Movie ' 的十進位資料行 ' Price ' 未指定任何類型。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="3c6d1-202">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="3c6d1-203">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="3c6d1-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="3c6d1-204">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="3c6d1-205">[遷移] 命令會產生程式碼來建立初始資料庫架構。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="3c6d1-206">架構是以 `DbContext`中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="3c6d1-207">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="3c6d1-208">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="3c6d1-209">`update` 命令會在尚未套用的遷移中執行 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="3c6d1-210">在此情況下，`update` 會在建立資料庫的「*遷移/\<時間戳記 > _InitialCreate .cs*檔案中執行 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="3c6d1-212">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="3c6d1-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="3c6d1-213">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3c6d1-214">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3c6d1-215">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="3c6d1-216">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="3c6d1-217">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="3c6d1-218">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3c6d1-219">強調顯示的行由 Scaffolder 新增：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="3c6d1-220">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="3c6d1-221">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="3c6d1-222">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="3c6d1-223">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="3c6d1-224">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="3c6d1-225">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="3c6d1-226">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="3c6d1-227">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c6d1-229">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-230">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c6d1-231">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="3c6d1-232">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="3c6d1-232">Test the app</span></span>

* <span data-ttu-id="3c6d1-233">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="3c6d1-234">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="3c6d1-235">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="3c6d1-236">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-236">Test the **Create** link.</span></span>

  ![[建立] 頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="3c6d1-238">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="3c6d1-239">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="3c6d1-240">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-240">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="3c6d1-241">測試 [編輯]、[詳細資料] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="3c6d1-242">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c6d1-243">其他資源</span><span class="sxs-lookup"><span data-stu-id="3c6d1-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c6d1-244">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="3c6d1-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3c6d1-245">在本節中，會加入類別來管理跨平臺[SQLite 資料庫](https://www.sqlite.org/index.html)中的電影。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="3c6d1-246">從 ASP.NET Core 範本建立的應用程式會使用 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="3c6d1-247">應用程式的模型類別會與[Entity Framework Core （EF Core）](/ef/core) （[SQLite EF Core 資料庫提供者](/ef/core/providers/sqlite)）搭配使用，以使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="3c6d1-248">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="3c6d1-249">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="3c6d1-250">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="3c6d1-251">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="3c6d1-251">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c6d1-253">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="3c6d1-254">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-254">Name the folder *Models*.</span></span>

<span data-ttu-id="3c6d1-255">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-255">Right click the *Models* folder.</span></span> <span data-ttu-id="3c6d1-256">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="3c6d1-257">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c6d1-259">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="3c6d1-260">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-261">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3c6d1-262">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="3c6d1-263">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="3c6d1-264">以滑鼠右鍵按一下 [*模型*] 資料夾，然後選取 [**加入**>**新增**檔案]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="3c6d1-265">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="3c6d1-266">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="3c6d1-267">在中央窗格中選取 [類別是空的]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="3c6d1-268">將類別命名為 **Movie**，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="3c6d1-269">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="3c6d1-270">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="3c6d1-270">Scaffold the movie model</span></span>

<span data-ttu-id="3c6d1-271">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="3c6d1-272">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c6d1-274">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c6d1-275">在 *頁面* 資料夾上按一下滑鼠右鍵，>**加入**>**新增資料夾**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c6d1-276">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-276">Name the folder *Movies*</span></span>

<span data-ttu-id="3c6d1-277">以滑鼠右鍵按一下  *Pages/電影* 資料夾 **，> 新增**>**新增 scaffold 專案**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="3c6d1-279">在 [**新增 Scaffold** ] 對話方塊中，選取 [ **Razor Pages 使用 Entity Framework （CRUD）** ] > [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="3c6d1-281">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="3c6d1-282">在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="3c6d1-283">在 [資料內容類別] 列中選取 [ **]+** (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="3c6d1-284">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-284">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="3c6d1-286">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="3c6d1-288">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="3c6d1-289">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="3c6d1-290">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-291">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c6d1-292">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="3c6d1-293">在 *頁面* 資料夾上按一下滑鼠右鍵，>**加入**>**新增資料夾**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="3c6d1-294">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-294">Name the folder *Movies*</span></span>

<span data-ttu-id="3c6d1-295">以滑鼠右鍵按一下  *Pages/電影* 資料夾 **，> 新增**>**新增 scaffold 專案**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/scaMac.png)

<span data-ttu-id="3c6d1-297">在 [**加入新**的架構] 對話方塊中，選取 [ **Razor Pages 使用 Entity Framework （CRUD）** ] > [**新增**]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

<span data-ttu-id="3c6d1-299">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="3c6d1-300">在 [**模型類別**] 下拉式選，選取或輸入**Movie**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="3c6d1-301">在 [**資料內容類別**] 列中，輸入選取**razorpagesmoviecoNtext-21** ，這會使用正確的命名空間建立新的 db 內容類別。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="3c6d1-302">在此情況下，它會是**RazorPagesMovie。 razorpagesmoviecoNtext-21**。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="3c6d1-303">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-303">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arpMac.png)

<span data-ttu-id="3c6d1-305">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="3c6d1-306">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="3c6d1-307">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="3c6d1-307">Files created</span></span>

* <span data-ttu-id="3c6d1-308">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="3c6d1-309">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="3c6d1-310">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="3c6d1-310">File updated</span></span>

* <span data-ttu-id="3c6d1-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="3c6d1-311">*Startup.cs*</span></span>

<span data-ttu-id="3c6d1-312">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="3c6d1-313">初始移轉</span><span class="sxs-lookup"><span data-stu-id="3c6d1-313">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c6d1-315">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="3c6d1-316">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-316">Add an initial migration.</span></span>
* <span data-ttu-id="3c6d1-317">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="3c6d1-318">從 [**工具**] 功能表中，選取 [ **NuGet 套件管理員**] > [**套件管理員主控台**]。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="3c6d1-320">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="3c6d1-321">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="3c6d1-322">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="3c6d1-323">`InitialCreate` 引數是用來命名遷移。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="3c6d1-324">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="3c6d1-325">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="3c6d1-326">`Update-Database` 命令會執行 `Up`Migrations/*時間戳記>_InitialCreate.cs\< 檔案中的*  方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="3c6d1-327">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-329">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="3c6d1-330">上述命令會產生下列警告：「*實體類型 ' Movie ' 的十進位資料行 ' Price ' 未指定任何類型。如果值不符合預設的有效位數和小數位數，這會導致以無訊息模式截斷值。使用 ' HasColumnType （） ' 明確指定可容納所有值的 SQL server 資料行類型。* 」您可以忽略該警告，其將在稍後的教學課程中修正。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c6d1-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c6d1-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="3c6d1-332">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="3c6d1-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="3c6d1-333">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3c6d1-334">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3c6d1-335">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="3c6d1-336">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="3c6d1-337">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="3c6d1-338">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3c6d1-339">強調顯示的行由 Scaffolder 新增：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="3c6d1-340">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="3c6d1-341">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="3c6d1-342">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="3c6d1-343">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="3c6d1-344">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="3c6d1-345">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="3c6d1-346">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="3c6d1-347">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c6d1-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c6d1-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c6d1-349">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c6d1-350">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3c6d1-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c6d1-351">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="3c6d1-352">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="3c6d1-352">Test the app</span></span>

* <span data-ttu-id="3c6d1-353">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="3c6d1-354">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="3c6d1-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="3c6d1-355">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="3c6d1-356">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-356">Test the **Create** link.</span></span>

  ![[建立] 頁面](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="3c6d1-358">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="3c6d1-359">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="3c6d1-360">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-360">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="3c6d1-361">測試 [編輯]、[詳細資料] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="3c6d1-362">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c6d1-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c6d1-363">其他資源</span><span class="sxs-lookup"><span data-stu-id="3c6d1-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c6d1-364">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="3c6d1-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
