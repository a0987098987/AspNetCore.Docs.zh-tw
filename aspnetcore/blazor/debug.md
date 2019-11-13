---
title: Debug ASP.NET Core Blazor
author: guardrex
description: 瞭解如何 Blazor 應用程式進行 debug。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/debug
ms.openlocfilehash: 3096ad9b3a6904804f239d61f374adcd4d851978
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963140"
---
# <a name="debug-aspnet-core-opno-locblazor"></a>Debug ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor 在 Chrome 的 WebAssembly 上執行的 WebAssembly 應用程式，都有舊版*的支援。*

偵錯工具功能有限。 可用的案例包括：

* 設定和移除中斷點。
* 透過程式碼執行的單一步驟（`F10`）（`F8`）。
* 在 [*區域變數*] 顯示中，觀察 `int`、`string` 和 `bool` 類型的任何本機變數值。
* 查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。

您*不能*：

* 觀察不是 `int`、`string` 或 `bool` 之任何區域變數的值。
* 觀察任何類別屬性或欄位的值。
* 將滑鼠停留在變數上以查看其值。
* 評估主控台中的運算式。
* 逐步執行跨非同步呼叫。
* 執行大部分其他的一般調試情況。

開發進一步的偵錯工具，是工程小組的持續焦點。

## <a name="prerequisites"></a>Prerequisites

調試需要下列其中一個瀏覽器：

* Google Chrome （版本70或更新版本）
* Microsoft Edge 預覽（[Edge 開發人員通道](https://www.microsoftedgeinsider.com)）

## <a name="procedure"></a>程序

1. 在 `Debug` 設定中執行 Blazor WebAssembly 應用程式。 將 `--configuration Debug` 選項傳遞至[dotnet 執行](/dotnet/core/tools/dotnet-run)命令： `dotnet run --configuration Debug`。
1. 在瀏覽器中存取應用程式。
1. 將鍵盤焦點放在應用程式，而不是 [開發人員工具] 面板。 起始調試時，可以關閉開發人員工具面板。
1. 選取下列 Blazor特定的鍵盤快速鍵：
   * Windows/Linux 上的 `Shift+Alt+D`
   * macOS 上的 `Shift+Cmd+D`
1. 依照畫面上所列的步驟，在啟用遠端偵測的情況下重新開機瀏覽器。
1. 再次選取下列 Blazor特定的鍵盤快速鍵，以啟動 debug 會話：
   * Windows/Linux 上的 `Shift+Alt+D`
   * macOS 上的 `Shift+Cmd+D`

## <a name="enable-remote-debugging"></a>啟用遠端偵錯

如果已停用遠端偵錯程式，則 Chrome 會產生 [**找不到可調試的瀏覽器]** 索引標籤錯誤頁面。 錯誤頁面包含執行 Chrome 並開啟偵錯工具埠的指示，讓 Blazor 的偵錯工具 proxy 可以連接到應用程式。 *關閉所有 Chrome 實例*，並依指示重新開機 Chrome。

## <a name="debug-the-app"></a>偵錯應用程式

當 Chrome 在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。 展開每個元件，並找出可用來進行偵錯工具的 *.cs*/ *. razor*原始程式檔。 設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。 叫用中斷點之後，透過程式碼或繼續（`F8`）程式碼執行的單一步驟（`F10`）。

Blazor 提供可執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並以擴充通訊協定的偵錯工具 proxy。NET 特定資訊。 當您按下 [調試鍵盤快速鍵] 時，Blazor 會指向位於 proxy 的 Chrome DevTools。 Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。

## <a name="browser-source-maps"></a>瀏覽器來源對應

瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。 不過，Blazor 目前不會C#直接對應至 JAVASCRIPT/WASM。 相反地，Blazor 會在瀏覽器中執行 IL 轉譯，因此來源對應不相關。

## <a name="troubleshooting-tip"></a>疑難排解秘訣

如果您遇到錯誤，下列秘訣可能會有説明：

在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。 在主控台中，執行 `localStorage.clear()`，以移除任何中斷點。
