---
title: SignalR 用戶端功能
author: bradygaster
description: 瞭解各種 ASP.NET Core SignalR 用戶端所支援的功能。
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925347"
---
# <a name="aspnet-core-signalr-client-features"></a>ASP.NET Core SignalR 用戶端功能

## <a name="feature-distribution"></a>功能散發

下表顯示提供即時支援之用戶端的功能與支援。 針對每個功能，會列出支援這項功能的*最低*版本。 如果沒有列出任何版本，則不支援此功能。

| 功能 | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Azure SignalR Service 支援 |1.0.0|1.0.0|1.0.0|
| [伺服器對用戶端串流](xref:signalr/streaming)          |1.0.0|1.0.0|1.0.0|
| [用戶端對伺服器串流](xref:signalr/streaming)          |3.0.0|3.0.0|3.0.0|
| 自動重新連接（[.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection)、 [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients)）          |3.0.0|3.0.0|❌|
| Websocket 傳輸 |1.0.0|1.0.0|1.0.0|
| 伺服器傳送的事件傳輸 |1.0.0|1.0.0|❌|
| 長輪詢傳輸 |1.0.0|1.0.0|3.0.0|
| JSON 中樞通訊協定 |1.0.0|1.0.0|1.0.0|
| MessagePack 中樞通訊協定 |1.0.0|1.0.0|❌|

在[我們的問題追蹤](https://github.com/aspnet/AspNetCore/issues/8711)程式中，會追蹤 JAVA 用戶端的自動重新連線支援。

## <a name="additional-resources"></a>其他資源

* [開始使用 SignalR for ASP.NET Core](xref:tutorials/signalr)
* [支援的平台](xref:signalr/supported-platforms)
* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
