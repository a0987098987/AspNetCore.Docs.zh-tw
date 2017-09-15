---
title: "ASP.NET Core 簡介"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: d8516643393cdf9175adc78ab98420dc1dc5d1d9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="013a5-103">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="013a5-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="013a5-104">由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供</span><span class="sxs-lookup"><span data-stu-id="013a5-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="013a5-105">ASP.NET Core 是一種跨平台且高效能的[開放原始碼](https://github.com/aspnet/home)架構，用於建置現代化、雲端式、網際網路連線的應用程式。</span><span class="sxs-lookup"><span data-stu-id="013a5-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="013a5-106">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="013a5-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="013a5-107">建置 Web 應用程式和服務、IoT 應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="013a5-107">Build web apps and services, IoT apps, and mobile backends.</span></span>
* <span data-ttu-id="013a5-108">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="013a5-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="013a5-109">部署到雲端和在內部部署。</span><span class="sxs-lookup"><span data-stu-id="013a5-109">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="013a5-110">在 [.NET Core 或 .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上執行。</span><span class="sxs-lookup"><span data-stu-id="013a5-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="013a5-111">為何使用 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="013a5-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="013a5-112">數百萬的開發人員已使用 ASP.NET (並繼續使用它) 來建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="013a5-112">Millions of developers have used ASP.NET (and continue to use it) to create web apps.</span></span> <span data-ttu-id="013a5-113">ASP.NET Core 是 ASP.NET 的重新設計，其架構變更可產生精簡且模組化的架構。</span><span class="sxs-lookup"><span data-stu-id="013a5-113">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="013a5-114">ASP.NET Core 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="013a5-114">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="013a5-115">用於建置 Web UI 和 Web API 的統一劇本。</span><span class="sxs-lookup"><span data-stu-id="013a5-115">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="013a5-116">整合[現代化的用戶端架構](xref:client-side/index)和開發工作流程。</span><span class="sxs-lookup"><span data-stu-id="013a5-116">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="013a5-117">雲端就緒、以環境為基礎的[組態系統](xref:fundamentals/configuration)。</span><span class="sxs-lookup"><span data-stu-id="013a5-117">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration).</span></span>
* <span data-ttu-id="013a5-118">內建的[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="013a5-118">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="013a5-119">輕量型、高效能且模組化的 HTTP 要求管線。</span><span class="sxs-lookup"><span data-stu-id="013a5-119">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="013a5-120">能夠在 IIS 上裝載，或自我裝載於您自己的處理序。</span><span class="sxs-lookup"><span data-stu-id="013a5-120">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="013a5-121">可以在 [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上執行，其支援真正的並存應用程式版本控制。</span><span class="sxs-lookup"><span data-stu-id="013a5-121">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="013a5-122">可簡化現代網頁程式開發的工具。</span><span class="sxs-lookup"><span data-stu-id="013a5-122">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="013a5-123">能夠在 Windows、macOS 和 Linux 上建置並執行。</span><span class="sxs-lookup"><span data-stu-id="013a5-123">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="013a5-124">開放原始碼和社群導向。</span><span class="sxs-lookup"><span data-stu-id="013a5-124">Open-source and community-focused.</span></span>

<span data-ttu-id="013a5-125">ASP.NET Core 完全以 [NuGet](https://www.nuget.org/) 套件的形式提供。</span><span class="sxs-lookup"><span data-stu-id="013a5-125">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="013a5-126">這可讓您最佳化您的應用程式，使其只包含需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="013a5-126">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="013a5-127">應用程式介面區較小的優點包括更嚴密的安全性、減少維護工作，以及提升效能。</span><span class="sxs-lookup"><span data-stu-id="013a5-127">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="013a5-128">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="013a5-128">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="013a5-129">ASP.NET Core MVC 提供了一些功能，可協助您建置 [Web API](xref:tutorials/index#building-web-apis) 和 [Web 應用程式](xref:tutorials/index#building-web-applications)：</span><span class="sxs-lookup"><span data-stu-id="013a5-129">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="013a5-130">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式[可測試](testing/index.md)。</span><span class="sxs-lookup"><span data-stu-id="013a5-130">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="013a5-131">[Razor 頁面](xref:mvc/razor-pages/index) (2.0 中的新功能) 是以頁面為基礎的程式設計模型，可讓建置 Web UI 更容易且更具工作效率。</span><span class="sxs-lookup"><span data-stu-id="013a5-131">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="013a5-132">[Razor 語法](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:mvc/razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語言。</span><span class="sxs-lookup"><span data-stu-id="013a5-132">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="013a5-133">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="013a5-133">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="013a5-134">[多個資料格式和內容交涉](mvc/models/formatting.md)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="013a5-134">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="013a5-135">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="013a5-135">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="013a5-136">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="013a5-136">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="013a5-137">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="013a5-137">Client-side development</span></span>

<span data-ttu-id="013a5-138">ASP.NET Core 的設計目的是要與各種不同的用戶端架構緊密整合，包括 [AngularJS](xref:client-side/angular)、[KnockoutJS](xref:client-side/knockout) 和 [Bootstrap](xref:client-side/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="013a5-138">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="013a5-139">如需詳細資料，請參閱[用戶端開發](client-side/index.md)。</span><span class="sxs-lookup"><span data-stu-id="013a5-139">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="013a5-140">後續步驟</span><span class="sxs-lookup"><span data-stu-id="013a5-140">Next steps</span></span>

<span data-ttu-id="013a5-141">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="013a5-141">For more information, see the following resources:</span></span>

* [<span data-ttu-id="013a5-142">ASP.NET Core 教學課程</span><span class="sxs-lookup"><span data-stu-id="013a5-142">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="013a5-143">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="013a5-143">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="013a5-144">[每週的 ASP.NET 社群之聲](https://live.asp.net/)涵蓋小組的進度和計劃，並提供新的部落格和協力廠商軟體。</span><span class="sxs-lookup"><span data-stu-id="013a5-144">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
