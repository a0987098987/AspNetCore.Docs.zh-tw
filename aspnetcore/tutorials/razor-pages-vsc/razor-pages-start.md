---
title: "利用 Visual Studio Code 開始使用 ASP.NET Core 中的 Razor 頁面"
author: rick-anderson
description: "利用 Visual Studio Code 開始使用 ASP.NET Core 中的 Razor 頁面"
keywords: "ASP.NET Core, Razor 頁面, Scaffolding, Entity Framework Core, EF, EF Core, 資料庫, mac, macOS, Visual Studio Code, Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 096a60dae171ac5dbfa935be4c16e0903d8f5bb3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="4abf7-104">利用 Visual Studio Code 開始使用 ASP.NET Core 中的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="4abf7-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="4abf7-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="4abf7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4abf7-106">本教學課程將教導您建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="4abf7-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="4abf7-107">建議您先完成 [Razor 頁面的簡介](xref:mvc/razor-pages/index)，再開始本教學課程。</span><span class="sxs-lookup"><span data-stu-id="4abf7-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="4abf7-108">Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。</span><span class="sxs-lookup"><span data-stu-id="4abf7-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4abf7-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="4abf7-109">Prerequisites</span></span>

<span data-ttu-id="4abf7-110">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="4abf7-110">Install the following:</span></span>

* <span data-ttu-id="4abf7-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) (含) 以上版本</span><span class="sxs-lookup"><span data-stu-id="4abf7-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="4abf7-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4abf7-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="4abf7-113">VS Code [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="4abf7-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="4abf7-114">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4abf7-114">Create a Razor web app</span></span>

<span data-ttu-id="4abf7-115">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4abf7-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="4abf7-116">上述命令使用 [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。</span><span class="sxs-lookup"><span data-stu-id="4abf7-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="4abf7-117">將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。</span><span class="sxs-lookup"><span data-stu-id="4abf7-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Home 或 Index 頁面](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="4abf7-119">開啟專案</span><span class="sxs-lookup"><span data-stu-id="4abf7-119">Open the project</span></span>

<span data-ttu-id="4abf7-120">按 Ctrl + C 以關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="4abf7-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="4abf7-121">從 Visual Studio Code (VS Code)，選取 [檔案] > [開啟資料夾]，然後選取 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="4abf7-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="4abf7-122">針對下列 [警告] 訊息選取 [是]：「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="4abf7-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="4abf7-123">新增它們嗎？」</span><span class="sxs-lookup"><span data-stu-id="4abf7-123">Add them?"</span></span>
- <span data-ttu-id="4abf7-124">選取 [還原] 至 [資訊] 訊息「有未解析的相依性」。</span><span class="sxs-lookup"><span data-stu-id="4abf7-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="4abf7-125">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="4abf7-125">Launch the app</span></span>

<span data-ttu-id="4abf7-126">(按 Ctrl+F5 即可啟動應用程式而不偵錯)。</span><span class="sxs-lookup"><span data-stu-id="4abf7-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="4abf7-127">從 [偵錯] 功能表中，選取 [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="4abf7-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="4abf7-128">在下一個教學課程中，我們可以將模型新增至專案。</span><span class="sxs-lookup"><span data-stu-id="4abf7-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="4abf7-129">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="4abf7-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
