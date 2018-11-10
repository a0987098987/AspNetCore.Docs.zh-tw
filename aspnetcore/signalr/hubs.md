---
title: 用於 ASP.NET Core SignalR 中樞
author: tdykstra
description: 了解如何使用 ASP.NET Core signalr 的中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 27aedc5b2f2060d961070fbd1ff5304eaa3956d1
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225352"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>使用 ASP.NET Core SignalR 中樞

藉由[Rachel Appel](https://twitter.com/rachelappel)和[Kevin Griffin](https://twitter.com/1kevgriff)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>什麼是 SignalR 中樞

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

> [!NOTE]
> 中樞是暫時性的：
> * 不會將狀態儲存在中樞類別上的屬性。 每個中樞方法的呼叫會在新的中樞執行個體上執行。  
> * 使用`await`呼叫非同步方法，取決於中樞保持運作時。 比方說，這類方法`Clients.All.SendAsync(...)`如果在未呼叫可能會失敗`await`和中樞方法完成之後才`SendAsync`完成。

## <a name="the-context-object"></a>內容物件

`Hub`類別具有`Context`屬性，其中包含與連線相關資訊的下列屬性：

| 屬性 | 描述 |
| ------ | ----------- |
| `ConnectionId` | 取得連接，SignalR 所指派的唯一識別碼。 還有一個針對每個連線的連線識別碼。|
| `UserIdentifier` | 取得[使用者識別碼](xref:signalr/groups)。 根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。 |
| `User` | 取得`ClaimsPrincipal`與目前的使用者相關聯。 |
| `Items` | 取得索引鍵/值集合，可用來共用此連線的範圍內的資料。 資料可以儲存在這個集合中，它會保存連接到不同的中樞方法叫用。 |
| `Features` | 取得連接上的可用功能的集合。 現在，這個集合不需要在大部分情況下，因此它不尚未記載於詳細資料。 |
| `ConnectionAborted` | 取得`CancellationToken`連線中止時，可通知。 |

`Hub.Context` 也包含下列方法：

| 方法 | 描述 |
| ------ | ----------- |
| `GetHttpContext` | 傳回`HttpContext`進行連接，或`null`如果連接不是 HTTP 要求相關聯。 適用於 HTTP 連線，您可以使用這個方法，以取得資訊，例如 HTTP 標頭和查詢字串。 |
| `Abort` | 中止連接。 |

## <a name="the-clients-object"></a>用戶端物件

`Hub`類別具有`Clients`屬性，其中包含伺服器與用戶端之間通訊的下列屬性：

| 屬性 | 描述 |
| ------ | ----------- |
| `All` | 所有連線的用戶端上呼叫方法 |
| `Caller` | 叫用中樞方法的用戶端上呼叫方法 |
| `Others` | 所有連線的用戶端，除了叫用方法的用戶端上呼叫方法 |


`Hub.Clients` 也包含下列方法：

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

## <a name="strongly-typed-hubs"></a>強型別的中樞

使用的一項缺點`SendAsync`是它依賴 magic 的字串，以指定要呼叫的用戶端方法。 這可讓程式碼開啟，如果方法名稱的拼字錯誤的執行階段錯誤或遺漏的用戶端。

使用替代`SendAsync`是強型別`Hub`使用<xref:Microsoft.AspNetCore.SignalR.Hub`1>。 在下列範例中，`ChatHub`用戶端方法已成呼叫登出擷取`IChatClient`。  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

此介面可用來重構上述`ChatHub`範例。

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

使用`Hub<IChatClient>`啟用編譯時間檢查的用戶端方法。 這可避免使用魔術字串，因為所造成的問題`Hub<T>`只能提供存取的介面中定義的方法。

使用強型別`Hub<T>`停用重新使用`SendAsync`。

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
