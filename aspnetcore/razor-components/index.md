---
title: Razor 元件簡介
author: guardrex
description: 探索 ASP.NET Core Razor 元件，這是在 ASP.NET Core 應用程式中使用 .NET 建置互動式用戶端 Web UI 的方式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="bdf8d-103">Razor 元件簡介</span><span class="sxs-lookup"><span data-stu-id="bdf8d-103">Introduction to Razor Components</span></span>

<span data-ttu-id="bdf8d-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bdf8d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bdf8d-105">*Razor 元件*是使用 .NET 建置互動式用戶端 Web UI 的一種方式：</span><span class="sxs-lookup"><span data-stu-id="bdf8d-105">*Razor Components* is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="bdf8d-106">使用 C# 而不是 JavaScript 建置豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="bdf8d-107">共用伺服器端與用戶端應用程式邏輯都是以 .NET 撰寫。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="bdf8d-108">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="bdf8d-109">Razor 元件支援大部分應用程式所需的核心功能，包括：</span><span class="sxs-lookup"><span data-stu-id="bdf8d-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="bdf8d-110">參數</span><span class="sxs-lookup"><span data-stu-id="bdf8d-110">Parameters</span></span>
* <span data-ttu-id="bdf8d-111">事件處理</span><span class="sxs-lookup"><span data-stu-id="bdf8d-111">Event handling</span></span>
* <span data-ttu-id="bdf8d-112">資料繫結</span><span class="sxs-lookup"><span data-stu-id="bdf8d-112">Data binding</span></span>
* <span data-ttu-id="bdf8d-113">路由</span><span class="sxs-lookup"><span data-stu-id="bdf8d-113">Routing</span></span>
* <span data-ttu-id="bdf8d-114">相依性插入</span><span class="sxs-lookup"><span data-stu-id="bdf8d-114">Dependency injection</span></span>
* <span data-ttu-id="bdf8d-115">版面配置</span><span class="sxs-lookup"><span data-stu-id="bdf8d-115">Layouts</span></span>
* <span data-ttu-id="bdf8d-116">範本化</span><span class="sxs-lookup"><span data-stu-id="bdf8d-116">Templating</span></span>
* <span data-ttu-id="bdf8d-117">階層式值</span><span class="sxs-lookup"><span data-stu-id="bdf8d-117">Cascading values</span></span>

<span data-ttu-id="bdf8d-118">Razor 元件將元件轉譯邏輯從 UI 套用更新的方式分隔。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="bdf8d-119">.NET Core 3.0 中的 ASP.NET Core Razor 元件在 ASP.NET Core 應用程式裡新增支援以將 Razor 元件裝載在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="bdf8d-120">UI 更新透過 SignalR 連線處理。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="bdf8d-121">執行階段：</span><span class="sxs-lookup"><span data-stu-id="bdf8d-121">The runtime:</span></span>

* <span data-ttu-id="bdf8d-122">處理從瀏覽器傳送到伺服器的 UI 事件。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="bdf8d-123">在執行元件後，伺服器會將套用 UI 更新傳回瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="bdf8d-124">Razor 元件與瀏覽器通訊所使用的連線也會用來處理 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor 元件在伺服器上執行 .NET 程式碼，並透過 SignalR 連線與用戶端上的文件物件模型互動](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="bdf8d-126">如需詳細資訊，請參閱<xref:razor-components/hosting-models#server-side-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="bdf8d-127">*Blazor* 是實驗性的 Razor 元件用戶端裝載模型。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="bdf8d-128">Blazor 會在使用開放式 Web 標準，且不含任何外掛程式、任何程式碼轉譯的瀏覽器中，在 .NET 上執行。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="bdf8d-129">如需詳細資訊，請參閱<xref:razor-components/hosting-models#client-side-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-129">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="bdf8d-130">元件</span><span class="sxs-lookup"><span data-stu-id="bdf8d-130">Components</span></span>

<span data-ttu-id="bdf8d-131">*Razor 元件*是一種 UI，例如頁面、對話方塊或資料輸入表單。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="bdf8d-132">元件會處理使用者事件，並定義彈性的 UI 轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="bdf8d-133">元件可以為巢狀，且可重複使用。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-133">Components can be nested and reused.</span></span>

<span data-ttu-id="bdf8d-134">元件是內建在 .NET 組建內的 .NET 類別，能夠以 NuGet 套件的方式共用及散發。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="bdf8d-135">該類別通常以 Razor 標記頁面的形式編寫，它包含 *.razor* 副檔名。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-135">The class is normally written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="bdf8d-136">[Razor](xref:mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="bdf8d-137">Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="bdf8d-138">Razor Pages 和 MVC 檢視也會使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="bdf8d-139">不同於 Razor Pages 和 MVC 檢視，它們是圍繞在要求/回應模型而建置，元件則是專門用來處理 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="bdf8d-140">Razor 元件可專門用來處理用戶端 UI 邏輯和組合。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="bdf8d-141">下列標記是 Razor 檔案 (*DialogComponent.razor*) 中自訂對話方塊元件的範例：</span><span class="sxs-lookup"><span data-stu-id="bdf8d-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.razor*):</span></span>

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

<span data-ttu-id="bdf8d-142">在應用程式的其他地方使用此元件時，IntelliSense 會藉由語法和參數完成而加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="bdf8d-143">元件會轉譯至瀏覽器 DOM 的記憶體內表示法，稱為「轉譯樹」，接著能夠以彈性有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="bdf8d-144">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="bdf8d-144">JavaScript interop</span></span>

<span data-ttu-id="bdf8d-145">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，元件能夠和 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="bdf8d-146">元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="bdf8d-147">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="bdf8d-148">如需詳細資訊，請參閱 [JavaScript Interop](xref:razor-components/javascript-interop)。</span><span class="sxs-lookup"><span data-stu-id="bdf8d-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bdf8d-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="bdf8d-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="bdf8d-150">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="bdf8d-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="bdf8d-151">C# 指南</span><span class="sxs-lookup"><span data-stu-id="bdf8d-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="bdf8d-152">HTML</span><span class="sxs-lookup"><span data-stu-id="bdf8d-152">HTML</span></span>](https://www.w3.org/html/)
