---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR 效能 (SignalR 1.x) |Microsoft 文件
author: pfletcher
description: SignalR 效能
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037148"
---
<a name="signalr-performance-signalr-1x"></a>SignalR 效能 (SignalR 1.x)
====================
由[Patrick Fletcher](https://github.com/pfletcher)

> 本主題描述如何設計、 量值，以及改善的 SignalR 應用程式的效能。


SignalR 效能和調整的最新簡報，請參閱[調整透過 ASP.NET SignalR 即時Web</](https://channel9.msdn.com/Events/Build/2013/3-502)。

此主題包括下列章節：

- [設計考量](#design)
- [微調您 SignalR 的伺服器效能](#tuning)
- [疑難排解效能問題](#troubleshooting)
- [使用 SignalR 效能計數器](#perfcounters)
- [使用其他效能計數器](#othercounters)
- [其他資源](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>設計考量

本節說明可以實作在 SignalR 應用程式，以確保不會被效能妨礙藉由產生不必要的網路流量的設計模式。

### <a name="throttling-message-frequency"></a>節流的訊息頻率

即使在送出訊息 （例如即時遊戲應用程式） 的高頻率的應用程式，大部分的應用程式不需要傳送第二個的多個訊息。 若要減少每個用戶端會產生的流量，訊息迴圈可以實作佇列，並送出訊息不超過經常固定 （也就是最高達訊息數目不會傳送每秒中，如果沒有訊息中該時間terval 傳送)。 在特定速率會調整訊息 （來自用戶端和伺服器） 的範例應用程式，請參閱[與 SignalR 的頻率高的即時](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)。

### <a name="reducing-message-size"></a>降低訊息大小

您可以藉由減少序列化物件的大小減少 SignalR 訊息的大小。 在伺服端程式碼，如果您要傳送的物件，其中包含要傳送不需要的屬性，使這些屬性無法序列化使用的`JsonIgnore`屬性。 屬性的名稱也會儲存在訊息。屬性名稱可以使用縮短`JsonProperty`屬性。 下列程式碼範例示範如何排除屬性傳送到用戶端，以及如何縮短屬性名稱：

**.NET 伺服器程式碼，示範 JsonIgnore 屬性，若要排除的資料傳送到用戶端，以及要降低訊息大小的 JsonProperty 屬性**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

若要保留可讀性 / 用戶端程式碼中的可維護性、 收到訊息之後的縮寫的屬性名稱可以是重新對應至人們易記的名稱。 下列程式碼範例示範一個可能方式重新對應縮短的名稱長度的項目，藉由定義訊息合約 （對應），並使用`reMap`来套用的最佳化的訊息類別合約函式：

**重新對應的用戶端 JavaScript 程式碼縮短人類看得懂的名稱屬性名稱**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

名稱可以縮短到伺服器，用戶端的訊息中使用相同的方法。

降低記憶體耗用量 （也就是訊息所使用的記憶體數量） 訊息的物件也可以改善效能。 例如，如果完整的`int`不需要`short`或`byte`可改為使用。

由於訊息會儲存在伺服器記憶體中，以減少的訊息大小的訊息匯流排中也可以處理伺服器的記憶體問題。

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>微調您 SignalR 的伺服器效能

下列組態設定可以用來微調您的伺服器，以提升效能的 SignalR 應用程式中。 如需如何改善 ASP.NET 應用程式效能的一般資訊，請參閱[改善 ASP.NET 效能](https://msdn.microsoft.com/library/ff647787.aspx)。

**SignalR 的組態設定**

- **DefaultMessageBufferSize**： 根據預設，SignalR 會保留在記憶體中每個連線的中樞每 1000年的訊息。 如果使用大型訊息，這可能會產生可以減輕藉由減少此值的記憶體問題。 這項設定可以在中設定`Application_Start`或 ASP.NET 應用程式中的事件處理常式`Configuration`OWIN 啟動類別自我裝載的應用程式中的方法。 下列範例會示範如何以降低您的應用程式，以降低伺服器使用的記憶體數量的記憶體使用量降低此值：

    **伺服器的.NET 程式碼在 Global.asax 中減少預設的訊息緩衝區大小**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS 組態設定**

- **每個應用程式的最大並行要求**： 增加並行的 IIS 要求將會增加伺服器資源可用來服務要求。 預設值為 5000。若要增加此設定，請在提升權限的命令提示字元中執行下列命令：

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**ASP.NET 組態設定**

本節包含可以在中設定的組態設定`aspnet.config`檔案。 找到這個檔案中的兩個位置，根據平台：

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

可能會改善效能 SignalR 的 ASP.NET 設定包括下列各項：

- **每一 CPU 的最大並行要求**： 增加此設定可能會解決效能瓶頸。 若要增加此設定，加入下列組態設定加入`aspnet.config`檔案：

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **要求佇列限制**： 當連線總數超過`maxConcurrentRequestsPerCPU`設定，ASP.NET 將開始節流使用佇列的要求。 若要增加佇列的大小，您可以增加`requestQueueLimit`設定。 若要這樣做，請將加入下列組態設定加入`processModel`節點`config/machine.config`(而非`aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>疑難排解效能問題

本章節描述如何在應用程式中，找出效能瓶頸。

### <a name="verifying-that-websocket-is-being-used"></a>確認正在使用 WebSocket

雖然 SignalR 可以使用各種傳輸用戶端和伺服器之間的通訊，WebSocket 提供顯著的效能優勢，，而且如果用戶端和伺服器支援它，則應該使用。 若要判斷您的用戶端和伺服器是否符合 WebSocket 的需求，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。 若要判斷您的應用程式中使用何種傳輸，您可以使用瀏覽器開發人員工具，並檢查記錄檔，以查看所使用的傳輸正在使用的連接。 如需使用 Internet Explorer 和 Chrome 瀏覽器開發工具的資訊，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>使用 SignalR 效能計數器

本章節描述如何啟用及使用 SignalR 效能計數器、 位於`Microsoft.AspNet.SignalR.Utils`封裝。

### <a name="installing-signalrexe"></a>安裝 signalr.exe

效能計數器可以加入至使用稱為 SignalR.exe 的公用程式的伺服器。 若要安裝此公用程式，請遵循下列步驟：

1. 在 Visual Studio 應用程式中，選取**工具**，**程式庫套件管理員**，**管理方案的 NuGet 套件...**
2. 搜尋**signalr.utils**，並選取 安裝。

    ![](signalr-performance/_static/image1.png)
3. 接受授權合約，若要安裝封裝。
4. SignalR.exe 將會安裝成`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`。

### <a name="installing-performance-counters-with-signalrexe"></a>使用 SignalR.exe 安裝效能計數器

若要安裝 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

若要移除 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR 效能計數器

公用程式封裝會安裝下列效能計數器。 自從最後一個應用程式集區或伺服器重新啟動之後，「 總計 」 計數器會測量事件的數目。

**連接度量**

下列度量會測量所發生的連線存留期事件。 如需詳細資訊，請參閱[了解和處理連線存留期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。

- **連線的連線**
- **重新連線的連線**
- **連線 Disonnected**
- **目前的連線**

**訊息計量**

下列度量會測量 SignalR 所產生的訊息流量。

- **已接收連線郵件總數**
- **連接訊息已傳送總數**
- **連接訊息 Received/Sec**
- **連接訊息傳送數/秒**

**訊息匯流排計量**

下列度量會測量透過內部 SignalR 訊息匯流排中所有的傳入和傳出 SignalR 訊息都會放到的佇列的流量。 訊息是**發佈**何時傳送或廣播。 A**訂閱者**在此內容是在訊息匯流排的訂閱，則這應該會等於用戶端加上伺服器本身的數目。 **配置工作者**是一種元件傳送資料至作用中連線;**忙碌的背景工作**一個主動傳送一則訊息。

- **訊息匯流排的訊息已接收的總計**
- **訊息匯流排的訊息數/秒**
- **已發佈的訊息匯流排訊息總數**
- **訊息匯流排的訊息發行/Sec**
- **目前的訊息匯流排的訂閱者**
- **訊息匯流排的訂閱者總數**
- **訊息匯流排的訂閱者/Sec**
- **配置背景工作的訊息匯流排**
- **訊息匯流排忙碌員工**
- **目前的訊息匯流排主題**

**誤差度量**

下列度量會測量 SignalR 訊息傳輸所產生的錯誤。 **中樞解析**集線器或中樞方法無法解析時，會發生錯誤。 **中樞叫用**錯誤會叫用中樞方法時擲回例外狀況。 **傳輸**錯誤是在 HTTP 要求或回應期間擲回的連接錯誤。

- **錯誤： 所有總計**
- **錯誤： All/Sec**
- **錯誤： 中樞解析總計**
- **錯誤： 中樞解析數/秒**
- **錯誤： 中樞叫用總數**
- **錯誤： 中樞叫用數/秒**
- **錯誤： 傳輸總計**
- **錯誤： 傳輸數/秒**

**範圍外的度量**

下列度量會測量流量和向外延展提供者所產生的錯誤。 A**資料流**在此內容是縮放單位向外延展提供者使用; 如果這是資料表，如果使用 SQL Server、 使用服務匯流排時，如果某個主題和訂用帳戶使用 Redis。 根據預設，只有一個資料流使用，但可以將此增加透過 SQL Server 和服務匯流排上的設定。 A**緩衝處理**資料流是已進入錯誤的狀態，則當資料流處於錯誤狀態時，所有傳送至後擋板的訊息將會失敗，資料流不會再判定為失敗之前，立即。 **傳送佇列長度**是張貼但尚未傳送的訊息數目。

- **範圍外訊息匯流排的訊息數/秒**
- **向外延展資料流總數**
- **向外延展資料流開啟**
- **向外延展資料流緩衝**
- **範圍外錯誤總數**
- **Scaleout Errors/Sec**
- **範圍外傳送佇列長度**

如需有關這些計數器會測量的詳細資訊，請參閱[SignalR 範圍外使用 Azure 服務匯流排](scaleout-with-windows-azure-service-bus.md)。

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>使用其他效能計數器

下列效能計數器也可能在監視應用程式的效能很有用。

**Memory**

- .NET CLR 記憶體中的位元組數全部堆積 （w3wp)

**ASP.NET**

- ASP.NET\Requests Current
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- 處理器 Information\Processor 時間

**TCP/IP**

- TCPv6/建立的連接
- TCPv4/建立的連接

**Web 服務**

- Web Service\Current Connections
- Web Service\Maximum 連線

**執行緒處理**

- .NET CLR LocksAndThreads\#個目前的邏輯執行緒
- .NET CLR LocksAnd 執行緒\#的目前實體的執行緒

<a id="otherresources"></a>

## <a name="other-resources"></a>其他資源

如需 ASP.NET 效能監視與微調的詳細資訊，請參閱下列主題：

- [ASP.NET 效能概觀](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [IIS 7.5、 IIS 7.0 和 IIS 6.0 的 ASP.NET 執行緒用法](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;應用程式集區&gt;項目 （Web 設定）](https://msdn.microsoft.com/library/dd560842.aspx)
