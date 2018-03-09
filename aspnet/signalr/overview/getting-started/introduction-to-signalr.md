---
uid: signalr/overview/getting-started/introduction-to-signalr
title: "SignalR 簡介 |Microsoft 文件"
author: pfletcher
description: "本文說明 SignalR 的是，以及它設計用來建立解決方案的部分。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5bb49c9c2405d232ba5e067d99f8879b3bc99361
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2018
---
<a name="introduction-to-signalr"></a><span data-ttu-id="23398-103">SignalR 的簡介</span><span class="sxs-lookup"><span data-stu-id="23398-103">Introduction to SignalR</span></span>
====================
<span data-ttu-id="23398-104">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="23398-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="23398-105">本文說明 SignalR 的是，以及它設計用來建立解決方案的部分。</span><span class="sxs-lookup"><span data-stu-id="23398-105">This article describes what SignalR is, and some of the solutions it was designed to create.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="23398-106">問題和註解</span><span class="sxs-lookup"><span data-stu-id="23398-106">Questions and comments</span></span>
> 
> <span data-ttu-id="23398-107">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="23398-107">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="23398-108">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)。</span><span class="sxs-lookup"><span data-stu-id="23398-108">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).</span></span>


## <a name="what-is-signalr"></a><span data-ttu-id="23398-109">SignalR 是什麼？</span><span class="sxs-lookup"><span data-stu-id="23398-109">What is SignalR?</span></span>

<span data-ttu-id="23398-110">ASP.NET SignalR 是 ASP.NET 開發人員的程式庫，可簡化將即時 web 功能新增至應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="23398-110">ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.</span></span> <span data-ttu-id="23398-111">即時 web 功能是能夠讓伺服器程式碼推入內容給連接的用戶端立即可用，而不是讓伺服器等候用戶端要求新的資料。</span><span class="sxs-lookup"><span data-stu-id="23398-111">Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.</span></span>

<span data-ttu-id="23398-112">SignalR 可用來將任何類型的 「 即時 」 web 功能新增至您的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="23398-112">SignalR can be used to add any sort of "real-time" web functionality to your ASP.NET application.</span></span> <span data-ttu-id="23398-113">雖然交談通常會使用做為範例中，您可以一大堆多。</span><span class="sxs-lookup"><span data-stu-id="23398-113">While chat is often used as an example, you can do a whole lot more.</span></span> <span data-ttu-id="23398-114">每當使用者重新整理網頁即可看到新的資料，或頁面實作[長輪詢](http://en.wikipedia.org/wiki/Push_technology#Long_polling)擷取新的資料，它是使用 SignalR 的候選。</span><span class="sxs-lookup"><span data-stu-id="23398-114">Any time a user refreshes a web page to see new data, or the page implements [long polling](http://en.wikipedia.org/wiki/Push_technology#Long_polling) to retrieve new data, it is a candidate for using SignalR.</span></span> <span data-ttu-id="23398-115">範例包括儀表板和監視應用程式、 共同作業應用程式 （例如同時編輯的文件），工作進度更新和即時的表單。</span><span class="sxs-lookup"><span data-stu-id="23398-115">Examples include dashboards and monitoring applications, collaborative applications (such as simultaneous editing of documents), job progress updates, and real-time forms.</span></span>

<span data-ttu-id="23398-116">SignalR 也可讓支援全新的 web 應用程式需要在伺服器上，從高頻率更新類型，例如即時遊戲。</span><span class="sxs-lookup"><span data-stu-id="23398-116">SignalR also enables completely new types of web applications that require high frequency updates from the server, for example, real-time gaming.</span></span>

<span data-ttu-id="23398-117">SignalR 提供一個簡單 API 來建立伺服器到用戶端的遠端程序呼叫 (RPC) 從伺服器端.NET 程式碼呼叫 JavaScript 函式，在用戶端瀏覽器 （和其他用戶端平台）。</span><span class="sxs-lookup"><span data-stu-id="23398-117">SignalR provides a simple API for creating server-to-client remote procedure calls (RPC) that call JavaScript functions in client browsers (and other client platforms) from server-side .NET code.</span></span> <span data-ttu-id="23398-118">SignalR 也包含連接管理 API （例如，連接和中斷連線事件），及群組的連線。</span><span class="sxs-lookup"><span data-stu-id="23398-118">SignalR also includes API for connection management (for instance, connect and disconnect events), and grouping connections.</span></span>

![與 SignalR 的叫用方法](introduction-to-signalr/_static/image1.png)

<span data-ttu-id="23398-120">SignalR 自動處理連接管理，並像聊天室的同時，讓您廣播給所有連接的用戶端的訊息。</span><span class="sxs-lookup"><span data-stu-id="23398-120">SignalR handles connection management automatically, and lets you broadcast messages to all connected clients simultaneously, like a chat room.</span></span> <span data-ttu-id="23398-121">您也可以傳送訊息給特定的用戶端。</span><span class="sxs-lookup"><span data-stu-id="23398-121">You can also send messages to specific clients.</span></span> <span data-ttu-id="23398-122">用戶端與伺服器之間的連線是永久性的不同於傳統的 HTTP 連線，也就是重新建立每個通訊的。</span><span class="sxs-lookup"><span data-stu-id="23398-122">The connection between the client and server is persistent, unlike a classic HTTP connection, which is re-established for each communication.</span></span>

<span data-ttu-id="23398-123">SignalR 支援 「 伺服器推入 」 功能，伺服端程式碼呼叫立即從網站上使用遠端程序呼叫 (RPC)，而不是常用的要求-回應模型瀏覽器中的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="23398-123">SignalR supports "server push" functionality, in which server code can call out to client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model common on the web today.</span></span>

<span data-ttu-id="23398-124">SignalR 應用程式可以向外延展至數千個用戶端使用服務匯流排、 SQL Server 或[Redis](http://redis.io)。</span><span class="sxs-lookup"><span data-stu-id="23398-124">SignalR applications can scale out to thousands of clients using Service Bus, SQL Server or [Redis](http://redis.io).</span></span>

<span data-ttu-id="23398-125">SignalR 是開放原始碼，可透過存取[GitHub](https://github.com/signalr)。</span><span class="sxs-lookup"><span data-stu-id="23398-125">SignalR is open-source, accessible through [GitHub](https://github.com/signalr).</span></span>

## <a name="signalr-and-websocket"></a><span data-ttu-id="23398-126">SignalR 和 WebSocket</span><span class="sxs-lookup"><span data-stu-id="23398-126">SignalR and WebSocket</span></span>

<span data-ttu-id="23398-127">SignalR 情況下，會使用新的 WebSocket 傳輸，並會回復為舊版傳輸在需要時。</span><span class="sxs-lookup"><span data-stu-id="23398-127">SignalR uses the new WebSocket transport where available, and falls back to older transports where necessary.</span></span> <span data-ttu-id="23398-128">雖然您當然可以撰寫您直接使用 WebSocket，請使用 SignalR 表示額外的功能，您需要實作大量會有已完成您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="23398-128">While you could certainly write your application using WebSocket directly, using SignalR means that a lot of the extra functionality you would need to implement will already have been done for you.</span></span> <span data-ttu-id="23398-129">最重要的是，這表示您可以撰寫您的應用程式，以利用 WebSocket 而不必擔心舊版的用戶端建立不同的程式碼路徑。</span><span class="sxs-lookup"><span data-stu-id="23398-129">Most importantly, this means that you can code your application to take advantage of WebSocket without having to worry about creating a separate code path for older clients.</span></span> <span data-ttu-id="23398-130">SignalR 也保護不受影響您不必擔心更新 WebSocket，因為 SignalR 會繼續更新為基礎的傳輸，支援的變更在 WebSocket 版本之間提供您的應用程式一致的介面。</span><span class="sxs-lookup"><span data-stu-id="23398-130">SignalR also shields you from having to worry about updates to WebSocket, since SignalR will continue to be updated to support changes in the underlying transport, providing your application a consistent interface across versions of WebSocket.</span></span>

<span data-ttu-id="23398-131">當然，您可以建立使用 WebSocket 單獨的方案，而 SignalR 提供的所有功能，您必須撰寫您自己，例如回到其他傳輸和修訂您的應用程式更新的 WebSocket 實作。</span><span class="sxs-lookup"><span data-stu-id="23398-131">While you could certainly create a solution using WebSocket alone, SignalR provides all of the functionality you would need to write yourself, such as fallback to other transports and revising your application for updates to WebSocket implementations.</span></span>

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a><span data-ttu-id="23398-132">傳輸和後援</span><span class="sxs-lookup"><span data-stu-id="23398-132">Transports and fallbacks</span></span>

<span data-ttu-id="23398-133">SignalR 是抽象的傳輸，才能執行用戶端與伺服器之間的即時工作的部分。</span><span class="sxs-lookup"><span data-stu-id="23398-133">SignalR is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="23398-134">SignalR 連線做為 HTTP，啟動，然後提升為 WebSocket 連接是否可用。</span><span class="sxs-lookup"><span data-stu-id="23398-134">A SignalR connection starts as HTTP, and is then promoted to a WebSocket connection if it is available.</span></span> <span data-ttu-id="23398-135">WebSocket 是 SignalR 的理想傳輸，因為它可讓伺服器記憶體最有效地使用延遲最低、 有最基本功能 （例如完整雙工用戶端和伺服器之間通訊），但它也具有最嚴格需求： WebSocket 要求的伺服器會使用 Windows Server 2012 或 Windows 8 和.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="23398-135">WebSocket is the ideal transport for SignalR, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: WebSocket requires the server to be using Windows Server 2012 or Windows 8, and .NET Framework 4.5.</span></span> <span data-ttu-id="23398-136">如果不符合這些需求，SignalR 會嘗試使用其他傳輸進行其連線。</span><span class="sxs-lookup"><span data-stu-id="23398-136">If these requirements are not met, SignalR will attempt to use other transports to make its connections.</span></span>

### <a name="html-5-transports"></a><span data-ttu-id="23398-137">HTML 5 傳輸</span><span class="sxs-lookup"><span data-stu-id="23398-137">HTML 5 transports</span></span>

<span data-ttu-id="23398-138">這些傳輸的支援取決於[HTML 5](http://en.wikipedia.org/wiki/HTML5)。</span><span class="sxs-lookup"><span data-stu-id="23398-138">These transports depend on support for [HTML 5](http://en.wikipedia.org/wiki/HTML5).</span></span> <span data-ttu-id="23398-139">如果用戶端瀏覽器不支援 HTML 5 標準，將會使用較舊的傳輸。</span><span class="sxs-lookup"><span data-stu-id="23398-139">If the client browser does not support the HTML 5 standard, older transports will be used.</span></span>

- <span data-ttu-id="23398-140">**WebSocket** (如果它們可以支援 Websocket 伺服器和瀏覽器表示)。</span><span class="sxs-lookup"><span data-stu-id="23398-140">**WebSocket** (if the both the server and browser indicate they can support Websocket).</span></span> <span data-ttu-id="23398-141">WebSocket 是只會建立，則為 true 的持續性的雙向連線用戶端與伺服器之間的傳輸。</span><span class="sxs-lookup"><span data-stu-id="23398-141">WebSocket is the only transport that establishes a true persistent, two-way connection between client and server.</span></span> <span data-ttu-id="23398-142">不過，WebSocket 也具有最嚴格的需求。它只能在 Microsoft Internet Explorer 及 Google Chrome、 Mozilla Firefox 的最新版本中完全支援，並在其他瀏覽器，例如 Opera 與 Safari 中只有部分實作。</span><span class="sxs-lookup"><span data-stu-id="23398-142">However, WebSocket also has the most stringent requirements; it is fully supported only in the latest versions of Microsoft Internet Explorer, Google Chrome, and Mozilla Firefox, and only has a partial implementation in other browsers such as Opera and Safari.</span></span>
- <span data-ttu-id="23398-143">**伺服器傳送事件**，也稱為 EventSource （如果瀏覽器支援伺服器傳送事件，這基本上是 Internet Explorer 以外的所有瀏覽器）。</span><span class="sxs-lookup"><span data-stu-id="23398-143">**Server Sent Events**, also known as EventSource (if the browser supports Server Sent Events, which is basically all browsers except Internet Explorer.)</span></span>

### <a name="comet-transports"></a><span data-ttu-id="23398-144">Comet 傳輸</span><span class="sxs-lookup"><span data-stu-id="23398-144">Comet transports</span></span>

<span data-ttu-id="23398-145">會根據下列傳輸[Comet](http://en.wikipedia.org/wiki/Comet_(programming))瀏覽器或其他用戶端維持長時間保留 HTTP 要求中，伺服器可以用來將資料推送至沒有用戶端的用戶端特別的 web 應用程式模型要求的方式。</span><span class="sxs-lookup"><span data-stu-id="23398-145">The following transports are based on the [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web application model, in which a browser or other client maintains a long-held HTTP request, which the server can use to push data to the client without the client specifically requesting it.</span></span>

- <span data-ttu-id="23398-146">**永久框架**（適用於僅限 Internet Explorer)。</span><span class="sxs-lookup"><span data-stu-id="23398-146">**Forever Frame** (for Internet Explorer only).</span></span> <span data-ttu-id="23398-147">永久框架會建立隱藏的 IFrame 的未完成的伺服器上的端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="23398-147">Forever Frame creates a hidden IFrame which makes a request to an endpoint on the server that does not complete.</span></span> <span data-ttu-id="23398-148">伺服器再持續傳送指令碼至用戶端立即執行，此用戶端從伺服器的單向即時連線。</span><span class="sxs-lookup"><span data-stu-id="23398-148">The server then continually sends script to the client which is immediately executed, providing a one-way realtime connection from server to client.</span></span> <span data-ttu-id="23398-149">用戶端與伺服器的連接使用個別連接從伺服器到用戶端，並像標準的 HTTP 要求中，針對每個需要傳送的資料片段建立新的連接。</span><span class="sxs-lookup"><span data-stu-id="23398-149">The connection from client to server uses a separate connection from the server to client connection, and like a standard HTTP request, a new connection is created for each piece of data that needs to be sent.</span></span>
- <span data-ttu-id="23398-150">**Ajax 長期輪詢**。</span><span class="sxs-lookup"><span data-stu-id="23398-150">**Ajax long polling**.</span></span> <span data-ttu-id="23398-151">長期輪詢不會建立持續連線，但是會改為輪詢伺服器的要求，直到伺服器的回應，此時會關閉連接，並立即要求新的連接會保持開啟。</span><span class="sxs-lookup"><span data-stu-id="23398-151">Long polling does not create a persistent connection, but instead polls the server with a request that stays open until the server responds, at which point the connection closes, and a new connection is requested immediately.</span></span> <span data-ttu-id="23398-152">重設連接時，這樣可能會導致一些延遲。</span><span class="sxs-lookup"><span data-stu-id="23398-152">This may introduce some latency while the connection resets.</span></span>

<span data-ttu-id="23398-153">如需有關在哪些組態支援何種傳輸的詳細資訊，請參閱[支援的平台](supported-platforms.md)。</span><span class="sxs-lookup"><span data-stu-id="23398-153">For more information on what transports are supported under which configurations, see [Supported Platforms](supported-platforms.md).</span></span>

### <a name="transport-selection-process"></a><span data-ttu-id="23398-154">傳輸選取程序</span><span class="sxs-lookup"><span data-stu-id="23398-154">Transport selection process</span></span>

<span data-ttu-id="23398-155">下列清單顯示 SignalR 用來決定要使用的傳輸的步驟。</span><span class="sxs-lookup"><span data-stu-id="23398-155">The following list shows the steps that SignalR uses to decide which transport to use.</span></span>

1. <span data-ttu-id="23398-156">如果瀏覽器是 Internet Explorer 8 或更早版本，會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="23398-156">If the browser is Internet Explorer 8 or earlier, Long Polling is used.</span></span>
2. <span data-ttu-id="23398-157">如果設定 JSONP (也就是`jsonp`參數設定為`true`連接上啟動時)，長輪詢用。</span><span class="sxs-lookup"><span data-stu-id="23398-157">If JSONP is configured (that is, the `jsonp` parameter is set to `true` when the connection is started), Long Polling is used.</span></span>
3. <span data-ttu-id="23398-158">如果建立跨定義域連線時 （也就是如果 SignalR 端點不是在與裝載網頁位於相同網域中），然後 WebSocket 如果就會使用符合下列準則：</span><span class="sxs-lookup"><span data-stu-id="23398-158">If a cross-domain connection is being made (that is, if the SignalR endpoint is not in the same domain as the hosting page), then WebSocket will be used if the following criteria are met:</span></span>

    - <span data-ttu-id="23398-159">用戶端支援 CORS （跨原始資源共用）。</span><span class="sxs-lookup"><span data-stu-id="23398-159">The client supports CORS (Cross-Origin Resource Sharing).</span></span> <span data-ttu-id="23398-160">在用戶端支援 CORS 的詳細資訊，請參閱[在 caniuse.com CORS](http://www.caniuse.com/CORS)。</span><span class="sxs-lookup"><span data-stu-id="23398-160">For details on which clients support CORS, see [CORS at caniuse.com](http://www.caniuse.com/CORS).</span></span>
    - <span data-ttu-id="23398-161">用戶端支援的 WebSocket</span><span class="sxs-lookup"><span data-stu-id="23398-161">The client supports WebSocket</span></span>
    - <span data-ttu-id="23398-162">伺服器支援 WebSocket</span><span class="sxs-lookup"><span data-stu-id="23398-162">The server supports WebSocket</span></span>

    <span data-ttu-id="23398-163">如果不符合任何一個準則，將會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="23398-163">If any of these criteria are not met, Long Polling will be used.</span></span> <span data-ttu-id="23398-164">如需有關跨網域連線的詳細資訊，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="23398-164">For more information on cross-domain connections, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>
4. <span data-ttu-id="23398-165">如果未設定 JSONP 連接不是跨網域，如果用戶端和伺服器支援它，就會使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="23398-165">If JSONP is not configured and the connection is not cross-domain, WebSocket will be used if both the client and server support it.</span></span>
5. <span data-ttu-id="23398-166">如果用戶端或伺服器不支援 WebSocket，如果有的話，就是會使用伺服器傳送事件。</span><span class="sxs-lookup"><span data-stu-id="23398-166">If either the client or server do not support WebSocket, Server Sent Events is used if it is available.</span></span>
6. <span data-ttu-id="23398-167">如果找不到伺服器傳送事件，會嘗試永久框架。</span><span class="sxs-lookup"><span data-stu-id="23398-167">If Server Sent Events is not available, Forever Frame is attempted.</span></span>
7. <span data-ttu-id="23398-168">如果永久框架失敗時，會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="23398-168">If Forever Frame fails, Long Polling is used.</span></span>

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a><span data-ttu-id="23398-169">監視的傳輸</span><span class="sxs-lookup"><span data-stu-id="23398-169">Monitoring transports</span></span>

<span data-ttu-id="23398-170">您可以判斷應用程式藉由啟用登入您的中樞，並在您的瀏覽器中開啟 [主控台] 視窗中使用何種傳輸。</span><span class="sxs-lookup"><span data-stu-id="23398-170">You can determine what transport your application is using by enabling logging on your hub, and opening the console window in your browser.</span></span>

<span data-ttu-id="23398-171">若要啟用您的中樞事件的瀏覽器中的記錄，用戶端應用程式，將下列命令：</span><span class="sxs-lookup"><span data-stu-id="23398-171">To enable logging for your hub's events in a browser, add the following command to your client application:</span></span>

`$.connection.hub.logging = true;`

- <span data-ttu-id="23398-172">在 Internet Explorer 中，開啟 [開發人員工具]，請按 F12，然後按一下 [主控台] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="23398-172">In Internet Explorer, open the developer tools by pressing F12, and click the Console tab.</span></span>

    ![在 Microsoft Internet Explorer 中的主控台](introduction-to-signalr/_static/image2.png)
- <span data-ttu-id="23398-174">在 Chrome 中，請按 Ctrl + Shift + J 開啟主控台。</span><span class="sxs-lookup"><span data-stu-id="23398-174">In Chrome, open the console by pressing Ctrl+Shift+J.</span></span>

    ![Google Chrome 中的主控台](introduction-to-signalr/_static/image3.png)

<span data-ttu-id="23398-176">主控台開啟和已啟用記錄，您可以查看 SignalR 正在使用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="23398-176">With the console open and logging enabled, you'll be able to see which transport is being used by SignalR.</span></span>

![主控台中顯示 WebSocket 傳輸的 Internet Explorer](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a><span data-ttu-id="23398-178">指定傳輸</span><span class="sxs-lookup"><span data-stu-id="23398-178">Specifying a transport</span></span>

<span data-ttu-id="23398-179">交涉傳輸會在特定的一段時間和用戶端/伺服器資源。</span><span class="sxs-lookup"><span data-stu-id="23398-179">Negotiating a transport takes a certain amount of time and client/server resources.</span></span> <span data-ttu-id="23398-180">如果已知的用戶端功能，則為傳輸可以指定啟動用戶端連線時。</span><span class="sxs-lookup"><span data-stu-id="23398-180">If the client capabilities are known, then a transport can be specified when the client connection is started.</span></span> <span data-ttu-id="23398-181">下列程式碼片段會示範如何開始使用 Ajax 長輪詢傳輸，如果它已知的用戶端並不支援任何其他通訊協定所使用的連接：</span><span class="sxs-lookup"><span data-stu-id="23398-181">The following code snippet demonstrates starting a connection using the Ajax Long Polling transport, as would be used if it was known that the client did not support any other protocol:</span></span>

`connection.start({ transport: 'longPolling' });`

<span data-ttu-id="23398-182">如果您想要再試一次在順序中的特定傳輸的用戶端，您可以指定後援的順序。</span><span class="sxs-lookup"><span data-stu-id="23398-182">You can specify a fallback order if you want a client to try specific transports in order.</span></span> <span data-ttu-id="23398-183">下列程式碼片段示範嘗試 WebSocket，並再次失敗，直接前往長輪詢。</span><span class="sxs-lookup"><span data-stu-id="23398-183">The following code snippet demonstrates trying WebSocket, and failing that, going directly to Long Polling.</span></span>

`connection.start({ transport: ['webSockets','longPolling'] });`

<span data-ttu-id="23398-184">用來指定傳輸的字串常數的定義方式如下：</span><span class="sxs-lookup"><span data-stu-id="23398-184">The string constants for specifying transports are defined as follows:</span></span>

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a><span data-ttu-id="23398-185">連線和中樞</span><span class="sxs-lookup"><span data-stu-id="23398-185">Connections and Hubs</span></span>

<span data-ttu-id="23398-186">SignalR 應用程式開發介面包含兩個模型，以用戶端和伺服器之間的通訊： 持續性連線和中樞。</span><span class="sxs-lookup"><span data-stu-id="23398-186">The SignalR API contains two models for communicating between clients and servers: Persistent Connections and Hubs.</span></span>

<span data-ttu-id="23398-187">連接代表單一收件者、 群組或廣播訊息傳送的簡單端點。</span><span class="sxs-lookup"><span data-stu-id="23398-187">A Connection represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="23398-188">持續性連線應用程式開發介面 （由.NET 程式碼 PersistentConnection 類別） 可讓開發人員直接存取 SignalR 公開 （expose） 的低層級的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="23398-188">The Persistent Connection API (represented in .NET code by the PersistentConnection class) gives the developer direct access to the low-level communication protocol that SignalR exposes.</span></span> <span data-ttu-id="23398-189">使用連線通訊模型都很熟悉的開發人員使用以連線為基礎的 Api，例如 Windows Communication Foundation。</span><span class="sxs-lookup"><span data-stu-id="23398-189">Using the Connections communication model will be familiar to developers who have used connection-based APIs such as Windows Communication Foundation.</span></span>

<span data-ttu-id="23398-190">建立連接的 API，可讓用戶端與伺服器彼此直接呼叫方法所在的更高層級管線的中樞。</span><span class="sxs-lookup"><span data-stu-id="23398-190">A Hub is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span> <span data-ttu-id="23398-191">SignalR 處理如同魔術，跨電腦界限分派允許用戶端在伺服器上呼叫方法，以輕鬆地為本機的方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="23398-191">SignalR handles the dispatching across machine boundaries as if by magic, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="23398-192">使用中樞通訊模型都很熟悉的開發人員使用遠端叫用應用程式開發介面，例如.NET 遠端處理。</span><span class="sxs-lookup"><span data-stu-id="23398-192">Using the Hubs communication model will be familiar to developers who have used remote invocation APIs such as .NET Remoting.</span></span> <span data-ttu-id="23398-193">使用中樞也可讓您將強型別的參數傳遞給方法，可讓模型繫結。</span><span class="sxs-lookup"><span data-stu-id="23398-193">Using a Hub also allows you to pass strongly typed parameters to methods, enabling model binding.</span></span>

### <a name="architecture-diagram"></a><span data-ttu-id="23398-194">架構圖表</span><span class="sxs-lookup"><span data-stu-id="23398-194">Architecture diagram</span></span>

<span data-ttu-id="23398-195">下圖顯示集線器、 持續連線，以及用於傳輸的基礎技術之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="23398-195">The following diagram shows the relationship between Hubs, Persistent Connections, and the underlying technologies used for transports.</span></span>

![顯示應用程式開發介面、 傳輸與用戶端的 SignalR 架構圖](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a><span data-ttu-id="23398-197">中樞的運作方式</span><span class="sxs-lookup"><span data-stu-id="23398-197">How Hubs work</span></span>

<span data-ttu-id="23398-198">當伺服器端程式碼在用戶端上呼叫方法時，作用中傳輸，其中包含要呼叫之方法的參數與名稱之間，會傳送封包 （當物件做為方法參數傳送，它會使用序列化 JSON）。</span><span class="sxs-lookup"><span data-stu-id="23398-198">When server-side code calls a method on the client, a packet is sent across the active transport that contains the name and parameters of the method to be called (when an object is sent as a method parameter, it is serialized using JSON).</span></span> <span data-ttu-id="23398-199">接著，用戶端會比對方法名稱，以在用戶端程式碼中定義的方法。</span><span class="sxs-lookup"><span data-stu-id="23398-199">The client then matches the method name to methods defined in client-side code.</span></span> <span data-ttu-id="23398-200">如果沒有相符項目，將使用已還原序列化的參數資料執行用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="23398-200">If there is a match, the client method will be executed using the deserialized parameter data.</span></span>

<span data-ttu-id="23398-201">方法呼叫可以使用類似的工具來監視[Fiddler。](http://fiddler2.com/)</span><span class="sxs-lookup"><span data-stu-id="23398-201">The method call can be monitored using tools like [Fiddler.](http://fiddler2.com/)</span></span> <span data-ttu-id="23398-202">下圖顯示從 SignalR 伺服器傳送至 Fiddler 的記錄檔 窗格中的 web 瀏覽器用戶端方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="23398-202">The following image shows a method call sent from a SignalR server to a web browser client in the Logs pane of Fiddler.</span></span> <span data-ttu-id="23398-203">方法呼叫傳送與中樞呼叫`MoveShapeHub`，並叫用此方法會呼叫`updateShape`。</span><span class="sxs-lookup"><span data-stu-id="23398-203">The method call is being sent from a hub called `MoveShapeHub`, and the method being invoked is called `updateShape`.</span></span>

![Fiddler 日誌顯示 SignalR 流量的檢視](introduction-to-signalr/_static/image6.png)

<span data-ttu-id="23398-205">在此範例中，以識別中樞名稱`H`參數; 方法名稱會用來識別`M`參數和資料傳送至該方法會用來識別`A`參數。</span><span class="sxs-lookup"><span data-stu-id="23398-205">In this example, the hub name is identified with the `H` parameter; the method name is identified with the `M` parameter, and the data being sent to the method is identified with the `A` parameter.</span></span> <span data-ttu-id="23398-206">產生此訊息的應用程式中建立[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="23398-206">The application that generated this message is created in the [High-Frequency Realtime](tutorial-high-frequency-realtime-with-signalr.md) tutorial.</span></span>

### <a name="choosing-a-communication-model"></a><span data-ttu-id="23398-207">選擇的通訊模式</span><span class="sxs-lookup"><span data-stu-id="23398-207">Choosing a communication model</span></span>

<span data-ttu-id="23398-208">大部分的應用程式應該使用中樞應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="23398-208">Most applications should use the Hubs API.</span></span> <span data-ttu-id="23398-209">連接 API 無法用於下列情況：</span><span class="sxs-lookup"><span data-stu-id="23398-209">The Connections API could be used in the following circumstances:</span></span>

- <span data-ttu-id="23398-210">實際傳送的訊息必須指定格式。</span><span class="sxs-lookup"><span data-stu-id="23398-210">The format of the actual message sent needs to be specified.</span></span>
- <span data-ttu-id="23398-211">開發人員偏好使用傳訊和發送模型，而不是遠端的引動過程模型。</span><span class="sxs-lookup"><span data-stu-id="23398-211">The developer prefers to work with a messaging and dispatching model rather than a remote invocation model.</span></span>
- <span data-ttu-id="23398-212">使用 SignalR 被移植現有的應用程式使用訊息模式。</span><span class="sxs-lookup"><span data-stu-id="23398-212">An existing application that uses a messaging model is being ported to use SignalR.</span></span>
