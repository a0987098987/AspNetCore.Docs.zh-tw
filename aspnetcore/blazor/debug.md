---
title: Debug ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何調試 Blazor 程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 193dc656c2ee0154f0ae534bc00f8dc29bab3258
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84239218"
---
# <a name="debug-aspnet-core-blazor-webassembly"></a>Debug ASP.NET Core Blazor WebAssembly

[Daniel Roth](https://github.com/danroth27)

BlazorWebAssembly 應用程式可以使用 Chromium 式瀏覽器中的瀏覽器開發工具（邊緣/Chrome）進行調試。 或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。

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

* Microsoft Edge （版本80或更新版本）
* Google Chrome （版本70或更新版本）

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a>啟用 Visual Studio 和 Visual Studio Code 的偵錯工具

若要啟用現有 Blazor WebAssembly 應用程式的偵測，請更新啟始專案中的*launchsettings.json* ，以 `inspectUri` 在每個啟動設定檔中包含下列屬性：

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

更新之後， *launchsettings.json*看起來應該類似下列範例：

[!code-json[](debug/launchSettings.json?highlight=14,22)]

`inspectUri`屬性：

* 可讓 IDE 偵測應用程式是否為 Blazor WebAssembly 應用程式。
* 指示腳本的偵錯工具，透過的偵錯工具 proxy 連接到瀏覽器 Blazor 。

在 `wsProtocol` 啟動的瀏覽器（）上，websocket 通訊協定（）、主機（ `url.hostname` ）、埠（ `url.port` ）和偵測器 URI 的預留位置值 `browserInspectUri` 是由架構所提供。

## <a name="visual-studio"></a>Visual Studio

若要 Blazor 在 Visual Studio 中進行 WebAssembly 應用程式的 debug：

1. 建立新的 ASP.NET Core 託管 Blazor WebAssembly 應用程式。
1. 按<kbd>F5</kbd>以在偵錯工具中執行應用程式。
1. 在方法的 [ *razor* ] 中設定中斷點 `IncrementCount` 。
1. 流覽至 [**計數器**] 索引標籤，然後選取按鈕以點擊中斷點：

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. 查看 `currentCount` [區域變數] 視窗中的欄位值：

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. 按<kbd>F5</kbd>繼續執行。

在偵測 Blazor WebAssembly 應用程式時，您也可以對伺服器程式碼進行偵錯工具：

1. 在的 [ *FetchData* ] 頁面中設定中斷點 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 。
1. 在動作方法的中設定中斷點 `WeatherForecastController` `Get` 。
1. 流覽至 [**提取資料**] 索引標籤，以叫用元件中的第一個中斷點， `FetchData` 然後再對伺服器發出 HTTP 要求：

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. 按<kbd>F5</kbd>繼續執行，然後在的伺服器上叫用中斷點 `WeatherForecastController` ：

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. 再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。

<a id="vscode"></a>

## <a name="visual-studio-code"></a>Visual Studio Code

若要 Blazor 在 Visual Studio Code 中進行 WebAssembly 應用程式的 debug：
 
安裝[c # 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸，並 `debug.javascript.usePreview` 將設定為 `true` 。

![延伸模組](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

![JS 預覽偵錯工具](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

### <a name="debug-standalone-blazor-webassembly"></a>Debug 獨立 Blazor WebAssembly

1. Blazor在 VS Code 中開啟獨立的 WebAssembly 應用程式。

   如果您收到下列通知，表示需要額外的設定才能啟用偵錯工具：
   
   * 確認您已安裝正確的延伸模組。
   * 確認 JavaScript preview 的偵錯工具已啟用。
   * 重載視窗。

   ![需要額外的設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

1. 使用<kbd>F5</kbd>鍵盤快速鍵或功能表項目開始進行調試。

1. 出現提示時，選取 [ ** Blazor WebAssembly] Debug**選項以開始進行調試。

   ![可用的調試選項清單](index/_static/blazor-vscode-debugtypes.png)

1. 隨即啟動獨立應用程式，並開啟偵錯工具瀏覽器。

1. 在元件的方法中設定中斷點 `IncrementCount` `Counter` ，然後選取按鈕以叫用中斷點：

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

### <a name="debug-hosted-blazor-webassembly"></a>Debug hosted Blazor WebAssembly

1. Blazor在 VS Code 中開啟 hosted WebAssembly 應用程式。

1. 如果沒有為專案設定啟動設定，則會出現下列通知。 選取 [是]。

   ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. 在 [選取範圍] 視窗中，選取裝載解決方案內的*伺服器*專案。

隨即會產生啟動的*json*檔案，並啟動偵錯工具。

### <a name="attach-to-an-existing-debugging-session"></a>附加至現有的偵錯工具會話

若要附加至執行 Blazor 中的應用程式，請使用下列設定建立一個*啟動 json*檔案：

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

以下是支援的啟動設定選項： `blazorwasm` debug 類型。

| 選項    | 描述 |
| --------- | ----------- |
| `request` | 使用 `launch` 來啟動 WebAssembly 應用程式，並將其連結至已在執行中的 Blazor `attach` 應用程式。 |
| `url`     | 要在瀏覽器中開啟的 URL。 預設為 `https://localhost:5001`。 |
| `browser` | 要為偵錯工具啟動的瀏覽器。 設為 `edge` 或 `chrome`。 預設為 `chrome`。 |
| `trace`   | 用來從 JS 偵錯工具產生記錄。 將設定為 `true` 以產生記錄。 |
| `hosted`  | `true`如果啟動和偵測託管的 WebAssembly 應用程式，則必須設定為 Blazor 。 |
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

#### <a name="launch-and-debug-a-hosted-blazor-webassembly-app"></a>啟動和偵測託管的 Blazor WebAssembly 應用程式

```json
{
  "type": "blazorwasm",
  "request": "launch",
  "name": "Launch and Debug Hosted App",
  "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/MyHostedApp.Server.dll",
  "cwd": "${workspaceFolder}"
}
```

## <a name="debug-in-the-browser"></a>瀏覽器中的 Debug

1. 在開發環境中執行應用程式的 Debug 組建。

1. 按<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>。

1. 瀏覽器必須在啟用遠端偵錯功能的情況下執行。 如果已停用遠端偵錯程式，就會產生 [找**不到可調試的瀏覽器]** 索引標籤錯誤頁面。 錯誤頁面包含在開啟偵錯工具的情況之下執行瀏覽器的指示，讓 Blazor 偵錯工具 proxy 可以連接到應用程式。 *關閉所有瀏覽器實例*，然後依照指示重新開機瀏覽器。

當瀏覽器在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。 展開每個元件，並 *.cs*尋找 / *.razor*可用來進行偵錯工具的 .cs 原始檔案。 設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。 叫用中斷點之後，以單一步驟（<kbd>F10</kbd>），透過程式碼或繼續（<kbd>F8</kbd>）程式碼執行正常。

Blazor提供的偵錯工具 proxy 會執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並使用來擴充通訊協定。NET 特定資訊。 當您按下 [調試鍵盤快速鍵] 時，會將 Blazor Chrome DevTools 指向 proxy。 Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。

## <a name="browser-source-maps"></a>瀏覽器來源對應

瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。 不過， Blazor 目前不會將 c # 直接對應至 JavaScript/WASM。 相反地， Blazor 會在瀏覽器中進行 IL 轉譯，因此來源對應不相關。

## <a name="troubleshoot"></a>疑難排解

如果您遇到錯誤，下列秘訣可能會有説明：

* 在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。 在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。
* 確認您已安裝並信任 ASP.NET Core 的 HTTPS 開發憑證。 如需詳細資訊，請參閱<xref:security/enforcing-ssl#troubleshoot-certificate-problems>。
