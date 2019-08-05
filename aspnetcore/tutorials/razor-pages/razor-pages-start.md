---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 57a10895c718c539ece280afcb27cb4033c7fb45
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682798"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="9ef2d-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9ef2d-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="9ef2d-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9ef2d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="9ef2d-106">此教學課程是系列中的第一個課程，教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9ef2d-107">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9ef2d-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ef2d-109">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9ef2d-110">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-110">Run the app.</span></span>
> * <span data-ttu-id="9ef2d-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-111">Examine the project files.</span></span>

<span data-ttu-id="9ef2d-112">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9ef2d-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="9ef2d-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ef2d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ef2d-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ef2d-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ef2d-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ef2d-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ef2d-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9ef2d-118">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9ef2d-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ef2d-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ef2d-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9ef2d-120">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9ef2d-121">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="9ef2d-122">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="9ef2d-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="9ef2d-123">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9ef2d-124">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="9ef2d-125">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="9ef2d-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="9ef2d-126">在下拉式清單中選取 [ASP.NET Core 3.0]  ，然後依序選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="9ef2d-128">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-128">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ef2d-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ef2d-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9ef2d-131">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9ef2d-132">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9ef2d-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9ef2d-134">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9ef2d-135">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9ef2d-136">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="9ef2d-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9ef2d-137">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-137">Select **Yes**.</span></span>

  <span data-ttu-id="9ef2d-138">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ef2d-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ef2d-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9ef2d-140">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9ef2d-141">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9ef2d-142">開啟專案</span><span class="sxs-lookup"><span data-stu-id="9ef2d-142">Open the project</span></span>

<span data-ttu-id="9ef2d-143">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9ef2d-144">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="9ef2d-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ef2d-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ef2d-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9ef2d-146">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9ef2d-147">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9ef2d-148">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9ef2d-149">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9ef2d-150">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9ef2d-151">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ef2d-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ef2d-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9ef2d-153">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9ef2d-154">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9ef2d-155">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9ef2d-156">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9ef2d-157">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ef2d-158">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ef2d-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9ef2d-159">按 **Alt-Cmd-Enter** 鍵，在沒有偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-159">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="9ef2d-160">或者，瀏覽到功能表列，然後前往 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-160">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="9ef2d-161">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9ef2d-162">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="9ef2d-162">Examine the project files</span></span>

<span data-ttu-id="9ef2d-163">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-163">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9ef2d-164">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="9ef2d-164">Pages folder</span></span>

<span data-ttu-id="9ef2d-165">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-165">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9ef2d-166">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-166">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9ef2d-167">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-167">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9ef2d-168">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-168">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9ef2d-169">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-169">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9ef2d-170">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-170">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9ef2d-171">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-171">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9ef2d-172">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-172">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9ef2d-173">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="9ef2d-173">wwwroot folder</span></span>

<span data-ttu-id="9ef2d-174">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-174">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9ef2d-175">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-175">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9ef2d-176">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9ef2d-176">appSettings.json</span></span>

<span data-ttu-id="9ef2d-177">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-177">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9ef2d-178">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-178">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9ef2d-179">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9ef2d-179">Program.cs</span></span>

<span data-ttu-id="9ef2d-180">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-180">Contains the entry point for the program.</span></span> <span data-ttu-id="9ef2d-181">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-181">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9ef2d-182">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9ef2d-182">Startup.cs</span></span>

<span data-ttu-id="9ef2d-183">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-183">Contains code that configures app behavior.</span></span> <span data-ttu-id="9ef2d-184">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-184">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ef2d-185">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9ef2d-185">Next steps</span></span>

<span data-ttu-id="9ef2d-186">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-186">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9ef2d-187">新增模型</span><span class="sxs-lookup"><span data-stu-id="9ef2d-187">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9ef2d-188">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-188">This is the first tutorial of a series.</span></span> <span data-ttu-id="9ef2d-189">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-189">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9ef2d-190">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-190">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9ef2d-191">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ef2d-192">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-192">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9ef2d-193">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-193">Run the app.</span></span>
> * <span data-ttu-id="9ef2d-194">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-194">Examine the project files.</span></span>

<span data-ttu-id="9ef2d-195">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-195">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9ef2d-197">必要條件</span><span class="sxs-lookup"><span data-stu-id="9ef2d-197">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ef2d-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ef2d-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ef2d-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ef2d-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ef2d-200">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ef2d-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9ef2d-201">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9ef2d-201">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ef2d-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ef2d-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9ef2d-203">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-203">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="9ef2d-204">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-204">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="9ef2d-206">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-206">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9ef2d-207">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-207">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="9ef2d-209">在下拉式清單中選取 [ASP.NET Core 2.2]  ，然後選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-209">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="9ef2d-211">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-211">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ef2d-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ef2d-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9ef2d-214">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-214">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9ef2d-215">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-215">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9ef2d-216">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-216">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9ef2d-217">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-217">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9ef2d-218">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-218">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9ef2d-219">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="9ef2d-219">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9ef2d-220">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-220">Select **Yes**.</span></span>

  <span data-ttu-id="9ef2d-221">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-221">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ef2d-222">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ef2d-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9ef2d-223">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-223">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9ef2d-224">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-224">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9ef2d-225">開啟專案</span><span class="sxs-lookup"><span data-stu-id="9ef2d-225">Open the project</span></span>

<span data-ttu-id="9ef2d-226">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-226">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9ef2d-227">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="9ef2d-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ef2d-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ef2d-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9ef2d-229">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-229">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9ef2d-230">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9ef2d-231">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-231">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9ef2d-232">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-232">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9ef2d-233">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-233">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9ef2d-234">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-234">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="9ef2d-235">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9ef2d-236">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9ef2d-238">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ef2d-240">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ef2d-240">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9ef2d-241">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-241">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9ef2d-242">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-242">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9ef2d-243">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-243">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9ef2d-244">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-244">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9ef2d-245">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-245">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="9ef2d-246">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-246">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9ef2d-247">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-247">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9ef2d-249">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-249">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9ef2d-251">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9ef2d-251">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9ef2d-252">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-252">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9ef2d-253">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-253">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="9ef2d-254">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-254">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9ef2d-255">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-255">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="9ef2d-257">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-257">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9ef2d-259">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="9ef2d-259">Examine the project files</span></span>

<span data-ttu-id="9ef2d-260">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-260">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9ef2d-261">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="9ef2d-261">Pages folder</span></span>

<span data-ttu-id="9ef2d-262">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-262">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9ef2d-263">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-263">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9ef2d-264">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-264">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9ef2d-265">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-265">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9ef2d-266">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-266">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9ef2d-267">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-267">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9ef2d-268">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-268">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9ef2d-269">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-269">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9ef2d-270">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="9ef2d-270">wwwroot folder</span></span>

<span data-ttu-id="9ef2d-271">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-271">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9ef2d-272">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-272">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9ef2d-273">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9ef2d-273">appSettings.json</span></span>

<span data-ttu-id="9ef2d-274">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-274">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9ef2d-275">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-275">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9ef2d-276">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9ef2d-276">Program.cs</span></span>

<span data-ttu-id="9ef2d-277">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-277">Contains the entry point for the program.</span></span> <span data-ttu-id="9ef2d-278">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-278">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9ef2d-279">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9ef2d-279">Startup.cs</span></span>

<span data-ttu-id="9ef2d-280">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-280">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="9ef2d-281">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="9ef2d-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ef2d-282">其他資源</span><span class="sxs-lookup"><span data-stu-id="9ef2d-282">Additional resources</span></span>

* [<span data-ttu-id="9ef2d-283">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="9ef2d-283">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="9ef2d-284">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9ef2d-284">Next steps</span></span>

<span data-ttu-id="9ef2d-285">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="9ef2d-285">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9ef2d-286">新增模型</span><span class="sxs-lookup"><span data-stu-id="9ef2d-286">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
