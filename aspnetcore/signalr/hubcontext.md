---
title: SignalR HubContext
author: rachelappel
description: 了解如何使用 ASP.NET Core SignalR HubContext 服務來傳送通知給從中樞外的用戶端。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277757"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="06e74-103">從傳送訊息之外集線器</span><span class="sxs-lookup"><span data-stu-id="06e74-103">Send messages from outside a hub</span></span>

<span data-ttu-id="06e74-104">由[黃冠馬力](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="06e74-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="06e74-105">SignalR 中樞會將訊息傳送至用戶端連線到 SignalR 伺服器的核心概念。</span><span class="sxs-lookup"><span data-stu-id="06e74-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="06e74-106">它也可從其他位置中的應用程式使用傳送郵件`IHubContext`服務。</span><span class="sxs-lookup"><span data-stu-id="06e74-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="06e74-107">這篇文章說明如何存取 SignalR`IHubContext`傳送通知給從中樞外的用戶端。</span><span class="sxs-lookup"><span data-stu-id="06e74-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="06e74-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="06e74-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="06e74-109">取得執行的個體 `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="06e74-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="06e74-110">在 ASP.NET Core SignalR，您可以存取的執行個體`IHubContext`透過相依性插入。</span><span class="sxs-lookup"><span data-stu-id="06e74-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="06e74-111">您可以將插入的執行個體`IHubContext`插入控制器、 中介軟體或其他 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="06e74-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="06e74-112">將訊息傳送至用戶端使用的執行個體。</span><span class="sxs-lookup"><span data-stu-id="06e74-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="06e74-113">這不同於 ASP.NET SignalR 用來提供存取權的 GlobalHost `IHubContext`。</span><span class="sxs-lookup"><span data-stu-id="06e74-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="06e74-114">ASP.NET Core 已不再需要此全域 singleton 的相依性插入架構。</span><span class="sxs-lookup"><span data-stu-id="06e74-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="06e74-115">插入的執行個體`IHubContext`控制器中</span><span class="sxs-lookup"><span data-stu-id="06e74-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="06e74-116">您可以將插入的執行個體`IHubContext`到控制器，以將它加入至您的建構函式：</span><span class="sxs-lookup"><span data-stu-id="06e74-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="06e74-117">現在，與執行個體的存取權`IHubContext`，如同您是在本身的中樞，您可以呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="06e74-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="06e74-118">取得的執行個體`IHubContext`中介軟體中</span><span class="sxs-lookup"><span data-stu-id="06e74-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="06e74-119">存取`IHubContext`內中介軟體管線就像這樣：</span><span class="sxs-lookup"><span data-stu-id="06e74-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="06e74-120">當中樞方法從呼叫外部`Hub`類別，所以會叫用相關聯的任何呼叫端。</span><span class="sxs-lookup"><span data-stu-id="06e74-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="06e74-121">因此，就沒有存取權`ConnectionId`， `Caller`，和`Others`屬性。</span><span class="sxs-lookup"><span data-stu-id="06e74-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="06e74-122">相關資源</span><span class="sxs-lookup"><span data-stu-id="06e74-122">Related resources</span></span>

* [<span data-ttu-id="06e74-123">開始使用</span><span class="sxs-lookup"><span data-stu-id="06e74-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="06e74-124">中樞</span><span class="sxs-lookup"><span data-stu-id="06e74-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="06e74-125">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="06e74-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
