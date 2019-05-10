---
title: ASP.NET Core SignalR 簡介
author: bradygaster
description: 了解 ASP.NET Core SignalR 程式庫如何簡化將即時功能新增至應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 673efafce60dfa46cb99f9537fda2bca42bf9822
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892245"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core SignalR 簡介

## <a name="what-is-signalr"></a>SignalR 是什麼？

ASP.NET Core SignalR 是能簡化新增即時 web 功能的應用程式的開放原始碼程式庫。 即時 web 功能立即可讓用戶端推入內容的伺服器端程式碼。

Signalr 的良好候選項目：

* 需要來自伺服器的高頻率更新的應用程式。 範例包括遊戲、 社交網路、 投票、 拍賣、 對應和 GPS 應用程式。
* 儀表板和監視的應用程式。 範例包括公司儀表板，立即銷售的更新，或的旅遊警示。
* 共同作業應用程式。 白板應用程式和小組會議軟體是共同作業應用程式的範例。
* 需要通知的應用程式。 社交網路、 電子郵件、 聊天室、 遊戲、 的移動警示和許多其他應用程式使用通知。

SignalR 提供的 API 來建立伺服器到用戶端[遠端程序呼叫 (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)。 從伺服器端.NET Core 程式碼，Rpc 會呼叫 JavaScript 函式在用戶端上。

以下是適用於 ASP.NET Core SignalR 的一些功能：

* 會自動處理連接管理。
* 同時將訊息傳送至所有連線的用戶端。 比方說，聊天室。
* 將訊息傳送至特定的用戶端或群組的用戶端。
* 可調整來處理增加的流量。

來源裝載於[SignalR GitHub 上的存放庫](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR)。

## <a name="transports"></a>傳輸

SignalR 支援數種技術來處理即時通訊：

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* 伺服器傳送事件
* 長輪詢

SignalR 會自動選擇最佳的傳輸方法內的伺服器和用戶端功能。

## <a name="hubs"></a>中樞

使用 SignalR*中樞*用戶端和伺服器之間進行通訊。

中樞是可讓用戶端與伺服器彼此呼叫方法的高階管線。 SignalR 處理分派跨電腦界限會自動允許用戶端呼叫方法，在伺服器上，反之亦然。 您可以傳遞強型別參數至方法，可使用模型繫結。 SignalR 提供兩個內建中樞通訊協定： 文字通訊協定會根據 JSON 和二進位通訊協定為基礎[MessagePack](https://msgpack.org/)。  MessagePack 通常會建立較小的訊息，相較於 JSON。 必須支援舊版瀏覽器[XHR 層級 2](https://caniuse.com/#feat=xhr2)提供 MessagePack 通訊協定支援。

中樞會呼叫用戶端程式碼，藉由傳送訊息，它包含名稱和用戶端方法的參數。 做為方法參數傳送的物件會使用設定的通訊協定來還原序列化。 用戶端會嘗試比對用戶端程式碼中的方法名稱。 當用戶端找到相符項目時，它會呼叫方法，並已還原序列化的參數資料傳遞給它。

## <a name="additional-resources"></a>其他資源

* [開始使用 SignalR 的 ASP.NET Core](xref:tutorials/signalr)
* [支援的平台](xref:signalr/supported-platforms)
* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
