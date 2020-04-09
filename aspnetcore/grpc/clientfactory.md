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
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="b3d71-103">gRPC 客戶端工廠整合在 .NET 核心</span><span class="sxs-lookup"><span data-stu-id="b3d71-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="b3d71-104">gRPC`HttpClientFactory`整合提供建立 gRPC 用戶端的集中式方法。</span><span class="sxs-lookup"><span data-stu-id="b3d71-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="b3d71-105">它可以用作[配置獨立 gRPC 客戶端實例的](xref:grpc/client)替代方法。</span><span class="sxs-lookup"><span data-stu-id="b3d71-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="b3d71-106">工廠整合在[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet 套件中提供。</span><span class="sxs-lookup"><span data-stu-id="b3d71-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="b3d71-107">工廠提供以下優勢:</span><span class="sxs-lookup"><span data-stu-id="b3d71-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="b3d71-108">提供設定邏輯 gRPC 客戶端實體的中心位置</span><span class="sxs-lookup"><span data-stu-id="b3d71-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="b3d71-109">管理基礎的存留期`HttpClientMessageHandler`</span><span class="sxs-lookup"><span data-stu-id="b3d71-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="b3d71-110">在ASP.NET核心 gRPC 服務中自動傳播截止時間並取消</span><span class="sxs-lookup"><span data-stu-id="b3d71-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="b3d71-111">註冊 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="b3d71-111">Register gRPC clients</span></span>

<span data-ttu-id="b3d71-112">要註冊 gRPC 客戶`AddGrpcClient`端`Startup.ConfigureServices`,可以在 中 使用泛型擴充方法,指定 gRPC 鍵入的用戶端類和服務位址:</span><span class="sxs-lookup"><span data-stu-id="b3d71-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

<span data-ttu-id="b3d71-113">gRPC 客戶端類型註冊為臨時依賴項注入 (DI)。</span><span class="sxs-lookup"><span data-stu-id="b3d71-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="b3d71-114">用戶端現在可以直接注入和使用 DI 創建的類型。</span><span class="sxs-lookup"><span data-stu-id="b3d71-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="b3d71-115">ASP.NET核心 MVCSignalR控制器、集線器和 gRPC 服務是可自動注入 gRPC 用戶端的地方:</span><span class="sxs-lookup"><span data-stu-id="b3d71-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

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

## <a name="configure-httpclient"></a><span data-ttu-id="b3d71-116">設定 HTTPClient</span><span class="sxs-lookup"><span data-stu-id="b3d71-116">Configure HttpClient</span></span>

<span data-ttu-id="b3d71-117">`HttpClientFactory`創建`HttpClient`gRPC 用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="b3d71-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="b3d71-118">標準`HttpClientFactory`方法可用於新增傳出要求中間元件或`HttpClientHandler`設定的基礎`HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="b3d71-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

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

<span data-ttu-id="b3d71-119">有關詳細資訊,請參閱使用[IHTTPClientFactory 發出 HTTP 請求](xref:fundamentals/http-requests)。</span><span class="sxs-lookup"><span data-stu-id="b3d71-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="b3d71-120">設定通道與攔截器</span><span class="sxs-lookup"><span data-stu-id="b3d71-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="b3d71-121">gRPC 特定方法可用於:</span><span class="sxs-lookup"><span data-stu-id="b3d71-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="b3d71-122">配置 gRPC 客戶端的基礎通道。</span><span class="sxs-lookup"><span data-stu-id="b3d71-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="b3d71-123">添加`Interceptor`用戶端在進行 gRPC 調用時將使用的實例。</span><span class="sxs-lookup"><span data-stu-id="b3d71-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

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

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="b3d71-124">截止日期和取消傳播</span><span class="sxs-lookup"><span data-stu-id="b3d71-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="b3d71-125">工廠在 gRPC 服務中創建的 gRPC`EnableCallContextPropagation()`用戶端可以配置為 自動將截止時間通知和取消權杖傳播到子調用。</span><span class="sxs-lookup"><span data-stu-id="b3d71-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="b3d71-126">擴`EnableCallContextPropagation()`充方法在[Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet 包中可用。</span><span class="sxs-lookup"><span data-stu-id="b3d71-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="b3d71-127">調用上下文傳播的工作原理是從當前 gRPC 請求上下文中讀取截止時間並取消權杖,並自動將它們傳播到 gRPC 客戶端發出的傳出調用。</span><span class="sxs-lookup"><span data-stu-id="b3d71-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="b3d71-128">調用上下文傳播是確保複雜嵌套 gRPC 方案始終傳播截止時間並取消的絕佳方式。</span><span class="sxs-lookup"><span data-stu-id="b3d71-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="b3d71-129">有關截止日期和 RPC 取消的詳細資訊,請參閱[RPC 生命週期](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle)。</span><span class="sxs-lookup"><span data-stu-id="b3d71-129">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b3d71-130">其他資源</span><span class="sxs-lookup"><span data-stu-id="b3d71-130">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
