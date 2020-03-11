---
title: ASP.NET Core SignalR 簡介
author: bradygaster
description: 瞭解 ASP.NET Core SignalR 程式庫如何簡化將即時功能新增至應用程式的工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 635431abf9263c2dff261aea47e6f8324061763f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662915"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a><span data-ttu-id="50093-103">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="50093-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-opno-locsignalr"></a><span data-ttu-id="50093-104">什麼是 SignalR？</span><span class="sxs-lookup"><span data-stu-id="50093-104">What is SignalR?</span></span>

<span data-ttu-id="50093-105">ASP.NET Core SignalR 是一個開放原始碼程式庫，可簡化將即時 web 功能新增至應用程式的流程。</span><span class="sxs-lookup"><span data-stu-id="50093-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="50093-106">即時 web 功能可讓伺服器端程式碼立即將內容推送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="50093-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="50093-107">適用于 SignalR的候選項目：</span><span class="sxs-lookup"><span data-stu-id="50093-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="50093-108">需要經常從伺服器取得更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="50093-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="50093-109">例如遊戲、社交網路、投票、拍賣、地圖和 GPS 應用程式。</span><span class="sxs-lookup"><span data-stu-id="50093-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="50093-110">儀表板和監視應用程式。</span><span class="sxs-lookup"><span data-stu-id="50093-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="50093-111">範例包括公司儀表板、即時銷售更新或旅行警示。</span><span class="sxs-lookup"><span data-stu-id="50093-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="50093-112">共同作業應用程式。</span><span class="sxs-lookup"><span data-stu-id="50093-112">Collaborative apps.</span></span> <span data-ttu-id="50093-113">共同作業應用程式的範例包括白板應用程式和小組會議軟體。</span><span class="sxs-lookup"><span data-stu-id="50093-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="50093-114">需要通知的應用程式。</span><span class="sxs-lookup"><span data-stu-id="50093-114">Apps that require notifications.</span></span> <span data-ttu-id="50093-115">社交網路、電子郵件、交談、遊戲、旅行警示和其他使用通知的應用程式。</span><span class="sxs-lookup"><span data-stu-id="50093-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

SignalR<span data-ttu-id="50093-116"> 提供用來建立伺服器對用戶端[遠端程序呼叫（RPC）](https://wikipedia.org/wiki/Remote_procedure_call)的 API。</span><span class="sxs-lookup"><span data-stu-id="50093-116"> provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="50093-117">Rpc 會從伺服器端 .NET Core 程式碼呼叫用戶端上的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="50093-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="50093-118">以下是 ASP.NET Core SignalR 的一些功能：</span><span class="sxs-lookup"><span data-stu-id="50093-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="50093-119">自動處理連接管理。</span><span class="sxs-lookup"><span data-stu-id="50093-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="50093-120">同時將訊息傳送至所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="50093-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="50093-121">例如，聊天室。</span><span class="sxs-lookup"><span data-stu-id="50093-121">For example, a chat room.</span></span>
* <span data-ttu-id="50093-122">將訊息傳送至特定用戶端或用戶端群組。</span><span class="sxs-lookup"><span data-stu-id="50093-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="50093-123">調整以處理增加的流量。</span><span class="sxs-lookup"><span data-stu-id="50093-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="50093-124">來源裝載于[GitHub 上的SignalR 存放庫](https://github.com/dotnet/AspNetCore/tree/master/src/SignalR)中。</span><span class="sxs-lookup"><span data-stu-id="50093-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/dotnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="50093-125">傳輸</span><span class="sxs-lookup"><span data-stu-id="50093-125">Transports</span></span>

SignalR<span data-ttu-id="50093-126"> 支援下列用來處理即時通訊的技術（依正常回溯的順序）：</span><span class="sxs-lookup"><span data-stu-id="50093-126"> supports the following techniques for handling real-time communication (in order of graceful fallback):</span></span>

* [<span data-ttu-id="50093-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="50093-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="50093-128">伺服器傳送的事件</span><span class="sxs-lookup"><span data-stu-id="50093-128">Server-Sent Events</span></span>
* <span data-ttu-id="50093-129">長時間輪詢</span><span class="sxs-lookup"><span data-stu-id="50093-129">Long Polling</span></span>

SignalR<span data-ttu-id="50093-130"> 會自動選擇伺服器和用戶端功能內的最佳傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="50093-130"> automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="50093-131">集線器</span><span class="sxs-lookup"><span data-stu-id="50093-131">Hubs</span></span>

SignalR<span data-ttu-id="50093-132"> 使用*中樞*在用戶端和伺服器之間進行通訊。</span><span class="sxs-lookup"><span data-stu-id="50093-132"> uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="50093-133">中樞是高階管線，可讓用戶端和伺服器彼此呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="50093-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> SignalR<span data-ttu-id="50093-134"> 會自動處理跨電腦界限的分派，讓用戶端可以在伺服器上呼叫方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="50093-134"> handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="50093-135">您可以將強型別參數傳遞至方法，以啟用模型系結。</span><span class="sxs-lookup"><span data-stu-id="50093-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> SignalR<span data-ttu-id="50093-136"> 提供兩個內建的中樞通訊協定：以 JSON 為基礎的文字通訊協定，以及以[MessagePack](https://msgpack.org/)為基礎的二進位通訊協定。</span><span class="sxs-lookup"><span data-stu-id="50093-136"> provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="50093-137">與 JSON 相比，MessagePack 通常會建立較小的訊息。</span><span class="sxs-lookup"><span data-stu-id="50093-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="50093-138">較舊的瀏覽器必須支援[XHR 層級 2](https://caniuse.com/#feat=xhr2) ，才能提供 MessagePack 通訊協定支援。</span><span class="sxs-lookup"><span data-stu-id="50093-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="50093-139">中樞會藉由傳送包含用戶端方法之名稱和參數的訊息來呼叫用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="50093-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="50093-140">以方法參數傳送的物件會使用設定的通訊協定進行還原序列化。</span><span class="sxs-lookup"><span data-stu-id="50093-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="50093-141">用戶端會嘗試將名稱與用戶端程式代碼中的方法比對。</span><span class="sxs-lookup"><span data-stu-id="50093-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="50093-142">當用戶端找到相符的時，它會呼叫方法，並將已還原序列化的參數資料傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="50093-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="50093-143">其他資源</span><span class="sxs-lookup"><span data-stu-id="50093-143">Additional resources</span></span>

* <span data-ttu-id="50093-144">[開始使用適用于 ASP.NET Core 的 SignalR](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="50093-144">[Get started with SignalR for ASP.NET Core](xref:tutorials/signalr)</span></span>
* [<span data-ttu-id="50093-145">支援的平台</span><span class="sxs-lookup"><span data-stu-id="50093-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="50093-146">中樞</span><span class="sxs-lookup"><span data-stu-id="50093-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="50093-147">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="50093-147">JavaScript client</span></span>](xref:signalr/javascript-client)
