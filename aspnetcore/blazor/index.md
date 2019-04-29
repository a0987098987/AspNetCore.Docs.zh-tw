---
title: ASP.NET Core 中的 Blazor 簡介
author: guardrex
description: 探索 ASP.NET Core Blazor，這是在 ASP.NET Core 應用程式中使用 .NET 建置互動式用戶端 Web UI 的方式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982993"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="88be8-103">Blazor 簡介</span><span class="sxs-lookup"><span data-stu-id="88be8-103">Introduction to Blazor</span></span>

<span data-ttu-id="88be8-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="88be8-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="88be8-105">歡迎使用 Blazor！</span><span class="sxs-lookup"><span data-stu-id="88be8-105">Welcome to Blazor!</span></span>

<span data-ttu-id="88be8-106">使用.NET 建立互動式用戶端 Web UI：</span><span class="sxs-lookup"><span data-stu-id="88be8-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="88be8-107">使用 C# 而不是 JavaScript 建置豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="88be8-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="88be8-108">共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="88be8-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="88be8-109">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="88be8-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="88be8-110">Blazor 支援大部分應用程式所需的核心案例：</span><span class="sxs-lookup"><span data-stu-id="88be8-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="88be8-111">參數</span><span class="sxs-lookup"><span data-stu-id="88be8-111">Parameters</span></span>
* <span data-ttu-id="88be8-112">事件處理</span><span class="sxs-lookup"><span data-stu-id="88be8-112">Event handling</span></span>
* <span data-ttu-id="88be8-113">資料繫結</span><span class="sxs-lookup"><span data-stu-id="88be8-113">Data binding</span></span>
* <span data-ttu-id="88be8-114">路由</span><span class="sxs-lookup"><span data-stu-id="88be8-114">Routing</span></span>
* <span data-ttu-id="88be8-115">相依性插入</span><span class="sxs-lookup"><span data-stu-id="88be8-115">Dependency injection</span></span>
* <span data-ttu-id="88be8-116">版面配置</span><span class="sxs-lookup"><span data-stu-id="88be8-116">Layouts</span></span>
* <span data-ttu-id="88be8-117">範本</span><span class="sxs-lookup"><span data-stu-id="88be8-117">Templates</span></span>
* <span data-ttu-id="88be8-118">階層式值</span><span class="sxs-lookup"><span data-stu-id="88be8-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="88be8-119">元件</span><span class="sxs-lookup"><span data-stu-id="88be8-119">Components</span></span>

<span data-ttu-id="88be8-120">Blazor 中的「元件」是一種 UI 元素，例如頁面、對話方塊或資料輸入表單。</span><span class="sxs-lookup"><span data-stu-id="88be8-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="88be8-121">元件會處理使用者事件，並定義彈性的 UI 轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="88be8-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="88be8-122">元件可以為巢狀，且可重複使用。</span><span class="sxs-lookup"><span data-stu-id="88be8-122">Components can be nested and reused.</span></span>

<span data-ttu-id="88be8-123">元件是內建在 .NET 組建內的 .NET 類別，能夠以 NuGet 套件的方式共用及散發。</span><span class="sxs-lookup"><span data-stu-id="88be8-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="88be8-124">該元件類別通常使用副檔名為 *.razor* 的 Razor 標記頁面形式撰寫而成。</span><span class="sxs-lookup"><span data-stu-id="88be8-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="88be8-125">Blazor 中的元件有時也稱為 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="88be8-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="88be8-126">[Razor](xref:mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。</span><span class="sxs-lookup"><span data-stu-id="88be8-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="88be8-127">Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。</span><span class="sxs-lookup"><span data-stu-id="88be8-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="88be8-128">Razor Pages 和 MVC 檢視也會使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="88be8-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="88be8-129">不同於 Razor Pages 和 MVC 檢視，它們是圍繞在要求/回應模型而建置，元件則是專門用來處理 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="88be8-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="88be8-130">Razor 元件可專門用來處理用戶端 UI 邏輯和組合。</span><span class="sxs-lookup"><span data-stu-id="88be8-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="88be8-131">下列標記是自訂對話方塊元件的範例：</span><span class="sxs-lookup"><span data-stu-id="88be8-131">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="88be8-132">在應用程式的其他地方使用此元件時，[Visual Studio](https://visualstudio.microsoft.com/vs/) 中的 IntelliSense 會藉由語法和參數完成加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="88be8-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="88be8-133">元件會轉譯至瀏覽器 DOM 的記憶體內表示法，稱為「轉譯樹」，接著能夠以彈性有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="88be8-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="88be8-134">Blazor 伺服器端</span><span class="sxs-lookup"><span data-stu-id="88be8-134">Blazor server-side</span></span>

<span data-ttu-id="88be8-135">Blazor 會將元件轉譯邏輯與套用 UI 更新的方式分隔。</span><span class="sxs-lookup"><span data-stu-id="88be8-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="88be8-136">Blazor 伺服器端提供從 ASP.NET Core 應用程式於伺服器上裝載 Razor 元件的支援。</span><span class="sxs-lookup"><span data-stu-id="88be8-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="88be8-137">UI 更新透過 SignalR 連線處理。</span><span class="sxs-lookup"><span data-stu-id="88be8-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="88be8-138">執行階段：</span><span class="sxs-lookup"><span data-stu-id="88be8-138">The runtime:</span></span>

* <span data-ttu-id="88be8-139">處理從瀏覽器傳送到伺服器的 UI 事件。</span><span class="sxs-lookup"><span data-stu-id="88be8-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="88be8-140">在執行元件後，伺服器會將套用 UI 更新傳回瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="88be8-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="88be8-141">Blazor 伺服器端用於與瀏覽器通訊的連線也會用於處理 JavaScript Interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="88be8-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor 伺服器端在伺服器上執行 .NET 程式碼，並經由 SignalR 連線與用戶端上的文件物件模型互動](index/_static/blazor-server-side.png)

<span data-ttu-id="88be8-143">如需詳細資訊，請參閱<xref:blazor/hosting-models#server-side>。</span><span class="sxs-lookup"><span data-stu-id="88be8-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="88be8-144">Blazor 用戶端</span><span class="sxs-lookup"><span data-stu-id="88be8-144">Blazor client-side</span></span>

<span data-ttu-id="88be8-145">Blazor 用戶端是單一頁面應用程式架構，用於使用 .NET 建置互動式用戶端 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="88be8-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="88be8-146">Blazor 用戶端使用開放式 Web 標準，不需要轉譯外掛程式或程式碼。</span><span class="sxs-lookup"><span data-stu-id="88be8-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="88be8-147">Blazor 用戶端適用於所有新式網頁瀏覽器，包括行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="88be8-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="88be8-148">在瀏覽器中使用 .NET 進行用戶端 Web 開發提供許多優點：</span><span class="sxs-lookup"><span data-stu-id="88be8-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="88be8-149">**C# 語言**：以 C# 撰寫而不是 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="88be8-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="88be8-150">**.NET 生態系統**：利用現有的 .NET 程式庫生態系統。</span><span class="sxs-lookup"><span data-stu-id="88be8-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="88be8-151">**完整堆疊開發**：共用伺服器和用戶端邏輯。</span><span class="sxs-lookup"><span data-stu-id="88be8-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="88be8-152">**速度與延展性**：.NET 專為效能、可靠性和安全性設計。</span><span class="sxs-lookup"><span data-stu-id="88be8-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="88be8-153">**領先業界的工具**：使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。</span><span class="sxs-lookup"><span data-stu-id="88be8-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="88be8-154">**穩定性與一致性**：以常用的語言、架構和工具建置，不僅穩定、功能豐富，而且容易使用。</span><span class="sxs-lookup"><span data-stu-id="88be8-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="88be8-155">在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](http://webassembly.org) (縮寫為 *wasm*) 達成。</span><span class="sxs-lookup"><span data-stu-id="88be8-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="88be8-156">WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。</span><span class="sxs-lookup"><span data-stu-id="88be8-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="88be8-157">WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。</span><span class="sxs-lookup"><span data-stu-id="88be8-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="88be8-158">WebAssembly 程式碼可以透過 JavaScript Interop 存取瀏覽器的完整功能。</span><span class="sxs-lookup"><span data-stu-id="88be8-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="88be8-159">在此同時，透過 WebAssembly 執行的 .NET 程式碼會在與 JavaScript 相同的受信任沙箱中執行，以防止對用戶端電腦執行惡意的動作。</span><span class="sxs-lookup"><span data-stu-id="88be8-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor 用戶端會透過 WebAssembly 在瀏覽器中執行 .NET 程式碼。](index/_static/blazor-client-side.png)

<span data-ttu-id="88be8-161">當 Blazor 用戶端應用程式建置好並在瀏覽器中執行時：</span><span class="sxs-lookup"><span data-stu-id="88be8-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="88be8-162">C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。</span><span class="sxs-lookup"><span data-stu-id="88be8-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="88be8-163">組件和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="88be8-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="88be8-164">Blazor 用戶端會啟動 .NET 執行階段，並設定執行階段以載入應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="88be8-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="88be8-165">Blazor 執行階段會透過 JavaScript Interop 處理文件物件模型 (DOM) 操作和瀏覽器 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="88be8-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="88be8-166">為了減少下載之應用程式的大小，透過[中繼語言 (IL) 連接器](xref:host-and-deploy/blazor/configure-linker)發行時會刪除應用程式未使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="88be8-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="88be8-167">Blazor 用戶端是用戶端裝載模型。</span><span class="sxs-lookup"><span data-stu-id="88be8-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="88be8-168">因為 Blazor 解除了元件轉譯邏輯與 UI 更新套用方式的聯結，所以裝載 Blazor 的方式具有彈性。</span><span class="sxs-lookup"><span data-stu-id="88be8-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="88be8-169">使用 [Blazor 伺服器端](#blazor-server-side)可從 ASP.NET Core 應用程式在伺服器上裝載 Blazor，以透過 SignalR 連線處理 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="88be8-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="88be8-170">如需詳細資訊，請參閱<xref:blazor/hosting-models#server-side>。</span><span class="sxs-lookup"><span data-stu-id="88be8-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="88be8-171">承載大小是應用程式使用性的重要效能因素。</span><span class="sxs-lookup"><span data-stu-id="88be8-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="88be8-172">Blazor 會將承載調整成最佳大小，以縮短下載時間：</span><span class="sxs-lookup"><span data-stu-id="88be8-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="88be8-173">未使用的 .NET 組件部分會在建置程序期間移除。</span><span class="sxs-lookup"><span data-stu-id="88be8-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="88be8-174">HTTP 回應會進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="88be8-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="88be8-175">.NET 執行階段與組件會在瀏覽器中進行快取。</span><span class="sxs-lookup"><span data-stu-id="88be8-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="88be8-176">[Blazor 伺服器端](#blazor-server-side)藉由維護 .NET 組件、應用程式的組件以及執行階段伺服器端，可提供較 Blazor 用戶端小的承載大小。</span><span class="sxs-lookup"><span data-stu-id="88be8-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="88be8-177">Blazor 伺服器端應用程式只提供標記檔案和靜態資產給用戶端。</span><span class="sxs-lookup"><span data-stu-id="88be8-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="88be8-178">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="88be8-178">JavaScript interop</span></span>

<span data-ttu-id="88be8-179">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，元件能夠和 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="88be8-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="88be8-180">元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="88be8-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="88be8-181">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="88be8-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="88be8-182">如需詳細資訊，請參閱 [JavaScript Interop](xref:blazor/javascript-interop)。</span><span class="sxs-lookup"><span data-stu-id="88be8-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="88be8-183">程式碼共用和 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="88be8-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="88be8-184">應用程式可以參考並使用現有的 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="88be8-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="88be8-185">.NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。</span><span class="sxs-lookup"><span data-stu-id="88be8-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="88be8-186">Blazor 實作 .NET Standard 2.0。</span><span class="sxs-lookup"><span data-stu-id="88be8-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="88be8-187">網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端、執行緒作業和其他功能) 會擲回 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="88be8-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="88be8-188">.NET Standard 類別庫可與不同的 .NET 平台共用，例如 Blazor、.NET Framework、.NET Core、Xamarin、Mono 和 Unity。</span><span class="sxs-lookup"><span data-stu-id="88be8-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="88be8-189">其他資源</span><span class="sxs-lookup"><span data-stu-id="88be8-189">Additional resources</span></span>

* [<span data-ttu-id="88be8-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="88be8-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="88be8-191">C# 指南</span><span class="sxs-lookup"><span data-stu-id="88be8-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="88be8-192">HTML</span><span class="sxs-lookup"><span data-stu-id="88be8-192">HTML</span></span>](https://www.w3.org/html/)
