---
title: Debug ASP.NET CoreBlazor WebAssembly
author: guardrex
description: 瞭解如何調試 Blazor 程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 828fb0ce5101407b6f40195138d59c335eec389f
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2020
ms.locfileid: "86407667"
---
# <a name="debug-aspnet-core-blazor-webassembly"></a>Debug ASP.NET CoreBlazor WebAssembly

[Daniel Roth](https://github.com/danroth27)

Blazor WebAssembly應用程式可以使用瀏覽器開發工具，以 Chromium 為基礎的瀏覽器（邊緣/Chrome）進行調試。 或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。

可用的案例包括：

* 設定和移除中斷點。
* 在 Visual Studio 和 Visual Studio Code （<kbd>F5</kbd>支援）中，以支援偵錯工具的方式執行應用程式。
* 透過程式碼的單一步驟（<kbd>F10</kbd>）。
* 在瀏覽器中使用<kbd>F8</kbd>或 Visual Studio 或 Visual Studio Code 中的<kbd>F5</kbd>繼續執行程式碼。
* 在 [*區域變數*] 顯示中，觀察本機變數的值。
* 查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。

目前，您*無法*：

* 中斷未處理的例外狀況。
* 在應用程式啟動期間叫用中斷點。

我們將繼續改善即將發行的版本中的調試過程。

## <a name="prerequisites"></a>必要條件

調試需要下列其中一個瀏覽器：

* Google Chrome （70版或更新版本）（預設值）
* Microsoft Edge （版本80或更新版本）

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a>啟用 Visual Studio 和 Visual Studio Code 的偵錯工具

若要啟用現有應用程式的偵測功能 Blazor WebAssembly ，請更新 `launchSettings.json` 啟始專案中的檔案，以 `inspectUri` 在每個啟動設定檔中包含下列屬性：

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

更新之後，檔案 `launchSettings.json` 看起來應該類似下列範例：

[!code-json[](debug/launchSettings.json?highlight=14,22)]

`inspectUri`屬性：

* 可讓 IDE 偵測應用程式是否為應用程式 Blazor WebAssembly 。
* 指示腳本的偵錯工具，透過的偵錯工具 proxy 連接到瀏覽器 Blazor 。

在 `wsProtocol` 啟動的瀏覽器（）上，websocket 通訊協定（）、主機（ `url.hostname` ）、埠（ `url.port` ）和偵測器 URI 的預留位置值 `browserInspectUri` 是由架構所提供。

## <a name="visual-studio"></a>Visual Studio

若要 Blazor WebAssembly 在 Visual Studio 中進行應用程式的 debug：

1. 建立新的 ASP.NET Core 託管 Blazor WebAssembly 應用程式。
1. 按<kbd>F5</kbd>以在偵錯工具中執行應用程式。
1. 在 `Pages/Counter.razor` 方法的中設定中斷點 `IncrementCount` 。
1. 流覽至 [] 索引標籤 **`Counter`** ，然後選取按鈕以叫用中斷點：

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. 查看 `currentCount` [區域變數] 視窗中的欄位值：

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. 按<kbd>F5</kbd>繼續執行。

在對您的 Blazor WebAssembly 應用程式進行調試時，您也可以對伺服器程式碼進行 debug：

1. 在的頁面中設定中斷點 `Pages/FetchData.razor` <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 。
1. 在動作方法的中設定中斷點 `WeatherForecastController` `Get` 。
1. 流覽至索引 **`Fetch Data`** 標籤，以叫用元件中的第一個中斷點， `FetchData` 然後再對伺服器發出 HTTP 要求：

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. 按<kbd>F5</kbd>繼續執行，然後在的伺服器上叫用中斷點 `WeatherForecastController` ：

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. 再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。

<a id="vscode"></a>

## <a name="visual-studio-code"></a>Visual Studio Code

如需安裝應用程式開發 Visual Studio Code 的詳細資訊 Blazor ，請參閱 <xref:blazor/tooling> 。

### <a name="debug-standalone-blazor-webassembly"></a>獨立調試Blazor WebAssembly

1. Blazor WebAssembly在 VS Code 中開啟獨立應用程式。

   如果您收到下列通知，表示需要額外的設定才能啟用偵錯工具：
   
   * 確認您已安裝正確的延伸模組。
   * 確認 JavaScript preview 的偵錯工具已啟用。
   * 重載視窗。

   ![需要額外的設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

1. 使用<kbd>F5</kbd>鍵盤快速鍵或功能表項目開始進行調試。

1. 出現提示時，請選取 [ ** Blazor WebAssembly Debug** ] 選項來開始進行調試。

   ![可用的調試選項清單](index/_static/blazor-vscode-debugtypes.png)

1. 隨即啟動獨立應用程式，並開啟偵錯工具瀏覽器。

1. 在元件的方法中設定中斷點 `IncrementCount` `Counter` ，然後選取按鈕以叫用中斷點：

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

### <a name="debug-hosted-blazor-webassembly"></a>已裝載的調試Blazor WebAssembly

1. Blazor WebAssembly在 VS Code 中開啟裝載應用程式的 [解決方案] 資料夾。

1. 如果沒有為專案設定啟動設定，則會出現下列通知。 選取 [是]。

   ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. 在視窗頂端的 [命令選擇區] 中，選取裝載解決方案內的*伺服器*專案。

`launch.json`會使用啟動偵錯工具來產生檔案。

### <a name="attach-to-an-existing-debugging-session"></a>附加至現有的偵錯工具會話

若要附加至執行 Blazor 中的應用程式，請使用下列設定來建立檔案 `launch.json` ：

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach to Existing Blazor WebAssembly Application"
}
```

> [!NOTE]
> 只有針對獨立應用程式才支援附加至「偵測會話」。 若要使用完整堆疊的偵錯工具，您必須從 VS Code 啟動應用程式。

### <a name="launch-configuration-options"></a>啟動設定選項

以下是針對 `blazorwasm` debug 類型（）支援的啟動設定選項 `.vscode/launch.json` 。

| 選項    | 描述 |
| --------- | ----------- |
| `request` | 使用 `launch` 來啟動並將偵錯工具連結至 Blazor WebAssembly 應用程式，或將 `attach` 偵錯工具附加至已在執行中的應用程式。 |
| `url`     | 要在瀏覽器中開啟的 URL。 預設為 `https://localhost:5001`。 |
| `browser` | 要為偵錯工具啟動的瀏覽器。 設為 `edge` 或 `chrome`。 預設為 `chrome`。 |
| `trace`   | 用來從 JS 偵錯工具產生記錄。 將設定為 `true` 以產生記錄。 |
| `hosted`  | `true`如果啟動和偵測託管應用程式，則必須設定為 Blazor WebAssembly 。 |
| `webRoot` | 指定 web 伺服器的絕對路徑。 如果從子路由提供應用程式，則應設定。 |
| `timeout` | 等候偵錯工具附加的毫秒數。 預設為30000毫秒（30秒）。 |
| `program` | 可執行檔的參考，可執行裝載應用程式的伺服器。 如果為，則必須設定 `hosted` `true` 。 |
| `cwd`     | 要在其下啟動應用程式的工作目錄。 如果為，則必須設定 `hosted` `true` 。 |
| `env`     | 要提供給已啟動進程的環境變數。 只有在設為時才適用 `hosted` `true` 。 |

### <a name="example-launch-configurations"></a>啟動設定範例

#### <a name="launch-and-debug-a-standalone-blazor-webassembly-app"></a>啟動和調試獨立 Blazor WebAssembly 應用程式

```json
{
  "type": "blazorwasm",
  "request": "launch",
  "name": "Launch and Debug"
}
```

#### <a name="attach-to-a-running-app-at-a-specified-url"></a>在指定的 URL 附加至執行中的應用程式

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach and Debug",
  "url": "http://localhost:5000"
}
```

#### <a name="launch-and-debug-a-hosted-blazor-webassembly-app-with-microsoft-edge"></a>使用 Microsoft Edge 啟動和調試託管 Blazor WebAssembly 應用程式

瀏覽器設定預設為 Google Chrome。 當您使用 Microsoft Edge 進行偵錯工具時，請將設定 `browser` 為 `edge` 。 若要使用 Google Chrome，請不要設定 `browser` 選項，或將選項的值設定為 `chrome` 。

```json
{
  "name": "Launch and Debug Hosted Blazor WebAssembly App",
  "type": "blazorwasm",
  "request": "launch",
  "hosted": true,
  "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/MyHostedApp.Server.dll",
  "cwd": "${workspaceFolder}/Server",
  "browser": "edge"
}
```

在上述範例中， `MyHostedApp.Server.dll` 是*伺服器*應用程式的元件。 `.vscode`資料夾位於 `Client` 、 `Server` 和資料夾旁的方案資料夾中 `Shared` 。

## <a name="debug-in-the-browser"></a>瀏覽器中的 Debug

1. 在開發環境中執行應用程式的 Debug 組建。

1. 啟動瀏覽器並流覽至應用程式的 URL （例如， `https://localhost:5001` ）。

1. 在瀏覽器中，按下<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>，嘗試開始進行遠端偵錯程式。

   瀏覽器必須在啟用遠端偵測功能的情況下執行，這不是預設值。 如果已停用遠端偵錯程式，將會顯示 [**找不到可調試的瀏覽器]** 索引標籤錯誤頁面，並提供啟動瀏覽器並開啟偵錯工具的指示。 遵循瀏覽器的指示，這會開啟新的瀏覽器視窗。 關閉先前的瀏覽器視窗。

1. 一旦瀏覽器在啟用遠端偵錯程式的情況下執行，偵錯工具鍵盤快速鍵（<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>）就會開啟新的偵錯工具索引標籤。

1. 經過一段時間之後，[**來源**] 索引標籤會顯示該節點內的應用程式 .net 元件清單 `file://` 。

1. 在元件程式碼（檔案 `.razor` ）和 c # 程式碼檔案（ `.cs` ）中，您設定的中斷點會在執行程式碼時叫用。 叫用中斷點之後，以單一步驟（<kbd>F10</kbd>），透過程式碼或繼續（<kbd>F8</kbd>）程式碼執行正常。

Blazor提供的偵錯工具 proxy 會執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並使用來擴充通訊協定。NET 特定資訊。 當您按下 [調試鍵盤快速鍵] 時，會將 Blazor Chrome DevTools 指向 proxy。 Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。

## <a name="browser-source-maps"></a>瀏覽器來源對應

瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。 不過， Blazor 目前不會將 c # 直接對應至 JavaScript/WASM。 相反地， Blazor 會在瀏覽器中進行 IL 轉譯，因此來源對應不相關。

## <a name="troubleshoot"></a>疑難排解

如果您遇到錯誤，下列秘訣可能會有説明：

* 在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。 在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。
* 確認您已安裝並信任 ASP.NET Core 的 HTTPS 開發憑證。 如需詳細資訊，請參閱 <xref:security/enforcing-ssl#troubleshoot-certificate-problems> 。
* Visual Studio 需要 [**工具**] [選項] [一般] 中的 [**啟用 ASP.NET （Chrome、Edge 和 IE）的 JavaScript 偵錯工具**] 選項  >  **Options**  >  **Debugging**  >  ** **。 這是 Visual Studio 的預設設定。 如果偵錯工具無法運作，請確認已選取此選項。
