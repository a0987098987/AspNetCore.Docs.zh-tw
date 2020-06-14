---
title: ASP.NET Core SignalR .Net 用戶端
author: bradygaster
description: ASP.NET Core SignalR .Net 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/14/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: 7423624bdddfe6cee696cf87c255415170f46455
ms.sourcegitcommit: a423e8fcde4b6181a3073ed646a603ba20bfa5f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2020
ms.locfileid: "84755751"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="00162-103">ASP.NET Core SignalR .Net 用戶端</span><span class="sxs-lookup"><span data-stu-id="00162-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="00162-104">ASP.NET Core SignalR .net 用戶端程式庫可讓您 SignalR 從 .net 應用程式與中樞進行通訊。</span><span class="sxs-lookup"><span data-stu-id="00162-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="00162-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="00162-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="00162-106">本文中的程式碼範例是使用 ASP.NET Core .Net 用戶端的 WPF 應用程式 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="00162-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="00162-107">安裝 SignalR .net 用戶端封裝</span><span class="sxs-lookup"><span data-stu-id="00162-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="00162-108">[AspNetCore。 SignalR需要用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)封裝，.net 用戶端才能連接到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="00162-108">The [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package is required for .NET clients to connect to SignalR hubs.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="00162-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00162-109">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="00162-110">若要安裝用戶端程式庫，請在 [**套件管理員主控台**] 視窗中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="00162-110">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-cli"></a>[<span data-ttu-id="00162-111">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="00162-111">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="00162-112">若要安裝用戶端程式庫，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="00162-112">To install the client library, run the following command in a command shell:</span></span>

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a><span data-ttu-id="00162-113">連接到中樞</span><span class="sxs-lookup"><span data-stu-id="00162-113">Connect to a hub</span></span>

<span data-ttu-id="00162-114">若要建立連接，請建立， `HubConnectionBuilder` 並呼叫 `Build` 。</span><span class="sxs-lookup"><span data-stu-id="00162-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="00162-115">建立連接時，可以設定中樞 URL、通訊協定、傳輸類型、記錄層級、標頭和其他選項。</span><span class="sxs-lookup"><span data-stu-id="00162-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="00162-116">將任何方法插入，以設定任何必要的選項 `HubConnectionBuilder` `Build` 。</span><span class="sxs-lookup"><span data-stu-id="00162-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="00162-117">開始與的連接 `StartAsync` 。</span><span class="sxs-lookup"><span data-stu-id="00162-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="00162-118">處理遺失的連接</span><span class="sxs-lookup"><span data-stu-id="00162-118">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="00162-119">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="00162-119">Automatically reconnect</span></span>

<span data-ttu-id="00162-120"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection>可以設定為使用上的方法自動重新連接 `WithAutomaticReconnect` <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="00162-120">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="00162-121">根據預設，它不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="00162-121">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chathub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="00162-122">如果沒有任何參數， `WithAutomaticReconnect()` 則會先將用戶端設定為等待0、2、10和30秒，再嘗試重新連線嘗試，並在四次失敗嘗試之後停止。</span><span class="sxs-lookup"><span data-stu-id="00162-122">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="00162-123">開始任何重新連線嘗試之前， `HubConnection` 會轉換成 `HubConnectionState.Reconnecting` 狀態並引發 `Reconnecting` 事件。</span><span class="sxs-lookup"><span data-stu-id="00162-123">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="00162-124">這會讓使用者有機會警告連接已遺失，並停用 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="00162-124">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="00162-125">非互動式應用程式可以開始佇列或卸載訊息。</span><span class="sxs-lookup"><span data-stu-id="00162-125">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="00162-126">如果用戶端在前四次嘗試中成功重新連接， `HubConnection` 將會轉換回 `Connected` 狀態並引發 `Reconnected` 事件。</span><span class="sxs-lookup"><span data-stu-id="00162-126">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="00162-127">這讓您有機會通知使用者，已重新建立連線，並清除佇列的任何訊息。</span><span class="sxs-lookup"><span data-stu-id="00162-127">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="00162-128">由於連接看起來就是伺服器的新連線，因此 `ConnectionId` 會提供新的給 `Reconnected` 事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="00162-128">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="00162-129">`Reconnected` `connectionId` 如果 `HubConnection` 已設定為[略過協商](xref:signalr/configuration#configure-client-options)，事件處理常式的參數將會是 null。</span><span class="sxs-lookup"><span data-stu-id="00162-129">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="00162-130">`WithAutomaticReconnect()`不會將設定 `HubConnection` 為重試初始啟動失敗，因此必須手動處理啟動失敗：</span><span class="sxs-lookup"><span data-stu-id="00162-130">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

<span data-ttu-id="00162-131">如果用戶端在前四次嘗試中未成功重新連接， `HubConnection` 將會轉換成 `Disconnected` 狀態並引發 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> 事件。</span><span class="sxs-lookup"><span data-stu-id="00162-131">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="00162-132">這可讓您手動嘗試重新開機連線，或通知使用者已永久失去連接。</span><span class="sxs-lookup"><span data-stu-id="00162-132">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="00162-133">為了在中斷連線或變更重新連線時間之前設定自訂的重新連線嘗試次數，會 `WithAutomaticReconnect` 接受代表在開始每次重新連線嘗試之前等待的延遲（以毫秒為單位）的數位陣列。</span><span class="sxs-lookup"><span data-stu-id="00162-133">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chathub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="00162-134">上述範例會將設定 `HubConnection` 為，以便在連接中斷之後立即開始嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="00162-134">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="00162-135">這也適用于預設設定。</span><span class="sxs-lookup"><span data-stu-id="00162-135">This is also true for the default configuration.</span></span>

<span data-ttu-id="00162-136">如果第一次重新連線嘗試失敗，第二次重新連線嘗試也會立即開始，而不是等待2秒，就像在預設設定中一樣。</span><span class="sxs-lookup"><span data-stu-id="00162-136">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="00162-137">如果第二次重新連線嘗試失敗，第三次重新連線嘗試會在10秒內啟動，這再次是預設設定。</span><span class="sxs-lookup"><span data-stu-id="00162-137">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="00162-138">自訂行為接著會在第三次重新連線嘗試失敗之後停止，再次從預設行為分歧。</span><span class="sxs-lookup"><span data-stu-id="00162-138">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="00162-139">在預設設定中，會在另一個30秒內重新連線嘗試一次。</span><span class="sxs-lookup"><span data-stu-id="00162-139">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="00162-140">如果您想要更充分掌控自動重新連線嘗試的時間和數目，則會 `WithAutomaticReconnect` 接受一個物件 `IRetryPolicy` 來執行介面，此介面具有名為的單一方法 `NextRetryDelay` 。</span><span class="sxs-lookup"><span data-stu-id="00162-140">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="00162-141">`NextRetryDelay`接受具有類型的單一引數 `RetryContext` 。</span><span class="sxs-lookup"><span data-stu-id="00162-141">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="00162-142">`RetryContext`有三個屬性： `PreviousRetryCount` 、 `ElapsedTime` 和 `RetryReason` ， `long` 分別為、和 `TimeSpan` `Exception` 。</span><span class="sxs-lookup"><span data-stu-id="00162-142">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason`, which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="00162-143">第一次重新連線嘗試之前， `PreviousRetryCount` 和都 `ElapsedTime` 是零，而且 `RetryReason` 會是造成連接遺失的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="00162-143">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="00162-144">每次重試失敗之後，將會 `PreviousRetryCount` 遞增一次， `ElapsedTime` 將會更新以反映到目前為止的重新連接所花費的時間，而且 `RetryReason` 將會是導致上次重新連線嘗試失敗的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="00162-144">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="00162-145">`NextRetryDelay`必須傳回代表下次重新連線嘗試之前等候時間的 TimeSpan，或 `null` 如果應該停止重新連接，則為 `HubConnection` 。</span><span class="sxs-lookup"><span data-stu-id="00162-145">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.NextDouble() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chathub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

<span data-ttu-id="00162-146">或者，您可以撰寫程式碼，以手動方式重新連線用戶端，如[手動重新](#manually-reconnect)連線中所示。</span><span class="sxs-lookup"><span data-stu-id="00162-146">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="00162-147">手動重新連線</span><span class="sxs-lookup"><span data-stu-id="00162-147">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="00162-148">在3.0 之前，.NET 用戶端 SignalR 不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="00162-148">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="00162-149">您必須撰寫程式碼，以手動方式重新連線用戶端。</span><span class="sxs-lookup"><span data-stu-id="00162-149">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="00162-150">使用 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> 事件來回應遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="00162-150">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="00162-151">例如，您可能會想要自動重新連接。</span><span class="sxs-lookup"><span data-stu-id="00162-151">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="00162-152">`Closed`事件需要傳回的委派 `Task` ，這可讓非同步程式碼在不使用的情況下執行 `async void` 。</span><span class="sxs-lookup"><span data-stu-id="00162-152">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="00162-153">若要在同步執行的事件處理常式中滿足委派簽章 `Closed` ，請傳回 `Task.CompletedTask` ：</span><span class="sxs-lookup"><span data-stu-id="00162-153">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="00162-154">非同步支援的主要原因是為了讓您可以重新開機連接。</span><span class="sxs-lookup"><span data-stu-id="00162-154">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="00162-155">啟動連接是非同步動作。</span><span class="sxs-lookup"><span data-stu-id="00162-155">Starting a connection is an async action.</span></span>

<span data-ttu-id="00162-156">在 `Closed` 重新開機連接的處理常式中，請考慮等候一些隨機延遲，以避免伺服器超載，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="00162-156">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="00162-157">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="00162-157">Call hub methods from client</span></span>

<span data-ttu-id="00162-158">`InvokeAsync`在中樞上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="00162-158">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="00162-159">將中樞方法名稱和 hub 方法中定義的任何引數傳遞至 `InvokeAsync` 。</span><span class="sxs-lookup"><span data-stu-id="00162-159">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> SignalR<span data-ttu-id="00162-160">是非同步，因此 `async` `await` 在進行呼叫時，請使用和。</span><span class="sxs-lookup"><span data-stu-id="00162-160"> is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="00162-161">方法會傳回在 `InvokeAsync` `Task` 伺服器方法傳回時完成的。</span><span class="sxs-lookup"><span data-stu-id="00162-161">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="00162-162">傳回值（如果有的話）會當做的結果提供 `Task` 。</span><span class="sxs-lookup"><span data-stu-id="00162-162">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="00162-163">伺服器上方法所擲回的任何例外狀況都會產生錯誤 `Task` 。</span><span class="sxs-lookup"><span data-stu-id="00162-163">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="00162-164">請使用 `await` 語法來等候伺服器方法完成，並使用 `try...catch` 語法來處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="00162-164">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="00162-165">方法會傳回在 `SendAsync` `Task` 訊息傳送至伺服器時完成的。</span><span class="sxs-lookup"><span data-stu-id="00162-165">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="00162-166">因為這 `Task` 不會等到伺服器方法完成，所以不會提供傳回值。</span><span class="sxs-lookup"><span data-stu-id="00162-166">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="00162-167">傳送訊息時，在用戶端擲回的任何例外狀況都會產生錯誤 `Task` 。</span><span class="sxs-lookup"><span data-stu-id="00162-167">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="00162-168">使用 `await` 和 `try...catch` 語法來處理傳送錯誤。</span><span class="sxs-lookup"><span data-stu-id="00162-168">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="00162-169">如果您是 SignalR 在*無伺服器模式*中使用 Azure 服務，則無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="00162-169">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="00162-170">如需詳細資訊，請參閱[ SignalR 服務檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="00162-170">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="00162-171">從中樞呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="00162-171">Call client methods from hub</span></span>

<span data-ttu-id="00162-172">定義中樞在 `connection.On` 建立之後，但在開始連接之前所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="00162-172">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="00162-173">`connection.On`當伺服器端程式碼使用方法呼叫它時，中的上述程式碼就會執行 `SendAsync` 。</span><span class="sxs-lookup"><span data-stu-id="00162-173">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="00162-174">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="00162-174">Error handling and logging</span></span>

<span data-ttu-id="00162-175">使用 try catch 語句來處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="00162-175">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="00162-176">檢查 `Exception` 物件，以判斷錯誤發生後要採取的適當動作。</span><span class="sxs-lookup"><span data-stu-id="00162-176">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="00162-177">其他資源</span><span class="sxs-lookup"><span data-stu-id="00162-177">Additional resources</span></span>

* [<span data-ttu-id="00162-178">中樞</span><span class="sxs-lookup"><span data-stu-id="00162-178">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="00162-179">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="00162-179">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="00162-180">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="00162-180">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* <span data-ttu-id="00162-181">[Azure SignalR 服務無伺服器檔](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="00162-181">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
