---
title: 3\.0 ASP.NET Core 的新功能
author: rick-anderson
description: 深入瞭解 ASP.NET Core 3.0 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.0
ms.openlocfilehash: c3dde383507ec919f82b5268ddbf23911c3d24f8
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963114"
---
# <a name="whats-new-in-aspnet-core-30"></a><span data-ttu-id="8250b-103">3\.0 ASP.NET Core 的新功能</span><span class="sxs-lookup"><span data-stu-id="8250b-103">What's new in ASP.NET Core 3.0</span></span>

<span data-ttu-id="8250b-104">本文將重點放在 ASP.NET Core 3.0 中最重要的變更，並提供相關檔的連結。</span><span class="sxs-lookup"><span data-stu-id="8250b-104">This article highlights the most significant changes in ASP.NET Core 3.0 with links to relevant documentation.</span></span>

## Blazor

Blazor<span data-ttu-id="8250b-105"> 是使用 .NET 建立互動式用戶端 web UI ASP.NET Core 的新架構：</span><span class="sxs-lookup"><span data-stu-id="8250b-105"> is a new framework in ASP.NET Core for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="8250b-106">使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="8250b-106">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="8250b-107">共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="8250b-107">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="8250b-108">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="8250b-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

Blazor<span data-ttu-id="8250b-109"> framework 支援的案例：</span><span class="sxs-lookup"><span data-stu-id="8250b-109"> framework supported scenarios:</span></span>

* <span data-ttu-id="8250b-110">可重複使用的 UI 元件（Razor 元件）</span><span class="sxs-lookup"><span data-stu-id="8250b-110">Reusable UI components (Razor components)</span></span>
* <span data-ttu-id="8250b-111">用戶端路由</span><span class="sxs-lookup"><span data-stu-id="8250b-111">Client-side routing</span></span>
* <span data-ttu-id="8250b-112">元件版面配置</span><span class="sxs-lookup"><span data-stu-id="8250b-112">Component layouts</span></span>
* <span data-ttu-id="8250b-113">支援相依性插入</span><span class="sxs-lookup"><span data-stu-id="8250b-113">Support for dependency injection</span></span>
* <span data-ttu-id="8250b-114">表單和驗證</span><span class="sxs-lookup"><span data-stu-id="8250b-114">Forms and validation</span></span>
* <span data-ttu-id="8250b-115">使用 Razor 類別庫建立元件程式庫</span><span class="sxs-lookup"><span data-stu-id="8250b-115">Build component libraries with Razor class libraries</span></span>
* <span data-ttu-id="8250b-116">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="8250b-116">JavaScript interop</span></span>

<span data-ttu-id="8250b-117">如需詳細資訊，請參閱<xref:blazor/index>。</span><span class="sxs-lookup"><span data-stu-id="8250b-117">For more information, see <xref:blazor/index>.</span></span>

### <a name="opno-locblazor-server"></a>Blazor<span data-ttu-id="8250b-118"> 伺服器</span><span class="sxs-lookup"><span data-stu-id="8250b-118"> Server</span></span>

Blazor<span data-ttu-id="8250b-119"> 將元件轉譯邏輯與 UI 更新的套用方式分離。</span><span class="sxs-lookup"><span data-stu-id="8250b-119"> decouples component rendering logic from how UI updates are applied.</span></span> Blazor<span data-ttu-id="8250b-120"> Server 提供在 ASP.NET Core 應用程式中將 Razor 元件裝載在伺服器上的支援。</span><span class="sxs-lookup"><span data-stu-id="8250b-120"> Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="8250b-121">UI 更新會透過 SignalR 連接來處理。</span><span class="sxs-lookup"><span data-stu-id="8250b-121">UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="8250b-122">ASP.NET Core 3.0 支援 Blazor Server。</span><span class="sxs-lookup"><span data-stu-id="8250b-122">Blazor Server is supported in ASP.NET Core 3.0.</span></span>

### <a name="opno-locblazor-webassembly-preview"></a>Blazor<span data-ttu-id="8250b-123"> WebAssembly （預覽）</span><span class="sxs-lookup"><span data-stu-id="8250b-123"> WebAssembly (Preview)</span></span>

Blazor<span data-ttu-id="8250b-124"> 應用程式也可以直接在瀏覽器中使用以 WebAssembly 為基礎的 .NET 執行時間來執行。</span><span class="sxs-lookup"><span data-stu-id="8250b-124"> apps can also be run directly in the browser using a WebAssembly-based .NET runtime.</span></span> Blazor<span data-ttu-id="8250b-125"> WebAssembly 處於預覽狀態，ASP.NET Core 3.0*不*支援。</span><span class="sxs-lookup"><span data-stu-id="8250b-125"> WebAssembly is in preview and *not* supported in ASP.NET Core 3.0.</span></span> <span data-ttu-id="8250b-126">ASP.NET Core 的未來版本將支援 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="8250b-126">Blazor WebAssembly will be supported in a future release of ASP.NET Core.</span></span>

### <a name="razor-components"></a><span data-ttu-id="8250b-127">Razor 元件</span><span class="sxs-lookup"><span data-stu-id="8250b-127">Razor components</span></span>

Blazor<span data-ttu-id="8250b-128"> 的應用程式是從元件建立而成。</span><span class="sxs-lookup"><span data-stu-id="8250b-128"> apps are built from components.</span></span> <span data-ttu-id="8250b-129">元件是獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。</span><span class="sxs-lookup"><span data-stu-id="8250b-129">Components are self-contained chunks of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="8250b-130">元件是定義 UI 呈現邏輯和用戶端事件處理常式的一般 .NET 類別。</span><span class="sxs-lookup"><span data-stu-id="8250b-130">Components are normal .NET classes that define UI rendering logic and client-side event handlers.</span></span> <span data-ttu-id="8250b-131">您可以建立豐富的互動式 web 應用程式，而不需要 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="8250b-131">You can create rich interactive web apps without JavaScript.</span></span>

<span data-ttu-id="8250b-132">Blazor 中的元件通常是使用 Razor 語法（這是 HTML 和C#的自然 blend）來撰寫。</span><span class="sxs-lookup"><span data-stu-id="8250b-132">Components in Blazor are typically authored using Razor syntax, a natural blend of HTML and C#.</span></span> <span data-ttu-id="8250b-133">Razor 元件與 Razor Pages 和 MVC 的觀點相似，因為它們都使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="8250b-133">Razor components are similar to Razor Pages and MVC views in that they both use Razor.</span></span> <span data-ttu-id="8250b-134">不同于以要求-回應模型為基礎的頁面和視圖，元件是專門用來處理 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="8250b-134">Unlike pages and views, which are based on a request-response model, components are used specifically for handling UI composition.</span></span>

## <a name="grpc"></a><span data-ttu-id="8250b-135">gRPC</span><span class="sxs-lookup"><span data-stu-id="8250b-135">gRPC</span></span>

<span data-ttu-id="8250b-136">[gRPC](https://grpc.io/)：</span><span class="sxs-lookup"><span data-stu-id="8250b-136">[gRPC](https://grpc.io/):</span></span>

* <span data-ttu-id="8250b-137">是一種熱門、高效能的 RPC （遠端程序呼叫）架構。</span><span class="sxs-lookup"><span data-stu-id="8250b-137">Is a popular, high-performance RPC (remote procedure call) framework.</span></span>
* <span data-ttu-id="8250b-138">提供固定合約優先的 API 開發方法。</span><span class="sxs-lookup"><span data-stu-id="8250b-138">Offers an opinionated contract-first approach to API development.</span></span>
* <span data-ttu-id="8250b-139">使用現代化技術，例如：</span><span class="sxs-lookup"><span data-stu-id="8250b-139">Uses modern technologies such as:</span></span>

  * <span data-ttu-id="8250b-140">用於傳輸的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="8250b-140">HTTP/2 for transport.</span></span>
  * <span data-ttu-id="8250b-141">通訊協定緩衝區，做為介面描述語言。</span><span class="sxs-lookup"><span data-stu-id="8250b-141">Protocol Buffers as the interface description language.</span></span>
  * <span data-ttu-id="8250b-142">二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="8250b-142">Binary serialization format.</span></span>
* <span data-ttu-id="8250b-143">提供下列功能：</span><span class="sxs-lookup"><span data-stu-id="8250b-143">Provides features such as:</span></span>

  * <span data-ttu-id="8250b-144">驗證</span><span class="sxs-lookup"><span data-stu-id="8250b-144">Authentication</span></span>
  * <span data-ttu-id="8250b-145">雙向串流和流量控制。</span><span class="sxs-lookup"><span data-stu-id="8250b-145">Bidirectional streaming and flow control.</span></span>
  * <span data-ttu-id="8250b-146">取消和超時。</span><span class="sxs-lookup"><span data-stu-id="8250b-146">Cancellation and timeouts.</span></span>

<span data-ttu-id="8250b-147">ASP.NET Core 3.0 中的 gRPC 功能包括：</span><span class="sxs-lookup"><span data-stu-id="8250b-147">gRPC functionality in ASP.NET Core 3.0 includes:</span></span>

* <span data-ttu-id="8250b-148">[Grpc. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; 用來裝載 Grpc 服務的 ASP.NET Core 架構。</span><span class="sxs-lookup"><span data-stu-id="8250b-148">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; An ASP.NET Core framework for hosting gRPC services.</span></span> <span data-ttu-id="8250b-149">ASP.NET Core 上的 gRPC 與標準 ASP.NET Core 功能整合，例如記錄、相依性插入（DI）、驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="8250b-149">gRPC on ASP.NET Core integrates with standard ASP.NET Core features like logging, dependency injection (DI), authentication, and authorization.</span></span>
* <span data-ttu-id="8250b-150">[Grpc .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)&ndash; Grpc 用戶端，適用于以熟悉的 `HttpClient`為基礎的 .net Core。</span><span class="sxs-lookup"><span data-stu-id="8250b-150">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; A gRPC client for .NET Core that builds upon the familiar `HttpClient`.</span></span>
* <span data-ttu-id="8250b-151">[Grpc .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; Grpc 與 `HttpClientFactory`的用戶端整合。</span><span class="sxs-lookup"><span data-stu-id="8250b-151">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC client integration with `HttpClientFactory`.</span></span>

<span data-ttu-id="8250b-152">如需詳細資訊，請參閱<xref:grpc/index>。</span><span class="sxs-lookup"><span data-stu-id="8250b-152">For more information, see <xref:grpc/index>.</span></span>

## SignalR

<span data-ttu-id="8250b-153">如需遷移指示，請參閱[更新 SignalR 程式碼](xref:migration/22-to-30#signalr)。</span><span class="sxs-lookup"><span data-stu-id="8250b-153">See [Update SignalR code](xref:migration/22-to-30#signalr) for migration instructions.</span></span> SignalR<span data-ttu-id="8250b-154"> 現在會使用 `System.Text.Json` 來序列化/還原序列化 JSON 訊息。</span><span class="sxs-lookup"><span data-stu-id="8250b-154"> now uses `System.Text.Json` to serialize/deserialize JSON messages.</span></span> <span data-ttu-id="8250b-155">如需還原以 `Newtonsoft.Json` 為基礎之序列化程式的指示，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="8250b-155">See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson) for instructions to restore the `Newtonsoft.Json`-based serializer.</span></span>

<span data-ttu-id="8250b-156">在 SignalR的 JavaScript 和 .NET 用戶端中，已加入自動重新連接的支援。</span><span class="sxs-lookup"><span data-stu-id="8250b-156">In the JavaScript and .NET Clients for SignalR, support was added for automatic reconnection.</span></span> <span data-ttu-id="8250b-157">根據預設，用戶端會嘗試立即重新連線，並在 2、10 和 30 秒後重試（如有必要）。</span><span class="sxs-lookup"><span data-stu-id="8250b-157">By default, the client tries to reconnect immediately and retry after 2, 10, and 30 seconds if necessary.</span></span> <span data-ttu-id="8250b-158">如果用戶端成功重新連接，則會收到新的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="8250b-158">If the client successfully reconnects, it receives a new connection ID.</span></span> <span data-ttu-id="8250b-159">自動重新連線是加入宣告的：</span><span class="sxs-lookup"><span data-stu-id="8250b-159">Automatic reconnect is opt-in:</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="8250b-160">藉由傳遞以毫秒為依據的持續時間陣列，可以指定重新連接間隔：</span><span class="sxs-lookup"><span data-stu-id="8250b-160">The reconnection intervals can be specified by passing an array of millisecond-based durations:</span></span>

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

<span data-ttu-id="8250b-161">您可以傳入自訂的執行，以取得重新連接間隔的完整控制。</span><span class="sxs-lookup"><span data-stu-id="8250b-161">A custom implementation can be passed in for full control of the reconnection intervals.</span></span>

<span data-ttu-id="8250b-162">如果在上一次重新連線間隔之後失敗重新連接：</span><span class="sxs-lookup"><span data-stu-id="8250b-162">If the reconnection fails after the last reconnect interval:</span></span>

* <span data-ttu-id="8250b-163">用戶端會將連接視為離線。</span><span class="sxs-lookup"><span data-stu-id="8250b-163">The client considers the connection is offline.</span></span>
* <span data-ttu-id="8250b-164">用戶端停止嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="8250b-164">The client stops trying to reconnect.</span></span>

<span data-ttu-id="8250b-165">在重新連線嘗試期間，更新應用程式 UI，以通知使用者正在嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="8250b-165">During reconnection attempts, update the app UI to notify the user that the reconnection is being attempted.</span></span>

<span data-ttu-id="8250b-166">若要在連接中斷時提供 UI 意見反應，SignalR 用戶端 API 已擴充為包含下列事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="8250b-166">To provide UI feedback when the connection is interrupted, the SignalR client API has been expanded to include the following event handlers:</span></span>

* <span data-ttu-id="8250b-167">`onreconnecting`：讓開發人員有機會停用 UI，或讓使用者知道應用程式已離線。</span><span class="sxs-lookup"><span data-stu-id="8250b-167">`onreconnecting`:  Gives developers an opportunity to disable UI or to let users know the app is offline.</span></span>
* <span data-ttu-id="8250b-168">`onreconnected`：讓開發人員有機會在連接重新建立後更新 UI。</span><span class="sxs-lookup"><span data-stu-id="8250b-168">`onreconnected`: Gives developers an opportunity to update the UI once the connection is reestablished.</span></span>

<span data-ttu-id="8250b-169">下列程式碼會在嘗試連接時，使用 `onreconnecting` 來更新 UI：</span><span class="sxs-lookup"><span data-stu-id="8250b-169">The following code uses `onreconnecting` to update the UI while trying to connect:</span></span>

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="8250b-170">下列程式碼會使用 `onreconnected` 來更新連接上的 UI：</span><span class="sxs-lookup"><span data-stu-id="8250b-170">The following code uses `onreconnected` to update the UI on connection:</span></span>

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

SignalR<span data-ttu-id="8250b-171"> 3.0 和更新版本會在中樞方法需要授權時，將自訂資源提供給授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="8250b-171"> 3.0 and later provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="8250b-172">資源是 `HubInvocationContext` 的實例。</span><span class="sxs-lookup"><span data-stu-id="8250b-172">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="8250b-173">`HubInvocationContext` 包括：</span><span class="sxs-lookup"><span data-stu-id="8250b-173">The `HubInvocationContext` includes the:</span></span>

* `HubCallerContext`
* <span data-ttu-id="8250b-174">所叫用之中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="8250b-174">Name of the hub method being invoked.</span></span>
* <span data-ttu-id="8250b-175">中樞方法的引數。</span><span class="sxs-lookup"><span data-stu-id="8250b-175">Arguments to the hub method.</span></span>

<span data-ttu-id="8250b-176">請考慮下列聊天室應用程式範例，讓多個組織能夠透過 Azure Active Directory 進行登入。</span><span class="sxs-lookup"><span data-stu-id="8250b-176">Consider the following example of a chat room app allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="8250b-177">具有 Microsoft 帳戶的任何人都可以登入交談，但只有擁有組織的成員可以禁止使用者或觀看使用者的聊天記錄。</span><span class="sxs-lookup"><span data-stu-id="8250b-177">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization can ban users or view users’ chat histories.</span></span> <span data-ttu-id="8250b-178">應用程式可能會限制特定使用者的特定功能。</span><span class="sxs-lookup"><span data-stu-id="8250b-178">The app could restrict certain functionality from specific users.</span></span>

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="8250b-179">在上述程式碼中，`DomainRestrictedRequirement` 做為自訂 `IAuthorizationRequirement`。</span><span class="sxs-lookup"><span data-stu-id="8250b-179">In the preceding code, `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="8250b-180">因為傳入 `HubInvocationContext` 資源參數，所以內部邏輯可以：</span><span class="sxs-lookup"><span data-stu-id="8250b-180">Because the `HubInvocationContext` resource parameter is being passed in, the internal logic can:</span></span>

* <span data-ttu-id="8250b-181">檢查正在呼叫中樞的內容。</span><span class="sxs-lookup"><span data-stu-id="8250b-181">Inspect the context in which the Hub is being called.</span></span>
* <span data-ttu-id="8250b-182">請決定是否要讓使用者執行個別的中樞方法。</span><span class="sxs-lookup"><span data-stu-id="8250b-182">Make decisions on allowing the user to execute individual Hub methods.</span></span>

<span data-ttu-id="8250b-183">您可以使用程式碼在執行時間檢查的原則名稱來裝飾個別的中樞方法。</span><span class="sxs-lookup"><span data-stu-id="8250b-183">Individual Hub methods can be decorated with the name of the policy the code checks at run-time.</span></span> <span data-ttu-id="8250b-184">當用戶端嘗試呼叫個別的中樞方法時，`DomainRestrictedRequirement` 處理常式會執行並控制方法的存取。</span><span class="sxs-lookup"><span data-stu-id="8250b-184">As clients attempt to call individual Hub methods, the `DomainRestrictedRequirement` handler runs and controls access to the methods.</span></span> <span data-ttu-id="8250b-185">根據 `DomainRestrictedRequirement` 控制存取的方式：</span><span class="sxs-lookup"><span data-stu-id="8250b-185">Based on the way the `DomainRestrictedRequirement` controls access:</span></span>

* <span data-ttu-id="8250b-186">所有登入的使用者都可以呼叫 `SendMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="8250b-186">All logged-in users can call the `SendMessage` method.</span></span>
* <span data-ttu-id="8250b-187">只有使用 `@jabbr.net` 電子郵件地址登入的使用者，才能夠查看使用者的歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="8250b-187">Only users who have logged in with a `@jabbr.net` email address can view users’ histories.</span></span>
* <span data-ttu-id="8250b-188">只有 `bob42@jabbr.net` 可以禁止來自聊天室的使用者。</span><span class="sxs-lookup"><span data-stu-id="8250b-188">Only `bob42@jabbr.net` can ban users from the chat room.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

<span data-ttu-id="8250b-189">建立 `DomainRestricted` 原則可能牽涉到：</span><span class="sxs-lookup"><span data-stu-id="8250b-189">Creating the `DomainRestricted` policy might involve:</span></span>

* <span data-ttu-id="8250b-190">在*Startup.cs*中，新增原則。</span><span class="sxs-lookup"><span data-stu-id="8250b-190">In *Startup.cs*, adding the new policy.</span></span>
* <span data-ttu-id="8250b-191">提供自訂 `DomainRestrictedRequirement` 需求做為參數。</span><span class="sxs-lookup"><span data-stu-id="8250b-191">Provide the custom `DomainRestrictedRequirement` requirement as a parameter.</span></span>
* <span data-ttu-id="8250b-192">向授權中介軟體註冊 `DomainRestricted`。</span><span class="sxs-lookup"><span data-stu-id="8250b-192">Registering `DomainRestricted` with the authorization middleware.</span></span>

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

SignalR<span data-ttu-id="8250b-193"> 中樞會使用[端點路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="8250b-193"> hubs use [Endpoint Routing](xref:fundamentals/routing).</span></span> SignalR<span data-ttu-id="8250b-194"> 中樞連線先前已明確完成：</span><span class="sxs-lookup"><span data-stu-id="8250b-194"> hub connection was previously done explicitly:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="8250b-195">在舊版中，開發人員需要在各種地方連接控制器、Razor 頁面和中樞。</span><span class="sxs-lookup"><span data-stu-id="8250b-195">In the previous version, developers needed to wire up controllers, Razor pages, and hubs in a variety of places.</span></span> <span data-ttu-id="8250b-196">明確連接會產生一系列幾乎相同的路由區段：</span><span class="sxs-lookup"><span data-stu-id="8250b-196">Explicit connection results in a series of nearly-identical routing segments:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

SignalR<span data-ttu-id="8250b-197"> 3.0 中樞可以透過端點路由來路由傳送。</span><span class="sxs-lookup"><span data-stu-id="8250b-197"> 3.0 hubs can be routed via endpoint routing.</span></span> <span data-ttu-id="8250b-198">透過端點路由，通常可以在 `UseRouting`中設定所有路由：</span><span class="sxs-lookup"><span data-stu-id="8250b-198">With endpoint routing, typically all routing can be configured in `UseRouting`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="8250b-199">已新增 ASP.NET Core 3.0 SignalR：</span><span class="sxs-lookup"><span data-stu-id="8250b-199">ASP.NET Core 3.0 SignalR added:</span></span>

<span data-ttu-id="8250b-200">用戶端對伺服器串流。</span><span class="sxs-lookup"><span data-stu-id="8250b-200">Client-to-server streaming.</span></span> <span data-ttu-id="8250b-201">使用用戶端對伺服器串流，伺服器端方法可以接受 `IAsyncEnumerable<T>` 或 `ChannelReader<T>`的實例。</span><span class="sxs-lookup"><span data-stu-id="8250b-201">With client-to-server streaming, server-side methods can take instances of either an `IAsyncEnumerable<T>` or `ChannelReader<T>`.</span></span> <span data-ttu-id="8250b-202">在下列C#範例中，中樞上的 `UploadStream` 方法會接收來自用戶端的字串資料流程：</span><span class="sxs-lookup"><span data-stu-id="8250b-202">In the following C# sample, the `UploadStream` method on the Hub will receive a stream of strings from the client:</span></span>

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

<span data-ttu-id="8250b-203">.NET 用戶端應用程式可以將 `IAsyncEnumerable<T>` 或 `ChannelReader<T>` 實例當做上述 `UploadStream` 中樞方法的 `stream` 引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="8250b-203">.NET client apps can pass either an `IAsyncEnumerable<T>` or `ChannelReader<T>` instance as the `stream` argument of the `UploadStream` Hub method above.</span></span>

<span data-ttu-id="8250b-204">在 `for` 迴圈完成且區域函式結束後，就會傳送資料流程完成：</span><span class="sxs-lookup"><span data-stu-id="8250b-204">After the `for` loop has completed and the local function exits, the stream completion is sent:</span></span>

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="8250b-205">JavaScript 用戶端應用程式會針對上述 `UploadStream` 中樞方法的 `stream` 引數使用 SignalR `Subject` （或[RxJS 主體](https://rxjs.dev/api/index/class/Subject)）。</span><span class="sxs-lookup"><span data-stu-id="8250b-205">JavaScript client apps use the SignalR `Subject` (or an [RxJS Subject](https://rxjs.dev/api/index/class/Subject)) for the `stream` argument of the `UploadStream` Hub method above.</span></span>

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

<span data-ttu-id="8250b-206">JavaScript 程式碼可以使用 `subject.next` 方法來處理已捕捉並準備傳送至伺服器的字串。</span><span class="sxs-lookup"><span data-stu-id="8250b-206">The JavaScript code could use the `subject.next` method to handle strings as they are captured and ready to be sent to the server.</span></span>

```javascript
subject.next("example");
subject.complete();
```

<span data-ttu-id="8250b-207">使用上述兩個程式碼片段這類程式碼，可以建立即時串流體驗。</span><span class="sxs-lookup"><span data-stu-id="8250b-207">Using code like the two preceding snippets, real-time streaming experiences can be created.</span></span>

## <a name="new-json-serialization"></a><span data-ttu-id="8250b-208">新的 JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="8250b-208">New JSON serialization</span></span>

<span data-ttu-id="8250b-209">ASP.NET Core 3.0 現在會使用 <xref:System.Text.Json> JSON 序列化的預設值：</span><span class="sxs-lookup"><span data-stu-id="8250b-209">ASP.NET Core 3.0 now uses <xref:System.Text.Json> by default for JSON serialization:</span></span>

* <span data-ttu-id="8250b-210">非同步讀取和寫入 JSON。</span><span class="sxs-lookup"><span data-stu-id="8250b-210">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="8250b-211">已針對 UTF-8 文字進行優化。</span><span class="sxs-lookup"><span data-stu-id="8250b-211">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="8250b-212">通常比 `Newtonsoft.Json`更高的效能。</span><span class="sxs-lookup"><span data-stu-id="8250b-212">Typically higher performance than `Newtonsoft.Json`.</span></span>

<span data-ttu-id="8250b-213">若要將 Json.NET 新增至 ASP.NET Core 3.0，請參閱[新增 Newtonsoft 以 json 為基礎的 json 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。</span><span class="sxs-lookup"><span data-stu-id="8250b-213">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span>

## <a name="new-razor-directives"></a><span data-ttu-id="8250b-214">新的 Razor 指示詞</span><span class="sxs-lookup"><span data-stu-id="8250b-214">New Razor directives</span></span>

<span data-ttu-id="8250b-215">下列清單包含新的 Razor 指示詞：</span><span class="sxs-lookup"><span data-stu-id="8250b-215">The following list contains new Razor directives:</span></span>

* <span data-ttu-id="8250b-216">[@attribute](xref:mvc/views/razor#attribute) &ndash; `@attribute` 指示詞會將指定的屬性套用至所產生頁面或視圖的類別。</span><span class="sxs-lookup"><span data-stu-id="8250b-216">[@attribute](xref:mvc/views/razor#attribute) &ndash; The `@attribute` directive applies the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="8250b-217">例如，`@attribute [Authorize]`。</span><span class="sxs-lookup"><span data-stu-id="8250b-217">For example, `@attribute [Authorize]`.</span></span>
* <span data-ttu-id="8250b-218">[@implements](xref:mvc/views/razor#implements) &ndash; `@implements` 指示詞會為所產生的類別執行介面。</span><span class="sxs-lookup"><span data-stu-id="8250b-218">[@implements](xref:mvc/views/razor#implements) &ndash; The `@implements` directive implements an interface for the generated class.</span></span> <span data-ttu-id="8250b-219">例如，`@implements IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="8250b-219">For example, `@implements IDisposable`.</span></span>

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a><span data-ttu-id="8250b-220">IdentityServer4 支援 web Api 和 Spa 的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="8250b-220">IdentityServer4 supports authentication and authorization for web APIs and SPAs</span></span>

<span data-ttu-id="8250b-221">ASP.NET Core 3.0 使用 Web API 授權的支援，在單一頁面應用程式（SPA）中提供驗證。</span><span class="sxs-lookup"><span data-stu-id="8250b-221">ASP.NET Core 3.0 offers authentication in Single Page Apps (SPAs) using the support for web API authorization.</span></span> <span data-ttu-id="8250b-222">用於驗證和儲存使用者的 ASP.NET Core 身分識別會與[IdentityServer4](https://identityserver.io/)結合，以執行 Open ID Connect。</span><span class="sxs-lookup"><span data-stu-id="8250b-222">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer4](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="8250b-223">IdentityServer4 是適用于 ASP.NET Core 3.0 的 OpenID Connect 和 OAuth 2.0 架構。</span><span class="sxs-lookup"><span data-stu-id="8250b-223">IdentityServer4 is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core 3.0.</span></span> <span data-ttu-id="8250b-224">它會啟用下列安全性功能：</span><span class="sxs-lookup"><span data-stu-id="8250b-224">It enables the following security features:</span></span>

* <span data-ttu-id="8250b-225">驗證即服務（AaaS）</span><span class="sxs-lookup"><span data-stu-id="8250b-225">Authentication as a Service (AaaS)</span></span>
* <span data-ttu-id="8250b-226">多個應用程式類型的單一登入/關閉（SSO）</span><span class="sxs-lookup"><span data-stu-id="8250b-226">Single sign-on/off (SSO) over multiple application types</span></span>
* <span data-ttu-id="8250b-227">Api 的存取控制</span><span class="sxs-lookup"><span data-stu-id="8250b-227">Access control for APIs</span></span>
* <span data-ttu-id="8250b-228">同盟閘道</span><span class="sxs-lookup"><span data-stu-id="8250b-228">Federation Gateway</span></span>

<span data-ttu-id="8250b-229">如需詳細資訊，請參閱[IdentityServer4 檔](http://docs.identityserver.io/en/latest/index.html)或[spa 的驗證和授權](xref:security/authentication/identity/spa)。</span><span class="sxs-lookup"><span data-stu-id="8250b-229">For more information, see [the IdentityServer4 documentation](http://docs.identityserver.io/en/latest/index.html) or [Authentication and authorization for SPAs](xref:security/authentication/identity/spa).</span></span>

## <a name="certificate-and-kerberos-authentication"></a><span data-ttu-id="8250b-230">憑證和 Kerberos 驗證</span><span class="sxs-lookup"><span data-stu-id="8250b-230">Certificate and Kerberos authentication</span></span>

<span data-ttu-id="8250b-231">憑證驗證需要：</span><span class="sxs-lookup"><span data-stu-id="8250b-231">Certificate authentication requires:</span></span>

* <span data-ttu-id="8250b-232">正在設定伺服器以接受憑證。</span><span class="sxs-lookup"><span data-stu-id="8250b-232">Configuring the server to accept certificates.</span></span>
* <span data-ttu-id="8250b-233">在 `Startup.Configure` 中新增驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8250b-233">Adding the authentication middleware in `Startup.Configure`.</span></span>
* <span data-ttu-id="8250b-234">在 `Startup.ConfigureServices` 中新增憑證驗證服務。</span><span class="sxs-lookup"><span data-stu-id="8250b-234">Adding the certificate authentication service in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="8250b-235">憑證驗證的選項包括下列功能：</span><span class="sxs-lookup"><span data-stu-id="8250b-235">Options for certificate authentication include the ability to:</span></span>

* <span data-ttu-id="8250b-236">接受自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="8250b-236">Accept self-signed certificates.</span></span>
* <span data-ttu-id="8250b-237">檢查憑證是否已撤銷。</span><span class="sxs-lookup"><span data-stu-id="8250b-237">Check for certificate revocation.</span></span>
* <span data-ttu-id="8250b-238">檢查好處憑證中是否有正確的使用方式旗標。</span><span class="sxs-lookup"><span data-stu-id="8250b-238">Check that the proffered certificate has the right usage flags in it.</span></span>

<span data-ttu-id="8250b-239">預設的使用者主體會從憑證屬性來建立。</span><span class="sxs-lookup"><span data-stu-id="8250b-239">A default user principal is constructed from the certificate properties.</span></span> <span data-ttu-id="8250b-240">使用者主體包含的事件可讓您補充或取代主體。</span><span class="sxs-lookup"><span data-stu-id="8250b-240">The user principal contains an event that enables supplementing or replacing the principal.</span></span> <span data-ttu-id="8250b-241">如需詳細資訊，請參閱<xref:security/authentication/certauth>。</span><span class="sxs-lookup"><span data-stu-id="8250b-241">For more information, see <xref:security/authentication/certauth>.</span></span>

<span data-ttu-id="8250b-242">[Windows 驗證](/windows-server/security/windows-authentication/windows-authentication-overview)已擴充到 Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="8250b-242">[Windows Authentication](/windows-server/security/windows-authentication/windows-authentication-overview) has been extended onto Linux and macOS.</span></span> <span data-ttu-id="8250b-243">在先前的版本中，Windows 驗證僅限於[IIS](xref:host-and-deploy/iis/index)和[HttpSys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="8250b-243">In previous versions, Windows Authentication was limited to [IIS](xref:host-and-deploy/iis/index) and [HttpSys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="8250b-244">在 ASP.NET Core 3.0 中， [Kestrel](xref:fundamentals/servers/kestrel)可以在 windows、Linux 和 macOS 上針對已加入網域的 windows 主機使用 Negotiate、 [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview)和[NTLM](/windows-server/security/kerberos/ntlm-overview)。</span><span class="sxs-lookup"><span data-stu-id="8250b-244">In ASP.NET Core 3.0, [Kestrel](xref:fundamentals/servers/kestrel) has the ability to use Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview), and [NTLM on Windows](/windows-server/security/kerberos/ntlm-overview), Linux, and macOS for Windows domain-joined hosts.</span></span> <span data-ttu-id="8250b-245">這些驗證配置的 Kestrel 支援是由 AspNetCore 所提供。 [Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)套件。</span><span class="sxs-lookup"><span data-stu-id="8250b-245">Kestrel support of these authentication schemes is provided by the [Microsoft.AspNetCore.Authentication.Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package.</span></span> <span data-ttu-id="8250b-246">如同其他驗證服務，請將驗證應用程式設定為 [寬]，然後設定服務：</span><span class="sxs-lookup"><span data-stu-id="8250b-246">As with the other authentication services, configure authentication app wide, then configure the service:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="8250b-247">主機需求：</span><span class="sxs-lookup"><span data-stu-id="8250b-247">Host requirements:</span></span>

* <span data-ttu-id="8250b-248">Windows 主機必須將[服務主體名稱](/windows/win32/ad/service-principal-names)（SPN）新增至裝載應用程式的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="8250b-248">Windows hosts must have [Service Principal Names](/windows/win32/ad/service-principal-names) (SPNs) added to the user account hosting the app.</span></span>
* <span data-ttu-id="8250b-249">Linux 和 macOS 機器必須加入網域。</span><span class="sxs-lookup"><span data-stu-id="8250b-249">Linux and macOS machines must be joined to the domain.</span></span>
  * <span data-ttu-id="8250b-250">必須為 web 進程建立 SPN。</span><span class="sxs-lookup"><span data-stu-id="8250b-250">SPNs must be created for the web process.</span></span>
  * <span data-ttu-id="8250b-251">必須在主機電腦上產生和設定[Keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)檔案。</span><span class="sxs-lookup"><span data-stu-id="8250b-251">[Keytab files](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) must be generated and configured on the host machine.</span></span>

<span data-ttu-id="8250b-252">如需詳細資訊，請參閱<xref:security/authentication/windowsauth>。</span><span class="sxs-lookup"><span data-stu-id="8250b-252">For more information, see <xref:security/authentication/windowsauth>.</span></span>

## <a name="template-changes"></a><span data-ttu-id="8250b-253">範本變更</span><span class="sxs-lookup"><span data-stu-id="8250b-253">Template changes</span></span>

<span data-ttu-id="8250b-254">Web UI 範本（Razor Pages、具有控制器和 views 的 MVC）已移除下列各項：</span><span class="sxs-lookup"><span data-stu-id="8250b-254">The web UI templates (Razor Pages, MVC with controller and views) have the following removed:</span></span>

* <span data-ttu-id="8250b-255">Cookie 同意 UI 已不再包含在內。</span><span class="sxs-lookup"><span data-stu-id="8250b-255">The cookie consent UI is no longer included.</span></span> <span data-ttu-id="8250b-256">若要在 ASP.NET Core 3.0 範本產生的應用程式中啟用 cookie 同意功能，請參閱 <xref:security/gdpr>。</span><span class="sxs-lookup"><span data-stu-id="8250b-256">To enable the cookie consent feature in an ASP.NET Core 3.0 template-generated app, see <xref:security/gdpr>.</span></span>
* <span data-ttu-id="8250b-257">腳本和相關的靜態資產現在會當做本機檔案來參考，而不是使用 Cdn。</span><span class="sxs-lookup"><span data-stu-id="8250b-257">Scripts and related static assets are now referenced as local files instead of using CDNs.</span></span> <span data-ttu-id="8250b-258">如需詳細資訊，請參閱[腳本和相關靜態資產現在會當做本機檔案參考，而不是根據目前的環境使用 cdn （aspnet/AspNetCore #14350）](https://github.com/aspnet/AspNetCore.Docs/issues/14350)。</span><span class="sxs-lookup"><span data-stu-id="8250b-258">For more information, see [Scripts and related static assets are now referenced as local files instead of using CDNs based on the current environment (aspnet/AspNetCore.Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span></span>

<span data-ttu-id="8250b-259">Angular 範本已更新為使用 Angular 8。</span><span class="sxs-lookup"><span data-stu-id="8250b-259">The Angular template updated to use Angular 8.</span></span>

<span data-ttu-id="8250b-260">根據預設，Razor 類別庫（RCL）範本預設為 Razor 元件開發。</span><span class="sxs-lookup"><span data-stu-id="8250b-260">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="8250b-261">Visual Studio 中的新範本選項會提供頁面和視圖的範本支援。</span><span class="sxs-lookup"><span data-stu-id="8250b-261">A new template option in Visual Studio provides template support for pages and views.</span></span> <span data-ttu-id="8250b-262">從命令 shell 中的範本建立 RCL 時，請傳遞 `--support-pages-and-views` 選項（`dotnet new razorclasslib --support-pages-and-views`）。</span><span class="sxs-lookup"><span data-stu-id="8250b-262">When creating an RCL from the template in a command shell, pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`).</span></span>

## <a name="generic-host"></a><span data-ttu-id="8250b-263">一般主機</span><span class="sxs-lookup"><span data-stu-id="8250b-263">Generic Host</span></span>

<span data-ttu-id="8250b-264">ASP.NET Core 3.0 範本會使用 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="8250b-264">The ASP.NET Core 3.0 templates use <xref:fundamentals/host/generic-host>.</span></span> <span data-ttu-id="8250b-265">先前使用的版本 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="8250b-265">Previous versions used <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="8250b-266">使用 .NET Core 泛型主機（<xref:Microsoft.Extensions.Hosting.HostBuilder>）可讓 ASP.NET Core 應用程式與其他不是 web 特定的伺服器案例更緊密整合。</span><span class="sxs-lookup"><span data-stu-id="8250b-266">Using the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) provides better integration of ASP.NET Core apps with other server scenarios that aren't web-specific.</span></span> <span data-ttu-id="8250b-267">如需詳細資訊，請參閱[HostBuilder 取代 WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="8250b-267">For more information, see [HostBuilder replaces WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span></span>

### <a name="host-configuration"></a><span data-ttu-id="8250b-268">主機組態</span><span class="sxs-lookup"><span data-stu-id="8250b-268">Host configuration</span></span>

<span data-ttu-id="8250b-269">在 ASP.NET Core 3.0 發行之前，會針對 Web 主機的主機設定載入前面加上 `ASPNETCORE_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="8250b-269">Prior to the release of ASP.NET Core 3.0, environment variables prefixed with `ASPNETCORE_` were loaded for host configuration of the Web Host.</span></span> <span data-ttu-id="8250b-270">在 3.0 中，`AddEnvironmentVariables` 是用來載入前面加上 `DOTNET_` 的環境變數，以使用 `CreateDefaultBuilder`進行主機設定。</span><span class="sxs-lookup"><span data-stu-id="8250b-270">In 3.0, `AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for host configuration with `CreateDefaultBuilder`.</span></span>

### <a name="changes-to-startup-constructor-injection"></a><span data-ttu-id="8250b-271">啟動函式插入的變更</span><span class="sxs-lookup"><span data-stu-id="8250b-271">Changes to Startup constructor injection</span></span>

<span data-ttu-id="8250b-272">泛型主機僅支援下列類型的 `Startup` 的程式表示式插入：</span><span class="sxs-lookup"><span data-stu-id="8250b-272">The Generic Host only supports the following types for `Startup` constructor injection:</span></span>

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="8250b-273">所有服務仍然可以直接插入 `Startup.Configure` 方法的引數。</span><span class="sxs-lookup"><span data-stu-id="8250b-273">All services can still be injected directly as arguments to the `Startup.Configure` method.</span></span> <span data-ttu-id="8250b-274">如需詳細資訊，請參閱[泛型主機限制啟動函數插入（aspnet/公告 #353）](https://github.com/aspnet/Announcements/issues/353)。</span><span class="sxs-lookup"><span data-stu-id="8250b-274">For more information, see [Generic Host restricts Startup constructor injection (aspnet/Announcements #353)](https://github.com/aspnet/Announcements/issues/353).</span></span>

## <a name="kestrel"></a><span data-ttu-id="8250b-275">Kestrel</span><span class="sxs-lookup"><span data-stu-id="8250b-275">Kestrel</span></span>

* <span data-ttu-id="8250b-276">Kestrel 設定已更新，可供遷移至一般主機。</span><span class="sxs-lookup"><span data-stu-id="8250b-276">Kestrel configuration has been updated for the migration to the Generic Host.</span></span> <span data-ttu-id="8250b-277">在3.0 中，Kestrel 是在 `ConfigureWebHostDefaults`所提供的 web 主機產生器上設定。</span><span class="sxs-lookup"><span data-stu-id="8250b-277">In 3.0, Kestrel is configured on the web host builder provided by `ConfigureWebHostDefaults`.</span></span>
* <span data-ttu-id="8250b-278">連接介面卡已從 Kestrel 中移除，並以連線中介軟體取代，類似于 ASP.NET Core 管線中的 HTTP 中介軟體，但較低層級的連接。</span><span class="sxs-lookup"><span data-stu-id="8250b-278">Connection Adapters have been removed from Kestrel and replaced with Connection Middleware, which is similar to HTTP Middleware in the ASP.NET Core pipeline but for lower-level connections.</span></span>
* <span data-ttu-id="8250b-279">Kestrel 傳輸層已公開為 `Connections.Abstractions` 中的公用介面。</span><span class="sxs-lookup"><span data-stu-id="8250b-279">The Kestrel transport layer has been exposed as a public interface in `Connections.Abstractions`.</span></span>
* <span data-ttu-id="8250b-280">標頭和尾端之間的多義性已藉由將尾端標頭移至新集合來解決。</span><span class="sxs-lookup"><span data-stu-id="8250b-280">Ambiguity between headers and trailers has been resolved by moving trailing headers to a new collection.</span></span>
* <span data-ttu-id="8250b-281">同步 IO Api （例如 `HttpRequest.Body.Read`）是執行緒耗盡的常見來源，因而導致應用程式當機。</span><span class="sxs-lookup"><span data-stu-id="8250b-281">Synchronous IO APIs, such as `HttpRequest.Body.Read`, are a common source of thread starvation leading to app crashes.</span></span> <span data-ttu-id="8250b-282">在3.0 中，預設會停用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="8250b-282">In 3.0, `AllowSynchronousIO` is disabled by default.</span></span>

<span data-ttu-id="8250b-283">如需詳細資訊，請參閱<xref:migration/22-to-30#kestrel>。</span><span class="sxs-lookup"><span data-stu-id="8250b-283">For more information, see <xref:migration/22-to-30#kestrel>.</span></span>

## <a name="http2-enabled-by-default"></a><span data-ttu-id="8250b-284">預設啟用 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="8250b-284">HTTP/2 enabled by default</span></span>

<span data-ttu-id="8250b-285">在 HTTPS 端點的 Kestrel 中，預設會啟用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="8250b-285">HTTP/2 is enabled by default in Kestrel for HTTPS endpoints.</span></span> <span data-ttu-id="8250b-286">受作業系統支援時，會啟用 IIS 或 HTTP.sys 的 HTTP/2 支援。</span><span class="sxs-lookup"><span data-stu-id="8250b-286">HTTP/2 support for IIS or HTTP.sys is enabled when supported by the operating system.</span></span>

## <a name="eventcounters-on-request"></a><span data-ttu-id="8250b-287">要求 EventCounters</span><span class="sxs-lookup"><span data-stu-id="8250b-287">EventCounters on request</span></span>

<span data-ttu-id="8250b-288">主控 EventSource （`Microsoft.AspNetCore.Hosting`）會發出下列與連入要求相關的新 <xref:System.Diagnostics.Tracing.EventCounter> 類型：</span><span class="sxs-lookup"><span data-stu-id="8250b-288">The Hosting EventSource, `Microsoft.AspNetCore.Hosting`, emits the following new <xref:System.Diagnostics.Tracing.EventCounter> types related to incoming requests:</span></span>

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a><span data-ttu-id="8250b-289">端點路由</span><span class="sxs-lookup"><span data-stu-id="8250b-289">Endpoint routing</span></span>

<span data-ttu-id="8250b-290">端點路由可讓架構（例如 MVC）適用于中介軟體，已增強：</span><span class="sxs-lookup"><span data-stu-id="8250b-290">Endpoint Routing, which allows frameworks (for example, MVC) to work well with middleware, is enhanced:</span></span>

* <span data-ttu-id="8250b-291">中介軟體和端點的順序可在 `Startup.Configure`的要求處理管線中設定。</span><span class="sxs-lookup"><span data-stu-id="8250b-291">The order of middleware and endpoints is configurable in the request processing pipeline of `Startup.Configure`.</span></span>
* <span data-ttu-id="8250b-292">端點和中介軟體會與其他以 ASP.NET Core 為基礎的技術（例如健康狀態檢查）妥善地撰寫。</span><span class="sxs-lookup"><span data-stu-id="8250b-292">Endpoints and middleware compose well with other ASP.NET Core-based technologies, such as Health Checks.</span></span>
* <span data-ttu-id="8250b-293">端點可以在中介軟體和 MVC 中執行原則，例如 CORS 或授權。</span><span class="sxs-lookup"><span data-stu-id="8250b-293">Endpoints can implement a policy, such as CORS or authorization, in both middleware and MVC.</span></span>
* <span data-ttu-id="8250b-294">篩選器和屬性可以放在控制器中的方法上。</span><span class="sxs-lookup"><span data-stu-id="8250b-294">Filters and attributes can be placed on methods in controllers.</span></span>

<span data-ttu-id="8250b-295">如需詳細資訊，請參閱<xref:fundamentals/routing#routing-basics>。</span><span class="sxs-lookup"><span data-stu-id="8250b-295">For more information, see <xref:fundamentals/routing#routing-basics>.</span></span>

## <a name="health-checks"></a><span data-ttu-id="8250b-296">健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="8250b-296">Health Checks</span></span>

<span data-ttu-id="8250b-297">健全狀況檢查會搭配泛型主機使用端點路由。</span><span class="sxs-lookup"><span data-stu-id="8250b-297">Health Checks use endpoint routing with the Generic Host.</span></span> <span data-ttu-id="8250b-298">在 `Startup.Configure` 中，使用端點 URL 或相對路徑，在端點產生器上呼叫 `MapHealthChecks`：</span><span class="sxs-lookup"><span data-stu-id="8250b-298">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

<span data-ttu-id="8250b-299">健康情況檢查端點可以：</span><span class="sxs-lookup"><span data-stu-id="8250b-299">Health Checks endpoints can:</span></span>

* <span data-ttu-id="8250b-300">指定一或多個允許的主機/埠。</span><span class="sxs-lookup"><span data-stu-id="8250b-300">Specify one or more permitted hosts/ports.</span></span>
* <span data-ttu-id="8250b-301">需要授權。</span><span class="sxs-lookup"><span data-stu-id="8250b-301">Require authorization.</span></span>
* <span data-ttu-id="8250b-302">需要 CORS。</span><span class="sxs-lookup"><span data-stu-id="8250b-302">Require CORS.</span></span>

<span data-ttu-id="8250b-303">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="8250b-303">For more information, see the following articles:</span></span>

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a><span data-ttu-id="8250b-304">HttpCoNtext 上的管道</span><span class="sxs-lookup"><span data-stu-id="8250b-304">Pipes on HttpContext</span></span>

<span data-ttu-id="8250b-305">現在可以使用 <xref:System.IO.Pipelines> API 來讀取要求本文並寫入回應主體。</span><span class="sxs-lookup"><span data-stu-id="8250b-305">It's now possible to read the request body and write the response body using the <xref:System.IO.Pipelines> API.</span></span> <span data-ttu-id="8250b-306">必須提供</span><span class="sxs-lookup"><span data-stu-id="8250b-306">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> <span data-ttu-id="8250b-307">`HttpRequest.BodyReader` 屬性提供可用於讀取要求本文的 <xref:System.IO.Pipelines.PipeReader>。</span><span class="sxs-lookup"><span data-stu-id="8250b-307">`HttpRequest.BodyReader` property provides a <xref:System.IO.Pipelines.PipeReader> that can be used to read the request body.</span></span> <span data-ttu-id="8250b-308">必須提供</span><span class="sxs-lookup"><span data-stu-id="8250b-308">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.> --> <span data-ttu-id="8250b-309">`HttpResponse.BodyWriter` 屬性提供可用於寫入回應主體的 <xref:System.IO.Pipelines.PipeWriter>。</span><span class="sxs-lookup"><span data-stu-id="8250b-309">`HttpResponse.BodyWriter` property provides a <xref:System.IO.Pipelines.PipeWriter> that can be used to write the response body.</span></span> <span data-ttu-id="8250b-310">`HttpRequest.BodyReader` 是 `HttpRequest.Body` 串流的類比。</span><span class="sxs-lookup"><span data-stu-id="8250b-310">`HttpRequest.BodyReader` is an analogue of the `HttpRequest.Body` stream.</span></span> <span data-ttu-id="8250b-311">`HttpResponse.BodyWriter` 是 `HttpResponse.Body` 串流的類比。</span><span class="sxs-lookup"><span data-stu-id="8250b-311">`HttpResponse.BodyWriter` is an analogue of the `HttpResponse.Body` stream.</span></span>

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a><span data-ttu-id="8250b-312">改善 IIS 中的錯誤報表</span><span class="sxs-lookup"><span data-stu-id="8250b-312">Improved error reporting in IIS</span></span>

<span data-ttu-id="8250b-313">在 IIS 中裝載 ASP.NET Core 應用程式時，啟動錯誤現在會產生更豐富的診斷資料。</span><span class="sxs-lookup"><span data-stu-id="8250b-313">Startup errors when hosting ASP.NET Core apps in IIS now produce richer diagnostic data.</span></span> <span data-ttu-id="8250b-314">這些錯誤會在適用的情況下向 Windows 事件記錄檔回報堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="8250b-314">These errors are reported to the Windows Event Log with stack traces wherever applicable.</span></span> <span data-ttu-id="8250b-315">此外，所有的警告、錯誤和未處理的例外狀況都會記錄到 Windows 事件記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="8250b-315">In addition, all warnings, errors, and unhandled exceptions are logged to the Windows Event Log.</span></span>

## <a name="worker-service-and-worker-sdk"></a><span data-ttu-id="8250b-316">背景工作服務和背景工作角色 SDK</span><span class="sxs-lookup"><span data-stu-id="8250b-316">Worker Service and Worker SDK</span></span>

<span data-ttu-id="8250b-317">.NET Core 3.0 引進了新的背景工作服務應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="8250b-317">.NET Core 3.0 introduces the new Worker Service app template.</span></span> <span data-ttu-id="8250b-318">此範本提供在 .NET Core 中撰寫長時間執行服務的起點。</span><span class="sxs-lookup"><span data-stu-id="8250b-318">This template provides a starting point for writing long running services in .NET Core.</span></span>

<span data-ttu-id="8250b-319">如需詳細資訊，請參閱:</span><span class="sxs-lookup"><span data-stu-id="8250b-319">For more information, see:</span></span>

* [<span data-ttu-id="8250b-320">.NET Core 背景工作角色做為 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="8250b-320">.NET Core Workers as Windows Services</span></span>](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a><span data-ttu-id="8250b-321">轉送的標頭中介軟體改善</span><span class="sxs-lookup"><span data-stu-id="8250b-321">Forwarded Headers Middleware improvements</span></span>

<span data-ttu-id="8250b-322">在舊版的 ASP.NET Core 中，當部署到 Azure Linux 或 IIS 以外的任何反向 proxy 後方時，呼叫 <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> 和 <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> 會有問題。</span><span class="sxs-lookup"><span data-stu-id="8250b-322">In previous versions of ASP.NET Core, calling <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> and  <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> were problematic when deployed to an Azure Linux or behind any reverse proxy other than IIS.</span></span> <span data-ttu-id="8250b-323">先前版本的修正已記載于[轉送 Linux 和非 IIS 反向 proxy 的配置](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies)中。</span><span class="sxs-lookup"><span data-stu-id="8250b-323">The fix for previous versions is documented in [Forward the scheme for Linux and non-IIS reverse proxies](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span></span>

<span data-ttu-id="8250b-324">此案例已在 ASP.NET Core 3.0 中修正。</span><span class="sxs-lookup"><span data-stu-id="8250b-324">This scenario is fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="8250b-325">當 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true` 時，主機會啟用[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="8250b-325">The host enables the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) when the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span> <span data-ttu-id="8250b-326">在我們的容器映射中，`ASPNETCORE_FORWARDEDHEADERS_ENABLED` 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="8250b-326">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` is set to `true` in our container images.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="8250b-327">效能改善</span><span class="sxs-lookup"><span data-stu-id="8250b-327">Performance improvements</span></span>

<span data-ttu-id="8250b-328">ASP.NET Core 3.0 包含許多增強功能，可減少記憶體使用量並改善輸送量：</span><span class="sxs-lookup"><span data-stu-id="8250b-328">ASP.NET Core 3.0 includes many improvements that reduce memory usage and improve throughput:</span></span>

* <span data-ttu-id="8250b-329">針對範圍服務使用內建的相依性插入容器時，減少記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="8250b-329">Reduction in memory usage when using the built-in dependency injection container for scoped services.</span></span>
* <span data-ttu-id="8250b-330">減少整個架構的配置，包括中介軟體案例和路由。</span><span class="sxs-lookup"><span data-stu-id="8250b-330">Reduction in allocations across the framework, including middleware scenarios and routing.</span></span>
* <span data-ttu-id="8250b-331">減少 WebSocket 連接的記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="8250b-331">Reduction in memory usage for WebSocket connections.</span></span>
* <span data-ttu-id="8250b-332">HTTPS 連線的記憶體減少和輸送量改善。</span><span class="sxs-lookup"><span data-stu-id="8250b-332">Memory reduction and throughput improvements for HTTPS connections.</span></span>
* <span data-ttu-id="8250b-333">新的優化和完全非同步 JSON 序列化程式。</span><span class="sxs-lookup"><span data-stu-id="8250b-333">New optimized and fully asynchronous JSON serializer.</span></span>
* <span data-ttu-id="8250b-334">減少在表單剖析中的記憶體使用量和輸送量改善。</span><span class="sxs-lookup"><span data-stu-id="8250b-334">Reduction in memory usage and throughput improvements in form parsing.</span></span>

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a><span data-ttu-id="8250b-335">ASP.NET Core 3.0 只會在 .NET Core 3.0 上執行</span><span class="sxs-lookup"><span data-stu-id="8250b-335">ASP.NET Core 3.0 only runs on .NET Core 3.0</span></span>

<span data-ttu-id="8250b-336">從 ASP.NET Core 3.0，.NET Framework 不再是支援的目標架構。</span><span class="sxs-lookup"><span data-stu-id="8250b-336">As of ASP.NET Core 3.0, .NET Framework is no longer a supported target framework.</span></span> <span data-ttu-id="8250b-337">以 .NET Framework 為目標的專案可以使用[.Net Core 2.1 LTS 版本](https://www.microsoft.com/net/download/dotnet-core/2.1)，以完全支援的方式繼續進行。</span><span class="sxs-lookup"><span data-stu-id="8250b-337">Projects targeting .NET Framework can continue in a fully supported fashion using the [.NET Core 2.1 LTS release](https://www.microsoft.com/net/download/dotnet-core/2.1).</span></span> <span data-ttu-id="8250b-338">大部分的 ASP.NET Core 2.1. x 相關套件會無限期地受到支援，超過 .NET Core 2.1 的三年 LTS 期限。</span><span class="sxs-lookup"><span data-stu-id="8250b-338">Most ASP.NET Core 2.1.x related packages will be supported indefinitely, beyond the three-year LTS period for .NET Core 2.1.</span></span>

<span data-ttu-id="8250b-339">如需遷移資訊，請參閱[將您的程式碼從 .NET Framework 移植到 .Net Core](/dotnet/core/porting/)。</span><span class="sxs-lookup"><span data-stu-id="8250b-339">For migration information, see [Port your code from .NET Framework to .NET Core](/dotnet/core/porting/).</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="8250b-340">使用 ASP.NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="8250b-340">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="8250b-341">[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中所包含的 ASP.NET Core 3.0 共用架構，不再需要專案檔中明確的 `<PackageReference />` 元素。</span><span class="sxs-lookup"><span data-stu-id="8250b-341">The ASP.NET Core 3.0 shared framework, contained in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), no longer requires an explicit `<PackageReference />` element in the project file.</span></span> <span data-ttu-id="8250b-342">在專案檔中使用 `Microsoft.NET.Sdk.Web` SDK 時，會自動參考共用架構：</span><span class="sxs-lookup"><span data-stu-id="8250b-342">The shared framework is automatically referenced when using the `Microsoft.NET.Sdk.Web` SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a><span data-ttu-id="8250b-343">從 ASP.NET Core 共用架構移除的元件</span><span class="sxs-lookup"><span data-stu-id="8250b-343">Assemblies removed from the ASP.NET Core shared framework</span></span>

<span data-ttu-id="8250b-344">從 ASP.NET Core 3.0 共用架構中移除的最顯著元件如下：</span><span class="sxs-lookup"><span data-stu-id="8250b-344">The most notable assemblies removed from the ASP.NET Core 3.0 shared framework are:</span></span>

* <span data-ttu-id="8250b-345">[Newtonsoft. Json](https://www.nuget.org/packages/Newtonsoft.Json/) （Json.NET）。</span><span class="sxs-lookup"><span data-stu-id="8250b-345">[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET).</span></span> <span data-ttu-id="8250b-346">若要將 Json.NET 新增至 ASP.NET Core 3.0，請參閱[新增 Newtonsoft 以 json 為基礎的 json 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。</span><span class="sxs-lookup"><span data-stu-id="8250b-346">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span> <span data-ttu-id="8250b-347">ASP.NET Core 3.0 引進讀取和寫入 JSON 的 `System.Text.Json`。</span><span class="sxs-lookup"><span data-stu-id="8250b-347">ASP.NET Core 3.0 introduces `System.Text.Json` for reading and writing JSON.</span></span> <span data-ttu-id="8250b-348">如需詳細資訊，請參閱本檔中的[新 JSON 序列化](#new-json-serialization)。</span><span class="sxs-lookup"><span data-stu-id="8250b-348">For more information, see [New JSON serialization](#new-json-serialization) in this document.</span></span>
* [<span data-ttu-id="8250b-349">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8250b-349">Entity Framework Core</span></span>](/ef/core/)

<span data-ttu-id="8250b-350">如需從共用架構中移除之元件的完整清單，請參閱[從 3.0 AspNetCore 中移除的元件](https://github.com/aspnet/AspNetCore/issues/3755)。</span><span class="sxs-lookup"><span data-stu-id="8250b-350">For a complete list of assemblies removed from the shared framework, see [Assemblies being removed from Microsoft.AspNetCore.App 3.0](https://github.com/aspnet/AspNetCore/issues/3755).</span></span> <span data-ttu-id="8250b-351">如需這項變更動機的詳細資訊，請參閱[3.0 中 AspNetCore 應用程式的重大變更](https://github.com/aspnet/Announcements/issues/325)，以及[第一次介紹 ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)中的變更。</span><span class="sxs-lookup"><span data-stu-id="8250b-351">For more information on the motivation for this change, see [Breaking changes to Microsoft.AspNetCore.App in 3.0](https://github.com/aspnet/Announcements/issues/325) and [A first look at changes coming in ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
