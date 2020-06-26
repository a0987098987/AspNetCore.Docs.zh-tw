---
title: ASP.NET Core SignalR JavaScript 用戶端
author: bradygaster
description: ASP.NET Core SignalR JavaScript 用戶端的總覽。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: 8c7acad42f3a49ccf1bc60f8ae5b4f68a602d97b
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406923"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript 用戶端

作者：[Rachel Appel](https://twitter.com/rachelappel)

ASP.NET Core 的 SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞程式碼。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="install-the-signalr-client-package"></a>安裝 SignalR 用戶端套件

SignalRJavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)封裝的形式提供。 下列各節概述安裝用戶端程式庫的不同方式。

### <a name="install-with-npm"></a>使用 npm 安裝

如果使用 Visual Studio，請在根資料夾中，從 [**套件管理員主控台**] 執行下列命令。 針對 Visual Studio Code，請從**整合式終端**機執行下列命令。

::: moniker range=">= aspnetcore-3.0"

```bash
npm init -y
npm install @microsoft/signalr
```

npm 會在*node_modules \\ @microsoft\signalr\dist\browser *資料夾中安裝封裝內容。 在*wwwroot \\ lib*資料夾底下，建立名為*signalr*的新資料夾。 將*signalr.js*檔案複製到*wwwroot\lib\signalr*資料夾。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```bash
npm init -y
npm install @aspnet/signalr
```

npm 會在*node_modules \\ @aspnet\signalr\dist\browser *資料夾中安裝封裝內容。 在*wwwroot \\ lib*資料夾底下，建立名為*signalr*的新資料夾。 將*signalr.js*檔案複製到*wwwroot\lib\signalr*資料夾。

::: moniker-end

參考 SignalR 元素中的 JavaScript 用戶端 `<script>` 。 例如：

```html
<script src="~/lib/signalr/signalr.js"></script>
```

### <a name="use-a-content-delivery-network-cdn"></a>使用內容傳遞網路（CDN）

若要使用不含 npm 必要條件的用戶端程式庫，請參考 CDN 主控的用戶端程式庫複本。 例如：

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/3.1.3/signalr.min.js"></script>
```

用戶端程式庫可在下列 Cdn 中取得：

::: moniker range=">= aspnetcore-3.0"

* [cdnjs](https://cdnjs.com/libraries/microsoft-signalr)
* [jsDelivr](https://www.jsdelivr.com/package/npm/@microsoft/signalr)
* [unpkg](https://unpkg.com/@microsoft/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [cdnjs](https://cdnjs.com/libraries/aspnet-signalr)
* [jsDelivr](https://www.jsdelivr.com/package/npm/@aspnet/signalr)
* [unpkg](https://unpkg.com/@aspnet/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

### <a name="install-with-libman"></a>使用 LibMan 安裝

[LibMan](xref:client-side/libman/index)可以用來從 CDN 裝載的用戶端程式庫安裝特定的用戶端程式庫檔案。 例如，只將縮減 JavaScript 檔案新增至專案。 如需該方法的詳細資訊，請參閱[新增 SignalR 用戶端程式庫](xref:tutorials/signalr#add-the-signalr-client-library)。

## <a name="connect-to-a-hub"></a>連接到中樞

下列程式碼會建立並啟動連接。 中樞的名稱不區分大小寫。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,28-51)]

### <a name="cross-origin-connections"></a>跨原始來源連接

通常，瀏覽器會載入與所要求頁面來自相同網域的連接。 不過，在某些情況下需要連接到另一個網域。

為了防止惡意網站從另一個網站讀取敏感性資料，預設會停用[跨原始來源](xref:security/cors)連線。 若要允許跨原始來源要求，請在類別中加以啟用 `Startup` 。

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

JavaScript 用戶端會透過[HubConnection](/javascript/api/%40aspnet/signalr/hubconnection)的[invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke)方法，在中樞上呼叫公用方法。 `invoke`方法接受兩個引數：

* 中樞方法的名稱。 在下列範例中，中樞上的方法名稱是 `SendMessage` 。
* 中樞方法中定義的任何引數。 在下列範例中，引數名稱是 `message` 。 範例程式碼使用箭號函式語法，在 Internet Explorer 以外的所有主要瀏覽器的目前版本中都受到支援。

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> 如果您是 SignalR 在*無伺服器模式*中使用 Azure 服務，則無法從用戶端呼叫中樞方法。 如需詳細資訊，請參閱[ SignalR 服務檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。

方法會傳回 `invoke` JavaScript[承諾](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)。 `Promise`當伺服器上的方法傳回時，會使用傳回值（如果有的話）來解析。 如果伺服器上的方法擲回錯誤，則 `Promise` 會拒絕並顯示錯誤訊息。 `then` `catch` 在本身上使用和方法 `Promise` 來處理這些案例（或 `await` 語法）。

方法會傳回 `send` JavaScript `Promise` 。 `Promise`當訊息已傳送到伺服器時，就會解決。 如果傳送訊息時發生錯誤，則 `Promise` 會拒絕，並顯示錯誤訊息。 `then` `catch` 在本身上使用和方法 `Promise` 來處理這些案例（或 `await` 語法）。

> [!NOTE]
> 使用 `send` 並不會等到伺服器收到訊息為止。 因此，不可能從伺服器傳回資料或錯誤。

## <a name="call-client-methods-from-hub"></a>從中樞呼叫用戶端方法

若要從中樞接收訊息，請使用的[on](/javascript/api/%40aspnet/signalr/hubconnection#on)方法來定義方法 `HubConnection` 。

* JavaScript 用戶端方法的名稱。 在下列範例中，方法名稱是 `ReceiveMessage` 。
* 中樞傳遞至方法的引數。 在下列範例中，引數值為 `message` 。

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

`connection.on`當伺服器端程式碼使用[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)方法呼叫它時，中的上述程式碼就會執行。

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR藉由比對和中定義的方法名稱和引數，來判斷要呼叫的用戶端方法 `SendAsync` `connection.on` 。

> [!NOTE]
> 最佳做法是在之後呼叫[start](/javascript/api/%40aspnet/signalr/hubconnection#start)方法 `HubConnection` `on` 。 這麼做可確保您的處理常式會在收到任何訊息之前註冊。

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

將 `catch` 方法連結至方法的結尾 `start` ，以處理用戶端錯誤。 使用將 `console.error` 錯誤輸出至瀏覽器的主控台。

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=50)]

設定用戶端記錄追蹤，方法是在建立連接時傳遞記錄器和事件種類來記錄。 訊息會記錄到指定的記錄層級和更新版本。 可用的記錄層級如下：

* `signalR.LogLevel.Error`：錯誤訊息。 `Error`僅記錄訊息。
* `signalR.LogLevel.Warning`：有關潛在錯誤的警告訊息。 記錄檔 `Warning` 和 `Error` 訊息。
* `signalR.LogLevel.Information`：沒有錯誤的狀態訊息。 記錄 `Information` 、 `Warning` 和 `Error` 訊息。
* `signalR.LogLevel.Trace`：追蹤訊息。 記錄所有專案，包括中樞和用戶端之間傳輸的資料。

在[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法來設定記錄層級。 訊息會記錄到瀏覽器主控台。

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>重新連線用戶端

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>自動重新連線

的 JavaScript 用戶端 SignalR 可以設定為使用 `withAutomaticReconnect` [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上的方法自動重新連線。 根據預設，它不會自動重新連線。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withAutomaticReconnect()
    .build();
```

如果沒有任何參數， `withAutomaticReconnect()` 則會先將用戶端設定為等待0、2、10和30秒，再嘗試重新連線嘗試，並在四次失敗嘗試之後停止。

開始任何重新連線嘗試之前， `HubConnection` 會轉換成 `HubConnectionState.Reconnecting` 狀態並引發其 `onreconnecting` 回呼，而不是轉換成 `Disconnected` 狀態並觸發其 `onclose` 回呼，例如 `HubConnection` 未設定自動重新連線。 這會讓使用者有機會警告連接已遺失，並停用 UI 元素。

```javascript
connection.onreconnecting(error => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

如果用戶端在前四次嘗試中成功重新連接， `HubConnection` 將會轉換回 `Connected` 狀態並引發其 `onreconnected` 回呼。 這讓您有機會通知使用者已重新建立連接。

由於連接看起來就是伺服器的全新連線，因此 `connectionId` 會提供新的給 `onreconnected` 回呼。

> [!WARNING]
> `onreconnected` `connectionId` 如果 `HubConnection` 設定為[略過協商](xref:signalr/configuration#configure-client-options)，回呼的參數將會是未定義的。

```javascript
connection.onreconnected(connectionId => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()`不會將設定 `HubConnection` 為重試初始啟動失敗，因此必須手動處理啟動失敗：

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

如果用戶端在前四次嘗試中未成功重新連接， `HubConnection` 將會轉換成 `Disconnected` 狀態並引發其[onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose)回呼。 這讓您有機會通知使用者連線已永久遺失，並建議重新整理頁面：

```javascript
connection.onclose(error => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

為了在中斷連線或變更重新連線時間之前設定自訂的重新連線嘗試次數，會 `withAutomaticReconnect` 接受代表在開始每次重新連線嘗試之前等待的延遲（以毫秒為單位）的數位陣列。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

上述範例會將設定 `HubConnection` 為，以便在連接中斷之後立即開始嘗試重新連接。 這也適用于預設設定。

如果第一次重新連線嘗試失敗，第二次重新連線嘗試也會立即開始，而不是等待2秒，就像在預設設定中一樣。

如果第二次重新連線嘗試失敗，第三次重新連線嘗試會在10秒內啟動，這再次是預設設定。

然後，自訂行為會在第三次重新連線嘗試失敗之後停止，而不是在另一個30秒嘗試重新連線嘗試，就會再次從預設行為分歧，而不是在預設設定中再試一次。

如果您想要更充分掌控自動重新連線嘗試的時間和數目，則會 `withAutomaticReconnect` 接受一個物件 `IRetryPolicy` 來執行介面，此介面具有名為的單一方法 `nextRetryDelayInMilliseconds` 。

`nextRetryDelayInMilliseconds`接受具有類型的單一引數 `RetryContext` 。 `RetryContext`有三個屬性： `previousRetryCount` 、 `elapsedMilliseconds` 和 `retryReason` `number` 分別為、 `number` 和 `Error` 。 第一次重新連線嘗試之前， `previousRetryCount` 和都 `elapsedMilliseconds` 是零，而且 `retryReason` 會是造成連接遺失的錯誤。 每次重試失敗之後，將會 `previousRetryCount` 遞增一次， `elapsedMilliseconds` 將會更新，以反映到目前為止的重新連線時間量（以毫秒為單位），而 `retryReason` 會是導致上次重新連線嘗試失敗的錯誤。

`nextRetryDelayInMilliseconds`必須傳回數位，代表下一次重新連接嘗試之前要等候的毫秒數，或者 `null` ，如果應該停止重新連接，則為 `HubConnection` 。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
            if (retryContext.elapsedMilliseconds < 60000) {
                // If we've been reconnecting for less than 60 seconds so far,
                // wait between 0 and 10 seconds before the next reconnect attempt.
                return Math.random() * 10000;
            } else {
                // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
                return null;
            }
        }
    })
    .build();
```

或者，您可以撰寫程式碼，以手動方式重新連線用戶端，如[手動重新](#manually-reconnect)連線中所示。

::: moniker-end

### <a name="manually-reconnect"></a>手動重新連線

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 在3.0 之前，的 JavaScript 用戶端 SignalR 不會自動重新連線。 您必須撰寫程式碼，以手動方式重新連線用戶端。

::: moniker-end

下列程式碼示範一般手動重新連接方法：

1. 建立函式（在此案例中為函式 `start` ）來啟動連接。
1. 呼叫 `start` 連接的事件處理常式中的函式 `onclose` 。

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

實際執行會使用指數退避，或在放棄之前重試指定的次數。

## <a name="additional-resources"></a>其他資源

* [JavaScript API 參考](/javascript/api/?view=signalr-js-latest)
* [JavaScript 教學課程](xref:tutorials/signalr)
* [WebPack 和 TypeScript 教學課程](xref:tutorials/signalr-typescript-webpack)
* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [跨原始來源要求（CORS）](xref:security/cors)
* [Azure SignalR 服務無伺服器檔](/azure/azure-signalr/signalr-concept-serverless-development-config)
