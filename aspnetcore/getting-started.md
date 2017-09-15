---
title: "ASP.NET Core 2.0 使用者入門"
author: rick-anderson
description: "使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。"
keywords: "ASP.NET Core, 教學課程, 使用者入門"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="bed26-104">開始使用 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bed26-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="bed26-105">這些指示是針對最新版本的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="bed26-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="bed26-106">要從較早版本開始入門嗎？</span><span class="sxs-lookup"><span data-stu-id="bed26-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="bed26-107">請參閱 [1.1 版的本教學課程](xref:getting-started-1.1)。</span><span class="sxs-lookup"><span data-stu-id="bed26-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="bed26-108">安裝 [.NET Core](https://microsoft.com/net/core/)。</span><span class="sxs-lookup"><span data-stu-id="bed26-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="bed26-109">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="bed26-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="bed26-110">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="bed26-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="bed26-111">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="bed26-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. <span data-ttu-id="bed26-112">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="bed26-112">Run the app.</span></span>

   <span data-ttu-id="bed26-113">`dotnet run` 命令會在必要時先建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="bed26-113">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="bed26-114">瀏覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="bed26-114">Browse to `http://localhost:5000`</span></span>

### <a name="next-steps"></a><span data-ttu-id="bed26-115">後續步驟</span><span class="sxs-lookup"><span data-stu-id="bed26-115">Next steps</span></span>

<span data-ttu-id="bed26-116">如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。</span><span class="sxs-lookup"><span data-stu-id="bed26-116">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="bed26-117">如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。</span><span class="sxs-lookup"><span data-stu-id="bed26-117">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="bed26-118">ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。</span><span class="sxs-lookup"><span data-stu-id="bed26-118">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="bed26-119">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="bed26-119">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
