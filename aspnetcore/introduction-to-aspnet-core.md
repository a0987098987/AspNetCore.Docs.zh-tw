---
title: ASP.NET Core 簡介
author: rick-anderson
description: 取得 ASP.NET Core 的簡介，這是一種跨平臺、高效能、開放原始碼的架構，可用於建立現代化、具備雲端功能的網際網路連線應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2020
no-loc:
- Blazor
- SignalR
uid: index
ms.openlocfilehash: d70998d0149a0cc7018946a93ab63b5e10b76b78
ms.sourcegitcommit: f9a5069577e8f7c53f8bcec9e13e117950f4f033
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82558922"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="3a199-103">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="3a199-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="3a199-104">由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供</span><span class="sxs-lookup"><span data-stu-id="3a199-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3a199-105">ASP.NET Core 是一種跨平臺、高效能、[開放原始](https://github.com/dotnet/aspnetcore)碼的架構，可用於建立現代化、具備雲端功能的網際網路連線應用程式。</span><span class="sxs-lookup"><span data-stu-id="3a199-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/dotnet/aspnetcore) framework for building modern, cloud-enabled, Internet-connected apps.</span></span> <span data-ttu-id="3a199-106">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="3a199-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="3a199-107">建立 web 應用程式和服務、[物聯網（IoT）](https://www.microsoft.com/internet-of-things/)應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="3a199-107">Build web apps and services, [Internet of Things (IoT)](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="3a199-108">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="3a199-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="3a199-109">部署到雲端或在內部部署。</span><span class="sxs-lookup"><span data-stu-id="3a199-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="3a199-110">在[.Net Core](/dotnet/core/introduction)上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-110">Run on [.NET Core](/dotnet/core/introduction).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="3a199-111">為什麼要選擇 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="3a199-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="3a199-112">數百萬名開發人員使用或已使用[ASP.NET](/aspnet/overview) 4.x 來建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3a199-112">Millions of developers use or have used [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="3a199-113">ASP.NET Core 是 ASP.NET 4.x 的重新設計，包括會導致更精簡、更模組化架構的架構變更。</span><span class="sxs-lookup"><span data-stu-id="3a199-113">ASP.NET Core is a redesign of ASP.NET 4.x, including architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="3a199-114">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="3a199-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="3a199-115">ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/first-web-api) 和 [Web 應用程式](xref:tutorials/razor-pages/index)的功能：</span><span class="sxs-lookup"><span data-stu-id="3a199-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="3a199-116">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式可測試。</span><span class="sxs-lookup"><span data-stu-id="3a199-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="3a199-117">[Razor Pages](xref:razor-pages/index)是以頁面為基礎的程式設計模型，可讓建立 web UI 變得更容易且更具生產力。</span><span class="sxs-lookup"><span data-stu-id="3a199-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="3a199-118">[Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。</span><span class="sxs-lookup"><span data-stu-id="3a199-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="3a199-119">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="3a199-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="3a199-120">[多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="3a199-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="3a199-121">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="3a199-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="3a199-122">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="3a199-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="3a199-123">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="3a199-123">Client-side development</span></span>

<span data-ttu-id="3a199-124">ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Blazor](xref:blazor/index)、[Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](https://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="3a199-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="3a199-125">如需詳細資訊，請參閱 <xref:blazor/index> 及*用戶端開發*下的相關主題。</span><span class="sxs-lookup"><span data-stu-id="3a199-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-target-frameworks"></a><span data-ttu-id="3a199-126">ASP.NET Core 目標 framework</span><span class="sxs-lookup"><span data-stu-id="3a199-126">ASP.NET Core target frameworks</span></span>

<span data-ttu-id="3a199-127">ASP.NET Core 3.x 和更新版本只能以 .NET Core 為目標。</span><span class="sxs-lookup"><span data-stu-id="3a199-127">ASP.NET Core 3.x and later can only target .NET Core.</span></span> <span data-ttu-id="3a199-128">一般來說，ASP.NET Core 是由[.NET Standard](/dotnet/standard/net-standard)程式庫所組成。</span><span class="sxs-lookup"><span data-stu-id="3a199-128">Generally, ASP.NET Core is composed of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="3a199-129">使用 .NET Standard 2.0 撰寫的程式庫可在[任何實作 .NET Standard 2.0 的 .NET 平台](/dotnet/standard/net-standard#net-implementation-support)上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-129">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="3a199-130">將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。</span><span class="sxs-lookup"><span data-stu-id="3a199-130">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="3a199-131">NET Core 較 .NET Framework 多的好處包含：</span><span class="sxs-lookup"><span data-stu-id="3a199-131">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="3a199-132">跨平台。</span><span class="sxs-lookup"><span data-stu-id="3a199-132">Cross-platform.</span></span> <span data-ttu-id="3a199-133">在 Windows、macOS 和 Linux 上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-133">Runs on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="3a199-134">提升效能</span><span class="sxs-lookup"><span data-stu-id="3a199-134">Improved performance</span></span>
* [<span data-ttu-id="3a199-135">並存版本控制</span><span class="sxs-lookup"><span data-stu-id="3a199-135">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#side-by-side-net-versions-per-application-level)
* <span data-ttu-id="3a199-136">新的 API</span><span class="sxs-lookup"><span data-stu-id="3a199-136">New APIs</span></span>
* <span data-ttu-id="3a199-137">開放原始碼</span><span class="sxs-lookup"><span data-stu-id="3a199-137">Open source</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="3a199-138">建議學習路徑</span><span class="sxs-lookup"><span data-stu-id="3a199-138">Recommended learning path</span></span>

<span data-ttu-id="3a199-139">如需開發 ASP.NET Core 應用程式的簡介，建議使用下列教學課程的順序：</span><span class="sxs-lookup"><span data-stu-id="3a199-139">We recommend the following sequence of tutorials for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="3a199-140">遵循您想要開發或維護之應用程式類型的教學課程。</span><span class="sxs-lookup"><span data-stu-id="3a199-140">Follow a tutorial for the app type you want to develop or maintain.</span></span>

   |<span data-ttu-id="3a199-141">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="3a199-141">App type</span></span>  |<span data-ttu-id="3a199-142">狀況</span><span class="sxs-lookup"><span data-stu-id="3a199-142">Scenario</span></span>  |<span data-ttu-id="3a199-143">教學課程</span><span class="sxs-lookup"><span data-stu-id="3a199-143">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="3a199-144">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-144">Web app</span></span>                   | <span data-ttu-id="3a199-145">新的伺服器端 web UI 開發</span><span class="sxs-lookup"><span data-stu-id="3a199-145">New server-side web UI development</span></span> |[<span data-ttu-id="3a199-146">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3a199-146">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="3a199-147">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-147">Web app</span></span>                   | <span data-ttu-id="3a199-148">維護 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-148">Maintaining an MVC app</span></span> |[<span data-ttu-id="3a199-149">開始使用 MVC</span><span class="sxs-lookup"><span data-stu-id="3a199-149">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="3a199-150">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-150">Web app</span></span>                   | <span data-ttu-id="3a199-151">用戶端 web UI 開發</span><span class="sxs-lookup"><span data-stu-id="3a199-151">Client-side web UI development</span></span> |[<span data-ttu-id="3a199-152">開始使用 Blazor</span><span class="sxs-lookup"><span data-stu-id="3a199-152">Get started with Blazor</span></span>](xref:tutorials/first-blazor-app) |
   |<span data-ttu-id="3a199-153">Web API</span><span class="sxs-lookup"><span data-stu-id="3a199-153">Web API</span></span>                   | <span data-ttu-id="3a199-154">RESTful HTTP 服務</span><span class="sxs-lookup"><span data-stu-id="3a199-154">RESTful HTTP services</span></span> |<span data-ttu-id="3a199-155">[建立 Web API](xref:tutorials/first-web-api)&dagger;</span><span class="sxs-lookup"><span data-stu-id="3a199-155">[Create a web API](xref:tutorials/first-web-api)&dagger;</span></span> |
   |<span data-ttu-id="3a199-156">遠端程序呼叫應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-156">Remote Procedure Call app</span></span> | <span data-ttu-id="3a199-157">使用通訊協定緩衝區的合約優先服務</span><span class="sxs-lookup"><span data-stu-id="3a199-157">Contract-first services using Protocol Buffers</span></span> |[<span data-ttu-id="3a199-158">開始使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="3a199-158">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |
   |<span data-ttu-id="3a199-159">即時應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-159">Real-time app</span></span>             | <span data-ttu-id="3a199-160">伺服器和連線用戶端之間的雙向通訊</span><span class="sxs-lookup"><span data-stu-id="3a199-160">Bidirectional communication between servers and connected clients</span></span> |[<span data-ttu-id="3a199-161">開始使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="3a199-161">Get started with SignalR</span></span>](xref:tutorials/signalr) |

1. <span data-ttu-id="3a199-162">遵循示範如何執行基本資料存取的教學課程。</span><span class="sxs-lookup"><span data-stu-id="3a199-162">Follow a tutorial that shows how to do basic data access.</span></span>

   |<span data-ttu-id="3a199-163">狀況</span><span class="sxs-lookup"><span data-stu-id="3a199-163">Scenario</span></span>  |<span data-ttu-id="3a199-164">教學課程</span><span class="sxs-lookup"><span data-stu-id="3a199-164">Tutorial</span></span>  |
   |----------|----------|
   |<span data-ttu-id="3a199-165">新開發</span><span class="sxs-lookup"><span data-stu-id="3a199-165">New development</span></span>        |[<span data-ttu-id="3a199-166">搭配 Entity Framework Core 的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="3a199-166">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   |<span data-ttu-id="3a199-167">維護 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-167">Maintaining an MVC app</span></span> |[<span data-ttu-id="3a199-168">搭配 Entity Framework Core 的 MVC</span><span class="sxs-lookup"><span data-stu-id="3a199-168">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro) |

1. <span data-ttu-id="3a199-169">閱讀適用于所有應用程式類型之 ASP.NET Core[基本](xref:fundamentals/index)概念的總覽。</span><span class="sxs-lookup"><span data-stu-id="3a199-169">Read an overview of ASP.NET Core [fundamentals](xref:fundamentals/index) that apply to all app types.</span></span>

1. <span data-ttu-id="3a199-170">流覽目錄中的其他相關主題。</span><span class="sxs-lookup"><span data-stu-id="3a199-170">Browse the table of contents for other topics of interest.</span></span>

<span data-ttu-id="3a199-171">&dagger;另外還有[互動式 Web API 教學](/learn/modules/build-web-api-net-core)課程。</span><span class="sxs-lookup"><span data-stu-id="3a199-171">&dagger;There's also an [interactive web API tutorial](/learn/modules/build-web-api-net-core).</span></span> <span data-ttu-id="3a199-172">不需要任何本機安裝的開發工具。</span><span class="sxs-lookup"><span data-stu-id="3a199-172">No local installation of development tools is required.</span></span> <span data-ttu-id="3a199-173">程式碼會在瀏覽器的[Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)中執行，而[捲曲](https://curl.haxx.se/)則用來進行測試。</span><span class="sxs-lookup"><span data-stu-id="3a199-173">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) in your browser, and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migrate-from-net-framework"></a><span data-ttu-id="3a199-174">從 .NET Framework 遷移</span><span class="sxs-lookup"><span data-stu-id="3a199-174">Migrate from .NET Framework</span></span>

<span data-ttu-id="3a199-175">如需將 ASP.NET 4.x 應用程式遷移至 ASP.NET Core 的參考指南，請<xref:migration/proper-to-2x/index>參閱。</span><span class="sxs-lookup"><span data-stu-id="3a199-175">For a reference guide to migrating ASP.NET 4.x apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3a199-176">ASP.NET Core 是一種跨平臺、高效能、[開放原始](https://github.com/dotnet/aspnetcore)碼的架構，可用於建立現代化、具備雲端功能的網際網路連線應用程式。</span><span class="sxs-lookup"><span data-stu-id="3a199-176">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/dotnet/aspnetcore) framework for building modern, cloud-enabled, Internet-connected apps.</span></span> <span data-ttu-id="3a199-177">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="3a199-177">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="3a199-178">建立 web 應用程式和服務、[物聯網（IoT）](https://www.microsoft.com/internet-of-things/)應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="3a199-178">Build web apps and services, [Internet of Things (IoT)](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="3a199-179">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="3a199-179">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="3a199-180">部署到雲端或在內部部署。</span><span class="sxs-lookup"><span data-stu-id="3a199-180">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="3a199-181">在 [.NET Core 或 .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) 上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-181">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="3a199-182">為什麼要選擇 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="3a199-182">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="3a199-183">數百萬名開發人員使用或已使用[ASP.NET](/aspnet/overview) 4.x 來建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3a199-183">Millions of developers use or have used [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="3a199-184">ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。</span><span class="sxs-lookup"><span data-stu-id="3a199-184">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="3a199-185">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="3a199-185">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="3a199-186">ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/first-web-api) 和 [Web 應用程式](xref:tutorials/razor-pages/index)的功能：</span><span class="sxs-lookup"><span data-stu-id="3a199-186">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="3a199-187">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式可測試。</span><span class="sxs-lookup"><span data-stu-id="3a199-187">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="3a199-188">[Razor Pages](xref:razor-pages/index)是以頁面為基礎的程式設計模型，可讓建立 web UI 變得更容易且更具生產力。</span><span class="sxs-lookup"><span data-stu-id="3a199-188">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="3a199-189">[Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。</span><span class="sxs-lookup"><span data-stu-id="3a199-189">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="3a199-190">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="3a199-190">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="3a199-191">[多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="3a199-191">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="3a199-192">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="3a199-192">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="3a199-193">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="3a199-193">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="3a199-194">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="3a199-194">Client-side development</span></span>

<span data-ttu-id="3a199-195">ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Blazor](xref:blazor/index)、[Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](https://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="3a199-195">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="3a199-196">如需詳細資訊，請參閱 <xref:blazor/index> 及*用戶端開發*下的相關主題。</span><span class="sxs-lookup"><span data-stu-id="3a199-196">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="3a199-197">將目標指向 .NET Framework 的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a199-197">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="3a199-198">ASP.NET Core 2.x 的目標可以是 NET Core 或 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="3a199-198">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="3a199-199">將目標指向 .NET Framework 的 ASP.NET Core 應用程式無法跨平台&mdash;而只能在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-199">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="3a199-200">ASP.NET Core 2.x 通常會包含 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="3a199-200">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="3a199-201">使用 .NET Standard 2.0 撰寫的程式庫可在[任何實作 .NET Standard 2.0 的 .NET 平台](/dotnet/standard/net-standard#net-implementation-support)上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-201">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="3a199-202">實作 .NET Standard 2.0 的 .NET Framework 版本支援 ASP.NET Core 2.x：</span><span class="sxs-lookup"><span data-stu-id="3a199-202">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="3a199-203">建議使用 .NET Framework 最新版本。</span><span class="sxs-lookup"><span data-stu-id="3a199-203">.NET Framework latest version is recommended.</span></span>
* <span data-ttu-id="3a199-204">.NET Framework 4.6.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="3a199-204">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="3a199-205">ASP.NET Core 3.0 及更新版本只可在.NET Core 上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-205">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="3a199-206">如需此變更的詳細資料，請參閱[A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (搶先看 ASP.NET Core 3.0 的變更)。</span><span class="sxs-lookup"><span data-stu-id="3a199-206">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="3a199-207">將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。</span><span class="sxs-lookup"><span data-stu-id="3a199-207">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="3a199-208">NET Core 較 .NET Framework 多的好處包含：</span><span class="sxs-lookup"><span data-stu-id="3a199-208">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="3a199-209">跨平台。</span><span class="sxs-lookup"><span data-stu-id="3a199-209">Cross-platform.</span></span> <span data-ttu-id="3a199-210">可在 macOS、Linux 及 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="3a199-210">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="3a199-211">提升效能</span><span class="sxs-lookup"><span data-stu-id="3a199-211">Improved performance</span></span>
* [<span data-ttu-id="3a199-212">並存版本控制</span><span class="sxs-lookup"><span data-stu-id="3a199-212">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#side-by-side-net-versions-per-application-level)
* <span data-ttu-id="3a199-213">新的 API</span><span class="sxs-lookup"><span data-stu-id="3a199-213">New APIs</span></span>
* <span data-ttu-id="3a199-214">開放原始碼</span><span class="sxs-lookup"><span data-stu-id="3a199-214">Open source</span></span>

<span data-ttu-id="3a199-215">為了協助關閉從 .NET Framework 到 .NET Core 的 API 差距， [Windows 相容性套件](/dotnet/core/porting/windows-compat-pack)在 .net core 中提供了數千種僅限 Windows 的 api。</span><span class="sxs-lookup"><span data-stu-id="3a199-215">To help close the API gap from .NET Framework to .NET Core, the [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="3a199-216">這些 API 並不適用於 .NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="3a199-216">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="3a199-217">建議學習路徑</span><span class="sxs-lookup"><span data-stu-id="3a199-217">Recommended learning path</span></span>

<span data-ttu-id="3a199-218">我們建議遵循一系列的教學課程和文章，取得開發 ASP.NET Core 應用程式的簡介：</span><span class="sxs-lookup"><span data-stu-id="3a199-218">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="3a199-219">遵循教學課程以瞭解您想要開發或維護的應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="3a199-219">Follow a tutorial for the type of app you want to develop or maintain.</span></span>

   |<span data-ttu-id="3a199-220">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="3a199-220">App type</span></span>  |<span data-ttu-id="3a199-221">狀況</span><span class="sxs-lookup"><span data-stu-id="3a199-221">Scenario</span></span>  |<span data-ttu-id="3a199-222">教學課程</span><span class="sxs-lookup"><span data-stu-id="3a199-222">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="3a199-223">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-223">Web app</span></span>                   | <span data-ttu-id="3a199-224">針對全新開發</span><span class="sxs-lookup"><span data-stu-id="3a199-224">For new development</span></span>        |[<span data-ttu-id="3a199-225">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3a199-225">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="3a199-226">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-226">Web app</span></span>                   | <span data-ttu-id="3a199-227">針對維護 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-227">For maintaining an MVC app</span></span> |[<span data-ttu-id="3a199-228">開始使用 MVC</span><span class="sxs-lookup"><span data-stu-id="3a199-228">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="3a199-229">Web API</span><span class="sxs-lookup"><span data-stu-id="3a199-229">Web API</span></span>                   |                            |<span data-ttu-id="3a199-230">[建立 Web API](xref:tutorials/first-web-api)&dagger;</span><span class="sxs-lookup"><span data-stu-id="3a199-230">[Create a web API](xref:tutorials/first-web-api)&dagger;</span></span> |
   |<span data-ttu-id="3a199-231">即時應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-231">Real-time app</span></span>             |                            |[<span data-ttu-id="3a199-232">開始使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="3a199-232">Get started with SignalR</span></span>](xref:tutorials/signalr) |

1. <span data-ttu-id="3a199-233">遵循示範如何執行基本資料存取的教學課程。</span><span class="sxs-lookup"><span data-stu-id="3a199-233">Follow a tutorial that shows how to do basic data access.</span></span>

   |<span data-ttu-id="3a199-234">狀況</span><span class="sxs-lookup"><span data-stu-id="3a199-234">Scenario</span></span>  |<span data-ttu-id="3a199-235">教學課程</span><span class="sxs-lookup"><span data-stu-id="3a199-235">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="3a199-236">針對全新開發</span><span class="sxs-lookup"><span data-stu-id="3a199-236">For new development</span></span>        |[<span data-ttu-id="3a199-237">搭配 Entity Framework Core 的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="3a199-237">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="3a199-238">針對維護 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="3a199-238">For maintaining an MVC app</span></span> |[<span data-ttu-id="3a199-239">搭配 Entity Framework Core 的 MVC</span><span class="sxs-lookup"><span data-stu-id="3a199-239">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro) |

1. <span data-ttu-id="3a199-240">閱讀適用于所有應用程式類型之 ASP.NET Core[基本](xref:fundamentals/index)概念的總覽。</span><span class="sxs-lookup"><span data-stu-id="3a199-240">Read an overview of ASP.NET Core [fundamentals](xref:fundamentals/index) that apply to all app types.</span></span>

1. <span data-ttu-id="3a199-241">瀏覽其他您感興趣主題的目錄。</span><span class="sxs-lookup"><span data-stu-id="3a199-241">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="3a199-242">&dagger;另外還有一個 Web API 的教學課程，[您會在瀏覽器中完全遵循](/learn/modules/build-web-api-net-core)，不需要本機 IDE 安裝。</span><span class="sxs-lookup"><span data-stu-id="3a199-242">&dagger;There's also a [web API tutorial that you follow entirely in the browser](/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span> <span data-ttu-id="3a199-243">程式碼會在 [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) 中執行，且會使用 [curl](https://curl.haxx.se/) 來進行測試。</span><span class="sxs-lookup"><span data-stu-id="3a199-243">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migrate-from-net-framework"></a><span data-ttu-id="3a199-244">從 .NET Framework 遷移</span><span class="sxs-lookup"><span data-stu-id="3a199-244">Migrate from .NET Framework</span></span>

<span data-ttu-id="3a199-245">如需將 ASP.NET 應用程式遷移至 ASP.NET Core 的參考指南<xref:migration/proper-to-2x/index>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="3a199-245">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

::: moniker-end

## <a name="how-to-download-a-sample"></a><span data-ttu-id="3a199-246">如何下載範例</span><span class="sxs-lookup"><span data-stu-id="3a199-246">How to download a sample</span></span>

<span data-ttu-id="3a199-247">許多文章及教學課程都有包含範例程式碼的連結。</span><span class="sxs-lookup"><span data-stu-id="3a199-247">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="3a199-248">[下載 ASP.NET 存放庫 ZIP 檔案](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master)。</span><span class="sxs-lookup"><span data-stu-id="3a199-248">[Download the ASP.NET repository zip file](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="3a199-249">解壓縮 *Docs-master.zip* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3a199-249">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="3a199-250">使用範例連結中的 URL，協助您巡覽至範例目錄。</span><span class="sxs-lookup"><span data-stu-id="3a199-250">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="3a199-251">範例程式碼中的前置處理器指示詞</span><span class="sxs-lookup"><span data-stu-id="3a199-251">Preprocessor directives in sample code</span></span>

<span data-ttu-id="3a199-252">為了示範多個案例，範例應用程式`#define`會`#if-#else/#elif-#endif`使用和預處理器指示詞，選擇性地編譯和執行範例程式碼的不同區段。</span><span class="sxs-lookup"><span data-stu-id="3a199-252">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="3a199-253">針對使用這種方法的範例，請在 c # `#define`檔案頂端設定指示詞，以定義與您要執行之情節相關聯的符號。</span><span class="sxs-lookup"><span data-stu-id="3a199-253">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="3a199-254">有些範例需要在多個檔案的頂端定義符號，才能執行案例。</span><span class="sxs-lookup"><span data-stu-id="3a199-254">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="3a199-255">例如下列 `#define` 符號清單指出其提供四個情節 (每個符號一個情節)。</span><span class="sxs-lookup"><span data-stu-id="3a199-255">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="3a199-256">目前的範例設定會執行 `TemplateCode` 情節：</span><span class="sxs-lookup"><span data-stu-id="3a199-256">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="3a199-257">若要變更此範例，以執行 `ExpandDefault` 情節，請定義 `ExpandDefault` 符號，並將剩餘的符號設為註解：</span><span class="sxs-lookup"><span data-stu-id="3a199-257">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="3a199-258">如需如何使用 [C# 前置處理器指示詞](/dotnet/csharp/language-reference/preprocessor-directives/)，以選擇編譯不同之程式碼區段的詳細資訊，請參閱 [#define (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) 及 [#if (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)。</span><span class="sxs-lookup"><span data-stu-id="3a199-258">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="3a199-259">範例程式碼中的區域</span><span class="sxs-lookup"><span data-stu-id="3a199-259">Regions in sample code</span></span>

<span data-ttu-id="3a199-260">部分範例應用程式包含[#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region)和[#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) c # 指示詞括住的程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="3a199-260">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="3a199-261">文件建置系統會將這些區域插入轉譯的文件主題中。</span><span class="sxs-lookup"><span data-stu-id="3a199-261">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="3a199-262">區域名稱通常包含字組 "snippet"。</span><span class="sxs-lookup"><span data-stu-id="3a199-262">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="3a199-263">下列範例顯示了名為 `snippet_WebHostDefaults` 的區域：</span><span class="sxs-lookup"><span data-stu-id="3a199-263">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="3a199-264">上述的 C# 程式碼片段會在主題的 Markdown 檔案中，透過以下程式行加以參考：</span><span class="sxs-lookup"><span data-stu-id="3a199-264">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="3a199-265">您可以放心地忽略（或移除） `#region`程式`#endregion`代碼周圍的和指示詞。</span><span class="sxs-lookup"><span data-stu-id="3a199-265">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="3a199-266">如果您打算執行主題中所述的範例案例，請不要更改這些指示詞內的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3a199-266">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="3a199-267">您可以在試驗其他案例時自由改變程式碼。</span><span class="sxs-lookup"><span data-stu-id="3a199-267">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="3a199-268">如需詳細資訊，請參閱 [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets) (參與 ASP.NET 文件：程式碼片段)。</span><span class="sxs-lookup"><span data-stu-id="3a199-268">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a199-269">後續步驟</span><span class="sxs-lookup"><span data-stu-id="3a199-269">Next steps</span></span>

<span data-ttu-id="3a199-270">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="3a199-270">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="3a199-271">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="3a199-271">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="3a199-272">[每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃，</span><span class="sxs-lookup"><span data-stu-id="3a199-272">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="3a199-273">並提供新的部落格和協力廠商軟體。</span><span class="sxs-lookup"><span data-stu-id="3a199-273">It features new blogs and third-party software.</span></span>
