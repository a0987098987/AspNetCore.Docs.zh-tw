---
title: ASP.NET Core MVC 與 Visual Studio for Mac 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC 與 Visual Studio
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="1f6dc-103">ASP.NET Core MVC 與 Visual Studio for Mac 使用者入門</span><span class="sxs-lookup"><span data-stu-id="1f6dc-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="1f6dc-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f6dc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f6dc-105">本教學課程會讓您了解使用 [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 建置 ASP.NET Core MVC Web 應用程式的基本知識。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="1f6dc-106">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="1f6dc-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="1f6dc-107">macOS：[使用 Visual Studio for Mac 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1f6dc-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="1f6dc-108">Windows：[使用 Visual Studio 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1f6dc-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="1f6dc-109">Linux、macOS 和 Windows：[使用 Visual Studio Code 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="1f6dc-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f6dc-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="1f6dc-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="1f6dc-111">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="1f6dc-111">Create a web app</span></span>

<span data-ttu-id="1f6dc-112">從 Visual Studio 中，選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-112">From Visual Studio, select **File > New Solution**.</span></span>

![macOS 新增方案](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="1f6dc-114">選取 [.NET Core 應用程式] > [ASP.NET Core] > [Web 應用程式] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS [新增專案] 對話方塊](start-mvc/1.png)

<span data-ttu-id="1f6dc-116">將專案命名為 **MvcMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS [新增專案] 對話方塊](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="1f6dc-118">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="1f6dc-118">Launch the app</span></span>

<span data-ttu-id="1f6dc-119">在 Visual Studio 中，選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="1f6dc-120">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel)，再啟動瀏覽器並瀏覽至 `http://localhost:port`，其中 *port* 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![含有新專案的瀏覽器](start-mvc/b1.png)

* <span data-ttu-id="1f6dc-122">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1f6dc-123">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="1f6dc-124">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="1f6dc-125">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="1f6dc-126">您可以從 [執行] 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="1f6dc-127">預設範本提供您**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="1f6dc-128">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="1f6dc-129">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![含有新專案的瀏覽器](start-mvc/b2.png)

<span data-ttu-id="1f6dc-131">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="1f6dc-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1f6dc-132">下一步</span><span class="sxs-lookup"><span data-stu-id="1f6dc-132">Next</span></span>](adding-controller.md)  
