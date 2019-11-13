---
title: .NET Core 中的 gRPC 用戶端 factory 整合
author: jamesnk
description: 瞭解如何使用用戶端 factory 建立 gRPC 用戶端。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963687"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>.NET Core 中的 gRPC 用戶端 factory 整合

gRPC 與 `HttpClientFactory` 整合提供了建立 gRPC 用戶端的集中方式。 它可以用來做為設定[獨立 gRPC 用戶端實例](xref:grpc/client)的替代方案。 Factory 整合可在[Grpc ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet 套件中取得。

Factory 提供下列優點：

* 提供設定邏輯 gRPC 用戶端實例的集中位置
* 管理基礎 `HttpClientMessageHandler` 的存留期
* 在 ASP.NET Core gRPC 服務中自動傳播期限和取消

## <a name="register-grpc-clients"></a>註冊 gRPC 用戶端

若要註冊 gRPC 用戶端，可以在 `Startup.ConfigureServices`內使用泛型 `AddGrpcClient` 擴充方法，並指定 gRPC 具類型的用戶端類別和服務位址：

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

GRPC 用戶端類型會使用相依性插入（DI）註冊為暫時性。 現在可以在 DI 所建立的類型中直接插入和取用用戶端。 ASP.NET Core MVC 控制器，SignalR 中樞和 gRPC 服務是可自動插入 gRPC 用戶端的位置：

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

## <a name="configure-httpclient"></a>設定 HttpClient

`HttpClientFactory` 會建立 gRPC 用戶端所使用的 `HttpClient`。 標準 `HttpClientFactory` 方法可以用來新增連出要求中介軟體或設定 `HttpClient`的基礎 `HttpClientHandler`：

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

如需詳細資訊，請參閱[使用 IHttpClientFactory 提出 HTTP 要求](xref:fundamentals/http-requests)。

## <a name="configure-channel-and-interceptors"></a>設定通道和攔截器

gRPC 特有的方法適用于：

* 設定 gRPC 用戶端的基礎通道。
* 新增用戶端在進行 gRPC 呼叫時將使用的 `Interceptor` 實例。

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

## <a name="deadline-and-cancellation-propagation"></a>期限和取消傳播

在 gRPC 服務中，由 factory 建立的 gRPC 用戶端可以使用 `EnableCallContextPropagation()` 設定，以自動將期限和取消權杖傳播至子呼叫。 `EnableCallContextPropagation()` 的擴充方法可在[Grpc. AspNetCore. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet 套件中取得。

呼叫內容傳播的運作方式是從目前的 gRPC 要求內容讀取期限和解除標記，並自動將它們傳播至 gRPC 用戶端所發出的撥出電話。 呼叫內容傳播是確保複雜的嵌套 gRPC 案例一律會傳播期限和取消的絕佳方式。

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

如需有關期限和 RPC 取消的詳細資訊，請參閱[rpc 生命週期](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle)。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
