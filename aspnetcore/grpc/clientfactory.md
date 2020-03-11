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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667164"
---
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="bf6ce-103">.NET Core 中的 gRPC 用戶端 factory 整合</span><span class="sxs-lookup"><span data-stu-id="bf6ce-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="bf6ce-104">gRPC 與 `HttpClientFactory` 整合提供了建立 gRPC 用戶端的集中方式。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="bf6ce-105">它可以用來做為設定[獨立 gRPC 用戶端實例](xref:grpc/client)的替代方案。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="bf6ce-106">Factory 整合可在[Grpc ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet 套件中取得。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="bf6ce-107">Factory 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="bf6ce-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="bf6ce-108">提供設定邏輯 gRPC 用戶端實例的集中位置</span><span class="sxs-lookup"><span data-stu-id="bf6ce-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="bf6ce-109">管理基礎 `HttpClientMessageHandler` 的存留期</span><span class="sxs-lookup"><span data-stu-id="bf6ce-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="bf6ce-110">在 ASP.NET Core gRPC 服務中自動傳播期限和取消</span><span class="sxs-lookup"><span data-stu-id="bf6ce-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="bf6ce-111">註冊 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="bf6ce-111">Register gRPC clients</span></span>

<span data-ttu-id="bf6ce-112">若要註冊 gRPC 用戶端，可以在 `Startup.ConfigureServices`內使用泛型 `AddGrpcClient` 擴充方法，並指定 gRPC 具類型的用戶端類別和服務位址：</span><span class="sxs-lookup"><span data-stu-id="bf6ce-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

<span data-ttu-id="bf6ce-113">GRPC 用戶端類型會使用相依性插入（DI）註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="bf6ce-114">現在可以在 DI 所建立的類型中直接插入和取用用戶端。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="bf6ce-115">ASP.NET Core MVC 控制器，SignalR 中樞和 gRPC 服務是可自動插入 gRPC 用戶端的位置：</span><span class="sxs-lookup"><span data-stu-id="bf6ce-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

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

## <a name="configure-httpclient"></a><span data-ttu-id="bf6ce-116">設定 HttpClient</span><span class="sxs-lookup"><span data-stu-id="bf6ce-116">Configure HttpClient</span></span>

<span data-ttu-id="bf6ce-117">`HttpClientFactory` 會建立 gRPC 用戶端所使用的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="bf6ce-118">標準 `HttpClientFactory` 方法可以用來新增連出要求中介軟體或設定 `HttpClient`的基礎 `HttpClientHandler`：</span><span class="sxs-lookup"><span data-stu-id="bf6ce-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

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

<span data-ttu-id="bf6ce-119">如需詳細資訊，請參閱[使用 IHttpClientFactory 提出 HTTP 要求](xref:fundamentals/http-requests)。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="bf6ce-120">設定通道和攔截器</span><span class="sxs-lookup"><span data-stu-id="bf6ce-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="bf6ce-121">gRPC 特有的方法適用于：</span><span class="sxs-lookup"><span data-stu-id="bf6ce-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="bf6ce-122">設定 gRPC 用戶端的基礎通道。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="bf6ce-123">新增用戶端在進行 gRPC 呼叫時將使用的 `Interceptor` 實例。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

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

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="bf6ce-124">期限和取消傳播</span><span class="sxs-lookup"><span data-stu-id="bf6ce-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="bf6ce-125">在 gRPC 服務中，由 factory 建立的 gRPC 用戶端可以使用 `EnableCallContextPropagation()` 設定，以自動將期限和取消權杖傳播至子呼叫。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="bf6ce-126">`EnableCallContextPropagation()` 的擴充方法可在[Grpc. AspNetCore. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet 套件中取得。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="bf6ce-127">呼叫內容傳播的運作方式是從目前的 gRPC 要求內容讀取期限和解除標記，並自動將它們傳播至 gRPC 用戶端所發出的撥出電話。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="bf6ce-128">呼叫內容傳播是確保複雜的嵌套 gRPC 案例一律會傳播期限和取消的絕佳方式。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="bf6ce-129">如需有關期限和 RPC 取消的詳細資訊，請參閱[rpc 生命週期](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle)。</span><span class="sxs-lookup"><span data-stu-id="bf6ce-129">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf6ce-130">其他資源</span><span class="sxs-lookup"><span data-stu-id="bf6ce-130">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
