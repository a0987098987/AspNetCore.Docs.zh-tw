---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 81a2a76fc1cecc78b69226fe714d7c9272b04bf7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899186"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="1b44b-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1b44b-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="1b44b-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1b44b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1b44b-106">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="1b44b-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="1b44b-107">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="1b44b-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="1b44b-108">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="1b44b-109">在本教學課程中，您將：</span><span class="sxs-lookup"><span data-stu-id="1b44b-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b44b-110">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="1b44b-111">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-111">Run the app.</span></span>
> * <span data-ttu-id="1b44b-112">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="1b44b-112">Examine the project files.</span></span>

<span data-ttu-id="1b44b-113">在本教學課程結束時，您將會有一個運作正常的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="1b44b-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="1b44b-115">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="1b44b-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b44b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b44b-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1b44b-117">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="1b44b-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="1b44b-118">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="1b44b-119">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="1b44b-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="1b44b-120">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="1b44b-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="1b44b-122">在下拉式清單中選取 [ASP.NET Core 2.2]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="1b44b-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="1b44b-124">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="1b44b-124">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b44b-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1b44b-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1b44b-127">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="1b44b-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="1b44b-128">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b44b-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="1b44b-129">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1b44b-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="1b44b-130">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="1b44b-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="1b44b-131">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b44b-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="1b44b-132">這會顯示對話方塊並指出：**'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="1b44b-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="1b44b-133">選取 [是]</span><span class="sxs-lookup"><span data-stu-id="1b44b-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b44b-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1b44b-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1b44b-135">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1b44b-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="1b44b-136">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="1b44b-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="1b44b-137">開啟專案</span><span class="sxs-lookup"><span data-stu-id="1b44b-137">Open the project</span></span>

<span data-ttu-id="1b44b-138">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1b44b-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="1b44b-139">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="1b44b-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1b44b-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b44b-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1b44b-141">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="1b44b-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="1b44b-142">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="1b44b-143">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="1b44b-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1b44b-144">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="1b44b-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="1b44b-145">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="1b44b-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="1b44b-146">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="1b44b-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1b44b-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1b44b-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1b44b-148">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="1b44b-148">Press **Ctrl-F5** to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="1b44b-149">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="1b44b-149">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="1b44b-150">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="1b44b-150">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1b44b-151">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="1b44b-151">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="1b44b-152">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="1b44b-152">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1b44b-153">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1b44b-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1b44b-154">選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-154">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="1b44b-155">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="1b44b-155">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* <span data-ttu-id="1b44b-156">在應用程式的首頁上，選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="1b44b-156">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1b44b-157">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="1b44b-157">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1b44b-159">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="1b44b-159">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="1b44b-161">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="1b44b-161">Examine the project files</span></span>

<span data-ttu-id="1b44b-162">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="1b44b-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="1b44b-163">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="1b44b-163">Pages folder</span></span>

<span data-ttu-id="1b44b-164">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="1b44b-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="1b44b-165">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="1b44b-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="1b44b-166">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="1b44b-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="1b44b-167">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="1b44b-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="1b44b-168">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="1b44b-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="1b44b-169">例如，*_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="1b44b-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="1b44b-170">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="1b44b-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="1b44b-171">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="1b44b-171">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="1b44b-172">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="1b44b-172">wwwroot folder</span></span>

<span data-ttu-id="1b44b-173">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="1b44b-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="1b44b-174">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="1b44b-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="1b44b-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="1b44b-175">appSettings.json</span></span>

<span data-ttu-id="1b44b-176">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="1b44b-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="1b44b-177">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="1b44b-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="1b44b-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="1b44b-178">Program.cs</span></span>

<span data-ttu-id="1b44b-179">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="1b44b-179">Contains the entry point for the program.</span></span> <span data-ttu-id="1b44b-180">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="1b44b-180">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="1b44b-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="1b44b-181">Startup.cs</span></span>

<span data-ttu-id="1b44b-182">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="1b44b-182">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="1b44b-183">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="1b44b-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b44b-184">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1b44b-184">Next steps</span></span>

<span data-ttu-id="1b44b-185">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="1b44b-185">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b44b-186">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-186">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="1b44b-187">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b44b-187">Ran the app.</span></span>
> * <span data-ttu-id="1b44b-188">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="1b44b-188">Examined the project files.</span></span>

<span data-ttu-id="1b44b-189">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="1b44b-189">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1b44b-190">新增模型</span><span class="sxs-lookup"><span data-stu-id="1b44b-190">Add a model</span></span>](xref:tutorials/razor-pages/model)
