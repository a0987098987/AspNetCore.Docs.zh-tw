---
title: 從 C 核心移轉的 gRPC 服務，ASP.NET core
author: juntaoluo
description: 了解如何移動現有的 C core 根據的 gRPC 應用程式來執行 ASP.NET Core 堆疊的頂端。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895235"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="f5030-103">從 C 核心移轉的 gRPC 服務，ASP.NET core</span><span class="sxs-lookup"><span data-stu-id="f5030-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="f5030-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="f5030-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="f5030-105">由於基礎堆疊實作，並非所有功能則是之間的相同方式都運作[C 核心為基礎的 gRPC](https://grpc.io/blog/grpc-stacks)應用程式和 ASP.NET Core 為基礎的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f5030-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="f5030-106">本文件特別說明移轉兩個堆疊之間的主要差異。</span><span class="sxs-lookup"><span data-stu-id="f5030-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="f5030-107">gRPC 服務實作的存留期</span><span class="sxs-lookup"><span data-stu-id="f5030-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="f5030-108">在 ASP.NET Core 堆疊，gRPC 服務，根據預設，會建立與[具範圍存留期](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="f5030-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="f5030-109">相較之下，gRPC C core 預設會將服務繫結[單一存留期](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="f5030-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="f5030-110">具範圍存留期可讓您用來解析已設定領域的存留期與其他服務的服務實作。</span><span class="sxs-lookup"><span data-stu-id="f5030-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="f5030-111">比方說，也可以解析已設定領域的存留期`DBContext`從 DI 容器透過建構函式插入。</span><span class="sxs-lookup"><span data-stu-id="f5030-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="f5030-112">使用具範圍存留期：</span><span class="sxs-lookup"><span data-stu-id="f5030-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="f5030-113">服務實作的新執行個體的建構針對每個要求。</span><span class="sxs-lookup"><span data-stu-id="f5030-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="f5030-114">您無法透過執行個體上的實作類型的成員要求間共用狀態。</span><span class="sxs-lookup"><span data-stu-id="f5030-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="f5030-115">預期是儲存在 DI 容器中的單一服務中的共用的狀態。</span><span class="sxs-lookup"><span data-stu-id="f5030-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="f5030-116">GRPC 服務實作的建構函式中，會解析預存的共用的狀態。</span><span class="sxs-lookup"><span data-stu-id="f5030-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="f5030-117">如需有關服務生命週期的詳細資訊，請參閱<xref:fundamentals/dependency-injection#service-lifetimes>。</span><span class="sxs-lookup"><span data-stu-id="f5030-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="f5030-118">新增單一服務</span><span class="sxs-lookup"><span data-stu-id="f5030-118">Add a singleton service</span></span>

<span data-ttu-id="f5030-119">為了方便從 gRPC C core 實作轉換到 ASP.NET Core，就可以變更服務實作的服務的存留期只限於單一。</span><span class="sxs-lookup"><span data-stu-id="f5030-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="f5030-120">這牽涉到將服務實作的執行個體新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="f5030-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="f5030-121">不過，已不再能夠解析範圍的服務，透過建構函式插入具有單一存留期服務實作。</span><span class="sxs-lookup"><span data-stu-id="f5030-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="f5030-122">設定 gRPC 服務選項</span><span class="sxs-lookup"><span data-stu-id="f5030-122">Configure gRPC services options</span></span>

<span data-ttu-id="f5030-123">在 C 核心為基礎的應用程式，設定，例如`grpc.max_receive_message_length`並`grpc.max_send_message_length`設有`ChannelOption`時[建構的伺服器執行個體](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)。</span><span class="sxs-lookup"><span data-stu-id="f5030-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="f5030-124">ASP.NET Core 中 gRPC 提供透過設定`GrpcServiceOptions`型別。</span><span class="sxs-lookup"><span data-stu-id="f5030-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="f5030-125">比方說，gRPC 服務的連入訊息大小上限可以透過設定`AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="f5030-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="f5030-126">如需有關組態的詳細資訊，請參閱<xref:grpc/configuration>。</span><span class="sxs-lookup"><span data-stu-id="f5030-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="f5030-127">記錄</span><span class="sxs-lookup"><span data-stu-id="f5030-127">Logging</span></span>

<span data-ttu-id="f5030-128">C 核心為基礎的應用程式依賴`GrpcEnvironment`要[設定記錄器](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)以進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="f5030-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="f5030-129">ASP.NET Core 堆疊提供這項功能，透過[記錄 API](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="f5030-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="f5030-130">比方說，記錄器可以新增至透過建構函式插入 gRPC 服務：</span><span class="sxs-lookup"><span data-stu-id="f5030-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="f5030-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f5030-131">HTTPS</span></span>

<span data-ttu-id="f5030-132">C 核心為基礎的應用程式設定透過 HTTPS [Server.Ports 屬性](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)。</span><span class="sxs-lookup"><span data-stu-id="f5030-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="f5030-133">類似的概念用來設定 ASP.NET Core 中的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f5030-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="f5030-134">例如，會使用 Kestrel[端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)這項功能。</span><span class="sxs-lookup"><span data-stu-id="f5030-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="f5030-135">攔截器與中介軟體</span><span class="sxs-lookup"><span data-stu-id="f5030-135">Interceptors and Middleware</span></span>

<span data-ttu-id="f5030-136">ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)提供類似的功能相較於 C 核心為基礎的 gRPC 應用程式中的攔截器。</span><span class="sxs-lookup"><span data-stu-id="f5030-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="f5030-137">中介軟體和攔截器在概念上是相同，兩者都用來建構管線，以處理 gRPC 要求。</span><span class="sxs-lookup"><span data-stu-id="f5030-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="f5030-138">這兩者都可以在管線中的下一個元件的前後執行的工作。</span><span class="sxs-lookup"><span data-stu-id="f5030-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="f5030-139">不過，ASP.NET Core 中介軟體作基礎 HTTP/2 的訊息，而使用抽象的 gRPC 圖層上運作的攔截器[ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)。</span><span class="sxs-lookup"><span data-stu-id="f5030-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f5030-140">其他資源</span><span class="sxs-lookup"><span data-stu-id="f5030-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
