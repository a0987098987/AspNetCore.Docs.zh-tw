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
# <a name="host-aspnet-core-signalr-in-background-services"></a>在背景服務中的主應用程式 ASP.NET Core SignalR

藉由[Brady Gaster](https://twitter.com/bradygaster)

本文提供指導方針：

* 裝載使用背景工作者處理序裝載 ASP.NET Core SignalR 中樞。
* 將訊息傳送到連線從.NET Core 中的用戶端[BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>在啟動期間接通 SignalR

背景工作者處理序的內容中裝載 ASP.NET Core SignalR 中樞等同於裝載的 ASP.NET Core web 應用程式中的中樞。 在 `Startup.ConfigureServices`方法，並呼叫`services.AddSignalR`加入以支援 SignalR 的 ASP.NET Core 相依性插入 (DI) 層中的所需的服務。 在`Startup.Configure`，則`UseSignalR`方法稱為連結是 ASP.NET Core 要求管線中的中樞端點。

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

在上述範例中，`ClockHub`類別會實作`Hub<T>`類別來建立強型別的中樞。 `ClockHub`中已設定`Startup`類別，以回應要求的端點`/hubs/clock`。

如需有關強型別中樞的詳細資訊，請參閱[使用 ASP.NET Core signalr 中樞](xref:signalr/hubs#strongly-typed-hubs)。

> [!NOTE]
> 這項功能並不限於[集線器\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。 任何類別都繼承自[集線器](xref:Microsoft.AspNetCore.SignalR.Hub)，例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)，也能運作。

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

強型別所使用的介面`ClockHub`是`IClock`介面。

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>從背景服務呼叫 SignalR Hub

在啟動期間，`Worker`類別， `BackgroundService`，您可以使用有線網路`AddHostedService`。

```csharp
services.AddHostedService<Worker>();
```

因為 SignalR 也妥當期間`Startup`階段時，請在每個中樞附加至 ASP.NET Core 的 HTTP 要求管線中的個別端點，每個集線器由`IHubContext<T>`伺服器上。 使用 ASP.NET Core DI 功能，來具現化裝載層，例如其他類別`BackgroundService`類別、 MVC 控制器類別或 Razor 頁面模型，可以接受的執行個體取得參考伺服器端中樞`IHubContext<ClockHub, IClock>`在建構期間。

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

作為`ExecuteAsync`背景服務中反覆地呼叫方法時，伺服器的目前日期和時間會傳送至連線的用戶端使用`ClockHub`。

## <a name="react-to-signalr-events-with-background-services"></a>回應 SignalR 事件，以背景服務

例如單一頁面應用程式使用 SignalR 或.NET 桌面應用程式可執行的 JavaScript 用戶端使用的 using <xref:signalr/dotnet-client>，則`BackgroundService`或`IHostedService`實作也可用來連線到 SignalR 中樞和回應事件。

`ClockHubClient`類別會實作`IClock`介面和`IHostedService`介面。 如此一來，它可以有線設定期間`Startup`持續執行，並回應來自伺服器的事件中樞。 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

在初始化期間，`ClockHubClient`建立的執行個體`HubConnection`，並將`IClock.ShowTime`做為處理常式的方法為中樞`ShowTime`事件。

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

在 `IHostedService.StartAsync`實作，`HubConnection`以非同步方式啟動。

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

期間`IHostedService.StopAsync`方法，`HubConnection`以非同步的方式處置。

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>其他資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [強型別的中樞](xref:signalr/hubs#strongly-typed-hubs)
