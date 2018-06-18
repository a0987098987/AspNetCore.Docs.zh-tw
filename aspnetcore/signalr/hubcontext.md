---
title: SignalR HubContext
author: rachelappel
description: 了解如何使用 ASP.NET Core SignalR HubContext 服務來傳送通知給從中樞外的用戶端。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726072"
---
# <a name="send-messages-from-outside-a-hub"></a>從傳送訊息之外集線器

由[黃冠馬力](https://twitter.com/MikaelM_12)

SignalR 中樞會將訊息傳送至用戶端連線到 SignalR 伺服器的核心概念。 它也可從其他位置中的應用程式使用傳送郵件`IHubContext`服務。 這篇文章說明如何存取 SignalR`IHubContext`傳送通知給從中樞外的用戶端。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>取得執行的個體 `IHubContext`

在 ASP.NET Core SignalR，您可以存取的執行個體`IHubContext`透過相依性插入。 您可以將插入的執行個體`IHubContext`插入控制器、 中介軟體或其他 DI 服務。 將訊息傳送至用戶端使用的執行個體。

> [!NOTE]
> 這不同於 ASP.NET SignalR 用來提供存取權的 GlobalHost `IHubContext`。 ASP.NET Core 已不再需要此全域 singleton 的相依性插入架構。

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>插入的執行個體`IHubContext`控制器中

您可以將插入的執行個體`IHubContext`到控制器，以將它加入至您的建構函式：

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

現在，與執行個體的存取權`IHubContext`，如同您是在本身的中樞，您可以呼叫中樞方法。

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>取得的執行個體`IHubContext`中介軟體中

存取`IHubContext`內中介軟體管線就像這樣：

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> 當中樞方法從呼叫外部`Hub`類別，所以會叫用相關聯的任何呼叫端。 因此，就沒有存取權`ConnectionId`， `Caller`，和`Others`屬性。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:signalr/get-started)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
