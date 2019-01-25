---
title: SignalR HubContext
author: bradygaster
description: 了解如何使用 ASP.NET Core SignalR HubContext 服務來傳送通知給從中樞以外的用戶端。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: 73cf2c9d30ed5e409a75827fdab1f22b20427884
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837932"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="820a7-103">傳送來自外部中樞訊息</span><span class="sxs-lookup"><span data-stu-id="820a7-103">Send messages from outside a hub</span></span>

<span data-ttu-id="820a7-104">藉由[Mikael 馬力](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="820a7-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="820a7-105">SignalR 中樞會將訊息傳送至用戶端連線到 SignalR 伺服器的核心概念。</span><span class="sxs-lookup"><span data-stu-id="820a7-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="820a7-106">您也可從您的應用程式使用中的其他地方將訊息傳送`IHubContext`服務。</span><span class="sxs-lookup"><span data-stu-id="820a7-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="820a7-107">這篇文章說明如何存取 SignalR`IHubContext`來傳送通知給從中樞以外的用戶端。</span><span class="sxs-lookup"><span data-stu-id="820a7-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="820a7-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="820a7-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="820a7-109">取得 IHubContext 的執行個體</span><span class="sxs-lookup"><span data-stu-id="820a7-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="820a7-110">在 ASP.NET Core SignalR 中，您可以存取的執行個體`IHubContext`透過相依性插入。</span><span class="sxs-lookup"><span data-stu-id="820a7-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="820a7-111">您可以插入的執行個體`IHubContext`至控制器、 中介軟體或其他的 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="820a7-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="820a7-112">若要將訊息傳送至用戶端使用的執行個體。</span><span class="sxs-lookup"><span data-stu-id="820a7-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="820a7-113">這不同於 ASP.NET 4.x SignalR 用來提供存取權的 GlobalHost `IHubContext`。</span><span class="sxs-lookup"><span data-stu-id="820a7-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="820a7-114">ASP.NET Core 已不再需要這個全域的單一相依性插入架構。</span><span class="sxs-lookup"><span data-stu-id="820a7-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="820a7-115">插入 IHubContext 控制器中的執行的個體</span><span class="sxs-lookup"><span data-stu-id="820a7-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="820a7-116">您可以插入的執行個體`IHubContext`到控制器，以將它加入您的建構函式：</span><span class="sxs-lookup"><span data-stu-id="820a7-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="820a7-117">現在，具有存取權的執行個體`IHubContext`，如同您之前參與中樞本身，您可以呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="820a7-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="820a7-118">取得 IHubContext 的執行個體，在中介軟體</span><span class="sxs-lookup"><span data-stu-id="820a7-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="820a7-119">存取`IHubContext`內中介軟體管線就像這樣：</span><span class="sxs-lookup"><span data-stu-id="820a7-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="820a7-120">從呼叫中樞方法的時機，外部`Hub`類別，所以會沒有相關聯的引動過程的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="820a7-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="820a7-121">因此，就沒有存取權`ConnectionId`， `Caller`，和`Others`屬性。</span><span class="sxs-lookup"><span data-stu-id="820a7-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

### <a name="inject-a-strongly-typed-hubcontext"></a><span data-ttu-id="820a7-122">插入的強型別 HubContext</span><span class="sxs-lookup"><span data-stu-id="820a7-122">Inject a strongly-typed HubContext</span></span>

<span data-ttu-id="820a7-123">若要插入的強型別 HubContext，請確定您的中樞繼承自`Hub<T>`。</span><span class="sxs-lookup"><span data-stu-id="820a7-123">To inject a strongly-typed HubContext, ensure your Hub inherits from `Hub<T>`.</span></span> <span data-ttu-id="820a7-124">將使用其插入`IHubContext<THub, T>`介面而非`IHubContext<THub>`。</span><span class="sxs-lookup"><span data-stu-id="820a7-124">Inject it using the `IHubContext<THub, T>` interface rather than `IHubContext<THub>`.</span></span>

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

## <a name="related-resources"></a><span data-ttu-id="820a7-125">相關資源</span><span class="sxs-lookup"><span data-stu-id="820a7-125">Related resources</span></span>

* [<span data-ttu-id="820a7-126">開始使用</span><span class="sxs-lookup"><span data-stu-id="820a7-126">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="820a7-127">中樞</span><span class="sxs-lookup"><span data-stu-id="820a7-127">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="820a7-128">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="820a7-128">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
