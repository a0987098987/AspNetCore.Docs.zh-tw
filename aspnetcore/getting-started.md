---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="484b6-103">ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="484b6-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="484b6-104">這些指示是針對最新版本的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="484b6-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="484b6-105">如需本文件的 1.1 版本，請參閱 [ASP.NET Core 1.1 使用者入門](xref:getting-started-1.1)。</span><span class="sxs-lookup"><span data-stu-id="484b6-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="484b6-106">安裝 [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]。</span><span class="sxs-lookup"><span data-stu-id="484b6-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="484b6-107">建立新的 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="484b6-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="484b6-108">在 macOS 和 Linux 上，開啟終端機視窗。</span><span class="sxs-lookup"><span data-stu-id="484b6-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="484b6-109">在 Windows 上，開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="484b6-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="484b6-110">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="484b6-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="484b6-111">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="484b6-111">Run the app.</span></span>

    <span data-ttu-id="484b6-112">使用以下命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="484b6-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="484b6-113">瀏覽至 [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="484b6-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="484b6-114">開啟 <em>Pages/About.cshtml</em> 並將頁面顯示訊息修改為 "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="484b6-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="484b6-115">The time on the server is @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="484b6-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="484b6-116">瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。</span><span class="sxs-lookup"><span data-stu-id="484b6-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="484b6-117">後續步驟</span><span class="sxs-lookup"><span data-stu-id="484b6-117">Next steps</span></span>

<span data-ttu-id="484b6-118">如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。</span><span class="sxs-lookup"><span data-stu-id="484b6-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="484b6-119">如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。</span><span class="sxs-lookup"><span data-stu-id="484b6-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="484b6-120">ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。</span><span class="sxs-lookup"><span data-stu-id="484b6-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="484b6-121">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="484b6-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
