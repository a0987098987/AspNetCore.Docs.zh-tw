---
title: "在 Mac 上開始使用 ASP.NET Core 中的 Razor 頁面"
author: rick-anderson
description: "利用 Visual Studio for Mac 開始使用 ASP.NET Core 中的 Razor 頁面"
keywords: "ASP.NET Core ,Razor 頁面, Scaffolding, Entity Framework Core, EF, EF Core, 資料庫, mac, macOS, Visual Studio for Mac"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 56ff18589d189b0d2760c761c58b5b030d02940b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="34bd4-104">利用 Visual Studio for Mac 開始使用 ASP.NET Core 中的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="34bd4-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="34bd4-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="34bd4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="34bd4-106">本教學課程將教導您建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="34bd4-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="34bd4-107">建議您先完成 [Razor 頁面的簡介](xref:mvc/razor-pages/index)，再開始本教學課程。</span><span class="sxs-lookup"><span data-stu-id="34bd4-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="34bd4-108">Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。</span><span class="sxs-lookup"><span data-stu-id="34bd4-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34bd4-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="34bd4-109">Prerequisites</span></span>

<span data-ttu-id="34bd4-110">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="34bd4-110">Install the following:</span></span>

* <span data-ttu-id="34bd4-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) (含) 以上版本</span><span class="sxs-lookup"><span data-stu-id="34bd4-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="34bd4-112">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="34bd4-112">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="34bd4-113">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="34bd4-113">Create a Razor web app</span></span>

<span data-ttu-id="34bd4-114">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34bd4-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="34bd4-115">上述命令使用 [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。</span><span class="sxs-lookup"><span data-stu-id="34bd4-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="34bd4-116">將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。</span><span class="sxs-lookup"><span data-stu-id="34bd4-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Home 或 Index 頁面](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="34bd4-118">開啟專案</span><span class="sxs-lookup"><span data-stu-id="34bd4-118">Open the project</span></span>

<span data-ttu-id="34bd4-119">按 Ctrl + C 來關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="34bd4-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="34bd4-120">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="34bd4-120">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="34bd4-121">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="34bd4-121">Launch the app</span></span>

<span data-ttu-id="34bd4-122">在 Visual Studio 中，選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="34bd4-122">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="34bd4-123">Visual Studio 會啟動 [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)，啟動瀏覽器，然後巡覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="34bd4-123">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="34bd4-124">在下一個教學課程中，我們可以將模型新增至專案。</span><span class="sxs-lookup"><span data-stu-id="34bd4-124">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="34bd4-125">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="34bd4-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
