---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 10/16/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: c2b76b59ae775b9268fa77019bf8420e5e4108b6
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84452270"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="13289-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="13289-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="13289-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="13289-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="13289-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="13289-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="13289-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="13289-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="13289-107">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="13289-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13289-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-108">Create a web app.</span></span>
> * <span data-ttu-id="13289-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="13289-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="13289-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="13289-110">Work with a database.</span></span>
> * <span data-ttu-id="13289-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="13289-111">Add search and validation.</span></span>

<span data-ttu-id="13289-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="13289-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="13289-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13289-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13289-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="13289-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13289-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13289-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="13289-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="13289-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="13289-117">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13289-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13289-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="13289-119">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="13289-120">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="13289-122">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="13289-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="13289-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="13289-125">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="13289-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="13289-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="13289-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="13289-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="13289-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="13289-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="13289-129">This is a basic starter project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="13289-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13289-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="13289-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="13289-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="13289-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="13289-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="13289-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="13289-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="13289-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="13289-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="13289-135">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="13289-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="13289-136">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="13289-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="13289-137">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-137">Select **Yes**</span></span>

  * <span data-ttu-id="13289-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="13289-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="13289-139">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="13289-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13289-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="13289-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="13289-141">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="13289-143">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式（模型-視圖控制器）**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="13289-143">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="13289-144">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式] [web 應用程式（模型-視圖控制器）**  >  **] 下一步**。</span><span class="sxs-lookup"><span data-stu-id="13289-144">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS web 應用程式範本選取專案](start-mvc/_static/web_app_template_vsmac.png)

* <span data-ttu-id="13289-146">確認下列設定：</span><span class="sxs-lookup"><span data-stu-id="13289-146">Confirm the following configurations:</span></span>

  * <span data-ttu-id="13289-147">設定為 **.Net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="13289-147">**Target Framework** set to **.NET Core 3.1**.</span></span>
  * <span data-ttu-id="13289-148">**驗證**設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="13289-148">**Authentication** set to **No Authentication**.</span></span>
   
  <span data-ttu-id="13289-149">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="13289-149">Select **Next**.</span></span>

  ![macOS .NET Core 3.1 選項](start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="13289-151">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-151">Name the project **MvcMovie**, and then select **Create**.</span></span>

  ![macOS 將專案命名為](start-mvc/_static/MvcMovie.png)

---

### <a name="run-the-app"></a><span data-ttu-id="13289-153">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="13289-153">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13289-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13289-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="13289-155">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-155">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="13289-156">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="13289-157">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="13289-157">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="13289-158">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="13289-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="13289-159">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="13289-159">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="13289-160">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="13289-160">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="13289-161">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="13289-161">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="13289-162">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="13289-162">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="13289-164">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="13289-164">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="13289-166">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="13289-166">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="13289-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13289-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="13289-169">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="13289-169">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="13289-170">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="13289-170">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="13289-171">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="13289-171">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="13289-172">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="13289-172">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="13289-173">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="13289-173">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="13289-174">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="13289-174">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="13289-175">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="13289-175">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13289-177">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="13289-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="13289-178">選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="13289-179">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="13289-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="13289-180">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="13289-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="13289-181">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="13289-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="13289-182">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="13289-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="13289-183">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="13289-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="13289-184">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="13289-185">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="13289-185">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="13289-187">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="13289-187">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="13289-188">下一步</span><span class="sxs-lookup"><span data-stu-id="13289-188">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="13289-189">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="13289-189">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="13289-190">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="13289-190">The app manages a database of movie titles.</span></span> <span data-ttu-id="13289-191">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="13289-191">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13289-192">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-192">Create a web app.</span></span>
> * <span data-ttu-id="13289-193">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="13289-193">Add and scaffold a model.</span></span>
> * <span data-ttu-id="13289-194">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="13289-194">Work with a database.</span></span>
> * <span data-ttu-id="13289-195">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="13289-195">Add search and validation.</span></span>

<span data-ttu-id="13289-196">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-196">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="13289-197">必要條件</span><span class="sxs-lookup"><span data-stu-id="13289-197">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13289-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13289-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="13289-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13289-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13289-200">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="13289-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="13289-201">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="13289-201">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13289-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13289-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="13289-203">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-203">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="13289-204">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-204">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="13289-206">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-206">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="13289-207">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="13289-207">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="13289-209">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-209">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="13289-210">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="13289-210">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="13289-211">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="13289-211">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="13289-212">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-212">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="13289-213">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="13289-213">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="13289-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13289-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="13289-215">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="13289-215">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="13289-216">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="13289-216">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="13289-217">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="13289-217">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="13289-218">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="13289-218">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="13289-219">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="13289-219">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="13289-220">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="13289-220">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="13289-221">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-221">Select **Yes**</span></span>

  * <span data-ttu-id="13289-222">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="13289-222">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="13289-223">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="13289-223">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13289-224">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="13289-224">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="13289-225">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-225">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="13289-227">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式（模型-視圖控制器）**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="13289-227">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="13289-228">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式] [web 應用程式（模型-視圖控制器）**  >  **] 下一步**。</span><span class="sxs-lookup"><span data-stu-id="13289-228">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

* <span data-ttu-id="13289-229">在 [**設定新的 ASP.NET Core WEB API** ] 對話方塊中，接受預設的 [ **.net Core 2.2**]**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="13289-229">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="13289-231">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="13289-231">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="13289-232">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="13289-232">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="13289-233">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13289-233">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="13289-234">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-234">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="13289-235">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-235">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="13289-236">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="13289-236">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="13289-237">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="13289-237">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="13289-238">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="13289-238">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="13289-239">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="13289-239">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="13289-240">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="13289-240">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="13289-241">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="13289-241">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="13289-243">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="13289-243">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="13289-245">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="13289-245">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="13289-246">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="13289-246">This app doesn't track personal information.</span></span> <span data-ttu-id="13289-247">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="13289-247">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="13289-249">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="13289-249">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="13289-251">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="13289-251">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="13289-252">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="13289-252">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="13289-253">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="13289-253">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="13289-254">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="13289-254">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="13289-255">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="13289-255">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="13289-256">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="13289-256">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="13289-257">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="13289-257">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="13289-258">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="13289-258">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="13289-259">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="13289-259">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="13289-260">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="13289-260">This app doesn't track personal information.</span></span> <span data-ttu-id="13289-261">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="13289-261">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="13289-263">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="13289-263">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="13289-265">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="13289-265">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="13289-266">選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-266">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="13289-267">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="13289-267">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="13289-268">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="13289-268">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="13289-269">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="13289-269">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="13289-270">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="13289-270">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="13289-271">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="13289-271">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="13289-272">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="13289-272">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="13289-273">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="13289-273">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="13289-274">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="13289-274">This app doesn't track personal information.</span></span> <span data-ttu-id="13289-275">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="13289-275">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="13289-277">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="13289-277">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="13289-279">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="13289-279">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="13289-280">下一步</span><span class="sxs-lookup"><span data-stu-id="13289-280">Next</span></span>](adding-controller.md)

::: moniker-end
