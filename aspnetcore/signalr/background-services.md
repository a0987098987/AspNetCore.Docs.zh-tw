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
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/background-services
ms.openlocfilehash: bf5fff213b2cd7db0b3227922a8c5babba2fc904
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409081"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>SignalR背景服務中的主機 ASP.NET Core

依[Brady Gaster](https://twitter.com/bradygaster)

本文提供下列指引：

* SignalR使用以 ASP.NET Core 主控的背景工作進程來裝載中樞。
* 從 .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)中將訊息傳送至已連線的用戶端。

::: moniker range=">= aspnetcore-3.0"

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/samples/3.x) [（如何下載）](xref:index#how-to-download-a-sample)

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/samples/2.2) [（如何下載）](xref:index#how-to-download-a-sample)

::: moniker-end

## <a name="enable-signalr-in-startup"></a>SignalR在啟動時啟用

::: moniker range=">= aspnetcore-3.0"

SignalR在背景工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在 `Startup.ConfigureServices` 方法中，呼叫會 `services.AddSignalR` 將必要的服務新增至 ASP.NET Core 相依性插入（DI）層以支援 SignalR 。 在中 `Startup.Configure` ， `MapHub` 會在回呼中呼叫方法， `UseEndpoints` 以連接 ASP.NET Core 要求管線中的中樞端點。

[!code-csharp[Startup](background-service/samples/3.x/Server/Startup.cs?name=Startup)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

SignalR在背景工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在 `Startup.ConfigureServices` 方法中，呼叫會 `services.AddSignalR` 將必要的服務新增至 ASP.NET Core 相依性插入（DI）層以支援 SignalR 。 在中 `Startup.Configure` ， `UseSignalR` 會呼叫方法來連接 ASP.NET Core 要求管線中的中樞端點。

[!code-csharp[Startup](background-service/samples/2.2/Server/Startup.cs?name=Startup)]

::: moniker-end

在上述範例中，類別會實 `ClockHub` 作為 `Hub<T>` 建立強型別中樞的類別。 已 `ClockHub` 在類別中設定 `Startup` ，以回應端點上的要求 `/hubs/clock` 。

如需強型別中樞的詳細資訊，請參閱[在中使用中樞 SignalR 以進行 ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs)。

> [!NOTE]
> 這種功能並不限於[中樞 \<T> ](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。 任何繼承自[中樞](xref:Microsoft.AspNetCore.SignalR.Hub)的類別（例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)）都可以運作。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/Server/ClockHub.cs?name=ClockHub)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/Server/ClockHub.cs?name=ClockHub)]

::: moniker-end

強型別所使用的介面 `ClockHub` 是 `IClock` 介面。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/HubServiceInterfaces/IClock.cs?name=IClock)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/HubServiceInterfaces/IClock.cs?name=IClock)]

::: moniker-end

## <a name="call-a-signalr-hub-from-a-background-service"></a>SignalR從背景服務呼叫中樞

在啟動期間， `Worker` `BackgroundService` 會使用來啟用類別（a） `AddHostedService` 。

```csharp
services.AddHostedService<Worker>();
```

由於 SignalR 也會在階段中啟用 `Startup` ，因此每個中樞都會附加至 ASP.NET CORE 的 HTTP 要求管線中的個別端點，而每個中樞都會由 `IHubContext<T>` 伺服器上的表示。 使用 ASP.NET Core 的 DI 功能，由裝載層（例如 `BackgroundService` 類別、MVC 控制器類別或頁面模型）具現化的其他類別， Razor 可以藉由接受在架構中的實例來取得伺服器端中樞的參考 `IHubContext<ClockHub, IClock>` 。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/Server/Worker.cs?name=Worker)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/Server/Worker.cs?name=Worker)]

::: moniker-end

隨著在 `ExecuteAsync` 背景服務中反復呼叫方法，伺服器的目前日期和時間會使用傳送至已連線的用戶端 `ClockHub` 。

## <a name="react-to-signalr-events-with-background-services"></a>SignalR使用背景服務回應事件

就像使用適用于的 JavaScript 用戶端或 .NET 傳統型應用程式的單一頁面應用程式 SignalR ，可以使用來執行 <xref:signalr/dotnet-client> ， `BackgroundService` 或執行 `IHostedService` 也可以用來連接到 SignalR 中樞並回應事件。

`ClockHubClient`類別會同時執行 `IClock` 介面和 `IHostedService` 介面。 如此一來，就可以在持續執行期間啟用此功能 `Startup` ，並從伺服器回應中樞事件。

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

在初始化期間，會 `ClockHubClient` 建立的實例 `HubConnection` ，並啟用 `IClock.ShowTime` 方法做為中樞事件的處理常式 `ShowTime` 。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[The ClockHubClient constructor](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

在 `IHostedService.StartAsync` 執行 `HubConnection` 時，會以非同步方式啟動。

[!code-csharp[StartAsync method](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

在 `IHostedService.StopAsync` 方法期間， `HubConnection` 會以非同步方式處置。

[!code-csharp[StopAsync method](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[The ClockHubClient constructor](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

在 `IHostedService.StartAsync` 執行 `HubConnection` 時，會以非同步方式啟動。

[!code-csharp[StartAsync method](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

在 `IHostedService.StopAsync` 方法期間， `HubConnection` 會以非同步方式處置。

[!code-csharp[StopAsync method](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [強型別中樞](xref:signalr/hubs#strongly-typed-hubs)
