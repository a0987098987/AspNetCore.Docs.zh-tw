---
title: 適用于 .NET 設定的 gRPC
author: jamesnk
description: 瞭解如何設定 .NET 應用程式的 gRPC。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: de478752f94713d7f3d55ed66e6410500c3a72e8
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355724"
---
# <a name="grpc-for-net-configuration"></a><span data-ttu-id="a6559-103">適用于 .NET 設定的 gRPC</span><span class="sxs-lookup"><span data-stu-id="a6559-103">gRPC for .NET configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="a6559-104">設定服務選項</span><span class="sxs-lookup"><span data-stu-id="a6559-104">Configure services options</span></span>

<span data-ttu-id="a6559-105">gRPC 服務會使用*Startup.cs*中的 `AddGrpc` 進行設定。</span><span class="sxs-lookup"><span data-stu-id="a6559-105">gRPC services are configured with `AddGrpc` in *Startup.cs*.</span></span> <span data-ttu-id="a6559-106">下表說明設定 gRPC 服務的選項：</span><span class="sxs-lookup"><span data-stu-id="a6559-106">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="a6559-107">選項</span><span class="sxs-lookup"><span data-stu-id="a6559-107">Option</span></span> | <span data-ttu-id="a6559-108">預設值</span><span class="sxs-lookup"><span data-stu-id="a6559-108">Default Value</span></span> | <span data-ttu-id="a6559-109">描述</span><span class="sxs-lookup"><span data-stu-id="a6559-109">Description</span></span> |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | <span data-ttu-id="a6559-110">可以從伺服器傳送的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="a6559-110">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="a6559-111">如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a6559-111">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `MaxReceiveMessageSize` | <span data-ttu-id="a6559-112">4 MB</span><span class="sxs-lookup"><span data-stu-id="a6559-112">4 MB</span></span> | <span data-ttu-id="a6559-113">伺服器可以接收的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="a6559-113">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="a6559-114">如果伺服器收到超過此限制的訊息，就會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a6559-114">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="a6559-115">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="a6559-115">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="a6559-116">如果 `true`，則會在服務方法中擲回例外狀況時，將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="a6559-116">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="a6559-117">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="a6559-117">The default is `false`.</span></span> <span data-ttu-id="a6559-118">將 `EnableDetailedErrors` 設定為 `true` 可能會洩漏機密資訊。</span><span class="sxs-lookup"><span data-stu-id="a6559-118">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="a6559-119">gzip</span><span class="sxs-lookup"><span data-stu-id="a6559-119">gzip</span></span> | <span data-ttu-id="a6559-120">用來壓縮和解壓縮訊息的壓縮提供者集合。</span><span class="sxs-lookup"><span data-stu-id="a6559-120">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="a6559-121">您可以建立自訂壓縮提供者，並將其新增至集合。</span><span class="sxs-lookup"><span data-stu-id="a6559-121">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="a6559-122">預設設定的提供者支援**gzip**壓縮。</span><span class="sxs-lookup"><span data-stu-id="a6559-122">The default configured providers support **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="a6559-123">壓縮演算法，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="a6559-123">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="a6559-124">此演算法必須符合 `CompressionProviders`中的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="a6559-124">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="a6559-125">若要讓演算法壓縮回應，用戶端必須在**grpc-accept-encoding**標頭中傳送它，以指示它支援演算法。</span><span class="sxs-lookup"><span data-stu-id="a6559-125">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="a6559-126">壓縮層級，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="a6559-126">The compress level used to compress messages sent from the server.</span></span> |
| `Interceptors` | <span data-ttu-id="a6559-127">None</span><span class="sxs-lookup"><span data-stu-id="a6559-127">None</span></span> | <span data-ttu-id="a6559-128">以每個 gRPC 呼叫執行的攔截器集合。</span><span class="sxs-lookup"><span data-stu-id="a6559-128">A collection of interceptors that are run with each gRPC call.</span></span> <span data-ttu-id="a6559-129">攔截器會依照其註冊的循序執行。</span><span class="sxs-lookup"><span data-stu-id="a6559-129">Interceptors are run in the order they are registered.</span></span> <span data-ttu-id="a6559-130">全域設定的攔截器會在設定單一服務的攔截器之前執行。</span><span class="sxs-lookup"><span data-stu-id="a6559-130">Globally configured interceptors are run before interceptors configured for a single service.</span></span> <span data-ttu-id="a6559-131">如需 gRPC 攔截器的詳細資訊，請參閱[GRPC 攔截器與中介軟體](xref:grpc/migration#grpc-interceptors-vs-middleware)。</span><span class="sxs-lookup"><span data-stu-id="a6559-131">For more information about gRPC interceptors, see [gRPC Interceptors vs. Middleware](xref:grpc/migration#grpc-interceptors-vs-middleware).</span></span> |

<span data-ttu-id="a6559-132">您可以為所有服務設定選項，方法是在 `Startup.ConfigureServices`中提供 `AddGrpc` 呼叫的選項委派：</span><span class="sxs-lookup"><span data-stu-id="a6559-132">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="a6559-133">單一服務的選項會覆寫 `AddGrpc` 中提供的全域選項，而且可以使用 `AddServiceOptions<TService>`來設定：</span><span class="sxs-lookup"><span data-stu-id="a6559-133">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="a6559-134">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="a6559-134">Configure client options</span></span>

<span data-ttu-id="a6559-135">`GrpcChannelOptions`上設定 gRPC 用戶端設定。</span><span class="sxs-lookup"><span data-stu-id="a6559-135">gRPC client configuration is set on `GrpcChannelOptions`.</span></span> <span data-ttu-id="a6559-136">下表說明設定 gRPC 通道的選項：</span><span class="sxs-lookup"><span data-stu-id="a6559-136">The following table describes options for configuring gRPC channels:</span></span>

| <span data-ttu-id="a6559-137">選項</span><span class="sxs-lookup"><span data-stu-id="a6559-137">Option</span></span> | <span data-ttu-id="a6559-138">預設值</span><span class="sxs-lookup"><span data-stu-id="a6559-138">Default Value</span></span> | <span data-ttu-id="a6559-139">描述</span><span class="sxs-lookup"><span data-stu-id="a6559-139">Description</span></span> |
| ------ | ------------- | ----------- |
| `HttpClient` | <span data-ttu-id="a6559-140">新增實例</span><span class="sxs-lookup"><span data-stu-id="a6559-140">New instance</span></span> | <span data-ttu-id="a6559-141">用來進行 gRPC 呼叫的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="a6559-141">The `HttpClient` used to make gRPC calls.</span></span> <span data-ttu-id="a6559-142">您可以設定用戶端來設定自訂 `HttpClientHandler`，或將其他處理常式新增至 HTTP 管線以進行 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="a6559-142">A client can be set to configure a custom `HttpClientHandler`, or add additional handlers to the HTTP pipeline for gRPC calls.</span></span> <span data-ttu-id="a6559-143">如果未指定 `HttpClient`，則會為通道建立新的 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="a6559-143">If no `HttpClient` is specified, then a new `HttpClient` instance is created for the channel.</span></span> <span data-ttu-id="a6559-144">它會自動被處置。</span><span class="sxs-lookup"><span data-stu-id="a6559-144">It will automatically be disposed.</span></span> |
| `DisposeHttpClient` | `false` | <span data-ttu-id="a6559-145">如果 `true`，且指定了 `HttpClient`，則在處置 `GrpcChannel` 時，將會處置 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="a6559-145">If `true`, and an `HttpClient` is specified, then the `HttpClient` instance will be disposed when the `GrpcChannel` is disposed.</span></span> |
| `LoggerFactory` | `null` | <span data-ttu-id="a6559-146">用戶端用來記錄 gRPC 呼叫相關資訊的 `LoggerFactory`。</span><span class="sxs-lookup"><span data-stu-id="a6559-146">The `LoggerFactory` used by the client to log information about gRPC calls.</span></span> <span data-ttu-id="a6559-147">`LoggerFactory` 實例可以從相依性插入解析，或使用 `LoggerFactory.Create`來建立。</span><span class="sxs-lookup"><span data-stu-id="a6559-147">A `LoggerFactory` instance can be resolved from dependency injection or created using `LoggerFactory.Create`.</span></span> <span data-ttu-id="a6559-148">如需設定記錄的範例，請參閱 <xref:grpc/diagnostics#grpc-client-logging>。</span><span class="sxs-lookup"><span data-stu-id="a6559-148">For examples of configuring logging, see <xref:grpc/diagnostics#grpc-client-logging>.</span></span> |
| `MaxSendMessageSize` | `null` | <span data-ttu-id="a6559-149">可以從用戶端傳送的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="a6559-149">The maximum message size in bytes that can be sent from the client.</span></span> <span data-ttu-id="a6559-150">如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a6559-150">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `MaxReceiveMessageSize` | <span data-ttu-id="a6559-151">4 MB</span><span class="sxs-lookup"><span data-stu-id="a6559-151">4 MB</span></span> | <span data-ttu-id="a6559-152">用戶端可以接收的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="a6559-152">The maximum message size in bytes that can be received by the client.</span></span> <span data-ttu-id="a6559-153">如果用戶端收到超過此限制的訊息，就會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a6559-153">If the client receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="a6559-154">增加此值可讓用戶端接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="a6559-154">Increasing this value allows the client to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `Credentials` | `null` | <span data-ttu-id="a6559-155">`ChannelCredentials` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="a6559-155">A `ChannelCredentials` instance.</span></span> <span data-ttu-id="a6559-156">認證是用來將驗證中繼資料新增至 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="a6559-156">Credentials are used to add authentication metadata to gRPC calls.</span></span> |
| `CompressionProviders` | <span data-ttu-id="a6559-157">gzip</span><span class="sxs-lookup"><span data-stu-id="a6559-157">gzip</span></span> | <span data-ttu-id="a6559-158">用來壓縮和解壓縮訊息的壓縮提供者集合。</span><span class="sxs-lookup"><span data-stu-id="a6559-158">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="a6559-159">您可以建立自訂壓縮提供者，並將其新增至集合。</span><span class="sxs-lookup"><span data-stu-id="a6559-159">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="a6559-160">預設設定的提供者支援**gzip**壓縮。</span><span class="sxs-lookup"><span data-stu-id="a6559-160">The default configured providers support **gzip** compression.</span></span> |

<span data-ttu-id="a6559-161">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="a6559-161">The following code:</span></span>

* <span data-ttu-id="a6559-162">設定通道上的傳送和接收訊息大小上限。</span><span class="sxs-lookup"><span data-stu-id="a6559-162">Sets the maximum send and receive message size on the channel.</span></span>
* <span data-ttu-id="a6559-163">建立用戶端。</span><span class="sxs-lookup"><span data-stu-id="a6559-163">Creates a client.</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="a6559-164">其他資源</span><span class="sxs-lookup"><span data-stu-id="a6559-164">Additional resources</span></span>

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
