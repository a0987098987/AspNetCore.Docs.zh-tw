---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 36f4811f876a6e35440445103a1f86ae06b31b6a
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022513"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="19c43-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="19c43-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="19c43-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="19c43-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="19c43-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="19c43-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="19c43-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="19c43-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="19c43-107">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="19c43-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="19c43-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-108">Create a web app.</span></span>
> * <span data-ttu-id="19c43-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="19c43-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="19c43-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="19c43-110">Work with a database.</span></span>
> * <span data-ttu-id="19c43-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="19c43-111">Add search and validation.</span></span>

<span data-ttu-id="19c43-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="19c43-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="19c43-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19c43-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19c43-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19c43-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19c43-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19c43-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="19c43-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="19c43-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="19c43-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19c43-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19c43-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="19c43-119">從 Visual Studio 中，選取 [建立新專案]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="19c43-120">依序選取 [ASP.NET Core Web 應用程式]  和 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="19c43-122">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="19c43-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="19c43-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="19c43-125">選取 [Web 應用程式 (Model-View-Controller)]  ，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="19c43-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="19c43-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="19c43-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="19c43-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="19c43-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="19c43-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="19c43-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19c43-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19c43-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="19c43-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="19c43-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="19c43-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="19c43-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="19c43-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="19c43-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="19c43-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="19c43-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="19c43-135">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="19c43-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="19c43-136">對話方塊隨即顯示，並指出 **'MvcMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="19c43-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="19c43-137">選取 [是] </span><span class="sxs-lookup"><span data-stu-id="19c43-137">Select **Yes**</span></span>

  * <span data-ttu-id="19c43-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="19c43-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="19c43-139">`code -r MvcMovie`：在 Visual Studio Code 中載入 *MvcMovie.csproj* 專案檔。</span><span class="sxs-lookup"><span data-stu-id="19c43-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19c43-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="19c43-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="19c43-141">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="19c43-143">選取 [.Net Core]  > [應用程式]  > [Web應用程式 (Model-View-Controller)]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="19c43-145">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，設定 **.NET Core 3.0** 的 [目標 Framework]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="19c43-146">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="19c43-147">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="19c43-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19c43-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19c43-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="19c43-149">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="19c43-150">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="19c43-151">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="19c43-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="19c43-152">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="19c43-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="19c43-153">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="19c43-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="19c43-154">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="19c43-155">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="19c43-156">您可以從 [偵錯]  功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="19c43-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="19c43-158">您可以選取 [IIS Express]  按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="19c43-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="19c43-160">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="19c43-160">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19c43-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19c43-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="19c43-163">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="19c43-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="19c43-164">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="19c43-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="19c43-165">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="19c43-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="19c43-166">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="19c43-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="19c43-167">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="19c43-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="19c43-168">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="19c43-169">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19c43-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="19c43-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="19c43-172">選取 [執行]   > [啟動但不偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="19c43-173">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="19c43-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="19c43-174">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="19c43-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="19c43-175">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="19c43-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="19c43-176">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="19c43-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="19c43-177">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="19c43-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="19c43-178">您可以從 [執行]  功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="19c43-179">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="19c43-179">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="19c43-181">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="19c43-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="19c43-182">下一步</span><span class="sxs-lookup"><span data-stu-id="19c43-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="19c43-183">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="19c43-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="19c43-184">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="19c43-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="19c43-185">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="19c43-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="19c43-186">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-186">Create a web app.</span></span>
> * <span data-ttu-id="19c43-187">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="19c43-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="19c43-188">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="19c43-188">Work with a database.</span></span>
> * <span data-ttu-id="19c43-189">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="19c43-189">Add search and validation.</span></span>

<span data-ttu-id="19c43-190">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="19c43-191">必要條件</span><span class="sxs-lookup"><span data-stu-id="19c43-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19c43-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19c43-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19c43-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19c43-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19c43-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="19c43-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="19c43-195">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="19c43-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19c43-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19c43-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="19c43-197">從 Visual Studio 中，選取 [建立新專案]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="19c43-198">依序選取 [ASP.NET Core Web 應用程式]  和 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="19c43-200">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="19c43-201">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="19c43-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="19c43-203">選取 [Web 應用程式 (Model-View-Controller)]  ，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="19c43-204">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="19c43-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="19c43-205">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="19c43-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="19c43-206">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="19c43-207">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="19c43-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19c43-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19c43-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="19c43-209">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="19c43-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="19c43-210">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="19c43-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="19c43-211">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="19c43-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="19c43-212">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="19c43-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="19c43-213">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="19c43-213">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="19c43-214">對話方塊隨即顯示，並指出 **'MvcMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="19c43-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="19c43-215">選取 [是] </span><span class="sxs-lookup"><span data-stu-id="19c43-215">Select **Yes**</span></span>

  * <span data-ttu-id="19c43-216">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="19c43-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="19c43-217">`code -r MvcMovie`：在 Visual Studio Code 中載入 *MvcMovie.csproj* 專案檔。</span><span class="sxs-lookup"><span data-stu-id="19c43-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19c43-218">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="19c43-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="19c43-219">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-219">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="19c43-221">選取 [.Net Core]  > [應用程式]  > [Web應用程式 (Model-View-Controller)]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="19c43-223">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，接受 **.NET Core 2.2** 的預設**目標 Framework**。</span><span class="sxs-lookup"><span data-stu-id="19c43-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="19c43-225">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="19c43-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="19c43-226">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="19c43-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="19c43-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19c43-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="19c43-228">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="19c43-229">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="19c43-230">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="19c43-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="19c43-231">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="19c43-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="19c43-232">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="19c43-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="19c43-233">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="19c43-234">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="19c43-235">您可以從 [偵錯]  功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="19c43-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="19c43-237">您可以選取 [IIS Express]  按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="19c43-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="19c43-239">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="19c43-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="19c43-240">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="19c43-240">This app doesn't track personal information.</span></span> <span data-ttu-id="19c43-241">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="19c43-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="19c43-243">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="19c43-243">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="19c43-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="19c43-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="19c43-246">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="19c43-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="19c43-247">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="19c43-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="19c43-248">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="19c43-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="19c43-249">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="19c43-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="19c43-250">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="19c43-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="19c43-251">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="19c43-252">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="19c43-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="19c43-253">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="19c43-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="19c43-254">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="19c43-254">This app doesn't track personal information.</span></span> <span data-ttu-id="19c43-255">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="19c43-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="19c43-257">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="19c43-257">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="19c43-259">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="19c43-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="19c43-260">選取 [執行]   > [啟動但不偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="19c43-261">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="19c43-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="19c43-262">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="19c43-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="19c43-263">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="19c43-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="19c43-264">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="19c43-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="19c43-265">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="19c43-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="19c43-266">您可以從 [執行]  功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="19c43-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="19c43-267">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="19c43-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="19c43-268">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="19c43-268">This app doesn't track personal information.</span></span> <span data-ttu-id="19c43-269">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="19c43-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="19c43-271">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="19c43-271">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="19c43-273">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="19c43-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="19c43-274">下一步</span><span class="sxs-lookup"><span data-stu-id="19c43-274">Next</span></span>](adding-controller.md)

::: moniker-end
