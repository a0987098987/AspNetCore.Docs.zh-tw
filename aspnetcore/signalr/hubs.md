---
title: 用於 ASP.NET Core SignalR 中樞
author: tdykstra
description: 了解如何使用 ASP.NET Core signalr 的中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095277"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>使用 ASP.NET Core signalr 中樞

藉由[Rachel Appel](https://twitter.com/rachelappel)和[Kevin Griffin](https://twitter.com/1kevgriff)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>什麼是 SignalR hub

SignalR 中樞 API 可讓您連線的用戶端上呼叫方法，從伺服器。 在伺服器程式碼中，您會定義用戶端所呼叫的方法。 在用戶端程式碼中，您可以定義會從伺服器呼叫的方法。 SignalR 會負責在幕後能夠即時的用戶端對伺服器和伺服器到用戶端通訊的所有項目。

## <a name="configure-signalr-hubs"></a>設定 SignalR 中樞

SignalR 中介軟體會需要某些服務，已藉由呼叫`services.AddSignalR`。

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

將 SignalR 功能加入至 ASP.NET Core 應用程式中，設定 SignalR 的路由，方法是呼叫`app.UseSignalR`在`Startup.Configure`方法。

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>建立和使用中樞

藉由宣告繼承自的類別建立中樞`Hub`，並為其新增公用方法。 用戶端可以呼叫方法定義為`public`。

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。 SignalR 處理的序列化和還原序列化複雜物件並在您的參數和傳回值的陣列。

## <a name="the-clients-object"></a>用戶端物件

每個執行個體`Hub`類別具有名為`Clients`，其中包含伺服器與用戶端之間通訊的下列成員：

| 屬性 | 描述 |
| ------ | ----------- |
| `All` | 所有連線的用戶端上呼叫方法 |
| `Caller` | 叫用中樞方法的用戶端上呼叫方法 |
| `Others` | 所有連線的用戶端，除了叫用方法的用戶端上呼叫方法 |


此外，`Hub.Clients`包含下列方法：

| 方法 | 描述 |
| ------ | ----------- |
| `AllExcept` | 所有連線的用戶端，除了指定的連接上呼叫方法 |
| `Client` | 特定連線的用戶端上呼叫方法 |
| `Clients` | 特定連線的用戶端上呼叫方法 |
| `Group` | 呼叫中指定的群組至所有連線的方法  |
| `GroupExcept` | 呼叫中指定的群組，除非指定連線到所有連線的方法 |
| `Groups` | 呼叫方法，以多個連接群組  |
| `OthersInGroup` | 呼叫方法，以連線，不包括叫用中樞方法的用戶端群組  |
| `User` | 呼叫方法，以與特定使用者相關聯的所有連線 |
| `Users` | 呼叫方法，以指定的使用者相關聯的所有連線 |

每個屬性或方法，上述資料表中的傳回的物件`SendAsync`方法。 `SendAsync`方法可讓您提供的名稱和用戶端方法呼叫的參數。

## <a name="send-messages-to-clients"></a>將訊息傳送至用戶端

若要為特定用戶端的呼叫，使用的屬性`Clients`物件。 在下列範例中，`SendMessageToCaller`方法示範如何將訊息傳送至叫用中樞方法的連接。 `SendMessageToGroups`方法會將訊息傳送至儲存在群組`List`名為`groups`。

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>處理連接事件

提供 SignalR 中樞 API`OnConnectedAsync`和`OnDisconnectedAsync`來管理和追蹤連線的虛擬方法。 覆寫`OnConnectedAsync`虛擬方法，以執行動作，當用戶端連線至中樞，例如將它新增至群組。

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>處理錯誤

在 hub 方法中擲回例外狀況會傳送至用戶端叫用方法。 在 JavaScript 用戶端`invoke`方法會傳回[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。 當用戶端會收到的錯誤處理常式附加至承諾使用`catch`，它已叫用，並傳遞為 JavaScript`Error`物件。

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>相關資源

* [ASP.NET Core SignalR 簡介](xref:signalr/introduction)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
