---
title: 用於 ASP.NET Core SignalR 中樞
author: tdykstra
description: 了解如何使用 ASP.NET Core signalr 的中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: be42314afad4ff43d2fcf1abbc96c5b78c773977
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206012"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="d6c18-103">使用 ASP.NET Core SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="d6c18-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="d6c18-104">藉由[Rachel Appel](https://twitter.com/rachelappel)和[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="d6c18-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="d6c18-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="d6c18-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="d6c18-106">什麼是 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="d6c18-106">What is a SignalR hub</span></span>

<span data-ttu-id="d6c18-107">SignalR 中樞 API 可讓您連線的用戶端上呼叫方法，從伺服器。</span><span class="sxs-lookup"><span data-stu-id="d6c18-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="d6c18-108">在伺服器程式碼中，您會定義用戶端所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="d6c18-109">在用戶端程式碼中，您可以定義會從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="d6c18-110">SignalR 會負責在幕後能夠即時的用戶端對伺服器和伺服器到用戶端通訊的所有項目。</span><span class="sxs-lookup"><span data-stu-id="d6c18-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="d6c18-111">設定 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="d6c18-111">Configure SignalR hubs</span></span>

<span data-ttu-id="d6c18-112">SignalR 中介軟體會需要某些服務，已藉由呼叫`services.AddSignalR`。</span><span class="sxs-lookup"><span data-stu-id="d6c18-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="d6c18-113">將 SignalR 功能加入至 ASP.NET Core 應用程式中，設定 SignalR 的路由，方法是呼叫`app.UseSignalR`在`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="d6c18-114">建立和使用中樞</span><span class="sxs-lookup"><span data-stu-id="d6c18-114">Create and use hubs</span></span>

<span data-ttu-id="d6c18-115">藉由宣告繼承自的類別建立中樞`Hub`，並為其新增公用方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="d6c18-116">用戶端可以呼叫方法定義為`public`。</span><span class="sxs-lookup"><span data-stu-id="d6c18-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="d6c18-117">您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="d6c18-118">SignalR 處理的序列化和還原序列化複雜物件並在您的參數和傳回值的陣列。</span><span class="sxs-lookup"><span data-stu-id="d6c18-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="d6c18-119">內容物件</span><span class="sxs-lookup"><span data-stu-id="d6c18-119">The Context object</span></span>

<span data-ttu-id="d6c18-120">`Hub`類別具有`Context`屬性，其中包含與連線相關資訊的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="d6c18-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="d6c18-121">屬性</span><span class="sxs-lookup"><span data-stu-id="d6c18-121">Property</span></span> | <span data-ttu-id="d6c18-122">描述</span><span class="sxs-lookup"><span data-stu-id="d6c18-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="d6c18-123">取得連接，SignalR 所指派的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="d6c18-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="d6c18-124">還有一個針對每個連線的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="d6c18-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="d6c18-125">取得[使用者識別碼](xref:signalr/groups)。</span><span class="sxs-lookup"><span data-stu-id="d6c18-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="d6c18-126">根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。</span><span class="sxs-lookup"><span data-stu-id="d6c18-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="d6c18-127">取得`ClaimsPrincipal`與目前的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="d6c18-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="d6c18-128">取得索引鍵/值集合，可用來共用此連線的範圍內的資料。</span><span class="sxs-lookup"><span data-stu-id="d6c18-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="d6c18-129">資料可以儲存在這個集合中，它會保存連接到不同的中樞方法叫用。</span><span class="sxs-lookup"><span data-stu-id="d6c18-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="d6c18-130">取得連接上的可用功能的集合。</span><span class="sxs-lookup"><span data-stu-id="d6c18-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="d6c18-131">現在，這個集合不需要在大部分情況下，因此它不尚未記載於詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d6c18-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="d6c18-132">取得`CancellationToken`連線中止時，可通知。</span><span class="sxs-lookup"><span data-stu-id="d6c18-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="d6c18-133">`Hub.Context` 也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="d6c18-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="d6c18-134">方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-134">Method</span></span> | <span data-ttu-id="d6c18-135">描述</span><span class="sxs-lookup"><span data-stu-id="d6c18-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="d6c18-136">傳回`HttpContext`進行連接，或`null`如果連接不是 HTTP 要求相關聯。</span><span class="sxs-lookup"><span data-stu-id="d6c18-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="d6c18-137">適用於 HTTP 連線，您可以使用這個方法，以取得資訊，例如 HTTP 標頭和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="d6c18-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="d6c18-138">中止連接。</span><span class="sxs-lookup"><span data-stu-id="d6c18-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="d6c18-139">用戶端物件</span><span class="sxs-lookup"><span data-stu-id="d6c18-139">The Clients object</span></span>

<span data-ttu-id="d6c18-140">`Hub`類別具有`Clients`屬性，其中包含伺服器與用戶端之間通訊的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="d6c18-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="d6c18-141">屬性</span><span class="sxs-lookup"><span data-stu-id="d6c18-141">Property</span></span> | <span data-ttu-id="d6c18-142">描述</span><span class="sxs-lookup"><span data-stu-id="d6c18-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="d6c18-143">所有連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="d6c18-144">叫用中樞方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="d6c18-145">所有連線的用戶端，除了叫用方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="d6c18-146">`Hub.Clients` 也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="d6c18-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="d6c18-147">方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-147">Method</span></span> | <span data-ttu-id="d6c18-148">描述</span><span class="sxs-lookup"><span data-stu-id="d6c18-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="d6c18-149">所有連線的用戶端，除了指定的連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="d6c18-150">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="d6c18-151">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="d6c18-152">呼叫中指定的群組至所有連線的方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="d6c18-153">呼叫中指定的群組，除非指定連線到所有連線的方法</span><span class="sxs-lookup"><span data-stu-id="d6c18-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="d6c18-154">呼叫方法，以多個連接群組</span><span class="sxs-lookup"><span data-stu-id="d6c18-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="d6c18-155">呼叫方法，以連線，不包括叫用中樞方法的用戶端群組</span><span class="sxs-lookup"><span data-stu-id="d6c18-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="d6c18-156">呼叫方法，以與特定使用者相關聯的所有連線</span><span class="sxs-lookup"><span data-stu-id="d6c18-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="d6c18-157">呼叫方法，以指定的使用者相關聯的所有連線</span><span class="sxs-lookup"><span data-stu-id="d6c18-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="d6c18-158">每個屬性或方法，上述資料表中的傳回的物件`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="d6c18-159">`SendAsync`方法可讓您提供的名稱和用戶端方法呼叫的參數。</span><span class="sxs-lookup"><span data-stu-id="d6c18-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="d6c18-160">將訊息傳送至用戶端</span><span class="sxs-lookup"><span data-stu-id="d6c18-160">Send messages to clients</span></span>

<span data-ttu-id="d6c18-161">若要為特定用戶端的呼叫，使用的屬性`Clients`物件。</span><span class="sxs-lookup"><span data-stu-id="d6c18-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="d6c18-162">在下列範例中，`SendMessageToCaller`方法示範如何將訊息傳送至叫用中樞方法的連接。</span><span class="sxs-lookup"><span data-stu-id="d6c18-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="d6c18-163">`SendMessageToGroups`方法會將訊息傳送至儲存在群組`List`名為`groups`。</span><span class="sxs-lookup"><span data-stu-id="d6c18-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="d6c18-164">強型別的中樞</span><span class="sxs-lookup"><span data-stu-id="d6c18-164">Strongly typed hubs</span></span>

<span data-ttu-id="d6c18-165">使用的一項缺點`SendAsync`是它依賴 magic 的字串，以指定要呼叫的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="d6c18-166">這可讓程式碼開啟，如果方法名稱的拼字錯誤的執行階段錯誤或遺漏的用戶端。</span><span class="sxs-lookup"><span data-stu-id="d6c18-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="d6c18-167">使用替代`SendAsync`是強型別`Hub`使用<xref:Microsoft.AspNetCore.SignalR.Hub`1>。</span><span class="sxs-lookup"><span data-stu-id="d6c18-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="d6c18-168">在下列範例中，`ChatHub`用戶端方法已成呼叫登出擷取`IChatClient`。</span><span class="sxs-lookup"><span data-stu-id="d6c18-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="d6c18-169">此介面可用來重構上述`ChatHub`範例。</span><span class="sxs-lookup"><span data-stu-id="d6c18-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="d6c18-170">使用`Hub<IChatClient>`啟用編譯時間檢查的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="d6c18-171">這可避免使用魔術字串，因為所造成的問題`Hub<T>`只能提供存取的介面中定義的方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="d6c18-172">使用強型別`Hub<T>`停用重新使用`SendAsync`。</span><span class="sxs-lookup"><span data-stu-id="d6c18-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="d6c18-173">處理連接事件</span><span class="sxs-lookup"><span data-stu-id="d6c18-173">Handle events for a connection</span></span>

<span data-ttu-id="d6c18-174">提供 SignalR 中樞 API`OnConnectedAsync`和`OnDisconnectedAsync`來管理和追蹤連線的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="d6c18-175">覆寫`OnConnectedAsync`虛擬方法，以執行動作，當用戶端連線至中樞，例如將它新增至群組。</span><span class="sxs-lookup"><span data-stu-id="d6c18-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="d6c18-176">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="d6c18-176">Handle errors</span></span>

<span data-ttu-id="d6c18-177">在 hub 方法中擲回例外狀況會傳送至用戶端叫用方法。</span><span class="sxs-lookup"><span data-stu-id="d6c18-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="d6c18-178">在 JavaScript 用戶端`invoke`方法會傳回[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。</span><span class="sxs-lookup"><span data-stu-id="d6c18-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="d6c18-179">當用戶端會收到的錯誤處理常式附加至承諾使用`catch`，它已叫用，並傳遞為 JavaScript`Error`物件。</span><span class="sxs-lookup"><span data-stu-id="d6c18-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="d6c18-180">相關資源</span><span class="sxs-lookup"><span data-stu-id="d6c18-180">Related resources</span></span>

* [<span data-ttu-id="d6c18-181">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="d6c18-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="d6c18-182">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="d6c18-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="d6c18-183">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="d6c18-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
