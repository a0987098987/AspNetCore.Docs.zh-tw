---
title: 在背景服務中 SignalR 主機 ASP.NET Core
author: bradygaster
description: 瞭解如何從 .NET Core BackgroundService 類別將訊息傳送至 SignalR 用戶端。
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 324592759af79d1229eb147fb4551e97c678ef64
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358674"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a>在背景服務中 SignalR 主機 ASP.NET Core

依[Brady Gaster](https://twitter.com/bradygaster)

本文提供下列指引：

* 使用以 ASP.NET Core 主控的背景工作進程來裝載 SignalR 中樞。
* 從 .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)中將訊息傳送至已連線的用戶端。

[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="enable-opno-locsignalr-in-startup"></a>在啟動時啟用 SignalR

::: moniker range=">= aspnetcore-3.0"

在背景工作進程的內容中裝載 ASP.NET Core SignalR 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在 `Startup.ConfigureServices` 方法中，呼叫 `services.AddSignalR` 會將必要的服務新增至 ASP.NET Core 相依性插入（DI）層，以支援 SignalR。 在 `Startup.Configure`中，會在 `UseEndpoints` 回呼中呼叫 `MapHub` 方法，以連接 ASP.NET Core 要求管線中的中樞端點。

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

在背景工作進程的內容中裝載 ASP.NET Core SignalR 中樞，等同于在 ASP.NET Core web 應用程式中裝載中樞。 在 `Startup.ConfigureServices` 方法中，呼叫 `services.AddSignalR` 會將必要的服務新增至 ASP.NET Core 相依性插入（DI）層，以支援 SignalR。 在 `Startup.Configure`中，會呼叫 `UseSignalR` 方法來連接 ASP.NET Core 要求管線中的中樞端點。

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

在上述範例中，`ClockHub` 類別會執行 `Hub<T>` 類別，以建立強型別中樞。 `ClockHub` 已在 `Startup` 類別中設定，以回應端點 `/hubs/clock`的要求。

如需強型別中樞的詳細資訊，請參閱[在適用于 ASP.NET Core 的 SignalR 中使用中樞](xref:signalr/hubs#strongly-typed-hubs)。

> [!NOTE]
> 這種功能並不限於[中樞\<t >](xref:Microsoft.AspNetCore.SignalR.Hub`1)類別。 任何繼承自[中樞](xref:Microsoft.AspNetCore.SignalR.Hub)的類別（例如[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)）也都可以使用。

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

強型別 `ClockHub` 所使用的介面是 `IClock` 介面。

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a>從背景服務呼叫 SignalR 中樞

在啟動期間，會使用 `AddHostedService`來啟用 `Worker` 類別，也就是 `BackgroundService`。

```csharp
services.AddHostedService<Worker>();
```

由於 SignalR 也會在 `Startup` 階段中啟用，其中每個中樞都會附加至 ASP.NET Core 的 HTTP 要求管線中的個別端點，而每個中樞都是由伺服器上的 `IHubContext<T>` 表示。 使用 ASP.NET Core 的 DI 功能，由裝載層具現化的其他類別（例如 `BackgroundService` 類別、MVC 控制器類別或 Razor 頁面模型），可以藉由接受在架構中的 `IHubContext<ClockHub, IClock>` 實例，來取得伺服器端中樞的參考。

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

隨著在背景服務中反復呼叫 `ExecuteAsync` 方法，伺服器的目前日期和時間會使用 `ClockHub`傳送至已連線的用戶端。

## <a name="react-to-opno-locsignalr-events-with-background-services"></a>使用背景服務回應 SignalR 事件

就像使用適用于 SignalR 的 JavaScript 用戶端或 .NET 傳統型應用程式的單一頁面應用程式，可以使用 <xref:signalr/dotnet-client>，`BackgroundService` 或 `IHostedService` 的執行也可以用來連接到 SignalR 中樞並回應事件。

`ClockHubClient` 類別會同時執行 `IClock` 介面和 `IHostedService` 介面。 如此一來，就可以在 `Startup` 連續執行並從伺服器回應中樞事件時，啟用此功能。

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

在初始化期間，`ClockHubClient` 會建立 `HubConnection` 的實例，並啟用 `IClock.ShowTime` 方法做為中樞 `ShowTime` 事件的處理常式。

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

在 `IHostedService.StartAsync` 的執行中，`HubConnection` 會以非同步方式啟動。

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

在 `IHostedService.StopAsync` 方法期間，會以非同步方式處置 `HubConnection`。

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>其他資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [強型別中樞](xref:signalr/hubs#strongly-typed-hubs)
