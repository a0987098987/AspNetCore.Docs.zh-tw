---
title: ASP.NET Core SignalR 中的安全性考慮
author: bradygaster
description: 瞭解如何在 ASP.NET Core SignalR中使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: 4b27d9abb36938ed8161ff0d3535204e3fa68765
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294710"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="329a0-103">ASP.NET Core SignalR 中的安全性考慮</span><span class="sxs-lookup"><span data-stu-id="329a0-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="329a0-104">[Andrew Stanton-護士](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="329a0-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="329a0-105">本文提供保護 SignalR的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="329a0-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="329a0-106">跨原始資源共用</span><span class="sxs-lookup"><span data-stu-id="329a0-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="329a0-107">[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)可以用來允許瀏覽器中的跨原始來源 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="329a0-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="329a0-108">如果 JavaScript 程式碼裝載于 SignalR 應用程式的不同網域，則必須啟用[CORS 中介軟體](xref:security/cors)，才能讓 JavaScript 連線到 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="329a0-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="329a0-109">只允許來自您信任或控制之網域的跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="329a0-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="329a0-110">例如：</span><span class="sxs-lookup"><span data-stu-id="329a0-110">For example:</span></span>

* <span data-ttu-id="329a0-111">您的網站主控于 `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="329a0-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="329a0-112">您的 SignalR 應用程式裝載于 `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="329a0-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="329a0-113">應該在 SignalR 應用程式中設定 CORS，只允許原始 `www.example.com`。</span><span class="sxs-lookup"><span data-stu-id="329a0-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="329a0-114">如需設定 CORS 的詳細資訊，請參閱[啟用跨原始來源要求（CORS）](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="329a0-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> SignalR<span data-ttu-id="329a0-115">**需要**下列 CORS 原則：</span><span class="sxs-lookup"><span data-stu-id="329a0-115"> **requires** the following CORS policies:</span></span>

* <span data-ttu-id="329a0-116">允許特定的預期來源。</span><span class="sxs-lookup"><span data-stu-id="329a0-116">Allow the specific expected origins.</span></span> <span data-ttu-id="329a0-117">允許任何來源，**但不是安全或**建議的。</span><span class="sxs-lookup"><span data-stu-id="329a0-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="329a0-118">必須允許 `GET` 和 `POST` 的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="329a0-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="329a0-119">必須允許認證，才能讓以 cookie 為基礎的適當會話正常運作。</span><span class="sxs-lookup"><span data-stu-id="329a0-119">Credentials must be allowed in order for cookie-based sticky sessions to work correctly.</span></span> <span data-ttu-id="329a0-120">即使未使用驗證，也必須啟用它們。</span><span class="sxs-lookup"><span data-stu-id="329a0-120">They must be enabled even when authentication isn't used.</span></span>

<!--
::: moniker range=">= aspnetcore-5.0"  // Moniker here just to make sure this doesn't get missed in the 5.0 version update.
However, in 5.0 we have provided an option in the TypeScript client to not use credentials.
The not to use credentials option should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers)

For more info, see https://github.com/aspnet/AspNetCore.Docs/issues/16003
.-->

<span data-ttu-id="329a0-121">例如，下列 CORS 原則可讓裝載于 `https://example.com` 上的 SignalR 瀏覽器用戶端存取 `https://signalr.example.com`上裝載的 SignalR 應用程式：</span><span class="sxs-lookup"><span data-stu-id="329a0-121">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> SignalR<span data-ttu-id="329a0-122"> 與 Azure App Service 中內建的 CORS 功能不相容。</span><span class="sxs-lookup"><span data-stu-id="329a0-122"> is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="329a0-123">WebSocket 來源限制</span><span class="sxs-lookup"><span data-stu-id="329a0-123">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="329a0-124">CORS 所提供的保護不套用至 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="329a0-124">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="329a0-125">如需 Websocket 的原始限制，請參閱[websocket 原始限制](xref:fundamentals/websockets#websocket-origin-restriction)。</span><span class="sxs-lookup"><span data-stu-id="329a0-125">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="329a0-126">CORS 所提供的保護不套用至 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="329a0-126">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="329a0-127">瀏覽器**不**會：</span><span class="sxs-lookup"><span data-stu-id="329a0-127">Browsers do **not**:</span></span>

* <span data-ttu-id="329a0-128">執行 CORS 的事前要求。</span><span class="sxs-lookup"><span data-stu-id="329a0-128">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="329a0-129">進行 WebSocket 要求時，採用 `Access-Control` 標頭中所指定的限制。</span><span class="sxs-lookup"><span data-stu-id="329a0-129">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="329a0-130">不過，瀏覽器會在發出 WebSocket 要求時，傳送 `Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="329a0-130">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="329a0-131">應設定應用程式驗證這些標頭，以確保只允許來自預期來源的 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="329a0-131">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="329a0-132">在 ASP.NET Core 2.1 和更新版本中，可使用在 `UseSignalR`之前放置的自訂中介軟體，以及 `Configure`中的**驗證中間**件，來達成標頭驗證：</span><span class="sxs-lookup"><span data-stu-id="329a0-132">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="329a0-133">因為 `Origin` 由用戶端控制，所以和 `Referer` 標頭一樣可能受到偽造。</span><span class="sxs-lookup"><span data-stu-id="329a0-133">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="329a0-134">這些標頭**不**應做為驗證機制使用。</span><span class="sxs-lookup"><span data-stu-id="329a0-134">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="connectionid"></a><span data-ttu-id="329a0-135">ConnectionId</span><span class="sxs-lookup"><span data-stu-id="329a0-135">ConnectionId</span></span>

<span data-ttu-id="329a0-136">如果 SignalR 伺服器或用戶端版本 ASP.NET Core 2.2 或更早版本，公開 `ConnectionId` 可能會導致惡意模擬。</span><span class="sxs-lookup"><span data-stu-id="329a0-136">Exposing `ConnectionId` can lead to malicious impersonation if the SignalR server or client version is ASP.NET Core 2.2 or earlier.</span></span> <span data-ttu-id="329a0-137">如果 SignalR 伺服器和用戶端版本是 ASP.NET Core 3.0 或更新版本，則 `ConnectionToken` 而非 `ConnectionId` 必須保持機密。</span><span class="sxs-lookup"><span data-stu-id="329a0-137">If the SignalR server and client version are ASP.NET Core 3.0 or later, the `ConnectionToken` rather than the `ConnectionId` must be kept secret.</span></span> <span data-ttu-id="329a0-138">`ConnectionToken` 刻意不會在任何 API 中公開。</span><span class="sxs-lookup"><span data-stu-id="329a0-138">The `ConnectionToken` is purposely not exposed in any API.</span></span>  <span data-ttu-id="329a0-139">這可能很容易確保較舊的 SignalR 用戶端不會連線到伺服器，因此即使您的 SignalR 伺服器版本 ASP.NET Core 3.0 或更新版本，`ConnectionId` 也不應公開。</span><span class="sxs-lookup"><span data-stu-id="329a0-139">It can be difficult to ensure that older SignalR clients aren't connecting to the server, so even if your SignalR server version is ASP.NET Core 3.0 or later, the `ConnectionId` shouldn't be exposed.</span></span>

## <a name="access-token-logging"></a><span data-ttu-id="329a0-140">存取權杖記錄</span><span class="sxs-lookup"><span data-stu-id="329a0-140">Access token logging</span></span>

<span data-ttu-id="329a0-141">使用 Websocket 或伺服器傳送事件時，瀏覽器用戶端會在查詢字串中傳送存取權杖。</span><span class="sxs-lookup"><span data-stu-id="329a0-141">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="329a0-142">透過查詢字串接收存取權杖通常是使用標準 `Authorization` 標頭的安全方式。</span><span class="sxs-lookup"><span data-stu-id="329a0-142">Receiving the access token via query string is generally secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="329a0-143">請一律使用 HTTPS 來確保用戶端與伺服器之間的安全端對端連接。</span><span class="sxs-lookup"><span data-stu-id="329a0-143">Always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="329a0-144">許多 web 伺服器會記錄每個要求的 URL，包括查詢字串。</span><span class="sxs-lookup"><span data-stu-id="329a0-144">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="329a0-145">記錄 Url 可能會記錄存取權杖。</span><span class="sxs-lookup"><span data-stu-id="329a0-145">Logging the URLs may log the access token.</span></span> <span data-ttu-id="329a0-146">ASP.NET Core 預設會記錄每個要求的 URL，其中會包含查詢字串。</span><span class="sxs-lookup"><span data-stu-id="329a0-146">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="329a0-147">例如：</span><span class="sxs-lookup"><span data-stu-id="329a0-147">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="329a0-148">如果您對於使用伺服器記錄檔記錄這項資料有疑慮，您可以將 `Microsoft.AspNetCore.Hosting` 記錄器設定為 `Warning` 層級（或更新版本）來完全停用此記錄（這些訊息是以 `Info` 層級寫入）。</span><span class="sxs-lookup"><span data-stu-id="329a0-148">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="329a0-149">如需詳細資訊，請參閱[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)。</span><span class="sxs-lookup"><span data-stu-id="329a0-149">For more information, see [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="329a0-150">如果您仍然想要記錄特定的要求資訊，可以[撰寫中介軟體](xref:fundamentals/middleware/write)來記錄所需的資料，並篩選出 `access_token` 查詢字串值（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="329a0-150">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="329a0-151">例外狀況</span><span class="sxs-lookup"><span data-stu-id="329a0-151">Exceptions</span></span>

<span data-ttu-id="329a0-152">例外狀況訊息通常會被視為不應該向用戶端顯示的敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="329a0-152">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="329a0-153">根據預設，SignalR 不會將中樞方法擲回之例外狀況的詳細資料傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="329a0-153">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="329a0-154">相反地，用戶端會收到一般訊息，指出發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="329a0-154">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="329a0-155">您可以使用[EnableDetailedErrors](xref:signalr/configuration#configure-server-options)覆寫傳遞給用戶端的例外狀況訊息（例如，在開發或測試中）。</span><span class="sxs-lookup"><span data-stu-id="329a0-155">Exception message delivery to the client can be overridden (for example in development or test) with [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="329a0-156">例外狀況訊息不應在生產環境應用程式中公開給用戶端。</span><span class="sxs-lookup"><span data-stu-id="329a0-156">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="329a0-157">緩衝區管理</span><span class="sxs-lookup"><span data-stu-id="329a0-157">Buffer management</span></span>

SignalR<span data-ttu-id="329a0-158"> 會使用每個連接的緩衝區來管理傳入和傳出訊息。</span><span class="sxs-lookup"><span data-stu-id="329a0-158"> uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="329a0-159">根據預設，SignalR 會將這些緩衝區限制為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="329a0-159">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="329a0-160">用戶端或伺服器可以傳送的最大訊息為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="329a0-160">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="329a0-161">訊息的連接所耗用的最大記憶體為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="329a0-161">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="329a0-162">如果您的訊息一律小於 32 KB，您可以減少限制，這會：</span><span class="sxs-lookup"><span data-stu-id="329a0-162">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="329a0-163">防止用戶端能夠傳送較大的訊息。</span><span class="sxs-lookup"><span data-stu-id="329a0-163">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="329a0-164">伺服器永遠都不需要配置大型緩衝區來接受訊息。</span><span class="sxs-lookup"><span data-stu-id="329a0-164">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="329a0-165">如果您的訊息大於 32 KB，您可以增加限制。</span><span class="sxs-lookup"><span data-stu-id="329a0-165">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="329a0-166">增加此限制表示：</span><span class="sxs-lookup"><span data-stu-id="329a0-166">Increasing this limit means:</span></span>

* <span data-ttu-id="329a0-167">用戶端可能會導致伺服器配置大量的記憶體緩衝區。</span><span class="sxs-lookup"><span data-stu-id="329a0-167">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="329a0-168">大型緩衝區的伺服器配置可能會減少並行連接的數目。</span><span class="sxs-lookup"><span data-stu-id="329a0-168">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="329a0-169">傳入和傳出訊息有一些限制，這兩者都可以在 `MapHub`中設定的[HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options)物件上進行設定：</span><span class="sxs-lookup"><span data-stu-id="329a0-169">There are limits for incoming and outgoing messages, both can be configured on the [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="329a0-170">`ApplicationMaxBufferSize` 代表用戶端中伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="329a0-170">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="329a0-171">如果用戶端嘗試傳送大於此限制的訊息，連接可能會關閉。</span><span class="sxs-lookup"><span data-stu-id="329a0-171">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="329a0-172">`TransportMaxBufferSize` 代表伺服器可以傳送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="329a0-172">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="329a0-173">如果伺服器嘗試傳送大於此限制的訊息（包括來自中樞方法的傳回值），則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="329a0-173">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="329a0-174">將限制設定為 `0` 會停用此限制。</span><span class="sxs-lookup"><span data-stu-id="329a0-174">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="329a0-175">移除此限制可讓用戶端傳送任何大小的訊息。</span><span class="sxs-lookup"><span data-stu-id="329a0-175">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="329a0-176">傳送大型訊息的惡意用戶端可能會導致配置過量的記憶體。</span><span class="sxs-lookup"><span data-stu-id="329a0-176">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="329a0-177">過多的記憶體使用量可能會大幅減少並行連接的數目。</span><span class="sxs-lookup"><span data-stu-id="329a0-177">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
