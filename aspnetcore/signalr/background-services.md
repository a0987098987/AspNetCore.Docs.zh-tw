---
title: 背景服務中的主機 ASP.NET Core SignalR
author: bradygaster
description: 瞭解如何從 .NET Core BackgroundService 類別將訊息傳送至 SignalR 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773943"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>背景服務中的主機 ASP.NET Core SignalR

依[Brady Gaster](https://twitter.com/bradygaster)

本文提供下列指引：

* 使用以 ASP.NET Core 主控的背景工作進程來裝載 SignalR 中樞。
* 從 .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)中將訊息傳送至已連線的用戶端。

[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/)[（如何下載）](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>在啟動期間連線到 SignalR

::: moniker range=">= aspnetcore-3.0"

在背景工作進程的內容中裝載 ASP.NET Core SignalR 集線器，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在方法中，呼叫`services.AddSignalR`會將必要的服務加入至 ASP.NET Core 相依性插入（DI）層，以支援 SignalR。 `Startup.ConfigureServices` 在`Startup.Configure`中，會`UseEndpoints`在回呼中呼叫方法，以在ASP.NETCore要求管線中連接中樞端點。`MapHub`

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

在背景工作進程的內容中裝載 ASP.NET Core SignalR 集線器，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在方法中，呼叫`services.AddSignalR`會將必要的服務加入至 ASP.NET Core 相依性插入（DI）層，以支援 SignalR。 `Startup.ConfigureServices` 在`Startup.Configure`中`UseSignalR` ，會呼叫方法來連接 ASP.NET Core 要求管線中的中樞端點。

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

在上述範例中， `ClockHub`類別會實作為建立強型別中樞的`Hub<T>`類別。 已在類別中設定，以回應端點`/hubs/clock`上的要求。 `Startup` `ClockHub`

如需強型別中樞的詳細資訊，請參閱[在 SignalR 中使用中樞以進行 ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs)。

> [!NOTE]
> 此功能不限於[\<中樞 t >](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。 任何繼承自[中樞](xref:Microsoft.AspNetCore.SignalR.Hub)的類別（例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)）也都可以使用。

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

強`ClockHub`型別所使用的介面`IClock`是介面。

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>從背景服務呼叫 SignalR 中樞

在啟動期間， `Worker`類別（a `BackgroundService`）會使用`AddHostedService`進行有線。

```csharp
services.AddHostedService<Worker>();
```

由於 SignalR 也會在`Startup`階段期間進行連線，其中每個中樞都會附加至 ASP.NET Core 的 HTTP 要求管線中的個別端點，而每個中樞都會由伺服器`IHubContext<T>`上的表示。 使用 ASP.NET Core 的 DI 功能，由裝載層（例如`BackgroundService`類別、MVC 控制器類別或 Razor 頁面模型）具現化的其他類別，可以藉由在結構中接受實例來取得伺服器端中樞的`IHubContext<ClockHub, IClock>`參考。

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

隨著在背景服務中反復呼叫`ClockHub`方法，伺服器的目前日期和時間會使用傳送至已連線的用戶端。`ExecuteAsync`

## <a name="react-to-signalr-events-with-background-services"></a>使用背景服務回應 SignalR 事件

就像使用適用于 SignalR 的 JavaScript 用戶端或 .net 傳統型應用程式的單一頁面應用程式<xref:signalr/dotnet-client> `BackgroundService` ，可以使用來執行`IHostedService` ，或執行也可以用來連接到 SignalR 中樞並回應事件。

類別會同時執行介面和`IHostedService`介面。 `IClock` `ClockHubClient` 如此一來，它就可以在`Startup`持續執行期間進行連接，並從伺服器回應中樞事件。

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

在初始化期間， `ClockHubClient`會建立的實例`HubConnection` ，並`IClock.ShowTime`將`ShowTime`方法作為中樞事件的處理常式。

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

`IHostedService.StartAsync`在執行`HubConnection`時，會以非同步方式啟動。

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

在方法期間`HubConnection` ，會以非同步方式處置。 `IHostedService.StopAsync`

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>其他資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [強型別中樞](xref:signalr/hubs#strongly-typed-hubs)
