---
title: "在 ASP.NET 和 ASP.NET Core 之間進行選擇"
author: rick-anderson
description: "了解如何在 ASP.NET 和 ASP.NET Core 之間進行選擇。"
keywords: "ASP.NET Core, 基本概念, 概觀"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.assetid: f0d17abf-3c69-413e-87fc-30780805e33f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 875064bd3437acc4e2a53220e19e86431d8c159b
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="8977e-104">在 ASP.NET 和 ASP.NET Core 之間進行選擇</span><span class="sxs-lookup"><span data-stu-id="8977e-104">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="8977e-105">不論您要建立哪種 Web 應用程式，ASP.NET 都為您提供了解決方案：從以 Windows Server 為目標的企業 Web 應用程式，到以 Linux 容器為目標的小型微服務，以及這兩者之間的所有項目。</span><span class="sxs-lookup"><span data-stu-id="8977e-105">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="8977e-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8977e-106">ASP.NET Core</span></span>

<span data-ttu-id="8977e-107">ASP.NET Core 是一種開放原始碼、跨平台的架構，用於在 Windows、macOS 或 Linux 上建置現代化的雲端式 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8977e-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="8977e-108">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8977e-108">ASP.NET</span></span>

<span data-ttu-id="8977e-109">ASP.NET 是一種成熟的架構，提供了建置企業級伺服器端 Web 應用程式所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="8977e-109">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="8977e-110">哪一個最適合我？</span><span class="sxs-lookup"><span data-stu-id="8977e-110">Which one is right for me?</span></span>

| <span data-ttu-id="8977e-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8977e-111">ASP.NET Core</span></span> | <span data-ttu-id="8977e-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8977e-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="8977e-113">為 Windows、macOS 或 Linux 建置</span><span class="sxs-lookup"><span data-stu-id="8977e-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="8977e-114">為 Windows 建置</span><span class="sxs-lookup"><span data-stu-id="8977e-114">Build for Windows</span></span>|
|<span data-ttu-id="8977e-115">[Razor 頁面](xref:mvc/razor-pages/index)是使用 ASP.NET Core 2.0 建立 Web UI 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="8977e-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="8977e-116">另請參閱 [MVC](xref:mvc/overview) 和 [Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="8977e-116">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="8977e-117">使用 [Web Forms](https://docs.microsoft.com/aspnet/web-forms)、[SignalR](https://docs.microsoft.com/aspnet/signalr)、[MVC](https://docs.microsoft.com/aspnet/mvc)、[Web API](https://docs.microsoft.com/aspnet/web-api/) 或 [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="8977e-117">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="8977e-118">每部電腦多個版本</span><span class="sxs-lookup"><span data-stu-id="8977e-118">Multiple versions per machine</span></span>|<span data-ttu-id="8977e-119">每部電腦一個版本</span><span class="sxs-lookup"><span data-stu-id="8977e-119">One version per machine</span></span>|
|<span data-ttu-id="8977e-120">在 Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 中使用 C# 或 F# 進行開發</span><span class="sxs-lookup"><span data-stu-id="8977e-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="8977e-121">在 Visual Studio 中使用 C#、VB 或 F# 進行開發</span><span class="sxs-lookup"><span data-stu-id="8977e-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="8977e-122">效能比 ASP.NET 更高</span><span class="sxs-lookup"><span data-stu-id="8977e-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="8977e-123">效能良好</span><span class="sxs-lookup"><span data-stu-id="8977e-123">Good performance</span></span>|
|[<span data-ttu-id="8977e-124">選擇 .NET Framework 或.NET Core 執行階段</span><span class="sxs-lookup"><span data-stu-id="8977e-124">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="8977e-125">使用 .NET Framework 執行階段</span><span class="sxs-lookup"><span data-stu-id="8977e-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="8977e-126">ASP.NET Core 案例</span><span class="sxs-lookup"><span data-stu-id="8977e-126">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="8977e-127">[Razor 頁面](xref:mvc/razor-pages/index)是使用 ASP.NET Core 2.0 建立 Web UI 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="8977e-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="8977e-128">網站</span><span class="sxs-lookup"><span data-stu-id="8977e-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="8977e-129">API</span><span class="sxs-lookup"><span data-stu-id="8977e-129">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="8977e-130">ASP.NET 案例</span><span class="sxs-lookup"><span data-stu-id="8977e-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="8977e-131">網站</span><span class="sxs-lookup"><span data-stu-id="8977e-131">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="8977e-132">API</span><span class="sxs-lookup"><span data-stu-id="8977e-132">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="8977e-133">即時</span><span class="sxs-lookup"><span data-stu-id="8977e-133">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="8977e-134">資源</span><span class="sxs-lookup"><span data-stu-id="8977e-134">Resources</span></span>

* [<span data-ttu-id="8977e-135">ASP.NET 簡介</span><span class="sxs-lookup"><span data-stu-id="8977e-135">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="8977e-136">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="8977e-136">Introduction to ASP.NET Core</span></span>](xref:index)
