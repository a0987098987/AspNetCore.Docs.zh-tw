---
title: "ASP.NET Core 中的 WebSockets 支援"
author: tdykstra
description: "了解如何在 ASP.NET Core 中開始使用 WebSocket。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="05868-103">ASP.NET Core 中的 WebSocket 簡介</span><span class="sxs-lookup"><span data-stu-id="05868-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="05868-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="05868-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="05868-105">本文說明如何在 ASP.NET Core 中開始使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="05868-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="05868-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。</span><span class="sxs-lookup"><span data-stu-id="05868-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="05868-107">其可用於像是聊天、股票行情、遊戲等應用程式，以及您希望在 Web 應用程式中使用即時功能的任何位置。</span><span class="sxs-lookup"><span data-stu-id="05868-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="05868-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="05868-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="05868-109">如需詳細資訊，請參閱[後續步驟](#next-steps)一節。</span><span class="sxs-lookup"><span data-stu-id="05868-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="05868-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="05868-110">Prerequisites</span></span>

* <span data-ttu-id="05868-111">ASP.NET Core 1.1 (請勿在 1.0 上執行)</span><span class="sxs-lookup"><span data-stu-id="05868-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="05868-112">執行 ASP.NET Core 的任何作業系統：</span><span class="sxs-lookup"><span data-stu-id="05868-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="05868-113">Windows 7 / Windows Server 2008 和更新版本</span><span class="sxs-lookup"><span data-stu-id="05868-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="05868-114">Linux</span><span class="sxs-lookup"><span data-stu-id="05868-114">Linux</span></span>
  * <span data-ttu-id="05868-115">macOS</span><span class="sxs-lookup"><span data-stu-id="05868-115">macOS</span></span>

* <span data-ttu-id="05868-116">**例外狀況**：如果在 Windows 上搭配 IIS 或搭配 WebListener 執行應用程式，您必須使用：</span><span class="sxs-lookup"><span data-stu-id="05868-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="05868-117">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="05868-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="05868-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="05868-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="05868-119">必須在 IIS 中啟用 WebSocket</span><span class="sxs-lookup"><span data-stu-id="05868-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="05868-120">如需支援的瀏覽器，請參閱 http://caniuse.com/#feat=websockets。</span><span class="sxs-lookup"><span data-stu-id="05868-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="05868-121">使用時機</span><span class="sxs-lookup"><span data-stu-id="05868-121">When to use it</span></span>

<span data-ttu-id="05868-122">當您必須直接使用通訊端連線時，請使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="05868-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="05868-123">例如，您可能需要針對即時遊戲取得最佳效能。</span><span class="sxs-lookup"><span data-stu-id="05868-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="05868-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) 提供更豐富的應用程式模型來執行即時功能，但它只會在 ASP.NET 上執行，而不是在 ASP.NET Core 上執行。</span><span class="sxs-lookup"><span data-stu-id="05868-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="05868-125">SignalR Core 版本仍在開發中；若要追蹤其進度，請參閱 [SignalR Core 的 GitHub 存放庫](https://github.com/aspnet/SignalR)。</span><span class="sxs-lookup"><span data-stu-id="05868-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="05868-126">如果您不想等待 SignalR Core，現在可以直接使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="05868-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="05868-127">但是您可能需要開發 SignalR 提供的功能，例如：</span><span class="sxs-lookup"><span data-stu-id="05868-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="05868-128">透過使用替代傳輸方法的自動後援，支援廣泛的瀏覽器版本。</span><span class="sxs-lookup"><span data-stu-id="05868-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="05868-129">連線中斷時自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="05868-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="05868-130">支援伺服器上的用戶端呼叫方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="05868-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="05868-131">支援擴展為多部伺服器。</span><span class="sxs-lookup"><span data-stu-id="05868-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="05868-132">如何使用</span><span class="sxs-lookup"><span data-stu-id="05868-132">How to use it</span></span>

* <span data-ttu-id="05868-133">安裝 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 套件。</span><span class="sxs-lookup"><span data-stu-id="05868-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="05868-134">設定中介軟體。</span><span class="sxs-lookup"><span data-stu-id="05868-134">Configure the middleware.</span></span>
* <span data-ttu-id="05868-135">接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="05868-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="05868-136">傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="05868-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="05868-137">設定中介軟體</span><span class="sxs-lookup"><span data-stu-id="05868-137">Configure the middleware</span></span>

<span data-ttu-id="05868-138">在 `Startup` 類別的 `Configure` 方法中新增 WebSocket 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="05868-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="05868-139">您可以設定下列設定：</span><span class="sxs-lookup"><span data-stu-id="05868-139">The following settings can be configured:</span></span>

* <span data-ttu-id="05868-140">`KeepAliveInterval` - 要將 "ping" 框架傳送到用戶端，以確保 Proxy 保持連線開啟的頻率。</span><span class="sxs-lookup"><span data-stu-id="05868-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="05868-141">`ReceiveBufferSize` - 用來接收資料的緩衝區大小。</span><span class="sxs-lookup"><span data-stu-id="05868-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="05868-142">只有進階使用者才必須變更此設定，以便根據其資料的大小進行效能調整。</span><span class="sxs-lookup"><span data-stu-id="05868-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="05868-143">接受 WebSocket 要求</span><span class="sxs-lookup"><span data-stu-id="05868-143">Accept WebSocket requests</span></span>

<span data-ttu-id="05868-144">在要求生命週期的後半部某處 (例如，在 `Configure` 方法或 MVC 動作的後半部)，檢查它是否為 WebSocket 要求並接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="05868-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="05868-145">這個範例取自 `Configure` 方法的後半部。</span><span class="sxs-lookup"><span data-stu-id="05868-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="05868-146">WebSocket 要求可以傳入任何 URL，但此範例程式碼只接受 `/ws` 的要求。</span><span class="sxs-lookup"><span data-stu-id="05868-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="05868-147">傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="05868-147">Send and receive messages</span></span>

<span data-ttu-id="05868-148">`AcceptWebSocketAsync` 方法可將 TCP 連線升級為 WebSocket 連線，並提供您 [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) 物件。</span><span class="sxs-lookup"><span data-stu-id="05868-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="05868-149">請使用 WebSocket 物件來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="05868-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="05868-150">稍早所示接受 WebSocket 要求的程式碼會將 `WebSocket` 物件傳遞給 `Echo` 方法；以下是 `Echo` 方法。</span><span class="sxs-lookup"><span data-stu-id="05868-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="05868-151">程式碼會收到一則訊息，並立即傳送回相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="05868-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="05868-152">它會保留在執行該動作的迴圈中，直到用戶端關閉連線為止。</span><span class="sxs-lookup"><span data-stu-id="05868-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="05868-153">如果您在開始此迴圈之前接受 WebSocket，中介軟體管線就會結束。</span><span class="sxs-lookup"><span data-stu-id="05868-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="05868-154">在關閉通訊端後，管線會回溯。</span><span class="sxs-lookup"><span data-stu-id="05868-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="05868-155">也就是說，當您接受 WebSocket 時，要求會在管線中停止向前移動；比方說，就跟當您叫用 MVC 動作時一樣。</span><span class="sxs-lookup"><span data-stu-id="05868-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="05868-156">但是，在您完成此迴圈並關閉通訊端時，要求會繼續備份管線。</span><span class="sxs-lookup"><span data-stu-id="05868-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05868-157">後續步驟</span><span class="sxs-lookup"><span data-stu-id="05868-157">Next steps</span></span>

<span data-ttu-id="05868-158">本文附帶的[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)是簡單的回應應用程式。</span><span class="sxs-lookup"><span data-stu-id="05868-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="05868-159">其具有一個進行 WebSocket 連線的網頁，而伺服器只會將其接收的任何訊息重新傳送回用戶端。</span><span class="sxs-lookup"><span data-stu-id="05868-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="05868-160">請從命令提示字元執行它 (它未設定為從 Visual Studio 搭配 IIS Express 執行)，並巡覽至 http://localhost:5000。</span><span class="sxs-lookup"><span data-stu-id="05868-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="05868-161">網頁會在左上方顯示連線狀態：</span><span class="sxs-lookup"><span data-stu-id="05868-161">The web page shows connection status at the upper left:</span></span>

![網頁的初始狀態](websockets/_static/start.png)

<span data-ttu-id="05868-163">選取 [連線] 將 WebSocket 要求傳送到顯示的 URL。</span><span class="sxs-lookup"><span data-stu-id="05868-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="05868-164">輸入測試訊息，然後選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="05868-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="05868-165">完成後，請選取 [關閉通訊端]。</span><span class="sxs-lookup"><span data-stu-id="05868-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="05868-166">[通訊記錄檔] 區段會報告每次進行的開啟、傳送和關閉動作。</span><span class="sxs-lookup"><span data-stu-id="05868-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![網頁的初始狀態](websockets/_static/end.png)
