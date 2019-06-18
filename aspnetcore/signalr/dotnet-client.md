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
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR.NET 用戶端

ASP.NET Core SignalR.NET 用戶端程式庫可讓您與 SignalR 中樞從.NET 應用程式進行通訊。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

這篇文章中的程式碼範例是使用 ASP.NET Core SignalR.NET 用戶端的 WPF 應用程式。

## <a name="install-the-signalr-net-client-package"></a>安裝 SignalR.NET 用戶端套件

`Microsoft.AspNetCore.SignalR.Client`封裝所需的.NET 用戶端連線到 SignalR 中樞。 若要安裝用戶端程式庫，請執行下列命令**Package Manager Console**視窗：

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>連線至中樞

若要建立連線，建立`HubConnectionBuilder`並呼叫`Build`。 中樞 URL、 通訊協定、 傳輸類型、 記錄層級、 標頭，以及其他選項可以在建立連接時設定。 設定任何所需的選項，插入的任何`HubConnectionBuilder`方法`Build`。 啟動與連線`StartAsync`。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>中斷連線的控制代碼

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>自動重新連線

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection>可以設定為自動重新連線使用`WithAutomaticReconnect`方法<xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>。 它不會依預設會自動重新連線。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

如果沒有任何參數，`WithAutomaticReconnect()`設定用戶端一直等候 0、 2、 10 和 30 秒，分別是然後再嘗試每次重新連接嘗試，四個失敗的嘗試之後停止。

開始任何重新連接嘗試之前`HubConnection`會轉為`HubConnectionState.Reconnecting`狀態，並引發`Reconnecting`事件。  這便有機會時警告使用者的連線已遺失，並在停用 UI 項目。 非互動式應用程式可以啟動佇列，或卸除訊息。

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

如果用戶端順利重新連線內的前四個的試`HubConnection`就會轉換回`Connected`狀態，並引發`Reconnected`事件。 這便有機會以通知的使用者已重新建立連線，以及從佇列中清除任何已排入佇列的訊息。

連線看起來完全新增到伺服器，因為新`ConnectionId`將提供給`Reconnected`事件處理常式。

> [!WARNING]
> `Reconnected`事件處理常式`connectionId`參數將為 null 如果`HubConnection`已設定為[略過交涉](xref:signalr/configuration#configure-client-options)。

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` 不會設定`HubConnection`重試初始啟動時失敗，因此需要手動處理啟動失敗：

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

如果用戶端不會成功地重新連接內的前四個的試`HubConnection`會轉為`Disconnected`狀態，並引發<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件。 這便有機會嘗試以手動方式重新連線，或通知使用者此連接已經永久遺失。

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

若要設定自訂的數字的中斷連線之前重新連接嘗試，或變更重新連線延遲時間，`WithAutomaticReconnect`接受代表數字以毫秒為單位的延遲開始每次重新連接嘗試之前等候的陣列。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

上述範例會設定`HubConnection`啟動連線將會中斷之後，立即嘗試重新連線。 這也是預設組態，則為 true。

如果第一次重新連接嘗試失敗，第二次重新連接嘗試將也會立即開始而不是等到 2 秒，像是在預設組態中。

如果第二次重新連接嘗試失敗，第三次重新連接嘗試就會啟動在 10 秒，這又是預設組態類似。

自訂行為然後分離一次的預設行為的第三個重新連接嘗試失敗之後停止。 在預設組態會是一個更重新連接嘗試在另一個 30 秒內。

如果您想要更進一步控制時間和數字的自動重新連線嘗試`WithAutomaticReconnect`物件，實作會接受`IRetryPolicy`介面，其具有單一方法，名為`NextRetryDelay`。

`NextRetryDelay` 接受單一引數與類型`RetryContext`。 `RetryContext`有三個屬性： `PreviousRetryCount`，`ElapsedTime`並`RetryReason`哪些是`long`，則`TimeSpan`和`Exception`分別。 第一次重新連接嘗試中前, 兩者`PreviousRetryCount`和`ElapsedTime`將會是零，和`RetryReason`會導致連接遺失的例外狀況。 每個失敗的重試嘗試之後， `PreviousRetryCount` ，將會遞增`ElapsedTime`將會更新以反映所花費的時間重新連線到目前為止，和`RetryReason`會導致上次重新連接嘗試失敗的例外狀況。

`NextRetryDelay` 必須傳回可能是 TimeSpan，代表時間等待下一次重新連接嘗試或`null`如果`HubConnection`應該停止重新連線。

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

或者，您可以在其中撰寫程式碼所示的手動將重新連線您的用戶端[以手動方式重新連線](#manually-reconnect)。

::: moniker-end

### <a name="manually-reconnect"></a>以手動方式重新連線

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 在 3.0 之前，SignalR 的.NET 用戶端不會自動重新連線。 您必須撰寫程式碼會以手動方式重新連接您的用戶端。

::: moniker-end

使用<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>事件回應失去連線。 例如，您可能要自動重新連線。

`Closed`事件要求委派，會傳回`Task`，可讓非同步程式碼執行，而不使用`async void`。 為了滿足中的委派簽章`Closed`執行以同步方式傳回的事件處理常式`Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

非同步支援的主要原因是，因此您可以重新連線。 正在開始連線是非同步動作。

在 `Closed`連線，會重新啟動的處理常式考慮一些隨機的延遲，避免多載在伺服器上，等待，如下列範例所示：

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

`InvokeAsync` 集線器上呼叫方法。 將中樞方法的名稱和任何定義於中樞方法的引數傳遞`InvokeAsync`。 SignalR 是非同步，因此請使用`async`和`await`時進行呼叫。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync`方法會傳回`Task`伺服器方法傳回時，其完成。 傳回值，如果有的話，可作為結果`Task`。 在伺服器上的方法所擲回任何例外狀況會產生錯誤`Task`。 使用`await`等待完成要求伺服器方法的語法和`try...catch`語法來處理錯誤。

`SendAsync`方法會傳回`Task`其完成，當訊息已傳送到伺服器。 未提供任何傳回的值，因為這`Task`不等候，直到伺服器在方法完成。 用戶端傳送訊息時擲回任何例外狀況會產生錯誤`Task`。 使用`await`和`try...catch`語法來處理將錯誤傳送。

> [!NOTE]
> 如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

定義的方法使用呼叫中樞`connection.On`之後建置，但之前啟動連線。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

在上述程式碼`connection.On`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

處理錯誤的 try / catch 陳述式。 檢查`Exception`物件，以判斷發生錯誤之後所採取的適當動作。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [Azure SignalR 服務的無伺服器文件](/azure/azure-signalr/signalr-concept-serverless-development-config)
