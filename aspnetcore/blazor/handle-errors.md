---
title: 處理 ASP.NET Core Blazor應用程式中的錯誤
author: guardrex
description: 探索如何 ASP.NET Core Blazor如何Blazor管理未處理的例外狀況，以及如何開發偵測和處理錯誤的應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: 63bb791958785fa9a4a676f1aab79126c6873068
ms.sourcegitcommit: 7bb14d005155a5044c7902a08694ee8ccb20c113
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82111041"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a>處理 ASP.NET Core Blazor應用程式中的錯誤

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

本文說明如何Blazor管理未處理的例外狀況，以及如何開發偵測和處理錯誤的應用程式。

## <a name="detailed-errors-during-development"></a>開發期間的詳細錯誤

當Blazor應用程式在開發期間無法正常運作時，從應用程式接收詳細的錯誤資訊有助於疑難排解和修正問題。 發生錯誤時， Blazor應用程式會在畫面底部顯示 [金色] 橫條：

* 在開發期間，「金級」列會將您導向至瀏覽器主控台，您可以在其中看到例外狀況。
* 在生產環境中，「金級」列會通知使用者發生錯誤，並建議重新整理瀏覽器。

此錯誤處理體驗的 UI 是Blazor專案範本的一部分。

在Blazor WebAssembly 應用程式中，自訂*wwwroot/index.html*檔案中的體驗：

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

在Blazor伺服器應用程式中，自訂*Pages/_Host. cshtml*檔案中的體驗：

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

`blazor-error-ui`專案是由Blazor範本中包含的樣式（*wwwroot/css/.css*）所隱藏，然後在發生錯誤時顯示：

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

## <a name="how-a-opno-locblazor-server-app-reacts-to-unhandled-exceptions"></a>Blazor伺服器應用程式如何回應未處理的例外狀況

Blazor伺服器是可設定狀態的架構。 當使用者與應用程式互動時，它們會維持與伺服器的連線，稱為「*線路*」。 線路包含作用中的元件實例，以及其他許多狀態層面，例如：

* 最新呈現的元件輸出。
* 可由用戶端事件觸發的目前事件處理委派集合。

如果使用者在多個瀏覽器索引標籤中開啟應用程式，則會有多個獨立線路。

Blazor將大部分未處理的例外狀況視為發生的嚴重錯誤。 如果線路因未處理的例外狀況而終止，則使用者只需重載頁面來建立新的線路，即可繼續與應用程式互動。 在終止的電路以外的線路（也就是其他使用者或其他瀏覽器索引標籤的線路）不受影響。 此案例類似于將損毀&mdash;的應用程式損毀的桌面應用程式，必須重新開機，但不會影響其他應用程式。

當未處理的例外狀況發生時，會終止電路，原因如下：

* 未處理的例外狀況通常會使線路處於未定義的狀態。
* 無法在未處理的例外狀況之後保證應用程式的一般作業。
* 如果線路繼續進行，應用程式中可能會出現安全性弱點。

## <a name="manage-unhandled-exceptions-in-developer-code"></a>在開發人員程式碼中管理未處理的例外狀況

若要讓應用程式在發生錯誤後繼續，應用程式必須有錯誤處理邏輯。 本文稍後的章節會描述未處理之例外狀況的可能來源。

在生產環境中，請勿轉譯架構例外狀況訊息，或在 UI 中呈現堆疊追蹤。 呈現例外狀況訊息或堆疊追蹤可能：

* 向終端使用者公開機密資訊。
* 協助惡意使用者探索應用程式中可能會危害應用程式、伺服器或網路安全性的弱點。

## <a name="log-errors-with-a-persistent-provider"></a>使用持續性提供者記錄錯誤

如果發生未處理的例外狀況，則會將<xref:Microsoft.Extensions.Logging.ILogger>例外狀況記錄到服務容器中設定的實例。 根據預設， Blazor應用程式會使用主控台記錄提供者來記錄主控台輸出。 請考慮使用可記錄管理大小和記錄輪替的提供者，記錄到更永久的位置。 如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。

在開發期間Blazor ，通常會將例外狀況的完整詳細資料傳送至瀏覽器的主控台，以協助進行偵錯工具。 在生產環境中，瀏覽器主控台中的詳細錯誤預設為停用，這表示錯誤不會傳送至用戶端，但例外狀況的完整詳細資料仍會記錄在伺服器端。 如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。

您必須決定要記錄哪些事件，以及記錄事件的嚴重性層級。 惡意的使用者可能可以故意觸發錯誤。 例如，請勿從顯示產品詳細資料之元件的 URL 中`ProductId`提供不明的錯誤中記錄事件。 並非所有錯誤都應該視為高嚴重性事件以進行記錄。

如需詳細資訊，請參閱 <xref:fundamentals/logging/index#create-logs-in-blazor>。

## <a name="places-where-errors-may-occur"></a>可能發生錯誤的位置

架構和應用程式程式碼可能會在下列任何位置觸發未處理的例外狀況：

* [元件具現化](#component-instantiation)
* [生命週期方法](#lifecycle-methods)
* [呈現邏輯](#rendering-logic)
* [事件處理常式](#event-handlers)
* [元件處置](#component-disposal)
* [JavaScript Interop](#javascript-interop)
* [Blazor伺服器 rerendering](#blazor-server-prerendering)

前述未處理的例外狀況會在本文的下列各節中說明。

### <a name="component-instantiation"></a>元件具現化

當Blazor建立元件的實例時：

* 會叫用元件的函式。
* 系統會叫用透過指示詞或[`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component)屬性提供給元件之函式的任何非 singleton DI 服務的函式。

當Blazor任何執行的函式或任何`[Inject]`屬性的 setter 擲回未處理的例外狀況時，伺服器線路就會失敗。 例外狀況是嚴重的，因為架構無法具現化元件。 如果函式邏輯可能會擲回例外狀況，則應用程式應該使用具有錯誤處理和記錄功能的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來捕捉例外狀況。

### <a name="lifecycle-methods"></a>生命週期方法

在元件的存留期間， Blazor會叫用下列[生命週期方法](xref:blazor/lifecycle)：

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

如果任何生命週期方法以同步或非同步方式擲回例外狀況，則例外狀況Blazor對伺服器線路而言是嚴重的。 若要讓元件處理生命週期方法中的錯誤，請新增錯誤處理邏輯。

在下列範例中， `OnParametersSetAsync`會呼叫方法來取得產品：

* `ProductRepository.GetProductByIdAsync`方法中擲回的例外狀況是由`try-catch`語句處理。
* 執行`catch`區塊時：
  * `loadFailed`設定為`true`，用來向使用者顯示錯誤訊息。
  * 會記錄錯誤。

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>呈現邏輯

`.razor`元件檔案中的宣告式標記會編譯成稱為`BuildRenderTree`的 c # 方法。 當元件轉譯時， `BuildRenderTree`會執行並建立資料結構，以描述所轉譯元件的元素、文字和子元件。

轉譯邏輯可能會擲回例外狀況。 當`@someObject.PropertyName`評估但`@someObject`為`null`時，就會發生此案例的範例。 轉譯邏輯所擲回的未處理例外狀況， Blazor對伺服器線路而言是嚴重的。

若要避免轉譯邏輯中出現 null 參考例外狀況，請`null`先檢查物件，然後再存取其成員。 在下列範例中， `person.Address`如果`person.Address`是，則不`null`會存取屬性：

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

上述程式`person`代碼假設不是`null`。 通常，程式碼的結構會保證在呈現元件時，物件存在。 在這些情況下，不需要檢查`null`呈現邏輯。 在先前的範例中`person` ，可能會保證存在， `person`因為會在元件具現化時建立。

### <a name="event-handlers"></a>事件處理常式

當使用建立事件處理常式時，用戶端程式代碼會觸發 c # 程式碼的調用：

* `@onclick`
* `@onchange`
* 其他`@on...`屬性
* `@bind`

在這些情況下，事件處理常式程式碼可能會擲回未處理的例外狀況。

如果事件處理常式擲回未處理的例外狀況（例如，資料庫查詢失敗），則例外狀況對Blazor伺服器線路而言是嚴重的。 如果應用程式呼叫可能因外部原因而失敗的程式碼，請使用具有錯誤處理和記錄的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來設陷例外狀況。

如果使用者程式碼不會陷印並處理例外狀況，則架構會記錄例外狀況並終止線路。

### <a name="component-disposal"></a>元件處置

例如，元件可能會從 UI 移除，因為使用者已流覽至另一個頁面。 當執行的元件從<xref:System.IDisposable?displayProperty=fullName> UI 中移除時，架構會呼叫元件的<xref:System.IDisposable.Dispose*>方法。

如果元件的`Dispose`方法擲回未處理的例外狀況，則例外狀況對Blazor伺服器線路而言是嚴重的。 如果處置邏輯可能會擲回例外狀況，則應用程式應該使用具有錯誤處理和記錄的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來捕捉例外狀況。

如需有關元件處置的詳細資訊<xref:blazor/lifecycle#component-disposal-with-idisposable>，請參閱。

### <a name="javascript-interop"></a>JavaScript Interop

`IJSRuntime.InvokeAsync<T>`允許 .NET 程式碼在使用者的瀏覽器中對 JavaScript 執行時間進行非同步呼叫。

下列條件適用于使用`InvokeAsync<T>`的錯誤處理：

* 如果對的呼叫`InvokeAsync<T>`同步失敗，就會發生 .net 例外狀況。 例如，對`InvokeAsync<T>`的呼叫可能會失敗，因為無法序列化提供的引數。 開發人員程式碼必須攔截例外狀況。 如果事件處理常式或元件生命週期方法中的應用程式程式碼不會處理例外狀況，則產生Blazor的例外狀況對伺服器線路而言是嚴重的。
* 如果的呼叫非同步`InvokeAsync<T>`失敗，.net <xref:System.Threading.Tasks.Task>就會失敗。 例如，對`InvokeAsync<T>`的呼叫可能會失敗，因為 JavaScript 端程式碼會擲回例外狀況，或`Promise`傳回以形式`rejected`完成的。 開發人員程式碼必須攔截例外狀況。 如果使用[await](/dotnet/csharp/language-reference/keywords/await)運算子，請考慮將方法呼叫包裝在含有錯誤處理和記錄的[try catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。 否則，失敗的程式碼會導致Blazor伺服器線路嚴重的未處理例外狀況。
* 根據預設，的呼叫`InvokeAsync<T>`必須在特定期間內完成，否則呼叫會超時。預設的超時時間為一分鐘。 Timeout 會保護程式碼不會遺失網路連線，或永遠不會傳回完成訊息的 JavaScript 程式碼。 如果呼叫超時，則產生`Task`的會失敗並出現<xref:System.OperationCanceledException>。 使用記錄來設陷並處理例外狀況。

同樣地，JavaScript 程式碼可能會起始對[`[JSInvokable]`](xref:blazor/call-dotnet-from-javascript)屬性所指示之 .net 方法的呼叫。 如果這些 .NET 方法擲回未處理的例外狀況：

* 此例外狀況不會被視為Blazor伺服器迴圈的嚴重錯誤。
* JavaScript 端`Promise`遭到拒絕。

您可以選擇在 .NET 端或方法呼叫的 JavaScript 端使用錯誤處理常式代碼。

如需詳細資訊，請參閱下列文章：

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

### <a name="opno-locblazor-server-prerendering"></a>Blazor伺服器已預呈現

Blazor元件可以使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式來資源清單，使其呈現的 HTML 標籤會當做使用者初始 HTTP 要求的一部分傳回。 其運作方式如下：

* 針對屬於相同頁面的所有資源清單元件建立新的電路。
* 產生初始 HTML。
* 將線路視為， `disconnected`直到使用者的瀏覽器建立回到SignalR相同伺服器的連接為止。 建立連線時，線路上的互動會繼續，而且元件的 HTML 標籤也會更新。

如果任何元件在進行預入期間擲回未處理的例外狀況（例如，在生命週期方法或轉譯邏輯期間）：

* 這是電路的嚴重例外狀況。
* 例外狀況會從`Component`標記協助程式中擲回呼叫堆疊。 因此，除非開發人員程式碼明確攔截到例外狀況，否則整個 HTTP 要求都會失敗。

在一般情況下，如果無法轉譯，則繼續建立並轉譯元件並沒有意義，因為無法轉譯運作中的元件。

若要容忍在自動處理期間可能發生的錯誤，錯誤處理邏輯必須放在可能會擲回例外狀況的元件內部。 使用[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句搭配錯誤處理和記錄。 不要將`Component`標記協助套裝程式裝在`try-catch`語句中，而是將錯誤處理邏輯放在`Component`標記協助程式所轉譯的元件中。

## <a name="advanced-scenarios"></a>進階案例

### <a name="recursive-rendering"></a>遞迴轉譯

元件可以遞迴方式進行嵌套。 這適用于表示遞迴資料結構。 例如， `TreeNode`元件可以呈現每個節點`TreeNode`子系的更多元件。

以遞迴方式轉譯時，避免產生無限遞迴的編碼模式：

* 不要以遞迴方式呈現包含迴圈的資料結構。 例如，不要轉譯其子系包含其本身的樹狀節點。
* 請勿建立包含迴圈的版面配置鏈。 例如，請勿建立版面配置本身的版面配置。
* 不允許終端使用者透過惡意資料輸入或 JavaScript interop 呼叫來違反遞迴不變數（規則）。

呈現期間的無限迴圈：

* 導致轉譯進程永遠繼續。
* 相當於建立未結束的迴圈。

在這些情況下，受Blazor影響的伺服器線路會失敗，而執行緒通常會嘗試：

* 會無限期地耗用作業系統所允許的 CPU 時間量。
* 耗用不限數量的伺服器記憶體。 使用無限制的記憶體相當於未結束的迴圈在每次反覆運算時將專案新增至集合的案例。

若要避免無限遞迴模式，請確定遞迴呈現程式碼包含適當的停止條件。

### <a name="custom-render-tree-logic"></a>自訂呈現樹狀結構邏輯

大部分Blazor的元件會實作為*razor*檔案，並且會進行編譯，以產生在上`RenderTreeBuilder`操作以轉譯其輸出的邏輯。 開發人員可以使用程式`RenderTreeBuilder` c # 程式碼手動執行邏輯。 如需詳細資訊，請參閱 <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>。

> [!WARNING]
> 手動轉譯樹狀結構產生器邏輯的使用會被視為先進且不安全的案例，不建議用於一般元件開發。

撰寫`RenderTreeBuilder`程式碼時，開發人員必須保證程式碼的正確性。 例如，開發人員必須確定：

* 對`OpenElement`和`CloseElement`的呼叫已正確平衡。
* 屬性只會加入正確的位置。

不正確的手動轉譯樹狀結構產生器邏輯可能會造成任意未定義的行為，包括損毀、伺服器停止回應和安全性弱點。

請考慮以手動方式呈現相同的複雜性層級的轉譯樹產生器邏輯，以及使用與手動撰寫元件程式碼或 MSIL 指令相同層級的*危險*。
