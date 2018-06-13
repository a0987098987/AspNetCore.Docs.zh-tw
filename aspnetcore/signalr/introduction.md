---
title: ASP.NET Core SignalR 簡介
author: rachelappel
description: 了解如何 ASP.NET Core SignalR 程式庫可簡化將即時功能加入至應用程式。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: f05b7cbf05372dc5d5cdadaf5a534d7a9d9bfecc
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2018
ms.locfileid: "33923351"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core SignalR 簡介

作者：[Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>SignalR 是什麼？

ASP.NET Core SignalR 是簡化新增即時 web 功能的應用程式的程式庫。 即時 web 功能立即可讓用戶端推入內容的伺服器端程式碼。

適用於 SignalR 的對象：

* 需要來自伺服器的高頻率更新的應用程式。 範例包括遊戲、 社交網路、 投票、 稽核、 對應和 GPS 應用程式。
* 儀表板和監視的應用程式。 範例包括公司儀表板，立即銷售的更新，或傳送警示。
* 共同作業應用程式。 白板應用程式和軟體的會議的小組都協同合作應用程式的範例。
* 需要通知的應用程式。 社交網路、 電子郵件、 聊天室、 遊戲、 旅行警示和許多其他的應用程式使用通知。

SignalR 提供一個 API 建立伺服器到用戶端[遠端程序呼叫 (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)。 Rpc 用戶端上呼叫 JavaScript 函數，從伺服器端.NET Core 程式碼。

SignalR 的 ASP.NET Core:

* 會自動處理連接管理。
* 同時廣播給所有連接的用戶端的訊息啟用。 例如，小組室。
* 能夠將訊息傳送至特定的用戶端或用戶端的群組。
* 是開放在[GitHub](https://github.com/aspnet/signalr)。
* 可擴充的。

用戶端與伺服器之間的連線是持續性的不同的 HTTP 連線。

## <a name="transports"></a>傳輸

透過一些技術來建置即時 SignalR 摘要 web 應用程式。 [WebSockets](https://tools.ietf.org/html/rfc7118)是最佳的傳輸，但這些無法使用時，就可以使用其他技術，如 Server-Sent 事件和長輪詢。 SignalR 會自動偵測並初始化適當伺服器和用戶端上支援的功能為基礎的傳輸。

## <a name="hubs"></a>集線器

SignalR 使用中樞用戶端和伺服器之間進行通訊。

可讓用戶端與伺服器彼此呼叫方法的高層級管線的中樞。 SignalR 處理分派跨電腦界限會自動允許用戶端在伺服器上呼叫方法，以輕鬆地為本機的方法，反之亦然。 中樞允許將強型別參數傳遞至方法，可讓模型繫結。 SignalR 提供兩個內建的中樞通訊協定： 文字通訊協定會根據 JSON 和二進位通訊協定為基礎[MessagePack](https://msgpack.org/)。  MessagePack 通常會建立比使用 JSON 較小的訊息。 舊的瀏覽器必須支援[XHR 層級 2](https://caniuse.com/#feat=xhr2)提供 MessagePack 通訊協定支援。

集線器呼叫用戶端程式碼使用作用中傳輸傳送訊息。 由於訊息包含名稱和用戶端方法的參數。 做為方法參數傳送的物件會還原序列化使用的設定通訊協定。 用戶端會嘗試比對用戶端程式碼中的方法名稱。 發現相符時，用戶端方法會使用執行的已還原序列化的參數資料。

## <a name="additional-resources"></a>其他資源

* [開始使用 SignalR 的 ASP.NET Core](xref:signalr/get-started)
* [支援的平台](xref:signalr/supported-platforms)
* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
