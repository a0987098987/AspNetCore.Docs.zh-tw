---
title: "ASP.NET Core MVC 與 Visual Studio 使用者入門"
author: rick-anderson
description: "ASP.NET Core MVC 與 Visual Studio 使用者入門"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 72852678a296107e6a2a146ed2324335a6e870ee
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="26c5a-104">ASP.NET Core MVC 與 Visual Studio 使用者入門</span><span class="sxs-lookup"><span data-stu-id="26c5a-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="26c5a-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26c5a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="26c5a-106">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="26c5a-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="26c5a-107">macOS：[使用 Visual Studio for Mac 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="26c5a-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="26c5a-108">Windows：[使用 Visual Studio 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="26c5a-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="26c5a-109">macOS、Linux 和 Windows：[使用 Visual Studio Code 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="26c5a-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="26c5a-110">安裝 Visual Studio 和 .NET Core</span><span class="sxs-lookup"><span data-stu-id="26c5a-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26c5a-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26c5a-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26c5a-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26c5a-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26c5a-113">安裝 Visual Studio Community 2017。</span><span class="sxs-lookup"><span data-stu-id="26c5a-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="26c5a-114">選取要下載的社群。</span><span class="sxs-lookup"><span data-stu-id="26c5a-114">Select the Community download.</span></span> <span data-ttu-id="26c5a-115">如已安裝 Visual Studio 2017 請跳過此步驟。</span><span class="sxs-lookup"><span data-stu-id="26c5a-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="26c5a-116">Visual Studio 2017 首頁的安裝程式</span><span class="sxs-lookup"><span data-stu-id="26c5a-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="26c5a-117">執行安裝程式並選取下列工作負載：</span><span class="sxs-lookup"><span data-stu-id="26c5a-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="26c5a-118">**ASP.NET 與網頁程式開發** (位在 [Web & Cloud] (Web 與雲端) 下)</span><span class="sxs-lookup"><span data-stu-id="26c5a-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="26c5a-119">**.NET Core 跨平台開發** (位在 [其他工具組] 下)</span><span class="sxs-lookup"><span data-stu-id="26c5a-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET 與網頁程式開發** (位在 [Web & Cloud] (Web 與雲端)**** 下)](start-mvc/_static/web_workload.png)

![**.NET Core 跨平台開發** (位在 [其他工具組]**** 下)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="26c5a-122">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="26c5a-122">Create a web app</span></span>

<span data-ttu-id="26c5a-123">從 Visual Studio 中，選取 [檔案] > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-123">From Visual Studio, select  **File > New > Project**.</span></span>

![[檔案] > [新增] > [專案]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="26c5a-125">完成 [新增專案] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="26c5a-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="26c5a-126">在左窗格中，點選 [.NET Core]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="26c5a-127">在中央窗格中，點選 [ASP.NET Core Web 應用程式 (.NET Core)]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="26c5a-128">將專案命名為 "MvcMovie" (請務必將專案命名為 "MvcMovie"，以便複製程式碼時命名空間相符)。</span><span class="sxs-lookup"><span data-stu-id="26c5a-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="26c5a-129">點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-129">Tap **OK**</span></span>

![<span data-ttu-id="26c5a-130">[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="26c5a-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26c5a-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26c5a-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="26c5a-132">完成 [新增 ASP.NET Core Web 應用程式 (.NET Core) - MvcMovie] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="26c5a-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="26c5a-133">在版本選取器下拉式清單方塊中，選取 [ASP.NET Core 2.-]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="26c5a-134">選取 [Web 應用程式(模型檢視控制器)]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="26c5a-135">點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-135">Tap **OK**.</span></span>

![<span data-ttu-id="26c5a-136">[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="26c5a-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26c5a-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26c5a-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26c5a-138">完成 [新增 ASP.NET Core Web 應用程式 (.NET Core) - MvcMovie] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="26c5a-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="26c5a-139">在版本選取器下拉式清單方塊中，點選 [ASP.NET Core 1.1]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="26c5a-140">點選 [Web 應用程式]</span><span class="sxs-lookup"><span data-stu-id="26c5a-140">Tap **Web Application**</span></span>
* <span data-ttu-id="26c5a-141">保留預設值 [No Authentication] (無驗證)</span><span class="sxs-lookup"><span data-stu-id="26c5a-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="26c5a-142">點選 [確定]。</span><span class="sxs-lookup"><span data-stu-id="26c5a-142">Tap **OK**.</span></span>

![新的 ASP.NET Core Web 應用程式](start-mvc/_static/p3.png)

---

<span data-ttu-id="26c5a-144">Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。</span><span class="sxs-lookup"><span data-stu-id="26c5a-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="26c5a-145">您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="26c5a-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="26c5a-146">這是簡單的入門專案，是個好開始，</span><span class="sxs-lookup"><span data-stu-id="26c5a-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="26c5a-147">點選 **F5** 在偵錯模式中執行應用程式，或 **Ctrl-F5** 在非偵錯模式中執行。</span><span class="sxs-lookup"><span data-stu-id="26c5a-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![執行中的應用程式](start-mvc/_static/1.png)

* <span data-ttu-id="26c5a-149">Visual Studio 會啟動 [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="26c5a-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="26c5a-150">請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="26c5a-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="26c5a-151">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="26c5a-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="26c5a-152">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="26c5a-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="26c5a-153">在上圖中，連接埠號碼為 5000。</span><span class="sxs-lookup"><span data-stu-id="26c5a-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="26c5a-154">瀏覽器中的 URL 顯示`localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="26c5a-154">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="26c5a-155">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="26c5a-155">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="26c5a-156">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="26c5a-156">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="26c5a-157">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="26c5a-157">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="26c5a-158">您可以從 [偵錯] 功能表項目的偵錯或非偵錯模式中啟動應用程式：</span><span class="sxs-lookup"><span data-stu-id="26c5a-158">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="26c5a-160">您可以點選 [IIS Express] 按鈕偵錯應用程式</span><span class="sxs-lookup"><span data-stu-id="26c5a-160">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="26c5a-162">預設範本提供您作用中的**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="26c5a-162">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="26c5a-163">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="26c5a-163">The browser image above doesn't show these links.</span></span> <span data-ttu-id="26c5a-164">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="26c5a-164">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上角的瀏覽圖示](start-mvc/_static/2.png)

<span data-ttu-id="26c5a-166">如果您在偵錯模式中執行，請點選 **Shift + F5** 停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="26c5a-166">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="26c5a-167">在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="26c5a-167">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="26c5a-168">下一步</span><span class="sxs-lookup"><span data-stu-id="26c5a-168">Next</span></span>](adding-controller.md)  
