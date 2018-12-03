---
title: ASP.NET Core MVC 與 Visual Studio 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC 與 Visual Studio。
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 9d50607899058c887597a3d73198552d3ef5b020
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710084"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="9b508-103">ASP.NET Core MVC 與 Visual Studio 使用者入門</span><span class="sxs-lookup"><span data-stu-id="9b508-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="9b508-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9b508-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="9b508-105">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="9b508-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="9b508-106">macOS：[使用 Visual Studio for Mac 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="9b508-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="9b508-107">Windows：[使用 Visual Studio 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="9b508-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="9b508-108">macOS、Linux 和 Windows：[使用 Visual Studio Code 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="9b508-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

> [!NOTE]
> <span data-ttu-id="9b508-109">我們正為規劃中的 ASP.NET Core 目錄新結構測試其可用性。</span><span class="sxs-lookup"><span data-stu-id="9b508-109">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="9b508-110">如果您有幾分鐘的時間可以嘗試在目前或建議的目錄中尋找 7 個不同主題，請[按一下這裡參加研究](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5)。</span><span class="sxs-lookup"><span data-stu-id="9b508-110">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="9b508-111">安裝 Visual Studio 和 .NET Core</span><span class="sxs-lookup"><span data-stu-id="9b508-111">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="9b508-112">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9b508-112">Create a web app</span></span>

<span data-ttu-id="9b508-113">從 Visual Studio 中，選取 [檔案] > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="9b508-113">From Visual Studio, select  **File > New > Project**.</span></span>

![[檔案] > [新增] > [專案]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="9b508-115">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="9b508-115">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="9b508-116">在左窗格中，點選 [.NET Core]。</span><span class="sxs-lookup"><span data-stu-id="9b508-116">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="9b508-117">在中央窗格中，點選 [ASP.NET Core Web 應用程式 (.NET Core)]。</span><span class="sxs-lookup"><span data-stu-id="9b508-117">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="9b508-118">將專案命名為 "MvcMovie" (請務必將專案命名為 "MvcMovie"，以便複製程式碼時命名空間相符)。</span><span class="sxs-lookup"><span data-stu-id="9b508-118">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="9b508-119">點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="9b508-119">Tap **OK**</span></span>

![<span data-ttu-id="9b508-120">[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="9b508-120">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="9b508-121">完成 [新增 ASP.NET Core Web 應用程式 (.NET Core) - MvcMovie] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="9b508-121">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="9b508-122">在版本選取器下拉式清單方塊中，選取 [ASP.NET Core 2.1]</span><span class="sxs-lookup"><span data-stu-id="9b508-122">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="9b508-123">選取 [Web 應用程式 (模型-檢視-控制器)]</span><span class="sxs-lookup"><span data-stu-id="9b508-123">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="9b508-124">點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="9b508-124">Tap **OK**.</span></span>

![<span data-ttu-id="9b508-125">[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="9b508-125">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="9b508-126">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="9b508-126">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="9b508-127">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9b508-127">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="9b508-128">這是基本的入門專案，讓我們從這裡開始吧。</span><span class="sxs-lookup"><span data-stu-id="9b508-128">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="9b508-129">點選 **F5** 在偵錯模式中執行應用程式，或 **Ctrl-F5** 在非偵錯模式中執行。</span><span class="sxs-lookup"><span data-stu-id="9b508-129">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="9b508-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![執行中的應用程式](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="9b508-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="9b508-131">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9b508-131">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="9b508-132">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9b508-132">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9b508-133">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9b508-133">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="9b508-134">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="9b508-134">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="9b508-135">在上圖中，連接埠號碼為 5000。</span><span class="sxs-lookup"><span data-stu-id="9b508-135">In the image above, the port number is 5000.</span></span> <span data-ttu-id="9b508-136">瀏覽器中的 URL 顯示`localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="9b508-136">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="9b508-137">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="9b508-137">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="9b508-138">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="9b508-138">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="9b508-139">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="9b508-139">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="9b508-140">您可以從 [偵錯] 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="9b508-140">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="9b508-142">您可以點選 [IIS Express] 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="9b508-142">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="9b508-144">預設範本提供您作用中的**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="9b508-144">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="9b508-145">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="9b508-145">The browser image above doesn't show these links.</span></span> <span data-ttu-id="9b508-146">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="9b508-146">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上角的瀏覽圖示](start-mvc/_static/2.png)

<span data-ttu-id="9b508-148">如果您在偵錯模式中執行，請點選 **Shift + F5** 停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="9b508-148">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="9b508-149">在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="9b508-149">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="9b508-150">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9b508-150">Create a web app</span></span>

<span data-ttu-id="9b508-151">從 Visual Studio 中，選取 [檔案] > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="9b508-151">From Visual Studio, select  **File > New > Project**.</span></span>

![[檔案] > [新增] > [專案]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="9b508-153">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="9b508-153">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="9b508-154">在左窗格中，點選 [.NET Core]。</span><span class="sxs-lookup"><span data-stu-id="9b508-154">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="9b508-155">在中央窗格中，點選 [ASP.NET Core Web 應用程式 (.NET Core)]。</span><span class="sxs-lookup"><span data-stu-id="9b508-155">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="9b508-156">將專案命名為 "MvcMovie" (請務必將專案命名為 "MvcMovie"，以便複製程式碼時命名空間相符)。</span><span class="sxs-lookup"><span data-stu-id="9b508-156">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="9b508-157">點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="9b508-157">Tap **OK**</span></span>

![<span data-ttu-id="9b508-158">[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="9b508-158">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="9b508-159">完成 [新增 ASP.NET Core Web 應用程式 (.NET Core) - MvcMovie] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="9b508-159">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="9b508-160">在版本選取器下拉式清單方塊中，選取 [ASP.NET Core 2.-]。</span><span class="sxs-lookup"><span data-stu-id="9b508-160">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="9b508-161">選取 [Web 應用程式(模型檢視控制器)]。</span><span class="sxs-lookup"><span data-stu-id="9b508-161">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="9b508-162">點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="9b508-162">Tap **OK**.</span></span>

![<span data-ttu-id="9b508-163">[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="9b508-163">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="9b508-164">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="9b508-164">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="9b508-165">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9b508-165">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="9b508-166">這是基本的入門專案，是個好開始。</span><span class="sxs-lookup"><span data-stu-id="9b508-166">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="9b508-167">點選 **F5** 在偵錯模式中執行應用程式，或 **Ctrl-F5** 在非偵錯模式中執行。</span><span class="sxs-lookup"><span data-stu-id="9b508-167">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="9b508-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![執行中的應用程式](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="9b508-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="9b508-169">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9b508-169">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="9b508-170">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="9b508-170">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9b508-171">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="9b508-171">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="9b508-172">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="9b508-172">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="9b508-173">在上圖中，連接埠號碼為 5000。</span><span class="sxs-lookup"><span data-stu-id="9b508-173">In the image above, the port number is 5000.</span></span> <span data-ttu-id="9b508-174">瀏覽器中的 URL 顯示`localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="9b508-174">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="9b508-175">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="9b508-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="9b508-176">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="9b508-176">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="9b508-177">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="9b508-177">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="9b508-178">您可以從 [偵錯] 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="9b508-178">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="9b508-180">您可以點選 [IIS Express] 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="9b508-180">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="9b508-182">預設範本提供您作用中的**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="9b508-182">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="9b508-183">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="9b508-183">The browser image above doesn't show these links.</span></span> <span data-ttu-id="9b508-184">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="9b508-184">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上角的瀏覽圖示](start-mvc/_static/2.png)

<span data-ttu-id="9b508-186">如果您在偵錯模式中執行，請點選 **Shift + F5** 停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="9b508-186">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="9b508-187">在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="9b508-187">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="9b508-188">下一步</span><span class="sxs-lookup"><span data-stu-id="9b508-188">Next</span></span>](adding-controller.md)  
