---
title: ASP.NET Core 中的 WebSockets 支援
author: rick-anderson
description: 了解如何在 ASP.NET Core 中開始使用 WebSocket。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153679"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="c67d8-103">ASP.NET Core 中的 WebSockets 支援</span><span class="sxs-lookup"><span data-stu-id="c67d8-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="c67d8-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="c67d8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="c67d8-105">本文說明如何在 ASP.NET Core 中開始使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="c67d8-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="c67d8-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。</span><span class="sxs-lookup"><span data-stu-id="c67d8-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="c67d8-107">它用於受益於快速且即時通訊的應用程式，例如聊天、儀表板和遊戲應用程式。</span><span class="sxs-lookup"><span data-stu-id="c67d8-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="c67d8-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c67d8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c67d8-109">如需詳細資訊，請參閱[後續步驟](#next-steps)一節。</span><span class="sxs-lookup"><span data-stu-id="c67d8-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c67d8-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="c67d8-110">Prerequisites</span></span>

* <span data-ttu-id="c67d8-111">ASP.NET Core 1.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c67d8-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="c67d8-112">支援 ASP.NET Core 的任何作業系統：</span><span class="sxs-lookup"><span data-stu-id="c67d8-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="c67d8-113">Windows 7/Windows Server 2008 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c67d8-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="c67d8-114">Linux</span><span class="sxs-lookup"><span data-stu-id="c67d8-114">Linux</span></span>
  * <span data-ttu-id="c67d8-115">macOS</span><span class="sxs-lookup"><span data-stu-id="c67d8-115">macOS</span></span>
  
* <span data-ttu-id="c67d8-116">如果應用程式在 Windows 上與 IIS 搭配執行：</span><span class="sxs-lookup"><span data-stu-id="c67d8-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="c67d8-117">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c67d8-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="c67d8-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="c67d8-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="c67d8-119">必須在 IIS 中啟用 WebSocket (請參閱 [IIS/IIS Express 支援](#iisiis-express-support)一節。)</span><span class="sxs-lookup"><span data-stu-id="c67d8-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="c67d8-120">如果應用程式在 [HTTP.sys](xref:fundamentals/servers/httpsys) 上執行：</span><span class="sxs-lookup"><span data-stu-id="c67d8-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="c67d8-121">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c67d8-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="c67d8-122">如需支援的瀏覽器，請請參閱 https://caniuse.com/#feat=websockets。</span><span class="sxs-lookup"><span data-stu-id="c67d8-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="c67d8-123">WebSocket 使用時機</span><span class="sxs-lookup"><span data-stu-id="c67d8-123">When to use WebSockets</span></span>

<span data-ttu-id="c67d8-124">請使用 WebSocket 以直接使用通訊端連線。</span><span class="sxs-lookup"><span data-stu-id="c67d8-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="c67d8-125">例如，若要取得即時遊戲的可能最佳效能，請使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="c67d8-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="c67d8-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) 提供更豐富的應用程式模型來執行即時功能，但它只會在 ASP.NET 4.x 上執行，而不是在 ASP.NET Core 上執行。</span><span class="sxs-lookup"><span data-stu-id="c67d8-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="c67d8-127">SignalR 的 ASP.NET Core 版本已排定隨 ASP.NET Core 2.1 發行。</span><span class="sxs-lookup"><span data-stu-id="c67d8-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="c67d8-128">請參閱 [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288) (ASP.NET Core 2.1 高階規劃)。</span><span class="sxs-lookup"><span data-stu-id="c67d8-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="c67d8-129">在 SignalR Core 發行之前，可以使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="c67d8-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="c67d8-130">不過，開發人員必須提供和支援 SignalR 所提供的功能。</span><span class="sxs-lookup"><span data-stu-id="c67d8-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="c67d8-131">例如: </span><span class="sxs-lookup"><span data-stu-id="c67d8-131">For example:</span></span>

* <span data-ttu-id="c67d8-132">透過使用替代傳輸方法的自動後援，支援廣泛的瀏覽器版本。</span><span class="sxs-lookup"><span data-stu-id="c67d8-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="c67d8-133">連線中斷時自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="c67d8-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="c67d8-134">支援伺服器上的用戶端呼叫方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="c67d8-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="c67d8-135">支援擴展為多部伺服器。</span><span class="sxs-lookup"><span data-stu-id="c67d8-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="c67d8-136">如何使用</span><span class="sxs-lookup"><span data-stu-id="c67d8-136">How to use it</span></span>

* <span data-ttu-id="c67d8-137">安裝 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 套件。</span><span class="sxs-lookup"><span data-stu-id="c67d8-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="c67d8-138">設定中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c67d8-138">Configure the middleware.</span></span>
* <span data-ttu-id="c67d8-139">接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="c67d8-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="c67d8-140">傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="c67d8-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="c67d8-141">設定中介軟體</span><span class="sxs-lookup"><span data-stu-id="c67d8-141">Configure the middleware</span></span>

<span data-ttu-id="c67d8-142">在 `Startup` 類別的 `Configure` 方法中新增 WebSocket 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="c67d8-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="c67d8-143">您可以設定下列設定：</span><span class="sxs-lookup"><span data-stu-id="c67d8-143">The following settings can be configured:</span></span>

* <span data-ttu-id="c67d8-144">`KeepAliveInterval` - 要將 "ping" 框架傳送到用戶端，以確保 Proxy 保持連線開啟的頻率。</span><span class="sxs-lookup"><span data-stu-id="c67d8-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="c67d8-145">`ReceiveBufferSize` - 用來接收資料的緩衝區大小。</span><span class="sxs-lookup"><span data-stu-id="c67d8-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="c67d8-146">進階使用者可能需要變更此設定，以便根據資料的大小進行效能調整。</span><span class="sxs-lookup"><span data-stu-id="c67d8-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="c67d8-147">接受 WebSocket 要求</span><span class="sxs-lookup"><span data-stu-id="c67d8-147">Accept WebSocket requests</span></span>

<span data-ttu-id="c67d8-148">在要求生命週期的後半部某處 (例如，在 `Configure` 方法或 MVC 動作的後半部)，檢查它是否為 WebSocket 要求並接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="c67d8-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="c67d8-149">下列範例取自 `Configure` 方法的後半部：</span><span class="sxs-lookup"><span data-stu-id="c67d8-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="c67d8-150">WebSocket 要求可以傳入任何 URL，但此範例程式碼只接受 `/ws` 的要求。</span><span class="sxs-lookup"><span data-stu-id="c67d8-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="c67d8-151">傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="c67d8-151">Send and receive messages</span></span>

<span data-ttu-id="c67d8-152">`AcceptWebSocketAsync` 方法可將 TCP 連線升級為 WebSocket 連線，並提供 [WebSocket](/dotnet/core/api/system.net.websockets.websocket) 物件。</span><span class="sxs-lookup"><span data-stu-id="c67d8-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="c67d8-153">請使用 `WebSocket` 物件來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="c67d8-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="c67d8-154">稍早所示接受 WebSocket 要求的程式碼會將 `WebSocket` 物件傳遞給 `Echo` 方法。</span><span class="sxs-lookup"><span data-stu-id="c67d8-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="c67d8-155">程式碼會收到一則訊息，並立即傳送回相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="c67d8-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="c67d8-156">在用戶端關閉連線之前，訊息會在迴圈中傳送和接收：</span><span class="sxs-lookup"><span data-stu-id="c67d8-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="c67d8-157">如果在開始此迴圈之前接受 WebSocket 連線，中介軟體管線就會結束。</span><span class="sxs-lookup"><span data-stu-id="c67d8-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="c67d8-158">在關閉通訊端後，管線會回溯。</span><span class="sxs-lookup"><span data-stu-id="c67d8-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="c67d8-159">也就是說，在接受 WebSocket 後，要求就會在管線中停止向前移動。</span><span class="sxs-lookup"><span data-stu-id="c67d8-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="c67d8-160">當迴圈完成且通訊端關閉時，要求會繼續備份管線。</span><span class="sxs-lookup"><span data-stu-id="c67d8-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="c67d8-161">IIS/IIS Express 支援</span><span class="sxs-lookup"><span data-stu-id="c67d8-161">IIS/IIS Express support</span></span>

<span data-ttu-id="c67d8-162">搭配 IIS/IIS Express 8 或更新版本的 Windows Server 2012 或更新版本和 Windows 8 或更新版本具有 WebSocket 通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="c67d8-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="c67d8-163">若要在 Windows Server 2012 或更新版本中啟用 WebSocket 通訊協定的支援：</span><span class="sxs-lookup"><span data-stu-id="c67d8-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="c67d8-164">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="c67d8-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="c67d8-165">選取 [角色型或功能型安裝]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="c67d8-166">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-166">Select **Next**.</span></span>
1. <span data-ttu-id="c67d8-167">選取適當的伺服器 (預設會選取本機伺服器)。</span><span class="sxs-lookup"><span data-stu-id="c67d8-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="c67d8-168">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-168">Select **Next**.</span></span>
1. <span data-ttu-id="c67d8-169">展開 [角色] 樹狀目錄中的 [網頁伺服器 (IIS)]，展開 [網頁伺服器]，然後展開 [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="c67d8-170">選取 [WebSocket 通訊協定]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="c67d8-171">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-171">Select **Next**.</span></span>
1. <span data-ttu-id="c67d8-172">如果不需要額外的功能，請選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="c67d8-173">選取 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="c67d8-173">Select **Install**.</span></span>
1. <span data-ttu-id="c67d8-174">當安裝完成時，選取 [關閉] 來結束精靈。</span><span class="sxs-lookup"><span data-stu-id="c67d8-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="c67d8-175">若要在 Windows 8 或更新版本中啟用 WebSocket 通訊協定的支援：</span><span class="sxs-lookup"><span data-stu-id="c67d8-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="c67d8-176">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="c67d8-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="c67d8-177">開啟下列節點：[Internet Information Services] > [全球資訊網服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="c67d8-178">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="c67d8-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="c67d8-179">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-179">Select **OK**.</span></span>

<span data-ttu-id="c67d8-180">**在 node.js 上使用 socket.io 時停用 WebSocket**</span><span class="sxs-lookup"><span data-stu-id="c67d8-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="c67d8-181">如果在 [Node.js](https://nodejs.org/) 的 [socket.io](https://socket.io/) 中使用 WebSocket 支援，請在 *web.config* 或 *applicationHost.config* 中使用 `webSocket`　項目，以停用預設的 IIS WebSocket 模組。如果未執行此步驟，IIS WebSocket 模組會嘗試處理 WebSocket 通訊，而非 Node.js 和應用程式。</span><span class="sxs-lookup"><span data-stu-id="c67d8-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="c67d8-182">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c67d8-182">Next steps</span></span>

<span data-ttu-id="c67d8-183">本文附帶的[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)是回應應用程式。</span><span class="sxs-lookup"><span data-stu-id="c67d8-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="c67d8-184">其具有一個進行 WebSocket 連線的網頁，而伺服器會將其接收的任何訊息重新傳送回用戶端。</span><span class="sxs-lookup"><span data-stu-id="c67d8-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="c67d8-185">請從命令提示字元執行應用程式 (它未設定為從 Visual Studio 搭配 IIS Express 執行)，並巡覽至 http://localhost:5000。</span><span class="sxs-lookup"><span data-stu-id="c67d8-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="c67d8-186">網頁會在左上方顯示連線狀態：</span><span class="sxs-lookup"><span data-stu-id="c67d8-186">The web page shows the connection status in the upper left:</span></span>

![網頁的初始狀態](websockets/_static/start.png)

<span data-ttu-id="c67d8-188">選取 [連線] 將 WebSocket 要求傳送到顯示的 URL。</span><span class="sxs-lookup"><span data-stu-id="c67d8-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="c67d8-189">輸入測試訊息，然後選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="c67d8-190">完成後，請選取 [關閉通訊端]。</span><span class="sxs-lookup"><span data-stu-id="c67d8-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="c67d8-191">[通訊記錄檔] 區段會報告每次進行的開啟、傳送和關閉動作。</span><span class="sxs-lookup"><span data-stu-id="c67d8-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![網頁的初始狀態](websockets/_static/end.png)
