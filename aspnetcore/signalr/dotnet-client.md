---
title: ASP.NET Core SignalR.NET 用戶端
author: bradygaster
description: ASP.NET Core SignalR.NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153119"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="c6e09-103">ASP.NET Core SignalR.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="c6e09-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="c6e09-104">ASP.NET Core SignalR.NET 用戶端程式庫可讓您與 SignalR 中樞從.NET 應用程式進行通訊。</span><span class="sxs-lookup"><span data-stu-id="c6e09-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="c6e09-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c6e09-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c6e09-106">這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c6e09-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="c6e09-107">安裝 SignalR.NET 用戶端套件</span><span class="sxs-lookup"><span data-stu-id="c6e09-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="c6e09-108">`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="c6e09-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c6e09-109">若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：</span><span class="sxs-lookup"><span data-stu-id="c6e09-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c6e09-110">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="c6e09-110">Connect to a hub</span></span>

<span data-ttu-id="c6e09-111">若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="c6e09-112">中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。</span><span class="sxs-lookup"><span data-stu-id="c6e09-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="c6e09-113">設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="c6e09-114">啟動與連線`StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="c6e09-115">中斷連線的控制代碼</span><span class="sxs-lookup"><span data-stu-id="c6e09-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="c6e09-116">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="c6e09-116">Automatically reconnect</span></span>

<span data-ttu-id="c6e09-117"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection>可以設定為自動重新連線使用`WithAutomaticReconnect`方法<xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>。</span><span class="sxs-lookup"><span data-stu-id="c6e09-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="c6e09-118">它不會依預設會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="c6e09-119">如果沒有任何參數，`WithAutomaticReconnect()`設定用戶端一直等候 0、 2、 10 和 30 秒，分別是然後再嘗試每次重新連接嘗試，四個失敗的嘗試之後停止。</span><span class="sxs-lookup"><span data-stu-id="c6e09-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="c6e09-120">開始任何重新連接嘗試之前`HubConnection`會轉為`HubConnectionState.Reconnecting`狀態，並引發`Reconnecting`事件。</span><span class="sxs-lookup"><span data-stu-id="c6e09-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="c6e09-121">這便有機會時警告使用者的連線已遺失，並在停用 UI 項目。</span><span class="sxs-lookup"><span data-stu-id="c6e09-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="c6e09-122">非互動式應用程式可以啟動佇列，或卸除訊息。</span><span class="sxs-lookup"><span data-stu-id="c6e09-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c6e09-123">如果用戶端順利重新連線內的前四個的試`HubConnection`就會轉換回`Connected`狀態，並引發`Reconnected`事件。</span><span class="sxs-lookup"><span data-stu-id="c6e09-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="c6e09-124">這便有機會以通知的使用者已重新建立連線，以及從佇列中清除任何已排入佇列的訊息。</span><span class="sxs-lookup"><span data-stu-id="c6e09-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="c6e09-125">連線看起來完全新增到伺服器，因為新`ConnectionId`將提供給`Reconnected`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="c6e09-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="c6e09-126">`Reconnected`事件處理常式`connectionId`參數將為 null 如果`HubConnection`已設定為[略過交涉](xref:signalr/configuration#configure-client-options)。</span><span class="sxs-lookup"><span data-stu-id="c6e09-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c6e09-127">`WithAutomaticReconnect()` 不會設定`HubConnection`重試初始啟動時失敗，因此需要手動處理啟動失敗：</span><span class="sxs-lookup"><span data-stu-id="c6e09-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="c6e09-128">如果用戶端不會成功地重新連接內的前四個的試`HubConnection`會轉為`Disconnected`狀態，並引發<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件。</span><span class="sxs-lookup"><span data-stu-id="c6e09-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="c6e09-129">這便有機會嘗試以手動方式重新連線，或通知使用者此連接已經永久遺失。</span><span class="sxs-lookup"><span data-stu-id="c6e09-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c6e09-130">若要設定自訂的數字的中斷連線之前重新連接嘗試，或變更重新連線延遲時間，`WithAutomaticReconnect`接受代表數字以毫秒為單位的延遲開始每次重新連接嘗試之前等候的陣列。</span><span class="sxs-lookup"><span data-stu-id="c6e09-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="c6e09-131">上述範例會設定`HubConnection`啟動連線將會中斷之後，立即嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="c6e09-132">這也是預設組態，則為 true。</span><span class="sxs-lookup"><span data-stu-id="c6e09-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="c6e09-133">如果第一次重新連接嘗試失敗，第二次重新連接嘗試將也會立即開始而不是等到 2 秒，像是在預設組態中。</span><span class="sxs-lookup"><span data-stu-id="c6e09-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="c6e09-134">如果第二次重新連接嘗試失敗，第三次重新連接嘗試就會啟動在 10 秒，這又是預設組態類似。</span><span class="sxs-lookup"><span data-stu-id="c6e09-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="c6e09-135">自訂行為然後分離一次的預設行為的第三個重新連接嘗試失敗之後停止。</span><span class="sxs-lookup"><span data-stu-id="c6e09-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="c6e09-136">在預設組態會是一個更重新連接嘗試在另一個 30 秒內。</span><span class="sxs-lookup"><span data-stu-id="c6e09-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="c6e09-137">如果您想要更進一步控制時間和數字的自動重新連線嘗試`WithAutomaticReconnect`物件，實作會接受`IRetryPolicy`介面，其具有單一方法，名為`NextRetryDelay`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="c6e09-138">`NextRetryDelay` 接受單一引數與類型`RetryContext`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="c6e09-139">`RetryContext`有三個屬性： `PreviousRetryCount`，`ElapsedTime`並`RetryReason`哪些是`long`，則`TimeSpan`和`Exception`分別。</span><span class="sxs-lookup"><span data-stu-id="c6e09-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="c6e09-140">第一次重新連接嘗試中前, 兩者`PreviousRetryCount`和`ElapsedTime`將會是零，和`RetryReason`會導致連接遺失的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c6e09-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="c6e09-141">每個失敗的重試嘗試之後， `PreviousRetryCount` ，將會遞增`ElapsedTime`將會更新以反映所花費的時間重新連線到目前為止，和`RetryReason`會導致上次重新連接嘗試失敗的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c6e09-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="c6e09-142">`NextRetryDelay` 必須傳回可能是 TimeSpan，代表時間等待下一次重新連接嘗試或`null`如果`HubConnection`應該停止重新連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
            return TimeSpan.FromSeconds(_random.Next() * 10);
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
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

<span data-ttu-id="c6e09-143">或者，您可以在其中撰寫程式碼所示的手動將重新連線您的用戶端[以手動方式重新連線](#manually-reconnect)。</span><span class="sxs-lookup"><span data-stu-id="c6e09-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="c6e09-144">以手動方式重新連線</span><span class="sxs-lookup"><span data-stu-id="c6e09-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="c6e09-145">在 3.0 之前，SignalR 的.NET 用戶端不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="c6e09-146">您必須撰寫程式碼會以手動方式重新連接您的用戶端。</span><span class="sxs-lookup"><span data-stu-id="c6e09-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="c6e09-147">使用<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件回應失去連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="c6e09-148">例如，您可能要自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="c6e09-149">`Closed`事件要求委派，會傳回`Task`，可讓非同步程式碼執行，而不使用`async void`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="c6e09-150">為了滿足中的委派簽章`Closed`執行以同步方式傳回的事件處理常式`Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="c6e09-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="c6e09-151">非同步支援的主要原因是，因此您可以重新連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="c6e09-152">正在開始連線是非同步動作。</span><span class="sxs-lookup"><span data-stu-id="c6e09-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="c6e09-153">在 `Closed`連線，會重新啟動的處理常式考慮一些隨機的延遲，避免多載在伺服器上，等待，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="c6e09-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c6e09-154">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="c6e09-154">Call hub methods from client</span></span>

<span data-ttu-id="c6e09-155">`InvokeAsync` 集線器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="c6e09-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="c6e09-156">將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="c6e09-157">SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="c6e09-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="c6e09-158">`InvokeAsync`方法會傳回`Task`伺服器方法傳回時，其完成。</span><span class="sxs-lookup"><span data-stu-id="c6e09-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="c6e09-159">傳回值，如果有的話，可作為結果`Task`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="c6e09-160">在伺服器上的方法所擲回任何例外狀況會產生錯誤`Task`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="c6e09-161">使用`await`等待完成要求伺服器方法的語法和`try...catch`語法來處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="c6e09-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="c6e09-162">`SendAsync`方法會傳回`Task`其完成，當訊息已傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="c6e09-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="c6e09-163">未提供任何傳回的值，因為這`Task`不等候，直到伺服器在方法完成。</span><span class="sxs-lookup"><span data-stu-id="c6e09-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="c6e09-164">用戶端傳送訊息時擲回任何例外狀況會產生錯誤`Task`。</span><span class="sxs-lookup"><span data-stu-id="c6e09-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="c6e09-165">使用`await`和`try...catch`語法來處理將錯誤傳送。</span><span class="sxs-lookup"><span data-stu-id="c6e09-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="c6e09-166">如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="c6e09-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="c6e09-167">如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="c6e09-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c6e09-168">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="c6e09-168">Call client methods from hub</span></span>

<span data-ttu-id="c6e09-169">定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。</span><span class="sxs-lookup"><span data-stu-id="c6e09-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="c6e09-170">在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="c6e09-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="c6e09-171">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="c6e09-171">Error handling and logging</span></span>

<span data-ttu-id="c6e09-172">處理錯誤的 try / catch 陳述式。</span><span class="sxs-lookup"><span data-stu-id="c6e09-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="c6e09-173">檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。</span><span class="sxs-lookup"><span data-stu-id="c6e09-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="c6e09-174">其他資源</span><span class="sxs-lookup"><span data-stu-id="c6e09-174">Additional resources</span></span>

* [<span data-ttu-id="c6e09-175">中樞</span><span class="sxs-lookup"><span data-stu-id="c6e09-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c6e09-176">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="c6e09-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c6e09-177">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="c6e09-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="c6e09-178">Azure SignalR 服務的無伺服器文件</span><span class="sxs-lookup"><span data-stu-id="c6e09-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
