---
uid: signalr/overview/performance/signalr-performance
title: SignalR 效能 |Microsoft Docs
author: pfletcher
description: SignalR 效能
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 9346f0ff9720361f07afe196f59305f0f38ffe8a
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287765"
---
<a name="signalr-performance"></a>SignalR 效能
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本主題描述如何設計、 測量和改善的 SignalR 應用程式中的效能。
>
> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR 第 2 版
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>本主題的上一個版本
>
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
>
> ## <a name="questions-and-comments"></a>提出問題或意見
>
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


SignalR 效能和調整的近期簡報，請參閱 <<c0> [ 調整與 ASP.NET SignalR 即時 Web](https://channel9.msdn.com/Events/Build/2013/3-502)。

此主題包括下列章節：

- [設計考量](#design)
- [微調 SignalR 伺服器效能](#tuning)
- [針對效能問題進行疑難排解](#troubleshooting)
- [使用 SignalR 效能計數器](#perfcounters)
- [使用其他的效能計數器](#othercounters)
- [其他資源](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>設計考量

本章節描述可實作 SignalR 應用程式，以確保效能，正在降低藉由產生不必要的網路流量的設計期間的模式。

### <a name="throttling-message-frequency"></a>節流的訊息頻率

即使在送出訊息 （例如即時遊戲應用程式） 的高頻率的應用程式，大部分的應用程式不需要傳送第二個的多個訊息。 若要減少每個用戶端產生的流量，訊息迴圈可以實作佇列，並送出訊息不超過經常以固定費率 （也就是特定數目的訊息將會傳送到每秒中，是否在該時間中有訊息terval 傳送)。 節流的訊息，以特定的費率 （從用戶端和伺服器） 的範例應用程式，請參閱[高頻率即時與 SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)。

### <a name="reducing-message-size"></a>減少訊息大小

您可以藉由減少您序列化物件的大小減少 SignalR 訊息的大小。 在伺服器程式碼，如果您要傳送的物件，其中包含要傳送不需要的屬性，會阻止這些屬性序列化使用`JsonIgnore`屬性。 屬性名稱也會儲存在訊息;屬性名稱可以使用縮短`JsonProperty`屬性。 下列程式碼範例示範如何排除屬性傳送至用戶端，以及如何縮短屬性名稱：

**.NET 伺服器程式碼，示範 JsonIgnore 屬性，若要排除的資料傳送至用戶端，以及減少訊息大小，JsonProperty 屬性**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

若要保留可讀性 / 用戶端程式碼中的可維護性，在收到訊息之後的縮寫的屬性名稱可能會以人類易重新對應的名稱。 下列程式碼範例示範一個可能方式重新對應的簡短的名稱，以較長的藉由定義訊息合約 （對應），並使用`reMap`来套用的最佳化的訊息類別合約函式：

**重新對應的用戶端 JavaScript 程式碼已縮短人類看得懂的名稱的屬性名稱**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

名稱可以縮短在用戶端的訊息至伺服器，使用相同的方法。

減少記憶體使用量 （也就是訊息所使用的記憶體數量） 訊息的物件也可以改善效能。 例如，如果的全套`int`不需要`short`或`byte`可以改為使用。

由於訊息會儲存在伺服器記憶體中，進而減少訊息大小的訊息匯流排也可以處理伺服器的記憶體問題。

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>微調 SignalR 伺服器效能

下列組態設定可用來微調您的伺服器中的 SignalR 應用程式更佳的效能。 如需如何在 ASP.NET 應用程式中改善效能的一般資訊，請參閱[提升 ASP.NET 效能](https://msdn.microsoft.com/library/ff647787.aspx)。

**SignalR 組態設定**

- **DefaultMessageBufferSize**:根據預設，SignalR 會保留在記憶體中每個連線的中樞每 1000 則訊息。 如果使用大型訊息時，這可能會產生記憶體問題可以降低此值可以緩解這些問題。 這項設定可以在中設定`Application_Start`事件處理常式在 ASP.NET 應用程式，或`Configuration`的 OWIN 啟動類別中的自我裝載的應用程式的方法。 下列範例示範如何減少此值，以降低您的應用程式，以降低伺服器使用的記憶體數量的記憶體使用量：

    **.NET 伺服器程式碼在 Startup.cs 中的減少預設的訊息緩衝區大小**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS 組態設定**

- **每個應用程式的最大並行要求**:增加並行的 IIS 要求將會增加可用的服務要求的伺服器資源。 預設值為 5000。若要增加此設定，請在提升權限的命令提示字元執行下列命令：

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**:這是 Http.sys 的應用程式集區排入佇列的要求數目上限。 當佇列已滿時，新的要求就會收到 503 「 服務無法使用 」 回應。 預設值為 1000。

    縮短工作者處理序中裝載您的應用程式的應用程式集區佇列長度，將會節省記憶體資源。 如需詳細資訊，請參閱 <<c0> [ 管理、 調整，並設定應用程式集區](https://technet.microsoft.com/library/cc745955.aspx)。

**ASP.NET 組態設定**

本節包含可在設定的組態設定`aspnet.config`檔案。 這個檔案位於其中一個位置，根據平台：

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

可能會改善 SignalR 效能的 ASP.NET 設定包括下列各項：

- **每一 CPU 的最大並行要求**:增加此設定，可能有助於解決效能瓶頸。 若要增加此設定，新增下列組態設定加入`aspnet.config`檔案：

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **要求佇列限制**:當連線總數超過`maxConcurrentRequestsPerCPU`設定時，ASP.NET 會開始節流使用佇列的要求。 若要增加佇列的大小，您可以增加`requestQueueLimit`設定。 若要這樣做，請新增下列組態設定加入`processModel`中的節點`config/machine.config`(而非`aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>針對效能問題進行疑難排解

本章節描述如何在您的應用程式中，找出效能瓶頸。

### <a name="verifying-that-websocket-is-being-used"></a>驗證正在使用 WebSocket

雖然 SignalR 可以使用各種傳輸用戶端與伺服器之間的通訊，WebSocket 提供顯著的優勢，並應在用戶端和伺服器支援它。 若要判斷您的用戶端和伺服器是否符合 WebSocket 的需求，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。 若要判斷何種傳輸正用於您的應用程式，您可以使用瀏覽器的開發人員工具，，並檢查記錄，以查看所使用的傳輸用於連接。 如需使用瀏覽器的開發工具，在 Internet Explorer 和 Chrome 中的資訊，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>使用 SignalR 效能計數器

本節說明如何啟用和使用 SignalR 效能計數器、 位於`Microsoft.AspNet.SignalR.Utils`封裝。

### <a name="installing-signalrexe"></a>安裝 signalr.exe

效能計數器可以加入至使用稱為 SignalR.exe 的公用程式的伺服器。 若要安裝此公用程式，請遵循下列步驟：

1. 在 Visual Studio 中，選取**工具** > **NuGet 套件管理員** > **管理方案的 NuGet 套件**
2. 搜尋**signalr.utils**，並選取 [安裝]。

    ![](signalr-performance/_static/image1.png)
3. 接受授權合約，來安裝封裝。
4. SignalR.exe 將會安裝成`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`。

### <a name="installing-performance-counters-with-signalrexe"></a>使用 SignalR.exe 安裝效能計數器

若要安裝 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

若要移除 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR 效能計數器

公用程式封裝會安裝下列效能計數器。 自從最後一個應用程式集區或伺服器重新啟動之後，「 總計 」 計數器會測量事件的數目。

**連線計量**

下列度量會測量所發生的連線存留期事件。 如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。

- **連線的連線**
- **重新連線的連線**
- **連線已中斷連線**
- **目前的連線**

**訊息計量**

下列度量會測量產生 SignalR 的訊息流量。

- **已接收連線的郵件總數**
- **連接訊息已傳送總數**
- **連接訊息 Received/Sec**
- **連線的訊息傳送/秒**

**訊息匯流排計量**

下列度量會測量流量通過內部 SignalR 訊息匯流排，位於所有傳入和傳出的 SignalR 訊息的佇列。 訊息**已發佈**當它是傳送或廣播。 A**訂閱者**在此內容是在訊息匯流排上的訂用帳戶，這應該符合用戶端，再加上伺服器本身的數目。 **配置的背景工作角色**是一種元件，將資料傳送至作用中連線;**忙碌的背景工作角色**是指是否主動傳送一則訊息。

- **訊息匯流排的訊息接收的總計**
- **訊息匯流排的訊息數/秒**
- **已發佈的訊息匯流排訊息總計**
- **訊息匯流排郵件發佈數/秒**
- **目前的訊息匯流排的訂閱者**
- **訊息匯流排的訂閱者總數**
- **訊息匯流排的訂閱者/Sec**
- **訊息匯流排配置的背景工作角色**
- **訊息匯流排忙碌的背景工作角色**
- **目前的訊息匯流排的主題**

**誤差度量**

下列度量會測量 SignalR 訊息傳輸所產生的錯誤。 **中樞解析**中樞或中樞方法無法解析時，會發生錯誤。 **中樞叫用**錯誤是在叫用中樞方法時擲回的例外狀況。 **傳輸**錯誤是在 HTTP 要求或回應期間擲回的連線錯誤。

- **錯誤：所有總計**
- **錯誤：All/Sec**
- **錯誤：中樞解析總計**
- **錯誤：每秒中樞解析**
- **錯誤：中樞叫用總數**
- **錯誤：每秒中樞叫用**
- **錯誤：傳輸的總數**
- **錯誤：傳輸/秒**

<a id="scaleout_metrics"></a>

**向外延展計量**

下列度量會測量流量和向外延展提供者所產生的錯誤。 A **Stream**在此內容中向外延展提供者所使用的縮放單位; 這是資料表，如果使用 SQL Server、 使用服務匯流排時，如果某個主題和訂用帳戶如果使用 Redis。 每個資料流可確保已排序的讀取和寫入作業;單一資料流是潛在擴展瓶頸，因此可以增加的資料流數目，以減少該瓶頸。 如果使用多個資料流，SignalR 會自動分散方式，從指定的任何連線傳送訊息的順序可確保這些資料流 （分區） 訊息。

[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)設定可以控制維護由 SignalR 範圍外傳送佇列的長度。 將它設定為值大於 0 會將傳送一次一個已設定的訊息後擋板以在傳送佇列中的所有訊息。 如果佇列的大小超過設定的長度，後續的呼叫，將會失敗，立即[InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx)直到佇列中的訊息數目未超過設定一次。 因為實作的背板通常採用自己的佇列或流量控制，所以預設會停用佇列。 在 SQL Server 的情況下連接共用實際上會限制將在上一次的傳送數目。

根據預設，只有一個資料流用於 SQL Server 和 Redis、 五個資料流用於服務匯流排佇列已停用，但可以變更這些設定，透過 SQL Server 和服務匯流排的組態：

**.NET 伺服器程式碼來設定資料表的 SQL Server 後擋板的計數和佇列長度**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**.NET 伺服器程式碼來設定服務匯流排後擋板的主題計數和佇列長度**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

A**緩衝處理**資料流是已進入錯誤的狀態; 當資料流處於錯誤狀態，所有傳送至後擋板的訊息將會失敗，資料流不會再判定為失敗之前，立即。 **傳送佇列長度**是張貼但尚未傳送的訊息數目。

- **範圍外訊息匯流排的訊息數/秒**
- **向外延展資料流總數**
- **向外延展資料流開啟**
- **向外延展資料流緩衝**
- **範圍外錯誤總數**
- **範圍外錯誤數/秒**
- **範圍外傳送佇列長度**

如需有關這些計數器會測量的詳細資訊，請參閱[SignalR 向外延展與 Azure 服務匯流排](scaleout-with-windows-azure-service-bus.md)。

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>使用其他的效能計數器

下列效能計數器也可能在監視應用程式的效能很有用。

**記憶體**

- .NET CLR Memory\\中全部堆積 （w3wp) 的 # bytes

**ASP.NET**

- Asp.net\requests Current
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- 處理器 Information\Processor 時間

**TCP/IP**

- Tcpv6-已/建立的連接
- TCPv4/建立的連接

**Web 服務**

- Web Service\Current Connections
- Web Service\Maximum 連線

**執行緒處理**

- .NET CLR 鎖定和執行緒\\# 個目前的邏輯執行緒
- .NET CLR 鎖定和執行緒\\# 個目前的實體執行緒

<a id="otherresources"></a>

## <a name="other-resources"></a>其他資源

如需有關 ASP.NET 的效能監視和調整的詳細資訊，請參閱下列主題：

- [ASP.NET 效能概觀](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [IIS 7.5，IIS 7.0 和 IIS 6.0 的 ASP.NET 執行緒用法](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt;項目 （Web 設定）](https://msdn.microsoft.com/library/dd560842.aspx)
