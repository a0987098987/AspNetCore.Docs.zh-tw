---
uid: signalr/overview/getting-started/introduction-to-signalr
title: SignalR 簡介 |Microsoft Docs
author: pfletcher
description: 此文章說明 SignalR 為何，以及一些應建立解決方案。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0b7e223b6b793d1860797157be6021ffb7f1bc12
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090289"
---
<a name="introduction-to-signalr"></a><span data-ttu-id="a34e9-103">SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="a34e9-103">Introduction to SignalR</span></span>
====================

<span data-ttu-id="a34e9-104">請參閱[ASP.NET Core SignalR 簡介](/aspnet/core/signalr/introduction)針對本教學課程中使用最新版本的 Visual Studio 的更新版本。</span><span class="sxs-lookup"><span data-stu-id="a34e9-104">See [Introduction to ASP.NET Core SignalR](/aspnet/core/signalr/introduction) for an updated version of this tutorial that uses the latest version of Visual Studio.</span></span> <span data-ttu-id="a34e9-105">新的教學課程會使用[ASP.NET Core](/aspnet/core/)，透過本教學課程提供許多增強功能。</span><span class="sxs-lookup"><span data-stu-id="a34e9-105">The new tutorial uses [ASP.NET Core](/aspnet/core/), which provides many improvements over this tutorial.</span></span>

<span data-ttu-id="a34e9-106">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a34e9-106">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="a34e9-107">此文章說明 SignalR 為何，以及一些應建立解決方案。</span><span class="sxs-lookup"><span data-stu-id="a34e9-107">This article describes what SignalR is, and some of the solutions it was designed to create.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a34e9-108">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="a34e9-108">Questions and comments</span></span>
> 
> <span data-ttu-id="a34e9-109">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="a34e9-109">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a34e9-110">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-110">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).</span></span>


## <a name="what-is-signalr"></a><span data-ttu-id="a34e9-111">SignalR 是什麼？</span><span class="sxs-lookup"><span data-stu-id="a34e9-111">What is SignalR?</span></span>

<span data-ttu-id="a34e9-112">ASP.NET SignalR 是 ASP.NET 開發人員適用的程式庫，可簡化將即時 web 功能新增至應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="a34e9-112">ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.</span></span> <span data-ttu-id="a34e9-113">即時 web 功能是能夠有伺服器程式碼推送內容至連線的用戶端立即可供使用，而不需要伺服器等候用戶端要求新的資料。</span><span class="sxs-lookup"><span data-stu-id="a34e9-113">Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.</span></span>

<span data-ttu-id="a34e9-114">SignalR 可用來將任何類型的 「 即時 」 的 web 功能新增至您的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a34e9-114">SignalR can be used to add any sort of "real-time" web functionality to your ASP.NET application.</span></span> <span data-ttu-id="a34e9-115">交談通常是用做為範例，您可以處理一大堆多。</span><span class="sxs-lookup"><span data-stu-id="a34e9-115">While chat is often used as an example, you can do a whole lot more.</span></span> <span data-ttu-id="a34e9-116">每當使用者重新整理網頁，以查看新的資料，或頁面實作[長輪詢](http://en.wikipedia.org/wiki/Push_technology#Long_polling)擷取新的資料，它是使用 SignalR 的候選。</span><span class="sxs-lookup"><span data-stu-id="a34e9-116">Any time a user refreshes a web page to see new data, or the page implements [long polling](http://en.wikipedia.org/wiki/Push_technology#Long_polling) to retrieve new data, it is a candidate for using SignalR.</span></span> <span data-ttu-id="a34e9-117">範例包括儀表板和監視應用程式，共同作業應用程式 （例如同時編輯的文件），工作進度更新和即時的表單。</span><span class="sxs-lookup"><span data-stu-id="a34e9-117">Examples include dashboards and monitoring applications, collaborative applications (such as simultaneous editing of documents), job progress updates, and real-time forms.</span></span>

<span data-ttu-id="a34e9-118">SignalR 也可讓 web 應用程式需要頻繁更新，從伺服器的全新類型，例如即時遊戲。</span><span class="sxs-lookup"><span data-stu-id="a34e9-118">SignalR also enables completely new types of web applications that require high frequency updates from the server, for example, real-time gaming.</span></span>

<span data-ttu-id="a34e9-119">SignalR 提供一個簡單 API 來建立伺服器到用戶端的遠端程序呼叫 (RPC) 從伺服器端.NET 程式碼呼叫 JavaScript 函式，在用戶端瀏覽器 （和其他用戶端平台）。</span><span class="sxs-lookup"><span data-stu-id="a34e9-119">SignalR provides a simple API for creating server-to-client remote procedure calls (RPC) that call JavaScript functions in client browsers (and other client platforms) from server-side .NET code.</span></span> <span data-ttu-id="a34e9-120">SignalR 也包含連接管理 API （例如，連接和中斷連接事件），和群組的連線。</span><span class="sxs-lookup"><span data-stu-id="a34e9-120">SignalR also includes API for connection management (for instance, connect and disconnect events), and grouping connections.</span></span>

![使用 SignalR 的叫用方法](introduction-to-signalr/_static/image1.png)

<span data-ttu-id="a34e9-122">SignalR 自動處理連接管理，並像聊天室的案例，同時讓您廣播到所有已連線的用戶端的訊息。</span><span class="sxs-lookup"><span data-stu-id="a34e9-122">SignalR handles connection management automatically, and lets you broadcast messages to all connected clients simultaneously, like a chat room.</span></span> <span data-ttu-id="a34e9-123">您也可以傳送訊息給特定的用戶端。</span><span class="sxs-lookup"><span data-stu-id="a34e9-123">You can also send messages to specific clients.</span></span> <span data-ttu-id="a34e9-124">用戶端與伺服器之間的連線是持續性的不同於傳統的 HTTP 連線，也就是重新建立每個通訊。</span><span class="sxs-lookup"><span data-stu-id="a34e9-124">The connection between the client and server is persistent, unlike a classic HTTP connection, which is re-established for each communication.</span></span>

<span data-ttu-id="a34e9-125">SignalR 支援 「 伺服器推入 」 功能，伺服端程式碼呼叫立即在網站上使用遠端程序呼叫 (RPC)，而不是常見的要求-回應模型的瀏覽器中的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="a34e9-125">SignalR supports "server push" functionality, in which server code can call out to client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model common on the web today.</span></span>

<span data-ttu-id="a34e9-126">SignalR 應用程式可以擴充至數千個用戶端使用服務匯流排、 SQL Server 或[Redis](http://redis.io)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-126">SignalR applications can scale out to thousands of clients using Service Bus, SQL Server or [Redis](http://redis.io).</span></span>

<span data-ttu-id="a34e9-127">SignalR 是開放原始碼，可透過存取[GitHub](https://github.com/signalr)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-127">SignalR is open-source, accessible through [GitHub](https://github.com/signalr).</span></span>

## <a name="signalr-and-websocket"></a><span data-ttu-id="a34e9-128">SignalR 和 WebSocket</span><span class="sxs-lookup"><span data-stu-id="a34e9-128">SignalR and WebSocket</span></span>

<span data-ttu-id="a34e9-129">SignalR （如果可用），會使用新的 WebSocket 傳輸，並會回復到舊版的傳輸在必要時。</span><span class="sxs-lookup"><span data-stu-id="a34e9-129">SignalR uses the new WebSocket transport where available and falls back to older transports where necessary.</span></span> <span data-ttu-id="a34e9-130">雖然您當然可以撰寫您的應用程式直接使用 WebSocket，請使用 SignalR，表示您已經完成額外的功能，您就需要實作許多。</span><span class="sxs-lookup"><span data-stu-id="a34e9-130">While you could certainly write your app using WebSocket directly, using SignalR means that a lot of the extra functionality you would need to implement is already done for you.</span></span> <span data-ttu-id="a34e9-131">最重要的是，這表示您可以撰寫您的應用程式，以利用而不必擔心建立不同的程式碼路徑的較舊的用戶端 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="a34e9-131">Most importantly, this means that you can code your app to take advantage of WebSocket without having to worry about creating a separate code path for older clients.</span></span> <span data-ttu-id="a34e9-132">SignalR 也會幫助您不必擔心 WebSocket，更新，因為 SignalR 會更新為基礎的傳輸，支援變更的 WebSocket 版本提供您的應用程式一致的介面。</span><span class="sxs-lookup"><span data-stu-id="a34e9-132">SignalR also shields you from having to worry about updates to WebSocket, since SignalR is updated to support changes in the underlying transport, providing your application a consistent interface across versions of WebSocket.</span></span>

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a><span data-ttu-id="a34e9-133">傳輸和後援</span><span class="sxs-lookup"><span data-stu-id="a34e9-133">Transports and fallbacks</span></span>

<span data-ttu-id="a34e9-134">SignalR 是一些需要執行用戶端與伺服器之間的即時工作傳輸的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="a34e9-134">SignalR is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="a34e9-135">SignalR 連線一開始為 HTTP，並再升級為 WebSocket 連線是否可用。</span><span class="sxs-lookup"><span data-stu-id="a34e9-135">A SignalR connection starts as HTTP, and is then promoted to a WebSocket connection if it is available.</span></span> <span data-ttu-id="a34e9-136">WebSocket 是 SignalR 的理想傳輸，因為伺服器記憶體的最有效率的使用、 具有最低的延遲，以及具有最基礎的功能 （例如完整的雙工用戶端和伺服器之間通訊），但它也具有最嚴格需求： WebSocket 要求的伺服器使用 Windows Server 2012 或 Windows 8 和.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="a34e9-136">WebSocket is the ideal transport for SignalR, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: WebSocket requires the server to be using Windows Server 2012 or Windows 8, and .NET Framework 4.5.</span></span> <span data-ttu-id="a34e9-137">如果不符合這些需求，SignalR 會嘗試使用其他傳輸進行其連線。</span><span class="sxs-lookup"><span data-stu-id="a34e9-137">If these requirements are not met, SignalR will attempt to use other transports to make its connections.</span></span>

### <a name="html-5-transports"></a><span data-ttu-id="a34e9-138">HTML 5 傳輸</span><span class="sxs-lookup"><span data-stu-id="a34e9-138">HTML 5 transports</span></span>

<span data-ttu-id="a34e9-139">支援取決於這些傳輸[HTML 5](http://en.wikipedia.org/wiki/HTML5)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-139">These transports depend on support for [HTML 5](http://en.wikipedia.org/wiki/HTML5).</span></span> <span data-ttu-id="a34e9-140">如果用戶端瀏覽器不支援 HTML 5 標準，就會使用較舊的傳輸。</span><span class="sxs-lookup"><span data-stu-id="a34e9-140">If the client browser does not support the HTML 5 standard, older transports will be used.</span></span>

- <span data-ttu-id="a34e9-141">**WebSocket** (如果伺服器和瀏覽器指出它們可以支援 Websocket)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-141">**WebSocket** (if the both the server and browser indicate they can support Websocket).</span></span> <span data-ttu-id="a34e9-142">WebSocket 是唯一會建立，則為 true 的持續性的雙向連線用戶端與伺服器之間的傳輸。</span><span class="sxs-lookup"><span data-stu-id="a34e9-142">WebSocket is the only transport that establishes a true persistent, two-way connection between client and server.</span></span> <span data-ttu-id="a34e9-143">不過，WebSocket 也具有最嚴格的要求;它完全只支援最新版的 Microsoft Internet Explorer、 Google Chrome 和 Mozilla Firefox 和 Opera 和 Safari 等其他瀏覽器中只有部分的實作。</span><span class="sxs-lookup"><span data-stu-id="a34e9-143">However, WebSocket also has the most stringent requirements; it is fully supported only in the latest versions of Microsoft Internet Explorer, Google Chrome, and Mozilla Firefox, and only has a partial implementation in other browsers such as Opera and Safari.</span></span>
- <span data-ttu-id="a34e9-144">**伺服器傳送事件**，也稱為 EventSource （如果瀏覽器支援伺服器傳送事件，這基本上是 Internet Explorer 以外的所有瀏覽器）。</span><span class="sxs-lookup"><span data-stu-id="a34e9-144">**Server Sent Events**, also known as EventSource (if the browser supports Server Sent Events, which is basically all browsers except Internet Explorer.)</span></span>

### <a name="comet-transports"></a><span data-ttu-id="a34e9-145">Comet 傳輸</span><span class="sxs-lookup"><span data-stu-id="a34e9-145">Comet transports</span></span>

<span data-ttu-id="a34e9-146">下列傳輸為基礎[Comet](http://en.wikipedia.org/wiki/Comet_(programming))瀏覽器或其他用戶端維持長時間保留 HTTP 要求，伺服器可以用來將資料推送至用戶端不是用戶端特別的 web 應用程式模型要求的方式。</span><span class="sxs-lookup"><span data-stu-id="a34e9-146">The following transports are based on the [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web application model, in which a browser or other client maintains a long-held HTTP request, which the server can use to push data to the client without the client specifically requesting it.</span></span>

- <span data-ttu-id="a34e9-147">**永久框架**（適用於僅限 Internet Explorer)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-147">**Forever Frame** (for Internet Explorer only).</span></span> <span data-ttu-id="a34e9-148">永久框架會建立隱藏的 IFrame 的未完成的伺服器上的端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="a34e9-148">Forever Frame creates a hidden IFrame which makes a request to an endpoint on the server that does not complete.</span></span> <span data-ttu-id="a34e9-149">伺服器再持續傳送指令碼到用戶端，便會立即執行，提供用戶端從伺服器的單向的即時連線。</span><span class="sxs-lookup"><span data-stu-id="a34e9-149">The server then continually sends script to the client which is immediately executed, providing a one-way realtime connection from server to client.</span></span> <span data-ttu-id="a34e9-150">從用戶端到伺服器的連線會使用從伺服器個別連接到用戶端，並像標準的 HTTP 要求中，針對每一項需要傳送的資料建立新的連線。</span><span class="sxs-lookup"><span data-stu-id="a34e9-150">The connection from client to server uses a separate connection from the server to client connection, and like a standard HTTP request, a new connection is created for each piece of data that needs to be sent.</span></span>
- <span data-ttu-id="a34e9-151">**長時間輪詢的 Ajax**。</span><span class="sxs-lookup"><span data-stu-id="a34e9-151">**Ajax long polling**.</span></span> <span data-ttu-id="a34e9-152">長時間輪詢不會建立持續連線，但改為會輪詢伺服器以維持開啟，直到伺服器回應，屆時會關閉連接，並立即要求新的連接要求。</span><span class="sxs-lookup"><span data-stu-id="a34e9-152">Long polling does not create a persistent connection, but instead polls the server with a request that stays open until the server responds, at which point the connection closes, and a new connection is requested immediately.</span></span> <span data-ttu-id="a34e9-153">連接重設時，這可能會造成一些延遲。</span><span class="sxs-lookup"><span data-stu-id="a34e9-153">This may introduce some latency while the connection resets.</span></span>

<span data-ttu-id="a34e9-154">如需有關何種傳輸皆支援哪些組態的詳細資訊，請參閱[支援的平台](supported-platforms.md)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-154">For more information on what transports are supported under which configurations, see [Supported Platforms](supported-platforms.md).</span></span>

### <a name="transport-selection-process"></a><span data-ttu-id="a34e9-155">傳輸選取程序</span><span class="sxs-lookup"><span data-stu-id="a34e9-155">Transport selection process</span></span>

<span data-ttu-id="a34e9-156">下列清單顯示 SignalR 用來決定要使用的傳輸的步驟。</span><span class="sxs-lookup"><span data-stu-id="a34e9-156">The following list shows the steps that SignalR uses to decide which transport to use.</span></span>

1. <span data-ttu-id="a34e9-157">如果瀏覽器是 Internet Explorer 8 或更早版本，會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="a34e9-157">If the browser is Internet Explorer 8 or earlier, Long Polling is used.</span></span>
2. <span data-ttu-id="a34e9-158">如果設定 JSONP (亦即`jsonp`參數設為`true`啟動連線時)，長輪詢用。</span><span class="sxs-lookup"><span data-stu-id="a34e9-158">If JSONP is configured (that is, the `jsonp` parameter is set to `true` when the connection is started), Long Polling is used.</span></span>
3. <span data-ttu-id="a34e9-159">如果建立跨網域連線時 （也就是如果 SignalR 端點不在相同的網域裝載的網頁中），則 WebSocket 會使用如果符合下列準則：</span><span class="sxs-lookup"><span data-stu-id="a34e9-159">If a cross-domain connection is being made (that is, if the SignalR endpoint is not in the same domain as the hosting page), then WebSocket will be used if the following criteria are met:</span></span>

   - <span data-ttu-id="a34e9-160">用戶端支援 CORS （跨原始資源共用）。</span><span class="sxs-lookup"><span data-stu-id="a34e9-160">The client supports CORS (Cross-Origin Resource Sharing).</span></span> <span data-ttu-id="a34e9-161">在其的用戶端支援 CORS 的詳細資訊，請參閱 < [caniuse.com 在 CORS](http://www.caniuse.com/CORS)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-161">For details on which clients support CORS, see [CORS at caniuse.com](http://www.caniuse.com/CORS).</span></span>
   - <span data-ttu-id="a34e9-162">用戶端支援 WebSocket</span><span class="sxs-lookup"><span data-stu-id="a34e9-162">The client supports WebSocket</span></span>
   - <span data-ttu-id="a34e9-163">伺服器支援 WebSocket</span><span class="sxs-lookup"><span data-stu-id="a34e9-163">The server supports WebSocket</span></span>

     <span data-ttu-id="a34e9-164">如果不符合任何這些準則，將會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="a34e9-164">If any of these criteria are not met, Long Polling will be used.</span></span> <span data-ttu-id="a34e9-165">如需有關跨網域連接的詳細資訊，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="a34e9-165">For more information on cross-domain connections, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>
4. <span data-ttu-id="a34e9-166">如果未設定 JSONP 連接不是跨網域，如果用戶端和伺服器支援它，就會使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="a34e9-166">If JSONP is not configured and the connection is not cross-domain, WebSocket will be used if both the client and server support it.</span></span>
5. <span data-ttu-id="a34e9-167">如果用戶端或伺服器不支援 WebSocket，如果有的話，就是會使用伺服器傳送事件。</span><span class="sxs-lookup"><span data-stu-id="a34e9-167">If either the client or server do not support WebSocket, Server Sent Events is used if it is available.</span></span>
6. <span data-ttu-id="a34e9-168">如果無法使用伺服器傳送事件，則會嘗試不限次數的框架。</span><span class="sxs-lookup"><span data-stu-id="a34e9-168">If Server Sent Events is not available, Forever Frame is attempted.</span></span>
7. <span data-ttu-id="a34e9-169">如果不限次數的框架就會失敗，會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="a34e9-169">If Forever Frame fails, Long Polling is used.</span></span>

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a><span data-ttu-id="a34e9-170">監視的傳輸</span><span class="sxs-lookup"><span data-stu-id="a34e9-170">Monitoring transports</span></span>

<span data-ttu-id="a34e9-171">您可以判斷您的應用程式使用藉由啟用登入您的中樞，並在您的瀏覽器中開啟主控台視窗中所使用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="a34e9-171">You can determine what transport your application is using by enabling logging on your hub, and opening the console window in your browser.</span></span>

<span data-ttu-id="a34e9-172">若要啟用您的中樞事件，在瀏覽器中的記錄功能，加入您的用戶端應用程式中的下列命令：</span><span class="sxs-lookup"><span data-stu-id="a34e9-172">To enable logging for your hub's events in a browser, add the following command to your client application:</span></span>

`$.connection.hub.logging = true;`

- <span data-ttu-id="a34e9-173">在 Internet Explorer 中，開啟 [開發人員工具]，請按 F12，然後按一下 [主控台] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="a34e9-173">In Internet Explorer, open the developer tools by pressing F12, and click the Console tab.</span></span>

    ![在 Microsoft Internet Explorer 中的主控台](introduction-to-signalr/_static/image2.png)
- <span data-ttu-id="a34e9-175">在 Chrome 中，請按下 Ctrl + Shift + J 開啟主控台。</span><span class="sxs-lookup"><span data-stu-id="a34e9-175">In Chrome, open the console by pressing Ctrl+Shift+J.</span></span>

    ![在 Google Chrome 中的主控台](introduction-to-signalr/_static/image3.png)

<span data-ttu-id="a34e9-177">主控台開啟和啟用記錄，您將能夠看到 SignalR 正在使用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="a34e9-177">With the console open and logging enabled, you'll be able to see which transport is being used by SignalR.</span></span>

![主控台中顯示 WebSocket 傳輸的 Internet Explorer](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a><span data-ttu-id="a34e9-179">指定傳輸</span><span class="sxs-lookup"><span data-stu-id="a34e9-179">Specifying a transport</span></span>

<span data-ttu-id="a34e9-180">交涉傳輸需要一段時間和用戶端/伺服器資源。</span><span class="sxs-lookup"><span data-stu-id="a34e9-180">Negotiating a transport takes a certain amount of time and client/server resources.</span></span> <span data-ttu-id="a34e9-181">如果已知用戶端功能，則傳輸可以指定啟動用戶端連線時。</span><span class="sxs-lookup"><span data-stu-id="a34e9-181">If the client capabilities are known, then a transport can be specified when the client connection is started.</span></span> <span data-ttu-id="a34e9-182">下列程式碼片段會示範如何開始使用 Ajax 長輪詢傳輸，如果它已知的用戶端並不支援任何其他通訊協定所使用的連接：</span><span class="sxs-lookup"><span data-stu-id="a34e9-182">The following code snippet demonstrates starting a connection using the Ajax Long Polling transport, as would be used if it was known that the client did not support any other protocol:</span></span>

`connection.start({ transport: 'longPolling' });`

<span data-ttu-id="a34e9-183">如果您想要嘗試在順序中的特定傳輸的用戶端，您可以指定後援的順序。</span><span class="sxs-lookup"><span data-stu-id="a34e9-183">You can specify a fallback order if you want a client to try specific transports in order.</span></span> <span data-ttu-id="a34e9-184">下列程式碼片段示範嘗試 WebSocket，而且失敗，直接前往長輪詢。</span><span class="sxs-lookup"><span data-stu-id="a34e9-184">The following code snippet demonstrates trying WebSocket, and failing that, going directly to Long Polling.</span></span>

`connection.start({ transport: ['webSockets','longPolling'] });`

<span data-ttu-id="a34e9-185">指定傳輸的字串常數定義，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a34e9-185">The string constants for specifying transports are defined as follows:</span></span>

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a><span data-ttu-id="a34e9-186">連線和中樞</span><span class="sxs-lookup"><span data-stu-id="a34e9-186">Connections and Hubs</span></span>

<span data-ttu-id="a34e9-187">SignalR API 包含用戶端和伺服器之間進行通訊的兩個模型： 持續連線和中樞。</span><span class="sxs-lookup"><span data-stu-id="a34e9-187">The SignalR API contains two models for communicating between clients and servers: Persistent Connections and Hubs.</span></span>

<span data-ttu-id="a34e9-188">連接代表單一收件者、 群組或廣播訊息傳送的簡單端點。</span><span class="sxs-lookup"><span data-stu-id="a34e9-188">A Connection represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="a34e9-189">開發人員直接存取 SignalR 公開的低階通訊協定 （由.NET 程式碼 PersistentConnection 類別） 的持續性連接 API 提供。</span><span class="sxs-lookup"><span data-stu-id="a34e9-189">The Persistent Connection API (represented in .NET code by the PersistentConnection class) gives the developer direct access to the low-level communication protocol that SignalR exposes.</span></span> <span data-ttu-id="a34e9-190">使用連線的通訊模型都很熟悉的開發人員已使用連接為基礎的 Api，例如 Windows Communication Foundation。</span><span class="sxs-lookup"><span data-stu-id="a34e9-190">Using the Connections communication model will be familiar to developers who have used connection-based APIs such as Windows Communication Foundation.</span></span>

<span data-ttu-id="a34e9-191">中樞是建置連接 API，可讓您的用戶端與伺服器彼此直接呼叫方法為基礎的更高層級管線。</span><span class="sxs-lookup"><span data-stu-id="a34e9-191">A Hub is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span> <span data-ttu-id="a34e9-192">SignalR 處理分派跨電腦界限的 magic，如同允許用戶端呼叫伺服器上的方法，以輕鬆地為本機方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="a34e9-192">SignalR handles the dispatching across machine boundaries as if by magic, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="a34e9-193">使用中樞通訊模型都很熟悉的開發人員已使用遠端叫用 Api，例如.NET 遠端處理。</span><span class="sxs-lookup"><span data-stu-id="a34e9-193">Using the Hubs communication model will be familiar to developers who have used remote invocation APIs such as .NET Remoting.</span></span> <span data-ttu-id="a34e9-194">使用中樞也可讓您將強型別的參數傳遞給方法，可讓模型繫結。</span><span class="sxs-lookup"><span data-stu-id="a34e9-194">Using a Hub also allows you to pass strongly typed parameters to methods, enabling model binding.</span></span>

### <a name="architecture-diagram"></a><span data-ttu-id="a34e9-195">架構圖</span><span class="sxs-lookup"><span data-stu-id="a34e9-195">Architecture diagram</span></span>

<span data-ttu-id="a34e9-196">下圖顯示中樞、 持續性連線和用於傳輸的基礎技術之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="a34e9-196">The following diagram shows the relationship between Hubs, Persistent Connections, and the underlying technologies used for transports.</span></span>

![顯示 Api、 傳輸和用戶端的 SignalR 架構圖](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a><span data-ttu-id="a34e9-198">中樞的運作方式</span><span class="sxs-lookup"><span data-stu-id="a34e9-198">How Hubs work</span></span>

<span data-ttu-id="a34e9-199">當伺服器端程式碼會在用戶端上呼叫的方法時，在作用中傳輸，其中包含的名稱和要呼叫之方法的參數傳送封包 （如果物件作為方法參數傳送，它會使用 JSON 序列化）。</span><span class="sxs-lookup"><span data-stu-id="a34e9-199">When server-side code calls a method on the client, a packet is sent across the active transport that contains the name and parameters of the method to be called (when an object is sent as a method parameter, it is serialized using JSON).</span></span> <span data-ttu-id="a34e9-200">接著，用戶端會比對方法名稱，以用戶端程式碼中定義的方法。</span><span class="sxs-lookup"><span data-stu-id="a34e9-200">The client then matches the method name to methods defined in client-side code.</span></span> <span data-ttu-id="a34e9-201">如果沒有相符項目，就將使用已還原序列化的參數資料來執行用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="a34e9-201">If there is a match, the client method will be executed using the deserialized parameter data.</span></span>

<span data-ttu-id="a34e9-202">您可以使用工具，例如監視方法呼叫[Fiddler。](http://fiddler2.com/)</span><span class="sxs-lookup"><span data-stu-id="a34e9-202">The method call can be monitored using tools like [Fiddler.](http://fiddler2.com/)</span></span> <span data-ttu-id="a34e9-203">下圖顯示從 SignalR 伺服器傳送至 web 瀏覽器用戶端的 Fiddler 的記錄檔 窗格中的方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="a34e9-203">The following image shows a method call sent from a SignalR server to a web browser client in the Logs pane of Fiddler.</span></span> <span data-ttu-id="a34e9-204">正在傳送方法呼叫從中樞，稱為`MoveShapeHub`，並叫用的方法呼叫`updateShape`。</span><span class="sxs-lookup"><span data-stu-id="a34e9-204">The method call is being sent from a hub called `MoveShapeHub`, and the method being invoked is called `updateShape`.</span></span>

![顯示 SignalR 流量的 Fiddler 記錄的檢視](introduction-to-signalr/_static/image6.png)

<span data-ttu-id="a34e9-206">在此範例中的中樞名稱會用來識別`H`參數，方法名稱會用來識別`M`參數，並傳送至方法的資料會用來識別`A`參數。</span><span class="sxs-lookup"><span data-stu-id="a34e9-206">In this example, the hub name is identified with the `H` parameter; the method name is identified with the `M` parameter, and the data being sent to the method is identified with the `A` parameter.</span></span> <span data-ttu-id="a34e9-207">產生這則訊息的應用程式中建立[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="a34e9-207">The application that generated this message is created in the [High-Frequency Realtime](tutorial-high-frequency-realtime-with-signalr.md) tutorial.</span></span>

### <a name="choosing-a-communication-model"></a><span data-ttu-id="a34e9-208">選擇的通訊模型</span><span class="sxs-lookup"><span data-stu-id="a34e9-208">Choosing a communication model</span></span>

<span data-ttu-id="a34e9-209">大部分的應用程式應該使用中樞 API。</span><span class="sxs-lookup"><span data-stu-id="a34e9-209">Most applications should use the Hubs API.</span></span> <span data-ttu-id="a34e9-210">連線 API 無法用於下列情況：</span><span class="sxs-lookup"><span data-stu-id="a34e9-210">The Connections API could be used in the following circumstances:</span></span>

- <span data-ttu-id="a34e9-211">實際傳送的訊息必須指定格式。</span><span class="sxs-lookup"><span data-stu-id="a34e9-211">The format of the actual message sent needs to be specified.</span></span>
- <span data-ttu-id="a34e9-212">開發人員偏好使用傳訊和發送模型，而不是遠端的引動過程模型。</span><span class="sxs-lookup"><span data-stu-id="a34e9-212">The developer prefers to work with a messaging and dispatching model rather than a remote invocation model.</span></span>
- <span data-ttu-id="a34e9-213">使用 SignalR 被移植現有的應用程式使用訊息的模型。</span><span class="sxs-lookup"><span data-stu-id="a34e9-213">An existing application that uses a messaging model is being ported to use SignalR.</span></span>
