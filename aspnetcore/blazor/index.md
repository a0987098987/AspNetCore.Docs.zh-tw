---
title: ASP.NET Core 中的 Blazor 簡介
author: guardrex
description: 探索 ASP.NET Core Blazor，這是在 ASP.NET Core 應用程式中使用 .NET 建置互動式用戶端 Web UI 的方式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 09/05/2019
uid: blazor/index
ms.openlocfilehash: 6b62eb372d642c1ad9df880a4b71e5d5a8e40b60
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800321"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="54967-103">Blazor 簡介</span><span class="sxs-lookup"><span data-stu-id="54967-103">Introduction to Blazor</span></span>

<span data-ttu-id="54967-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="54967-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="54967-105">*歡迎使用 Blazor！*</span><span class="sxs-lookup"><span data-stu-id="54967-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="54967-106">Blazor 是一個可使用 .NET 來建置互動式用戶端 Web UI 的架構：</span><span class="sxs-lookup"><span data-stu-id="54967-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="54967-107">使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="54967-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="54967-108">共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="54967-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="54967-109">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="54967-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="54967-110">使用 .NET 進行用戶端網頁程式開發可提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="54967-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="54967-111">以 C# 撰寫而不是 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="54967-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="54967-112">利用 .NET 程式庫的現有 .NET 生態系統。</span><span class="sxs-lookup"><span data-stu-id="54967-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="54967-113">跨伺服器和用戶端共用應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="54967-113">Share app logic across server and client.</span></span>
* <span data-ttu-id="54967-114">從 .NET 的效能、可靠性和安全性中獲益。</span><span class="sxs-lookup"><span data-stu-id="54967-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="54967-115">使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。</span><span class="sxs-lookup"><span data-stu-id="54967-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="54967-116">以常用的語言、架構和工具建置，不僅穩定、功能豐富，而且容易使用。</span><span class="sxs-lookup"><span data-stu-id="54967-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="54967-117">元件</span><span class="sxs-lookup"><span data-stu-id="54967-117">Components</span></span>

<span data-ttu-id="54967-118">Blazor 應用程式的基礎為「元件」。</span><span class="sxs-lookup"><span data-stu-id="54967-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="54967-119">Blazor 中的元件為 UI 的元素，例如頁面、對話方塊或資料輸入表單。</span><span class="sxs-lookup"><span data-stu-id="54967-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="54967-120">元件是內建在 .NET 組件中且具有下列功能的 .NET 類別：</span><span class="sxs-lookup"><span data-stu-id="54967-120">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="54967-121">定義彈性 UI 轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="54967-121">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="54967-122">處理使用者動作。</span><span class="sxs-lookup"><span data-stu-id="54967-122">Handle user events.</span></span>
* <span data-ttu-id="54967-123">可以為巢狀結構，且可重複使用。</span><span class="sxs-lookup"><span data-stu-id="54967-123">Can be nested and reused.</span></span>
* <span data-ttu-id="54967-124">能以 [Razor 類別庫](xref:razor-pages/ui-class)或 [NuGet 套件](/nuget/what-is-nuget)方式共用及散發。</span><span class="sxs-lookup"><span data-stu-id="54967-124">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="54967-125">該元件類別通常使用副檔名為 *.razor* 的 [Razor](xref:mvc/views/razor) 標記頁面形式撰寫而成。</span><span class="sxs-lookup"><span data-stu-id="54967-125">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a *.razor* file extension.</span></span> <span data-ttu-id="54967-126">Blazor 中的元件正式稱為「Razor 元件」。</span><span class="sxs-lookup"><span data-stu-id="54967-126">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="54967-127">Razor 是結合了 HTML 標記與 C# 程式碼的語法，專為開發人員的生產力而設計。</span><span class="sxs-lookup"><span data-stu-id="54967-127">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="54967-128">Razor 可讓您在同一個具有 [IntelliSense](/visualstudio/ide/using-intellisense) 支援的檔案中，於 HTML 標記和 C# 之間切換。</span><span class="sxs-lookup"><span data-stu-id="54967-128">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="54967-129">Razor Pages 和 MVC 也會使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="54967-129">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="54967-130">不同於 Razor Pages 和 MVC，它們是圍繞著要求/回應模型而建置的，元件則是專門用來處理用戶端 UI 邏輯與組合。</span><span class="sxs-lookup"><span data-stu-id="54967-130">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="54967-131">下列 Razor 標記示範一個元件 (*Dialog.razor*)，此元件可巢狀於另一個元件內：</span><span class="sxs-lookup"><span data-stu-id="54967-131">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
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

<span data-ttu-id="54967-132">對話方塊的內文內容 (`ChildContent`) 和標題 (`Title`) 均由要在 UI 中使用此元件的元件所提供。</span><span class="sxs-lookup"><span data-stu-id="54967-132">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="54967-133">`OnYes` 是按鈕的 `onclick` 事件所觸發的 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="54967-133">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="54967-134">Blazor 會針對 UI 組合使用自然的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="54967-134">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="54967-135">HTML 元素會指定元件，而標記的屬性 (Attribute) 會將值傳遞至元件的屬性 (Property)。</span><span class="sxs-lookup"><span data-stu-id="54967-135">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="54967-136">在下列範例中， `Index` 元件使用 `Dialog` 元件。</span><span class="sxs-lookup"><span data-stu-id="54967-136">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="54967-137">`ChildContent` 與 `Title` 是由 `<Dialog>` 元素的屬性與內容設定的。</span><span class="sxs-lookup"><span data-stu-id="54967-137">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="54967-138">*Index.razor*：</span><span class="sxs-lookup"><span data-stu-id="54967-138">*Index.razor*:</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="54967-139">當您於瀏覽器中存取父代 (*Index.razor*) 時，即會轉譯此對話方塊：</span><span class="sxs-lookup"><span data-stu-id="54967-139">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![瀏覽器中轉譯的對話方塊元件](index/_static/dialog.png)

<span data-ttu-id="54967-141">在應用程式中使用此元件時，[Visual Studio](/visualstudio/ide/using-intellisense) 和 [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) \(英文\) 中的 IntelliSense 會藉由完成語法和參數來加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="54967-141">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="54967-142">元件會轉譯為瀏覽器文件物件模型 (DOM) 的記憶體內部表示法 (稱為「轉譯樹」)，可用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="54967-142">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="54967-143">Blazor 用戶端</span><span class="sxs-lookup"><span data-stu-id="54967-143">Blazor client-side</span></span>

<span data-ttu-id="54967-144">Blazor 用戶端是單一頁面應用程式架構，可使用 .NET 來建置互動式用戶端 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="54967-144">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="54967-145">Blazor 用戶端會使用開放式 Web 標準，而不需外掛程式或程式碼轉譯，且適用於包括行動瀏覽器在內的所有新式網頁瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="54967-145">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="54967-146">在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](https://webassembly.org) (縮寫為 *wasm*) 達成。</span><span class="sxs-lookup"><span data-stu-id="54967-146">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="54967-147">WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。</span><span class="sxs-lookup"><span data-stu-id="54967-147">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="54967-148">WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。</span><span class="sxs-lookup"><span data-stu-id="54967-148">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="54967-149">WebAssembly 程式碼可以透過 JavaScript 存取瀏覽器的完整功能，稱為 「JavaScript 互通性」(或 *JavaScript Interop*)。</span><span class="sxs-lookup"><span data-stu-id="54967-149">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="54967-150">在瀏覽器中透過 WebAssembly 執行的 .NET 程式碼會在瀏覽器的 JavaScript 沙箱執行，且受沙箱所提供對用戶端電腦之惡意動作的保護。</span><span class="sxs-lookup"><span data-stu-id="54967-150">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

![Blazor 用戶端會透過 WebAssembly 在瀏覽器中執行 .NET 程式碼。](index/_static/blazor-client-side.png)

<span data-ttu-id="54967-152">當 Blazor 用戶端應用程式建置好並在瀏覽器中執行時：</span><span class="sxs-lookup"><span data-stu-id="54967-152">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="54967-153">C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。</span><span class="sxs-lookup"><span data-stu-id="54967-153">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="54967-154">組件和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="54967-154">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="54967-155">Blazor 用戶端會啟動 .NET 執行階段，並設定執行階段以載入應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="54967-155">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="54967-156">Blazor 用戶端執行階段會使用 JavaScript Interop 來處理 DOM 操作和瀏覽器 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="54967-156">The Blazor client-side runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="54967-157">發佈的應用程式大小 (它的「承載大小」) 是應用程式使用性的重要效能因素。</span><span class="sxs-lookup"><span data-stu-id="54967-157">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="54967-158">大型應用程式需要相對較長的時間才能下載至瀏覽器，這點會對使用者體驗造成傷害。</span><span class="sxs-lookup"><span data-stu-id="54967-158">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> <span data-ttu-id="54967-159">Blazor 會將承載調整成最佳大小，以縮短下載時間：</span><span class="sxs-lookup"><span data-stu-id="54967-159">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="54967-160">若應用程式是透過[中繼語言 (IL) 連接器](xref:host-and-deploy/blazor/configure-linker)所發佈的，則會移除未使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="54967-160">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="54967-161">HTTP 回應會進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="54967-161">HTTP responses are compressed.</span></span>
* <span data-ttu-id="54967-162">.NET 執行階段與組件會在瀏覽器中進行快取。</span><span class="sxs-lookup"><span data-stu-id="54967-162">The .NET runtime and assemblies are cached in the browser.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="54967-163">Blazor 伺服器端</span><span class="sxs-lookup"><span data-stu-id="54967-163">Blazor server-side</span></span>

<span data-ttu-id="54967-164">Blazor 會將元件轉譯邏輯與套用 UI 更新的方式分隔。</span><span class="sxs-lookup"><span data-stu-id="54967-164">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="54967-165">Blazor 伺服器端提供從 ASP.NET Core 應用程式於伺服器上裝載 Razor 元件的支援。</span><span class="sxs-lookup"><span data-stu-id="54967-165">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="54967-166">UI 更新會透過 [SignalR](xref:signalr/introduction) 連線來處理。</span><span class="sxs-lookup"><span data-stu-id="54967-166">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="54967-167">執行階段會處理將 UI 事件從瀏覽器傳送到伺服器，然後在執行元件之後，套用由伺服器傳送回瀏覽器的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="54967-167">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="54967-168">Blazor 伺服器端用於與瀏覽器通訊的連線也會用於處理 JavaScript Interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="54967-168">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor 伺服器端在伺服器上執行 .NET 程式碼，並經由 SignalR 連線與用戶端上的文件物件模型互動](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a><span data-ttu-id="54967-170">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="54967-170">JavaScript interop</span></span>

<span data-ttu-id="54967-171">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 存取的應用程式，元件能夠和 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="54967-171">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="54967-172">元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="54967-172">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="54967-173">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="54967-173">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="54967-174">如需詳細資訊，請參閱 <xref:blazor/javascript-interop>。</span><span class="sxs-lookup"><span data-stu-id="54967-174">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="54967-175">程式碼共用和 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="54967-175">Code sharing and .NET Standard</span></span>

<span data-ttu-id="54967-176">Blazor 會實作 [.NET Standard 2.0](/dotnet/standard/net-standard)。</span><span class="sxs-lookup"><span data-stu-id="54967-176">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="54967-177">.NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。</span><span class="sxs-lookup"><span data-stu-id="54967-177">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="54967-178">.NET Standard 類別庫可與不同的 .NET 平台共用，例如 Blazor、.NET Framework、.NET Core、Xamarin、Mono 和 Unity。</span><span class="sxs-lookup"><span data-stu-id="54967-178">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="54967-179">網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端與執行緒) 均會擲回 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="54967-179">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="54967-180">其他資源</span><span class="sxs-lookup"><span data-stu-id="54967-180">Additional resources</span></span>

* [<span data-ttu-id="54967-181">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="54967-181">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [<span data-ttu-id="54967-182">C# 指南</span><span class="sxs-lookup"><span data-stu-id="54967-182">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="54967-183">HTML</span><span class="sxs-lookup"><span data-stu-id="54967-183">HTML</span></span>](https://www.w3.org/html/)
