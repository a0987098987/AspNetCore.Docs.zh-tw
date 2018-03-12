---
title: "ASP.NET Core 的 SignalR 簡介"
author: rachelappel
description: "了解如何 ASP.NET Core SignalR 程式庫可簡化將即時 web 功能加入至應用程式。"
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction-signalr-core
ms.openlocfilehash: d4ad9bb1910a3339ac8d0d8ff740417f4e7262b7
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-signalr"></a><span data-ttu-id="1342e-103">SignalR 的簡介</span><span class="sxs-lookup"><span data-stu-id="1342e-103">Introduction to SignalR</span></span>

<span data-ttu-id="1342e-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="1342e-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="1342e-105">SignalR 是什麼？</span><span class="sxs-lookup"><span data-stu-id="1342e-105">What is SignalR?</span></span>

<span data-ttu-id="1342e-106">ASP.NET Core SignalR 是簡化新增即時 web 功能的應用程式的程式庫。</span><span class="sxs-lookup"><span data-stu-id="1342e-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="1342e-107">即時 web 功能立即可讓用戶端推入內容的伺服器端程式碼。</span><span class="sxs-lookup"><span data-stu-id="1342e-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="1342e-108">適用於 SignalR 的對象：</span><span class="sxs-lookup"><span data-stu-id="1342e-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="1342e-109">需要來自伺服器的高頻率更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1342e-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="1342e-110">範例包括遊戲、 社交網路、 投票、 稽核、 對應和 GPS 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1342e-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="1342e-111">儀表板和監視的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1342e-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="1342e-112">範例包括公司儀表板，立即銷售的更新，或傳送警示。</span><span class="sxs-lookup"><span data-stu-id="1342e-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="1342e-113">共同作業應用程式。</span><span class="sxs-lookup"><span data-stu-id="1342e-113">Collaborative apps.</span></span> <span data-ttu-id="1342e-114">白板應用程式和軟體的會議的小組都協同合作應用程式的範例。</span><span class="sxs-lookup"><span data-stu-id="1342e-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="1342e-115">需要通知的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1342e-115">Apps that require notifications.</span></span> <span data-ttu-id="1342e-116">社交網路、 電子郵件、 聊天室、 遊戲、 旅行警示和許多其他的應用程式使用通知。</span><span class="sxs-lookup"><span data-stu-id="1342e-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="1342e-117">SignalR 提供一個 API 建立伺服器到用戶端[遠端程序呼叫 (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)。</span><span class="sxs-lookup"><span data-stu-id="1342e-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="1342e-118">Rpc 用戶端上呼叫 JavaScript 函數，從伺服器端.NET Core 程式碼。</span><span class="sxs-lookup"><span data-stu-id="1342e-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="1342e-119">SignalR 的 ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1342e-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="1342e-120">會自動處理連接管理。</span><span class="sxs-lookup"><span data-stu-id="1342e-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="1342e-121">同時廣播給所有連接的用戶端的訊息啟用。</span><span class="sxs-lookup"><span data-stu-id="1342e-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="1342e-122">例如，小組室。</span><span class="sxs-lookup"><span data-stu-id="1342e-122">For example, a chat room.</span></span>
* <span data-ttu-id="1342e-123">能夠將訊息傳送至特定的用戶端或用戶端的群組。</span><span class="sxs-lookup"><span data-stu-id="1342e-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="1342e-124">是開放在[GitHub](https://github.com/aspnet/signalr)。</span><span class="sxs-lookup"><span data-stu-id="1342e-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="1342e-125">可妥善調整。</span><span class="sxs-lookup"><span data-stu-id="1342e-125">Scales nicely.</span></span>

<span data-ttu-id="1342e-126">用戶端與伺服器之間的連線是持續性的不同的 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="1342e-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="1342e-127">傳輸</span><span class="sxs-lookup"><span data-stu-id="1342e-127">Transports</span></span>

<span data-ttu-id="1342e-128">透過一些技術來建置即時 SignalR 摘要 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1342e-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="1342e-129">[WebSockets](https://tools.ietf.org/html/rfc7118)是最佳的傳輸，但這些無法使用時，就可以使用其他技術，如 Server-Sent 事件和長輪詢。</span><span class="sxs-lookup"><span data-stu-id="1342e-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="1342e-130">SignalR 會自動偵測並初始化適當伺服器和用戶端上支援的功能為基礎的傳輸。</span><span class="sxs-lookup"><span data-stu-id="1342e-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs-and-endpoints"></a><span data-ttu-id="1342e-131">中樞和端點</span><span class="sxs-lookup"><span data-stu-id="1342e-131">Hubs and Endpoints</span></span>

<span data-ttu-id="1342e-132">SignalR 使用中樞和端點，用戶端和伺服器之間進行通訊。</span><span class="sxs-lookup"><span data-stu-id="1342e-132">SignalR uses Hubs and Endpoints to communicate between clients and servers.</span></span> <span data-ttu-id="1342e-133">中樞應用程式開發介面涵蓋大部分的案例。</span><span class="sxs-lookup"><span data-stu-id="1342e-133">The Hubs API covers the most scenarios.</span></span>

<span data-ttu-id="1342e-134">建立端點的 API，可讓用戶端與伺服器彼此呼叫方法所在的高層級管線的中樞。</span><span class="sxs-lookup"><span data-stu-id="1342e-134">A hub is a high-level pipeline built upon the Endpoint API that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="1342e-135">SignalR 處理分派跨電腦界限會自動允許用戶端在伺服器上呼叫方法，以輕鬆地為本機的方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="1342e-135">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="1342e-136">中樞允許將強型別參數傳遞至方法，可讓模型繫結。</span><span class="sxs-lookup"><span data-stu-id="1342e-136">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="1342e-137">SignalR 提供兩個內建的中樞通訊協定： 文字通訊協定會根據 JSON 和二進位通訊協定為基礎[MessagePack](https://msgpack.org/)。</span><span class="sxs-lookup"><span data-stu-id="1342e-137">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="1342e-138">MessagePack 通常會建立比使用 JSON 較小的訊息。</span><span class="sxs-lookup"><span data-stu-id="1342e-138">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="1342e-139">舊的瀏覽器必須支援[XHR 層級 2](https://caniuse.com/#feat=xhr2)提供 MessagePack 通訊協定支援。</span><span class="sxs-lookup"><span data-stu-id="1342e-139">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="1342e-140">集線器呼叫用戶端程式碼使用作用中傳輸傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="1342e-140">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="1342e-141">由於訊息包含名稱和用戶端方法的參數。</span><span class="sxs-lookup"><span data-stu-id="1342e-141">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="1342e-142">做為方法參數傳送的物件會還原序列化使用的設定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="1342e-142">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="1342e-143">用戶端會嘗試比對用戶端程式碼中的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="1342e-143">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="1342e-144">發現相符時，用戶端方法會使用執行的已還原序列化的參數資料。</span><span class="sxs-lookup"><span data-stu-id="1342e-144">When a match happens, the client method runs using the deserialized parameter data.</span></span>

<span data-ttu-id="1342e-145">端點提供未經處理的類似通訊端的 API，讓他們能夠讀取和寫入從用戶端。</span><span class="sxs-lookup"><span data-stu-id="1342e-145">Endpoints provide a raw socket-like API, enabling them to read and write from the client.</span></span> <span data-ttu-id="1342e-146">它是由開發人員處理群組、 廣播，以及其他功能。</span><span class="sxs-lookup"><span data-stu-id="1342e-146">It's up to the developer to handle grouping, broadcasting, and other functions.</span></span> <span data-ttu-id="1342e-147">中樞應用程式開發介面建置的結束點圖層的頂端。</span><span class="sxs-lookup"><span data-stu-id="1342e-147">The Hubs API is built on top of the Endpoints layer.</span></span>

<span data-ttu-id="1342e-148">下圖顯示集線器、 端點和用戶端之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="1342e-148">The following diagram shows the relationship between hubs, endpoints, and clients.</span></span>

![SignalR 對應](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a><span data-ttu-id="1342e-150">相關資源</span><span class="sxs-lookup"><span data-stu-id="1342e-150">Related resources</span></span>

[<span data-ttu-id="1342e-151">開始使用 SignalR 的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1342e-151">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started-signalr-core)
