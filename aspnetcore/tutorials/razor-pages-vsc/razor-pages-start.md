---
title: Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門
author: rick-anderson
description: 了解使用 Visual Studio Code 建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244849"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="6ff84-103">Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門</span><span class="sxs-lookup"><span data-stu-id="6ff84-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="6ff84-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6ff84-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6ff84-105">本教學課程將教導您建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="6ff84-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="6ff84-106">建議您先完成 [Razor 頁面的簡介](xref:razor-pages/index)，再開始本教學課程。</span><span class="sxs-lookup"><span data-stu-id="6ff84-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="6ff84-107">Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。</span><span class="sxs-lookup"><span data-stu-id="6ff84-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ff84-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="6ff84-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="6ff84-109">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="6ff84-109">Create a Razor web app</span></span>

<span data-ttu-id="6ff84-110">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="6ff84-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="6ff84-111">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。</span><span class="sxs-lookup"><span data-stu-id="6ff84-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="6ff84-112">將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ff84-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Home 或 Index 頁面](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="6ff84-114">開啟專案</span><span class="sxs-lookup"><span data-stu-id="6ff84-114">Open the project</span></span>

<span data-ttu-id="6ff84-115">按 Ctrl + C 以關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ff84-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="6ff84-116">從 Visual Studio Code (VS Code)，選取 [檔案] > [開啟資料夾]，然後選取 *RazorPagesMovie* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="6ff84-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="6ff84-117">針對下列 [警告] 訊息選取 [是]：「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="6ff84-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="6ff84-118">新增它們嗎？」</span><span class="sxs-lookup"><span data-stu-id="6ff84-118">Add them?"</span></span>
- <span data-ttu-id="6ff84-119">選取 [還原] 至 [資訊] 訊息「有未解析的相依性」。</span><span class="sxs-lookup"><span data-stu-id="6ff84-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="6ff84-120">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="6ff84-120">Launch the app</span></span>

<span data-ttu-id="6ff84-121">從 [偵錯] 功能表，選取 [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="6ff84-121">From the **Debug** menu, select **Start Without Debugging**.</span></span> <span data-ttu-id="6ff84-122">或者，您可以按功能表選項旁顯示的鍵盤快速鍵。</span><span class="sxs-lookup"><span data-stu-id="6ff84-122">Alternatively, you can press the keyboard shortcut displayed next to the menu option.</span></span> <span data-ttu-id="6ff84-123">這個快速鍵視您的作業系統而異。</span><span class="sxs-lookup"><span data-stu-id="6ff84-123">This shortcut varies depending on your operating system.</span></span>

<span data-ttu-id="6ff84-124">在下一個教學課程中，我們可以將模型新增至專案。</span><span class="sxs-lookup"><span data-stu-id="6ff84-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="6ff84-125">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="6ff84-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
