---
title: SignalRHubCoNtext
author: bradygaster
description: 瞭解如何使用 ASP.NET Core SignalR HubCoNtext 服務，將通知從中樞外部傳送到用戶端。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/hubcontext
ms.openlocfilehash: 85f0f48dd6586b40b8db21eb4b59793069afe2c5
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405805"
---
# <a name="send-messages-from-outside-a-hub"></a>從中樞外部傳送訊息

依[Mikael Mengistu](https://twitter.com/MikaelM_12)

SignalR中樞是核心的抽象概念，可將訊息傳送給連接到伺服器的用戶端 SignalR 。 也可以使用服務，從應用程式中的其他位置傳送訊息 `IHubContext` 。 本文說明如何存取 SignalR `IHubContext` ，以從中樞外部傳送通知給用戶端。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>取得 IHubCoNtext 的實例

在 ASP.NET Core 中 SignalR ，您可以透過相依性插入來存取的實例 `IHubContext` 。 您可以將的實例插入 `IHubContext` 控制器、中介軟體或其他 DI 服務中。 使用實例可將訊息傳送至用戶端。

> [!NOTE]
> 這與 ASP.NET 4.x 不同 SignalR ，後者使用 GlobalHost 來提供的存取權 `IHubContext` 。 ASP.NET Core 具有相依性插入架構，因此不需要此全域 singleton。

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>在控制器中插入 IHubCoNtext 的實例

您可以藉由將實例加入至您的函式，將其插入至 `IHubContext` 控制器：

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

現在，有了實例的存取權 `IHubContext` ，您就可以呼叫中樞方法，就像您是在中樞本身一樣。

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>取得中介軟體中的 IHubCoNtext 實例

存取 `IHubContext` 中介軟體管線內的，如下所示：

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<ChatHub>>();
    //...
    
    if (next != null)
    {
        await next.Invoke();
    }
});
```

> [!NOTE]
> 從類別外部呼叫中樞方法時 `Hub` ，沒有與調用相關聯的呼叫端。 因此，沒有 `ConnectionId` 、和屬性的存取權 `Caller` `Others` 。

### <a name="get-an-instance-of-ihubcontext-from-ihost"></a>從 IHost 取得 IHubCoNtext 的實例

`IHubContext`從 web 主機存取，適用于整合 ASP.NET Core 以外的區域，例如，使用協力廠商相依性插入架構：

```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = CreateHostBuilder(args).Build();
            var hubContext = host.Services.GetService(typeof(IHubContext<ChatHub>));
            host.Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder => {
                    webBuilder.UseStartup<Startup>();
                });
    }
```

### <a name="inject-a-strongly-typed-hubcontext"></a>插入強型別 HubCoNtext

若要插入強型別 HubCoNtext，請確定您的中樞繼承自 `Hub<T>` 。 使用 `IHubContext<THub, T>` 介面（而非）插入它 `IHubContext<THub>` 。

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
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
