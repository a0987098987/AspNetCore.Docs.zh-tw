---
title: 用於 ASP.NET Core SignalR 中樞
author: bradygaster
description: 了解如何使用 ASP.NET Core signalr 的中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 244ddc40e647bfcc3ca8cda2797c51bc49174822
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320143"
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

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。 SignalR 處理的序列化和還原序列化複雜物件並在您的參數和傳回值的陣列。

> [!NOTE]
> 中樞是暫時性的：
>
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
| `Group` | 指定群組中的所有連接上呼叫方法  |
| `GroupExcept` | 在指定群組中，除非指定連線的所有連接上呼叫方法 |
| `Groups` | 多個群組的連接上呼叫方法  |
| `OthersInGroup` | 在群組的連線，不包括叫用中樞方法的用戶端上呼叫方法  |
| `User` | 與特定使用者相關聯的所有連接上呼叫方法 |
| `Users` | 與指定的使用者相關聯的所有連接上呼叫方法 |

每個屬性或方法，上述資料表中的傳回的物件`SendAsync`方法。 `SendAsync`方法可讓您提供的名稱和用戶端方法呼叫的參數。

## <a name="send-messages-to-clients"></a>將訊息傳送至用戶端

若要為特定用戶端的呼叫，使用的屬性`Clients`物件。 在下列範例中，有三個中樞的方法：

* `SendMessage` 將訊息傳送至所有已連線的用戶端，使用`Clients.All`。
* `SendMessageToCaller` 將訊息傳送至呼叫端，使用`Clients.Caller`。
* `SendMessageToGroups` 將訊息傳送至所有用戶端`SignalR Users`群組。

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>強型別的中樞

使用的一項缺點`SendAsync`是它依賴 magic 的字串，以指定要呼叫的用戶端方法。 這可讓程式碼開啟，如果方法名稱的拼字錯誤的執行階段錯誤或遺漏的用戶端。

使用替代`SendAsync`是強型別`Hub`使用<xref:Microsoft.AspNetCore.SignalR.Hub`1>。 在下列範例中，`ChatHub`用戶端方法已成呼叫登出擷取`IChatClient`。  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

此介面可用來重構上述`ChatHub`範例。

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

使用`Hub<IChatClient>`啟用編譯時間檢查的用戶端方法。 這可避免使用魔術字串，因為所造成的問題`Hub<T>`只能提供存取的介面中定義的方法。

使用強型別`Hub<T>`停用重新使用`SendAsync`。 介面上定義任何方法仍然可以定義會以非同步的。 事實上，每一種方法應傳回`Task`。 因為它是一個介面，請勿使用`async`關鍵字。 例如: 

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> `Async`尾碼不會移除與方法名稱。 除非您用戶端的方法以定義`.on('MyMethodAsync')`，您不應該使用`MyMethodAsync`做為名稱。

## <a name="change-the-name-of-a-hub-method"></a>變更中樞方法的名稱

根據預設，伺服器中樞的方法名稱會是.NET 方法的名稱。 不過，您可以使用[HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute)屬性來變更這個預設值，並且手動指定方法的名稱。 用戶端在叫用方法時，應該使用這個名稱，而不是.NET 方法名稱。

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>處理連接事件

提供 SignalR 中樞 API`OnConnectedAsync`和`OnDisconnectedAsync`來管理和追蹤連線的虛擬方法。 覆寫`OnConnectedAsync`虛擬方法，以執行動作，當用戶端連線至中樞，例如將它新增至群組。

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

覆寫`OnDisconnectedAsync`虛擬方法，用戶端中斷連線時執行動作。 如果用戶端刻意中斷連線 (藉由呼叫`connection.stop()`，例如)，則`exception`參數將會是`null`。 不過，如果用戶端已中斷連接錯誤 （例如網路失敗），因為`exception`參數會包含描述失敗的例外狀況。

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a>處理錯誤

在 hub 方法中擲回例外狀況會傳送至用戶端叫用方法。 在 JavaScript 用戶端`invoke`方法會傳回[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。 當用戶端會收到的錯誤處理常式附加至承諾使用`catch`，它已叫用，並傳遞為 JavaScript`Error`物件。

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

如果您的中樞擲回例外狀況，不關閉連線。 根據預設，SignalR 會傳回給用戶端的一般錯誤訊息。 例如: 

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

未預期的例外狀況通常會包含機密資訊，例如資料庫連線失敗時，觸發例外狀況中的資料庫伺服器的名稱。 SignalR 不公開預設這些詳細的錯誤訊息，基於安全性考量。 請參閱[安全性考量文章](xref:signalr/security#exceptions)詳細了解為何會隱藏例外狀況詳細資料。

如果您有例外狀況您*請勿*想要傳播至用戶端，您可以使用`HubException`類別。 如果您擲回`HubException`從您的中樞方法、 SignalR**將**整個訊息傳送至用戶端，未經修改的狀態。

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> 只會傳送 SignalR`Message`例外狀況至用戶端的屬性。 堆疊追蹤和例外狀況的其他屬性無法使用用戶端。

## <a name="related-resources"></a>相關資源

* [ASP.NET Core SignalR 簡介](xref:signalr/introduction)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
