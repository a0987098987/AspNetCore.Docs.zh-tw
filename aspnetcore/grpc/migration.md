---
title: 將 gRPC 服務從 C-核心遷移至 ASP.NET Core
author: juntaoluo
description: 瞭解如何移動現有以 C 核心為基礎的 gRPC 應用程式，以在 ASP.NET Core 堆疊上執行。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: c4c07808540c9af370bfa253e8154a8a19f0f3de
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634061"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="9a8e7-103">將 gRPC 服務從 C-核心遷移至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a8e7-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="9a8e7-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="9a8e7-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="9a8e7-105">由於基礎堆疊的執行，並非所有功能在以[C 核心為基礎的 gRPC](https://grpc.io/blog/grpc-stacks)應用程式和 ASP.NET Core 型應用程式之間以相同的方式工作。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="9a8e7-106">本檔將重點放在兩個堆疊之間進行遷移的主要差異。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="9a8e7-107">gRPC 服務實生命週期</span><span class="sxs-lookup"><span data-stu-id="9a8e7-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="9a8e7-108">在 ASP.NET Core 堆疊中，預設會以[限定範圍的存留期](xref:fundamentals/dependency-injection#service-lifetimes)來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="9a8e7-109">相反地，gRPC C-core 預設會系結至具有[單一存留期](xref:fundamentals/dependency-injection#service-lifetimes)的服務。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="9a8e7-110">限定範圍存留期可讓服務執行解析具有限定範圍存留期的其他服務。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="9a8e7-111">例如，範圍存留期也可以透過函式插入，從 DI 容器解析 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="9a8e7-112">使用範圍存留期：</span><span class="sxs-lookup"><span data-stu-id="9a8e7-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="9a8e7-113">服務實的新實例會針對每個要求而建立。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="9a8e7-114">您無法透過執行類型上的實例成員，在要求之間共用狀態。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="9a8e7-115">預期的情況是將共用狀態儲存在 DI 容器的單一服務中。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="9a8e7-116">儲存的共用狀態會在 gRPC 服務執行的函式中解析。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="9a8e7-117">如需服務存留期的詳細資訊，請參閱 <xref:fundamentals/dependency-injection#service-lifetimes>。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="9a8e7-118">新增單一服務</span><span class="sxs-lookup"><span data-stu-id="9a8e7-118">Add a singleton service</span></span>

<span data-ttu-id="9a8e7-119">為了協助從 gRPC C 核心的執行轉換成 ASP.NET Core，您可以將服務實的服務存留期從範圍變更為 singleton。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="9a8e7-120">這牽涉到將服務實作為實例新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="9a8e7-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="9a8e7-121">不過，具有單一存留期的服務執行，無法再透過函式插入來解析已設定範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="9a8e7-122">設定 gRPC services 選項</span><span class="sxs-lookup"><span data-stu-id="9a8e7-122">Configure gRPC services options</span></span>

<span data-ttu-id="9a8e7-123">在以 C 為基礎的應用程式中，`grpc.max_receive_message_length` 和 `grpc.max_send_message_length` 等設定會在[建立伺服器實例](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)時，使用 `ChannelOption` 來設定。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="9a8e7-124">在 ASP.NET Core 中，gRPC 會透過 `GrpcServiceOptions` 類型提供設定。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="9a8e7-125">例如，gRPC 服務的傳入訊息大小上限可以透過 `AddGrpc` 來設定。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="9a8e7-126">下列範例會將 4 MB 的預設 `MaxReceiveMessageSize` 變更為 16 MB：</span><span class="sxs-lookup"><span data-stu-id="9a8e7-126">The following example changes the default `MaxReceiveMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="9a8e7-127">如需設定的詳細資訊，請參閱 <xref:grpc/configuration>。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="9a8e7-128">記錄</span><span class="sxs-lookup"><span data-stu-id="9a8e7-128">Logging</span></span>

<span data-ttu-id="9a8e7-129">以 C 核心為基礎的應用程式依賴 `GrpcEnvironment` 來[設定記錄器](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)以進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="9a8e7-130">ASP.NET Core 堆疊會透過[記錄 API](xref:fundamentals/logging/index)提供這種功能。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="9a8e7-131">例如，您可以透過函式插入，將記錄器新增至 gRPC 服務：</span><span class="sxs-lookup"><span data-stu-id="9a8e7-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="9a8e7-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9a8e7-132">HTTPS</span></span>

<span data-ttu-id="9a8e7-133">以 C 核心為基礎的應用程式會透過[伺服器埠屬性](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)來設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="9a8e7-134">在 ASP.NET Core 中設定伺服器時，會使用類似的概念。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="9a8e7-135">例如，Kestrel 會使用[端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定來取得這種功能。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="grpc-interceptors-vs-middleware"></a><span data-ttu-id="9a8e7-136">gRPC 攔截器 vs 中介軟體</span><span class="sxs-lookup"><span data-stu-id="9a8e7-136">gRPC Interceptors vs Middleware</span></span>

<span data-ttu-id="9a8e7-137">相較于以 C 核心為基礎的 gRPC 應用程式中的攔截器，ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="9a8e7-138">ASP.NET Core 中介軟體和攔截器在概念上類似。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-138">ASP.NET Core middleware and interceptors are conceptually similar.</span></span> <span data-ttu-id="9a8e7-139">既</span><span class="sxs-lookup"><span data-stu-id="9a8e7-139">Both:</span></span>

* <span data-ttu-id="9a8e7-140">是用來建立處理 gRPC 要求的管線。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-140">Are used to construct a pipeline that handles a gRPC request.</span></span>
* <span data-ttu-id="9a8e7-141">允許在管線中的下一個元件之前或之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-141">Allow work to be performed before or after the next component in the pipeline.</span></span>
* <span data-ttu-id="9a8e7-142">提供 `HttpContext`的存取權：</span><span class="sxs-lookup"><span data-stu-id="9a8e7-142">Provide access to `HttpContext`:</span></span>
  * <span data-ttu-id="9a8e7-143">在中介軟體中，`HttpContext` 是一個參數。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-143">In middleware the `HttpContext` is a parameter.</span></span>
  * <span data-ttu-id="9a8e7-144">在攔截器中，可以使用 `ServerCallContext` 參數搭配 `ServerCallContext.GetHttpContext` 擴充方法來存取 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-144">In interceptors the `HttpContext` can be accessed using the `ServerCallContext` parameter with the `ServerCallContext.GetHttpContext` extension method.</span></span> <span data-ttu-id="9a8e7-145">請注意，這項功能是在 ASP.NET Core 中執行的攔截器所特有。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-145">Note that this feature is specific to interceptors running in ASP.NET Core.</span></span>

<span data-ttu-id="9a8e7-146">gRPC 攔截器與 ASP.NET Core 中介軟體的差異：</span><span class="sxs-lookup"><span data-stu-id="9a8e7-146">gRPC Interceptor differences from ASP.NET Core Middleware:</span></span>

* <span data-ttu-id="9a8e7-147">攔截器</span><span class="sxs-lookup"><span data-stu-id="9a8e7-147">Interceptors:</span></span>
  * <span data-ttu-id="9a8e7-148">使用[ServerCallCoNtext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)在抽象概念的 gRPC 層上操作。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-148">Operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>
  * <span data-ttu-id="9a8e7-149">提供存取權：</span><span class="sxs-lookup"><span data-stu-id="9a8e7-149">Provide access to:</span></span>
    * <span data-ttu-id="9a8e7-150">傳送至呼叫的已還原序列化訊息。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-150">The deserialized message sent to a call.</span></span>
    * <span data-ttu-id="9a8e7-151">在序列化之前，從呼叫傳回的訊息。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-151">The message being returned from the call before it is serialized.</span></span>
* <span data-ttu-id="9a8e7-152">中介軟體</span><span class="sxs-lookup"><span data-stu-id="9a8e7-152">Middleware:</span></span>
  * <span data-ttu-id="9a8e7-153">在 gRPC 攔截器之前執行。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-153">Runs before gRPC interceptors.</span></span>
  * <span data-ttu-id="9a8e7-154">在基礎 HTTP/2 訊息上操作。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-154">Operates on the underlying HTTP/2 messages.</span></span>
  * <span data-ttu-id="9a8e7-155">只能存取來自要求和回應資料流程的位元組。</span><span class="sxs-lookup"><span data-stu-id="9a8e7-155">Can only access bytes from the request and response streams.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a8e7-156">其他資源</span><span class="sxs-lookup"><span data-stu-id="9a8e7-156">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
