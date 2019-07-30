---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1605197188d97f27a884739a72400da2d5818b1a
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371984"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="11b1a-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="11b1a-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="11b1a-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="11b1a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="11b1a-106">此教學課程是系列中的第一個課程，教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="11b1a-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="11b1a-107">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="11b1a-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="11b1a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="11b1a-109">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="11b1a-110">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-110">Run the app.</span></span>
> * <span data-ttu-id="11b1a-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="11b1a-111">Examine the project files.</span></span>

<span data-ttu-id="11b1a-112">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="11b1a-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="11b1a-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="11b1a-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="11b1a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11b1a-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="11b1a-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11b1a-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="11b1a-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="11b1a-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="11b1a-118">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="11b1a-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="11b1a-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11b1a-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="11b1a-120">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="11b1a-121">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="11b1a-122">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="11b1a-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="11b1a-123">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="11b1a-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="11b1a-124">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="11b1a-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="11b1a-125">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="11b1a-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="11b1a-126">在下拉式清單中選取 [ASP.NET Core 3.0]  ，然後依序選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="11b1a-128">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="11b1a-128">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="11b1a-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11b1a-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="11b1a-131">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="11b1a-132">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="11b1a-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="11b1a-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="11b1a-134">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="11b1a-135">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="11b1a-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="11b1a-136">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="11b1a-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="11b1a-137">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-137">Select **Yes**.</span></span>

  <span data-ttu-id="11b1a-138">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="11b1a-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="11b1a-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="11b1a-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="11b1a-140">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="11b1a-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="11b1a-141">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="11b1a-142">開啟專案</span><span class="sxs-lookup"><span data-stu-id="11b1a-142">Open the project</span></span>

<span data-ttu-id="11b1a-143">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="11b1a-144">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="11b1a-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="11b1a-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11b1a-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="11b1a-146">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="11b1a-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="11b1a-147">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="11b1a-148">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="11b1a-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="11b1a-149">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="11b1a-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="11b1a-150">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="11b1a-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="11b1a-151">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="11b1a-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="11b1a-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11b1a-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="11b1a-153">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="11b1a-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="11b1a-154">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="11b1a-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="11b1a-155">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="11b1a-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="11b1a-156">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="11b1a-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="11b1a-157">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="11b1a-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="11b1a-158">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="11b1a-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="11b1a-159">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="11b1a-159">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="11b1a-160">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="11b1a-160">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="11b1a-161">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="11b1a-161">Examine the project files</span></span>

<span data-ttu-id="11b1a-162">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="11b1a-163">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="11b1a-163">Pages folder</span></span>

<span data-ttu-id="11b1a-164">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="11b1a-165">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="11b1a-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="11b1a-166">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="11b1a-167">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="11b1a-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="11b1a-168">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="11b1a-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="11b1a-169">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="11b1a-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="11b1a-170">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="11b1a-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="11b1a-171">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-171">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="11b1a-172">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="11b1a-172">wwwroot folder</span></span>

<span data-ttu-id="11b1a-173">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="11b1a-174">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="11b1a-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="11b1a-175">appSettings.json</span></span>

<span data-ttu-id="11b1a-176">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="11b1a-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="11b1a-177">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="11b1a-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="11b1a-178">Program.cs</span></span>

<span data-ttu-id="11b1a-179">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="11b1a-179">Contains the entry point for the program.</span></span> <span data-ttu-id="11b1a-180">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-180">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="11b1a-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="11b1a-181">Startup.cs</span></span>

<span data-ttu-id="11b1a-182">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="11b1a-182">Contains code that configures app behavior.</span></span> <span data-ttu-id="11b1a-183">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11b1a-184">後續步驟</span><span class="sxs-lookup"><span data-stu-id="11b1a-184">Next steps</span></span>

<span data-ttu-id="11b1a-185">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="11b1a-185">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="11b1a-186">新增模型</span><span class="sxs-lookup"><span data-stu-id="11b1a-186">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="11b1a-187">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="11b1a-187">This is the first tutorial of a series.</span></span> <span data-ttu-id="11b1a-188">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="11b1a-188">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="11b1a-189">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-189">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="11b1a-190">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="11b1a-190">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="11b1a-191">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-191">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="11b1a-192">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-192">Run the app.</span></span>
> * <span data-ttu-id="11b1a-193">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="11b1a-193">Examine the project files.</span></span>

<span data-ttu-id="11b1a-194">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="11b1a-194">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="11b1a-196">必要條件</span><span class="sxs-lookup"><span data-stu-id="11b1a-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="11b1a-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11b1a-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="11b1a-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11b1a-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="11b1a-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="11b1a-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="11b1a-200">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="11b1a-200">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="11b1a-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11b1a-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="11b1a-202">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-202">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="11b1a-203">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-203">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="11b1a-205">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="11b1a-205">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="11b1a-206">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="11b1a-206">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="11b1a-208">在下拉式清單中選取 [ASP.NET Core 2.2]  ，然後選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-208">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="11b1a-210">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="11b1a-210">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="11b1a-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11b1a-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="11b1a-213">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-213">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="11b1a-214">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-214">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="11b1a-215">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="11b1a-215">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="11b1a-216">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-216">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="11b1a-217">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="11b1a-217">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="11b1a-218">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="11b1a-218">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="11b1a-219">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="11b1a-219">Select **Yes**.</span></span>

  <span data-ttu-id="11b1a-220">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="11b1a-220">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="11b1a-221">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="11b1a-221">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="11b1a-222">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="11b1a-222">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="11b1a-223">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-223">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="11b1a-224">開啟專案</span><span class="sxs-lookup"><span data-stu-id="11b1a-224">Open the project</span></span>

<span data-ttu-id="11b1a-225">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-225">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="11b1a-226">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="11b1a-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="11b1a-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11b1a-227">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="11b1a-228">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="11b1a-228">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="11b1a-229">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="11b1a-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="11b1a-230">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="11b1a-230">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="11b1a-231">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="11b1a-231">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="11b1a-232">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="11b1a-232">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="11b1a-233">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="11b1a-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="11b1a-234">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="11b1a-234">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="11b1a-235">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-235">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="11b1a-237">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="11b1a-237">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="11b1a-239">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11b1a-239">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="11b1a-240">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="11b1a-240">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="11b1a-241">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="11b1a-241">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="11b1a-242">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="11b1a-242">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="11b1a-243">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="11b1a-243">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="11b1a-244">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="11b1a-244">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="11b1a-245">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="11b1a-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="11b1a-246">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="11b1a-248">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="11b1a-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="11b1a-250">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="11b1a-250">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="11b1a-251">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="11b1a-251">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="11b1a-252">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="11b1a-252">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="11b1a-253">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="11b1a-253">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="11b1a-254">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-254">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="11b1a-256">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="11b1a-256">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="11b1a-258">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="11b1a-258">Examine the project files</span></span>

<span data-ttu-id="11b1a-259">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-259">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="11b1a-260">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="11b1a-260">Pages folder</span></span>

<span data-ttu-id="11b1a-261">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-261">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="11b1a-262">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="11b1a-262">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="11b1a-263">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="11b1a-263">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="11b1a-264">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="11b1a-264">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="11b1a-265">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="11b1a-265">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="11b1a-266">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="11b1a-266">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="11b1a-267">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="11b1a-267">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="11b1a-268">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-268">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="11b1a-269">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="11b1a-269">wwwroot folder</span></span>

<span data-ttu-id="11b1a-270">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="11b1a-270">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="11b1a-271">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-271">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="11b1a-272">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="11b1a-272">appSettings.json</span></span>

<span data-ttu-id="11b1a-273">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="11b1a-273">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="11b1a-274">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-274">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="11b1a-275">Program.cs</span><span class="sxs-lookup"><span data-stu-id="11b1a-275">Program.cs</span></span>

<span data-ttu-id="11b1a-276">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="11b1a-276">Contains the entry point for the program.</span></span> <span data-ttu-id="11b1a-277">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-277">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="11b1a-278">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="11b1a-278">Startup.cs</span></span>

<span data-ttu-id="11b1a-279">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="11b1a-279">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="11b1a-280">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="11b1a-280">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11b1a-281">其他資源</span><span class="sxs-lookup"><span data-stu-id="11b1a-281">Additional resources</span></span>

* [<span data-ttu-id="11b1a-282">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="11b1a-282">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="11b1a-283">後續步驟</span><span class="sxs-lookup"><span data-stu-id="11b1a-283">Next steps</span></span>

<span data-ttu-id="11b1a-284">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="11b1a-284">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="11b1a-285">新增模型</span><span class="sxs-lookup"><span data-stu-id="11b1a-285">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end