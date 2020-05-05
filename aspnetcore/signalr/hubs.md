---
title: 在 ASP.NET Core 中使用中樞SignalR
author: bradygaster
description: 瞭解如何在 ASP.NET Core SignalR中使用中樞。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/hubs
ms.openlocfilehash: 6ea8a8e9ffb6549a285f320eb0a4a2e5d218483a
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775215"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="160b4-103">使用 SignalR for ASP.NET Core 的中樞</span><span class="sxs-lookup"><span data-stu-id="160b4-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="160b4-104">By [Rachel Appel](https://twitter.com/rachelappel)和[古柯 Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="160b4-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="160b4-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [（如何下載）](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="160b4-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="160b4-106">什麼是 SignalR hub</span><span class="sxs-lookup"><span data-stu-id="160b4-106">What is a SignalR hub</span></span>

<span data-ttu-id="160b4-107">SignalR 中樞 API 可讓您從伺服器呼叫已連線用戶端上的方法。</span><span class="sxs-lookup"><span data-stu-id="160b4-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="160b4-108">在伺服器程式碼中，您可以定義用戶端所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="160b4-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="160b4-109">在用戶端程式代碼中，您可以定義從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="160b4-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="160b4-110">SignalR 會處理幕後的所有內容，讓您能夠進行即時的用戶端對伺服器和伺服器對用戶端通訊。</span><span class="sxs-lookup"><span data-stu-id="160b4-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="160b4-111">設定 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="160b4-111">Configure SignalR hubs</span></span>

<span data-ttu-id="160b4-112">SignalR 中介軟體需要一些服務，透過呼叫`services.AddSignalR`來設定。</span><span class="sxs-lookup"><span data-stu-id="160b4-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="160b4-113">將 SignalR 功能新增至 ASP.NET Core 應用程式時，請在`endpoint.MapHub` `Startup.Configure`方法的`app.UseEndpoints`回呼中呼叫以設定 SignalR 路由。</span><span class="sxs-lookup"><span data-stu-id="160b4-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `endpoint.MapHub` in the `Startup.Configure` method's `app.UseEndpoints` callback.</span></span>

```csharp
app.UseRouting();
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/chathub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="160b4-114">將 SignalR 功能新增至 ASP.NET Core 應用程式時，請在`app.UseSignalR` `Startup.Configure`方法中呼叫以設定 SignalR 路由。</span><span class="sxs-lookup"><span data-stu-id="160b4-114">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

::: moniker-end

## <a name="create-and-use-hubs"></a><span data-ttu-id="160b4-115">建立和使用中樞</span><span class="sxs-lookup"><span data-stu-id="160b4-115">Create and use hubs</span></span>

<span data-ttu-id="160b4-116">藉由宣告繼承自`Hub`的類別來建立中樞，並在其中新增公用方法。</span><span class="sxs-lookup"><span data-stu-id="160b4-116">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="160b4-117">用戶端可以呼叫定義為`public`的方法。</span><span class="sxs-lookup"><span data-stu-id="160b4-117">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="160b4-118">您可以指定傳回類型和參數，包括複雜類型和陣列，就像在任何 c # 方法中一樣。</span><span class="sxs-lookup"><span data-stu-id="160b4-118">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="160b4-119">SignalR 會在您的參數和傳回值中處理複雜物件和陣列的序列化和還原序列化。</span><span class="sxs-lookup"><span data-stu-id="160b4-119">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="160b4-120">中樞為暫時性：</span><span class="sxs-lookup"><span data-stu-id="160b4-120">Hubs are transient:</span></span>
>
> * <span data-ttu-id="160b4-121">不要在中樞類別的屬性中儲存狀態。</span><span class="sxs-lookup"><span data-stu-id="160b4-121">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="160b4-122">每個中樞方法呼叫都會在新的中樞實例上執行。</span><span class="sxs-lookup"><span data-stu-id="160b4-122">Every hub method call is executed on a new hub instance.</span></span>
> * <span data-ttu-id="160b4-123">呼叫`await`相依于中樞保持運作的非同步方法時，請使用。</span><span class="sxs-lookup"><span data-stu-id="160b4-123">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="160b4-124">例如，如果在沒有的情況`Clients.All.SendAsync(...)`下`await`呼叫，且中樞方法在完成之前`SendAsync`完成，則之類的方法可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="160b4-124">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="160b4-125">內容物件</span><span class="sxs-lookup"><span data-stu-id="160b4-125">The Context object</span></span>

<span data-ttu-id="160b4-126">`Hub`類別具有`Context`屬性，其中包含下列具有連接相關資訊的屬性：</span><span class="sxs-lookup"><span data-stu-id="160b4-126">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="160b4-127">屬性</span><span class="sxs-lookup"><span data-stu-id="160b4-127">Property</span></span> | <span data-ttu-id="160b4-128">說明</span><span class="sxs-lookup"><span data-stu-id="160b4-128">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="160b4-129">取得連接的唯一識別碼，由 SignalR 指派。</span><span class="sxs-lookup"><span data-stu-id="160b4-129">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="160b4-130">每個連接都有一個連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="160b4-130">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="160b4-131">取得[使用者識別碼](xref:signalr/groups)。</span><span class="sxs-lookup"><span data-stu-id="160b4-131">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="160b4-132">根據預設，SignalR 會使用`ClaimTypes.NameIdentifier`與連接`ClaimsPrincipal`相關聯的，做為使用者識別碼。</span><span class="sxs-lookup"><span data-stu-id="160b4-132">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="160b4-133">取得與`ClaimsPrincipal`目前使用者相關聯的。</span><span class="sxs-lookup"><span data-stu-id="160b4-133">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="160b4-134">取得索引鍵/值集合，可用來在此連接的範圍內共用資料。</span><span class="sxs-lookup"><span data-stu-id="160b4-134">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="160b4-135">資料可以儲存在此集合中，且會保存在不同中樞方法叫用的連接中。</span><span class="sxs-lookup"><span data-stu-id="160b4-135">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="160b4-136">取得連接上可用的功能集合。</span><span class="sxs-lookup"><span data-stu-id="160b4-136">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="160b4-137">目前，在大部分的情況下不需要此集合，因此尚未詳細記載。</span><span class="sxs-lookup"><span data-stu-id="160b4-137">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="160b4-138">取得當`CancellationToken`連接中止時，會通知的。</span><span class="sxs-lookup"><span data-stu-id="160b4-138">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="160b4-139">`Hub.Context`也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="160b4-139">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="160b4-140">方法</span><span class="sxs-lookup"><span data-stu-id="160b4-140">Method</span></span> | <span data-ttu-id="160b4-141">描述</span><span class="sxs-lookup"><span data-stu-id="160b4-141">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="160b4-142">如果連接未與 HTTP 要求相關聯，則會傳回連接的`HttpContext` `null`</span><span class="sxs-lookup"><span data-stu-id="160b4-142">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="160b4-143">針對 HTTP 連線，您可以使用這個方法來取得資訊，例如 HTTP 標頭和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="160b4-143">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="160b4-144">中止連線。</span><span class="sxs-lookup"><span data-stu-id="160b4-144">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="160b4-145">用戶端物件</span><span class="sxs-lookup"><span data-stu-id="160b4-145">The Clients object</span></span>

<span data-ttu-id="160b4-146">`Hub`類別具有`Clients`屬性，其中包含伺服器和用戶端之間通訊的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="160b4-146">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="160b4-147">屬性</span><span class="sxs-lookup"><span data-stu-id="160b4-147">Property</span></span> | <span data-ttu-id="160b4-148">說明</span><span class="sxs-lookup"><span data-stu-id="160b4-148">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="160b4-149">在所有已連線的用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="160b4-149">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="160b4-150">呼叫用戶端上叫用中樞方法的方法</span><span class="sxs-lookup"><span data-stu-id="160b4-150">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="160b4-151">在所有已連線的用戶端上呼叫方法，但叫用方法的用戶端除外</span><span class="sxs-lookup"><span data-stu-id="160b4-151">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="160b4-152">`Hub.Clients`也包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="160b4-152">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="160b4-153">方法</span><span class="sxs-lookup"><span data-stu-id="160b4-153">Method</span></span> | <span data-ttu-id="160b4-154">描述</span><span class="sxs-lookup"><span data-stu-id="160b4-154">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="160b4-155">在所有已連線的用戶端上呼叫方法，但指定的連接除外</span><span class="sxs-lookup"><span data-stu-id="160b4-155">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="160b4-156">在特定的已連線用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="160b4-156">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="160b4-157">在特定的已連線用戶端上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="160b4-157">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="160b4-158">在指定群組中的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="160b4-158">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="160b4-159">在指定之群組中的所有連接上呼叫方法，但指定的連接除外</span><span class="sxs-lookup"><span data-stu-id="160b4-159">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="160b4-160">在多個連接群組上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="160b4-160">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="160b4-161">在一組連接上呼叫方法，但不包括叫用中樞方法的用戶端</span><span class="sxs-lookup"><span data-stu-id="160b4-161">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="160b4-162">在與特定使用者相關聯的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="160b4-162">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="160b4-163">在與指定使用者相關聯的所有連接上呼叫方法</span><span class="sxs-lookup"><span data-stu-id="160b4-163">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="160b4-164">上表中的每個屬性或方法都會傳回具有`SendAsync`方法的物件。</span><span class="sxs-lookup"><span data-stu-id="160b4-164">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="160b4-165">`SendAsync`方法可讓您提供要呼叫之用戶端方法的名稱和參數。</span><span class="sxs-lookup"><span data-stu-id="160b4-165">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="160b4-166">將訊息傳送至用戶端</span><span class="sxs-lookup"><span data-stu-id="160b4-166">Send messages to clients</span></span>

<span data-ttu-id="160b4-167">若要對特定用戶端進行呼叫，請使用`Clients`物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="160b4-167">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="160b4-168">在下列範例中，有三個中樞方法：</span><span class="sxs-lookup"><span data-stu-id="160b4-168">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="160b4-169">`SendMessage`使用`Clients.All`將訊息傳送至所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="160b4-169">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="160b4-170">`SendMessageToCaller`使用`Clients.Caller`，將訊息傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="160b4-170">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="160b4-171">`SendMessageToGroups`將訊息傳送至群組中的`SignalR Users`所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="160b4-171">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="160b4-172">強型別中樞</span><span class="sxs-lookup"><span data-stu-id="160b4-172">Strongly typed hubs</span></span>

<span data-ttu-id="160b4-173">使用`SendAsync`的缺點是，它依賴魔術字串來指定要呼叫的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="160b4-173">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="160b4-174">如果用戶端的方法名稱拼錯或遺失，這就會讓程式碼保持開啟，直到發生執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="160b4-174">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="160b4-175">使用`SendAsync`的另一種方法是，使用`Hub`強<xref:Microsoft.AspNetCore.SignalR.Hub%601>型別。</span><span class="sxs-lookup"><span data-stu-id="160b4-175">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span></span> <span data-ttu-id="160b4-176">在下列範例中， `ChatHub`用戶端方法已解壓縮至名`IChatClient`為的介面。</span><span class="sxs-lookup"><span data-stu-id="160b4-176">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="160b4-177">這個介面可以用來重構上述`ChatHub`範例。</span><span class="sxs-lookup"><span data-stu-id="160b4-177">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="160b4-178">使用`Hub<IChatClient>`可啟用用戶端方法的編譯時間檢查。</span><span class="sxs-lookup"><span data-stu-id="160b4-178">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="160b4-179">這可防止使用魔術字串所造成的問題`Hub<T>` ，因為只能提供對介面中定義之方法的存取權。</span><span class="sxs-lookup"><span data-stu-id="160b4-179">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="160b4-180">使用強型別`Hub<T>`會停用使用`SendAsync`的功能。</span><span class="sxs-lookup"><span data-stu-id="160b4-180">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="160b4-181">在介面上定義的任何方法仍可定義為非同步。</span><span class="sxs-lookup"><span data-stu-id="160b4-181">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="160b4-182">事實上，每個方法都應該傳回`Task`。</span><span class="sxs-lookup"><span data-stu-id="160b4-182">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="160b4-183">由於它是介面，請不要使用`async`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="160b4-183">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="160b4-184">例如：</span><span class="sxs-lookup"><span data-stu-id="160b4-184">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="160b4-185">`Async`尾碼不會從方法名稱中移除。</span><span class="sxs-lookup"><span data-stu-id="160b4-185">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="160b4-186">除非您的用戶端方法是`.on('MyMethodAsync')`以定義，否則`MyMethodAsync`您不應該使用做為名稱。</span><span class="sxs-lookup"><span data-stu-id="160b4-186">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="160b4-187">變更中樞方法的名稱</span><span class="sxs-lookup"><span data-stu-id="160b4-187">Change the name of a hub method</span></span>

<span data-ttu-id="160b4-188">根據預設，伺服器中樞方法名稱是 .NET 方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="160b4-188">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="160b4-189">不過，您可以使用[HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute)屬性來變更此預設值，並手動指定方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="160b4-189">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="160b4-190">叫用方法時，用戶端應該使用這個名稱，而不是 .NET 方法名稱。</span><span class="sxs-lookup"><span data-stu-id="160b4-190">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="160b4-191">處理連接的事件</span><span class="sxs-lookup"><span data-stu-id="160b4-191">Handle events for a connection</span></span>

<span data-ttu-id="160b4-192">SignalR中樞 API 提供`OnConnectedAsync`和`OnDisconnectedAsync`虛擬方法來管理和追蹤連接。</span><span class="sxs-lookup"><span data-stu-id="160b4-192">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="160b4-193">覆寫`OnConnectedAsync`虛擬方法，以在用戶端連線至中樞時執行動作，例如將其新增至群組。</span><span class="sxs-lookup"><span data-stu-id="160b4-193">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="160b4-194">覆寫`OnDisconnectedAsync`虛擬方法，以在用戶端中斷連線時執行動作。</span><span class="sxs-lookup"><span data-stu-id="160b4-194">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="160b4-195">如果用戶端刻意中斷連接（例如`connection.stop()`，藉由呼叫）， `exception`則參數會`null`是。</span><span class="sxs-lookup"><span data-stu-id="160b4-195">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="160b4-196">不過，如果用戶端因為錯誤（例如網路故障）而中斷連線，此`exception`參數將會包含描述失敗的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="160b4-196">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

[!INCLUDE[](~/includes/connectionid-signalr.md)]

## <a name="handle-errors"></a><span data-ttu-id="160b4-197">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="160b4-197">Handle errors</span></span>

<span data-ttu-id="160b4-198">在您的中樞方法中擲回的例外狀況會傳送至叫用方法的用戶端。</span><span class="sxs-lookup"><span data-stu-id="160b4-198">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="160b4-199">在 JavaScript 用戶端上， `invoke`方法會傳回[javascript 承諾](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)。</span><span class="sxs-lookup"><span data-stu-id="160b4-199">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="160b4-200">當用戶端收到錯誤，並使用`catch`附加至承諾的處理常式時，它會被叫用並當做`Error` JavaScript 物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="160b4-200">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="160b4-201">如果您的中樞擲回例外狀況，則不會關閉連接。</span><span class="sxs-lookup"><span data-stu-id="160b4-201">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="160b4-202">根據預設， SignalR會將一般錯誤訊息傳回到用戶端。</span><span class="sxs-lookup"><span data-stu-id="160b4-202">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="160b4-203">例如：</span><span class="sxs-lookup"><span data-stu-id="160b4-203">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="160b4-204">非預期的例外狀況通常包含敏感性資訊，例如資料庫連接失敗時所觸發的例外狀況中的資料庫伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="160b4-204">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> SignalR<span data-ttu-id="160b4-205">預設不會公開這些詳細的錯誤訊息做為安全性措施。</span><span class="sxs-lookup"><span data-stu-id="160b4-205"> doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="160b4-206">如需隱藏例外狀況詳細資料的詳細資訊，請參閱[安全性考慮一文](xref:signalr/security#exceptions)。</span><span class="sxs-lookup"><span data-stu-id="160b4-206">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="160b4-207">如果您想要將例外狀況傳播至用戶端 *，您可以*使用`HubException`類別。</span><span class="sxs-lookup"><span data-stu-id="160b4-207">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="160b4-208">如果您`HubException`從中樞方法擲回， SignalR **會將**整個訊息傳送至未修改的用戶端。</span><span class="sxs-lookup"><span data-stu-id="160b4-208">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR<span data-ttu-id="160b4-209">只會將`Message`例外狀況的屬性傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="160b4-209"> only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="160b4-210">例外狀況的堆疊追蹤和其他屬性無法供用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="160b4-210">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="160b4-211">相關資源</span><span class="sxs-lookup"><span data-stu-id="160b4-211">Related resources</span></span>

* <span data-ttu-id="160b4-212">[ASP.NET Core 簡介SignalR](xref:signalr/introduction)</span><span class="sxs-lookup"><span data-stu-id="160b4-212">[Intro to ASP.NET Core SignalR](xref:signalr/introduction)</span></span>
* [<span data-ttu-id="160b4-213">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="160b4-213">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="160b4-214">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="160b4-214">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
