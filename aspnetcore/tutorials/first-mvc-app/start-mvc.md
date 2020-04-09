---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 901257efdfbc7b36249233745175f5ed253da2c7
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78662474"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="0ec93-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="0ec93-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="0ec93-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ec93-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="0ec93-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="0ec93-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="0ec93-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="0ec93-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="0ec93-107">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="0ec93-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ec93-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-108">Create a web app.</span></span>
> * <span data-ttu-id="0ec93-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="0ec93-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="0ec93-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="0ec93-110">Work with a database.</span></span>
> * <span data-ttu-id="0ec93-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="0ec93-111">Add search and validation.</span></span>

<span data-ttu-id="0ec93-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="0ec93-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="0ec93-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ec93-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ec93-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="0ec93-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ec93-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ec93-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ec93-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="0ec93-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="0ec93-117">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ec93-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ec93-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ec93-119">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="0ec93-120">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="0ec93-122">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="0ec93-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="0ec93-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="0ec93-125">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="0ec93-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="0ec93-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="0ec93-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="0ec93-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="0ec93-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="0ec93-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="0ec93-129">This is a basic starter project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="0ec93-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ec93-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ec93-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="0ec93-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0ec93-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="0ec93-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="0ec93-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="0ec93-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0ec93-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="0ec93-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0ec93-135">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="0ec93-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="0ec93-136">**"MvcMovie"中缺少一個對話框,其中包含用於生成和調試所需的資產。添加它們?**</span><span class="sxs-lookup"><span data-stu-id="0ec93-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="0ec93-137">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-137">Select **Yes**</span></span>

  * <span data-ttu-id="0ec93-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="0ec93-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="0ec93-139">`code -r MvcMovie`:在可視化工作室代碼中載入*MvcMovie.csproj*專案檔。</span><span class="sxs-lookup"><span data-stu-id="0ec93-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ec93-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ec93-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0ec93-141">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="0ec93-143">選取 [.Net Core]\*\* [應用程式]\*\* > \*\* [Web應用程式 (Model-View-Controller)]\*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="0ec93-145">在 **'設定新的ASP.NET核心 Web API 對話框**中,設定 **.NET 核心 3.1\*\*\*\*的目標框架**。</span><span class="sxs-lookup"><span data-stu-id="0ec93-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

  ![macOS .NET 核心 3.1 選擇](./start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="0ec93-147">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="0ec93-148">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="0ec93-148">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ec93-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ec93-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0ec93-150">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="0ec93-151">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0ec93-152">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0ec93-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ec93-153">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0ec93-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ec93-154">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="0ec93-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="0ec93-155">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ec93-156">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="0ec93-157">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="0ec93-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="0ec93-159">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="0ec93-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="0ec93-161">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="0ec93-161">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="0ec93-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ec93-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ec93-164">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="0ec93-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="0ec93-165">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="0ec93-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="0ec93-166">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0ec93-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="0ec93-167">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0ec93-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0ec93-168">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="0ec93-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="0ec93-169">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ec93-170">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ec93-172">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ec93-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0ec93-173">選擇 **「在不** > **除錯的情況下執行啟動**」以啟動應用。</span><span class="sxs-lookup"><span data-stu-id="0ec93-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0ec93-174">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="0ec93-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0ec93-175">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0ec93-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ec93-176">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0ec93-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ec93-177">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="0ec93-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0ec93-178">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="0ec93-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0ec93-179">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="0ec93-180">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="0ec93-180">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="0ec93-182">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="0ec93-182">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ec93-183">下一步</span><span class="sxs-lookup"><span data-stu-id="0ec93-183">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="0ec93-184">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="0ec93-184">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="0ec93-185">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="0ec93-185">The app manages a database of movie titles.</span></span> <span data-ttu-id="0ec93-186">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="0ec93-186">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ec93-187">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-187">Create a web app.</span></span>
> * <span data-ttu-id="0ec93-188">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="0ec93-188">Add and scaffold a model.</span></span>
> * <span data-ttu-id="0ec93-189">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="0ec93-189">Work with a database.</span></span>
> * <span data-ttu-id="0ec93-190">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="0ec93-190">Add search and validation.</span></span>

<span data-ttu-id="0ec93-191">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-191">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="0ec93-192">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="0ec93-192">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ec93-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ec93-193">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="0ec93-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ec93-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ec93-195">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ec93-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="0ec93-196">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="0ec93-196">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ec93-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ec93-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ec93-198">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-198">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="0ec93-199">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-199">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="0ec93-201">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-201">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="0ec93-202">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="0ec93-202">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="0ec93-204">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-204">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="0ec93-205">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="0ec93-205">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="0ec93-206">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="0ec93-206">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="0ec93-207">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-207">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="0ec93-208">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="0ec93-208">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="0ec93-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ec93-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ec93-210">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="0ec93-210">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0ec93-211">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="0ec93-211">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="0ec93-212">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="0ec93-212">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0ec93-213">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="0ec93-213">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="0ec93-214">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="0ec93-214">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="0ec93-215">**"MvcMovie"中缺少一個對話框,其中包含用於生成和調試所需的資產。添加它們?**</span><span class="sxs-lookup"><span data-stu-id="0ec93-215">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="0ec93-216">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-216">Select **Yes**</span></span>

  * <span data-ttu-id="0ec93-217">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="0ec93-217">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="0ec93-218">`code -r MvcMovie`:在可視化工作室代碼中載入*MvcMovie.csproj*專案檔。</span><span class="sxs-lookup"><span data-stu-id="0ec93-218">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ec93-219">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ec93-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0ec93-220">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-220">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="0ec93-222">選取 [.Net Core]\*\* [應用程式]\*\* > \*\* [Web應用程式 (Model-View-Controller)]\*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-222">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="0ec93-224">在 **「設定新的ASP.NET核心 Web API**對話框中,接受 **.NET Core 2.2**的預設**目標框架**。</span><span class="sxs-lookup"><span data-stu-id="0ec93-224">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="0ec93-226">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0ec93-226">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="0ec93-227">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="0ec93-227">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="0ec93-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ec93-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0ec93-229">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-229">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="0ec93-230">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0ec93-231">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0ec93-231">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ec93-232">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0ec93-232">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ec93-233">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="0ec93-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="0ec93-234">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-234">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ec93-235">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-235">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="0ec93-236">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="0ec93-236">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="0ec93-238">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="0ec93-238">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="0ec93-240">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="0ec93-240">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0ec93-241">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="0ec93-241">This app doesn't track personal information.</span></span> <span data-ttu-id="0ec93-242">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="0ec93-242">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="0ec93-244">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="0ec93-244">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="0ec93-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ec93-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0ec93-247">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="0ec93-247">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="0ec93-248">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="0ec93-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="0ec93-249">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0ec93-249">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="0ec93-250">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0ec93-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0ec93-251">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="0ec93-251">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="0ec93-252">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-252">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0ec93-253">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="0ec93-253">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="0ec93-254">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="0ec93-254">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0ec93-255">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="0ec93-255">This app doesn't track personal information.</span></span> <span data-ttu-id="0ec93-256">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="0ec93-256">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="0ec93-258">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="0ec93-258">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="0ec93-260">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0ec93-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0ec93-261">選擇 **「在不** > **除錯的情況下執行啟動**」以啟動應用。</span><span class="sxs-lookup"><span data-stu-id="0ec93-261">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0ec93-262">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="0ec93-262">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0ec93-263">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="0ec93-263">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ec93-264">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0ec93-264">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0ec93-265">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="0ec93-265">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0ec93-266">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="0ec93-266">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0ec93-267">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec93-267">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="0ec93-268">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="0ec93-268">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="0ec93-269">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="0ec93-269">This app doesn't track personal information.</span></span> <span data-ttu-id="0ec93-270">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="0ec93-270">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="0ec93-272">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="0ec93-272">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="0ec93-274">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="0ec93-274">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ec93-275">下一步</span><span class="sxs-lookup"><span data-stu-id="0ec93-275">Next</span></span>](adding-controller.md)

::: moniker-end
