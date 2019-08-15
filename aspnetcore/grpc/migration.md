---
title: 將 gRPC 服務從 C-核心遷移至 ASP.NET Core
author: juntaoluo
description: 瞭解如何移動現有以 C 核心為基礎的 gRPC 應用程式, 以在 ASP.NET Core 堆疊上執行。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 39aa711a1a47cf11ec5b08903b4130c7caa1501c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022293"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="91202-103">將 gRPC 服務從 C-核心遷移至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91202-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="91202-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="91202-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="91202-105">由於基礎堆疊的執行, 並非所有功能在以[C 核心為基礎的 gRPC](https://grpc.io/blog/grpc-stacks)應用程式和 ASP.NET Core 型應用程式之間以相同的方式工作。</span><span class="sxs-lookup"><span data-stu-id="91202-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="91202-106">本檔將重點放在兩個堆疊之間進行遷移的主要差異。</span><span class="sxs-lookup"><span data-stu-id="91202-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="91202-107">gRPC 服務實生命週期</span><span class="sxs-lookup"><span data-stu-id="91202-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="91202-108">在 ASP.NET Core 堆疊中, 預設會以[限定範圍的存留期](xref:fundamentals/dependency-injection#service-lifetimes)來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="91202-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="91202-109">相反地, gRPC C-core 預設會系結至具有[單一存留期](xref:fundamentals/dependency-injection#service-lifetimes)的服務。</span><span class="sxs-lookup"><span data-stu-id="91202-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="91202-110">限定範圍存留期可讓服務執行解析具有限定範圍存留期的其他服務。</span><span class="sxs-lookup"><span data-stu-id="91202-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="91202-111">例如, 範圍存留期也可以透過函`DbContext`式插入, 從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="91202-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="91202-112">使用範圍存留期:</span><span class="sxs-lookup"><span data-stu-id="91202-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="91202-113">服務實的新實例會針對每個要求而建立。</span><span class="sxs-lookup"><span data-stu-id="91202-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="91202-114">您無法透過執行類型上的實例成員, 在要求之間共用狀態。</span><span class="sxs-lookup"><span data-stu-id="91202-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="91202-115">預期的情況是將共用狀態儲存在 DI 容器的單一服務中。</span><span class="sxs-lookup"><span data-stu-id="91202-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="91202-116">儲存的共用狀態會在 gRPC 服務執行的函式中解析。</span><span class="sxs-lookup"><span data-stu-id="91202-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="91202-117">如需服務存留期的詳細資訊<xref:fundamentals/dependency-injection#service-lifetimes>, 請參閱。</span><span class="sxs-lookup"><span data-stu-id="91202-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="91202-118">新增單一服務</span><span class="sxs-lookup"><span data-stu-id="91202-118">Add a singleton service</span></span>

<span data-ttu-id="91202-119">為了協助從 gRPC C 核心的執行轉換成 ASP.NET Core, 您可以將服務實的服務存留期從範圍變更為 singleton。</span><span class="sxs-lookup"><span data-stu-id="91202-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="91202-120">這牽涉到將服務實作為實例新增至 DI 容器:</span><span class="sxs-lookup"><span data-stu-id="91202-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="91202-121">不過, 具有單一存留期的服務執行, 無法再透過函式插入來解析已設定範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="91202-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="91202-122">設定 gRPC services 選項</span><span class="sxs-lookup"><span data-stu-id="91202-122">Configure gRPC services options</span></span>

<span data-ttu-id="91202-123">在以 C 為基礎的應用程式中, 當`grpc.max_receive_message_length`您`grpc.max_send_message_length`在[建立伺服器實例](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)時, 會使用`ChannelOption`和等設定進行設定。</span><span class="sxs-lookup"><span data-stu-id="91202-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="91202-124">在 ASP.NET Core 中, gRPC 會透過`GrpcServiceOptions`類型提供設定。</span><span class="sxs-lookup"><span data-stu-id="91202-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="91202-125">例如, 您可以透過`AddGrpc`下列方式設定 gRPC 服務的最大傳入訊息大小:</span><span class="sxs-lookup"><span data-stu-id="91202-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="91202-126">如需設定的詳細資訊, <xref:grpc/configuration>請參閱。</span><span class="sxs-lookup"><span data-stu-id="91202-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="91202-127">記錄</span><span class="sxs-lookup"><span data-stu-id="91202-127">Logging</span></span>

<span data-ttu-id="91202-128">`GrpcEnvironment`以 C 核心為基礎的應用程式依賴來[設定記錄器](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="91202-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="91202-129">ASP.NET Core 堆疊會透過[記錄 API](xref:fundamentals/logging/index)提供這種功能。</span><span class="sxs-lookup"><span data-stu-id="91202-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="91202-130">例如, 您可以透過函式插入, 將記錄器新增至 gRPC 服務:</span><span class="sxs-lookup"><span data-stu-id="91202-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="91202-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="91202-131">HTTPS</span></span>

<span data-ttu-id="91202-132">以 C 核心為基礎的應用程式會透過[伺服器埠屬性](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)來設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="91202-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="91202-133">在 ASP.NET Core 中設定伺服器時, 會使用類似的概念。</span><span class="sxs-lookup"><span data-stu-id="91202-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="91202-134">例如, Kestrel 會使用[端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定來取得這種功能。</span><span class="sxs-lookup"><span data-stu-id="91202-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="91202-135">攔截器和中介軟體</span><span class="sxs-lookup"><span data-stu-id="91202-135">Interceptors and Middleware</span></span>

<span data-ttu-id="91202-136">相較于以 C 核心為基礎的 gRPC 應用程式中的攔截器, ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="91202-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="91202-137">中介軟體和攔截器的概念相同, 兩者都是用來建立處理 gRPC 要求的管線。</span><span class="sxs-lookup"><span data-stu-id="91202-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="91202-138">兩者都允許在管線中的下一個元件之前或之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="91202-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="91202-139">不過, ASP.NET Core 中介軟體會在基礎 HTTP/2 訊息上運作, 而攔截器則是使用[ServerCallCoNtext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)在抽象概念的 gRPC 層上運作。</span><span class="sxs-lookup"><span data-stu-id="91202-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="91202-140">其他資源</span><span class="sxs-lookup"><span data-stu-id="91202-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
