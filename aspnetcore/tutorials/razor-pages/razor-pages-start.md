---
title: 開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 了解建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。 對於 ASP.NET Core 中的 Web 工作負載，建議使用 Razor 頁面。
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582813"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="684db-104">開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="684db-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="684db-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="684db-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="684db-106">我們建議您遵循本教學課程的 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="684db-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="684db-107">它**更**容易理解並涵蓋更多功能。</span><span class="sxs-lookup"><span data-stu-id="684db-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="684db-108">在版本選取器中選取 [ASP.NET Core 2.1]。</span><span class="sxs-lookup"><span data-stu-id="684db-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![目錄中的版本選取器](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="684db-110">本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="684db-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="684db-111">Razor Pages 是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。</span><span class="sxs-lookup"><span data-stu-id="684db-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="684db-112">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="684db-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="684db-113">Windows：本教學課程</span><span class="sxs-lookup"><span data-stu-id="684db-113">Windows: This tutorial</span></span>
* <span data-ttu-id="684db-114">macOS：[Razor 頁面與 Visual Studio for Mac 使用者入門](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="684db-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="684db-115">macOS、Linux 和 Windows：[Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="684db-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="684db-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="684db-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="684db-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="684db-117">Prerequisites</span></span>

<span data-ttu-id="684db-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="684db-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="684db-119">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="684db-119">Create a Razor web app</span></span>

* <span data-ttu-id="684db-120">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="684db-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="684db-121">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="684db-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="684db-122">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="684db-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="684db-123">請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="684db-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="684db-124">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="684db-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="684db-125">在下拉式清單中選取 [ASP.NET Core 2.1]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="684db-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="684db-127">Visual Studio 範本會建立入門專案：</span><span class="sxs-lookup"><span data-stu-id="684db-127">The Visual Studio template creates a starter project:</span></span>

![底下提供說明，包括方案總管](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="684db-129">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="684db-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="684db-130">選取 [接受] 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="684db-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="684db-131">此應用程式不會追踪個人資訊。</span><span class="sxs-lookup"><span data-stu-id="684db-131">This app doesn't track personal information.</span></span> <span data-ttu-id="684db-132">範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="684db-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="684db-134">下圖顯示接受追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="684db-134">The following image shows the app after accepting tracking:</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="684db-136">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="684db-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="684db-137">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="684db-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="684db-138">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="684db-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="684db-139">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="684db-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="684db-140">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="684db-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="684db-141">在上述影像中，連接埠編號為 5000。</span><span class="sxs-lookup"><span data-stu-id="684db-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="684db-142">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="684db-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="684db-143">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="684db-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="684db-144">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="684db-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="684db-145">必要條件</span><span class="sxs-lookup"><span data-stu-id="684db-145">Prerequisites</span></span>

<span data-ttu-id="684db-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="684db-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="684db-147">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="684db-147">Create a Razor web app</span></span>

* <span data-ttu-id="684db-148">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="684db-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="684db-149">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="684db-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="684db-150">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="684db-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="684db-151">請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="684db-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="684db-152">![新增 ASP.NET Core Web 應用程式](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="684db-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="684db-153">在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="684db-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="684db-154">Visual Studio 範本會建立入門專案：</span><span class="sxs-lookup"><span data-stu-id="684db-154">The Visual Studio template creates a starter project:</span></span>

![底下提供說明，包括方案總管](razor-pages-start/_static/se.png)

<span data-ttu-id="684db-156">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="684db-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home.png)

* <span data-ttu-id="684db-158">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="684db-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="684db-159">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="684db-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="684db-160">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="684db-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="684db-161">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="684db-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="684db-162">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="684db-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="684db-163">在上述影像中，連接埠編號為 5000。</span><span class="sxs-lookup"><span data-stu-id="684db-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="684db-164">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="684db-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="684db-165">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="684db-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="684db-166">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="684db-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="684db-167">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="684db-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
