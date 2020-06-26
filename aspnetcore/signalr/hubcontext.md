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
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="11c76-103">從中樞外部傳送訊息</span><span class="sxs-lookup"><span data-stu-id="11c76-103">Send messages from outside a hub</span></span>

<span data-ttu-id="11c76-104">依[Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="11c76-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="11c76-105">SignalR中樞是核心的抽象概念，可將訊息傳送給連接到伺服器的用戶端 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="11c76-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="11c76-106">也可以使用服務，從應用程式中的其他位置傳送訊息 `IHubContext` 。</span><span class="sxs-lookup"><span data-stu-id="11c76-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="11c76-107">本文說明如何存取 SignalR `IHubContext` ，以從中樞外部傳送通知給用戶端。</span><span class="sxs-lookup"><span data-stu-id="11c76-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="11c76-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="11c76-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="11c76-109">取得 IHubCoNtext 的實例</span><span class="sxs-lookup"><span data-stu-id="11c76-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="11c76-110">在 ASP.NET Core 中 SignalR ，您可以透過相依性插入來存取的實例 `IHubContext` 。</span><span class="sxs-lookup"><span data-stu-id="11c76-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="11c76-111">您可以將的實例插入 `IHubContext` 控制器、中介軟體或其他 DI 服務中。</span><span class="sxs-lookup"><span data-stu-id="11c76-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="11c76-112">使用實例可將訊息傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="11c76-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="11c76-113">這與 ASP.NET 4.x 不同 SignalR ，後者使用 GlobalHost 來提供的存取權 `IHubContext` 。</span><span class="sxs-lookup"><span data-stu-id="11c76-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="11c76-114">ASP.NET Core 具有相依性插入架構，因此不需要此全域 singleton。</span><span class="sxs-lookup"><span data-stu-id="11c76-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="11c76-115">在控制器中插入 IHubCoNtext 的實例</span><span class="sxs-lookup"><span data-stu-id="11c76-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="11c76-116">您可以藉由將實例加入至您的函式，將其插入至 `IHubContext` 控制器：</span><span class="sxs-lookup"><span data-stu-id="11c76-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="11c76-117">現在，有了實例的存取權 `IHubContext` ，您就可以呼叫中樞方法，就像您是在中樞本身一樣。</span><span class="sxs-lookup"><span data-stu-id="11c76-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="11c76-118">取得中介軟體中的 IHubCoNtext 實例</span><span class="sxs-lookup"><span data-stu-id="11c76-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="11c76-119">存取 `IHubContext` 中介軟體管線內的，如下所示：</span><span class="sxs-lookup"><span data-stu-id="11c76-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="11c76-120">從類別外部呼叫中樞方法時 `Hub` ，沒有與調用相關聯的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="11c76-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="11c76-121">因此，沒有 `ConnectionId` 、和屬性的存取權 `Caller` `Others` 。</span><span class="sxs-lookup"><span data-stu-id="11c76-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

### <a name="get-an-instance-of-ihubcontext-from-ihost"></a><span data-ttu-id="11c76-122">從 IHost 取得 IHubCoNtext 的實例</span><span class="sxs-lookup"><span data-stu-id="11c76-122">Get an instance of IHubContext from IHost</span></span>

<span data-ttu-id="11c76-123">`IHubContext`從 web 主機存取，適用于整合 ASP.NET Core 以外的區域，例如，使用協力廠商相依性插入架構：</span><span class="sxs-lookup"><span data-stu-id="11c76-123">Accessing an `IHubContext` from the web host is useful for integrating with areas outside of ASP.NET Core, for example, using 3rd party dependency injection frameworks:</span></span>

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

### <a name="inject-a-strongly-typed-hubcontext"></a><span data-ttu-id="11c76-124">插入強型別 HubCoNtext</span><span class="sxs-lookup"><span data-stu-id="11c76-124">Inject a strongly-typed HubContext</span></span>

<span data-ttu-id="11c76-125">若要插入強型別 HubCoNtext，請確定您的中樞繼承自 `Hub<T>` 。</span><span class="sxs-lookup"><span data-stu-id="11c76-125">To inject a strongly-typed HubContext, ensure your Hub inherits from `Hub<T>`.</span></span> <span data-ttu-id="11c76-126">使用 `IHubContext<THub, T>` 介面（而非）插入它 `IHubContext<THub>` 。</span><span class="sxs-lookup"><span data-stu-id="11c76-126">Inject it using the `IHubContext<THub, T>` interface rather than `IHubContext<THub>`.</span></span>

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

## <a name="related-resources"></a><span data-ttu-id="11c76-127">相關資源</span><span class="sxs-lookup"><span data-stu-id="11c76-127">Related resources</span></span>

* [<span data-ttu-id="11c76-128">開始使用</span><span class="sxs-lookup"><span data-stu-id="11c76-128">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="11c76-129">中樞</span><span class="sxs-lookup"><span data-stu-id="11c76-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="11c76-130">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="11c76-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
