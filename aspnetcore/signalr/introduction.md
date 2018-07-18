---
title: ASP.NET Core SignalR 簡介
author: tdykstra
description: 了解 ASP.NET Core SignalR 程式庫如何簡化將即時功能新增至應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095385"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="75fc7-103">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="75fc7-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="75fc7-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="75fc7-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="75fc7-105">SignalR 是什麼？</span><span class="sxs-lookup"><span data-stu-id="75fc7-105">What is SignalR?</span></span>

<span data-ttu-id="75fc7-106">ASP.NET Core SignalR 是能簡化新增即時 web 功能的應用程式的程式庫。</span><span class="sxs-lookup"><span data-stu-id="75fc7-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="75fc7-107">即時 web 功能立即可讓用戶端推入內容的伺服器端程式碼。</span><span class="sxs-lookup"><span data-stu-id="75fc7-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="75fc7-108">Signalr 的良好候選項目：</span><span class="sxs-lookup"><span data-stu-id="75fc7-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="75fc7-109">需要來自伺服器的高頻率更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75fc7-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="75fc7-110">範例包括遊戲、 社交網路、 投票、 拍賣、 對應和 GPS 應用程式。</span><span class="sxs-lookup"><span data-stu-id="75fc7-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="75fc7-111">儀表板和監視的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75fc7-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="75fc7-112">範例包括公司儀表板，立即銷售的更新，或的旅遊警示。</span><span class="sxs-lookup"><span data-stu-id="75fc7-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="75fc7-113">共同作業應用程式。</span><span class="sxs-lookup"><span data-stu-id="75fc7-113">Collaborative apps.</span></span> <span data-ttu-id="75fc7-114">白板應用程式和小組會議軟體是共同作業應用程式的範例。</span><span class="sxs-lookup"><span data-stu-id="75fc7-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="75fc7-115">需要通知的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75fc7-115">Apps that require notifications.</span></span> <span data-ttu-id="75fc7-116">社交網路、 電子郵件、 聊天室、 遊戲、 的移動警示和許多其他應用程式使用通知。</span><span class="sxs-lookup"><span data-stu-id="75fc7-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="75fc7-117">SignalR 提供的 API 來建立伺服器到用戶端[遠端程序呼叫 (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)。</span><span class="sxs-lookup"><span data-stu-id="75fc7-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="75fc7-118">從伺服器端.NET Core 程式碼，Rpc 會呼叫 JavaScript 函式在用戶端上。</span><span class="sxs-lookup"><span data-stu-id="75fc7-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="75fc7-119">適用於 ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="75fc7-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="75fc7-120">會自動處理連接管理。</span><span class="sxs-lookup"><span data-stu-id="75fc7-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="75fc7-121">啟用同時廣播到所有已連線的用戶端的訊息。</span><span class="sxs-lookup"><span data-stu-id="75fc7-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="75fc7-122">比方說，聊天室。</span><span class="sxs-lookup"><span data-stu-id="75fc7-122">For example, a chat room.</span></span>
* <span data-ttu-id="75fc7-123">能夠將訊息傳送至特定的用戶端或群組的用戶端。</span><span class="sxs-lookup"><span data-stu-id="75fc7-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="75fc7-124">是開放原始碼，在[GitHub](https://github.com/aspnet/signalr)。</span><span class="sxs-lookup"><span data-stu-id="75fc7-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="75fc7-125">可調整。</span><span class="sxs-lookup"><span data-stu-id="75fc7-125">Scalable.</span></span>

<span data-ttu-id="75fc7-126">用戶端與伺服器之間的連線是持續性的不同於 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="75fc7-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="75fc7-127">傳輸</span><span class="sxs-lookup"><span data-stu-id="75fc7-127">Transports</span></span>

<span data-ttu-id="75fc7-128">SignalR 透過一些技巧來建置即時 web 應用程式的摘要。</span><span class="sxs-lookup"><span data-stu-id="75fc7-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="75fc7-129">[Websocket](https://tools.ietf.org/html/rfc7118)是最佳的傳輸，但這些無法使用時，就可以使用其他方法，像是 Server-Sent 事件和長輪詢。</span><span class="sxs-lookup"><span data-stu-id="75fc7-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="75fc7-130">SignalR 會自動偵測，並初始化適當的伺服器和用戶端上支援的功能為基礎的傳輸。</span><span class="sxs-lookup"><span data-stu-id="75fc7-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="75fc7-131">中樞</span><span class="sxs-lookup"><span data-stu-id="75fc7-131">Hubs</span></span>

<span data-ttu-id="75fc7-132">SignalR 使用中樞用戶端和伺服器之間進行通訊。</span><span class="sxs-lookup"><span data-stu-id="75fc7-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="75fc7-133">中樞是可讓您的用戶端與伺服器彼此呼叫方法的高階管線。</span><span class="sxs-lookup"><span data-stu-id="75fc7-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="75fc7-134">SignalR 處理分派跨電腦界限會自動允許用戶端呼叫伺服器上的方法，以輕鬆地為本機方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="75fc7-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="75fc7-135">中樞可讓強型別參數傳遞至方法，可使用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="75fc7-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="75fc7-136">SignalR 提供兩個內建中樞通訊協定： 文字通訊協定會根據 JSON 和二進位通訊協定為基礎[MessagePack](https://msgpack.org/)。</span><span class="sxs-lookup"><span data-stu-id="75fc7-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="75fc7-137">MessagePack 通常會建立較小的訊息，比使用 JSON。</span><span class="sxs-lookup"><span data-stu-id="75fc7-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="75fc7-138">必須支援舊版瀏覽器[XHR 層級 2](https://caniuse.com/#feat=xhr2)提供 MessagePack 通訊協定支援。</span><span class="sxs-lookup"><span data-stu-id="75fc7-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="75fc7-139">中樞會呼叫用戶端程式碼，藉由使用作用中傳輸傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="75fc7-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="75fc7-140">訊息包含的名稱和用戶端方法的參數。</span><span class="sxs-lookup"><span data-stu-id="75fc7-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="75fc7-141">做為方法參數傳送的物件會使用設定的通訊協定來還原序列化。</span><span class="sxs-lookup"><span data-stu-id="75fc7-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="75fc7-142">用戶端會嘗試比對用戶端程式碼中的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="75fc7-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="75fc7-143">相符項目時，會執行用戶端方法，使用已還原序列化的參數資料。</span><span class="sxs-lookup"><span data-stu-id="75fc7-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75fc7-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="75fc7-144">Additional resources</span></span>

* [<span data-ttu-id="75fc7-145">開始使用 SignalR 的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75fc7-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="75fc7-146">支援的平台</span><span class="sxs-lookup"><span data-stu-id="75fc7-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="75fc7-147">中樞</span><span class="sxs-lookup"><span data-stu-id="75fc7-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="75fc7-148">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="75fc7-148">JavaScript client</span></span>](xref:signalr/javascript-client)
