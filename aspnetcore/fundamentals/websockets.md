---
title: "在 ASP.NET Core Websocket 支援"
author: tdykstra
description: "什麼是 Websocket 支援 ASP.NET Core 及如何使用它。"
keywords: ASP.NET Core WebSockets
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 8a6b5cc8ca8ac17f0e4c5b23f20013130cd472c8
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>在 ASP.NET Core WebSockets 簡介

由[Tom Dykstra](https://github.com/tdykstra)和[Andrew Stanton 護士](https://github.com/anurse)

本文說明如何開始使用 ASP.NET Core 中 WebSockets。 [WebSocket](https://wikipedia.org/wiki/WebSocket)是啟用透過 TCP 連線的持續性的雙向通訊通道的通訊協定。 用於應用程式，例如聊天、 股票行情即時看板、 遊戲，您想要在 web 應用程式的即時功能的任何位置。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)。 請參閱[接下來的步驟](#next-steps)節的詳細資訊。


## <a name="prerequisites"></a>必要條件

* ASP.NET Core 1.1 （不執行於 1.0）
* ASP.NET Core 執行的任何作業系統：
  
  * Windows 7 / Windows Server 2008 及更新版本
  * Linux
  * MacOS

* **例外狀況**： 如果您的應用程式會使用 IIS 時，Windows 上執行，或使用 WebListener，您必須使用：

  * Windows 8 / Windows Server 2012 或更新版本
  * IIS 8 / IIS 8 Express
  * 必須在 IIS 中啟用 WebSocket

* 支援的瀏覽器，請參閱 http://caniuse.com/#feat=websockets。

## <a name="when-to-use-it"></a>使用時機

當您需要直接使用的通訊端連接時使用 Websocket。 例如，您可能需要最佳效能，即時遊戲。

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr)提供更豐富的應用程式模型進行即時的功能，但是它只會在執行 ASP.NET、 非 ASP.NET Core。 SignalR 的核心版本仍在開發。若要依照其進度，請參閱[SignalR Core 的 GitHub 儲存機制](https://github.com/aspnet/SignalR)。

如果您不想等候 SignalR Core，您現在可以使用 WebSockets 直接。 但您可能想要開發 SignalR 會提供功能，例如：

* 廣泛使用替代的傳輸方法，自動遞補的瀏覽器版本的支援。
* 自動重新連線時卸除連接。
* 支援用戶端呼叫方法，在伺服器上，反之亦然。
* 縮放至多部伺服器的支援。

## <a name="how-to-use-it"></a>如何使用它

* 安裝[Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/)封裝。
* 設定中介軟體。
* 接受 WebSocket 要求。
* 傳送和接收訊息。

### <a name="configure-the-middleware"></a>設定中介軟體

加入的 Websocket 中介軟體中`Configure`方法`Startup`類別。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

可以設定下列設定：

* `KeepAliveInterval`-如何經常"ping"框架傳送到用戶端，以確保 proxy 保持開啟的連接。
* `ReceiveBufferSize`的用來接收資料的緩衝區大小。 只有進階的使用者必須變更此設定，進行效能微調根據其資料的大小。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>接受 WebSocket 要求

在要求的生命週期中某處更新 (稍後`Configure`方法或在 MVC 動作中，例如) 檢查它是否為 WebSocket 要求並接受 WebSocket 要求。

這個範例取自稍後在`Configure`方法。

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket 要求無法傳入任何 URL，但此範例程式碼只會接受要求`/ws`。

### <a name="send-and-receive-messages"></a>傳送和接收訊息

`AcceptWebSocketAsync`方法升級 WebSocket 連線的 TCP 連線，並提供您[WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket)物件。 您可以使用 WebSocket 物件來傳送和接收訊息。

在稍早所示，接受 WebSocket 要求的程式碼通過`WebSocket`物件`Echo`方法; 這裡的`Echo`方法。 程式碼會收到一則訊息，並立即傳送回相同的訊息。 它會保持在迴圈，這麼做可在用戶端關閉連接。 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

當您開始此迴圈之前接受 WebSocket 時中, 介軟體管線就會結束。  在關閉通訊端，管線會回溯。 也就是在管線中向前移動，當您接受 WebSocket，因為它要求停駐點就是當您叫用 MVC 動作，例如。  但是，當您完成此迴圈，並關閉通訊端，要求會繼續備份管線。

## <a name="next-steps"></a>後續步驟

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)伴隨著此發行項是簡單的回應的應用程式。 它具有 WebSocket 連接的網頁，以及在伺服器重新傳送回用戶端接收之任何訊息。 執行 （它有未設定從 Visual Studio 和 IIS Express 上執行） 在命令提示字元並瀏覽至 http://localhost:5000/。 Web 網頁會顯示在左上方的連接狀態：

![網頁的初始狀態](websockets/_static/start.png)

選取**連接**將 WebSocket 要求傳送至顯示的 URL。  輸入測試訊息，並選取**傳送**。 完成，請選取**關閉通訊端**。 **通訊記錄**區段會報告每次開啟時，傳送，並關閉的動作，因為它。

![網頁的初始狀態](websockets/_static/end.png)
