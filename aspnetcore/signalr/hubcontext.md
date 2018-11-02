---
title: SignalR HubContext
author: tdykstra
description: 了解如何使用 ASP.NET Core SignalR HubContext 服務來傳送通知給從中樞以外的用戶端。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: af125791a75a2dd68c236dd8c5b51eecff244ce4
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758150"
---
# <a name="send-messages-from-outside-a-hub"></a>傳送來自外部中樞訊息

藉由[Mikael 馬力](https://twitter.com/MikaelM_12)

SignalR 中樞會將訊息傳送至用戶端連線到 SignalR 伺服器的核心概念。 您也可從您的應用程式使用中的其他地方將訊息傳送`IHubContext`服務。 這篇文章說明如何存取 SignalR`IHubContext`來傳送通知給從中樞以外的用戶端。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>取得 IHubContext 的執行個體

在 ASP.NET Core SignalR 中，您可以存取的執行個體`IHubContext`透過相依性插入。 您可以插入的執行個體`IHubContext`至控制器、 中介軟體或其他的 DI 服務。 若要將訊息傳送至用戶端使用的執行個體。

> [!NOTE]
> 這不同於 ASP.NET 4.x SignalR 用來提供存取權的 GlobalHost `IHubContext`。 ASP.NET Core 已不再需要這個全域的單一相依性插入架構。

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>插入 IHubContext 控制器中的執行的個體

您可以插入的執行個體`IHubContext`到控制器，以將它加入您的建構函式：

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

現在，具有存取權的執行個體`IHubContext`，如同您之前參與中樞本身，您可以呼叫中樞方法。

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>取得 IHubContext 的執行個體，在中介軟體

存取`IHubContext`內中介軟體管線就像這樣：

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> 從呼叫中樞方法的時機，外部`Hub`類別，所以會沒有相關聯的引動過程的呼叫端。 因此，就沒有存取權`ConnectionId`， `Caller`，和`Others`屬性。

### <a name="inject-a-strongly-typed-hubcontext"></a>插入的強型別 HubContext

若要插入的強型別 HubContext，請確定您的中樞繼承自`Hub<T>`。 將使用其插入`IHubContext<THub, T>`介面而非`IHubContext<THub>`。

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public SampleDataController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(message);
    }
}
```

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
