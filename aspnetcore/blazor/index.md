---
title: ASP.NET核心簡介Blazor
author: guardrex
description: 探索ASP.NET核心Blazor,一種在ASP.NET核心應用程式中使用 .NET 構建互動式用戶端 Web UI 的方法。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 03/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/index
ms.openlocfilehash: 6d2e95cd2ec92f97a97cb558fb39e4540450c766
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80405946"
---
# <a name="introduction-to-aspnet-core-opno-locblazor"></a><span data-ttu-id="c639e-103">ASP.NET核心簡介Blazor</span><span class="sxs-lookup"><span data-stu-id="c639e-103">Introduction to ASP.NET Core Blazor</span></span>

<span data-ttu-id="c639e-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c639e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c639e-105">*歡迎光Blazor臨!*</span><span class="sxs-lookup"><span data-stu-id="c639e-105">*Welcome to Blazor!*</span></span>

Blazor<span data-ttu-id="c639e-106">是一個框架,用於使用 .NET 建構式用戶端 Web UI:</span><span class="sxs-lookup"><span data-stu-id="c639e-106"> is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="c639e-107">使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="c639e-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="c639e-108">共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="c639e-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="c639e-109">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c639e-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>
* <span data-ttu-id="c639e-110">與現代託管平臺(如[Docker)](/dotnet/standard/microservices-architecture/container-docker-introduction/index)集成。</span><span class="sxs-lookup"><span data-stu-id="c639e-110">Integrate with modern hosting platforms, such as [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index).</span></span>

<span data-ttu-id="c639e-111">使用 .NET 進行用戶端網頁程式開發可提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="c639e-111">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="c639e-112">以 C# 撰寫而不是 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="c639e-112">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="c639e-113">利用 .NET 程式庫的現有 .NET 生態系統。</span><span class="sxs-lookup"><span data-stu-id="c639e-113">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="c639e-114">跨伺服器和用戶端共用應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="c639e-114">Share app logic across server and client.</span></span>
* <span data-ttu-id="c639e-115">從 .NET 的效能、可靠性和安全性中獲益。</span><span class="sxs-lookup"><span data-stu-id="c639e-115">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="c639e-116">使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。</span><span class="sxs-lookup"><span data-stu-id="c639e-116">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="c639e-117">以常用的語言、架構和工具建置，不僅穩定、功能豐富，而且容易使用。</span><span class="sxs-lookup"><span data-stu-id="c639e-117">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="c639e-118">元件</span><span class="sxs-lookup"><span data-stu-id="c639e-118">Components</span></span>

Blazor<span data-ttu-id="c639e-119">應用程式以*元件*。</span><span class="sxs-lookup"><span data-stu-id="c639e-119"> apps are based on *components*.</span></span> <span data-ttu-id="c639e-120">中的Blazor元件是 UI 的元素,如頁面、對話框或數據輸入窗體。</span><span class="sxs-lookup"><span data-stu-id="c639e-120">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="c639e-121">元件是內建在 .NET 組件中且具有下列功能的 .NET 類別：</span><span class="sxs-lookup"><span data-stu-id="c639e-121">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="c639e-122">定義彈性 UI 轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="c639e-122">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="c639e-123">處理使用者動作。</span><span class="sxs-lookup"><span data-stu-id="c639e-123">Handle user events.</span></span>
* <span data-ttu-id="c639e-124">可以為巢狀結構，且可重複使用。</span><span class="sxs-lookup"><span data-stu-id="c639e-124">Can be nested and reused.</span></span>
* <span data-ttu-id="c639e-125">能以 [Razor 類別庫](xref:razor-pages/ui-class)或 [NuGet 套件](/nuget/what-is-nuget)方式共用及散發。</span><span class="sxs-lookup"><span data-stu-id="c639e-125">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="c639e-126">元件類通常以[Razor](xref:mvc/views/razor)標記頁的形式編寫,該頁具有 *.razor*檔案副檔名。</span><span class="sxs-lookup"><span data-stu-id="c639e-126">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a *.razor* file extension.</span></span> <span data-ttu-id="c639e-127">群組Blazor被正式稱為*剃刀元件*。</span><span class="sxs-lookup"><span data-stu-id="c639e-127">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="c639e-128">Razor 是結合了 HTML 標記與 C# 程式碼的語法，專為開發人員的生產力而設計。</span><span class="sxs-lookup"><span data-stu-id="c639e-128">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="c639e-129">Razor 可讓您在同一個具有 [IntelliSense](/visualstudio/ide/using-intellisense) 支援的檔案中，於 HTML 標記和 C# 之間切換。</span><span class="sxs-lookup"><span data-stu-id="c639e-129">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="c639e-130">Razor Pages 和 MVC 也會使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="c639e-130">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="c639e-131">不同於 Razor Pages 和 MVC，它們是圍繞著要求/回應模型而建置的，元件則是專門用來處理用戶端 UI 邏輯與組合。</span><span class="sxs-lookup"><span data-stu-id="c639e-131">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="c639e-132">下列 Razor 標記示範一個元件 (*Dialog.razor*)，此元件可巢狀於另一個元件內：</span><span class="sxs-lookup"><span data-stu-id="c639e-132">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```razor
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

<span data-ttu-id="c639e-133">對話方塊的內文內容 (`ChildContent`) 和標題 (`Title`) 均由要在 UI 中使用此元件的元件所提供。</span><span class="sxs-lookup"><span data-stu-id="c639e-133">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="c639e-134">`OnYes` 是按鈕的 `onclick` 事件所觸發的 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="c639e-134">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

Blazor<span data-ttu-id="c639e-135">使用自然 HTML 標記進行 UI 合成。</span><span class="sxs-lookup"><span data-stu-id="c639e-135"> uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="c639e-136">HTML 元素會指定元件，而標記的屬性 (Attribute) 會將值傳遞至元件的屬性 (Property)。</span><span class="sxs-lookup"><span data-stu-id="c639e-136">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="c639e-137">在下列範例中， `Index` 元件使用 `Dialog` 元件。</span><span class="sxs-lookup"><span data-stu-id="c639e-137">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="c639e-138">`ChildContent` 與 `Title` 是由 `<Dialog>` 元素的屬性與內容設定的。</span><span class="sxs-lookup"><span data-stu-id="c639e-138">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="c639e-139">*Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="c639e-139">*Index.razor*:</span></span>

```razor
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="c639e-140">當您於瀏覽器中存取父代 (*Index.razor*) 時，即會轉譯此對話方塊：</span><span class="sxs-lookup"><span data-stu-id="c639e-140">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![瀏覽器中轉譯的對話方塊元件](index/_static/dialog.png)

<span data-ttu-id="c639e-142">在應用程式中使用此元件時，[Visual Studio](/visualstudio/ide/using-intellisense) 和 [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) \(英文\) 中的 IntelliSense 會藉由完成語法和參數來加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="c639e-142">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="c639e-143">元件會轉譯為瀏覽器文件物件模型 (DOM) 的記憶體內部表示法 (稱為「轉譯樹」\*\*)，可用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="c639e-143">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="c639e-144">網路組裝</span><span class="sxs-lookup"><span data-stu-id="c639e-144"> WebAssembly</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="c639e-145">WebAssembly 是單頁應用框架,用於使用 .NET 建構互動式用戶端 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="c639e-145"> WebAssembly is a single-page app framework for building interactive client-side web apps with .NET.</span></span> Blazor<span data-ttu-id="c639e-146">WebAssembly 使用開放式 Web 標準,無需外掛程式或代碼轉用,適用於所有現代 Web 瀏覽器(包括行動瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="c639e-146"> WebAssembly uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="c639e-147">在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](https://webassembly.org) (縮寫為 *wasm*) 達成。</span><span class="sxs-lookup"><span data-stu-id="c639e-147">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="c639e-148">WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。</span><span class="sxs-lookup"><span data-stu-id="c639e-148">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="c639e-149">WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。</span><span class="sxs-lookup"><span data-stu-id="c639e-149">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="c639e-150">WebAssembly 程式碼可以透過 JavaScript 存取瀏覽器的完整功能，稱為 「JavaScript 互通性」\*\*(或 *JavaScript Interop*)。</span><span class="sxs-lookup"><span data-stu-id="c639e-150">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="c639e-151">在瀏覽器中透過 WebAssembly 執行的 .NET 程式碼會在瀏覽器的 JavaScript 沙箱執行，且受沙箱所提供對用戶端電腦之惡意動作的保護。</span><span class="sxs-lookup"><span data-stu-id="c639e-151">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

<span data-ttu-id="c639e-152">![BlazorWebAssembly 在瀏覽器中使用 Web 程式集運行 .NET 代碼。](index/_static/blazor-webassembly.png)</span><span class="sxs-lookup"><span data-stu-id="c639e-152">![Blazor WebAssembly runs .NET code in the browser with WebAssembly.](index/_static/blazor-webassembly.png)</span></span>

<span data-ttu-id="c639e-153">在Blazor瀏覽器產生與執行 WebAssembly 應用程式時:</span><span class="sxs-lookup"><span data-stu-id="c639e-153">When a Blazor WebAssembly app is built and run in a browser:</span></span>

* <span data-ttu-id="c639e-154">C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。</span><span class="sxs-lookup"><span data-stu-id="c639e-154">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="c639e-155">組件和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c639e-155">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* Blazor<span data-ttu-id="c639e-156">WebAssembly 將引導 .NET 運行時並配置運行時以載入應用的程式集。</span><span class="sxs-lookup"><span data-stu-id="c639e-156"> WebAssembly bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="c639e-157">Blazor WebAssembly 執行時使用 JAvaScript 互通來處理 DOM 操作和瀏覽器 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="c639e-157">The Blazor WebAssembly runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="c639e-158">發佈的應用程式大小 (它的「承載大小」\*\*) 是應用程式使用性的重要效能因素。</span><span class="sxs-lookup"><span data-stu-id="c639e-158">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="c639e-159">大型應用程式需要相對較長的時間才能下載至瀏覽器，這點會對使用者體驗造成傷害。</span><span class="sxs-lookup"><span data-stu-id="c639e-159">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> Blazor<span data-ttu-id="c639e-160">WebAssembly 優化有效負載大小以減少下載時間:</span><span class="sxs-lookup"><span data-stu-id="c639e-160"> WebAssembly optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="c639e-161">若應用程式是透過[中繼語言 (IL) 連接器](xref:host-and-deploy/blazor/configure-linker)所發佈的，則會移除未使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="c639e-161">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="c639e-162">HTTP 回應會進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="c639e-162">HTTP responses are compressed.</span></span>
* <span data-ttu-id="c639e-163">.NET 執行階段與組件會在瀏覽器中進行快取。</span><span class="sxs-lookup"><span data-stu-id="c639e-163">The .NET runtime and assemblies are cached in the browser.</span></span>

## <a name="opno-locblazor-server"></a>Blazor<span data-ttu-id="c639e-164">伺服器</span><span class="sxs-lookup"><span data-stu-id="c639e-164"> Server</span></span>

Blazor<span data-ttu-id="c639e-165">將元件呈現邏輯與 UI 更新的應用方式分離。</span><span class="sxs-lookup"><span data-stu-id="c639e-165"> decouples component rendering logic from how UI updates are applied.</span></span> Blazor<span data-ttu-id="c639e-166">伺服器支援在 ASP.NET核心應用中在伺服器上託管 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="c639e-166"> Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="c639e-167">UI 更新[SignalR](xref:signalr/introduction)通過 連接處理。</span><span class="sxs-lookup"><span data-stu-id="c639e-167">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="c639e-168">執行階段會處理將 UI 事件從瀏覽器傳送到伺服器，然後在執行元件之後，套用由伺服器傳送回瀏覽器的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="c639e-168">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="c639e-169">Blazor伺服器用於與瀏覽器通信的連接也用於處理 JAVAScript 互通調用。</span><span class="sxs-lookup"><span data-stu-id="c639e-169">The connection used by Blazor Server to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

<span data-ttu-id="c639e-170">![Blazor伺服器在伺服器上執行 .NET 代碼SignalR,並在 用戶端上透過連接與文件物件模型進行互動](index/_static/blazor-server.png)</span><span class="sxs-lookup"><span data-stu-id="c639e-170">![Blazor Server runs .NET code on the server and interacts with the Document Object Model on the client over a SignalR connection](index/_static/blazor-server.png)</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="c639e-171">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="c639e-171">JavaScript interop</span></span>

<span data-ttu-id="c639e-172">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 存取的應用程式，元件能夠和 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="c639e-172">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="c639e-173">元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="c639e-173">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="c639e-174">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="c639e-174">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="c639e-175">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="c639e-175">For more information, see the following articles:</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="c639e-176">程式碼共用和 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="c639e-176">Code sharing and .NET Standard</span></span>

Blazor<span data-ttu-id="c639e-177">實[作 .NET 標準 2.0](/dotnet/standard/net-standard)。</span><span class="sxs-lookup"><span data-stu-id="c639e-177"> implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="c639e-178">.NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。</span><span class="sxs-lookup"><span data-stu-id="c639e-178">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="c639e-179">.NET 標準類庫可以在不同的 .NET 平臺(Blazor如 .NET 框架、.NET Core、Xamarin、Mono 和 Unity)之間共用。</span><span class="sxs-lookup"><span data-stu-id="c639e-179">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="c639e-180">網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端與執行緒) 均會擲回 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="c639e-180">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c639e-181">其他資源</span><span class="sxs-lookup"><span data-stu-id="c639e-181">Additional resources</span></span>

* [<span data-ttu-id="c639e-182">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="c639e-182">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* <xref:tutorials/signalr-blazor-webassembly>
* [<span data-ttu-id="c639e-183">C# 指南</span><span class="sxs-lookup"><span data-stu-id="c639e-183">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="c639e-184">Html</span><span class="sxs-lookup"><span data-stu-id="c639e-184">HTML</span></span>](https://www.w3.org/html/)
* <span data-ttu-id="c639e-185">[真棒Blazor](https://github.com/AdrienTorris/awesome-blazor)社區連結</span><span class="sxs-lookup"><span data-stu-id="c639e-185">[Awesome Blazor](https://github.com/AdrienTorris/awesome-blazor) community links</span></span>
