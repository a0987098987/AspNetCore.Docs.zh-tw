---
title: 處理 ASP.NET Core Blazor 應用程式中的錯誤
author: guardrex
description: 探索 ASP.NET Core 如何 Blazor Blazor 如何管理未處理的例外狀況，以及如何開發偵測和處理錯誤的應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/handle-errors
ms.openlocfilehash: fb4c7cacfe8be2417d6009cfc722595d0d91d530
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288842"
---
# <a name="handle-errors-in-aspnet-core-blazor-apps"></a>處理 ASP.NET Core Blazor 應用程式中的錯誤

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

本文說明 Blazor 如何管理未處理的例外狀況，以及如何開發偵測和處理錯誤的應用程式。

## <a name="how-the-blazor-framework-reacts-to-unhandled-exceptions"></a>Blazor 架構如何回應未處理的例外狀況

Blazor 伺服器是可設定狀態的架構。 當使用者與應用程式互動時，它們會維持與伺服器的連線，稱為「*線路*」。 線路包含作用中的元件實例，以及其他許多狀態層面，例如：

* 最新呈現的元件輸出。
* 可由用戶端事件觸發的目前事件處理委派集合。

如果使用者在多個瀏覽器索引標籤中開啟應用程式，則會有多個獨立線路。

Blazor 會將大部分未處理的例外狀況視為其發生所在之線路的嚴重問題。 如果線路因未處理的例外狀況而終止，則使用者只需重載頁面來建立新的線路，即可繼續與應用程式互動。 在終止的電路以外的線路（也就是其他使用者或其他瀏覽器索引標籤的線路）不受影響。 此案例類似于桌面應用程式，因為當機的 @ no__t 0the 損毀應用程式必須重新開機，但其他應用程式不會受到影響。

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

如果發生未處理的例外狀況，則會將例外狀況記錄到服務容器中所設定 @no__t 0 實例。 根據預設，Blazor apps 會使用主控台記錄提供者來記錄至主控台輸出。 請考慮使用可記錄管理大小和記錄輪替的提供者，記錄到更永久的位置。 如需詳細資訊，請參閱<xref:fundamentals/logging/index>。

在開發期間，Blazor 通常會將例外狀況的完整詳細資料傳送至瀏覽器的主控台，以協助進行偵錯工具。 在生產環境中，瀏覽器主控台中的詳細錯誤預設為停用，這表示錯誤不會傳送至用戶端，但例外狀況的完整詳細資料仍會記錄在伺服器端。 如需詳細資訊，請參閱<xref:fundamentals/error-handling>。

您必須決定要記錄哪些事件，以及記錄事件的嚴重性層級。 惡意的使用者可能可以故意觸發錯誤。 例如，請勿從錯誤中記錄事件，其中會在顯示產品詳細資料的元件 URL 中提供未知的 `ProductId`。 並非所有錯誤都應該視為高嚴重性事件以進行記錄。

## <a name="places-where-errors-may-occur"></a>可能發生錯誤的位置

架構和應用程式程式碼可能會在下列任何位置觸發未處理的例外狀況：

* [元件具現化](#component-instantiation)
* [生命週期方法](#lifecycle-methods)
* [呈現邏輯](#rendering-logic)
* [事件處理常式](#event-handlers)
* [元件處置](#component-disposal)
* [JavaScript interop](#javascript-interop)
* [線路處理常式](#circuit-handlers)
* [線路處置](#circuit-disposal)
* [呈現](#prerendering)

前述未處理的例外狀況會在本文的下列各節中說明。

### <a name="component-instantiation"></a>元件具現化

當 Blazor 建立元件的實例時：

* 會叫用元件的函式。
* 系統會叫用透過[@inject](xref:blazor/dependency-injection#request-a-service-in-a-component)指示詞或[[插入]](xref:blazor/dependency-injection#request-a-service-in-a-component)屬性提供給元件之函式的任何非 singleton DI 服務的函式。 

當任何 `[Inject]` 屬性的任何執行的函式或 setter 擲回未處理的例外狀況時，電路就會失敗。 例外狀況是嚴重的，因為架構無法具現化元件。 如果函式邏輯可能會擲回例外狀況，則應用程式應該使用具有錯誤處理和記錄功能的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來捕捉例外狀況。

### <a name="lifecycle-methods"></a>生命週期方法

在元件的存留期間，Blazor 會叫用生命週期方法：

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

如果任何生命週期方法以同步或非同步方式擲回例外狀況，則例外狀況對線路而言是嚴重的。 若要讓元件處理生命週期方法中的錯誤，請新增錯誤處理邏輯。

在下列範例中，`OnParametersSetAsync` 會呼叫方法來取得產品：

* 在 `ProductRepository.GetProductByIdAsync` 方法中擲回的例外狀況是由 `try-catch` 語句所處理。
* 執行 `catch` 區塊時：
  * `loadFailed` 設定為 `true`，用來向使用者顯示錯誤訊息。
  * 會記錄錯誤。

[!code-cshtml[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>呈現邏輯

@No__t 0 元件檔案中的宣告式標記會編譯成稱為 @no__t C# -2 的方法。 當元件呈現時，`BuildRenderTree` 會執行並建立資料結構，以描述所轉譯元件的元素、文字和子元件。

轉譯邏輯可能會擲回例外狀況。 當評估 `@someObject.PropertyName`，但 `@someObject` `null` 時，就會發生這種情況的範例。 轉譯邏輯所擲回的未處理例外狀況對線路而言是嚴重的。

若要避免轉譯邏輯中有 null 參考例外狀況，請先檢查是否有 `null` 物件，然後再存取其成員。 在下列範例中，如果 `person.Address` @no__t，則不會存取 `person.Address` 屬性-2：

[!code-cshtml[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

上述程式碼假設 `person` 不 `null`。 通常，程式碼的結構會保證在呈現元件時，物件存在。 在這些情況下，不需要檢查轉譯邏輯中的 `null`。 在先前的範例中，`person` 可能會保證存在，因為當元件具現化時，會建立 `person`。

### <a name="event-handlers"></a>事件處理常式

當事件處理常式是使用建立C#時，用戶端程式代碼會觸發程式碼的調用：

* `@onclick`
* `@onchange`
* 其他 `@on...` 屬性
* `@bind`

在這些情況下，事件處理常式程式碼可能會擲回未處理的例外狀況。

如果事件處理常式擲回未處理的例外狀況（例如，資料庫查詢失敗），則例外狀況對線路而言是嚴重的。 如果應用程式呼叫可能因外部原因而失敗的程式碼，請使用具有錯誤處理和記錄的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來設陷例外狀況。

如果使用者程式碼不會陷印並處理例外狀況，則架構會記錄例外狀況並終止線路。

### <a name="component-disposal"></a>元件處置

例如，元件可能會從 UI 移除，因為使用者已流覽至另一個頁面。 從 UI 移除執行 <xref:System.IDisposable?displayProperty=fullName> 的元件時，架構會呼叫元件的 @no__t 1 方法。 

如果元件的 `Dispose` 方法擲回未處理的例外狀況，則例外狀況對線路而言是嚴重的。 如果處置邏輯可能會擲回例外狀況，則應用程式應該使用具有錯誤處理和記錄的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句來捕捉例外狀況。

如需有關元件處置的詳細資訊，請參閱 <xref:blazor/components#component-disposal-with-idisposable>。

### <a name="javascript-interop"></a>JavaScript Interop

`IJSRuntime.InvokeAsync<T>` 可讓 .NET 程式碼在使用者的瀏覽器中對 JavaScript 執行時間進行非同步呼叫。

下列條件適用于使用 `InvokeAsync<T>` 的錯誤處理：

* 如果 `InvokeAsync<T>` 的呼叫同步失敗，就會發生 .NET 例外狀況。 例如，`InvokeAsync<T>` 的呼叫可能會失敗，因為無法序列化提供的引數。 開發人員程式碼必須攔截例外狀況。 如果事件處理常式或元件生命週期方法中的應用程式程式碼不會處理例外狀況，則產生的例外狀況對線路而言是嚴重的。
* 如果 `InvokeAsync<T>` 的呼叫以非同步方式失敗，則 .NET <xref:System.Threading.Tasks.Task> 會失敗。 例如，`InvokeAsync<T>` 的呼叫可能會失敗，因為 JavaScript 端程式碼會擲回例外狀況，或傳回以 `rejected` 完成的 `Promise`。 開發人員程式碼必須攔截例外狀況。 如果使用[await](/dotnet/csharp/language-reference/keywords/await)運算子，請考慮將方法呼叫包裝在含有錯誤處理和記錄的[try catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。 否則，失敗的程式碼會產生對線路而言是嚴重的未處理例外狀況。
* 根據預設，`InvokeAsync<T>` 的呼叫必須在特定期間內完成，否則呼叫會超時。預設的超時時間為一分鐘。 Timeout 會保護程式碼不會遺失網路連線，或永遠不會傳回完成訊息的 JavaScript 程式碼。 如果呼叫超時，則產生的 `Task` 會失敗，並出現 <xref:System.OperationCanceledException>。 使用記錄來設陷並處理例外狀況。

同樣地，JavaScript 程式碼可能會起始對[[JSInvokable] 屬性](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions)所指示之 .net 方法的呼叫。 如果這些 .NET 方法擲回未處理的例外狀況：

* 此例外狀況不會被視為對線路的嚴重錯誤。
* JavaScript 端 `Promise` 會遭到拒絕。

您可以選擇在 .NET 端或方法呼叫的 JavaScript 端使用錯誤處理常式代碼。

如需詳細資訊，請參閱<xref:blazor/javascript-interop>。

### <a name="circuit-handlers"></a>線路處理常式

Blazor 可讓程式碼定義*電路處理常式*，以在使用者的線路狀態變更時接收通知。 使用下列狀態：

* `initialized`
* `connected`
* `disconnected`
* `disposed`

通知的管理方式是註冊繼承自 `CircuitHandler` 抽象基類的 DI 服務。

如果自訂電路處理常式的方法擲回未處理的例外狀況，則例外狀況對線路而言是嚴重的。 若要容忍處理常式程式碼或呼叫方法中的例外狀況，請使用錯誤處理和記錄，將程式碼包裝在一個或多個[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。

### <a name="circuit-disposal"></a>線路處置

當線路因使用者已中斷連線而結束，而架構正在清除線路狀態時，架構會處置線路的 DI 範圍。 處置範圍會處置任何執行 <xref:System.IDisposable?displayProperty=fullName> 的線路範圍 DI 服務。 如果任何 DI 服務在處置期間擲回未處理的例外狀況，則架構會記錄例外狀況。

### <a name="prerendering"></a>呈現

Blazor 元件可以使用 `Html.RenderComponentAsync` 來資源清單，以便在使用者的初始 HTTP 要求中傳回其呈現的 HTML 標籤。 其運作方式如下：

* 建立新的電路，其中包含屬於相同頁面的所有資源清單元件。
* 產生初始 HTML。
* 將電路視為 `disconnected`，直到使用者的瀏覽器建立 SignalR 連線回到相同的伺服器，以恢復線路上的互動性。

如果任何元件在進行預入期間擲回未處理的例外狀況（例如，在生命週期方法或轉譯邏輯期間）：

* 這是電路的嚴重例外狀況。
* 例外狀況會從 `Html.RenderComponentAsync` 呼叫中擲回呼叫堆疊。 因此，除非開發人員程式碼明確攔截到例外狀況，否則整個 HTTP 要求都會失敗。

在一般情況下，如果無法轉譯，則繼續建立並轉譯元件並沒有意義，因為無法轉譯運作中的元件。

若要容忍在自動處理期間可能發生的錯誤，錯誤處理邏輯必須放在可能會擲回例外狀況的元件內部。 使用[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句搭配錯誤處理和記錄。 請將錯誤處理邏輯放在 `RenderComponentAsync` 所轉譯的元件中，而不是在 @no__t 1 的語句中包裝對 `RenderComponentAsync` 的呼叫。

## <a name="advanced-scenarios"></a>Advanced 案例

### <a name="recursive-rendering"></a>遞迴轉譯

元件可以遞迴方式進行嵌套。 這適用于表示遞迴資料結構。 例如，@no__t 0 元件可以為每個節點的子系轉譯更多的 @no__t 1 元件。

以遞迴方式轉譯時，避免產生無限遞迴的編碼模式：

* 不要以遞迴方式呈現包含迴圈的資料結構。 例如，不要轉譯其子系包含其本身的樹狀節點。
* 請勿建立包含迴圈的版面配置鏈。 例如，請勿建立版面配置本身的版面配置。
* 不允許終端使用者透過惡意資料輸入或 JavaScript interop 呼叫來違反遞迴不變數（規則）。

呈現期間的無限迴圈：

* 導致轉譯進程永遠繼續。
* 相當於建立未結束的迴圈。

在這些情況下，受影響的線路會停止回應，而執行緒通常會嘗試：

* 會無限期地耗用作業系統所允許的 CPU 時間量。
* 耗用不限數量的伺服器記憶體。 使用無限制的記憶體相當於未結束的迴圈在每次反覆運算時將專案新增至集合的案例。

若要避免無限遞迴模式，請確定遞迴呈現程式碼包含適當的停止條件。

### <a name="custom-render-tree-logic"></a>自訂呈現樹狀結構邏輯

大部分的 Blazor 元件都會實作為*razor*檔案，並且會進行編譯，以產生可在 `RenderTreeBuilder` 上操作以轉譯其輸出的邏輯。 開發人員可以使用程式C#代碼手動執行 `RenderTreeBuilder` 邏輯。 如需詳細資訊，請參閱<xref:blazor/components#manual-rendertreebuilder-logic>。

> [!WARNING]
> 手動轉譯樹狀結構產生器邏輯的使用會被視為先進且不安全的案例，不建議用於一般元件開發。

如果撰寫 `RenderTreeBuilder` 代碼，開發人員必須保證程式碼的正確性。 例如，開發人員必須確定：

* @No__t-0 和 `CloseElement` 的呼叫已正確平衡。
* 屬性只會加入正確的位置。

不正確的手動轉譯樹狀結構產生器邏輯可能會造成任意未定義的行為，包括損毀、伺服器停止回應和安全性弱點。

請考慮以手動方式呈現相同的複雜性層級的轉譯樹產生器邏輯，以及使用與手動撰寫元件程式碼或 MSIL 指令相同層級的*危險*。
