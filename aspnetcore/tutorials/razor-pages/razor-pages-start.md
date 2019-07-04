---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 6/3/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 207f5a5ab5e21d8cd8f6cf2f63641b94d8077ae5
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555801"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="0a606-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0a606-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0a606-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a606-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a606-106">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="0a606-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="0a606-107">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="0a606-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="0a606-108">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a606-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="0a606-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="0a606-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0a606-110">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a606-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="0a606-111">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a606-111">Run the app.</span></span>
> * <span data-ttu-id="0a606-112">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="0a606-112">Examine the project files.</span></span>

<span data-ttu-id="0a606-113">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="0a606-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="0a606-115">必要條件</span><span class="sxs-lookup"><span data-stu-id="0a606-115">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a606-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a606-116">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a606-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a606-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a606-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0a606-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="0a606-119">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="0a606-119">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a606-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a606-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a606-121">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="0a606-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="0a606-122">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="0a606-122">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="0a606-124">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="0a606-124">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="0a606-125">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="0a606-125">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="0a606-127">在下拉式清單中選取 [ASP.NET Core 2.2]  ，然後選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="0a606-127">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="0a606-129">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="0a606-129">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a606-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a606-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a606-132">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="0a606-132">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="0a606-133">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="0a606-133">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="0a606-134">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0a606-134">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="0a606-135">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="0a606-135">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="0a606-136">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="0a606-136">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="0a606-137">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="0a606-137">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="0a606-138">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="0a606-138">Select **Yes**.</span></span>

  <span data-ttu-id="0a606-139">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="0a606-139">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a606-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0a606-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0a606-141">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0a606-141">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="0a606-142">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="0a606-142">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="0a606-143">開啟專案</span><span class="sxs-lookup"><span data-stu-id="0a606-143">Open the project</span></span>

<span data-ttu-id="0a606-144">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0a606-144">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="0a606-145">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="0a606-145">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a606-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a606-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a606-147">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="0a606-147">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="0a606-148">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a606-148">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0a606-149">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0a606-149">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0a606-150">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0a606-150">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="0a606-151">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="0a606-151">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="0a606-152">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="0a606-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="0a606-153">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="0a606-153">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0a606-154">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="0a606-154">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="0a606-156">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="0a606-156">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a606-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a606-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="0a606-159">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="0a606-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="0a606-160">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="0a606-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="0a606-161">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0a606-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0a606-162">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0a606-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0a606-163">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="0a606-163">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="0a606-164">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="0a606-164">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0a606-165">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="0a606-165">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="0a606-167">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="0a606-167">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a606-169">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0a606-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0a606-170">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="0a606-170">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="0a606-171">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="0a606-171">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="0a606-172">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="0a606-172">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0a606-173">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="0a606-173">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="0a606-175">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="0a606-175">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="0a606-177">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="0a606-177">Examine the project files</span></span>

<span data-ttu-id="0a606-178">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="0a606-178">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="0a606-179">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="0a606-179">Pages folder</span></span>

<span data-ttu-id="0a606-180">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="0a606-180">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="0a606-181">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="0a606-181">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="0a606-182">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="0a606-182">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="0a606-183">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="0a606-183">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="0a606-184">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="0a606-184">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="0a606-185">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="0a606-185">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="0a606-186">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="0a606-186">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="0a606-187">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="0a606-187">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="0a606-188">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="0a606-188">wwwroot folder</span></span>

<span data-ttu-id="0a606-189">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="0a606-189">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="0a606-190">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="0a606-190">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="0a606-191">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="0a606-191">appSettings.json</span></span>

<span data-ttu-id="0a606-192">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="0a606-192">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="0a606-193">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="0a606-193">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="0a606-194">Program.cs</span><span class="sxs-lookup"><span data-stu-id="0a606-194">Program.cs</span></span>

<span data-ttu-id="0a606-195">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="0a606-195">Contains the entry point for the program.</span></span> <span data-ttu-id="0a606-196">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="0a606-196">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="0a606-197">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="0a606-197">Startup.cs</span></span>

<span data-ttu-id="0a606-198">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="0a606-198">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="0a606-199">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="0a606-199">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a606-200">其他資源</span><span class="sxs-lookup"><span data-stu-id="0a606-200">Additional resources</span></span>

* [<span data-ttu-id="0a606-201">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="0a606-201">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="0a606-202">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0a606-202">Next steps</span></span>

<span data-ttu-id="0a606-203">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="0a606-203">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0a606-204">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a606-204">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="0a606-205">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a606-205">Ran the app.</span></span>
> * <span data-ttu-id="0a606-206">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="0a606-206">Examined the project files.</span></span>

<span data-ttu-id="0a606-207">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="0a606-207">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0a606-208">新增模型</span><span class="sxs-lookup"><span data-stu-id="0a606-208">Add a model</span></span>](xref:tutorials/razor-pages/model)
