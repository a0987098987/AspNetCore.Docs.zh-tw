---
title: "在 ASP.NET Core Websocket 支援"
author: tdykstra
description: "什麼是 Websocket 支援 ASP.NET Core 及如何使用它。"
keywords: ASP.NET Core WebSockets
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 8a6b5cc8ca8ac17f0e4c5b23f20013130cd472c8
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="eeb44-104">在 ASP.NET Core WebSockets 簡介</span><span class="sxs-lookup"><span data-stu-id="eeb44-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="eeb44-105">由[Tom Dykstra](https://github.com/tdykstra)和[Andrew Stanton 護士](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="eeb44-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="eeb44-106">本文說明如何開始使用 ASP.NET Core 中 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="eeb44-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="eeb44-107">[WebSocket](https://wikipedia.org/wiki/WebSocket)是啟用透過 TCP 連線的持續性的雙向通訊通道的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="eeb44-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="eeb44-108">用於應用程式，例如聊天、 股票行情即時看板、 遊戲，您想要在 web 應用程式的即時功能的任何位置。</span><span class="sxs-lookup"><span data-stu-id="eeb44-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="eeb44-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)。</span><span class="sxs-lookup"><span data-stu-id="eeb44-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span></span> <span data-ttu-id="eeb44-110">請參閱[接下來的步驟](#next-steps)節的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="eeb44-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="eeb44-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="eeb44-111">Prerequisites</span></span>

* <span data-ttu-id="eeb44-112">ASP.NET Core 1.1 （不執行於 1.0）</span><span class="sxs-lookup"><span data-stu-id="eeb44-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="eeb44-113">ASP.NET Core 執行的任何作業系統：</span><span class="sxs-lookup"><span data-stu-id="eeb44-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="eeb44-114">Windows 7 / Windows Server 2008 及更新版本</span><span class="sxs-lookup"><span data-stu-id="eeb44-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="eeb44-115">Linux</span><span class="sxs-lookup"><span data-stu-id="eeb44-115">Linux</span></span>
  * <span data-ttu-id="eeb44-116">MacOS</span><span class="sxs-lookup"><span data-stu-id="eeb44-116">macOS</span></span>

* <span data-ttu-id="eeb44-117">**例外狀況**： 如果您的應用程式會使用 IIS 時，Windows 上執行，或使用 WebListener，您必須使用：</span><span class="sxs-lookup"><span data-stu-id="eeb44-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="eeb44-118">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="eeb44-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="eeb44-119">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="eeb44-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="eeb44-120">必須在 IIS 中啟用 WebSocket</span><span class="sxs-lookup"><span data-stu-id="eeb44-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="eeb44-121">支援的瀏覽器，請參閱 http://caniuse.com/#feat=websockets。</span><span class="sxs-lookup"><span data-stu-id="eeb44-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="eeb44-122">使用時機</span><span class="sxs-lookup"><span data-stu-id="eeb44-122">When to use it</span></span>

<span data-ttu-id="eeb44-123">當您需要直接使用的通訊端連接時使用 Websocket。</span><span class="sxs-lookup"><span data-stu-id="eeb44-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="eeb44-124">例如，您可能需要最佳效能，即時遊戲。</span><span class="sxs-lookup"><span data-stu-id="eeb44-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="eeb44-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr)提供更豐富的應用程式模型進行即時的功能，但是它只會在執行 ASP.NET、 非 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="eeb44-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="eeb44-126">SignalR 的核心版本仍在開發。若要依照其進度，請參閱[SignalR Core 的 GitHub 儲存機制](https://github.com/aspnet/SignalR)。</span><span class="sxs-lookup"><span data-stu-id="eeb44-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="eeb44-127">如果您不想等候 SignalR Core，您現在可以使用 WebSockets 直接。</span><span class="sxs-lookup"><span data-stu-id="eeb44-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="eeb44-128">但您可能想要開發 SignalR 會提供功能，例如：</span><span class="sxs-lookup"><span data-stu-id="eeb44-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="eeb44-129">廣泛使用替代的傳輸方法，自動遞補的瀏覽器版本的支援。</span><span class="sxs-lookup"><span data-stu-id="eeb44-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="eeb44-130">自動重新連線時卸除連接。</span><span class="sxs-lookup"><span data-stu-id="eeb44-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="eeb44-131">支援用戶端呼叫方法，在伺服器上，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="eeb44-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="eeb44-132">縮放至多部伺服器的支援。</span><span class="sxs-lookup"><span data-stu-id="eeb44-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="eeb44-133">如何使用它</span><span class="sxs-lookup"><span data-stu-id="eeb44-133">How to use it</span></span>

* <span data-ttu-id="eeb44-134">安裝[Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/)封裝。</span><span class="sxs-lookup"><span data-stu-id="eeb44-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="eeb44-135">設定中介軟體。</span><span class="sxs-lookup"><span data-stu-id="eeb44-135">Configure the middleware.</span></span>
* <span data-ttu-id="eeb44-136">接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="eeb44-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="eeb44-137">傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="eeb44-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="eeb44-138">設定中介軟體</span><span class="sxs-lookup"><span data-stu-id="eeb44-138">Configure the middleware</span></span>

<span data-ttu-id="eeb44-139">加入的 Websocket 中介軟體中`Configure`方法`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="eeb44-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="eeb44-140">可以設定下列設定：</span><span class="sxs-lookup"><span data-stu-id="eeb44-140">The following settings can be configured:</span></span>

* <span data-ttu-id="eeb44-141">`KeepAliveInterval`-如何經常"ping"框架傳送到用戶端，以確保 proxy 保持開啟的連接。</span><span class="sxs-lookup"><span data-stu-id="eeb44-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="eeb44-142">`ReceiveBufferSize`的用來接收資料的緩衝區大小。</span><span class="sxs-lookup"><span data-stu-id="eeb44-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="eeb44-143">只有進階的使用者必須變更此設定，進行效能微調根據其資料的大小。</span><span class="sxs-lookup"><span data-stu-id="eeb44-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="eeb44-144">接受 WebSocket 要求</span><span class="sxs-lookup"><span data-stu-id="eeb44-144">Accept WebSocket requests</span></span>

<span data-ttu-id="eeb44-145">在要求的生命週期中某處更新 (稍後`Configure`方法或在 MVC 動作中，例如) 檢查它是否為 WebSocket 要求並接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="eeb44-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="eeb44-146">這個範例取自稍後在`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="eeb44-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="eeb44-147">WebSocket 要求無法傳入任何 URL，但此範例程式碼只會接受要求`/ws`。</span><span class="sxs-lookup"><span data-stu-id="eeb44-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="eeb44-148">傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="eeb44-148">Send and receive messages</span></span>

<span data-ttu-id="eeb44-149">`AcceptWebSocketAsync`方法升級 WebSocket 連線的 TCP 連線，並提供您[WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket)物件。</span><span class="sxs-lookup"><span data-stu-id="eeb44-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="eeb44-150">您可以使用 WebSocket 物件來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="eeb44-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="eeb44-151">在稍早所示，接受 WebSocket 要求的程式碼通過`WebSocket`物件`Echo`方法; 這裡的`Echo`方法。</span><span class="sxs-lookup"><span data-stu-id="eeb44-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="eeb44-152">程式碼會收到一則訊息，並立即傳送回相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="eeb44-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="eeb44-153">它會保持在迴圈，這麼做可在用戶端關閉連接。</span><span class="sxs-lookup"><span data-stu-id="eeb44-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="eeb44-154">當您開始此迴圈之前接受 WebSocket 時中, 介軟體管線就會結束。</span><span class="sxs-lookup"><span data-stu-id="eeb44-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="eeb44-155">在關閉通訊端，管線會回溯。</span><span class="sxs-lookup"><span data-stu-id="eeb44-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="eeb44-156">也就是在管線中向前移動，當您接受 WebSocket，因為它要求停駐點就是當您叫用 MVC 動作，例如。</span><span class="sxs-lookup"><span data-stu-id="eeb44-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="eeb44-157">但是，當您完成此迴圈，並關閉通訊端，要求會繼續備份管線。</span><span class="sxs-lookup"><span data-stu-id="eeb44-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eeb44-158">後續步驟</span><span class="sxs-lookup"><span data-stu-id="eeb44-158">Next steps</span></span>

<span data-ttu-id="eeb44-159">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)伴隨著此發行項是簡單的回應的應用程式。</span><span class="sxs-lookup"><span data-stu-id="eeb44-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="eeb44-160">它具有 WebSocket 連接的網頁，以及在伺服器重新傳送回用戶端接收之任何訊息。</span><span class="sxs-lookup"><span data-stu-id="eeb44-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="eeb44-161">執行 （它有未設定從 Visual Studio 和 IIS Express 上執行） 在命令提示字元並瀏覽至 http://localhost:5000/。</span><span class="sxs-lookup"><span data-stu-id="eeb44-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="eeb44-162">Web 網頁會顯示在左上方的連接狀態：</span><span class="sxs-lookup"><span data-stu-id="eeb44-162">The web page shows connection status at the upper left:</span></span>

![網頁的初始狀態](websockets/_static/start.png)

<span data-ttu-id="eeb44-164">選取**連接**將 WebSocket 要求傳送至顯示的 URL。</span><span class="sxs-lookup"><span data-stu-id="eeb44-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="eeb44-165">輸入測試訊息，並選取**傳送**。</span><span class="sxs-lookup"><span data-stu-id="eeb44-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="eeb44-166">完成，請選取**關閉通訊端**。</span><span class="sxs-lookup"><span data-stu-id="eeb44-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="eeb44-167">**通訊記錄**區段會報告每次開啟時，傳送，並關閉的動作，因為它。</span><span class="sxs-lookup"><span data-stu-id="eeb44-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![網頁的初始狀態](websockets/_static/end.png)
