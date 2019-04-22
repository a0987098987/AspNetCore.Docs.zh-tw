---
title: ASP.NET Core 中的 Blazor 簡介
author: guardrex
description: 探索 ASP.NET Core Blazor，這是在 ASP.NET Core 應用程式中使用 .NET 建置互動式用戶端 Web UI 的方式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614572"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="f7c45-103">Blazor 簡介</span><span class="sxs-lookup"><span data-stu-id="f7c45-103">Introduction to Blazor</span></span>

<span data-ttu-id="f7c45-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f7c45-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="f7c45-105">Razor 元件</span><span class="sxs-lookup"><span data-stu-id="f7c45-105">Razor Components</span></span>

<span data-ttu-id="f7c45-106">Blazor 的伺服器端裝載模型 *Razor 元件*，是使用 .NET 建置互動式用戶端 Web UI 的一種方式：</span><span class="sxs-lookup"><span data-stu-id="f7c45-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="f7c45-107">使用 C# 而不是 JavaScript 建置豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="f7c45-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="f7c45-108">共用伺服器端與用戶端應用程式邏輯都是以 .NET 撰寫。</span><span class="sxs-lookup"><span data-stu-id="f7c45-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="f7c45-109">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f7c45-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="f7c45-110">Razor 元件支援大部分應用程式所需的核心功能，包括：</span><span class="sxs-lookup"><span data-stu-id="f7c45-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="f7c45-111">參數</span><span class="sxs-lookup"><span data-stu-id="f7c45-111">Parameters</span></span>
* <span data-ttu-id="f7c45-112">事件處理</span><span class="sxs-lookup"><span data-stu-id="f7c45-112">Event handling</span></span>
* <span data-ttu-id="f7c45-113">資料繫結</span><span class="sxs-lookup"><span data-stu-id="f7c45-113">Data binding</span></span>
* <span data-ttu-id="f7c45-114">路由</span><span class="sxs-lookup"><span data-stu-id="f7c45-114">Routing</span></span>
* <span data-ttu-id="f7c45-115">相依性插入</span><span class="sxs-lookup"><span data-stu-id="f7c45-115">Dependency injection</span></span>
* <span data-ttu-id="f7c45-116">版面配置</span><span class="sxs-lookup"><span data-stu-id="f7c45-116">Layouts</span></span>
* <span data-ttu-id="f7c45-117">範本化</span><span class="sxs-lookup"><span data-stu-id="f7c45-117">Templating</span></span>
* <span data-ttu-id="f7c45-118">階層式值</span><span class="sxs-lookup"><span data-stu-id="f7c45-118">Cascading values</span></span>

<span data-ttu-id="f7c45-119">Razor 元件將元件轉譯邏輯從 UI 套用更新的方式分隔。</span><span class="sxs-lookup"><span data-stu-id="f7c45-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="f7c45-120">.NET Core 3.0 中的 ASP.NET Core Razor 元件在 ASP.NET Core 應用程式裡新增支援以將 Razor 元件裝載在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="f7c45-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="f7c45-121">UI 更新透過 SignalR 連線處理。</span><span class="sxs-lookup"><span data-stu-id="f7c45-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="f7c45-122">執行階段：</span><span class="sxs-lookup"><span data-stu-id="f7c45-122">The runtime:</span></span>

* <span data-ttu-id="f7c45-123">處理從瀏覽器傳送到伺服器的 UI 事件。</span><span class="sxs-lookup"><span data-stu-id="f7c45-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="f7c45-124">在執行元件後，伺服器會將套用 UI 更新傳回瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f7c45-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="f7c45-125">Razor 元件與瀏覽器通訊所使用的連線也會用來處理 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="f7c45-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor 元件在伺服器上執行 .NET 程式碼，並透過 SignalR 連線與用戶端上的文件物件模型互動](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="f7c45-127">如需詳細資訊，請參閱<xref:blazor/hosting-models#server-side-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="f7c45-128">元件</span><span class="sxs-lookup"><span data-stu-id="f7c45-128">Components</span></span>

<span data-ttu-id="f7c45-129">*Razor 元件*是一種 UI，例如頁面、對話方塊或資料輸入表單。</span><span class="sxs-lookup"><span data-stu-id="f7c45-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="f7c45-130">元件會處理使用者事件，並定義彈性的 UI 轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="f7c45-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="f7c45-131">元件可以為巢狀，且可重複使用。</span><span class="sxs-lookup"><span data-stu-id="f7c45-131">Components can be nested and reused.</span></span>

<span data-ttu-id="f7c45-132">元件是內建在 .NET 組建內的 .NET 類別，能夠以 NuGet 套件的方式共用及散發。</span><span class="sxs-lookup"><span data-stu-id="f7c45-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="f7c45-133">該類別通常以 Razor 標記頁面的形式撰寫，副檔名為 *.razor* (Razor 元件) 或副檔名 *.cshtml* (Blazor) 的 Razor 標記頁面。</span><span class="sxs-lookup"><span data-stu-id="f7c45-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="f7c45-134">[Razor](xref:mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。</span><span class="sxs-lookup"><span data-stu-id="f7c45-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="f7c45-135">Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。</span><span class="sxs-lookup"><span data-stu-id="f7c45-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="f7c45-136">Razor Pages 和 MVC 檢視也會使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="f7c45-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="f7c45-137">不同於 Razor Pages 和 MVC 檢視，它們是圍繞在要求/回應模型而建置，元件則是專門用來處理 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="f7c45-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="f7c45-138">Razor 元件可專門用來處理用戶端 UI 邏輯和組合。</span><span class="sxs-lookup"><span data-stu-id="f7c45-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="f7c45-139">下列標記是自訂對話方塊元件的範例：</span><span class="sxs-lookup"><span data-stu-id="f7c45-139">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="f7c45-140">在應用程式的其他地方使用此元件時，IntelliSense 會藉由語法和參數完成而加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="f7c45-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="f7c45-141">元件會轉譯至瀏覽器 DOM 的記憶體內表示法，稱為「轉譯樹」，接著能夠以彈性有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="f7c45-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="f7c45-142">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="f7c45-142">JavaScript interop</span></span>

<span data-ttu-id="f7c45-143">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，元件能夠和 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="f7c45-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="f7c45-144">元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="f7c45-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="f7c45-145">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7c45-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="f7c45-146">如需詳細資訊，請參閱 [JavaScript Interop](xref:blazor/javascript-interop)。</span><span class="sxs-lookup"><span data-stu-id="f7c45-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="f7c45-147">程式碼共用和 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="f7c45-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="f7c45-148">應用程式可以參考並使用現有的 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="f7c45-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="f7c45-149">.NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。</span><span class="sxs-lookup"><span data-stu-id="f7c45-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="f7c45-150">Blazor 實作 .NET Standard 2.0。</span><span class="sxs-lookup"><span data-stu-id="f7c45-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="f7c45-151">網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端、執行緒作業和其他功能) 會擲回 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="f7c45-152">.NET Standard 類別庫可跨不同的 .NET 平台 (例如 Blazor、.NET Framework、.NET Core、Xamarin、Mono 和 Unity) 進行共用。</span><span class="sxs-lookup"><span data-stu-id="f7c45-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="f7c45-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="f7c45-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="f7c45-154">Blazor 是單一頁面應用程式架構，適用於建置使用 .NET 的互動式用戶端 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7c45-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="f7c45-155">Blazor 使用開放式 Web 標準，而不需要讓和外掛程式或程式碼轉譯。</span><span class="sxs-lookup"><span data-stu-id="f7c45-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="f7c45-156">Blazor 適用於所有現代化網頁瀏覽器，包括行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f7c45-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="f7c45-157">在瀏覽器中使用 .NET 進行用戶端 Web 開發提供許多優點：</span><span class="sxs-lookup"><span data-stu-id="f7c45-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="f7c45-158">**C# 語言**：以 C# 撰寫而不是 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="f7c45-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="f7c45-159">**.NET 生態系統**：利用現有的 .NET 程式庫生態系統。</span><span class="sxs-lookup"><span data-stu-id="f7c45-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="f7c45-160">**完整堆疊開發**：共用伺服器和用戶端邏輯。</span><span class="sxs-lookup"><span data-stu-id="f7c45-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="f7c45-161">**速度與延展性**：.NET 專為效能、可靠性和安全性設計。</span><span class="sxs-lookup"><span data-stu-id="f7c45-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="f7c45-162">**領先業界的工具**：使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。</span><span class="sxs-lookup"><span data-stu-id="f7c45-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="f7c45-163">**穩定性與一致性**：以平易近人的語言、架構和工具建置，穩定、功能豐富且容易使用。</span><span class="sxs-lookup"><span data-stu-id="f7c45-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="f7c45-164">在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](http://webassembly.org) (縮寫為 *wasm*) 達成。</span><span class="sxs-lookup"><span data-stu-id="f7c45-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="f7c45-165">WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。</span><span class="sxs-lookup"><span data-stu-id="f7c45-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="f7c45-166">WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。</span><span class="sxs-lookup"><span data-stu-id="f7c45-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="f7c45-167">WebAssembly 程式碼可以透過 JavaScript Interop 存取瀏覽器的完整功能。</span><span class="sxs-lookup"><span data-stu-id="f7c45-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="f7c45-168">在此同時，透過 WebAssembly 執行的 .NET 程式碼會在與 JavaScript 相同的受信任沙箱中執行，以防止對用戶端電腦執行惡意的動作。</span><span class="sxs-lookup"><span data-stu-id="f7c45-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor 在具有 WebAssembly 的瀏覽器中執行 .NET 程式碼。](index/_static/blazor.png)

<span data-ttu-id="f7c45-170">當建置 Blazor 應用程式並在瀏覽器中執行時：</span><span class="sxs-lookup"><span data-stu-id="f7c45-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="f7c45-171">C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。</span><span class="sxs-lookup"><span data-stu-id="f7c45-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="f7c45-172">組件和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="f7c45-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="f7c45-173">Blazor 啟動 .NET 執行階段，並設定執行階段以載入應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="f7c45-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="f7c45-174">Blazor 執行階段會透過 JavaScript 互通，處理文件物件模型 (DOM) 操作和瀏覽器 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="f7c45-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="f7c45-175">Blazor 支援大部分應用程式所需的核心功能，包括：</span><span class="sxs-lookup"><span data-stu-id="f7c45-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="f7c45-176">參數</span><span class="sxs-lookup"><span data-stu-id="f7c45-176">Parameters</span></span>
* <span data-ttu-id="f7c45-177">事件處理</span><span class="sxs-lookup"><span data-stu-id="f7c45-177">Event handling</span></span>
* <span data-ttu-id="f7c45-178">資料繫結</span><span class="sxs-lookup"><span data-stu-id="f7c45-178">Data binding</span></span>
* <span data-ttu-id="f7c45-179">路由</span><span class="sxs-lookup"><span data-stu-id="f7c45-179">Routing</span></span>
* <span data-ttu-id="f7c45-180">相依性插入</span><span class="sxs-lookup"><span data-stu-id="f7c45-180">Dependency injection</span></span>
* <span data-ttu-id="f7c45-181">版面配置</span><span class="sxs-lookup"><span data-stu-id="f7c45-181">Layouts</span></span>
* <span data-ttu-id="f7c45-182">範本化</span><span class="sxs-lookup"><span data-stu-id="f7c45-182">Templating</span></span>
* <span data-ttu-id="f7c45-183">階層式值</span><span class="sxs-lookup"><span data-stu-id="f7c45-183">Cascading values</span></span>

<span data-ttu-id="f7c45-184">為了減少下載之應用程式的大小，透過[中繼語言 (IL) 連接器](xref:host-and-deploy/blazor/configure-linker)發行時會刪除應用程式未使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7c45-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="f7c45-185">Blazor 用戶端是用戶端裝載模型。</span><span class="sxs-lookup"><span data-stu-id="f7c45-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="f7c45-186">因為 Blazor 解除了元件轉譯邏輯與 UI 更新套用方式的聯結，所以裝載 Blazor 的方式具有彈性。</span><span class="sxs-lookup"><span data-stu-id="f7c45-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="f7c45-187">使用 ASP.NET Core [Razor 元件](#razor-components)，在 ASP.NET Core 應用程式裡將 Razor 元件裝載在伺服器上，使得透過 SignalR 連線處理所有的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="f7c45-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="f7c45-188">如需詳細資訊，請參閱<xref:blazor/hosting-models#server-side-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="f7c45-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="f7c45-189">承載大小是應用程式使用性的重要效能因素。</span><span class="sxs-lookup"><span data-stu-id="f7c45-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="f7c45-190">Blazor 對承載大小進行最佳化，以縮短下載時間：</span><span class="sxs-lookup"><span data-stu-id="f7c45-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="f7c45-191">未使用的 .NET 組件部分會在建置程序期間移除。</span><span class="sxs-lookup"><span data-stu-id="f7c45-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="f7c45-192">HTTP 回應會進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="f7c45-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="f7c45-193">.NET 執行階段與組件會在瀏覽器中進行快取。</span><span class="sxs-lookup"><span data-stu-id="f7c45-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="f7c45-194">[Razor 元件](#razor-components)藉由在伺服器端維護 .NET 組件、應用程式的組件和執行階段，提供比 Blazor 更小的承載大小。</span><span class="sxs-lookup"><span data-stu-id="f7c45-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="f7c45-195">Razor 元件應用程式只會提供標記檔案和靜態資產給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f7c45-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7c45-196">其他資源</span><span class="sxs-lookup"><span data-stu-id="f7c45-196">Additional resources</span></span>

* [<span data-ttu-id="f7c45-197">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="f7c45-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="f7c45-198">C# 指南</span><span class="sxs-lookup"><span data-stu-id="f7c45-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="f7c45-199">HTML</span><span class="sxs-lookup"><span data-stu-id="f7c45-199">HTML</span></span>](https://www.w3.org/html/)
