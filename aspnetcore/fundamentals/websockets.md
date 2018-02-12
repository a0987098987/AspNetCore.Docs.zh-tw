---
title: "ASP.NET Core 中的 WebSockets 支援"
author: tdykstra
description: "了解如何在 ASP.NET Core 中開始使用 WebSocket。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>ASP.NET Core 中的 WebSocket 簡介

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Andrew Stanton-Nurse](https://github.com/anurse)

本文說明如何在 ASP.NET Core 中開始使用 WebSocket。 [WebSocket](https://wikipedia.org/wiki/WebSocket) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。 其可用於像是聊天、股票行情、遊戲等應用程式，以及您希望在 Web 應用程式中使用即時功能的任何位置。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。 如需詳細資訊，請參閱[後續步驟](#next-steps)一節。


## <a name="prerequisites"></a>必要條件

* ASP.NET Core 1.1 (請勿在 1.0 上執行)
* 執行 ASP.NET Core 的任何作業系統：
  
  * Windows 7 / Windows Server 2008 和更新版本
  * Linux
  * macOS

* **例外狀況**：如果在 Windows 上搭配 IIS 或搭配 WebListener 執行應用程式，您必須使用：

  * Windows 8 / Windows Server 2012 或更新版本
  * IIS 8 / IIS 8 Express
  * 必須在 IIS 中啟用 WebSocket

* 如需支援的瀏覽器，請參閱 http://caniuse.com/#feat=websockets。

## <a name="when-to-use-it"></a>使用時機

當您必須直接使用通訊端連線時，請使用 WebSocket。 例如，您可能需要針對即時遊戲取得最佳效能。

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) 提供更豐富的應用程式模型來執行即時功能，但它只會在 ASP.NET 上執行，而不是在 ASP.NET Core 上執行。 SignalR Core 版本仍在開發中；若要追蹤其進度，請參閱 [SignalR Core 的 GitHub 存放庫](https://github.com/aspnet/SignalR)。

如果您不想等待 SignalR Core，現在可以直接使用 WebSocket。 但是您可能需要開發 SignalR 提供的功能，例如：

* 透過使用替代傳輸方法的自動後援，支援廣泛的瀏覽器版本。
* 連線中斷時自動重新連線。
* 支援伺服器上的用戶端呼叫方法，反之亦然。
* 支援擴展為多部伺服器。

## <a name="how-to-use-it"></a>如何使用

* 安裝 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 套件。
* 設定中介軟體。
* 接受 WebSocket 要求。
* 傳送和接收訊息。

### <a name="configure-the-middleware"></a>設定中介軟體

在 `Startup` 類別的 `Configure` 方法中新增 WebSocket 中介軟體。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

您可以設定下列設定：

* `KeepAliveInterval` - 要將 "ping" 框架傳送到用戶端，以確保 Proxy 保持連線開啟的頻率。
* `ReceiveBufferSize` - 用來接收資料的緩衝區大小。 只有進階使用者才必須變更此設定，以便根據其資料的大小進行效能調整。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>接受 WebSocket 要求

在要求生命週期的後半部某處 (例如，在 `Configure` 方法或 MVC 動作的後半部)，檢查它是否為 WebSocket 要求並接受 WebSocket 要求。

這個範例取自 `Configure` 方法的後半部。

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket 要求可以傳入任何 URL，但此範例程式碼只接受 `/ws` 的要求。

### <a name="send-and-receive-messages"></a>傳送和接收訊息

`AcceptWebSocketAsync` 方法可將 TCP 連線升級為 WebSocket 連線，並提供您 [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) 物件。 請使用 WebSocket 物件來傳送和接收訊息。

稍早所示接受 WebSocket 要求的程式碼會將 `WebSocket` 物件傳遞給 `Echo` 方法；以下是 `Echo` 方法。 程式碼會收到一則訊息，並立即傳送回相同的訊息。 它會保留在執行該動作的迴圈中，直到用戶端關閉連線為止。 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

如果您在開始此迴圈之前接受 WebSocket，中介軟體管線就會結束。  在關閉通訊端後，管線會回溯。 也就是說，當您接受 WebSocket 時，要求會在管線中停止向前移動；比方說，就跟當您叫用 MVC 動作時一樣。  但是，在您完成此迴圈並關閉通訊端時，要求會繼續備份管線。

## <a name="next-steps"></a>後續步驟

本文附帶的[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)是簡單的回應應用程式。 其具有一個進行 WebSocket 連線的網頁，而伺服器只會將其接收的任何訊息重新傳送回用戶端。 請從命令提示字元執行它 (它未設定為從 Visual Studio 搭配 IIS Express 執行)，並巡覽至 http://localhost:5000。 網頁會在左上方顯示連線狀態：

![網頁的初始狀態](websockets/_static/start.png)

選取 [連線] 將 WebSocket 要求傳送到顯示的 URL。  輸入測試訊息，然後選取 [傳送]。 完成後，請選取 [關閉通訊端]。 [通訊記錄檔] 區段會報告每次進行的開啟、傳送和關閉動作。

![網頁的初始狀態](websockets/_static/end.png)
