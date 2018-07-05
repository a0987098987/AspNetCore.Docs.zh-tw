---
title: 在 ASP.NET Core 中測試、偵錯和疑難排解
author: guardrex
description: 可連結至 ASP.NET Core 應用程式的測試和偵錯資源。
ms.author: riande
ms.custom: mvc
ms.date: 07/03/2018
uid: test/index
ms.openlocfilehash: 20f6804c1db588a88abb0d5686f894b7463ff6a9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433957"
---
# <a name="test-debug-and-troubleshoot-in-aspnet-core"></a><span data-ttu-id="457f6-103">在 ASP.NET Core 中測試、偵錯和疑難排解</span><span class="sxs-lookup"><span data-stu-id="457f6-103">Test, debug, and troubleshoot in ASP.NET Core</span></span>

## <a name="test"></a><span data-ttu-id="457f6-104">測試</span><span class="sxs-lookup"><span data-stu-id="457f6-104">Test</span></span>

[<span data-ttu-id="457f6-105">.NET Core 與 .NET Standard 中的單元測試</span><span class="sxs-lookup"><span data-stu-id="457f6-105">Unit Testing in .NET Core and .NET Standard</span></span>](/dotnet/articles/core/testing/)  
<span data-ttu-id="457f6-106">了解如何在 .NET Core 與 .NET Standard 專案中使用單元測試。</span><span class="sxs-lookup"><span data-stu-id="457f6-106">See how to use unit testing in .NET Core and .NET Standard projects.</span></span>

[<span data-ttu-id="457f6-107">整合測試</span><span class="sxs-lookup"><span data-stu-id="457f6-107">Integration tests</span></span>](xref:test/integration-tests)  
<span data-ttu-id="457f6-108">了解整合測試如何確保應用程式的元件在基礎結構層級 (包括資料庫、檔案系統和網路) 正確運作。</span><span class="sxs-lookup"><span data-stu-id="457f6-108">Learn how integration tests ensure that an app's components function correctly at the infrastructure level, including the database, file system, and network.</span></span>

[<span data-ttu-id="457f6-109">Razor 頁面單元測試</span><span class="sxs-lookup"><span data-stu-id="457f6-109">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)  
<span data-ttu-id="457f6-110">了解如何建立 Razor 頁面應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="457f6-110">Discover how to create unit tests for Razor Pages apps.</span></span>

[<span data-ttu-id="457f6-111">測試控制器</span><span class="sxs-lookup"><span data-stu-id="457f6-111">Test controllers</span></span>](xref:mvc/controllers/testing)  
<span data-ttu-id="457f6-112">了解如何使用 Moq 和 xUnit 測試 ASP.NET Core 中的控制器邏輯。</span><span class="sxs-lookup"><span data-stu-id="457f6-112">Learn how to test controller logic in ASP.NET Core with Moq and xUnit.</span></span>

## <a name="debug"></a><span data-ttu-id="457f6-113">偵錯</span><span class="sxs-lookup"><span data-stu-id="457f6-113">Debug</span></span>

[<span data-ttu-id="457f6-114">了解使用 Visual Studio 進行偵錯</span><span class="sxs-lookup"><span data-stu-id="457f6-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="457f6-115">在逐步解說中探索 Visual Studio 偵錯工具的功能。</span><span class="sxs-lookup"><span data-stu-id="457f6-115">Discover the features of the Visual Studio debugger in a step-by-step walkthrough.</span></span>

<span data-ttu-id="457f6-116">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="457f6-116">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>  
<span data-ttu-id="457f6-117">了解 Visual Studio Code 中內建的偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="457f6-117">Find out about the debugging support built into Visual Studio Code.</span></span>

[<span data-ttu-id="457f6-118">偵錯 ASP.NET Core 2.x 原始檔</span><span class="sxs-lookup"><span data-stu-id="457f6-118">Debug ASP.NET Core 2.x source</span></span>](https://github.com/aspnet/Docs/issues/4155)  
<span data-ttu-id="457f6-119">了解如何偵錯 .NET Core 與 ASP.NET Core 來源。</span><span class="sxs-lookup"><span data-stu-id="457f6-119">Learn how to debug .NET Core and ASP.NET Core sources.</span></span>

[<span data-ttu-id="457f6-120">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="457f6-120">Remote debugging</span></span>](/visualstudio/debugger/remote-debugging-azure)  
<span data-ttu-id="457f6-121">探索如何安裝和設定 Visual Studio 2017 ASP.NET Core 應用程式、使用 Azure 將之部署到 IIS，以及從 Visual Studio 附加遠端偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="457f6-121">Explore how to set up and configure a Visual Studio 2017 ASP.NET Core app, deploy it to IIS using Azure, and attach the remote debugger from Visual Studio.</span></span>

[<span data-ttu-id="457f6-122">快照集偵錯</span><span class="sxs-lookup"><span data-stu-id="457f6-122">Snapshot debugging</span></span>](/azure/application-insights/app-insights-snapshot-debugger)  
<span data-ttu-id="457f6-123">了解如何在最常擲回的例外狀況上收集快照集，以便取得診斷生產環境中問題所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="457f6-123">Find out how to collect snapshots on your top-throwing exceptions so that you have the information you need to diagnose issues in production.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="457f6-124">疑難排解</span><span class="sxs-lookup"><span data-stu-id="457f6-124">Troubleshoot</span></span>

[<span data-ttu-id="457f6-125">疑難排解</span><span class="sxs-lookup"><span data-stu-id="457f6-125">Troubleshoot</span></span>](xref:test/troubleshoot)  
<span data-ttu-id="457f6-126">了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="457f6-126">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>
