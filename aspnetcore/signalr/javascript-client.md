---
title: ASP.NET核心SignalRJavaScript 用戶端
author: bradygaster
description: ASP.NET核心SignalRJavaScript用戶端概述。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: a99c1dd2aba6ef6ff925783762a98e2c81ed7225
ms.sourcegitcommit: 9a46e78c79d167e5fa0cddf89c1ef584e5fe1779
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994576"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a>ASP.NET核心SignalRJavaScript 用戶端

作者：[Rachel Appel](https://twitter.com/rachelappel)

ASP.NET核心SignalRJavaScript 用戶端庫使開發人員能夠調用伺服器端集線器代碼。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample)([如何下載](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-client-package"></a>安裝SignalR用戶端套件

JavaScriptSignalR用戶端庫作為[npm](https://www.npmjs.com/)包提供。 以下各節概述了安裝用戶端庫的不同方法。

### <a name="install-with-npm"></a>使用 npm 安裝

如果使用 Visual Studio,則在根資料夾中運行**包管理器主控台**中的以下命令。 對於可視化工作室代碼,請從**整合終端**運行以下命令。

::: moniker range=">= aspnetcore-3.0"

```bash
npm init -y
npm install @microsoft/signalr
```

npm 在*node_modules\\*資料夾中安裝包內容。 在*wwwroot\\lib*資料夾下創建名為*信號器*的新資料夾。 將*Signalr.js*檔案複製到*wwwroot_lib_signalr*資料夾。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```bash
npm init -y
npm install @aspnet/signalr
```

npm 在*node_modules\\*資料夾中安裝包內容。 在*wwwroot\\lib*資料夾下創建名為*信號器*的新資料夾。 將*Signalr.js*檔案複製到*wwwroot_lib_signalr*資料夾。

::: moniker-end

引用`<script>`SignalR元素 中的 JavaScript 用戶端。 例如：

```html
<script src="~/lib/signalr/signalr.js"></script>
```

### <a name="use-a-content-delivery-network-cdn"></a>使用內容提供網路 (CDN)

要使用沒有 npm 先決條件的用戶端庫,請引用用戶端庫的 CDN 託管副本。 例如：

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/3.1.3/signalr.min.js"></script>
```

用戶端函式庫在以下 CDN 上可用:

::: moniker range=">= aspnetcore-3.0"

* [恩傑斯](https://cdnjs.com/libraries/microsoft-signalr)
* [jsDelivr](https://www.jsdelivr.com/package/npm/@microsoft/signalr)
* [unpkg](https://unpkg.com/@microsoft/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [恩傑斯](https://cdnjs.com/libraries/aspnet-signalr)
* [jsDelivr](https://www.jsdelivr.com/package/npm/@aspnet/signalr)
* [unpkg](https://unpkg.com/@aspnet/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

### <a name="install-with-libman"></a>使用 LibMan 安裝

[LibMan](xref:client-side/libman/index)可用於從 CDN 託管的用戶端庫安裝特定的用戶端庫檔。 例如,僅將已小的 JavaScript 檔添加到專案中。 有關該方法的詳細資訊,請參閱[SignalR添加用戶端庫](xref:tutorials/signalr#add-the-signalr-client-library)。

## <a name="connect-to-a-hub"></a>連接到集線器

以下代碼創建並啟動連接。 中心的名稱不區分大小寫。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>交叉來源連線

通常,瀏覽器將連接與請求的頁面從同一域載入。 但是,有時需要連接到另一個域。

為了防止惡意網站從其他網站讀取敏感資料,預設情況下將禁用[跨源連接](xref:security/cors)。 要允許跨源請求,請在類中`Startup`啟用它。

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中心方法

JavaScript客戶端透過[HubConnect](/javascript/api/%40aspnet/signalr/hubconnection)的[呼叫](/javascript/api/%40aspnet/signalr/hubconnection#invoke)方法在集線器上調用公共方法。 該方法`invoke`接受兩個參數:

* 中心方法的名稱。 在下面的範例中,中心上的方法名稱為`SendMessage`。
* 在中心方法中定義的任何參數。 在下面的範例中,參數名稱稱為`message`。 範例代碼使用除 Internet Explorer 以外的所有主要瀏覽器的當前版本中支援的箭頭函數語法。

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> 如果在*無伺服器模式下*SignalR使用 Azure 服務,則無法從用戶端呼叫集線器方法。 有關詳細資訊,請參閱[SignalR服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。

此方法`invoke`傳回 JavaScript[承諾](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)。 當`Promise`伺服器上的方法傳回時,使用傳回值(如果有)解析 。 如果伺服器上的方法引發錯誤,`Promise`則 拒絕使用錯誤訊息。 使用`then`本身`catch`上的`Promise`和方法來處理這些情況(`await`或語法)。

該方法`send`返回JAVAScript。 `Promise` 將`Promise`解解訊息發送到伺服器。 如果傳送訊息有錯誤`Promise`, 則拒絕使用錯誤訊息。 使用`then`本身`catch`上的`Promise`和方法來處理這些情況(`await`或語法)。

> [!NOTE]
> 使用`send`不會等到伺服器收到消息。 因此,無法從伺服器返回數據或錯誤。

## <a name="call-client-methods-from-hub"></a>從中心呼叫用戶端方法

要從集線器接收消息,請使用的[on](/javascript/api/%40aspnet/signalr/hubconnection#on)`HubConnection`方法定義方法。

* JavaScript 用戶端方法的名稱。 在下面的範例中,方法名稱稱為`ReceiveMessage`。
* 中心傳遞給方法的參數。 在下面的範例中,參數值為`message`。

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

當伺服器端代碼`connection.on`使用[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)方法調用它時,前面的代碼將運行。

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR以符合與中`SendAsync`定義的名稱與參數來決定要呼叫的用戶端方法`connection.on`。

> [!NOTE]
> 最佳做法是調用`HubConnection``on`後 上的[開始](/javascript/api/%40aspnet/signalr/hubconnection#start)方法。 這樣做可確保在收到任何消息之前註冊處理程式。

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

將`catch`方法連結到方法的末尾,`start`以處理用戶端錯誤。 用於`console.error`將錯誤輸出到瀏覽器的主控台。

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

通過傳遞記錄器和事件類型來在建立連接時記錄客戶端日誌跟蹤。 消息記錄與指定的日誌級別和更高。 可用紀錄等級如下:

* `signalR.LogLevel.Error`&ndash;錯誤消息。 僅`Error`記錄郵件。
* `signalR.LogLevel.Warning`&ndash;有關潛在錯誤的警告消息。 日誌`Warning``Error`和消息。
* `signalR.LogLevel.Information`&ndash;狀態消息,無錯誤。 日誌`Information``Warning``Error`和消息。
* `signalR.LogLevel.Trace`&ndash;跟蹤消息。 記錄所有內容,包括集線器和客戶端之間傳輸的數據。

使用[HubConnectBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上的[配置記錄](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法設定紀錄等級。 消息將記錄到瀏覽器控制台。

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>重新連線客戶端

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>自動重新連線

JavaScriptSignalR用戶端可以配置為`withAutomaticReconnect`使用[HubConnectBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上的方法自動重新連接。 默認情況下,它不會自動重新連接。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

沒有任何參數,`withAutomaticReconnect()`將用戶端配置為分別等待 0、2、10 和 30 秒,然後再嘗試每次重新連接嘗試,在四次嘗試失敗後停止。

在開始任何重新連線嘗試之前,`HubConnection``HubConnectionState.Reconnecting`將轉換為`onreconnecting`狀態 並觸發其回調,而不是`Disconnected`轉換到 狀態並`onclose`觸發其回調,就像未配置自動`HubConnection`重新連接一樣。 這提供了一個機會來警告使用者連接已丟失並禁用 UI 元素。

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

如果用戶端在前四次嘗試中成功重新連接,則將`HubConnection`轉換回`Connected`狀態並觸發`onreconnected`其回調。 這提供了一個機會,通知用戶連接已重新建立。

由於連接對伺服器看起來完全新,因此將為`connectionId``onreconnected`回調提供新的連接。

> [!WARNING]
> 如果`onreconnected`配置為[跳過協商](xref:signalr/configuration#configure-client-options)`connectionId`,`HubConnection`則回調的參數將未定義。

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()`不會設定`HubConnection`以 重試初始啟動失敗,因此需要手動處理啟動失敗:

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

如果用戶端在前四次嘗試中未成功重新連接,則將`HubConnection`轉換為`Disconnected`狀態並觸發其[關閉](/javascript/api/%40aspnet/signalr/hubconnection#onclose)回調。 這提供了一個機會,通知使用者連接已永久丟失,並建議刷新頁面:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

為了在斷開連接或更改重新連接計時之前設定自定義的重新連接嘗試次數,`withAutomaticReconnect`請接受表示延遲(以毫秒為單位)的數位陣列,以等待,然後再開始每次重新連接嘗試。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

前面的範例配置`HubConnection`要在連接丟失後立即開始嘗試重新連接。 對於默認配置也是如此。

如果第一次重新連接嘗試失敗,第二次重新連接嘗試也將立即開始,而不是像預設配置中那樣等待 2 秒。

如果第二次重新連接嘗試失敗,第三次重新連接嘗試將在 10 秒內開始,這再次類似於默認配置。

然後,自定義行為會在第三次重新連接嘗試失敗后停止,而不是像在預設配置中那樣在 30 秒內嘗試再次重新連接嘗試,從而再次偏離默認行為。

如果希望對自動重新連接嘗試的計時和次數進行更多控制,請`withAutomaticReconnect`接受`IRetryPolicy`實現介面的物件,該物件具有名為`nextRetryDelayInMilliseconds`的 單個方法。

`nextRetryDelayInMilliseconds`使用類型`RetryContext`獲取單個參數。 有`RetryContext`三個屬性: `previousRetryCount` `elapsedMilliseconds``retryReason`與`number`分別為`number``Error`a 和 。 在第一次重新連接嘗試之前,`previousRetryCount``elapsedMilliseconds`兩 者都將為`retryReason`零, 並且將是導致連接丟失的錯誤。 每次失敗的重試嘗試(`previousRetryCount`將遞增為 1)`elapsedMilliseconds`後, 將更新以反映到目前為止重新連接所花費的時間(以毫秒為`retryReason`單位), 並且將是導致上次重新連接嘗試失敗的錯誤。

`nextRetryDelayInMilliseconds`必須傳回表示在下一次重新連接嘗試之前等待的毫秒數的數位`null``HubConnection`, 或者是否應停止重新連接。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
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

或者,您可以編寫代碼,這些代碼將手動重新連接用戶端,如[手動重新連接](#manually-reconnect)中所示。

::: moniker-end

### <a name="manually-reconnect"></a>手動重新連接

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 在 3.0 之前SignalR,的 JavaScript 用戶端不會自動重新連接。 您必須編寫代碼,以手動重新連接用戶端。

::: moniker-end

以下代碼展示了典型的手動重新連接方法:

1. 創建函數(在本例中為`start`函數)以啟動連接。
1. 在`start`連接`onclose`的事件處理程序中調用函數。

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

實際實現將使用指數級回退或在放棄之前重試指定次數。

## <a name="additional-resources"></a>其他資源

* [JavaScript API 參考](/javascript/api/?view=signalr-js-latest)
* [JavaScript 教學](xref:tutorials/signalr)
* [WebPack 與 TypeScript 教學](xref:tutorials/signalr-typescript-webpack)
* [樞紐](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [跨源要求 (CORS)](xref:security/cors)
* [AzureSignalR服務無伺服器文件](/azure/azure-signalr/signalr-concept-serverless-development-config)
