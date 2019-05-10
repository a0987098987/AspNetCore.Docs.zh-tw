---
title: 記錄和診斷在 ASP.NET Core SignalR
author: anurse
description: 了解如何從您的 ASP.NET Core SignalR 應用程式收集診斷。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896885"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>記錄和診斷在 ASP.NET Core SignalR

藉由[Andrew Stanton-nurse](https://twitter.com/anurse)

本文提供指引，以收集從您的 ASP.NET Core SignalR 應用程式，來協助疑難排解問題的診斷。

## <a name="server-side-logging"></a>伺服器端記錄功能

> [!WARNING]
> 伺服器端記錄檔可能包含從您的應用程式的機密資訊。 **永遠不會**GitHub 等公開論壇，從生產環境應用程式張貼的未經處理記錄。

SignalR 是 ASP.NET Core 的一部分，因為它會使用 ASP.NET Core 記錄系統。 在預設組態中，SignalR 資訊非常少，但這可以設定的記錄。 請參閱文件[ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration)如需設定 ASP.NET Core 記錄的詳細資訊。

SignalR 使用兩個記錄器類別：

* `Microsoft.AspNetCore.SignalR` -為與中樞通訊協定相關的記錄檔，啟用 中樞、 叫用方法和其他中樞相關的活動。
* `Microsoft.AspNetCore.Http.Connections` -傳輸，例如 WebSockets、 長時間輪詢和 Server-Sent 事件以及低階 SignalR infrastructure 相關的記錄檔。

若要啟用詳細的記錄檔從 SignalR，設定這兩個上述的前置詞`Debug`層級中您`appsettings.json`檔案中的新增下列項目，以`LogLevel`子區段中`Logging`:

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

您也可以設定此程式碼中您`CreateWebHostBuilder`方法：

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

如果您未使用 JSON 為基礎的組態，設定下列設定值，組態系統中：

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

請檢查您的組態系統，以判斷如何指定巢狀的組態值的文件。 例如，當使用環境變數，兩個`_`字元來取代`:`(例如： `Logging__LogLevel__Microsoft.AspNetCore.SignalR`)。

我們建議使用`Debug`收集更詳細的診斷您的應用程式時，層級。 `Trace`層級會產生非常低層級的診斷和很少會需要診斷應用程式中的問題。

## <a name="access-server-side-logs"></a>存取伺服器端記錄檔

存取伺服器端記錄檔的方式取決於您執行環境。

### <a name="as-a-console-app-outside-iis"></a>IIS 外部的主控台應用程式

如果您執行主控台應用程式中[主控台記錄器](xref:fundamentals/logging/index#console-provider)應該依預設啟用。 SignalR 記錄會顯示在主控台中。

### <a name="within-iis-express-from-visual-studio"></a>從 Visual Studio 的 IIS Express 中

Visual Studio 會顯示中的記錄輸出**輸出**視窗。 選取  **ASP.NET Core 網頁伺服器**下拉式清單選項。

### <a name="azure-app-service"></a>Azure App Service

啟用 Azure App Service 入口網站的 [診斷記錄] 區段中的 「 應用程式記錄 （檔案系統） 」 選項及設定的層級`Verbose`。 記錄檔應該可從 「 記錄檔資料流 」 服務，以及與您的 App Service 的檔案系統上的記錄檔。 如需詳細資訊，請參閱文件上[Azure 記錄資料流](xref:fundamentals/logging/index#azure-log-streaming)。

### <a name="other-environments"></a>其他環境

如果您在另一個執行環境 （Docker、 Kubernetes、 Windows 服務等），請參閱完整的文件上[ASP.NET Core 記錄](xref:fundamentals/logging/index)如需有關如何設定記錄提供者適合您的環境。

## <a name="javascript-client-logging"></a>JavaScript 用戶端記錄

> [!WARNING]
> 用戶端記錄檔可能包含從您的應用程式的機密資訊。 **永遠不會**GitHub 等公開論壇，從生產環境應用程式張貼的未經處理記錄。

使用 JavaScript 用戶端時，您可以設定使用的記錄選項`configureLogging`方法`HubConnectionBuilder`:

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

若要停用整個記錄，請指定`signalR.LogLevel.None`在`configureLogging`方法。

下表顯示可用的記錄層級的 JavaScript 用戶端。 將記錄層級設定為下列值之一，可讓資料表中在它上方的所有層級，該層級的記錄。

| 層級 | 描述 |
| ----- | ----------- |
| `None` | 會不記錄任何訊息。 |
| `Critical` | 表示在整個應用程式中的失敗的訊息。 |
| `Error` | 表示目前的作業中失敗的訊息。 |
| `Warning` | 表示非嚴重問題的訊息。 |
| `Information` | 參考用訊息。 |
| `Debug` | 適用於偵錯的診斷訊息。 |
| `Trace` | 設計用來診斷特定問題非常詳細的診斷訊息。 |

一旦您設定的詳細資訊，記錄檔會寫入瀏覽器主控台 （或 NodeJS 應用程式中的標準輸出）。

如果您想要將記錄傳送至自訂的記錄系統，您可以提供 JavaScript 物件，實作`ILogger`介面。 必須實作的唯一方法是`log`，它會接受事件的層級和訊息事件相關聯。 例如: 

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>.NET 用戶端記錄

> [!WARNING]
> 用戶端記錄檔可能包含從您的應用程式的機密資訊。 **永遠不會**GitHub 等公開論壇，從生產環境應用程式張貼的未經處理記錄。

若要從.NET 用戶端取得記錄檔，您可以使用`ConfigureLogging`方法`HubConnectionBuilder`。 其運作方式與`ConfigureLogging`方法`WebHostBuilder`和`HostBuilder`。 ASP.NET Core 中，您可以設定您所使用的相同記錄提供者。 不過，您必須手動安裝並啟用個別的記錄提供者 NuGet 封裝。

### <a name="console-logging"></a>主控台記錄

若要讓主控台記錄，新增[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console)封裝。 然後，使用`AddConsole`方法來設定主控台記錄器：

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>偵錯輸出視窗中的記錄

您也可以設定記錄移至**輸出**Visual Studio 中的視窗。 安裝[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug)封裝，然後使用`AddDebug`方法：

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>其他的記錄提供者

SignalR 支援其他的記錄提供者，例如 Serilog、 Seq、 NLog 或任何其他的記錄系統與整合`Microsoft.Extensions.Logging`。 如果您的記錄系統提供`ILoggerProvider`，您也可以將它註冊`AddProvider`:

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>控制項的詳細資訊

如果您在您的應用程式記錄從其他地方，變更的預設層級`Debug`可能太詳細資訊。 若要設定 SignalR 記錄的記錄層級，您可以使用篩選器。 這可以透過程式中大部分的程式碼，在伺服器上的相同方式來：

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>網路追蹤

> [!WARNING]
> 網路追蹤中包含每個應用程式所傳送的訊息完整的內容。 **永遠不會**生產應用程式從 GitHub 等的公共論壇張貼原始的網路追蹤。

如果您遇到問題，網路追蹤有時可以提供許多有用的資訊。 這是特別有用，如果您打算在問題追蹤程式上提出問題。

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>收集網路追蹤使用 Fiddler （偏好選項）

這個方法適用於所有應用程式。

Fiddler 是功能強大的工具，來收集 HTTP 追蹤。 安裝從[telerik.com/fiddler](https://www.telerik.com/fiddler)、 啟動它，然後執行您的應用程式並重現問題。 Fiddler 是適用於 Windows，而且有適用於 macOS 和 Linux 的 beta 版。

如果您使用 HTTPS 連線，有一些額外的步驟，以確保 Fiddler 可以解密 HTTPS 流量。 如需詳細資訊，請參閱 < [Fiddler 文件](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS)。

一旦您已收集追蹤，您可以選擇匯出追蹤**檔案** > **儲存** > **所有工作階段...** 從功能表列。

![Fiddler 從匯出的所有工作階段](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>收集網路追蹤與 tcpdump （macOS 及僅限 Linux）

這個方法適用於所有應用程式。

您可以收集未經處理的 TCP 追蹤使用 tcpdump 從命令殼層執行下列命令。 您可能需要`root`前置詞使用的指令或`sudo`如果權限錯誤：

```console
tcpdump -i [interface] -w trace.pcap
```

取代`[interface]`與您想要在擷取的網路介面。 通常，這會像下面`/dev/eth0`（適用於您標準的乙太網路介面） 或`/dev/lo0`（適用於 localhost 流量）。 如需詳細資訊，請參閱`tcpdump`手冊頁，在主機系統。

## <a name="collect-a-network-trace-in-the-browser"></a>收集網路追蹤，在瀏覽器

這個方法只適用於瀏覽器為基礎的應用程式。

大部分的瀏覽器開發人員工具會有可讓您擷取瀏覽器與伺服器之間的網路活動的 [網路] 索引標籤。 不過，這些追蹤不包含 WebSocket 和 Server-Sent 事件訊息。 如果您使用這些傳輸，使用 Fiddler 或 TcpDump （如下所述） 之類的工具是較好的方法。

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge 及 Internet Explorer

（指示也適用於 Edge 及 Internet Explorer）

1. 按下 F12 以開啟 開發人員工具
2. 按一下 [網路] 索引標籤
3. （如有需要），請重新整理頁面並重現問題
4. 按一下 [匯出為"HAR 」 檔案的追蹤] 工具列中的 [儲存] 圖示：

![儲存圖示上的 Microsoft Edge 開發人員工具 [網路] 索引標籤](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. 按下 F12 以開啟 開發人員工具
2. 按一下 [網路] 索引標籤
3. （如有需要），請重新整理頁面並重現問題
4. 以滑鼠右鍵按一下清單中的要求的任何位置，然後選擇 「 另存為 HAR 內容 」:

![Google Chrome Dev Tools 網路 索引標籤中的 「 另存為 HAR 內容 」 選項](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. 按下 F12 以開啟 開發人員工具
2. 按一下 [網路] 索引標籤
3. （如有需要），請重新整理頁面並重現問題
4. 以滑鼠右鍵按一下清單中的要求的任何位置，然後選擇 「 儲存所有為 HAR"

![Mozilla Firefox 開發人員工具網路] 索引標籤中的 [儲存所有為 HAR 」 選項](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>將診斷檔案附加至 GitHub 問題

您可以將診斷檔案附加至 GitHub 問題重新命名，以便讓他們自己`.txt`延伸模組，然後拖曳和置放即可入問題。

> [!NOTE]
> 請不要貼上網路追蹤或記錄檔的內容 GitHub 問題。 這些記錄檔和追蹤可能會相當大，GitHub 會通常它們截斷。

![拖曳到 GitHub 問題的記錄檔](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>其他資源

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
