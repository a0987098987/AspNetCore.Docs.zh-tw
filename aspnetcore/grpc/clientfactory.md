---
title: .NET Core 中的 gRPC 用戶端 factory 整合
author: jamesnk
description: 瞭解如何使用用戶端 factory 建立 gRPC 用戶端。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 05/26/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 00ec2b6268d27143972f9cb663be19a3ed14ebea
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408522"
---
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="db9f4-103">.NET Core 中的 gRPC 用戶端 factory 整合</span><span class="sxs-lookup"><span data-stu-id="db9f4-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="db9f4-104">的 gRPC 整合 `HttpClientFactory` 提供了建立 gRPC 用戶端的集中方式。</span><span class="sxs-lookup"><span data-stu-id="db9f4-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="db9f4-105">它可以用來做為設定[獨立 gRPC 用戶端實例](xref:grpc/client)的替代方案。</span><span class="sxs-lookup"><span data-stu-id="db9f4-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="db9f4-106">Factory 整合可在[Grpc ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet 套件中取得。</span><span class="sxs-lookup"><span data-stu-id="db9f4-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="db9f4-107">Factory 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="db9f4-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="db9f4-108">提供設定邏輯 gRPC 用戶端實例的集中位置</span><span class="sxs-lookup"><span data-stu-id="db9f4-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="db9f4-109">管理基礎的存留期`HttpClientMessageHandler`</span><span class="sxs-lookup"><span data-stu-id="db9f4-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="db9f4-110">在 ASP.NET Core gRPC 服務中自動傳播期限和取消</span><span class="sxs-lookup"><span data-stu-id="db9f4-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="db9f4-111">註冊 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="db9f4-111">Register gRPC clients</span></span>

<span data-ttu-id="db9f4-112">若要註冊 gRPC 用戶端， `AddGrpcClient` 可以在內使用泛型擴充方法 `Startup.ConfigureServices` ，並指定 gRPC 具類型的用戶端類別和服務位址：</span><span class="sxs-lookup"><span data-stu-id="db9f4-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

<span data-ttu-id="db9f4-113">GRPC 用戶端類型會使用相依性插入（DI）註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="db9f4-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="db9f4-114">現在可以在 DI 所建立的類型中直接插入和取用用戶端。</span><span class="sxs-lookup"><span data-stu-id="db9f4-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="db9f4-115">ASP.NET Core MVC 控制器、 SignalR 中樞和 gRPC 服務都是可自動插入 gRPC 用戶端的地方：</span><span class="sxs-lookup"><span data-stu-id="db9f4-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

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

## <a name="configure-httpclient"></a><span data-ttu-id="db9f4-116">設定 HttpClient</span><span class="sxs-lookup"><span data-stu-id="db9f4-116">Configure HttpClient</span></span>

<span data-ttu-id="db9f4-117">`HttpClientFactory`建立 `HttpClient` gRPC 用戶端所使用的。</span><span class="sxs-lookup"><span data-stu-id="db9f4-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="db9f4-118">標準 `HttpClientFactory` 方法可以用來新增外寄要求中介軟體，或設定的基礎 `HttpClientHandler` `HttpClient` ：</span><span class="sxs-lookup"><span data-stu-id="db9f4-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

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

<span data-ttu-id="db9f4-119">如需詳細資訊，請參閱[使用 IHttpClientFactory 提出 HTTP 要求](xref:fundamentals/http-requests)。</span><span class="sxs-lookup"><span data-stu-id="db9f4-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="db9f4-120">設定通道和攔截器</span><span class="sxs-lookup"><span data-stu-id="db9f4-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="db9f4-121">gRPC 特有的方法適用于：</span><span class="sxs-lookup"><span data-stu-id="db9f4-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="db9f4-122">設定 gRPC 用戶端的基礎通道。</span><span class="sxs-lookup"><span data-stu-id="db9f4-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="db9f4-123">新增 `Interceptor` 用戶端在進行 gRPC 呼叫時將使用的實例。</span><span class="sxs-lookup"><span data-stu-id="db9f4-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

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

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="db9f4-124">期限和取消傳播</span><span class="sxs-lookup"><span data-stu-id="db9f4-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="db9f4-125">在 gRPC 服務中，由處理站所建立的 gRPC 用戶端可以設定 `EnableCallContextPropagation()` 為，以自動將期限和取消權杖傳播至子呼叫。</span><span class="sxs-lookup"><span data-stu-id="db9f4-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="db9f4-126">`EnableCallContextPropagation()`擴充方法可在[Grpc. AspNetCore. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet 套件中取得。</span><span class="sxs-lookup"><span data-stu-id="db9f4-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="db9f4-127">呼叫內容傳播的運作方式是從目前的 gRPC 要求內容讀取期限和解除標記，並自動將它們傳播至 gRPC 用戶端所發出的撥出電話。</span><span class="sxs-lookup"><span data-stu-id="db9f4-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="db9f4-128">呼叫內容傳播是確保複雜的嵌套 gRPC 案例一律會傳播期限和取消的絕佳方式。</span><span class="sxs-lookup"><span data-stu-id="db9f4-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="db9f4-129">根據預設， `EnableCallContextPropagation` 如果用戶端是在 gRPC 呼叫的內容之外使用，就會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="db9f4-129">By default, `EnableCallContextPropagation` raises an error if the client is used outside the context of a gRPC call.</span></span> <span data-ttu-id="db9f4-130">此錯誤是設計來警示您沒有要傳播的呼叫內容。</span><span class="sxs-lookup"><span data-stu-id="db9f4-130">The error is designed to alert you that there isn't a call context to propagate.</span></span> <span data-ttu-id="db9f4-131">如果您想要在呼叫內容外使用用戶端，請在設定用戶端時，隱藏錯誤 `SuppressContextNotFoundErrors` ：</span><span class="sxs-lookup"><span data-stu-id="db9f4-131">If you want to use the client outside of a call context, suppress the error when the client is configured with `SuppressContextNotFoundErrors`:</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation(o => o.SuppressContextNotFoundErrors = true);
```

<span data-ttu-id="db9f4-132">如需有關期限和 RPC 取消的詳細資訊，請參閱[rpc 生命週期](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle)。</span><span class="sxs-lookup"><span data-stu-id="db9f4-132">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db9f4-133">其他資源</span><span class="sxs-lookup"><span data-stu-id="db9f4-133">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
