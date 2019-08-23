---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 67a5fcee0a37861fd39a018443edbc0b9e513213
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487690"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="c6e82-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c6e82-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c6e82-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c6e82-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="c6e82-106">此教學課程是系列中的第一個課程，教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="c6e82-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="c6e82-107">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="c6e82-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="c6e82-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c6e82-109">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="c6e82-110">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-110">Run the app.</span></span>
> * <span data-ttu-id="c6e82-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="c6e82-111">Examine the project files.</span></span>

<span data-ttu-id="c6e82-112">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="c6e82-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="c6e82-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="c6e82-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6e82-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e82-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6e82-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6e82-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c6e82-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c6e82-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="c6e82-118">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c6e82-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6e82-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e82-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c6e82-120">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c6e82-121">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="c6e82-122">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="c6e82-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="c6e82-123">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="c6e82-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c6e82-124">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="c6e82-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="c6e82-125">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="c6e82-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="c6e82-126">在下拉式清單中選取 [ASP.NET Core 3.0]  ，然後依序選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="c6e82-128">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="c6e82-128">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6e82-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6e82-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c6e82-131">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="c6e82-132">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="c6e82-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c6e82-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="c6e82-134">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="c6e82-135">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c6e82-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="c6e82-136">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="c6e82-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="c6e82-137">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-137">Select **Yes**.</span></span>

  <span data-ttu-id="c6e82-138">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="c6e82-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c6e82-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c6e82-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c6e82-140">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-140">Select **File** > **New Solution**.</span></span>

![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="c6e82-142">選取 [.NET Core]  > [應用程式]  > [Web 應用程式]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="c6e82-144">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，為 [.NET Core 3.0]  設定 [目標 Framework]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![macOS .NET Core 3.0 選取項目](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="c6e82-146">將專案命名為 **RazorPagesMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="c6e82-148">開啟專案</span><span class="sxs-lookup"><span data-stu-id="c6e82-148">Open the project</span></span>

<span data-ttu-id="c6e82-149">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="c6e82-150">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c6e82-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6e82-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e82-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c6e82-152">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c6e82-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="c6e82-153">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c6e82-154">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c6e82-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c6e82-155">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c6e82-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="c6e82-156">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="c6e82-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c6e82-157">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="c6e82-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6e82-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6e82-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="c6e82-159">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c6e82-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c6e82-160">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="c6e82-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="c6e82-161">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c6e82-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c6e82-162">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c6e82-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="c6e82-163">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="c6e82-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c6e82-164">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c6e82-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="c6e82-165">按 **Alt-Cmd-Enter** 鍵，在沒有偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="c6e82-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="c6e82-166">或者，瀏覽到功能表列，然後前往 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="c6e82-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="c6e82-167">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="c6e82-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="c6e82-168">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="c6e82-168">Examine the project files</span></span>

<span data-ttu-id="c6e82-169">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="c6e82-170">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="c6e82-170">Pages folder</span></span>

<span data-ttu-id="c6e82-171">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="c6e82-172">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="c6e82-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="c6e82-173">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="c6e82-174">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="c6e82-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="c6e82-175">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="c6e82-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="c6e82-176">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="c6e82-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="c6e82-177">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="c6e82-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="c6e82-178">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="c6e82-179">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="c6e82-179">wwwroot folder</span></span>

<span data-ttu-id="c6e82-180">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="c6e82-181">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c6e82-182">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="c6e82-182">appSettings.json</span></span>

<span data-ttu-id="c6e82-183">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="c6e82-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="c6e82-184">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="c6e82-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="c6e82-185">Program.cs</span></span>

<span data-ttu-id="c6e82-186">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="c6e82-186">Contains the entry point for the program.</span></span> <span data-ttu-id="c6e82-187">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="c6e82-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c6e82-188">Startup.cs</span></span>

<span data-ttu-id="c6e82-189">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="c6e82-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="c6e82-190">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6e82-191">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c6e82-191">Next steps</span></span>

<span data-ttu-id="c6e82-192">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="c6e82-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c6e82-193">新增模型</span><span class="sxs-lookup"><span data-stu-id="c6e82-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c6e82-194">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="c6e82-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="c6e82-195">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="c6e82-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="c6e82-196">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="c6e82-197">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="c6e82-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c6e82-198">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="c6e82-199">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-199">Run the app.</span></span>
> * <span data-ttu-id="c6e82-200">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="c6e82-200">Examine the project files.</span></span>

<span data-ttu-id="c6e82-201">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="c6e82-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="c6e82-203">必要條件</span><span class="sxs-lookup"><span data-stu-id="c6e82-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6e82-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e82-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6e82-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6e82-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c6e82-206">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c6e82-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="c6e82-207">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c6e82-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6e82-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e82-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c6e82-209">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="c6e82-210">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="c6e82-212">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="c6e82-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c6e82-213">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="c6e82-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="c6e82-215">在下拉式清單中選取 [ASP.NET Core 2.2]  ，然後選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="c6e82-217">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="c6e82-217">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6e82-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6e82-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c6e82-220">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="c6e82-221">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="c6e82-222">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c6e82-222">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="c6e82-223">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="c6e82-224">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c6e82-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="c6e82-225">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="c6e82-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="c6e82-226">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="c6e82-226">Select **Yes**.</span></span>

  <span data-ttu-id="c6e82-227">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="c6e82-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c6e82-228">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c6e82-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c6e82-229">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c6e82-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="c6e82-230">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="c6e82-231">開啟專案</span><span class="sxs-lookup"><span data-stu-id="c6e82-231">Open the project</span></span>

<span data-ttu-id="c6e82-232">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="c6e82-233">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c6e82-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6e82-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6e82-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c6e82-235">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c6e82-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="c6e82-236">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e82-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c6e82-237">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c6e82-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c6e82-238">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c6e82-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="c6e82-239">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="c6e82-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c6e82-240">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="c6e82-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="c6e82-241">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="c6e82-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c6e82-242">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="c6e82-244">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c6e82-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6e82-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6e82-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="c6e82-247">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c6e82-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c6e82-248">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="c6e82-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="c6e82-249">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c6e82-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c6e82-250">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c6e82-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="c6e82-251">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="c6e82-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="c6e82-252">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="c6e82-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c6e82-253">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="c6e82-255">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c6e82-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c6e82-257">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c6e82-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="c6e82-258">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="c6e82-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c6e82-259">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="c6e82-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="c6e82-260">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="c6e82-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c6e82-261">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="c6e82-263">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c6e82-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="c6e82-265">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="c6e82-265">Examine the project files</span></span>

<span data-ttu-id="c6e82-266">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="c6e82-267">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="c6e82-267">Pages folder</span></span>

<span data-ttu-id="c6e82-268">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="c6e82-269">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="c6e82-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="c6e82-270">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="c6e82-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="c6e82-271">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="c6e82-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="c6e82-272">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="c6e82-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="c6e82-273">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="c6e82-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="c6e82-274">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="c6e82-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="c6e82-275">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="c6e82-276">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="c6e82-276">wwwroot folder</span></span>

<span data-ttu-id="c6e82-277">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="c6e82-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="c6e82-278">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c6e82-279">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="c6e82-279">appSettings.json</span></span>

<span data-ttu-id="c6e82-280">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="c6e82-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="c6e82-281">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="c6e82-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="c6e82-282">Program.cs</span></span>

<span data-ttu-id="c6e82-283">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="c6e82-283">Contains the entry point for the program.</span></span> <span data-ttu-id="c6e82-284">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="c6e82-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c6e82-285">Startup.cs</span></span>

<span data-ttu-id="c6e82-286">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="c6e82-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="c6e82-287">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="c6e82-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6e82-288">其他資源</span><span class="sxs-lookup"><span data-stu-id="c6e82-288">Additional resources</span></span>

* [<span data-ttu-id="c6e82-289">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="c6e82-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="c6e82-290">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c6e82-290">Next steps</span></span>

<span data-ttu-id="c6e82-291">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="c6e82-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c6e82-292">新增模型</span><span class="sxs-lookup"><span data-stu-id="c6e82-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
