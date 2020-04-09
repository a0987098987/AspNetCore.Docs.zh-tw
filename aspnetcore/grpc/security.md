---
title: ASP.NET核心的 gRPC 中的安全注意事項
author: jamesnk
description: 瞭解 gRPC ASP.NET核心的安全注意事項。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
uid: grpc/security
ms.openlocfilehash: f84bec0ef485b701b2be36384a2e49b9b28e473d
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667318"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a><span data-ttu-id="6c91f-103">ASP.NET核心的 gRPC 中的安全注意事項</span><span class="sxs-lookup"><span data-stu-id="6c91f-103">Security considerations in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="6c91f-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="6c91f-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="6c91f-105">本文提供有關使用 .NET Core 保護 gRPC 的資訊。</span><span class="sxs-lookup"><span data-stu-id="6c91f-105">This article provides information on securing gRPC with .NET Core.</span></span>

## <a name="transport-security"></a><span data-ttu-id="6c91f-106">傳輸安全性</span><span class="sxs-lookup"><span data-stu-id="6c91f-106">Transport security</span></span>

<span data-ttu-id="6c91f-107">使用 HTTP/2 發送和接收 gRPC 消息。</span><span class="sxs-lookup"><span data-stu-id="6c91f-107">gRPC messages are sent and received using HTTP/2.</span></span> <span data-ttu-id="6c91f-108">我們建議:</span><span class="sxs-lookup"><span data-stu-id="6c91f-108">We recommend:</span></span>

* <span data-ttu-id="6c91f-109">[傳輸層安全 (TLS)](https://tools.ietf.org/html/rfc5246)用於保護生產 gRPC 應用中的消息。</span><span class="sxs-lookup"><span data-stu-id="6c91f-109">[Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) be used to secure messages in production gRPC apps.</span></span>
* <span data-ttu-id="6c91f-110">gRPC 服務應僅偵聽和回應安全埠。</span><span class="sxs-lookup"><span data-stu-id="6c91f-110">gRPC services should only listen and respond over secured ports.</span></span>

<span data-ttu-id="6c91f-111">TLS 在 Kestrel 中配置。</span><span class="sxs-lookup"><span data-stu-id="6c91f-111">TLS is configured in Kestrel.</span></span> <span data-ttu-id="6c91f-112">有關設定 Kestrel 終結點的詳細資訊,請參閱[Kestrel 終結點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="6c91f-112">For more information on configuring Kestrel endpoints, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="exceptions"></a><span data-ttu-id="6c91f-113">例外狀況</span><span class="sxs-lookup"><span data-stu-id="6c91f-113">Exceptions</span></span>

<span data-ttu-id="6c91f-114">異常消息通常被視為不應向客戶端顯示的敏感數據。</span><span class="sxs-lookup"><span data-stu-id="6c91f-114">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="6c91f-115">默認情況下,gRPC不會將 gRPC 服務引發的異常的詳細資訊發送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="6c91f-115">By default, gRPC doesn't send the details of an exception thrown by a gRPC service to the client.</span></span> <span data-ttu-id="6c91f-116">相反,用戶端會收到一條泛型消息,指示發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="6c91f-116">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="6c91f-117">例外訊息傳遞到客戶端可以重寫(例如,在開發或測試中)與[啟用詳細錯誤](xref:grpc/configuration#configure-services-options)。</span><span class="sxs-lookup"><span data-stu-id="6c91f-117">Exception message delivery to the client can be overridden (for example, in development or test) with [EnableDetailedErrors](xref:grpc/configuration#configure-services-options).</span></span> <span data-ttu-id="6c91f-118">異常消息不應在生產應用中向客戶端公開。</span><span class="sxs-lookup"><span data-stu-id="6c91f-118">Exception messages shouldn't be exposed to the client in production apps.</span></span>

## <a name="message-size-limits"></a><span data-ttu-id="6c91f-119">訊息大小限制</span><span class="sxs-lookup"><span data-stu-id="6c91f-119">Message size limits</span></span>

<span data-ttu-id="6c91f-120">傳入到 gRPC 用戶端和服務的消息將載入到記憶體中。</span><span class="sxs-lookup"><span data-stu-id="6c91f-120">Incoming messages to gRPC clients and services are loaded into memory.</span></span> <span data-ttu-id="6c91f-121">消息大小限制是一種機制,有助於防止 gRPC 消耗過多的資源。</span><span class="sxs-lookup"><span data-stu-id="6c91f-121">Message size limits are a mechanism to help prevent gRPC from consuming excessive resources.</span></span>

<span data-ttu-id="6c91f-122">gRPC 使用每封郵件大小限制來管理傳入和傳出消息。</span><span class="sxs-lookup"><span data-stu-id="6c91f-122">gRPC uses per-message size limits to manage incoming and outgoing messages.</span></span> <span data-ttu-id="6c91f-123">默認情況下,gRPC將傳入消息限制為 4 MB。</span><span class="sxs-lookup"><span data-stu-id="6c91f-123">By default, gRPC limits incoming messages to 4 MB.</span></span> <span data-ttu-id="6c91f-124">傳出郵件沒有限制。</span><span class="sxs-lookup"><span data-stu-id="6c91f-124">There is no limit on outgoing messages.</span></span>

<span data-ttu-id="6c91f-125">在伺服器上,gRPC 訊息限制可以設定為應用中的所有服務`AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="6c91f-125">On the server, gRPC message limits can be configured for all services in an app with `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 1 * 1024 * 1024; // 1 MB
        options.MaxSendMessageSize = 1 * 1024 * 1024; // 1 MB
    });
}
```

<span data-ttu-id="6c91f-126">也可以為使用`AddServiceOptions<TService>`的單個服務配置限制。</span><span class="sxs-lookup"><span data-stu-id="6c91f-126">Limits can also be configured for an individual service using `AddServiceOptions<TService>`.</span></span> <span data-ttu-id="6c91f-127">有關設定訊息大小限制的詳細資訊,請參考[gRPC 設定](xref:grpc/configuration)。</span><span class="sxs-lookup"><span data-stu-id="6c91f-127">For more information on configuring message size limits, see [gRPC configuration](xref:grpc/configuration).</span></span>

## <a name="client-certificate-validation"></a><span data-ttu-id="6c91f-128">用戶端憑證驗證</span><span class="sxs-lookup"><span data-stu-id="6c91f-128">Client certificate validation</span></span>

<span data-ttu-id="6c91f-129">建立連線時,最初將驗證[客戶端憑證](https://tools.ietf.org/html/rfc5246#section-7.4.4)。</span><span class="sxs-lookup"><span data-stu-id="6c91f-129">[Client certificates](https://tools.ietf.org/html/rfc5246#section-7.4.4) are initially validated when the connection is established.</span></span> <span data-ttu-id="6c91f-130">默認情況下,Kestrel 不執行連接客戶端證書的其他驗證。</span><span class="sxs-lookup"><span data-stu-id="6c91f-130">By default, Kestrel doesn't perform additional validation of a connection's client certificate.</span></span>

<span data-ttu-id="6c91f-131">我們建議由用戶端證書保護的 gRPC 服務使用[Microsoft.AspNetCore.身份驗證.證書](xref:security/authentication/certauth)包。</span><span class="sxs-lookup"><span data-stu-id="6c91f-131">We recommend that gRPC services secured by client certificates use the [Microsoft.AspNetCore.Authentication.Certificate](xref:security/authentication/certauth) package.</span></span> <span data-ttu-id="6c91f-132">ASP.NET核心認證認證將對用戶端憑證執行其他驗證,包括:</span><span class="sxs-lookup"><span data-stu-id="6c91f-132">ASP.NET Core certification authentication will perform additional validation on a client certificate, including:</span></span>

* <span data-ttu-id="6c91f-133">憑證具有有效的延伸金鑰使用 (EKU)</span><span class="sxs-lookup"><span data-stu-id="6c91f-133">Certificate has a valid extended key use (EKU)</span></span>
* <span data-ttu-id="6c91f-134">在其有效期內</span><span class="sxs-lookup"><span data-stu-id="6c91f-134">Is within its validity period</span></span>
* <span data-ttu-id="6c91f-135">檢查憑證撤銷</span><span class="sxs-lookup"><span data-stu-id="6c91f-135">Check certificate revocation</span></span>
