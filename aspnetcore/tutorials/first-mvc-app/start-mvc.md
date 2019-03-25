---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dbc07558d7d7672e60e8834dc3e4e9d8aab437e3
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265280"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="a50ae-103">ASP.NET Core MVC 使用者入門</span><span class="sxs-lookup"><span data-stu-id="a50ae-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="a50ae-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a50ae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="a50ae-105">本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a50ae-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="a50ae-106">此應用程式會管理電影標題的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a50ae-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="a50ae-107">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="a50ae-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a50ae-108">建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a50ae-108">Create a web app.</span></span>
> * <span data-ttu-id="a50ae-109">新增並建構模型。</span><span class="sxs-lookup"><span data-stu-id="a50ae-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="a50ae-110">使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="a50ae-110">Work with a database.</span></span>
> * <span data-ttu-id="a50ae-111">新增搜尋和驗證。</span><span class="sxs-lookup"><span data-stu-id="a50ae-111">Add search and validation.</span></span>

<span data-ttu-id="a50ae-112">結束時，您將會有一個可管理及顯示電影資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a50ae-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="a50ae-113">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="a50ae-113">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a50ae-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a50ae-114">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a50ae-115">從 Visual Studio 中，選取 [檔案] > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="a50ae-115">From Visual Studio, select  **File > New > Project**.</span></span>

![[檔案] > [新增] > [專案]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="a50ae-117">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="a50ae-117">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="a50ae-118">在左窗格中，選取 [.NET Core]</span><span class="sxs-lookup"><span data-stu-id="a50ae-118">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="a50ae-119">在中央窗格中，選取 [ASP.NET Core Web 應用程式 (.NET Core)]</span><span class="sxs-lookup"><span data-stu-id="a50ae-119">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="a50ae-120">將專案命名為 "MvcMovie" (請務必將專案命名為 "MvcMovie"，以便複製程式碼時命名空間相符)。</span><span class="sxs-lookup"><span data-stu-id="a50ae-120">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="a50ae-121">選取 [確定]</span><span class="sxs-lookup"><span data-stu-id="a50ae-121">select **OK**</span></span>

![<span data-ttu-id="a50ae-122">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="a50ae-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="a50ae-123">完成 [新增 ASP.NET Core Web 應用程式 (.NET Core) - MvcMovie] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="a50ae-123">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a50ae-124">在版本選取器下拉式清單方塊中，選取 [ASP.NET Core 2.2]</span><span class="sxs-lookup"><span data-stu-id="a50ae-124">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="a50ae-125">選取 [Web 應用程式 (模型-檢視-控制器)]</span><span class="sxs-lookup"><span data-stu-id="a50ae-125">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="a50ae-126">選取 [確定]</span><span class="sxs-lookup"><span data-stu-id="a50ae-126">select **OK**.</span></span>

![<span data-ttu-id="a50ae-127">[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="a50ae-127">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="a50ae-128">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="a50ae-128">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="a50ae-129">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a50ae-129">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a50ae-130">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="a50ae-130">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a50ae-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a50ae-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a50ae-132">本教學課程假設您熟悉 VS Code。</span><span class="sxs-lookup"><span data-stu-id="a50ae-132">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="a50ae-133">如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="a50ae-133">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="a50ae-134">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="a50ae-134">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a50ae-135">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a50ae-135">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a50ae-136">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a50ae-136">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="a50ae-137">對話方塊隨即顯示，並指出 **'MvcMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="a50ae-137">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="a50ae-138">選取 [是]</span><span class="sxs-lookup"><span data-stu-id="a50ae-138">Select **Yes**</span></span>

  * <span data-ttu-id="a50ae-139">`dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="a50ae-139">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="a50ae-140">`code -r MvcMovie`：在 Visual Studio Code 中載入 *MvcMovie.csproj* 專案檔。</span><span class="sxs-lookup"><span data-stu-id="a50ae-140">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a50ae-141">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a50ae-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a50ae-142">選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="a50ae-142">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="a50ae-144">選取 [.NET Core 應用程式] > [ASP.NET Core] > [ASP.NET Core Web 應用程式 (MVC)] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="a50ae-144">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="a50ae-146">在 [設定您的新 ASP.NET Core Web API] 對話方塊中，接受 [目標 Framework] 的預設 \**.NET Core 2.2*。</span><span class="sxs-lookup"><span data-stu-id="a50ae-146">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="a50ae-147">將專案命名為 **MvcMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a50ae-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="a50ae-148">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a50ae-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a50ae-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a50ae-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a50ae-150">選取 **Ctrl-F5** 以非偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a50ae-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="a50ae-151">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a50ae-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="a50ae-152">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a50ae-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a50ae-153">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a50ae-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a50ae-154">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="a50ae-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="a50ae-155">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="a50ae-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a50ae-156">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="a50ae-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a50ae-157">您可以從 [偵錯] 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="a50ae-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a50ae-159">您可以選取 [IIS Express] 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="a50ae-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a50ae-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a50ae-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a50ae-162">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a50ae-162">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="a50ae-163">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="a50ae-163">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="a50ae-164">位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a50ae-164">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="a50ae-165">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a50ae-165">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="a50ae-166">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="a50ae-166">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="a50ae-167">使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="a50ae-167">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a50ae-168">許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="a50ae-168">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a50ae-169">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a50ae-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a50ae-170">選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a50ae-170">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="a50ae-171">Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="a50ae-171">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="a50ae-172">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a50ae-172">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a50ae-173">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a50ae-173">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a50ae-174">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="a50ae-174">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a50ae-175">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="a50ae-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a50ae-176">您可以從 [執行] 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="a50ae-176">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---

* <span data-ttu-id="a50ae-177">選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="a50ae-177">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a50ae-178">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="a50ae-178">This app doesn't track personal information.</span></span> <span data-ttu-id="a50ae-179">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="a50ae-179">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  <span data-ttu-id="a50ae-181">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a50ae-181">The following image shows the app after accepting tracking:</span></span>

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="a50ae-183">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="a50ae-183">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a50ae-184">下一步</span><span class="sxs-lookup"><span data-stu-id="a50ae-184">Next</span></span>](adding-controller.md)
