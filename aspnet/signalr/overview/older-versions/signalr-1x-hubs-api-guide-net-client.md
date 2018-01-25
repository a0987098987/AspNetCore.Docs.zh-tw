---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: "ASP.NET SignalR 中樞 API 指南-.NET 用戶端 (SignalR 1.x) |Microsoft 文件"
author: pfletcher
description: "本文件提供適用於 SignalR.NET 用戶端，例如 Windows 市集 (WinRT)、 WPF、 Silverlight 和優缺點比較的第 2 版使用集線器 API 的簡介..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9a61bd255a217876aa2fdbeb6389539483b9f013
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>ASP.NET SignalR 中樞 API 指南-.NET 用戶端 (SignalR 1.x)
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文件提供適用於 SignalR.NET 用戶端，例如 Windows 市集 (WinRT)、 WPF、 Silverlight 和主控台應用程式中的第 2 版使用集線器 API 的簡介。
> 
> SignalR 中樞應用程式開發介面可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。 伺服端程式碼，並定義用戶端，可以呼叫的方法，呼叫用戶端執行的方法。 用戶端程式碼，並定義可在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。 SignalR 會負責為您的用戶端-伺服器一切細節。
> 
> SignalR 也能提供較低層級應用程式開發介面呼叫持續連線。 如需簡介 SignalR、 集線器及持續連線，或示範如何建置完整的 SignalR 應用程式的教學課程，請參閱[SignalR-快速入門](../getting-started/index.md)。


## <a name="overview"></a>總覽

本文件包含下列章節：

- [用戶端安裝](#clientsetup)
- [如何建立連接](#establishconnection)

    - [Silverlight 用戶端進行跨網域連線](#slcrossdomain)
- [如何設定連接](#configureconnection)

    - [如何在 WPF 用戶端中設定的同時連線數目上限](#maxconnections)
    - [如何指定查詢字串參數](#querystring)
    - [如何指定傳輸方法](#transport)
    - [如何指定 HTTP 標頭](#httpheaders)
    - [如何指定用戶端憑證](#clientcertificate)
- [如何建立中樞 proxy](#proxy)
- [如何定義伺服器可以呼叫的用戶端上的方法](#callclient)

    - [不含參數的方法](#clientmethodswithoutparms)
    - [具有指定參數型別參數的方法](#clientmethodswithparmtypes)
    - [具有指定之參數的動態物件參數的方法](#clientmethodswithdynamparms)
    - [如何移除處理常式](#removehandler)
- [如何從用戶端呼叫伺服器方法](#callserver)
- [如何處理連接的存留期事件](#connectionlifetime)
- [如何處理錯誤](#handleerrors)
- [如何啟用用戶端記錄](#logging)
- [WPF、 Silverlight 和主控台應用程式程式碼可以呼叫伺服器的用戶端方法的範例](#wpfsl)

如範例.NET 用戶端專案中，請參閱下列資源：

- [羅 armenta / SignalR 範例](https://github.com/gustavo-armenta/SignalR-Samples)上 GitHub.com （WinRT，Silverlight，主控台應用程式範例）。
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) GitHub.com （例如，WPF） 上。
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples)上 GitHub.com （主控台應用程式範例）。

如需如何將伺服器或 JavaScript 用戶端進行程式設計的文件，請參閱下列資源：

- [SignalR 中樞 API 指南-伺服器](../guide-to-the-api/hubs-api-guide-server.md)
- [SignalR 中樞 API 指南 JavaScript 用戶端](../guide-to-the-api/hubs-api-guide-javascript-client.md)

應用程式開發介面參考主題的連結是.NET 4.5 版的 API。 如果您使用.NET 4，請參閱[API 主題.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。

<a id="clientsetup"></a>

## <a name="client-setup"></a>用戶端安裝

安裝[Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet 封裝 (不[Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)封裝)。 此套件支援 WinRT、 Silverlight、 WPF、 主控台應用程式和 Windows Phone 用戶端，.NET 4 和.NET 4.5。

如果您有在用戶端的 SignalR 的版本是與您在伺服器擁有的版本不同，SignalR 是通常可以調整的差異。 比方說，釋放 SignalR 2.0 版時，在伺服器上安裝，伺服器將會支援具有 1.1.x 以及具有 2.0 安裝的用戶端安裝的用戶端。 如果伺服器上的版本與用戶端上的版本之間的差異太大，會擲回 SignalR`InvalidOperationException`用戶端會嘗試建立連接時的例外狀況。 錯誤訊息是 「`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>如何建立連接

您可以建立連線之前，您必須建立`HubConnection`物件，並建立 proxy。 若要建立連接，呼叫`Start`方法`HubConnection`物件。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> 您必須註冊至少一個事件處理常式，然後再呼叫 JavaScript 用戶端`Start`方法，以建立連接。 這是不必要的.NET 用戶端。 JavaScript 用戶端，產生的 proxy 程式碼會自動建立存在的所有主機的 proxy 的伺服器上，以及註冊處理常式是哪一個中心指示您的用戶端所要使用。 但.NET 用戶端您中樞 proxy 手動建立，因此 SignalR 假設您將會使用任何您建立的 proxy 的中樞。


範例程式碼會使用預設 「 / signalr 」 來連線到 SignalR 服務的 URL。 如需如何指定不同的基底 URL，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。

`Start`方法以非同步方式執行。 若要確定連線建立之後，後續幾行程式碼不執行之前，請使用`await`ASP.NET 4.5 非同步方法中或`.Wait()`同步方法中。 請勿使用`.Wait()`WinRT 用戶端中。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

`HubConnection` 類別是安全執行緒。

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Silverlight 用戶端進行跨網域連線

如需如何啟用跨網域 Silverlight 用戶端的連接資訊，請參閱[讓服務提供跨網域界限](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)。

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>如何設定連接

建立連線之前，您可以指定下列選項：

- 並行連線限制。
- 查詢字串參數。
- 傳輸方法。
- HTTP 標頭。
- 用戶端憑證。

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>如何在 WPF 用戶端中設定的同時連線數目上限

在 WPF 用戶端，您可能必須增加的非預設值為 2 的並行連線數目上限。 建議的值為 10。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

如需詳細資訊，請參閱[ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>如何指定查詢字串參數

如果您想要在用戶端連線時，將資料傳送到伺服器，您可以查詢字串參數加入連接物件。 下列範例會示範如何在用戶端程式碼中設定的查詢字串參數。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

下列範例會示範如何讀取伺服器程式碼中的查詢字串參數。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>如何指定傳輸方法

連接的程序的一部分，SignalR 用戶端通常會與交涉的伺服器和用戶端到伺服器來判斷最佳支援的傳輸。 如果您已經知道您想要使用的傳輸，您可以略過此交涉程序。 若要指定傳輸方法，傳入傳輸物件的 Start 方法。 下列範例顯示如何在用戶端程式碼中指定的傳輸方法。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)命名空間包含下列類別可讓您指定的傳輸。

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) （使用伺服器和用戶端使用.NET 4.5 時，才）。
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) （會自動選擇最佳傳輸支援的用戶端和伺服器。 這是預設的傳輸。 傳遞此中`Start`方法具有相同的效果不會傳遞任何項目。)

ForeverFrame 傳輸不會包含這份清單中，因為它僅由瀏覽器。

如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-如何取得用戶端的相關資訊的內容屬性從](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)。 如需傳輸與後援的詳細資訊，請參閱[簡介 SignalR 的傳輸與後援](../getting-started/introduction-to-signalr.md#transports)。

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>如何指定 HTTP 標頭

若要設定 HTTP 標頭，使用`Headers`連線物件上的屬性。 下列範例會示範如何新增 HTTP 標頭。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>如何指定用戶端憑證

若要新增用戶端憑證，請使用`AddClientCertificate`連線物件上的方法。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>如何建立中樞 proxy

為了在集線器可以呼叫在伺服器上，從用戶端上定義方法，並叫用中樞，以在伺服器上的方法，來建立中樞的 proxy 呼叫`CreateHubProxy`連線物件上。 字串您傳遞給`CreateHubProxy`是您的中樞類別名稱或所指定的名稱`HubName`屬性如果在伺服器上已使用的其中一種。 名稱比對不區分大小寫。

**在伺服器上的中樞類別**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**建立 Hub 類別的用戶端的 proxy**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

如果裝飾中樞類別`HubName`屬性，請使用該名稱。

**在伺服器上的中樞類別**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**建立 Hub 類別的用戶端的 proxy**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Proxy 物件是安全執行緒。 事實上，如果您呼叫`HubConnection.CreateHubProxy`多次具有相同`hubName`，您可以取得相同的快取`IHubProxy`物件。

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>如何定義伺服器可以呼叫的用戶端上的方法

若要定義伺服器可以呼叫的方法，使用 proxy 的`On`方法，以註冊事件處理常式。

方法名稱比對不區分大小寫。 例如，`Clients.All.UpdateStockPrice`在伺服器上將會執行`updateStockPrice`， `updatestockprice`，或`UpdateStockPrice`用戶端上。

不同的用戶端的平台有不同需求，撰寫方法的程式碼的方式更新 UI。 所顯示的範例是針對 WinRT (Windows 市集.NET) 用戶端。 WPF、 Silverlight 和主控台應用程式範例中提供[稍後在本主題中的個別區段](#wpfsl)。

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>不含參數的方法

如果您正在處理的方法沒有參數，使用的非泛型多載`On`方法：

**伺服端程式碼呼叫不含參數的用戶端方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**從伺服器不使用參數呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>具有指定的參數型別參數的方法

如果您正在處理的方法有參數，指定的參數類型的泛型型別之`On`方法。 有泛型多載的`On`方法可讓您指定最多 8 個參數 (在 Windows Phone 7 上為 4)。 在下列範例中，一個參數會傳送至`UpdateStockPrice`方法。

**伺服端程式碼呼叫具有參數的用戶端方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**用於參數的庫存類別**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**從伺服器以參數呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>具有指定之參數的動態物件參數的方法

做為參數指定做為泛型型別替代`On`方法，您可以指定參數為動態物件：

**伺服端程式碼呼叫具有參數的用戶端方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**用於參數的庫存類別**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**從伺服器具有參數，使用參數的動態物件呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>如何移除處理常式

若要移除處理常式，呼叫其`Dispose`方法。

**從伺服器呼叫的方法的用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**用戶端程式碼中移除處理常式**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>如何從用戶端呼叫伺服器方法

若要在伺服器上呼叫方法，使用`Invoke`中樞 proxy 上的方法。

如果伺服器方法沒有傳回值，請使用非泛型多載`Invoke`方法。

**伺服端程式碼沒有傳回值的方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**用戶端程式碼呼叫的方法沒有傳回值，**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

如果伺服器方法具有傳回值，指定的傳回型別為泛型型別`Invoke`方法。

**伺服端程式碼的方法具有傳回值，使用複雜型別參數**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**用於參數和傳回值的庫存類別**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**用戶端程式碼呼叫的方法具有傳回值，並使用 ASP.NET 4.5 的非同步方法中的複雜型別參數**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**用戶端程式碼呼叫的方法具有傳回值，同步方法中使用複雜型別參數**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke`方法以非同步方式執行，並傳回`Task`物件。 如果您未指定`await`或`.Wait()`之前您叫用的方法已完成執行時,，會執行下的一行程式碼。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>如何處理連接的存留期事件

SignalR 提供下列的連線，您可以控制它們的存留期事件：

- `Received`： 在此連接上收到任何資料時引發。 提供已接收的資料。
- `ConnectionSlow`： 用戶端偵測到低速或經常卸除連接時引發。
- `Reconnecting`： 基礎傳輸可讓您開始重新連線時引發。
- `Reconnected`： 基礎傳輸已重新連接時引發。
- `StateChanged`： 連線狀態變更時引發。 提供舊的狀態和新的狀態。 如需連接狀態值請參閱[ConnectionState 列舉，](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)。
- `Closed`： 當連接已中斷連接時引發。

例如，如果您想要顯示的不嚴重，但間歇性連線問題會造成錯誤的警告訊息，例如為緩慢或太頻繁地卸除的連接，處理`ConnectionSlow`事件。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

如需詳細資訊，請參閱[了解與 SignalR 中處理連接存留期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>如何處理錯誤

如果您未明確啟用在伺服器上的詳細的錯誤訊息，在發生錯誤之後，傳回 SignalR 的例外狀況物件包含錯誤的相關基本資訊。 例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解用途，在伺服器上使用下列程式碼。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

若要處理 SignalR 引發的錯誤，您可以加入的處理常式`Error`連線物件上的事件。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

若要處理來自方法引動過程的錯誤，程式碼包裝在 try catch 區塊中。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>如何啟用用戶端記錄

若要啟用用戶端記錄，`TraceLevel`和`TraceWriter`連線物件上的屬性。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF、 Silverlight 和主控台應用程式程式碼可以呼叫伺服器的用戶端方法的範例

稍早所示，定義伺服器可以呼叫的用戶端方法的程式碼範例會套用至 WinRT 用戶端。 下列範例顯示 WPF、 Silverlight 和主控台應用程式用戶端的對等的程式碼。

### <a name="methods-without-parameters"></a>不含參數的方法

**從伺服器不使用參數呼叫方法的 WPF 用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**從伺服器不使用參數呼叫方法的 Silverlight 用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**從伺服器不使用參數呼叫方法的主控台應用程式用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>具有指定的參數型別參數的方法

**從伺服器以參數呼叫方法的 WPF 用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Silverlight 用戶端程式碼從具有參數的伺服器呼叫的方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**從伺服器以參數呼叫方法的主控台應用程式用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>具有指定之參數的動態物件參數的方法

**從伺服器具有參數，使用參數的動態物件呼叫方法的 WPF 用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**從伺服器具有參數，使用參數的動態物件呼叫方法的 Silverlight 用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**從伺服器具有參數，使用參數的動態物件呼叫方法的主控台應用程式用戶端程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
