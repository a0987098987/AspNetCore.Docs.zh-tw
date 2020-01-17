---
title: ASP.NET Core SignalR .NET 用戶端
author: bradygaster
description: .NET 用戶端 SignalR ASP.NET Core 的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/14/2020
no-loc:
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: 39d9eccdb1e0457b177e75e6f94f3dd185b0093d
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146312"
---
# <a name="aspnet-core-opno-locsignalr-net-client"></a>ASP.NET Core SignalR .NET 用戶端

ASP.NET Core SignalR .NET 用戶端程式庫可讓您從 .NET 應用程式與 SignalR 中樞進行通訊。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

本文中的程式碼範例是使用 ASP.NET Core SignalR .NET 用戶端的 WPF 應用程式。

## <a name="install-the-opno-locsignalr-net-client-package"></a>安裝 SignalR .NET 用戶端封裝

[AspNetCore.SignalR。需要用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)封裝，.net 用戶端才能連接到 SignalR 中樞。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

若要安裝用戶端程式庫，請在 [**套件管理員主控台**] 視窗中執行下列命令：

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

若要安裝用戶端程式庫，請在命令 shell 中執行下列命令：

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a>連接到中樞

若要建立連接，請建立 `HubConnectionBuilder` 並呼叫 `Build`。 建立連接時，可以設定中樞 URL、通訊協定、傳輸類型、記錄層級、標頭和其他選項。 將任何 `HubConnectionBuilder` 方法插入 `Build`，以設定任何必要的選項。 開始與 `StartAsync`的連接。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>處理遺失的連接

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>自動重新連線

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> 可以設定為在 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>上使用 `WithAutomaticReconnect` 方法自動重新連接。 根據預設，它不會自動重新連線。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

在沒有任何參數的情況下，`WithAutomaticReconnect()` 會先將用戶端設定為等待0、2、10和30秒，再嘗試重新連線嘗試，並在四次失敗嘗試之後停止。

開始進行任何重新連線嘗試之前，`HubConnection` 會轉換成 `HubConnectionState.Reconnecting` 狀態，並引發 `Reconnecting` 事件。  這會讓使用者有機會警告連接已遺失，並停用 UI 元素。 非互動式應用程式可以開始佇列或卸載訊息。

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

如果用戶端在前四次嘗試中成功重新連接，則 `HubConnection` 會轉換回 `Connected` 狀態並引發 `Reconnected` 事件。 這讓您有機會通知使用者，已重新建立連線，並清除佇列的任何訊息。

由於連接看起來就是伺服器的新連線，因此會提供新的 `ConnectionId` 給 `Reconnected` 的事件處理常式。

> [!WARNING]
> 如果 `HubConnection` 設定為[略過協商](xref:signalr/configuration#configure-client-options)，`Reconnected` 事件處理常式的 `connectionId` 參數將會是 null。

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` 不會將 `HubConnection` 設定為重試初始啟動失敗，因此必須手動處理啟動失敗：

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

如果用戶端在前四次嘗試中未成功重新連接，則 `HubConnection` 會轉換成 `Disconnected` 狀態，並引發 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> 事件。 這可讓您手動嘗試重新開機連線，或通知使用者已永久失去連接。

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

為了在中斷連線或變更重新連線時間之前，設定自訂的重新連線嘗試次數，`WithAutomaticReconnect` 接受數位陣列，代表在開始每次重新連線嘗試之前等待的延遲（以毫秒為單位）。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

上述範例會設定 `HubConnection`，以便在連接中斷之後立即開始嘗試重新連線。 這也適用于預設設定。

如果第一次重新連線嘗試失敗，第二次重新連線嘗試也會立即開始，而不是等待2秒，就像在預設設定中一樣。

如果第二次重新連線嘗試失敗，第三次重新連線嘗試會在10秒內啟動，這再次是預設設定。

自訂行為接著會在第三次重新連線嘗試失敗之後停止，再次從預設行為分歧。 在預設設定中，會在另一個30秒內重新連線嘗試一次。

如果您想要更充分掌控自動重新連線嘗試的時間和次數，`WithAutomaticReconnect` 會接受一個物件，該介面會執行 `IRetryPolicy` 介面，其具有名為 `NextRetryDelay`的單一方法。

`NextRetryDelay` 接受具有類型 `RetryContext`的單一引數。 `RetryContext` 有三個屬性： `PreviousRetryCount`、`ElapsedTime` 和 `RetryReason`，分別是 `long`、`TimeSpan` 和 `Exception`。 第一次重新連線嘗試之前，`PreviousRetryCount` 和 `ElapsedTime` 都是零，而 `RetryReason` 則是造成連接遺失的例外狀況。 在每次重試失敗後，`PreviousRetryCount` 將會遞增一次，`ElapsedTime` 將會更新以反映到目前為止的重新連接所花費的時間，而 `RetryReason` 將會是導致上次重新連線嘗試失敗的例外狀況。

`NextRetryDelay` 必須傳回代表在下一次重新連線嘗試之前等候時間的 TimeSpan，或 `null` 如果 `HubConnection` 應該停止重新連接，則為。

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
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

或者，您可以撰寫程式碼，以手動方式重新連線用戶端，如[手動重新](#manually-reconnect)連線中所示。

::: moniker-end

### <a name="manually-reconnect"></a>手動重新連線

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 在3.0 之前，SignalR 的 .NET 用戶端不會自動重新連線。 您必須撰寫程式碼，以手動方式重新連線用戶端。

::: moniker-end

使用 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> 事件來回應遺失的連接。 例如，您可能會想要自動重新連接。

`Closed` 事件需要傳回 `Task`的委派，這允許非同步程式碼在不使用 `async void`的情況下執行。 若要在同步執行的 `Closed` 事件處理常式中滿足委派簽章，請傳回 `Task.CompletedTask`：

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

非同步支援的主要原因是為了讓您可以重新開機連接。 啟動連接是非同步動作。

在重新開機連接的 `Closed` 處理常式中，請考慮等候一些隨機延遲，以避免伺服器超載，如下列範例所示：

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

`InvokeAsync` 會在中樞上呼叫方法。 傳遞中樞方法名稱和中樞方法中所定義的任何引數，以 `InvokeAsync`。 SignalR 是非同步，因此在進行呼叫時，請使用 `async` 和 `await`。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync` 方法會傳回在伺服器方法傳回時完成的 `Task`。 傳回值（如果有的話）會當做 `Task`的結果提供。 伺服器上方法所擲回的任何例外狀況都會產生錯誤的 `Task`。 請使用 `await` 語法來等候伺服器方法完成，並 `try...catch` 語法來處理錯誤。

`SendAsync` 方法會傳回在訊息傳送至伺服器時完成的 `Task`。 因為此 `Task` 不會等到伺服器方法完成，所以不會提供傳回值。 傳送訊息時，在用戶端擲回的任何例外狀況都會產生錯誤的 `Task`。 使用 `await` 和 `try...catch` 語法來處理傳送錯誤。

> [!NOTE]
> 如果您使用*無伺服器模式*的 Azure SignalR 服務，則無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱[SignalR 服務檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。

## <a name="call-client-methods-from-hub"></a>從中樞呼叫用戶端方法

定義中樞使用 `connection.On` 建立之後，但在開始連接之前所呼叫的方法。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

當伺服器端程式碼使用 `SendAsync` 方法呼叫它時，`connection.On` 中的上述程式碼就會執行。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

使用 try catch 語句來處理錯誤。 檢查 `Exception` 物件，以判斷錯誤發生後要採取的適當動作。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [Azure SignalR 服務無伺服器檔](/azure/azure-signalr/signalr-concept-serverless-development-config)
