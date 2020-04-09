---
title: gRPC 客戶端工廠整合在 .NET 核心
author: jamesnk
description: 瞭解如何使用用戶端工廠創建 gRPC 用戶端。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667164"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>gRPC 客戶端工廠整合在 .NET 核心

gRPC`HttpClientFactory`整合提供建立 gRPC 用戶端的集中式方法。 它可以用作[配置獨立 gRPC 客戶端實例的](xref:grpc/client)替代方法。 工廠整合在[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet 套件中提供。

工廠提供以下優勢:

* 提供設定邏輯 gRPC 客戶端實體的中心位置
* 管理基礎的存留期`HttpClientMessageHandler`
* 在ASP.NET核心 gRPC 服務中自動傳播截止時間並取消

## <a name="register-grpc-clients"></a>註冊 gRPC 用戶端

要註冊 gRPC 客戶`AddGrpcClient`端`Startup.ConfigureServices`,可以在 中 使用泛型擴充方法,指定 gRPC 鍵入的用戶端類和服務位址:

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

gRPC 客戶端類型註冊為臨時依賴項注入 (DI)。 用戶端現在可以直接注入和使用 DI 創建的類型。 ASP.NET核心 MVCSignalR控制器、集線器和 gRPC 服務是可自動注入 gRPC 用戶端的地方:

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a>設定 HTTPClient

`HttpClientFactory`創建`HttpClient`gRPC 用戶端使用。 標準`HttpClientFactory`方法可用於新增傳出要求中間元件或`HttpClientHandler`設定的基礎`HttpClient`:

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

有關詳細資訊,請參閱使用[IHTTPClientFactory 發出 HTTP 請求](xref:fundamentals/http-requests)。

## <a name="configure-channel-and-interceptors"></a>設定通道與攔截器

gRPC 特定方法可用於:

* 配置 gRPC 客戶端的基礎通道。
* 添加`Interceptor`用戶端在進行 gRPC 調用時將使用的實例。

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a>截止日期和取消傳播

工廠在 gRPC 服務中創建的 gRPC`EnableCallContextPropagation()`用戶端可以配置為 自動將截止時間通知和取消權杖傳播到子調用。 擴`EnableCallContextPropagation()`充方法在[Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet 包中可用。

調用上下文傳播的工作原理是從當前 gRPC 請求上下文中讀取截止時間並取消權杖,並自動將它們傳播到 gRPC 客戶端發出的傳出調用。 調用上下文傳播是確保複雜嵌套 gRPC 方案始終傳播截止時間並取消的絕佳方式。

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

有關截止日期和 RPC 取消的詳細資訊,請參閱[RPC 生命週期](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle)。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
