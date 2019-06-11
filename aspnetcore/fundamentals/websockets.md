---
title: ASP.NET Core 中的 WebSockets 支援
author: rick-anderson
description: 了解如何在 ASP.NET Core 中開始使用 WebSocket。
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/websockets
ms.openlocfilehash: 4c49a5349c0718e5c59f30e6d51caf7a43fa0454
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458454"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="31413-103">ASP.NET Core 中的 WebSockets 支援</span><span class="sxs-lookup"><span data-stu-id="31413-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="31413-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="31413-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="31413-105">本文說明如何在 ASP.NET Core 中開始使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="31413-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="31413-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。</span><span class="sxs-lookup"><span data-stu-id="31413-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="31413-107">它用於受益於快速且即時通訊的應用程式，例如聊天、儀表板和遊戲應用程式。</span><span class="sxs-lookup"><span data-stu-id="31413-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="31413-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="31413-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="31413-109">[如何執行](#sample-app)。</span><span class="sxs-lookup"><span data-stu-id="31413-109">[How to run](#sample-app).</span></span>

## <a name="signalr"></a><span data-ttu-id="31413-110">SignalR</span><span class="sxs-lookup"><span data-stu-id="31413-110">SignalR</span></span>

<span data-ttu-id="31413-111">[ASP.NET Core SignalR](xref:signalr/introduction) 是程式庫，可簡化將即時 Web 功能新增至應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="31413-111">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="31413-112">它會盡可能使用 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="31413-112">It uses WebSockets whenever possible.</span></span>

<span data-ttu-id="31413-113">針對大部分的應用程式，我們建議透過原始 WebSocket 的 SignalR。</span><span class="sxs-lookup"><span data-stu-id="31413-113">For most applications, we recommend SignalR over raw WebSockets.</span></span> <span data-ttu-id="31413-114">SignalR 為不提供 WebSocket 的環境提供傳輸後援。</span><span class="sxs-lookup"><span data-stu-id="31413-114">SignalR provides transport fallback for environments where WebSockets is not available.</span></span> <span data-ttu-id="31413-115">它也提供簡單的遠端程序呼叫應用程式模型。</span><span class="sxs-lookup"><span data-stu-id="31413-115">It also provides a simple remote procedure call app model.</span></span> <span data-ttu-id="31413-116">而且在大部分的案例中，相較於使用原始 WebSocket，SignalR 沒有顯著的效能缺點。</span><span class="sxs-lookup"><span data-stu-id="31413-116">And in most scenarios, SignalR has no significant performance disadvantage compared to using raw WebSockets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31413-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="31413-117">Prerequisites</span></span>

* <span data-ttu-id="31413-118">ASP.NET Core 1.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="31413-118">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="31413-119">支援 ASP.NET Core 的任何作業系統：</span><span class="sxs-lookup"><span data-stu-id="31413-119">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="31413-120">Windows 7/Windows Server 2008 或更新版本</span><span class="sxs-lookup"><span data-stu-id="31413-120">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="31413-121">Linux</span><span class="sxs-lookup"><span data-stu-id="31413-121">Linux</span></span>
  * <span data-ttu-id="31413-122">macOS</span><span class="sxs-lookup"><span data-stu-id="31413-122">macOS</span></span>
  
* <span data-ttu-id="31413-123">如果應用程式在 Windows 上與 IIS 搭配執行：</span><span class="sxs-lookup"><span data-stu-id="31413-123">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="31413-124">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="31413-124">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="31413-125">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="31413-125">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="31413-126">必須啟用 WebSocket (請參閱 [IIS/IIS Express 支援](#iisiis-express-support)一節。)。</span><span class="sxs-lookup"><span data-stu-id="31413-126">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="31413-127">如果應用程式在 [HTTP.sys](xref:fundamentals/servers/httpsys) 上執行：</span><span class="sxs-lookup"><span data-stu-id="31413-127">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="31413-128">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="31413-128">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="31413-129">如需支援的瀏覽器，請請參閱 https://caniuse.com/#feat=websockets。</span><span class="sxs-lookup"><span data-stu-id="31413-129">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

::: moniker range="< aspnetcore-2.1"

## <a name="nuget-package"></a><span data-ttu-id="31413-130">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="31413-130">NuGet package</span></span>

<span data-ttu-id="31413-131">安裝 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 套件。</span><span class="sxs-lookup"><span data-stu-id="31413-131">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>

::: moniker-end

## <a name="configure-the-middleware"></a><span data-ttu-id="31413-132">設定中介軟體</span><span class="sxs-lookup"><span data-stu-id="31413-132">Configure the middleware</span></span>


<span data-ttu-id="31413-133">在 `Startup` 類別的 `Configure` 方法中新增 WebSocket 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="31413-133">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="31413-134">您可以設定下列設定：</span><span class="sxs-lookup"><span data-stu-id="31413-134">The following settings can be configured:</span></span>

* <span data-ttu-id="31413-135">`KeepAliveInterval` - 要將 "ping" 框架傳送到用戶端，以確保 Proxy 保持連線開啟的頻率。</span><span class="sxs-lookup"><span data-stu-id="31413-135">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="31413-136">預設為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="31413-136">The default is two minutes.</span></span>
* <span data-ttu-id="31413-137">`ReceiveBufferSize` - 用來接收資料的緩衝區大小。</span><span class="sxs-lookup"><span data-stu-id="31413-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="31413-138">進階使用者可能需要變更此設定，以便根據資料的大小進行效能調整。</span><span class="sxs-lookup"><span data-stu-id="31413-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="31413-139">預設為 4 KB。</span><span class="sxs-lookup"><span data-stu-id="31413-139">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="31413-140">您可以設定下列設定：</span><span class="sxs-lookup"><span data-stu-id="31413-140">The following settings can be configured:</span></span>

* <span data-ttu-id="31413-141">`KeepAliveInterval` - 要將 "ping" 框架傳送到用戶端，以確保 Proxy 保持連線開啟的頻率。</span><span class="sxs-lookup"><span data-stu-id="31413-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="31413-142">預設為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="31413-142">The default is two minutes.</span></span>
* <span data-ttu-id="31413-143">`ReceiveBufferSize` - 用來接收資料的緩衝區大小。</span><span class="sxs-lookup"><span data-stu-id="31413-143">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="31413-144">進階使用者可能需要變更此設定，以便根據資料的大小進行效能調整。</span><span class="sxs-lookup"><span data-stu-id="31413-144">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="31413-145">預設為 4 KB。</span><span class="sxs-lookup"><span data-stu-id="31413-145">The default is 4 KB.</span></span>
* <span data-ttu-id="31413-146">`AllowedOrigins` - WebSocket 要求之允許 Origin 標頭值的清單。</span><span class="sxs-lookup"><span data-stu-id="31413-146">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="31413-147">根據預設，會允許所有來源。</span><span class="sxs-lookup"><span data-stu-id="31413-147">By default, all origins are allowed.</span></span> <span data-ttu-id="31413-148">如需詳細資訊，請參閱下方的 "WebSocket origin restriction" (WebSocket 來源限制)。</span><span class="sxs-lookup"><span data-stu-id="31413-148">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

## <a name="accept-websocket-requests"></a><span data-ttu-id="31413-149">接受 WebSocket 要求</span><span class="sxs-lookup"><span data-stu-id="31413-149">Accept WebSocket requests</span></span>

<span data-ttu-id="31413-150">在要求生命週期的後半部某處 (例如，在 `Configure` 方法或動作方法的後半部)，檢查它是否為 WebSocket 要求並接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="31413-150">Somewhere later in the request life cycle (later in the `Configure` method or in an action method, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="31413-151">下列範例取自 `Configure` 方法的後半部：</span><span class="sxs-lookup"><span data-stu-id="31413-151">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="31413-152">WebSocket 要求可以傳入任何 URL，但此範例程式碼只接受 `/ws` 的要求。</span><span class="sxs-lookup"><span data-stu-id="31413-152">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="31413-153">使用 WebSocket 時，您**必須**確保中介軟體管線會在連線期間內持續執行。</span><span class="sxs-lookup"><span data-stu-id="31413-153">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="31413-154">如果您嘗試在中介軟體管線結束後傳送或接收 WebSocket 訊息，便可能會收到類似下列的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="31413-154">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="31413-155">如果您是使用背景服務來將資料寫入 WebSocket，請務必使中介軟體管線持續執行。</span><span class="sxs-lookup"><span data-stu-id="31413-155">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="31413-156">請使用 <xref:System.Threading.Tasks.TaskCompletionSource%601> 來這麼做。</span><span class="sxs-lookup"><span data-stu-id="31413-156">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="31413-157">將 `TaskCompletionSource` 傳遞至您的背景服務，並讓它在您完成使用 WebSocket 時呼叫 <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A>。</span><span class="sxs-lookup"><span data-stu-id="31413-157">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="31413-158">然後，在要求期間，`await` <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> 屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="31413-158">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request, as shown in the following example:</span></span>

```csharp
app.Use(async (context, next) => {
    var socket = await context.WebSockets.AcceptWebSocketAsync();
    var socketFinishedTcs = new TaskCompletionSource<object>();

    BackgroundSocketProcessor.AddSocket(socket, socketFinishedTcs); 

    await socketFinishedTcs.Task;
});
```
<span data-ttu-id="31413-159">如果您太快從動作方法返回，也可能發生 WebSocket 關閉的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="31413-159">The WebSocket closed exception can also happen if you return too soon from an action method.</span></span> <span data-ttu-id="31413-160">如果您在動作方法中接受通訊端，請先等候使用通訊端的程式碼完成，再從動作方法返回。</span><span class="sxs-lookup"><span data-stu-id="31413-160">If you accept a socket in an action method, wait for the code that uses the socket to complete before returning from the action method.</span></span>

<span data-ttu-id="31413-161">絕不要使用 `Task.Wait()`、`Task.Result` 或類似的封鎖呼叫等候通訊端完成，因為這會導致嚴重的執行緒處理問題。</span><span class="sxs-lookup"><span data-stu-id="31413-161">Never use `Task.Wait()`, `Task.Result`, or similar blocking calls to wait for the socket to complete, as that can cause serious threading issues.</span></span> <span data-ttu-id="31413-162">一律使用 `await`。</span><span class="sxs-lookup"><span data-stu-id="31413-162">Always use `await`.</span></span>

## <a name="send-and-receive-messages"></a><span data-ttu-id="31413-163">傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="31413-163">Send and receive messages</span></span>

<span data-ttu-id="31413-164">`AcceptWebSocketAsync` 方法可將 TCP 連線升級為 WebSocket 連線，並提供 [WebSocket](/dotnet/core/api/system.net.websockets.websocket) 物件。</span><span class="sxs-lookup"><span data-stu-id="31413-164">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="31413-165">請使用 `WebSocket` 物件來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="31413-165">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="31413-166">稍早所示接受 WebSocket 要求的程式碼會將 `WebSocket` 物件傳遞給 `Echo` 方法。</span><span class="sxs-lookup"><span data-stu-id="31413-166">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="31413-167">程式碼會收到一則訊息，並立即傳送回相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="31413-167">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="31413-168">在用戶端關閉連線之前，訊息會在迴圈中傳送和接收：</span><span class="sxs-lookup"><span data-stu-id="31413-168">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

<span data-ttu-id="31413-169">如果在開始此迴圈之前接受 WebSocket 連線，中介軟體管線就會結束。</span><span class="sxs-lookup"><span data-stu-id="31413-169">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="31413-170">在關閉通訊端後，管線會回溯。</span><span class="sxs-lookup"><span data-stu-id="31413-170">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="31413-171">也就是說，在接受 WebSocket 後，要求就會在管線中停止向前移動。</span><span class="sxs-lookup"><span data-stu-id="31413-171">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="31413-172">當迴圈完成且通訊端關閉時，要求會繼續備份管線。</span><span class="sxs-lookup"><span data-stu-id="31413-172">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="handle-client-disconnects"></a><span data-ttu-id="31413-173">處理用戶端中斷連線問題</span><span class="sxs-lookup"><span data-stu-id="31413-173">Handle client disconnects</span></span>

<span data-ttu-id="31413-174">當用戶端由於失去連線而中斷連線時，不會自動通知伺服器。</span><span class="sxs-lookup"><span data-stu-id="31413-174">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="31413-175">只有當用戶端傳送通知時，伺服器才會收到連線中斷訊息，而在網際網路連線中斷的情況下無法傳送通知。</span><span class="sxs-lookup"><span data-stu-id="31413-175">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="31413-176">若想要在發生此情況時採取一些動作，請設定逾時，在指定的時間內未從用戶端收到通知之後觸發逾時。</span><span class="sxs-lookup"><span data-stu-id="31413-176">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="31413-177">若用戶端並非總是會傳送訊息，而且您不想讓系統只是因為連線閒置而觸發逾時，請讓用戶端使用計時器每隔 X 秒傳送一次 Ping 訊息。</span><span class="sxs-lookup"><span data-stu-id="31413-177">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="31413-178">在伺服器上，若訊息未在收到前一個訊息後的 2\*X 秒內抵達，則中止連線並回報用戶端已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="31413-178">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="31413-179">等候預期時間間隔兩倍的時間，為網路延遲保留足夠的額外時間，看看能不能收到耽誤的 Ping 訊息。</span><span class="sxs-lookup"><span data-stu-id="31413-179">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="31413-180">WebSocket 來源限制</span><span class="sxs-lookup"><span data-stu-id="31413-180">WebSocket origin restriction</span></span>

<span data-ttu-id="31413-181">CORS 所提供的保護不套用至 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="31413-181">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="31413-182">瀏覽器**不**會：</span><span class="sxs-lookup"><span data-stu-id="31413-182">Browsers do **not**:</span></span>

* <span data-ttu-id="31413-183">執行 CORS 的事前要求。</span><span class="sxs-lookup"><span data-stu-id="31413-183">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="31413-184">進行 WebSocket 要求時，採用 `Access-Control` 標頭中所指定的限制。</span><span class="sxs-lookup"><span data-stu-id="31413-184">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="31413-185">不過，瀏覽器會在發出 WebSocket 要求時，傳送 `Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="31413-185">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="31413-186">應設定應用程式驗證這些標頭，以確保只允許來自預期來源的 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="31413-186">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="31413-187">若您在 "https://server.com" 上裝載伺服器，且在 "https://client.com" 上裝載用戶端，請將 "https://client.com" 新增至 `AllowedOrigins` 清單中，以讓 WebSockets 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="31413-187">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="31413-188">因為 `Origin` 由用戶端控制，所以和 `Referer` 標頭一樣可能受到偽造。</span><span class="sxs-lookup"><span data-stu-id="31413-188">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="31413-189">**請勿**使用這些標頭作為驗證機制。</span><span class="sxs-lookup"><span data-stu-id="31413-189">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="31413-190">IIS/IIS Express 支援</span><span class="sxs-lookup"><span data-stu-id="31413-190">IIS/IIS Express support</span></span>

<span data-ttu-id="31413-191">搭配 IIS/IIS Express 8 或更新版本的 Windows Server 2012 或更新版本和 Windows 8 或更新版本具有 WebSocket 通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="31413-191">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="31413-192">使用 IIS Express 時一律會啟用 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="31413-192">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="31413-193">在 IIS 上啟用 WebSockets</span><span class="sxs-lookup"><span data-stu-id="31413-193">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="31413-194">若要在 Windows Server 2012 或更新版本中啟用 WebSocket 通訊協定的支援：</span><span class="sxs-lookup"><span data-stu-id="31413-194">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="31413-195">使用 IIS Express 時，不需要這些步驟</span><span class="sxs-lookup"><span data-stu-id="31413-195">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="31413-196">使用來自 [管理]  功能表的 [新增角色及功能]  精靈，或是 [伺服器管理員]  中的連結。</span><span class="sxs-lookup"><span data-stu-id="31413-196">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="31413-197">選取 [角色型或功能型安裝]  。</span><span class="sxs-lookup"><span data-stu-id="31413-197">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="31413-198">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="31413-198">Select **Next**.</span></span>
1. <span data-ttu-id="31413-199">選取適當的伺服器 (預設會選取本機伺服器)。</span><span class="sxs-lookup"><span data-stu-id="31413-199">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="31413-200">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="31413-200">Select **Next**.</span></span>
1. <span data-ttu-id="31413-201">展開 [角色]  樹狀目錄中的 [網頁伺服器 (IIS)]  ，展開 [網頁伺服器]  ，然後展開 [應用程式開發]  。</span><span class="sxs-lookup"><span data-stu-id="31413-201">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="31413-202">選取 [WebSocket 通訊協定]  。</span><span class="sxs-lookup"><span data-stu-id="31413-202">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="31413-203">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="31413-203">Select **Next**.</span></span>
1. <span data-ttu-id="31413-204">如果不需要額外的功能，請選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="31413-204">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="31413-205">選取 [安裝]  。</span><span class="sxs-lookup"><span data-stu-id="31413-205">Select **Install**.</span></span>
1. <span data-ttu-id="31413-206">當安裝完成時，選取 [關閉]  來結束精靈。</span><span class="sxs-lookup"><span data-stu-id="31413-206">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="31413-207">若要在 Windows 8 或更新版本中啟用 WebSocket 通訊協定的支援：</span><span class="sxs-lookup"><span data-stu-id="31413-207">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="31413-208">使用 IIS Express 時，不需要這些步驟</span><span class="sxs-lookup"><span data-stu-id="31413-208">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="31413-209">瀏覽至控制台   > [程式]   > [程式和功能]   > [開啟或關閉 Windows 功能]  (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="31413-209">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="31413-210">開啟下列節點：[Internet Information Services]   > [全球資訊網服務]   > [應用程式開發功能]  。</span><span class="sxs-lookup"><span data-stu-id="31413-210">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="31413-211">選取 [WebSocket 通訊協定]  功能。</span><span class="sxs-lookup"><span data-stu-id="31413-211">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="31413-212">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="31413-212">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="31413-213">在 Node.js 上使用 socket.io 時停用 WebSocket</span><span class="sxs-lookup"><span data-stu-id="31413-213">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="31413-214">如果在 [Node.js](https://nodejs.org/) 的 [socket.io](https://socket.io/) 中使用 WebSocket 支援，請在 *web.config* 或 *applicationHost.config* 中使用 `webSocket`　項目，以停用預設的 IIS WebSocket 模組。如果未執行此步驟，IIS WebSocket 模組會嘗試處理 WebSocket 通訊，而非 Node.js 和應用程式。</span><span class="sxs-lookup"><span data-stu-id="31413-214">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="sample-app"></a><span data-ttu-id="31413-215">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="31413-215">Sample app</span></span>

<span data-ttu-id="31413-216">本文附帶的[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples)是回應應用程式。</span><span class="sxs-lookup"><span data-stu-id="31413-216">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="31413-217">其具有一個進行 WebSocket 連線的網頁，而伺服器會將其接收的任何訊息重新傳送回用戶端。</span><span class="sxs-lookup"><span data-stu-id="31413-217">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="31413-218">請從命令提示字元執行應用程式 (它未設定為從 Visual Studio 搭配 IIS Express 執行)，並巡覽至 http://localhost:5000。</span><span class="sxs-lookup"><span data-stu-id="31413-218">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="31413-219">網頁會在左上方顯示連線狀態：</span><span class="sxs-lookup"><span data-stu-id="31413-219">The web page shows the connection status in the upper left:</span></span>

![網頁的初始狀態](websockets/_static/start.png)

<span data-ttu-id="31413-221">選取 [連線]  將 WebSocket 要求傳送到顯示的 URL。</span><span class="sxs-lookup"><span data-stu-id="31413-221">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="31413-222">輸入測試訊息，然後選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="31413-222">Enter a test message and select **Send**.</span></span> <span data-ttu-id="31413-223">完成後，請選取 [關閉通訊端]  。</span><span class="sxs-lookup"><span data-stu-id="31413-223">When done, select **Close Socket**.</span></span> <span data-ttu-id="31413-224">[通訊記錄檔]  區段會報告每次進行的開啟、傳送和關閉動作。</span><span class="sxs-lookup"><span data-stu-id="31413-224">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![網頁的初始狀態](websockets/_static/end.png)

