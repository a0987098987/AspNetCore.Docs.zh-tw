---
title: 用於 ASP.NET Core SignalR 中樞
author: tdykstra
description: 了解如何使用 ASP.NET Core signalr 的中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095277"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="300be-103">使用 ASP.NET Core signalr 中樞</span><span class="sxs-lookup"><span data-stu-id="300be-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="300be-104">藉由[Rachel Appel](https://twitter.com/rachelappel)和[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="300be-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="300be-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="300be-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="300be-106">什麼是 SignalR hub</span><span class="sxs-lookup"><span data-stu-id="300be-106">What is a SignalR hub</span></span>

<span data-ttu-id="300be-107">SignalR 中樞 API 可讓您連線的用戶端上呼叫方法，從伺服器。</span><span class="sxs-lookup"><span data-stu-id="300be-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="300be-108">在伺服器程式碼中，您會定義用戶端所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="300be-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="300be-109">在用戶端程式碼中，您可以定義會從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="300be-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="300be-110">SignalR 會負責在幕後能夠即時的用戶端對伺服器和伺服器到用戶端通訊的所有項目。</span><span class="sxs-lookup"><span data-stu-id="300be-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="300be-111">設定 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="300be-111">Configure SignalR hubs</span></span>

<span data-ttu-id="300be-112">SignalR 中介軟體會需要某些服務，已藉由呼叫`services.AddSignalR`。</span><span class="sxs-lookup"><span data-stu-id="300be-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="300be-113">將 SignalR 功能加入至 ASP.NET Core 應用程式中，設定 SignalR 的路由，方法是呼叫`app.UseSignalR`在`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="300be-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="300be-114">建立和使用中樞</span><span class="sxs-lookup"><span data-stu-id="300be-114">Create and use hubs</span></span>

<span data-ttu-id="300be-115">藉由宣告繼承自的類別建立中樞`Hub`，並為其新增公用方法。</span><span class="sxs-lookup"><span data-stu-id="300be-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="300be-116">用戶端可以呼叫方法定義為`public`。</span><span class="sxs-lookup"><span data-stu-id="300be-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="300be-117">您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="300be-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="300be-118">SignalR 處理的序列化和還原序列化複雜物件並在您的參數和傳回值的陣列。</span><span class="sxs-lookup"><span data-stu-id="300be-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="300be-119">用戶端物件</span><span class="sxs-lookup"><span data-stu-id="300be-119">The Clients object</span></span>

<span data-ttu-id="300be-120">每個執行個體`Hub`類別具有名為`Clients`，其中包含伺服器與用戶端之間通訊的下列成員：</span><span class="sxs-lookup"><span data-stu-id="300be-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="300be-121">屬性</span><span class="sxs-lookup"><span data-stu-id="300be-121">Property</span></span> | <span data-ttu-id="300be-122">描述</span><span class="sxs-lookup"><span data-stu-id="300be-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="300be-123">所有連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="300be-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="300be-124">叫用中樞方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="300be-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="300be-125">所有連線的用戶端，除了叫用方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="300be-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="300be-126">此外，`Hub.Clients`包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="300be-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="300be-127">方法</span><span class="sxs-lookup"><span data-stu-id="300be-127">Method</span></span> | <span data-ttu-id="300be-128">描述</span><span class="sxs-lookup"><span data-stu-id="300be-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="300be-129">所有連線的用戶端，除了指定的連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="300be-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="300be-130">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="300be-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="300be-131">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="300be-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="300be-132">呼叫中指定的群組至所有連線的方法</span><span class="sxs-lookup"><span data-stu-id="300be-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="300be-133">呼叫中指定的群組，除非指定連線到所有連線的方法</span><span class="sxs-lookup"><span data-stu-id="300be-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="300be-134">呼叫方法，以多個連接群組</span><span class="sxs-lookup"><span data-stu-id="300be-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="300be-135">呼叫方法，以連線，不包括叫用中樞方法的用戶端群組</span><span class="sxs-lookup"><span data-stu-id="300be-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="300be-136">呼叫方法，以與特定使用者相關聯的所有連線</span><span class="sxs-lookup"><span data-stu-id="300be-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="300be-137">呼叫方法，以指定的使用者相關聯的所有連線</span><span class="sxs-lookup"><span data-stu-id="300be-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="300be-138">每個屬性或方法，上述資料表中的傳回的物件`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="300be-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="300be-139">`SendAsync`方法可讓您提供的名稱和用戶端方法呼叫的參數。</span><span class="sxs-lookup"><span data-stu-id="300be-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="300be-140">將訊息傳送至用戶端</span><span class="sxs-lookup"><span data-stu-id="300be-140">Send messages to clients</span></span>

<span data-ttu-id="300be-141">若要為特定用戶端的呼叫，使用的屬性`Clients`物件。</span><span class="sxs-lookup"><span data-stu-id="300be-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="300be-142">在下列範例中，`SendMessageToCaller`方法示範如何將訊息傳送至叫用中樞方法的連接。</span><span class="sxs-lookup"><span data-stu-id="300be-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="300be-143">`SendMessageToGroups`方法會將訊息傳送至儲存在群組`List`名為`groups`。</span><span class="sxs-lookup"><span data-stu-id="300be-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="300be-144">處理連接事件</span><span class="sxs-lookup"><span data-stu-id="300be-144">Handle events for a connection</span></span>

<span data-ttu-id="300be-145">提供 SignalR 中樞 API`OnConnectedAsync`和`OnDisconnectedAsync`來管理和追蹤連線的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="300be-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="300be-146">覆寫`OnConnectedAsync`虛擬方法，以執行動作，當用戶端連線至中樞，例如將它新增至群組。</span><span class="sxs-lookup"><span data-stu-id="300be-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="300be-147">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="300be-147">Handle errors</span></span>

<span data-ttu-id="300be-148">在 hub 方法中擲回例外狀況會傳送至用戶端叫用方法。</span><span class="sxs-lookup"><span data-stu-id="300be-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="300be-149">在 JavaScript 用戶端`invoke`方法會傳回[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。</span><span class="sxs-lookup"><span data-stu-id="300be-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="300be-150">當用戶端會收到的錯誤處理常式附加至承諾使用`catch`，它已叫用，並傳遞為 JavaScript`Error`物件。</span><span class="sxs-lookup"><span data-stu-id="300be-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="300be-151">相關資源</span><span class="sxs-lookup"><span data-stu-id="300be-151">Related resources</span></span>

* [<span data-ttu-id="300be-152">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="300be-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="300be-153">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="300be-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="300be-154">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="300be-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
