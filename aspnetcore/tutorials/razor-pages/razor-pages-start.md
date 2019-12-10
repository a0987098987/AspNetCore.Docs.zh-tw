---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: b651437b698d01310f90c5f14832616c1896e6c0
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959095"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f0b14-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f0b14-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f0b14-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="f0b14-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="f0b14-106">此教學課程是系列中的第一個課程，教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f0b14-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="f0b14-107">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0b14-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="f0b14-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="f0b14-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f0b14-109">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0b14-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="f0b14-110">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0b14-110">Run the app.</span></span>
> * <span data-ttu-id="f0b14-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="f0b14-111">Examine the project files.</span></span>

<span data-ttu-id="f0b14-112">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="f0b14-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="f0b14-114">必要條件：</span><span class="sxs-lookup"><span data-stu-id="f0b14-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0b14-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0b14-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0b14-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0b14-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0b14-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0b14-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="f0b14-118">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f0b14-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0b14-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0b14-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0b14-120">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f0b14-121">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="f0b14-122">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="f0b14-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="f0b14-123">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="f0b14-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f0b14-124">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="f0b14-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="f0b14-125">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="f0b14-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="f0b14-126">在下拉式清單 [ **Web 應用程式**] 中選取 [ **ASP.NET Core 3.1** ]，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="f0b14-128">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="f0b14-128">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0b14-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0b14-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f0b14-131">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="f0b14-132">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="f0b14-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f0b14-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="f0b14-134">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="f0b14-135">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0b14-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="f0b14-136">狀態列的 OmniSharp 火焰圖示變為綠色後，會出現一個對話方塊詢問「' RazorPagesMovie ' 缺少建置及偵錯所需的資產。要新增它們嗎?」</span><span class="sxs-lookup"><span data-stu-id="f0b14-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="f0b14-137">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-137">Select **Yes**.</span></span>

  <span data-ttu-id="f0b14-138">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="f0b14-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0b14-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0b14-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f0b14-140">選取 [檔案] > [新增解決方案]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-140">Select **File** > **New Solution**.</span></span>

![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="f0b14-142">選取 [.NET Core] > [應用程式] > [Web 應用程式] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="f0b14-144">在 [設定**您的新 ASP.NET Core WEB API** ] 對話方塊中，將 [**目標 Framework** ] 設為 [ **.net Core 3.1**]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![macOS .NET Core 3.0 選取項目](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="f0b14-146">將專案命名為 **RazorPagesMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="f0b14-148">開啟專案</span><span class="sxs-lookup"><span data-stu-id="f0b14-148">Open the project</span></span>

<span data-ttu-id="f0b14-149">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="f0b14-150">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f0b14-150">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="f0b14-151">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="f0b14-151">Examine the project files</span></span>

<span data-ttu-id="f0b14-152">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-152">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="f0b14-153">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="f0b14-153">Pages folder</span></span>

<span data-ttu-id="f0b14-154">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-154">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="f0b14-155">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="f0b14-155">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="f0b14-156">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-156">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="f0b14-157">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="f0b14-157">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="f0b14-158">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="f0b14-158">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="f0b14-159">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="f0b14-159">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="f0b14-160">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="f0b14-160">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="f0b14-161">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-161">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="f0b14-162">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="f0b14-162">wwwroot folder</span></span>

<span data-ttu-id="f0b14-163">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-163">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="f0b14-164">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-164">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="f0b14-165">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="f0b14-165">appSettings.json</span></span>

<span data-ttu-id="f0b14-166">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="f0b14-166">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="f0b14-167">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-167">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="f0b14-168">Program.cs</span><span class="sxs-lookup"><span data-stu-id="f0b14-168">Program.cs</span></span>

<span data-ttu-id="f0b14-169">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="f0b14-169">Contains the entry point for the program.</span></span> <span data-ttu-id="f0b14-170">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="f0b14-171">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f0b14-171">Startup.cs</span></span>

<span data-ttu-id="f0b14-172">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f0b14-172">Contains code that configures app behavior.</span></span> <span data-ttu-id="f0b14-173">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-173">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0b14-174">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f0b14-174">Next steps</span></span>

<span data-ttu-id="f0b14-175">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="f0b14-175">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f0b14-176">新增模型</span><span class="sxs-lookup"><span data-stu-id="f0b14-176">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f0b14-177">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="f0b14-177">This is the first tutorial of a series.</span></span> <span data-ttu-id="f0b14-178">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f0b14-178">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="f0b14-179">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0b14-179">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="f0b14-180">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="f0b14-180">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f0b14-181">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0b14-181">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="f0b14-182">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0b14-182">Run the app.</span></span>
> * <span data-ttu-id="f0b14-183">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="f0b14-183">Examine the project files.</span></span>

<span data-ttu-id="f0b14-184">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="f0b14-184">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="f0b14-186">必要條件：</span><span class="sxs-lookup"><span data-stu-id="f0b14-186">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0b14-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0b14-187">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0b14-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0b14-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0b14-189">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0b14-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="f0b14-190">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f0b14-190">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0b14-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0b14-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0b14-192">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="f0b14-193">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-193">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="f0b14-195">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="f0b14-195">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f0b14-196">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="f0b14-196">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="f0b14-198">在下拉式清單中選取 [ASP.NET Core 2.2]，然後選取 [Web 應用程式] 及 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-198">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="f0b14-200">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="f0b14-200">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0b14-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0b14-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f0b14-203">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-203">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="f0b14-204">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-204">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="f0b14-205">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f0b14-205">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="f0b14-206">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-206">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="f0b14-207">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0b14-207">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="f0b14-208">狀態列的 OmniSharp 火焰圖示變為綠色後，會出現一個對話方塊詢問「' RazorPagesMovie ' 缺少建置及偵錯所需的資產。要新增它們嗎?」</span><span class="sxs-lookup"><span data-stu-id="f0b14-208">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="f0b14-209">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0b14-209">Select **Yes**.</span></span>

  <span data-ttu-id="f0b14-210">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="f0b14-210">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0b14-211">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0b14-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f0b14-212">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f0b14-212">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="f0b14-213">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-213">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="f0b14-214">開啟專案</span><span class="sxs-lookup"><span data-stu-id="f0b14-214">Open the project</span></span>

<span data-ttu-id="f0b14-215">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-215">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="f0b14-216">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f0b14-216">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0b14-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0b14-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0b14-218">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="f0b14-218">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="f0b14-219">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0b14-219">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f0b14-220">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="f0b14-220">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f0b14-221">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="f0b14-221">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="f0b14-222">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="f0b14-222">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f0b14-223">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="f0b14-223">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="f0b14-224">在應用程式的首頁上，選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="f0b14-224">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f0b14-225">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-225">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f0b14-227">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f0b14-227">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0b14-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0b14-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="f0b14-230">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="f0b14-230">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f0b14-231">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="f0b14-231">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="f0b14-232">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="f0b14-232">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f0b14-233">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="f0b14-233">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="f0b14-234">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="f0b14-234">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="f0b14-235">在應用程式的首頁上，選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="f0b14-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f0b14-236">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f0b14-238">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f0b14-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0b14-240">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0b14-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="f0b14-241">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="f0b14-241">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f0b14-242">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="f0b14-242">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="f0b14-243">在應用程式的首頁上，選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="f0b14-243">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f0b14-244">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-244">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="f0b14-246">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f0b14-246">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="f0b14-248">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="f0b14-248">Examine the project files</span></span>

<span data-ttu-id="f0b14-249">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-249">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="f0b14-250">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="f0b14-250">Pages folder</span></span>

<span data-ttu-id="f0b14-251">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-251">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="f0b14-252">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="f0b14-252">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="f0b14-253">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="f0b14-253">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="f0b14-254">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="f0b14-254">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="f0b14-255">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="f0b14-255">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="f0b14-256">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="f0b14-256">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="f0b14-257">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="f0b14-257">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="f0b14-258">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-258">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="f0b14-259">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="f0b14-259">wwwroot folder</span></span>

<span data-ttu-id="f0b14-260">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="f0b14-260">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="f0b14-261">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-261">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="f0b14-262">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="f0b14-262">appSettings.json</span></span>

<span data-ttu-id="f0b14-263">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="f0b14-263">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="f0b14-264">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-264">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="f0b14-265">Program.cs</span><span class="sxs-lookup"><span data-stu-id="f0b14-265">Program.cs</span></span>

<span data-ttu-id="f0b14-266">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="f0b14-266">Contains the entry point for the program.</span></span> <span data-ttu-id="f0b14-267">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-267">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="f0b14-268">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f0b14-268">Startup.cs</span></span>

<span data-ttu-id="f0b14-269">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="f0b14-269">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="f0b14-270">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="f0b14-270">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f0b14-271">其他資源</span><span class="sxs-lookup"><span data-stu-id="f0b14-271">Additional resources</span></span>

* [<span data-ttu-id="f0b14-272">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="f0b14-272">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="f0b14-273">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f0b14-273">Next steps</span></span>

<span data-ttu-id="f0b14-274">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="f0b14-274">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f0b14-275">新增模型</span><span class="sxs-lookup"><span data-stu-id="f0b14-275">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
