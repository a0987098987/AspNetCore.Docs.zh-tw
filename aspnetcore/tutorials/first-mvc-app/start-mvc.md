---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 10/16/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 01321d68defafbe79371250669f921307bcfdba6
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407040"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="d754f-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="d754f-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="d754f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d754f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="d754f-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="d754f-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="d754f-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="d754f-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="d754f-107">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="d754f-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d754f-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-108">Create a web app.</span></span>
> * <span data-ttu-id="d754f-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="d754f-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="d754f-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="d754f-110">Work with a database.</span></span>
> * <span data-ttu-id="d754f-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="d754f-111">Add search and validation.</span></span>

<span data-ttu-id="d754f-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="d754f-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="d754f-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d754f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d754f-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="d754f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d754f-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="d754f-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d754f-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="d754f-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d754f-117">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d754f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d754f-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d754f-119">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="d754f-120">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="d754f-122">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="d754f-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="d754f-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="d754f-125">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="d754f-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="d754f-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="d754f-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="d754f-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="d754f-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="d754f-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="d754f-129">This is a basic starter project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d754f-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d754f-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d754f-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="d754f-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="d754f-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="d754f-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="d754f-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="d754f-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d754f-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d754f-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="d754f-135">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="d754f-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="d754f-136">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="d754f-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="d754f-137">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-137">Select **Yes**</span></span>

  * <span data-ttu-id="d754f-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="d754f-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="d754f-139">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="d754f-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="d754f-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d754f-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d754f-141">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="d754f-143">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式（模型-視圖控制器）**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="d754f-143">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="d754f-144">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式] [web 應用程式（模型-視圖控制器）**  >  **] 下一步**。</span><span class="sxs-lookup"><span data-stu-id="d754f-144">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS web 應用程式範本選取專案](start-mvc/_static/web_app_template_vsmac.png)

* <span data-ttu-id="d754f-146">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="d754f-146">Confirm the following configurations:</span></span>

  * <span data-ttu-id="d754f-147">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="d754f-147">**Target Framework** set to **.NET Core 3.1**.</span></span>
  * <span data-ttu-id="d754f-148">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="d754f-148">**Authentication** set to **No Authentication**.</span></span>
   
  <span data-ttu-id="d754f-149">選取 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="d754f-149">Select **Next**.</span></span>

  ![macOS .NET Core 3.1 選項](start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="d754f-151">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-151">Name the project **MvcMovie**, and then select **Create**.</span></span>

  ![macOS 將專案命名為](start-mvc/_static/MvcMovie.png)

---

### <a name="run-the-app"></a><span data-ttu-id="d754f-153">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="d754f-153">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d754f-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d754f-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d754f-155">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-155">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="d754f-156">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d754f-157">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="d754f-157">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d754f-158">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d754f-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d754f-159">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="d754f-159">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="d754f-160">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-160">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d754f-161">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-161">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="d754f-162">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="d754f-162">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="d754f-164">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="d754f-164">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="d754f-166">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="d754f-166">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="d754f-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d754f-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d754f-169">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="d754f-169">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="d754f-170">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="d754f-170">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="d754f-171">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="d754f-171">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="d754f-172">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d754f-172">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d754f-173">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="d754f-173">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="d754f-174">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-174">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d754f-175">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-175">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="d754f-177">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d754f-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d754f-178">選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="d754f-179">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="d754f-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d754f-180">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="d754f-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d754f-181">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d754f-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d754f-182">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="d754f-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="d754f-183">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="d754f-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="d754f-184">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="d754f-185">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="d754f-185">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="d754f-187">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="d754f-187">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d754f-188">下一步</span><span class="sxs-lookup"><span data-stu-id="d754f-188">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="d754f-189">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="d754f-189">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="d754f-190">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="d754f-190">The app manages a database of movie titles.</span></span> <span data-ttu-id="d754f-191">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="d754f-191">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d754f-192">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-192">Create a web app.</span></span>
> * <span data-ttu-id="d754f-193">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="d754f-193">Add and scaffold a model.</span></span>
> * <span data-ttu-id="d754f-194">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="d754f-194">Work with a database.</span></span>
> * <span data-ttu-id="d754f-195">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="d754f-195">Add search and validation.</span></span>

<span data-ttu-id="d754f-196">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-196">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="d754f-197">必要條件</span><span class="sxs-lookup"><span data-stu-id="d754f-197">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d754f-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d754f-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="d754f-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d754f-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="d754f-200">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d754f-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="d754f-201">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d754f-201">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d754f-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d754f-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d754f-203">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-203">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="d754f-204">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-204">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="d754f-206">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-206">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="d754f-207">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="d754f-207">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="d754f-209">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-209">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="d754f-210">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="d754f-210">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="d754f-211">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="d754f-211">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="d754f-212">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-212">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="d754f-213">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="d754f-213">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d754f-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d754f-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d754f-215">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="d754f-215">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="d754f-216">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="d754f-216">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="d754f-217">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="d754f-217">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d754f-218">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d754f-218">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="d754f-219">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="d754f-219">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="d754f-220">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="d754f-220">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="d754f-221">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-221">Select **Yes**</span></span>

  * <span data-ttu-id="d754f-222">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="d754f-222">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="d754f-223">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="d754f-223">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="d754f-224">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d754f-224">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d754f-225">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-225">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="d754f-227">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式（模型-視圖控制器）**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="d754f-227">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="d754f-228">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式] [web 應用程式（模型-視圖控制器）**  >  **] 下一步**。</span><span class="sxs-lookup"><span data-stu-id="d754f-228">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

* <span data-ttu-id="d754f-229">在 [**設定新的 ASP.NET Core WEB API** ] 對話方塊中，接受預設的 [ **.net Core 2.2**]**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="d754f-229">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="d754f-231">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d754f-231">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="d754f-232">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="d754f-232">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d754f-233">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d754f-233">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d754f-234">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-234">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="d754f-235">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-235">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d754f-236">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="d754f-236">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d754f-237">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d754f-237">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d754f-238">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="d754f-238">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="d754f-239">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-239">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d754f-240">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-240">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="d754f-241">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="d754f-241">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="d754f-243">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="d754f-243">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="d754f-245">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="d754f-245">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="d754f-246">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="d754f-246">This app doesn't track personal information.</span></span> <span data-ttu-id="d754f-247">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="d754f-247">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="d754f-249">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="d754f-249">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="d754f-251">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d754f-251">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d754f-252">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="d754f-252">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="d754f-253">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="d754f-253">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="d754f-254">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="d754f-254">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="d754f-255">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d754f-255">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d754f-256">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="d754f-256">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="d754f-257">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-257">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d754f-258">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="d754f-258">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="d754f-259">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="d754f-259">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="d754f-260">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="d754f-260">This app doesn't track personal information.</span></span> <span data-ttu-id="d754f-261">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="d754f-261">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="d754f-263">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="d754f-263">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="d754f-265">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d754f-265">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d754f-266">選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-266">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="d754f-267">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="d754f-267">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d754f-268">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="d754f-268">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d754f-269">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d754f-269">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d754f-270">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="d754f-270">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="d754f-271">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="d754f-271">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="d754f-272">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d754f-272">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="d754f-273">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="d754f-273">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="d754f-274">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="d754f-274">This app doesn't track personal information.</span></span> <span data-ttu-id="d754f-275">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="d754f-275">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="d754f-277">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="d754f-277">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="d754f-279">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="d754f-279">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d754f-280">下一步</span><span class="sxs-lookup"><span data-stu-id="d754f-280">Next</span></span>](adding-controller.md)

::: moniker-end
