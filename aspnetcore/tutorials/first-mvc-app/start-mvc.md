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
ms.openlocfilehash: d4eb1744b1186704603430584b3da0793f90ee49
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86213079"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="ff75f-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="ff75f-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="ff75f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ff75f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ff75f-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="ff75f-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ff75f-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ff75f-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="ff75f-107">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="ff75f-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ff75f-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-108">Create a web app.</span></span>
> * <span data-ttu-id="ff75f-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="ff75f-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="ff75f-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="ff75f-110">Work with a database.</span></span>
> * <span data-ttu-id="ff75f-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="ff75f-111">Add search and validation.</span></span>

<span data-ttu-id="ff75f-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="ff75f-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="ff75f-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ff75f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff75f-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="ff75f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff75f-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ff75f-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ff75f-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="ff75f-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ff75f-117">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ff75f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff75f-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ff75f-119">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="ff75f-120">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="ff75f-122">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="ff75f-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="ff75f-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="ff75f-125">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="ff75f-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="ff75f-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="ff75f-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="ff75f-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="ff75f-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ff75f-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="ff75f-129">This is a basic starter project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="ff75f-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff75f-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ff75f-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="ff75f-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ff75f-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="ff75f-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="ff75f-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="ff75f-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ff75f-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff75f-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ff75f-135">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="ff75f-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="ff75f-136">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="ff75f-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="ff75f-137">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-137">Select **Yes**</span></span>

  * <span data-ttu-id="ff75f-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="ff75f-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="ff75f-139">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="ff75f-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ff75f-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ff75f-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ff75f-141">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ff75f-143">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式] ([模型-視圖) 控制器**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="ff75f-143">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="ff75f-144">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  \*\*程式] [web 應用程式] ([模型-視圖控制器) \*\*  >  **下一步]**</span><span class="sxs-lookup"><span data-stu-id="ff75f-144">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS web 應用程式範本選取專案](start-mvc/_static/web_app_template_vsmac.png)

* <span data-ttu-id="ff75f-146">在 [**設定您的新 Web 應用程式**] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="ff75f-146">In the **Configure your new Web Application** dialog:</span></span>

  * <span data-ttu-id="ff75f-147">確認 [**驗證**] 已設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="ff75f-147">Confirm that **Authentication** is set to **No Authentication**.</span></span>
  * <span data-ttu-id="ff75f-148">如果提供選取**目標 Framework**的選項，請選取最新的3.x 版。</span><span class="sxs-lookup"><span data-stu-id="ff75f-148">If presented an option to select a **Target Framework**, select the latest 3.x version.</span></span>

  <span data-ttu-id="ff75f-149">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="ff75f-149">Select **Next**.</span></span>

* <span data-ttu-id="ff75f-150">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-150">Name the project **MvcMovie**, and then select **Create**.</span></span>

  ![macOS 將專案命名為](start-mvc/_static/MvcMovie.png)

---

### <a name="run-the-app"></a><span data-ttu-id="ff75f-152">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="ff75f-152">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ff75f-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff75f-153">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ff75f-154">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-154">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="ff75f-155">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-155">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ff75f-156">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="ff75f-156">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ff75f-157">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="ff75f-157">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ff75f-158">當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="ff75f-158">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="ff75f-159">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-159">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ff75f-160">許多開發人員偏好使用非偵錯模式來快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-160">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ff75f-161">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff75f-161">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ff75f-163">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="ff75f-163">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="ff75f-165">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff75f-165">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="ff75f-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff75f-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ff75f-168">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="ff75f-168">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="ff75f-169">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="ff75f-169">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="ff75f-170">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="ff75f-170">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="ff75f-171">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="ff75f-171">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ff75f-172">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="ff75f-172">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="ff75f-173">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-173">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ff75f-174">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-174">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ff75f-176">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ff75f-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ff75f-177">選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ff75f-178">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="ff75f-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ff75f-179">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="ff75f-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ff75f-180">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="ff75f-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ff75f-181">當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="ff75f-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ff75f-182">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="ff75f-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ff75f-183">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="ff75f-184">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff75f-184">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="ff75f-186">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="ff75f-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ff75f-187">下一步</span><span class="sxs-lookup"><span data-stu-id="ff75f-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ff75f-188">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="ff75f-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ff75f-189">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="ff75f-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="ff75f-190">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="ff75f-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ff75f-191">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-191">Create a web app.</span></span>
> * <span data-ttu-id="ff75f-192">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="ff75f-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="ff75f-193">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="ff75f-193">Work with a database.</span></span>
> * <span data-ttu-id="ff75f-194">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="ff75f-194">Add search and validation.</span></span>

<span data-ttu-id="ff75f-195">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="ff75f-196">必要條件</span><span class="sxs-lookup"><span data-stu-id="ff75f-196">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ff75f-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff75f-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="ff75f-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff75f-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ff75f-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ff75f-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="ff75f-200">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ff75f-200">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ff75f-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff75f-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ff75f-202">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="ff75f-203">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-203">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="ff75f-205">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="ff75f-206">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="ff75f-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="ff75f-208">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="ff75f-209">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="ff75f-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="ff75f-210">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="ff75f-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="ff75f-211">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ff75f-212">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="ff75f-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="ff75f-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff75f-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ff75f-214">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="ff75f-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ff75f-215">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="ff75f-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="ff75f-216">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="ff75f-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ff75f-217">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff75f-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ff75f-218">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="ff75f-218">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="ff75f-219">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="ff75f-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="ff75f-220">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-220">Select **Yes**</span></span>

  * <span data-ttu-id="ff75f-221">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="ff75f-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="ff75f-222">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="ff75f-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ff75f-223">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ff75f-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ff75f-224">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-224">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ff75f-226">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式] ([模型-視圖) 控制器**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="ff75f-226">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="ff75f-227">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  \*\*程式] [web 應用程式] ([模型-視圖控制器) \*\*  >  **下一步]**</span><span class="sxs-lookup"><span data-stu-id="ff75f-227">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

* <span data-ttu-id="ff75f-228">在 [**設定您的新 Web 應用程式**] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="ff75f-228">In the **Configure your new Web Application** dialog:</span></span>

  * <span data-ttu-id="ff75f-229">確認 [**驗證**] 已設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="ff75f-229">Confirm that **Authentication** is set to **No Authentication**.</span></span>
  * <span data-ttu-id="ff75f-230">如果提供選取**目標 Framework**的選項，請選取最新的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="ff75f-230">If presented an option to select a **Target Framework**, select the latest 2.x version.</span></span>

  <span data-ttu-id="ff75f-231">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="ff75f-231">Select **Next**.</span></span>

* <span data-ttu-id="ff75f-232">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff75f-232">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="ff75f-233">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="ff75f-233">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ff75f-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff75f-234">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ff75f-235">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-235">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="ff75f-236">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ff75f-237">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="ff75f-237">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ff75f-238">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="ff75f-238">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ff75f-239">當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="ff75f-239">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="ff75f-240">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-240">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ff75f-241">許多開發人員偏好使用非偵錯模式來快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-241">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ff75f-242">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff75f-242">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ff75f-244">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="ff75f-244">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="ff75f-246">選取 [接受] 以同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="ff75f-246">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ff75f-247">此應用程式不會追蹤個人資訊。</span><span class="sxs-lookup"><span data-stu-id="ff75f-247">This app doesn't track personal information.</span></span> <span data-ttu-id="ff75f-248">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="ff75f-248">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="ff75f-250">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff75f-250">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="ff75f-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff75f-252">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ff75f-253">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="ff75f-253">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="ff75f-254">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="ff75f-254">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="ff75f-255">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="ff75f-255">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="ff75f-256">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="ff75f-256">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ff75f-257">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="ff75f-257">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="ff75f-258">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-258">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ff75f-259">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="ff75f-259">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="ff75f-260">選取 [接受] 以同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="ff75f-260">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ff75f-261">此應用程式不會追蹤個人資訊。</span><span class="sxs-lookup"><span data-stu-id="ff75f-261">This app doesn't track personal information.</span></span> <span data-ttu-id="ff75f-262">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="ff75f-262">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="ff75f-264">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff75f-264">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ff75f-266">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ff75f-266">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ff75f-267">選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-267">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ff75f-268">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="ff75f-268">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ff75f-269">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="ff75f-269">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ff75f-270">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="ff75f-270">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ff75f-271">當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="ff75f-271">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ff75f-272">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="ff75f-272">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ff75f-273">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff75f-273">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="ff75f-274">選取 [接受] 以同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="ff75f-274">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ff75f-275">此應用程式不會追蹤個人資訊。</span><span class="sxs-lookup"><span data-stu-id="ff75f-275">This app doesn't track personal information.</span></span> <span data-ttu-id="ff75f-276">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="ff75f-276">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="ff75f-278">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff75f-278">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="ff75f-280">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="ff75f-280">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ff75f-281">下一步</span><span class="sxs-lookup"><span data-stu-id="ff75f-281">Next</span></span>](adding-controller.md)

::: moniker-end
