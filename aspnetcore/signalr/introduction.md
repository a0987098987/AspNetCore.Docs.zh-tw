---
title: ASP.NET Core SignalR 簡介
author: bradygaster
description: 瞭解 ASP.NET Core SignalR 程式庫如何簡化將即時功能新增至應用程式的工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 635431abf9263c2dff261aea47e6f8324061763f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662915"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a>ASP.NET Core SignalR 簡介

## <a name="what-is-opno-locsignalr"></a>什麼是 SignalR？

ASP.NET Core SignalR 是一個開放原始碼程式庫，可簡化將即時 web 功能新增至應用程式的流程。 即時 web 功能可讓伺服器端程式碼立即將內容推送至用戶端。

適用于 SignalR的候選項目：

* 需要經常從伺服器取得更新的應用程式。 例如遊戲、社交網路、投票、拍賣、地圖和 GPS 應用程式。
* 儀表板和監視應用程式。 範例包括公司儀表板、即時銷售更新或旅行警示。
* 共同作業應用程式。 共同作業應用程式的範例包括白板應用程式和小組會議軟體。
* 需要通知的應用程式。 社交網路、電子郵件、交談、遊戲、旅行警示和其他使用通知的應用程式。

SignalR 提供用來建立伺服器對用戶端[遠端程序呼叫（RPC）](https://wikipedia.org/wiki/Remote_procedure_call)的 API。 Rpc 會從伺服器端 .NET Core 程式碼呼叫用戶端上的 JavaScript 函數。

以下是 ASP.NET Core SignalR 的一些功能：

* 自動處理連接管理。
* 同時將訊息傳送至所有已連線的用戶端。 例如，聊天室。
* 將訊息傳送至特定用戶端或用戶端群組。
* 調整以處理增加的流量。

來源裝載于[GitHub 上的SignalR 存放庫](https://github.com/dotnet/AspNetCore/tree/master/src/SignalR)中。

## <a name="transports"></a>傳輸

SignalR 支援下列用來處理即時通訊的技術（依正常回溯的順序）：

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* 伺服器傳送的事件
* 長時間輪詢

SignalR 會自動選擇伺服器和用戶端功能內的最佳傳輸方法。

## <a name="hubs"></a>集線器

SignalR 使用*中樞*在用戶端和伺服器之間進行通訊。

中樞是高階管線，可讓用戶端和伺服器彼此呼叫方法。 SignalR 會自動處理跨電腦界限的分派，讓用戶端可以在伺服器上呼叫方法，反之亦然。 您可以將強型別參數傳遞至方法，以啟用模型系結。 SignalR 提供兩個內建的中樞通訊協定：以 JSON 為基礎的文字通訊協定，以及以[MessagePack](https://msgpack.org/)為基礎的二進位通訊協定。  與 JSON 相比，MessagePack 通常會建立較小的訊息。 較舊的瀏覽器必須支援[XHR 層級 2](https://caniuse.com/#feat=xhr2) ，才能提供 MessagePack 通訊協定支援。

中樞會藉由傳送包含用戶端方法之名稱和參數的訊息來呼叫用戶端程式代碼。 以方法參數傳送的物件會使用設定的通訊協定進行還原序列化。 用戶端會嘗試將名稱與用戶端程式代碼中的方法比對。 當用戶端找到相符的時，它會呼叫方法，並將已還原序列化的參數資料傳遞給它。

## <a name="additional-resources"></a>其他資源

* [開始使用適用于 ASP.NET Core 的 SignalR](xref:tutorials/signalr)
* [支援的平台](xref:signalr/supported-platforms)
* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
