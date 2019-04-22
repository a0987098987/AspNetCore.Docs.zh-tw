---
title: ASP.NET Core SignalR JavaScript 用戶端
author: bradygaster
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: e58015221497a9f962edf9f9fdba7ea3025d7694
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705600"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript 用戶端

作者：[Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>SignalR 用戶端封裝安裝

SignalR JavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)套件形式傳遞。 如果您使用 Visual Studio，可以在**Package Manager Console**的根資料夾中執行`npm install`。 如果您使用 Visual Studio Code，請從**整合式終端機**中執行命令。

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm 會將套件內容安裝在 *node_modules\\@aspnet\signalr\dist\browser* 資料夾中。 在 *wwwroot\\lib* 資料夾下，建立名為 *signalr* 的新資料夾。 將 *signalr.js* 檔案複製到 *wwwroot\lib\signalr* 資料夾中。

## <a name="use-the-signalr-javascript-client"></a>使用 SignalR JavaScript 用戶端

在 `<script>` 元素中參考 SignalR JavaScript 用戶端。

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>連線至中樞

下列程式碼會建立並啟動連線。 中樞的名稱不區分大小寫。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>跨原始來源的連線

一般而言，瀏覽器會從與所要求之頁面相同的網域載入連線。 不過，有些情況下會需要連線到另一個網域的連線。

若要防止惡意網站從另一個網站讀取敏感性資料，預設會停用[跨來源連線](xref:security/cors)。 若要允許跨來源要求，請在 `Startup` 類別中啟用它。

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

JavaScript 用戶端會透過 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection) [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) 方法呼叫中樞的公用方法。 `invoke` 方法接受兩個引數：

* 中樞方法的名稱。 在下列範例中，在中樞的方法名稱是`SendMessage`。
* 中樞的方法中定義的任何引數。 在下列範例中，引數名稱是`message`。 範例程式碼會使用目前版本的 Internet Explorer 以外的所有主要瀏覽器都支援的箭號函式語法。

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> 如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

若要從中樞接收訊息，請使用 `HubConnection` 的 [on](/javascript/api/%40aspnet/signalr/hubconnection#on) 方法來定義方法。

* JavaScript 用戶端方法的名稱。 下列範例中，在方法名稱是`ReceiveMessage`。
* 中樞會將傳遞給方法的引數。 在下列範例中，引數值是`message`。

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

當伺服器端程式碼使用 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 方法來呼叫它時，會執行 `connection.on` 中的上述程式碼。

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR 透過比對 `SendAsync` 與 `connection.on` 中定義的方法名稱與引數，來判斷要呼叫哪個用戶端方法。

> [!NOTE]
> 最佳做法是，在 `on` 之後呼叫 `HubConnection` 的 [start](/javascript/api/%40aspnet/signalr/hubconnection#start) 方法。 這麼做可確保您的處理常式會在註冊之後，才會收到訊息。

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

在 `start` 方法的結尾鏈結 `catch` 方法以處理用戶端錯誤。 您可以使用 `console.error` 輸出錯誤至瀏覽器的主控台。

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。 使用指定的記錄層級和更新版本，則會記錄訊息。 可用的記錄層級如下所示：

* `signalR.LogLevel.Error` &ndash; 錯誤訊息。 記錄檔`Error`只有訊息。
* `signalR.LogLevel.Warning` &ndash; 可能的錯誤的相關警告訊息。 會記錄 `Warning` 和 `Error` 訊息。
* `signalR.LogLevel.Information` &ndash; 沒有錯誤的狀態訊息。 會記錄 `Information`、`Warning` 和 `Error` 訊息。
* `signalR.LogLevel.Trace` &ndash; 追蹤訊息。 記錄所有事件，包括中樞和用戶端之間傳輸的資料。

使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)設定記錄層級。 訊息會記錄到瀏覽器主控台。

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>重新連線用戶端

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>自動重新連線

SignalR 的 JavaScript 用戶端可以設定為使用自動重新`withAutomaticReconnect`方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)。 它不會依預設會自動重新連線。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

如果沒有任何參數，`withAutomaticReconnect()`設定用戶端一直等候 0、 2、 10 和 30 秒，分別是然後再嘗試每次重新連接嘗試，四個失敗的嘗試之後停止。

開始任何重新連接嘗試之前`HubConnection`會轉為`HubConnectionState.Reconnecting`狀態，並引發其`onreconnecting`回呼，而不是轉換為`Disconnected`狀態，並觸發其`onclose`回呼喜歡`HubConnection`未自動重新連線設定。 這便有機會時警告使用者的連線已遺失，並在停用 UI 項目。

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

如果用戶端順利重新連線內的前四個的試`HubConnection`就會轉換回`Connected`狀態，並引發其`onreconnected`回呼。 這便有機會以通知的使用者已重新建立連線。

連線看起來完全新增到伺服器，因為新`connectionId`將提供給`onreconnected`回呼。

> [!WARNING]
> `onreconnected`回呼的`connectionId`參數將會是未定義，如果`HubConnection`已設定為[略過交涉](xref:signalr/configuration#configure-client-options)。

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` 不會設定`HubConnection`重試初始啟動時失敗，因此需要手動處理啟動失敗：

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

如果用戶端不會成功地重新連接內的前四個的試`HubConnection`會轉為`Disconnected`狀態，並引發其[onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose)回呼。 這讓您有機會通知的使用者此連接已經永久遺失，並建議重新整理頁面：

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

若要設定自訂的數字的中斷連線之前重新連接嘗試，或變更重新連線延遲時間，`withAutomaticReconnect`接受代表數字以毫秒為單位的延遲開始每次重新連接嘗試之前等候的陣列。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

上述範例會設定`HubConnection`啟動連線將會中斷之後，立即嘗試重新連線。 這也是預設組態，則為 true。

如果第一次重新連接嘗試失敗，第二次重新連接嘗試將也會立即開始而不是等到 2 秒，像是在預設組態中。

如果第二次重新連接嘗試失敗，第三次重新連接嘗試就會啟動在 10 秒，這又是預設組態類似。

自訂行為然後分離一次的預設行為藉由停止之後第三個重新連接嘗試失敗，而不是嘗試其中一個更重新連接嘗試中像是在預設組態中的另一個 30 秒。

如果您想要更進一步控制時間和數字的自動重新連線嘗試`withAutomaticReconnect`物件，實作會接受`IReconnectPolicy`介面，其具有單一方法，名為`nextRetryDelayInMilliseconds`。

`nextRetryDelayInMilliseconds` 接受兩個引數，`previousRetryCount`和`elapsedMilliseconds`，這是兩個數字。 第一次重新連接嘗試中前, 兩者`previousRetryCount`和`elapsedMilliseconds`會是零。 在每次失敗的重試之後,`previousRetryCount`會遞增 1 和`elapsedMilliseconds`將會更新以反映所花費的時間重新連線到目前為止，以毫秒為單位。

`nextRetryDelayInMilliseconds` 必須傳回任一數字表示的毫秒數下一次重新連接嘗試之前要等待或`null`如果`HubConnection`應該停止重新連線。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
          if (elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

或者，您可以在其中撰寫程式碼所示的手動將重新連線您的用戶端[以手動方式重新連線](#manually-reconnect)。

::: moniker-end

### <a name="manually-reconnect"></a>以手動方式重新連線

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 在 3.0 之前，SignalR 的 JavaScript 用戶端不會自動重新連線。 您必須撰寫程式碼會以手動方式重新連接您的用戶端。

::: moniker-end

下列程式碼示範一個典型的手動重新連線的方法：

1. 函式 (在此情況下，`start`函式) 建立以啟動的連線。
1. 呼叫`start`函式中的連線`onclose`事件處理常式。

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

真實世界實作會使用指數退避法，或指定的次數後放棄重試一次。 

## <a name="additional-resources"></a>其他資源

* [JavaScript API 參考](/javascript/api/?view=signalr-js-latest)
* [JavaScript 教學課程](xref:tutorials/signalr)
* [WebPack 和 TypeScript 的教學課程](xref:tutorials/signalr-typescript-webpack)
* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [跨原始來源要求 (CORS)](xref:security/cors)
* [Azure SignalR 服務的無伺服器文件](/azure/azure-signalr/signalr-concept-serverless-development-config)
