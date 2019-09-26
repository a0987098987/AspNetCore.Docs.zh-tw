---
title: SignalR 用戶端功能
author: bradygaster
description: 瞭解各種 ASP.NET Core SignalR 用戶端所支援的功能。
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301194"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="862ec-103">ASP.NET Core SignalR 用戶端功能</span><span class="sxs-lookup"><span data-stu-id="862ec-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="862ec-104">功能散發</span><span class="sxs-lookup"><span data-stu-id="862ec-104">Feature distribution</span></span>

<span data-ttu-id="862ec-105">下表顯示提供即時支援之用戶端的功能與支援。</span><span class="sxs-lookup"><span data-stu-id="862ec-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="862ec-106">功能</span><span class="sxs-lookup"><span data-stu-id="862ec-106">Feature</span></span> | <span data-ttu-id="862ec-107">.NET</span><span class="sxs-lookup"><span data-stu-id="862ec-107">.NET</span></span> | <span data-ttu-id="862ec-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="862ec-108">JavaScript</span></span> | <span data-ttu-id="862ec-109">Java</span><span class="sxs-lookup"><span data-stu-id="862ec-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="862ec-110">Azure SignalR Service 支援</span><span class="sxs-lookup"><span data-stu-id="862ec-110">Azure SignalR Service Support</span></span> |<span data-ttu-id="862ec-111">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-111">✔</span></span>|<span data-ttu-id="862ec-112">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-112">✔</span></span>|<span data-ttu-id="862ec-113">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-113">✔</span></span>|
| [<span data-ttu-id="862ec-114">伺服器對用戶端串流</span><span class="sxs-lookup"><span data-stu-id="862ec-114">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="862ec-115">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-115">✔</span></span>|<span data-ttu-id="862ec-116">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-116">✔</span></span>|<span data-ttu-id="862ec-117">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-117">✔</span></span>|
| [<span data-ttu-id="862ec-118">用戶端對伺服器串流</span><span class="sxs-lookup"><span data-stu-id="862ec-118">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="862ec-119">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-119">✔</span></span>|<span data-ttu-id="862ec-120">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-120">✔</span></span>|<span data-ttu-id="862ec-121">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-121">✔</span></span>|
| <span data-ttu-id="862ec-122">自動重新連接（[.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection)、 [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients)）</span><span class="sxs-lookup"><span data-stu-id="862ec-122">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="862ec-123">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-123">✔</span></span>|<span data-ttu-id="862ec-124">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-124">✔</span></span>| |
| <span data-ttu-id="862ec-125">Websocket 傳輸</span><span class="sxs-lookup"><span data-stu-id="862ec-125">WebSockets Transport</span></span> |<span data-ttu-id="862ec-126">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-126">✔</span></span>|<span data-ttu-id="862ec-127">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-127">✔</span></span>|<span data-ttu-id="862ec-128">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-128">✔</span></span>|
| <span data-ttu-id="862ec-129">伺服器傳送的事件傳輸</span><span class="sxs-lookup"><span data-stu-id="862ec-129">Server-Sent Events Transport</span></span> |<span data-ttu-id="862ec-130">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-130">✔</span></span>|<span data-ttu-id="862ec-131">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-131">✔</span></span>| |
| <span data-ttu-id="862ec-132">長輪詢傳輸</span><span class="sxs-lookup"><span data-stu-id="862ec-132">Long Polling Transport</span></span> |<span data-ttu-id="862ec-133">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-133">✔</span></span>|<span data-ttu-id="862ec-134">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-134">✔</span></span>|<span data-ttu-id="862ec-135">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-135">✔</span></span>|
| <span data-ttu-id="862ec-136">JSON 中樞通訊協定</span><span class="sxs-lookup"><span data-stu-id="862ec-136">JSON Hub Protocol</span></span> |<span data-ttu-id="862ec-137">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-137">✔</span></span>|<span data-ttu-id="862ec-138">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-138">✔</span></span>|<span data-ttu-id="862ec-139">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-139">✔</span></span>|
| <span data-ttu-id="862ec-140">MessagePack 中樞通訊協定</span><span class="sxs-lookup"><span data-stu-id="862ec-140">MessagePack Hub Protocol</span></span> |<span data-ttu-id="862ec-141">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-141">✔</span></span>|<span data-ttu-id="862ec-142">✔</span><span class="sxs-lookup"><span data-stu-id="862ec-142">✔</span></span>| |

<span data-ttu-id="862ec-143">在[我們的問題追蹤](https://github.com/aspnet/AspNetCore/issues/8711)程式中，會追蹤 JAVA 用戶端的自動重新連線支援。</span><span class="sxs-lookup"><span data-stu-id="862ec-143">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="862ec-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="862ec-144">Additional resources</span></span>

* [<span data-ttu-id="862ec-145">開始使用 SignalR for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="862ec-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="862ec-146">支援的平台</span><span class="sxs-lookup"><span data-stu-id="862ec-146">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="862ec-147">中樞</span><span class="sxs-lookup"><span data-stu-id="862ec-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="862ec-148">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="862ec-148">JavaScript client</span></span>](xref:signalr/javascript-client)
