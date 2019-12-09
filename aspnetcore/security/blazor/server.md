---
title: 保護 ASP.NET Core Blazor 伺服器應用程式
author: guardrex
description: 瞭解如何降低 Blazor 伺服器應用程式的安全性威脅。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: security/blazor/server
ms.openlocfilehash: 2d644b84b304a31ad0debc16164ad155c7f7da65
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944278"
---
# <a name="secure-aspnet-core-opno-locblazor-server-apps"></a>保護 ASP.NET Core Blazor 伺服器應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)

Blazor 伺服器應用程式會採用具*狀態*的資料處理模型，其中伺服器和用戶端會維護長期的關聯性。 持續性狀態是由[電路](xref:blazor/state-management)維護，其可跨越也可能很長的連接。

當使用者造訪 Blazor 伺服器網站時，伺服器會在伺服器的記憶體中建立線路。 線路會向瀏覽器指出要呈現哪些內容並回應事件，例如當使用者選取 UI 中的按鈕時。 若要執行這些動作，線路會在使用者的瀏覽器中叫用 JavaScript 函數，並在伺服器上叫用 .NET 方法。 這個雙向以 JavaScript 為基礎的互動稱為[javascript interop （JS interop）](xref:blazor/javascript-interop)。

由於 JS interop 是透過網際網路執行，而用戶端使用遠端瀏覽器，因此 Blazor 伺服器應用程式共用大部分的 web 應用程式安全性考慮。 本主題說明 Blazor 伺服器應用程式的常見威脅，並提供著重于網際網路面向應用程式的威脅緩和方針。

在受限制的環境（例如公司網路或內部網路內部）中，以下是一些緩和措施指引：

* 不適用於受限的環境。
* 不值得實行成本，因為在受限的環境中，安全性風險很低。

## <a name="resource-exhaustion"></a>資源耗盡

當用戶端與伺服器互動，並導致伺服器耗用過多的資源時，就會發生資源耗盡的情況。 過多的資源耗用量主要會影響：

* [CPU](#cpu)
* [記憶體](#memory)
* [用戶端連線](#client-connections)

阻絕服務（DoS）攻擊通常會設法耗盡應用程式或伺服器的資源。 不過，資源耗盡不一定是系統遭受攻擊的結果。 例如，有限的資源可能會因為高使用者需求而耗盡。 [拒絕服務（dos）攻擊](#denial-of-service-dos-attacks)一節會進一步涵蓋 DoS。

Blazor framework 外部的資源，例如資料庫和檔案控制代碼（用來讀取和寫入檔案），也可能會遇到資源耗盡的情況。 如需詳細資訊，請參閱<xref:performance/performance-best-practices>。

### <a name="cpu"></a>CPU

當一或多個用戶端強制服務器執行密集的 CPU 工作時，可能會發生 CPU 耗盡。

例如，假設有一個 Blazor 的伺服器應用程式會計算*Fibonnacci 號碼*。 Fibonnacci 號碼是從 Fibonnacci 序列產生的，其中序列中的每個數位都是上述兩個數字的總和。 到達答案所需的工作量取決於順序的長度和初始值的大小。 如果應用程式不會對用戶端的要求加上限制，則需要大量 CPU 的計算可能會佔據 CPU 的時間，並降低其他工作的效能。 過多的資源耗用量是影響可用性的安全性考慮。

CPU 耗盡是所有公開應用程式的考慮。 在一般 web 應用程式中，要求和連線會以保護的形式提供，但 Blazor 伺服器應用程式不會提供相同的保護措施。 Blazor 伺服器應用程式必須先包含適當的檢查和限制，才能執行可能耗用大量 CPU 的工作。

### <a name="memory"></a>記憶體

當一或多個用戶端強制服務器耗用大量的記憶體時，可能會發生記憶體耗盡。

例如，假設有一個 Blazor伺服器端應用程式，其中包含可接受並顯示專案清單的元件。 如果 Blazor 應用程式不會限制允許的專案數，或轉譯回用戶端的專案數，則記憶體密集型處理和轉譯可能會使伺服器的記憶體在伺服器的效能受到影響的時間點。 伺服器可能當機，或其似乎損毀的時間點變慢。

請考慮下列案例，以維護和顯示伺服器上潛在記憶體耗盡案例的相關專案清單：

* `List<MyItem>` 屬性或欄位中的專案會使用伺服器的記憶體。 如果應用程式允許專案清單不受限制地成長，伺服器就會發生記憶體不足的風險。 記憶體不足會導致目前的會話結束（損毀），而該伺服器實例中的所有並行會話都會收到記憶體不足的例外狀況。 為了避免發生這種情況，應用程式必須使用在並行使用者上強加專案限制的資料結構。
* 如果分頁配置不會用於轉譯，伺服器會針對在 UI 中看不到的物件使用額外的記憶體。 如果專案數沒有限制，記憶體需求可能會耗盡可用的伺服器記憶體。 若要避免這種情況，請使用下列其中一種方法：
  * 轉譯時使用分頁清單。
  * 只顯示前100到1000個專案，並要求使用者輸入搜尋條件，以尋找超過所顯示專案的專案。
  * 如需更先進的轉譯案例，請執行支援*虛擬化*的清單或格線。 使用虛擬化時，清單只會呈現使用者目前可見的專案子集。 當使用者與 UI 中的捲軸互動時，元件只會轉譯顯示所需的專案。 目前不需要顯示的專案可以保留在次要儲存體中，這是理想的方法。 Undisplayed 專案也可以保留在記憶體中，這比較不理想。

Blazor 伺服器應用程式會針對具狀態應用程式（例如 WPF、Windows Forms 或 Blazor WebAssembly）的其他 UI 架構提供類似的程式設計模型。 主要的差異在於，在數個 UI 架構中，應用程式所耗用的記憶體屬於用戶端，而且只會影響該個別用戶端。 例如，Blazor WebAssembly 應用程式會完全在用戶端上執行，而且只會使用用戶端記憶體資源。 在 Blazor 伺服器案例中，應用程式所耗用的記憶體屬於伺服器，而且會在伺服器實例的用戶端之間共用。

伺服器端記憶體需求是所有 Blazor 伺服器應用程式的考慮。 不過，大部分的 web 應用程式都是無狀態的，而且在處理要求時所使用的記憶體會在傳迴響應時釋放。 做為一般建議，請勿允許用戶端在保存用戶端連線的任何其他伺服器端應用程式中，配置未系結的記憶體數量。 Blazor 伺服器應用程式所耗用的記憶體會持續一段時間，而不是單一要求。

> [!NOTE]
> 在開發期間，可以流量分析工具或捕捉到的追蹤來評估用戶端的記憶體需求。 Profiler 或追蹤不會捕獲配置給特定用戶端的記憶體。 若要在開發期間捕捉特定用戶端的記憶體使用量，請捕捉傾印，並檢查以使用者線路為根之所有物件的記憶體需求。

### <a name="client-connections"></a>用戶端連線

當一或多個用戶端開啟太多與伺服器的並行連線，導致其他用戶端無法建立新的連線時，就會發生連線耗盡的情況。

Blazor 用戶端會在每個會話建立單一連線，並且只要開啟瀏覽器視窗，就會讓連線保持開啟狀態。 維護所有連線的伺服器上的需求不是 Blazor 應用程式特有的。 由於連線的持續性和 Blazor 伺服器應用程式的具狀態本質，連接耗盡會對應用程式的可用性造成較大的風險。

根據預設，Blazor 伺服器應用程式的每個使用者連線數目沒有限制。 如果應用程式需要連線限制，請採取下列一或多種方法：

* 需要驗證，這自然會限制未經授權的使用者連線到應用程式的能力。 為了讓此案例生效，使用者必須避免布建新的使用者。
* 限制每個使用者的連接數目。 限制連接可以透過下列方法來完成。 請小心讓合法使用者存取應用程式（例如，根據用戶端的 IP 位址建立連線限制時）。
  * 在應用層級：
    * 端點路由擴充性。
    * 需要驗證才能連接到應用程式，並追蹤每位使用者的使用中會話。
    * 在達到限制時拒絕新的會話。
    * Proxy WebSocket 透過使用 proxy 連線至應用程式，例如從用戶端將分離信號連線到應用程式的[Azure SignalR 服務](/azure/azure-signalr/signalr-overview)。 這會提供比單一用戶端可以建立的連接容量更大的應用程式，以防止用戶端耗盡與伺服器的連接。
  * 在伺服器層級：使用應用程式前方的 proxy/閘道。 例如， [Azure Front 門板](/azure/frontdoor/front-door-overview)可讓您定義、管理及監視應用程式的網路流量全域路由。

## <a name="denial-of-service-dos-attacks"></a>拒絕服務（DoS）攻擊

阻絕服務（DoS）攻擊牽涉到用戶端導致伺服器耗盡其一或多項資源，讓應用程式無法使用。 Blazor 伺服器應用程式包含一些預設限制，而且依賴其他 ASP.NET Core 和 SignalR 限制，以防範 DoS 攻擊：

| Blazor 伺服器應用程式限制                            | 描述 | Default |
| ------------------------------------------------------- | ----------- | ------- |
| `CircuitOptions.DisconnectedCircuitMaxRetained`         | 給定伺服器一次保存在記憶體中的中斷連線線路數目上限。 | 100 |
| `CircuitOptions.DisconnectedCircuitRetentionPeriod`     | 中斷連線的線路在損毀之前，保留在記憶體中的最大時間量。 | 3 分鐘 |
| `CircuitOptions.JSInteropDefaultCallTimeout`            | 伺服器在計時非同步 JavaScript 函式呼叫之前等待的最大時間量。 | 1 分鐘 |
| `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches` | 伺服器在指定時間將每個線路保留在記憶體中的未認可轉譯批次數目上限，以支援健全的重新連接。 達到此限制之後，伺服器就會停止產生新的轉譯批次，直到用戶端認可一或多個批次為止。 | 10 |


| SignalR 和 ASP.NET Core 限制             | 描述 | Default |
| ------------------------------------------ | ----------- | ------- |
| `CircuitOptions.MaximumReceiveMessageSize` | 個別訊息的訊息大小。 | 32 KB |

## <a name="interactions-with-the-browser-client"></a>與瀏覽器的互動（用戶端）

用戶端會透過 JS interop 事件分派和轉譯完成來與伺服器互動。 JS interop 通訊會在 JavaScript 和 .NET 之間進行這兩種方式：

* 瀏覽器事件會以非同步方式從用戶端分派至伺服器。
* 伺服器會視需要以非同步方式回應 rerendering UI。

### <a name="javascript-functions-invoked-from-net"></a>從 .NET 叫用的 JavaScript 函式

針對從 .NET 方法到 JavaScript 的呼叫：

* 所有調用都有一個可設定的超時時間，在這之後，會將 <xref:System.OperationCanceledException> 傳回給呼叫者。
  * 呼叫（`CircuitOptions.JSInteropDefaultCallTimeout`）的預設超時時間為一分鐘。 若要設定此限制，請參閱 <xref:blazor/javascript-interop#harden-js-interop-calls>。
  * 可以提供解除標記來控制每個呼叫的取消。 如果提供解除標記，則依賴預設的呼叫超時時間（如果有的話，也可以呼叫用戶端）。
* 無法信任 JavaScript 呼叫的結果。 在瀏覽器中執行的 Blazor 應用程式用戶端會搜尋要叫用的 JavaScript 函數。 系統會叫用函式，並產生結果或錯誤。 惡意用戶端可以嘗試：
  * 從 JavaScript 函式傳回錯誤，導致應用程式發生問題。
  * 藉由從 JavaScript 函式傳回非預期的結果，在伺服器上引發非預期的行為。

請採取下列預防措施來防範前述案例：

* 在[try catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中包裝 JS interop 呼叫，以考慮調用期間可能發生的錯誤。 如需詳細資訊，請參閱<xref:blazor/handle-errors#javascript-interop>。
* 在採取任何動作之前，請先驗證從 JS interop 調用傳回的資料，包括錯誤訊息。

### <a name="net-methods-invoked-from-the-browser"></a>從瀏覽器叫用的 .NET 方法

不信任從 JavaScript 到 .NET 方法的呼叫。 將 .NET 方法公開給 JavaScript 時，請考慮 .NET 方法的叫用方式：

* 將任何公開給 JavaScript 的 .NET 方法視為應用程式的公用端點。
  * 驗證輸入。
    * 確定值在預期的範圍內。
    * 請確定使用者具有執行所要求動作的許可權。
  * 請勿在 .NET 方法調用中配置過多的資源數量。 例如，執行檢查並對 CPU 和記憶體使用量進行限制。
  * 請考慮靜態和實例方法可以公開給 JavaScript 用戶端。 避免在會話間共用狀態，除非設計呼叫具有適當條件約束的共用狀態。
    * 若為透過相依性插入（DI）所建立 `DotNetReference` 物件所公開的實例方法，則應該將物件註冊為已設定範圍的物件。 這適用于 Blazor 伺服器應用程式所使用的任何 DI 服務。
    * 針對靜態方法，除非應用程式在伺服器實例上的所有使用者之間依設計明確共用狀態，否則請避免建立不能限定于用戶端範圍的狀態。
  * 避免將參數中使用者提供的資料傳遞至 JavaScript 呼叫。 如果絕對需要在參數中傳遞資料，請確定 JavaScript 程式碼會在不引進[跨網站腳本（XSS）](#cross-site-scripting-xss)弱點的情況下，處理傳遞資料。 例如，請不要藉由設定元素的 `innerHTML` 屬性，將使用者提供的資料寫入檔物件模型（DOM）。 請考慮使用[內容安全性原則（CSP）](https://developer.mozilla.org/docs/Web/HTTP/CSP)來停用 `eval` 和其他不安全的 JavaScript 基本類型。
* 避免在架構的分派執行上，執行 .NET 調用的自訂分派。 將 .NET 方法公開至瀏覽器是一個先進的案例，不建議用於一般 Blazor 開發。

### <a name="events"></a>「事件」

事件會提供 Blazor 伺服器應用程式的進入點。 Web apps 中保護端點的相同規則適用于 Blazor 伺服器應用程式中的事件處理。 惡意用戶端可以將任何想要傳送的資料傳送為事件的內容。

例如：

* `<select>` 的變更事件可能會傳送的值不在應用程式呈現給用戶端的選項內。
* `<input>` 可以將任何文字資料傳送至伺服器，略過用戶端驗證。

應用程式必須驗證應用程式所處理之任何事件的資料。 Blazor framework [forms 元件](xref:blazor/forms-validation)會執行基本驗證。 如果應用程式使用自訂表單元件，就必須撰寫自訂程式碼來適當地驗證事件資料。

Blazor 的伺服器事件是非同步，因此可以藉由產生新的轉譯，將多個事件分派至伺服器，讓應用程式有時間反應。 這有一些要考慮的安全性含意。 限制應用程式中的用戶端動作必須在事件處理常式內部執行，而不是取決於目前呈現的檢視狀態。

假設有一個「計數器」元件，應該允許使用者增加最多三次的計數器。 根據 `count`的值，有條件地遞增計數器的按鈕：

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        count++;
    }
}
```

用戶端可以分派一或多個增量事件，架構才會產生這個元件的新轉譯。 結果是，使用者可以將 `count` 遞增*三次*，因為 UI 不會快速移除按鈕。 以下範例顯示達成三個 `count` 增量之限制的正確方式：

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        if (count < 3)
        {
            count++;
        }
    }
}
```

藉由在處理常式內加入 `if (count < 3) { ... }` 檢查，會根據目前的應用程式狀態來決定是否要遞增 `count`。 這項決策不是以上一個範例中的 UI 狀態為基礎，這可能會暫時過時。

### <a name="guard-against-multiple-dispatches"></a>防護多個分派

如果事件回呼叫用長時間執行的作業（例如從外部服務或資料庫提取資料），請考慮使用「防護」。 此防護可以防止使用者在作業進行中時，使用視覺效果的意見反應來排入多個作業。 下列元件程式碼會將 `isLoading` 設定為 `true`，同時 `GetForecastAsync` 從伺服器取得資料。 當 `isLoading` `true`時，UI 中的按鈕會停用：

```razor
@page "/fetchdata"
@using BlazorServerSample.Data
@inject WeatherForecastService ForecastService

<button disabled="@isLoading" @onclick="UpdateForecasts">Update</button>

@code {
    private bool isLoading;
    private WeatherForecast[] forecasts;

    private async Task UpdateForecasts()
    {
        if (!isLoading)
        {
            isLoading = true;
            forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
            isLoading = false;
        }
    }
}
```

### <a name="cancel-early-and-avoid-use-after-dispose"></a>及早取消並避免使用-處置後

除了使用防護[多個分派](#guard-against-multiple-dispatches)一節中所述的防護以外，請考慮在處置元件時，使用 <xref:System.Threading.CancellationToken> 來取消長時間執行的作業。 這種方法的優點是避免在元件中*使用-dispose* ：

```razor
@implements IDisposable

...

@code {
    private readonly CancellationTokenSource TokenSource = 
        new CancellationTokenSource();

    private async Task UpdateForecasts()
    {
        ...

        forecasts = await ForecastService.GetForecastAsync(DateTime.Now, 
            TokenSource.Token);

        if (TokenSource.Token.IsCancellationRequested)
        {
           return;
        }

        ...
    }

    public void Dispose()
    {
        CancellationTokenSource.Cancel();
    }
}
```

### <a name="avoid-events-that-produce-large-amounts-of-data"></a>避免產生大量資料的事件

某些 DOM 事件（例如 `oninput` 或 `onscroll`）可能會產生大量的資料。 請避免在 Blazor 伺服器應用程式中使用這些事件。

## <a name="additional-security-guidance"></a>其他安全性指引

保護 ASP.NET Core 應用程式的指導方針適用于 Blazor Server 應用程式，並在下列各節中涵蓋：

* [記錄和敏感性資料](#logging-and-sensitive-data)
* [使用 HTTPS 保護傳輸中的資訊](#protect-information-in-transit-with-https)
* [跨網站腳本（XSS）](#cross-site-scripting-xss)
* [跨原始來源保護](#cross-origin-protection)
* [按一下-劫持](#click-jacking)
* [開啟重新導向](#open-redirects)

### <a name="logging-and-sensitive-data"></a>記錄和敏感性資料

用戶端與伺服器之間的 JS interop 互動會記錄在具有 <xref:Microsoft.Extensions.Logging.ILogger> 實例的伺服器記錄中。 Blazor 可避免記錄敏感性資訊，例如實際事件或 JS interop 輸入和輸出。

當伺服器上發生錯誤時，架構會通知用戶端並向下眼淚會話。 根據預設，用戶端會收到一般錯誤訊息，可在瀏覽器的開發人員工具中看到。

用戶端錯誤不會包含呼叫堆疊，也不會提供錯誤原因的詳細資料，但伺服器記錄檔包含這類資訊。 基於開發目的，您可以藉由啟用詳細錯誤，將敏感性錯誤資訊提供給用戶端。

使用下列內容啟用詳細錯誤：

* `CircuitOptions.DetailedErrors`。
* `DetailedErrors` 組態機碼。 例如，將 `ASPNETCORE_DETAILEDERRORS` 環境變數設定為 `true`的值。

> [!WARNING]
> 將錯誤資訊公開給網際網路上的用戶端，是應一律避免的安全性風險。

### <a name="protect-information-in-transit-with-https"></a>使用 HTTPS 保護傳輸中的資訊

Blazor Server 會使用 SignalR 來進行用戶端與伺服器之間的通訊。 Blazor Server 通常會使用 SignalR 協商的傳輸，這通常是 Websocket。

Blazor Server 不會確保在伺服器與用戶端之間傳送之資料的完整性與機密性。 一律使用 HTTPS。

### <a name="cross-site-scripting-xss"></a>跨網站腳本（XSS）

跨網站腳本（XSS）可讓未經授權的合作物件在瀏覽器的內容中執行任意邏輯。 遭入侵的應用程式可能會在用戶端上執行任意程式碼。 此弱點可能會用來對伺服器執行一些惡意動作：

* 將假/無效事件分派至伺服器。
* 分派失敗/不正確轉譯完成。
* 避免分派轉譯完成。
* 將來自 JavaScript 的 interop 呼叫分派至 .NET。
* 修改從 .NET 到 JavaScript 的 interop 呼叫回應。
* 避免將 .NET 分派到 JS interop 結果。

Blazor Server framework 會採取下列步驟來防範先前的威脅：

* 如果用戶端未確認轉譯批次，則停止產生新的 UI 更新。 設定 `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`。
* 在一分鐘之後，任何 .NET 到 JavaScript 的呼叫都不會收到來自用戶端的回應。 設定 `CircuitOptions.JSInteropDefaultCallTimeout`。
* 在 JS interop 期間，對來自瀏覽器的所有輸入執行基本驗證：
  * .NET 參考是有效的，而且是 .NET 方法所預期的類型。
  * 資料的格式不正確。
  * 此方法的引數數目正確，會出現在裝載中。
  * 引數或結果可以在叫用方法之前正確還原序列化。
* 在來自瀏覽器的所有輸入中，從分派的事件執行基本驗證：
  * 事件具有有效的類型。
  * 事件的資料可以還原序列化。
  * 有一個事件處理常式與事件相關聯。

除了架構所實行的保護措施以外，應用程式必須由開發人員撰寫程式碼，以防範威脅並採取適當的動作：

* 在處理事件時，一律驗證資料。
* 接收無效資料時採取適當的動作：
  * 忽略資料並返回。 這可讓應用程式繼續處理要求。
  * 如果應用程式判斷出輸入是非法的，而且無法由合法的用戶端產生，則會擲回例外狀況。 擲回眼淚的例外狀況，並結束會話。
* 請勿信任記錄檔中所包含的轉譯批次完成所提供的錯誤訊息。 此錯誤是由用戶端所提供，而且通常無法受到信任，因為用戶端可能會遭到入侵。
* 請勿信任 JavaScript 和 .NET 方法之間任一方向的 JS interop 呼叫上的輸入。
* 應用程式會負責驗證引數和結果的內容是否有效，即使引數或結果已正確還原序列化也一樣。

若要讓 XSS 弱點存在，應用程式必須在呈現的頁面中納入使用者輸入。 Blazor 伺服器元件會執行編譯時期步驟，其中*razor*檔案中的標記會轉換成程式C#邏輯。 在執行時間， C#邏輯會建立描述元素、文字和子元件的轉譯*樹狀結構*。 這會透過一系列的 JavaScript 指示套用至瀏覽器的 DOM （或在進行預建的情況下序列化為 HTML）：

* 透過一般 Razor 語法呈現的使用者輸入（例如 `@someStringValue`）不會公開 XSS 弱點，因為 Razor 語法會透過只能寫入文字的命令新增至 DOM。 即使值包含 HTML 標籤，值也會顯示為靜態文字。 預先呈現時，輸出會以 HTML 編碼，這也會將內容顯示為靜態文字。
* 不允許腳本標記，且不應包含在應用程式的元件轉譯樹狀結構中。 如果腳本標記包含在元件的標記中，就會產生編譯時期錯誤。
* 元件作者可以在中撰寫C#元件，而不需使用 Razor。 元件作者負責在發出輸出時使用正確的 Api。 例如，使用 `builder.AddContent(0, someUserSuppliedString)` 而*不*是 `builder.AddMarkupContent(0, someUserSuppliedString)`，因為後者可能會建立 XSS 弱點。

在保護 XSS 攻擊的過程中，請考慮執行 XSS 緩和措施，例如[內容安全性原則（CSP）](https://developer.mozilla.org/docs/Web/HTTP/CSP)。

如需詳細資訊，請參閱<xref:security/cross-site-scripting>。

### <a name="cross-origin-protection"></a>跨原始來源保護

跨原始來源攻擊牽涉到來自不同來源的用戶端對伺服器執行動作。 惡意動作通常是 GET 要求或表單 POST （跨網站偽造要求，CSRF），但也可能開啟惡意的 WebSocket。 Blazor 伺服器應用程式[會提供相同的保證，讓任何其他 SignalR 應用程式使用中樞通訊協定供應](xref:signalr/security)專案：

* 除非採取額外的措施來防止此情況，否則可以跨來源存取 Blazor 伺服器應用程式。 若要停用跨原始來源存取，請在端點中停用 CORS，方法是將 CORS 中介軟體新增至管線，然後將 `DisableCorsAttribute` 新增至 Blazor 端點中繼資料，或藉由設定[跨原始來源資源分享的 SignalR](xref:signalr/security#cross-origin-resource-sharing)來限制允許的來源集合。
* 如果已啟用 CORS，則可能需要額外的步驟來保護應用程式，視 CORS 設定而定。 如果已全域啟用 CORS，則可以在呼叫 `hub.MapBlazorHub()`之後，將 `DisableCorsAttribute` 中繼資料新增至端點中繼資料，藉此停用 Blazor 伺服器中樞的 CORS。

如需詳細資訊，請參閱<xref:security/anti-request-forgery>。

### <a name="click-jacking"></a>按一下-劫持

按一下-劫持包含以不同的來源將網站轉譯為網站內的 `<iframe>`，以誘騙使用者在遭受攻擊的網站上執行動作。

若要保護應用程式不在 `<iframe>`內呈現，請使用[內容安全性原則（CSP）](https://developer.mozilla.org/docs/Web/HTTP/CSP)和 `X-Frame-Options` 標頭。 如需詳細資訊，請參閱[MDN web 檔： X 框架-選項](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options)。

### <a name="open-redirects"></a>開啟重新導向

當 Blazor 伺服器應用程式會話啟動時，伺服器會針對啟動會話時所傳送的 Url 執行基本驗證。 在建立線路之前，架構會檢查基底 URL 是否為目前 URL 的父系。 架構不會執行其他檢查。

當使用者選取用戶端上的連結時，會將連結的 URL 傳送至伺服器，以決定要採取的動作。 例如，應用程式可能會執行用戶端導覽，或向瀏覽器表示前往新的位置。

元件也可以透過使用 `NavigationManager`以程式設計方式觸發導覽要求。 在這種情況下，應用程式可能會執行用戶端導覽，或向瀏覽器表示前往新的位置。

元件必須：

* 避免在導覽呼叫引數中使用使用者輸入。
* 驗證引數，以確保應用程式允許該目標。

否則，惡意使用者可以強制瀏覽器移至攻擊者控制的網站。 在此案例中，攻擊者會將應用程式訣竅為在 `NavigationManager.Navigate` 方法的調用中使用某些使用者輸入。

這項建議也適用于將連結轉譯為應用程式的一部分時：

* 可能的話，請使用相對連結。
* 先驗證絕對連結目的地是否有效，再將它們包含在頁面中。

如需詳細資訊，請參閱<xref:security/preventing-open-redirects>。

## <a name="authentication-and-authorization"></a>驗證與授權

如需驗證和授權的指引，請參閱 <xref:security/blazor/index>。

## <a name="security-checklist"></a>安全性檢查清單

下列安全性考慮清單並不完整：

* 驗證來自事件的引數。
* 驗證 JS interop 呼叫的輸入和結果。
* 避免使用（或預先驗證） .NET 到 JS interop 呼叫的使用者輸入。
* 防止用戶端配置未系結的記憶體數量。
  * 元件中的資料。
  * 傳回給用戶端的 `DotNetObject` 參考。
* 針對多個分派進行防護。
* 處置元件時，取消長時間執行的作業。
* 避免產生大量資料的事件。
* 請避免在呼叫 `NavigationManager.Navigate` 中使用使用者輸入，並先針對一組允許的原始來源驗證 Url 的使用者輸入（如果無法避免）。
* 請勿根據 UI 狀態（但僅從元件狀態）進行授權決策。
* 請考慮使用[內容安全性原則（CSP）](https://developer.mozilla.org/docs/Web/HTTP/CSP)來防範 XSS 攻擊。
* 請考慮使用 CSP 和[X 框架選項](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options)來防止按一下劫持。
* 請確定 CORS 設定適用于啟用 CORS，或明確停用 Blazor 應用程式的 CORS。
* 測試以確保 Blazor 應用程式的伺服器端限制提供可接受的使用者體驗，而不會有無法接受的風險層級。
