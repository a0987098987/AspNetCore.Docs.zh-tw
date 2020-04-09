---
title: 將 gRPC 服務從 C-Core 遷移至 ASP.NET Core
author: juntaoluo
description: 瞭解如何行動現有的基於 C 核的 gRPC 應用以在 ASP.NET 核心堆疊上運行。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 451171a041f7bbb3711babd73d2fa2e245aadd28
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664133"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="c6f0b-103">將 gRPC 服務從 C-Core 遷移至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6f0b-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="c6f0b-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="c6f0b-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="c6f0b-105">由於底層堆疊的實現,並非所有功能在[基於 C 核的 gRPC](https://grpc.io/blog/grpc-stacks)應用和基於 ASP.NET 的基於核心的應用程序之間以相同的方式工作。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="c6f0b-106">本文檔突出顯示了在兩個堆疊之間遷移的關鍵差異。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="c6f0b-107">gRPC 服務實現存留期</span><span class="sxs-lookup"><span data-stu-id="c6f0b-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="c6f0b-108">在ASP.NET核心堆疊中,默認情況下,gRPC 服務使用[作用域的存留期](xref:fundamentals/dependency-injection#service-lifetimes)創建。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c6f0b-109">相反,默認情況下 gRPC C 核綁定到具有[單噸存留期的](xref:fundamentals/dependency-injection#service-lifetimes)服務。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="c6f0b-110">作用域存留期允許服務實現使用作用域存留期解析其他服務。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="c6f0b-111">例如,作用域存留期還可以通過構造函數`DbContext`注入從DI容器解析。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="c6f0b-112">使用作用域存留期:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="c6f0b-113">為每個請求構造服務實現的新實例。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="c6f0b-114">無法通過實現類型的實例成員在請求之間共享狀態。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="c6f0b-115">期望將共用狀態存儲在 DI 容器中的單個服務中。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="c6f0b-116">存儲的共享狀態在 gRPC 服務實現的構造函數中解析。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="c6f0b-117">有關服務存留期的詳細資訊,請參<xref:fundamentals/dependency-injection#service-lifetimes>閱 。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="c6f0b-118">新增單例服務</span><span class="sxs-lookup"><span data-stu-id="c6f0b-118">Add a singleton service</span></span>

<span data-ttu-id="c6f0b-119">為了便於從 gRPC C 核心實現過渡到ASP.NET核心,可以將服務實現的服務存留期從作用域更改為單例。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="c6f0b-120">這涉及將服務實現的實體加入 DI 容器:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="c6f0b-121">但是,具有 singleton 生存期的服務實現不再能夠通過構造函數注入解析作用域服務。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="c6f0b-122">設定 gRPC 服務選項</span><span class="sxs-lookup"><span data-stu-id="c6f0b-122">Configure gRPC services options</span></span>

<span data-ttu-id="c6f0b-123">在基於 C 核的應用中,[在建構 Server](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)實體`ChannelOption`設定`grpc.max_receive_message_length``grpc.max_send_message_length`了 和 等設定。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="c6f0b-124">在ASP.NET核心中,gRPC`GrpcServiceOptions`通過類型提供配置。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="c6f0b-125">例如,gRPC 服務的最大傳入消息大小`AddGrpc`可以通過進行配置。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="c6f0b-126">以下範例將預設值`MaxReceiveMessageSize`4 MB 變更為 16 MB:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-126">The following example changes the default `MaxReceiveMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="c6f0b-127">有關設定的詳細資訊,請參閱<xref:grpc/configuration>。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="c6f0b-128">記錄</span><span class="sxs-lookup"><span data-stu-id="c6f0b-128">Logging</span></span>

<span data-ttu-id="c6f0b-129">基於 C 核的應用`GrpcEnvironment`依賴於[配置記錄器](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)以進行調試。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="c6f0b-130">ASP.NET核心堆疊通過[日誌記錄 API](xref:fundamentals/logging/index)提供此功能。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="c6f0b-131">例如,可以透過建構函數注入將記錄器添加到 gRPC 服務中:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="c6f0b-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="c6f0b-132">HTTPS</span></span>

<span data-ttu-id="c6f0b-133">基於 C 核的應用程式透過[Server.Ports 屬性](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)配置 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="c6f0b-134">類似的概念用於配置ASP.NET核心中的伺服器。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="c6f0b-135">例如,Kestrel 使用[終結點配置](xref:fundamentals/servers/kestrel#endpoint-configuration)進行此功能。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="grpc-interceptors-vs-middleware"></a><span data-ttu-id="c6f0b-136">gRPC 攔截器 vs 中間件</span><span class="sxs-lookup"><span data-stu-id="c6f0b-136">gRPC Interceptors vs Middleware</span></span>

<span data-ttu-id="c6f0b-137">ASP.NET核心[中間件](xref:fundamentals/middleware/index)提供類似的功能,與基於C核的gRPC應用中的攔截器相比。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="c6f0b-138">ASP.NET核心中間件和攔截器在概念上相似。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-138">ASP.NET Core middleware and interceptors are conceptually similar.</span></span> <span data-ttu-id="c6f0b-139">Both (兩者)：</span><span class="sxs-lookup"><span data-stu-id="c6f0b-139">Both:</span></span>

* <span data-ttu-id="c6f0b-140">用於建構處理 gRPC 請求的管道。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-140">Are used to construct a pipeline that handles a gRPC request.</span></span>
* <span data-ttu-id="c6f0b-141">允許在管道中的下一個元件之前或之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-141">Allow work to be performed before or after the next component in the pipeline.</span></span>
* <span data-ttu-id="c6f0b-142">提供存`HttpContext`取存取權限:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-142">Provide access to `HttpContext`:</span></span>
  * <span data-ttu-id="c6f0b-143">在中間件中`HttpContext`,是一個參數。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-143">In middleware the `HttpContext` is a parameter.</span></span>
  * <span data-ttu-id="c6f0b-144">在攔截器中,`HttpContext`可以使用擴充`ServerCallContext`方法`ServerCallContext.GetHttpContext`的 參數 存取。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-144">In interceptors the `HttpContext` can be accessed using the `ServerCallContext` parameter with the `ServerCallContext.GetHttpContext` extension method.</span></span> <span data-ttu-id="c6f0b-145">請注意,此功能特定於在 ASP.NET 核心中運行的攔截器。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-145">Note that this feature is specific to interceptors running in ASP.NET Core.</span></span>

<span data-ttu-id="c6f0b-146">gRPC 攔截器與 ASP.NET 核心中間件的差異:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-146">gRPC Interceptor differences from ASP.NET Core Middleware:</span></span>

* <span data-ttu-id="c6f0b-147">攔截器:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-147">Interceptors:</span></span>
  * <span data-ttu-id="c6f0b-148">使用[ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)在 gRPC 抽象層上操作。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-148">Operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>
  * <span data-ttu-id="c6f0b-149">提供對:</span><span class="sxs-lookup"><span data-stu-id="c6f0b-149">Provide access to:</span></span>
    * <span data-ttu-id="c6f0b-150">發送到呼叫的序列化消息。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-150">The deserialized message sent to a call.</span></span>
    * <span data-ttu-id="c6f0b-151">在序列化之前從調用返回的消息。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-151">The message being returned from the call before it is serialized.</span></span>
  * <span data-ttu-id="c6f0b-152">可以捕獲和處理從 gRPC 服務引發的異常。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-152">Can catch and handle exceptions thrown from gRPC services.</span></span>
* <span data-ttu-id="c6f0b-153">中介軟體：</span><span class="sxs-lookup"><span data-stu-id="c6f0b-153">Middleware:</span></span>
  * <span data-ttu-id="c6f0b-154">在 gRPC 攔截器之前運行。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-154">Runs before gRPC interceptors.</span></span>
  * <span data-ttu-id="c6f0b-155">對基礎 HTTP/2 消息進行操作。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-155">Operates on the underlying HTTP/2 messages.</span></span>
  * <span data-ttu-id="c6f0b-156">只能從請求和回應流訪問位元組。</span><span class="sxs-lookup"><span data-stu-id="c6f0b-156">Can only access bytes from the request and response streams.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6f0b-157">其他資源</span><span class="sxs-lookup"><span data-stu-id="c6f0b-157">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
