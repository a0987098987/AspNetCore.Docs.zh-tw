---
uid: signalr/overview/getting-started/introduction-to-signalr
title: SignalR 簡介 |Microsoft Docs
author: pfletcher
description: 此文章說明 SignalR 為何，以及一些應建立解決方案。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: c865078c14b8615faa278819f86a9dd623a42f36
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287561"
---
<a name="introduction-to-signalr"></a>SignalR 簡介
====================

藉由[Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]


> 此文章說明 SignalR 為何，以及一些應建立解決方案。 
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)。

## <a name="what-is-signalr"></a>SignalR 是什麼？

ASP.NET SignalR 是 ASP.NET 開發人員適用的程式庫，可簡化將即時 web 功能新增至應用程式的程序。 即時 web 功能是能夠有伺服器程式碼推送內容至連線的用戶端立即可供使用，而不需要伺服器等候用戶端要求新的資料。

SignalR 可用來將任何類型的 「 即時 」 的 web 功能新增至您的 ASP.NET 應用程式。 交談通常是用做為範例，您可以處理一大堆多。 每當使用者重新整理網頁，以查看新的資料，或頁面實作[長輪詢](http://en.wikipedia.org/wiki/Push_technology#Long_polling)擷取新的資料，它是使用 SignalR 的候選。 範例包括儀表板和監視應用程式，共同作業應用程式 （例如同時編輯的文件），工作進度更新和即時的表單。

SignalR 也可讓 web 應用程式需要頻繁更新，從伺服器的全新類型，例如即時遊戲。

SignalR 提供一個簡單 API 來建立伺服器到用戶端的遠端程序呼叫 (RPC) 從伺服器端.NET 程式碼呼叫 JavaScript 函式，在用戶端瀏覽器 （和其他用戶端平台）。 SignalR 也包含連接管理 API （例如，連接和中斷連接事件），和群組的連線。

![使用 SignalR 的叫用方法](introduction-to-signalr/_static/image1.png)

SignalR 自動處理連接管理，並像聊天室的案例，同時讓您廣播到所有已連線的用戶端的訊息。 您也可以傳送訊息給特定的用戶端。 用戶端與伺服器之間的連線是持續性的不同於傳統的 HTTP 連線，也就是重新建立每個通訊。

SignalR 支援 「 伺服器推入 」 功能，伺服端程式碼呼叫立即在網站上使用遠端程序呼叫 (RPC)，而不是常見的要求-回應模型的瀏覽器中的用戶端程式碼。

SignalR 應用程式可以擴充至數千個用戶端使用服務匯流排、 SQL Server 或[Redis](http://redis.io)。

SignalR 是開放原始碼，可透過存取[GitHub](https://github.com/signalr)。

## <a name="signalr-and-websocket"></a>SignalR 和 WebSocket

SignalR （如果可用），會使用新的 WebSocket 傳輸，並會回復到舊版的傳輸在必要時。 雖然您當然可以撰寫您的應用程式直接使用 WebSocket，請使用 SignalR，表示您已經完成額外的功能，您就需要實作許多。 最重要的是，這表示您可以撰寫您的應用程式，以利用而不必擔心建立不同的程式碼路徑的較舊的用戶端 WebSocket。 SignalR 也會幫助您不必擔心 WebSocket，更新，因為 SignalR 會更新為基礎的傳輸，支援變更的 WebSocket 版本提供您的應用程式一致的介面。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>傳輸和後援

SignalR 是一些需要執行用戶端與伺服器之間的即時工作傳輸的抽象概念。 SignalR 連線一開始為 HTTP，並再升級為 WebSocket 連線是否可用。 WebSocket 是 SignalR 的理想傳輸，因為伺服器記憶體的最有效率的使用、 具有最低的延遲，以及具有最基礎的功能 （例如完整的雙工用戶端和伺服器之間通訊），但它也具有最嚴格需求：WebSocket 要求伺服器使用 Windows Server 2012 或 Windows 8 和.NET Framework 4.5。 如果不符合這些需求，SignalR 會嘗試使用其他傳輸進行其連線。

### <a name="html-5-transports"></a>HTML 5 傳輸

支援取決於這些傳輸[HTML 5](http://en.wikipedia.org/wiki/HTML5)。 如果用戶端瀏覽器不支援 HTML 5 標準，就會使用較舊的傳輸。

- **WebSocket** (如果伺服器和瀏覽器指出它們可以支援 Websocket)。 WebSocket 是唯一會建立，則為 true 的持續性的雙向連線用戶端與伺服器之間的傳輸。 不過，WebSocket 也具有最嚴格的要求;它完全只支援最新版的 Microsoft Internet Explorer、 Google Chrome 和 Mozilla Firefox 和 Opera 和 Safari 等其他瀏覽器中只有部分的實作。
- **伺服器傳送事件**，也稱為 EventSource （如果瀏覽器支援伺服器傳送事件，這基本上是 Internet Explorer 以外的所有瀏覽器）。

### <a name="comet-transports"></a>Comet 傳輸

下列傳輸為基礎[Comet](http://en.wikipedia.org/wiki/Comet_(programming))瀏覽器或其他用戶端維持長時間保留 HTTP 要求，伺服器可以用來將資料推送至用戶端不是用戶端特別的 web 應用程式模型要求的方式。

- **永久框架**（適用於僅限 Internet Explorer)。 永久框架會建立隱藏的 IFrame 的未完成的伺服器上的端點提出要求。 伺服器再持續傳送指令碼到用戶端，便會立即執行，提供用戶端從伺服器的單向的即時連線。 從用戶端到伺服器的連線會使用從伺服器個別連接到用戶端，並像標準的 HTTP 要求中，針對每一項需要傳送的資料建立新的連線。
- **長時間輪詢的 Ajax**。 長時間輪詢不會建立持續連線，但改為會輪詢伺服器以維持開啟，直到伺服器回應，屆時會關閉連接，並立即要求新的連接要求。 連接重設時，這可能會造成一些延遲。

如需有關何種傳輸皆支援哪些組態的詳細資訊，請參閱[支援的平台](supported-platforms.md)。

### <a name="transport-selection-process"></a>傳輸選取程序

下列清單顯示 SignalR 用來決定要使用的傳輸的步驟。

1. 如果瀏覽器是 Internet Explorer 8 或更早版本，會使用長輪詢。
2. 如果設定 JSONP (亦即`jsonp`參數設為`true`啟動連線時)，長輪詢用。
3. 如果建立跨網域連線時 （也就是如果 SignalR 端點不在相同的網域裝載的網頁中），則 WebSocket 會使用如果符合下列準則：

   - 用戶端支援 CORS （跨原始資源共用）。 在其的用戶端支援 CORS 的詳細資訊，請參閱 < [caniuse.com 在 CORS](http://www.caniuse.com/CORS)。
   - 用戶端支援 WebSocket
   - 伺服器支援 WebSocket

     如果不符合任何這些準則，將會使用長輪詢。 如需有關跨網域連接的詳細資訊，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。
4. 如果未設定 JSONP 連接不是跨網域，如果用戶端和伺服器支援它，就會使用 WebSocket。
5. 如果用戶端或伺服器不支援 WebSocket，如果有的話，就是會使用伺服器傳送事件。
6. 如果無法使用伺服器傳送事件，則會嘗試不限次數的框架。
7. 如果不限次數的框架就會失敗，會使用長輪詢。

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>監視的傳輸

您可以判斷您的應用程式使用藉由啟用登入您的中樞，並在您的瀏覽器中開啟主控台視窗中所使用的傳輸。

若要啟用您的中樞事件，在瀏覽器中的記錄功能，加入您的用戶端應用程式中的下列命令：

`$.connection.hub.logging = true;`

- 在 Internet Explorer 中，開啟 [開發人員工具]，請按 F12，然後按一下 [主控台] 索引標籤。

    ![在 Microsoft Internet Explorer 中的主控台](introduction-to-signalr/_static/image2.png)
- 在 Chrome 中，請按下 Ctrl + Shift + J 開啟主控台。

    ![在 Google Chrome 中的主控台](introduction-to-signalr/_static/image3.png)

主控台開啟和啟用記錄，您將能夠看到 SignalR 正在使用的傳輸。

![主控台中顯示 WebSocket 傳輸的 Internet Explorer](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>指定傳輸

交涉傳輸需要一段時間和用戶端/伺服器資源。 如果已知用戶端功能，則傳輸可以指定啟動用戶端連線時。 下列程式碼片段會示範如何開始使用 Ajax 長輪詢傳輸，如果它已知的用戶端並不支援任何其他通訊協定所使用的連接：

`connection.start({ transport: 'longPolling' });`

如果您想要嘗試在順序中的特定傳輸的用戶端，您可以指定後援的順序。 下列程式碼片段示範嘗試 WebSocket，而且失敗，直接前往長輪詢。

`connection.start({ transport: ['webSockets','longPolling'] });`

指定傳輸的字串常數定義，如下所示：

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>連線和中樞

SignalR API 包含用戶端和伺服器之間進行通訊的兩個模型：持續連線和中樞。

連接代表單一收件者、 群組或廣播訊息傳送的簡單端點。 開發人員直接存取 SignalR 公開的低階通訊協定 （由.NET 程式碼 PersistentConnection 類別） 的持續性連接 API 提供。 使用連線的通訊模型都很熟悉的開發人員已使用連接為基礎的 Api，例如 Windows Communication Foundation。

中樞是建置連接 API，可讓您的用戶端與伺服器彼此直接呼叫方法為基礎的更高層級管線。 SignalR 處理分派跨電腦界限的 magic，如同允許用戶端呼叫伺服器上的方法，以輕鬆地為本機方法，反之亦然。 使用中樞通訊模型都很熟悉的開發人員已使用遠端叫用 Api，例如.NET 遠端處理。 使用中樞也可讓您將強型別的參數傳遞給方法，可讓模型繫結。

### <a name="architecture-diagram"></a>架構圖

下圖顯示中樞、 持續性連線和用於傳輸的基礎技術之間的關聯性。

![顯示 Api、 傳輸和用戶端的 SignalR 架構圖](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>中樞的運作方式

當伺服器端程式碼會在用戶端上呼叫的方法時，在作用中傳輸，其中包含的名稱和要呼叫之方法的參數傳送封包 （如果物件作為方法參數傳送，它會使用 JSON 序列化）。 接著，用戶端會比對方法名稱，以用戶端程式碼中定義的方法。 如果沒有相符項目，就將使用已還原序列化的參數資料來執行用戶端方法。

您可以使用工具，例如監視方法呼叫[Fiddler。](http://fiddler2.com/) 下圖顯示從 SignalR 伺服器傳送至 web 瀏覽器用戶端的 Fiddler 的記錄檔 窗格中的方法呼叫。 正在傳送方法呼叫從中樞，稱為`MoveShapeHub`，並叫用的方法呼叫`updateShape`。

![顯示 SignalR 流量的 Fiddler 記錄的檢視](introduction-to-signalr/_static/image6.png)

在此範例中的中樞名稱會用來識別`H`參數，方法名稱會用來識別`M`參數，並傳送至方法的資料會用來識別`A`參數。 產生這則訊息的應用程式中建立[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)教學課程。

### <a name="choosing-a-communication-model"></a>選擇的通訊模型

大部分的應用程式應該使用中樞 API。 連線 API 無法用於下列情況：

- 實際傳送的訊息必須指定格式。
- 開發人員偏好使用傳訊和發送模型，而不是遠端的引動過程模型。
- 使用 SignalR 被移植現有的應用程式使用訊息的模型。
