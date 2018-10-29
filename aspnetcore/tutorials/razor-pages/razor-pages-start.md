---
title: 開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 4ee9a014114e2536f7584b2a1ff9d699fb971aa8
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206974"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="2ba92-104">教學課程：開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ba92-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="2ba92-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ba92-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2ba92-106">我們建議您遵循本教學課程的 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="2ba92-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="2ba92-107">它**更**容易理解並涵蓋更多功能。</span><span class="sxs-lookup"><span data-stu-id="2ba92-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="2ba92-108">在版本選取器中選取 [ASP.NET Core 2.1]。</span><span class="sxs-lookup"><span data-stu-id="2ba92-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![目錄中的版本選取器](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="2ba92-110">本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="2ba92-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="2ba92-111">Razor Pages 是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。</span><span class="sxs-lookup"><span data-stu-id="2ba92-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="2ba92-112">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="2ba92-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="2ba92-113">Windows：本教學課程</span><span class="sxs-lookup"><span data-stu-id="2ba92-113">Windows: This tutorial</span></span>
* <span data-ttu-id="2ba92-114">macOS：[Razor 頁面與 Visual Studio for Mac 使用者入門](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="2ba92-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="2ba92-115">macOS、Linux 和 Windows：[Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="2ba92-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="2ba92-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ba92-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="2ba92-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="2ba92-117">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="2ba92-118">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2ba92-118">Create a Razor web app</span></span>

* <span data-ttu-id="2ba92-119">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="2ba92-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2ba92-120">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ba92-120">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="2ba92-121">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="2ba92-121">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="2ba92-122">請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="2ba92-122">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="2ba92-123">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="2ba92-123">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="2ba92-124">在下拉式清單中選取 [ASP.NET Core 2.1]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="2ba92-124">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="2ba92-126">Visual Studio 範本會建立入門專案：</span><span class="sxs-lookup"><span data-stu-id="2ba92-126">The Visual Studio template creates a starter project:</span></span>

![底下提供說明，包括方案總管](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="2ba92-128">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="2ba92-128">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="2ba92-129">選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="2ba92-129">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="2ba92-130">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="2ba92-130">This app doesn't track personal information.</span></span> <span data-ttu-id="2ba92-131">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="2ba92-131">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="2ba92-133">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="2ba92-133">The following image shows the app after accepting tracking:</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="2ba92-135">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ba92-135">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="2ba92-136">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="2ba92-136">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2ba92-137">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="2ba92-137">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2ba92-138">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="2ba92-138">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="2ba92-139">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="2ba92-139">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2ba92-140">在上述影像中，連接埠編號為 5001。</span><span class="sxs-lookup"><span data-stu-id="2ba92-140">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="2ba92-141">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="2ba92-141">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="2ba92-142">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="2ba92-142">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="2ba92-143">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="2ba92-143">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="2ba92-144">必要條件</span><span class="sxs-lookup"><span data-stu-id="2ba92-144">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="2ba92-145">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2ba92-145">Create a Razor web app</span></span>

* <span data-ttu-id="2ba92-146">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="2ba92-146">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2ba92-147">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ba92-147">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="2ba92-148">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="2ba92-148">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="2ba92-149">請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="2ba92-149">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="2ba92-150">![新增 ASP.NET Core Web 應用程式](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="2ba92-150">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="2ba92-151">在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="2ba92-151">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="2ba92-152">Visual Studio 範本會建立入門專案：</span><span class="sxs-lookup"><span data-stu-id="2ba92-152">The Visual Studio template creates a starter project:</span></span>

![底下提供說明，包括方案總管](razor-pages-start/_static/se.png)

<span data-ttu-id="2ba92-154">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="2ba92-154">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home.png)

* <span data-ttu-id="2ba92-156">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ba92-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="2ba92-157">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="2ba92-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2ba92-158">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="2ba92-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2ba92-159">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="2ba92-159">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="2ba92-160">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="2ba92-160">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2ba92-161">在上述影像中，連接埠編號為 5000。</span><span class="sxs-lookup"><span data-stu-id="2ba92-161">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="2ba92-162">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="2ba92-162">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="2ba92-163">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="2ba92-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="2ba92-164">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="2ba92-164">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="2ba92-165">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="2ba92-165">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
