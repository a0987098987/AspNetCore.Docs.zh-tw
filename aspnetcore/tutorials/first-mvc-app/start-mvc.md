---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f6a92423546ebd9d4c8e1a92fb81b6b72f847f61
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820095"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="c8744-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="c8744-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="c8744-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c8744-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="c8744-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="c8744-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="c8744-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8744-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="c8744-107">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="c8744-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c8744-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-108">Create a web app.</span></span>
> * <span data-ttu-id="c8744-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="c8744-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="c8744-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8744-110">Work with a database.</span></span>
> * <span data-ttu-id="c8744-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="c8744-111">Add search and validation.</span></span>

<span data-ttu-id="c8744-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="c8744-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="c8744-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8744-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8744-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8744-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8744-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8744-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8744-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="c8744-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c8744-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8744-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8744-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c8744-119">從 Visual Studio 中，選取 [建立新專案]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="c8744-120">依序選取 [ASP.NET Core Web 應用程式]  和 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="c8744-122">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="c8744-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="c8744-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* <span data-ttu-id="c8744-125">選取 [Web 應用程式 (Model-View-Controller)]  ，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="c8744-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="c8744-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="c8744-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="c8744-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="c8744-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="c8744-129">這是基本的入門專案。</span><span class="sxs-lookup"><span data-stu-id="c8744-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8744-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8744-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c8744-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="c8744-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="c8744-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="c8744-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="c8744-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="c8744-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="c8744-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c8744-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="c8744-135">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c8744-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="c8744-136">對話方塊隨即顯示，並指出 **'MvcMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="c8744-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="c8744-137">選取 [是] </span><span class="sxs-lookup"><span data-stu-id="c8744-137">Select **Yes**</span></span>

  * <span data-ttu-id="c8744-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="c8744-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="c8744-139">`code -r MvcMovie`：在 Visual Studio Code 中載入 *MvcMovie.csproj* 專案檔。</span><span class="sxs-lookup"><span data-stu-id="c8744-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8744-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8744-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c8744-141">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="c8744-143">選取 [.Net Core]  > [應用程式]  > [Web應用程式 (Model-View-Controller)]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="c8744-145">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，設定 **.NET Core 3.0** 的 [目標 Framework]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="c8744-146">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="c8744-147">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c8744-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8744-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8744-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c8744-149">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="c8744-150">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c8744-151">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c8744-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c8744-152">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c8744-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c8744-153">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="c8744-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="c8744-154">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c8744-155">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="c8744-156">您可以從 [偵錯]  功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="c8744-158">您可以選取 [IIS Express]  按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="c8744-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)


  <span data-ttu-id="c8744-160">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-160">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8744-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8744-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c8744-163">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c8744-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="c8744-164">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="c8744-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="c8744-165">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c8744-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="c8744-166">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c8744-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="c8744-167">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="c8744-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="c8744-168">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c8744-169">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="c8744-170">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="c8744-170">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="c8744-171">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="c8744-171">This app doesn't track personal information.</span></span> <span data-ttu-id="c8744-172">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="c8744-172">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="c8744-174">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-174">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8744-176">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8744-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c8744-177">選取 [執行]   > [啟動但不偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="c8744-178">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="c8744-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="c8744-179">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c8744-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c8744-180">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c8744-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c8744-181">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="c8744-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c8744-182">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="c8744-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c8744-183">您可以從 [執行]  功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="c8744-184">下圖顯示應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-184">The following image shows the app:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="c8744-186">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="c8744-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8744-187">下一步</span><span class="sxs-lookup"><span data-stu-id="c8744-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="c8744-188">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="c8744-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="c8744-189">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8744-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="c8744-190">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="c8744-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c8744-191">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-191">Create a web app.</span></span>
> * <span data-ttu-id="c8744-192">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="c8744-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="c8744-193">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8744-193">Work with a database.</span></span>
> * <span data-ttu-id="c8744-194">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="c8744-194">Add search and validation.</span></span>

<span data-ttu-id="c8744-195">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="c8744-196">必要條件</span><span class="sxs-lookup"><span data-stu-id="c8744-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8744-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8744-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8744-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8744-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8744-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8744-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="c8744-200">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c8744-200">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8744-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8744-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c8744-202">從 Visual Studio 中，選取 [建立新專案]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="c8744-203">依序選取 [ASP.NET Core Web 應用程式]  和 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-203">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="c8744-205">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="c8744-206">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="c8744-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="c8744-208">選取 [Web 應用程式 (Model-View-Controller)]  ，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="c8744-209">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="c8744-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="c8744-210">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="c8744-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="c8744-211">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="c8744-212">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="c8744-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8744-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8744-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c8744-214">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="c8744-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="c8744-215">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="c8744-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="c8744-216">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="c8744-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="c8744-217">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c8744-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="c8744-218">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c8744-218">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="c8744-219">對話方塊隨即顯示，並指出 **'MvcMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="c8744-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="c8744-220">選取 [是] </span><span class="sxs-lookup"><span data-stu-id="c8744-220">Select **Yes**</span></span>

  * <span data-ttu-id="c8744-221">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="c8744-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="c8744-222">`code -r MvcMovie`：在 Visual Studio Code 中載入 *MvcMovie.csproj* 專案檔。</span><span class="sxs-lookup"><span data-stu-id="c8744-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8744-223">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8744-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c8744-224">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-224">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="c8744-226">選取 [.Net Core]  > [應用程式]  > [Web應用程式 (Model-View-Controller)]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-226">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="c8744-228">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，接受 **.NET Core 2.2** 的預設**目標 Framework**。</span><span class="sxs-lookup"><span data-stu-id="c8744-228">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="c8744-230">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="c8744-230">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="c8744-231">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c8744-231">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8744-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8744-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c8744-233">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-233">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="c8744-234">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-234">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c8744-235">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c8744-235">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c8744-236">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c8744-236">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c8744-237">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="c8744-237">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="c8744-238">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-238">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c8744-239">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-239">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="c8744-240">您可以從 [偵錯]  功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-240">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="c8744-242">您可以選取 [IIS Express]  按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="c8744-242">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="c8744-244">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="c8744-244">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="c8744-245">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="c8744-245">This app doesn't track personal information.</span></span> <span data-ttu-id="c8744-246">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="c8744-246">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="c8744-248">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-248">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8744-250">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8744-250">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c8744-251">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c8744-251">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="c8744-252">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="c8744-252">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="c8744-253">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c8744-253">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="c8744-254">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c8744-254">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="c8744-255">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="c8744-255">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="c8744-256">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-256">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c8744-257">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="c8744-257">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="c8744-258">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="c8744-258">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="c8744-259">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="c8744-259">This app doesn't track personal information.</span></span> <span data-ttu-id="c8744-260">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="c8744-260">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="c8744-262">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-262">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8744-264">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c8744-264">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c8744-265">選取 [執行]   > [啟動但不偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-265">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="c8744-266">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="c8744-266">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="c8744-267">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="c8744-267">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c8744-268">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="c8744-268">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c8744-269">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="c8744-269">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c8744-270">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="c8744-270">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c8744-271">您可以從 [執行]  功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8744-271">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="c8744-272">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="c8744-272">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="c8744-273">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="c8744-273">This app doesn't track personal information.</span></span> <span data-ttu-id="c8744-274">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="c8744-274">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="c8744-276">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c8744-276">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="c8744-278">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="c8744-278">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8744-279">下一步</span><span class="sxs-lookup"><span data-stu-id="c8744-279">Next</span></span>](adding-controller.md)

::: moniker-end
