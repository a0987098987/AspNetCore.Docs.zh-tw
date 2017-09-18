---
title: "ASP.NET Core 2.0 使用者入門"
author: rick-anderson
description: "使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。"
keywords: "ASP.NET Core, 教學課程, 使用者入門"
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: f7852f0dddb0585089f5ccd8f4c865f5b87b049b
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/08/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="010dc-104">開始使用 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="010dc-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="010dc-105">這些指示是針對最新版本的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="010dc-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="010dc-106">要從較早版本開始入門嗎？</span><span class="sxs-lookup"><span data-stu-id="010dc-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="010dc-107">請參閱 [1.1 版的本教學課程](xref:getting-started-1.1)。</span><span class="sxs-lookup"><span data-stu-id="010dc-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="010dc-108">安裝 [.NET Core](https://microsoft.com/net/core/)。</span><span class="sxs-lookup"><span data-stu-id="010dc-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="010dc-109">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="010dc-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="010dc-110">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="010dc-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="010dc-111">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="010dc-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="010dc-112">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="010dc-112">Run the app.</span></span>

    <span data-ttu-id="010dc-113">使用以下命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="010dc-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="010dc-114">瀏覽至 [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="010dc-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="010dc-115">開啟 *Pages/About.cshtml* 並修改頁面以顯示訊息 "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="010dc-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="010dc-116">伺服器的時間為 @DateTime.Now：</span><span class="sxs-lookup"><span data-stu-id="010dc-116">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="010dc-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="010dc-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

7. <span data-ttu-id="010dc-118">瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並驗證變更。</span><span class="sxs-lookup"><span data-stu-id="010dc-118">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="010dc-119">後續步驟</span><span class="sxs-lookup"><span data-stu-id="010dc-119">Next steps</span></span>

<span data-ttu-id="010dc-120">如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。</span><span class="sxs-lookup"><span data-stu-id="010dc-120">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="010dc-121">如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。</span><span class="sxs-lookup"><span data-stu-id="010dc-121">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="010dc-122">ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。</span><span class="sxs-lookup"><span data-stu-id="010dc-122">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="010dc-123">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="010dc-123">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
