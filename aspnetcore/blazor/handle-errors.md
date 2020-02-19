---
title: 處理 ASP.NET Core Blazor 應用程式中的錯誤
author: guardrex
description: 探索 ASP.NET Core 如何 Blazor Blazor 如何管理未處理的例外狀況，以及如何開發偵測和處理錯誤的應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: 7191ae50d64ebd6a9b23b391116aedf3a6d01de2
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447018"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a><span data-ttu-id="c7ba8-103">處理 ASP.NET Core Blazor 應用程式中的錯誤</span><span class="sxs-lookup"><span data-stu-id="c7ba8-103">Handle errors in ASP.NET Core Blazor apps</span></span>

<span data-ttu-id="c7ba8-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="c7ba8-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="c7ba8-105">本文說明 Blazor 如何管理未處理的例外狀況，以及如何開發偵測和處理錯誤的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-105">This article describes how Blazor manages unhandled exceptions and how to develop apps that detect and handle errors.</span></span>

## <a name="detailed-errors-during-development"></a><span data-ttu-id="c7ba8-106">開發期間的詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="c7ba8-106">Detailed errors during development</span></span>

<span data-ttu-id="c7ba8-107">當 Blazor 應用程式在開發期間無法正常運作時，從應用程式接收詳細的錯誤資訊有助於疑難排解和修正問題。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-107">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="c7ba8-108">發生錯誤時，Blazor 應用程式會在畫面底部顯示 [金色] 列：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-108">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="c7ba8-109">在開發期間，「金級」列會將您導向至瀏覽器主控台，您可以在其中看到例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-109">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="c7ba8-110">在生產環境中，「金級」列會通知使用者發生錯誤，並建議重新整理瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-110">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="c7ba8-111">此錯誤處理體驗的 UI 是 Blazor 專案範本的一部分。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-111">The UI for this error handling experience is part of the Blazor project templates.</span></span>

<span data-ttu-id="c7ba8-112">在 Blazor WebAssembly 應用程式中，自訂*wwwroot/index.html*檔案中的體驗：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-112">In a Blazor WebAssembly app, customize the experience in the *wwwroot/index.html* file:</span></span>

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="c7ba8-113">在 Blazor 伺服器應用程式中，自訂*Pages/_Host. cshtml*檔案中的體驗：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-113">In a Blazor Server app, customize the experience in the *Pages/_Host.cshtml* file:</span></span>

```cshtml
<div id="blazor-error-ui">
    <environment include="Staging,Production">
        An error has occurred. This application may no longer respond until reloaded.
    </environment>
    <environment include="Development">
        An unhandled exception has occurred. See browser dev tools for details.
    </environment>
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="c7ba8-114">`blazor-error-ui` 專案是由 Blazor 範本所包含的樣式所隱藏，然後在發生錯誤時顯示。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-114">The `blazor-error-ui` element is hidden by the styles included with the Blazor templates and then shown when an error occurs.</span></span>

## <a name="how-a-opno-locblazor-server-app-reacts-to-unhandled-exceptions"></a><span data-ttu-id="c7ba8-115">Blazor Server 應用程式如何回應未處理的例外狀況</span><span class="sxs-lookup"><span data-stu-id="c7ba8-115">How a Blazor Server app reacts to unhandled exceptions</span></span>

Blazor<span data-ttu-id="c7ba8-116"> Server 是具狀態架構。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-116"> Server is a stateful framework.</span></span> <span data-ttu-id="c7ba8-117">當使用者與應用程式互動時，它們會維持與伺服器的連線，稱為「*線路*」。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-117">While users interact with an app, they maintain a connection to the server known as a *circuit*.</span></span> <span data-ttu-id="c7ba8-118">線路包含作用中的元件實例，以及其他許多狀態層面，例如：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-118">The circuit holds active component instances, plus many other aspects of state, such as:</span></span>

* <span data-ttu-id="c7ba8-119">最新呈現的元件輸出。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-119">The most recent rendered output of components.</span></span>
* <span data-ttu-id="c7ba8-120">可由用戶端事件觸發的目前事件處理委派集合。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-120">The current set of event-handling delegates that could be triggered by client-side events.</span></span>

<span data-ttu-id="c7ba8-121">如果使用者在多個瀏覽器索引標籤中開啟應用程式，則會有多個獨立線路。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-121">If a user opens the app in multiple browser tabs, they have multiple independent circuits.</span></span>

Blazor<span data-ttu-id="c7ba8-122"> 將大部分未處理的例外狀況視為其發生所在的電路的嚴重問題。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-122"> treats most unhandled exceptions as fatal to the circuit where they occur.</span></span> <span data-ttu-id="c7ba8-123">如果線路因未處理的例外狀況而終止，則使用者只需重載頁面來建立新的線路，即可繼續與應用程式互動。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-123">If a circuit is terminated due to an unhandled exception, the user can only continue to interact with the app by reloading the page to create a new circuit.</span></span> <span data-ttu-id="c7ba8-124">在終止的電路以外的線路（也就是其他使用者或其他瀏覽器索引標籤的線路）不受影響。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-124">Circuits outside of the one that's terminated, which are circuits for other users or other browser tabs, aren't affected.</span></span> <span data-ttu-id="c7ba8-125">此案例與當機&mdash;的桌面應用程式相似，因為必須重新開機損毀的應用程式，但不會影響其他應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-125">This scenario is similar to a desktop app that crashes&mdash;the crashed app must be restarted, but other apps aren't affected.</span></span>

<span data-ttu-id="c7ba8-126">當未處理的例外狀況發生時，會終止電路，原因如下：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-126">A circuit is terminated when an unhandled exception occurs for the following reasons:</span></span>

* <span data-ttu-id="c7ba8-127">未處理的例外狀況通常會使線路處於未定義的狀態。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-127">An unhandled exception often leaves the circuit in an undefined state.</span></span>
* <span data-ttu-id="c7ba8-128">無法在未處理的例外狀況之後保證應用程式的一般作業。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-128">The app's normal operation can't be guaranteed after an unhandled exception.</span></span>
* <span data-ttu-id="c7ba8-129">如果線路繼續進行，應用程式中可能會出現安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-129">Security vulnerabilities may appear in the app if the circuit continues.</span></span>

## <a name="manage-unhandled-exceptions-in-developer-code"></a><span data-ttu-id="c7ba8-130">在開發人員程式碼中管理未處理的例外狀況</span><span class="sxs-lookup"><span data-stu-id="c7ba8-130">Manage unhandled exceptions in developer code</span></span>

<span data-ttu-id="c7ba8-131">若要讓應用程式在發生錯誤後繼續，應用程式必須有錯誤處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-131">For an app to continue after an error, the app must have error handling logic.</span></span> <span data-ttu-id="c7ba8-132">本文稍後的章節會描述未處理之例外狀況的可能來源。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-132">Later sections of this article describe potential sources of unhandled exceptions.</span></span>

<span data-ttu-id="c7ba8-133">在生產環境中，請勿轉譯架構例外狀況訊息，或在 UI 中呈現堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-133">In production, don't render framework exception messages or stack traces in the UI.</span></span> <span data-ttu-id="c7ba8-134">呈現例外狀況訊息或堆疊追蹤可能：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-134">Rendering exception messages or stack traces could:</span></span>

* <span data-ttu-id="c7ba8-135">向終端使用者公開機密資訊。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-135">Disclose sensitive information to end users.</span></span>
* <span data-ttu-id="c7ba8-136">協助惡意使用者探索應用程式中可能會危害應用程式、伺服器或網路安全性的弱點。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-136">Help a malicious user discover weaknesses in an app that can compromise the security of the app, server, or network.</span></span>

## <a name="log-errors-with-a-persistent-provider"></a><span data-ttu-id="c7ba8-137">使用持續性提供者記錄錯誤</span><span class="sxs-lookup"><span data-stu-id="c7ba8-137">Log errors with a persistent provider</span></span>

<span data-ttu-id="c7ba8-138">如果發生未處理的例外狀況，則會將例外狀況記錄到服務容器中所設定 <xref:Microsoft.Extensions.Logging.ILogger> 實例。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-138">If an unhandled exception occurs, the exception is logged to <xref:Microsoft.Extensions.Logging.ILogger> instances configured in the service container.</span></span> <span data-ttu-id="c7ba8-139">根據預設，Blazor apps 會使用主控台記錄提供者來記錄至主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-139">By default, Blazor apps log to console output with the Console Logging Provider.</span></span> <span data-ttu-id="c7ba8-140">請考慮使用可記錄管理大小和記錄輪替的提供者，記錄到更永久的位置。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-140">Consider logging to a more permanent location with a provider that manages log size and log rotation.</span></span> <span data-ttu-id="c7ba8-141">如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-141">For more information, see <xref:fundamentals/logging/index>.</span></span>

<span data-ttu-id="c7ba8-142">在開發期間，Blazor 通常會將例外狀況的完整詳細資料傳送至瀏覽器的主控台，以協助進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-142">During development, Blazor usually sends the full details of exceptions to the browser's console to aid in debugging.</span></span> <span data-ttu-id="c7ba8-143">在生產環境中，瀏覽器主控台中的詳細錯誤預設為停用，這表示錯誤不會傳送至用戶端，但例外狀況的完整詳細資料仍會記錄在伺服器端。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-143">In production, detailed errors in the browser's console are disabled by default, which means that errors aren't sent to clients but the exception's full details are still logged server-side.</span></span> <span data-ttu-id="c7ba8-144">如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-144">For more information, see <xref:fundamentals/error-handling>.</span></span>

<span data-ttu-id="c7ba8-145">您必須決定要記錄哪些事件，以及記錄事件的嚴重性層級。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-145">You must decide which incidents to log and the level of severity of logged incidents.</span></span> <span data-ttu-id="c7ba8-146">惡意的使用者可能可以故意觸發錯誤。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-146">Hostile users might be able to trigger errors deliberately.</span></span> <span data-ttu-id="c7ba8-147">例如，請勿從顯示產品詳細資料之元件的 URL 中提供不明 `ProductId` 的錯誤中記錄事件。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-147">For example, don't log an incident from an error where an unknown `ProductId` is supplied in the URL of a component that displays product details.</span></span> <span data-ttu-id="c7ba8-148">並非所有錯誤都應該視為高嚴重性事件以進行記錄。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-148">Not all errors should be treated as high-severity incidents for logging.</span></span>

## <a name="places-where-errors-may-occur"></a><span data-ttu-id="c7ba8-149">可能發生錯誤的位置</span><span class="sxs-lookup"><span data-stu-id="c7ba8-149">Places where errors may occur</span></span>

<span data-ttu-id="c7ba8-150">架構和應用程式程式碼可能會在下列任何位置觸發未處理的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-150">Framework and app code may trigger unhandled exceptions in any of the following locations:</span></span>

* [<span data-ttu-id="c7ba8-151">元件具現化</span><span class="sxs-lookup"><span data-stu-id="c7ba8-151">Component instantiation</span></span>](#component-instantiation)
* [<span data-ttu-id="c7ba8-152">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="c7ba8-152">Lifecycle methods</span></span>](#lifecycle-methods)
* [<span data-ttu-id="c7ba8-153">呈現邏輯</span><span class="sxs-lookup"><span data-stu-id="c7ba8-153">Rendering logic</span></span>](#rendering-logic)
* [<span data-ttu-id="c7ba8-154">事件處理常式</span><span class="sxs-lookup"><span data-stu-id="c7ba8-154">Event handlers</span></span>](#event-handlers)
* [<span data-ttu-id="c7ba8-155">元件處置</span><span class="sxs-lookup"><span data-stu-id="c7ba8-155">Component disposal</span></span>](#component-disposal)
* [<span data-ttu-id="c7ba8-156">JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="c7ba8-156">JavaScript interop</span></span>](#javascript-interop)
* <span data-ttu-id="c7ba8-157">[Blazor Server 線路處理常式](#blazor-server-circuit-handlers)</span><span class="sxs-lookup"><span data-stu-id="c7ba8-157">[Blazor Server circuit handlers](#blazor-server-circuit-handlers)</span></span>
* <span data-ttu-id="c7ba8-158">[Blazor 伺服器線路處置](#blazor-server-circuit-disposal)</span><span class="sxs-lookup"><span data-stu-id="c7ba8-158">[Blazor Server circuit disposal](#blazor-server-circuit-disposal)</span></span>
* <span data-ttu-id="c7ba8-159">[Blazor 伺服器 rerendering](#blazor-server-prerendering)</span><span class="sxs-lookup"><span data-stu-id="c7ba8-159">[Blazor Server rerendering](#blazor-server-prerendering)</span></span>

<span data-ttu-id="c7ba8-160">前述未處理的例外狀況會在本文的下列各節中說明。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-160">The preceding unhandled exceptions are described in the following sections of this article.</span></span>

### <a name="component-instantiation"></a><span data-ttu-id="c7ba8-161">元件具現化</span><span class="sxs-lookup"><span data-stu-id="c7ba8-161">Component instantiation</span></span>

<span data-ttu-id="c7ba8-162">當 Blazor 建立元件的實例時：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-162">When Blazor creates an instance of a component:</span></span>

* <span data-ttu-id="c7ba8-163">會叫用元件的函式。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-163">The component's constructor is invoked.</span></span>
* <span data-ttu-id="c7ba8-164">系統會叫用透過[`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component)指示詞或[`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component)屬性提供給元件之函式的任何非 singleton DI 服務的函式。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-164">The constructors of any non-singleton DI services supplied to the component's constructor via the [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) directive or the [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) attribute are invoked.</span></span>

<span data-ttu-id="c7ba8-165">當任何 `[Inject]` 屬性的任何執行的函式或 setter 擲回未處理的例外狀況時，Blazor Server 線路就會失敗。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-165">A Blazor Server circuit fails when any executed constructor or a setter for any `[Inject]` property throws an unhandled exception.</span></span> <span data-ttu-id="c7ba8-166">例外狀況是嚴重的，因為架構無法具現化元件。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-166">The exception is fatal because the framework can't instantiate the component.</span></span> <span data-ttu-id="c7ba8-167">如果函式邏輯可能會擲回例外狀況，則應用程式應該使用具有錯誤處理和記錄功能的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來捕捉例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-167">If constructor logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

### <a name="lifecycle-methods"></a><span data-ttu-id="c7ba8-168">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="c7ba8-168">Lifecycle methods</span></span>

<span data-ttu-id="c7ba8-169">在元件的存留期間，Blazor 會叫用下列[生命週期方法](xref:blazor/lifecycle)：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-169">During the lifetime of a component, Blazor invokes the following [lifecycle methods](xref:blazor/lifecycle):</span></span>

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

<span data-ttu-id="c7ba8-170">如果任何生命週期方法以同步或非同步方式擲回例外狀況，則例外狀況對 Blazor 伺服器線路而言是嚴重的。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-170">If any lifecycle method throws an exception, synchronously or asynchronously, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="c7ba8-171">若要讓元件處理生命週期方法中的錯誤，請新增錯誤處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-171">For components to deal with errors in lifecycle methods, add error handling logic.</span></span>

<span data-ttu-id="c7ba8-172">在下列範例中，`OnParametersSetAsync` 會呼叫方法來取得產品：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-172">In the following example where `OnParametersSetAsync` calls a method to obtain a product:</span></span>

* <span data-ttu-id="c7ba8-173">在 `ProductRepository.GetProductByIdAsync` 方法中擲回的例外狀況是由 `try-catch` 語句所處理。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-173">An exception thrown in the `ProductRepository.GetProductByIdAsync` method is handled by a `try-catch` statement.</span></span>
* <span data-ttu-id="c7ba8-174">執行 `catch` 區塊時：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-174">When the `catch` block is executed:</span></span>
  * <span data-ttu-id="c7ba8-175">`loadFailed` 設定為 `true`，用來向使用者顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-175">`loadFailed` is set to `true`, which is used to display an error message to the user.</span></span>
  * <span data-ttu-id="c7ba8-176">會記錄錯誤。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-176">The error is logged.</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a><span data-ttu-id="c7ba8-177">呈現邏輯</span><span class="sxs-lookup"><span data-stu-id="c7ba8-177">Rendering logic</span></span>

<span data-ttu-id="c7ba8-178">`.razor` 元件檔案中的宣告式標記會編譯成稱為C# `BuildRenderTree`的方法。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-178">The declarative markup in a `.razor` component file is compiled into a C# method called `BuildRenderTree`.</span></span> <span data-ttu-id="c7ba8-179">當元件呈現時，`BuildRenderTree` 會執行並建立資料結構，以描述所轉譯元件的元素、文字和子元件。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-179">When a component renders, `BuildRenderTree` executes and builds up a data structure describing the elements, text, and child components of the rendered component.</span></span>

<span data-ttu-id="c7ba8-180">轉譯邏輯可能會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-180">Rendering logic can throw an exception.</span></span> <span data-ttu-id="c7ba8-181">當評估 `@someObject.PropertyName`，但 `@someObject` `null`時，就會發生這種情況的範例。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-181">An example of this scenario occurs when `@someObject.PropertyName` is evaluated but `@someObject` is `null`.</span></span> <span data-ttu-id="c7ba8-182">轉譯邏輯所擲回的未處理例外狀況是 Blazor 伺服器線路的嚴重錯誤。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-182">An unhandled exception thrown by rendering logic is fatal to a Blazor Server circuit.</span></span>

<span data-ttu-id="c7ba8-183">若要避免轉譯邏輯中出現 null 參考例外狀況，請先檢查是否有 `null` 物件，然後再存取其成員。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-183">To prevent a null reference exception in rendering logic, check for a `null` object before accessing its members.</span></span> <span data-ttu-id="c7ba8-184">在下列範例中，如果 `null``person.Address`，則不會存取 `person.Address` 屬性：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-184">In the following example, `person.Address` properties aren't accessed if `person.Address` is `null`:</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

<span data-ttu-id="c7ba8-185">上述程式碼假設 `person` 不 `null`。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-185">The preceding code assumes that `person` isn't `null`.</span></span> <span data-ttu-id="c7ba8-186">通常，程式碼的結構會保證在呈現元件時，物件存在。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-186">Often, the structure of the code guarantees that an object exists at the time the component is rendered.</span></span> <span data-ttu-id="c7ba8-187">在這些情況下，不需要檢查呈現邏輯中的 `null`。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-187">In those cases, it isn't necessary to check for `null` in rendering logic.</span></span> <span data-ttu-id="c7ba8-188">在先前的範例中，`person` 可能會保證存在，因為 `person` 會在元件具現化時建立。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-188">In the prior example, `person` might be guaranteed to exist because `person` is created when the component is instantiated.</span></span>

### <a name="event-handlers"></a><span data-ttu-id="c7ba8-189">事件處理常式</span><span class="sxs-lookup"><span data-stu-id="c7ba8-189">Event handlers</span></span>

<span data-ttu-id="c7ba8-190">當事件處理常式是使用建立C#時，用戶端程式代碼會觸發程式碼的調用：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-190">Client-side code triggers invocations of C# code when event handlers are created using:</span></span>

* `@onclick`
* `@onchange`
* <span data-ttu-id="c7ba8-191">其他 `@on...` 屬性</span><span class="sxs-lookup"><span data-stu-id="c7ba8-191">Other `@on...` attributes</span></span>
* `@bind`

<span data-ttu-id="c7ba8-192">在這些情況下，事件處理常式程式碼可能會擲回未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-192">Event handler code might throw an unhandled exception in these scenarios.</span></span>

<span data-ttu-id="c7ba8-193">如果事件處理常式擲回未處理的例外狀況（例如，資料庫查詢失敗），則例外狀況對 Blazor 伺服器線路而言是嚴重的。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-193">If an event handler throws an unhandled exception (for example, a database query fails), the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="c7ba8-194">如果應用程式呼叫可能因外部原因而失敗的程式碼，請使用具有錯誤處理和記錄的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來設陷例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-194">If the app calls code that could fail for external reasons, trap exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="c7ba8-195">如果使用者程式碼不會陷印並處理例外狀況，則架構會記錄例外狀況並終止線路。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-195">If user code doesn't trap and handle the exception, the framework logs the exception and terminates the circuit.</span></span>

### <a name="component-disposal"></a><span data-ttu-id="c7ba8-196">元件處置</span><span class="sxs-lookup"><span data-stu-id="c7ba8-196">Component disposal</span></span>

<span data-ttu-id="c7ba8-197">例如，元件可能會從 UI 移除，因為使用者已流覽至另一個頁面。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-197">A component may be removed from the UI, for example, because the user has navigated to another page.</span></span> <span data-ttu-id="c7ba8-198">從 UI 移除執行 <xref:System.IDisposable?displayProperty=fullName> 的元件時，架構會呼叫元件的 <xref:System.IDisposable.Dispose*> 方法。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-198">When a component that implements <xref:System.IDisposable?displayProperty=fullName> is removed from the UI, the framework calls the component's <xref:System.IDisposable.Dispose*> method.</span></span>

<span data-ttu-id="c7ba8-199">如果元件的 `Dispose` 方法擲回未處理的例外狀況，則例外狀況對 Blazor 伺服器線路而言是嚴重的。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-199">If the component's `Dispose` method throws an unhandled exception, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="c7ba8-200">如果處置邏輯可能會擲回例外狀況，則應用程式應該使用具有錯誤處理和記錄的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來捕捉例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-200">If disposal logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="c7ba8-201">如需有關元件處置的詳細資訊，請參閱 <xref:blazor/lifecycle#component-disposal-with-idisposable>。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-201">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

### <a name="javascript-interop"></a><span data-ttu-id="c7ba8-202">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="c7ba8-202">JavaScript interop</span></span>

<span data-ttu-id="c7ba8-203">`IJSRuntime.InvokeAsync<T>` 可讓 .NET 程式碼在使用者的瀏覽器中對 JavaScript 執行時間進行非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-203">`IJSRuntime.InvokeAsync<T>` allows .NET code to make asynchronous calls to the JavaScript runtime in the user's browser.</span></span>

<span data-ttu-id="c7ba8-204">下列條件適用于使用 `InvokeAsync<T>`的錯誤處理：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-204">The following conditions apply to error handling with `InvokeAsync<T>`:</span></span>

* <span data-ttu-id="c7ba8-205">如果 `InvokeAsync<T>` 的呼叫同步失敗，就會發生 .NET 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-205">If a call to `InvokeAsync<T>` fails synchronously, a .NET exception occurs.</span></span> <span data-ttu-id="c7ba8-206">例如，`InvokeAsync<T>` 的呼叫可能會失敗，因為無法序列化提供的引數。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-206">A call to `InvokeAsync<T>` may fail, for example, because the supplied arguments can't be serialized.</span></span> <span data-ttu-id="c7ba8-207">開發人員程式碼必須攔截例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-207">Developer code must catch the exception.</span></span> <span data-ttu-id="c7ba8-208">如果事件處理常式或元件生命週期方法中的應用程式程式碼不會處理例外狀況，則產生的例外狀況對 Blazor 伺服器線路而言是嚴重的。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-208">If app code in an event handler or component lifecycle method doesn't handle an exception, the resulting exception is fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="c7ba8-209">如果 `InvokeAsync<T>` 的呼叫以非同步方式失敗，則 .NET <xref:System.Threading.Tasks.Task> 會失敗。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-209">If a call to `InvokeAsync<T>` fails asynchronously, the .NET <xref:System.Threading.Tasks.Task> fails.</span></span> <span data-ttu-id="c7ba8-210">`InvokeAsync<T>` 的呼叫可能會失敗，例如，JavaScript 端程式碼會擲回例外狀況，或傳回以 `rejected`完成的 `Promise`。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-210">A call to `InvokeAsync<T>` may fail, for example, because the JavaScript-side code throws an exception or returns a `Promise` that completed as `rejected`.</span></span> <span data-ttu-id="c7ba8-211">開發人員程式碼必須攔截例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-211">Developer code must catch the exception.</span></span> <span data-ttu-id="c7ba8-212">如果使用[await](/dotnet/csharp/language-reference/keywords/await)運算子，請考慮將方法呼叫包裝在含有錯誤處理和記錄的[try catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-212">If using the [await](/dotnet/csharp/language-reference/keywords/await) operator, consider wrapping the method call in a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span> <span data-ttu-id="c7ba8-213">否則，失敗的程式碼會導致 Blazor 伺服器電路嚴重的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-213">Otherwise, the failing code results in an unhandled exception that's fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="c7ba8-214">根據預設，`InvokeAsync<T>` 的呼叫必須在特定期間內完成，否則呼叫會超時。預設的超時時間為一分鐘。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-214">By default, calls to `InvokeAsync<T>` must complete within a certain period or else the call times out. The default timeout period is one minute.</span></span> <span data-ttu-id="c7ba8-215">Timeout 會保護程式碼不會遺失網路連線，或永遠不會傳回完成訊息的 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-215">The timeout protects the code against a loss in network connectivity or JavaScript code that never sends back a completion message.</span></span> <span data-ttu-id="c7ba8-216">如果呼叫超時，則產生的 `Task` 會因 <xref:System.OperationCanceledException>而失敗。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-216">If the call times out, the resulting `Task` fails with an <xref:System.OperationCanceledException>.</span></span> <span data-ttu-id="c7ba8-217">使用記錄來設陷並處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-217">Trap and process the exception with logging.</span></span>

<span data-ttu-id="c7ba8-218">同樣地，JavaScript 程式碼可能會起始呼叫[`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions)屬性所指示的 .net 方法。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-218">Similarly, JavaScript code may initiate calls to .NET methods indicated by the [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) attribute.</span></span> <span data-ttu-id="c7ba8-219">如果這些 .NET 方法擲回未處理的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-219">If these .NET methods throw an unhandled exception:</span></span>

* <span data-ttu-id="c7ba8-220">例外狀況不會被視為 Blazor 伺服器線路的嚴重錯誤。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-220">The exception isn't treated as fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="c7ba8-221">JavaScript 端 `Promise` 遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-221">The JavaScript-side `Promise` is rejected.</span></span>

<span data-ttu-id="c7ba8-222">您可以選擇在 .NET 端或方法呼叫的 JavaScript 端使用錯誤處理常式代碼。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-222">You have the option of using error handling code on either the .NET side or the JavaScript side of the method call.</span></span>

<span data-ttu-id="c7ba8-223">如需詳細資訊，請參閱 <xref:blazor/javascript-interop>。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-223">For more information, see <xref:blazor/javascript-interop>.</span></span>

### <a name="opno-locblazor-server-circuit-handlers"></a>Blazor<span data-ttu-id="c7ba8-224"> Server 線路處理常式</span><span class="sxs-lookup"><span data-stu-id="c7ba8-224"> Server circuit handlers</span></span>

Blazor<span data-ttu-id="c7ba8-225"> Server 可讓程式碼定義*電路處理常式*，以允許對使用者線路狀態的變更執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-225"> Server allows code to define a *circuit handler*, which allows running code on changes to the state of a user's circuit.</span></span> <span data-ttu-id="c7ba8-226">線路處理常式是透過衍生自 `CircuitHandler` 並在應用程式的服務容器中註冊類別來執行。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-226">A circuit handler is implemented by deriving from `CircuitHandler` and registering the class in the app's service container.</span></span> <span data-ttu-id="c7ba8-227">下列線路處理常式範例會追蹤開啟的 SignalR 連接：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-227">The following example of a circuit handler tracks open SignalR connections:</span></span>

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

<span data-ttu-id="c7ba8-228">線路處理常式是使用 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-228">Circuit handlers are registered using DI.</span></span> <span data-ttu-id="c7ba8-229">範圍實例會針對每個線路實例而建立。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-229">Scoped instances are created per instance of a circuit.</span></span> <span data-ttu-id="c7ba8-230">使用上述範例中的 `TrackingCircuitHandler`，會建立單一服務，因為必須追蹤所有線路的狀態：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-230">Using the `TrackingCircuitHandler` in the preceding example, a singleton service is created because the state of all circuits must be tracked:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

<span data-ttu-id="c7ba8-231">如果自訂電路處理常式的方法擲回未處理的例外狀況，則例外狀況對 Blazor 伺服器線路而言是嚴重的。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-231">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the Blazor Server circuit.</span></span> <span data-ttu-id="c7ba8-232">若要容忍處理常式程式碼或呼叫方法中的例外狀況，請使用錯誤處理和記錄，將程式碼包裝在一個或多個[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-232">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

### <a name="opno-locblazor-server-circuit-disposal"></a>Blazor<span data-ttu-id="c7ba8-233"> 伺服器線路處置</span><span class="sxs-lookup"><span data-stu-id="c7ba8-233"> Server circuit disposal</span></span>

<span data-ttu-id="c7ba8-234">當線路因使用者已中斷連線而結束，而架構正在清除線路狀態時，架構會處置線路的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-234">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="c7ba8-235">處置範圍會處置任何執行 <xref:System.IDisposable?displayProperty=fullName>的線路範圍 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-235">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="c7ba8-236">如果任何 DI 服務在處置期間擲回未處理的例外狀況，則架構會記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-236">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

### <a name="opno-locblazor-server-prerendering"></a>Blazor<span data-ttu-id="c7ba8-237"> 伺服器已預呈現</span><span class="sxs-lookup"><span data-stu-id="c7ba8-237"> Server prerendering</span></span>

Blazor<span data-ttu-id="c7ba8-238"> 元件可以使用 `Component` 標籤協助程式來資源清單，如此一來，就會在使用者的初始 HTTP 要求中傳回其呈現的 HTML 標籤。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-238"> components can be prerendered using the `Component` Tag Helper so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="c7ba8-239">其運作方式如下：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-239">This works by:</span></span>

* <span data-ttu-id="c7ba8-240">針對屬於相同頁面的所有資源清單元件建立新的電路。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-240">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="c7ba8-241">產生初始 HTML。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-241">Generating the initial HTML.</span></span>
* <span data-ttu-id="c7ba8-242">將電路視為 `disconnected`，直到使用者的瀏覽器建立回到相同伺服器的 SignalR 連接為止。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-242">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="c7ba8-243">建立連線時，線路上的互動會繼續，而且元件的 HTML 標籤也會更新。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-243">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="c7ba8-244">如果任何元件在進行預入期間擲回未處理的例外狀況（例如，在生命週期方法或轉譯邏輯期間）：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-244">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="c7ba8-245">這是電路的嚴重例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-245">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="c7ba8-246">例外狀況會從 `Component` 標籤協助程式的呼叫堆疊中擲出。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-246">The exception is thrown up the call stack from the `Component` Tag Helper.</span></span> <span data-ttu-id="c7ba8-247">因此，除非開發人員程式碼明確攔截到例外狀況，否則整個 HTTP 要求都會失敗。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-247">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="c7ba8-248">在一般情況下，如果無法轉譯，則繼續建立並轉譯元件並沒有意義，因為無法轉譯運作中的元件。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-248">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="c7ba8-249">若要容忍在自動處理期間可能發生的錯誤，錯誤處理邏輯必須放在可能會擲回例外狀況的元件內部。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-249">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="c7ba8-250">使用[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句搭配錯誤處理和記錄。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-250">Use [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="c7ba8-251">不要將 `Component` 標籤協助套裝程式裝在 `try-catch` 的語句中，而是將錯誤處理邏輯放在 `Component` 標籤協助程式所呈現的元件中。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-251">Instead of wrapping the `Component` Tag Helper in a `try-catch` statement, place error handling logic in the component rendered by the `Component` Tag Helper.</span></span>

## <a name="advanced-scenarios"></a><span data-ttu-id="c7ba8-252">進階案例</span><span class="sxs-lookup"><span data-stu-id="c7ba8-252">Advanced scenarios</span></span>

### <a name="recursive-rendering"></a><span data-ttu-id="c7ba8-253">遞迴轉譯</span><span class="sxs-lookup"><span data-stu-id="c7ba8-253">Recursive rendering</span></span>

<span data-ttu-id="c7ba8-254">元件可以遞迴方式進行嵌套。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-254">Components can be nested recursively.</span></span> <span data-ttu-id="c7ba8-255">這適用于表示遞迴資料結構。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-255">This is useful for representing recursive data structures.</span></span> <span data-ttu-id="c7ba8-256">例如，`TreeNode` 元件可以呈現每個節點子系的更 `TreeNode` 元件。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-256">For example, a `TreeNode` component can render more `TreeNode` components for each of the node's children.</span></span>

<span data-ttu-id="c7ba8-257">以遞迴方式轉譯時，避免產生無限遞迴的編碼模式：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-257">When rendering recursively, avoid coding patterns that result in infinite recursion:</span></span>

* <span data-ttu-id="c7ba8-258">不要以遞迴方式呈現包含迴圈的資料結構。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-258">Don't recursively render a data structure that contains a cycle.</span></span> <span data-ttu-id="c7ba8-259">例如，不要轉譯其子系包含其本身的樹狀節點。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-259">For example, don't render a tree node whose children includes itself.</span></span>
* <span data-ttu-id="c7ba8-260">請勿建立包含迴圈的版面配置鏈。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-260">Don't create a chain of layouts that contain a cycle.</span></span> <span data-ttu-id="c7ba8-261">例如，請勿建立版面配置本身的版面配置。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-261">For example, don't create a layout whose layout is itself.</span></span>
* <span data-ttu-id="c7ba8-262">不允許終端使用者透過惡意資料輸入或 JavaScript interop 呼叫來違反遞迴不變數（規則）。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-262">Don't allow an end user to violate recursion invariants (rules) through malicious data entry or JavaScript interop calls.</span></span>

<span data-ttu-id="c7ba8-263">呈現期間的無限迴圈：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-263">Infinite loops during rendering:</span></span>

* <span data-ttu-id="c7ba8-264">導致轉譯進程永遠繼續。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-264">Causes the rendering process to continue forever.</span></span>
* <span data-ttu-id="c7ba8-265">相當於建立未結束的迴圈。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-265">Is equivalent to creating an unterminated loop.</span></span>

<span data-ttu-id="c7ba8-266">在這些情況下，受影響的 Blazor 伺服器線路會失敗，而執行緒通常會嘗試：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-266">In these scenarios, an affected Blazor Server circuit fails, and the thread usually attempts to:</span></span>

* <span data-ttu-id="c7ba8-267">會無限期地耗用作業系統所允許的 CPU 時間量。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-267">Consume as much CPU time as permitted by the operating system, indefinitely.</span></span>
* <span data-ttu-id="c7ba8-268">耗用不限數量的伺服器記憶體。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-268">Consume an unlimited amount of server memory.</span></span> <span data-ttu-id="c7ba8-269">使用無限制的記憶體相當於未結束的迴圈在每次反覆運算時將專案新增至集合的案例。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-269">Consuming unlimited memory is equivalent to the scenario where an unterminated loop adds entries to a collection on every iteration.</span></span>

<span data-ttu-id="c7ba8-270">若要避免無限遞迴模式，請確定遞迴呈現程式碼包含適當的停止條件。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-270">To avoid infinite recursion patterns, ensure that recursive rendering code contains suitable stopping conditions.</span></span>

### <a name="custom-render-tree-logic"></a><span data-ttu-id="c7ba8-271">自訂呈現樹狀結構邏輯</span><span class="sxs-lookup"><span data-stu-id="c7ba8-271">Custom render tree logic</span></span>

<span data-ttu-id="c7ba8-272">大部分的 Blazor 元件會實作為*razor*檔案，並且會進行編譯，以產生可在 `RenderTreeBuilder` 上操作以呈現其輸出的邏輯。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-272">Most Blazor components are implemented as *.razor* files and are compiled to produce logic that operates on a `RenderTreeBuilder` to render their output.</span></span> <span data-ttu-id="c7ba8-273">開發人員可以使用程式性程式C#代碼手動執行 `RenderTreeBuilder` 邏輯。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-273">A developer may manually implement `RenderTreeBuilder` logic using procedural C# code.</span></span> <span data-ttu-id="c7ba8-274">如需詳細資訊，請參閱 <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-274">For more information, see <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.</span></span>

> [!WARNING]
> <span data-ttu-id="c7ba8-275">手動轉譯樹狀結構產生器邏輯的使用會被視為先進且不安全的案例，不建議用於一般元件開發。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-275">Use of manual render tree builder logic is considered an advanced and unsafe scenario, not recommended for general component development.</span></span>

<span data-ttu-id="c7ba8-276">如果撰寫 `RenderTreeBuilder` 的程式碼，開發人員必須保證程式碼的正確性。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-276">If `RenderTreeBuilder` code is written, the developer must guarantee the correctness of the code.</span></span> <span data-ttu-id="c7ba8-277">例如，開發人員必須確定：</span><span class="sxs-lookup"><span data-stu-id="c7ba8-277">For example, the developer must ensure that:</span></span>

* <span data-ttu-id="c7ba8-278">`OpenElement` 和 `CloseElement` 的呼叫已正確平衡。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-278">Calls to `OpenElement` and `CloseElement` are correctly balanced.</span></span>
* <span data-ttu-id="c7ba8-279">屬性只會加入正確的位置。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-279">Attributes are only added in the correct places.</span></span>

<span data-ttu-id="c7ba8-280">不正確的手動轉譯樹狀結構產生器邏輯可能會造成任意未定義的行為，包括損毀、伺服器停止回應和安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-280">Incorrect manual render tree builder logic can cause arbitrary undefined behavior, including crashes, server hangs, and security vulnerabilities.</span></span>

<span data-ttu-id="c7ba8-281">請考慮以手動方式呈現相同的複雜性層級的轉譯樹產生器邏輯，以及使用與手動撰寫元件程式碼或 MSIL 指令相同層級的*危險*。</span><span class="sxs-lookup"><span data-stu-id="c7ba8-281">Consider manual render tree builder logic on the same level of complexity and with the same level of *danger* as writing assembly code or MSIL instructions by hand.</span></span>
