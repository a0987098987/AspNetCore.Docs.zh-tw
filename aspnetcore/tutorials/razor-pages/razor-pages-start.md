---
title: 教學課程：開始使用Razor ASP.NET Core 中的頁面
author: rick-anderson
description: 這一系列的教學課程會示範Razor如何使用 ASP.NET Core 中的頁面。 瞭解如何建立模型、產生Razor頁面的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證，以及使用遷移來更新模型。
ms.author: riande
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 8ed12b1778673962fe0b174e005bd6d8a7f54168
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774869"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="9dc67-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9dc67-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="9dc67-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9dc67-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="9dc67-106">此教學課程是系列中的第一個課程，教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9dc67-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9dc67-107">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc67-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9dc67-108">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="9dc67-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9dc67-109">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc67-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9dc67-110">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc67-110">Run the app.</span></span>
> * <span data-ttu-id="9dc67-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="9dc67-111">Examine the project files.</span></span>

<span data-ttu-id="9dc67-112">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="9dc67-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9dc67-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="9dc67-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9dc67-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dc67-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="9dc67-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9dc67-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9dc67-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9dc67-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9dc67-118">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9dc67-118">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9dc67-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dc67-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9dc67-120">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="9dc67-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9dc67-121">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="9dc67-122">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="9dc67-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="9dc67-123">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="9dc67-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9dc67-124">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="9dc67-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="9dc67-125">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="9dc67-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="9dc67-126">在下拉式清單 [ **Web 應用程式**] 中選取 [ **ASP.NET Core 3.1** ]，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="9dc67-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="9dc67-128">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="9dc67-128">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="9dc67-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9dc67-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9dc67-131">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9dc67-132">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9dc67-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9dc67-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9dc67-134">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9dc67-135">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9dc67-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9dc67-136">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' RazorPagesMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="9dc67-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9dc67-137">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9dc67-137">Select **Yes**.</span></span>

  <span data-ttu-id="9dc67-138">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="9dc67-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9dc67-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9dc67-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9dc67-140">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-140">Select **File** > **New Solution**.</span></span>

![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="9dc67-142">選取 [.NET Core]\*\* [應用程式]\*\* > \*\* [Web 應用程式] \*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="9dc67-144">在 [設定**新的 Web 應用程式**] 對話方塊中，將 [**目標 Framework** ] 設為 [ **.net Core 3.1**]。</span><span class="sxs-lookup"><span data-stu-id="9dc67-144">In the **Configure your new Web Application** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![macOS .NET Core 3.1 選項](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="9dc67-146">將專案命名為 **RazorPagesMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9dc67-148">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="9dc67-148">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="9dc67-149">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="9dc67-149">Examine the project files</span></span>

<span data-ttu-id="9dc67-150">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-150">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9dc67-151">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="9dc67-151">Pages folder</span></span>

<span data-ttu-id="9dc67-152">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-152">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9dc67-153">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="9dc67-153">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9dc67-154">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-154">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9dc67-155">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="9dc67-155">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9dc67-156">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="9dc67-156">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9dc67-157">例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="9dc67-157">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9dc67-158">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="9dc67-158">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9dc67-159">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-159">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9dc67-160">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="9dc67-160">wwwroot folder</span></span>

<span data-ttu-id="9dc67-161">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-161">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9dc67-162">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-162">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9dc67-163">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9dc67-163">appSettings.json</span></span>

<span data-ttu-id="9dc67-164">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="9dc67-164">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9dc67-165">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-165">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9dc67-166">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9dc67-166">Program.cs</span></span>

<span data-ttu-id="9dc67-167">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="9dc67-167">Contains the entry point for the program.</span></span> <span data-ttu-id="9dc67-168">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9dc67-169">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9dc67-169">Startup.cs</span></span>

<span data-ttu-id="9dc67-170">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9dc67-170">Contains code that configures app behavior.</span></span> <span data-ttu-id="9dc67-171">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-171">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9dc67-172">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9dc67-172">Next steps</span></span>

<span data-ttu-id="9dc67-173">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="9dc67-173">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9dc67-174">新增模型</span><span class="sxs-lookup"><span data-stu-id="9dc67-174">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9dc67-175">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="9dc67-175">This is the first tutorial of a series.</span></span> <span data-ttu-id="9dc67-176">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9dc67-176">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9dc67-177">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc67-177">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9dc67-178">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="9dc67-178">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9dc67-179">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc67-179">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9dc67-180">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc67-180">Run the app.</span></span>
> * <span data-ttu-id="9dc67-181">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="9dc67-181">Examine the project files.</span></span>

<span data-ttu-id="9dc67-182">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="9dc67-182">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9dc67-184">必要條件</span><span class="sxs-lookup"><span data-stu-id="9dc67-184">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9dc67-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dc67-185">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="9dc67-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9dc67-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9dc67-187">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9dc67-187">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9dc67-188">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9dc67-188">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9dc67-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dc67-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9dc67-190">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="9dc67-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="9dc67-191">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-191">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="9dc67-193">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="9dc67-193">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9dc67-194">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="9dc67-194">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="9dc67-196">在下拉式清單中選取 [ASP.NET Core 2.2]\*\*\*\*，然後選取 [Web 應用程式]\*\*\*\* 及 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-196">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="9dc67-198">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="9dc67-198">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="9dc67-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9dc67-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9dc67-201">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-201">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9dc67-202">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-202">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9dc67-203">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9dc67-203">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9dc67-204">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-204">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9dc67-205">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9dc67-205">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9dc67-206">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' RazorPagesMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="9dc67-206">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9dc67-207">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9dc67-207">Select **Yes**.</span></span>

  <span data-ttu-id="9dc67-208">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="9dc67-208">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9dc67-209">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9dc67-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9dc67-210">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-210">Select **File** > **New Solution**.</span></span>

![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="9dc67-212">選取 [.NET Core]\*\* [應用程式]\*\* > \*\* [Web 應用程式] \*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-212">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="9dc67-214">在 [設定**您的新 ASP.NET Core WEB API** ] 對話方塊中，將 [**目標 Framework** ] 設為 [ **.net Core 3.1**]。</span><span class="sxs-lookup"><span data-stu-id="9dc67-214">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![macOS .NET Core 3.0 選取項目](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="9dc67-216">將專案命名為 **RazorPagesMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9dc67-216">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9dc67-218">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="9dc67-218">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9dc67-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dc67-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9dc67-220">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9dc67-220">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9dc67-221">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9dc67-221">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9dc67-222">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9dc67-222">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9dc67-223">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9dc67-223">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9dc67-224">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="9dc67-224">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9dc67-225">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="9dc67-225">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="9dc67-226">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="9dc67-226">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9dc67-227">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-227">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9dc67-229">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="9dc67-229">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[<span data-ttu-id="9dc67-231">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9dc67-231">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9dc67-232">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9dc67-232">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9dc67-233">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="9dc67-233">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9dc67-234">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9dc67-234">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9dc67-235">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9dc67-235">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9dc67-236">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="9dc67-236">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="9dc67-237">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="9dc67-237">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9dc67-238">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-238">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9dc67-240">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="9dc67-240">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9dc67-242">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9dc67-242">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9dc67-243">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="9dc67-243">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9dc67-244">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="9dc67-244">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="9dc67-245">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="9dc67-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9dc67-246">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="9dc67-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="9dc67-248">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="9dc67-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9dc67-250">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="9dc67-250">Examine the project files</span></span>

<span data-ttu-id="9dc67-251">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-251">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9dc67-252">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="9dc67-252">Pages folder</span></span>

<span data-ttu-id="9dc67-253">包含Razor頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-253">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9dc67-254">每Razor個頁面都是一對檔案：</span><span class="sxs-lookup"><span data-stu-id="9dc67-254">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9dc67-255">包含 HTML 標籤的*cshtml*檔案，其 c # 程式碼Razor使用語法。</span><span class="sxs-lookup"><span data-stu-id="9dc67-255">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9dc67-256">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="9dc67-256">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9dc67-257">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="9dc67-257">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9dc67-258">例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="9dc67-258">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9dc67-259">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="9dc67-259">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9dc67-260">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-260">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9dc67-261">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="9dc67-261">wwwroot folder</span></span>

<span data-ttu-id="9dc67-262">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="9dc67-262">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9dc67-263">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-263">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9dc67-264">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9dc67-264">appSettings.json</span></span>

<span data-ttu-id="9dc67-265">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="9dc67-265">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9dc67-266">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-266">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9dc67-267">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9dc67-267">Program.cs</span></span>

<span data-ttu-id="9dc67-268">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="9dc67-268">Contains the entry point for the program.</span></span> <span data-ttu-id="9dc67-269">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-269">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9dc67-270">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9dc67-270">Startup.cs</span></span>

<span data-ttu-id="9dc67-271">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="9dc67-271">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="9dc67-272">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="9dc67-272">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9dc67-273">其他資源</span><span class="sxs-lookup"><span data-stu-id="9dc67-273">Additional resources</span></span>

* [<span data-ttu-id="9dc67-274">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="9dc67-274">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="9dc67-275">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9dc67-275">Next steps</span></span>

<span data-ttu-id="9dc67-276">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="9dc67-276">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9dc67-277">新增模型</span><span class="sxs-lookup"><span data-stu-id="9dc67-277">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
