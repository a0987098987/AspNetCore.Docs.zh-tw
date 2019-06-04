---
title: ASP.NET Core 組態 gRPC
author: jamesnk
description: 了解如何設定 gRPC 的 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 5/30/2019
uid: grpc/configuration
ms.openlocfilehash: 1f8250dc9aa8b82da384ee28287011baa19dc11f
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491240"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="52007-103">ASP.NET Core 組態 gRPC</span><span class="sxs-lookup"><span data-stu-id="52007-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="52007-104">設定服務選項</span><span class="sxs-lookup"><span data-stu-id="52007-104">Configure services options</span></span>

<span data-ttu-id="52007-105">下表說明設定 gRPC 服務的選項：</span><span class="sxs-lookup"><span data-stu-id="52007-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="52007-106">選項</span><span class="sxs-lookup"><span data-stu-id="52007-106">Option</span></span> | <span data-ttu-id="52007-107">預設值</span><span class="sxs-lookup"><span data-stu-id="52007-107">Default Value</span></span> | <span data-ttu-id="52007-108">描述</span><span class="sxs-lookup"><span data-stu-id="52007-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="52007-109">最大訊息大小可以從伺服器傳送的位元組數。</span><span class="sxs-lookup"><span data-stu-id="52007-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="52007-110">嘗試傳送的訊息長度超過設定的最大訊息大小會導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="52007-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="52007-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="52007-111">4 MB</span></span> | <span data-ttu-id="52007-112">最大訊息大小可以由伺服器接收的位元組數。</span><span class="sxs-lookup"><span data-stu-id="52007-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="52007-113">如果伺服器收到的訊息超過此限制，它會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="52007-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="52007-114">增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="52007-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="52007-115">如果`true`詳細服務方法擲回例外狀況時，將會傳回給用戶端的例外狀況訊息。</span><span class="sxs-lookup"><span data-stu-id="52007-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="52007-116">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="52007-116">The default is `false`.</span></span> <span data-ttu-id="52007-117">設定`EnableDetailedErrors`至`true`可能會導致洩漏機密資訊。</span><span class="sxs-lookup"><span data-stu-id="52007-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="52007-118">gzip</span><span class="sxs-lookup"><span data-stu-id="52007-118">gzip</span></span> | <span data-ttu-id="52007-119">壓縮來壓縮和解壓縮訊息使用的提供者集合。</span><span class="sxs-lookup"><span data-stu-id="52007-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="52007-120">可以建立自訂壓縮提供者，並加入至集合。</span><span class="sxs-lookup"><span data-stu-id="52007-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="52007-121">預設值設定提供者會支援**gzip**壓縮。</span><span class="sxs-lookup"><span data-stu-id="52007-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="52007-122">壓縮演算法，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="52007-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="52007-123">Algorithm 必須符合壓縮提供者中的`CompressionProviders`。</span><span class="sxs-lookup"><span data-stu-id="52007-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="52007-124">要壓縮回應的演算法，用戶端必須指出它傳送給它，以支援的演算法**grpc-接受編碼**標頭。</span><span class="sxs-lookup"><span data-stu-id="52007-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="52007-125">會壓縮從伺服器傳送的訊息所使用的壓縮層級。</span><span class="sxs-lookup"><span data-stu-id="52007-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="52007-126">可以為所有服務設定選項，藉由提供選項委派`AddGrpc`呼叫中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="52007-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="52007-127">單一服務的選項會覆寫中提供的全域選項`AddGrpc`，並可使用設定`AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="52007-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="52007-128">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="52007-128">Configure client options</span></span>

<span data-ttu-id="52007-129">下列程式碼會設定用戶端的最大傳送和接收訊息大小：</span><span class="sxs-lookup"><span data-stu-id="52007-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="52007-130">其他資源</span><span class="sxs-lookup"><span data-stu-id="52007-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
