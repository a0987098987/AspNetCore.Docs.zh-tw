---
title: ASP.NET Core SignalR .NET 用戶端
author: bradygaster
description: ASP.NET Core SignalR .NET 用戶端的相關資訊
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/13/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 4419799ef11469413f813843a9d02ac0223d30c6
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081286"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET 用戶端

ASP.NET Core SignalR .NET 用戶端程式庫可讓您從 .NET 應用程式與 SignalR 中樞進行通訊。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

本文中的程式碼範例是使用 ASP.NET Core SignalR .NET 用戶端的 WPF 應用程式。

## <a name="install-the-signalr-net-client-package"></a>安裝 SignalR .NET 用戶端封裝

.NET 用戶端必須要有[AspNetCore. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)封裝，才能連接到 SignalR hub。

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

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立連接，請`HubConnectionBuilder`建立，並呼叫。 `Build` 建立連接時，可以設定中樞 URL、通訊協定、傳輸類型、記錄層級、標頭和其他選項。 將任何`HubConnectionBuilder` `Build`方法插入，以設定任何必要的選項。 開始與`StartAsync`的連接。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>處理遺失的連接

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>自動重新連線

可以設定為使用上<xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>的`WithAutomaticReconnect`方法自動重新連接。 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> 根據預設, 它不會自動重新連線。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

如果沒有任何參數`WithAutomaticReconnect()` , 則會先將用戶端設定為等待0、2、10和30秒, 再嘗試重新連線嘗試, 並在四次失敗嘗試之後停止。

開始任何重新連線嘗試之前， `HubConnection`會轉換`HubConnectionState.Reconnecting`成狀態並引發`Reconnecting`事件。  這會讓使用者有機會警告連接已遺失, 並停用 UI 元素。 非互動式應用程式可以開始佇列或卸載訊息。

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

如果用戶端在前四次嘗試中成功重新連接`HubConnection` ，將會轉換回`Connected`狀態並引發`Reconnected`事件。 這讓您有機會通知使用者，已重新建立連線，並清除佇列的任何訊息。

由於連接看起來就是伺服器的新連線，因此`ConnectionId`會提供`Reconnected`新的給事件處理常式。

> [!WARNING]
> 如果已設定為`connectionId` [略過協商](xref:signalr/configuration#configure-client-options)，事件處理常式的參數將會是null。`Reconnected` `HubConnection`

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()`不會將`HubConnection`設定為重試初始啟動失敗, 因此必須手動處理啟動失敗:

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

如果用戶端在前四次嘗試中未成功重新連接`HubConnection` ，將會轉換`Disconnected`成狀態並引發<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件。 這可讓您手動嘗試重新開機連線，或通知使用者已永久失去連接。

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

為了在中斷連線或變更重新連線時間之前設定自訂的重新連線嘗試次數`WithAutomaticReconnect` , 會接受代表在開始每次重新連線嘗試之前等待的延遲 (以毫秒為單位) 的數位陣列。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

上述範例`HubConnection`會將設定為, 以便在連接中斷之後立即開始嘗試重新連接。 這也適用于預設設定。

如果第一次重新連線嘗試失敗, 第二次重新連線嘗試也會立即開始, 而不是等待2秒, 就像在預設設定中一樣。

如果第二次重新連線嘗試失敗, 第三次重新連線嘗試會在10秒內啟動, 這再次是預設設定。

自訂行為接著會在第三次重新連線嘗試失敗之後停止，再次從預設行為分歧。 在預設設定中，會在另一個30秒內重新連線嘗試一次。

如果您想要更充分掌控自動重新連線嘗試的時間和數目, `WithAutomaticReconnect`則會接受一個物件`IRetryPolicy`來執行介面, 此介面具有名`NextRetryDelay`為的單一方法。

`NextRetryDelay`接受具有類型`RetryContext`的單一引數。 `PreviousRetryCount` `RetryReason` 有三`Exception`個屬性: `ElapsedTime`、和分別為、`TimeSpan`和。 `long` `RetryContext` 第一次重新連線嘗試之前`PreviousRetryCount` ， `ElapsedTime`和都`RetryReason`是零，而且會是造成連接遺失的例外狀況。 每次重試`PreviousRetryCount`失敗之後，將會遞增一次， `ElapsedTime`將會更新以反映到目前為止的重新連接所花費的時間`RetryReason` ，而且將會是導致上次重新連線嘗試失敗的例外狀況。

`NextRetryDelay`必須傳回代表下次重新連線嘗試之前等候時間的 TimeSpan，或`null`如果應該停止重新連接， `HubConnection`則為。

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

或者, 您可以撰寫程式碼, 以手動方式重新連線用戶端, 如[手動重新](#manually-reconnect)連線中所示。

::: moniker-end

### <a name="manually-reconnect"></a>手動重新連線

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 在3.0 之前，SignalR 的 .NET 用戶端不會自動重新連線。 您必須撰寫程式碼會以手動方式重新連接您的用戶端。

::: moniker-end

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>使用事件來回應遺失的連接。 例如，您可能會想要自動重新連接。

事件需要傳回的`Task`委派，這可讓非同步程式碼在不使用`async void`的情況下執行。 `Closed` 若要在同步執行的`Closed`事件處理常式中滿足委派簽章`Task.CompletedTask`，請傳回：

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

非同步支援的主要原因是為了讓您可以重新開機連接。 啟動連接是非同步動作。

在重新開機連接的處理常式中，請考慮等候一些隨機延遲，以避免伺服器超載，如下列範例所示：`Closed`

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

`InvokeAsync`在中樞上呼叫方法。 將中樞方法名稱和 hub 方法中定義的任何引數傳遞`InvokeAsync`至。 SignalR 是非同步，因此`async`在`await`進行呼叫時，請使用和。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync` 方法`Task`會傳回在伺服器方法傳回時完成的。 傳回值（如果有的話）會當做的結果`Task`提供。 伺服器上方法所擲回的任何例外狀況都會產生`Task`錯誤。 請`await`使用語法來等候伺服器方法完成，並`try...catch`使用語法來處理錯誤。

`SendAsync` 方法`Task`會傳回在訊息傳送至伺服器時完成的。 因為這`Task`不會等到伺服器方法完成，所以不會提供傳回值。 傳送訊息時，在用戶端擲回的任何例外狀況`Task`都會產生錯誤。 使用`await` 和`try...catch`語法來處理傳送錯誤。

> [!NOTE]
> 如果您使用*無伺服器模式*中的 Azure SignalR Service, 就無法從用戶端呼叫中樞方法。 如需詳細資訊, 請參閱[SignalR Service 檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

定義中樞在建立`connection.On`之後，但在開始連接之前所呼叫的方法。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

當伺服器端程式`connection.On`代碼`SendAsync`使用方法呼叫它時，中的上述程式碼就會執行。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

使用 try catch 語句來處理錯誤。 `Exception`檢查物件，以判斷錯誤發生後要採取的適當動作。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [Azure SignalR Service 無伺服器檔](/azure/azure-signalr/signalr-concept-serverless-development-config)
