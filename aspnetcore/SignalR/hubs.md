---
title: 用於 ASP.NET Core SignalR 中樞
author: rachelappel
description: 了解如何使用在 ASP.NET Core SignalR 中樞。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 7da0c4832b1aa6a844172bf751a46b280a02f37a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="00abc-103">使用 ASP.NET Core 中 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="00abc-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="00abc-104">由[Rachel Appel](https://twitter.com/rachelappel)和[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="00abc-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="00abc-105">SignalR 中樞為何</span><span class="sxs-lookup"><span data-stu-id="00abc-105">What is a SignalR hub</span></span>

<span data-ttu-id="00abc-106">SignalR 中樞應用程式開發介面可讓您連線的用戶端上呼叫方法，從伺服器。</span><span class="sxs-lookup"><span data-stu-id="00abc-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="00abc-107">在伺服器程式碼中，您會定義用戶端所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="00abc-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="00abc-108">在用戶端程式碼中，您可以定義從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="00abc-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="00abc-109">SignalR 會負責在幕後，可讓即時的用戶端-伺服器和伺服器用戶端通訊的所有項目。</span><span class="sxs-lookup"><span data-stu-id="00abc-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="00abc-110">設定 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="00abc-110">Configure SignalR hubs</span></span>

<span data-ttu-id="00abc-111">SignalR 的中介軟體需要某些服務，已藉由呼叫`services.AddSignalR`。</span><span class="sxs-lookup"><span data-stu-id="00abc-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="00abc-112">將 SignalR 功能加入至 ASP.NET Core 應用程式，安裝 SignalR 的路由，藉由呼叫`app.UseSignalR`中`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="00abc-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="00abc-113">建立和使用集線器</span><span class="sxs-lookup"><span data-stu-id="00abc-113">Create and use hubs</span></span>

<span data-ttu-id="00abc-114">藉由宣告繼承自一個類別建立中樞`Hub`，並將公用方法加入至它。</span><span class="sxs-lookup"><span data-stu-id="00abc-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="00abc-115">用戶端可以呼叫方法，定義為`public`。</span><span class="sxs-lookup"><span data-stu-id="00abc-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="00abc-116">您可以指定傳回型別和參數，包括複雜型別及陣列，您可以按照任何 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="00abc-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="00abc-117">SignalR 處理的序列化和還原序列化複雜的物件，並在您的參數和傳回值的陣列。</span><span class="sxs-lookup"><span data-stu-id="00abc-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="00abc-118">用戶端物件</span><span class="sxs-lookup"><span data-stu-id="00abc-118">The Clients object</span></span>

<span data-ttu-id="00abc-119">每個執行個體`Hub`類別具有內容，名為`Clients`，其中包含伺服器和用戶端之間通訊的下列成員：</span><span class="sxs-lookup"><span data-stu-id="00abc-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="00abc-120">屬性</span><span class="sxs-lookup"><span data-stu-id="00abc-120">Property</span></span> | <span data-ttu-id="00abc-121">描述</span><span class="sxs-lookup"><span data-stu-id="00abc-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="00abc-122">所有已連線的用戶端上呼叫的方法</span><span class="sxs-lookup"><span data-stu-id="00abc-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="00abc-123">在用戶端中樞方法叫用呼叫的方法</span><span class="sxs-lookup"><span data-stu-id="00abc-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="00abc-124">已叫用方法的用戶端以外的所有連線用戶端上呼叫的方法</span><span class="sxs-lookup"><span data-stu-id="00abc-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="00abc-125">此外，`Hub`類別包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="00abc-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="00abc-126">方法</span><span class="sxs-lookup"><span data-stu-id="00abc-126">Method</span></span> | <span data-ttu-id="00abc-127">描述</span><span class="sxs-lookup"><span data-stu-id="00abc-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="00abc-128">指定的連接除外的所有已連線用戶端上呼叫的方法</span><span class="sxs-lookup"><span data-stu-id="00abc-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="00abc-129">呼叫特定連線的用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="00abc-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="00abc-130">呼叫特定連線的用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="00abc-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="00abc-131">傳送訊息至指定的群組中的所有連接</span><span class="sxs-lookup"><span data-stu-id="00abc-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="00abc-132">傳送訊息至指定的群組，除了指定的連接中的所有連接</span><span class="sxs-lookup"><span data-stu-id="00abc-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="00abc-133">將訊息傳送至多個連接群組</span><span class="sxs-lookup"><span data-stu-id="00abc-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="00abc-134">將訊息傳送至的連線，不包括叫用中樞方法的用戶端群組</span><span class="sxs-lookup"><span data-stu-id="00abc-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="00abc-135">傳送訊息至特定使用者相關聯的所有連接</span><span class="sxs-lookup"><span data-stu-id="00abc-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="00abc-136">傳送訊息至指定的使用者相關聯的所有連接</span><span class="sxs-lookup"><span data-stu-id="00abc-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="00abc-137">每個屬性或方法之前表格中的傳回的物件`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="00abc-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="00abc-138">`SendAsync`方法可讓您提供的名稱和用戶端方法呼叫的參數。</span><span class="sxs-lookup"><span data-stu-id="00abc-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="00abc-139">將訊息傳送至用戶端</span><span class="sxs-lookup"><span data-stu-id="00abc-139">Send messages to clients</span></span>

<span data-ttu-id="00abc-140">若要讓特定的用戶端呼叫，使用的屬性`Clients`物件。</span><span class="sxs-lookup"><span data-stu-id="00abc-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="00abc-141">在下列範例中，下列`SendMessageToCaller`方法將示範將訊息傳送到叫用中樞方法的連線。</span><span class="sxs-lookup"><span data-stu-id="00abc-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="00abc-142">`SendMessageToGroups`方法將訊息傳送至儲存在群組`List`名為`groups`。</span><span class="sxs-lookup"><span data-stu-id="00abc-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="00abc-143">處理連接事件</span><span class="sxs-lookup"><span data-stu-id="00abc-143">Handle events for a connection</span></span>

<span data-ttu-id="00abc-144">提供 SignalR 中樞 API`OnConnectedAsync`和`OnDisconnectedAsync`用於管理及追蹤連線的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="00abc-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="00abc-145">覆寫`OnConnectedAsync`虛擬方法，當用戶端連線至中樞，例如新增到群組執行動作。</span><span class="sxs-lookup"><span data-stu-id="00abc-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="00abc-146">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="00abc-146">Handle errors</span></span>

<span data-ttu-id="00abc-147">在 hub 方法中擲回例外狀況會傳送至已叫用方法的用戶端。</span><span class="sxs-lookup"><span data-stu-id="00abc-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="00abc-148">JavaScript 用戶端上`invoke`方法會傳回[JavaScript 承諾](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。</span><span class="sxs-lookup"><span data-stu-id="00abc-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="00abc-149">當用戶端會收到錯誤與處理常式附加至承諾使用`catch`，它已叫用，並傳遞為 JavaScript`Error`物件。</span><span class="sxs-lookup"><span data-stu-id="00abc-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="00abc-150">相關資源</span><span class="sxs-lookup"><span data-stu-id="00abc-150">Related resources</span></span>

[<span data-ttu-id="00abc-151">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="00abc-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)