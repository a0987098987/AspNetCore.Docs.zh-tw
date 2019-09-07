---
title: ASP.NET Core Blazor 裝載模型
author: guardrex
description: 瞭解 Blazor 用戶端和伺服器端裝載模型。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/hosting-models
ms.openlocfilehash: f7a16d64e1f874a4f6b3c8db5217810b13c7c6ff
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800437"
---
# <a name="aspnet-core-blazor-hosting-models"></a>ASP.NET Core Blazor 裝載模型

依[Daniel Roth](https://github.com/danroth27)

Blazor 是一種 web 架構，設計用來在瀏覽器中以[WebAssembly](https://webassembly.org/)為基礎的 .net 執行時間（*Blazor 客戶*端）或伺服器端在 ASP.NET Core （*Blazor 伺服器端*）中執行用戶端。 無論裝載模型為何，應用程式和元件模型*都相同*。

若要為本文所述的主控模型建立專案，請參閱<xref:blazor/get-started>。

## <a name="client-side"></a>用戶端

Blazor 的主要裝載模型是在 WebAssembly 的瀏覽器中執行用戶端。 Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。 應用程式會直接在瀏覽器 UI 執行緒上執行。 UI 更新和事件處理會在同一個進程中進行。 應用程式的資產會以靜態檔案的形式部署至 web 伺服器或服務，以提供靜態內容給用戶端。

![Blazor 用戶端：Blazor 應用程式會在瀏覽器內的 UI 執行緒上執行。](hosting-models/_static/client-side.png)

若要使用用戶端裝載模型來建立 Blazor 應用程式，請使用**Blazor WebAssembly 應用程式**範本（[dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)）。

選取 [ **Blazor WebAssembly 應用程式**] 範本之後，您可以選擇將應用程式設定為使用 ASP.NET Core 後端，方法是選取 [ **ASP.NET Core**裝載] 核取方塊（[dotnet] [[新增 blazorwasm-](/dotnet/core/tools/dotnet-new)裝載）]。 ASP.NET Core 應用程式會將 Blazor 應用程式提供給用戶端。 Blazor 用戶端應用程式可以使用 Web API 呼叫或[SignalR](xref:signalr/introduction)，透過網路與伺服器互動。

這些範本包含處理下列內容的*blazor. webassembly*腳本：

* 下載 .NET 執行時間、應用程式和應用程式的相依性。
* 初始化執行時間以執行應用程式。

用戶端裝載模型提供數個優點：

* 沒有 .NET 伺服器端相依性。 應用程式會在下載至用戶端之後完全正常運作。
* 用戶端資源和功能都可以充分運用。
* 工作會從伺服器卸載至用戶端。
* 不需要 ASP.NET Core web 伺服器來裝載應用程式。 無伺服器部署案例可行（例如，從 CDN 提供應用程式）。

用戶端裝載有缺點：

* 應用程式僅限於瀏覽器的功能。
* 需要支援的用戶端硬體和軟體（例如 WebAssembly 支援）。
* 下載大小較大，且應用程式需要較長的時間來載入。
* .NET 執行時間和工具支援較不成熟。 例如， [.NET Standard](/dotnet/standard/net-standard)支援和偵錯工具中有限制。

## <a name="server-side"></a>伺服器端

使用伺服器端裝載模型，應用程式會在伺服器上從 ASP.NET Core 應用程式中執行。 UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。

![瀏覽器會透過 SignalR 連線，與伺服器上的應用程式互動（裝載于 ASP.NET Core 應用程式內）。](hosting-models/_static/server-side.png)

若要使用伺服器端裝載模型來建立 Blazor 應用程式，請使用 ASP.NET Core **Blazor 伺服器應用程式**範本（[dotnet new blazorserver](/dotnet/core/tools/dotnet-new)）。 ASP.NET Core 應用程式會裝載伺服器端應用程式，並建立用戶端連接的 SignalR 端點。

ASP.NET Core 應用程式會參考要新增`Startup`的應用程式類別：

* 伺服器端服務。
* 應用程式至要求處理管線。

*Blazor*腳本&dagger;會建立用戶端連接。 應用程式會負責保存和還原所需的應用程式狀態（例如，萬一網路連線中斷時）。

伺服器端裝載模型提供數個優點：

* 下載大小明顯小於用戶端應用程式，且應用程式載入速度會更快。
* 應用程式會充分利用伺服器功能，包括使用任何 .NET Core 相容的 Api。
* 伺服器上的 .NET Core 是用來執行應用程式，因此現有的 .NET 工具（例如，「偵測」）會如預期般運作。
* 支援瘦用戶端。 例如，伺服器端應用程式會使用不支援 WebAssembly 的瀏覽器，以及在資源限制的裝置上。
* 應用程式的 .NET/C#程式碼基底（包括應用程式的元件代碼）不會提供給用戶端。

伺服器端裝載有缺點：

* 通常會有較高的延遲。 每個使用者互動都牽涉到網路躍點。
* 沒有離線支援。 如果用戶端連線失敗，應用程式就會停止運作。
* 對於具有許多使用者的應用程式而言，擴充性是極具挑戰性的。 伺服器必須管理多個用戶端連接並處理用戶端狀態。
* 需要 ASP.NET Core 伺服器才能服務應用程式。 無伺服器部署案例不可行（例如，從 CDN 提供應用程式）。

&dagger;*Blazor*腳本是從 ASP.NET Core 共用架構中的內嵌資源提供。

### <a name="reconnection-to-the-same-server"></a>重新連接到相同的伺服器

Blazor 伺服器端應用程式需要伺服器的作用中 SignalR 連接。 如果連接中斷，應用程式會嘗試重新連線到伺服器。 只要用戶端的狀態仍在記憶體中，用戶端會話就會繼續，而不會失去狀態。
 
當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。 如果重新連線失敗，則會提供使用者重試的選項。 若要自訂 UI，請在 *_Host*的 [ `id` Razor 頁面] 中，以`components-reconnect-modal`做為它的來定義元素。 用戶端會根據連接的狀態，使用下列其中一個 CSS 類別來更新這個元素：
 
* `components-reconnect-show`&ndash;顯示 UI 以指出連線已中斷，而且用戶端正在嘗試重新連接。
* `components-reconnect-hide`&ndash;用戶端具有使用中的連線，並隱藏 UI。
* `components-reconnect-failed`&ndash;重新連接失敗。 若要再次嘗試重新連接`window.Blazor.reconnect()`，請呼叫。

### <a name="stateful-reconnection-after-prerendering"></a>預呈現後的具狀態重新連接
 
在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器端應用程式，以預先呈現伺服器上的 UI。 這是在 *_Host* Razor 頁面中設定：
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode`設定元件是否：

* 會資源清單到頁面中。
* 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。

| `RenderMode`        | 描述 |
| ------------------- | ----------- |
| `ServerPrerendered` | 將元件轉譯為靜態 HTML，並包含 Blazor 伺服器端應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 不支援參數。 |
| `Server`            | 呈現 Blazor 伺服器端應用程式的標記。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 不支援參數。 |
| `Static`            | 將元件轉譯為靜態 HTML。 支援參數。 |

不支援從靜態 HTML 網頁轉譯伺服器元件。
 
用戶端會重新連線至伺服器，其狀態與用來呈現應用程式的狀態相同。 如果應用程式的狀態仍在記憶體中，則在建立 SignalR 連接之後，元件狀態不會重新顯示。

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>從 Razor 頁面和 views 轉譯具狀態的互動式元件
 
可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。

當頁面或視圖呈現時：

* 此元件是使用頁面或視圖所資源清單。
* 用來進行預呈現的初始元件狀態會遺失。
* 建立 SignalR 連接時，會建立新的元件狀態。
 
下列 Razor 頁面會呈現`Counter`元件：

```cshtml
<h1>My Razor Page</h1>
 
@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>從 Razor 頁面和 views 轉譯非互動式元件

在下列 Razor 頁面中， `MyComponent`元件是使用以表單指定的初始值進行靜態轉譯：
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>
 
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

由於`MyComponent`是以靜態方式呈現，因此元件不能是互動式的。

### <a name="detect-when-the-app-is-prerendering"></a>偵測應用程式何時已進行預呈現
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a>設定適用于 Blazor 伺服器端應用程式的 SignalR 用戶端
 
有時候，您需要設定 Blazor 伺服器端應用程式所使用的 SignalR 用戶端。 例如，您可能會想要在 SignalR 用戶端上設定記錄，以診斷連線問題。
 
在*Pages/_Host. cshtml*檔案中設定 SignalR 用戶端：

* 將屬性新增`<script>`至 blazor 的標記。 `autostart="false"`
* 呼叫`Blazor.start`並傳入指定 SignalR 產生器的設定物件。
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>其他資源

* <xref:blazor/get-started>
* <xref:signalr/introduction>
