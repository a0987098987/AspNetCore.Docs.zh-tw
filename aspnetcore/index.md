---
title: ASP.NET Core 簡介
author: rick-anderson
description: 取得 ASP.NET Core 的簡介，ASP.NET Core 是一種跨平台且高效能的開放原始碼架構，用於建置現代化、雲端式、網際網路連線的應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 11/16/2018
uid: index
ms.openlocfilehash: 13bb8fafe8d5d34185b3016630d5dd3df4212119
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288651"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="36189-103">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="36189-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="36189-104">由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供</span><span class="sxs-lookup"><span data-stu-id="36189-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="36189-105">ASP.NET Core 是一種跨平台且高效能的[開放原始碼](https://github.com/aspnet/home)架構，用於建置現代化、雲端式、網際網路連線的應用程式。</span><span class="sxs-lookup"><span data-stu-id="36189-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="36189-106">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="36189-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="36189-107">建置 Web 應用程式和服務、[IoT](https://www.microsoft.com/internet-of-things/) 應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="36189-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="36189-108">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="36189-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="36189-109">部署到雲端或在內部部署。</span><span class="sxs-lookup"><span data-stu-id="36189-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="36189-110">在 [.NET Core 或 .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) 上執行。</span><span class="sxs-lookup"><span data-stu-id="36189-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="36189-111">為何使用 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="36189-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="36189-112">數百萬的開發人員已使用 (並持續使用) [ASP.NET 4.x](/aspnet/overview) 來建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="36189-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="36189-113">ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。</span><span class="sxs-lookup"><span data-stu-id="36189-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="36189-114">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="36189-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="36189-115">ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/first-web-api) 和 [Web 應用程式](xref:tutorials/razor-pages/index)的功能：</span><span class="sxs-lookup"><span data-stu-id="36189-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="36189-116">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式可測試。</span><span class="sxs-lookup"><span data-stu-id="36189-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="36189-117">[Razor 頁面](xref:razor-pages/index) (ASP.NET Core 2.0 中的新功能) 是以頁面為基礎的程式設計模型，可讓建置 Web UI 更容易且更具工作效率。</span><span class="sxs-lookup"><span data-stu-id="36189-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="36189-118">[Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。</span><span class="sxs-lookup"><span data-stu-id="36189-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="36189-119">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="36189-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="36189-120">[多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="36189-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="36189-121">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="36189-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="36189-122">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="36189-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="36189-123">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="36189-123">Client-side development</span></span>

<span data-ttu-id="36189-124">ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](https://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="36189-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="36189-125">如需詳細資訊，請參閱[用戶端開發](xref:client-side/index)。</span><span class="sxs-lookup"><span data-stu-id="36189-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="36189-126">將目標指向 .NET Framework 的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36189-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="36189-127">ASP.NET Core 2.x 的目標可以是 NET Core 或 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="36189-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="36189-128">將目標指向 .NET Framework 的 ASP.NET Core 應用程式無法跨平台&mdash;而只能在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="36189-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="36189-129">ASP.NET Core 2.x 通常會包含 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="36189-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="36189-130">以 .NET Standard 2.0 編寫的應用程式可在任何支援 .NET Standard 2.0 的位置執行。</span><span class="sxs-lookup"><span data-stu-id="36189-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="36189-131">與 .NET Standard 2.0 相容的 .NET Framework 版本支援 ASP.NET Core 2.x：</span><span class="sxs-lookup"><span data-stu-id="36189-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="36189-132">強烈建議使用 .NET Framework 4.7.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="36189-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="36189-133">.NET Framework 4.6.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="36189-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="36189-134">ASP.NET Core 3.0 及更新版本只可在.NET Core 上執行。</span><span class="sxs-lookup"><span data-stu-id="36189-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="36189-135">如需此變更的詳細資料，請參閱[A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (搶先看 ASP.NET Core 3.0 的變更)。</span><span class="sxs-lookup"><span data-stu-id="36189-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="36189-136">將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。</span><span class="sxs-lookup"><span data-stu-id="36189-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="36189-137">NET Core 較 .NET Framework 多的好處包含：</span><span class="sxs-lookup"><span data-stu-id="36189-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="36189-138">跨平台。</span><span class="sxs-lookup"><span data-stu-id="36189-138">Cross-platform.</span></span> <span data-ttu-id="36189-139">可在 macOS、Linux 及 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="36189-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="36189-140">提升效能</span><span class="sxs-lookup"><span data-stu-id="36189-140">Improved performance</span></span>
* <span data-ttu-id="36189-141">並存版本</span><span class="sxs-lookup"><span data-stu-id="36189-141">Side-by-side versioning</span></span>
* <span data-ttu-id="36189-142">新的 API</span><span class="sxs-lookup"><span data-stu-id="36189-142">New APIs</span></span>
* <span data-ttu-id="36189-143">開啟原始檔</span><span class="sxs-lookup"><span data-stu-id="36189-143">Open source</span></span>

<span data-ttu-id="36189-144">我們正致力於縮短 .NET Framework 與 .NET Core 之間的 API 差距。</span><span class="sxs-lookup"><span data-stu-id="36189-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="36189-145">[Windows 相容性套件](/dotnet/core/porting/windows-compat-pack)在 .NET Core 中發佈了上千個僅供 Windows 使用的 API。</span><span class="sxs-lookup"><span data-stu-id="36189-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="36189-146">這些 API 並不適用於 .NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="36189-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="36189-147">如何下載範例</span><span class="sxs-lookup"><span data-stu-id="36189-147">How to download a sample</span></span>

<span data-ttu-id="36189-148">許多文章及教學課程都有包含範例程式碼的連結。</span><span class="sxs-lookup"><span data-stu-id="36189-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="36189-149">[下載 ASP.NET 存放庫 ZIP 檔案](https://codeload.github.com/aspnet/Docs/zip/master)。</span><span class="sxs-lookup"><span data-stu-id="36189-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="36189-150">解壓縮 *Docs-master.zip* 檔案。</span><span class="sxs-lookup"><span data-stu-id="36189-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="36189-151">使用範例連結中的 URL，協助您巡覽至範例目錄。</span><span class="sxs-lookup"><span data-stu-id="36189-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="36189-152">範例程式碼中的前置處理器指示詞</span><span class="sxs-lookup"><span data-stu-id="36189-152">Preprocessor directives in sample code</span></span>

<span data-ttu-id="36189-153">為了示範多種情節，範例應用程式會使用 `#define` 及 `#if-#else/#elif-#endif` 這兩個 C# 陳述式，從範例程式碼選取不同的區段來執行。</span><span class="sxs-lookup"><span data-stu-id="36189-153">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="36189-154">對於使用此法的範例，請將 C# 檔案頂端的 `#define` 陳述式，設定為您要執行之情節相關聯的符號。</span><span class="sxs-lookup"><span data-stu-id="36189-154">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="36189-155">部分範例會要求在多個檔案的頂端設定符號，以執行情節。</span><span class="sxs-lookup"><span data-stu-id="36189-155">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="36189-156">例如下列 `#define` 符號清單指出其提供四個情節 (每個符號一個情節)。</span><span class="sxs-lookup"><span data-stu-id="36189-156">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="36189-157">目前的範例設定會執行 `TemplateCode` 情節：</span><span class="sxs-lookup"><span data-stu-id="36189-157">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="36189-158">若要變更此範例，以執行 `ExpandDefault` 情節，請定義 `ExpandDefault` 符號，並將剩餘的符號設為註解：</span><span class="sxs-lookup"><span data-stu-id="36189-158">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="36189-159">如需如何使用 [C# 前置處理器指示詞](/dotnet/csharp/language-reference/preprocessor-directives/)，以選擇編譯不同之程式碼區段的詳細資訊，請參閱 [#define (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) 及 [#if (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)。</span><span class="sxs-lookup"><span data-stu-id="36189-159">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="36189-160">範例程式碼中的區域</span><span class="sxs-lookup"><span data-stu-id="36189-160">Regions in sample code</span></span>

<span data-ttu-id="36189-161">部分範例應用程式包含由 [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) 與 [#end-region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# 陳述式括住的程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="36189-161">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#end-region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="36189-162">文件建置系統會將這些區域插入轉譯的文件主題中。</span><span class="sxs-lookup"><span data-stu-id="36189-162">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="36189-163">區域名稱通常包含字組 "snippet"。</span><span class="sxs-lookup"><span data-stu-id="36189-163">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="36189-164">下列範例顯示了名為 `snippet_FilterInCode` 的區域：</span><span class="sxs-lookup"><span data-stu-id="36189-164">The following example shows a region named `snippet_FilterInCode`:</span></span>

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

<span data-ttu-id="36189-165">上述的 C# 程式碼片段會在主題的 Markdown 檔案中，透過以下程式行加以參考：</span><span class="sxs-lookup"><span data-stu-id="36189-165">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="36189-166">您可以放心忽略 (或移除) 程式碼周圍的 `#region` 與 `#end-region` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="36189-166">You may safely ignore (or remove) the `#region` and `#end-region` statements that surround the code.</span></span> <span data-ttu-id="36189-167">但若您打算執行主題中所述的範例情境，則請勿變更這些陳述式內的程式碼。</span><span class="sxs-lookup"><span data-stu-id="36189-167">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="36189-168">您可以在試驗其他案例時自由改變程式碼。</span><span class="sxs-lookup"><span data-stu-id="36189-168">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="36189-169">如需詳細資訊，請參閱 [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets) (參與 ASP.NET 文件：程式碼片段)。</span><span class="sxs-lookup"><span data-stu-id="36189-169">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="36189-170">後續步驟</span><span class="sxs-lookup"><span data-stu-id="36189-170">Next steps</span></span>

<span data-ttu-id="36189-171">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="36189-171">For more information, see the following resources:</span></span>

* [<span data-ttu-id="36189-172">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="36189-172">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="36189-173">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="36189-173">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="36189-174">[每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃，</span><span class="sxs-lookup"><span data-stu-id="36189-174">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="36189-175">並提供新的部落格和協力廠商軟體。</span><span class="sxs-lookup"><span data-stu-id="36189-175">It features new blogs and third-party software.</span></span>

> [!NOTE]
> <span data-ttu-id="36189-176">我們正為規劃中的 ASP.NET Core 目錄新結構測試其可用性。</span><span class="sxs-lookup"><span data-stu-id="36189-176">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="36189-177">如果您有幾分鐘的時間可以嘗試在目前或建議的目錄中尋找 7 個不同主題，請[按一下這裡參加研究](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5)。</span><span class="sxs-lookup"><span data-stu-id="36189-177">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>