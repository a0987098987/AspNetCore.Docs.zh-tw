---
title: "ASP.NET Core MVC 與 Visual Studio for Mac 使用者入門"
author: rick-anderson
description: "ASP.NET Core MVC 與 Visual Studio 使用者入門"
keywords: ASP.NET Core,MVC,Visual Studio for Mac,Entity Framework
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 387d7a91ae7d58cbc293c04039017df1dd208c82
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="adaa1-104">ASP.NET Core MVC 與 Visual Studio for Mac 使用者入門</span><span class="sxs-lookup"><span data-stu-id="adaa1-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="adaa1-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="adaa1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="adaa1-106">本教學課程會讓您了解使用 [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 建置 ASP.NET Core MVC Web 應用程式的基本知識。</span><span class="sxs-lookup"><span data-stu-id="adaa1-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="adaa1-107">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="adaa1-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="adaa1-108">macOS：[使用 Visual Studio for Mac 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="adaa1-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="adaa1-109">Windows：[使用 Visual Studio 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="adaa1-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="adaa1-110">Linux、macOS 和 Windows：[使用 Visual Studio Code 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="adaa1-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="adaa1-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="adaa1-111">Prerequisites</span></span>

<span data-ttu-id="adaa1-112">本教學課程需要 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="adaa1-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="adaa1-113">若要了解 ASP.NET Core 1.1 版，請參閱[此 PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf)。</span><span class="sxs-lookup"><span data-stu-id="adaa1-113">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="adaa1-114">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="adaa1-114">Install the following:</span></span>

- <span data-ttu-id="adaa1-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) (含) 以上版本</span><span class="sxs-lookup"><span data-stu-id="adaa1-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="adaa1-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="adaa1-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="adaa1-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="adaa1-117">Create a web app</span></span>

<span data-ttu-id="adaa1-118">從 Visual Studio 中，選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="adaa1-118">From Visual Studio, select **File > New Solution**.</span></span>

![macOS 新增方案](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="adaa1-120">選取 [.NET Core 應用程式] > [ASP.NET Core] > [Web 應用程式] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="adaa1-120">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS [新增專案] 對話方塊](start-mvc/1.png)

<span data-ttu-id="adaa1-122">將專案命名為 **MvcMovie**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="adaa1-122">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS [新增專案] 對話方塊](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="adaa1-124">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="adaa1-124">Launch the app</span></span>

<span data-ttu-id="adaa1-125">在 Visual Studio 中，選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="adaa1-125">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="adaa1-126">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/index#Kestrel)，再啟動瀏覽器並瀏覽至 `http://localhost:port`，其中 *port* 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="adaa1-126">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#Kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![含有新專案的瀏覽器](start-mvc/b1.png)

* <span data-ttu-id="adaa1-128">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="adaa1-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="adaa1-129">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="adaa1-129">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="adaa1-130">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="adaa1-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="adaa1-131">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="adaa1-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="adaa1-132">您可以從 [執行] 功能表的偵錯或非偵錯模式中啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="adaa1-132">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="adaa1-133">預設範本提供您**首頁、關於**和**連絡人**連結。</span><span class="sxs-lookup"><span data-stu-id="adaa1-133">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="adaa1-134">上圖的瀏覽器不會顯示這些連結。</span><span class="sxs-lookup"><span data-stu-id="adaa1-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="adaa1-135">根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。</span><span class="sxs-lookup"><span data-stu-id="adaa1-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![含有新專案的瀏覽器](start-mvc/b2.png)

<span data-ttu-id="adaa1-137">在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="adaa1-137">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="adaa1-138">下一步</span><span class="sxs-lookup"><span data-stu-id="adaa1-138">Next</span></span>](adding-controller.md)  
