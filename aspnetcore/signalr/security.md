---
title: ASP.NET Core 中的安全性考慮SignalR
author: bradygaster
description: 瞭解如何在 ASP.NET Core SignalR中使用驗證和授權。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/security
ms.openlocfilehash: 2b049d9d8131c6c95b2f768620c984d0f67f92cc
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775319"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="91487-103">ASP.NET Core 中的安全性考慮SignalR</span><span class="sxs-lookup"><span data-stu-id="91487-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="91487-104">[Andrew Stanton-護士](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="91487-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="91487-105">本文提供保護SignalR的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="91487-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="91487-106">跨原始資源共用</span><span class="sxs-lookup"><span data-stu-id="91487-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="91487-107">[跨原始來源資源分享（CORS）](https://www.w3.org/TR/cors/)可以用來允許瀏覽器中的SignalR跨原始來源連接。</span><span class="sxs-lookup"><span data-stu-id="91487-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="91487-108">如果 JavaScript 程式碼裝載于不同的網域，則SignalR必須啟用[CORS 中介軟體](xref:security/cors)，才能讓 JavaScript 連線到SignalR應用程式。</span><span class="sxs-lookup"><span data-stu-id="91487-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="91487-109">只允許來自您信任或控制之網域的跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="91487-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="91487-110">例如：</span><span class="sxs-lookup"><span data-stu-id="91487-110">For example:</span></span>

* <span data-ttu-id="91487-111">您的網站託管于`http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="91487-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="91487-112">您SignalR的應用程式裝載于`http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="91487-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="91487-113">應該在SignalR應用程式中設定 CORS，使其只允許`www.example.com`來源。</span><span class="sxs-lookup"><span data-stu-id="91487-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="91487-114">如需設定 CORS 的詳細資訊，請參閱[啟用跨原始來源要求（CORS）](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="91487-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> SignalR<span data-ttu-id="91487-115">**需要**下列 CORS 原則：</span><span class="sxs-lookup"><span data-stu-id="91487-115"> **requires** the following CORS policies:</span></span>

* <span data-ttu-id="91487-116">允許特定的預期來源。</span><span class="sxs-lookup"><span data-stu-id="91487-116">Allow the specific expected origins.</span></span> <span data-ttu-id="91487-117">允許任何來源，**但不是安全或**建議的。</span><span class="sxs-lookup"><span data-stu-id="91487-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="91487-118">HTTP 方法`GET`和`POST`必須是允許的。</span><span class="sxs-lookup"><span data-stu-id="91487-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="91487-119">必須允許認證，才能讓以 cookie 為基礎的適當會話正常運作。</span><span class="sxs-lookup"><span data-stu-id="91487-119">Credentials must be allowed in order for cookie-based sticky sessions to work correctly.</span></span> <span data-ttu-id="91487-120">即使未使用驗證，也必須啟用它們。</span><span class="sxs-lookup"><span data-stu-id="91487-120">They must be enabled even when authentication isn't used.</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="91487-121">不過，在5.0 中，我們在 TypeScript 用戶端中提供了不使用認證的選項。</span><span class="sxs-lookup"><span data-stu-id="91487-121">However, in 5.0 we have provided an option in the TypeScript client to not use credentials.</span></span>
<span data-ttu-id="91487-122">當您知道100% 時，只有在您的應用程式中不需要 Cookie （例如，azure app service 會在使用多部伺服器來處理粘滯會話時使用 cookie）時，才應使用不使用認證的選項。</span><span class="sxs-lookup"><span data-stu-id="91487-122">The option to not use credentials should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers for sticky sessions).</span></span>

::: moniker-end

<span data-ttu-id="91487-123">例如，下列 CORS 原則允許裝載在上SignalR `https://example.com`的瀏覽器用戶端存取上SignalR `https://signalr.example.com`裝載的應用程式：</span><span class="sxs-lookup"><span data-stu-id="91487-123">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

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

## <a name="websocket-origin-restriction"></a><span data-ttu-id="91487-124">WebSocket 來源限制</span><span class="sxs-lookup"><span data-stu-id="91487-124">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="91487-125">CORS 所提供的保護不套用至 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="91487-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="91487-126">如需 Websocket 的原始限制，請參閱[websocket 原始限制](xref:fundamentals/websockets#websocket-origin-restriction)。</span><span class="sxs-lookup"><span data-stu-id="91487-126">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="91487-127">CORS 所提供的保護不套用至 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="91487-127">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="91487-128">瀏覽器**不**會：</span><span class="sxs-lookup"><span data-stu-id="91487-128">Browsers do **not**:</span></span>

* <span data-ttu-id="91487-129">執行 CORS 的事前要求。</span><span class="sxs-lookup"><span data-stu-id="91487-129">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="91487-130">進行 WebSocket 要求時，採用 `Access-Control` 標頭中所指定的限制。</span><span class="sxs-lookup"><span data-stu-id="91487-130">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="91487-131">不過，瀏覽器會在發出 WebSocket 要求時，傳送 `Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="91487-131">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="91487-132">應設定應用程式驗證這些標頭，以確保只允許來自預期來源的 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="91487-132">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="91487-133">在 ASP.NET Core 2.1 和更新版本中，可以使用之前`Configure` \*\* `UseSignalR`\*\* 放置的自訂中介軟體來達成標頭驗證，並在中進行驗證中介軟體：</span><span class="sxs-lookup"><span data-stu-id="91487-133">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="91487-134">因為 `Origin` 由用戶端控制，所以和 `Referer` 標頭一樣可能受到偽造。</span><span class="sxs-lookup"><span data-stu-id="91487-134">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="91487-135">這些標頭**不**應做為驗證機制使用。</span><span class="sxs-lookup"><span data-stu-id="91487-135">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="connectionid"></a><span data-ttu-id="91487-136">ConnectionId</span><span class="sxs-lookup"><span data-stu-id="91487-136">ConnectionId</span></span>

<span data-ttu-id="91487-137">如果`ConnectionId` SignalR伺服器或用戶端版本已 ASP.NET Core 2.2 或更早版本，則公開可能會導致惡意模擬。</span><span class="sxs-lookup"><span data-stu-id="91487-137">Exposing `ConnectionId` can lead to malicious impersonation if the SignalR server or client version is ASP.NET Core 2.2 or earlier.</span></span> <span data-ttu-id="91487-138">如果SignalR伺服器和用戶端版本是 ASP.NET Core 3.0 或更新版本， `ConnectionToken`則（而`ConnectionId`非）必須保持機密。</span><span class="sxs-lookup"><span data-stu-id="91487-138">If the SignalR server and client version are ASP.NET Core 3.0 or later, the `ConnectionToken` rather than the `ConnectionId` must be kept secret.</span></span> <span data-ttu-id="91487-139">刻意`ConnectionToken`不會在任何 API 中公開。</span><span class="sxs-lookup"><span data-stu-id="91487-139">The `ConnectionToken` is purposely not exposed in any API.</span></span>  <span data-ttu-id="91487-140">這可能很容易確保較舊SignalR的用戶端不會連線到伺服器，因此即使您SignalR的`ConnectionId`伺服器版本是 ASP.NET Core 3.0 或更新版本，也不應該公開。</span><span class="sxs-lookup"><span data-stu-id="91487-140">It can be difficult to ensure that older SignalR clients aren't connecting to the server, so even if your SignalR server version is ASP.NET Core 3.0 or later, the `ConnectionId` shouldn't be exposed.</span></span>

## <a name="access-token-logging"></a><span data-ttu-id="91487-141">存取權杖記錄</span><span class="sxs-lookup"><span data-stu-id="91487-141">Access token logging</span></span>

<span data-ttu-id="91487-142">使用 Websocket 或伺服器傳送事件時，瀏覽器用戶端會在查詢字串中傳送存取權杖。</span><span class="sxs-lookup"><span data-stu-id="91487-142">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="91487-143">透過查詢字串接收存取權杖通常是使用標準`Authorization`標頭的安全方式。</span><span class="sxs-lookup"><span data-stu-id="91487-143">Receiving the access token via query string is generally secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="91487-144">請一律使用 HTTPS 來確保用戶端與伺服器之間的安全端對端連接。</span><span class="sxs-lookup"><span data-stu-id="91487-144">Always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="91487-145">許多 web 伺服器會記錄每個要求的 URL，包括查詢字串。</span><span class="sxs-lookup"><span data-stu-id="91487-145">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="91487-146">記錄 Url 可能會記錄存取權杖。</span><span class="sxs-lookup"><span data-stu-id="91487-146">Logging the URLs may log the access token.</span></span> <span data-ttu-id="91487-147">ASP.NET Core 預設會記錄每個要求的 URL，其中會包含查詢字串。</span><span class="sxs-lookup"><span data-stu-id="91487-147">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="91487-148">例如：</span><span class="sxs-lookup"><span data-stu-id="91487-148">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="91487-149">如果您對於使用伺服器記錄檔記錄這項資料有疑慮，您可以將`Microsoft.AspNetCore.Hosting`記錄器設定為`Warning`層級（或更新版本）來完全停用此記錄（ `Info`這些訊息是在層級寫入）。</span><span class="sxs-lookup"><span data-stu-id="91487-149">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="91487-150">如需詳細資訊，請參閱[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)。</span><span class="sxs-lookup"><span data-stu-id="91487-150">For more information, see [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="91487-151">如果您仍然想要記錄特定的要求資訊，可以[撰寫中介軟體](xref:fundamentals/middleware/write)來記錄所需的資料，並篩選出`access_token`查詢字串值（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="91487-151">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="91487-152">例外狀況</span><span class="sxs-lookup"><span data-stu-id="91487-152">Exceptions</span></span>

<span data-ttu-id="91487-153">例外狀況訊息通常會被視為不應該向用戶端顯示的敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="91487-153">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="91487-154">根據預設， SignalR不會將中樞方法擲回之例外狀況的詳細資料傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="91487-154">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="91487-155">相反地，用戶端會收到一般訊息，指出發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="91487-155">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="91487-156">您可以使用[EnableDetailedErrors](xref:signalr/configuration#configure-server-options)覆寫傳遞給用戶端的例外狀況訊息（例如，在開發或測試中）。</span><span class="sxs-lookup"><span data-stu-id="91487-156">Exception message delivery to the client can be overridden (for example in development or test) with [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="91487-157">例外狀況訊息不應在生產環境應用程式中公開給用戶端。</span><span class="sxs-lookup"><span data-stu-id="91487-157">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="91487-158">緩衝區管理</span><span class="sxs-lookup"><span data-stu-id="91487-158">Buffer management</span></span>

SignalR<span data-ttu-id="91487-159">會使用每個連接的緩衝區來管理傳入和傳出訊息。</span><span class="sxs-lookup"><span data-stu-id="91487-159"> uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="91487-160">根據預設， SignalR會將這些緩衝區限制為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="91487-160">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="91487-161">用戶端或伺服器可以傳送的最大訊息為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="91487-161">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="91487-162">訊息的連接所耗用的最大記憶體為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="91487-162">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="91487-163">如果您的訊息一律小於 32 KB，您可以減少限制，這會：</span><span class="sxs-lookup"><span data-stu-id="91487-163">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="91487-164">防止用戶端能夠傳送較大的訊息。</span><span class="sxs-lookup"><span data-stu-id="91487-164">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="91487-165">伺服器永遠都不需要配置大型緩衝區來接受訊息。</span><span class="sxs-lookup"><span data-stu-id="91487-165">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="91487-166">如果您的訊息大於 32 KB，您可以增加限制。</span><span class="sxs-lookup"><span data-stu-id="91487-166">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="91487-167">增加此限制表示：</span><span class="sxs-lookup"><span data-stu-id="91487-167">Increasing this limit means:</span></span>

* <span data-ttu-id="91487-168">用戶端可能會導致伺服器配置大量的記憶體緩衝區。</span><span class="sxs-lookup"><span data-stu-id="91487-168">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="91487-169">大型緩衝區的伺服器配置可能會減少並行連接的數目。</span><span class="sxs-lookup"><span data-stu-id="91487-169">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="91487-170">傳入和傳出訊息有限制，這兩者都可以在中`MapHub`設定的[HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options)物件上進行設定：</span><span class="sxs-lookup"><span data-stu-id="91487-170">There are limits for incoming and outgoing messages, both can be configured on the [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="91487-171">`ApplicationMaxBufferSize`代表用戶端中伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="91487-171">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="91487-172">如果用戶端嘗試傳送大於此限制的訊息，連接可能會關閉。</span><span class="sxs-lookup"><span data-stu-id="91487-172">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="91487-173">`TransportMaxBufferSize`表示伺服器可以傳送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="91487-173">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="91487-174">如果伺服器嘗試傳送大於此限制的訊息（包括來自中樞方法的傳回值），則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="91487-174">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="91487-175">將限制設為`0`會停用限制。</span><span class="sxs-lookup"><span data-stu-id="91487-175">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="91487-176">移除此限制可讓用戶端傳送任何大小的訊息。</span><span class="sxs-lookup"><span data-stu-id="91487-176">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="91487-177">傳送大型訊息的惡意用戶端可能會導致配置過量的記憶體。</span><span class="sxs-lookup"><span data-stu-id="91487-177">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="91487-178">過多的記憶體使用量可能會大幅減少並行連接的數目。</span><span class="sxs-lookup"><span data-stu-id="91487-178">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
