---
title: ASP.NET Core SignalR 中的安全性考量
author: tdykstra
description: 了解如何在 ASP.NET Core SignalR 使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095128"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="d465d-103">ASP.NET Core SignalR 中的安全性考量</span><span class="sxs-lookup"><span data-stu-id="d465d-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="d465d-104">藉由[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="d465d-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="d465d-105">總覽</span><span class="sxs-lookup"><span data-stu-id="d465d-105">Overview</span></span>

<span data-ttu-id="d465d-106">SignalR 預設提供安全性保護的數的字。</span><span class="sxs-lookup"><span data-stu-id="d465d-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="d465d-107">請務必了解如何設定這些保護設定。</span><span class="sxs-lookup"><span data-stu-id="d465d-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="d465d-108">跨原始資源共用</span><span class="sxs-lookup"><span data-stu-id="d465d-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="d465d-109">[跨原始資源共用 (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)可用來允許跨原始來源 SignalR 連線的瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="d465d-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="d465d-110">如果您的 JavaScript 程式碼裝載在不同的網域名稱從 SignalR 應用程式，您必須啟用[ASP.NET Core CORS 中介軟體](xref:security/cors)才能允許連線。</span><span class="sxs-lookup"><span data-stu-id="d465d-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="d465d-111">一般情況下，允許跨原始來源要求，只能從您所控制的網域。</span><span class="sxs-lookup"><span data-stu-id="d465d-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="d465d-112">例如，如果您的網站裝載於 `http://www.example.com` 和您的 SignalR 應用程式裝載於 `http://signalr.example.com`，您應該在您的 SignalR 應用程式，只允許來源中設定 CORS `www.example.com`。</span><span class="sxs-lookup"><span data-stu-id="d465d-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="d465d-113">如需有關如何設定 CORS 的詳細資訊，請參閱 < [ASP.NET Core CORS 的相關文件](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="d465d-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="d465d-114">SignalR 需要下列的 CORS 原則，才能正確運作：</span><span class="sxs-lookup"><span data-stu-id="d465d-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="d465d-115">原則必須允許特定的原始來源，您預期，或允許任何來源 （不建議）。</span><span class="sxs-lookup"><span data-stu-id="d465d-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="d465d-116">HTTP 方法`GET`和`POST`必須允許。</span><span class="sxs-lookup"><span data-stu-id="d465d-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="d465d-117">必須啟用認證，即使您未使用驗證。</span><span class="sxs-lookup"><span data-stu-id="d465d-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="d465d-118">例如，下列的 CORS 原則允許 SignalR 瀏覽器的用戶端，裝載於 `http://example.com` 存取 SignalR 應用程式：</span><span class="sxs-lookup"><span data-stu-id="d465d-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="d465d-119">SignalR 與不相容的內建的 CORS 功能在 Azure App Service 中。</span><span class="sxs-lookup"><span data-stu-id="d465d-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="d465d-120">存取權杖的記錄</span><span class="sxs-lookup"><span data-stu-id="d465d-120">Access token logging</span></span>

<span data-ttu-id="d465d-121">使用 WebSockets 或 Server-Sent 事件時，瀏覽器用戶端會查詢字串中傳送存取權杖。</span><span class="sxs-lookup"><span data-stu-id="d465d-121">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="d465d-122">這是使用標準通常一樣安全`Authorization`標頭，不過許多網頁伺服器會記錄每個要求的 URL，包括查詢字串。</span><span class="sxs-lookup"><span data-stu-id="d465d-122">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="d465d-123">這表示記錄檔中，可能包含存取權杖。</span><span class="sxs-lookup"><span data-stu-id="d465d-123">This means the access token may be included in logs.</span></span> <span data-ttu-id="d465d-124">請考慮檢閱 網頁伺服器的記錄設定，以避免記錄這項資訊。</span><span class="sxs-lookup"><span data-stu-id="d465d-124">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="d465d-125">例外狀況</span><span class="sxs-lookup"><span data-stu-id="d465d-125">Exceptions</span></span>

<span data-ttu-id="d465d-126">例外狀況訊息通常會被認為不應該洩漏給用戶端的敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="d465d-126">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="d465d-127">根據預設，SignalR 不會傳送至用戶端中樞方法擲回的例外狀況詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d465d-127">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="d465d-128">相反地，用戶端會收到一般的訊息，指出發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d465d-128">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="d465d-129">您可以藉由設定覆寫這個行為[ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options)設定。</span><span class="sxs-lookup"><span data-stu-id="d465d-129">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="d465d-130">緩衝區管理</span><span class="sxs-lookup"><span data-stu-id="d465d-130">Buffer management</span></span>

<span data-ttu-id="d465d-131">SignalR 使用每個連線的緩衝區，以管理內送和外寄訊息。</span><span class="sxs-lookup"><span data-stu-id="d465d-131">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="d465d-132">根據預設，SignalR 會限制為 32 KB 的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="d465d-132">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="d465d-133">這表示用戶端或伺服器可以傳送最大可能的訊息為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="d465d-133">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="d465d-134">這也表示的訊息的連線所耗用的記憶體數量上限為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="d465d-134">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="d465d-135">如果您知道您的訊息都小於此限制，您可以減少此大小，以防止用戶端可以傳送較大的訊息，並強制伺服器配置的記憶體來接受它。</span><span class="sxs-lookup"><span data-stu-id="d465d-135">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="d465d-136">同樣地，如果您知道您的訊息大小超過此限制，您可以增加它。</span><span class="sxs-lookup"><span data-stu-id="d465d-136">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="d465d-137">不過，請注意，增加此限制表示用戶端可導致伺服器配置額外的記憶體，而且可能會降低您的應用程式可以處理的並行連線數目。</span><span class="sxs-lookup"><span data-stu-id="d465d-137">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="d465d-138">內送和外寄訊息的個別限制，都可以在設定成[ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)物件中設定`MapHub`:</span><span class="sxs-lookup"><span data-stu-id="d465d-138">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="d465d-139">`ApplicationMaxBufferSize` 表示從用戶端的最大位元組數，伺服器緩衝區。</span><span class="sxs-lookup"><span data-stu-id="d465d-139">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="d465d-140">如果用戶端會嘗試傳送訊息的大小超過此限制，可能會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="d465d-140">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="d465d-141">`TransportMaxBufferSize` 代表伺服器可以傳送的位元組的數目上限。</span><span class="sxs-lookup"><span data-stu-id="d465d-141">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="d465d-142">如果伺服器會嘗試傳送訊息 （包括從中樞方法的傳回值） 大於此限制，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d465d-142">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="d465d-143">若要設定的限制`0`完全停用限制。</span><span class="sxs-lookup"><span data-stu-id="d465d-143">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="d465d-144">不過，這應該格外小心。</span><span class="sxs-lookup"><span data-stu-id="d465d-144">However, this should be done with extreme caution.</span></span> <span data-ttu-id="d465d-145">移除此限制可讓用戶端傳送任何大小的訊息。</span><span class="sxs-lookup"><span data-stu-id="d465d-145">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="d465d-146">這可由惡意用戶端會造成過多的記憶體配置，因而大幅減少您的應用程式可支援的並行連線數目。</span><span class="sxs-lookup"><span data-stu-id="d465d-146">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
