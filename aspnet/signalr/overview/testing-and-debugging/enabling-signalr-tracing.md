---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: 啟用 SignalR 追蹤 |Microsoft 文件
author: tfitzmac
description: 本文件說明如何啟用及設定 SignalR 的伺服器和用戶端的追蹤。 追蹤可讓您檢視事件的診斷資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28032814"
---
<a name="enabling-signalr-tracing"></a>啟用 SignalR 追蹤
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文件說明如何啟用及設定 SignalR 的伺服器和用戶端的追蹤。 追蹤可讓您在 SignalR 應用程式中檢視事件的診斷資訊。
> 
> 本主題最初是 Patrick Fletcher 所撰寫。
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
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


當啟用追蹤時，SignalR 應用程式會建立事件記錄項目。 您可以從用戶端和伺服器記錄事件。 追蹤對伺服器記錄檔的連接、 向外延展提供者，以及訊息匯流排的事件。 追蹤對用戶端記錄檔的連接事件。 SignalR 2.1 和更新版本，用戶端上的追蹤記錄中樞叫用訊息完整的內容。

## <a name="contents"></a>內容

- [在伺服器上啟用追蹤](#server)

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
## <a name="enabling-tracing-on-the-server"></a>在伺服器上啟用追蹤

啟用應用程式的組態檔 （App.config 或 Web.config，視專案類型而定。） 內的伺服器上的追蹤您可以指定您想要記錄的事件分類。 在組態檔中，您也指定是否將事件記錄到文字檔、 Windows 事件記錄檔或自訂的記錄檔使用的實作[TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx)。

Server 事件類別目錄包含下列排序的訊息：

| 原始程式檔 | 訊息 |
| --- | --- |
| SignalR.SqlMessageBus | SQL 訊息匯流排範圍外的 provider 安裝程式、 資料庫作業、 錯誤和事件逾時 |
| SignalR.ServiceBusMessageBus | 服務匯流排範圍外提供者建立主題和訂用帳戶、 錯誤和訊息事件 |
| SignalR.RedisMessageBus | Redis 範圍外提供者的連接、 中斷連線和錯誤事件 |
| SignalR.ScaleoutMessageBus | 範圍外訊息事件 |
| SignalR.Transports.WebSocketTransport | WebSocket 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.LongPollingTransport | LongPolling 傳輸連線、 中斷連線、 訊息和錯誤事件 |
| SignalR.Transports.TransportHeartBeat | 傳輸連線、 中斷連線，以及 keepalive 事件 |
| SignalR.ReflectedHubDescriptorProvider | 中樞探索事件 |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>伺服器事件記錄到文字檔

下列程式碼會示範如何啟用追蹤每一種事件類別。 這個範例會設定應用程式事件記錄到文字檔。

**XML 伺服器程式碼啟用追蹤**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

在上述程式碼中`SignalRSwitch`項目會指定[TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx)用於傳送至指定的記錄檔的事件。 在此情況下，設定為`Verbose`這表示所有偵錯和追蹤訊息會記錄。

下列的輸出顯示的項目`transports.log.txt`使用以上的組態檔的應用程式的檔案。 它會顯示新的連接、 移除的連接和傳輸活動訊號事件。

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>伺服器事件記錄到事件記錄檔

若要將事件記錄到事件記錄檔，而不是文字檔案，變更的值中的項目`sharedListeners`節點。 下列程式碼會示範如何在伺服器事件記錄到事件記錄檔：

**事件記錄到事件記錄檔的 XML 伺服器程式碼**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

事件會記錄在應用程式記錄檔中，而且都是透過事件檢視器中，如下所示：

![事件檢視器顯示 SignalR 記錄檔](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> 事件記錄檔時，設定**TraceLevel**至**錯誤**保留可管理的訊息數目。

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>在.NET 用戶端 （Windows 桌面應用程式） 中啟用追蹤

.NET 用戶端可以在主控台中，文字檔案，或使用的實作自訂的記錄檔記錄事件[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)。

若要啟用記錄的.NET 用戶端中，設定連接的`TraceLevel`屬性[TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)值，而`TraceWriter`屬性設為有效[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)執行個體。

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>桌面用戶端事件記錄到主控台

下列 C# 程式碼會示範如何在.NET 用戶端主控台中記錄事件：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>桌面用戶端事件記錄到文字檔

下列 C# 程式碼會示範如何在.NET 用戶端至文字檔案中記錄事件：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

下列的輸出顯示的項目`ClientLog.txt`使用以上的組態檔的應用程式的檔案。 它會顯示連接到伺服器的用戶端與中樞叫用用戶端方法呼叫`addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>在 Windows Phone 8 的用戶端中啟用追蹤

Windows Phone 應用程式的 SignalR 應用程式相同的.NET 用戶端做為桌面應用程式，但[Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx)和寫入的檔案[StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx)則無法使用。 相反地，您需要建立的自訂實作[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)進行追蹤。 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Windows Phone 用戶端事件記錄到 UI

[SignalR 程式碼基底](https://github.com/SignalR/SignalR/archive/master.zip)包含寫入追蹤輸出的 Windows Phone 範例[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)使用自訂[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)實作呼叫`TextBlockWriter`。 這個類別可以位於**samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**專案。 建立的執行個體時`TextBlockWriter`，傳入目前[SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)，和[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)它將在其中建立[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)来用於追蹤輸出：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

然後將追蹤輸出寫入至新[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)中建立[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)您傳入：

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Windows Phone 用戶端事件記錄到偵錯主控台

若要將輸出傳送至偵錯主控台，而不是在 UI 中，建立的實作[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) ，寫入至偵錯視窗，並將它指派給您的連線[TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)屬性：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

然後在 Visual Studio 中偵錯視窗會寫入追蹤資訊：

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>在 JavaScript 用戶端中啟用追蹤

若要啟用用戶端登入連線，`logging`屬性上的連接物件，才能呼叫`start`方法，以建立連接。

**用戶端 JavaScript 程式碼啟用追蹤，以瀏覽器主控台 （與產生的 proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**用戶端 JavaScript 程式碼啟用追蹤，以瀏覽器主控台 （而不產生的 proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

當啟用追蹤時，JavaScript 用戶端會記錄事件到瀏覽器主控台。 若要存取的瀏覽器主控台，請參閱[監視傳輸](../getting-started/introduction-to-signalr.md#MonitoringTransports)。

下列螢幕擷取畫面顯示 SignalR JavaScript 用戶端啟用追蹤。 它在瀏覽器主控台中會顯示連線和中樞叫用事件：

![在瀏覽器主控台中的 SignalR 追蹤事件](enabling-signalr-tracing/_static/image4.png)
