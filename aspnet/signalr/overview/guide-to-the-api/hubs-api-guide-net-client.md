---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR 中樞 API 指南-.NET 用戶端 (C#) |Microsoft Docs
author: pfletcher
description: 本文件提供使用 signalr.NET 用戶端，例如 Windows 市集 (WinRT)、 WPF、 Silverlight 和優缺點比較的第 2 版的中樞 API 的簡介...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 0a2b24039259ef90579a7f215bb9e35ebef7b9b9
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288036"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>ASP.NET SignalR 中樞 API 指南-.NET 用戶端 (C#)
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本文件提供使用 signalr 第 2 版，.NET 用戶端，例如 Windows 市集 (WinRT)、 WPF、 Silverlight 和主控台應用程式中的中樞 API 的簡介。
>
> SignalR 中樞 API 可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。 在伺服器程式碼中，您定義可由用戶端，呼叫的方法，呼叫用戶端執行的方法。 在用戶端程式碼中，您定義可以在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。 SignalR 會處理所有為您的用戶端-伺服器配管。
>
> SignalR 也提供一個名為持續連線的較低層級 API。 如需 SignalR、 中樞和持續連線，或該教學課程說明如何建置完整的 SignalR 應用程式，請參閱[SignalR-開始使用](../getting-started/index.md)。
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

## <a name="overview"></a>總覽

本文件包含下列章節：

- [用戶端安裝程式](#clientsetup)
- [如何建立連線](#establishconnection)

    - [從 Silverlight 用戶端的跨網域連接](#slcrossdomain)
- [如何設定連線](#configureconnection)

    - [如何在 WPF 用戶端設定的並行連線數目上限](#maxconnections)
    - [如何指定查詢字串參數](#querystring)
    - [如何指定傳輸方法](#transport)
    - [如何指定 HTTP 標頭](#httpheaders)
    - [如何指定用戶端憑證](#clientcertificate)
- [如何建立中樞 proxy](#proxy)
- [如何定義伺服器可以呼叫用戶端上的方法](#callclient)

    - [不含參數的方法](#clientmethodswithoutparms)
    - [使用指定的參數型別參數的方法](#clientmethodswithparmtypes)
    - [具有參數，指定參數的動態物件的方法](#clientmethodswithdynamparms)
    - [如何移除處理常式](#removehandler)
- [如何從用戶端呼叫伺服器方法](#callserver)
- [如何處理連接的存留期事件](#connectionlifetime)
- [如何處理錯誤](#handleerrors)
- [如何啟用用戶端記錄](#logging)
- [WPF、 Silverlight 和主控台應用程式的程式碼可以呼叫伺服器的用戶端方法的範例](#wpfsl)

範例.NET 用戶端專案，請參閱下列資源：

- [gustavo armenta / SignalR 範例](https://github.com/gustavo-armenta/SignalR-Samples)github.com （WinRT，Silverlight 中的主控台應用程式範例）。
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) github.com （WPF 範例）。
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) github.com （主控台應用程式範例）。

如需如何撰寫程式的伺服器或 JavaScript 用戶端的文件，請參閱下列資源：

- [SignalR 中樞 API 指南-伺服器](hubs-api-guide-server.md)
- [SignalR 中樞 API 指南-JavaScript 用戶端](hubs-api-guide-javascript-client.md)

API 參考主題的連結是 API 的.NET 4.5 版本。 如果您使用.NET 4，請參閱[API 主題的.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。

<a id="clientsetup"></a>

## <a name="client-setup"></a>用戶端安裝程式

安裝[Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet 套件 (不[Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)封裝)。 此套件支援 WinRT、 Silverlight、 WPF、 主控台應用程式和 Windows Phone 用戶端，.NET 4 和.NET 4.5。

如果您尚未在伺服器的版本不同 SignalR 用戶端上所擁有的版本，SignalR 是通常能夠適應差異。 例如，具有 1.1.x 安裝用戶端，以及具有版本 2 安裝的用戶端，將支援執行 SignalR 第 2 版的伺服器。 如果伺服器上的版本與用戶端上的版本之間的差異是太大，或如果用戶端比伺服器更新時，會擲回 SignalR`InvalidOperationException`用戶端會嘗試建立連接時的例外狀況。 錯誤訊息是 「`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>如何建立連線

您可以建立連線之前，您必須建立`HubConnection`物件，並建立 proxy。 若要建立連線，呼叫`Start`方法`HubConnection`物件。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> 您必須註冊至少一個事件處理常式，然後再呼叫 JavaScript 用戶端`Start`方法來建立連線。 這是不必要的.NET 用戶端。 JavaScript 的用戶端，產生的 proxy 程式碼會自動建立存在的所有主機的 proxy 的伺服器上，以及您該如何指出哪一個中樞註冊的處理常式是您的用戶端想要使用。 但.NET 用戶端您中樞 proxy 手動建立，因此 SignalR 假設您將會使用任何中樞所建立的 proxy。


範例程式碼會使用預設值"/ signalr 」 來連線到您的 SignalR 服務的 URL。 如需有關如何指定不同的基底 URL 的資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-/signalr URL](hubs-api-guide-server.md#signalrurl)。

`Start`方法以非同步方式執行。 若要確定接下來的幾行程式碼不執行直到連線建立之後，請使用`await`在 ASP.NET 4.5 的非同步方法或`.Wait()`同步方法中。 請勿使用`.Wait()`WinRT 用戶端中。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>從 Silverlight 用戶端的跨網域連接

如需如何啟用從 Silverlight 用戶端的跨網域連接資訊，請參閱[讓服務提供跨網域界限](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)。

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>如何設定連線

建立連線之前，您可以指定任何下列選項：

- 並行連線限制。
- 查詢字串參數。
- 傳輸方法。
- HTTP 標頭。
- 用戶端憑證。

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>如何在 WPF 用戶端設定的並行連線數目上限

在 WPF 用戶端，您可能增加的預設值為 2 的並行連線數目上限。 建議的值為 10。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

如需詳細資訊，請參閱 < [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>如何指定查詢字串參數

如果您想要將資料傳送至伺服器，用戶端連線時，您可以加入連接物件來查詢字串參數。 下列範例會示範如何在用戶端程式碼中設定查詢字串參數。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

下列範例示範如何讀取伺服器程式碼中的查詢字串參數。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>如何指定傳輸方法

連接的程序的一部分，通常與伺服器，以判斷最佳傳輸所支援的伺服器和用戶端交涉，SignalR 用戶端。 如果您已經知道您想要使用哪一個的傳輸，您可以略過此協商處理。 若要指定的傳輸方法，傳入的傳輸物件的 Start 方法。 下列範例示範如何在用戶端程式碼中指定的傳輸方法。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)命名空間包含下列類別可供您指定的傳輸。

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) （只有時才能使用伺服器和用戶端使用.NET 4.5。）
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) （會自動選擇最佳用戶端和伺服器所支援的傳輸。 這是預設的傳輸。 這在以傳遞`Start`方法有相同的效果不傳遞任何項目。)

ForeverFrame 傳輸不會包含這份清單中，因為它僅供瀏覽器。

如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-Server-如何取得用戶端的資訊，從內容屬性](hubs-api-guide-server.md#contextproperty)。 如需有關傳輸與後援的詳細資訊，請參閱[SignalR-傳輸和後援簡介](../getting-started/introduction-to-signalr.md#transports)。

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>如何指定 HTTP 標頭

若要設定 HTTP 標頭，使用`Headers`連線物件上的屬性。 下列範例示範如何新增 HTTP 標頭。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>如何指定用戶端憑證

若要新增用戶端憑證，請使用`AddClientCertificate`連線物件上的方法。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>如何建立中樞 proxy

為了定義中樞可以從伺服器呼叫的用戶端上的方法，並叫用中樞，以在伺服器上的方法，來建立中樞的 proxy 呼叫`CreateHubProxy`連線物件上。 字串您傳遞給`CreateHubProxy`是您的中樞類別名稱或所指定的名稱`HubName`屬性如果在伺服器上使用。 名稱比對不區分大小寫。

**在伺服器上的中樞類別**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**建立中樞類別的用戶端 proxy**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

如果裝飾您的中樞類別與`HubName`屬性，請使用該名稱。

**在伺服器上的中樞類別**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**建立中樞類別的用戶端 proxy**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

如果您呼叫`HubConnection.CreateHubProxy`多次使用相同`hubName`，您得到相同的快取`IHubProxy`物件。

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>如何定義伺服器可以呼叫用戶端上的方法

若要定義伺服器可以呼叫的方法，使用 proxy 的`On`方法，以註冊事件處理常式。

方法名稱比對不區分大小寫。 例如，`Clients.All.UpdateStockPrice`伺服器上將會執行`updateStockPrice`， `updatestockprice`，或`UpdateStockPrice`用戶端上。

不同的用戶端平台有不同撰寫方法的程式碼的方式來更新 UI 的需求。 所顯示的範例適用於 WinRT (Windows 市集.NET) 用戶端。 WPF、 Silverlight 和主控台應用程式範例中提供[本主題稍後的個別區段](#wpfsl)。

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>不含參數的方法

如果您正在處理的方法沒有參數，使用的非泛型多載`On`方法：

**伺服端程式碼呼叫不含參數的用戶端方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**WinRT 用戶端程式碼，方法呼叫不含參數的伺服器 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>使用指定的參數型別參數的方法

如果您正在處理的方法有參數，則指定的參數類型的泛型型別之`On`方法。 有泛型多載`On`方法，讓您指定最多 8 個參數 (在 Windows Phone 7 上為 4)。 在下列範例中，有一個參數會傳送至`UpdateStockPrice`方法。

**伺服端程式碼呼叫具有參數的用戶端方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**參數所用的庫存類別**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**來自伺服器的參數呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>具有參數，指定參數的動態物件的方法

做為泛型類型的指定參數的替代方式為`On`方法中，您可以指定參數作為動態物件：

**伺服端程式碼呼叫具有參數的用戶端方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**參數所用的庫存類別**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**從伺服器參數，使用動態物件的參數呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>如何移除處理常式

若要移除的處理常式，呼叫其`Dispose`方法。

**從伺服器呼叫的方法的用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**用戶端程式碼，以移除處理常式**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>如何從用戶端呼叫伺服器方法

若要在伺服器上呼叫方法，使用`Invoke`中樞 proxy 上的方法。

如果伺服器方法沒有傳回值，請使用非泛型多載`Invoke`方法。

**伺服端程式碼沒有傳回值的方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**用戶端程式碼呼叫的方法沒有傳回值，**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

如果伺服器方法的傳回值，指定傳回類型的泛型型別為`Invoke`方法。

**伺服端程式碼之方法的傳回值，並採用複雜型別參數**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**用於參數和傳回值的庫存類別**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**用戶端程式碼呼叫的方法，傳回的值，並採用 ASP.NET 4.5 非同步方法中的複雜型別參數，**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**用戶端程式碼呼叫的方法，傳回值和同步方法會採用複雜類型的參數，**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke`方法以非同步方式執行，並傳回`Task`物件。 如果您未指定`await`或`.Wait()`，您可以叫用的方法完成執行之前，將會執行下的一行程式碼。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>如何處理連接的存留期事件

SignalR 提供以下連線，您可以處理存留期事件：

- `Received`：在此連接上收到任何資料時，就會引發。 提供已接收的資料。
- `ConnectionSlow`：當用戶端偵測到較慢或經常卸除連接時引發。
- `Reconnecting`：基礎傳輸可讓您開始重新連線時引發。
- `Reconnected`：當基礎傳輸已重新連接時引發。
- `StateChanged`：連線狀態變更時引發。 提供的舊狀態和新的狀態。 如需連線狀態的值請參閱[ConnectionState 列舉](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)。
- `Closed`：當連接已中斷連線時，就會引發。

例如，如果您想要顯示的錯誤，不嚴重，但會造成間歇性連線問題的警告訊息，例如速度很慢或頻繁的連接，卸除處理`ConnectionSlow`事件。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件 SignalR](handling-connection-lifetime-events.md)。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>如何處理錯誤

如果您未明確啟用在伺服器上的詳細的錯誤訊息，SignalR 在發生錯誤之後所傳回的例外狀況物件包含有關錯誤的最少資訊。 例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解的目的，在伺服器上使用下列程式碼。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

若要處理 SignalR 引發的錯誤，您可以加入的處理常式`Error`連線物件上的事件。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

若要處理來自方法引動過程的錯誤，請將程式碼包裝在 try / catch 區塊中。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>如何啟用用戶端記錄

若要啟用用戶端記錄，請設定`TraceLevel`和`TraceWriter`連線物件上的屬性。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF、 Silverlight 和主控台應用程式的程式碼可以呼叫伺服器的用戶端方法的範例

稍早所示來定義伺服器可以呼叫的用戶端方法的程式碼範例會套用至 WinRT 用戶端。 下列範例顯示 WPF、 Silverlight 和主控台應用程式用戶端的對等的程式碼。

### <a name="methods-without-parameters"></a>不含參數的方法

**從伺服器不使用參數呼叫方法的 WPF 用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**從伺服器不使用參數呼叫方法的 Silverlight 用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**方法的主控台應用程式用戶端程式碼呼叫不含參數的伺服器**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>使用指定的參數型別參數的方法

**從伺服器的參數呼叫方法的 WPF 用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**從伺服器的參數呼叫方法的 Silverlight 用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**從具有參數的伺服器呼叫的方法的主控台應用程式用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>具有參數，指定參數的動態物件的方法

**從 伺服器參數，使用動態物件的參數呼叫方法的 WPF 用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**從 伺服器參數，使用動態物件的參數呼叫方法的 Silverlight 用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**主控台應用程式用戶端程式碼，從 伺服器參數，使用動態物件的參數呼叫方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
