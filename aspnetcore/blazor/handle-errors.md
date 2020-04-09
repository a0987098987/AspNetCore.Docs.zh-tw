---
title: 處理ASP.NET核心Blazor應用程式中的錯誤
author: guardrex
description: 瞭解 ASP.NETBlazorBlazor酷 如何 管理未處理的異常以及如何開發檢測和處理錯誤的應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: 4fdaf7fb90d126b8f7f029aac3af49eec3b69e74
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80382271"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a><span data-ttu-id="90ef1-103">處理ASP.NET核心Blazor應用程式中的錯誤</span><span class="sxs-lookup"><span data-stu-id="90ef1-103">Handle errors in ASP.NET Core Blazor apps</span></span>

<span data-ttu-id="90ef1-104">作者：[Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="90ef1-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="90ef1-105">本文介紹如何Blazor管理未處理的異常以及如何開發檢測和處理錯誤的應用。</span><span class="sxs-lookup"><span data-stu-id="90ef1-105">This article describes how Blazor manages unhandled exceptions and how to develop apps that detect and handle errors.</span></span>

## <a name="detailed-errors-during-development"></a><span data-ttu-id="90ef1-106">開發過程中的詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="90ef1-106">Detailed errors during development</span></span>

<span data-ttu-id="90ef1-107">當應用Blazor在開發過程中無法正常運行時,從應用接收詳細的錯誤資訊有助於解決問題。</span><span class="sxs-lookup"><span data-stu-id="90ef1-107">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="90ef1-108">發生錯誤時,Blazor應用在螢幕底部顯示金條:</span><span class="sxs-lookup"><span data-stu-id="90ef1-108">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="90ef1-109">在開發過程中,金條將引導您到瀏覽器控制台,您可以在其中看到異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-109">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="90ef1-110">在生產中,金條會通知使用者錯誤,並建議刷新瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="90ef1-110">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="90ef1-111">此錯誤處理體驗的 UI 是專案Blazor範本的一部分。</span><span class="sxs-lookup"><span data-stu-id="90ef1-111">The UI for this error handling experience is part of the Blazor project templates.</span></span>

<span data-ttu-id="90ef1-112">在BlazorWebAssembly 應用中,自訂*wwwroot/index.html*檔中的體驗:</span><span class="sxs-lookup"><span data-stu-id="90ef1-112">In a Blazor WebAssembly app, customize the experience in the *wwwroot/index.html* file:</span></span>

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

<span data-ttu-id="90ef1-113">在Blazor'伺服器"應用中,自訂*主頁/_Host.cshtml*檔案中的體驗:</span><span class="sxs-lookup"><span data-stu-id="90ef1-113">In a Blazor Server app, customize the experience in the *Pages/_Host.cshtml* file:</span></span>

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

<span data-ttu-id="90ef1-114">`blazor-error-ui`元素被Blazor樣本中包含的樣式隱藏 *(wwwroot/css/site.css),* 然後在發生錯誤時顯示:</span><span class="sxs-lookup"><span data-stu-id="90ef1-114">The `blazor-error-ui` element is hidden by the styles included in the Blazor templates (*wwwroot/css/site.css*) and then shown when an error occurs:</span></span>

```css
#blazor-error-ui {
    background: lightyellow;
    bottom: 0;
    box-shadow: 0 -1px 2px rgba(0, 0, 0, 0.2);
    display: none;
    left: 0;
    padding: 0.6rem 1.25rem 0.7rem 1.25rem;
    position: fixed;
    width: 100%;
    z-index: 1000;
}

#blazor-error-ui .dismiss {
    cursor: pointer;
    position: absolute;
    right: 0.75rem;
    top: 0.5rem;
}
```

## <a name="how-a-opno-locblazor-server-app-reacts-to-unhandled-exceptions"></a><span data-ttu-id="90ef1-115">Blazor伺服器應用如何回應未處理的例外</span><span class="sxs-lookup"><span data-stu-id="90ef1-115">How a Blazor Server app reacts to unhandled exceptions</span></span>

Blazor<span data-ttu-id="90ef1-116">伺服器是一個有狀態的框架。</span><span class="sxs-lookup"><span data-stu-id="90ef1-116"> Server is a stateful framework.</span></span> <span data-ttu-id="90ef1-117">當使用者與應用交互時,他們維護與稱為*電路*的伺服器的連接。</span><span class="sxs-lookup"><span data-stu-id="90ef1-117">While users interact with an app, they maintain a connection to the server known as a *circuit*.</span></span> <span data-ttu-id="90ef1-118">該電路包含活動元件實例,以及狀態的許多其他方面,例如:</span><span class="sxs-lookup"><span data-stu-id="90ef1-118">The circuit holds active component instances, plus many other aspects of state, such as:</span></span>

* <span data-ttu-id="90ef1-119">元件的最新呈現輸出。</span><span class="sxs-lookup"><span data-stu-id="90ef1-119">The most recent rendered output of components.</span></span>
* <span data-ttu-id="90ef1-120">用戶端事件可能觸發的當前事件處理委託集。</span><span class="sxs-lookup"><span data-stu-id="90ef1-120">The current set of event-handling delegates that could be triggered by client-side events.</span></span>

<span data-ttu-id="90ef1-121">如果使用者在多個瀏覽器選項卡中打開應用,則他們有多個獨立電路。</span><span class="sxs-lookup"><span data-stu-id="90ef1-121">If a user opens the app in multiple browser tabs, they have multiple independent circuits.</span></span>

Blazor<span data-ttu-id="90ef1-122">將大多數未處理的異常視為對發生異常的電路的致命異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-122"> treats most unhandled exceptions as fatal to the circuit where they occur.</span></span> <span data-ttu-id="90ef1-123">如果電路由於未處理的異常而終止,則使用者只能通過重新載入頁面來創建新電路,繼續與應用交互。</span><span class="sxs-lookup"><span data-stu-id="90ef1-123">If a circuit is terminated due to an unhandled exception, the user can only continue to interact with the app by reloading the page to create a new circuit.</span></span> <span data-ttu-id="90ef1-124">終止的電路(其他使用者或其他瀏覽器選項卡的電路)以外的電路不受影響。</span><span class="sxs-lookup"><span data-stu-id="90ef1-124">Circuits outside of the one that's terminated, which are circuits for other users or other browser tabs, aren't affected.</span></span> <span data-ttu-id="90ef1-125">此方案類似於必須重新啟動崩潰的應用&mdash;崩潰的桌面應用,但其他應用不受影響。</span><span class="sxs-lookup"><span data-stu-id="90ef1-125">This scenario is similar to a desktop app that crashes&mdash;the crashed app must be restarted, but other apps aren't affected.</span></span>

<span data-ttu-id="90ef1-126">當由於以下原因發生未處理的異常時,電路終止:</span><span class="sxs-lookup"><span data-stu-id="90ef1-126">A circuit is terminated when an unhandled exception occurs for the following reasons:</span></span>

* <span data-ttu-id="90ef1-127">未處理的異常通常會使電路處於未定義狀態。</span><span class="sxs-lookup"><span data-stu-id="90ef1-127">An unhandled exception often leaves the circuit in an undefined state.</span></span>
* <span data-ttu-id="90ef1-128">未處理異常后無法保證應用的正常操作。</span><span class="sxs-lookup"><span data-stu-id="90ef1-128">The app's normal operation can't be guaranteed after an unhandled exception.</span></span>
* <span data-ttu-id="90ef1-129">如果電路繼續,應用中可能會出現安全漏洞。</span><span class="sxs-lookup"><span data-stu-id="90ef1-129">Security vulnerabilities may appear in the app if the circuit continues.</span></span>

## <a name="manage-unhandled-exceptions-in-developer-code"></a><span data-ttu-id="90ef1-130">管理開發人員代碼中未處理的例外</span><span class="sxs-lookup"><span data-stu-id="90ef1-130">Manage unhandled exceptions in developer code</span></span>

<span data-ttu-id="90ef1-131">要在錯誤後繼續應用,應用必須具有錯誤處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="90ef1-131">For an app to continue after an error, the app must have error handling logic.</span></span> <span data-ttu-id="90ef1-132">本文後面的部分介紹未處理異常的潛在來源。</span><span class="sxs-lookup"><span data-stu-id="90ef1-132">Later sections of this article describe potential sources of unhandled exceptions.</span></span>

<span data-ttu-id="90ef1-133">在生產中,不要在 UI 中呈現框架異常消息或堆疊跟蹤。</span><span class="sxs-lookup"><span data-stu-id="90ef1-133">In production, don't render framework exception messages or stack traces in the UI.</span></span> <span data-ttu-id="90ef1-134">呈現例外訊息或堆疊追蹤可以:</span><span class="sxs-lookup"><span data-stu-id="90ef1-134">Rendering exception messages or stack traces could:</span></span>

* <span data-ttu-id="90ef1-135">向最終使用者披露敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="90ef1-135">Disclose sensitive information to end users.</span></span>
* <span data-ttu-id="90ef1-136">説明惡意用戶發現應用中可能危及應用、伺服器或網路安全性的弱點。</span><span class="sxs-lookup"><span data-stu-id="90ef1-136">Help a malicious user discover weaknesses in an app that can compromise the security of the app, server, or network.</span></span>

## <a name="log-errors-with-a-persistent-provider"></a><span data-ttu-id="90ef1-137">使用持久提供程式記錄錯誤</span><span class="sxs-lookup"><span data-stu-id="90ef1-137">Log errors with a persistent provider</span></span>

<span data-ttu-id="90ef1-138">如果發生未處理的異常,則將異常記錄到<xref:Microsoft.Extensions.Logging.ILogger>服務容器中配置的實例。</span><span class="sxs-lookup"><span data-stu-id="90ef1-138">If an unhandled exception occurs, the exception is logged to <xref:Microsoft.Extensions.Logging.ILogger> instances configured in the service container.</span></span> <span data-ttu-id="90ef1-139">默認情況下,Blazor應用使用主控台日誌記錄提供程式登錄到控制台輸出。</span><span class="sxs-lookup"><span data-stu-id="90ef1-139">By default, Blazor apps log to console output with the Console Logging Provider.</span></span> <span data-ttu-id="90ef1-140">請考慮使用管理日誌大小和日誌輪換的提供程式登錄到更永久的位置。</span><span class="sxs-lookup"><span data-stu-id="90ef1-140">Consider logging to a more permanent location with a provider that manages log size and log rotation.</span></span> <span data-ttu-id="90ef1-141">如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="90ef1-141">For more information, see <xref:fundamentals/logging/index>.</span></span>

<span data-ttu-id="90ef1-142">在開發過程中Blazor,通常會將異常的完整詳細資訊發送到瀏覽器的主控台,以説明調試。</span><span class="sxs-lookup"><span data-stu-id="90ef1-142">During development, Blazor usually sends the full details of exceptions to the browser's console to aid in debugging.</span></span> <span data-ttu-id="90ef1-143">在生產中,默認情況下禁用瀏覽器控制台中的詳細錯誤,這意味著錯誤不會發送給用戶端,但異常的完整詳細資訊仍記錄伺服器端。</span><span class="sxs-lookup"><span data-stu-id="90ef1-143">In production, detailed errors in the browser's console are disabled by default, which means that errors aren't sent to clients but the exception's full details are still logged server-side.</span></span> <span data-ttu-id="90ef1-144">如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="90ef1-144">For more information, see <xref:fundamentals/error-handling>.</span></span>

<span data-ttu-id="90ef1-145">您必須決定要記錄的事件和記錄事件的嚴重性級別。</span><span class="sxs-lookup"><span data-stu-id="90ef1-145">You must decide which incidents to log and the level of severity of logged incidents.</span></span> <span data-ttu-id="90ef1-146">敵對使用者可能能夠故意觸發錯誤。</span><span class="sxs-lookup"><span data-stu-id="90ef1-146">Hostile users might be able to trigger errors deliberately.</span></span> <span data-ttu-id="90ef1-147">例如,不要從顯示產品詳細資訊的元件的 URL 中`ProductId`提供 未知內容的錯誤中記錄事件。</span><span class="sxs-lookup"><span data-stu-id="90ef1-147">For example, don't log an incident from an error where an unknown `ProductId` is supplied in the URL of a component that displays product details.</span></span> <span data-ttu-id="90ef1-148">並非所有錯誤都應被視為日誌記錄的高嚴重性事件。</span><span class="sxs-lookup"><span data-stu-id="90ef1-148">Not all errors should be treated as high-severity incidents for logging.</span></span>

## <a name="places-where-errors-may-occur"></a><span data-ttu-id="90ef1-149">可能發生錯誤的地方</span><span class="sxs-lookup"><span data-stu-id="90ef1-149">Places where errors may occur</span></span>

<span data-ttu-id="90ef1-150">框架和應用程式代碼可能會在以下任何位置觸發未處理的異常:</span><span class="sxs-lookup"><span data-stu-id="90ef1-150">Framework and app code may trigger unhandled exceptions in any of the following locations:</span></span>

* [<span data-ttu-id="90ef1-151">元件實例化</span><span class="sxs-lookup"><span data-stu-id="90ef1-151">Component instantiation</span></span>](#component-instantiation)
* [<span data-ttu-id="90ef1-152">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="90ef1-152">Lifecycle methods</span></span>](#lifecycle-methods)
* [<span data-ttu-id="90ef1-153">渲染邏輯</span><span class="sxs-lookup"><span data-stu-id="90ef1-153">Rendering logic</span></span>](#rendering-logic)
* [<span data-ttu-id="90ef1-154">事件處理常式</span><span class="sxs-lookup"><span data-stu-id="90ef1-154">Event handlers</span></span>](#event-handlers)
* [<span data-ttu-id="90ef1-155">元件處理</span><span class="sxs-lookup"><span data-stu-id="90ef1-155">Component disposal</span></span>](#component-disposal)
* [<span data-ttu-id="90ef1-156">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="90ef1-156">JavaScript interop</span></span>](#javascript-interop)
* <span data-ttu-id="90ef1-157">[Blazor伺服器重新成像](#blazor-server-prerendering)</span><span class="sxs-lookup"><span data-stu-id="90ef1-157">[Blazor Server rerendering](#blazor-server-prerendering)</span></span>

<span data-ttu-id="90ef1-158">本文的以下部分介紹了上述未處理的異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-158">The preceding unhandled exceptions are described in the following sections of this article.</span></span>

### <a name="component-instantiation"></a><span data-ttu-id="90ef1-159">元件實例化</span><span class="sxs-lookup"><span data-stu-id="90ef1-159">Component instantiation</span></span>

<span data-ttu-id="90ef1-160">建立Blazor元件實體時:</span><span class="sxs-lookup"><span data-stu-id="90ef1-160">When Blazor creates an instance of a component:</span></span>

* <span data-ttu-id="90ef1-161">調用元件的構造函數。</span><span class="sxs-lookup"><span data-stu-id="90ef1-161">The component's constructor is invoked.</span></span>
* <span data-ttu-id="90ef1-162">將呼叫透過[`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component)指令或[`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component)屬性提供給元件構造函數的任何非單例 DI 服務的建構函數。</span><span class="sxs-lookup"><span data-stu-id="90ef1-162">The constructors of any non-singleton DI services supplied to the component's constructor via the [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) directive or the [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) attribute are invoked.</span></span>

<span data-ttu-id="90ef1-163">當Blazor任何執行的構造函數或`[Inject]`任何 屬性的設定器引發未處理的異常時,伺服器電路將失敗。</span><span class="sxs-lookup"><span data-stu-id="90ef1-163">A Blazor Server circuit fails when any executed constructor or a setter for any `[Inject]` property throws an unhandled exception.</span></span> <span data-ttu-id="90ef1-164">異常是致命的,因為框架無法實例化元件。</span><span class="sxs-lookup"><span data-stu-id="90ef1-164">The exception is fatal because the framework can't instantiate the component.</span></span> <span data-ttu-id="90ef1-165">如果建構函數邏輯可能會引發異常,則應用應使用具有錯誤處理和日誌記錄[的 try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句捕獲異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-165">If constructor logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

### <a name="lifecycle-methods"></a><span data-ttu-id="90ef1-166">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="90ef1-166">Lifecycle methods</span></span>

<span data-ttu-id="90ef1-167">在元件的生存期內,Blazor呼叫以下[生命週期方法](xref:blazor/lifecycle):</span><span class="sxs-lookup"><span data-stu-id="90ef1-167">During the lifetime of a component, Blazor invokes the following [lifecycle methods](xref:blazor/lifecycle):</span></span>

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

<span data-ttu-id="90ef1-168">如果任何生命週期方法以同步或非同步方式引發異常,則異常對Blazor伺服器電路是致命的。</span><span class="sxs-lookup"><span data-stu-id="90ef1-168">If any lifecycle method throws an exception, synchronously or asynchronously, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="90ef1-169">對於處理生命週期方法中的錯誤的元件,添加錯誤處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="90ef1-169">For components to deal with errors in lifecycle methods, add error handling logic.</span></span>

<span data-ttu-id="90ef1-170">在以下範例中`OnParametersSetAsync`呼叫方法以取得產品:</span><span class="sxs-lookup"><span data-stu-id="90ef1-170">In the following example where `OnParametersSetAsync` calls a method to obtain a product:</span></span>

* <span data-ttu-id="90ef1-171">`ProductRepository.GetProductByIdAsync`方法中引發的異常由`try-catch`語句處理。</span><span class="sxs-lookup"><span data-stu-id="90ef1-171">An exception thrown in the `ProductRepository.GetProductByIdAsync` method is handled by a `try-catch` statement.</span></span>
* <span data-ttu-id="90ef1-172">執行`catch`區塊時:</span><span class="sxs-lookup"><span data-stu-id="90ef1-172">When the `catch` block is executed:</span></span>
  * <span data-ttu-id="90ef1-173">`loadFailed`設置為`true`,用於向使用者顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="90ef1-173">`loadFailed` is set to `true`, which is used to display an error message to the user.</span></span>
  * <span data-ttu-id="90ef1-174">記錄錯誤。</span><span class="sxs-lookup"><span data-stu-id="90ef1-174">The error is logged.</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a><span data-ttu-id="90ef1-175">渲染邏輯</span><span class="sxs-lookup"><span data-stu-id="90ef1-175">Rendering logic</span></span>

<span data-ttu-id="90ef1-176">`.razor`元件檔中的聲明性標記被編譯為稱為`BuildRenderTree`的 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="90ef1-176">The declarative markup in a `.razor` component file is compiled into a C# method called `BuildRenderTree`.</span></span> <span data-ttu-id="90ef1-177">當元件的成像時`BuildRenderTree`, 執行並建譯描述呈現元件的元素、文本和子元件的資料結構。</span><span class="sxs-lookup"><span data-stu-id="90ef1-177">When a component renders, `BuildRenderTree` executes and builds up a data structure describing the elements, text, and child components of the rendered component.</span></span>

<span data-ttu-id="90ef1-178">呈現邏輯可能會引發異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-178">Rendering logic can throw an exception.</span></span> <span data-ttu-id="90ef1-179">此方案的範例在計算時`@someObject.PropertyName`發生,但`@someObject`為`null`。</span><span class="sxs-lookup"><span data-stu-id="90ef1-179">An example of this scenario occurs when `@someObject.PropertyName` is evaluated but `@someObject` is `null`.</span></span> <span data-ttu-id="90ef1-180">呈現邏輯引發的未處理異常對Blazor伺服器電路是致命的。</span><span class="sxs-lookup"><span data-stu-id="90ef1-180">An unhandled exception thrown by rendering logic is fatal to a Blazor Server circuit.</span></span>

<span data-ttu-id="90ef1-181">為了防止呈現邏輯中的空引用異常,請在訪問`null`物件成員之前檢查物件。</span><span class="sxs-lookup"><span data-stu-id="90ef1-181">To prevent a null reference exception in rendering logic, check for a `null` object before accessing its members.</span></span> <span data-ttu-id="90ef1-182">在下面的範例中,`person.Address``person.Address`如果`null`是, 則不存取屬性:</span><span class="sxs-lookup"><span data-stu-id="90ef1-182">In the following example, `person.Address` properties aren't accessed if `person.Address` is `null`:</span></span>

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

<span data-ttu-id="90ef1-183">前面的代碼假定`person`不是`null`。</span><span class="sxs-lookup"><span data-stu-id="90ef1-183">The preceding code assumes that `person` isn't `null`.</span></span> <span data-ttu-id="90ef1-184">通常,代碼的結構保證物件在呈現元件時存在。</span><span class="sxs-lookup"><span data-stu-id="90ef1-184">Often, the structure of the code guarantees that an object exists at the time the component is rendered.</span></span> <span data-ttu-id="90ef1-185">在這些情況下,無需在呈現邏輯中檢查`null`。</span><span class="sxs-lookup"><span data-stu-id="90ef1-185">In those cases, it isn't necessary to check for `null` in rendering logic.</span></span> <span data-ttu-id="90ef1-186">在上一個示例中`person`,可以保證存在,`person`因為 是在實例化元件時創建的。</span><span class="sxs-lookup"><span data-stu-id="90ef1-186">In the prior example, `person` might be guaranteed to exist because `person` is created when the component is instantiated.</span></span>

### <a name="event-handlers"></a><span data-ttu-id="90ef1-187">事件處理常式</span><span class="sxs-lookup"><span data-stu-id="90ef1-187">Event handlers</span></span>

<span data-ttu-id="90ef1-188">使用: 建立事件處理程式時,客戶端代碼觸發 C# 程式的呼叫:</span><span class="sxs-lookup"><span data-stu-id="90ef1-188">Client-side code triggers invocations of C# code when event handlers are created using:</span></span>

* `@onclick`
* `@onchange`
* <span data-ttu-id="90ef1-189">其他`@on...`屬性</span><span class="sxs-lookup"><span data-stu-id="90ef1-189">Other `@on...` attributes</span></span>
* `@bind`

<span data-ttu-id="90ef1-190">在這種情況下,事件處理程式代碼可能會引發未處理的異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-190">Event handler code might throw an unhandled exception in these scenarios.</span></span>

<span data-ttu-id="90ef1-191">如果事件處理程序引發未處理的異常(例如,資料庫查詢失敗),則異常對Blazor伺服器電路是致命的。</span><span class="sxs-lookup"><span data-stu-id="90ef1-191">If an event handler throws an unhandled exception (for example, a database query fails), the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="90ef1-192">如果應用呼叫的代碼可能會由於外部原因失敗,請使用具有錯誤處理和日誌記錄[的 try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句捕獲異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-192">If the app calls code that could fail for external reasons, trap exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="90ef1-193">如果用戶代碼不捕獲和處理異常,則框架將記錄異常並終止電路。</span><span class="sxs-lookup"><span data-stu-id="90ef1-193">If user code doesn't trap and handle the exception, the framework logs the exception and terminates the circuit.</span></span>

### <a name="component-disposal"></a><span data-ttu-id="90ef1-194">元件處理</span><span class="sxs-lookup"><span data-stu-id="90ef1-194">Component disposal</span></span>

<span data-ttu-id="90ef1-195">例如,可以從 UI 中刪除元件,因為使用者已導航到另一個頁面。</span><span class="sxs-lookup"><span data-stu-id="90ef1-195">A component may be removed from the UI, for example, because the user has navigated to another page.</span></span> <span data-ttu-id="90ef1-196">從 UI<xref:System.IDisposable?displayProperty=fullName>中刪除 實現元件時,框架將調用該<xref:System.IDisposable.Dispose*>元件的方法。</span><span class="sxs-lookup"><span data-stu-id="90ef1-196">When a component that implements <xref:System.IDisposable?displayProperty=fullName> is removed from the UI, the framework calls the component's <xref:System.IDisposable.Dispose*> method.</span></span>

<span data-ttu-id="90ef1-197">如果元件`Dispose`的方法引發未處理的異常,則異常對Blazor伺服器電路是致命的。</span><span class="sxs-lookup"><span data-stu-id="90ef1-197">If the component's `Dispose` method throws an unhandled exception, the exception is fatal to a Blazor Server circuit.</span></span> <span data-ttu-id="90ef1-198">如果處置邏輯可能會引發異常,則應用應使用具有錯誤處理和日誌記錄[的 try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句捕獲異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-198">If disposal logic may throw exceptions, the app should trap the exceptions using a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span>

<span data-ttu-id="90ef1-199">有關元件處理的詳細資訊,請參閱<xref:blazor/lifecycle#component-disposal-with-idisposable>。</span><span class="sxs-lookup"><span data-stu-id="90ef1-199">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>

### <a name="javascript-interop"></a><span data-ttu-id="90ef1-200">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="90ef1-200">JavaScript interop</span></span>

<span data-ttu-id="90ef1-201">`IJSRuntime.InvokeAsync<T>`允許 .NET 代碼在使用者的瀏覽器中對 JavaScript 運行時進行非同步調用。</span><span class="sxs-lookup"><span data-stu-id="90ef1-201">`IJSRuntime.InvokeAsync<T>` allows .NET code to make asynchronous calls to the JavaScript runtime in the user's browser.</span></span>

<span data-ttu-id="90ef1-202">以下條件適用於 使用的`InvokeAsync<T>`錯誤 處理:</span><span class="sxs-lookup"><span data-stu-id="90ef1-202">The following conditions apply to error handling with `InvokeAsync<T>`:</span></span>

* <span data-ttu-id="90ef1-203">如果調用`InvokeAsync<T>`同步失敗,則會發生 .NET 異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-203">If a call to `InvokeAsync<T>` fails synchronously, a .NET exception occurs.</span></span> <span data-ttu-id="90ef1-204">例如,調用`InvokeAsync<T>`可能會失敗,因為提供的參數無法序列化。</span><span class="sxs-lookup"><span data-stu-id="90ef1-204">A call to `InvokeAsync<T>` may fail, for example, because the supplied arguments can't be serialized.</span></span> <span data-ttu-id="90ef1-205">開發人員代碼必須捕獲異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-205">Developer code must catch the exception.</span></span> <span data-ttu-id="90ef1-206">如果事件處理程式或元件生命週期方法中的應用代碼不處理異常,則生成的異常對Blazor伺服器電路是致命的。</span><span class="sxs-lookup"><span data-stu-id="90ef1-206">If app code in an event handler or component lifecycle method doesn't handle an exception, the resulting exception is fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="90ef1-207">如果調用`InvokeAsync<T>`以非同步方式失敗,則<xref:System.Threading.Tasks.Task>.NET 將失敗。</span><span class="sxs-lookup"><span data-stu-id="90ef1-207">If a call to `InvokeAsync<T>` fails asynchronously, the .NET <xref:System.Threading.Tasks.Task> fails.</span></span> <span data-ttu-id="90ef1-208">呼叫`InvokeAsync<T>`可能會失敗,例如,因為 JavaScript 連接程式碼引發`Promise``rejected`異常或傳回作為 完成的 。</span><span class="sxs-lookup"><span data-stu-id="90ef1-208">A call to `InvokeAsync<T>` may fail, for example, because the JavaScript-side code throws an exception or returns a `Promise` that completed as `rejected`.</span></span> <span data-ttu-id="90ef1-209">開發人員代碼必須捕獲異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-209">Developer code must catch the exception.</span></span> <span data-ttu-id="90ef1-210">如果使用[await](/dotnet/csharp/language-reference/keywords/await)運算符,請考慮在[嘗試 catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中包裝方法調用,並處理錯誤並記錄。</span><span class="sxs-lookup"><span data-stu-id="90ef1-210">If using the [await](/dotnet/csharp/language-reference/keywords/await) operator, consider wrapping the method call in a [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statement with error handling and logging.</span></span> <span data-ttu-id="90ef1-211">否則,故障代碼會導致未處理的異常,該異常對Blazor伺服器電路造成致命。</span><span class="sxs-lookup"><span data-stu-id="90ef1-211">Otherwise, the failing code results in an unhandled exception that's fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="90ef1-212">默認情況下,調用`InvokeAsync<T>`必須在一定時間段內完成,否則呼叫超時。預設超時週期為一分鐘。</span><span class="sxs-lookup"><span data-stu-id="90ef1-212">By default, calls to `InvokeAsync<T>` must complete within a certain period or else the call times out. The default timeout period is one minute.</span></span> <span data-ttu-id="90ef1-213">超時可保護程式碼免受網路連接丟失或從未發送回完成消息的 JavaScript 代碼的丟失。</span><span class="sxs-lookup"><span data-stu-id="90ef1-213">The timeout protects the code against a loss in network connectivity or JavaScript code that never sends back a completion message.</span></span> <span data-ttu-id="90ef1-214">如果呼叫逾時,則產生的`Task`失敗與<xref:System.OperationCanceledException>。</span><span class="sxs-lookup"><span data-stu-id="90ef1-214">If the call times out, the resulting `Task` fails with an <xref:System.OperationCanceledException>.</span></span> <span data-ttu-id="90ef1-215">捕獲和處理日誌記錄的異常。</span><span class="sxs-lookup"><span data-stu-id="90ef1-215">Trap and process the exception with logging.</span></span>

<span data-ttu-id="90ef1-216">同樣,JavaScript 代碼可以啟動對屬性指示的[`[JSInvokable]`](xref:blazor/call-dotnet-from-javascript).NET 方法的調用。</span><span class="sxs-lookup"><span data-stu-id="90ef1-216">Similarly, JavaScript code may initiate calls to .NET methods indicated by the [`[JSInvokable]`](xref:blazor/call-dotnet-from-javascript) attribute.</span></span> <span data-ttu-id="90ef1-217">如果這些 .NET 方法引發未處理的異常:</span><span class="sxs-lookup"><span data-stu-id="90ef1-217">If these .NET methods throw an unhandled exception:</span></span>

* <span data-ttu-id="90ef1-218">異常不被視為Blazor對伺服器電路致命。</span><span class="sxs-lookup"><span data-stu-id="90ef1-218">The exception isn't treated as fatal to a Blazor Server circuit.</span></span>
* <span data-ttu-id="90ef1-219">JavaScript`Promise`端被拒絕。</span><span class="sxs-lookup"><span data-stu-id="90ef1-219">The JavaScript-side `Promise` is rejected.</span></span>

<span data-ttu-id="90ef1-220">您可以選擇在方法呼叫的 .NET 端或 JavaScript 端使用錯誤處理代碼。</span><span class="sxs-lookup"><span data-stu-id="90ef1-220">You have the option of using error handling code on either the .NET side or the JavaScript side of the method call.</span></span>

<span data-ttu-id="90ef1-221">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="90ef1-221">For more information, see the following articles:</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

### <a name="opno-locblazor-server-prerendering"></a>Blazor<span data-ttu-id="90ef1-222">伺服器預算</span><span class="sxs-lookup"><span data-stu-id="90ef1-222"> Server prerendering</span></span>

Blazor<span data-ttu-id="90ef1-223">可以使用[元件標記説明程式](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)預呈現元件,以便將其呈現的 HTML 標記作為使用者初始 HTTP 請求的一部分返回。</span><span class="sxs-lookup"><span data-stu-id="90ef1-223"> components can be prerendered using the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper) so that their rendered HTML markup is returned as part of the user's initial HTTP request.</span></span> <span data-ttu-id="90ef1-224">其工作方式為:</span><span class="sxs-lookup"><span data-stu-id="90ef1-224">This works by:</span></span>

* <span data-ttu-id="90ef1-225">為屬於同一頁面的所有預渲染元件創建新電路。</span><span class="sxs-lookup"><span data-stu-id="90ef1-225">Creating a new circuit for all of the prerendered components that are part of the same page.</span></span>
* <span data-ttu-id="90ef1-226">生成初始 HTML。</span><span class="sxs-lookup"><span data-stu-id="90ef1-226">Generating the initial HTML.</span></span>
* <span data-ttu-id="90ef1-227">將電路視為`disconnected`在用戶的瀏覽器建立回同SignalR一伺服器的連接之前。</span><span class="sxs-lookup"><span data-stu-id="90ef1-227">Treating the circuit as `disconnected` until the user's browser establishes a SignalR connection back to the same server.</span></span> <span data-ttu-id="90ef1-228">建立連接後,電路上恢復互動性,並更新元件的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="90ef1-228">When the connection is established, interactivity on the circuit is resumed and the components' HTML markup is updated.</span></span>

<span data-ttu-id="90ef1-229">如果任何元件在預成成週期期間(例如,在生命週期方法期間或在呈現邏輯中)引發未處理的異常:</span><span class="sxs-lookup"><span data-stu-id="90ef1-229">If any component throws an unhandled exception during prerendering, for example, during a lifecycle method or in rendering logic:</span></span>

* <span data-ttu-id="90ef1-230">異常對電路是致命的。</span><span class="sxs-lookup"><span data-stu-id="90ef1-230">The exception is fatal to the circuit.</span></span>
* <span data-ttu-id="90ef1-231">異常從`Component`標記説明程序引發調用堆疊。</span><span class="sxs-lookup"><span data-stu-id="90ef1-231">The exception is thrown up the call stack from the `Component` Tag Helper.</span></span> <span data-ttu-id="90ef1-232">因此,除非開發人員代碼顯式捕獲異常,否則整個 HTTP 請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="90ef1-232">Therefore, the entire HTTP request fails unless the exception is explicitly caught by developer code.</span></span>

<span data-ttu-id="90ef1-233">在正常情況下,當預渲染失敗時,繼續生成和呈現元件沒有意義,因為無法呈現工作元件。</span><span class="sxs-lookup"><span data-stu-id="90ef1-233">Under normal circumstances when prerendering fails, continuing to build and render the component doesn't make sense because a working component can't be rendered.</span></span>

<span data-ttu-id="90ef1-234">為了容忍預渲染期間可能發生的錯誤,錯誤處理邏輯必須放置在可能引發異常的元件中。</span><span class="sxs-lookup"><span data-stu-id="90ef1-234">To tolerate errors that may occur during prerendering, error handling logic must be placed inside a component that may throw exceptions.</span></span> <span data-ttu-id="90ef1-235">將[嘗試捕獲](/dotnet/csharp/language-reference/keywords/try-catch)語句與錯誤處理和日誌記錄一起使用。</span><span class="sxs-lookup"><span data-stu-id="90ef1-235">Use [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span> <span data-ttu-id="90ef1-236">不要將`Component`標記説明程式包裝`try-catch`在 語句中,`Component`而是在標記説明程序呈現的元件中放置錯誤處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="90ef1-236">Instead of wrapping the `Component` Tag Helper in a `try-catch` statement, place error handling logic in the component rendered by the `Component` Tag Helper.</span></span>

## <a name="advanced-scenarios"></a><span data-ttu-id="90ef1-237">進階案例</span><span class="sxs-lookup"><span data-stu-id="90ef1-237">Advanced scenarios</span></span>

### <a name="recursive-rendering"></a><span data-ttu-id="90ef1-238">遞迴成</span><span class="sxs-lookup"><span data-stu-id="90ef1-238">Recursive rendering</span></span>

<span data-ttu-id="90ef1-239">元件可以遞迴嵌套。</span><span class="sxs-lookup"><span data-stu-id="90ef1-239">Components can be nested recursively.</span></span> <span data-ttu-id="90ef1-240">這對於表示遞歸數據結構很有用。</span><span class="sxs-lookup"><span data-stu-id="90ef1-240">This is useful for representing recursive data structures.</span></span> <span data-ttu-id="90ef1-241">例如,元件`TreeNode`可以為每個節點的子`TreeNode`節點呈現更多元件。</span><span class="sxs-lookup"><span data-stu-id="90ef1-241">For example, a `TreeNode` component can render more `TreeNode` components for each of the node's children.</span></span>

<span data-ttu-id="90ef1-242">遞迴時,避免編碼模式導致無限遞歸:</span><span class="sxs-lookup"><span data-stu-id="90ef1-242">When rendering recursively, avoid coding patterns that result in infinite recursion:</span></span>

* <span data-ttu-id="90ef1-243">不要遞歸地呈現包含週期的數據結構。</span><span class="sxs-lookup"><span data-stu-id="90ef1-243">Don't recursively render a data structure that contains a cycle.</span></span> <span data-ttu-id="90ef1-244">例如,不要呈現其子節點包括自身的樹節點。</span><span class="sxs-lookup"><span data-stu-id="90ef1-244">For example, don't render a tree node whose children includes itself.</span></span>
* <span data-ttu-id="90ef1-245">不要創建包含迴圈的佈局鏈。</span><span class="sxs-lookup"><span data-stu-id="90ef1-245">Don't create a chain of layouts that contain a cycle.</span></span> <span data-ttu-id="90ef1-246">例如,不要創建佈局本身的佈局。</span><span class="sxs-lookup"><span data-stu-id="90ef1-246">For example, don't create a layout whose layout is itself.</span></span>
* <span data-ttu-id="90ef1-247">不允許最終用戶通過惡意數據輸入或 JAVAScript 互通調用違反遞歸不變(規則)。</span><span class="sxs-lookup"><span data-stu-id="90ef1-247">Don't allow an end user to violate recursion invariants (rules) through malicious data entry or JavaScript interop calls.</span></span>

<span data-ttu-id="90ef1-248">成像過程中的無限迴圈:</span><span class="sxs-lookup"><span data-stu-id="90ef1-248">Infinite loops during rendering:</span></span>

* <span data-ttu-id="90ef1-249">使渲染過程永久繼續。</span><span class="sxs-lookup"><span data-stu-id="90ef1-249">Causes the rendering process to continue forever.</span></span>
* <span data-ttu-id="90ef1-250">等效於創建未終止的迴圈。</span><span class="sxs-lookup"><span data-stu-id="90ef1-250">Is equivalent to creating an unterminated loop.</span></span>

<span data-ttu-id="90ef1-251">在這些情況下,受影響的Blazor伺服器電路失敗,並且線程通常嘗試:</span><span class="sxs-lookup"><span data-stu-id="90ef1-251">In these scenarios, an affected Blazor Server circuit fails, and the thread usually attempts to:</span></span>

* <span data-ttu-id="90ef1-252">無限期地消耗操作系統允許的 CPU 時間。</span><span class="sxs-lookup"><span data-stu-id="90ef1-252">Consume as much CPU time as permitted by the operating system, indefinitely.</span></span>
* <span data-ttu-id="90ef1-253">消耗無限量的伺服器記憶體。</span><span class="sxs-lookup"><span data-stu-id="90ef1-253">Consume an unlimited amount of server memory.</span></span> <span data-ttu-id="90ef1-254">使用無限記憶體等效於未終止迴圈在每次反覆運算時將條目添加到集合的情況。</span><span class="sxs-lookup"><span data-stu-id="90ef1-254">Consuming unlimited memory is equivalent to the scenario where an unterminated loop adds entries to a collection on every iteration.</span></span>

<span data-ttu-id="90ef1-255">為了避免無限遞歸模式,請確保遞歸呈現代碼包含適當的停止條件。</span><span class="sxs-lookup"><span data-stu-id="90ef1-255">To avoid infinite recursion patterns, ensure that recursive rendering code contains suitable stopping conditions.</span></span>

### <a name="custom-render-tree-logic"></a><span data-ttu-id="90ef1-256">自訂成圖邏輯</span><span class="sxs-lookup"><span data-stu-id="90ef1-256">Custom render tree logic</span></span>

<span data-ttu-id="90ef1-257">大多數Blazor元件都實現為 *.razor*檔,並編譯以生成在`RenderTreeBuilder`上操作 以呈現其輸出的邏輯。</span><span class="sxs-lookup"><span data-stu-id="90ef1-257">Most Blazor components are implemented as *.razor* files and are compiled to produce logic that operates on a `RenderTreeBuilder` to render their output.</span></span> <span data-ttu-id="90ef1-258">開發人員可以使用過程 C#`RenderTreeBuilder`程式碼手動實現邏輯。</span><span class="sxs-lookup"><span data-stu-id="90ef1-258">A developer may manually implement `RenderTreeBuilder` logic using procedural C# code.</span></span> <span data-ttu-id="90ef1-259">如需詳細資訊，請參閱 <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>。</span><span class="sxs-lookup"><span data-stu-id="90ef1-259">For more information, see <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>.</span></span>

> [!WARNING]
> <span data-ttu-id="90ef1-260">使用手動呈現樹生成器邏輯被視為高級和不安全方案,不建議用於一般元件開發。</span><span class="sxs-lookup"><span data-stu-id="90ef1-260">Use of manual render tree builder logic is considered an advanced and unsafe scenario, not recommended for general component development.</span></span>

<span data-ttu-id="90ef1-261">如果`RenderTreeBuilder`編寫了代碼,開發人員必須保證代碼的正確性。</span><span class="sxs-lookup"><span data-stu-id="90ef1-261">If `RenderTreeBuilder` code is written, the developer must guarantee the correctness of the code.</span></span> <span data-ttu-id="90ef1-262">例如,開發人員必須確保:</span><span class="sxs-lookup"><span data-stu-id="90ef1-262">For example, the developer must ensure that:</span></span>

* <span data-ttu-id="90ef1-263">調用`OpenElement``CloseElement`和 的呼叫正確平衡。</span><span class="sxs-lookup"><span data-stu-id="90ef1-263">Calls to `OpenElement` and `CloseElement` are correctly balanced.</span></span>
* <span data-ttu-id="90ef1-264">屬性僅添加到正確的位置。</span><span class="sxs-lookup"><span data-stu-id="90ef1-264">Attributes are only added in the correct places.</span></span>

<span data-ttu-id="90ef1-265">不正確的手動呈現樹生成器邏輯可能會導致任意未定義的行為,包括崩潰、伺服器掛起和安全漏洞。</span><span class="sxs-lookup"><span data-stu-id="90ef1-265">Incorrect manual render tree builder logic can cause arbitrary undefined behavior, including crashes, server hangs, and security vulnerabilities.</span></span>

<span data-ttu-id="90ef1-266">請考慮手動渲染具有相同複雜性級別的樹生成器邏輯,其*危險*級別與手動編寫程式集代碼或 MSIL 指令的風險級別相同。</span><span class="sxs-lookup"><span data-stu-id="90ef1-266">Consider manual render tree builder logic on the same level of complexity and with the same level of *danger* as writing assembly code or MSIL instructions by hand.</span></span>
