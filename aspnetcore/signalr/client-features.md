---
title: SignalR 用戶端功能
author: bradygaster
description: 瞭解各種 ASP.NET Core SignalR 用戶端所支援的功能。
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925347"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="65f8b-103">ASP.NET Core SignalR 用戶端功能</span><span class="sxs-lookup"><span data-stu-id="65f8b-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="65f8b-104">功能散發</span><span class="sxs-lookup"><span data-stu-id="65f8b-104">Feature distribution</span></span>

<span data-ttu-id="65f8b-105">下表顯示提供即時支援之用戶端的功能與支援。</span><span class="sxs-lookup"><span data-stu-id="65f8b-105">The table below shows the features and support for the clients that offer real-time support.</span></span> <span data-ttu-id="65f8b-106">針對每個功能，會列出支援這項功能的*最低*版本。</span><span class="sxs-lookup"><span data-stu-id="65f8b-106">For each feature, the *minimum* version supporting this feature is listed.</span></span> <span data-ttu-id="65f8b-107">如果沒有列出任何版本，則不支援此功能。</span><span class="sxs-lookup"><span data-stu-id="65f8b-107">If no version is listed, the feature isn't supported.</span></span>

| <span data-ttu-id="65f8b-108">功能</span><span class="sxs-lookup"><span data-stu-id="65f8b-108">Feature</span></span> | <span data-ttu-id="65f8b-109">.NET</span><span class="sxs-lookup"><span data-stu-id="65f8b-109">.NET</span></span> | <span data-ttu-id="65f8b-110">JavaScript</span><span class="sxs-lookup"><span data-stu-id="65f8b-110">JavaScript</span></span> | <span data-ttu-id="65f8b-111">Java</span><span class="sxs-lookup"><span data-stu-id="65f8b-111">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="65f8b-112">Azure SignalR Service 支援</span><span class="sxs-lookup"><span data-stu-id="65f8b-112">Azure SignalR Service Support</span></span> |<span data-ttu-id="65f8b-113">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-113">1.0.0</span></span>|<span data-ttu-id="65f8b-114">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-114">1.0.0</span></span>|<span data-ttu-id="65f8b-115">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-115">1.0.0</span></span>|
| [<span data-ttu-id="65f8b-116">伺服器對用戶端串流</span><span class="sxs-lookup"><span data-stu-id="65f8b-116">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="65f8b-117">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-117">1.0.0</span></span>|<span data-ttu-id="65f8b-118">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-118">1.0.0</span></span>|<span data-ttu-id="65f8b-119">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-119">1.0.0</span></span>|
| [<span data-ttu-id="65f8b-120">用戶端對伺服器串流</span><span class="sxs-lookup"><span data-stu-id="65f8b-120">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="65f8b-121">3.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-121">3.0.0</span></span>|<span data-ttu-id="65f8b-122">3.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-122">3.0.0</span></span>|<span data-ttu-id="65f8b-123">3.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-123">3.0.0</span></span>|
| <span data-ttu-id="65f8b-124">自動重新連接（[.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection)、 [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients)）</span><span class="sxs-lookup"><span data-stu-id="65f8b-124">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="65f8b-125">3.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-125">3.0.0</span></span>|<span data-ttu-id="65f8b-126">3.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-126">3.0.0</span></span>|<span data-ttu-id="65f8b-127">❌</span><span class="sxs-lookup"><span data-stu-id="65f8b-127">❌</span></span>|
| <span data-ttu-id="65f8b-128">Websocket 傳輸</span><span class="sxs-lookup"><span data-stu-id="65f8b-128">WebSockets Transport</span></span> |<span data-ttu-id="65f8b-129">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-129">1.0.0</span></span>|<span data-ttu-id="65f8b-130">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-130">1.0.0</span></span>|<span data-ttu-id="65f8b-131">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-131">1.0.0</span></span>|
| <span data-ttu-id="65f8b-132">伺服器傳送的事件傳輸</span><span class="sxs-lookup"><span data-stu-id="65f8b-132">Server-Sent Events Transport</span></span> |<span data-ttu-id="65f8b-133">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-133">1.0.0</span></span>|<span data-ttu-id="65f8b-134">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-134">1.0.0</span></span>|<span data-ttu-id="65f8b-135">❌</span><span class="sxs-lookup"><span data-stu-id="65f8b-135">❌</span></span>|
| <span data-ttu-id="65f8b-136">長輪詢傳輸</span><span class="sxs-lookup"><span data-stu-id="65f8b-136">Long Polling Transport</span></span> |<span data-ttu-id="65f8b-137">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-137">1.0.0</span></span>|<span data-ttu-id="65f8b-138">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-138">1.0.0</span></span>|<span data-ttu-id="65f8b-139">3.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-139">3.0.0</span></span>|
| <span data-ttu-id="65f8b-140">JSON 中樞通訊協定</span><span class="sxs-lookup"><span data-stu-id="65f8b-140">JSON Hub Protocol</span></span> |<span data-ttu-id="65f8b-141">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-141">1.0.0</span></span>|<span data-ttu-id="65f8b-142">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-142">1.0.0</span></span>|<span data-ttu-id="65f8b-143">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-143">1.0.0</span></span>|
| <span data-ttu-id="65f8b-144">MessagePack 中樞通訊協定</span><span class="sxs-lookup"><span data-stu-id="65f8b-144">MessagePack Hub Protocol</span></span> |<span data-ttu-id="65f8b-145">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-145">1.0.0</span></span>|<span data-ttu-id="65f8b-146">1.0.0</span><span class="sxs-lookup"><span data-stu-id="65f8b-146">1.0.0</span></span>|<span data-ttu-id="65f8b-147">❌</span><span class="sxs-lookup"><span data-stu-id="65f8b-147">❌</span></span>|

<span data-ttu-id="65f8b-148">在[我們的問題追蹤](https://github.com/aspnet/AspNetCore/issues/8711)程式中，會追蹤 JAVA 用戶端的自動重新連線支援。</span><span class="sxs-lookup"><span data-stu-id="65f8b-148">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65f8b-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="65f8b-149">Additional resources</span></span>

* [<span data-ttu-id="65f8b-150">開始使用 SignalR for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65f8b-150">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="65f8b-151">支援的平台</span><span class="sxs-lookup"><span data-stu-id="65f8b-151">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="65f8b-152">中樞</span><span class="sxs-lookup"><span data-stu-id="65f8b-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="65f8b-153">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="65f8b-153">JavaScript client</span></span>](xref:signalr/javascript-client)
