---
title: ASP.NET Core 設定的 gRPC
author: jamesnk
description: 瞭解如何設定 ASP.NET Core 應用程式的 gRPC。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/21/2019
uid: grpc/configuration
ms.openlocfilehash: 34eb598211c87fbb2c68ae5e041da50d02f543f7
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310321"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="2aa65-103">ASP.NET Core 設定的 gRPC</span><span class="sxs-lookup"><span data-stu-id="2aa65-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="2aa65-104">設定服務選項</span><span class="sxs-lookup"><span data-stu-id="2aa65-104">Configure services options</span></span>

<span data-ttu-id="2aa65-105">下表說明設定 gRPC 服務的選項：</span><span class="sxs-lookup"><span data-stu-id="2aa65-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="2aa65-106">選項</span><span class="sxs-lookup"><span data-stu-id="2aa65-106">Option</span></span> | <span data-ttu-id="2aa65-107">預設值</span><span class="sxs-lookup"><span data-stu-id="2aa65-107">Default Value</span></span> | <span data-ttu-id="2aa65-108">描述</span><span class="sxs-lookup"><span data-stu-id="2aa65-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | <span data-ttu-id="2aa65-109">可以從伺服器傳送的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="2aa65-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="2aa65-110">如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2aa65-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `MaxReceiveMessageSize` | <span data-ttu-id="2aa65-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="2aa65-111">4 MB</span></span> | <span data-ttu-id="2aa65-112">伺服器可以接收的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="2aa65-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="2aa65-113">如果伺服器收到超過此限制的訊息，就會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2aa65-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="2aa65-114">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="2aa65-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="2aa65-115">如果`true`為，則在服務方法中擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="2aa65-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="2aa65-116">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="2aa65-116">The default is `false`.</span></span> <span data-ttu-id="2aa65-117">將`EnableDetailedErrors`設定`true`為可能會洩漏機密資訊。</span><span class="sxs-lookup"><span data-stu-id="2aa65-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="2aa65-118">gzip、deflate</span><span class="sxs-lookup"><span data-stu-id="2aa65-118">gzip, deflate</span></span> | <span data-ttu-id="2aa65-119">用來壓縮和解壓縮訊息的壓縮提供者集合。</span><span class="sxs-lookup"><span data-stu-id="2aa65-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="2aa65-120">您可以建立自訂壓縮提供者，並將其新增至集合。</span><span class="sxs-lookup"><span data-stu-id="2aa65-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="2aa65-121">預設設定的提供者支援**gzip**和**deflate**壓縮。</span><span class="sxs-lookup"><span data-stu-id="2aa65-121">The default configured providers support **gzip** and **deflate** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="2aa65-122">壓縮演算法，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="2aa65-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="2aa65-123">演算法必須符合中`CompressionProviders`的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="2aa65-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="2aa65-124">若要讓演算法壓縮回應，用戶端必須在**grpc-accept-encoding**標頭中傳送它，以指示它支援演算法。</span><span class="sxs-lookup"><span data-stu-id="2aa65-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="2aa65-125">壓縮層級，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="2aa65-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="2aa65-126">您可以為所有服務設定選項，方法是提供選項委派給`AddGrpc`中`Startup.ConfigureServices`的呼叫：</span><span class="sxs-lookup"><span data-stu-id="2aa65-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="2aa65-127">單一服務的選項會覆寫中`AddGrpc`提供的全域選項，而且可以使用`AddServiceOptions<TService>`來設定：</span><span class="sxs-lookup"><span data-stu-id="2aa65-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="2aa65-128">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="2aa65-128">Configure client options</span></span>

<span data-ttu-id="2aa65-129">下表說明設定 gRPC 通道的選項：</span><span class="sxs-lookup"><span data-stu-id="2aa65-129">The following table describes options for configuring gRPC channels:</span></span>

| <span data-ttu-id="2aa65-130">選項</span><span class="sxs-lookup"><span data-stu-id="2aa65-130">Option</span></span> | <span data-ttu-id="2aa65-131">預設值</span><span class="sxs-lookup"><span data-stu-id="2aa65-131">Default Value</span></span> | <span data-ttu-id="2aa65-132">描述</span><span class="sxs-lookup"><span data-stu-id="2aa65-132">Description</span></span> |
| ------ | ------------- | ----------- |
| `HttpClient` | <span data-ttu-id="2aa65-133">新增實例</span><span class="sxs-lookup"><span data-stu-id="2aa65-133">New instance</span></span> | <span data-ttu-id="2aa65-134">用`HttpClient`來進行 gRPC 呼叫的。</span><span class="sxs-lookup"><span data-stu-id="2aa65-134">The `HttpClient` used to make gRPC calls.</span></span> <span data-ttu-id="2aa65-135">用戶端可以設定為設定自訂`HttpClientHandler`，或將額外的處理常式新增至 HTTP 管線以進行 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2aa65-135">A client can be set to configure a custom `HttpClientHandler`, or add additional handlers to the HTTP pipeline for gRPC calls.</span></span> <span data-ttu-id="2aa65-136">預設會建立新`HttpClient`的實例。</span><span class="sxs-lookup"><span data-stu-id="2aa65-136">By default a new `HttpClient` instance is created.</span></span> |
| `MaxSendMessageSize` | `null` | <span data-ttu-id="2aa65-137">可以從用戶端傳送的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="2aa65-137">The maximum message size in bytes that can be sent from the client.</span></span> <span data-ttu-id="2aa65-138">如果嘗試傳送的訊息超過設定的訊息大小上限，就會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2aa65-138">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `MaxReceiveMessageSize` | <span data-ttu-id="2aa65-139">4 MB</span><span class="sxs-lookup"><span data-stu-id="2aa65-139">4 MB</span></span> | <span data-ttu-id="2aa65-140">用戶端可以接收的訊息大小上限（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="2aa65-140">The maximum message size in bytes that can be received by the client.</span></span> <span data-ttu-id="2aa65-141">如果伺服器收到超過此限制的訊息，就會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2aa65-141">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="2aa65-142">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="2aa65-142">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `TransportOptions` | `null` | <span data-ttu-id="2aa65-143">傳輸選項會設定通道呼叫 gRPC 服務的方式。</span><span class="sxs-lookup"><span data-stu-id="2aa65-143">Transport options configure how the channel calls the gRPC service.</span></span> <span data-ttu-id="2aa65-144">目前唯一的實`HttpClientTransport`作為選項可讓您指定 gRPC 所使用的。 `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="2aa65-144">Currently the only implementation is `HttpClientTransport` options lets you specify the `HttpClient` used by gRPC.</span></span> |
| `Credentials` | `null` | <span data-ttu-id="2aa65-145">`ChannelCredentials` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2aa65-145">A `ChannelCredentials` instance.</span></span> <span data-ttu-id="2aa65-146">認證是用來將驗證中繼資料新增至 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2aa65-146">Credentials are used to add authentication metadata to gRPC calls.</span></span> |
| `CompressionProviders` | <span data-ttu-id="2aa65-147">gzip、deflate</span><span class="sxs-lookup"><span data-stu-id="2aa65-147">gzip, deflate</span></span> | <span data-ttu-id="2aa65-148">用來壓縮和解壓縮訊息的壓縮提供者集合。</span><span class="sxs-lookup"><span data-stu-id="2aa65-148">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="2aa65-149">您可以建立自訂壓縮提供者，並將其新增至集合。</span><span class="sxs-lookup"><span data-stu-id="2aa65-149">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="2aa65-150">預設設定的提供者支援**gzip**和**deflate**壓縮。</span><span class="sxs-lookup"><span data-stu-id="2aa65-150">The default configured providers support **gzip** and **deflate** compression.</span></span> |

<span data-ttu-id="2aa65-151">下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="2aa65-151">The following code:</span></span>

* <span data-ttu-id="2aa65-152">設定通道上的傳送和接收訊息大小上限。</span><span class="sxs-lookup"><span data-stu-id="2aa65-152">Sets the maximum send and receive message size on the channel.</span></span>
* <span data-ttu-id="2aa65-153">建立用戶端。</span><span class="sxs-lookup"><span data-stu-id="2aa65-153">Creates a client.</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a><span data-ttu-id="2aa65-154">其他資源</span><span class="sxs-lookup"><span data-stu-id="2aa65-154">Additional resources</span></span>

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
