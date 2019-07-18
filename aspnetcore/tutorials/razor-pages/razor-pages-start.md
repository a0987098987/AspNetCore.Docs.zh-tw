---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 06/03/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7e228c99b4d55c14cea9c915cf06a7fbbbd5af44
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855729"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f7c45-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f7c45-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f7c45-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f7c45-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f7c45-106">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="f7c45-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="f7c45-107">[本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f7c45-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="f7c45-108">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7c45-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="f7c45-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="f7c45-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f7c45-110">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7c45-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="f7c45-111">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7c45-111">Run the app.</span></span>
> * <span data-ttu-id="f7c45-112">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="f7c45-112">Examine the project files.</span></span>

<span data-ttu-id="f7c45-113">在本教學課程結束時，您將會有一個能夠運作的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。</span><span class="sxs-lookup"><span data-stu-id="f7c45-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="f7c45-115">必要條件</span><span class="sxs-lookup"><span data-stu-id="f7c45-115">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7c45-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7c45-116">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f7c45-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f7c45-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f7c45-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f7c45-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="f7c45-119">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f7c45-119">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7c45-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7c45-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f7c45-121">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="f7c45-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="f7c45-122">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="f7c45-122">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="f7c45-124">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="f7c45-124">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f7c45-125">請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="f7c45-125">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="f7c45-127">在下拉式清單中選取 [ASP.NET Core 2.2]  ，然後選取 [Web 應用程式]  及 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="f7c45-127">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="f7c45-129">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="f7c45-129">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f7c45-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f7c45-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f7c45-132">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f7c45-132">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="f7c45-133">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="f7c45-133">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="f7c45-134">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f7c45-134">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="f7c45-135">`dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="f7c45-135">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="f7c45-136">`code` 命令會在目前的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f7c45-136">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="f7c45-137">在狀態列的 OmniSharp 火焰圖示變成綠色之後，螢幕會出現對話方塊並詢問「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。**新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="f7c45-137">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="f7c45-138">選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="f7c45-138">Select **Yes**.</span></span>

  <span data-ttu-id="f7c45-139">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="f7c45-139">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f7c45-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f7c45-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f7c45-141">從終端機執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f7c45-141">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="f7c45-142">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="f7c45-142">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="f7c45-143">開啟專案</span><span class="sxs-lookup"><span data-stu-id="f7c45-143">Open the project</span></span>

<span data-ttu-id="f7c45-144">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f7c45-144">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="f7c45-145">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f7c45-145">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7c45-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7c45-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f7c45-147">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="f7c45-147">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="f7c45-148">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7c45-148">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f7c45-149">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="f7c45-149">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f7c45-150">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="f7c45-150">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="f7c45-151">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="f7c45-151">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f7c45-152">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="f7c45-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="f7c45-153">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="f7c45-153">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f7c45-154">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="f7c45-154">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f7c45-156">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f7c45-156">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f7c45-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f7c45-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="f7c45-159">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="f7c45-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f7c45-160">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="f7c45-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="f7c45-161">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="f7c45-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f7c45-162">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="f7c45-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="f7c45-163">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="f7c45-163">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="f7c45-164">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="f7c45-164">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f7c45-165">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="f7c45-165">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f7c45-167">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f7c45-167">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f7c45-169">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f7c45-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="f7c45-170">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="f7c45-170">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f7c45-171">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="f7c45-171">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="f7c45-172">在應用程式的首頁上，選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="f7c45-172">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f7c45-173">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="f7c45-173">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="f7c45-175">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f7c45-175">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="f7c45-177">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="f7c45-177">Examine the project files</span></span>

<span data-ttu-id="f7c45-178">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="f7c45-178">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="f7c45-179">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="f7c45-179">Pages folder</span></span>

<span data-ttu-id="f7c45-180">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="f7c45-180">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="f7c45-181">每個 Razor 頁面都是一組檔案：</span><span class="sxs-lookup"><span data-stu-id="f7c45-181">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="f7c45-182">*.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。</span><span class="sxs-lookup"><span data-stu-id="f7c45-182">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="f7c45-183">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7c45-183">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="f7c45-184">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="f7c45-184">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="f7c45-185">例如， *_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="f7c45-185">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="f7c45-186">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="f7c45-186">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="f7c45-187">如需詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-187">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="f7c45-188">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="f7c45-188">wwwroot folder</span></span>

<span data-ttu-id="f7c45-189">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="f7c45-189">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="f7c45-190">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-190">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="f7c45-191">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="f7c45-191">appSettings.json</span></span>

<span data-ttu-id="f7c45-192">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="f7c45-192">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="f7c45-193">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-193">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="f7c45-194">Program.cs</span><span class="sxs-lookup"><span data-stu-id="f7c45-194">Program.cs</span></span>

<span data-ttu-id="f7c45-195">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="f7c45-195">Contains the entry point for the program.</span></span> <span data-ttu-id="f7c45-196">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-196">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="f7c45-197">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f7c45-197">Startup.cs</span></span>

<span data-ttu-id="f7c45-198">包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。</span><span class="sxs-lookup"><span data-stu-id="f7c45-198">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="f7c45-199">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-199">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7c45-200">其他資源</span><span class="sxs-lookup"><span data-stu-id="f7c45-200">Additional resources</span></span>

* [<span data-ttu-id="f7c45-201">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="f7c45-201">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="f7c45-202">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f7c45-202">Next steps</span></span>

<span data-ttu-id="f7c45-203">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="f7c45-203">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f7c45-204">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7c45-204">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="f7c45-205">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7c45-205">Ran the app.</span></span>
> * <span data-ttu-id="f7c45-206">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="f7c45-206">Examined the project files.</span></span>

<span data-ttu-id="f7c45-207">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="f7c45-207">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f7c45-208">新增模型</span><span class="sxs-lookup"><span data-stu-id="f7c45-208">Add a model</span></span>](xref:tutorials/razor-pages/model)
