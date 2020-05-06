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
# <a name="host-aspnet-core-signalr-in-background-services"></a>背景服務SignalR中的主機 ASP.NET Core

依[Brady Gaster](https://twitter.com/bradygaster)

本文提供下列指引：

* 使用SignalR以 ASP.NET Core 主控的背景工作進程來裝載中樞。
* 從 .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)中將訊息傳送至已連線的用戶端。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="enable-signalr-in-startup"></a>在SignalR啟動時啟用

::: moniker range=">= aspnetcore-3.0"

在背景SignalR工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在`Startup.ConfigureServices`方法中，呼叫`services.AddSignalR`會將必要的服務新增至 ASP.NET Core 相依性插入（DI）層SignalR以支援。 在`Startup.Configure`中， `MapHub`會在`UseEndpoints`回呼中呼叫方法，以連接 ASP.NET Core 要求管線中的中樞端點。

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

在背景SignalR工作進程的內容中裝載 ASP.NET Core 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在`Startup.ConfigureServices`方法中，呼叫`services.AddSignalR`會將必要的服務新增至 ASP.NET Core 相依性插入（DI）層SignalR以支援。 在`Startup.Configure`中， `UseSignalR`會呼叫方法來連接 ASP.NET Core 要求管線中的中樞端點。

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

在上述範例中， `ClockHub`類別會實作為`Hub<T>`建立強型別中樞的類別。 `ClockHub`已在`Startup`類別中設定，以回應端點`/hubs/clock`上的要求。

如需強型別中樞的詳細資訊，請參閱[在中SignalR使用中樞以進行 ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs)。

> [!NOTE]
> 此功能不限於[\<中樞 t>](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。 任何繼承自[中樞](xref:Microsoft.AspNetCore.SignalR.Hub)的類別（例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)）也都可以使用。

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

強型別所使用的介面`ClockHub`是`IClock`介面。

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>從背景SignalR服務呼叫中樞

在啟動期間， `Worker`會使用來`BackgroundService` `AddHostedService`啟用類別（a）。

```csharp
services.AddHostedService<Worker>();
```

由於SignalR也會在`Startup`階段中啟用，因此每個中樞都會附加至 ASP.NET Core 的 HTTP 要求管線中的個別端點，而每個中樞都會由伺服器`IHubContext<T>`上的表示。 使用 ASP.NET Core 的 DI 功能，由裝載層（例如`BackgroundService`類別、MVC 控制器類別或Razor頁面模型）具現化的其他類別，可以藉由接受在架構中的`IHubContext<ClockHub, IClock>`實例來取得伺服器端中樞的參考。

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

隨著在`ExecuteAsync`背景服務中反復呼叫方法，伺服器的目前日期和時間會使用傳送至已連線的用戶端`ClockHub`。

## <a name="react-to-signalr-events-with-background-services"></a>使用背景SignalR服務回應事件

就像使用SignalR適用于的 JavaScript 用戶端或 .net 傳統型應用程式的單一頁面應用程式<xref:signalr/dotnet-client>，可以使用來`BackgroundService`執行`IHostedService` ，或執行也可以用來連接SignalR到中樞並回應事件。

`ClockHubClient`類別會同時執行`IClock`介面和`IHostedService`介面。 如此一來，就可以在`Startup`持續執行期間啟用此功能，並從伺服器回應中樞事件。

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

在初始化期間， `ClockHubClient`會建立的實例`HubConnection` ，並啟用`IClock.ShowTime`方法做為中樞`ShowTime`事件的處理常式。

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

在`IHostedService.StartAsync`執行`HubConnection`時，會以非同步方式啟動。

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

在`IHostedService.StopAsync`方法期間， `HubConnection`會以非同步方式處置。

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>其他資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [強型別中樞](xref:signalr/hubs#strongly-typed-hubs)
