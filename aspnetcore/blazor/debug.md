---
title: 除錯ASP.NET核心BlazorWeb 組裝
author: guardrex
description: 瞭解如何調試Blazor應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 7273ae3d240de0b59a58069fdcc1880247379751
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661599"
---
# <a name="debug-aspnet-core-opno-locblazor-webassembly"></a>除錯ASP.NET核心BlazorWeb 組裝

[丹尼爾·羅斯](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor可以使用基於鉻瀏覽器(邊緣/Chrome)中的瀏覽器開發工具調試 WebAssembly 應用程式。  或者,您可以使用可視化工作室或可視化工作室代碼調試應用。

可用機制:

* 設置並刪除斷點。
* 在可視化工作室和可視化工作室代碼<kbd>(F5</kbd>支援)中使用調試支援運行應用程式。
* 通過代碼執行單步執行(<kbd>F10</kbd>)。
* 在瀏覽器中使用<kbd>F8</kbd>或可視化工作室代碼中的<kbd>F5</kbd>恢復代碼執行。
* 在 *「局部變數」* 顯示中,觀察局部變數的值。
* 請參閱調用堆疊,包括從 JAVAScript 到 .NET 並從 .NET 到 JavaScript 的調用鏈。

現在,*你不能*:

* 檢查陣列。
* 懸停以檢查成員。
* 步進調試到或從託管代碼中輸入。
* 完全支援檢查值類型。
* 中斷未處理的異常。
* 在應用啟動期間命中斷點。
* 使用服務輔助角色調試應用。

我們將繼續改進即將發佈的調試體驗。

## <a name="prerequisites"></a>Prerequisites

除錯需要以下任瀏覽器:

* 微軟邊緣(版本80或更高版本)
* 谷歌Chrome(版本70或更高版本)

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a>啟用視覺化工作室和視覺化工作室代碼的除錯

對於使用 ASP.NET酷 3.2 預覽 3Blazor或更高版本的 WebAssembly 專案樣本([目前版本為 3.2 預覽 4)](xref:blazor/get-started)創建的新專案,將自動啟用調試。

要開啟現有BlazorWebAssembly 應用的除錯,請在啟動項目中更新*啟動設定.json*檔,以便`inspectUri`在每個啟動 設定檔中包括以下屬性:

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

更新後,*啟動設定.json*檔應類似於以下範例:

[!code-json[](debug/launchSettings.json?highlight=14,22)]

屬性`inspectUri`:

* 使 IDE 能夠檢測Blazor應用是 Web 裝配應用。
* 指示腳本調試基礎結構通過Blazor調試代理連接到瀏覽器。

## <a name="visual-studio"></a>Visual Studio

要在Blazor可視化工作室中調試 Web 組裝應用,可以執行:

1. 確保您[已安裝 Visual Studio 2019 16.6(](https://visualstudio.com/preview)預覽版 2 或更高版本)的最新預覽版本。
1. 創建新ASP.NET核心託管BlazorWeb 組裝應用。
1. 按<kbd>F5</kbd>在調試器中運行應用。
1. 在`IncrementCount`方法中的*Counter.razor*中設定斷點。
1. 瀏覽到 **「計數器」** 選項卡並選擇按鈕以命中斷點:

   ![除錯計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. 在局部變數視窗中檢視`currentCount`欄位的值:

   ![檢視本地人](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. 按<kbd>F5</kbd>繼續執行。

除錯BlazorWebAssembly 應用程式時,您還可以除錯伺服器碼:

1. 在中的*FetchData.razor*`OnInitializedAsync`頁中 設置斷點。
1. 在`WeatherForecastController``Get`操作方法中設置斷點。
1. 瀏覽到 **「取得資料**」選項卡,在向`FetchData`伺服器發出 HTTP 請求之前,點擊元件中的第一個斷點:

   ![除錯提取資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. 按<kbd>F5</kbd>繼續執行,然後`WeatherForecastController`在 中 命中伺服器上的斷點:

   ![除錯伺服器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. 再次按<kbd>F5</kbd>以允許執行繼續,並查看已呈現的天氣預報表。

<a id="vscode"></a>

## <a name="visual-studio-code"></a>Visual Studio Code

要在可視化Blazor工作室代碼中調試 Web 組裝應用,請執行:
 
1. 安裝[C# 延伸](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)與[JavaScript 除錯器(夜間)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸`debug.javascript.usePreview`, 設定為`true`。

   ![延伸模組](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![JS 預覽除錯器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. 打開啟用調試Blazor的現有 WebAssembly 應用。

   * 如果收到以下通知,表示啟用除錯需要額外的設定,請確認已安裝正確的擴展並啟用 JavaScript 預覽除錯,然後重新載入視窗:

     ![附加設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * 通知提供將所需資產添加到應用以生成和調試。 選擇 **"是**"

     ![新增所需資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. 在除錯器中啟動應用是一個兩步過程:

   1\. **首先**,使用 **.NETBlazor核心啟動(獨立)啟動**配置啟動應用程式。

   2\. **應用啟動後**,在 Chrome 啟動配置**中使用 .NETBlazor核心調試網路程式集**啟動瀏覽器(需要 Chrome)。 要使用 Edge 而不是`type`Chrome, 請將 *.vscode/launch.json*中的啟動`pwa-chrome`設定`pwa-msedge`從 更改為 。

1. 在`IncrementCount``Counter`元件中的方法中設定斷點,然後選擇按鈕以命中斷點:

   ![VS 代碼中的除錯計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a>在瀏覽器中除錯

1. 在開發環境中運行應用的調試生成。

1. 按<kbd>移位</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.

1. 必須在啟用遠端調試後運行瀏覽器。 如果關閉遠端除錯,將產生 **「無法搜尋可除錯的瀏覽器選項卡**錯誤」 頁。 錯誤頁包含在打開除錯埠時執行瀏覽器的說明,以便Blazor除錯代理可以連接到應用。 *關閉所有瀏覽器實例*,並按照指示重新啟動瀏覽器。

啟用遠端除錯後,瀏覽器運行後,除錯鍵盤快捷方式將打開一個新的除錯器選項卡。片刻後,「**源**」選項卡顯示應用中 .NET 程式集的清單。 展開每個程式集並找到可用於除錯的 *.cs.razor*/*.razor*源檔。 設置斷點,切換回應用的選項卡,並在代碼執行時命中斷點。 命中斷點後,透過代碼執行單步(<kbd>F10</kbd>) 或正常恢復 (<kbd>F8</kbd>) 程式碼執行.

Blazor提供了一個調試代理,用於實現[Chrome 開發人員工具協定](https://chromedevtools.github.io/devtools-protocol/),並使用 來增強協定。特定於 NET 的資訊。 按下調試鍵盤快捷鍵時,Blazor將 Chrome 開發人員工具指向代理。 代理連接到您要除錯的瀏覽器視窗(因此需要啟用遠端除錯)。

## <a name="browser-source-maps"></a>瀏覽器來源映射

瀏覽器源映射允許瀏覽器將編譯的檔映射回其原始源檔,並通常用於用戶端調試。 但是,Blazor當前不會將 C# 直接映射到 JAVAScript/WASM。 相反,ILBlazor在瀏覽器中解釋,因此源映射不相關。

## <a name="troubleshoot"></a>疑難排解

如果您遇到錯誤,以下提示可能會有所説明:

在 **「除錯器」** 選項卡中,在瀏覽器中打開開發人員工具。 在控制台中,執行`localStorage.clear()`以刪除任何斷點。
