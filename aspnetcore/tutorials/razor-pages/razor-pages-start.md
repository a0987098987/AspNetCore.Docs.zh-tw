---
title: 開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861624"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="71e18-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="71e18-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="71e18-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71e18-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71e18-106">本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="71e18-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="71e18-107">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="71e18-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="71e18-108">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="71e18-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="71e18-109">建立 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="71e18-110">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="71e18-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="71e18-111">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="71e18-111">Work with a database.</span></span>
> * <span data-ttu-id="71e18-112">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="71e18-112">Add search and validation.</span></span>

<span data-ttu-id="71e18-113">結束時，您將會有一個可管理及顯示電影標題項目的應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="71e18-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="71e18-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="71e18-115">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="71e18-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71e18-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71e18-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="71e18-117">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="71e18-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="71e18-118">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="71e18-119">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="71e18-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="71e18-120">請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="71e18-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="71e18-121">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="71e18-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="71e18-122">在下拉式清單中選取 [ASP.NET Core 2.2]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="71e18-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="71e18-124">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="71e18-124">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="71e18-126">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="71e18-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="71e18-127">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="71e18-128">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="71e18-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="71e18-129">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="71e18-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="71e18-130">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="71e18-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="71e18-131">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="71e18-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="71e18-132">在上述影像中，連接埠編號為 5001。</span><span class="sxs-lookup"><span data-stu-id="71e18-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="71e18-133">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="71e18-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="71e18-134">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="71e18-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="71e18-135">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="71e18-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71e18-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71e18-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="71e18-137">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="71e18-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="71e18-138">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="71e18-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="71e18-139">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="71e18-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="71e18-140">這會顯示對話方塊並指出：**'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="71e18-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="71e18-141">選取 [是]</span><span class="sxs-lookup"><span data-stu-id="71e18-141">Select **Yes**</span></span>

  * <span data-ttu-id="71e18-142">`dotnet new webapp -o RazorPagesMovie`：在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。</span><span class="sxs-lookup"><span data-stu-id="71e18-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="71e18-143">`code -r RazorPagesMovie`：在 Visual Studio Code 中載入 *RazorPagesMovie.csproj* 專案檔。</span><span class="sxs-lookup"><span data-stu-id="71e18-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="71e18-144">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="71e18-144">Launch the app</span></span>

* <span data-ttu-id="71e18-145">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="71e18-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="71e18-146">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="71e18-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="71e18-147">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="71e18-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="71e18-148">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="71e18-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="71e18-149">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="71e18-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="71e18-150">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="71e18-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="71e18-151">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="71e18-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71e18-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="71e18-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71e18-153">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="71e18-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="71e18-154">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。</span><span class="sxs-lookup"><span data-stu-id="71e18-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="71e18-155">將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="71e18-156">開啟專案</span><span class="sxs-lookup"><span data-stu-id="71e18-156">Open the project</span></span>

<span data-ttu-id="71e18-157">按 Ctrl + C 來關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="71e18-158">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="71e18-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="71e18-159">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="71e18-159">Launch the app</span></span>

<span data-ttu-id="71e18-160">選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="71e18-161">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="71e18-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="71e18-162">選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="71e18-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="71e18-163">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="71e18-163">This app doesn't track personal information.</span></span> <span data-ttu-id="71e18-164">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="71e18-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="71e18-166">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="71e18-166">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="71e18-168">專案檔和資料夾</span><span class="sxs-lookup"><span data-stu-id="71e18-168">Project files and folders</span></span>

<span data-ttu-id="71e18-169">下表列出專案中的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="71e18-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="71e18-170">在這部分的教學課程中，*Startup.cs* 檔案是需要了解的最重要項目。</span><span class="sxs-lookup"><span data-stu-id="71e18-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="71e18-171">您不需要檢閱以下提供的每個連結。</span><span class="sxs-lookup"><span data-stu-id="71e18-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="71e18-172">當您需要專案中的檔案或資料夾的詳細資訊時，便會提供連結作為參考。</span><span class="sxs-lookup"><span data-stu-id="71e18-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="71e18-173">檔案或資料夾</span><span class="sxs-lookup"><span data-stu-id="71e18-173">File or folder</span></span>              | <span data-ttu-id="71e18-174">用途</span><span class="sxs-lookup"><span data-stu-id="71e18-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="71e18-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="71e18-175">*wwwroot*</span></span> | <span data-ttu-id="71e18-176">包含靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="71e18-176">Contains static files.</span></span> <span data-ttu-id="71e18-177">請參閱[靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="71e18-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="71e18-178">*頁面*</span><span class="sxs-lookup"><span data-stu-id="71e18-178">*Pages*</span></span> | <span data-ttu-id="71e18-179">[Razor 頁面](xref:razor-pages/index)的資料夾。</span><span class="sxs-lookup"><span data-stu-id="71e18-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="71e18-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="71e18-180">*appsettings.json*</span></span> | [<span data-ttu-id="71e18-181">組態</span><span class="sxs-lookup"><span data-stu-id="71e18-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="71e18-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="71e18-182">*Program.cs*</span></span> | <span data-ttu-id="71e18-183">[裝載](xref:fundamentals/host/index) ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="71e18-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="71e18-184">*Startup.cs*</span></span> | <span data-ttu-id="71e18-185">設定服務和要求管線。</span><span class="sxs-lookup"><span data-stu-id="71e18-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="71e18-186">請參閱 [Startup](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="71e18-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="71e18-187">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="71e18-187">The Pages folder</span></span>

<span data-ttu-id="71e18-188">*_Layout.cshtml* 檔案包含通用的 HTML 元素 (指令碼和樣式表)，並設定應用程式的配置。</span><span class="sxs-lookup"><span data-stu-id="71e18-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="71e18-189">例如，當您按一下 **RazorPagesMovie**、**Home** 或 **Privacy** 時，會看到相同的元素。</span><span class="sxs-lookup"><span data-stu-id="71e18-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="71e18-190">通用元素包含頂端的導覽功能表和視窗底部的標頭。</span><span class="sxs-lookup"><span data-stu-id="71e18-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="71e18-191">如需詳細資訊，請參閱 [Layout](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="71e18-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="71e18-192">*_ViewImports.cshtml* 檔案包含匯入至每個 Razor 頁面的 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="71e18-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="71e18-193">如需詳細資訊，請參閱[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)。</span><span class="sxs-lookup"><span data-stu-id="71e18-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="71e18-194">*_ViewStart.cshtml* 會設定 Razor 頁面 `Layout` 屬性，以使用 *_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="71e18-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="71e18-195">如需詳細資訊，請參閱 [Layout](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="71e18-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="71e18-196">*_ValidationScriptsPartial.cshtml* 檔案提供 [jQuery](https://jquery.com/) 驗證指令碼的參考。</span><span class="sxs-lookup"><span data-stu-id="71e18-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="71e18-197">稍後在教學課程中新增 `Create` 和 `Edit` 頁面時，會使用 *_ValidationScriptsPartial.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="71e18-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="71e18-198">`Index`、`Error` 和 `Privacy` 頁面的提供目的如下：</span><span class="sxs-lookup"><span data-stu-id="71e18-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="71e18-199">`Index`：啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="71e18-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="71e18-200">`Error`：顯示錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="71e18-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="71e18-201">`Privacy`：指定網站隱私權原則的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="71e18-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="71e18-202">在本教學課程中，不會使用上述頁面。</span><span class="sxs-lookup"><span data-stu-id="71e18-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71e18-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71e18-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="71e18-204">使用 F7 鍵在 Razor 頁面與 PageModel 之間進行切換</span><span class="sxs-lookup"><span data-stu-id="71e18-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="71e18-205">F7 鍵可在 Razor 頁面 (*\*.cshtml* 檔案) 與 C# 檔案 (*\*.cshtml.cs*) 之間進行切換。</span><span class="sxs-lookup"><span data-stu-id="71e18-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71e18-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71e18-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="71e18-207">依照慣例，Razor 頁面 (*\*.cshtml* 檔案) 與相關聯的 `PageModel` 具有相同的根檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="71e18-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71e18-208">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="71e18-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71e18-209">依照慣例，Razor 頁面 (*\*.cshtml* 檔案) 與相關聯的 `PageModel` 具有相同的根檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="71e18-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="71e18-210">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="71e18-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)