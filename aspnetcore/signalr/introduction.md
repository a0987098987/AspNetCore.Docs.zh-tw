---
title: ASP.NET Core SignalR 簡介
author: tdykstra
description: 了解 ASP.NET Core SignalR 程式庫如何簡化將即時功能新增至應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095385"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core SignalR 簡介

作者：[Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>SignalR 是什麼？

ASP.NET Core SignalR 是能簡化新增即時 web 功能的應用程式的程式庫。 即時 web 功能立即可讓用戶端推入內容的伺服器端程式碼。

Signalr 的良好候選項目：

* 需要來自伺服器的高頻率更新的應用程式。 範例包括遊戲、 社交網路、 投票、 拍賣、 對應和 GPS 應用程式。
* 儀表板和監視的應用程式。 範例包括公司儀表板，立即銷售的更新，或的旅遊警示。
* 共同作業應用程式。 白板應用程式和小組會議軟體是共同作業應用程式的範例。
* 需要通知的應用程式。 社交網路、 電子郵件、 聊天室、 遊戲、 的移動警示和許多其他應用程式使用通知。

SignalR 提供的 API 來建立伺服器到用戶端[遠端程序呼叫 (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)。 從伺服器端.NET Core 程式碼，Rpc 會呼叫 JavaScript 函式在用戶端上。

適用於 ASP.NET Core SignalR:

* 會自動處理連接管理。
* 啟用同時廣播到所有已連線的用戶端的訊息。 比方說，聊天室。
* 能夠將訊息傳送至特定的用戶端或群組的用戶端。
* 是開放原始碼，在[GitHub](https://github.com/aspnet/signalr)。
* 可調整。

用戶端與伺服器之間的連線是持續性的不同於 HTTP 連線。

## <a name="transports"></a>傳輸

SignalR 透過一些技巧來建置即時 web 應用程式的摘要。 [Websocket](https://tools.ietf.org/html/rfc7118)是最佳的傳輸，但這些無法使用時，就可以使用其他方法，像是 Server-Sent 事件和長輪詢。 SignalR 會自動偵測，並初始化適當的伺服器和用戶端上支援的功能為基礎的傳輸。

## <a name="hubs"></a>中樞

SignalR 使用中樞用戶端和伺服器之間進行通訊。

中樞是可讓您的用戶端與伺服器彼此呼叫方法的高階管線。 SignalR 處理分派跨電腦界限會自動允許用戶端呼叫伺服器上的方法，以輕鬆地為本機方法，反之亦然。 中樞可讓強型別參數傳遞至方法，可使用模型繫結。 SignalR 提供兩個內建中樞通訊協定： 文字通訊協定會根據 JSON 和二進位通訊協定為基礎[MessagePack](https://msgpack.org/)。  MessagePack 通常會建立較小的訊息，比使用 JSON。 必須支援舊版瀏覽器[XHR 層級 2](https://caniuse.com/#feat=xhr2)提供 MessagePack 通訊協定支援。

中樞會呼叫用戶端程式碼，藉由使用作用中傳輸傳送訊息。 訊息包含的名稱和用戶端方法的參數。 做為方法參數傳送的物件會使用設定的通訊協定來還原序列化。 用戶端會嘗試比對用戶端程式碼中的方法名稱。 相符項目時，會執行用戶端方法，使用已還原序列化的參數資料。

## <a name="additional-resources"></a>其他資源

* [開始使用 SignalR 的 ASP.NET Core](xref:tutorials/signalr)
* [支援的平台](xref:signalr/supported-platforms)
* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
