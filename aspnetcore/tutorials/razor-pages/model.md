---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 7f7c2a09b74e6007ee3ea9c038398bac54988186
ms.sourcegitcommit: 77c046331f3d633d7cc247ba77e58b89e254f487
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81488867"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="79f07-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="79f07-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="79f07-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="79f07-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="79f07-105">在本節中,將添加用於管理影片的類。</span><span class="sxs-lookup"><span data-stu-id="79f07-105">In this section, classes are added for managing movies.</span></span> <span data-ttu-id="79f07-106">應用的模型類使用[實體框架核心 (EF Core)](/ef/core)處理資料庫。</span><span class="sxs-lookup"><span data-stu-id="79f07-106">The app's model classes use [Entity Framework Core (EF Core)](/ef/core) to work with the database.</span></span> <span data-ttu-id="79f07-107">EF Core 是一種物件關係映射器 (O/RM),可簡化數據存取。</span><span class="sxs-lookup"><span data-stu-id="79f07-107">EF Core is an object-relational mapper (O/RM) that simplifies data access.</span></span>

<span data-ttu-id="79f07-108">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="79f07-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="79f07-109">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="79f07-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="79f07-110">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="79f07-110">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79f07-112">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]\*\*\*\* > [新增資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="79f07-113">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="79f07-113">Name the folder *Models*.</span></span>

<span data-ttu-id="79f07-114">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f07-114">Right click the *Models* folder.</span></span> <span data-ttu-id="79f07-115">選擇 **「添加** > **類**」。</span><span class="sxs-lookup"><span data-stu-id="79f07-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="79f07-116">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="79f07-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79f07-118">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f07-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="79f07-119">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f07-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="79f07-121">在解決方案墊中,右鍵單擊**RazorPagesMovie**專案**Add**,然後>選擇"**添加新資料夾..."** 命名資料夾*模型*。</span><span class="sxs-lookup"><span data-stu-id="79f07-121">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="79f07-122">右鍵按下 *"模型"* 資料夾,**Add**>然後選擇 「**添加新檔..."。**</span><span class="sxs-lookup"><span data-stu-id="79f07-122">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="79f07-123">在 [新增檔案]\*\*\*\* 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="79f07-123">In the **New File** dialog:</span></span>

  * <span data-ttu-id="79f07-124">在左窗格中選取 [一般]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-124">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="79f07-125">在中央窗格中選取 [類別是空的]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-125">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="79f07-126">將類別命名為 **Movie**，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-126">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="79f07-127">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="79f07-127">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="79f07-128">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="79f07-128">Scaffold the movie model</span></span>

<span data-ttu-id="79f07-129">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="79f07-129">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="79f07-130">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="79f07-130">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79f07-132">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="79f07-132">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="79f07-133">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-133">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="79f07-134">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="79f07-134">Name the folder *Movies*</span></span>

<span data-ttu-id="79f07-135">以滑鼠右鍵按一下 [Pages/Movies]\*\* 資料夾 > [新增]\*\* [新增 Scaffolded 項目]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-135">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="79f07-137">在 [新增 Scaffold]\*\*\*\* 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]\*\* [新增]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-137">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="79f07-139">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="79f07-139">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="79f07-140">在 [模型類別]\*\*\*\* 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-140">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="79f07-141">在 [資料內容類別]\*\*\*\* 列中選取 [+]\*\*\*\* \(加號\)，並將產生的名稱 RazorPagesMovie.**Models**.RazorPagesMovieContext 變更為 RazorPagesMovie.**Data**.RazorPagesMovieContext。</span><span class="sxs-lookup"><span data-stu-id="79f07-141">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="79f07-142">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="79f07-142">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="79f07-143">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="79f07-143">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="79f07-144">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="79f07-144">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/3/arp.png)

<span data-ttu-id="79f07-146">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="79f07-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="79f07-148">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="79f07-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="79f07-149">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="79f07-149">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="79f07-150">**對於 Windows**:執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="79f07-150">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="79f07-151">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="79f07-151">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79f07-153">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="79f07-153">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="79f07-154">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-154">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="79f07-155">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="79f07-155">Name the folder *Movies*</span></span>

<span data-ttu-id="79f07-156">右鍵單擊 *"頁面/電影*"資料夾 **>添加新**>**基架..."**</span><span class="sxs-lookup"><span data-stu-id="79f07-156">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![前述指示中的圖片。](model/_static/scaMac.png)

<span data-ttu-id="79f07-158">在 **"新建基架"對話框中**,選擇 **"使用實體框架 (CRUD)** > **下一步**剃刀頁面"。</span><span class="sxs-lookup"><span data-stu-id="79f07-158">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

<span data-ttu-id="79f07-160">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="79f07-160">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="79f07-161">在**模型"類**中,下拉、選擇或鍵入**影片(RazorPagesMovie.Model)。**</span><span class="sxs-lookup"><span data-stu-id="79f07-161">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="79f07-162">在 **「數據上下文」類**行中,鍵入新類 RazorPagesMovie 的名稱。**資料**。剃刀頁電影上下文。</span><span class="sxs-lookup"><span data-stu-id="79f07-162">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="79f07-163">這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="79f07-163">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="79f07-164">它會使用正確的命名空間來建立資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="79f07-164">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="79f07-165">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="79f07-165">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arpMac.png)

<span data-ttu-id="79f07-167">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="79f07-167">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="79f07-168">新增 EF 工具</span><span class="sxs-lookup"><span data-stu-id="79f07-168">Add EF tools</span></span>

<span data-ttu-id="79f07-169">執行以下 .NET 核心 CLI 指令:</span><span class="sxs-lookup"><span data-stu-id="79f07-169">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="79f07-170">前面的命令為 .NET 核心 CLI 添加了實體框架核心工具。</span><span class="sxs-lookup"><span data-stu-id="79f07-170">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="79f07-171">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="79f07-171">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-172">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-172">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79f07-173">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="79f07-173">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="79f07-174">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="79f07-174">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="79f07-175">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="79f07-175">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="79f07-176">已更新</span><span class="sxs-lookup"><span data-stu-id="79f07-176">Updated</span></span>

* <span data-ttu-id="79f07-177">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="79f07-177">*Startup.cs*</span></span>

<span data-ttu-id="79f07-178">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f07-178">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-179">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79f07-180">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="79f07-180">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="79f07-181">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="79f07-181">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="79f07-182">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="79f07-182">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="79f07-183">已更新</span><span class="sxs-lookup"><span data-stu-id="79f07-183">Updated</span></span>

* <span data-ttu-id="79f07-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="79f07-184">*Startup.cs*</span></span>

<span data-ttu-id="79f07-185">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f07-185">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="79f07-187">Scaffold 處理序會建立下列檔案：</span><span class="sxs-lookup"><span data-stu-id="79f07-187">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="79f07-188">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="79f07-188">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="79f07-189">下一節將說明所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f07-189">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="79f07-190">初始移轉</span><span class="sxs-lookup"><span data-stu-id="79f07-190">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79f07-192">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="79f07-192">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="79f07-193">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="79f07-193">Add an initial migration.</span></span>
* <span data-ttu-id="79f07-194">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="79f07-194">Update the database with the initial migration.</span></span>

<span data-ttu-id="79f07-195">從 [工具]\*\*\*\* 功能表中，選取 [NuGet 封裝管理員]**[封裝管理員主控台]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-195">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="79f07-197">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="79f07-197">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="79f07-200">前面的命令生成以下警告:「未為實體類型」Movie"上的十進位列"價格"指定類型。</span><span class="sxs-lookup"><span data-stu-id="79f07-200">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="79f07-201">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="79f07-201">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="79f07-202">使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」</span><span class="sxs-lookup"><span data-stu-id="79f07-202">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="79f07-203">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="79f07-203">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="79f07-204">遷移命令生成代碼以創建初始資料庫架構。</span><span class="sxs-lookup"><span data-stu-id="79f07-204">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="79f07-205">架構基於`DbContext`中 指定的模型。</span><span class="sxs-lookup"><span data-stu-id="79f07-205">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="79f07-206">`InitialCreate` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="79f07-206">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="79f07-207">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="79f07-207">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="79f07-208">該`update`命令`Up`在尚未應用的遷移中運行該方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-208">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="79f07-209">在這種情況下,`update``Up`在*建立資料庫的移轉\</ 時間戳>_InitialCreate.cs*檔中執行該方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-209">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-210">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-210">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="79f07-211">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="79f07-211">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="79f07-212">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="79f07-212">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="79f07-213">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="79f07-213">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="79f07-214">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="79f07-214">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="79f07-215">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="79f07-215">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="79f07-216">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="79f07-216">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="79f07-217">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-217">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="79f07-218">強調顯示的行由 Scaffolder 新增：</span><span class="sxs-lookup"><span data-stu-id="79f07-218">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="79f07-219">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="79f07-219">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="79f07-220">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="79f07-220">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="79f07-221">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="79f07-221">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="79f07-222">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="79f07-222">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="79f07-223">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="79f07-223">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="79f07-224">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="79f07-224">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="79f07-225">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="79f07-225">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="79f07-226">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="79f07-226">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-227">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-227">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="79f07-228">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-228">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-229">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-229">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79f07-230">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-230">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="79f07-231">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="79f07-231">Test the app</span></span>

* <span data-ttu-id="79f07-232">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="79f07-232">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="79f07-233">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="79f07-233">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="79f07-234">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="79f07-234">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="79f07-235">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="79f07-235">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="79f07-237">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="79f07-237">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="79f07-238">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="79f07-238">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="79f07-239">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="79f07-239">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="79f07-240">測試 [編輯]\*\*\*\*、[詳細資料]\*\*\*\* 和 [刪除]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="79f07-240">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="79f07-241">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f07-241">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79f07-242">其他資源</span><span class="sxs-lookup"><span data-stu-id="79f07-242">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="79f07-243">[上一篇:](xref:tutorials/razor-pages/razor-pages-start)
> [開始下一步: 腳手架剃刀頁](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="79f07-243">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="79f07-244">在本節中,將添加類,用於在跨平臺[SQLite 資料庫中](https://www.sqlite.org/index.html)管理影片。</span><span class="sxs-lookup"><span data-stu-id="79f07-244">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="79f07-245">從ASP.NET核心範本創建的應用使用SQLite資料庫。</span><span class="sxs-lookup"><span data-stu-id="79f07-245">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="79f07-246">該應用程式的模型類與[實體框架核心 (EF Core)](/ef/core) [(SQLite EF 核心資料庫提供者](/ef/core/providers/sqlite)) 一起使用,以便與資料庫一起使用。</span><span class="sxs-lookup"><span data-stu-id="79f07-246">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="79f07-247">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。</span><span class="sxs-lookup"><span data-stu-id="79f07-247">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="79f07-248">模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="79f07-248">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="79f07-249">它們會定義資料儲存在資料庫中的屬性。</span><span class="sxs-lookup"><span data-stu-id="79f07-249">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="79f07-250">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="79f07-250">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79f07-252">以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]\*\*\*\* > [新增資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-252">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="79f07-253">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="79f07-253">Name the folder *Models*.</span></span>

<span data-ttu-id="79f07-254">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f07-254">Right click the *Models* folder.</span></span> <span data-ttu-id="79f07-255">選擇 **「添加** > **類**」。</span><span class="sxs-lookup"><span data-stu-id="79f07-255">Select **Add** > **Class**.</span></span> <span data-ttu-id="79f07-256">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="79f07-256">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-257">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-257">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79f07-258">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f07-258">Add a folder named *Models*.</span></span>
* <span data-ttu-id="79f07-259">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f07-259">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-260">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="79f07-261">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增]\*\*\*\* > [新增資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-261">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="79f07-262">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="79f07-262">Name the folder *Models*.</span></span>
* <span data-ttu-id="79f07-263">以滑鼠右鍵按一下 [Models]\*\* 資料夾，然後選取 [新增]\*\* [新增檔案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-263">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="79f07-264">在 [新增檔案]\*\*\*\* 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="79f07-264">In the **New File** dialog:</span></span>

  * <span data-ttu-id="79f07-265">在左窗格中選取 [一般]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-265">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="79f07-266">在中央窗格中選取 [類別是空的]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-266">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="79f07-267">將類別命名為 **Movie**，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-267">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="79f07-268">建置專案，以確認沒有任何編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="79f07-268">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="79f07-269">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="79f07-269">Scaffold the movie model</span></span>

<span data-ttu-id="79f07-270">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="79f07-270">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="79f07-271">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="79f07-271">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-272">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-272">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79f07-273">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="79f07-273">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="79f07-274">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-274">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="79f07-275">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="79f07-275">Name the folder *Movies*</span></span>

<span data-ttu-id="79f07-276">以滑鼠右鍵按一下 [Pages/Movies]\*\* 資料夾 > [新增]\*\* [新增 Scaffolded 項目]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-276">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/sca.png)

<span data-ttu-id="79f07-278">在 [新增 Scaffold]\*\*\*\* 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]\*\* [新增]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-278">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffold.png)

<span data-ttu-id="79f07-280">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="79f07-280">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="79f07-281">在 [模型類別]\*\*\*\* 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-281">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="79f07-282">在 [資料內容類別]\*\*\*\* 列中選取 [+]\*\*\*\* (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。</span><span class="sxs-lookup"><span data-stu-id="79f07-282">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="79f07-283">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="79f07-283">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arp.png)

<span data-ttu-id="79f07-285">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="79f07-285">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-286">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-286">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="79f07-287">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="79f07-287">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="79f07-288">**對於 Windows**:執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="79f07-288">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="79f07-289">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="79f07-289">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-290">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79f07-291">建立 *Pages/Movies* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="79f07-291">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="79f07-292">以滑鼠右鍵按一下 [Pages]\*\* 資料夾 > [新增]\*\* [新增資料夾]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-292">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="79f07-293">將資料夾命名為 *Movies*</span><span class="sxs-lookup"><span data-stu-id="79f07-293">Name the folder *Movies*</span></span>

<span data-ttu-id="79f07-294">以滑鼠右鍵按一下 [Pages/Movies]\*\* 資料夾 > [新增]\*\* [新增 Scaffolded 項目]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-294">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前述指示中的圖片。](model/_static/scaMac.png)

<span data-ttu-id="79f07-296">在 **「新增新基架」對話框中**,**選擇使用實體框架 (CRUD)** >**添加**的剃刀頁面。</span><span class="sxs-lookup"><span data-stu-id="79f07-296">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

<span data-ttu-id="79f07-298">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="79f07-298">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="79f07-299">在**模型"類**下拉清單中,選擇或鍵入 **「影片**」。</span><span class="sxs-lookup"><span data-stu-id="79f07-299">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="79f07-300">在 **「資料上下文類」行**中,鍵入 **「RazorPages MovieContext」,** 這將創建具有正確命名空間的新 db 上下文類。</span><span class="sxs-lookup"><span data-stu-id="79f07-300">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="79f07-301">在這種情況下,這將是**RazorPages 電影.模型.RazorPages 電影上下文**。</span><span class="sxs-lookup"><span data-stu-id="79f07-301">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="79f07-302">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="79f07-302">Select **Add**.</span></span>

![前述指示中的圖片。](model/_static/arpMac.png)

<span data-ttu-id="79f07-304">*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="79f07-304">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="79f07-305">隨即建立 Scaffold 處理序並更新下列檔案：</span><span class="sxs-lookup"><span data-stu-id="79f07-305">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="79f07-306">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="79f07-306">Files created</span></span>

* <span data-ttu-id="79f07-307">*Pages/Movies*：建立、刪除、詳細資料、編輯和索引。</span><span class="sxs-lookup"><span data-stu-id="79f07-307">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="79f07-308">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="79f07-308">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="79f07-309">檔案已更新</span><span class="sxs-lookup"><span data-stu-id="79f07-309">File updated</span></span>

* <span data-ttu-id="79f07-310">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="79f07-310">*Startup.cs*</span></span>

<span data-ttu-id="79f07-311">下一節將說明所建立和更新的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f07-311">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="79f07-312">初始移轉</span><span class="sxs-lookup"><span data-stu-id="79f07-312">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-313">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-313">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79f07-314">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="79f07-314">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="79f07-315">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="79f07-315">Add an initial migration.</span></span>
* <span data-ttu-id="79f07-316">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="79f07-316">Update the database with the initial migration.</span></span>

<span data-ttu-id="79f07-317">從 [工具]\*\*\*\* 功能表中，選取 [NuGet 封裝管理員]**[封裝管理員主控台]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="79f07-317">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="79f07-319">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="79f07-319">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="79f07-320">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="79f07-320">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="79f07-321">結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="79f07-321">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="79f07-322">參數`InitialCreate`用於命名遷移。</span><span class="sxs-lookup"><span data-stu-id="79f07-322">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="79f07-323">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="79f07-323">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="79f07-324">如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="79f07-324">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="79f07-325">`Update-Database` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-325">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="79f07-326">`Up` 方法會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="79f07-326">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-327">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-327">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-328">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-328">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="79f07-329">前面的命令生成以下警告:"*未為實體類型"Movie"上的十進位列"價格"指定類型。如果值不適合預設精度和比例,則會導致值被靜默截斷。顯式指定 SQL Server 列類型,該類型可以使用"HasColumnType()「容納所有值。* 您可以忽略該警告,它將在後面的教程中修復。</span><span class="sxs-lookup"><span data-stu-id="79f07-329">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="79f07-330">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79f07-330">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="79f07-331">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="79f07-331">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="79f07-332">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="79f07-332">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="79f07-333">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="79f07-333">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="79f07-334">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="79f07-334">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="79f07-335">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="79f07-335">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="79f07-336">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="79f07-336">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="79f07-337">檢查 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-337">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="79f07-338">強調顯示的行由 Scaffolder 新增：</span><span class="sxs-lookup"><span data-stu-id="79f07-338">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="79f07-339">`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="79f07-339">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="79f07-340">資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="79f07-340">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="79f07-341">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="79f07-341">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="79f07-342">上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="79f07-342">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="79f07-343">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="79f07-343">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="79f07-344">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="79f07-344">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="79f07-345">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="79f07-345">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="79f07-346">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="79f07-346">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="79f07-347">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79f07-347">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="79f07-348">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-348">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="79f07-349">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79f07-349">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79f07-350">檢查 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="79f07-350">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="79f07-351">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="79f07-351">Test the app</span></span>

* <span data-ttu-id="79f07-352">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="79f07-352">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="79f07-353">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="79f07-353">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="79f07-354">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="79f07-354">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="79f07-355">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="79f07-355">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="79f07-357">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="79f07-357">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="79f07-358">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="79f07-358">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="79f07-359">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="79f07-359">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="79f07-360">測試 [編輯]\*\*\*\*、[詳細資料]\*\*\*\* 和 [刪除]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="79f07-360">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="79f07-361">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f07-361">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79f07-362">其他資源</span><span class="sxs-lookup"><span data-stu-id="79f07-362">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="79f07-363">[上一篇:](xref:tutorials/razor-pages/razor-pages-start)
> [開始下一步: 腳手架剃刀頁](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="79f07-363">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
