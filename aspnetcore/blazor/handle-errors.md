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
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a>處理ASP.NET核心Blazor應用程式中的錯誤

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

本文介紹如何Blazor管理未處理的異常以及如何開發檢測和處理錯誤的應用。

## <a name="detailed-errors-during-development"></a>開發過程中的詳細錯誤

當應用Blazor在開發過程中無法正常運行時,從應用接收詳細的錯誤資訊有助於解決問題。 發生錯誤時,Blazor應用在螢幕底部顯示金條:

* 在開發過程中,金條將引導您到瀏覽器控制台,您可以在其中看到異常。
* 在生產中,金條會通知使用者錯誤,並建議刷新瀏覽器。

此錯誤處理體驗的 UI 是專案Blazor範本的一部分。

在BlazorWebAssembly 應用中,自訂*wwwroot/index.html*檔中的體驗:

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

在Blazor'伺服器"應用中,自訂*主頁/_Host.cshtml*檔案中的體驗:

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

`blazor-error-ui`元素被Blazor樣本中包含的樣式隱藏 *(wwwroot/css/site.css),* 然後在發生錯誤時顯示:

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

## <a name="how-a-opno-locblazor-server-app-reacts-to-unhandled-exceptions"></a>Blazor伺服器應用如何回應未處理的例外

Blazor伺服器是一個有狀態的框架。 當使用者與應用交互時,他們維護與稱為*電路*的伺服器的連接。 該電路包含活動元件實例,以及狀態的許多其他方面,例如:

* 元件的最新呈現輸出。
* 用戶端事件可能觸發的當前事件處理委託集。

如果使用者在多個瀏覽器選項卡中打開應用,則他們有多個獨立電路。

Blazor將大多數未處理的異常視為對發生異常的電路的致命異常。 如果電路由於未處理的異常而終止,則使用者只能通過重新載入頁面來創建新電路,繼續與應用交互。 終止的電路(其他使用者或其他瀏覽器選項卡的電路)以外的電路不受影響。 此方案類似於必須重新啟動崩潰的應用&mdash;崩潰的桌面應用,但其他應用不受影響。

當由於以下原因發生未處理的異常時,電路終止:

* 未處理的異常通常會使電路處於未定義狀態。
* 未處理異常后無法保證應用的正常操作。
* 如果電路繼續,應用中可能會出現安全漏洞。

## <a name="manage-unhandled-exceptions-in-developer-code"></a>管理開發人員代碼中未處理的例外

要在錯誤後繼續應用,應用必須具有錯誤處理邏輯。 本文後面的部分介紹未處理異常的潛在來源。

在生產中,不要在 UI 中呈現框架異常消息或堆疊跟蹤。 呈現例外訊息或堆疊追蹤可以:

* 向最終使用者披露敏感資訊。
* 説明惡意用戶發現應用中可能危及應用、伺服器或網路安全性的弱點。

## <a name="log-errors-with-a-persistent-provider"></a>使用持久提供程式記錄錯誤

如果發生未處理的異常,則將異常記錄到<xref:Microsoft.Extensions.Logging.ILogger>服務容器中配置的實例。 默認情況下,Blazor應用使用主控台日誌記錄提供程式登錄到控制台輸出。 請考慮使用管理日誌大小和日誌輪換的提供程式登錄到更永久的位置。 如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。

在開發過程中Blazor,通常會將異常的完整詳細資訊發送到瀏覽器的主控台,以説明調試。 在生產中,默認情況下禁用瀏覽器控制台中的詳細錯誤,這意味著錯誤不會發送給用戶端,但異常的完整詳細資訊仍記錄伺服器端。 如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。

您必須決定要記錄的事件和記錄事件的嚴重性級別。 敵對使用者可能能夠故意觸發錯誤。 例如,不要從顯示產品詳細資訊的元件的 URL 中`ProductId`提供 未知內容的錯誤中記錄事件。 並非所有錯誤都應被視為日誌記錄的高嚴重性事件。

## <a name="places-where-errors-may-occur"></a>可能發生錯誤的地方

框架和應用程式代碼可能會在以下任何位置觸發未處理的異常:

* [元件實例化](#component-instantiation)
* [生命週期方法](#lifecycle-methods)
* [渲染邏輯](#rendering-logic)
* [事件處理常式](#event-handlers)
* [元件處理](#component-disposal)
* [JavaScript Interop](#javascript-interop)
* [Blazor伺服器重新成像](#blazor-server-prerendering)

本文的以下部分介紹了上述未處理的異常。

### <a name="component-instantiation"></a>元件實例化

建立Blazor元件實體時:

* 調用元件的構造函數。
* 將呼叫透過[`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component)指令或[`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component)屬性提供給元件構造函數的任何非單例 DI 服務的建構函數。

當Blazor任何執行的構造函數或`[Inject]`任何 屬性的設定器引發未處理的異常時,伺服器電路將失敗。 異常是致命的,因為框架無法實例化元件。 如果建構函數邏輯可能會引發異常,則應用應使用具有錯誤處理和日誌記錄[的 try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句捕獲異常。

### <a name="lifecycle-methods"></a>生命週期方法

在元件的生存期內,Blazor呼叫以下[生命週期方法](xref:blazor/lifecycle):

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

如果任何生命週期方法以同步或非同步方式引發異常,則異常對Blazor伺服器電路是致命的。 對於處理生命週期方法中的錯誤的元件,添加錯誤處理邏輯。

在以下範例中`OnParametersSetAsync`呼叫方法以取得產品:

* `ProductRepository.GetProductByIdAsync`方法中引發的異常由`try-catch`語句處理。
* 執行`catch`區塊時:
  * `loadFailed`設置為`true`,用於向使用者顯示錯誤訊息。
  * 記錄錯誤。

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>渲染邏輯

`.razor`元件檔中的聲明性標記被編譯為稱為`BuildRenderTree`的 C# 方法。 當元件的成像時`BuildRenderTree`, 執行並建譯描述呈現元件的元素、文本和子元件的資料結構。

呈現邏輯可能會引發異常。 此方案的範例在計算時`@someObject.PropertyName`發生,但`@someObject`為`null`。 呈現邏輯引發的未處理異常對Blazor伺服器電路是致命的。

為了防止呈現邏輯中的空引用異常,請在訪問`null`物件成員之前檢查物件。 在下面的範例中,`person.Address``person.Address`如果`null`是, 則不存取屬性:

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

前面的代碼假定`person`不是`null`。 通常,代碼的結構保證物件在呈現元件時存在。 在這些情況下,無需在呈現邏輯中檢查`null`。 在上一個示例中`person`,可以保證存在,`person`因為 是在實例化元件時創建的。

### <a name="event-handlers"></a>事件處理常式

使用: 建立事件處理程式時,客戶端代碼觸發 C# 程式的呼叫:

* `@onclick`
* `@onchange`
* 其他`@on...`屬性
* `@bind`

在這種情況下,事件處理程式代碼可能會引發未處理的異常。

如果事件處理程序引發未處理的異常(例如,資料庫查詢失敗),則異常對Blazor伺服器電路是致命的。 如果應用呼叫的代碼可能會由於外部原因失敗,請使用具有錯誤處理和日誌記錄[的 try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句捕獲異常。

如果用戶代碼不捕獲和處理異常,則框架將記錄異常並終止電路。

### <a name="component-disposal"></a>元件處理

例如,可以從 UI 中刪除元件,因為使用者已導航到另一個頁面。 從 UI<xref:System.IDisposable?displayProperty=fullName>中刪除 實現元件時,框架將調用該<xref:System.IDisposable.Dispose*>元件的方法。

如果元件`Dispose`的方法引發未處理的異常,則異常對Blazor伺服器電路是致命的。 如果處置邏輯可能會引發異常,則應用應使用具有錯誤處理和日誌記錄[的 try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句捕獲異常。

有關元件處理的詳細資訊,請參閱<xref:blazor/lifecycle#component-disposal-with-idisposable>。

### <a name="javascript-interop"></a>JavaScript Interop

`IJSRuntime.InvokeAsync<T>`允許 .NET 代碼在使用者的瀏覽器中對 JavaScript 運行時進行非同步調用。

以下條件適用於 使用的`InvokeAsync<T>`錯誤 處理:

* 如果調用`InvokeAsync<T>`同步失敗,則會發生 .NET 異常。 例如,調用`InvokeAsync<T>`可能會失敗,因為提供的參數無法序列化。 開發人員代碼必須捕獲異常。 如果事件處理程式或元件生命週期方法中的應用代碼不處理異常,則生成的異常對Blazor伺服器電路是致命的。
* 如果調用`InvokeAsync<T>`以非同步方式失敗,則<xref:System.Threading.Tasks.Task>.NET 將失敗。 呼叫`InvokeAsync<T>`可能會失敗,例如,因為 JavaScript 連接程式碼引發`Promise``rejected`異常或傳回作為 完成的 。 開發人員代碼必須捕獲異常。 如果使用[await](/dotnet/csharp/language-reference/keywords/await)運算符,請考慮在[嘗試 catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中包裝方法調用,並處理錯誤並記錄。 否則,故障代碼會導致未處理的異常,該異常對Blazor伺服器電路造成致命。
* 默認情況下,調用`InvokeAsync<T>`必須在一定時間段內完成,否則呼叫超時。預設超時週期為一分鐘。 超時可保護程式碼免受網路連接丟失或從未發送回完成消息的 JavaScript 代碼的丟失。 如果呼叫逾時,則產生的`Task`失敗與<xref:System.OperationCanceledException>。 捕獲和處理日誌記錄的異常。

同樣,JavaScript 代碼可以啟動對屬性指示的[`[JSInvokable]`](xref:blazor/call-dotnet-from-javascript).NET 方法的調用。 如果這些 .NET 方法引發未處理的異常:

* 異常不被視為Blazor對伺服器電路致命。
* JavaScript`Promise`端被拒絕。

您可以選擇在方法呼叫的 .NET 端或 JavaScript 端使用錯誤處理代碼。

如需詳細資訊，請參閱下列文章：

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

### <a name="opno-locblazor-server-prerendering"></a>Blazor伺服器預算

Blazor可以使用[元件標記説明程式](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)預呈現元件,以便將其呈現的 HTML 標記作為使用者初始 HTTP 請求的一部分返回。 其工作方式為:

* 為屬於同一頁面的所有預渲染元件創建新電路。
* 生成初始 HTML。
* 將電路視為`disconnected`在用戶的瀏覽器建立回同SignalR一伺服器的連接之前。 建立連接後,電路上恢復互動性,並更新元件的 HTML 標記。

如果任何元件在預成成週期期間(例如,在生命週期方法期間或在呈現邏輯中)引發未處理的異常:

* 異常對電路是致命的。
* 異常從`Component`標記説明程序引發調用堆疊。 因此,除非開發人員代碼顯式捕獲異常,否則整個 HTTP 請求將失敗。

在正常情況下,當預渲染失敗時,繼續生成和呈現元件沒有意義,因為無法呈現工作元件。

為了容忍預渲染期間可能發生的錯誤,錯誤處理邏輯必須放置在可能引發異常的元件中。 將[嘗試捕獲](/dotnet/csharp/language-reference/keywords/try-catch)語句與錯誤處理和日誌記錄一起使用。 不要將`Component`標記説明程式包裝`try-catch`在 語句中,`Component`而是在標記説明程序呈現的元件中放置錯誤處理邏輯。

## <a name="advanced-scenarios"></a>進階案例

### <a name="recursive-rendering"></a>遞迴成

元件可以遞迴嵌套。 這對於表示遞歸數據結構很有用。 例如,元件`TreeNode`可以為每個節點的子`TreeNode`節點呈現更多元件。

遞迴時,避免編碼模式導致無限遞歸:

* 不要遞歸地呈現包含週期的數據結構。 例如,不要呈現其子節點包括自身的樹節點。
* 不要創建包含迴圈的佈局鏈。 例如,不要創建佈局本身的佈局。
* 不允許最終用戶通過惡意數據輸入或 JAVAScript 互通調用違反遞歸不變(規則)。

成像過程中的無限迴圈:

* 使渲染過程永久繼續。
* 等效於創建未終止的迴圈。

在這些情況下,受影響的Blazor伺服器電路失敗,並且線程通常嘗試:

* 無限期地消耗操作系統允許的 CPU 時間。
* 消耗無限量的伺服器記憶體。 使用無限記憶體等效於未終止迴圈在每次反覆運算時將條目添加到集合的情況。

為了避免無限遞歸模式,請確保遞歸呈現代碼包含適當的停止條件。

### <a name="custom-render-tree-logic"></a>自訂成圖邏輯

大多數Blazor元件都實現為 *.razor*檔,並編譯以生成在`RenderTreeBuilder`上操作 以呈現其輸出的邏輯。 開發人員可以使用過程 C#`RenderTreeBuilder`程式碼手動實現邏輯。 如需詳細資訊，請參閱 <xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic>。

> [!WARNING]
> 使用手動呈現樹生成器邏輯被視為高級和不安全方案,不建議用於一般元件開發。

如果`RenderTreeBuilder`編寫了代碼,開發人員必須保證代碼的正確性。 例如,開發人員必須確保:

* 調用`OpenElement``CloseElement`和 的呼叫正確平衡。
* 屬性僅添加到正確的位置。

不正確的手動呈現樹生成器邏輯可能會導致任意未定義的行為,包括崩潰、伺服器掛起和安全漏洞。

請考慮手動渲染具有相同複雜性級別的樹生成器邏輯,其*危險*級別與手動編寫程式集代碼或 MSIL 指令的風險級別相同。
