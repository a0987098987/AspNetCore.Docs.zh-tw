---
title: SignalR背景服務中的主機 ASP.NET Core
author: bradygaster
description: 瞭解如何 SignalR 從 .Net Core BackgroundService 類別將訊息傳送至用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 7bc3b9535055e3fccf23ffa4638bd29676910348
ms.sourcegitcommit: e87dfa08fec0be1008249b1be678e5f79dcc5acb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/13/2020
ms.locfileid: "83382565"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="50d45-103">SignalR背景服務中的主機 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50d45-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="50d45-104">依[Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="50d45-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="50d45-105">本文提供下列指引：</span><span class="sxs-lookup"><span data-stu-id="50d45-105">This article provides guidance for:</span></span>

* <span data-ttu-id="50d45-106">SignalR使用以 ASP.NET Core 主控的背景工作進程來裝載中樞。</span><span class="sxs-lookup"><span data-stu-id="50d45-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="50d45-107">從 .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)中將訊息傳送至已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="50d45-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="50d45-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/samples/3.x) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="50d45-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/samples/3.x) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="50d45-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/samples/2.2) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="50d45-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/samples/2.2) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

::: moniker-end

## <a name="enable-signalr-in-startup"></a><span data-ttu-id="50d45-110">SignalR在啟動時啟用</span><span class="sxs-lookup"><span data-stu-id="50d45-110">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="50d45-111">SignalR在背景工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。</span><span class="sxs-lookup"><span data-stu-id="50d45-111">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="50d45-112">在 `Startup.ConfigureServices` 方法中，呼叫會 `services.AddSignalR` 將必要的服務新增至 ASP.NET Core 相依性插入（DI）層以支援 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="50d45-112">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="50d45-113">在中 `Startup.Configure` ， `MapHub` 會在回呼中呼叫方法， `UseEndpoints` 以連接 ASP.NET Core 要求管線中的中樞端點。</span><span class="sxs-lookup"><span data-stu-id="50d45-113">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/samples/3.x/Server/Startup.cs?name=Startup)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="50d45-114">SignalR在背景工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。</span><span class="sxs-lookup"><span data-stu-id="50d45-114">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="50d45-115">在 `Startup.ConfigureServices` 方法中，呼叫會 `services.AddSignalR` 將必要的服務新增至 ASP.NET Core 相依性插入（DI）層以支援 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="50d45-115">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="50d45-116">在中 `Startup.Configure` ， `UseSignalR` 會呼叫方法來連接 ASP.NET Core 要求管線中的中樞端點。</span><span class="sxs-lookup"><span data-stu-id="50d45-116">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/samples/2.2/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="50d45-117">在上述範例中，類別會實 `ClockHub` 作為 `Hub<T>` 建立強型別中樞的類別。</span><span class="sxs-lookup"><span data-stu-id="50d45-117">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="50d45-118">已 `ClockHub` 在類別中設定 `Startup` ，以回應端點上的要求 `/hubs/clock` 。</span><span class="sxs-lookup"><span data-stu-id="50d45-118">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="50d45-119">如需強型別中樞的詳細資訊，請參閱[在中使用中樞 SignalR 以進行 ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs)。</span><span class="sxs-lookup"><span data-stu-id="50d45-119">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="50d45-120">此功能不限於[中樞 \< t>](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。</span><span class="sxs-lookup"><span data-stu-id="50d45-120">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="50d45-121">任何繼承自[中樞](xref:Microsoft.AspNetCore.SignalR.Hub)的類別（例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)）都可以運作。</span><span class="sxs-lookup"><span data-stu-id="50d45-121">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), works.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/Server/ClockHub.cs?name=ClockHub)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/Server/ClockHub.cs?name=ClockHub)]

::: moniker-end

<span data-ttu-id="50d45-122">強型別所使用的介面 `ClockHub` 是 `IClock` 介面。</span><span class="sxs-lookup"><span data-stu-id="50d45-122">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/HubServiceInterfaces/IClock.cs?name=IClock)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/HubServiceInterfaces/IClock.cs?name=IClock)]

::: moniker-end

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="50d45-123">SignalR從背景服務呼叫中樞</span><span class="sxs-lookup"><span data-stu-id="50d45-123">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="50d45-124">在啟動期間， `Worker` `BackgroundService` 會使用來啟用類別（a） `AddHostedService` 。</span><span class="sxs-lookup"><span data-stu-id="50d45-124">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="50d45-125">由於 SignalR 也會在階段中啟用 `Startup` ，因此每個中樞都會附加至 ASP.NET CORE 的 HTTP 要求管線中的個別端點，而每個中樞都會由 `IHubContext<T>` 伺服器上的表示。</span><span class="sxs-lookup"><span data-stu-id="50d45-125">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="50d45-126">使用 ASP.NET Core 的 DI 功能，由裝載層（例如 `BackgroundService` 類別、MVC 控制器類別或頁面模型）具現化的其他類別， Razor 可以藉由接受在架構中的實例來取得伺服器端中樞的參考 `IHubContext<ClockHub, IClock>` 。</span><span class="sxs-lookup"><span data-stu-id="50d45-126">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/Server/Worker.cs?name=Worker)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/Server/Worker.cs?name=Worker)]

::: moniker-end

<span data-ttu-id="50d45-127">隨著在 `ExecuteAsync` 背景服務中反復呼叫方法，伺服器的目前日期和時間會使用傳送至已連線的用戶端 `ClockHub` 。</span><span class="sxs-lookup"><span data-stu-id="50d45-127">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="50d45-128">SignalR使用背景服務回應事件</span><span class="sxs-lookup"><span data-stu-id="50d45-128">React to SignalR events with background services</span></span>

<span data-ttu-id="50d45-129">就像使用適用于的 JavaScript 用戶端或 .NET 傳統型應用程式的單一頁面應用程式 SignalR ，可以使用來執行 <xref:signalr/dotnet-client> ， `BackgroundService` 或執行 `IHostedService` 也可以用來連接到 SignalR 中樞並回應事件。</span><span class="sxs-lookup"><span data-stu-id="50d45-129">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="50d45-130">`ClockHubClient`類別會同時執行 `IClock` 介面和 `IHostedService` 介面。</span><span class="sxs-lookup"><span data-stu-id="50d45-130">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="50d45-131">如此一來，就可以在持續執行期間啟用此功能 `Startup` ，並從伺服器回應中樞事件。</span><span class="sxs-lookup"><span data-stu-id="50d45-131">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="50d45-132">在初始化期間，會 `ClockHubClient` 建立的實例 `HubConnection` ，並啟用 `IClock.ShowTime` 方法做為中樞事件的處理常式 `ShowTime` 。</span><span class="sxs-lookup"><span data-stu-id="50d45-132">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[The ClockHubClient constructor](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="50d45-133">在 `IHostedService.StartAsync` 執行 `HubConnection` 時，會以非同步方式啟動。</span><span class="sxs-lookup"><span data-stu-id="50d45-133">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="50d45-134">在 `IHostedService.StopAsync` 方法期間， `HubConnection` 會以非同步方式處置。</span><span class="sxs-lookup"><span data-stu-id="50d45-134">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[The ClockHubClient constructor](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="50d45-135">在 `IHostedService.StartAsync` 執行 `HubConnection` 時，會以非同步方式啟動。</span><span class="sxs-lookup"><span data-stu-id="50d45-135">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="50d45-136">在 `IHostedService.StopAsync` 方法期間， `HubConnection` 會以非同步方式處置。</span><span class="sxs-lookup"><span data-stu-id="50d45-136">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="50d45-137">其他資源</span><span class="sxs-lookup"><span data-stu-id="50d45-137">Additional resources</span></span>

* [<span data-ttu-id="50d45-138">開始使用</span><span class="sxs-lookup"><span data-stu-id="50d45-138">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="50d45-139">中樞</span><span class="sxs-lookup"><span data-stu-id="50d45-139">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="50d45-140">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="50d45-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="50d45-141">強型別中樞</span><span class="sxs-lookup"><span data-stu-id="50d45-141">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
