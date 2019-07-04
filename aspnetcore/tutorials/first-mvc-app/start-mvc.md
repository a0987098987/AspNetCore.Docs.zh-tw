---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 91564bac02df77632a3b56b6dbddeb3b622f6438
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555857"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="8b152-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="8b152-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="8b152-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8b152-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="8b152-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="8b152-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="8b152-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8b152-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="8b152-107">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="8b152-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8b152-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b152-108">Create a web app.</span></span>
> * <span data-ttu-id="8b152-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="8b152-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="8b152-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="8b152-110">Work with a database.</span></span>
> * <span data-ttu-id="8b152-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="8b152-111">Add search and validation.</span></span>

<span data-ttu-id="8b152-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b152-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="8b152-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="8b152-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b152-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b152-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b152-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b152-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b152-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8b152-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="8b152-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8b152-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b152-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b152-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b152-119">從 Visual Studio 中，選取 [建立新專案]  。</span><span class="sxs-lookup"><span data-stu-id="8b152-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="8b152-120">依序選取 [ASP.NET Core Web 應用程式]  和 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="8b152-120">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="8b152-122">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="8b152-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="8b152-123">請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。</span><span class="sxs-lookup"><span data-stu-id="8b152-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* <span data-ttu-id="8b152-125">選取 [Web 應用程式 (Model-View-Controller)]  ，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="8b152-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="8b152-126">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8b152-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="8b152-127">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="8b152-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="8b152-128">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b152-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8b152-129">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="8b152-129">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b152-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b152-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8b152-131">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="8b152-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="8b152-132">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="8b152-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="8b152-133">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="8b152-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="8b152-134">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8b152-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="8b152-135">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8b152-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="8b152-136">對話方塊隨即顯示，並指出 **'MvcMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="8b152-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="8b152-137">選取 [是] </span><span class="sxs-lookup"><span data-stu-id="8b152-137">Select **Yes**</span></span>

  * <span data-ttu-id="8b152-138">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="8b152-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="8b152-139">`code -r MvcMovie`：在 Visual Studio Code 中載入 *MvcMovie.csproj* 專案檔。</span><span class="sxs-lookup"><span data-stu-id="8b152-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b152-140">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8b152-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8b152-141">選取 [檔案]   > [新增方案]  。</span><span class="sxs-lookup"><span data-stu-id="8b152-141">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="8b152-143">選取 [.NET Core]   > [應用程式]   > [Web 應用程式 (Model-View-Controller)]   > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="8b152-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="8b152-145">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，接受 **.NET Core 2.2** 的預設**目標 Framework**。</span><span class="sxs-lookup"><span data-stu-id="8b152-145">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 選取項目](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="8b152-147">將專案命名為 **MvcMovie**，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="8b152-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="8b152-148">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="8b152-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b152-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b152-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8b152-150">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b152-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="8b152-151">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b152-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="8b152-152">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="8b152-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8b152-153">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="8b152-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8b152-154">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="8b152-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="8b152-155">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="8b152-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8b152-156">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="8b152-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8b152-157">您可以從 [偵錯]  功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="8b152-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8b152-159">您可以選取 [IIS Express]  按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="8b152-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="8b152-161">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="8b152-161">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="8b152-162">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="8b152-162">This app doesn't track personal information.</span></span> <span data-ttu-id="8b152-163">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="8b152-163">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="8b152-165">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8b152-165">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b152-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b152-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8b152-168">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="8b152-168">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="8b152-169">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="8b152-169">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="8b152-170">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="8b152-170">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="8b152-171">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="8b152-171">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="8b152-172">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="8b152-172">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="8b152-173">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="8b152-173">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8b152-174">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="8b152-174">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="8b152-175">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="8b152-175">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="8b152-176">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="8b152-176">This app doesn't track personal information.</span></span> <span data-ttu-id="8b152-177">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="8b152-177">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="8b152-179">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8b152-179">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b152-181">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8b152-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8b152-182">選取 [執行]   > [啟動但不偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b152-182">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="8b152-183">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="8b152-183">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="8b152-184">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="8b152-184">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8b152-185">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="8b152-185">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8b152-186">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="8b152-186">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8b152-187">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="8b152-187">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8b152-188">您可以從 [執行]  功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b152-188">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="8b152-189">選取 [接受]  同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="8b152-189">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="8b152-190">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="8b152-190">This app doesn't track personal information.</span></span> <span data-ttu-id="8b152-191">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="8b152-191">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="8b152-193">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8b152-193">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="8b152-195">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="8b152-195">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8b152-196">下一步</span><span class="sxs-lookup"><span data-stu-id="8b152-196">Next</span></span>](adding-controller.md)
