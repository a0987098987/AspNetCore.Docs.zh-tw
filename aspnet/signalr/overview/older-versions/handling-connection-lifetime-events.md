---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: 了解和處理 signalr 的連線存留期事件 1.x |Microsoft Docs
author: bradygaster
description: 本文說明如何使用事件中樞 API 所公開。
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: bf10cf3e3e1881a976e8a123b48007f7bd8821f7
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837659"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>了解和處理連線存留期事件 SignalR 1.x
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 這篇文章提供 SignalR 連線、 重新連線，以及中斷連線事件，您可以控制，以及您可以設定的逾時和 keepalive 設定的總覽。
> 
> 本文假設您已經具備 SignalR 和連線的存留期事件的一些知識。 Signalr 簡介，請參閱[開始使用 SignalR-概觀-](index.md)。 如需連線存留期事件清單，請參閱下列資源：
> 
> - [如何處理中樞類別中的連線存留期事件](index.md)
> - [如何處理 JavaScript 用戶端連線存留期事件](index.md)
> - [如何處理.NET 用戶端連線存留期事件](index.md)


## <a name="overview"></a>總覽

本文包含下列章節：

- [連接的存留期術語和案例](#terminology)

    - [SignalR 連線、 傳輸連線和實體的連線](#signalrvstransport)
    - [傳輸中斷連線的案例](#transportdisconnect)
    - [用戶端中斷連線的案例](#clientdisconnect)
    - [伺服器中斷連線的案例](#serverdisconnect)
- [逾時和 keepalive 設定](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [如何變更逾時和 keepalive 設定](#changetimeout)
- [如何通知使用者有關的連線中斷](#notifydisconnect)
- [如何持續重新連接](#continuousreconnect)
- [如何中斷連接的伺服器程式碼中的用戶端](#disconnectclientfromserver)

API 參考主題的連結是 API 的.NET 4.5 版本。 如果您使用.NET 4，請參閱[API 主題的.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>連接的存留期術語和案例

`OnReconnected` SignalR 中樞的事件處理常式可以直接在之後執行`OnConnected`但不是晚`OnDisconnected`指定用戶端。 您可以重新連線，而不中斷的原因是，有數個 「 連線 」 這個字用於 SignalR 的方式。

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR 連線、 傳輸連線和實體的連線

這篇文章會區別*SignalR 連線*，*傳輸連線*，並*實體連線*:

- **SignalR 連線**指的用戶端和伺服器 URL、 SignalR API 所維護，而且可以連接 ID 唯一識別之間的邏輯關聯性 此關聯性的相關資料由 SignalR 維護，並用來建立傳輸連接。 SignalR 和關聯性結尾時，處置資料的用戶端呼叫`Stop`SignalR 嘗試重新建立遺失的傳輸連線期間達到方法或逾時限制。
- **傳輸連線**指的用戶端和伺服器，維護的其中一個四種傳輸 Api 之間的邏輯關聯性：Websocket 伺服器傳送事件、 不限次數的框架或長輪詢。 SignalR 使用傳輸 API 以建立傳輸連接，並傳輸 API 取決於建立傳輸連線的實體網路連線存在。 SignalR 終止它時，或當傳輸 API 偵測到的實體連接已中斷，就會結束傳輸連線。
- **實體連接**參考的實體網路的連結-線、 無線訊號，路由器等，可簡化用戶端電腦和伺服器電腦之間的通訊。 實體連線必須存在於若要建立傳輸連線，而且必須以建立 SignalR 連線建立傳輸連接。 不過，重大的實體連接不一定會立即結束的傳輸連線或 SignalR 連線，如稍後本主題將說明。

在下列圖表中，由 SignalR 連線的中樞 API 和 PersistentConnection API SignalR 層級、 傳輸連線由傳輸層和實體連接由伺服器之間的線條和用戶端。

![SignalR 架構圖](handling-connection-lifetime-events/_static/image1.png)

當您呼叫`Start`SignalR 用戶端中的方法，您會提供 SignalR 用戶端程式碼需才能建立實體連接到伺服器的所有資訊。 SignalR 用戶端程式碼會使用這項資訊來發出 HTTP 要求，並建立使用其中一種四種傳輸方法的實體連接。 如果傳輸連線失敗，或伺服器失敗，SignalR 連線不會消失立即因為用戶端仍然需要自動重新建立新的傳輸連線至相同的 SignalR URL 的資訊。 在此案例中，從使用者的應用程式不需要介入是，而且當 SignalR 用戶端程式碼會建立新的傳輸連線，它不會啟動新的 SignalR 連線。 SignalR 連線的持續性會反映在事實的連線識別碼，這當您呼叫建立`Start`方法，不會變更。

`OnReconnected`集線器上的事件處理常式執行時的傳輸連線之後自動重新建立已遺失。 `OnDisconnected`事件處理常式執行 SignalR 連線的結尾。 SignalR 連線可以在下列任一方式結束：

- 如果用戶端呼叫`Stop`方法中，「 停止 」 訊息傳送至伺服器，以及用戶端和伺服器 SignalR 連線立即結束。
- 用戶端與伺服器之間的連線中斷後，用戶端會嘗試重新連線，以及伺服器等候用戶端重新連線。 如果要重新連線嘗試未成功，而且在中斷連線逾時期間結束，用戶端和伺服器端 SignalR 連線。 用戶端會停止嘗試重新連線，並處置它表示 SignalR 連線的伺服器。
- 如果用戶端會停止執行，而不需要呼叫有機會`Stop`方法中，伺服器等候用戶端重新連線，然後結束 中斷連線逾時期限之後 SignalR 連線。
- 如果伺服器停止執行，用戶端會嘗試重新連線 （重新建立傳輸連線），然後結束 中斷連線逾時期限之後 SignalR 連線。

沒有連線問題，與使用者應用程式藉由呼叫結束 SignalR 連線時`Stop`方法、 SignalR 連線和傳輸連線在開始與結束大約相同的時間。 下列各節更詳細說明其他案例。

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>傳輸中斷連線的案例

實體連線可能會變慢，或可能會有中斷連線。 中斷時間的長度等因素，傳輸連線可能會被捨棄。 SignalR 接著會嘗試重新建立傳輸連線。 有時候傳輸連線 API 偵測到中斷傳輸連線，會卸除和 SignalR 發現立即連線已中斷。 在其他情況下，既不傳輸連線的 API 或 SignalR 察覺立即連線已遺失。 除了長輪詢傳輸所有 SignalR 用戶端會都使用函式，呼叫*keepalive*檢查的傳輸 API 是無法偵測到的連線中斷。 如需長時間輪詢連線資訊，請參閱 <<c0> [ 逾時和 keepalive 設定](#timeoutkeepalive)本主題稍後的。

非使用中連接時，會定期伺服器傳送 keepalive 封包到用戶端。 正在寫入這篇文章的日期，從預設的頻率是每隔 10 秒。 藉由接聽這些封包，用戶端就可以知道是否有連線問題。 如果未在預期時，會收到 keepalive 封包，在短時間後用戶端會假設有連線問題，例如速度很慢或中斷。 如果 keepalive 仍未收到較長的時間之後，用戶端會假設此連接已經卸除，，然後開始嘗試重新連線。

下圖說明用戶端和伺服器事件不是立即辨識傳輸 API 的實體連接問題時，所引發的一般案例。 圖適用於下列情況：

- 傳輸方式是 WebSockets、 永久框架或伺服器傳送事件。
- 有不同的期間，在實體網路連線中斷的位置。
- 傳輸 API 不會知道中斷，因此 SignalR 依賴 keepalive 功能，以偵測到這些控制項。

![傳輸中斷連線](handling-connection-lifetime-events/_static/image2.png)

如果用戶端便會進入重新連接模式，但無法建立傳輸連線在中斷連線的逾時限制內，伺服器會終止 SignalR 連線。 當發生這種情況時，伺服器會執行的中樞`OnDisconnected`方法，並將傳送至用戶端，如果用戶端會管理連接稍後中斷連線訊息的佇列。 如果用戶端接著會重新連線，它會收到中斷連接命令和呼叫`Stop`方法。 在此案例中，`OnReconnected`不會執行，當用戶端重新連線，並`OnDisconnected`不會執行，當用戶端呼叫`Stop`。 下圖說明此案例。

![傳輸中斷-伺服器逾時](handling-connection-lifetime-events/_static/image3.png)

在用戶端，可能會引發 SignalR 連線存留期事件，如下所示：

- `ConnectionSlow` 用戶端的事件。

    Keepalive 逾時期限的預設的比例過的最後一個訊息，或收到 keepalive ping 時，就會引發。 預設 keepalive 逾時警告期間是 2/3 keepalive 逾時。 Keepalive 逾時為 20 秒，因此在大約 13 秒就會發生警告。

    根據預設，伺服器會傳送 keepalive ping 每隔 10 秒，並在用戶端檢查 keepalive ping 每 2 秒 （三分之一 keepalive 逾時值和 keepalive 逾時的警告值之間的差異） 的相關。

    如果傳輸 API 察覺的中斷，SignalR 可能通知中斷連接之前 keepalive 逾時警告期間傳遞。 在此情況下，`ConnectionSlow`不會引發事件，以及 SignalR 會直接移至`Reconnecting`事件。
- `Reconnecting` 用戶端的事件。

    當 API （a） 傳輸偵測到的連線已中斷，或 （b） 的 keepalive 逾時期限後的最後一個訊息，已經過或 keepalive ping 接收時，就會引發。 SignalR 用戶端程式碼開始嘗試重新連線。 如果您想要您的應用程式的傳輸連線已遺失時採取某個動作，您可以處理這個事件。 20 秒的目前預設 keepalive 逾時期限。

    如果您的用戶端程式碼嘗試呼叫中樞方法，而 SignalR 是在重新連線模式，SignalR 會嘗試傳送命令。 大部分的情況下，這類的嘗試都會失敗，但在某些情況下可能會成功。 伺服器傳送事件、 永久框架和長輪詢傳輸，SignalR 使用兩個通訊通道，其中用戶端用來傳送訊息，它會使用來接收訊息的另一個。 用來接收的通道是永遠開啟，而這是實體的連線中斷時，會關閉的一個。 通道用於傳送仍可供使用，因此，接收通道後重新建立實體的連線還原時，如果伺服器的方法呼叫從用戶端可能會成功。 直到 SignalR 重新開啟用來接收的通道，就不會收到傳回的值。
- `Reconnected` 用戶端的事件。

    當傳輸重新建立連線時，就會引發。 `OnReconnected`在中樞的事件處理常式執行。
- `Closed` 用戶端事件 (`disconnected`在 JavaScript 中的事件)。

    在中斷連線逾時期限到期時 SignalR 用戶端程式碼嘗試傳輸連線中斷之後重新連線時引發。 預設的中斷連線逾時為 30 秒。 (因為，連線結束時，也會引發這個事件`Stop`呼叫方法。)

未偵測到傳輸 API，且沒有延遲的 keepalive ping 從超過 keepalive 逾時警告期間伺服器接收的傳輸連線中斷可能會不會造成任何連線存留期間来引發的事件。

有些網路環境刻意關閉閒置的連接，並 keepalive 封包的另一個函式是為了避免這個問題讓這些網路可讓您知道 SignalR 連線正在使用中。 在極端情況下 keepalive ping 預設頻率不可能不足以防止已關閉的連線。 在此情況下，您可以設定更常傳送 keepalive ping。 如需詳細資訊，請參閱 <<c0> [ 逾時和 keepalive 設定](#timeoutkeepalive)本主題稍後的。

> [!NOTE] 
> 
> [!IMPORTANT]
> 此處所述的事件的順序並不保證。 SignalR 會不斷嘗試以便引發連線存留期事件，此配置中，根據可預測的方式，但有許多變化的網路事件和多種資訊，請在其中基礎的通訊架構，例如傳輸 Api 處理它們。 例如，`Reconnected`可能不會引發事件，當用戶端重新連接，或`OnConnected`建立的連線嘗試不成功時，可能會執行伺服器上的處理常式。 本主題說明某些常見的情況下將通常會產生的效果。


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>用戶端中斷連線的案例

在瀏覽器用戶端會維護 SignalR 連線的 SignalR 用戶端程式碼的內容中執行 JavaScript 的網頁。 具有為什麼 SignalR 連線已結束，當您從一個頁面上與另一個，且的為什麼您有多個連線使用多個連線識別碼如果您從多個瀏覽器視窗或索引標籤連線。 當使用者關閉瀏覽器視窗或索引標籤上，或瀏覽至新的頁面或重新整理頁面時，SignalR 連線會立即結束因為 SignalR 用戶端程式碼會處理該瀏覽器事件，寄件者和呼叫`Stop`方法。 在這些情況下，或當您的應用程式呼叫的任何用戶端平台`Stop`方法中，`OnDisconnected`事件處理常式會在伺服器上立即執行，並在用戶端引發`Closed`事件 (事件的名稱為`disconnected`中JavaScript)。

如果用戶端應用程式或電腦上執行的損毀或進入睡眠狀態 （例如，當使用者關閉的膝上型電腦），伺服器不會發生了什麼事收到通知。 只要伺服器知道，遺失的用戶端可能是因為連線中斷，用戶端可能會嘗試重新連線。 因此，在這些案例中，伺服器會等待讓用戶端重新連線，有機會和`OnDisconnected`不會執行到中斷連線逾時期限到期為止 （大約 30 秒的預設值）。 下圖說明此案例。

![用戶端電腦的失敗](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>伺服器中斷連線的案例

當伺服器離線時，它會重新啟動、 失敗，應用程式網域回收，等-結果可能會類似於遺失的連線，或傳輸 API，並使用 SignalR 可能立即得知伺服器不見了，而 SignalR 可能會開始嘗試重新連線，而不需要引發`ConnectionSlow`事件。 如果用戶端會進入重新連接 模式中，而且伺服器復原或重新啟動或新的伺服器處於線上時中斷連線逾時期間到期之前，用戶端會重新連接到已還原或新的伺服器。 在此情況下，SignalR 連線會繼續在用戶端和`Reconnected`就會引發事件。 第一部伺服器中，`OnDisconnected`永遠不會執行，並在新伺服器上，`OnReconnected`雖然執行`OnConnected`永遠不會執行該用戶端之前，該伺服器上。 （效果是相同的用戶端之後重新連線到相同的伺服器重新開機或應用程式網域回收，因為當伺服器重新啟動它，所以如果有先前的連接活動的任何記憶體）。下圖假設，傳輸 API 察覺到連線中斷立即執行，因此`ConnectionSlow`不會引發事件。

![伺服器失敗和重新連線](handling-connection-lifetime-events/_static/image5.png)

如果伺服器尚未變成可用的中斷連線逾時期限內，SignalR 連線就會結束。 在此案例中，`Closed`事件 (`disconnected` JavaScript 用戶端中) 會在用戶端上引發，但`OnDisconnected`絕不會呼叫伺服器上。 下圖假設，傳輸 API 不會留意失去連線，因此它會偵測到 SignalR keepalive 功能和`ConnectionSlow`就會引發事件。

![伺服器發生錯誤及逾時](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>逾時和 keepalive 設定

預設值`ConnectionTimeout`， `DisconnectTimeout`，和`KeepAlive`值適用於大部分的情況下，但如果您的環境具有特殊需求，可以變更。 比方說，如果您的網路環境關閉 5 秒會處於閒置狀態的連線，您可能會降低的 keepalive 值。

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

這項設定代表開啟並等待回應，以關閉它，並開啟新連接之前離開的傳輸連線的時間量。 預設值為 110 秒。

此設定適用於僅 keepalive 功能停用時，這通常僅適用於長輪詢傳輸。 下圖說明這項設定在長時間的影響輪詢傳輸連線。

![長輪詢傳輸的連線](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

這項設定代表之後的傳輸連線已遺失引發之前等待的時間量`Disconnected`事件。 預設值為 30 秒。 當您設定`DisconnectTimeout`，`KeepAlive`會自動設為 1/3`DisconnectTimeout`值。

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

這項設定代表透過閒置連線傳送 keepalive 封包之前等待時間的量。 預設值為 10 秒。 此值不能超過 1/3`DisconnectTimeout`值。

如果您想要同時設定`DisconnectTimeout`並`KeepAlive`，將`KeepAlive`之後`DisconnectTimeout`。 否則您`KeepAlive`設定將會覆寫時`DisconnectTimeout`會自動設定`KeepAlive`的逾時值的 1/3。

如果您想要停用 keepalive 功能，設定`KeepAlive`為 null。 Keepalive 功能會自動停用的長輪詢傳輸。

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>如何變更逾時和 keepalive 設定

若要變更這些設定的預設值，以設定它們`Application_Start`在您*Global.asax*檔案，如下列範例所示。 範例程式碼所示的值是相同的預設值。

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>如何通知使用者有關的連線中斷

在某些應用程式，您可能要向使用者顯示一則訊息時有連線問題。 您有數個選項，以及何時要執行這項操作。 下列程式碼範例專供 JavaScript 用戶端使用產生的 proxy。

- 處理`connectionSlow`事件，以顯示一則訊息，一旦 SignalR 的連線問題，了解之前，它會進入重新連接模式。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- 處理`reconnecting`SignalR 所知的中斷並進入重新連線模式時，顯示一則訊息的事件。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- 處理`disconnected`事件，以顯示一則訊息時嘗試重新連線已逾時。在此案例中，重新建立與伺服器連線一次的唯一方式是藉由呼叫重新 SignalR 連線`Start`方法，這將會建立新的連接識別碼。 下列程式碼範例使用旗標，藉此確定您發出的通知後重新連線的逾時，才不晚於藉由呼叫造成 SignalR 連線正常結束`Stop`方法。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>如何持續重新連接

在某些應用程式可能會想要已遺失，並嘗試重新連線已逾時之後自動重新建立連線。若要這樣做，您可以呼叫`Start`方法，從您`Closed`事件處理常式 (`disconnected` JavaScript 用戶端上的事件處理常式)。 您可能想要等候一段時間，然後再呼叫`Start`為了避免這樣太頻繁伺服器或實體的連線時無法使用。 下列程式碼範例是 JavaScript 用戶端使用產生的 proxy。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

要注意的行動用戶端中的潛在問題是連續的重新連線嘗試的伺服器或實體的連線無法使用時，可能會導致不必要的電池清空。

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>如何中斷連接的伺服器程式碼中的用戶端

SignalR 1.1.1 版並沒有內建的伺服器 API 的用戶端連接中斷。 有[計劃在未來新增這項功能](https://github.com/SignalR/SignalR/issues/2101)。 在目前的 SignalR 版本中，從伺服器中斷連線的用戶端的最簡單方式是實作在用戶端上的 中斷連線方法，並從伺服器呼叫該方法。 下列程式碼範例顯示使用產生的 proxy 的 JavaScript 用戶端中斷連線的方法。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> 安全性-既不中斷用戶端連接此方法也不建議的內建 API 會處理案例的遭駭客入侵的用戶端執行惡意程式碼，因為用戶端無法重新連線或遭駭客入侵的程式碼可能會移除`stopClient`方法或變更其用途。 實作可設定狀態的阻斷服務 (DOS) 保護的適當位置是不在 framework 或伺服器層中，但前端的基礎結構中，而不是。
