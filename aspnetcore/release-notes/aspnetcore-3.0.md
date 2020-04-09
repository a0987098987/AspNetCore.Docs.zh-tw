---
title: ASP.NET核心 3.0 中的新增功能
author: rick-anderson
description: 瞭解 ASP.NET 酷 3.0 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.0
ms.openlocfilehash: 4886673a9b16b8be8d9a0b0d5c7002a91760544e
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80976972"
---
# <a name="whats-new-in-aspnet-core-30"></a><span data-ttu-id="01b62-103">ASP.NET核心 3.0 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="01b62-103">What's new in ASP.NET Core 3.0</span></span>

<span data-ttu-id="01b62-104">本文重點介紹了 ASP.NET 酷 3.0 中最重要的更改,並包含指向相關文檔的連結。</span><span class="sxs-lookup"><span data-stu-id="01b62-104">This article highlights the most significant changes in ASP.NET Core 3.0 with links to relevant documentation.</span></span>

## Blazor

Blazor<span data-ttu-id="01b62-105">是ASP.NET核心中用於使用 .NET 建構式互動式客戶端 Web UI 的新框架:</span><span class="sxs-lookup"><span data-stu-id="01b62-105"> is a new framework in ASP.NET Core for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="01b62-106">使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="01b62-106">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="01b62-107">共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="01b62-107">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="01b62-108">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="01b62-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

Blazor<span data-ttu-id="01b62-109">框架支援的機制:</span><span class="sxs-lookup"><span data-stu-id="01b62-109"> framework supported scenarios:</span></span>

* <span data-ttu-id="01b62-110">可重用的 UI 元件(Razor 元件)</span><span class="sxs-lookup"><span data-stu-id="01b62-110">Reusable UI components (Razor components)</span></span>
* <span data-ttu-id="01b62-111">用戶端路由</span><span class="sxs-lookup"><span data-stu-id="01b62-111">Client-side routing</span></span>
* <span data-ttu-id="01b62-112">元件佈局</span><span class="sxs-lookup"><span data-stu-id="01b62-112">Component layouts</span></span>
* <span data-ttu-id="01b62-113">支援相依項</span><span class="sxs-lookup"><span data-stu-id="01b62-113">Support for dependency injection</span></span>
* <span data-ttu-id="01b62-114">表單和驗證</span><span class="sxs-lookup"><span data-stu-id="01b62-114">Forms and validation</span></span>
* <span data-ttu-id="01b62-115">使用 Razor 函式庫建構元件庫</span><span class="sxs-lookup"><span data-stu-id="01b62-115">Build component libraries with Razor class libraries</span></span>
* <span data-ttu-id="01b62-116">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="01b62-116">JavaScript interop</span></span>

<span data-ttu-id="01b62-117">如需詳細資訊，請參閱 <xref:blazor/index>。</span><span class="sxs-lookup"><span data-stu-id="01b62-117">For more information, see <xref:blazor/index>.</span></span>

### <a name="opno-locblazor-server"></a>Blazor<span data-ttu-id="01b62-118">伺服器</span><span class="sxs-lookup"><span data-stu-id="01b62-118"> Server</span></span>

Blazor<span data-ttu-id="01b62-119">將元件呈現邏輯與 UI 更新的應用方式分離。</span><span class="sxs-lookup"><span data-stu-id="01b62-119"> decouples component rendering logic from how UI updates are applied.</span></span> Blazor<span data-ttu-id="01b62-120">伺服器支援在 ASP.NET核心應用中在伺服器上託管 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="01b62-120"> Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="01b62-121">UI 更新SignalR通過 連接處理。</span><span class="sxs-lookup"><span data-stu-id="01b62-121">UI updates are handled over a SignalR connection.</span></span> Blazor<span data-ttu-id="01b62-122">ASP.NET核心 3.0 中支援伺服器。</span><span class="sxs-lookup"><span data-stu-id="01b62-122"> Server is supported in ASP.NET Core 3.0.</span></span>

### <a name="opno-locblazor-webassembly-preview"></a>Blazor<span data-ttu-id="01b62-123">網路組裝(預覽)</span><span class="sxs-lookup"><span data-stu-id="01b62-123"> WebAssembly (Preview)</span></span>

Blazor<span data-ttu-id="01b62-124">應用也可以使用基於 Web 大會的 .NET 運行時直接在瀏覽器中運行。</span><span class="sxs-lookup"><span data-stu-id="01b62-124"> apps can also be run directly in the browser using a WebAssembly-based .NET runtime.</span></span> Blazor<span data-ttu-id="01b62-125">Web組裝處於預覽狀態,ASP.NET酷睿 3.0 中*不支援*Web 組裝。</span><span class="sxs-lookup"><span data-stu-id="01b62-125"> WebAssembly is in preview and *not* supported in ASP.NET Core 3.0.</span></span> Blazor<span data-ttu-id="01b62-126">Web組裝將在將來發佈的ASP.NET核心版中得到支援。</span><span class="sxs-lookup"><span data-stu-id="01b62-126"> WebAssembly will be supported in a future release of ASP.NET Core.</span></span>

### <a name="razor-components"></a><span data-ttu-id="01b62-127">剃刀元件</span><span class="sxs-lookup"><span data-stu-id="01b62-127">Razor components</span></span>

Blazor<span data-ttu-id="01b62-128">應用程式是從元件構建的。</span><span class="sxs-lookup"><span data-stu-id="01b62-128"> apps are built from components.</span></span> <span data-ttu-id="01b62-129">元件是用戶介面 (UI) 的自包含塊,如頁面、對話框或窗體。</span><span class="sxs-lookup"><span data-stu-id="01b62-129">Components are self-contained chunks of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="01b62-130">元件是定義 UI 呈現邏輯和用戶端事件處理程式的正常 .NET 類。</span><span class="sxs-lookup"><span data-stu-id="01b62-130">Components are normal .NET classes that define UI rendering logic and client-side event handlers.</span></span> <span data-ttu-id="01b62-131">無需 JAVAScript 即可創建豐富的互動式 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="01b62-131">You can create rich interactive web apps without JavaScript.</span></span>

<span data-ttu-id="01b62-132">中的Blazor元件通常使用 Razor 語法創作,這是 HTML 和 C# 的自然混合。</span><span class="sxs-lookup"><span data-stu-id="01b62-132">Components in Blazor are typically authored using Razor syntax, a natural blend of HTML and C#.</span></span> <span data-ttu-id="01b62-133">剃刀元件類似於剃刀頁面和 MVC 視圖,因為它們都使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="01b62-133">Razor components are similar to Razor Pages and MVC views in that they both use Razor.</span></span> <span data-ttu-id="01b62-134">與基於請求-回應模型的頁面和視圖不同,元件專門用於處理 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="01b62-134">Unlike pages and views, which are based on a request-response model, components are used specifically for handling UI composition.</span></span>

## <a name="grpc"></a><span data-ttu-id="01b62-135">gRPC</span><span class="sxs-lookup"><span data-stu-id="01b62-135">gRPC</span></span>

<span data-ttu-id="01b62-136">[gRPC](https://grpc.io/):</span><span class="sxs-lookup"><span data-stu-id="01b62-136">[gRPC](https://grpc.io/):</span></span>

* <span data-ttu-id="01b62-137">是一個流行的高性能 RPC(遠端過程調用)框架。</span><span class="sxs-lookup"><span data-stu-id="01b62-137">Is a popular, high-performance RPC (remote procedure call) framework.</span></span>
* <span data-ttu-id="01b62-138">為 API 開發提供了一種有意見的合同優先方法。</span><span class="sxs-lookup"><span data-stu-id="01b62-138">Offers an opinionated contract-first approach to API development.</span></span>
* <span data-ttu-id="01b62-139">使用現代技術,例如:</span><span class="sxs-lookup"><span data-stu-id="01b62-139">Uses modern technologies such as:</span></span>

  * <span data-ttu-id="01b62-140">用於傳輸的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="01b62-140">HTTP/2 for transport.</span></span>
  * <span data-ttu-id="01b62-141">協定緩衝區作為介面描述語言。</span><span class="sxs-lookup"><span data-stu-id="01b62-141">Protocol Buffers as the interface description language.</span></span>
  * <span data-ttu-id="01b62-142">二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="01b62-142">Binary serialization format.</span></span>
* <span data-ttu-id="01b62-143">提供如下功能:</span><span class="sxs-lookup"><span data-stu-id="01b62-143">Provides features such as:</span></span>

  * <span data-ttu-id="01b62-144">驗證</span><span class="sxs-lookup"><span data-stu-id="01b62-144">Authentication</span></span>
  * <span data-ttu-id="01b62-145">雙向流和流控制。</span><span class="sxs-lookup"><span data-stu-id="01b62-145">Bidirectional streaming and flow control.</span></span>
  * <span data-ttu-id="01b62-146">取消和超時。</span><span class="sxs-lookup"><span data-stu-id="01b62-146">Cancellation and timeouts.</span></span>

<span data-ttu-id="01b62-147">ASP.NET核心 3.0 中的 gRPC 功能包括:</span><span class="sxs-lookup"><span data-stu-id="01b62-147">gRPC functionality in ASP.NET Core 3.0 includes:</span></span>

* <span data-ttu-id="01b62-148">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)&ndash;用於託管 gRPC 服務ASP.NET核心框架。</span><span class="sxs-lookup"><span data-stu-id="01b62-148">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; An ASP.NET Core framework for hosting gRPC services.</span></span> <span data-ttu-id="01b62-149">ASP.NET酷睿上的 gRPC 與標準ASP.NET核心功能(如日誌記錄、依賴項注入 (DI)、身份驗證和授權)集成。</span><span class="sxs-lookup"><span data-stu-id="01b62-149">gRPC on ASP.NET Core integrates with standard ASP.NET Core features like logging, dependency injection (DI), authentication, and authorization.</span></span>
* <span data-ttu-id="01b62-150">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; gRPC 客戶端,用於 .NET Core,`HttpClient`該用戶端基於熟悉的 。</span><span class="sxs-lookup"><span data-stu-id="01b62-150">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; A gRPC client for .NET Core that builds upon the familiar `HttpClient`.</span></span>
* <span data-ttu-id="01b62-151">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC`HttpClientFactory`用戶端整合。</span><span class="sxs-lookup"><span data-stu-id="01b62-151">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC client integration with `HttpClientFactory`.</span></span>

<span data-ttu-id="01b62-152">如需詳細資訊，請參閱 <xref:grpc/index>。</span><span class="sxs-lookup"><span data-stu-id="01b62-152">For more information, see <xref:grpc/index>.</span></span>

## SignalR

<span data-ttu-id="01b62-153">有關移轉說明,請參閱[更新SignalR程式碼](xref:migration/22-to-30#signalr)。</span><span class="sxs-lookup"><span data-stu-id="01b62-153">See [Update SignalR code](xref:migration/22-to-30#signalr) for migration instructions.</span></span> SignalR<span data-ttu-id="01b62-154">現在用於`System.Text.Json`序列化/去序列化 JSON 消息。</span><span class="sxs-lookup"><span data-stu-id="01b62-154"> now uses `System.Text.Json` to serialize/deserialize JSON messages.</span></span> <span data-ttu-id="01b62-155">有關還原基於`Newtonsoft.Json`序列化器的說明,請參閱[切換到 Newtonsoft.Json。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="01b62-155">See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson) for instructions to restore the `Newtonsoft.Json`-based serializer.</span></span>

<span data-ttu-id="01b62-156">在 JavaScript 和SignalR.NET 用戶端中,添加了自動重新連接的支援。</span><span class="sxs-lookup"><span data-stu-id="01b62-156">In the JavaScript and .NET Clients for SignalR, support was added for automatic reconnection.</span></span> <span data-ttu-id="01b62-157">默認情況下,客戶端嘗試立即重新連接,並在必要時在 2、10 和 30 秒後重試。</span><span class="sxs-lookup"><span data-stu-id="01b62-157">By default, the client tries to reconnect immediately and retry after 2, 10, and 30 seconds if necessary.</span></span> <span data-ttu-id="01b62-158">如果用戶端成功重新連接,它將收到一個新的連接 ID。</span><span class="sxs-lookup"><span data-stu-id="01b62-158">If the client successfully reconnects, it receives a new connection ID.</span></span> <span data-ttu-id="01b62-159">自動重新連線是選擇加入的:</span><span class="sxs-lookup"><span data-stu-id="01b62-159">Automatic reconnect is opt-in:</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="01b62-160">可以通過傳遞基於毫秒的持續時間陣列來指定重新連接間隔:</span><span class="sxs-lookup"><span data-stu-id="01b62-160">The reconnection intervals can be specified by passing an array of millisecond-based durations:</span></span>

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

<span data-ttu-id="01b62-161">可以傳入自定義實現以完全控制重新連接間隔。</span><span class="sxs-lookup"><span data-stu-id="01b62-161">A custom implementation can be passed in for full control of the reconnection intervals.</span></span>

<span data-ttu-id="01b62-162">如果重新連線在最後一次重新連接間隔後失敗:</span><span class="sxs-lookup"><span data-stu-id="01b62-162">If the reconnection fails after the last reconnect interval:</span></span>

* <span data-ttu-id="01b62-163">客戶端認為連接處於離線狀態。</span><span class="sxs-lookup"><span data-stu-id="01b62-163">The client considers the connection is offline.</span></span>
* <span data-ttu-id="01b62-164">用戶端停止嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="01b62-164">The client stops trying to reconnect.</span></span>

<span data-ttu-id="01b62-165">在重新連接嘗試期間,更新應用 UI 以通知使用者正在嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="01b62-165">During reconnection attempts, update the app UI to notify the user that the reconnection is being attempted.</span></span>

<span data-ttu-id="01b62-166">為了在連接中斷時提供 UISignalR回饋, 用戶端 API 已展開,以包括以下事件處理程式:</span><span class="sxs-lookup"><span data-stu-id="01b62-166">To provide UI feedback when the connection is interrupted, the SignalR client API has been expanded to include the following event handlers:</span></span>

* <span data-ttu-id="01b62-167">`onreconnecting`:讓開發人員有機會禁用 UI 或讓使用者知道應用處於脫機狀態。</span><span class="sxs-lookup"><span data-stu-id="01b62-167">`onreconnecting`:  Gives developers an opportunity to disable UI or to let users know the app is offline.</span></span>
* <span data-ttu-id="01b62-168">`onreconnected`:使開發人員有機會在重新建立連接后更新 UI。</span><span class="sxs-lookup"><span data-stu-id="01b62-168">`onreconnected`: Gives developers an opportunity to update the UI once the connection is reestablished.</span></span>

<span data-ttu-id="01b62-169">以下代碼用於`onreconnecting`在嘗試連接時更新 UI:</span><span class="sxs-lookup"><span data-stu-id="01b62-169">The following code uses `onreconnecting` to update the UI while trying to connect:</span></span>

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="01b62-170">以下代碼用於`onreconnected`在連線時更新 UI:</span><span class="sxs-lookup"><span data-stu-id="01b62-170">The following code uses `onreconnected` to update the UI on connection:</span></span>

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

SignalR<span data-ttu-id="01b62-171">當中心方法需要授權時,3.0 及更高版本向授權處理程式提供自定義資源。</span><span class="sxs-lookup"><span data-stu-id="01b62-171"> 3.0 and later provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="01b62-172">資源是的`HubInvocationContext`實例。</span><span class="sxs-lookup"><span data-stu-id="01b62-172">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="01b62-173">包含`HubInvocationContext`:</span><span class="sxs-lookup"><span data-stu-id="01b62-173">The `HubInvocationContext` includes the:</span></span>

* `HubCallerContext`
* <span data-ttu-id="01b62-174">要調用的集線器方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="01b62-174">Name of the hub method being invoked.</span></span>
* <span data-ttu-id="01b62-175">對中心方法的參數。</span><span class="sxs-lookup"><span data-stu-id="01b62-175">Arguments to the hub method.</span></span>

<span data-ttu-id="01b62-176">請考慮以下聊天室應用示例,該應用允許通過 Azure 活動目錄登錄多個組織。</span><span class="sxs-lookup"><span data-stu-id="01b62-176">Consider the following example of a chat room app allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="01b62-177">擁有 Microsoft 帳戶的任何人都可以登錄聊天,但只有擁有組織的成員可以禁止使用者或查看使用者的聊天歷史記錄。</span><span class="sxs-lookup"><span data-stu-id="01b62-177">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization can ban users or view users' chat histories.</span></span> <span data-ttu-id="01b62-178">該應用程式可能會限制特定使用者的某些功能。</span><span class="sxs-lookup"><span data-stu-id="01b62-178">The app could restrict certain functionality from specific users.</span></span>

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

<span data-ttu-id="01b62-179">在前面的代碼中,`DomainRestrictedRequirement`用作自`IAuthorizationRequirement`訂 。</span><span class="sxs-lookup"><span data-stu-id="01b62-179">In the preceding code, `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="01b62-180">由於`HubInvocationContext`資源參數正在傳入,因此內部邏輯可以:</span><span class="sxs-lookup"><span data-stu-id="01b62-180">Because the `HubInvocationContext` resource parameter is being passed in, the internal logic can:</span></span>

* <span data-ttu-id="01b62-181">檢查調用集線器的上下文。</span><span class="sxs-lookup"><span data-stu-id="01b62-181">Inspect the context in which the Hub is being called.</span></span>
* <span data-ttu-id="01b62-182">決定允許使用者執行單個中心方法。</span><span class="sxs-lookup"><span data-stu-id="01b62-182">Make decisions on allowing the user to execute individual Hub methods.</span></span>

<span data-ttu-id="01b62-183">單個中心方法可以使用代碼在運行時檢查的策略名稱進行標記。</span><span class="sxs-lookup"><span data-stu-id="01b62-183">Individual Hub methods can be marked with the name of the policy the code checks at run-time.</span></span> <span data-ttu-id="01b62-184">當客戶端嘗試呼叫單個中心方法時`DomainRestrictedRequirement`, 處理程式將運行和控制對這些方法的訪問。</span><span class="sxs-lookup"><span data-stu-id="01b62-184">As clients attempt to call individual Hub methods, the `DomainRestrictedRequirement` handler runs and controls access to the methods.</span></span> <span data-ttu-id="01b62-185">依`DomainRestrictedRequirement`控制項存取方式:</span><span class="sxs-lookup"><span data-stu-id="01b62-185">Based on the way the `DomainRestrictedRequirement` controls access:</span></span>

* <span data-ttu-id="01b62-186">所有登入使用者可以呼叫`SendMessage`該方法 。</span><span class="sxs-lookup"><span data-stu-id="01b62-186">All logged-in users can call the `SendMessage` method.</span></span>
* <span data-ttu-id="01b62-187">只有使用`@jabbr.net`電子郵件地址登錄的使用者才能查看使用者的歷史。</span><span class="sxs-lookup"><span data-stu-id="01b62-187">Only users who have logged in with a `@jabbr.net` email address can view users' histories.</span></span>
* <span data-ttu-id="01b62-188">只能`bob42@jabbr.net`禁止用戶離開聊天室。</span><span class="sxs-lookup"><span data-stu-id="01b62-188">Only `bob42@jabbr.net` can ban users from the chat room.</span></span>

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

<span data-ttu-id="01b62-189">建立`DomainRestricted`策略可能涉及:</span><span class="sxs-lookup"><span data-stu-id="01b62-189">Creating the `DomainRestricted` policy might involve:</span></span>

* <span data-ttu-id="01b62-190">在*Startup.cs,* 添加新政策。</span><span class="sxs-lookup"><span data-stu-id="01b62-190">In *Startup.cs*, adding the new policy.</span></span>
* <span data-ttu-id="01b62-191">將自定義`DomainRestrictedRequirement`要求作為參數提供。</span><span class="sxs-lookup"><span data-stu-id="01b62-191">Provide the custom `DomainRestrictedRequirement` requirement as a parameter.</span></span>
* <span data-ttu-id="01b62-192">`DomainRestricted`註冊授權中間件。</span><span class="sxs-lookup"><span data-stu-id="01b62-192">Registering `DomainRestricted` with the authorization middleware.</span></span>

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

SignalR<span data-ttu-id="01b62-193">集線器使用[連接端點路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="01b62-193"> hubs use [Endpoint Routing](xref:fundamentals/routing).</span></span> SignalR<span data-ttu-id="01b62-194">中心連線以前顯示式完成:</span><span class="sxs-lookup"><span data-stu-id="01b62-194"> hub connection was previously done explicitly:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="01b62-195">在以前的版本中,開發人員需要將控制器、Razor 頁面和集線器連接到不同位置。</span><span class="sxs-lookup"><span data-stu-id="01b62-195">In the previous version, developers needed to wire up controllers, Razor pages, and hubs in a variety of places.</span></span> <span data-ttu-id="01b62-196">明確連線會導致一系列幾乎相同的路由段:</span><span class="sxs-lookup"><span data-stu-id="01b62-196">Explicit connection results in a series of nearly-identical routing segments:</span></span>

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

SignalR<span data-ttu-id="01b62-197">3.0 集線器可通過端點路由進行路由。</span><span class="sxs-lookup"><span data-stu-id="01b62-197"> 3.0 hubs can be routed via endpoint routing.</span></span> <span data-ttu-id="01b62-198">使用終結點路由時,通常可以在`UseRouting`中 配置所有路由:</span><span class="sxs-lookup"><span data-stu-id="01b62-198">With endpoint routing, typically all routing can be configured in `UseRouting`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="01b62-199">ASP.NET核心 3.0SignalR新增了:</span><span class="sxs-lookup"><span data-stu-id="01b62-199">ASP.NET Core 3.0 SignalR added:</span></span>

<span data-ttu-id="01b62-200">用戶端到伺服器流。</span><span class="sxs-lookup"><span data-stu-id="01b62-200">Client-to-server streaming.</span></span> <span data-ttu-id="01b62-201">使用用戶端到伺服器流式處理時,伺服器端方法可以採用或`IAsyncEnumerable<T>``ChannelReader<T>`的實例。</span><span class="sxs-lookup"><span data-stu-id="01b62-201">With client-to-server streaming, server-side methods can take instances of either an `IAsyncEnumerable<T>` or `ChannelReader<T>`.</span></span> <span data-ttu-id="01b62-202">在以下 C# 範例中,Hub`UploadStream`上的方法將從客戶端接收字串流:</span><span class="sxs-lookup"><span data-stu-id="01b62-202">In the following C# sample, the `UploadStream` method on the Hub will receive a stream of strings from the client:</span></span>

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

<span data-ttu-id="01b62-203">.NET 客戶端應用可以`IAsyncEnumerable<T>``ChannelReader<T>`將 或`stream`實例作為`UploadStream`上面 的 Hub 方法的參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="01b62-203">.NET client apps can pass either an `IAsyncEnumerable<T>` or `ChannelReader<T>` instance as the `stream` argument of the `UploadStream` Hub method above.</span></span>

<span data-ttu-id="01b62-204">循環`for`完成並離開本地函數後,將傳送流完成:</span><span class="sxs-lookup"><span data-stu-id="01b62-204">After the `for` loop has completed and the local function exits, the stream completion is sent:</span></span>

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

<span data-ttu-id="01b62-205">JavaScript 用戶端應用SignalR`Subject`使用 (或[RxJS 主題](https://rxjs.dev/api/index/class/Subject)`stream`) 進行`UploadStream`上述中心方法的參數。</span><span class="sxs-lookup"><span data-stu-id="01b62-205">JavaScript client apps use the SignalR `Subject` (or an [RxJS Subject](https://rxjs.dev/api/index/class/Subject)) for the `stream` argument of the `UploadStream` Hub method above.</span></span>

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

<span data-ttu-id="01b62-206">JavaScript 程式碼可以`subject.next`使用 方法 處理字串捕獲並準備發送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="01b62-206">The JavaScript code could use the `subject.next` method to handle strings as they are captured and ready to be sent to the server.</span></span>

```javascript
subject.next("example");
subject.complete();
```

<span data-ttu-id="01b62-207">使用前兩個代碼段等代碼,可以創建即時流式處理體驗。</span><span class="sxs-lookup"><span data-stu-id="01b62-207">Using code like the two preceding snippets, real-time streaming experiences can be created.</span></span>

## <a name="new-json-serialization"></a><span data-ttu-id="01b62-208">新的 JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="01b62-208">New JSON serialization</span></span>

<span data-ttu-id="01b62-209">ASP.NET Core 3.0<xref:System.Text.Json>現在預設用於 JSON 序列化:</span><span class="sxs-lookup"><span data-stu-id="01b62-209">ASP.NET Core 3.0 now uses <xref:System.Text.Json> by default for JSON serialization:</span></span>

* <span data-ttu-id="01b62-210">非同步讀取和寫入 JSON。</span><span class="sxs-lookup"><span data-stu-id="01b62-210">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="01b62-211">針對 UTF-8 文本進行了優化。</span><span class="sxs-lookup"><span data-stu-id="01b62-211">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="01b62-212">效能通常高於`Newtonsoft.Json`。</span><span class="sxs-lookup"><span data-stu-id="01b62-212">Typically higher performance than `Newtonsoft.Json`.</span></span>

<span data-ttu-id="01b62-213">要將Json.NET新增到ASP.NET核心 3.0,請參閱[新增 Newtonsoft.基於 Json 的 JSON 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。</span><span class="sxs-lookup"><span data-stu-id="01b62-213">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span>

## <a name="new-razor-directives"></a><span data-ttu-id="01b62-214">新的剃刀指令</span><span class="sxs-lookup"><span data-stu-id="01b62-214">New Razor directives</span></span>

<span data-ttu-id="01b62-215">以下清單包含新的 Razor 指令:</span><span class="sxs-lookup"><span data-stu-id="01b62-215">The following list contains new Razor directives:</span></span>

* <span data-ttu-id="01b62-216">[`@attribute`](xref:mvc/views/razor#attribute)&ndash;該`@attribute`指令將給定的屬性應用於生成的頁面或視圖的類。</span><span class="sxs-lookup"><span data-stu-id="01b62-216">[`@attribute`](xref:mvc/views/razor#attribute) &ndash; The `@attribute` directive applies the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="01b62-217">例如： `@attribute [Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="01b62-217">For example, `@attribute [Authorize]`.</span></span>
* <span data-ttu-id="01b62-218">[`@implements`](xref:mvc/views/razor#implements)&ndash;該`@implements`指令為生成的類實現介面。</span><span class="sxs-lookup"><span data-stu-id="01b62-218">[`@implements`](xref:mvc/views/razor#implements) &ndash; The `@implements` directive implements an interface for the generated class.</span></span> <span data-ttu-id="01b62-219">例如： `@implements IDisposable` 。</span><span class="sxs-lookup"><span data-stu-id="01b62-219">For example, `@implements IDisposable`.</span></span>

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a><span data-ttu-id="01b62-220">識別伺服器4支援對 Web API 與 SA 的驗證與授權</span><span class="sxs-lookup"><span data-stu-id="01b62-220">IdentityServer4 supports authentication and authorization for web APIs and SPAs</span></span>

<span data-ttu-id="01b62-221">ASP.NET Core 3.0 使用 Web API 授權支援,在單頁應用 (SPA) 中提供身份驗證。</span><span class="sxs-lookup"><span data-stu-id="01b62-221">ASP.NET Core 3.0 offers authentication in Single Page Apps (SPAs) using the support for web API authorization.</span></span> <span data-ttu-id="01b62-222">ASP.NET用於驗證和存儲使用者的核心標識與[標識Server4](https://identityserver.io/)相結合,用於實現開放ID連接。</span><span class="sxs-lookup"><span data-stu-id="01b62-222">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer4](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="01b62-223">標識伺服器4 是一個 OpenID 連接和 OAuth 2.0 框架,用於ASP.NET核心 3.0。</span><span class="sxs-lookup"><span data-stu-id="01b62-223">IdentityServer4 is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core 3.0.</span></span> <span data-ttu-id="01b62-224">它支援以下安全功能:</span><span class="sxs-lookup"><span data-stu-id="01b62-224">It enables the following security features:</span></span>

* <span data-ttu-id="01b62-225">為服務身份驗證 (AaaS)</span><span class="sxs-lookup"><span data-stu-id="01b62-225">Authentication as a Service (AaaS)</span></span>
* <span data-ttu-id="01b62-226">跨多個應用程式類型的單一登入 /關閉 (SSO)</span><span class="sxs-lookup"><span data-stu-id="01b62-226">Single sign-on/off (SSO) over multiple application types</span></span>
* <span data-ttu-id="01b62-227">API 的存取控制</span><span class="sxs-lookup"><span data-stu-id="01b62-227">Access control for APIs</span></span>
* <span data-ttu-id="01b62-228">聯合閘道</span><span class="sxs-lookup"><span data-stu-id="01b62-228">Federation Gateway</span></span>

<span data-ttu-id="01b62-229">有關詳細資訊,請參閱[識別Server4 文件](http://docs.identityserver.io/en/latest/index.html)或[SA 的身份驗證和授權](xref:security/authentication/identity/spa)。</span><span class="sxs-lookup"><span data-stu-id="01b62-229">For more information, see [the IdentityServer4 documentation](http://docs.identityserver.io/en/latest/index.html) or [Authentication and authorization for SPAs](xref:security/authentication/identity/spa).</span></span>

## <a name="certificate-and-kerberos-authentication"></a><span data-ttu-id="01b62-230">憑證與 Kerberos 驗證</span><span class="sxs-lookup"><span data-stu-id="01b62-230">Certificate and Kerberos authentication</span></span>

<span data-ttu-id="01b62-231">憑證認證要求:</span><span class="sxs-lookup"><span data-stu-id="01b62-231">Certificate authentication requires:</span></span>

* <span data-ttu-id="01b62-232">配置伺服器以接受證書。</span><span class="sxs-lookup"><span data-stu-id="01b62-232">Configuring the server to accept certificates.</span></span>
* <span data-ttu-id="01b62-233">在`Startup.Configure`中 添加身份驗證中間件。</span><span class="sxs-lookup"><span data-stu-id="01b62-233">Adding the authentication middleware in `Startup.Configure`.</span></span>
* <span data-ttu-id="01b62-234">在`Startup.ConfigureServices`中 添加證書身份驗證服務。</span><span class="sxs-lookup"><span data-stu-id="01b62-234">Adding the certificate authentication service in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="01b62-235">憑證認證選項包括:</span><span class="sxs-lookup"><span data-stu-id="01b62-235">Options for certificate authentication include the ability to:</span></span>

* <span data-ttu-id="01b62-236">接受自簽名證書。</span><span class="sxs-lookup"><span data-stu-id="01b62-236">Accept self-signed certificates.</span></span>
* <span data-ttu-id="01b62-237">檢查證書吊銷。</span><span class="sxs-lookup"><span data-stu-id="01b62-237">Check for certificate revocation.</span></span>
* <span data-ttu-id="01b62-238">檢查提供的證書中具有正確的使用標誌。</span><span class="sxs-lookup"><span data-stu-id="01b62-238">Check that the proffered certificate has the right usage flags in it.</span></span>

<span data-ttu-id="01b62-239">默認用戶主體是從證書屬性構造的。</span><span class="sxs-lookup"><span data-stu-id="01b62-239">A default user principal is constructed from the certificate properties.</span></span> <span data-ttu-id="01b62-240">用戶主體包含一個事件,該事件允許補充或替換主體。</span><span class="sxs-lookup"><span data-stu-id="01b62-240">The user principal contains an event that enables supplementing or replacing the principal.</span></span> <span data-ttu-id="01b62-241">如需詳細資訊，請參閱 <xref:security/authentication/certauth>。</span><span class="sxs-lookup"><span data-stu-id="01b62-241">For more information, see <xref:security/authentication/certauth>.</span></span>

<span data-ttu-id="01b62-242">[Windows 身份驗證](/windows-server/security/windows-authentication/windows-authentication-overview)已擴展到 Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="01b62-242">[Windows Authentication](/windows-server/security/windows-authentication/windows-authentication-overview) has been extended onto Linux and macOS.</span></span> <span data-ttu-id="01b62-243">在以前的版本中,Windows 身份驗證僅限於[IIS](xref:host-and-deploy/iis/index)和[Hsys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="01b62-243">In previous versions, Windows Authentication was limited to [IIS](xref:host-and-deploy/iis/index) and [HttpSys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="01b62-244">在ASP.NET核心3.0中[,Kestrel](xref:fundamentals/servers/kestrel)能夠在Windows、Linux和macOS上為Windows網域加入的主機使用協商[、Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview)和[NTLM。](/windows-server/security/kerberos/ntlm-overview)</span><span class="sxs-lookup"><span data-stu-id="01b62-244">In ASP.NET Core 3.0, [Kestrel](xref:fundamentals/servers/kestrel) has the ability to use Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview), and [NTLM on Windows](/windows-server/security/kerberos/ntlm-overview), Linux, and macOS for Windows domain-joined hosts.</span></span> <span data-ttu-id="01b62-245">Kestrel 對這些身份驗證方案的支援由[Microsoft.AspNetCore.身份驗證.協商 NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)包提供。</span><span class="sxs-lookup"><span data-stu-id="01b62-245">Kestrel support of these authentication schemes is provided by the [Microsoft.AspNetCore.Authentication.Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package.</span></span> <span data-ttu-id="01b62-246">與其他身份驗證服務一樣,在寬配置身份驗證應用,然後配置服務:</span><span class="sxs-lookup"><span data-stu-id="01b62-246">As with the other authentication services, configure authentication app wide, then configure the service:</span></span>

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

<span data-ttu-id="01b62-247">主機要求:</span><span class="sxs-lookup"><span data-stu-id="01b62-247">Host requirements:</span></span>

* <span data-ttu-id="01b62-248">Windows 主機必須將[服務主體名稱](/windows/win32/ad/service-principal-names)(SPN) 添加到託管應用的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="01b62-248">Windows hosts must have [Service Principal Names](/windows/win32/ad/service-principal-names) (SPNs) added to the user account hosting the app.</span></span>
* <span data-ttu-id="01b62-249">Linux 和macOS電腦必須加入域。</span><span class="sxs-lookup"><span data-stu-id="01b62-249">Linux and macOS machines must be joined to the domain.</span></span>
  * <span data-ttu-id="01b62-250">必須為 Web 進程創建 SPN。</span><span class="sxs-lookup"><span data-stu-id="01b62-250">SPNs must be created for the web process.</span></span>
  * <span data-ttu-id="01b62-251">必須在主機上產生與設定[Keytab 檔案](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)。</span><span class="sxs-lookup"><span data-stu-id="01b62-251">[Keytab files](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) must be generated and configured on the host machine.</span></span>

<span data-ttu-id="01b62-252">如需詳細資訊，請參閱 <xref:security/authentication/windowsauth>。</span><span class="sxs-lookup"><span data-stu-id="01b62-252">For more information, see <xref:security/authentication/windowsauth>.</span></span>

## <a name="template-changes"></a><span data-ttu-id="01b62-253">樣本變更</span><span class="sxs-lookup"><span data-stu-id="01b62-253">Template changes</span></span>

<span data-ttu-id="01b62-254">Web UI 樣本(Razor 頁面、帶控制器和檢視的 MVC)已刪除以下內容:</span><span class="sxs-lookup"><span data-stu-id="01b62-254">The web UI templates (Razor Pages, MVC with controller and views) have the following removed:</span></span>

* <span data-ttu-id="01b62-255">Cookie 同意 UI 不再包括在內。</span><span class="sxs-lookup"><span data-stu-id="01b62-255">The cookie consent UI is no longer included.</span></span> <span data-ttu-id="01b62-256">要在 ASP.NET酷睿 3.0 範本生成的應用中啟用<xref:security/gdpr>Cookie 同意 功能,請參閱。</span><span class="sxs-lookup"><span data-stu-id="01b62-256">To enable the cookie consent feature in an ASP.NET Core 3.0 template-generated app, see <xref:security/gdpr>.</span></span>
* <span data-ttu-id="01b62-257">文本和相關靜態資產現在被引用為本地檔案,而不是使用CDN。</span><span class="sxs-lookup"><span data-stu-id="01b62-257">Scripts and related static assets are now referenced as local files instead of using CDNs.</span></span> <span data-ttu-id="01b62-258">有關詳細資訊,請參閱[文本和相關靜態資產現在被引用為本地檔,而不是基於當前環境(aspnet/AspNetCore.Docs #14350)使用CDN。](https://github.com/dotnet/AspNetCore.Docs/issues/14350)</span><span class="sxs-lookup"><span data-stu-id="01b62-258">For more information, see [Scripts and related static assets are now referenced as local files instead of using CDNs based on the current environment (aspnet/AspNetCore.Docs #14350)](https://github.com/dotnet/AspNetCore.Docs/issues/14350).</span></span>

<span data-ttu-id="01b62-259">角度範本更新為使用角 8。</span><span class="sxs-lookup"><span data-stu-id="01b62-259">The Angular template updated to use Angular 8.</span></span>

<span data-ttu-id="01b62-260">默認情況下,Razor 類庫 (RCL) 範本預設為 Razor 元件開發。</span><span class="sxs-lookup"><span data-stu-id="01b62-260">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="01b62-261">Visual Studio 中的新範本選項為頁面和檢視提供範本支援。</span><span class="sxs-lookup"><span data-stu-id="01b62-261">A new template option in Visual Studio provides template support for pages and views.</span></span> <span data-ttu-id="01b62-262">從命令 shell 中的樣本建立 RCL`--support-pages-and-views`時`dotnet new razorclasslib --support-pages-and-views`,傳遞選項 ( 。</span><span class="sxs-lookup"><span data-stu-id="01b62-262">When creating an RCL from the template in a command shell, pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`).</span></span>

## <a name="generic-host"></a><span data-ttu-id="01b62-263">一般主機</span><span class="sxs-lookup"><span data-stu-id="01b62-263">Generic Host</span></span>

<span data-ttu-id="01b62-264">ASP.NET核心 3.0 樣本<xref:fundamentals/host/generic-host>使用 。</span><span class="sxs-lookup"><span data-stu-id="01b62-264">The ASP.NET Core 3.0 templates use <xref:fundamentals/host/generic-host>.</span></span> <span data-ttu-id="01b62-265">以前使用<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>的版本。</span><span class="sxs-lookup"><span data-stu-id="01b62-265">Previous versions used <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="01b62-266">使用 .NET 核心通用<xref:Microsoft.Extensions.Hosting.HostBuilder>主機 ( ) 可更好地將 ASP.NET 核心應用與其他非 Web 特定的伺服器方案整合。</span><span class="sxs-lookup"><span data-stu-id="01b62-266">Using the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) provides better integration of ASP.NET Core apps with other server scenarios that aren't web-specific.</span></span> <span data-ttu-id="01b62-267">有關詳細資訊,請參閱[主機產生器取代 WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="01b62-267">For more information, see [HostBuilder replaces WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span></span>

### <a name="host-configuration"></a><span data-ttu-id="01b62-268">主機組態</span><span class="sxs-lookup"><span data-stu-id="01b62-268">Host configuration</span></span>

<span data-ttu-id="01b62-269">在 ASP.NET 酷 3.0 發佈之前,`ASPNETCORE_`已載入預 固定的環境變數,用於 Web 主機的主機配置。</span><span class="sxs-lookup"><span data-stu-id="01b62-269">Prior to the release of ASP.NET Core 3.0, environment variables prefixed with `ASPNETCORE_` were loaded for host configuration of the Web Host.</span></span> <span data-ttu-id="01b62-270">在 3.0`AddEnvironmentVariables`中,用於載入預固定`DOTNET_``CreateDefaultBuilder`的具有 主機配置的環境變數。</span><span class="sxs-lookup"><span data-stu-id="01b62-270">In 3.0, `AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for host configuration with `CreateDefaultBuilder`.</span></span>

### <a name="changes-to-startup-constructor-injection"></a><span data-ttu-id="01b62-271">對啟動建構函式注入的變更</span><span class="sxs-lookup"><span data-stu-id="01b62-271">Changes to Startup constructor injection</span></span>

<span data-ttu-id="01b62-272">泛型主機僅支援建構`Startup`函數注入的以下類型:</span><span class="sxs-lookup"><span data-stu-id="01b62-272">The Generic Host only supports the following types for `Startup` constructor injection:</span></span>

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="01b62-273">所有服務仍可以直接作為參數注入到方法。 `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="01b62-273">All services can still be injected directly as arguments to the `Startup.Configure` method.</span></span> <span data-ttu-id="01b62-274">有關詳細資訊,請參閱[通用主機限制啟動構造函數注入(aspnet/公告#353)](https://github.com/aspnet/Announcements/issues/353)。</span><span class="sxs-lookup"><span data-stu-id="01b62-274">For more information, see [Generic Host restricts Startup constructor injection (aspnet/Announcements #353)](https://github.com/aspnet/Announcements/issues/353).</span></span>

## <a name="kestrel"></a><span data-ttu-id="01b62-275">Kestrel</span><span class="sxs-lookup"><span data-stu-id="01b62-275">Kestrel</span></span>

* <span data-ttu-id="01b62-276">已更新 Kestrel 配置,以便遷移到通用主機。</span><span class="sxs-lookup"><span data-stu-id="01b62-276">Kestrel configuration has been updated for the migration to the Generic Host.</span></span> <span data-ttu-id="01b62-277">在 3.0 中,Kestrel`ConfigureWebHostDefaults`配置在 提供的 Web 主機生成器上。</span><span class="sxs-lookup"><span data-stu-id="01b62-277">In 3.0, Kestrel is configured on the web host builder provided by `ConfigureWebHostDefaults`.</span></span>
* <span data-ttu-id="01b62-278">連接配配器已從 Kestrel 中刪除,並替換為連接中間件,這與 ASP.NET核心管道中的 HTTP 中間件類似,但對於較低級別的連接。</span><span class="sxs-lookup"><span data-stu-id="01b62-278">Connection Adapters have been removed from Kestrel and replaced with Connection Middleware, which is similar to HTTP Middleware in the ASP.NET Core pipeline but for lower-level connections.</span></span>
* <span data-ttu-id="01b62-279">Kestrel 傳輸層已作為 公共介面`Connections.Abstractions`在 中 公開。</span><span class="sxs-lookup"><span data-stu-id="01b62-279">The Kestrel transport layer has been exposed as a public interface in `Connections.Abstractions`.</span></span>
* <span data-ttu-id="01b62-280">通過將尾隨標頭移動到新集合,解決了標頭和尾部之間的歧義。</span><span class="sxs-lookup"><span data-stu-id="01b62-280">Ambiguity between headers and trailers has been resolved by moving trailing headers to a new collection.</span></span>
* <span data-ttu-id="01b62-281">同步 I/O API(如`HttpRequest.Body.Read`)是導致應用崩潰的常見線程不足源。</span><span class="sxs-lookup"><span data-stu-id="01b62-281">Synchronous I/O APIs, such as `HttpRequest.Body.Read`, are a common source of thread starvation leading to app crashes.</span></span> <span data-ttu-id="01b62-282">在 3.0`AllowSynchronousIO`中,默認情況下處於禁用狀態。</span><span class="sxs-lookup"><span data-stu-id="01b62-282">In 3.0, `AllowSynchronousIO` is disabled by default.</span></span>

<span data-ttu-id="01b62-283">如需詳細資訊，請參閱 <xref:migration/22-to-30#kestrel>。</span><span class="sxs-lookup"><span data-stu-id="01b62-283">For more information, see <xref:migration/22-to-30#kestrel>.</span></span>

## <a name="http2-enabled-by-default"></a><span data-ttu-id="01b62-284">預設的功能啟用 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="01b62-284">HTTP/2 enabled by default</span></span>

<span data-ttu-id="01b62-285">默認情況下,HTTP/2 在 KEStrel 中為 HTTPS 終結點啟用。</span><span class="sxs-lookup"><span data-stu-id="01b62-285">HTTP/2 is enabled by default in Kestrel for HTTPS endpoints.</span></span> <span data-ttu-id="01b62-286">當作業系統支援時,將啟用對 IIS 或 HTTP.sys 的 HTTP/2 支援。</span><span class="sxs-lookup"><span data-stu-id="01b62-286">HTTP/2 support for IIS or HTTP.sys is enabled when supported by the operating system.</span></span>

## <a name="eventcounters-on-request"></a><span data-ttu-id="01b62-287">應要求的事件計數器</span><span class="sxs-lookup"><span data-stu-id="01b62-287">EventCounters on request</span></span>

<span data-ttu-id="01b62-288">主主事件來源`Microsoft.AspNetCore.Hosting`傳入要求相關的以下新<xref:System.Diagnostics.Tracing.EventCounter>型態:</span><span class="sxs-lookup"><span data-stu-id="01b62-288">The Hosting EventSource, `Microsoft.AspNetCore.Hosting`, emits the following new <xref:System.Diagnostics.Tracing.EventCounter> types related to incoming requests:</span></span>

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a><span data-ttu-id="01b62-289">端點路由</span><span class="sxs-lookup"><span data-stu-id="01b62-289">Endpoint routing</span></span>

<span data-ttu-id="01b62-290">終結點路由,允許框架(例如,MVC)很好地與中間件配合使用,得到了增強:</span><span class="sxs-lookup"><span data-stu-id="01b62-290">Endpoint Routing, which allows frameworks (for example, MVC) to work well with middleware, is enhanced:</span></span>

* <span data-ttu-id="01b62-291">中間件和端點的順序在`Startup.Configure`的請求處理管道中可配置。</span><span class="sxs-lookup"><span data-stu-id="01b62-291">The order of middleware and endpoints is configurable in the request processing pipeline of `Startup.Configure`.</span></span>
* <span data-ttu-id="01b62-292">端點和中間件與其他基於核心的技術(如運行狀況檢查)很好地組成了ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="01b62-292">Endpoints and middleware compose well with other ASP.NET Core-based technologies, such as Health Checks.</span></span>
* <span data-ttu-id="01b62-293">端點可以在中間件和 MVC 中實現策略,如 CORS 或授權。</span><span class="sxs-lookup"><span data-stu-id="01b62-293">Endpoints can implement a policy, such as CORS or authorization, in both middleware and MVC.</span></span>
* <span data-ttu-id="01b62-294">篩選器和屬性可以放置在控制器中的方法上。</span><span class="sxs-lookup"><span data-stu-id="01b62-294">Filters and attributes can be placed on methods in controllers.</span></span>

<span data-ttu-id="01b62-295">如需詳細資訊，請參閱 <xref:fundamentals/routing#routing-basics>。</span><span class="sxs-lookup"><span data-stu-id="01b62-295">For more information, see <xref:fundamentals/routing#routing-basics>.</span></span>

## <a name="health-checks"></a><span data-ttu-id="01b62-296">健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="01b62-296">Health Checks</span></span>

<span data-ttu-id="01b62-297">運行狀況檢查使用與通用主機的終結點路由。</span><span class="sxs-lookup"><span data-stu-id="01b62-297">Health Checks use endpoint routing with the Generic Host.</span></span> <span data-ttu-id="01b62-298">在`Startup.Configure`中`MapHealthChecks`,使用終結點網址 或相對路徑呼叫終結點產生器:</span><span class="sxs-lookup"><span data-stu-id="01b62-298">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

<span data-ttu-id="01b62-299">執行狀況檢查終結點可以:</span><span class="sxs-lookup"><span data-stu-id="01b62-299">Health Checks endpoints can:</span></span>

* <span data-ttu-id="01b62-300">指定一個或多個允許的主機/埠。</span><span class="sxs-lookup"><span data-stu-id="01b62-300">Specify one or more permitted hosts/ports.</span></span>
* <span data-ttu-id="01b62-301">需要授權。</span><span class="sxs-lookup"><span data-stu-id="01b62-301">Require authorization.</span></span>
* <span data-ttu-id="01b62-302">需要 CORS。</span><span class="sxs-lookup"><span data-stu-id="01b62-302">Require CORS.</span></span>

<span data-ttu-id="01b62-303">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="01b62-303">For more information, see the following articles:</span></span>

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a><span data-ttu-id="01b62-304">HTTPContext 上的導管</span><span class="sxs-lookup"><span data-stu-id="01b62-304">Pipes on HttpContext</span></span>

<span data-ttu-id="01b62-305">現在可以讀取請求正文並使用<xref:System.IO.Pipelines>API 寫入回應正文。</span><span class="sxs-lookup"><span data-stu-id="01b62-305">It's now possible to read the request body and write the response body using the <xref:System.IO.Pipelines> API.</span></span> <span data-ttu-id="01b62-306">此</span><span class="sxs-lookup"><span data-stu-id="01b62-306">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> <span data-ttu-id="01b62-307">`HttpRequest.BodyReader`屬性提供可用於<xref:System.IO.Pipelines.PipeReader>讀取要求正文的 。</span><span class="sxs-lookup"><span data-stu-id="01b62-307">`HttpRequest.BodyReader` property provides a <xref:System.IO.Pipelines.PipeReader> that can be used to read the request body.</span></span> <span data-ttu-id="01b62-308">此</span><span class="sxs-lookup"><span data-stu-id="01b62-308">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.> --> <span data-ttu-id="01b62-309">`HttpResponse.BodyWriter`屬性提供可用於<xref:System.IO.Pipelines.PipeWriter>寫入回應正文的 。</span><span class="sxs-lookup"><span data-stu-id="01b62-309">`HttpResponse.BodyWriter` property provides a <xref:System.IO.Pipelines.PipeWriter> that can be used to write the response body.</span></span> <span data-ttu-id="01b62-310">`HttpRequest.BodyReader`是`HttpRequest.Body`流的類比。</span><span class="sxs-lookup"><span data-stu-id="01b62-310">`HttpRequest.BodyReader` is an analogue of the `HttpRequest.Body` stream.</span></span> <span data-ttu-id="01b62-311">`HttpResponse.BodyWriter`是`HttpResponse.Body`流的類比。</span><span class="sxs-lookup"><span data-stu-id="01b62-311">`HttpResponse.BodyWriter` is an analogue of the `HttpResponse.Body` stream.</span></span>

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a><span data-ttu-id="01b62-312">改進 IIS 中的錯誤報告</span><span class="sxs-lookup"><span data-stu-id="01b62-312">Improved error reporting in IIS</span></span>

<span data-ttu-id="01b62-313">在IIS中託管ASP.NET核心應用時啟動錯誤現在會產生更豐富的診斷數據。</span><span class="sxs-lookup"><span data-stu-id="01b62-313">Startup errors when hosting ASP.NET Core apps in IIS now produce richer diagnostic data.</span></span> <span data-ttu-id="01b62-314">這些錯誤將報告給 Windows 事件日誌,在適用的情況下具有堆疊跟蹤。</span><span class="sxs-lookup"><span data-stu-id="01b62-314">These errors are reported to the Windows Event Log with stack traces wherever applicable.</span></span> <span data-ttu-id="01b62-315">此外,所有警告、錯誤和未處理的異常將記錄到 Windows 事件日誌。</span><span class="sxs-lookup"><span data-stu-id="01b62-315">In addition, all warnings, errors, and unhandled exceptions are logged to the Windows Event Log.</span></span>

## <a name="worker-service-and-worker-sdk"></a><span data-ttu-id="01b62-316">協助服務與輔助角色 SDK</span><span class="sxs-lookup"><span data-stu-id="01b62-316">Worker Service and Worker SDK</span></span>

<span data-ttu-id="01b62-317">.NET Core 3.0 引入了新的輔助服務應用範本。</span><span class="sxs-lookup"><span data-stu-id="01b62-317">.NET Core 3.0 introduces the new Worker Service app template.</span></span> <span data-ttu-id="01b62-318">此範本為在 .NET Core 中編寫長時間運行的服務提供了一個起點。</span><span class="sxs-lookup"><span data-stu-id="01b62-318">This template provides a starting point for writing long running services in .NET Core.</span></span>

<span data-ttu-id="01b62-319">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="01b62-319">For more information, see:</span></span>

* [<span data-ttu-id="01b62-320">.NET 核心工作人員作為 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="01b62-320">.NET Core Workers as Windows Services</span></span>](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a><span data-ttu-id="01b62-321">轉寄的標頭 中間件改進</span><span class="sxs-lookup"><span data-stu-id="01b62-321">Forwarded Headers Middleware improvements</span></span>

<span data-ttu-id="01b62-322">在以前版本的ASP.NET核心中,在<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>部署到<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>Azure Linux或IIS以外的任何反向代理後面時,調用和調用都存在問題。</span><span class="sxs-lookup"><span data-stu-id="01b62-322">In previous versions of ASP.NET Core, calling <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> and  <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> were problematic when deployed to an Azure Linux or behind any reverse proxy other than IIS.</span></span> <span data-ttu-id="01b62-323">早期版本的修復程序記錄在[Linux 和非 IIS 反向代理的方案](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies)中。</span><span class="sxs-lookup"><span data-stu-id="01b62-323">The fix for previous versions is documented in [Forward the scheme for Linux and non-IIS reverse proxies](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span></span>

<span data-ttu-id="01b62-324">此方案在ASP.NET核心3.0中修復。</span><span class="sxs-lookup"><span data-stu-id="01b62-324">This scenario is fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="01b62-325">當環境變數設定為`ASPNETCORE_FORWARDEDHEADERS_ENABLED``true`時,主機啟用[轉寄的標頭中間件](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="01b62-325">The host enables the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) when the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span> <span data-ttu-id="01b62-326">`ASPNETCORE_FORWARDEDHEADERS_ENABLED`設置為`true`我們的容器映射。</span><span class="sxs-lookup"><span data-stu-id="01b62-326">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` is set to `true` in our container images.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="01b62-327">效能改善</span><span class="sxs-lookup"><span data-stu-id="01b62-327">Performance improvements</span></span>

<span data-ttu-id="01b62-328">ASP.NET酷睿 3.0 包含許多改進,可降低記憶體使用並提高輸送量:</span><span class="sxs-lookup"><span data-stu-id="01b62-328">ASP.NET Core 3.0 includes many improvements that reduce memory usage and improve throughput:</span></span>

* <span data-ttu-id="01b62-329">使用內置依賴項注入容器進行作用域服務時,記憶體使用量減少。</span><span class="sxs-lookup"><span data-stu-id="01b62-329">Reduction in memory usage when using the built-in dependency injection container for scoped services.</span></span>
* <span data-ttu-id="01b62-330">減少整個框架的分配,包括中間件方案和路由。</span><span class="sxs-lookup"><span data-stu-id="01b62-330">Reduction in allocations across the framework, including middleware scenarios and routing.</span></span>
* <span data-ttu-id="01b62-331">減少 WebSocket 連接的記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="01b62-331">Reduction in memory usage for WebSocket connections.</span></span>
* <span data-ttu-id="01b62-332">HTTPS 連接的記憶體縮減和輸送量改進。</span><span class="sxs-lookup"><span data-stu-id="01b62-332">Memory reduction and throughput improvements for HTTPS connections.</span></span>
* <span data-ttu-id="01b62-333">新的優化和完全異步的 JSON 序列化器。</span><span class="sxs-lookup"><span data-stu-id="01b62-333">New optimized and fully asynchronous JSON serializer.</span></span>
* <span data-ttu-id="01b62-334">減少記憶體使用和表單分析的輸送量改進。</span><span class="sxs-lookup"><span data-stu-id="01b62-334">Reduction in memory usage and throughput improvements in form parsing.</span></span>

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a><span data-ttu-id="01b62-335">ASP.NET核心 3.0 僅在 .NET Core 3.0 上執行</span><span class="sxs-lookup"><span data-stu-id="01b62-335">ASP.NET Core 3.0 only runs on .NET Core 3.0</span></span>

<span data-ttu-id="01b62-336">從 ASP.NET核心 3.0 起,.NET 框架不再是受支持的目標框架。</span><span class="sxs-lookup"><span data-stu-id="01b62-336">As of ASP.NET Core 3.0, .NET Framework is no longer a supported target framework.</span></span> <span data-ttu-id="01b62-337">目標 .NET 框架的專案可以使用[.NET Core 2.1 LTS 版本](https://dotnet.microsoft.com/download/dotnet-core/2.1)以完全支援的方式繼續。</span><span class="sxs-lookup"><span data-stu-id="01b62-337">Projects targeting .NET Framework can continue in a fully supported fashion using the [.NET Core 2.1 LTS release](https://dotnet.microsoft.com/download/dotnet-core/2.1).</span></span> <span data-ttu-id="01b62-338">大多數ASP.NET核心 2.1.x 相關包將無限期地支援,超過 .NET Core 2.1 的三年 LTS 期限。</span><span class="sxs-lookup"><span data-stu-id="01b62-338">Most ASP.NET Core 2.1.x related packages will be supported indefinitely, beyond the three-year LTS period for .NET Core 2.1.</span></span>

<span data-ttu-id="01b62-339">有關移轉資訊,請參閱[將代碼從 .NET 框架移植到 .NET 核心](/dotnet/core/porting/)。</span><span class="sxs-lookup"><span data-stu-id="01b62-339">For migration information, see [Port your code from .NET Framework to .NET Core](/dotnet/core/porting/).</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="01b62-340">使用ASP.NET核心共用框架</span><span class="sxs-lookup"><span data-stu-id="01b62-340">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="01b62-341">ASP.NET Core 3.0 共用框架包含在[Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)中`<PackageReference />`,不再需要專案檔中 的顯式元素。</span><span class="sxs-lookup"><span data-stu-id="01b62-341">The ASP.NET Core 3.0 shared framework, contained in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), no longer requires an explicit `<PackageReference />` element in the project file.</span></span> <span data-ttu-id="01b62-342">在專案檔中使用 SDK`Microsoft.NET.Sdk.Web`時, 將自動參考共用框架:</span><span class="sxs-lookup"><span data-stu-id="01b62-342">The shared framework is automatically referenced when using the `Microsoft.NET.Sdk.Web` SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a><span data-ttu-id="01b62-343">從ASP.NET核心共用框架中移除的程式集</span><span class="sxs-lookup"><span data-stu-id="01b62-343">Assemblies removed from the ASP.NET Core shared framework</span></span>

<span data-ttu-id="01b62-344">從 ASP.NET Core 3.0 共用框架中刪除的最值得注意的程式集是:</span><span class="sxs-lookup"><span data-stu-id="01b62-344">The most notable assemblies removed from the ASP.NET Core 3.0 shared framework are:</span></span>

* <span data-ttu-id="01b62-345">[牛頓軟.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET)。</span><span class="sxs-lookup"><span data-stu-id="01b62-345">[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET).</span></span> <span data-ttu-id="01b62-346">要將Json.NET新增到ASP.NET核心 3.0,請參閱[新增 Newtonsoft.基於 Json 的 JSON 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。</span><span class="sxs-lookup"><span data-stu-id="01b62-346">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span> <span data-ttu-id="01b62-347">ASP.NET核心3.0介紹`System.Text.Json`閱讀和寫作JSON。</span><span class="sxs-lookup"><span data-stu-id="01b62-347">ASP.NET Core 3.0 introduces `System.Text.Json` for reading and writing JSON.</span></span> <span data-ttu-id="01b62-348">有關詳細資訊,請參閱本文件中[的新 JSON 序列化](#new-json-serialization)。</span><span class="sxs-lookup"><span data-stu-id="01b62-348">For more information, see [New JSON serialization](#new-json-serialization) in this document.</span></span>
* [<span data-ttu-id="01b62-349">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="01b62-349">Entity Framework Core</span></span>](/ef/core/)

<span data-ttu-id="01b62-350">有關從共用框架中刪除的程式集的完整清單,請參閱從[Microsoft 中刪除的程式集。](https://github.com/dotnet/AspNetCore/issues/3755)</span><span class="sxs-lookup"><span data-stu-id="01b62-350">For a complete list of assemblies removed from the shared framework, see [Assemblies being removed from Microsoft.AspNetCore.App 3.0](https://github.com/dotnet/AspNetCore/issues/3755).</span></span> <span data-ttu-id="01b62-351">有關此更改的動機的詳細資訊,請參閱[3.0 中對 Microsoft.AspNetCore.App 的中斷更改](https://github.com/aspnet/Announcements/issues/325),並[首先查看 ASP.NET酷 3.0 中即將出現的變化](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)。</span><span class="sxs-lookup"><span data-stu-id="01b62-351">For more information on the motivation for this change, see [Breaking changes to Microsoft.AspNetCore.App in 3.0](https://github.com/aspnet/Announcements/issues/325) and [A first look at changes coming in ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
 