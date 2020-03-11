---
title: 在 ASP.NET Core SignalR 中使用中樞
author: bradygaster
description: 瞭解如何在 ASP.NET Core SignalR中使用中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/hubs
ms.openlocfilehash: 54ffd8614c1cec4cfeba0878e910ed25fc6ba7d2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662950"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>使用 SignalR 中的中樞進行 ASP.NET Core

By [Rachel Appel](https://twitter.com/rachelappel)和[古柯 Griffin](https://twitter.com/1kevgriff)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>什麼是 SignalR hub

SignalR 中樞 API 可讓您從伺服器呼叫已連線用戶端上的方法。 在伺服器程式碼中，您可以定義用戶端所呼叫的方法。 在用戶端程式代碼中，您可以定義從伺服器呼叫的方法。 SignalR 會處理幕後的所有內容，讓您能夠進行即時的用戶端對伺服器和伺服器對用戶端通訊。

## <a name="configure-signalr-hubs"></a>設定 SignalR 中樞

SignalR 中介軟體需要一些服務，透過呼叫 `services.AddSignalR`來設定。

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

::: moniker range=">= aspnetcore-3.0"

將 SignalR 功能新增至 ASP.NET Core 應用程式時，請在 `Startup.Configure` 方法的 `app.UseEndpoints` 回呼中呼叫 `endpoint.MapHub`，以安裝 SignalR 路由。

```csharp
app.UseRouting();
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/chathub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

將 SignalR 功能新增至 ASP.NET Core 應用程式時，請在 `Startup.Configure` 方法中呼叫 `app.UseSignalR`，以安裝 SignalR 路由。

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

::: moniker-end

## <a name="create-and-use-hubs"></a>建立和使用中樞

藉由宣告繼承自 `Hub`的類別來建立中樞，並在其中新增公用方法。 用戶端可以呼叫定義為 `public`的方法。

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

您可以指定傳回類型和參數，包括複雜類型和陣列，就像在任何C#方法中一樣。 SignalR 會在您的參數和傳回值中處理複雜物件和陣列的序列化和還原序列化。

> [!NOTE]
> 中樞為暫時性：
>
> * 不要在中樞類別的屬性中儲存狀態。 每個中樞方法呼叫都會在新的中樞實例上執行。
> * 呼叫相依于中樞保持運作的非同步方法時，請使用 `await`。 例如，如果在沒有 `await` 的情況下呼叫，且中樞方法在 `SendAsync` 完成之前完成，則方法（例如 `Clients.All.SendAsync(...)`）可能會失敗。

## <a name="the-context-object"></a>內容物件

`Hub` 類別具有 `Context` 屬性，其中包含下列具有連接相關資訊的屬性：

| 屬性 | 描述 |
| ------ | ----------- |
| `ConnectionId` | 取得連接的唯一識別碼，由 SignalR 指派。 每個連接都有一個連接識別碼。|
| `UserIdentifier` | 取得[使用者識別碼](xref:signalr/groups)。 根據預設，SignalR 會使用與連接相關聯的 `ClaimsPrincipal` 中的 `ClaimTypes.NameIdentifier` 做為使用者識別碼。 |
| `User` | 取得與目前使用者相關聯的 `ClaimsPrincipal`。 |
| `Items` | 取得索引鍵/值集合，可用來在此連接的範圍內共用資料。 資料可以儲存在此集合中，且會保存在不同中樞方法叫用的連接中。 |
| `Features` | 取得連接上可用的功能集合。 目前，在大部分的情況下不需要此集合，因此尚未詳細記載。 |
| `ConnectionAborted` | 取得當連接中止時，所通知的 `CancellationToken`。 |

`Hub.Context` 也包含下列方法：

| 方法 | 描述 |
| ------ | ----------- |
| `GetHttpContext` | 傳回連接的 `HttpContext`，如果連接未與 HTTP 要求相關聯，則為 `null`。 針對 HTTP 連線，您可以使用這個方法來取得資訊，例如 HTTP 標頭和查詢字串。 |
| `Abort` | 中止連線。 |

## <a name="the-clients-object"></a>用戶端物件

`Hub` 類別具有 `Clients` 屬性，其中包含伺服器和用戶端之間通訊的下列屬性：

| 屬性 | 描述 |
| ------ | ----------- |
| `All` | 在所有已連線的用戶端上呼叫方法 |
| `Caller` | 呼叫用戶端上叫用中樞方法的方法 |
| `Others` | 在所有已連線的用戶端上呼叫方法，但叫用方法的用戶端除外 |

`Hub.Clients` 也包含下列方法：

| 方法 | 描述 |
| ------ | ----------- |
| `AllExcept` | 在所有已連線的用戶端上呼叫方法，但指定的連接除外 |
| `Client` | 在特定的已連線用戶端上呼叫方法 |
| `Clients` | 在特定的已連線用戶端上呼叫方法 |
| `Group` | 在指定群組中的所有連接上呼叫方法  |
| `GroupExcept` | 在指定之群組中的所有連接上呼叫方法，但指定的連接除外 |
| `Groups` | 在多個連接群組上呼叫方法  |
| `OthersInGroup` | 在一組連接上呼叫方法，但不包括叫用中樞方法的用戶端  |
| `User` | 在與特定使用者相關聯的所有連接上呼叫方法 |
| `Users` | 在與指定使用者相關聯的所有連接上呼叫方法 |

上表中的每個屬性或方法都會傳回具有 `SendAsync` 方法的物件。 `SendAsync` 方法可讓您提供要呼叫之用戶端方法的名稱和參數。

## <a name="send-messages-to-clients"></a>將訊息傳送至用戶端

若要對特定用戶端進行呼叫，請使用 `Clients` 物件的屬性。 在下列範例中，有三個中樞方法：

* `SendMessage` 會使用 `Clients.All`將訊息傳送至所有已連線的用戶端。
* `SendMessageToCaller` 使用 `Clients.Caller`，將訊息傳回給呼叫者。
* `SendMessageToGroups` 會將訊息傳送至 `SignalR Users` 群組中的所有用戶端。

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>強型別中樞

使用 `SendAsync` 的缺點是，它依賴魔術字串來指定要呼叫的用戶端方法。 如果用戶端的方法名稱拼錯或遺失，這就會讓程式碼保持開啟，直到發生執行階段錯誤。

使用 `SendAsync` 的另一種方式，就是強型別 <xref:Microsoft.AspNetCore.SignalR.Hub%601>的 `Hub`。 在下列範例中，`ChatHub` 的用戶端方法已解壓縮至稱為 `IChatClient`的介面。

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

這個介面可以用來重構前面的 `ChatHub` 範例。

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

使用 `Hub<IChatClient>` 可啟用用戶端方法的編譯時間檢查。 這可防止使用魔術字串所造成的問題，因為 `Hub<T>` 只能提供介面中定義之方法的存取權。

使用強型別 `Hub<T>` 會停用使用 `SendAsync`的功能。 在介面上定義的任何方法仍可定義為非同步。 事實上，每個方法都應該傳回 `Task`。 由於它是介面，請不要使用 `async` 關鍵字。 例如：

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> 不會從方法名稱中移除 `Async` 尾碼。 除非您的用戶端方法是使用 `.on('MyMethodAsync')`定義的，否則您不應該使用 `MyMethodAsync` 做為名稱。

## <a name="change-the-name-of-a-hub-method"></a>變更中樞方法的名稱

根據預設，伺服器中樞方法名稱是 .NET 方法的名稱。 不過，您可以使用[HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute)屬性來變更此預設值，並手動指定方法的名稱。 叫用方法時，用戶端應該使用這個名稱，而不是 .NET 方法名稱。

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>處理連接的事件

SignalR 中樞 API 提供 `OnConnectedAsync` 和 `OnDisconnectedAsync` 的虛擬方法來管理和追蹤連接。 覆寫 `OnConnectedAsync` 虛擬方法，以在用戶端連線至中樞時執行動作，例如將其新增至群組。

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

覆寫 `OnDisconnectedAsync` 虛擬方法，以在用戶端中斷連線時執行動作。 如果用戶端刻意中斷連接（例如，藉由呼叫 `connection.stop()`），則 `exception` 參數將會 `null`。 不過，如果用戶端因為錯誤（例如網路故障）而中斷連線，則 `exception` 參數會包含描述失敗的例外狀況。

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

[!INCLUDE[](~/includes/connectionid-signalr.md)]

## <a name="handle-errors"></a>處理錯誤

在您的中樞方法中擲回的例外狀況會傳送至叫用方法的用戶端。 在 JavaScript 用戶端上，`invoke` 方法會傳回[JAVAscript 承諾](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。 當用戶端收到錯誤，並將處理常式附加至使用 `catch`的承諾時，它會被叫用並當做 JavaScript `Error` 物件傳遞。

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

如果您的中樞擲回例外狀況，則不會關閉連接。 根據預設，SignalR 會將一般錯誤訊息傳回給用戶端。 例如：

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

非預期的例外狀況通常包含敏感性資訊，例如資料庫連接失敗時所觸發的例外狀況中的資料庫伺服器名稱。 SignalR 預設不會公開這些詳細的錯誤訊息做為安全性措施。 如需隱藏例外狀況詳細資料的詳細資訊，請參閱[安全性考慮一文](xref:signalr/security#exceptions)。

如果您想要將例外狀況傳播至用戶端 *，您可以*使用 `HubException` 類別。 如果您從中樞方法擲回 `HubException`，SignalR**會將**整個訊息傳送至未修改的用戶端。

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR 只會將例外狀況的 `Message` 屬性傳送至用戶端。 例外狀況的堆疊追蹤和其他屬性無法供用戶端使用。

## <a name="related-resources"></a>相關資源

* [ASP.NET Core SignalR 簡介](xref:signalr/introduction)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
