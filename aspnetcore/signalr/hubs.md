---
title: 用於 ASP.NET Core SignalR 中樞
author: tdykstra
description: 了解如何使用 ASP.NET Core signalr 的中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 91f92e9d6b776457cd319965d548ee401ddc5e0e
ms.sourcegitcommit: 4225e2c49a0081e6ac15acff673587201f54b4aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282133"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="c1800-103">使用 ASP.NET Core SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="c1800-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="c1800-104">藉由[Rachel Appel](https://twitter.com/rachelappel)和[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="c1800-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="c1800-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c1800-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="c1800-106">什麼是 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="c1800-106">What is a SignalR hub</span></span>

<span data-ttu-id="c1800-107">SignalR 中樞 API 可讓您連線的用戶端上呼叫方法，從伺服器。</span><span class="sxs-lookup"><span data-stu-id="c1800-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="c1800-108">在伺服器程式碼中，您會定義用戶端所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="c1800-109">在用戶端程式碼中，您可以定義會從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="c1800-110">SignalR 會負責在幕後能夠即時的用戶端對伺服器和伺服器到用戶端通訊的所有項目。</span><span class="sxs-lookup"><span data-stu-id="c1800-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="c1800-111">設定 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="c1800-111">Configure SignalR hubs</span></span>

<span data-ttu-id="c1800-112">SignalR 中介軟體會需要某些服務，已藉由呼叫`services.AddSignalR`。</span><span class="sxs-lookup"><span data-stu-id="c1800-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="c1800-113">將 SignalR 功能加入至 ASP.NET Core 應用程式中，設定 SignalR 的路由，方法是呼叫`app.UseSignalR`在`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="c1800-114">建立和使用中樞</span><span class="sxs-lookup"><span data-stu-id="c1800-114">Create and use hubs</span></span>

<span data-ttu-id="c1800-115">藉由宣告繼承自的類別建立中樞`Hub`，並為其新增公用方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="c1800-116">用戶端可以呼叫方法定義為`public`。</span><span class="sxs-lookup"><span data-stu-id="c1800-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="c1800-117">您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="c1800-118">SignalR 處理的序列化和還原序列化複雜物件並在您的參數和傳回值的陣列。</span><span class="sxs-lookup"><span data-stu-id="c1800-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="c1800-119">中樞是暫時性的：</span><span class="sxs-lookup"><span data-stu-id="c1800-119">Hubs are transient:</span></span>
> * <span data-ttu-id="c1800-120">不會將狀態儲存在中樞類別上的屬性。</span><span class="sxs-lookup"><span data-stu-id="c1800-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="c1800-121">每個中樞方法的呼叫會在新的中樞執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="c1800-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="c1800-122">使用`await`呼叫非同步方法，取決於中樞保持運作時。</span><span class="sxs-lookup"><span data-stu-id="c1800-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="c1800-123">比方說，這類方法`Clients.All.SendAsync(...)`如果在未呼叫可能會失敗`await`和中樞方法完成之後才`SendAsync`完成。</span><span class="sxs-lookup"><span data-stu-id="c1800-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="c1800-124">內容物件</span><span class="sxs-lookup"><span data-stu-id="c1800-124">The Context object</span></span>

<span data-ttu-id="c1800-125">`Hub`類別具有`Context`屬性，其中包含與連線相關資訊的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="c1800-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="c1800-126">屬性</span><span class="sxs-lookup"><span data-stu-id="c1800-126">Property</span></span> | <span data-ttu-id="c1800-127">描述</span><span class="sxs-lookup"><span data-stu-id="c1800-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="c1800-128">取得連接，SignalR 所指派的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="c1800-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="c1800-129">還有一個針對每個連線的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="c1800-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="c1800-130">取得[使用者識別碼](xref:signalr/groups)。</span><span class="sxs-lookup"><span data-stu-id="c1800-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="c1800-131">根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。</span><span class="sxs-lookup"><span data-stu-id="c1800-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="c1800-132">取得`ClaimsPrincipal`與目前的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="c1800-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="c1800-133">取得索引鍵/值集合，可用來共用此連線的範圍內的資料。</span><span class="sxs-lookup"><span data-stu-id="c1800-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="c1800-134">資料可以儲存在這個集合中，它會保存連接到不同的中樞方法叫用。</span><span class="sxs-lookup"><span data-stu-id="c1800-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="c1800-135">取得連接上的可用功能的集合。</span><span class="sxs-lookup"><span data-stu-id="c1800-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="c1800-136">現在，這個集合不需要在大部分情況下，因此它不尚未記載於詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c1800-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="c1800-137">取得`CancellationToken`連線中止時，可通知。</span><span class="sxs-lookup"><span data-stu-id="c1800-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="c1800-138">`Hub.Context` 也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="c1800-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="c1800-139">方法</span><span class="sxs-lookup"><span data-stu-id="c1800-139">Method</span></span> | <span data-ttu-id="c1800-140">描述</span><span class="sxs-lookup"><span data-stu-id="c1800-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="c1800-141">傳回`HttpContext`進行連接，或`null`如果連接不是 HTTP 要求相關聯。</span><span class="sxs-lookup"><span data-stu-id="c1800-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="c1800-142">適用於 HTTP 連線，您可以使用這個方法，以取得資訊，例如 HTTP 標頭和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="c1800-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="c1800-143">中止連接。</span><span class="sxs-lookup"><span data-stu-id="c1800-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="c1800-144">用戶端物件</span><span class="sxs-lookup"><span data-stu-id="c1800-144">The Clients object</span></span>

<span data-ttu-id="c1800-145">`Hub`類別具有`Clients`屬性，其中包含伺服器與用戶端之間通訊的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="c1800-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="c1800-146">屬性</span><span class="sxs-lookup"><span data-stu-id="c1800-146">Property</span></span> | <span data-ttu-id="c1800-147">描述</span><span class="sxs-lookup"><span data-stu-id="c1800-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="c1800-148">所有連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="c1800-149">叫用中樞方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="c1800-150">所有連線的用戶端，除了叫用方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-150">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="c1800-151">`Hub.Clients` 也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="c1800-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="c1800-152">方法</span><span class="sxs-lookup"><span data-stu-id="c1800-152">Method</span></span> | <span data-ttu-id="c1800-153">描述</span><span class="sxs-lookup"><span data-stu-id="c1800-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="c1800-154">所有連線的用戶端，除了指定的連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="c1800-155">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="c1800-156">特定連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="c1800-157">指定群組中的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="c1800-158">在指定群組中，除非指定連線的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="c1800-159">多個群組的連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="c1800-160">在群組的連線，不包括叫用中樞方法的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="c1800-161">與特定使用者相關聯的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="c1800-162">與指定的使用者相關聯的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="c1800-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="c1800-163">每個屬性或方法，上述資料表中的傳回的物件`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="c1800-164">`SendAsync`方法可讓您提供的名稱和用戶端方法呼叫的參數。</span><span class="sxs-lookup"><span data-stu-id="c1800-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="c1800-165">將訊息傳送至用戶端</span><span class="sxs-lookup"><span data-stu-id="c1800-165">Send messages to clients</span></span>

<span data-ttu-id="c1800-166">若要為特定用戶端的呼叫，使用的屬性`Clients`物件。</span><span class="sxs-lookup"><span data-stu-id="c1800-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="c1800-167">在下列範例中，有三個中樞的方法：</span><span class="sxs-lookup"><span data-stu-id="c1800-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="c1800-168">`SendMessage` 將訊息傳送至所有已連線的用戶端，使用`Clients.All`。</span><span class="sxs-lookup"><span data-stu-id="c1800-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="c1800-169">`SendMessageToCaller` 將訊息傳送至呼叫端，使用`Clients.Caller`。</span><span class="sxs-lookup"><span data-stu-id="c1800-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="c1800-170">`SendMessageToGroups` 將訊息傳送至所有用戶端`SignalR Users`群組。</span><span class="sxs-lookup"><span data-stu-id="c1800-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="c1800-171">強型別的中樞</span><span class="sxs-lookup"><span data-stu-id="c1800-171">Strongly typed hubs</span></span>

<span data-ttu-id="c1800-172">使用的一項缺點`SendAsync`是它依賴 magic 的字串，以指定要呼叫的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="c1800-173">這可讓程式碼開啟，如果方法名稱的拼字錯誤的執行階段錯誤或遺漏的用戶端。</span><span class="sxs-lookup"><span data-stu-id="c1800-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="c1800-174">使用替代`SendAsync`是強型別`Hub`使用<xref:Microsoft.AspNetCore.SignalR.Hub`1>。</span><span class="sxs-lookup"><span data-stu-id="c1800-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="c1800-175">在下列範例中，`ChatHub`用戶端方法已成呼叫登出擷取`IChatClient`。</span><span class="sxs-lookup"><span data-stu-id="c1800-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="c1800-176">此介面可用來重構上述`ChatHub`範例。</span><span class="sxs-lookup"><span data-stu-id="c1800-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="c1800-177">使用`Hub<IChatClient>`啟用編譯時間檢查的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="c1800-178">這可避免使用魔術字串，因為所造成的問題`Hub<T>`只能提供存取的介面中定義的方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="c1800-179">使用強型別`Hub<T>`停用重新使用`SendAsync`。</span><span class="sxs-lookup"><span data-stu-id="c1800-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="c1800-180">介面上定義任何方法仍然可以定義會以非同步的。</span><span class="sxs-lookup"><span data-stu-id="c1800-180">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="c1800-181">事實上，每一種方法應傳回`Task`。</span><span class="sxs-lookup"><span data-stu-id="c1800-181">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="c1800-182">因為它是一個介面，請勿使用`async`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="c1800-182">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="c1800-183">例如: </span><span class="sxs-lookup"><span data-stu-id="c1800-183">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="c1800-184">`Async`尾碼不會移除與方法名稱。</span><span class="sxs-lookup"><span data-stu-id="c1800-184">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="c1800-185">除非您用戶端的方法以定義`.on('MyMethodAsync')`，您不應該使用`MyMethodAsync`做為名稱。</span><span class="sxs-lookup"><span data-stu-id="c1800-185">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="c1800-186">變更中樞方法的名稱</span><span class="sxs-lookup"><span data-stu-id="c1800-186">Change the name of a hub method</span></span>

<span data-ttu-id="c1800-187">根據預設，伺服器中樞的方法名稱會是.NET 方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="c1800-187">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="c1800-188">不過，您可以使用[HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute)屬性來變更這個預設值，並且手動指定方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="c1800-188">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="c1800-189">用戶端在叫用方法時，應該使用這個名稱，而不是.NET 方法名稱。</span><span class="sxs-lookup"><span data-stu-id="c1800-189">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="c1800-190">處理連接事件</span><span class="sxs-lookup"><span data-stu-id="c1800-190">Handle events for a connection</span></span>

<span data-ttu-id="c1800-191">提供 SignalR 中樞 API`OnConnectedAsync`和`OnDisconnectedAsync`來管理和追蹤連線的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-191">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="c1800-192">覆寫`OnConnectedAsync`虛擬方法，以執行動作，當用戶端連線至中樞，例如將它新增至群組。</span><span class="sxs-lookup"><span data-stu-id="c1800-192">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="c1800-193">覆寫`OnDisconnectedAsync`虛擬方法，用戶端中斷連線時執行動作。</span><span class="sxs-lookup"><span data-stu-id="c1800-193">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="c1800-194">如果用戶端刻意中斷連線 (藉由呼叫`connection.stop()`，例如)，則`exception`參數將會是`null`。</span><span class="sxs-lookup"><span data-stu-id="c1800-194">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="c1800-195">不過，如果用戶端已中斷連接錯誤 （例如網路失敗），因為`exception`參數會包含描述失敗的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c1800-195">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="c1800-196">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="c1800-196">Handle errors</span></span>

<span data-ttu-id="c1800-197">在 hub 方法中擲回例外狀況會傳送至用戶端叫用方法。</span><span class="sxs-lookup"><span data-stu-id="c1800-197">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="c1800-198">在 JavaScript 用戶端`invoke`方法會傳回[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。</span><span class="sxs-lookup"><span data-stu-id="c1800-198">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="c1800-199">當用戶端會收到的錯誤處理常式附加至承諾使用`catch`，它已叫用，並傳遞為 JavaScript`Error`物件。</span><span class="sxs-lookup"><span data-stu-id="c1800-199">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="c1800-200">如果您的中樞擲回例外狀況，不關閉連線。</span><span class="sxs-lookup"><span data-stu-id="c1800-200">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="c1800-201">根據預設，SignalR 會傳回給用戶端的一般錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c1800-201">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="c1800-202">例如: </span><span class="sxs-lookup"><span data-stu-id="c1800-202">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="c1800-203">未預期的例外狀況通常會包含機密資訊，例如資料庫連線失敗時，觸發例外狀況中的資料庫伺服器的名稱。</span><span class="sxs-lookup"><span data-stu-id="c1800-203">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="c1800-204">SignalR 不公開預設這些詳細的錯誤訊息，基於安全性考量。</span><span class="sxs-lookup"><span data-stu-id="c1800-204">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="c1800-205">請參閱[安全性考量文章](xref:signalr/security#exceptions)詳細了解為何會隱藏例外狀況詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c1800-205">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="c1800-206">如果您有例外狀況您*請勿*想要傳播至用戶端，您可以使用`HubException`類別。</span><span class="sxs-lookup"><span data-stu-id="c1800-206">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="c1800-207">如果您擲回`HubException`從您的中樞方法、 SignalR**將**整個訊息傳送至用戶端，未經修改的狀態。</span><span class="sxs-lookup"><span data-stu-id="c1800-207">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="c1800-208">只會傳送 SignalR`Message`例外狀況至用戶端的屬性。</span><span class="sxs-lookup"><span data-stu-id="c1800-208">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="c1800-209">堆疊追蹤和例外狀況的其他屬性無法使用用戶端。</span><span class="sxs-lookup"><span data-stu-id="c1800-209">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="c1800-210">相關資源</span><span class="sxs-lookup"><span data-stu-id="c1800-210">Related resources</span></span>

* [<span data-ttu-id="c1800-211">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="c1800-211">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="c1800-212">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="c1800-212">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c1800-213">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="c1800-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
