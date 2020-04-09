---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658932"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="16e98-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="16e98-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="16e98-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16e98-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="16e98-105">在本節中,將添加類,用於在跨平臺[SQLite 資料庫中](https://www.sqlite.org/index.html)管理影片。</span><span class="sxs-lookup"><span data-stu-id="16e98-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="16e98-106">從ASP.NET核心範本創建的應用使用SQLite資料庫。</span><span class="sxs-lookup"><span data-stu-id="16e98-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="16e98-107">該應用程式的模型類與[實體框架核心 (EF Core)](/ef/core) [(SQLite EF 核心資料庫提供者](/ef/core/providers/sqlite)) 一起使用,以便與資料庫一起使用。</span><span class="sxs-lookup"><span data-stu-id="16e98-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="16e98-108">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="16e98-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="16e98-109">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="16e98-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="16e98-110">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="16e98-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="16e98-111">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="16e98-111">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16e98-113">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]\*\*\*\* > [新增資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="16e98-114">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="16e98-114">Name the folder *Models*.</span></span>

<span data-ttu-id="16e98-115">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="16e98-115">Right click the *Models* folder.</span></span> <span data-ttu-id="16e98-116">選擇 **「添加** > **類**」。</span><span class="sxs-lookup"><span data-stu-id="16e98-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="16e98-117">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="16e98-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="16e98-119">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="16e98-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="16e98-120">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="16e98-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="16e98-122">在解決方案墊中,右鍵單擊**RazorPagesMovie**專案**Add**,然後>選擇"**添加新資料夾..."** 命名資料夾*模型*。</span><span class="sxs-lookup"><span data-stu-id="16e98-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="16e98-123">右鍵按下 *"模型"* 資料夾,**Add**>然後選擇 「**添加新檔..."。**</span><span class="sxs-lookup"><span data-stu-id="16e98-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="16e98-124">在 [新增檔案]\*\*\*\* 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="16e98-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="16e98-125">在左窗格中選取 [一般]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="16e98-126">在中央窗格中選取 [類別是空的]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="16e98-127">將類別命名為 **Movie**，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="16e98-128">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="16e98-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="16e98-129">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="16e98-129">Scaffold the movie model</span></span>

<span data-ttu-id="16e98-130">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="16e98-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="16e98-131">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="16e98-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16e98-133">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="16e98-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="16e98-134">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="16e98-135">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="16e98-135">Name the folder *Movies*</span></span>

<span data-ttu-id="16e98-136">以滑鼠右鍵按一下 [Pages/Movies]\*\* 資料夾 > [新增]\*\* [新增 Scaffolded 項目]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="16e98-138">在 [新增 Scaffold]\*\*\*\* 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]\*\* [新增]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="16e98-140">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="16e98-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="16e98-141">在 [模型類別]\*\*\*\* 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="16e98-142">在 [資料內容類別]\*\*\*\* 列中選取 [+]\*\*\*\* \(加號\)，並將產生的名稱 RazorPagesMovie.**Models**.RazorPagesMovieContext 變更為 RazorPagesMovie.**Data**.RazorPagesMovieContext。</span><span class="sxs-lookup"><span data-stu-id="16e98-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="16e98-143">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="16e98-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="16e98-144">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="16e98-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="16e98-145">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="16e98-145">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/3/arp.png)

<span data-ttu-id="16e98-147">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="16e98-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="16e98-149">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="16e98-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="16e98-150">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="16e98-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="16e98-151">**對於 Windows**:執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="16e98-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="16e98-152">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="16e98-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-153">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="16e98-154">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="16e98-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="16e98-155">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="16e98-156">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="16e98-156">Name the folder *Movies*</span></span>

<span data-ttu-id="16e98-157">右鍵單擊 *"頁面/電影*"資料夾 **>添加新**>**基架..."**</span><span class="sxs-lookup"><span data-stu-id="16e98-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![前述指示中的圖片。](model/_static/scaMac.png)

<span data-ttu-id="16e98-159">在 **"新建基架"對話框中**,選擇 **"使用實體框架 (CRUD)** > **下一步**剃刀頁面"。</span><span class="sxs-lookup"><span data-stu-id="16e98-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

<span data-ttu-id="16e98-161">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="16e98-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="16e98-162">在**模型"類**中,下拉、選擇或鍵入**影片(RazorPagesMovie.Model)。**</span><span class="sxs-lookup"><span data-stu-id="16e98-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="16e98-163">在 **「數據上下文」類**行中,鍵入新類 RazorPagesMovie 的名稱。**資料**。剃刀頁電影上下文。</span><span class="sxs-lookup"><span data-stu-id="16e98-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="16e98-164">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="16e98-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="16e98-165">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="16e98-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="16e98-166">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="16e98-166">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arpMac.png)

<span data-ttu-id="16e98-168">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="16e98-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="16e98-169">新增 EF 工具</span><span class="sxs-lookup"><span data-stu-id="16e98-169">Add EF tools</span></span>

<span data-ttu-id="16e98-170">執行以下 .NET 核心 CLI 指令:</span><span class="sxs-lookup"><span data-stu-id="16e98-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="16e98-171">前面的命令為 .NET 核心 CLI 添加了實體框架核心工具。</span><span class="sxs-lookup"><span data-stu-id="16e98-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="16e98-172">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="16e98-172">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16e98-174">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="16e98-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="16e98-175">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="16e98-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="16e98-176">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="16e98-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="16e98-177">已更新</span><span class="sxs-lookup"><span data-stu-id="16e98-177">Updated</span></span>

* <span data-ttu-id="16e98-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="16e98-178">*Startup.cs*</span></span>

<span data-ttu-id="16e98-179">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="16e98-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-180">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="16e98-181">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="16e98-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="16e98-182">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="16e98-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="16e98-183">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="16e98-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="16e98-184">已更新</span><span class="sxs-lookup"><span data-stu-id="16e98-184">Updated</span></span>

* <span data-ttu-id="16e98-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="16e98-185">*Startup.cs*</span></span>

<span data-ttu-id="16e98-186">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="16e98-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="16e98-188">Scaffold 處理序會建立下列檔案：</span><span class="sxs-lookup"><span data-stu-id="16e98-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="16e98-189">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="16e98-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="16e98-190">下一節將說明所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="16e98-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="16e98-191">初始移轉</span><span class="sxs-lookup"><span data-stu-id="16e98-191">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16e98-193">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="16e98-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="16e98-194">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="16e98-194">Add an initial migration.</span></span>
* <span data-ttu-id="16e98-195">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="16e98-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="16e98-196">從 [工具]\*\*\*\* 功能表中，選取 [NuGet 封裝管理員]**[封裝管理員主控台]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="16e98-198">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="16e98-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-200">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="16e98-201">前面的命令生成以下警告:「未為實體類型」Movie"上的十進位列"價格"指定類型。</span><span class="sxs-lookup"><span data-stu-id="16e98-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="16e98-202">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="16e98-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="16e98-203">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="16e98-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="16e98-204">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="16e98-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="16e98-205">遷移命令生成代碼以創建初始資料庫架構。</span><span class="sxs-lookup"><span data-stu-id="16e98-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="16e98-206">架構基於`DbContext`中 指定的模型。</span><span class="sxs-lookup"><span data-stu-id="16e98-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="16e98-207">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="16e98-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="16e98-208">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="16e98-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="16e98-209">該`update`命令`Up`在尚未應用的遷移中運行該方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="16e98-210">在這種情況下,`update``Up`在*建立資料庫的移轉\</ 時間戳>_InitialCreate.cs*檔中執行該方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="16e98-212">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="16e98-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="16e98-213">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="16e98-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="16e98-214">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="16e98-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="16e98-215">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="16e98-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="16e98-216">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="16e98-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="16e98-217">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="16e98-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="16e98-218">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="16e98-219">強調顯示的行由 Scaffolder 新增：</span><span class="sxs-lookup"><span data-stu-id="16e98-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="16e98-220">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="16e98-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="16e98-221">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="16e98-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="16e98-222">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="16e98-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="16e98-223">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="16e98-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="16e98-224">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="16e98-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="16e98-225">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="16e98-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="16e98-226">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="16e98-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="16e98-227">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="16e98-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="16e98-229">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-230">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="16e98-231">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="16e98-232">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="16e98-232">Test the app</span></span>

* <span data-ttu-id="16e98-233">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="16e98-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="16e98-234">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="16e98-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="16e98-235">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="16e98-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="16e98-236">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="16e98-236">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="16e98-238">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="16e98-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="16e98-239">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="16e98-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="16e98-240">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="16e98-240">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="16e98-241">測試 [編輯]\*\*\*\*、[詳細資料]\*\*\*\* 和 [刪除]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="16e98-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="16e98-242">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="16e98-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16e98-243">其他資源</span><span class="sxs-lookup"><span data-stu-id="16e98-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16e98-244">[上一篇:](xref:tutorials/razor-pages/razor-pages-start)
> [開始下一步: 腳手架剃刀頁](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="16e98-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="16e98-245">在本節中,將添加類,用於在跨平臺[SQLite 資料庫中](https://www.sqlite.org/index.html)管理影片。</span><span class="sxs-lookup"><span data-stu-id="16e98-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="16e98-246">從ASP.NET核心範本創建的應用使用SQLite資料庫。</span><span class="sxs-lookup"><span data-stu-id="16e98-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="16e98-247">該應用程式的模型類與[實體框架核心 (EF Core)](/ef/core) [(SQLite EF 核心資料庫提供者](/ef/core/providers/sqlite)) 一起使用,以便與資料庫一起使用。</span><span class="sxs-lookup"><span data-stu-id="16e98-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="16e98-248">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="16e98-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="16e98-249">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="16e98-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="16e98-250">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="16e98-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="16e98-251">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="16e98-251">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16e98-253">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]\*\*\*\* > [新增資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="16e98-254">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="16e98-254">Name the folder *Models*.</span></span>

<span data-ttu-id="16e98-255">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="16e98-255">Right click the *Models* folder.</span></span> <span data-ttu-id="16e98-256">選擇 **「添加** > **類**」。</span><span class="sxs-lookup"><span data-stu-id="16e98-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="16e98-257">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="16e98-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="16e98-259">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="16e98-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="16e98-260">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="16e98-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-261">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="16e98-262">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增]\*\*\*\* > [新增資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="16e98-263">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="16e98-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="16e98-264">以滑鼠右鍵按一下 [Models]\*\* 資料夾，然後選取 [新增]\*\* [新增檔案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="16e98-265">在 [新增檔案]\*\*\*\* 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="16e98-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="16e98-266">在左窗格中選取 [一般]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="16e98-267">在中央窗格中選取 [類別是空的]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="16e98-268">將類別命名為 **Movie**，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="16e98-269">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="16e98-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="16e98-270">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="16e98-270">Scaffold the movie model</span></span>

<span data-ttu-id="16e98-271">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="16e98-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="16e98-272">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="16e98-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16e98-274">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="16e98-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="16e98-275">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="16e98-276">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="16e98-276">Name the folder *Movies*</span></span>

<span data-ttu-id="16e98-277">以滑鼠右鍵按一下 [Pages/Movies]\*\* 資料夾 > [新增]\*\* [新增 Scaffolded 項目]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="16e98-279">在 [新增 Scaffold]\*\*\*\* 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]\*\* [新增]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="16e98-281">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="16e98-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="16e98-282">在 [模型類別]\*\*\*\* 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="16e98-283">在 [資料內容類別]\*\*\*\* 列中選取 [+]\*\*\*\* (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="16e98-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="16e98-284">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="16e98-284">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="16e98-286">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="16e98-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="16e98-288">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="16e98-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="16e98-289">**對於 Windows**:執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="16e98-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="16e98-290">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="16e98-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-291">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="16e98-292">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="16e98-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="16e98-293">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="16e98-294">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="16e98-294">Name the folder *Movies*</span></span>

<span data-ttu-id="16e98-295">以滑鼠右鍵按一下 [Pages/Movies]\*\* 資料夾 > [新增]\*\* [新增 Scaffolded 項目]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/scaMac.png)

<span data-ttu-id="16e98-297">在 **「新增新基架」對話框中**,**選擇使用實體框架 (CRUD)** >**添加**的剃刀頁面。</span><span class="sxs-lookup"><span data-stu-id="16e98-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

<span data-ttu-id="16e98-299">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="16e98-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="16e98-300">在**模型"類**下拉清單中,選擇或鍵入 **「影片**」。</span><span class="sxs-lookup"><span data-stu-id="16e98-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="16e98-301">在 **「資料上下文類」行**中,鍵入 **「RazorPages MovieContext」,** 這將創建具有正確命名空間的新 db 上下文類。</span><span class="sxs-lookup"><span data-stu-id="16e98-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="16e98-302">在這種情況下,這將是**RazorPages 電影.模型.RazorPages 電影上下文**。</span><span class="sxs-lookup"><span data-stu-id="16e98-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="16e98-303">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="16e98-303">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arpMac.png)

<span data-ttu-id="16e98-305">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="16e98-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="16e98-306">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="16e98-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="16e98-307">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="16e98-307">Files created</span></span>

* <span data-ttu-id="16e98-308">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="16e98-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="16e98-309">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="16e98-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="16e98-310">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="16e98-310">File updated</span></span>

* <span data-ttu-id="16e98-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="16e98-311">*Startup.cs*</span></span>

<span data-ttu-id="16e98-312">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="16e98-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="16e98-313">初始移轉</span><span class="sxs-lookup"><span data-stu-id="16e98-313">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="16e98-315">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="16e98-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="16e98-316">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="16e98-316">Add an initial migration.</span></span>
* <span data-ttu-id="16e98-317">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="16e98-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="16e98-318">從 [工具]\*\*\*\* 功能表中，選取 [NuGet 封裝管理員]**[封裝管理員主控台]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="16e98-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="16e98-320">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="16e98-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="16e98-321">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="16e98-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="16e98-322">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="16e98-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="16e98-323">參數`InitialCreate`用於命名遷移。</span><span class="sxs-lookup"><span data-stu-id="16e98-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="16e98-324">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="16e98-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="16e98-325">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="16e98-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="16e98-326">`Update-Database` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="16e98-327">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="16e98-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-329">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="16e98-330">前面的命令生成以下警告:"*未為實體類型"Movie"上的十進位列"價格"指定類型。如果值不適合預設精度和比例,則會導致值被靜默截斷。顯式指定 SQL Server 列類型,該類型可以使用"HasColumnType()「容納所有值。* 您可以忽略該警告,它將在後面的教程中修復。</span><span class="sxs-lookup"><span data-stu-id="16e98-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="16e98-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16e98-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="16e98-332">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="16e98-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="16e98-333">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="16e98-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="16e98-334">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="16e98-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="16e98-335">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="16e98-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="16e98-336">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="16e98-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="16e98-337">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="16e98-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="16e98-338">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="16e98-339">強調顯示的行由 Scaffolder 新增：</span><span class="sxs-lookup"><span data-stu-id="16e98-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="16e98-340">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="16e98-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="16e98-341">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="16e98-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="16e98-342">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="16e98-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="16e98-343">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="16e98-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="16e98-344">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="16e98-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="16e98-345">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="16e98-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="16e98-346">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="16e98-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="16e98-347">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="16e98-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="16e98-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="16e98-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="16e98-349">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="16e98-350">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="16e98-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="16e98-351">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="16e98-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="16e98-352">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="16e98-352">Test the app</span></span>

* <span data-ttu-id="16e98-353">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="16e98-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="16e98-354">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="16e98-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="16e98-355">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="16e98-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="16e98-356">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="16e98-356">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="16e98-358">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="16e98-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="16e98-359">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="16e98-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="16e98-360">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="16e98-360">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="16e98-361">測試 [編輯]\*\*\*\*、[詳細資料]\*\*\*\* 和 [刪除]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="16e98-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="16e98-362">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="16e98-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16e98-363">其他資源</span><span class="sxs-lookup"><span data-stu-id="16e98-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16e98-364">[上一篇:](xref:tutorials/razor-pages/razor-pages-start)
> [開始下一步: 腳手架剃刀頁](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="16e98-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
