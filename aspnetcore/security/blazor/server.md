---
title: 安全ASP.NET核心Blazor伺服器應用
author: guardrex
description: 瞭解如何緩解對伺服器應用的Blazor安全威脅。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/server
ms.openlocfilehash: bd03f811d0425fdfdb7bbbc24fea5481b49b8ed3
ms.sourcegitcommit: 9675db7bf4b67ae269f9226b6f6f439b5cce4603
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2020
ms.locfileid: "80626015"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a>安全ASP.NET核心布拉佐伺服器應用程式

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)

Blazor Server 應用採用*有狀態*的數據處理模型,其中伺服器和用戶端保持長期關係。 持久狀態由電路保持,該[電路](xref:blazor/state-management)可以跨越可能壽命很長的連接。

當使用者存訪 Blazor Server 網站時,伺服器會在伺服器的記憶體中創建一個電路。 該電路向瀏覽器指示要呈現哪些內容並回應事件,例如當使用者在 UI 中選擇按鈕時。 要執行這些操作,電路在使用者的瀏覽器和伺服器上的 .NET 方法中調用 JavaScript 函數。 這種基於JAVAScript的雙向交互稱為[JAVAScript互操作(JS互操作)。](xref:blazor/call-javascript-from-dotnet)

由於 JS 互通透過 Internet 進行,並且用戶端使用遠端瀏覽器,因此 Blazor Server 應用共用大多數 Web 應用安全問題。 本主題介紹對 Blazor Server 應用的常見威脅,並提供側重於面向 Internet 的應用的威脅緩解指南。

在受限環境中(如公司網路或 Intranet 內部)中,一些緩解指南要麼:

* 不適用於受約束的環境。
* 不值得花費成本來實現,因為安全風險在受限的環境中很低。

## <a name="blazor-server-project-template"></a>布拉佐伺服器項目範本

布拉佐伺服器項目範本可以配置為在創建專案時進行身份驗證。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

按照<xref:blazor/get-started>文章中的可視化工作室指南創建具有身份驗證機制的新 Blazor Server 專案。

在 [建立新的 ASP.NET Core Web 應用程式]**** 對話方塊中選擇 [Blazor 伺服器應用程式]**** 範本之後，請選取 [驗證]**** 下的 [變更]****。

對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：

* **沒有認證**
* **個別使用者帳戶** &ndash; 使用者帳戶能以下列方式儲存：
  * 使用 ASP.NET Core 的[身分識別](xref:security/authentication/identity)系統儲存在應用程式內。
  * 使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。
* **工作或學校帳戶**
* **Windows 驗證**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

按照<xref:blazor/get-started>本文中的可視化工作室代碼指南創建具有身份驗證機制的新 Blazor Server 專案:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制                                                                 | `{AUTHENTICATION}` 值 |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| 不需要驗證                                                                        | `None`                   |
| 個人<br>搭配 ASP.NET Core 身分識別儲存在應用程式中的使用者。                        | `Individual`             |
| 個人<br>儲存在 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 中的使用者。 | `IndividualB2C`          |
| 公司或學校帳戶<br>適用於單一租用戶的組織驗證。            | `SingleOrg`              |
| 公司或學校帳戶<br>適用於多個租用戶的組織驗證。           | `MultiOrg`               |
| Windows 驗證                                                                   | `Windows`                |

命令會建立搭配為 `{APP NAME}` 所提供的值來命名的資料夾，並會使用該資料夾名稱作為應用程式的名稱。 如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 請按照「視覺工作室」瞭解本文中的<xref:blazor/get-started>Mac 指南。

1. 在 **「設定新的 Blazor 伺服器應用**」步驟中,從 **「身份驗證**」下拉清單中選擇 **「個人身份驗證(應用內」。)。**

1. 該應用程式是為存儲在應用中的具有ASP.NET核心標識的單個用戶創建的。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

按照<xref:blazor/get-started>本文中的 .NET Core CLI 指南建立具有身份驗證機制的新 Blazor Server 專案:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制                                                                 | `{AUTHENTICATION}` 值 |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| 不需要驗證                                                                        | `None`                   |
| 個人<br>搭配 ASP.NET Core 身分識別儲存在應用程式中的使用者。                        | `Individual`             |
| 個人<br>儲存在 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 中的使用者。 | `IndividualB2C`          |
| 公司或學校帳戶<br>適用於單一租用戶的組織驗證。            | `SingleOrg`              |
| 公司或學校帳戶<br>適用於多個租用戶的組織驗證。           | `MultiOrg`               |
| Windows 驗證                                                                   | `Windows`                |

命令會建立搭配為 `{APP NAME}` 所提供的值來命名的資料夾，並會使用該資料夾名稱作為應用程式的名稱。 如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。

---

## <a name="pass-tokens-to-a-blazor-server-app"></a>將權杖傳遞到 Blazor 伺服器應用

使用常規剃刀頁面或 MVC 應用驗證 Blazor Server 應用。 預配令牌並將其保存到身份驗證 Cookie。 例如：

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

...

services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
{
    options.ResponseType = "code";
    options.SaveTokens = true;

    options.Scope.Add("offline_access");
    options.Scope.Add("{SCOPE}");
    options.Resource = "{RESOURCE}";
});
```

有關範例代碼(包括完整`Startup.ConfigureServices`範例),請參閱[將權杖傳遞到伺服器端 Blazor 應用程式](https://github.com/javiercn/blazor-server-aad-sample)。

定義要在初始應用狀態中傳遞的類,並帶有存取和刷新權杖:

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

定義以 Blazor 應用中使用**的範圍範圍**權碼提供者服務來解決 DI 中的權杖:

```csharp
using System;
using System.Security.Claims;
using System.Threading.Tasks;

public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

在`Startup.ConfigureServices`中,新增服務:

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

在 *_Host.cshtml*檔案中,`InitialApplicationState`建立與實體並將其作為參數傳遞給應用:

```cshtml
@using Microsoft.AspNetCore.Authentication

...

@{
    var tokens = new InitialApplicationState
    {
        AccessToken = await HttpContext.GetTokenAsync("access_token"),
        RefreshToken = await HttpContext.GetTokenAsync("refresh_token")
    };
}

<app>
    <component type="typeof(App)" param-InitialState="tokens" 
        render-mode="ServerPrerendered" />
</app>
```

在`App`元件 *(App.razor)* 中,解析服務,並使用參數中的資料初始化該服務:

```razor
@inject TokenProvider TokensProvider

...

@code {
    [Parameter]
    public InitialApplicationState InitialState { get; set; }

    protected override Task OnInitializedAsync()
    {
        TokensProvider.AccessToken = InitialState.AccessToken;
        TokensProvider.RefreshToken = InitialState.RefreshToken;

        return base.OnInitializedAsync();
    }
}
```

在發出安全 API 要求的服務中,注入權杖提供者並檢索權以呼叫 API:

```csharp
public class WeatherForecastService
{
    private readonly TokenProvider _store;

    public WeatherForecastService(IHttpClientFactory clientFactory, 
        TokenProvider tokenProvider)
    {
        Client = clientFactory.CreateClient();
        _store = tokenProvider;
    }

    public HttpClient Client { get; }

    public async Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        var token = _store.AccessToken;
        var request = new HttpRequestMessage(HttpMethod.Get, 
            "https://localhost:5003/WeatherForecast");
        request.Headers.Add("Authorization", $"Bearer {token}");
        var response = await Client.SendAsync(request);
        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsAsync<WeatherForecast[]>();
    }
}
```

## <a name="resource-exhaustion"></a>資源耗盡

當用戶端與伺服器交互並導致伺服器消耗過多資源時,可能會出現資源耗盡。 過多的資源消耗主要影響:

* [Cpu](#cpu)
* [記憶體](#memory)
* [用戶端連線](#client-connections)

拒絕服務 (DoS) 攻擊通常會消耗應用或伺服器的資源。 但是,資源耗盡不一定是系統攻擊的結果。 例如,由於使用者需求高,有限資源可能會耗盡。 DoS 在[拒絕服務 (DoS) 攻擊](#denial-of-service-dos-attacks)部分中進一步介紹。

Blazor 框架外部的資源(如資料庫和檔句柄(用於讀取和寫入檔)也可能經歷資源耗盡。 如需詳細資訊，請參閱 <xref:performance/performance-best-practices>。

### <a name="cpu"></a>CPU

當一個或多個用戶端強制伺服器執行密集的 CPU 工作時,可能會出現 CPU 耗盡。

例如,考慮一個布拉佐伺服器應用程式,它計算*一個斐波納奇數位*。 菲博納奇數位由菲博納奇序列生成,其中序列中的每個數位都是前兩個數位的總和。 到達答案所需的工作量取決於序列的長度和初始值的大小。 如果應用未對用戶端的請求進行限制,則 CPU 密集型計算可能會支配 CPU 的時間,並降低其他任務的性能。 過多的資源消耗是影響可用性的安全問題。

CPU 耗盡是所有面向公眾的應用的問題。 在常規 Web 應用中,請求和連接超時作為一種保護措施,但 Blazor Server 應用不提供相同的安全措施。 Blazor Server 應用在執行潛在的 CPU 密集型工作之前必須包括適當的檢查和限制。

### <a name="memory"></a>記憶體

當一個或多個客戶端強制伺服器消耗大量記憶體時,可能會出現記憶體耗盡。

例如,請考慮 Blazor 伺服器端應用,該應用具有接受並顯示專案列表的元件。 如果 Blazor 應用沒有限制允許的項目數或呈現回用戶端的專案數,則記憶體密集型處理和呈現可能會支配伺服器的記憶體,使其達到伺服器性能受損程度。 伺服器可能會崩潰或慢速到它似乎已崩潰的點。

請考慮以下方案,用於維護和顯示與伺服器上的潛在記憶體耗盡方案相關的專案清單:

* 屬性或欄位中的專案`List<MyItem>`使用伺服器的記憶體。 如果應用允許項目清單無限制增長,則存在伺服器記憶體不足的風險。 記憶體不足會導致當前會話結束(崩潰),並且該伺服器實例中的所有併發會話都會收到記憶體不足異常。 為了防止發生此情況,應用必須使用對併發使用者施加項限制的數據結構。
* 如果分頁方案不用於呈現,則伺服器對 UI 中不可見的物件使用其他記憶體。 如果項目數量沒有限制,記憶體需求可能會耗盡可用的伺服器記憶體。 要防止這種情況,請使用以下方法之一:
  * 渲染時使用分頁清單。
  * 僅顯示前 100 到 1,000 個專案,並要求使用者輸入搜尋條件以查找顯示的專案以外的專案。
  * 對於更高級的呈現方案,實現支援*虛擬化*的清單或網格。 使用虛擬化,清單僅呈現當前對用戶可見的項子集。 當使用者與 UI 中的滾動條互動時,元件僅呈現顯示所需的項。 顯示當前不需要的專案可以保存在輔助存儲中,這是理想的方法。 未顯示的專案也可以記在記憶體中,這不太理想。

Blazor Server 應用為有狀態應用(如 WPF、Windows 窗體或 Blazor WebAssembly)提供與其他 UI 框架類似的程式設計模型。 主要區別是,在幾個 UI 框架中,應用使用的記憶體屬於用戶端,並且只影響該單個用戶端。 例如,Blazor WebAssembly 應用完全在用戶端上運行,並且僅使用用戶端記憶體資源。 在 Blazor Server 方案中,應用使用的記憶體屬於伺服器,並在伺服器實例上的客戶端之間共用。

伺服器端記憶體需求是所有 Blazor Server 應用的考慮因素。 但是,大多數 Web 應用都是無狀態的,在返回回應時,在處理請求時使用的記憶體將被釋放。 作為一般建議,不允許用戶端分配未綁定的記憶體量,就像保留用戶端連接的任何其他伺服器端應用一樣。 Blazor Server 應用使用的記憶體保留的時間比單個請求長。

> [!NOTE]
> 在開發過程中,可以使用探查器或捕獲跟蹤來評估客戶端的記憶體需求。 探查器或跟蹤不會捕獲分配給特定用戶端的記憶體。 要捕獲開發過程中特定用戶端的記憶體使用方式,捕獲轉儲並檢查根植於用戶電路中的所有物件的記憶體需求。

### <a name="client-connections"></a>用戶端連接

當一個或多個用戶端打開與伺服器的併發連接過多,從而阻止其他用戶端建立新連接時,可能會導致連接耗盡。

Blazor 用戶端為每個工作階段建立單個連接,並在瀏覽器窗口打開時保持連接打開狀態。 對伺服器維護所有連接的要求並不特定於 Blazor 應用。 鑒於連接的持久性和 Blazor Server 應用的狀態性,連接耗盡對應用的可用性風險更大。

默認情況下,Blazor Server 應用的每位使用者的連接數沒有限制。 如果應用需要連接限制,請採用以下一種或多種方法:

* 需要身份驗證,這自然會限制未經授權的使用者連接到應用的能力。 要使此方案有效,必須防止使用者以任何意願預配新使用者。
* 限制每個使用者的連接數。 限制連接可以通過以下方法完成。 練習謹慎以允許合法使用者存取應用(例如,噹基於用戶端的IP位址建立連接限制時)。
  * 在應用程式等級:
    * 端點路由可擴充性。
    * 需要身份驗證才能連接到應用並追蹤每個使用者的活動作業階段。
    * 達到限制時拒絕新會話。
    * 使用代理(如[Azure SignalR 服務](/azure/azure-signalr/signalr-overview))與應用的連接,該服務將連接從用戶端到應用進行多路複用。 這為具有比單個用戶端可以建立的連接容量更大的應用提供了,從而防止用戶端耗盡與伺服器的連接。
  * 在伺服器級別:在應用前面使用代理/閘道。 例如[,Azure 前門](/azure/frontdoor/front-door-overview)使您能夠定義、管理和監視 Web 流量到應用的全域路由。

## <a name="denial-of-service-dos-attacks"></a>拒絕服務 (DoS) 攻擊

拒絕服務 (DoS) 攻擊涉及用戶端,導致伺服器耗盡其一個或多個資源,使應用不可用。 Blazor Server 應用包含一些預設限制,並依賴於其他ASP.NET核心和訊號R限制來抵禦 DoS 攻擊:

| 布拉佐伺服器應用程式限制                            | 描述 | 預設 |
| ------------------------------------------------------- | ----------- | ------- |
| `CircuitOptions.DisconnectedCircuitMaxRetained`         | 給定伺服器一次在記憶體中保存的最大斷開連接電路數。 | 100 |
| `CircuitOptions.DisconnectedCircuitRetentionPeriod`     | 斷開電路在斷開之前在記憶體中保持最大時間。 | 3 分鐘 |
| `CircuitOptions.JSInteropDefaultCallTimeout`            | 伺服器在超時異步 JavaScript 函數調用之前等待的時間最長。 | 1 分鐘 |
| `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches` | 伺服器在給定時間每個電路的記憶體中保留的最大未確認呈現批處理數,以支援可靠的重新連接。 達到限制后,伺服器將停止生成新的呈現批處理,直到用戶端確認一個或多個批處理。 | 10 |


| 訊號R 與 ASP.NET核心限制             | 描述 | 預設 |
| ------------------------------------------ | ----------- | ------- |
| `CircuitOptions.MaximumReceiveMessageSize` | 單個郵件的消息大小。 | 32 KB |

## <a name="interactions-with-the-browser-client"></a>與瀏覽器(用戶端)的互動

用戶端透過 JS 互通事件調度和呈現完成與伺服器互動。 JS 互通通訊在 JavaScript 和 .NET 之間雙向進行:

* 瀏覽器事件以非同步方式從用戶端發送到伺服器。
* 伺服器根據需要異步重新呈現 UI。

### <a name="javascript-functions-invoked-from-net"></a>從 .NET 呼叫的 JavaScript 函式

對於從 .NET 方法到 JAVAScript 的呼叫:

* 所有調用都有可配置的超時,之後它們將返回<xref:System.OperationCanceledException>給調用方。
  * 呼叫的預設超時`CircuitOptions.JSInteropDefaultCallTimeout`( ) 為一分鐘。 要設定此限制,請參閱<xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls>。
  * 可以提供取消權杖以按每次呼叫控制取消。 如果提供了取消權杖,則盡可能依賴預設調用超時,並規定對用戶端的任何調用。
* 不能信任 JAVAScript 調用的結果。 在Blazor瀏覽器中運行的應用用戶端搜索要調用的 JavaScript 函數。 調用該函數,並生成結果或錯誤。 惡意客戶端可以嘗試:
  * 通過從 JavaScript 函數傳回錯誤,在應用中導致問題。
  * 通過從 JAVAScript 函數返回意外結果,在伺服器上引發意外行為。

採取以下預防措施,防止上述情況:

* 在[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中包裝 JS 互通調用,以考慮呼叫期間可能發生的錯誤。 如需詳細資訊，請參閱 <xref:blazor/handle-errors#javascript-interop>。
* 在採取任何操作之前,驗證從 JS 互通調用傳回的數據,包括錯誤消息。

### <a name="net-methods-invoked-from-the-browser"></a>從瀏覽器呼叫 .NET 方法

不要信任從 JAVAScript 到 .NET 方法的調用。 當 .NET 方法向 JAVAScript 公開時,請考慮如何調用 .NET 方法:

* 對待任何向 JAVAScript 公開的任何 .NET 方法,就像將應用的公共終結點一樣。
  * 驗證輸入。
    * 確保值在預期範圍內。
    * 確保使用者具有執行請求的操作的許可權。
  * 不要將過多的資源分配為 .NET 方法調用的一部分。 例如,執行檢查並限制 CPU 和記憶體使用。
  * 考慮到靜態和實例方法可以公開給 JAVAScript 用戶端。 除非設計要求共用具有適當約束的狀態,否則應避免跨會話共享狀態。
    * 對於最初通過`DotNetReference`依賴項注入 (DI) 創建的物件公開的方法,這些物件應註冊為作用域物件。 這適用於Blazor伺服器應用使用的任何 DI 服務。
    * 對於靜態方法,避免建立無法限定到用戶端的狀態,除非應用在伺服器實例上的所有用戶之間顯式共享狀態。
  * 避免將參數中使用者提供的數據傳遞給 JAVAScript 調用。 如果絕對需要傳入參數中的數據,請確保 JavaScript 代碼處理傳遞數據,而不會引入[跨網站腳本 (XSS)](#cross-site-scripting-xss)漏洞。 例如,不要通過設置元素`innerHTML`的屬性將使用者提供的數據寫入文檔物件模型 (DOM)。 請考慮使用[內容安全原則 (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP)`eval`來禁用和其他不安全的 JavaScript 基元。
* 避免在框架的調度實現之上實現自定義的 .NET 調用。 向瀏覽器公開 .NET 方法是一種高級方案,不Blazor建議用於一般開發。

### <a name="events"></a>事件

事件為Blazor伺服器應用提供入口點。 保護 Web 應用中終結點的相同規則適用於伺服器Blazor應用中 的事件處理。 惡意用戶端可以發送它希望作為事件的有效負載發送的任何數據。

例如：

* 的更改事件`<select>`可以發送不在應用向客戶端顯示的選項中的值。
* 可以`<input>`繞過客戶端驗證向伺服器發送任何文本數據。

應用必須驗證應用處理的任何事件的數據。 框架Blazor[表單元件](xref:blazor/forms-validation)執行基本驗證。 如果應用使用自定義表單元件,則必須編寫自訂代碼以根據需要驗證事件資料。

Blazor伺服器事件是異步的,因此在應用有時間通過生成新的呈現來做出反應之前,可以將多個事件調度到伺服器。 這需要考慮一些安全問題。 必須在事件處理程式內執行限制應用中的用戶端操作,而不是依賴於當前呈現的檢視狀態。

考慮一個計數器元件,該元件應允許使用者將計數器增量最多三次。 增加計數器的按鈕 :`count`的值有條件的 :

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

用戶端可以在框架生成此元件的新呈現之前調度一個或多個增量事件。 結果是,用戶可以增量`count`*三次以上*,因為 UI 未足夠快地刪除該按鈕。 實現三`count`個增量限制的正確方法如下示例所示:

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

通過在處理程式中`if (count < 3) { ... }`添加檢查,`count`增量 決定基於當前應用狀態。 決策不像以前示例中那樣基於 UI 的狀態,後者可能暫時過時。

### <a name="guard-against-multiple-dispatches"></a>防止多個排程

如果事件回調非同步調用長時間運行的操作(例如從外部服務或資料庫提取資料),請考慮使用保護。 在操作進行中,使用視覺反饋,保護可以防止用戶排隊執行多個操作。 從伺服器取得資料時`isLoading``GetForecastAsync`,`true`以下元件代碼將集到。 當`isLoading``true`是 時,該按鈕在 UI 中禁用:

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

如果後台操作使用`async`-`await`模式非同步執行,則上述示例中演示的防護模式有效。

### <a name="cancel-early-and-avoid-use-after-dispose"></a>提前取消,避免處置后使用

除了使用[「防護」中所述的防護裝置(如針對多個調度部分](#guard-against-multiple-dispatches))外,<xref:System.Threading.CancellationToken>請考慮 在釋放元件時使用 取消長時間運行的操作。 此方法具有避免元件在*處置後使用*的額外好處:

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
        TokenSource.Cancel();
    }
}
```

### <a name="avoid-events-that-produce-large-amounts-of-data"></a>避免產生大量資料的事件

某些 DOM`oninput`事件`onscroll`( 如或 )可以生成大量資料。 避免在Blazor伺服器應用中使用這些事件。

## <a name="additional-security-guidance"></a>其他安全指南

保護ASP.NET核心應用的指南適用於Blazor伺服器應用,並涵蓋以下各節:

* [紀錄記錄與敏感資料](#logging-and-sensitive-data)
* [使用 HTTPS 保護傳輸中的資訊](#protect-information-in-transit-with-https)
* [跨網站文稿 (XSS)](#cross-site-scripting-xss))
* [跨源保護](#cross-origin-protection)
* [按一下劫持](#click-jacking)
* [開啟重定](#open-redirects)

### <a name="logging-and-sensitive-data"></a>紀錄記錄與敏感資料

用戶端和伺服器之間的 JS 交互記錄在伺服器的日誌中,並與實例<xref:Microsoft.Extensions.Logging.ILogger>一起 記錄。 Blazor避免記錄敏感資訊,如實際事件或 JS 互通輸入和輸出。

當伺服器上發生錯誤時,框架會通知用戶端並撕下會話。 默認情況下,用戶端會收到一條通用錯誤消息,可在瀏覽器的開發人員工具中看到。

用戶端錯誤不包括調用堆疊,並且不提供有關錯誤原因的詳細資訊,但伺服器日誌確實包含此類資訊。 出於開發目的,可以通過啟用詳細的錯誤來向用戶端提供敏感的錯誤資訊。

通過:

* `CircuitOptions.DetailedErrors`.
* `DetailedErrors`配置金鑰。 例如,將`ASPNETCORE_DETAILEDERRORS`環境變數設置為`true`的值。

> [!WARNING]
> 向 Internet 上的用戶端公開錯誤資訊是應始終避免的安全風險。

### <a name="protect-information-in-transit-with-https"></a>使用 HTTPS 保護傳輸中的資訊

Blazor伺服器用於SignalR用戶端和伺服器之間的通信。 Blazor伺服器通常使用協商的SignalR傳輸,通常是 WebSocket。

Blazor伺服器不能確保伺服器和客戶端之間發送的數據的完整性和機密性。 始終使用 HHH。

### <a name="cross-site-scripting-xss"></a>跨網站文稿 (XSS)

跨網站文本 (XSS) 允許未經授權的一方在瀏覽器上下文中執行任意邏輯。 受攻擊的應用可能在用戶端上運行任意代碼。 該漏洞可用於對伺服器執行許多惡意操作:

* 向伺服器發送虛假/無效事件。
* 派單失敗/無效呈現完成。
* 避免調度渲染完成。
* 將跨攻呼叫從 JavaScript 調度到 .NET。
* 修改從 .NET 到 JAvaScript 的互通調用的回應。
* 避免將 .NET 調度到 JS 互通結果。

Blazor伺服器框架採取措施防止上述威脅:

* 如果用戶端未確認呈現批處理,則停止生成新的 UI 更新。 設定了`CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`。
* 在一分鐘後超時任何 .NET 到 JavaScript 調用,而不會收到用戶端的回應。 設定了`CircuitOptions.JSInteropDefaultCallTimeout`。
* 在 JS 互通期間對來自瀏覽器的所有輸入執行基本驗證:
  * .NET 引用有效,且為 .NET 方法預期的類型。
  * 數據沒有格式錯誤。
  * 有效負載中存在該方法的正確參數數。
  * 在調用 方法之前,可以正確反序列化參數或結果。
* 在來自來自來自調度事件的所有輸入中執行基本驗證:
  * 事件具有有效類型。
  * 可以對事件的數據進行反序列化。
  * 有一個事件處理程式與事件相關聯。

除了框架實現的安全措施外,開發人員還必須對應用進行編碼,以防止威脅並採取適當操作:

* 在處理事件時,始終驗證數據。
* 在接收不合法資料時採取適當的操作:
  * 忽略數據並返回。 這允許應用繼續處理請求。
  * 如果應用確定輸入是非法的,並且無法由合法用戶端生成,則引發異常。 引發異常會撕裂電路並結束會話。
* 不要相信日誌中包含的呈現批處理完成提供的錯誤消息。 該錯誤由用戶端提供,通常不能信任,因為用戶端可能會受到威脅。
* 不要信任在 JavaScript 和 .NET 方法之間朝任一方向對 JS 互通調用的輸入。
* 應用負責驗證參數和結果的內容是否有效,即使參數或結果已正確反序列化。

要存在 XSS 漏洞,應用必須在呈現的頁面中包含使用者輸入。 Blazor伺服器元件執行編譯時間步驟,其中 *.razor*檔中的標記轉換為過程 C# 邏輯。 在執行時,C# 邏輯產生描述元素、文字與子元件的*呈現樹*。 這使用 JavaScript 指令應用於瀏覽器的 DOM(或預先使用清單的 HTML):

* 透過普通 Razor 語法(例如`@someStringValue`), 呈現的使用者輸入不會公開 XSS 漏洞,因為 Razor 語法通過只能寫入文本的命令添加到 DOM。 即使該值包含 HTML 標記,該值也會顯示為靜態文本。 預顯時,輸出由 HTML 編碼,該輸出還會將內容顯示為靜態文本。
* 不允許腳本標記,不應包含在應用的元件呈現樹中。 如果元件的標記中包含腳本標記,則生成編譯時間錯誤。
* 元件作者無需使用 Razor 即可創作 C# 中的元件。 元件作者負責在發出輸出時使用正確的 API。 例如,使用`builder.AddContent(0, someUserSuppliedString)`*而不是*`builder.AddMarkupContent(0, someUserSuppliedString)`,因為後者可能會創建 XSS 漏洞。

作為防範 XSS 攻擊的一部分,請考慮實施 XSS 緩解措施,如[內容安全策略 (CSP)。](https://developer.mozilla.org/docs/Web/HTTP/CSP)

如需詳細資訊，請參閱 <xref:security/cross-site-scripting>。

### <a name="cross-origin-protection"></a>跨源保護

跨源攻擊涉及來自不同來源的用戶端對伺服器執行操作。 惡意操作通常是 GET 請求或表單 POST(跨網站請求偽造、CSRF),但也可以打開惡意 WebSocket。 Blazor伺服器應用提供[與使用中心協定的任何SignalR其他 應用相同的保證:](xref:signalr/security)

* Blazor除非採取其他措施防止伺服器應用跨源訪問,否則可以跨源訪問伺服器應用。 要禁用跨源訪問,請通過將 CORS 中間件添加到管道並將`DisableCorsAttribute`Blazor添加到 終結點元數據或透過[配置跨源SignalR資源分享 來](xref:signalr/security#cross-origin-resource-sharing)限制允許源集,從而禁用終結點中的 CORS。
* 如果啟用了 CORS,則可能需要執行額外的步驟來保護應用,具體取決於 CORS 配置。 如果 CORS 已全域啟用,則可以通過在Blazor`DisableCorsAttribute``hub.MapBlazorHub()`調用 後將中繼資料添加到終結點元數據來禁用伺服器中心。

如需詳細資訊，請參閱 <xref:security/anti-request-forgery>。

### <a name="click-jacking"></a>按一下劫持

單擊劫持涉及將網站從不同來源呈現為`<iframe>`網站內部,以誘使用戶在受到攻擊的網站上執行操作。

要保護應用不在`<iframe>`中呈現,請使用[內容安全策略 (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP)和`X-Frame-Options`標頭。 有關詳細資訊,請參閱[MDN 網頁文件:X 幀選項](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options)。

### <a name="open-redirects"></a>開啟重定

當Blazor伺服器應用作業階段啟動時,伺服器將執行作為啟動作業階段的一部分發送的 URL 的基本驗證。 在建立電路之前,框架會檢查基本 URL 是否是當前 URL 的父 URL。 框架不執行其他檢查。

當使用者選擇用戶端上的連結時,連結的 URL 將發送到伺服器,從而確定要執行的操作。 例如,應用可以執行客戶端導航或指示瀏覽器轉到新位置。

元件還可以使用 以程式設計方式觸發瀏覽`NavigationManager`要求 。 在這種情況下,應用可能會執行客戶端導航或指示瀏覽器轉到新位置。

元件必須:

* 避免將使用者輸入用作導航調用參數的一部分。
* 驗證參數以確保應用允許目標。

否則,惡意用戶可以強制瀏覽器轉到攻擊者控制的網站。 在這種情況下,攻擊者會誘使應用使用某些用戶輸入作為`NavigationManager.Navigate`方法調用的一部分。

在將連結作為應用的一部分呈現時,此建議也適用於:

* 如果可能,請使用相對連結。
* 在將絕對連結目標包括在頁面中之前,驗證其絕對連結目標是否有效。

如需詳細資訊，請參閱 <xref:security/preventing-open-redirects>。

## <a name="authentication-and-authorization"></a>驗證和授權

有關身份驗證和授權的指南,請參閱<xref:security/blazor/index>。

## <a name="security-checklist"></a>安全性檢查清單

以下安全注意事項清單並不全面:

* 驗證事件參數。
* 驗證 JS 互通調用的輸入和結果。
* 避免使用 (或事先驗證) 用戶輸入 .NET 到 JS 互通調用。
* 防止用戶端分配未綁定的內存量。
  * 元件中的數據。
  * `DotNetObject`返回給用戶端的引用。
* 防止多個調度。
* 釋放元件時取消長時間運行的操作。
* 避免生成大量數據的事件。
* 避免將使用者輸入用作調用`NavigationManager.Navigate`URL 的一部分,如果不可避免,請先根據一組允許的原點驗證 URL 的使用者輸入。
* 不要根據 UI 的狀態做出授權決策,而只能根據元件狀態做出授權決策。
* 請考慮使用[內容安全原則 (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP)來抵禦 XSS 攻擊。
* 請考慮使用 CSP 和[X 幀選項](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options)來防止咔嗒聲劫持。
* 在啟用 CORS 或顯式禁Blazor用應用的 CORS 時,確保 CORS 設定是合適的。
* 測試以確保Blazor應用的伺服器端限制提供可接受的用戶體驗,而不會造成不可接受的風險級別。
