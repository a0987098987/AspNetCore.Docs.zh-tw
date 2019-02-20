---
title: Blazor 簡介
author: guardrex
description: 探索 ASP.NET Core Blazor，這是使用 .NET 建置互動式用戶端應用程式的全新方式，透過此方式建置的應用程式可在具有 WebAssembly 的瀏覽器中執行。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: spa/blazor/index
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="62ce3-103">Blazor 簡介</span><span class="sxs-lookup"><span data-stu-id="62ce3-103">Introduction to Blazor</span></span>

<span data-ttu-id="62ce3-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="62ce3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="62ce3-105">Blazor 是單一頁面應用程式架構，適用於建置使用 .NET 的互動式用戶端 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="62ce3-105">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="62ce3-106">Blazor 使用開放式 Web 標準，而不需要讓和外掛程式或程式碼轉譯。</span><span class="sxs-lookup"><span data-stu-id="62ce3-106">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="62ce3-107">Blazor 適用於所有現代化網頁瀏覽器，包括行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="62ce3-107">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="62ce3-108">在瀏覽器中使用 .NET 進行用戶端 Web 開發提供許多優點：</span><span class="sxs-lookup"><span data-stu-id="62ce3-108">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="62ce3-109">**C# 語言**：以 C# 撰寫而不是 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="62ce3-109">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="62ce3-110">**.NET 生態系統**：利用現有的 .NET 程式庫生態系統。</span><span class="sxs-lookup"><span data-stu-id="62ce3-110">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="62ce3-111">**完整堆疊開發**：共用伺服器和用戶端邏輯。</span><span class="sxs-lookup"><span data-stu-id="62ce3-111">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="62ce3-112">**速度與延展性**：.NET 專為效能、可靠性和安全性設計。</span><span class="sxs-lookup"><span data-stu-id="62ce3-112">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="62ce3-113">**領先業界的工具**：使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。</span><span class="sxs-lookup"><span data-stu-id="62ce3-113">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="62ce3-114">**穩定性與一致性**：以平易近人的語言、架構和工具建置，穩定、功能豐富且容易使用。</span><span class="sxs-lookup"><span data-stu-id="62ce3-114">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="62ce3-115">在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](http://webassembly.org) (縮寫為 *wasm*) 達成。</span><span class="sxs-lookup"><span data-stu-id="62ce3-115">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="62ce3-116">WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。</span><span class="sxs-lookup"><span data-stu-id="62ce3-116">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="62ce3-117">WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。</span><span class="sxs-lookup"><span data-stu-id="62ce3-117">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="62ce3-118">WebAssembly 程式碼可以透過 JavaScript Interop 存取瀏覽器的完整功能。</span><span class="sxs-lookup"><span data-stu-id="62ce3-118">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="62ce3-119">在此同時，透過 WebAssembly 執行的 .NET 程式碼會在與 JavaScript 相同的受信任沙箱中執行，以防止對用戶端電腦執行惡意的動作。</span><span class="sxs-lookup"><span data-stu-id="62ce3-119">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor 在具有 WebAssembly 的瀏覽器中執行 .NET 程式碼。](index/_static/blazor.png)

<span data-ttu-id="62ce3-121">當建置 Blazor 應用程式並在瀏覽器中執行時：</span><span class="sxs-lookup"><span data-stu-id="62ce3-121">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="62ce3-122">C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。</span><span class="sxs-lookup"><span data-stu-id="62ce3-122">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="62ce3-123">組件和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="62ce3-123">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="62ce3-124">Blazor 啟動 .NET 執行階段，並設定執行階段以載入應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="62ce3-124">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="62ce3-125">Blazor 執行階段會透過 JavaScript 互通，處理文件物件模型 (DOM) 操作和瀏覽器 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="62ce3-125">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="62ce3-126">Blazor 支援大部分應用程式所需的核心功能，包括：</span><span class="sxs-lookup"><span data-stu-id="62ce3-126">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="62ce3-127">參數</span><span class="sxs-lookup"><span data-stu-id="62ce3-127">Parameters</span></span>
* <span data-ttu-id="62ce3-128">事件處理</span><span class="sxs-lookup"><span data-stu-id="62ce3-128">Event handling</span></span>
* <span data-ttu-id="62ce3-129">資料繫結</span><span class="sxs-lookup"><span data-stu-id="62ce3-129">Data binding</span></span>
* <span data-ttu-id="62ce3-130">路由</span><span class="sxs-lookup"><span data-stu-id="62ce3-130">Routing</span></span>
* <span data-ttu-id="62ce3-131">相依性插入</span><span class="sxs-lookup"><span data-stu-id="62ce3-131">Dependency injection</span></span>
* <span data-ttu-id="62ce3-132">版面配置</span><span class="sxs-lookup"><span data-stu-id="62ce3-132">Layouts</span></span>
* <span data-ttu-id="62ce3-133">範本化</span><span class="sxs-lookup"><span data-stu-id="62ce3-133">Templating</span></span>
* <span data-ttu-id="62ce3-134">階層式值</span><span class="sxs-lookup"><span data-stu-id="62ce3-134">Cascading values</span></span>

<span data-ttu-id="62ce3-135">透過[中繼語言 (IL) 連接器](xref:host-and-deploy/razor-components/configure-linker)發行時刪除應用程式未使用的程式碼，減少下載之應用程式的大小。</span><span class="sxs-lookup"><span data-stu-id="62ce3-135">To reduce the size of the downloaded app unused code stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components/configure-linker).</span></span>

<span data-ttu-id="62ce3-136">Blazor 是 Razor 元件的用戶端裝載模型。</span><span class="sxs-lookup"><span data-stu-id="62ce3-136">Blazor is the client-side hosting model for Razor Components.</span></span> <span data-ttu-id="62ce3-137">因為 Razor 元件解除了元件轉譯邏輯與 UI 更新套用方式的聯結，所以裝載 Razor 元件的方式具有彈性。</span><span class="sxs-lookup"><span data-stu-id="62ce3-137">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="62ce3-138">使用 ASP.NET Core Razor 元件，在 ASP.NET Core 應用程式裡將 Razor 元件裝載在伺服器上，使得透過 SignalR 連線處理所有的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="62ce3-138">Use ASP.NET Core Razor Components to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="62ce3-139">如需詳細資訊，請參閱<xref:razor-components/hosting-models#server-side-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="62ce3-139">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span> 

## <a name="components"></a><span data-ttu-id="62ce3-140">元件</span><span class="sxs-lookup"><span data-stu-id="62ce3-140">Components</span></span>

<span data-ttu-id="62ce3-141">*Razor 元件*是一種 UI，例如頁面、對話方塊或資料輸入表單。</span><span class="sxs-lookup"><span data-stu-id="62ce3-141">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="62ce3-142">元件會處理使用者事件，並定義彈性的 UI 轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="62ce3-142">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="62ce3-143">元件可以為巢狀，且可重複使用。</span><span class="sxs-lookup"><span data-stu-id="62ce3-143">Components can be nested and reused.</span></span>

<span data-ttu-id="62ce3-144">元件是內建在 .NET 組建內的 .NET 類別，能夠以 NuGet 套件的方式共用及散發。</span><span class="sxs-lookup"><span data-stu-id="62ce3-144">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="62ce3-145">此類別可使用 Razor 標記頁面 (*.cshtml*) 或 C# 類別 (*.cs*) 的形式撰寫。</span><span class="sxs-lookup"><span data-stu-id="62ce3-145">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="62ce3-146">[Razor](xref:mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。</span><span class="sxs-lookup"><span data-stu-id="62ce3-146">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="62ce3-147">Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。</span><span class="sxs-lookup"><span data-stu-id="62ce3-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="62ce3-148">Razor Pages 和 MVC 檢視也會使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="62ce3-148">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="62ce3-149">不同於 Razor Pages 和 MVC 檢視，它們是圍繞在要求/回應模型而建置，元件則是專門用來處理 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="62ce3-149">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="62ce3-150">Razor 元件可專門用來處理用戶端 UI 邏輯和組合。</span><span class="sxs-lookup"><span data-stu-id="62ce3-150">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="62ce3-151">下列標記是 Razor 檔案 (*DialogComponent.cshtml*) 中自訂對話方塊元件的範例：</span><span class="sxs-lookup"><span data-stu-id="62ce3-151">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

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

<span data-ttu-id="62ce3-152">在應用程式的其他地方使用此元件時，IntelliSense 會藉由語法和參數完成而加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="62ce3-152">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="62ce3-153">元件會轉譯至瀏覽器 DOM 的記憶體內表示法，稱為「轉譯樹」，接著能夠以彈性有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="62ce3-153">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="62ce3-154">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="62ce3-154">JavaScript interop</span></span>

<span data-ttu-id="62ce3-155">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，Blazor 能夠和 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="62ce3-155">For apps that require third-party JavaScript libraries and browser APIs, Blazor interoperates with JavaScript.</span></span> <span data-ttu-id="62ce3-156">元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="62ce3-156">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="62ce3-157">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="62ce3-157">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="62ce3-158">如需詳細資訊，請參閱 [JavaScript Interop](xref:razor-components/javascript-interop)。</span><span class="sxs-lookup"><span data-stu-id="62ce3-158">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="62ce3-159">程式碼共用和 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="62ce3-159">Code sharing and .NET Standard</span></span>

<span data-ttu-id="62ce3-160">應用程式可以參考並使用現有的 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="62ce3-160">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="62ce3-161">.NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。</span><span class="sxs-lookup"><span data-stu-id="62ce3-161">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="62ce3-162">支援 .NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="62ce3-162">.NET Standard 2.0 or higher is supported.</span></span> <span data-ttu-id="62ce3-163">網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端、執行緒作業和其他功能) 會擲回 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="62ce3-163">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="62ce3-164">.NET Standard 類別庫可以跨伺服端程式碼共用，以及在以瀏覽器為基礎的應用程式中共用。</span><span class="sxs-lookup"><span data-stu-id="62ce3-164">.NET Standard class libraries can be shared across server code and in browser-based apps.</span></span>

## <a name="optimization"></a><span data-ttu-id="62ce3-165">最佳化</span><span class="sxs-lookup"><span data-stu-id="62ce3-165">Optimization</span></span>

<span data-ttu-id="62ce3-166">承載大小是應用程式使用性的重要效能因素。</span><span class="sxs-lookup"><span data-stu-id="62ce3-166">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="62ce3-167">Blazor 對承載大小進行最佳化，以縮短下載時間：</span><span class="sxs-lookup"><span data-stu-id="62ce3-167">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="62ce3-168">未使用的 .NET 組件部分會在建置程序期間移除。</span><span class="sxs-lookup"><span data-stu-id="62ce3-168">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="62ce3-169">HTTP 回應會進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="62ce3-169">HTTP responses are compressed.</span></span>
* <span data-ttu-id="62ce3-170">.NET 執行階段與組件會在瀏覽器中進行快取。</span><span class="sxs-lookup"><span data-stu-id="62ce3-170">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="62ce3-171">[伺服器端 Razor 元件](xref:razor-components/index)藉由在伺服器端維護 .NET 組件、應用程式的組件和執行階段，提供比 Blazor 更小的承載大小。</span><span class="sxs-lookup"><span data-stu-id="62ce3-171">[Server-side Razor Components](xref:razor-components/index) provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="62ce3-172">Razor 元件應用程式只會提供標記檔案和靜態資產給用戶端。</span><span class="sxs-lookup"><span data-stu-id="62ce3-172">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62ce3-173">其他資源</span><span class="sxs-lookup"><span data-stu-id="62ce3-173">Additional resources</span></span>

* <xref:razor-components/index>
* [<span data-ttu-id="62ce3-174">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="62ce3-174">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="62ce3-175">C# 指南</span><span class="sxs-lookup"><span data-stu-id="62ce3-175">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="62ce3-176">HTML</span><span class="sxs-lookup"><span data-stu-id="62ce3-176">HTML</span></span>](https://www.w3.org/html/)
