---
title: ASP.NET Core 簡介
author: rick-anderson
description: 取得 ASP.NET Core 的簡介，ASP.NET Core 是一種跨平台且高效能的開放原始碼架構，用於建置現代化、雲端式、網際網路連線的應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: 60f7d64baa0441b90befb2d785999a707e1025c5
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225391"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="04513-103">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="04513-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="04513-104">由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供</span><span class="sxs-lookup"><span data-stu-id="04513-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="04513-105">ASP.NET Core 是一種跨平台且高效能的[開放原始碼](https://github.com/aspnet/home)架構，用於建置現代化、雲端式、網際網路連線的應用程式。</span><span class="sxs-lookup"><span data-stu-id="04513-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="04513-106">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="04513-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="04513-107">建置 Web 應用程式和服務、[IoT](https://www.microsoft.com/internet-of-things/) 應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="04513-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="04513-108">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="04513-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="04513-109">部署到雲端或在內部部署。</span><span class="sxs-lookup"><span data-stu-id="04513-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="04513-110">在 [.NET Core 或 .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) 上執行。</span><span class="sxs-lookup"><span data-stu-id="04513-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="04513-111">為何使用 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="04513-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="04513-112">數百萬的開發人員已使用 (並持續使用) [ASP.NET 4.x](/aspnet/overview) 來建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04513-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="04513-113">ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。</span><span class="sxs-lookup"><span data-stu-id="04513-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="04513-114">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="04513-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="04513-115">ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/first-web-api) 和 [Web 應用程式](xref:tutorials/razor-pages/index)的功能：</span><span class="sxs-lookup"><span data-stu-id="04513-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="04513-116">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式可測試。</span><span class="sxs-lookup"><span data-stu-id="04513-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="04513-117">[Razor 頁面](xref:razor-pages/index) (ASP.NET Core 2.0 中的新功能) 是以頁面為基礎的程式設計模型，可讓建置 Web UI 更容易且更具工作效率。</span><span class="sxs-lookup"><span data-stu-id="04513-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="04513-118">[Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。</span><span class="sxs-lookup"><span data-stu-id="04513-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="04513-119">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="04513-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="04513-120">[多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="04513-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="04513-121">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="04513-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="04513-122">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="04513-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="04513-123">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="04513-123">Client-side development</span></span>

<span data-ttu-id="04513-124">ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](https://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="04513-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="04513-125">如需詳細資訊，請參閱[用戶端開發](xref:client-side/index)。</span><span class="sxs-lookup"><span data-stu-id="04513-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="04513-126">將目標指向 .NET Framework 的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04513-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="04513-127">ASP.NET Core 2.x 的目標可以是 NET Core 或 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="04513-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="04513-128">將目標指向 .NET Framework 的 ASP.NET Core 應用程式無法跨平台&mdash;而只能在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="04513-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="04513-129">ASP.NET Core 2.x 通常會包含 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="04513-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="04513-130">以 .NET Standard 2.0 編寫的應用程式可在任何支援 .NET Standard 2.0 的位置執行。</span><span class="sxs-lookup"><span data-stu-id="04513-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="04513-131">與 .NET Standard 2.0 相容的 .NET Framework 版本支援 ASP.NET Core 2.x：</span><span class="sxs-lookup"><span data-stu-id="04513-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="04513-132">強烈建議使用 .NET Framework 4.7.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="04513-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="04513-133">.NET Framework 4.6.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="04513-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="04513-134">ASP.NET Core 3.0 及更新版本只可在.NET Core 上執行。</span><span class="sxs-lookup"><span data-stu-id="04513-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="04513-135">如需此變更的詳細資料，請參閱[A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (搶先看 ASP.NET Core 3.0 的變更)。</span><span class="sxs-lookup"><span data-stu-id="04513-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="04513-136">將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。</span><span class="sxs-lookup"><span data-stu-id="04513-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="04513-137">NET Core 較 .NET Framework 多的好處包含：</span><span class="sxs-lookup"><span data-stu-id="04513-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="04513-138">跨平台。</span><span class="sxs-lookup"><span data-stu-id="04513-138">Cross-platform.</span></span> <span data-ttu-id="04513-139">可在 macOS、Linux 及 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="04513-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="04513-140">提升效能</span><span class="sxs-lookup"><span data-stu-id="04513-140">Improved performance</span></span>
* <span data-ttu-id="04513-141">並存版本</span><span class="sxs-lookup"><span data-stu-id="04513-141">Side-by-side versioning</span></span>
* <span data-ttu-id="04513-142">新的 API</span><span class="sxs-lookup"><span data-stu-id="04513-142">New APIs</span></span>
* <span data-ttu-id="04513-143">開啟原始檔</span><span class="sxs-lookup"><span data-stu-id="04513-143">Open source</span></span>

<span data-ttu-id="04513-144">我們正致力於縮短 .NET Framework 與 .NET Core 之間的 API 差距。</span><span class="sxs-lookup"><span data-stu-id="04513-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="04513-145">[Windows 相容性套件](/dotnet/core/porting/windows-compat-pack)在 .NET Core 中發佈了上千個僅供 Windows 使用的 API。</span><span class="sxs-lookup"><span data-stu-id="04513-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="04513-146">這些 API 並不適用於 .NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="04513-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="04513-147">如何下載範例</span><span class="sxs-lookup"><span data-stu-id="04513-147">How to download a sample</span></span>

<span data-ttu-id="04513-148">許多文章及教學課程都有包含範例程式碼的連結。</span><span class="sxs-lookup"><span data-stu-id="04513-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="04513-149">[下載 ASP.NET 存放庫 ZIP 檔案](https://codeload.github.com/aspnet/Docs/zip/master)。</span><span class="sxs-lookup"><span data-stu-id="04513-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="04513-150">解壓縮 *Docs-master.zip* 檔案。</span><span class="sxs-lookup"><span data-stu-id="04513-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="04513-151">使用範例連結中的 URL，協助您巡覽至範例目錄。</span><span class="sxs-lookup"><span data-stu-id="04513-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04513-152">後續步驟</span><span class="sxs-lookup"><span data-stu-id="04513-152">Next steps</span></span>

<span data-ttu-id="04513-153">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="04513-153">For more information, see the following resources:</span></span>

* [<span data-ttu-id="04513-154">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="04513-154">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="04513-155">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="04513-155">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="04513-156">[每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃，</span><span class="sxs-lookup"><span data-stu-id="04513-156">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="04513-157">並提供新的部落格和協力廠商軟體。</span><span class="sxs-lookup"><span data-stu-id="04513-157">It features new blogs and third-party software.</span></span>
