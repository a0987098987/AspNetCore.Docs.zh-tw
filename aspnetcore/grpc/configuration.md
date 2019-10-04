---
title: 適用于 .NET 設定的 gRPC
author: jamesnk
description: 瞭解如何設定 .NET 應用程式的 gRPC。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: 3ef92f10d914ef9fa3e13a7bdd5c863bab297f57
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925212"
---
# <a name="grpc-for-net-configuration"></a><span data-ttu-id="1b555-103">適用于 .NET 設定的 gRPC</span><span class="sxs-lookup"><span data-stu-id="1b555-103">gRPC for .NET configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="1b555-104">設定服務選項</span><span class="sxs-lookup"><span data-stu-id="1b555-104">Configure services options</span></span>

<span data-ttu-id="1b555-105">gRPC 服務會在`AddGrpc` *Startup.cs*中以設定。</span><span class="sxs-lookup"><span data-stu-id="1b555-105">gRPC services are configured with `AddGrpc` in *Startup.cs*.</span></span> <span data-ttu-id="1b555-106">下表說明設定 gRPC 服務的選項：</span><span class="sxs-lookup"><span data-stu-id="1b555-106">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="1b555-107">選項</span><span class="sxs-lookup"><span data-stu-id="1b555-107">Option</span></span> | <span data-ttu-id="1b555-108">Default Value</span><span class="sxs-lookup"><span data-stu-id="1b555-108">Default Value</span></span> | <span data-ttu-id="1b555-109">描述</span><span class="sxs-lookup"><span data-stu-id="1b555-109">Description</span></span> |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | <span data-ttu-id="1b555-110">可以從伺服器傳送的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1b555-110">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="1b555-111">如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b555-111">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `MaxReceiveMessageSize` | <span data-ttu-id="1b555-112">4 MB</span><span class="sxs-lookup"><span data-stu-id="1b555-112">4 MB</span></span> | <span data-ttu-id="1b555-113">伺服器可以接收的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1b555-113">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="1b555-114">如果伺服器收到超過此限制的訊息，就會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b555-114">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="1b555-115">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="1b555-115">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="1b555-116">如果`true`為，則在服務方法中擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="1b555-116">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="1b555-117">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="1b555-117">The default is `false`.</span></span> <span data-ttu-id="1b555-118">將`EnableDetailedErrors`設定`true`為可能會洩漏機密資訊。</span><span class="sxs-lookup"><span data-stu-id="1b555-118">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="1b555-119">Gzip</span><span class="sxs-lookup"><span data-stu-id="1b555-119">gzip</span></span> | <span data-ttu-id="1b555-120">用來壓縮和解壓縮訊息的壓縮提供者集合。</span><span class="sxs-lookup"><span data-stu-id="1b555-120">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="1b555-121">您可以建立自訂壓縮提供者，並將其新增至集合。</span><span class="sxs-lookup"><span data-stu-id="1b555-121">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="1b555-122">預設設定的提供者支援**gzip**壓縮。</span><span class="sxs-lookup"><span data-stu-id="1b555-122">The default configured providers support **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="1b555-123">壓縮演算法，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="1b555-123">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="1b555-124">演算法必須符合中`CompressionProviders`的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="1b555-124">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="1b555-125">若要讓演算法壓縮回應，用戶端必須在**grpc-accept-encoding**標頭中傳送它，以指示它支援演算法。</span><span class="sxs-lookup"><span data-stu-id="1b555-125">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="1b555-126">壓縮層級，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="1b555-126">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="1b555-127">您可以為所有服務設定選項，方法是提供選項委派給`AddGrpc`中`Startup.ConfigureServices`的呼叫：</span><span class="sxs-lookup"><span data-stu-id="1b555-127">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="1b555-128">單一服務的選項會覆寫中`AddGrpc`提供的全域選項，而且可以使用`AddServiceOptions<TService>`來設定：</span><span class="sxs-lookup"><span data-stu-id="1b555-128">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="1b555-129">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="1b555-129">Configure client options</span></span>

<span data-ttu-id="1b555-130">gRPC client configuration 已設定為`GrpcChannelOptions`on。</span><span class="sxs-lookup"><span data-stu-id="1b555-130">gRPC client configuration is set on `GrpcChannelOptions`.</span></span> <span data-ttu-id="1b555-131">下表說明設定 gRPC 通道的選項：</span><span class="sxs-lookup"><span data-stu-id="1b555-131">The following table describes options for configuring gRPC channels:</span></span>

| <span data-ttu-id="1b555-132">選項</span><span class="sxs-lookup"><span data-stu-id="1b555-132">Option</span></span> | <span data-ttu-id="1b555-133">Default Value</span><span class="sxs-lookup"><span data-stu-id="1b555-133">Default Value</span></span> | <span data-ttu-id="1b555-134">描述</span><span class="sxs-lookup"><span data-stu-id="1b555-134">Description</span></span> |
| ------ | ------------- | ----------- |
| `HttpClient` | <span data-ttu-id="1b555-135">新增實例</span><span class="sxs-lookup"><span data-stu-id="1b555-135">New instance</span></span> | <span data-ttu-id="1b555-136">用`HttpClient`來進行 gRPC 呼叫的。</span><span class="sxs-lookup"><span data-stu-id="1b555-136">The `HttpClient` used to make gRPC calls.</span></span> <span data-ttu-id="1b555-137">用戶端可以設定為設定自訂`HttpClientHandler`，或將額外的處理常式新增至 HTTP 管線以進行 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="1b555-137">A client can be set to configure a custom `HttpClientHandler`, or add additional handlers to the HTTP pipeline for gRPC calls.</span></span> <span data-ttu-id="1b555-138">如果未指定，則會為通道`HttpClient`建立新的實例。 `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="1b555-138">If no `HttpClient` is specified, then a new `HttpClient` instance is created for the channel.</span></span> <span data-ttu-id="1b555-139">它會自動被處置。</span><span class="sxs-lookup"><span data-stu-id="1b555-139">It will automatically be disposed.</span></span> |
| `DisposeHttpClient` | `false` | <span data-ttu-id="1b555-140">如果`true`指定`HttpClient` 了和`GrpcChannel` ，則在處置時，將會處置實例。`HttpClient`</span><span class="sxs-lookup"><span data-stu-id="1b555-140">If `true`, and an `HttpClient` is specified, then the `HttpClient` instance will be disposed when the `GrpcChannel` is disposed.</span></span> |
| `LoggerFactory` | `null` | <span data-ttu-id="1b555-141">客戶`LoggerFactory`端用來記錄 gRPC 呼叫相關資訊的。</span><span class="sxs-lookup"><span data-stu-id="1b555-141">The `LoggerFactory` used by the client to log information about gRPC calls.</span></span> <span data-ttu-id="1b555-142">實例可以從相依性插入解析，或使用`LoggerFactory.Create`建立。 `LoggerFactory`</span><span class="sxs-lookup"><span data-stu-id="1b555-142">A `LoggerFactory` instance can be resolved from dependency injection or created using `LoggerFactory.Create`.</span></span> <span data-ttu-id="1b555-143">如需設定記錄的範例， <xref:grpc/diagnostics#grpc-client-logging>請參閱。</span><span class="sxs-lookup"><span data-stu-id="1b555-143">For examples of configuring logging, see <xref:grpc/diagnostics#grpc-client-logging>.</span></span> |
| `MaxSendMessageSize` | `null` | <span data-ttu-id="1b555-144">可以從用戶端傳送的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1b555-144">The maximum message size in bytes that can be sent from the client.</span></span> <span data-ttu-id="1b555-145">如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b555-145">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `MaxReceiveMessageSize` | <span data-ttu-id="1b555-146">4 MB</span><span class="sxs-lookup"><span data-stu-id="1b555-146">4 MB</span></span> | <span data-ttu-id="1b555-147">用戶端可以接收的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="1b555-147">The maximum message size in bytes that can be received by the client.</span></span> <span data-ttu-id="1b555-148">如果用戶端收到超過此限制的訊息，就會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b555-148">If the client receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="1b555-149">增加此值可讓用戶端接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="1b555-149">Increasing this value allows the client to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `Credentials` | `null` | <span data-ttu-id="1b555-150">`ChannelCredentials` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="1b555-150">A `ChannelCredentials` instance.</span></span> <span data-ttu-id="1b555-151">認證是用來將驗證中繼資料新增至 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="1b555-151">Credentials are used to add authentication metadata to gRPC calls.</span></span> |
| `CompressionProviders` | <span data-ttu-id="1b555-152">Gzip</span><span class="sxs-lookup"><span data-stu-id="1b555-152">gzip</span></span> | <span data-ttu-id="1b555-153">用來壓縮和解壓縮訊息的壓縮提供者集合。</span><span class="sxs-lookup"><span data-stu-id="1b555-153">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="1b555-154">您可以建立自訂壓縮提供者，並將其新增至集合。</span><span class="sxs-lookup"><span data-stu-id="1b555-154">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="1b555-155">預設設定的提供者支援**gzip**壓縮。</span><span class="sxs-lookup"><span data-stu-id="1b555-155">The default configured providers support **gzip** compression.</span></span> |

<span data-ttu-id="1b555-156">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="1b555-156">The following code:</span></span>

* <span data-ttu-id="1b555-157">設定通道上的傳送和接收訊息大小上限。</span><span class="sxs-lookup"><span data-stu-id="1b555-157">Sets the maximum send and receive message size on the channel.</span></span>
* <span data-ttu-id="1b555-158">建立用戶端。</span><span class="sxs-lookup"><span data-stu-id="1b555-158">Creates a client.</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="1b555-159">其他資源</span><span class="sxs-lookup"><span data-stu-id="1b555-159">Additional resources</span></span>

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
