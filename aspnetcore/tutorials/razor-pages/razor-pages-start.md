---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 6/3/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d843e47ccb5180fab34b4c4c4a4b5cbda21289bf
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491215"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="a6dc3-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a6dc3-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="a6dc3-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6dc3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a6dc3-106">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="a6dc3-107">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="a6dc3-108">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="a6dc3-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6dc3-110">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="a6dc3-111">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-111">Run the app.</span></span>
> * <span data-ttu-id="a6dc3-112">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-112">Examine the project files.</span></span>

<span data-ttu-id="a6dc3-113">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="a6dc3-115">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a6dc3-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6dc3-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6dc3-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a6dc3-117">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="a6dc3-118">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-118">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="a6dc3-120">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-120">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="a6dc3-121">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-121">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="a6dc3-123">在下拉式清單中選取 [ASP.NET Core 2.2]  ，然後選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-123">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="a6dc3-125">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-125">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6dc3-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6dc3-127">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a6dc3-128">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="a6dc3-129">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-129">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="a6dc3-130">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-130">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="a6dc3-131">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-131">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="a6dc3-132">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-132">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="a6dc3-133">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="a6dc3-133">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="a6dc3-134">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-134">Select **Yes**.</span></span>

  <span data-ttu-id="a6dc3-135">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-135">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6dc3-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a6dc3-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a6dc3-137">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-137">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="a6dc3-138">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-138">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="a6dc3-139">開啟專案</span><span class="sxs-lookup"><span data-stu-id="a6dc3-139">Open the project</span></span>

<span data-ttu-id="a6dc3-140">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-140">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="a6dc3-141">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a6dc3-141">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6dc3-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6dc3-142">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a6dc3-143">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-143">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="a6dc3-144">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-144">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="a6dc3-145">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-145">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a6dc3-146">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-146">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="a6dc3-147">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-147">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="a6dc3-148">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-148">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="a6dc3-149">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-149">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="a6dc3-150">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-150">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="a6dc3-152">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-152">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6dc3-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6dc3-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="a6dc3-155">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-155">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="a6dc3-156">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-156">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="a6dc3-157">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a6dc3-158">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-158">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="a6dc3-159">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-159">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="a6dc3-160">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-160">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="a6dc3-161">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-161">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="a6dc3-163">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-163">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6dc3-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a6dc3-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="a6dc3-166">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-166">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="a6dc3-167">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="a6dc3-168">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-168">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="a6dc3-169">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-169">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="a6dc3-171">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-171">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="a6dc3-173">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="a6dc3-173">Examine the project files</span></span>

<span data-ttu-id="a6dc3-174">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-174">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="a6dc3-175">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="a6dc3-175">Pages folder</span></span>

<span data-ttu-id="a6dc3-176">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-176">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="a6dc3-177">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-177">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="a6dc3-178">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-178">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="a6dc3-179">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-179">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="a6dc3-180">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-180">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="a6dc3-181">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-181">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="a6dc3-182">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-182">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="a6dc3-183">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-183">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="a6dc3-184">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="a6dc3-184">wwwroot folder</span></span>

<span data-ttu-id="a6dc3-185">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-185">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="a6dc3-186">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-186">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="a6dc3-187">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="a6dc3-187">appSettings.json</span></span>

<span data-ttu-id="a6dc3-188">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-188">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="a6dc3-189">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-189">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="a6dc3-190">Program.cs</span><span class="sxs-lookup"><span data-stu-id="a6dc3-190">Program.cs</span></span>

<span data-ttu-id="a6dc3-191">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-191">Contains the entry point for the program.</span></span> <span data-ttu-id="a6dc3-192">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-192">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="a6dc3-193">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="a6dc3-193">Startup.cs</span></span>

<span data-ttu-id="a6dc3-194">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-194">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="a6dc3-195">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-195">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6dc3-196">其他資源</span><span class="sxs-lookup"><span data-stu-id="a6dc3-196">Additional resources</span></span>

* [<span data-ttu-id="a6dc3-197">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="a6dc3-197">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="a6dc3-198">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a6dc3-198">Next steps</span></span>

<span data-ttu-id="a6dc3-199">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-199">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6dc3-200">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-200">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="a6dc3-201">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-201">Ran the app.</span></span>
> * <span data-ttu-id="a6dc3-202">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="a6dc3-202">Examined the project files.</span></span>

<span data-ttu-id="a6dc3-203">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="a6dc3-203">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a6dc3-204">新增模型</span><span class="sxs-lookup"><span data-stu-id="a6dc3-204">Add a model</span></span>](xref:tutorials/razor-pages/model)
