---
title: ASP.NET Core 組態 gRPC
author: jamesnk
description: 了解如何設定 gRPC 的 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 851c9ca1f7d62f6f368df66bb38eb4bbaf64bf32
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2019
ms.locfileid: "66041887"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="14c5b-103">ASP.NET Core 組態 gRPC</span><span class="sxs-lookup"><span data-stu-id="14c5b-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="14c5b-104">設定服務選項</span><span class="sxs-lookup"><span data-stu-id="14c5b-104">Configure services options</span></span>

<span data-ttu-id="14c5b-105">下表說明設定 gRPC 服務的選項：</span><span class="sxs-lookup"><span data-stu-id="14c5b-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="14c5b-106">選項</span><span class="sxs-lookup"><span data-stu-id="14c5b-106">Option</span></span> | <span data-ttu-id="14c5b-107">預設值</span><span class="sxs-lookup"><span data-stu-id="14c5b-107">Default Value</span></span> | <span data-ttu-id="14c5b-108">描述</span><span class="sxs-lookup"><span data-stu-id="14c5b-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="14c5b-109">最大訊息大小可以從伺服器傳送的位元組數。</span><span class="sxs-lookup"><span data-stu-id="14c5b-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="14c5b-110">嘗試傳送的訊息長度超過設定的最大訊息大小會導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="14c5b-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="14c5b-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="14c5b-111">4 MB</span></span> | <span data-ttu-id="14c5b-112">最大訊息大小可以由伺服器接收的位元組數。</span><span class="sxs-lookup"><span data-stu-id="14c5b-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="14c5b-113">如果伺服器收到的訊息超過此限制，它會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="14c5b-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="14c5b-114">增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="14c5b-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="14c5b-115">如果`true`詳細服務方法擲回例外狀況時，將會傳回給用戶端的例外狀況訊息。</span><span class="sxs-lookup"><span data-stu-id="14c5b-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="14c5b-116">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="14c5b-116">The default is `false`.</span></span> <span data-ttu-id="14c5b-117">這個設定設為`true`可能會導致洩漏機密資訊。</span><span class="sxs-lookup"><span data-stu-id="14c5b-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="14c5b-118">gzip</span><span class="sxs-lookup"><span data-stu-id="14c5b-118">gzip</span></span> | <span data-ttu-id="14c5b-119">壓縮來壓縮和解壓縮訊息使用的提供者集合。</span><span class="sxs-lookup"><span data-stu-id="14c5b-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="14c5b-120">可以建立自訂壓縮提供者，並加入至集合。</span><span class="sxs-lookup"><span data-stu-id="14c5b-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="14c5b-121">預設值設定提供者會支援**gzip**壓縮。</span><span class="sxs-lookup"><span data-stu-id="14c5b-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="14c5b-122">壓縮演算法，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="14c5b-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="14c5b-123">Algorithm 必須符合壓縮提供者中的`CompressionProviders`。</span><span class="sxs-lookup"><span data-stu-id="14c5b-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="14c5b-124">壓縮回應 algorthm 用戶端必須指出它傳送給它，以支援的演算法**grpc-接受編碼**標頭。</span><span class="sxs-lookup"><span data-stu-id="14c5b-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="14c5b-125">會壓縮從伺服器傳送的訊息所使用的壓縮層級。</span><span class="sxs-lookup"><span data-stu-id="14c5b-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="14c5b-126">可以為所有服務設定選項，藉由提供選項委派`AddGrpc`呼叫中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="14c5b-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="14c5b-127">單一服務的選項會覆寫中提供的全域選項`AddGrpc`，並可使用設定`AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="14c5b-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="additional-resources"></a><span data-ttu-id="14c5b-128">其他資源</span><span class="sxs-lookup"><span data-stu-id="14c5b-128">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
