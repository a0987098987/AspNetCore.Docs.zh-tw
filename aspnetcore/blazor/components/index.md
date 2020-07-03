---
title: 建立和使用 ASP.NET Core Razor 元件
author: guardrex
description: 瞭解如何建立和使用 Razor 元件，包括如何系結至資料、處理事件，以及管理元件生命週期。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/25/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/index
ms.openlocfilehash: d25dc3441c2373655558dfc101b899252a280814
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944441"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="1a379-103">建立和使用 ASP.NET Core Razor 元件</span><span class="sxs-lookup"><span data-stu-id="1a379-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="1a379-104">By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Tobias Bartsch](https://www.aveo-solutions.com/)</span><span class="sxs-lookup"><span data-stu-id="1a379-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Tobias Bartsch](https://www.aveo-solutions.com/)</span></span>

<span data-ttu-id="1a379-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/../common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1a379-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/../common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="1a379-106">應用程式是使用*元件*所建立。</span><span class="sxs-lookup"><span data-stu-id="1a379-106"> apps are built using *components*.</span></span> <span data-ttu-id="1a379-107">「元件」（component）是一種獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。</span><span class="sxs-lookup"><span data-stu-id="1a379-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="1a379-108">元件包含 HTML 標籤，以及插入資料或回應 UI 事件所需的處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="1a379-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="1a379-109">元件具有彈性且輕量。</span><span class="sxs-lookup"><span data-stu-id="1a379-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="1a379-110">它們可以在專案之間進行嵌套、重複使用及共用。</span><span class="sxs-lookup"><span data-stu-id="1a379-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="1a379-111">元件類別</span><span class="sxs-lookup"><span data-stu-id="1a379-111">Component classes</span></span>

<span data-ttu-id="1a379-112">元件會 [Razor](xref:mvc/views/razor) `.razor` 使用 c # 和 HTML 標籤的組合，在元件檔（）中執行。</span><span class="sxs-lookup"><span data-stu-id="1a379-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (`.razor`) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="1a379-113">中的元件 Blazor 正式地稱為「元件」（ \* Razor component\*）。</span><span class="sxs-lookup"><span data-stu-id="1a379-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

### <a name="razor-syntax"></a>Razor<span data-ttu-id="1a379-114"> 語法</span><span class="sxs-lookup"><span data-stu-id="1a379-114"> syntax</span></span>

Razor<span data-ttu-id="1a379-115">應用程式中的元件會 Blazor 廣泛使用 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="1a379-115"> components in Blazor apps extensively use Razor syntax.</span></span> <span data-ttu-id="1a379-116">如果您不熟悉 Razor 標記語言，建議您先閱讀， <xref:mvc/views/razor> 再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="1a379-116">If you aren't familiar with the Razor markup language, we recommend reading <xref:mvc/views/razor> before proceeding.</span></span>

<span data-ttu-id="1a379-117">存取語法上的內容時 Razor ，請特別注意下列各節：</span><span class="sxs-lookup"><span data-stu-id="1a379-117">When accessing the content on Razor syntax, pay special attention to the following sections:</span></span>

* <span data-ttu-id="1a379-118">[Directives](xref:mvc/views/razor#directives)指示 `@` 詞：-前面加上的保留關鍵字，通常會變更元件標記的剖析或運作方式。</span><span class="sxs-lookup"><span data-stu-id="1a379-118">[Directives](xref:mvc/views/razor#directives): `@`-prefixed reserved keywords that typically change the way component markup is parsed or function.</span></span>
* <span data-ttu-id="1a379-119">指示詞[屬性](xref:mvc/views/razor#directive-attributes)： `@` -前面加上的保留關鍵字，通常會變更元件元素的剖析或運作方式。</span><span class="sxs-lookup"><span data-stu-id="1a379-119">[Directive attributes](xref:mvc/views/razor#directive-attributes): `@`-prefixed reserved keywords that typically change the way component elements are parsed or function.</span></span>

### <a name="names"></a><span data-ttu-id="1a379-120">Names</span><span class="sxs-lookup"><span data-stu-id="1a379-120">Names</span></span>

<span data-ttu-id="1a379-121">元件的名稱必須以大寫字元開頭。</span><span class="sxs-lookup"><span data-stu-id="1a379-121">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="1a379-122">例如， `MyCoolComponent.razor` 是有效的，而且 `myCoolComponent.razor` 無效。</span><span class="sxs-lookup"><span data-stu-id="1a379-122">For example, `MyCoolComponent.razor` is valid, and `myCoolComponent.razor` is invalid.</span></span>

### <a name="routing"></a><span data-ttu-id="1a379-123">路由</span><span class="sxs-lookup"><span data-stu-id="1a379-123">Routing</span></span>

<span data-ttu-id="1a379-124">中的路由 Blazor 會藉由提供路由範本給應用程式中每個可存取的元件來達成。</span><span class="sxs-lookup"><span data-stu-id="1a379-124">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span> <span data-ttu-id="1a379-125">當編譯具有指示詞的檔案時 Razor [`@page`][9] ，系統會指定路由範本給產生的類別 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> 。</span><span class="sxs-lookup"><span data-stu-id="1a379-125">When a Razor file with an [`@page`][9] directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="1a379-126">在執行時間，路由器會尋找具有的元件類別 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> ，並轉譯哪個元件具有符合所要求 URL 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="1a379-126">At runtime, the router looks for component classes with a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> and renders whichever component has a route template that matches the requested URL.</span></span> <span data-ttu-id="1a379-127">如需詳細資訊，請參閱 <xref:blazor/fundamentals/routing> 。</span><span class="sxs-lookup"><span data-stu-id="1a379-127">For more information, see <xref:blazor/fundamentals/routing>.</span></span>

```razor
@page "/ParentComponent"

...
```

### <a name="markup"></a><span data-ttu-id="1a379-128">標記</span><span class="sxs-lookup"><span data-stu-id="1a379-128">Markup</span></span>

<span data-ttu-id="1a379-129">元件的 UI 是使用 HTML 定義的。</span><span class="sxs-lookup"><span data-stu-id="1a379-129">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="1a379-130">動態轉譯邏輯（例如，迴圈、條件、運算式）是使用名為的內嵌 c # 語法加入 *Razor* 。</span><span class="sxs-lookup"><span data-stu-id="1a379-130">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called *Razor*.</span></span> <span data-ttu-id="1a379-131">編譯應用程式時，會將 HTML 標籤和 c # 轉譯邏輯轉換成元件類別。</span><span class="sxs-lookup"><span data-stu-id="1a379-131">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="1a379-132">產生的類別名稱與檔案的名稱相符。</span><span class="sxs-lookup"><span data-stu-id="1a379-132">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="1a379-133">元件類別的成員定義于 [`@code`][1] 區塊中。</span><span class="sxs-lookup"><span data-stu-id="1a379-133">Members of the component class are defined in an [`@code`][1] block.</span></span> <span data-ttu-id="1a379-134">在 [`@code`][1] 區塊中，會使用事件處理或定義其他元件邏輯的方法來指定元件狀態（屬性、欄位）。</span><span class="sxs-lookup"><span data-stu-id="1a379-134">In the [`@code`][1] block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="1a379-135">允許一個以上的 [`@code`][1] 區塊。</span><span class="sxs-lookup"><span data-stu-id="1a379-135">More than one [`@code`][1] block is permissible.</span></span>

<span data-ttu-id="1a379-136">元件成員可以使用開頭為的 c # 運算式，做為元件轉譯邏輯的一部分 `@` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-136">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="1a379-137">例如，c # 欄位是藉由在功能變數名稱前面加上的方式 `@` 來呈現。</span><span class="sxs-lookup"><span data-stu-id="1a379-137">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="1a379-138">下列範例會評估並呈現：</span><span class="sxs-lookup"><span data-stu-id="1a379-138">The following example evaluates and renders:</span></span>

* <span data-ttu-id="1a379-139">`headingFontStyle`至的 CSS 屬性值 `font-style` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-139">`headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="1a379-140">`headingText`至元素的內容 `<h1>` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-140">`headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@headingFontStyle">@headingText</h1>

@code {
    private string headingFontStyle = "italic";
    private string headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="1a379-141">一開始呈現元件之後，元件會重新產生其轉譯樹狀結構，以回應事件。</span><span class="sxs-lookup"><span data-stu-id="1a379-141">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="1a379-142">然後比較新的轉譯樹狀結構與上一個，並將任何修改套用至瀏覽器的檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="1a379-142"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="1a379-143">元件是一般的 c # 類別，可以放在專案內的任何位置。</span><span class="sxs-lookup"><span data-stu-id="1a379-143">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="1a379-144">產生網頁的元件通常位於資料夾中 `Pages` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-144">Components that produce webpages usually reside in the `Pages` folder.</span></span> <span data-ttu-id="1a379-145">非頁面元件通常會放在資料夾中， `Shared` 或加入至專案的自訂資料夾中。</span><span class="sxs-lookup"><span data-stu-id="1a379-145">Non-page components are frequently placed in the `Shared` folder or a custom folder added to the project.</span></span>

### <a name="namespaces"></a><span data-ttu-id="1a379-146">命名空間</span><span class="sxs-lookup"><span data-stu-id="1a379-146">Namespaces</span></span>

<span data-ttu-id="1a379-147">一般而言，元件的命名空間是從應用程式的根命名空間和元件在應用程式內的位置（資料夾）衍生而來。</span><span class="sxs-lookup"><span data-stu-id="1a379-147">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="1a379-148">如果應用程式的根命名空間為 `BlazorApp` ，且 `Counter` 元件位於 `Pages` 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="1a379-148">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the `Pages` folder:</span></span>

* <span data-ttu-id="1a379-149">`Counter`元件的命名空間是 `BlazorApp.Pages` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-149">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="1a379-150">元件的完整類型名稱為 `BlazorApp.Pages.Counter` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-150">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="1a379-151">若為保存元件的自訂資料夾，請將指示詞新增 [`@using`][2] 至父元件或應用程式的檔案 `_Imports.razor` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-151">For custom folders that hold components, add a [`@using`][2] directive to the parent component or to the app's `_Imports.razor` file.</span></span> <span data-ttu-id="1a379-152">下列範例會讓資料夾中的元件 `Components` 可供使用：</span><span class="sxs-lookup"><span data-stu-id="1a379-152">The following example makes components in the `Components` folder available:</span></span>

```razor
@using BlazorApp.Components
```

<span data-ttu-id="1a379-153">元件也可以使用其完整名稱來參考，而不需要指示詞 [`@using`][2] ：</span><span class="sxs-lookup"><span data-stu-id="1a379-153">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`][2] directive:</span></span>

```razor
<BlazorSample.Components.MyComponent />
```

<span data-ttu-id="1a379-154">以撰寫之元件的命名空間 Razor 是根據（依優先順序排列）：</span><span class="sxs-lookup"><span data-stu-id="1a379-154">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="1a379-155">[`@namespace`][8]檔案 Razor （）標記中的指定 `.razor` （） `@namespace BlazorSample.MyNamespace` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-155">[`@namespace`][8] designation in Razor file (`.razor`) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="1a379-156">專案 `RootNamespace` 在專案檔中的（ `<RootNamespace>BlazorSample</RootNamespace>` ）。</span><span class="sxs-lookup"><span data-stu-id="1a379-156">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="1a379-157">從專案檔的檔案名（）取得的專案名稱 `.csproj` ，以及從專案根目錄到元件的路徑。</span><span class="sxs-lookup"><span data-stu-id="1a379-157">The project name, taken from the project file's file name (`.csproj`), and the path from the project root to the component.</span></span> <span data-ttu-id="1a379-158">例如，架構會將 `{PROJECT ROOT}/Pages/Index.razor` （ `BlazorSample.csproj` ）解析為命名空間 `BlazorSample.Pages` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-158">For example, the framework resolves `{PROJECT ROOT}/Pages/Index.razor` (`BlazorSample.csproj`) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="1a379-159">元件遵循 c # 名稱系結規則。</span><span class="sxs-lookup"><span data-stu-id="1a379-159">Components follow C# name binding rules.</span></span> <span data-ttu-id="1a379-160">針對 `Index` 此範例中的元件，範圍內的元件都是元件：</span><span class="sxs-lookup"><span data-stu-id="1a379-160">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="1a379-161">在相同的資料夾中， `Pages` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-161">In the same folder, `Pages`.</span></span>
  * <span data-ttu-id="1a379-162">專案根目錄中未明確指定不同命名空間的元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-162">The components in the project's root that don't explicitly specify a different namespace.</span></span>

> [!NOTE]
> <span data-ttu-id="1a379-163">`global::`不支援該限定性。</span><span class="sxs-lookup"><span data-stu-id="1a379-163">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="1a379-164">不支援使用具有別名的語句來匯入元件 [`using`](/dotnet/csharp/language-reference/keywords/using-statement) （例如 `@using Foo = Bar` ）。</span><span class="sxs-lookup"><span data-stu-id="1a379-164">Importing components with aliased [`using`](/dotnet/csharp/language-reference/keywords/using-statement) statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="1a379-165">不支援部分限定的名稱。</span><span class="sxs-lookup"><span data-stu-id="1a379-165">Partially qualified names aren't supported.</span></span> <span data-ttu-id="1a379-166">例如， `@using BlazorSample` 不支援新增和參考 `NavMenu` 元件（ `NavMenu.razor` ） `<Shared.NavMenu></Shared.NavMenu>` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-166">For example, adding `@using BlazorSample` and referencing the `NavMenu` component (`NavMenu.razor`) with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

### <a name="partial-class-support"></a><span data-ttu-id="1a379-167">部分類別支援</span><span class="sxs-lookup"><span data-stu-id="1a379-167">Partial class support</span></span>

Razor<span data-ttu-id="1a379-168">元件是以部分類別的形式產生。</span><span class="sxs-lookup"><span data-stu-id="1a379-168"> components are generated as partial classes.</span></span> Razor<span data-ttu-id="1a379-169">元件是使用下列其中一種方法來撰寫的：</span><span class="sxs-lookup"><span data-stu-id="1a379-169"> components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="1a379-170">C # 程式碼定義于 [`@code`][1] 區塊中，並在單一檔案中使用 HTML 標籤和程式 Razor 代碼。</span><span class="sxs-lookup"><span data-stu-id="1a379-170">C# code is defined in an [`@code`][1] block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="1a379-171">範本會 Razor 使用這種方法來定義其元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-171"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="1a379-172">C # 程式碼會放在定義為部分類別的程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="1a379-172">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="1a379-173">下列範例顯示在 `Counter` [`@code`][1] 從範本產生的應用程式中具有區塊的預設元件 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="1a379-173">The following example shows the default `Counter` component with an [`@code`][1] block in an app generated from a Blazor template.</span></span> <span data-ttu-id="1a379-174">HTML 標籤、程式 Razor 代碼和 c # 程式碼位於相同的檔案中：</span><span class="sxs-lookup"><span data-stu-id="1a379-174">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="1a379-175">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-175">`Pages/Counter.razor`:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="1a379-176">您 `Counter` 也可以使用具有部分類別的程式碼後置檔案來建立元件：</span><span class="sxs-lookup"><span data-stu-id="1a379-176">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="1a379-177">`Pages/Counter.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-177">`Pages/Counter.razor`:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="1a379-178">`Counter.razor.cs`:</span><span class="sxs-lookup"><span data-stu-id="1a379-178">`Counter.razor.cs`:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

<span data-ttu-id="1a379-179">視需要將任何必要的命名空間新增至部分類別檔案。</span><span class="sxs-lookup"><span data-stu-id="1a379-179">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="1a379-180">元件所使用的一般命名空間 Razor 包括：</span><span class="sxs-lookup"><span data-stu-id="1a379-180">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

### <a name="specify-a-base-class"></a><span data-ttu-id="1a379-181">指定基類</span><span class="sxs-lookup"><span data-stu-id="1a379-181">Specify a base class</span></span>

<span data-ttu-id="1a379-182">指示詞 [`@inherits`][6] 可以用來指定元件的基類。</span><span class="sxs-lookup"><span data-stu-id="1a379-182">The [`@inherits`][6] directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="1a379-183">下列範例會顯示元件如何繼承基類， `BlazorRocksBase` 以提供元件的屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="1a379-183">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="1a379-184">基類應該衍生自 <xref:Microsoft.AspNetCore.Components.ComponentBase> 。</span><span class="sxs-lookup"><span data-stu-id="1a379-184">The base class should derive from <xref:Microsoft.AspNetCore.Components.ComponentBase>.</span></span>

<span data-ttu-id="1a379-185">`Pages/BlazorRocks.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-185">`Pages/BlazorRocks.razor`:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="1a379-186">`BlazorRocksBase.cs`:</span><span class="sxs-lookup"><span data-stu-id="1a379-186">`BlazorRocksBase.cs`:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

### <a name="use-components"></a><span data-ttu-id="1a379-187">使用元件</span><span class="sxs-lookup"><span data-stu-id="1a379-187">Use components</span></span>

<span data-ttu-id="1a379-188">元件可以包含其他元件，方法是使用 HTML 專案語法來宣告它們。</span><span class="sxs-lookup"><span data-stu-id="1a379-188">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="1a379-189">使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。</span><span class="sxs-lookup"><span data-stu-id="1a379-189">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="1a379-190">中的下列標記會 `Pages/Index.razor` 呈現 `HeadingComponent` 實例：</span><span class="sxs-lookup"><span data-stu-id="1a379-190">The following markup in `Pages/Index.razor` renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="1a379-191">`Components/HeadingComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-191">`Components/HeadingComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/HeadingComponent.razor)]

<span data-ttu-id="1a379-192">如果元件包含的 HTML 專案具有大寫的第一個字母，但不符合元件名稱，則會發出警告，指出該元素有未預期的名稱。</span><span class="sxs-lookup"><span data-stu-id="1a379-192">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="1a379-193">[`@using`][2]為元件的命名空間加入指示詞可讓元件可用，這會解析警告。</span><span class="sxs-lookup"><span data-stu-id="1a379-193">Adding an [`@using`][2] directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="parameters"></a><span data-ttu-id="1a379-194">參數</span><span class="sxs-lookup"><span data-stu-id="1a379-194">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="1a379-195">路由參數</span><span class="sxs-lookup"><span data-stu-id="1a379-195">Route parameters</span></span>

<span data-ttu-id="1a379-196">元件可以從指示詞中提供的路由範本接收路由參數 [`@page`][9] 。</span><span class="sxs-lookup"><span data-stu-id="1a379-196">Components can receive route parameters from the route template provided in the [`@page`][9] directive.</span></span> <span data-ttu-id="1a379-197">路由器會使用路由參數來填入對應的元件參數。</span><span class="sxs-lookup"><span data-stu-id="1a379-197">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="1a379-198">`Pages/RouteParameter.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-198">`Pages/RouteParameter.razor`:</span></span>

[!code-razor[](index/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="1a379-199">不支援選擇性參數，因此 [`@page`][9] 在上述範例中會套用兩個指示詞。</span><span class="sxs-lookup"><span data-stu-id="1a379-199">Optional parameters aren't supported, so two [`@page`][9] directives are applied in the preceding example.</span></span> <span data-ttu-id="1a379-200">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-200">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="1a379-201">第二個指示詞會 [`@page`][9] 接收 `{text}` 路由參數，並將值指派給 `Text` 屬性。</span><span class="sxs-lookup"><span data-stu-id="1a379-201">The second [`@page`][9] directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="1a379-202">元件（）中不支援*Catch-all*參數語法（ `*` / `**` ），它會跨多**not**個資料夾界限來捕捉路徑 Razor `.razor` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-202">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (`.razor`).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="1a379-203">元件參數</span><span class="sxs-lookup"><span data-stu-id="1a379-203">Component parameters</span></span>

<span data-ttu-id="1a379-204">元件可以具有*元件參數*，其定義方式是在元件類別上使用具有屬性的公用屬性 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="1a379-204">Components can have *component parameters*, which are defined using public properties on the component class with the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute.</span></span> <span data-ttu-id="1a379-205">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="1a379-205">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="1a379-206">`Components/ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-206">`Components/ChildComponent.razor`:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="1a379-207">在範例應用程式的下列範例中，會 `ParentComponent` 設定的 `Title` 屬性值 `ChildComponent` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-207">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="1a379-208">`Pages/ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-208">`Pages/ParentComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/ParentComponent.razor?highlight=5-6)]

> [!WARNING]
> <span data-ttu-id="1a379-209">請勿建立可寫入其本身*元件參數*的元件，請改用私用欄位。</span><span class="sxs-lookup"><span data-stu-id="1a379-209">Don't create components that write to their own *component parameters*, use a private field instead.</span></span> <span data-ttu-id="1a379-210">如需詳細資訊，請參閱[不要建立可寫入自己的參數屬性的元件](#dont-create-components-that-write-to-their-own-parameter-properties)一節。</span><span class="sxs-lookup"><span data-stu-id="1a379-210">For more information, see the [Don't create components that write to their own parameter properties](#dont-create-components-that-write-to-their-own-parameter-properties) section.</span></span>

## <a name="child-content"></a><span data-ttu-id="1a379-211">子內容</span><span class="sxs-lookup"><span data-stu-id="1a379-211">Child content</span></span>

<span data-ttu-id="1a379-212">元件可以設定另一個元件的內容。</span><span class="sxs-lookup"><span data-stu-id="1a379-212">Components can set the content of another component.</span></span> <span data-ttu-id="1a379-213">指派元件會在指定接收元件的標記之間提供內容。</span><span class="sxs-lookup"><span data-stu-id="1a379-213">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="1a379-214">在下列範例中， `ChildComponent` 有一個 `ChildContent` 代表的屬性 <xref:Microsoft.AspNetCore.Components.RenderFragment> ，代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="1a379-214">In the following example, the `ChildComponent` has a `ChildContent` property that represents a <xref:Microsoft.AspNetCore.Components.RenderFragment>, which represents a segment of UI to render.</span></span> <span data-ttu-id="1a379-215">的值位於 `ChildContent` 元件的標記中，應在其中呈現內容。</span><span class="sxs-lookup"><span data-stu-id="1a379-215">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="1a379-216">的值 `ChildContent` 會從父元件接收，並在啟動載入面板的內轉譯 `panel-body` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-216">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="1a379-217">`Components/ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-217">`Components/ChildComponent.razor`:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="1a379-218">接收內容的屬性 <xref:Microsoft.AspNetCore.Components.RenderFragment> 必須依照慣例命名 `ChildContent` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-218">The property receiving the <xref:Microsoft.AspNetCore.Components.RenderFragment> content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="1a379-219">`ParentComponent`範例應用程式中的會將內容放在標籤內，藉以提供呈現的內容 `ChildComponent` `<ChildComponent>` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-219">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="1a379-220">`Pages/ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-220">`Pages/ParentComponent.razor`:</span></span>

[!code-razor[](index/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="1a379-221">屬性展開和任意參數</span><span class="sxs-lookup"><span data-stu-id="1a379-221">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="1a379-222">除了元件的宣告參數之外，元件還可以捕捉和轉譯其他屬性。</span><span class="sxs-lookup"><span data-stu-id="1a379-222">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="1a379-223">您可以在字典中捕捉其他屬性，然後在使用指示詞轉譯元件時， *splatted*至元素 [`@attributes`][3] Razor 。</span><span class="sxs-lookup"><span data-stu-id="1a379-223">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`][3] Razor directive.</span></span> <span data-ttu-id="1a379-224">當定義的元件會產生支援各種自訂的標記專案時，這個案例就很有用。</span><span class="sxs-lookup"><span data-stu-id="1a379-224">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="1a379-225">例如，針對支援許多參數的，分別定義屬性可能會很繁瑣 `<input>` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-225">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="1a379-226">在下列範例中，第一個 `<input>` 元素（ `id="useIndividualParams"` ）會使用個別的元件參數，而第二個 `<input>` 元素（ `id="useAttributesDict"` ）則使用屬性展開：</span><span class="sxs-lookup"><span data-stu-id="1a379-226">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="1a379-227">參數的類型必須 `IEnumerable<KeyValuePair<string, object>>` 使用字串索引鍵來執行。</span><span class="sxs-lookup"><span data-stu-id="1a379-227">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="1a379-228">`IReadOnlyDictionary<string, object>`在此案例中，使用也是一個選項。</span><span class="sxs-lookup"><span data-stu-id="1a379-228">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="1a379-229">`<input>`使用這兩種方法的轉譯元素都相同：</span><span class="sxs-lookup"><span data-stu-id="1a379-229">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="1a379-230">若要接受任意屬性，請使用屬性設定為的屬性來定義元件參數 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> `true` ：</span><span class="sxs-lookup"><span data-stu-id="1a379-230">To accept arbitrary attributes, define a component parameter using the [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) attribute with the <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="1a379-231"><xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues>上的屬性 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 允許參數比對與任何其他參數不相符的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="1a379-231">The <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> property on [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="1a379-232">元件只能定義具有的單一參數 <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> 。</span><span class="sxs-lookup"><span data-stu-id="1a379-232">A component can only define a single parameter with <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues>.</span></span> <span data-ttu-id="1a379-233">搭配使用的屬性類型 <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> 必須可從 `Dictionary<string, object>` 使用字串索引鍵來指派。</span><span class="sxs-lookup"><span data-stu-id="1a379-233">The property type used with <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="1a379-234">`IEnumerable<KeyValuePair<string, object>>`或 `IReadOnlyDictionary<string, object>` 也是此案例中的選項。</span><span class="sxs-lookup"><span data-stu-id="1a379-234">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="1a379-235">[`@attributes`][3]相對於元素屬性位置的位置很重要。</span><span class="sxs-lookup"><span data-stu-id="1a379-235">The position of [`@attributes`][3] relative to the position of element attributes is important.</span></span> <span data-ttu-id="1a379-236">當在專案 [`@attributes`][3] 上 splatted 時，會從右至左（最後一個）處理屬性。</span><span class="sxs-lookup"><span data-stu-id="1a379-236">When [`@attributes`][3] are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="1a379-237">請考慮使用元件的下列元件範例 `Child` ：</span><span class="sxs-lookup"><span data-stu-id="1a379-237">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="1a379-238">`ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-238">`ParentComponent.razor`:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="1a379-239">`ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-239">`ChildComponent.razor`:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="1a379-240">`Child`元件的 `extra` 屬性會設定為的右邊 [`@attributes`][3] 。</span><span class="sxs-lookup"><span data-stu-id="1a379-240">The `Child` component's `extra` attribute is set to the right of [`@attributes`][3].</span></span> <span data-ttu-id="1a379-241">`Parent` `<div>` `extra="5"` 當您透過其他屬性傳遞時，元件的會包含，因為屬性是由右至左（最後一個）來處理：</span><span class="sxs-lookup"><span data-stu-id="1a379-241">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="1a379-242">在下列範例中，和的順序 `extra` [`@attributes`][3] 會在 `Child` 元件的中反轉 `<div>` ：</span><span class="sxs-lookup"><span data-stu-id="1a379-242">In the following example, the order of `extra` and [`@attributes`][3] is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="1a379-243">`ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-243">`ParentComponent.razor`:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="1a379-244">`ChildComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="1a379-244">`ChildComponent.razor`:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="1a379-245">在元件中轉譯的會在 `<div>` `Parent` `extra="10"` 透過其他屬性傳遞時包含：</span><span class="sxs-lookup"><span data-stu-id="1a379-245">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="1a379-246">捕獲元件的參考</span><span class="sxs-lookup"><span data-stu-id="1a379-246">Capture references to components</span></span>

<span data-ttu-id="1a379-247">元件參考提供參考元件實例的方法，讓您可以對該實例發出命令，例如 `Show` 或 `Reset` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-247">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="1a379-248">若要捕捉元件參考：</span><span class="sxs-lookup"><span data-stu-id="1a379-248">To capture a component reference:</span></span>

* <span data-ttu-id="1a379-249">將 [`@ref`][4] 屬性加入至子元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-249">Add an [`@ref`][4] attribute to the child component.</span></span>
* <span data-ttu-id="1a379-250">定義與子元件類型相同的欄位。</span><span class="sxs-lookup"><span data-stu-id="1a379-250">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="1a379-251">當元件呈現時， `loginDialog` 欄位會填入 `MyLoginDialog` 子元件實例。</span><span class="sxs-lookup"><span data-stu-id="1a379-251">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="1a379-252">接著，您可以在元件實例上叫用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="1a379-252">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1a379-253">只有在轉譯 `loginDialog` 元件之後才會填入變數，而且其輸出會包含 `MyLoginDialog` 元素。</span><span class="sxs-lookup"><span data-stu-id="1a379-253">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="1a379-254">在轉譯此元件之前，沒有任何可參考的內容。</span><span class="sxs-lookup"><span data-stu-id="1a379-254">Until the component is rendered, there's nothing to reference.</span></span>
>
> <span data-ttu-id="1a379-255">若要在元件完成呈現之後操作元件參考，請使用[ `OnAfterRenderAsync` 或 `OnAfterRender` 方法](xref:blazor/components/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="1a379-255">To manipulate components references after the component has finished rendering, use the [`OnAfterRenderAsync` or `OnAfterRender` methods](xref:blazor/components/lifecycle#after-component-render).</span></span>
>
> <span data-ttu-id="1a379-256">若要將參考變數與事件處理常式搭配使用，請使用 lambda 運算式，或在[ `OnAfterRenderAsync` 或 `OnAfterRender` 方法](xref:blazor/components/lifecycle#after-component-render)中指派事件處理常式委派。</span><span class="sxs-lookup"><span data-stu-id="1a379-256">To use a reference variable with an event handler, use a lambda expression or assign the event handler delegate in the [`OnAfterRenderAsync` or `OnAfterRender` methods](xref:blazor/components/lifecycle#after-component-render).</span></span> <span data-ttu-id="1a379-257">這可確保在指派事件處理常式之前，會先指派參考變數。</span><span class="sxs-lookup"><span data-stu-id="1a379-257">This ensures that the reference variable is assigned before the event handler is assigned.</span></span>
>
> ```razor
> <button type="button" 
>     @onclick="@(() => loginDialog.DoSomething())">Do Something</button>
>
> <MyLoginDialog @ref="loginDialog" ... />
>
> @code {
>     private MyLoginDialog loginDialog;
> }
> ```

<span data-ttu-id="1a379-258">若要參考迴圈中的元件，請參閱[捕捉多個類似子元件的參考（dotnet/aspnetcore #13358）](https://github.com/dotnet/aspnetcore/issues/13358)。</span><span class="sxs-lookup"><span data-stu-id="1a379-258">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="1a379-259">雖然捕捉元件參考使用類似的語法來[捕捉元素參考](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)，但它並不是 JavaScript interop 功能。</span><span class="sxs-lookup"><span data-stu-id="1a379-259">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="1a379-260">元件參考不會傳遞至 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="1a379-260">Component references aren't passed to JavaScript code.</span></span> <span data-ttu-id="1a379-261">元件參考只在 .NET 程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="1a379-261">Component references are only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="1a379-262">請勿**使用元件**參考來改變子元件的狀態。</span><span class="sxs-lookup"><span data-stu-id="1a379-262">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="1a379-263">請改用一般宣告式參數，將資料傳遞至子元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-263">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="1a379-264">使用一般宣告式參數會導致子元件自動 rerender 正確的時間。</span><span class="sxs-lookup"><span data-stu-id="1a379-264">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="synchronization-context"></a><span data-ttu-id="1a379-265">同步處理內容</span><span class="sxs-lookup"><span data-stu-id="1a379-265">Synchronization context</span></span>

Blazor<span data-ttu-id="1a379-266">會使用同步處理內容（ <xref:System.Threading.SynchronizationContext> ）來強制執行單一邏輯執行緒。</span><span class="sxs-lookup"><span data-stu-id="1a379-266"> uses a synchronization context (<xref:System.Threading.SynchronizationContext>) to enforce a single logical thread of execution.</span></span> <span data-ttu-id="1a379-267">元件的[生命週期方法](xref:blazor/components/lifecycle)和所引發的任何事件回呼 Blazor 都會在同步處理內容上執行。</span><span class="sxs-lookup"><span data-stu-id="1a379-267">A component's [lifecycle methods](xref:blazor/components/lifecycle) and any event callbacks that are raised by Blazor are executed on the synchronization context.</span></span>

Blazor Server<span data-ttu-id="1a379-268">的同步處理內容會嘗試模擬單一執行緒環境，讓它與瀏覽器中的 WebAssembly 模型（單一執行緒）緊密相符。</span><span class="sxs-lookup"><span data-stu-id="1a379-268">'s synchronization context attempts to emulate a single-threaded environment so that it closely matches the WebAssembly model in the browser, which is single threaded.</span></span> <span data-ttu-id="1a379-269">在任何指定的時間點，只會在一個執行緒上執行工作，以提供單一邏輯執行緒的印象。</span><span class="sxs-lookup"><span data-stu-id="1a379-269">At any given point in time, work is performed on exactly one thread, giving the impression of a single logical thread.</span></span> <span data-ttu-id="1a379-270">不會同時執行兩個作業。</span><span class="sxs-lookup"><span data-stu-id="1a379-270">No two operations execute concurrently.</span></span>

### <a name="avoid-thread-blocking-calls"></a><span data-ttu-id="1a379-271">避免執行緒封鎖呼叫</span><span class="sxs-lookup"><span data-stu-id="1a379-271">Avoid thread-blocking calls</span></span>

<span data-ttu-id="1a379-272">一般而言，請勿呼叫下列方法。</span><span class="sxs-lookup"><span data-stu-id="1a379-272">Generally, don't call the following methods.</span></span> <span data-ttu-id="1a379-273">下列方法會封鎖執行緒，因此會封鎖應用程式繼續工作，直到基礎 <xref:System.Threading.Tasks.Task> 完成為止：</span><span class="sxs-lookup"><span data-stu-id="1a379-273">The following methods block the thread and thus block the app from resuming work until the underlying <xref:System.Threading.Tasks.Task> is complete:</span></span>

* <xref:System.Threading.Tasks.Task%601.Result%2A>
* <xref:System.Threading.Tasks.Task.Wait%2A>
* <xref:System.Threading.Tasks.Task.WaitAny%2A>
* <xref:System.Threading.Tasks.Task.WaitAll%2A>
* <xref:System.Threading.Thread.Sleep%2A>
* <xref:System.Runtime.CompilerServices.TaskAwaiter.GetResult%2A>

### <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="1a379-274">在外部叫用元件方法來更新狀態</span><span class="sxs-lookup"><span data-stu-id="1a379-274">Invoke component methods externally to update state</span></span>

<span data-ttu-id="1a379-275">在事件中，必須根據外來事件（例如計時器或其他通知）來更新元件，請使用 `InvokeAsync` 方法，這會分派至 Blazor 的同步處理內容。</span><span class="sxs-lookup"><span data-stu-id="1a379-275">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which dispatches to Blazor's synchronization context.</span></span> <span data-ttu-id="1a379-276">例如，假設有一個通知程式*服務*可通知任何處于已更新狀態的「接聽」元件：</span><span class="sxs-lookup"><span data-stu-id="1a379-276">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="1a379-277">將註冊 `NotifierService` 為 singletion：</span><span class="sxs-lookup"><span data-stu-id="1a379-277">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="1a379-278">在中，于中 Blazor WebAssembly 註冊服務 `Program.Main` ：</span><span class="sxs-lookup"><span data-stu-id="1a379-278">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="1a379-279">在中，于中 Blazor Server 註冊服務 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1a379-279">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddScoped<NotifierService>();
  ```

<span data-ttu-id="1a379-280">使用 `NotifierService` 來更新元件：</span><span class="sxs-lookup"><span data-stu-id="1a379-280">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="1a379-281">在上述範例中，會在 `NotifierService` 的同步處理內容之外叫用元件的 `OnNotify` 方法 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="1a379-281">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's synchronization context.</span></span> <span data-ttu-id="1a379-282">`InvokeAsync`用來切換至正確的內容，並將轉譯排入佇列。</span><span class="sxs-lookup"><span data-stu-id="1a379-282">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="1a379-283">使用 \@ 金鑰來控制元素和元件的保留</span><span class="sxs-lookup"><span data-stu-id="1a379-283">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="1a379-284">當轉譯專案或元件清單，以及後續變更的專案或元件時， Blazor 的比較演算法必須決定哪些先前的專案或元件可以保留，以及模型物件應如何對應至這些專案。</span><span class="sxs-lookup"><span data-stu-id="1a379-284">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="1a379-285">一般來說，此程式是自動的，可以忽略，但在某些情況下，您可能會想要控制進程。</span><span class="sxs-lookup"><span data-stu-id="1a379-285">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="1a379-286">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="1a379-286">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="1a379-287">集合的內容 `People` 可能會隨著插入、刪除或重新排序的專案而變更。</span><span class="sxs-lookup"><span data-stu-id="1a379-287">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="1a379-288">當元件 rerenders 時， `<DetailsEditor>` 元件可能會變更以接收不同的 `Details` 參數值。</span><span class="sxs-lookup"><span data-stu-id="1a379-288">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="1a379-289">這可能會導致比預期更複雜的 rerendering。</span><span class="sxs-lookup"><span data-stu-id="1a379-289">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="1a379-290">在某些情況下，rerendering 可能會導致可見的行為差異，例如失去元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="1a379-290">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="1a379-291">您可以使用指示詞屬性來控制對應進程 [`@key`][5] 。</span><span class="sxs-lookup"><span data-stu-id="1a379-291">The mapping process can be controlled with the [`@key`][5] directive attribute.</span></span> <span data-ttu-id="1a379-292">[`@key`][5]導致比較演算法根據索引鍵的值，保證保留元素或元件：</span><span class="sxs-lookup"><span data-stu-id="1a379-292">[`@key`][5] causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="1a379-293">當 `People` 集合變更時，比較演算法會保留 `<DetailsEditor>` 實例和實例之間的關聯 `person` ：</span><span class="sxs-lookup"><span data-stu-id="1a379-293">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="1a379-294">如果 `Person` 從清單中刪除 `People` ，則只 `<DetailsEditor>` 會從 UI 移除對應的實例。</span><span class="sxs-lookup"><span data-stu-id="1a379-294">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="1a379-295">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="1a379-295">Other instances are left unchanged.</span></span>
* <span data-ttu-id="1a379-296">如果在 `Person` 清單中的某個位置插入，則會在 `<DetailsEditor>` 對應的位置插入一個新的實例。</span><span class="sxs-lookup"><span data-stu-id="1a379-296">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="1a379-297">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="1a379-297">Other instances are left unchanged.</span></span>
* <span data-ttu-id="1a379-298">如果 `Person` 重新排序專案，則 `<DetailsEditor>` 會保留對應的實例，並在 UI 中重新排序。</span><span class="sxs-lookup"><span data-stu-id="1a379-298">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="1a379-299">在某些情況下，使用可將 [`@key`][5] rerendering 的複雜性降到最低，並避免 DOM 的具狀態部分可能發生的問題，例如焦點位置。</span><span class="sxs-lookup"><span data-stu-id="1a379-299">In some scenarios, use of [`@key`][5] minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1a379-300">索引鍵在每個容器元素或元件的本機。</span><span class="sxs-lookup"><span data-stu-id="1a379-300">Keys are local to each container element or component.</span></span> <span data-ttu-id="1a379-301">金鑰不會在檔之間進行全域比較。</span><span class="sxs-lookup"><span data-stu-id="1a379-301">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="1a379-302">使用金鑰的時機 \@</span><span class="sxs-lookup"><span data-stu-id="1a379-302">When to use \@key</span></span>

<span data-ttu-id="1a379-303">一般而言， [`@key`][5] 只要轉譯清單（例如，在[foreach](/dotnet/csharp/language-reference/keywords/foreach-in)區塊中），而且有適合的值來定義時，就有合理的使用方式 [`@key`][5] 。</span><span class="sxs-lookup"><span data-stu-id="1a379-303">Typically, it makes sense to use [`@key`][5] whenever a list is rendered (for example, in a [foreach](/dotnet/csharp/language-reference/keywords/foreach-in) block) and a suitable value exists to define the [`@key`][5].</span></span>

<span data-ttu-id="1a379-304">當物件變更時，您也可以使用 [`@key`][5] 來防止 Blazor 保留元素或元件子樹：</span><span class="sxs-lookup"><span data-stu-id="1a379-304">You can also use [`@key`][5] to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="1a379-305">如果 `@currentPerson` 變更，attribute 指示詞會 [`@key`][5] 強制 Blazor 捨棄整個及其下階， `<div>` 並使用新的元素和元件重建 UI 內的子樹。</span><span class="sxs-lookup"><span data-stu-id="1a379-305">If `@currentPerson` changes, the [`@key`][5] attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="1a379-306">如果您需要保證變更時不會保留任何 UI 狀態，這會很有用 `@currentPerson` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-306">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="1a379-307">不使用金鑰的時機 \@</span><span class="sxs-lookup"><span data-stu-id="1a379-307">When not to use \@key</span></span>

<span data-ttu-id="1a379-308">與比較時，會產生效能成本 [`@key`][5] 。</span><span class="sxs-lookup"><span data-stu-id="1a379-308">There's a performance cost when diffing with [`@key`][5].</span></span> <span data-ttu-id="1a379-309">效能成本並不大，但只會指定 [`@key`][5] 控制元素或元件保留規則是否能讓應用程式受益。</span><span class="sxs-lookup"><span data-stu-id="1a379-309">The performance cost isn't large, but only specify [`@key`][5] if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="1a379-310">即使 [`@key`][5] 未使用，也會盡可能 Blazor 保留子項目和元件實例。</span><span class="sxs-lookup"><span data-stu-id="1a379-310">Even if [`@key`][5] isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="1a379-311">使用的唯一優點 [`@key`][5] 是控制模型實例*如何*對應至保留的元件實例，而不是用來選取對應的比較演算法。</span><span class="sxs-lookup"><span data-stu-id="1a379-311">The only advantage to using [`@key`][5] is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="1a379-312">要用於金鑰的值 \@</span><span class="sxs-lookup"><span data-stu-id="1a379-312">What values to use for \@key</span></span>

<span data-ttu-id="1a379-313">一般來說，提供下列其中一種類型的值是合理的 [`@key`][5] ：</span><span class="sxs-lookup"><span data-stu-id="1a379-313">Generally, it makes sense to supply one of the following kinds of value for [`@key`][5]:</span></span>

* <span data-ttu-id="1a379-314">模型物件實例（例如， `Person` 如先前範例所示的實例）。</span><span class="sxs-lookup"><span data-stu-id="1a379-314">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="1a379-315">這可確保根據物件參考的相等性進行保留。</span><span class="sxs-lookup"><span data-stu-id="1a379-315">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="1a379-316">唯一識別碼（例如，、或類型的主要索引 `int` 鍵值 `string` `Guid` ）。</span><span class="sxs-lookup"><span data-stu-id="1a379-316">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="1a379-317">請確定用於的值 [`@key`][5] 不會造成衝突。</span><span class="sxs-lookup"><span data-stu-id="1a379-317">Ensure that values used for [`@key`][5] don't clash.</span></span> <span data-ttu-id="1a379-318">如果在相同的父元素中偵測到衝突值，則會擲回例外狀況， Blazor 因為它無法以決定性的方式將舊專案或元件對應到新的專案或元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-318">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="1a379-319">只使用不同的值，例如物件實例或主鍵值。</span><span class="sxs-lookup"><span data-stu-id="1a379-319">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="dont-create-components-that-write-to-their-own-parameter-properties"></a><span data-ttu-id="1a379-320">不要建立會寫入自己的參數屬性的元件</span><span class="sxs-lookup"><span data-stu-id="1a379-320">Don't create components that write to their own parameter properties</span></span>

<span data-ttu-id="1a379-321">在下列情況下，會覆寫參數：</span><span class="sxs-lookup"><span data-stu-id="1a379-321">Parameters are overwritten under the following conditions:</span></span>

* <span data-ttu-id="1a379-322">子元件的內容會以呈現 <xref:Microsoft.AspNetCore.Components.RenderFragment> 。</span><span class="sxs-lookup"><span data-stu-id="1a379-322">A child component's content is rendered with a <xref:Microsoft.AspNetCore.Components.RenderFragment>.</span></span>
* <span data-ttu-id="1a379-323"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>在父元件中呼叫。</span><span class="sxs-lookup"><span data-stu-id="1a379-323"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called in the parent component.</span></span>

<span data-ttu-id="1a379-324">參數會重設，因為呼叫時會 rerenders 父元件 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> ，並將新的參數值提供給子元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-324">Parameters are reset because the parent component rerenders when <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called and new parameter values are supplied to the child component.</span></span>

<span data-ttu-id="1a379-325">請考慮下列 `Expander` 元件：</span><span class="sxs-lookup"><span data-stu-id="1a379-325">Consider the following `Expander` component that:</span></span>

* <span data-ttu-id="1a379-326">呈現子內容。</span><span class="sxs-lookup"><span data-stu-id="1a379-326">Renders child content.</span></span>
* <span data-ttu-id="1a379-327">使用元件參數來顯示子內容的切換。</span><span class="sxs-lookup"><span data-stu-id="1a379-327">Toggles showing child content with a component parameter.</span></span>

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @Expanded)

    @if (Expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void Toggle()
    {
        Expanded = !Expanded;
    }
}
```

<span data-ttu-id="1a379-328">`Expander`元件會新增至可能呼叫的父元件 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> ：</span><span class="sxs-lookup"><span data-stu-id="1a379-328">The `Expander` component is added to a parent component that may call <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>:</span></span>

```razor
<Expander Expanded="true">
    <h1>Hello, world!</h1>
</Expander>

<Expander Expanded="true" />

<button @onclick="@(() => StateHasChanged())">
    Call StateHasChanged
</button>
```

<span data-ttu-id="1a379-329">一開始， `Expander` 元件會在其屬性切換時獨立行為 `Expanded` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-329">Initially, the `Expander` components behave independently when their `Expanded` properties are toggled.</span></span> <span data-ttu-id="1a379-330">子元件會如預期般維護其狀態。</span><span class="sxs-lookup"><span data-stu-id="1a379-330">The child components maintain their states as expected.</span></span> <span data-ttu-id="1a379-331"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>在父系中呼叫時， `Expanded` 第一個子元件的參數會重設回其初始值（ `true` ）。</span><span class="sxs-lookup"><span data-stu-id="1a379-331">When <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called in the parent, the `Expanded` parameter of the first child component is reset back to its initial value (`true`).</span></span> <span data-ttu-id="1a379-332">第二個 `Expander` 元件的 `Expanded` 值不會重設，因為第二個元件中不會轉譯任何子內容。</span><span class="sxs-lookup"><span data-stu-id="1a379-332">The second `Expander` component's `Expanded` value isn't reset because no child content is rendered in the second component.</span></span>

<span data-ttu-id="1a379-333">若要維護上述案例中的狀態，請使用元件中的*私用欄位* `Expander` 來維護其切換狀態。</span><span class="sxs-lookup"><span data-stu-id="1a379-333">To maintain state in the preceding scenario, use a *private field* in the `Expander` component to maintain its toggled state.</span></span>

<span data-ttu-id="1a379-334">下列 `Expander` 元件：</span><span class="sxs-lookup"><span data-stu-id="1a379-334">The following `Expander` component:</span></span>

* <span data-ttu-id="1a379-335">接受 `Expanded` 來自父系的元件參數值。</span><span class="sxs-lookup"><span data-stu-id="1a379-335">Accepts the `Expanded` component parameter value from the parent.</span></span>
* <span data-ttu-id="1a379-336">將元件參數值指派給 OnInitialized 事件中的私用*欄位*（ `expanded` ）。 [OnInitialized event](xref:blazor/components/lifecycle#component-initialization-methods)</span><span class="sxs-lookup"><span data-stu-id="1a379-336">Assigns the component parameter value to a *private field* (`expanded`) in the [OnInitialized event](xref:blazor/components/lifecycle#component-initialization-methods).</span></span>
* <span data-ttu-id="1a379-337">會使用私用欄位來維護其內部切換狀態。</span><span class="sxs-lookup"><span data-stu-id="1a379-337">Uses the private field to maintain its internal toggle state.</span></span>

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @expanded)

    @if (expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private bool expanded;

    protected override void OnInitialized()
    {
        expanded = Expanded;
    }

    private void Toggle()
    {
        expanded = !expanded;
    }
}
```

## <a name="apply-an-attribute"></a><span data-ttu-id="1a379-338">套用屬性</span><span class="sxs-lookup"><span data-stu-id="1a379-338">Apply an attribute</span></span>

<span data-ttu-id="1a379-339">屬性可以套用至具有指示詞的 Razor 元件 [`@attribute`][7] 。</span><span class="sxs-lookup"><span data-stu-id="1a379-339">Attributes can be applied to Razor components with the [`@attribute`][7] directive.</span></span> <span data-ttu-id="1a379-340">下列範例會將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至元件類別：</span><span class="sxs-lookup"><span data-stu-id="1a379-340">The following example applies the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="1a379-341">條件式 HTML 元素屬性</span><span class="sxs-lookup"><span data-stu-id="1a379-341">Conditional HTML element attributes</span></span>

<span data-ttu-id="1a379-342">HTML 專案屬性會根據 .NET 值有條件地呈現。</span><span class="sxs-lookup"><span data-stu-id="1a379-342">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="1a379-343">如果值為 `false` 或 `null` ，則不會呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="1a379-343">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="1a379-344">如果值為 `true` ，則會以最小化的方式呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="1a379-344">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="1a379-345">在下列範例中， `IsCompleted` 會判斷 `checked` 是否呈現在專案的標記中：</span><span class="sxs-lookup"><span data-stu-id="1a379-345">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="1a379-346">如果 `IsCompleted` 為 `true` ，則會將核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="1a379-346">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="1a379-347">如果 `IsCompleted` 為 `false` ，則會將核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="1a379-347">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="1a379-348">如需詳細資訊，請參閱 <xref:mvc/views/razor> 。</span><span class="sxs-lookup"><span data-stu-id="1a379-348">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="1a379-349">當 .NET 類型為時，某些 HTML 屬性（例如 [`aria-pressed`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons) ）無法正常運作 `bool` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-349">Some HTML attributes, such as [`aria-pressed`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="1a379-350">在這些情況下，請使用型別， `string` 而不是 `bool` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-350">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="1a379-351">原始 HTML</span><span class="sxs-lookup"><span data-stu-id="1a379-351">Raw HTML</span></span>

<span data-ttu-id="1a379-352">字串通常會使用 DOM 文位元組點來呈現，這表示它們可能包含的任何標記都會被忽略，並視為常值。</span><span class="sxs-lookup"><span data-stu-id="1a379-352">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="1a379-353">若要轉譯原始 HTML，請將 HTML 內容包裝在 `MarkupString` 值中。</span><span class="sxs-lookup"><span data-stu-id="1a379-353">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="1a379-354">此值會剖析為 HTML 或 SVG，並插入 DOM 中。</span><span class="sxs-lookup"><span data-stu-id="1a379-354">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="1a379-355">轉譯從任何未受信任來源所建立的原始 HTML 會有**安全性風險**，應予以避免！</span><span class="sxs-lookup"><span data-stu-id="1a379-355">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="1a379-356">下列範例顯示如何使用 `MarkupString` 類型，將靜態 HTML 內容的區塊新增至元件的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="1a379-356">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="razor-templates"></a>Razor<span data-ttu-id="1a379-357">範本</span><span class="sxs-lookup"><span data-stu-id="1a379-357"> templates</span></span>

<span data-ttu-id="1a379-358">您可以使用範本語法來定義轉譯片段 Razor 。</span><span class="sxs-lookup"><span data-stu-id="1a379-358">Render fragments can be defined using Razor template syntax.</span></span> Razor<span data-ttu-id="1a379-359">範本是定義 UI 程式碼片段並採用下列格式的方式：</span><span class="sxs-lookup"><span data-stu-id="1a379-359"> templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="1a379-360">下列範例說明如何指定 <xref:Microsoft.AspNetCore.Components.RenderFragment> 和 <xref:Microsoft.AspNetCore.Components.RenderFragment%601> 值，並直接在元件中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="1a379-360">The following example illustrates how to specify <xref:Microsoft.AspNetCore.Components.RenderFragment> and <xref:Microsoft.AspNetCore.Components.RenderFragment%601> values and render templates directly in a component.</span></span> <span data-ttu-id="1a379-361">轉譯片段也可以當做引數傳遞至樣板[化元件](xref:blazor/components/templated-components)。</span><span class="sxs-lookup"><span data-stu-id="1a379-361">Render fragments can also be passed as arguments to [templated components](xref:blazor/components/templated-components).</span></span>

```razor
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="1a379-362">上述程式碼的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="1a379-362">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="static-assets"></a><span data-ttu-id="1a379-363">靜態資產</span><span class="sxs-lookup"><span data-stu-id="1a379-363">Static assets</span></span>

Blazor<span data-ttu-id="1a379-364">遵循 ASP.NET Core 應用程式在專案[ `web root (wwwroot)` 資料夾](xref:fundamentals/index#web-root)下放置靜態資產的慣例。</span><span class="sxs-lookup"><span data-stu-id="1a379-364"> follows the convention of ASP.NET Core apps placing static assets under the project's [`web root (wwwroot)` folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="1a379-365">使用基底相對路徑（ `/` ）來參考靜態資產的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="1a379-365">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="1a379-366">在下列範例中， `logo.png` 實際上位於 `{PROJECT ROOT}/wwwroot/images` 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="1a379-366">In the following example, `logo.png` is physically located in the `{PROJECT ROOT}/wwwroot/images` folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

Razor<span data-ttu-id="1a379-367">元件**不**支援波形符-斜線標記法（ `~/` ）。</span><span class="sxs-lookup"><span data-stu-id="1a379-367"> components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="1a379-368">如需設定應用程式基底路徑的詳細資訊，請參閱 <xref:blazor/host-and-deploy/index#app-base-path> 。</span><span class="sxs-lookup"><span data-stu-id="1a379-368">For information on setting an app's base path, see <xref:blazor/host-and-deploy/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="1a379-369">元件中不支援標記協助程式</span><span class="sxs-lookup"><span data-stu-id="1a379-369">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="1a379-370">[`Tag Helpers`](xref:mvc/views/tag-helpers/intro)Razor元件（files）中不支援 `.razor` 。</span><span class="sxs-lookup"><span data-stu-id="1a379-370">[`Tag Helpers`](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (`.razor` files).</span></span> <span data-ttu-id="1a379-371">若要在中提供標籤協助程式的功能 Blazor ，請建立元件，其功能與標記協助程式相同，並改用元件。</span><span class="sxs-lookup"><span data-stu-id="1a379-371">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="1a379-372">可擴充向量圖形（SVG）影像</span><span class="sxs-lookup"><span data-stu-id="1a379-372">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="1a379-373">由於 Blazor 會轉譯 HTML，瀏覽器支援的影像（包括可擴充的向量圖形（SVG）影像（ `.svg` ））可透過標記來支援 `<img>` ：</span><span class="sxs-lookup"><span data-stu-id="1a379-373">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (`.svg`), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="1a379-374">同樣地，樣式表單檔案的 CSS 規則中也支援 SVG 影像（ `.css` ）：</span><span class="sxs-lookup"><span data-stu-id="1a379-374">Similarly, SVG images are supported in the CSS rules of a stylesheet file (`.css`):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="1a379-375">不過，在所有案例中不支援內嵌 SVG 標記。</span><span class="sxs-lookup"><span data-stu-id="1a379-375">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="1a379-376">如果您將 `<svg>` 標記直接放入元件檔案（ `.razor` ），則會支援基本映射轉譯，但尚不支援許多先進的案例。</span><span class="sxs-lookup"><span data-stu-id="1a379-376">If you place an `<svg>` tag directly into a component file (`.razor`), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="1a379-377">例如， `<use>` 目前未遵守標記，而且 [`@bind`][10] 無法與某些 SVG 標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="1a379-377">For example, `<use>` tags aren't currently respected, and [`@bind`][10] can't be used with some SVG tags.</span></span> <span data-ttu-id="1a379-378">如需詳細資訊，請參閱[中的 SVG 支援 Blazor （dotnet/aspnetcore #18271）](https://github.com/dotnet/aspnetcore/issues/18271)。</span><span class="sxs-lookup"><span data-stu-id="1a379-378">For more information, see [SVG support in Blazor (dotnet/aspnetcore #18271)](https://github.com/dotnet/aspnetcore/issues/18271).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a379-379">其他資源</span><span class="sxs-lookup"><span data-stu-id="1a379-379">Additional resources</span></span>

* <span data-ttu-id="1a379-380"><xref:blazor/security/server/threat-mitigation>：包含有關建立 Blazor Server 應用程式的指導方針，必須對抗資源耗盡。</span><span class="sxs-lookup"><span data-stu-id="1a379-380"><xref:blazor/security/server/threat-mitigation>: Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>

<!--Reference links in article-->
[1]: <xref:mvc/views/razor#code>
[2]: <xref:mvc/views/razor#using>
[3]: <xref:mvc/views/razor#attributes>
[4]: <xref:mvc/views/razor#ref>
[5]: <xref:mvc/views/razor#key>
[6]: <xref:mvc/views/razor#inherits>
[7]: <xref:mvc/views/razor#attribute>
[8]: <xref:mvc/views/razor#namespace>
[9]: <xref:mvc/views/razor#page>
[10]: <xref:mvc/views/razor#bind>
