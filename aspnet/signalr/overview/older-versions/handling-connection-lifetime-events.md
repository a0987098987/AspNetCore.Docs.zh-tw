---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: 了解和處理 SignalR 連線的存留期事件 1.x |Microsoft 文件
author: pfletcher
description: 本文說明如何使用集線器 API 所公開的事件。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 4fe77769c27dd46967da2e1d68791d7142021d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036722"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>了解和處理 SignalR 連線的存留期事件 1.x
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文章提供 SignalR 連線、 重新連線，並中斷連線事件，您可以控制它們，以及您可以設定的逾時和 keepalive 設定的總覽。
> 
> 本文假設您已經有 SignalR 和連線的存留期事件的一些知識。 如需 SignalR 的簡介，請參閱[SignalR-概觀-快速入門](index.md)。 連線的存留期事件的清單，請參閱下列資源：
> 
> - [如何處理連接的存留期事件中樞類別中](index.md)
> - [如何處理在 JavaScript 用戶端連線存留期事件](index.md)
> - [如何處理在.NET 用戶端連線的存留期事件](index.md)


## <a name="overview"></a>總覽

本文包含下列章節：

- [連線的存留期的術語和案例](#terminology)

    - [SignalR 連線傳輸、 連接和實體連接](#signalrvstransport)
    - [傳輸中斷連線的案例](#transportdisconnect)
    - [用戶端中斷連接案例](#clientdisconnect)
    - [伺服器正在中斷連接案例](#serverdisconnect)
- [逾時和 keepalive 設定](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [如何變更逾時和 keepalive 設定](#changetimeout)
- [如何通知使用者有關的連線中斷](#notifydisconnect)
- [如何重新持續連線](#continuousreconnect)
- [如何在伺服器程式碼中的用戶端中斷連接](#disconnectclientfromserver)

應用程式開發介面參考主題的連結是.NET 4.5 版的 API。 如果您使用.NET 4，請參閱[API 主題.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>連線的存留期的術語和案例

`OnReconnected` SignalR 中樞中的事件處理常式可以直接在後執行`OnConnected`但不是晚`OnDisconnected`針對給定的用戶端。 您可以重新連線，而不中斷的原因是 「 連接 」 這個字用於 SignalR 的數種方式。

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR 連線傳輸、 連接和實體連接

這篇文章會區分*SignalR 連線*，*傳輸連線*，和*實體連線*:

- **SignalR 連線**參考之間的用戶端和伺服器 URL、 維護的 SignalR 應用程式開發介面和唯一連接識別碼所識別的邏輯關聯性 此關聯性相關的資料保留由 SignalR 中，用來建立傳輸連接。 關聯性端點和 SignalR 的資料時處理用戶端呼叫`Stop`SignalR 嘗試重新建立遺失的傳輸連線期間達到方法或逾時限制。
- **傳輸連線**用戶端與伺服器之間，其中四個傳輸應用程式開發介面所維護的邏輯關聯性是指： WebSockets，伺服器傳送事件，永久框架時，或長時間輪詢。 SignalR 使用傳輸應用程式開發介面來建立傳輸連接，並傳輸 API 取決於建立傳輸連接的實體網路連線存在。 傳輸連線結束 SignalR 終止它時，或當應用程式開發介面的傳輸偵測到的實體連接已中斷。
- **實體連接**指的是實體網路連結--線路，無線訊號，路由器等-可簡化用戶端電腦與伺服器電腦之間的通訊。 實體的連線必須要有才能建立傳輸連接，且必須以建立 SignalR 連線建立傳輸連接。 不過，中斷的實體連接不都會立即結束的傳輸連線或 SignalR 連線，將在本主題稍後說明。

在下列圖表中，由 SignalR 連線的中樞應用程式開發介面和 PersistentConnection API SignalR 層、 傳輸連線由傳輸層中，而實體連接由伺服器之間的線條和用戶端。

![SignalR 架構圖表](handling-connection-lifetime-events/_static/image1.png)

當您呼叫`Start`方法 SignalR 用戶端中的，您要提供 SignalR 用戶端程式碼與它需要才能建立實體連接到伺服器的所有資訊。 SignalR 用戶端程式碼會使用此資訊來提出 HTTP 要求，並且建立使用四個傳輸方法的其中一個的實體連接。 如果傳輸連線失敗，或伺服器失敗，SignalR 連線不會消失立即因為用戶端仍然需要自動重新建立相同的 SignalR URL 的新傳輸連線的資訊。 在此案例中，不涉及任何介入的使用者應用程式時，與 SignalR 用戶端程式碼會建立新的傳輸連接時，才會開始新的 SignalR 連線。 SignalR 連線的持續性事實中反映的連線識別碼，這是當您呼叫`Start`方法，不會變更。

`OnReconnected`集線器上的事件處理常式都會執行傳輸連線時自動重新建立之後遺失。 `OnDisconnected`事件處理常式都會執行 SignalR 連線的結尾。 SignalR 連線可以下列方式結束：

- 如果用戶端呼叫`Stop`方法，則停止訊息傳送至伺服器，和用戶端和伺服器 SignalR 連線立即結束。
- 用戶端與伺服器之間的連線中斷後，用戶端會嘗試重新連線，伺服器會等候重新連接的用戶端。 如果嘗試重新連線失敗，而且在中斷連線逾時期間結束，用戶端和伺服器便會結束 SignalR 連線。 用戶端停止嘗試重新連線，並處置其表示 SignalR 連線的伺服器。
- 如果用戶端停止執行，而不需要呼叫有機會`Stop`方法，伺服器會等候用戶端重新連線，然後結束之後中斷連線逾時期限的 SignalR 連線。
- 如果伺服器停止執行，用戶端嘗試重新連線 （重新建立傳輸連接），然後結束之後中斷連線逾時期限的 SignalR 連線。

沒有連線問題，與使用者應用程式藉由呼叫結束 SignalR 連線時`Stop`方法、 SignalR 連線和傳輸連線開頭，而且在同一時間結束。 下列章節詳細說明其他案例。

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>傳輸中斷連線的案例

實體的連線可能會變慢，或可能會有中斷連線。 例如的中斷時間長度的因素，根據傳輸連線可能會被捨棄。 SignalR 然後嘗試重新建立傳輸連接。 有時傳輸連線應用程式開發介面偵測到中斷傳輸連線，會卸除與 SignalR 會找出立即的連線已中斷。 在其他情況下，都不傳輸連線應用程式開發介面或 SignalR 就得知立即連線已中斷。 除了長輪詢傳輸所有 SignalR 用戶端會都使用函式，呼叫*keepalive*檢查傳輸 API 是無法偵測到的連線中斷。 長輪詢連線的相關資訊，請參閱[逾時和 keepalive 設定](#timeoutkeepalive)本主題稍後。

非使用中連接時，會定期伺服器傳送 keepalive 封包到用戶端。 這篇文章正在寫入的日期，以預設頻率為每隔 10 秒。 透過接聽這些封包，用戶端可以知道是否有連線問題。 如果未在預期時，會收到 keepalive 封包，一小段時間後用戶端會假設沒有連線問題，例如緩慢或中斷。 如果 keepalive 仍未收到較長的時間之後，用戶端會假設已中斷連線，而且它一開始會嘗試重新連線。

下圖說明用戶端和伺服器事件不是立即辨識傳輸 API 的實體連接問題時，所引發的典型的案例。 圖表適用於下列情況：

- 傳輸方式是 WebSockets、 永久框架或伺服器傳送事件。
- 有不同的實體網路連線中斷期間。
- 傳輸應用程式開發介面不會留意中斷所影響，因此 SignalR 依賴 keepalive 功能加以偵測。

![傳輸的連線中斷](handling-connection-lifetime-events/_static/image2.png)

如果用戶端便會進入重新連線模式，但無法建立傳輸連線在中斷連線的逾時限制內，伺服器就會終止 SignalR 連線。 當發生這種情況時，伺服器會執行的中樞`OnDisconnected`方法，並將傳送至用戶端，萬一用戶端會管理連接稍後中斷連線訊息的佇列。 如果用戶端未重新連線，接收到中斷連線命令窗格和呼叫`Stop`方法。 在此案例中，`OnReconnected`當用戶端重新連接時，不會執行並`OnDisconnected`不會執行，當用戶端呼叫`Stop`。 下圖說明這種情況。

![傳輸中斷的伺服器逾時](handling-connection-lifetime-events/_static/image3.png)

可能會發生在用戶端的 SignalR 連線存留期事件如下所示：

- `ConnectionSlow`用戶端的事件。

    當收到 keepalive ping 或 keepalive 逾時期限的預設所佔百分比，已經過的最後一個訊息時引發。 預設 keepalive 逾時的警告期間是 2/3 的 keepalive 逾時。 Keepalive 逾時是 20 秒，因此在大約 13 秒就會發生警告。

    根據預設，伺服器會傳送 keepalive ping 每隔 10 秒鐘，並在用戶端檢查 keepalive ping 每 2 秒 （三分之一的 keepalive 逾時值和 keepalive 逾時的警告值之間的差異） 的相關。

    如果傳輸 API 變成留意中斷，SignalR 可能通知中斷連線之前的 keepalive 逾時的警告期間傳遞。 在此情況下，`ConnectionSlow`就不會引發事件，然後 SignalR 會直接前往`Reconnecting`事件。
- `Reconnecting`用戶端的事件。

    當應用程式開發介面 （a） 的傳輸偵測到的連線已中斷，或 （b） 的 keepalive 逾時期限，已經過的最後一個訊息或收到 keepalive ping 時，就會引發。 SignalR 用戶端程式碼開始嘗試重新連線。 如果您希望應用程式時要採取某些動作傳輸連線已中斷，您可以處理這個事件。 20 秒的目前預設 keepalive 逾時期限。

    如果用戶端程式碼嘗試呼叫中樞方法，在重新連線模式 SignalR 時，會嘗試 SignalR 傳送命令。 大部分的情況下，這類的嘗試都會失敗，但在某些情況下可能會成功。 伺服器傳送事件、 不限次數的框架，而且長輪詢傳輸，SignalR 使用兩個通訊通道，一個用戶端用於傳送訊息，另一個用來接收訊息。 用於接收通道是永遠開啟其中一個，而這是實體的連線中斷時，已關閉的一個。 用來傳送仍可讓實體的連線還原時，如果方法呼叫從用戶端到伺服器可能成功才能接收通道重新建立通道。 SignalR 重新開啟接收使用通道之前，會未收到傳回值。
- `Reconnected`用戶端的事件。

    會重新建立傳輸連接時引發。 `OnReconnected`中樞中的事件處理常式都會執行。
- `Closed`用戶端事件 (`disconnected`在 JavaScript 中的事件)。

    中斷連線逾時期限到期時 SignalR 用戶端程式碼會嘗試重新連線之後遺失傳輸連線時引發。 中斷連線的預設逾時為 30 秒。 (因為，連線結束時，也會引發這個事件`Stop`呼叫方法。)

傳輸應用程式開發介面不會偵測且沒有延遲的 keepalive ping 從 keepalive 逾時警告期限超過伺服器接收的傳輸連線中斷可能不會導致任何連接存留期間来引發的事件。

某些網路環境刻意關閉閒置的連接，並且 keepalive 封包的另一個函式來協助防止這讓這些網路可讓您知道 SignalR 連線正在使用中。 在極端情況下 keepalive ping 以預設頻率不可能不足以防止已關閉的連線。 在此情況下，您可以設定更常傳送 keepalive ping。 如需詳細資訊，請參閱[逾時和 keepalive 設定](#timeoutkeepalive)本主題稍後。

> [!NOTE] 
> 
> [!IMPORTANT]
> 此處所述的事件的順序並不保證。 SignalR 會每個嘗試連線的存留期事件引發根據這項機制，可預測的方式，但有許多變化網路事件和許多所在基礎通訊架構，例如傳輸應用程式開發介面會處理它們的方式。 例如，`Reconnected`可能不會引發事件，當用戶端重新連接時，或`OnConnected`連線嘗試不成功時，可能會執行伺服器上的處理常式。 本主題說明通常是由某些一般的情況下所產生的效果。


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>用戶端中斷連接案例

在瀏覽器用戶端會維護 SignalR 連線的 SignalR 用戶端程式碼的內容中執行 JavaScript 的網頁。 具有為什麼 SignalR 連線已結束，當您從一個瀏覽頁面上與另一個，且的為什麼您有多個連線多個連線識別碼與您從多個瀏覽器視窗或索引標籤連線時。 SignalR 用戶端程式碼會為您和呼叫處理該瀏覽器事件，因為當使用者關閉瀏覽器視窗或索引標籤上，或瀏覽至新的頁面或重新整理頁面時，SignalR 連線立即結束`Stop`方法。 在這些案例中，或當您的應用程式呼叫的任何用戶端平台`Stop`方法，`OnDisconnected`事件處理常式會在伺服器上立即執行，並在用戶端引發`Closed`事件 (事件命名為`disconnected`中JavaScript)。

如果用戶端應用程式或電腦上執行的損毀或進入睡眠 （例如，當使用者關閉的膝上型電腦），伺服器不會通知發生了什麼事。 就伺服器知道，遺失的用戶端可能是因為連線中斷，用戶端可能會嘗試重新連線。 因此，在伺服器等候，讓用戶端重新連接，這些案例和`OnDisconnected`不會執行到中斷連線逾時期限到期為止 （大約 30 秒預設）。 下圖說明這種情況。

![用戶端電腦失敗](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>伺服器正在中斷連接案例

當伺服器離線時-會重新開機次數、 失敗，應用程式定義域回收，等-結果可能會類似於失去連線，或傳輸 API，並使用 SignalR 可能會立即得知伺服器不見，而 SignalR 可能會嘗試重新連線，但不開始引發`ConnectionSlow`事件。 如果用戶端便會進入重新連接模式中，而且伺服器復原或重新啟動或新的伺服器已上線的中斷連線逾時期限到期之前，用戶端會重新連線到已還原或新的伺服器。 在此情況下的 SignalR 連線仍持續在用戶端和`Reconnected`就會引發事件。 第一部伺服器，`OnDisconnected`從未執行，並在新伺服器上，`OnReconnected`雖然執行`OnConnected`從未執行過該用戶端之前，該伺服器上。 （效果是相同的用戶端之後重新連線到相同的伺服器重新開機或應用程式網域回收，因為當伺服器重新啟動它，如果有先前的連接活動的任何記憶體）。下圖假設的傳輸應用程式開發介面成為留意失去連線，所以`ConnectionSlow`不會引發事件。

![伺服器失敗和重新連線](handling-connection-lifetime-events/_static/image5.png)

如果伺服器中斷連線逾時期限內中未提供，則不會結束 SignalR 連線。 在此案例中，`Closed`事件 (`disconnected` JavaScript 用戶端中) 會在用戶端上引發但`OnDisconnected`絕不會呼叫在伺服器上。 下圖假設，傳輸應用程式開發介面不會留意失去連線，讓它偵測到的 SignalR keepalive 功能和`ConnectionSlow`就會引發事件。

![伺服器發生錯誤及逾時](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>逾時和 keepalive 設定

預設值`ConnectionTimeout`， `DisconnectTimeout`，和`KeepAlive`值適合大部分情況下，但可以變更您的環境具有特殊需求。 例如，如果您的網路環境關閉 5 秒鐘會處於閒置狀態的連接，您可能降低 keepalive 值。

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

這項設定代表的時間將傳輸連線保持開啟且正在等候回應，才會關閉它，並開啟新的連接。 預設值是 110 的秒數。

此設定適用於僅當 keepalive 功能已停用，這通常僅適用於長輪詢傳輸。 下圖說明這項設定在長時間的影響輪詢傳輸連線。

![長輪詢傳輸的連線](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

這項設定代表之後的傳輸連線將會遺失引發之前等待的時間量`Disconnected`事件。 預設值為 30 秒。 當您將`DisconnectTimeout`，`KeepAlive`會自動設為 1/3 的`DisconnectTimeout`值。

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

這項設定代表透過閒置連線傳送 keepalive 封包之前等待時間的量。 預設值為 10 秒。 此值不能超過 1/3 的`DisconnectTimeout`值。

如果您想要同時設定`DisconnectTimeout`和`KeepAlive`，將`KeepAlive`之後`DisconnectTimeout`。 否則您`KeepAlive`設定將會覆寫時`DisconnectTimeout`會自動設定`KeepAlive`的逾時值的 1/3。

如果您想要停用 keepalive 功能，設定`KeepAlive`為 null。 Keepalive 功能會自動停用的長輪詢傳輸。

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>如何變更逾時和 keepalive 設定

若要變更這些設定的預設值，請設定這些`Application_Start`中您*Global.asax*檔案，如下列範例所示。 範例程式碼所示的值是相同的預設值。

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>如何通知使用者有關的連線中斷

在某些應用程式，您可以有連線問題時，顯示訊息給使用者。 您有數個選項可以如何和何時執行這項操作。 下列程式碼範例會使用產生的 proxy JavaScript 用戶端。

- 處理`connectionSlow`SignalR 是知道連線問題之前它會進入重新連線模式, 時顯示訊息的事件。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- 處理`reconnecting`SignalR 感知中斷並進入重新連線模式時顯示訊息的事件。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- 處理`disconnected`事件，以顯示一則訊息時嘗試重新連線已逾時。在此案例中，以重新建立與伺服器的連線一次的唯一方法是重新啟動 SignalR 連線，藉由呼叫`Start`方法，將會建立新的連接識別碼。 下列程式碼範例可用於旗標，請確定您發出的通知後重新連線的逾時，才呼叫所造成的 SignalR 連線的正常結束後`Stop`方法。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>如何重新持續連線

在某些應用程式，您可能要已遺失，並嘗試重新連線已逾時之後自動重新建立連線。若要這樣做，您可以呼叫`Start`方法，從您`Closed`事件處理常式 (`disconnected` JavaScript 用戶端上的事件處理常式)。 您可能想要等候一段時間，然後再呼叫`Start`為了避免這樣太經常當伺服器或實體的連接都無法使用。 下列程式碼範例是使用產生的 proxy JavaScript 用戶端。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

要注意的行動用戶端有潛在的問題是連續可以嘗試重新連線的伺服器或實體的連線無法使用時，可能會導致不必要的電池清空。

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>如何在伺服器程式碼中的用戶端中斷連接

SignalR 版本 1.1.1 並沒有內建的伺服器 API 的用戶端連接中斷。 有[加入這項功能在未來的計劃](https://github.com/SignalR/SignalR/issues/2101)。 在目前的 SignalR 版本中，從伺服器中斷連線的用戶端最簡單的方式是實作在用戶端中斷連線方法，並從伺服器呼叫該方法。 下列程式碼範例顯示使用產生的 proxy 的 JavaScript 用戶端中斷連線方法。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> 安全性-既不中斷用戶端連接此方法，也不建議的內建 API 會解決的案例是遭駭客入侵執行惡意程式碼，因為用戶端無法重新連接或遭駭客入侵的程式碼可能會移除的用戶端`stopClient`方法或變更其用途。 若要實作可設定狀態的阻絕服務 (DOS) 保護的適當位置不是在架構或伺服器層，而前端的基礎結構中，而不是。
