---
title: "ASP.NET Core 簡介"
author: rick-anderson
description: "取得 ASP.NET Core 的簡介，ASP.NET Core 是一種跨平台且高效能的開放原始碼架構，用於建置現代化、雲端式、網際網路連線的應用程式。"
manager: wpickett
ms.author: riande
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: index
ms.openlocfilehash: 103b7862900e08488dcc0f5fc78c08fefcfa17f3
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="3bf2b-103">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="3bf2b-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="3bf2b-104">由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供</span><span class="sxs-lookup"><span data-stu-id="3bf2b-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="3bf2b-105">ASP.NET Core 是一種跨平台且高效能的[開放原始碼](https://github.com/aspnet/home)架構，用於建置現代化、雲端式、網際網路連線的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="3bf2b-106">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="3bf2b-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="3bf2b-107">建置 Web 應用程式和服務、[IoT](https://www.microsoft.com/internet-of-things/) 應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="3bf2b-108">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="3bf2b-109">部署到雲端或在內部部署。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="3bf2b-110">在 [.NET Core 或 .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上執行。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="3bf2b-111">為何使用 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="3bf2b-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="3bf2b-112">數百萬的開發人員已使用 (並持續使用) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) 來建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="3bf2b-113">ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

<span data-ttu-id="3bf2b-114">ASP.NET Core 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="3bf2b-114">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="3bf2b-115">用於建置 Web UI 和 Web API 的統一劇本。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-115">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="3bf2b-116">整合[現代化用戶端架構](xref:client-side/index)和開發工作流程。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-116">Integration of [modern, client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="3bf2b-117">雲端就緒、以環境為基礎的[組態系統](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-117">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="3bf2b-118">內建的[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-118">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="3bf2b-119">輕量型、[高效能](https://github.com/aspnet/benchmarks) \(英文\) 且模組化的 HTTP 要求管線。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-119">A lightweight, [high-performance](https://github.com/aspnet/benchmarks), and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="3bf2b-120">能夠在 [IIS](xref:host-and-deploy/iis/index)、[Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)、[Docker](xref:host-and-deploy/docker/index) 上裝載，或自我裝載於您自己的處理序中。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-120">Ability to host on [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index), or self-host in your own process.</span></span>
* <span data-ttu-id="3bf2b-121">以 [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 為目標時，會有並存應用程式版本設定。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-121">Side-by-side app versioning when targeting [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
* <span data-ttu-id="3bf2b-122">可簡化現代網頁程式開發的工具。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-122">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="3bf2b-123">能夠在 Windows、macOS 和 Linux 上建置並執行。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-123">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="3bf2b-124">開放原始碼和[社群導向](https://live.asp.net/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-124">Open-source and [community-focused](https://live.asp.net/).</span></span>

<span data-ttu-id="3bf2b-125">ASP.NET Core 完全以 [NuGet](https://www.nuget.org/) 套件的形式提供。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-125">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="3bf2b-126">使用 NuGet 套件可讓您將應用程式最佳化，使其僅包含必要相依性。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-126">Using NuGet packages allows you to optimize your app to include only the necessary dependencies.</span></span> <span data-ttu-id="3bf2b-127">事實上，以 .NET Core 為目標的 ASP.NET Core 2.x 應用程式只需要[單一 NuGet 套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-127">In fact, ASP.NET Core 2.x apps targeting .NET Core only require a [single NuGet package](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="3bf2b-128">應用程式介面區較小的優點包括更嚴密的安全性、減少維護工作，以及提升效能。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="3bf2b-129">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="3bf2b-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="3bf2b-130">ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/index#build-web-apis) 和 [Web 應用程式](xref:tutorials/index#build-web-apps)的功能：</span><span class="sxs-lookup"><span data-stu-id="3bf2b-130">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="3bf2b-131">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式[可測試](testing/index.md)。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="3bf2b-132">[Razor 頁面](xref:mvc/razor-pages/index) (ASP.NET Core 2.0 中的新功能) 是以頁面為基礎的程式設計模型，可讓建置 Web UI 更容易且更具工作效率。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-132">[Razor Pages](xref:mvc/razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="3bf2b-133">[Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:mvc/razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-133">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:mvc/razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="3bf2b-134">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="3bf2b-135">[多個資料格式和內容交涉](mvc/models/formatting.md)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="3bf2b-136">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-136">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="3bf2b-137">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-137">[Model validation](xref:mvc/models/validation) automatically performs client- and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="3bf2b-138">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="3bf2b-138">Client-side development</span></span>

<span data-ttu-id="3bf2b-139">ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](xref:client-side/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-139">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="3bf2b-140">如需詳細資訊，請參閱[用戶端開發](xref:client-side/index)。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-140">For more information, see [Client-side development](xref:client-side/index).</span></span>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="3bf2b-141">將目標指向 .NET Framework 的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3bf2b-141">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="3bf2b-142">ASP.NET Core 可將目標指向 NET Core 或 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-142">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="3bf2b-143">將目標指向 .NET Framework 的 ASP.NET Core 應用程式無法跨平台&mdash;而只能在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-143">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="3bf2b-144">目前沒有任何要將 ASP.NET Core 中，以 .NET Framework 為目標之支援移除的計畫。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-144">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="3bf2b-145">一般來說，ASP.NET Core 以 [.NET Standard](/dotnet/standard/net-standard) 程式庫組成。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-145">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="3bf2b-146">以 .NET Standard 2.0 編寫的應用程式可在任何支援 .NET Standard 2.0 的位置執行。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-146">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="3bf2b-147">將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-147">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="3bf2b-148">NET Core 較 .NET Framework 多的好處包含：</span><span class="sxs-lookup"><span data-stu-id="3bf2b-148">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="3bf2b-149">跨平台。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-149">Cross-platform.</span></span> <span data-ttu-id="3bf2b-150">可在 macOS、Linux 及 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-150">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="3bf2b-151">提升效能</span><span class="sxs-lookup"><span data-stu-id="3bf2b-151">Improved performance</span></span>
* <span data-ttu-id="3bf2b-152">並存版本</span><span class="sxs-lookup"><span data-stu-id="3bf2b-152">Side-by-side versioning</span></span>
* <span data-ttu-id="3bf2b-153">新的 API</span><span class="sxs-lookup"><span data-stu-id="3bf2b-153">New APIs</span></span>
* <span data-ttu-id="3bf2b-154">開啟原始檔</span><span class="sxs-lookup"><span data-stu-id="3bf2b-154">Open source</span></span>

<span data-ttu-id="3bf2b-155">我們正致力於縮短 .NET Framework 與 .NET Core 之間的 API 差距。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-155">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="3bf2b-156">[Windows 相容性套件](/dotnet/core/porting/windows-compat-pack)在 .NET Core 中發佈了上千個僅供 Windows 使用的 API。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-156">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="3bf2b-157">這些 API 並不適用於 .NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-157">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bf2b-158">後續步驟</span><span class="sxs-lookup"><span data-stu-id="3bf2b-158">Next steps</span></span>

<span data-ttu-id="3bf2b-159">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="3bf2b-159">For more information, see the following resources:</span></span>

* [<span data-ttu-id="3bf2b-160">開始使用 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="3bf2b-160">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="3bf2b-161">ASP.NET Core 教學課程</span><span class="sxs-lookup"><span data-stu-id="3bf2b-161">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="3bf2b-162">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="3bf2b-162">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="3bf2b-163">[每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃，</span><span class="sxs-lookup"><span data-stu-id="3bf2b-163">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="3bf2b-164">並提供新的部落格和協力廠商軟體。</span><span class="sxs-lookup"><span data-stu-id="3bf2b-164">It features new blogs and third-party software.</span></span>
