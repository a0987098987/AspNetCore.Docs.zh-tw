---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: 啟用 SignalR 追蹤 |Microsoft Docs
author: tfitzmac
description: 本文件說明如何啟用和設定追蹤 SignalR 伺服器和用戶端。 追蹤可讓您檢視事件的診斷資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 7f7401814e8ca9e61e62a3ebf993b03f92570984
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373239"
---
<a name="enabling-signalr-tracing"></a>啟用 SignalR 追蹤
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文件說明如何啟用和設定追蹤 SignalR 伺服器和用戶端。 追蹤可讓您檢視 SignalR 應用程式中的 診斷事件的資訊。
> 
> 本主題最初是由 Patrick Fletcher 寫入。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


當啟用追蹤時，SignalR 應用程式會建立事件的記錄項目。 您可以從用戶端和伺服器記錄事件。 追蹤伺服器記錄檔的連接、 向外延展提供者，以及訊息匯流排事件。 追蹤用戶端記錄檔的連接事件。 SignalR 2.1 和更新版本用戶端上的追蹤記錄中樞叫用訊息的完整內容。

## <a name="contents"></a>內容

- [啟用在伺服器上的追蹤](#server)

    - [伺服器事件記錄到文字檔](#server_text)
    - [伺服器事件記錄到事件記錄檔](#server_eventlog)
- [在.NET 用戶端 （Windows 桌面應用程式） 中啟用追蹤](#net_client)

    - [桌面用戶端事件記錄到主控台](#desktop_console)
    - [桌面用戶端事件記錄到文字檔](#desktop_text)
- [在 Windows Phone 8 的用戶端中啟用追蹤](#phone)

    - [Windows Phone 用戶端事件記錄到 UI](#phone_ui)
    - [Windows Phone 用戶端事件記錄到偵錯主控台](#phone_debug)
- [在 JavaScript 用戶端中啟用追蹤](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>啟用在伺服器上的追蹤

啟用應用程式的組態檔 （App.config 或 Web.config，視專案類型而定。） 內的伺服器上的追蹤您指定您想要記錄哪些事件的類別。 在組態檔中，您也指定是否將事件記錄到文字檔、 Windows 事件記錄檔或使用的實作自訂記錄檔[TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx)。

伺服器事件類別目錄包含下列類型的訊息：

| 原始程式檔 | 訊息 |
| --- | --- |
| SignalR.SqlMessageBus | SQL 訊息匯流排範圍外的提供者安裝程式、 資料庫作業、 錯誤和逾時事件 |
| SignalR.ServiceBusMessageBus | 服務匯流排範圍外提供者建立主題和訂用帳戶、 錯誤和訊息事件 |
| SignalR.RedisMessageBus | Redis 範圍外提供者的連接、 中斷連線和錯誤事件 |
| SignalR.ScaleoutMessageBus | 向外延展訊息事件 |
| SignalR.Transports.WebSocketTransport | WebSocket 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.LongPollingTransport | LongPolling 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.TransportHeartBeat | 傳輸連線、 中斷連線和 keepalive 事件 |
| SignalR.ReflectedHubDescriptorProvider | 中樞探索事件 |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>伺服器事件記錄到文字檔

下列程式碼示範如何啟用追蹤每個事件類別。 此範例會設定應用程式事件記錄到文字檔。

**XML 伺服器程式碼啟用追蹤**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

在上述程式碼`SignalRSwitch`項目會指定[TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx)用於傳送至指定的記錄檔的事件。 在此情況下，它會設定為`Verbose`這表示所有的偵錯和追蹤訊息會記錄。

下列輸出顯示的項目`transports.log.txt`使用上述的組態檔的應用程式的檔案。 它會顯示新的連線、 已移除的連線和傳輸活動訊號事件。

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>伺服器事件記錄到事件記錄檔

若要將事件記錄到事件記錄檔，而不是文字檔案，變更的值中的項目`sharedListeners`節點。 下列程式碼顯示如何將伺服器事件記錄到事件記錄檔：

**事件記錄到事件記錄檔的 XML 伺服器程式碼**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

事件會記錄在應用程式記錄檔中，而且都是透過事件檢視器中，如下所示：

![顯示 SignalR 記錄的事件檢視器](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> 當使用事件記錄檔，設定**TraceLevel**來**錯誤**將可管理的訊息數目。

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>在.NET 用戶端 （Windows 桌面應用程式） 中啟用追蹤

.NET 用戶端可以記錄事件，主控台中，文字檔案，或使用的實作自訂記錄檔[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)。

若要啟用登入的.NET 用戶端，設定連接的`TraceLevel`屬性，以[TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)的值，而`TraceWriter`屬性設為有效[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)執行個體。

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>桌面用戶端事件記錄到主控台

下列 C# 程式碼示範如何在.NET 用戶端主控台中記錄事件：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>桌面用戶端事件記錄到文字檔

下列 C# 程式碼示範如何在.NET 用戶端至文字檔案中記錄事件：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

下列輸出顯示的項目`ClientLog.txt`使用上述的組態檔的應用程式的檔案。 它會顯示用戶端連接到伺服器，而且中樞叫用用戶端方法呼叫`addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>在 Windows Phone 8 的用戶端中啟用追蹤

Windows Phone 應用程式的 SignalR 應用程式相同的.NET 用戶端做為桌面應用程式，但[Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx)和寫入的檔案[StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx)不提供。 相反地，您需要建立的自訂實作[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)進行追蹤。 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Windows Phone 用戶端事件記錄到 UI

[SignalR 程式碼基底](https://github.com/SignalR/SignalR/archive/master.zip)包含寫入追蹤輸出的 Windows Phone 範例[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)使用自訂[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)稱為實作`TextBlockWriter`。 這個類別可在**samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**專案。 建立的執行個體時`TextBlockWriter`，傳入目前[SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)，以及[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)它將在其中建立[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)来用於追蹤輸出：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

追蹤輸出則會寫入至新[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)中建立[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)您傳入：

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Windows Phone 用戶端事件記錄到偵錯主控台

若要將輸出傳送至偵錯主控台，而不是 UI 中，建立 [的實作[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) ，將寫入至偵錯] 視窗中，並將它指派給您的連線[TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)屬性：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

然後在 Visual Studio 中的 [偵錯] 視窗會寫入追蹤資訊：

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>在 JavaScript 用戶端中啟用追蹤

若要啟用用戶端登入連線，請設定`logging`屬性上的連接物件，然後再呼叫`start`方法來建立連線。

**用戶端 JavaScript 程式碼啟用追蹤，以瀏覽器主控台 （與產生的 proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**用戶端 JavaScript 程式碼啟用追蹤，以瀏覽器主控台 （不含產生的 proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

當啟用追蹤時，JavaScript 用戶端會記錄事件到瀏覽器主控台。 若要存取瀏覽器主控台，請參閱[監視的傳輸](../getting-started/introduction-to-signalr.md#MonitoringTransports)。

下列螢幕擷取畫面顯示 SignalR JavaScript 用戶端已啟用追蹤。 它在瀏覽器主控台中會顯示連線和中樞叫用事件：

![在瀏覽器主控台中的 SignalR 追蹤事件](enabling-signalr-tracing/_static/image4.png)
