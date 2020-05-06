---
title: 背景服務SignalR中的主機 ASP.NET Core
author: bradygaster
description: 瞭解如何從 .NET Core BackgroundService SignalR類別將訊息傳送至用戶端。
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
ms.openlocfilehash: d5f1668d601f520939956985e46c62f3a5bdfcfa
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777289"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="e85ca-103">背景服務SignalR中的主機 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e85ca-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="e85ca-104">依[Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="e85ca-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="e85ca-105">本文提供下列指引：</span><span class="sxs-lookup"><span data-stu-id="e85ca-105">This article provides guidance for:</span></span>

* <span data-ttu-id="e85ca-106">使用SignalR以 ASP.NET Core 主控的背景工作進程來裝載中樞。</span><span class="sxs-lookup"><span data-stu-id="e85ca-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="e85ca-107">從 .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)中將訊息傳送至已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="e85ca-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="e85ca-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="e85ca-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="enable-signalr-in-startup"></a><span data-ttu-id="e85ca-109">在SignalR啟動時啟用</span><span class="sxs-lookup"><span data-stu-id="e85ca-109">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e85ca-110">在背景SignalR工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。</span><span class="sxs-lookup"><span data-stu-id="e85ca-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="e85ca-111">在`Startup.ConfigureServices`方法中，呼叫`services.AddSignalR`會將必要的服務新增至 ASP.NET Core 相依性插入（DI）層SignalR以支援。</span><span class="sxs-lookup"><span data-stu-id="e85ca-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="e85ca-112">在`Startup.Configure`中， `MapHub`會在`UseEndpoints`回呼中呼叫方法，以連接 ASP.NET Core 要求管線中的中樞端點。</span><span class="sxs-lookup"><span data-stu-id="e85ca-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="e85ca-113">在背景SignalR工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。</span><span class="sxs-lookup"><span data-stu-id="e85ca-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="e85ca-114">在`Startup.ConfigureServices`方法中，呼叫`services.AddSignalR`會將必要的服務新增至 ASP.NET Core 相依性插入（DI）層SignalR以支援。</span><span class="sxs-lookup"><span data-stu-id="e85ca-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="e85ca-115">在`Startup.Configure`中， `UseSignalR`會呼叫方法來連接 ASP.NET Core 要求管線中的中樞端點。</span><span class="sxs-lookup"><span data-stu-id="e85ca-115">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="e85ca-116">在上述範例中， `ClockHub`類別會實作為`Hub<T>`建立強型別中樞的類別。</span><span class="sxs-lookup"><span data-stu-id="e85ca-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="e85ca-117">`ClockHub`已在`Startup`類別中設定，以回應端點`/hubs/clock`上的要求。</span><span class="sxs-lookup"><span data-stu-id="e85ca-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="e85ca-118">如需強型別中樞的詳細資訊，請參閱[在中SignalR使用中樞以進行 ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs)。</span><span class="sxs-lookup"><span data-stu-id="e85ca-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="e85ca-119">此功能不限於[\<中樞 t>](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。</span><span class="sxs-lookup"><span data-stu-id="e85ca-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="e85ca-120">任何繼承自[中樞](xref:Microsoft.AspNetCore.SignalR.Hub)的類別（例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)）也都可以使用。</span><span class="sxs-lookup"><span data-stu-id="e85ca-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="e85ca-121">強型別所使用的介面`ClockHub`是`IClock`介面。</span><span class="sxs-lookup"><span data-stu-id="e85ca-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="e85ca-122">從背景SignalR服務呼叫中樞</span><span class="sxs-lookup"><span data-stu-id="e85ca-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="e85ca-123">在啟動期間， `Worker`會使用來`BackgroundService` `AddHostedService`啟用類別（a）。</span><span class="sxs-lookup"><span data-stu-id="e85ca-123">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="e85ca-124">由於SignalR也會在`Startup`階段中啟用，因此每個中樞都會附加至 ASP.NET Core 的 HTTP 要求管線中的個別端點，而每個中樞都會由伺服器`IHubContext<T>`上的表示。</span><span class="sxs-lookup"><span data-stu-id="e85ca-124">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="e85ca-125">使用 ASP.NET Core 的 DI 功能，由裝載層（例如`BackgroundService`類別、MVC 控制器類別或Razor頁面模型）具現化的其他類別，可以藉由接受在架構中的`IHubContext<ClockHub, IClock>`實例來取得伺服器端中樞的參考。</span><span class="sxs-lookup"><span data-stu-id="e85ca-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="e85ca-126">隨著在`ExecuteAsync`背景服務中反復呼叫方法，伺服器的目前日期和時間會使用傳送至已連線的用戶端`ClockHub`。</span><span class="sxs-lookup"><span data-stu-id="e85ca-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="e85ca-127">使用背景SignalR服務回應事件</span><span class="sxs-lookup"><span data-stu-id="e85ca-127">React to SignalR events with background services</span></span>

<span data-ttu-id="e85ca-128">就像使用SignalR適用于的 JavaScript 用戶端或 .net 傳統型應用程式的單一頁面應用程式<xref:signalr/dotnet-client>，可以使用來`BackgroundService`執行`IHostedService` ，或執行也可以用來連接SignalR到中樞並回應事件。</span><span class="sxs-lookup"><span data-stu-id="e85ca-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="e85ca-129">`ClockHubClient`類別會同時執行`IClock`介面和`IHostedService`介面。</span><span class="sxs-lookup"><span data-stu-id="e85ca-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="e85ca-130">如此一來，就可以在`Startup`持續執行期間啟用此功能，並從伺服器回應中樞事件。</span><span class="sxs-lookup"><span data-stu-id="e85ca-130">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="e85ca-131">在初始化期間， `ClockHubClient`會建立的實例`HubConnection` ，並啟用`IClock.ShowTime`方法做為中樞`ShowTime`事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="e85ca-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="e85ca-132">在`IHostedService.StartAsync`執行`HubConnection`時，會以非同步方式啟動。</span><span class="sxs-lookup"><span data-stu-id="e85ca-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="e85ca-133">在`IHostedService.StopAsync`方法期間， `HubConnection`會以非同步方式處置。</span><span class="sxs-lookup"><span data-stu-id="e85ca-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="e85ca-134">其他資源</span><span class="sxs-lookup"><span data-stu-id="e85ca-134">Additional resources</span></span>

* [<span data-ttu-id="e85ca-135">開始使用</span><span class="sxs-lookup"><span data-stu-id="e85ca-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e85ca-136">中樞</span><span class="sxs-lookup"><span data-stu-id="e85ca-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e85ca-137">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="e85ca-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="e85ca-138">強型別中樞</span><span class="sxs-lookup"><span data-stu-id="e85ca-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
