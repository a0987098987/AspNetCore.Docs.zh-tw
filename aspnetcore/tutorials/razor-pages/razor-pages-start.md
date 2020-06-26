---
title: 教學課程：開始使用 Razor ASP.NET Core 中的頁面
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 Razor ASP.NET Core 中的頁面。 瞭解如何建立模型、產生頁面的程式碼 Razor 、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證，以及使用遷移來更新模型。
ms.author: riande
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 97e3f60480bc8e7e88c8361e5b13f02d98765d9e
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405298"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="8bf8a-104">教學課程：開始使用 Razor ASP.NET Core 中的頁面</span><span class="sxs-lookup"><span data-stu-id="8bf8a-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="8bf8a-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8bf8a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="8bf8a-106">這是一系列的第一個教學課程，會教您建立 ASP.NET Core Razor 頁面 web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="8bf8a-107">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="8bf8a-108">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8bf8a-109">建立 Razor 頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="8bf8a-110">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-110">Run the app.</span></span>
> * <span data-ttu-id="8bf8a-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-111">Examine the project files.</span></span>

<span data-ttu-id="8bf8a-112">在本教學課程結束時，您將會有一個 Razor 可在稍後的教學課程中建立的工作頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="8bf8a-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="8bf8a-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8bf8a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bf8a-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="8bf8a-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8bf8a-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="8bf8a-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8bf8a-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="8bf8a-118">建立 Razor 頁面 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8bf8a-118">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8bf8a-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bf8a-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8bf8a-120">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8bf8a-121">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="8bf8a-122">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="8bf8a-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="8bf8a-123">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="8bf8a-124">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="8bf8a-125">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="8bf8a-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="8bf8a-126">在下拉式清單 [ **Web 應用程式**] 中選取 [ **ASP.NET Core 3.1** ]，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="8bf8a-128">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-128">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="8bf8a-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8bf8a-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8bf8a-131">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="8bf8a-132">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="8bf8a-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="8bf8a-134">`dotnet new`命令會 Razor 在*RazorPagesMovie*資料夾中建立新的頁面專案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="8bf8a-135">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="8bf8a-136">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' RazorPagesMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="8bf8a-137">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-137">Select **Yes**.</span></span>

  <span data-ttu-id="8bf8a-138">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="8bf8a-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8bf8a-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8bf8a-140">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-140">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="8bf8a-142">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-142">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application** > **Next**.</span></span> <span data-ttu-id="8bf8a-143">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-143">In version 8.6 or later, select **Web and Console** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS web 應用程式範本選取專案](razor-pages-start/_static/web_app_template_vsmac.png)

* <span data-ttu-id="8bf8a-145">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-145">Confirm the following configurations:</span></span>

  * <span data-ttu-id="8bf8a-146">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-146">**Target Framework** set to **.NET Core 3.1**.</span></span>
  * <span data-ttu-id="8bf8a-147">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-147">**Authentication** set to **No Authentication**.</span></span>
   
  <span data-ttu-id="8bf8a-148">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-148">Select **Next**.</span></span>

  ![macOS .NET Core 3.1 選項](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="8bf8a-150">將專案命名為 **RazorPagesMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-150">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![macOS 將專案命名為](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="8bf8a-152">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="8bf8a-152">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="8bf8a-153">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="8bf8a-153">Examine the project files</span></span>

<span data-ttu-id="8bf8a-154">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-154">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="8bf8a-155">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="8bf8a-155">Pages folder</span></span>

<span data-ttu-id="8bf8a-156">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-156">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="8bf8a-157">每個 Razor 頁面都是一對檔案：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-157">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="8bf8a-158">包含 HTML 標籤的*cshtml*檔案，其 c # 程式碼使用 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-158">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="8bf8a-159">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-159">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="8bf8a-160">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-160">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="8bf8a-161">例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-161">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="8bf8a-162">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-162">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="8bf8a-163">如需詳細資訊，請參閱 <xref:mvc/views/layout> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-163">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="8bf8a-164">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="8bf8a-164">wwwroot folder</span></span>

<span data-ttu-id="8bf8a-165">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-165">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="8bf8a-166">如需詳細資訊，請參閱 <xref:fundamentals/static-files> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-166">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="8bf8a-167">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="8bf8a-167">appSettings.json</span></span>

<span data-ttu-id="8bf8a-168">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-168">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="8bf8a-169">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="8bf8a-170">Program.cs</span><span class="sxs-lookup"><span data-stu-id="8bf8a-170">Program.cs</span></span>

<span data-ttu-id="8bf8a-171">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-171">Contains the entry point for the program.</span></span> <span data-ttu-id="8bf8a-172">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-172">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="8bf8a-173">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="8bf8a-173">Startup.cs</span></span>

<span data-ttu-id="8bf8a-174">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-174">Contains code that configures app behavior.</span></span> <span data-ttu-id="8bf8a-175">如需詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-175">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bf8a-176">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8bf8a-176">Next steps</span></span>

<span data-ttu-id="8bf8a-177">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-177">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8bf8a-178">新增模型</span><span class="sxs-lookup"><span data-stu-id="8bf8a-178">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8bf8a-179">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-179">This is the first tutorial of a series.</span></span> <span data-ttu-id="8bf8a-180">[本系列將](xref:tutorials/razor-pages/index)教您建立 ASP.NET Core Razor Pages web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-180">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="8bf8a-181">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-181">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="8bf8a-182">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-182">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8bf8a-183">建立 Razor 頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-183">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="8bf8a-184">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-184">Run the app.</span></span>
> * <span data-ttu-id="8bf8a-185">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-185">Examine the project files.</span></span>

<span data-ttu-id="8bf8a-186">在本教學課程結束時，您將會有一個 Razor 可在稍後的教學課程中建立的工作頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-186">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="8bf8a-188">必要條件</span><span class="sxs-lookup"><span data-stu-id="8bf8a-188">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8bf8a-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bf8a-189">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="8bf8a-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8bf8a-190">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="8bf8a-191">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8bf8a-191">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="8bf8a-192">建立 Razor 頁面 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8bf8a-192">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8bf8a-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bf8a-193">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8bf8a-194">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-194">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="8bf8a-195">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-195">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="8bf8a-197">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-197">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="8bf8a-198">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-198">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="8bf8a-200">在下拉式清單中選取 [ASP.NET Core 2.2]\*\*\*\*，然後選取 [Web 應用程式]\*\*\*\* 及 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-200">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="8bf8a-202">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-202">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="8bf8a-204">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8bf8a-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8bf8a-205">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-205">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="8bf8a-206">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-206">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="8bf8a-207">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-207">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="8bf8a-208">`dotnet new`命令會 Razor 在*RazorPagesMovie*資料夾中建立新的頁面專案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-208">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="8bf8a-209">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-209">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="8bf8a-210">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' RazorPagesMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="8bf8a-210">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="8bf8a-211">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-211">Select **Yes**.</span></span>

  <span data-ttu-id="8bf8a-212">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-212">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="8bf8a-213">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8bf8a-213">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8bf8a-214">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-214">Select **File** > **New Solution**.</span></span>

![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="8bf8a-216">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-216">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application** > **Next**.</span></span> <span data-ttu-id="8bf8a-217">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-217">In version 8.6 or later, select **Web and Console** > **App** > **Web Application** > **Next**.</span></span>

* <span data-ttu-id="8bf8a-218">在 [設定**您的新 ASP.NET Core WEB API** ] 對話方塊中，將 [**目標 Framework** ] 設為 [ **.net Core 3.1**]。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-218">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![macOS .NET Core 3.0 選取項目](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="8bf8a-220">將專案命名為 **RazorPagesMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-220">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="8bf8a-222">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="8bf8a-222">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="8bf8a-223">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bf8a-223">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8bf8a-224">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-224">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="8bf8a-225">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-225">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="8bf8a-226">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-226">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8bf8a-227">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-227">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="8bf8a-228">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-228">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="8bf8a-229">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-229">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="8bf8a-230">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-230">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="8bf8a-231">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-231">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="8bf8a-233">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-233">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[<span data-ttu-id="8bf8a-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8bf8a-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="8bf8a-236">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-236">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="8bf8a-237">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-237">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="8bf8a-238">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-238">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8bf8a-239">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-239">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="8bf8a-240">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-240">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="8bf8a-241">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="8bf8a-242">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="8bf8a-244">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="8bf8a-246">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8bf8a-246">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="8bf8a-247">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-247">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="8bf8a-248">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-248">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="8bf8a-249">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-249">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="8bf8a-250">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-250">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="8bf8a-252">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-252">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="8bf8a-254">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="8bf8a-254">Examine the project files</span></span>

<span data-ttu-id="8bf8a-255">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-255">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="8bf8a-256">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="8bf8a-256">Pages folder</span></span>

<span data-ttu-id="8bf8a-257">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-257">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="8bf8a-258">每個 Razor 頁面都是一對檔案：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-258">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="8bf8a-259">包含 HTML 標籤的*cshtml*檔案，其 c # 程式碼使用 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-259">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="8bf8a-260">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-260">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="8bf8a-261">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-261">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="8bf8a-262">例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-262">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="8bf8a-263">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-263">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="8bf8a-264">如需詳細資訊，請參閱 <xref:mvc/views/layout> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-264">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="8bf8a-265">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="8bf8a-265">wwwroot folder</span></span>

<span data-ttu-id="8bf8a-266">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-266">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="8bf8a-267">如需詳細資訊，請參閱 <xref:fundamentals/static-files> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-267">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="8bf8a-268">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="8bf8a-268">appSettings.json</span></span>

<span data-ttu-id="8bf8a-269">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-269">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="8bf8a-270">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-270">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="8bf8a-271">Program.cs</span><span class="sxs-lookup"><span data-stu-id="8bf8a-271">Program.cs</span></span>

<span data-ttu-id="8bf8a-272">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-272">Contains the entry point for the program.</span></span> <span data-ttu-id="8bf8a-273">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-273">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="8bf8a-274">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="8bf8a-274">Startup.cs</span></span>

<span data-ttu-id="8bf8a-275">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-275">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="8bf8a-276">如需詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="8bf8a-276">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8bf8a-277">其他資源</span><span class="sxs-lookup"><span data-stu-id="8bf8a-277">Additional resources</span></span>

* [<span data-ttu-id="8bf8a-278">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="8bf8a-278">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="8bf8a-279">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8bf8a-279">Next steps</span></span>

<span data-ttu-id="8bf8a-280">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="8bf8a-280">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8bf8a-281">新增模型</span><span class="sxs-lookup"><span data-stu-id="8bf8a-281">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
