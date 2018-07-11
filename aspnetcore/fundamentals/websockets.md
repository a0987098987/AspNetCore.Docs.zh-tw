---
title: ASP.NET Core 中的 WebSockets 支援
author: rick-anderson
description: 了解如何在 ASP.NET Core 中開始使用 WebSocket。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: a9fe13ef7895ea3ab43257dbbaf4521f883c0804
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433983"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="1b25a-103">ASP.NET Core 中的 WebSockets 支援</span><span class="sxs-lookup"><span data-stu-id="1b25a-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="1b25a-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="1b25a-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="1b25a-105">本文說明如何在 ASP.NET Core 中開始使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="1b25a-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="1b25a-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。</span><span class="sxs-lookup"><span data-stu-id="1b25a-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="1b25a-107">它用於受益於快速且即時通訊的應用程式，例如聊天、儀表板和遊戲應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b25a-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="1b25a-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1b25a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="1b25a-109">如需詳細資訊，請參閱[後續步驟](#next-steps)一節。</span><span class="sxs-lookup"><span data-stu-id="1b25a-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b25a-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="1b25a-110">Prerequisites</span></span>

* <span data-ttu-id="1b25a-111">ASP.NET Core 1.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1b25a-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="1b25a-112">支援 ASP.NET Core 的任何作業系統：</span><span class="sxs-lookup"><span data-stu-id="1b25a-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="1b25a-113">Windows 7/Windows Server 2008 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1b25a-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="1b25a-114">Linux</span><span class="sxs-lookup"><span data-stu-id="1b25a-114">Linux</span></span>
  * <span data-ttu-id="1b25a-115">macOS</span><span class="sxs-lookup"><span data-stu-id="1b25a-115">macOS</span></span>
  
* <span data-ttu-id="1b25a-116">如果應用程式在 Windows 上與 IIS 搭配執行：</span><span class="sxs-lookup"><span data-stu-id="1b25a-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="1b25a-117">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1b25a-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="1b25a-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="1b25a-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="1b25a-119">必須在 IIS 中啟用 WebSocket (請參閱 [IIS/IIS Express 支援](#iisiis-express-support)一節。)</span><span class="sxs-lookup"><span data-stu-id="1b25a-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="1b25a-120">如果應用程式在 [HTTP.sys](xref:fundamentals/servers/httpsys) 上執行：</span><span class="sxs-lookup"><span data-stu-id="1b25a-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="1b25a-121">Windows 8 / Windows Server 2012 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1b25a-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="1b25a-122">如需支援的瀏覽器，請請參閱 https://caniuse.com/#feat=websockets。</span><span class="sxs-lookup"><span data-stu-id="1b25a-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="1b25a-123">WebSocket 使用時機</span><span class="sxs-lookup"><span data-stu-id="1b25a-123">When to use WebSockets</span></span>

<span data-ttu-id="1b25a-124">請使用 WebSocket 以直接使用通訊端連線。</span><span class="sxs-lookup"><span data-stu-id="1b25a-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="1b25a-125">例如，若要取得即時遊戲的可能最佳效能，請使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="1b25a-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="1b25a-126">[ASP.NET Core SignalR](xref:signalr/introduction) 是程式庫，可簡化將即時 Web 功能新增至應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="1b25a-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="1b25a-127">它會盡可能使用 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="1b25a-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="1b25a-128">如何使用 WebSocket</span><span class="sxs-lookup"><span data-stu-id="1b25a-128">How to use WebSockets</span></span>

* <span data-ttu-id="1b25a-129">安裝 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 套件。</span><span class="sxs-lookup"><span data-stu-id="1b25a-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="1b25a-130">設定中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1b25a-130">Configure the middleware.</span></span>
* <span data-ttu-id="1b25a-131">接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="1b25a-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="1b25a-132">傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="1b25a-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="1b25a-133">設定中介軟體</span><span class="sxs-lookup"><span data-stu-id="1b25a-133">Configure the middleware</span></span>

<span data-ttu-id="1b25a-134">在 `Startup` 類別的 `Configure` 方法中新增 WebSocket 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="1b25a-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="1b25a-135">您可以設定下列設定：</span><span class="sxs-lookup"><span data-stu-id="1b25a-135">The following settings can be configured:</span></span>

* <span data-ttu-id="1b25a-136">`KeepAliveInterval` - 要將 "ping" 框架傳送到用戶端，以確保 Proxy 保持連線開啟的頻率。</span><span class="sxs-lookup"><span data-stu-id="1b25a-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="1b25a-137">`ReceiveBufferSize` - 用來接收資料的緩衝區大小。</span><span class="sxs-lookup"><span data-stu-id="1b25a-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="1b25a-138">進階使用者可能需要變更此設定，以便根據資料的大小進行效能調整。</span><span class="sxs-lookup"><span data-stu-id="1b25a-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="1b25a-139">接受 WebSocket 要求</span><span class="sxs-lookup"><span data-stu-id="1b25a-139">Accept WebSocket requests</span></span>

<span data-ttu-id="1b25a-140">在要求生命週期的後半部某處 (例如，在 `Configure` 方法或 MVC 動作的後半部)，檢查它是否為 WebSocket 要求並接受 WebSocket 要求。</span><span class="sxs-lookup"><span data-stu-id="1b25a-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="1b25a-141">下列範例取自 `Configure` 方法的後半部：</span><span class="sxs-lookup"><span data-stu-id="1b25a-141">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="1b25a-142">WebSocket 要求可以傳入任何 URL，但此範例程式碼只接受 `/ws` 的要求。</span><span class="sxs-lookup"><span data-stu-id="1b25a-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="1b25a-143">傳送和接收訊息</span><span class="sxs-lookup"><span data-stu-id="1b25a-143">Send and receive messages</span></span>

<span data-ttu-id="1b25a-144">`AcceptWebSocketAsync` 方法可將 TCP 連線升級為 WebSocket 連線，並提供 [WebSocket](/dotnet/core/api/system.net.websockets.websocket) 物件。</span><span class="sxs-lookup"><span data-stu-id="1b25a-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="1b25a-145">請使用 `WebSocket` 物件來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="1b25a-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="1b25a-146">稍早所示接受 WebSocket 要求的程式碼會將 `WebSocket` 物件傳遞給 `Echo` 方法。</span><span class="sxs-lookup"><span data-stu-id="1b25a-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="1b25a-147">程式碼會收到一則訊息，並立即傳送回相同的訊息。</span><span class="sxs-lookup"><span data-stu-id="1b25a-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="1b25a-148">在用戶端關閉連線之前，訊息會在迴圈中傳送和接收：</span><span class="sxs-lookup"><span data-stu-id="1b25a-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="1b25a-149">如果在開始此迴圈之前接受 WebSocket 連線，中介軟體管線就會結束。</span><span class="sxs-lookup"><span data-stu-id="1b25a-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="1b25a-150">在關閉通訊端後，管線會回溯。</span><span class="sxs-lookup"><span data-stu-id="1b25a-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="1b25a-151">也就是說，在接受 WebSocket 後，要求就會在管線中停止向前移動。</span><span class="sxs-lookup"><span data-stu-id="1b25a-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="1b25a-152">當迴圈完成且通訊端關閉時，要求會繼續備份管線。</span><span class="sxs-lookup"><span data-stu-id="1b25a-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="1b25a-153">IIS/IIS Express 支援</span><span class="sxs-lookup"><span data-stu-id="1b25a-153">IIS/IIS Express support</span></span>

<span data-ttu-id="1b25a-154">搭配 IIS/IIS Express 8 或更新版本的 Windows Server 2012 或更新版本和 Windows 8 或更新版本具有 WebSocket 通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="1b25a-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="1b25a-155">若要在 Windows Server 2012 或更新版本中啟用 WebSocket 通訊協定的支援：</span><span class="sxs-lookup"><span data-stu-id="1b25a-155">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="1b25a-156">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="1b25a-156">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="1b25a-157">選取 [角色型或功能型安裝]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-157">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="1b25a-158">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-158">Select **Next**.</span></span>
1. <span data-ttu-id="1b25a-159">選取適當的伺服器 (預設會選取本機伺服器)。</span><span class="sxs-lookup"><span data-stu-id="1b25a-159">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="1b25a-160">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-160">Select **Next**.</span></span>
1. <span data-ttu-id="1b25a-161">展開 [角色] 樹狀目錄中的 [網頁伺服器 (IIS)]，展開 [網頁伺服器]，然後展開 [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-161">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="1b25a-162">選取 [WebSocket 通訊協定]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-162">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="1b25a-163">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-163">Select **Next**.</span></span>
1. <span data-ttu-id="1b25a-164">如果不需要額外的功能，請選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-164">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="1b25a-165">選取 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="1b25a-165">Select **Install**.</span></span>
1. <span data-ttu-id="1b25a-166">當安裝完成時，選取 [關閉] 來結束精靈。</span><span class="sxs-lookup"><span data-stu-id="1b25a-166">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="1b25a-167">若要在 Windows 8 或更新版本中啟用 WebSocket 通訊協定的支援：</span><span class="sxs-lookup"><span data-stu-id="1b25a-167">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="1b25a-168">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="1b25a-168">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="1b25a-169">開啟下列節點：[Internet Information Services] > [全球資訊網服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-169">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="1b25a-170">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="1b25a-170">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="1b25a-171">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-171">Select **OK**.</span></span>

<span data-ttu-id="1b25a-172">**在 node.js 上使用 socket.io 時停用 WebSocket**</span><span class="sxs-lookup"><span data-stu-id="1b25a-172">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="1b25a-173">如果在 [Node.js](https://nodejs.org/) 的 [socket.io](https://socket.io/) 中使用 WebSocket 支援，請在 *web.config* 或 *applicationHost.config* 中使用 `webSocket`　項目，以停用預設的 IIS WebSocket 模組。如果未執行此步驟，IIS WebSocket 模組會嘗試處理 WebSocket 通訊，而非 Node.js 和應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b25a-173">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="1b25a-174">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1b25a-174">Next steps</span></span>

<span data-ttu-id="1b25a-175">本文附帶的[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)是回應應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b25a-175">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="1b25a-176">其具有一個進行 WebSocket 連線的網頁，而伺服器會將其接收的任何訊息重新傳送回用戶端。</span><span class="sxs-lookup"><span data-stu-id="1b25a-176">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="1b25a-177">請從命令提示字元執行應用程式 (它未設定為從 Visual Studio 搭配 IIS Express 執行)，並巡覽至 http://localhost:5000。</span><span class="sxs-lookup"><span data-stu-id="1b25a-177">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="1b25a-178">網頁會在左上方顯示連線狀態：</span><span class="sxs-lookup"><span data-stu-id="1b25a-178">The web page shows the connection status in the upper left:</span></span>

![網頁的初始狀態](websockets/_static/start.png)

<span data-ttu-id="1b25a-180">選取 [連線] 將 WebSocket 要求傳送到顯示的 URL。</span><span class="sxs-lookup"><span data-stu-id="1b25a-180">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="1b25a-181">輸入測試訊息，然後選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-181">Enter a test message and select **Send**.</span></span> <span data-ttu-id="1b25a-182">完成後，請選取 [關閉通訊端]。</span><span class="sxs-lookup"><span data-stu-id="1b25a-182">When done, select **Close Socket**.</span></span> <span data-ttu-id="1b25a-183">[通訊記錄檔] 區段會報告每次進行的開啟、傳送和關閉動作。</span><span class="sxs-lookup"><span data-stu-id="1b25a-183">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![網頁的初始狀態](websockets/_static/end.png)
