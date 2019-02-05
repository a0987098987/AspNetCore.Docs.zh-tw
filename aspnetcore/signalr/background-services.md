---
title: 在背景服務中的主應用程式 ASP.NET Core SignalR
author: bradygaster
description: 了解如何從.NET Core BackgroundService 類別時，將訊息傳送至 SignalR 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739666"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="ffbe6-103">在背景服務中的主應用程式 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ffbe6-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="ffbe6-104">藉由[Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="ffbe6-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="ffbe6-105">本文提供指導方針：</span><span class="sxs-lookup"><span data-stu-id="ffbe6-105">This article provides guidance for:</span></span>

* <span data-ttu-id="ffbe6-106">裝載使用背景工作者處理序裝載 ASP.NET Core SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="ffbe6-107">將訊息傳送到連線從.NET Core 中的用戶端[BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="ffbe6-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="ffbe6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="ffbe6-109">在啟動期間接通 SignalR</span><span class="sxs-lookup"><span data-stu-id="ffbe6-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="ffbe6-110">背景工作者處理序的內容中裝載 ASP.NET Core SignalR 中樞等同於裝載的 ASP.NET Core web 應用程式中的中樞。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="ffbe6-111">在 `Startup.ConfigureServices`方法，並呼叫`services.AddSignalR`加入以支援 SignalR 的 ASP.NET Core 相依性插入 (DI) 層中的所需的服務。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="ffbe6-112">在`Startup.Configure`，則`UseSignalR`方法稱為連結是 ASP.NET Core 要求管線中的中樞端點。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="ffbe6-113">在上述範例中，`ClockHub`類別會實作`Hub<T>`類別來建立強型別的中樞。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="ffbe6-114">`ClockHub`中已設定`Startup`類別，以回應要求的端點`/hubs/clock`。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="ffbe6-115">如需有關強型別中樞的詳細資訊，請參閱[使用 ASP.NET Core signalr 中樞](xref:signalr/hubs#strongly-typed-hubs)。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="ffbe6-116">這項功能並不限於[集線器\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="ffbe6-117">任何類別都繼承自[集線器](xref:Microsoft.AspNetCore.SignalR.Hub)，例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)，也能運作。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="ffbe6-118">強型別所使用的介面`ClockHub`是`IClock`介面。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="ffbe6-119">從背景服務呼叫 SignalR Hub</span><span class="sxs-lookup"><span data-stu-id="ffbe6-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="ffbe6-120">在啟動期間，`Worker`類別， `BackgroundService`，您可以使用有線網路`AddHostedService`。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="ffbe6-121">因為 SignalR 也妥當期間`Startup`階段時，請在每個中樞附加至 ASP.NET Core 的 HTTP 要求管線中的個別端點，每個集線器由`IHubContext<T>`伺服器上。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="ffbe6-122">使用 ASP.NET Core DI 功能，來具現化裝載層，例如其他類別`BackgroundService`類別、 MVC 控制器類別或 Razor 頁面模型，可以接受的執行個體取得參考伺服器端中樞`IHubContext<ClockHub, IClock>`在建構期間。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="ffbe6-123">作為`ExecuteAsync`背景服務中反覆地呼叫方法時，伺服器的目前日期和時間會傳送至連線的用戶端使用`ClockHub`。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="ffbe6-124">回應 SignalR 事件，以背景服務</span><span class="sxs-lookup"><span data-stu-id="ffbe6-124">React to SignalR events with background services</span></span>

<span data-ttu-id="ffbe6-125">例如單一頁面應用程式使用 SignalR 或.NET 桌面應用程式可執行的 JavaScript 用戶端使用的 using <xref:signalr/dotnet-client>，則`BackgroundService`或`IHostedService`實作也可用來連線到 SignalR 中樞和回應事件。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="ffbe6-126">`ClockHubClient`類別會實作`IClock`介面和`IHostedService`介面。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="ffbe6-127">如此一來，它可以有線設定期間`Startup`持續執行，並回應來自伺服器的事件中樞。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="ffbe6-128">在初始化期間，`ClockHubClient`建立的執行個體`HubConnection`，並將`IClock.ShowTime`做為處理常式的方法為中樞`ShowTime`事件。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="ffbe6-129">在 `IHostedService.StartAsync`實作，`HubConnection`以非同步方式啟動。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="ffbe6-130">期間`IHostedService.StopAsync`方法，`HubConnection`以非同步的方式處置。</span><span class="sxs-lookup"><span data-stu-id="ffbe6-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="ffbe6-131">其他資源</span><span class="sxs-lookup"><span data-stu-id="ffbe6-131">Additional resources</span></span>

* [<span data-ttu-id="ffbe6-132">開始使用</span><span class="sxs-lookup"><span data-stu-id="ffbe6-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ffbe6-133">中樞</span><span class="sxs-lookup"><span data-stu-id="ffbe6-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ffbe6-134">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="ffbe6-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="ffbe6-135">強型別的中樞</span><span class="sxs-lookup"><span data-stu-id="ffbe6-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
