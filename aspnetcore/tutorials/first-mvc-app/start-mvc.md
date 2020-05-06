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
ms.openlocfilehash: 0b28e99d9c56d92e157e9010880e58ec5165196a
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774981"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="a55ed-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="a55ed-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="a55ed-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a55ed-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="a55ed-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a55ed-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="a55ed-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a55ed-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="a55ed-107">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="a55ed-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a55ed-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-108">Create a web app.</span></span>
> * <span data-ttu-id="a55ed-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="a55ed-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="a55ed-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="a55ed-110">Work with a database.</span></span>
> * <span data-ttu-id="a55ed-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="a55ed-111">Add search and validation.</span></span>

<span data-ttu-id="a55ed-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="a55ed-113">先決條件</span><span class="sxs-lookup"><span data-stu-id="a55ed-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a55ed-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a55ed-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a55ed-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a55ed-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a55ed-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a55ed-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="a55ed-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a55ed-117">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a55ed-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a55ed-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a55ed-119">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="a55ed-120">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="a55ed-122">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="a55ed-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="a55ed-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="a55ed-125">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="a55ed-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="a55ed-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="a55ed-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="a55ed-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="a55ed-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a55ed-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="a55ed-129">This is a basic starter project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a55ed-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a55ed-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a55ed-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="a55ed-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="a55ed-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="a55ed-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="a55ed-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="a55ed-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a55ed-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a55ed-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a55ed-135">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="a55ed-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="a55ed-136">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="a55ed-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="a55ed-137">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-137">Select **Yes**</span></span>

  * <span data-ttu-id="a55ed-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="a55ed-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="a55ed-139">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="a55ed-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a55ed-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a55ed-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a55ed-141">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="a55ed-143">選取 [.Net Core]\*\* [應用程式]\*\* > \*\* [Web應用程式 (Model-View-Controller)]\*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="a55ed-145">在 [設定**您的新 ASP.NET Core WEB API** ] 對話方塊中，設定 **.net Core 3.1**的**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="a55ed-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

  ![macOS .NET Core 3.1 選項](./start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="a55ed-147">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="a55ed-148">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a55ed-148">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a55ed-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a55ed-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a55ed-150">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="a55ed-151">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="a55ed-152">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a55ed-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a55ed-153">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a55ed-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a55ed-154">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="a55ed-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="a55ed-155">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a55ed-156">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a55ed-157">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="a55ed-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a55ed-159">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="a55ed-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="a55ed-161">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="a55ed-161">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="a55ed-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a55ed-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a55ed-164">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a55ed-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="a55ed-165">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a55ed-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="a55ed-166">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a55ed-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="a55ed-167">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a55ed-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="a55ed-168">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="a55ed-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="a55ed-169">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a55ed-170">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a55ed-172">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a55ed-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a55ed-173">選取 [**執行** > ] [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="a55ed-174">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="a55ed-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="a55ed-175">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a55ed-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a55ed-176">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a55ed-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a55ed-177">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="a55ed-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a55ed-178">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="a55ed-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a55ed-179">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="a55ed-180">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="a55ed-180">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="a55ed-182">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="a55ed-182">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a55ed-183">下一步</span><span class="sxs-lookup"><span data-stu-id="a55ed-183">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="a55ed-184">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a55ed-184">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="a55ed-185">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a55ed-185">The app manages a database of movie titles.</span></span> <span data-ttu-id="a55ed-186">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="a55ed-186">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a55ed-187">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-187">Create a web app.</span></span>
> * <span data-ttu-id="a55ed-188">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="a55ed-188">Add and scaffold a model.</span></span>
> * <span data-ttu-id="a55ed-189">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="a55ed-189">Work with a database.</span></span>
> * <span data-ttu-id="a55ed-190">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="a55ed-190">Add search and validation.</span></span>

<span data-ttu-id="a55ed-191">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-191">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="a55ed-192">先決條件</span><span class="sxs-lookup"><span data-stu-id="a55ed-192">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a55ed-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a55ed-193">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a55ed-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a55ed-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a55ed-195">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a55ed-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="a55ed-196">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a55ed-196">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a55ed-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a55ed-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a55ed-198">從 Visual Studio 中，選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-198">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="a55ed-199">依序選取 [ASP.NET Core Web 應用程式]\*\*\*\* 和 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-199">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="a55ed-201">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-201">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="a55ed-202">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="a55ed-202">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="a55ed-204">選取 [Web 應用程式 (Model-View-Controller)]\*\*\*\*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-204">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="a55ed-205">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="a55ed-205">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="a55ed-206">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="a55ed-206">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="a55ed-207">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-207">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a55ed-208">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="a55ed-208">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a55ed-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a55ed-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a55ed-210">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="a55ed-210">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="a55ed-211">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="a55ed-211">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="a55ed-212">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="a55ed-212">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a55ed-213">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a55ed-213">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a55ed-214">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="a55ed-214">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="a55ed-215">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="a55ed-215">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="a55ed-216">選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-216">Select **Yes**</span></span>

  * <span data-ttu-id="a55ed-217">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="a55ed-217">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="a55ed-218">`code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。</span><span class="sxs-lookup"><span data-stu-id="a55ed-218">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a55ed-219">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a55ed-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a55ed-220">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-220">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="a55ed-222">選取 [.Net Core]\*\* [應用程式]\*\* > \*\* [Web應用程式 (Model-View-Controller)]\*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-222">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="a55ed-224">在 [**設定新的 ASP.NET Core WEB API** ] 對話方塊中，接受預設的 [ **.net Core 2.2**]**目標 Framework** 。</span><span class="sxs-lookup"><span data-stu-id="a55ed-224">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="a55ed-226">將專案命名為 **MvcMovie**，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a55ed-226">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="a55ed-227">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a55ed-227">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a55ed-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a55ed-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a55ed-229">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-229">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="a55ed-230">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="a55ed-231">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a55ed-231">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a55ed-232">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a55ed-232">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a55ed-233">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="a55ed-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="a55ed-234">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-234">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a55ed-235">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-235">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a55ed-236">您可以從 [偵錯]\*\*\*\* 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="a55ed-236">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a55ed-238">您可以選取 [IIS Express]\*\*\*\* 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="a55ed-238">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="a55ed-240">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="a55ed-240">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a55ed-241">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="a55ed-241">This app doesn't track personal information.</span></span> <span data-ttu-id="a55ed-242">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="a55ed-242">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="a55ed-244">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a55ed-244">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="a55ed-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a55ed-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a55ed-247">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a55ed-247">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="a55ed-248">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a55ed-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="a55ed-249">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a55ed-249">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="a55ed-250">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a55ed-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="a55ed-251">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="a55ed-251">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="a55ed-252">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-252">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a55ed-253">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="a55ed-253">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="a55ed-254">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="a55ed-254">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a55ed-255">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="a55ed-255">This app doesn't track personal information.</span></span> <span data-ttu-id="a55ed-256">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="a55ed-256">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="a55ed-258">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a55ed-258">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a55ed-260">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a55ed-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a55ed-261">選取 [**執行** > ] [**啟動但不**進行偵測] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-261">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="a55ed-262">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="a55ed-262">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="a55ed-263">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a55ed-263">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a55ed-264">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a55ed-264">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a55ed-265">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="a55ed-265">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a55ed-266">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="a55ed-266">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a55ed-267">您可以從 [執行]\*\*\*\* 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a55ed-267">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="a55ed-268">選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="a55ed-268">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a55ed-269">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="a55ed-269">This app doesn't track personal information.</span></span> <span data-ttu-id="a55ed-270">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="a55ed-270">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="a55ed-272">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a55ed-272">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="a55ed-274">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="a55ed-274">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a55ed-275">下一步</span><span class="sxs-lookup"><span data-stu-id="a55ed-275">Next</span></span>](adding-controller.md)

::: moniker-end
