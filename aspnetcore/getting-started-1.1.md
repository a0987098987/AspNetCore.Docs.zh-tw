---
title: ASP.NET Core 1.1 使用者入門
author: rick-anderson
description: 請遵循本快速教學課程，使用 ASP.NET Core 1.1 來建立並執行簡單的 Hello World 應用程式。
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a><span data-ttu-id="f3746-103">ASP.NET Core 1.1 使用者入門</span><span class="sxs-lookup"><span data-stu-id="f3746-103">Get Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="f3746-104">這些指示是針對 ASP.NET Core 1.1。</span><span class="sxs-lookup"><span data-stu-id="f3746-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="f3746-105">要尋找最新版本？</span><span class="sxs-lookup"><span data-stu-id="f3746-105">Looking for the latest version?</span></span> <span data-ttu-id="f3746-106">請參閱[目前版本的本教學課程](xref:getting-started)。</span><span class="sxs-lookup"><span data-stu-id="f3746-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="f3746-107">從 [.NET Core All Downloads](https://www.microsoft.com/net/download/all) (.NET Core 所有下載) 頁面安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="f3746-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="f3746-108">為新的 .NET Core 專案建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="f3746-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="f3746-109">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="f3746-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f3746-110">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="f3746-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="f3746-111">如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。</span><span class="sxs-lookup"><span data-stu-id="f3746-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="f3746-112">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="f3746-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="f3746-113">還原套件。</span><span class="sxs-lookup"><span data-stu-id="f3746-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="f3746-114">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f3746-114">Run the app.</span></span>

   <span data-ttu-id="f3746-115">[dotnet run](/dotnet/core/tools/dotnet-run) 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="f3746-115">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="f3746-116">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="f3746-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="f3746-117">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f3746-117">Next steps</span></span>

<span data-ttu-id="f3746-118">如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。</span><span class="sxs-lookup"><span data-stu-id="f3746-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="f3746-119">如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。</span><span class="sxs-lookup"><span data-stu-id="f3746-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="f3746-120">ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。</span><span class="sxs-lookup"><span data-stu-id="f3746-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="f3746-121">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="f3746-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
