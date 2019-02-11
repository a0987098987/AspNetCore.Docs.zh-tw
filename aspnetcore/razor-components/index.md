---
title: Razor 元件簡介
author: guardrex
description: 瀏覽 Blazor，這是使用 C#/Razor 和 HTML 的 .NET Web 架構，在具有 WebAssembly 的瀏覽器中執行。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667910"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="243e4-103">Razor 元件簡介</span><span class="sxs-lookup"><span data-stu-id="243e4-103">Introduction to Razor Components</span></span>

<span data-ttu-id="243e4-104">作者：[Steve Sanderson](http://blog.stevensanderson.com)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="243e4-104">By [Steve Sanderson](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="243e4-105">「ASP.NET Core Razor 元件」會在 ASP.NET Core 的伺服器端執行，而 *Blazor* (在用戶端執行的 Razor 元件) 是使用 C#/Razor 和 HTML 的實驗性 .NET Web 架構，在具有 WebAssembly 的瀏覽器中執行。</span><span class="sxs-lookup"><span data-stu-id="243e4-105">*ASP.NET Core Razor Components* executes server-side in ASP.NET Core, while *Blazor* (Razor Components executed client-side) is an experimental .NET web framework using C#/Razor and HTML that runs in the browser with WebAssembly.</span></span> <span data-ttu-id="243e4-106">Blazor 提供在用戶端上使用 .NET 的所有用戶端 Web UI 架構優點。</span><span class="sxs-lookup"><span data-stu-id="243e4-106">Blazor provides all of the benefits of a client-side web UI framework using .NET on the client.</span></span>

<span data-ttu-id="243e4-107">多年來，Web 開發已在許多方面改善，但建置現代化 Web 應用程式仍然造成一些挑戰。</span><span class="sxs-lookup"><span data-stu-id="243e4-107">Web development has improved in many ways over the years, but building modern web apps still poses challenges.</span></span> <span data-ttu-id="243e4-108">在瀏覽器中使用 .NET 提供了許多優點，可協助您更輕鬆且更具生產力地進行 Web 開發：</span><span class="sxs-lookup"><span data-stu-id="243e4-108">Using .NET in the browser offers many advantages that can help make web development easier and more productive:</span></span>

* <span data-ttu-id="243e4-109">**穩定性與一致性**：.NET 提供跨平台的標準化程式設計架構，不僅穩定、功能豐富，且容易使用。</span><span class="sxs-lookup"><span data-stu-id="243e4-109">**Stability and consistency**: .NET provides standardized programming frameworks across platforms that are stable, feature-rich, and easy to use.</span></span>
* <span data-ttu-id="243e4-110">**現代化的創新語言**：.NET 語言會持續以創新的新語言功能來改善。</span><span class="sxs-lookup"><span data-stu-id="243e4-110">**Modern innovative languages**: .NET languages are constantly improving with innovative new language features.</span></span>
* <span data-ttu-id="243e4-111">**領先業界的工具**：Visual Studio 產品系列在 Windows、Linux 和 macOS 上提供跨平台的卓越 .NET 開發體驗。</span><span class="sxs-lookup"><span data-stu-id="243e4-111">**Industry-leading tools**: The Visual Studio product family provides a fantastic .NET development experience across platforms on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="243e4-112">**速度和延展性**：.NET 有堅強的應用程式開發效能、可靠性和安全性歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="243e4-112">**Speed and scalability**: .NET has a strong history of performance, reliability, and security for app development.</span></span> <span data-ttu-id="243e4-113">使用 .NET 作為完整堆疊解決方案，可讓您更輕鬆地建置快速、可靠且安全的應用程式。</span><span class="sxs-lookup"><span data-stu-id="243e4-113">Using .NET as a full-stack solution makes it easier to build fast, reliable, and secure apps.</span></span>
* <span data-ttu-id="243e4-114">**運用現有技能的完整堆疊開發**：C#/Razor 開發人員使用其現有的 C#/Razor 技能，撰寫用戶端程式碼，並在應用程式之間共用伺服器和用戶端的邏輯。</span><span class="sxs-lookup"><span data-stu-id="243e4-114">**Full-stack development that leverages existing skills**: C#/Razor developers use their existing C#/Razor skills to write client-side code and share server and client-side logic among apps.</span></span>
* <span data-ttu-id="243e4-115">**廣泛的瀏覽器支援**：Razor 元件將 UI 轉譯為一般的標記和 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="243e4-115">**Wide browser support**: Razor Components render the UI as ordinary markup and JavaScript.</span></span> <span data-ttu-id="243e4-116">Blazor 會在使用開放式 Web 標準，且不含任何外掛程式、任何程式碼轉譯的瀏覽器中，在 .NET 上執行。</span><span class="sxs-lookup"><span data-stu-id="243e4-116">Blazor runs on .NET in the browser using open web standards with no plugins and no code transpilation.</span></span> <span data-ttu-id="243e4-117">Blazor 適用於所有現代化網頁瀏覽器，包括行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="243e4-117">Blazor works in all modern web browsers, including mobile browsers.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="243e4-118">裝載模型</span><span class="sxs-lookup"><span data-stu-id="243e4-118">Hosting models</span></span>

### <a name="server-side-hosting-model"></a><span data-ttu-id="243e4-119">伺服器端裝載模型</span><span class="sxs-lookup"><span data-stu-id="243e4-119">Server-side hosting model</span></span>

<span data-ttu-id="243e4-120">因為 Razor 元件解除了元件轉譯邏輯與 UI 更新套用方式的聯結，所以裝載 Razor 元件的方式具有彈性。</span><span class="sxs-lookup"><span data-stu-id="243e4-120">Because Razor Components decouple a component's rendering logic from how the UI updates are applied, there is flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="243e4-121">.NET Core 3.0 中的 ASP.NET Core Razor 元件在 ASP.NET Core 應用程式裡新增支援以將 Razor 元件裝載在伺服器上，使得透過 SignalR 連線處理所有的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="243e4-121">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app where all UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="243e4-122">執行階段會處理從瀏覽器將 UI 事件傳送到伺服器，然後在執行元件之後，套用由伺服器傳送回瀏覽器的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="243e4-122">The runtime handles sending UI events from the browser to the server and then applies UI updates sent by the server back to the browser after running the components.</span></span> <span data-ttu-id="243e4-123">相同的連線也用來處理 JavaScript Interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="243e4-123">The same connection is also used to handle JavaScript interop calls.</span></span>

![Razor 元件在伺服器上執行 .NET 程式碼，並透過 SignalR 連線與用戶端上的文件物件模型互動](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="243e4-125">如需詳細資訊，請參閱<xref:razor-components/hosting-models#server-side-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="243e4-125">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

### <a name="client-side-hosting-model"></a><span data-ttu-id="243e4-126">用戶端裝載模型</span><span class="sxs-lookup"><span data-stu-id="243e4-126">Client-side hosting model</span></span>

<span data-ttu-id="243e4-127">在網頁瀏覽器內執行 .NET 程式碼，已經藉由一項相當新的技術達成：[WebAssembly](http://webassembly.org) (縮寫為 *wasm*)。</span><span class="sxs-lookup"><span data-stu-id="243e4-127">Running .NET code inside web browsers is made possible by a relatively new technology, [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="243e4-128">WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。</span><span class="sxs-lookup"><span data-stu-id="243e4-128">WebAssembly is an open web standard and is supported in web browsers without plugins.</span></span> <span data-ttu-id="243e4-129">WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。</span><span class="sxs-lookup"><span data-stu-id="243e4-129">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="243e4-130">WebAssembly 程式碼可以透過 JavaScript Interop 存取瀏覽器的完整功能。</span><span class="sxs-lookup"><span data-stu-id="243e4-130">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="243e4-131">在此同時，WebAssembly 程式碼會在與 JavaScript 相同的受信任沙箱中執行，以防止對用戶端電腦執行惡意的動作。</span><span class="sxs-lookup"><span data-stu-id="243e4-131">At the same time, WebAssembly code runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor 在具有 WebAssembly 的瀏覽器中執行 .NET 程式碼。](index/_static/blazor.png)

<span data-ttu-id="243e4-133">當建置 Blazor 應用程式並在瀏覽器中執行時：</span><span class="sxs-lookup"><span data-stu-id="243e4-133">When a Blazor app is built and run in a browser:</span></span>

1. <span data-ttu-id="243e4-134">C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。</span><span class="sxs-lookup"><span data-stu-id="243e4-134">C# code files and Razor files are compiled into .NET assemblies.</span></span>
1. <span data-ttu-id="243e4-135">組件和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="243e4-135">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
1. <span data-ttu-id="243e4-136">Blazor 使用 JavaScript 來啟動 .NET 執行階段，並設定執行階段以載入必要的組件參考。</span><span class="sxs-lookup"><span data-stu-id="243e4-136">Blazor uses JavaScript to bootstrap the .NET runtime and configures the runtime to load required assembly references.</span></span> <span data-ttu-id="243e4-137">Blazor 執行階段會透過 JavaScript 互通性，處理文件物件模型 (DOM) 操作和瀏覽器 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="243e4-137">Document object model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interoperability.</span></span>

<span data-ttu-id="243e4-138">若要支援那些不支援 WebAssembly 的較舊瀏覽器，您可以使用 ASP.NET Core Razor 元件[伺服器端裝載模型](#server-side-hosting-model)。</span><span class="sxs-lookup"><span data-stu-id="243e4-138">To support older browsers that don't support WebAssembly, you can use the ASP.NET Core Razor Components [server-side hosting model](#server-side-hosting-model).</span></span>

<span data-ttu-id="243e4-139">如需詳細資訊，請參閱<xref:razor-components/hosting-models#client-side-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="243e4-139">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="243e4-140">元件</span><span class="sxs-lookup"><span data-stu-id="243e4-140">Components</span></span>

<span data-ttu-id="243e4-141">應用程式是使用「元件」建置。</span><span class="sxs-lookup"><span data-stu-id="243e4-141">Apps are built with *components*.</span></span> <span data-ttu-id="243e4-142">元件是一種 UI，例如頁面、對話方塊或資料輸入表單。</span><span class="sxs-lookup"><span data-stu-id="243e4-142">A component is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="243e4-143">元件可以為巢狀結構、可重複使用，且在專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="243e4-143">Components can be nested, reused, and shared between projects.</span></span>

<span data-ttu-id="243e4-144">「元件」是一種 .NET 類別。</span><span class="sxs-lookup"><span data-stu-id="243e4-144">A *component* is a .NET class.</span></span> <span data-ttu-id="243e4-145">其可以直接撰寫為 C# 類別 (*\*.cs*)，或更常見的 Razor 標記頁面形式 (*\*.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="243e4-145">The class can either be written directly, as a C# class (*\*.cs*), or more commonly in the form of a Razor markup page (*\*.cshtml*).</span></span>

<span data-ttu-id="243e4-146">[Razor](/aspnet/core/mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。</span><span class="sxs-lookup"><span data-stu-id="243e4-146">[Razor](/aspnet/core/mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="243e4-147">Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。</span><span class="sxs-lookup"><span data-stu-id="243e4-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="243e4-148">下列標記是 Razor 檔案中的基本自訂對話方塊元件範例 (*DialogComponent.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="243e4-148">The following markup is an example of a basic custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="243e4-149">在應用程式的其他地方使用此元件時，IntelliSense 會藉由語法和參數完成而加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="243e4-149">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="243e4-150">元件可以是：</span><span class="sxs-lookup"><span data-stu-id="243e4-150">Components can be:</span></span>

* <span data-ttu-id="243e4-151">巢狀結構。</span><span class="sxs-lookup"><span data-stu-id="243e4-151">Nested.</span></span>
* <span data-ttu-id="243e4-152">使用 Razor (*\*.cshtml*) 或 C# (*\*.cs*) 程式碼建立。</span><span class="sxs-lookup"><span data-stu-id="243e4-152">Created with Razor (*\*.cshtml*) or C# (*\*.cs*) code.</span></span>
* <span data-ttu-id="243e4-153">透過類別庫而共用。</span><span class="sxs-lookup"><span data-stu-id="243e4-153">Shared via class libraries.</span></span>
* <span data-ttu-id="243e4-154">進行單元測試而不需要瀏覽器 DOM。</span><span class="sxs-lookup"><span data-stu-id="243e4-154">Unit tested without requiring a browser DOM.</span></span>

## <a name="infrastructure"></a><span data-ttu-id="243e4-155">基礎結構</span><span class="sxs-lookup"><span data-stu-id="243e4-155">Infrastructure</span></span>

<span data-ttu-id="243e4-156">Razor 元件提供大部分應用程式需要的核心功能，包括：</span><span class="sxs-lookup"><span data-stu-id="243e4-156">Razor Components offers the core facilities that most apps require, including:</span></span>

* <span data-ttu-id="243e4-157">版面配置</span><span class="sxs-lookup"><span data-stu-id="243e4-157">Layouts</span></span>
* <span data-ttu-id="243e4-158">路由</span><span class="sxs-lookup"><span data-stu-id="243e4-158">Routing</span></span>
* <span data-ttu-id="243e4-159">相依性插入</span><span class="sxs-lookup"><span data-stu-id="243e4-159">Dependency injection</span></span>

<span data-ttu-id="243e4-160">所有這些功能皆為選擇性。</span><span class="sxs-lookup"><span data-stu-id="243e4-160">All of these features are optional.</span></span> <span data-ttu-id="243e4-161">當這些功能的其中一個未在應用程式中使用時，且由 [Intermediate Language (IL) 連結器](xref:host-and-deploy/razor-components/configure-linker) 發佈時會從應用程式移除實作。</span><span class="sxs-lookup"><span data-stu-id="243e4-161">When one of these features isn't used in an app, the implementation is stripped out of the app when published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components/configure-linker).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="243e4-162">程式碼共用和 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="243e4-162">Code sharing and .NET Standard</span></span>

<span data-ttu-id="243e4-163">應用程式可以參考並使用現有的 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="243e4-163">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="243e4-164">.NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。</span><span class="sxs-lookup"><span data-stu-id="243e4-164">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="243e4-165">支援 .NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="243e4-165">.NET Standard 2.0 or higher is supported.</span></span> <span data-ttu-id="243e4-166">網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端、執行緒作業和其他功能) 會擲回 [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception)。</span><span class="sxs-lookup"><span data-stu-id="243e4-166">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception).</span></span> <span data-ttu-id="243e4-167">.NET Standard 類別庫可以跨伺服端程式碼共用，以及在以瀏覽器為基礎的應用程式中共用。</span><span class="sxs-lookup"><span data-stu-id="243e4-167">.NET Standard class libraries can be shared across server code and in browser-based apps.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="243e4-168">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="243e4-168">JavaScript interop</span></span>

<span data-ttu-id="243e4-169">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，WebAssembly 設計為與 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="243e4-169">For apps that require third-party JavaScript libraries and browser APIs, WebAssembly is designed to interoperate with JavaScript.</span></span> <span data-ttu-id="243e4-170">Razor 元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="243e4-170">Razor Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="243e4-171">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="243e4-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="243e4-172">如需詳細資訊，請參閱 [JavaScript Interop](xref:razor-components/javascript-interop)。</span><span class="sxs-lookup"><span data-stu-id="243e4-172">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="optimization"></a><span data-ttu-id="243e4-173">最佳化</span><span class="sxs-lookup"><span data-stu-id="243e4-173">Optimization</span></span>

<span data-ttu-id="243e4-174">對於用戶端應用程式而言，承載大小很重要。</span><span class="sxs-lookup"><span data-stu-id="243e4-174">For client-side apps, payload size is critical.</span></span> <span data-ttu-id="243e4-175">Blazor 對承載大小進行最佳化，以縮短下載時間。</span><span class="sxs-lookup"><span data-stu-id="243e4-175">Blazor optimizes payload size to reduce download times.</span></span> <span data-ttu-id="243e4-176">比方說，在建置程序期間會移除未使用的 .NET 組件部分、HTTP 回應會壓縮，而 .NET 執行階段和組件會在瀏覽器中快取。</span><span class="sxs-lookup"><span data-stu-id="243e4-176">For example, unused parts of .NET assemblies are removed during the build process, HTTP responses are compressed, and the .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="243e4-177">Razor 元件藉由在伺服器端維護 .NET 組件、應用程式的組件和執行階段，提供比 Blazor 更小的承載大小。</span><span class="sxs-lookup"><span data-stu-id="243e4-177">Razor Components provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="243e4-178">Razor 元件應用程式只會提供標記、指令碼和樣式表給用戶端。</span><span class="sxs-lookup"><span data-stu-id="243e4-178">Razor Components apps only serve markup, script, and stylesheets to clients.</span></span>

## <a name="deployment"></a><span data-ttu-id="243e4-179">部署</span><span class="sxs-lookup"><span data-stu-id="243e4-179">Deployment</span></span>

<span data-ttu-id="243e4-180">使用 Blazor 建置一個單純的獨立用戶端應用程式，或同時包含伺服器和用戶端應用程式的完整堆疊 ASP.NET Core 應用程式：</span><span class="sxs-lookup"><span data-stu-id="243e4-180">Use Blazor to build a pure standalone client-side app or a full-stack ASP.NET Core app that contains both server and client apps:</span></span>

* <span data-ttu-id="243e4-181">在**獨立用戶端應用程式**中，Blazor 應用程式會編譯成僅包含靜態檔案的 *dist* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="243e4-181">In a **standalone client-side app**, the Blazor app is compiled into a *dist* folder that only contains static files.</span></span> <span data-ttu-id="243e4-182">檔案可以裝載於 Azure App Service、GitHub Pages、IIS (設定為靜態檔案伺服器)、Node.js 伺服器和其他許多伺服器和服務上。</span><span class="sxs-lookup"><span data-stu-id="243e4-182">The files can be hosted on Azure App Service, GitHub Pages, IIS (configured as a static file server), Node.js servers, and many other servers and services.</span></span> <span data-ttu-id="243e4-183">在生產的伺服器上，不需要 .NET。</span><span class="sxs-lookup"><span data-stu-id="243e4-183">.NET isn't required on the server in production.</span></span>
* <span data-ttu-id="243e4-184">在**完整堆疊 ASP.NET Core 應用程式**中，可以在伺服器和用戶端應用程式之間共用程式碼。</span><span class="sxs-lookup"><span data-stu-id="243e4-184">In a **full-stack ASP.NET Core app**, code can be shared between server and client apps.</span></span> <span data-ttu-id="243e4-185">產生的 ASP.NET Core Razor 元件應用程式，會提供用戶端 UI 和其他伺服器端 API 端點，其可被建置並部署到 ASP.NET Core 支援的任何雲端或內部部署主機。</span><span class="sxs-lookup"><span data-stu-id="243e4-185">The resulting ASP.NET Core Razor Components app, which serves the client-side UI and other server-side API endpoints, can be built and deployed to any cloud or on-premise host supported by ASP.NET Core.</span></span>

## <a name="suggest-a-feature-or-file-a-bug-report"></a><span data-ttu-id="243e4-186">建議功能或提出 Bug 報告</span><span class="sxs-lookup"><span data-stu-id="243e4-186">Suggest a feature or file a bug report</span></span>

<span data-ttu-id="243e4-187">若要提出建議和提出 Bug 報告，請[開立問題](https://github.com/aspnet/AspNetCore/issues/new)。</span><span class="sxs-lookup"><span data-stu-id="243e4-187">To make suggestions and file bug reports, please [open an issue](https://github.com/aspnet/AspNetCore/issues/new).</span></span> <span data-ttu-id="243e4-188">如需一般說明並從社群取得答案，請加入 [Gitter](https://gitter.im/aspnet/Blazor) 上的交談。</span><span class="sxs-lookup"><span data-stu-id="243e4-188">For general help and to get answers from the community, join the conversation on [Gitter](https://gitter.im/aspnet/Blazor).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="243e4-189">其他資源</span><span class="sxs-lookup"><span data-stu-id="243e4-189">Additional resources</span></span>

* [<span data-ttu-id="243e4-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="243e4-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="243e4-191">C# 指南</span><span class="sxs-lookup"><span data-stu-id="243e4-191">C# Guide</span></span>](/dotnet/csharp/)
* [<span data-ttu-id="243e4-192">Razor</span><span class="sxs-lookup"><span data-stu-id="243e4-192">Razor</span></span>](/aspnet/core/mvc/views/razor)
* [<span data-ttu-id="243e4-193">HTML</span><span class="sxs-lookup"><span data-stu-id="243e4-193">HTML</span></span>](https://www.w3.org/html/)
