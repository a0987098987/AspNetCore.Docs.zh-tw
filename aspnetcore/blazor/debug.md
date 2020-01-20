---
title: Debug ASP.NET Core Blazor
author: guardrex
description: 瞭解如何 Blazor 應用程式進行 debug。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159985"
---
# <a name="debug-aspnet-core-opno-locblazor"></a>Debug ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

在以 Chromium 為基礎的瀏覽器中使用瀏覽器開發工具（Chrome/Edge）進行偵錯工具 Blazor WebAssembly 時，會有*早期*支援。 工作進行中：

* 在 Visual Studio 中完全啟用調試。
* 在 Visual Studio Code 中啟用調試。

偵錯工具功能有限。 可用的案例包括：

* 設定和移除中斷點。
* 透過程式碼或繼續（`F8`）程式碼執行的單一步驟（`F10`）。
* 在 [*區域變數*] 顯示中，觀察 `int`、`string`和 `bool`類型的任何本機變數值。
* 查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。

您*不能*：

* 觀察不是 `int`、`string`或 `bool`之任何區域變數的值。
* 觀察任何類別屬性或欄位的值。
* 將滑鼠停留在變數上以查看其值。
* 評估主控台中的運算式。
* 逐步執行跨非同步呼叫。
* 執行大部分其他的一般調試情況。

開發進一步的偵錯工具，是工程小組的持續焦點。

## <a name="prerequisites"></a>必要條件：

調試需要下列其中一個瀏覽器：

* Google Chrome （版本70或更新版本）
* Microsoft Edge 預覽（[Edge 開發人員通道](https://www.microsoftedgeinsider.com)）

## <a name="procedure"></a>程序

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!WARNING]
> Visual Studio 中的偵錯工具支援是在開發的初期階段。 目前不支援**F5**調試。

1. 在 `Debug` 設定中執行 Blazor WebAssembly 應用程式而不進行任何偵測（**Ctrl**+**f5** ，而不是**f5**）。
1. 開啟應用程式的 [Debug] 屬性（[ **debug** ] 功能表中的最後一個專案），然後複製 HTTP**應用程式 URL**。 使用以 Chromium 為基礎的瀏覽器（Edge Beta 或 Chrome），流覽至應用程式的 HTTP 位址（而非 HTTPS 位址）。
1. 將鍵盤焦點放在應用程式的瀏覽器視窗中，而不是 [開發人員工具] 面板。 最好讓開發人員工具面板在此程式中關閉。 啟動偵錯工具之後，您可以重新開啟 [開發人員工具] 面板。
1. 選取下列 Blazor特定的鍵盤快速鍵：

   * Windows 上的 `Shift+Alt+D`
   * macOS 上的 `Shift+Cmd+D`

   如果您收到 [**找不到可調試的瀏覽器]** 索引標籤，請參閱[啟用遠端偵錯](#enable-remote-debugging)。
   
   啟用遠端偵錯之後：
   
   1\. 新的瀏覽器視窗隨即開啟。 關閉先前的視窗。

   2\. 將鍵盤焦點放在應用程式的瀏覽器視窗中。

   3\. 在新的瀏覽器視窗中選取 Blazor特定的鍵盤快速鍵：在 Windows 上 `Shift+Alt+D`，或在 macOS 上 `Shift+Cmd+D`。

   4\. [ **DevTools** ] 索引標籤會在瀏覽器中開啟。 **在瀏覽器視窗中重新選擇應用程式的索引標籤。**

   若要將應用程式附加至 Visual Studio，請參閱[Visual Studio 中的附加至進程](#attach-to-process-in-visual-studio)一節。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 藉由將 `--configuration Debug` 選項傳遞至[dotnet 執行](/dotnet/core/tools/dotnet-run)命令，在 `Debug` 設定中執行 Blazor WebAssembly 應用程式： `dotnet run --configuration Debug`。
1. 在 shell 視窗中顯示的 HTTP URL 流覽至應用程式。
1. 將鍵盤焦點放在應用程式，而不是 [開發人員工具] 面板。 最好讓開發人員工具面板在此程式中關閉。 啟動偵錯工具之後，您可以重新開啟 [開發人員工具] 面板。
1. 選取下列 Blazor特定的鍵盤快速鍵：

   * Windows 上的 `Shift+Alt+D`
   * macOS 上的 `Shift+Cmd+D`

   如果您收到 [**找不到可調試的瀏覽器]** 索引標籤，請參閱[啟用遠端偵錯](#enable-remote-debugging)。
   
   啟用遠端偵錯之後：
   
   1\. 新的瀏覽器視窗隨即開啟。 關閉先前的視窗。

   2\. 將鍵盤焦點放在應用程式的瀏覽器視窗中，而不是 [開發人員工具] 面板。

   3\. 在新的瀏覽器視窗中選取 Blazor特定的鍵盤快速鍵：在 Windows 上 `Shift+Alt+D`，或在 macOS 上 `Shift+Cmd+D`。

---

## <a name="enable-remote-debugging"></a>啟用遠端偵錯

如果已停用遠端偵錯程式，則 Chrome 會產生 [**找不到可調試的瀏覽器]** 索引標籤錯誤頁面。 錯誤頁面包含執行 Chrome 並開啟偵錯工具埠的指示，讓 Blazor 的偵錯工具 proxy 可以連接到應用程式。 *關閉所有 Chrome 實例*，並依指示重新開機 Chrome。

## <a name="debug-the-app"></a>偵錯應用程式

當 Chrome 在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵會開啟新的 [偵錯工具]經過一段時間之後，[**來源**] 索引標籤會顯示應用程式中的 .net 元件清單。 展開每個元件，並找出可用來進行偵錯工具的 *.cs*/ *. razor*原始程式檔。 設定中斷點、切換回應用程式的索引標籤，並在程式碼執行時叫用中斷點。 叫用中斷點之後，透過程式碼或繼續（`F8`）程式碼執行的單一步驟（`F10`）。

Blazor 提供可執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並以擴充通訊協定的偵錯工具 proxy。NET 特定資訊。 當您按下 [調試鍵盤快速鍵] 時，Blazor 會指向位於 proxy 的 Chrome DevTools。 Proxy 會連線到您想要進行調試的瀏覽器視窗（因此需要啟用遠端偵錯）。

## <a name="attach-to-process-in-visual-studio"></a>附加至 Visual Studio 中的進程

在 Visual Studio 中附加至應用程式的進程，是 Blazor WebAssembly 的*暫時性*偵錯工具，而**F5**調試正在開發中。

若要將執行中應用程式的進程附加至 Visual Studio：

1. 在 Visual Studio 中，選取  **Debug** > **附加至進程**。
1. 針對 [連線**類型**]，選取 [ **Chrome devtools protocol websocket （無驗證）** ]。
1. 針對 [連線**目標**]，貼上應用程式的 HTTP 位址（而非 HTTPS 位址）。
1. 選取 **[** 重新整理] 以重新整理 [**可用的進程**] 底下的專案。
1. 選取瀏覽器進程以進行偵錯工具，然後選取 [**附加**]。
1. 在 [**選取程式碼類型**] 對話方塊中，選取您要附加到（Edge 或 Chrome）之特定瀏覽器的程式碼類型，然後選取 **[確定]** 。

## <a name="browser-source-maps"></a>瀏覽器來源對應

瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。 不過，Blazor 目前不會C#直接對應至 JAVASCRIPT/WASM。 相反地，Blazor 會在瀏覽器中執行 IL 轉譯，因此來源對應不相關。

## <a name="troubleshoot"></a>疑難排解

如果您遇到錯誤，下列秘訣可能會有説明：

在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。 在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。
