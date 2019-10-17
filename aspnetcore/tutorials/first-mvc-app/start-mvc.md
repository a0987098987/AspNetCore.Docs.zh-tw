---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f07afb15c0a31110c20d96a5db5c02030cefe5d5
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519097"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="4bfc6-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="4bfc6-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="4bfc6-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="4bfc6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="4bfc6-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="4bfc6-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="4bfc6-107">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4bfc6-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-108">Create a web app.</span></span>
> * <span data-ttu-id="4bfc6-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="4bfc6-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-110">Work with a database.</span></span>
> * <span data-ttu-id="4bfc6-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-111">Add search and validation.</span></span>

<span data-ttu-id="4bfc6-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="4bfc6-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="4bfc6-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bfc6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bfc6-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4bfc6-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4bfc6-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4bfc6-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4bfc6-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="4bfc6-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4bfc6-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bfc6-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bfc6-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4bfc6-119">從 Visual Studio 中，選取 [建立新專案]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="4bfc6-120">依序選取 [ASP.NET Core Web 應用程式] 和 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="4bfc6-122">將專案命名為 **MvcMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="4bfc6-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="4bfc6-125">選取 [Web 應用程式 (Model-View-Controller)]，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="4bfc6-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="4bfc6-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="4bfc6-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="4bfc6-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="4bfc6-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4bfc6-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4bfc6-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4bfc6-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="4bfc6-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="4bfc6-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4bfc6-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4bfc6-135">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="4bfc6-136">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="4bfc6-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="4bfc6-137">選取 [是]</span><span class="sxs-lookup"><span data-stu-id="4bfc6-137">Select **Yes**</span></span>

  * <span data-ttu-id="4bfc6-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="4bfc6-139">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie .csproj*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4bfc6-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4bfc6-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4bfc6-141">選取 [檔案] > [新增解決方案]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="4bfc6-143">選取 [.Net Core] > [應用程式] > [Web應用程式 (Model-View-Controller)] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="4bfc6-145">在 [設定您的新 ASP.NET Core Web API] 對話方塊中，設定 **.NET Core 3.0** 的 [目標 Framework]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="4bfc6-146">將專案命名為 **MvcMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="4bfc6-147">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="4bfc6-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bfc6-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bfc6-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4bfc6-149">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="4bfc6-150">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="4bfc6-151">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4bfc6-152">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4bfc6-153">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="4bfc6-154">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4bfc6-155">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="4bfc6-156">您可以從 [偵錯] 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="4bfc6-158">您可以選取 [IIS Express] 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="4bfc6-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="4bfc6-160">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-160">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4bfc6-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4bfc6-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4bfc6-163">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="4bfc6-164">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="4bfc6-165">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="4bfc6-166">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="4bfc6-167">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="4bfc6-168">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4bfc6-169">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4bfc6-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4bfc6-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4bfc6-172">選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4bfc6-173">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="4bfc6-174">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4bfc6-175">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4bfc6-176">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4bfc6-177">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4bfc6-178">您可以從 [執行] 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="4bfc6-179">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-179">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="4bfc6-181">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4bfc6-182">下一步</span><span class="sxs-lookup"><span data-stu-id="4bfc6-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="4bfc6-183">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="4bfc6-184">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="4bfc6-185">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4bfc6-186">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-186">Create a web app.</span></span>
> * <span data-ttu-id="4bfc6-187">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="4bfc6-188">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-188">Work with a database.</span></span>
> * <span data-ttu-id="4bfc6-189">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-189">Add search and validation.</span></span>

<span data-ttu-id="4bfc6-190">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="4bfc6-191">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="4bfc6-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bfc6-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bfc6-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4bfc6-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4bfc6-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4bfc6-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4bfc6-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="4bfc6-195">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4bfc6-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bfc6-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bfc6-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4bfc6-197">從 Visual Studio 中，選取 [建立新專案]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="4bfc6-198">依序選取 [ASP.NET Core Web 應用程式] 和 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="4bfc6-200">將專案命名為 **MvcMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="4bfc6-201">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="4bfc6-203">選取 [Web 應用程式 (Model-View-Controller)]，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="4bfc6-204">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="4bfc6-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="4bfc6-205">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="4bfc6-206">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="4bfc6-207">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4bfc6-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4bfc6-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4bfc6-209">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="4bfc6-210">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="4bfc6-211">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="4bfc6-212">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="4bfc6-213">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="4bfc6-214">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="4bfc6-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="4bfc6-215">選取 [是]</span><span class="sxs-lookup"><span data-stu-id="4bfc6-215">Select **Yes**</span></span>

  * <span data-ttu-id="4bfc6-216">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="4bfc6-217">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie .csproj*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4bfc6-218">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4bfc6-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4bfc6-219">選取 [檔案] > [新增解決方案]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-219">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="4bfc6-221">選取 [.Net Core] > [應用程式] > [Web應用程式 (Model-View-Controller)] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="4bfc6-223">在 [設定您的新 ASP.NET Core Web API] 對話方塊中，接受 **.NET Core 2.2** 的預設**目標 Framework**。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="4bfc6-225">將專案命名為 **MvcMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="4bfc6-226">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="4bfc6-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4bfc6-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bfc6-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4bfc6-228">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="4bfc6-229">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="4bfc6-230">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4bfc6-231">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4bfc6-232">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="4bfc6-233">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4bfc6-234">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="4bfc6-235">您可以從 [偵錯] 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="4bfc6-237">您可以選取 [IIS Express] 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="4bfc6-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="4bfc6-239">選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4bfc6-240">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-240">This app doesn't track personal information.</span></span> <span data-ttu-id="4bfc6-241">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="4bfc6-243">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-243">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4bfc6-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4bfc6-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4bfc6-246">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="4bfc6-247">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="4bfc6-248">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="4bfc6-249">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="4bfc6-250">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="4bfc6-251">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="4bfc6-252">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="4bfc6-253">選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4bfc6-254">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-254">This app doesn't track personal information.</span></span> <span data-ttu-id="4bfc6-255">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="4bfc6-257">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-257">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4bfc6-259">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4bfc6-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4bfc6-260">選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4bfc6-261">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="4bfc6-262">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4bfc6-263">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4bfc6-264">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4bfc6-265">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4bfc6-266">您可以從 [執行] 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="4bfc6-267">選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="4bfc6-268">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-268">This app doesn't track personal information.</span></span> <span data-ttu-id="4bfc6-269">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="4bfc6-271">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bfc6-271">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="4bfc6-273">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="4bfc6-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4bfc6-274">下一步</span><span class="sxs-lookup"><span data-stu-id="4bfc6-274">Next</span></span>](adding-controller.md)

::: moniker-end
