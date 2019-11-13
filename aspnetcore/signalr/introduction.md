---
title: ASP.NET Core SignalR 簡介
author: bradygaster
description: 瞭解 ASP.NET Core SignalR 程式庫如何簡化將即時功能新增至應用程式的工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 7108d9f223db78937dd1203a1cb4b890006b20ec
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963945"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a><span data-ttu-id="d3448-103">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="d3448-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-opno-locsignalr"></a><span data-ttu-id="d3448-104">什麼是 SignalR？</span><span class="sxs-lookup"><span data-stu-id="d3448-104">What is SignalR?</span></span>

<span data-ttu-id="d3448-105">ASP.NET Core SignalR 是一個開放原始碼程式庫，可簡化將即時 web 功能新增至應用程式的流程。</span><span class="sxs-lookup"><span data-stu-id="d3448-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="d3448-106">即時 web 功能可讓伺服器端程式碼立即將內容推送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3448-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="d3448-107">適用于 SignalR的候選項目：</span><span class="sxs-lookup"><span data-stu-id="d3448-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="d3448-108">需要從伺服器進行高頻率更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3448-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="d3448-109">範例包括遊戲、社交網路、投票、拍賣、地圖和 GPS 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3448-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="d3448-110">儀表板和監視應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3448-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="d3448-111">範例包括公司儀表板、立即銷售更新或旅遊警示。</span><span class="sxs-lookup"><span data-stu-id="d3448-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="d3448-112">協同作業應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3448-112">Collaborative apps.</span></span> <span data-ttu-id="d3448-113">白板應用程式和小組會議軟體是共同作業應用程式的範例。</span><span class="sxs-lookup"><span data-stu-id="d3448-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="d3448-114">需要通知的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3448-114">Apps that require notifications.</span></span> <span data-ttu-id="d3448-115">社交網路、電子郵件、聊天、遊戲、旅遊警示，以及許多其他應用程式都使用通知。</span><span class="sxs-lookup"><span data-stu-id="d3448-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

SignalR<span data-ttu-id="d3448-116"> 提供用來建立伺服器對用戶端[遠端程序呼叫（RPC）](https://wikipedia.org/wiki/Remote_procedure_call)的 API。</span><span class="sxs-lookup"><span data-stu-id="d3448-116"> provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="d3448-117">Rpc 會從伺服器端 .NET Core 程式碼呼叫用戶端上的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="d3448-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="d3448-118">以下是 ASP.NET Core SignalR 的一些功能：</span><span class="sxs-lookup"><span data-stu-id="d3448-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="d3448-119">自動處理連接管理。</span><span class="sxs-lookup"><span data-stu-id="d3448-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="d3448-120">同時將訊息傳送至所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3448-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="d3448-121">例如，聊天室。</span><span class="sxs-lookup"><span data-stu-id="d3448-121">For example, a chat room.</span></span>
* <span data-ttu-id="d3448-122">將訊息傳送至特定用戶端或用戶端群組。</span><span class="sxs-lookup"><span data-stu-id="d3448-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="d3448-123">調整以處理增加的流量。</span><span class="sxs-lookup"><span data-stu-id="d3448-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="d3448-124">來源裝載于[GitHub 上的SignalR 存放庫](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR)中。</span><span class="sxs-lookup"><span data-stu-id="d3448-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="d3448-125">傳輸</span><span class="sxs-lookup"><span data-stu-id="d3448-125">Transports</span></span>

SignalR<span data-ttu-id="d3448-126"> 支援數種處理即時通訊的技巧：</span><span class="sxs-lookup"><span data-stu-id="d3448-126"> supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="d3448-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="d3448-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="d3448-128">伺服器傳送的事件</span><span class="sxs-lookup"><span data-stu-id="d3448-128">Server-Sent Events</span></span>
* <span data-ttu-id="d3448-129">長時間輪詢</span><span class="sxs-lookup"><span data-stu-id="d3448-129">Long Polling</span></span>

SignalR<span data-ttu-id="d3448-130"> 會自動選擇伺服器和用戶端功能內的最佳傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="d3448-130"> automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="d3448-131">中樞</span><span class="sxs-lookup"><span data-stu-id="d3448-131">Hubs</span></span>

SignalR<span data-ttu-id="d3448-132"> 使用*中樞*在用戶端和伺服器之間進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d3448-132"> uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="d3448-133">中樞是高階管線，可讓用戶端和伺服器彼此呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="d3448-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> SignalR<span data-ttu-id="d3448-134"> 會自動處理跨電腦界限的分派，讓用戶端可以在伺服器上呼叫方法，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="d3448-134"> handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="d3448-135">您可以將強型別參數傳遞至方法，以啟用模型系結。</span><span class="sxs-lookup"><span data-stu-id="d3448-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> SignalR<span data-ttu-id="d3448-136"> 提供兩個內建的中樞通訊協定：以 JSON 為基礎的文字通訊協定，以及以[MessagePack](https://msgpack.org/)為基礎的二進位通訊協定。</span><span class="sxs-lookup"><span data-stu-id="d3448-136"> provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="d3448-137">與 JSON 相比，MessagePack 通常會建立較小的訊息。</span><span class="sxs-lookup"><span data-stu-id="d3448-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="d3448-138">較舊的瀏覽器必須支援[XHR 層級 2](https://caniuse.com/#feat=xhr2) ，才能提供 MessagePack 通訊協定支援。</span><span class="sxs-lookup"><span data-stu-id="d3448-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="d3448-139">中樞會藉由傳送包含用戶端方法之名稱和參數的訊息來呼叫用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="d3448-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="d3448-140">以方法參數傳送的物件會使用設定的通訊協定進行還原序列化。</span><span class="sxs-lookup"><span data-stu-id="d3448-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="d3448-141">用戶端會嘗試將名稱與用戶端程式代碼中的方法比對。</span><span class="sxs-lookup"><span data-stu-id="d3448-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="d3448-142">當用戶端找到相符的時，它會呼叫方法，並將已還原序列化的參數資料傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="d3448-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3448-143">其他資源</span><span class="sxs-lookup"><span data-stu-id="d3448-143">Additional resources</span></span>

* <span data-ttu-id="d3448-144">[開始使用適用于 ASP.NET Core 的 SignalR](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="d3448-144">[Get started with SignalR for ASP.NET Core](xref:tutorials/signalr)</span></span>
* [<span data-ttu-id="d3448-145">支援的平台</span><span class="sxs-lookup"><span data-stu-id="d3448-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="d3448-146">中樞</span><span class="sxs-lookup"><span data-stu-id="d3448-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="d3448-147">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="d3448-147">JavaScript client</span></span>](xref:signalr/javascript-client)
