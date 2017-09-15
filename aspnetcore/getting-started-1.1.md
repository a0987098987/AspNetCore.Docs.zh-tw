---
title: "ASP.NET Core 1.1 使用者入門"
author: rick-anderson
description: "使用 ASP.NET Core 1.1 建立並執行簡單 Hello World 應用程式的快速教學課程。"
keywords: "ASP.NET Core, 教學課程, 使用者入門"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="b434a-104">ASP.NET Core 1.1 使用者入門</span><span class="sxs-lookup"><span data-stu-id="b434a-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="b434a-105">這些指示是針對 ASP.NET Core 1.1。</span><span class="sxs-lookup"><span data-stu-id="b434a-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="b434a-106">要尋找最新版本？</span><span class="sxs-lookup"><span data-stu-id="b434a-106">Looking for the latest version?</span></span> <span data-ttu-id="b434a-107">請參閱[目前版本的本教學課程](xref:getting-started)。</span><span class="sxs-lookup"><span data-stu-id="b434a-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="b434a-108">從 [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 下載頁面](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md)安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="b434a-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="b434a-109">為新的 .NET Core 專案建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="b434a-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="b434a-110">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="b434a-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="b434a-111">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="b434a-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="b434a-112">如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。</span><span class="sxs-lookup"><span data-stu-id="b434a-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="b434a-113">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="b434a-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="b434a-114">還原套件。</span><span class="sxs-lookup"><span data-stu-id="b434a-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="b434a-115">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b434a-115">Run the app.</span></span>

   <span data-ttu-id="b434a-116">`dotnet run` 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="b434a-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="b434a-117">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="b434a-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="b434a-118">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b434a-118">Next steps</span></span>

<span data-ttu-id="b434a-119">如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。</span><span class="sxs-lookup"><span data-stu-id="b434a-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="b434a-120">如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。</span><span class="sxs-lookup"><span data-stu-id="b434a-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="b434a-121">ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。</span><span class="sxs-lookup"><span data-stu-id="b434a-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="b434a-122">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="b434a-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
