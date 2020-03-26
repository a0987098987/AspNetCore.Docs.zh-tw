---
title: Debug ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何 Blazor 應用程式進行 debug。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 5dbc900ab68682068a7f9e3ffdaabef89a0c7798
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306480"
---
# <a name="debug-aspnet-core-opno-locblazor-webassembly"></a>Debug ASP.NET Core Blazor WebAssembly

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor WebAssembly 應用程式可以使用 Chromium 式瀏覽器中的瀏覽器開發工具（邊緣/Chrome）進行調試。  或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。

可用的案例包括：

* 設定和移除中斷點。
* 在 Visual Studio 和 Visual Studio Code （<kbd>F5</kbd>支援）中，以支援偵錯工具的方式執行應用程式。
* 透過程式碼的單一步驟（<kbd>F10</kbd>）。
* 在瀏覽器中使用<kbd>F8</kbd>或 Visual Studio 或 Visual Studio Code 中的<kbd>F5</kbd>繼續執行程式碼。
* 在 [*區域變數*] 顯示中，觀察本機變數的值。
* 查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。

目前，您*無法*：

* 檢查陣列。
* 將滑鼠暫留以檢查成員。
* 逐步執行 managed 程式碼的偵錯工具。
* 具有檢查實數值型別的完整支援。
* 中斷未處理的例外狀況。
* 在應用程式啟動期間叫用中斷點。
* 使用服務工作者來對應用程式進行 Debug。

我們將繼續改善即將發行的版本中的調試過程。

## <a name="prerequisites"></a>Prerequisites

調試需要下列其中一個瀏覽器：

* Microsoft Edge （版本80或更新版本）
* Google Chrome （版本70或更新版本）

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a>啟用 Visual Studio 和 Visual Studio Code 的偵錯工具

使用 ASP.NET Core 3.2 Preview 3 或更新版本 Blazor WebAssembly 專案範本所建立的新專案，會自動啟用偵錯工具。

若要為現有的 Blazor WebAssembly 應用程式啟用偵錯工具，請更新啟始專案中的*launchsettings.json* ，以在每個啟動設定檔中包含下列 `inspectUri` 屬性：

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

更新之後， *launchsettings.json*看起來應該類似下列範例：

[!code-json[](debug/launchSettings.json?highlight=14,22)]

`inspectUri` 屬性：

* 可讓 IDE 偵測應用程式是 Blazor WebAssembly 應用程式。
* 指示腳本的偵錯工具，透過 Blazor的偵錯工具 proxy 連接到瀏覽器。

## <a name="visual-studio"></a>Visual Studio

若要在 Visual Studio 中進行 Blazor WebAssembly 應用程式的 debug：

1. 請確定您已安裝 Visual Studio 2019 16.6 （preview 2 或更新版本）的[最新預覽版本](https://visualstudio.com/preview)。
1. 建立 Blazor WebAssembly 應用程式託管的新 ASP.NET Core。
1. 按<kbd>F5</kbd>以在偵錯工具中執行應用程式。
1. 在 `IncrementCount` 方法中的 [ *razor* ] 中設定中斷點。
1. 流覽至 [**計數器**] 索引標籤，然後選取按鈕以點擊中斷點：

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. 查看 [區域變數] 視窗中 [`currentCount`] 欄位的值：

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. 按<kbd>F5</kbd>繼續執行。

在對您的 Blazor WebAssembly 應用程式進行調試時，您也可以對伺服器程式碼進行 debug：

1. 在 `OnInitializedAsync`的*FetchData razor*頁面中設定中斷點。
1. 在 `Get` 動作方法 的 `WeatherForecastController` 中設定中斷點。
1. 流覽至 [**提取資料**] 索引標籤，在發出 HTTP 要求給伺服器之前，先叫用 `FetchData` 元件中的第一個中斷點：

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. 按<kbd>F5</kbd>繼續執行，然後在 `WeatherForecastController`中的伺服器上按下中斷點：

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. 再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。

## <a name="visual-studio-code"></a>Visual Studio Code

若要在 Visual Studio Code 中進行 Blazor WebAssembly 應用程式的 debug：
 
1. 安裝[ C#擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並將 `debug.javascript.usePreview` 設定為 `true`。

   ![延伸模組](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![JS 預覽偵錯工具](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. 開啟已啟用偵測的現有 Blazor WebAssembly 應用程式。

   * 如果您收到下列通知，表示需要額外的設定才能啟用偵錯工具，請確認您已安裝正確的延伸模組，並已啟用 JavaScript 預覽偵測，然後重載視窗：

     ![其他安裝必要](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * 通知會提供將所需的資產新增至應用程式，以進行建立和調試。 選取 **[是]** ：

     ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. 在偵錯工具中啟動應用程式是兩個步驟的進程：

   1\. **首先**，使用 **.net Core 啟動（Blazor 獨立）** 啟動設定來啟動應用程式。

   2\. **啟動應用程式之後**，請使用 chrome 啟動設定（需要 chrome）**中的 .net Core Debug Blazor Web 元件**來啟動瀏覽器。 若要使用 Edge 而不是 Chrome，請將*vscode/啟動*中的啟動設定 `type` 從 `pwa-chrome` 變更為 `pwa-edge`。

1. 在 `Counter` 元件的 `IncrementCount` 方法中設定中斷點，然後選取按鈕以叫用中斷點：

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a>瀏覽器中的 Debug

1. 在開發環境中執行應用程式的 Debug 組建。

1. 按<kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>。

1. 瀏覽器必須在啟用遠端偵錯功能的情況下執行。 如果已停用遠端偵錯程式，就會產生 [找**不到可調試的瀏覽器]** 索引標籤錯誤頁面。 錯誤頁面包含在開啟偵錯工具的情況之下執行瀏覽器的指示，讓 Blazor 的偵錯工具 proxy 可以連接到應用程式。 *關閉所有瀏覽器實例*，然後依照指示重新開機瀏覽器。

當瀏覽器在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。 展開每個元件，並找出可用來進行偵錯工具的 *.cs*/ *. razor*原始程式檔。 設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。 叫用中斷點之後，以單一步驟（<kbd>F10</kbd>），透過程式碼或繼續（<kbd>F8</kbd>）程式碼執行正常。

Blazor 提供可執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並以擴充通訊協定的偵錯工具 proxy。NET 特定資訊。 當您按下 [調試鍵盤快速鍵] 時，Blazor 會指向位於 proxy 的 Chrome DevTools。 Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。

## <a name="browser-source-maps"></a>瀏覽器來源對應

瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。 不過，Blazor 目前不會C#直接對應至 JAVASCRIPT/WASM。 相反地，Blazor 會在瀏覽器中執行 IL 轉譯，因此來源對應不相關。

## <a name="troubleshoot"></a>疑難排解

如果您遇到錯誤，下列秘訣可能會有説明：

在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。 在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。
