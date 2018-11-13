---
title: 用於 ASP.NET Core SignalR 中樞
author: tdykstra
description: 了解如何使用 ASP.NET Core signalr 的中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/hubs
ms.openlocfilehash: 0413d354307208726f4252f431ac59526effed08
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569915"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="7ae18-103">使用 ASP.NET Core SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="7ae18-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="7ae18-104">藉由[Rachel Appel](https://twitter.com/rachelappel)和[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="7ae18-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="7ae18-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="7ae18-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="7ae18-106">什麼是 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="7ae18-106">What is a SignalR hub</span></span>

<span data-ttu-id="7ae18-107">SignalR 中樞 API 可讓您連線的用戶端上呼叫方法，從伺服器。</span><span class="sxs-lookup"><span data-stu-id="7ae18-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="7ae18-108">在伺服器程式碼中，您會定義用戶端所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="7ae18-109">在用戶端程式碼中，您可以定義會從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="7ae18-110">SignalR 會負責在幕後能夠即時的用戶端對伺服器和伺服器到用戶端通訊的所有項目。</span><span class="sxs-lookup"><span data-stu-id="7ae18-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="7ae18-111">設定 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="7ae18-111">Configure SignalR hubs</span></span>

<span data-ttu-id="7ae18-112">SignalR 中介軟體會需要某些服務，已藉由呼叫`services.AddSignalR`。</span><span class="sxs-lookup"><span data-stu-id="7ae18-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="7ae18-113">將 SignalR 功能加入至 ASP.NET Core 應用程式中，設定 SignalR 的路由，方法是呼叫`app.UseSignalR`在`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="7ae18-114">建立和使用中樞</span><span class="sxs-lookup"><span data-stu-id="7ae18-114">Create and use hubs</span></span>

<span data-ttu-id="7ae18-115">藉由宣告繼承自的類別建立中樞`Hub`，並為其新增公用方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="7ae18-116">用戶端可以呼叫方法定義為`public`。</span><span class="sxs-lookup"><span data-stu-id="7ae18-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="7ae18-117">您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="7ae18-118">SignalR 處理的序列化和還原序列化複雜物件並在您的參數和傳回值的陣列。</span><span class="sxs-lookup"><span data-stu-id="7ae18-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="7ae18-119">中樞是暫時性的：</span><span class="sxs-lookup"><span data-stu-id="7ae18-119">Hubs are transient:</span></span>
> * <span data-ttu-id="7ae18-120">不會將狀態儲存在中樞類別上的屬性。</span><span class="sxs-lookup"><span data-stu-id="7ae18-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="7ae18-121">每個中樞方法的呼叫會在新的中樞執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="7ae18-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="7ae18-122">使用`await`呼叫非同步方法，取決於中樞保持運作時。</span><span class="sxs-lookup"><span data-stu-id="7ae18-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="7ae18-123">比方說，這類方法`Clients.All.SendAsync(...)`如果在未呼叫可能會失敗`await`和中樞方法完成之後才`SendAsync`完成。</span><span class="sxs-lookup"><span data-stu-id="7ae18-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="7ae18-124">內容物件</span><span class="sxs-lookup"><span data-stu-id="7ae18-124">The Context object</span></span>

<span data-ttu-id="7ae18-125">`Hub`類別具有`Context`屬性，其中包含與連線相關資訊的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="7ae18-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="7ae18-126">屬性</span><span class="sxs-lookup"><span data-stu-id="7ae18-126">Property</span></span> | <span data-ttu-id="7ae18-127">描述</span><span class="sxs-lookup"><span data-stu-id="7ae18-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="7ae18-128">取得連接，SignalR 所指派的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="7ae18-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="7ae18-129">還有一個針對每個連線的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="7ae18-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="7ae18-130">取得[使用者識別碼](xref:signalr/groups)。</span><span class="sxs-lookup"><span data-stu-id="7ae18-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="7ae18-131">根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。</span><span class="sxs-lookup"><span data-stu-id="7ae18-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="7ae18-132">取得`ClaimsPrincipal`與目前的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="7ae18-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="7ae18-133">取得索引鍵/值集合，可用來共用此連線的範圍內的資料。</span><span class="sxs-lookup"><span data-stu-id="7ae18-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="7ae18-134">資料可以儲存在這個集合中，它會保存連接到不同的中樞方法叫用。</span><span class="sxs-lookup"><span data-stu-id="7ae18-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="7ae18-135">取得連接上的可用功能的集合。</span><span class="sxs-lookup"><span data-stu-id="7ae18-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="7ae18-136">現在，這個集合不需要在大部分情況下，因此它不尚未記載於詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7ae18-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="7ae18-137">取得`CancellationToken`連線中止時，可通知。</span><span class="sxs-lookup"><span data-stu-id="7ae18-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="7ae18-138">`Hub.Context` 也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="7ae18-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="7ae18-139">方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-139">Method</span></span> | <span data-ttu-id="7ae18-140">描述</span><span class="sxs-lookup"><span data-stu-id="7ae18-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="7ae18-141">傳回`HttpContext`進行連接，或`null`如果連接不是 HTTP 要求相關聯。</span><span class="sxs-lookup"><span data-stu-id="7ae18-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="7ae18-142">適用於 HTTP 連線，您可以使用這個方法，以取得資訊，例如 HTTP 標頭和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="7ae18-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="7ae18-143">中止連接。</span><span class="sxs-lookup"><span data-stu-id="7ae18-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="7ae18-144">用戶端物件</span><span class="sxs-lookup"><span data-stu-id="7ae18-144">The Clients object</span></span>

<span data-ttu-id="7ae18-145">`Hub`類別具有`Clients`屬性，其中包含伺服器與用戶端之間通訊的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="7ae18-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="7ae18-146">屬性</span><span class="sxs-lookup"><span data-stu-id="7ae18-146">Property</span></span> | <span data-ttu-id="7ae18-147">描述</span><span class="sxs-lookup"><span data-stu-id="7ae18-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="7ae18-148">所有連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="7ae18-149">叫用中樞方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="7ae18-150">所有連線的用戶端，除了叫用方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-150">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="7ae18-151">`Hub.Clients` 也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="7ae18-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="7ae18-152">方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-152">Method</span></span> | <span data-ttu-id="7ae18-153">描述</span><span class="sxs-lookup"><span data-stu-id="7ae18-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="7ae18-154">所有連線的用戶端，除了指定的連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="7ae18-155">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="7ae18-156">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="7ae18-157">指定群組中的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="7ae18-158">在指定群組中，除非指定連線的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="7ae18-159">多個群組的連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="7ae18-160">在群組的連線，不包括叫用中樞方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="7ae18-161">與特定使用者相關聯的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="7ae18-162">與指定的使用者相關聯的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="7ae18-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="7ae18-163">每個屬性或方法，上述資料表中的傳回的物件`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="7ae18-164">`SendAsync`方法可讓您提供的名稱和用戶端方法呼叫的參數。</span><span class="sxs-lookup"><span data-stu-id="7ae18-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="7ae18-165">將訊息傳送至用戶端</span><span class="sxs-lookup"><span data-stu-id="7ae18-165">Send messages to clients</span></span>

<span data-ttu-id="7ae18-166">若要為特定用戶端的呼叫，使用的屬性`Clients`物件。</span><span class="sxs-lookup"><span data-stu-id="7ae18-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="7ae18-167">在下列範例中，有三個中樞的方法：</span><span class="sxs-lookup"><span data-stu-id="7ae18-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="7ae18-168">`SendMessage` 將訊息傳送至所有已連線的用戶端，使用`Clients.All`。</span><span class="sxs-lookup"><span data-stu-id="7ae18-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="7ae18-169">`SendMessageToCaller` 將訊息傳送至呼叫端，使用`Clients.Caller`。</span><span class="sxs-lookup"><span data-stu-id="7ae18-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="7ae18-170">`SendMessageToGroups` 將訊息傳送至所有用戶端`SignalR Users`群組。</span><span class="sxs-lookup"><span data-stu-id="7ae18-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="7ae18-171">強型別的中樞</span><span class="sxs-lookup"><span data-stu-id="7ae18-171">Strongly typed hubs</span></span>

<span data-ttu-id="7ae18-172">使用的一項缺點`SendAsync`是它依賴 magic 的字串，以指定要呼叫的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="7ae18-173">這可讓程式碼開啟，如果方法名稱的拼字錯誤的執行階段錯誤或遺漏的用戶端。</span><span class="sxs-lookup"><span data-stu-id="7ae18-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="7ae18-174">使用替代`SendAsync`是強型別`Hub`使用<xref:Microsoft.AspNetCore.SignalR.Hub`1>。</span><span class="sxs-lookup"><span data-stu-id="7ae18-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="7ae18-175">在下列範例中，`ChatHub`用戶端方法已成呼叫登出擷取`IChatClient`。</span><span class="sxs-lookup"><span data-stu-id="7ae18-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="7ae18-176">此介面可用來重構上述`ChatHub`範例。</span><span class="sxs-lookup"><span data-stu-id="7ae18-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="7ae18-177">使用`Hub<IChatClient>`啟用編譯時間檢查的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="7ae18-178">這可避免使用魔術字串，因為所造成的問題`Hub<T>`只能提供存取的介面中定義的方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="7ae18-179">使用強型別`Hub<T>`停用重新使用`SendAsync`。</span><span class="sxs-lookup"><span data-stu-id="7ae18-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="7ae18-180">變更中樞方法的名稱</span><span class="sxs-lookup"><span data-stu-id="7ae18-180">Change the name of a hub method</span></span>

<span data-ttu-id="7ae18-181">根據預設，伺服器中樞的方法名稱會是.NET 方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="7ae18-181">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="7ae18-182">不過，您可以使用[HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute)屬性來變更這個預設值，並且手動指定方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="7ae18-182">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="7ae18-183">用戶端在叫用方法時，應該使用這個名稱，而不是.NET 方法名稱。</span><span class="sxs-lookup"><span data-stu-id="7ae18-183">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="7ae18-184">處理連接事件</span><span class="sxs-lookup"><span data-stu-id="7ae18-184">Handle events for a connection</span></span>

<span data-ttu-id="7ae18-185">提供 SignalR 中樞 API`OnConnectedAsync`和`OnDisconnectedAsync`來管理和追蹤連線的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-185">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="7ae18-186">覆寫`OnConnectedAsync`虛擬方法，以執行動作，當用戶端連線至中樞，例如將它新增至群組。</span><span class="sxs-lookup"><span data-stu-id="7ae18-186">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="7ae18-187">覆寫`OnDisconnectedAsync`虛擬方法，用戶端中斷連線時執行動作。</span><span class="sxs-lookup"><span data-stu-id="7ae18-187">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="7ae18-188">如果用戶端刻意中斷連線 (藉由呼叫`connection.stop()`，例如)，則`exception`參數將會是`null`。</span><span class="sxs-lookup"><span data-stu-id="7ae18-188">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="7ae18-189">不過，如果用戶端已中斷連接錯誤 （例如網路失敗），因為`exception`參數會包含描述失敗的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7ae18-189">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="7ae18-190">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="7ae18-190">Handle errors</span></span>

<span data-ttu-id="7ae18-191">在 hub 方法中擲回例外狀況會傳送至用戶端叫用方法。</span><span class="sxs-lookup"><span data-stu-id="7ae18-191">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="7ae18-192">在 JavaScript 用戶端`invoke`方法會傳回[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。</span><span class="sxs-lookup"><span data-stu-id="7ae18-192">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="7ae18-193">當用戶端會收到的錯誤處理常式附加至承諾使用`catch`，它已叫用，並傳遞為 JavaScript`Error`物件。</span><span class="sxs-lookup"><span data-stu-id="7ae18-193">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="7ae18-194">根據預設，如果您的中樞擲回例外狀況，SignalR 泛型錯誤訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="7ae18-194">By default, if your Hub throws an exception, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="7ae18-195">例如: </span><span class="sxs-lookup"><span data-stu-id="7ae18-195">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="7ae18-196">未預期的例外狀況通常會包含機密資訊，例如資料庫連線失敗時，觸發例外狀況中的資料庫伺服器的名稱。</span><span class="sxs-lookup"><span data-stu-id="7ae18-196">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="7ae18-197">SignalR 不公開預設這些詳細的錯誤訊息，基於安全性考量。</span><span class="sxs-lookup"><span data-stu-id="7ae18-197">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="7ae18-198">請參閱[安全性考量文章](xref:signalr/security#exceptions)詳細了解為何會隱藏例外狀況詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7ae18-198">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="7ae18-199">如果您有例外狀況您*請勿*想要傳播至用戶端，您可以使用`HubException`類別。</span><span class="sxs-lookup"><span data-stu-id="7ae18-199">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="7ae18-200">如果您擲回`HubException`從您的中樞方法、 SignalR**將**整個訊息傳送至用戶端，未經修改的狀態。</span><span class="sxs-lookup"><span data-stu-id="7ae18-200">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="7ae18-201">只會傳送 SignalR`Message`例外狀況至用戶端的屬性。</span><span class="sxs-lookup"><span data-stu-id="7ae18-201">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="7ae18-202">堆疊追蹤和例外狀況的其他屬性無法使用用戶端。</span><span class="sxs-lookup"><span data-stu-id="7ae18-202">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="7ae18-203">相關資源</span><span class="sxs-lookup"><span data-stu-id="7ae18-203">Related resources</span></span>

* [<span data-ttu-id="7ae18-204">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="7ae18-204">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="7ae18-205">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="7ae18-205">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="7ae18-206">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="7ae18-206">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
