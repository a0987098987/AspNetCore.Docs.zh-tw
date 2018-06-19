---
uid: signalr/overview/getting-started/introduction-to-signalr
title: SignalR 簡介 |Microsoft 文件
author: pfletcher
description: 本文說明 SignalR 的是，以及它設計用來建立解決方案的部分。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0ceca3edc26d35b1155946e60863a84da0bbe592
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873689"
---
<a name="introduction-to-signalr"></a>SignalR 的簡介
====================
由[Patrick Fletcher](https://github.com/pfletcher)

> 本文說明 SignalR 的是，以及它設計用來建立解決方案的部分。 
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)。


## <a name="what-is-signalr"></a>SignalR 是什麼？

ASP.NET SignalR 是 ASP.NET 開發人員的程式庫，可簡化將即時 web 功能新增至應用程式的程序。 即時 web 功能是能夠讓伺服器程式碼推入內容給連接的用戶端立即可用，而不是讓伺服器等候用戶端要求新的資料。

SignalR 可用來將任何類型的 「 即時 」 web 功能新增至您的 ASP.NET 應用程式。 雖然交談通常會使用做為範例中，您可以一大堆多。 每當使用者重新整理網頁即可看到新的資料，或頁面實作[長輪詢](http://en.wikipedia.org/wiki/Push_technology#Long_polling)擷取新的資料，它是使用 SignalR 的候選。 範例包括儀表板和監視應用程式、 共同作業應用程式 （例如同時編輯的文件），工作進度更新和即時的表單。

SignalR 也可讓支援全新的 web 應用程式需要在伺服器上，從高頻率更新類型，例如即時遊戲。

SignalR 提供一個簡單 API 來建立伺服器到用戶端的遠端程序呼叫 (RPC) 從伺服器端.NET 程式碼呼叫 JavaScript 函式，在用戶端瀏覽器 （和其他用戶端平台）。 SignalR 也包含連接管理 API （例如，連接和中斷連線事件），及群組的連線。

![與 SignalR 的叫用方法](introduction-to-signalr/_static/image1.png)

SignalR 自動處理連接管理，並像聊天室的同時，讓您廣播給所有連接的用戶端的訊息。 您也可以傳送訊息給特定的用戶端。 用戶端與伺服器之間的連線是永久性的不同於傳統的 HTTP 連線，也就是重新建立每個通訊的。

SignalR 支援 「 伺服器推入 」 功能，伺服端程式碼呼叫立即從網站上使用遠端程序呼叫 (RPC)，而不是常用的要求-回應模型瀏覽器中的用戶端程式碼。

SignalR 應用程式可以向外延展至數千個用戶端使用服務匯流排、 SQL Server 或[Redis](http://redis.io)。

SignalR 是開放原始碼，可透過存取[GitHub](https://github.com/signalr)。

## <a name="signalr-and-websocket"></a>SignalR 和 WebSocket

SignalR 情況下，會使用新的 WebSocket 傳輸，並會回復為舊版傳輸在需要時。 雖然您當然可以撰寫您直接使用 WebSocket，請使用 SignalR 表示額外的功能，您需要實作大量會有已完成您的應用程式。 最重要的是，這表示您可以撰寫您的應用程式，以利用 WebSocket 而不必擔心舊版的用戶端建立不同的程式碼路徑。 SignalR 也保護不受影響您不必擔心更新 WebSocket，因為 SignalR 會繼續更新為基礎的傳輸，支援的變更在 WebSocket 版本之間提供您的應用程式一致的介面。

當然，您可以建立使用 WebSocket 單獨的方案，而 SignalR 提供的所有功能，您必須撰寫您自己，例如回到其他傳輸和修訂您的應用程式更新的 WebSocket 實作。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>傳輸和後援

SignalR 是抽象的傳輸，才能執行用戶端與伺服器之間的即時工作的部分。 SignalR 連線做為 HTTP，啟動，然後提升為 WebSocket 連接是否可用。 WebSocket 是 SignalR 的理想傳輸，因為它可讓伺服器記憶體最有效地使用延遲最低、 有最基本功能 （例如完整雙工用戶端和伺服器之間通訊），但它也具有最嚴格需求： WebSocket 要求的伺服器會使用 Windows Server 2012 或 Windows 8 和.NET Framework 4.5。 如果不符合這些需求，SignalR 會嘗試使用其他傳輸進行其連線。

### <a name="html-5-transports"></a>HTML 5 傳輸

這些傳輸的支援取決於[HTML 5](http://en.wikipedia.org/wiki/HTML5)。 如果用戶端瀏覽器不支援 HTML 5 標準，將會使用較舊的傳輸。

- **WebSocket** (如果它們可以支援 Websocket 伺服器和瀏覽器表示)。 WebSocket 是只會建立，則為 true 的持續性的雙向連線用戶端與伺服器之間的傳輸。 不過，WebSocket 也具有最嚴格的需求。它只能在 Microsoft Internet Explorer 及 Google Chrome、 Mozilla Firefox 的最新版本中完全支援，並在其他瀏覽器，例如 Opera 與 Safari 中只有部分實作。
- **伺服器傳送事件**，也稱為 EventSource （如果瀏覽器支援伺服器傳送事件，這基本上是 Internet Explorer 以外的所有瀏覽器）。

### <a name="comet-transports"></a>Comet 傳輸

會根據下列傳輸[Comet](http://en.wikipedia.org/wiki/Comet_(programming))瀏覽器或其他用戶端維持長時間保留 HTTP 要求中，伺服器可以用來將資料推送至沒有用戶端的用戶端特別的 web 應用程式模型要求的方式。

- **永久框架**（適用於僅限 Internet Explorer)。 永久框架會建立隱藏的 IFrame 的未完成的伺服器上的端點提出要求。 伺服器再持續傳送指令碼至用戶端立即執行，此用戶端從伺服器的單向即時連線。 用戶端與伺服器的連接使用個別連接從伺服器到用戶端，並像標準的 HTTP 要求中，針對每個需要傳送的資料片段建立新的連接。
- **Ajax 長期輪詢**。 長期輪詢不會建立持續連線，但是會改為輪詢伺服器的要求，直到伺服器的回應，此時會關閉連接，並立即要求新的連接會保持開啟。 重設連接時，這樣可能會導致一些延遲。

如需有關在哪些組態支援何種傳輸的詳細資訊，請參閱[支援的平台](supported-platforms.md)。

### <a name="transport-selection-process"></a>傳輸選取程序

下列清單顯示 SignalR 用來決定要使用的傳輸的步驟。

1. 如果瀏覽器是 Internet Explorer 8 或更早版本，會使用長輪詢。
2. 如果設定 JSONP (也就是`jsonp`參數設定為`true`連接上啟動時)，長輪詢用。
3. 如果建立跨定義域連線時 （也就是如果 SignalR 端點不是在與裝載網頁位於相同網域中），然後 WebSocket 如果就會使用符合下列準則：

   - 用戶端支援 CORS （跨原始資源共用）。 在用戶端支援 CORS 的詳細資訊，請參閱[在 caniuse.com CORS](http://www.caniuse.com/CORS)。
   - 用戶端支援的 WebSocket
   - 伺服器支援 WebSocket

     如果不符合任何一個準則，將會使用長輪詢。 如需有關跨網域連線的詳細資訊，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。
4. 如果未設定 JSONP 連接不是跨網域，如果用戶端和伺服器支援它，就會使用 WebSocket。
5. 如果用戶端或伺服器不支援 WebSocket，如果有的話，就是會使用伺服器傳送事件。
6. 如果找不到伺服器傳送事件，會嘗試永久框架。
7. 如果永久框架失敗時，會使用長輪詢。

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>監視的傳輸

您可以判斷應用程式藉由啟用登入您的中樞，並在您的瀏覽器中開啟 [主控台] 視窗中使用何種傳輸。

若要啟用您的中樞事件的瀏覽器中的記錄，用戶端應用程式，將下列命令：

`$.connection.hub.logging = true;`

- 在 Internet Explorer 中，開啟 [開發人員工具]，請按 F12，然後按一下 [主控台] 索引標籤。

    ![在 Microsoft Internet Explorer 中的主控台](introduction-to-signalr/_static/image2.png)
- 在 Chrome 中，請按 Ctrl + Shift + J 開啟主控台。

    ![Google Chrome 中的主控台](introduction-to-signalr/_static/image3.png)

主控台開啟和已啟用記錄，您可以查看 SignalR 正在使用的傳輸。

![主控台中顯示 WebSocket 傳輸的 Internet Explorer](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>指定傳輸

交涉傳輸會在特定的一段時間和用戶端/伺服器資源。 如果已知的用戶端功能，則為傳輸可以指定啟動用戶端連線時。 下列程式碼片段會示範如何開始使用 Ajax 長輪詢傳輸，如果它已知的用戶端並不支援任何其他通訊協定所使用的連接：

`connection.start({ transport: 'longPolling' });`

如果您想要再試一次在順序中的特定傳輸的用戶端，您可以指定後援的順序。 下列程式碼片段示範嘗試 WebSocket，並再次失敗，直接前往長輪詢。

`connection.start({ transport: ['webSockets','longPolling'] });`

用來指定傳輸的字串常數的定義方式如下：

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>連線和中樞

SignalR 應用程式開發介面包含兩個模型，以用戶端和伺服器之間的通訊： 持續性連線和中樞。

連接代表單一收件者、 群組或廣播訊息傳送的簡單端點。 持續性連線應用程式開發介面 （由.NET 程式碼 PersistentConnection 類別） 可讓開發人員直接存取 SignalR 公開 （expose） 的低層級的通訊協定。 使用連線通訊模型都很熟悉的開發人員使用以連線為基礎的 Api，例如 Windows Communication Foundation。

建立連接的 API，可讓用戶端與伺服器彼此直接呼叫方法所在的更高層級管線的中樞。 SignalR 處理如同魔術，跨電腦界限分派允許用戶端在伺服器上呼叫方法，以輕鬆地為本機的方法，反之亦然。 使用中樞通訊模型都很熟悉的開發人員使用遠端叫用應用程式開發介面，例如.NET 遠端處理。 使用中樞也可讓您將強型別的參數傳遞給方法，可讓模型繫結。

### <a name="architecture-diagram"></a>架構圖表

下圖顯示集線器、 持續連線，以及用於傳輸的基礎技術之間的關聯性。

![顯示應用程式開發介面、 傳輸與用戶端的 SignalR 架構圖](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>中樞的運作方式

當伺服器端程式碼在用戶端上呼叫方法時，作用中傳輸，其中包含要呼叫之方法的參數與名稱之間，會傳送封包 （當物件做為方法參數傳送，它會使用序列化 JSON）。 接著，用戶端會比對方法名稱，以在用戶端程式碼中定義的方法。 如果沒有相符項目，將使用已還原序列化的參數資料執行用戶端方法。

方法呼叫可以使用類似的工具來監視[Fiddler。](http://fiddler2.com/) 下圖顯示從 SignalR 伺服器傳送至 Fiddler 的記錄檔 窗格中的 web 瀏覽器用戶端方法呼叫。 方法呼叫傳送與中樞呼叫`MoveShapeHub`，並叫用此方法會呼叫`updateShape`。

![Fiddler 日誌顯示 SignalR 流量的檢視](introduction-to-signalr/_static/image6.png)

在此範例中，以識別中樞名稱`H`參數; 方法名稱會用來識別`M`參數和資料傳送至該方法會用來識別`A`參數。 產生此訊息的應用程式中建立[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)教學課程。

### <a name="choosing-a-communication-model"></a>選擇的通訊模式

大部分的應用程式應該使用中樞應用程式開發介面。 連接 API 無法用於下列情況：

- 實際傳送的訊息必須指定格式。
- 開發人員偏好使用傳訊和發送模型，而不是遠端的引動過程模型。
- 使用 SignalR 被移植現有的應用程式使用訊息模式。
