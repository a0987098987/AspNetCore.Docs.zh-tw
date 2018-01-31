---
title: "ASP.NET Core 1.1 使用者入門"
author: rick-anderson
description: "使用 ASP.NET Core 1.1 建立並執行簡單 Hello World 應用程式的快速教學課程。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: 895e91efbba931923540e4cd182862cbc1851585
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="86b82-103">ASP.NET Core 1.1 使用者入門</span><span class="sxs-lookup"><span data-stu-id="86b82-103">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="86b82-104">這些指示是針對 ASP.NET Core 1.1。</span><span class="sxs-lookup"><span data-stu-id="86b82-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="86b82-105">要尋找最新版本？</span><span class="sxs-lookup"><span data-stu-id="86b82-105">Looking for the latest version?</span></span> <span data-ttu-id="86b82-106">請參閱[目前版本的本教學課程](xref:getting-started)。</span><span class="sxs-lookup"><span data-stu-id="86b82-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="86b82-107">從 [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 下載頁面](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md)安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="86b82-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="86b82-108">為新的 .NET Core 專案建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="86b82-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="86b82-109">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="86b82-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="86b82-110">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="86b82-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="86b82-111">如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。</span><span class="sxs-lookup"><span data-stu-id="86b82-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="86b82-112">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="86b82-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="86b82-113">還原套件。</span><span class="sxs-lookup"><span data-stu-id="86b82-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="86b82-114">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="86b82-114">Run the app.</span></span>

   <span data-ttu-id="86b82-115">`dotnet run` 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="86b82-115">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="86b82-116">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="86b82-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="86b82-117">後續步驟</span><span class="sxs-lookup"><span data-stu-id="86b82-117">Next steps</span></span>

<span data-ttu-id="86b82-118">如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。</span><span class="sxs-lookup"><span data-stu-id="86b82-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="86b82-119">如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。</span><span class="sxs-lookup"><span data-stu-id="86b82-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="86b82-120">ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。</span><span class="sxs-lookup"><span data-stu-id="86b82-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="86b82-121">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="86b82-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
