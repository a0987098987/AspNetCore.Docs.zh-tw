---
title: ASP.NET Core Blazor WebAssembly 效能最佳做法
author: pranavkm
description: 增加 ASP.NET Core Blazor WebAssembly 應用程式的效能，並避免常見的效能問題的秘訣。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/08/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/webassembly-performance-best-practices
ms.openlocfilehash: 2b6d4e706856cb28f26c2502feca4f959ca4abac
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243027"
---
# <a name="aspnet-core-blazor-webassembly-performance-best-practices"></a><span data-ttu-id="9859e-103">ASP.NET Core Blazor WebAssembly 效能最佳做法</span><span class="sxs-lookup"><span data-stu-id="9859e-103">ASP.NET Core Blazor WebAssembly performance best practices</span></span>

<span data-ttu-id="9859e-104">依[Pranav Krishnamoorthy](https://github.com/pranavkm)</span><span class="sxs-lookup"><span data-stu-id="9859e-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm)</span></span>

<span data-ttu-id="9859e-105">本文提供 ASP.NET Core Blazor WebAssembly 效能最佳做法的指導方針。</span><span class="sxs-lookup"><span data-stu-id="9859e-105">This article provides guidelines for ASP.NET Core Blazor WebAssembly performance best practices.</span></span>

## <a name="avoid-unnecessary-component-renders"></a><span data-ttu-id="9859e-106">避免不必要的元件呈現</span><span class="sxs-lookup"><span data-stu-id="9859e-106">Avoid unnecessary component renders</span></span>

Blazor<span data-ttu-id="9859e-107">當演算法察覺元件尚未變更時，其比較演算法可避免 rerendering 元件。</span><span class="sxs-lookup"><span data-stu-id="9859e-107">'s diffing algorithm avoids rerendering a component when the algorithm perceives that the component hasn't changed.</span></span> <span data-ttu-id="9859e-108">覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> 以精細控制元件轉譯。</span><span class="sxs-lookup"><span data-stu-id="9859e-108">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> for fine-grained control over component rendering.</span></span>

<span data-ttu-id="9859e-109">如果撰寫的 UI 專用元件永遠不會在初始轉譯之後變更，請將設定 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 為傳回 `false` ：</span><span class="sxs-lookup"><span data-stu-id="9859e-109">If authoring a UI-only component that never changes after the initial render, configure <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> to return `false`:</span></span>

```razor
@code {
    protected override bool ShouldRender() => false;
}
```

<span data-ttu-id="9859e-110">大部分的應用程式都不需要更精細的控制，但 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 也可以用來選擇性地呈現回應 UI 事件的元件。</span><span class="sxs-lookup"><span data-stu-id="9859e-110">Most apps don't require fine-grained control, but <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> can also be used to selectively render a component responding to a UI event.</span></span>

<span data-ttu-id="9859e-111">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="9859e-111">In the following example:</span></span>

* <span data-ttu-id="9859e-112"><xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>會覆寫並設定為欄位的值 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> ，這是一開始 `false` 元件載入時。</span><span class="sxs-lookup"><span data-stu-id="9859e-112"><xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> is overridden and set to the value of the <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> field, which is initially `false` when the component loads.</span></span>
* <span data-ttu-id="9859e-113">選取按鈕時， <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 會設定為 `true` ，這會強制元件 rerender 已更新的 `currentCount` 。</span><span class="sxs-lookup"><span data-stu-id="9859e-113">When the button is selected, <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> is set to `true`, which forces the component to rerender with the updated `currentCount`.</span></span>
* <span data-ttu-id="9859e-114">緊接在 rerendering 之後，會 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 將的值設定為， <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> `false` 以防止進一步 rerendering，直到下一次選取按鈕為止。</span><span class="sxs-lookup"><span data-stu-id="9859e-114">Immediately after rerendering, <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> sets the value of <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> back to `false` to prevent further rerendering until the next time the button is selected.</span></span>

```razor
<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;
    private bool shouldRender;

    protected override bool ShouldRender() => shouldRender;

    protected override void OnAfterRender(bool first)
    {
        shouldRender = false;
    }

    private void IncrementCount()
    {
        currentCount++;
        shouldRender = true;
    }
}
```

<span data-ttu-id="9859e-115">如需詳細資訊，請參閱 <xref:blazor/components/lifecycle#after-component-render>。</span><span class="sxs-lookup"><span data-stu-id="9859e-115">For more information, see <xref:blazor/components/lifecycle#after-component-render>.</span></span>

## <a name="virtualize-re-usable-fragments"></a><span data-ttu-id="9859e-116">虛擬化重複使用的片段</span><span class="sxs-lookup"><span data-stu-id="9859e-116">Virtualize re-usable fragments</span></span>

<span data-ttu-id="9859e-117">元件提供便利的方法來產生可重複使用的程式碼和標記片段。</span><span class="sxs-lookup"><span data-stu-id="9859e-117">Components offer a convenient approach to produce re-usable fragments of code and markup.</span></span> <span data-ttu-id="9859e-118">一般來說，我們建議您撰寫最符合應用程式需求的個別元件。</span><span class="sxs-lookup"><span data-stu-id="9859e-118">In general, we recommend authoring individual components that best align with the app's requirements.</span></span> <span data-ttu-id="9859e-119">有一點要注意的是，每個額外的子元件都會產生呈現父元件所需的總時間。</span><span class="sxs-lookup"><span data-stu-id="9859e-119">One caveat is that each additional child component contributes to the total time it takes to render a parent component.</span></span> <span data-ttu-id="9859e-120">對於大部分的應用程式而言，額外的負荷是可忽略的。</span><span class="sxs-lookup"><span data-stu-id="9859e-120">For most apps, the additional overhead is negligible.</span></span> <span data-ttu-id="9859e-121">產生大量元件的應用程式應該考慮使用策略來減少處理額外負荷，例如限制轉譯的元件數目。</span><span class="sxs-lookup"><span data-stu-id="9859e-121">Apps that produce a large number of components should consider using strategies to reduce processing overhead, such as limiting the number of rendered components.</span></span>

<span data-ttu-id="9859e-122">例如，呈現數百個包含元件之資料列的方格或清單會耗用大量處理器來呈現。</span><span class="sxs-lookup"><span data-stu-id="9859e-122">For example, a grid or list that renders hundreds of rows containing components is processor intensive to render.</span></span> <span data-ttu-id="9859e-123">請考慮將方格或清單配置虛擬化，如此一來，就只會在任何指定的時間轉譯元件的子集。</span><span class="sxs-lookup"><span data-stu-id="9859e-123">Consider virtualizing a grid or list layout so that only a subset of the components is rendered at any given time.</span></span> <span data-ttu-id="9859e-124">如需元件子集轉譯的範例，請參閱範例應用程式中的下列元件[ `Virtualization` （aspnet/samples GitHub 存放庫）](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization)：</span><span class="sxs-lookup"><span data-stu-id="9859e-124">For an example of component subset rendering, see the following components in the [`Virtualization` sample app (aspnet/samples GitHub repository)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization):</span></span>

* <span data-ttu-id="9859e-125">`Virtualize`component （ [`Shared/Virtualize.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs) ）：以 c # 撰寫的元件，它會 <xref:Microsoft.AspNetCore.Components.ComponentBase> 根據使用者的滾動來進行轉譯，以呈現一組氣象資料列。</span><span class="sxs-lookup"><span data-stu-id="9859e-125">`Virtualize` component ([`Shared/Virtualize.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs)): A component written in C# that implements <xref:Microsoft.AspNetCore.Components.ComponentBase> to render a set of weather data rows based on user scrolling.</span></span>
* <span data-ttu-id="9859e-126">`FetchData`component （ [`Pages/FetchData.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor) ）：使用 `Virtualize` 元件一次顯示25列的天氣資料。</span><span class="sxs-lookup"><span data-stu-id="9859e-126">`FetchData` component ([`Pages/FetchData.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor)): Uses the `Virtualize` component to display 25 rows of weather data at a time.</span></span>

## <a name="avoid-javascript-interop-to-marshal-data"></a><span data-ttu-id="9859e-127">避免 JavaScript interop 來封送處理資料</span><span class="sxs-lookup"><span data-stu-id="9859e-127">Avoid JavaScript interop to marshal data</span></span>

<span data-ttu-id="9859e-128">在 Blazor WebAssembly 中，JavaScript （JS） interop 呼叫必須流經 WebAssembly-JS 界限。</span><span class="sxs-lookup"><span data-stu-id="9859e-128">In Blazor WebAssembly, a JavaScript (JS) interop call must traverse the WebAssembly-JS boundary.</span></span> <span data-ttu-id="9859e-129">在這兩個內容中序列化和還原序列化內容，會建立應用程式的處理額外負荷。</span><span class="sxs-lookup"><span data-stu-id="9859e-129">Serializing and deserializing content across the two contexts creates processing overhead for the app.</span></span> <span data-ttu-id="9859e-130">經常的 JS interop 呼叫通常會對效能造成不良影響。</span><span class="sxs-lookup"><span data-stu-id="9859e-130">Frequent JS interop calls often adversely affects performance.</span></span> <span data-ttu-id="9859e-131">若要減少跨界限的資料封送處理，請判斷應用程式是否可以將許多小型承載合併成單一大型承載，以避免在 WebAssembly 和 JS 之間進行大量的內容切換。</span><span class="sxs-lookup"><span data-stu-id="9859e-131">To reduce the marshalling of data across the boundary, determine if the app can consolidate many small payloads into a single large payload to avoid the high volume of context switching between WebAssembly and JS.</span></span>

## <a name="use-systemtextjson"></a><span data-ttu-id="9859e-132">使用 System.Text.Js</span><span class="sxs-lookup"><span data-stu-id="9859e-132">Use System.Text.Json</span></span>

Blazor<span data-ttu-id="9859e-133">的 JS interop 實現依賴 <xref:System.Text.Json> ，這是具有低記憶體配置的高效能 JSON 序列化程式庫。</span><span class="sxs-lookup"><span data-stu-id="9859e-133">'s JS interop implementation relies on <xref:System.Text.Json>, which is a high-performance JSON serialization library with low memory allocation.</span></span> <span data-ttu-id="9859e-134">使用 <xref:System.Text.Json> 不會導致額外的應用程式裝載大小超過新增一或多個替代 JSON 程式庫。</span><span class="sxs-lookup"><span data-stu-id="9859e-134">Using <xref:System.Text.Json> doesn't result in additional app payload size over adding one or more alternate JSON libraries.</span></span>

<span data-ttu-id="9859e-135">如需遷移指引，請參閱[如何從 `Newtonsoft.Json` 遷移 `System.Text.Json` 至](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to)。</span><span class="sxs-lookup"><span data-stu-id="9859e-135">For migration guidance, see [How to migrate from `Newtonsoft.Json` to `System.Text.Json`](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).</span></span>

## <a name="use-synchronous-and-unmarshalled-js-interop-apis-where-appropriate"></a><span data-ttu-id="9859e-136">適當時使用同步和 unmarshalled JS interop Api</span><span class="sxs-lookup"><span data-stu-id="9859e-136">Use synchronous and unmarshalled JS interop APIs where appropriate</span></span>

Blazor<span data-ttu-id="9859e-137">WebAssembly <xref:Microsoft.JSInterop.IJSRuntime> 在 Blazor 伺服器應用程式可用的單一版本上提供兩個額外版本：</span><span class="sxs-lookup"><span data-stu-id="9859e-137"> WebAssembly offers two additional versions of <xref:Microsoft.JSInterop.IJSRuntime> over the single version available to Blazor Server apps:</span></span>

* <span data-ttu-id="9859e-138"><xref:Microsoft.JSInterop.IJSInProcessRuntime>允許同步叫用 JS interop 呼叫，其額外負荷比非同步版本少：</span><span class="sxs-lookup"><span data-stu-id="9859e-138"><xref:Microsoft.JSInterop.IJSInProcessRuntime> allows invoking JS interop calls synchronously, which has less overhead than the asynchronous versions:</span></span>

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (IJSInProcessRuntime)JS;

          var value = jsInProcess.Invoke<string>("jsInteropCall");
      }
  }
  ```

* <span data-ttu-id="9859e-139"><xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime>允許 unmarshalled JS interop 呼叫：</span><span class="sxs-lookup"><span data-stu-id="9859e-139"><xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> permits unmarshalled JS interop calls:</span></span>

  ```javascript
  function jsInteropCall() {
    return BINDING.js_to_mono_obj("Hello world");
  }
  ```

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (WebAssemblyJSRuntime)JS;

          var value = jsInProcess.InvokeUnmarshalled<string>("jsInteropCall");
      }
  }
  ```

  > [!WARNING]
  > <span data-ttu-id="9859e-140">雖然使用的是 <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> JS interop 方法的最小額外負荷，但目前尚未記載與這些 api 互動所需的 JavaScript api，並在未來的版本中受到重大變更的影響。</span><span class="sxs-lookup"><span data-stu-id="9859e-140">While using <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> has the least overhead of the JS interop approaches, the JavaScript APIs required to interact with these APIs are currently undocumented and subject to breaking changes in future releases.</span></span>

## <a name="reduce-app-size"></a><span data-ttu-id="9859e-141">減少應用程式大小</span><span class="sxs-lookup"><span data-stu-id="9859e-141">Reduce app size</span></span>

### <a name="intermediate-language-il-linking"></a><span data-ttu-id="9859e-142">中繼語言（IL）連結</span><span class="sxs-lookup"><span data-stu-id="9859e-142">Intermediate Language (IL) linking</span></span>

<span data-ttu-id="9859e-143">[連結 BlazorWebAssembly 應用程式](xref:blazor/host-and-deploy/configure-linker)藉由在應用程式的二進位檔中修剪未使用的程式碼，來減少應用程式的大小。</span><span class="sxs-lookup"><span data-stu-id="9859e-143">[Linking a Blazor WebAssembly app](xref:blazor/host-and-deploy/configure-linker) reduces the app's size by trimming unused code in the app's binaries.</span></span> <span data-ttu-id="9859e-144">根據預設，只有在設定中建立時，才會啟用連結器 `Release` 。</span><span class="sxs-lookup"><span data-stu-id="9859e-144">By default, the linker is only enabled when building in `Release` configuration.</span></span> <span data-ttu-id="9859e-145">若要受益于此，請使用命令發佈應用程式以進行部署， [`dotnet publish`](/dotnet/core/tools/dotnet-publish) 並將[-c |--configuration](/dotnet/core/tools/dotnet-publish#options)選項設為 `Release` ：</span><span class="sxs-lookup"><span data-stu-id="9859e-145">To benefit from this, publish the app for deployment using the [`dotnet publish`](/dotnet/core/tools/dotnet-publish) command with the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option set to `Release`:</span></span>

```dotnetcli
dotnet publish -c Release
```

### <a name="compression"></a><span data-ttu-id="9859e-146">壓縮</span><span class="sxs-lookup"><span data-stu-id="9859e-146">Compression</span></span>

<span data-ttu-id="9859e-147">Blazor發佈 WebAssembly 應用程式時，會在發佈期間以靜態方式壓縮輸出，以減少應用程式的大小，並移除執行時間壓縮的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="9859e-147">When a Blazor WebAssembly app is published, the output is statically compressed during publish to reduce the app's size and remove the overhead for runtime compression.</span></span> Blazor<span data-ttu-id="9859e-148">依賴伺服器來執行 content negotation，並提供靜態壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="9859e-148"> relies on the server to perform content negotation and serve statically-compressed files.</span></span>

<span data-ttu-id="9859e-149">部署應用程式之後，請確認應用程式會提供壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="9859e-149">After an app is deployed, verify that the app serves compressed files.</span></span> <span data-ttu-id="9859e-150">檢查瀏覽器開發人員工具中的 [網路] 索引標籤，並確認已使用或提供檔案 `Content-Encoding: br` `Content-Encoding: gz` 。</span><span class="sxs-lookup"><span data-stu-id="9859e-150">Inspect the Network tab in a browser's Developer Tools and verify that the files are served with `Content-Encoding: br` or `Content-Encoding: gz`.</span></span> <span data-ttu-id="9859e-151">如果主機未提供壓縮檔案，請遵循中的指示 <xref:blazor/host-and-deploy/webassembly#compression> 。</span><span class="sxs-lookup"><span data-stu-id="9859e-151">If the host isn't serving compressed files, follow the instructions in <xref:blazor/host-and-deploy/webassembly#compression>.</span></span>

### <a name="disable-unused-features"></a><span data-ttu-id="9859e-152">停用未使用的功能</span><span class="sxs-lookup"><span data-stu-id="9859e-152">Disable unused features</span></span>

Blazor<span data-ttu-id="9859e-153">WebAssembly 的執行時間包含下列 .NET 功能，如果應用程式不需要它們來取得較小的承載大小，就可以停用這些功能：</span><span class="sxs-lookup"><span data-stu-id="9859e-153"> WebAssembly's runtime includes the following .NET features that can be disabled if the app doesn't require them for a smaller payload size:</span></span>

* <span data-ttu-id="9859e-154">包含資料檔案可讓時區資訊正確無誤。</span><span class="sxs-lookup"><span data-stu-id="9859e-154">A data file is included to make timezone information correct.</span></span> <span data-ttu-id="9859e-155">如果應用程式不需要這項功能，請考慮將 `BlazorEnableTimeZoneSupport` 應用程式的專案檔中的 MSBuild 屬性設定為來停用它 `false` ：</span><span class="sxs-lookup"><span data-stu-id="9859e-155">If the app doesn't require this feature, consider disabling it by setting the `BlazorEnableTimeZoneSupport` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorEnableTimeZoneSupport>false</BlazorEnableTimeZoneSupport>
  </PropertyGroup>
  ```

* <span data-ttu-id="9859e-156">已包含定序資訊，讓 Api （如 <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> 工作）正確運作。</span><span class="sxs-lookup"><span data-stu-id="9859e-156">Collation information is included to make APIs such as <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> work correctly.</span></span> <span data-ttu-id="9859e-157">如果您確定應用程式不需要定序資料，請考慮將 `BlazorWebAssemblyPreserveCollationData` 應用程式的專案檔中的 MSBuild 屬性設定為來停用它 `false` ：</span><span class="sxs-lookup"><span data-stu-id="9859e-157">If you're certain that the app doesn't require the collation data, consider disabling it by setting the `BlazorWebAssemblyPreserveCollationData` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorWebAssemblyPreserveCollationData>false</BlazorWebAssemblyPreserveCollationData>
  </PropertyGroup>
  ```
