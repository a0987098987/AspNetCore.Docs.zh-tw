---
title: ASP.NET Core SignalR JavaScript 用戶端
author: tdykstra
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 10958c414aa4a285c8a2810bb99e278f719c5b7f
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "46483045"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript 用戶端

作者：[Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>SignalR 用戶端封裝安裝

SignalR JavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)傳遞封裝。 如果您使用 Visual Studio，可以在**Package Manager Console**的專案根資料夾中執行`npm install`。 如果您使用 Visual Studio Code，請從**整合式終端機**中執行命令。

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm 安裝的套件會存放在 *node_modules\\@aspnet\signalr\dist\browser* 資料夾。 在*wwwroot\\lib*資料夾下，建立名為*signalr*的新資料夾，接著複製 *signalr.js* 檔案至 *wwwroot\lib\signalr* 資料夾中。

## <a name="use-the-signalr-javascript-client"></a>使用 SignalR JavaScript 用戶端

在`<script>`中加入 SignalR JavaScript 用戶端的參考。

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>連線至中樞

下列程式碼會建立並啟動連線。 中樞的名稱不區分大小寫。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>跨原始來源的連線

一般而言，瀏覽器會載入與要求的頁面相同的網域連線。 不過，有些情況下會需要連接到另一個網域。

若要防止惡意網站從另一個網站讀取敏感性資料，預設[跨原始來源連線](xref:security/cors)會停用。 若要允許跨原始來源要求，請在`Startup`類別中設定。

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

JavaScript 用戶端呼叫的公用方法上透過[HubConnection](/javascript/api/%40aspnet/signalr/hubconnection)集線器的[invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke)方法。 `invoke`方法接受兩個引數：

* 中樞方法的名稱。 在下列範例中，在中樞的方法名稱是`SendMessage`。
* 中樞的方法中定義的任何引數。 在下列範例中，引數名稱是`message`。

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

若要從中樞接收訊息，定義方法，使用`HubConnection`的[on](/javascript/api/%40aspnet/signalr/hubconnection#on)方法。

* JavaScript 用戶端方法的名稱。 下列範例中，在方法名稱是`ReceiveMessage`。
* 中樞會將傳遞給方法的引數。 在下列範例中，引數值是`message`。

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

在上述程式碼`connection.on`會執行伺服器端程式碼，並呼叫執行[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)方法。

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR 在`SendAsync`和`connection.on`中定義了用戶端要呼叫的方法及引數，並透過比對方法的名稱來判斷要呼叫哪個方法。

> [!NOTE]
> 最佳做法，在`on`之後呼叫`HubConnection`的[start](/javascript/api/%40aspnet/signalr/hubconnection#start)方法。 這麼做可確保您的處理常式會在註冊之後，才會收到訊息。

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

在`start`方法的結尾鏈結`catch`方法，來處理用戶端的錯誤。 可使用`console.error`輸出錯誤至瀏覽器的主控台。

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。 使用指定的記錄層級和更新版本，則會記錄訊息。 可用的記錄層級如下所示：

* `signalR.LogLevel.Error` &ndash; 錯誤訊息。 記錄檔`Error`只有訊息。
* `signalR.LogLevel.Warning` &ndash; 可能的錯誤相關的警告訊息。 記錄檔`Warning`和`Error`訊息。
* `signalR.LogLevel.Information` &ndash; 沒有錯誤的狀態訊息。 記錄檔`Information`、 `Warning`和`Error`訊息。
* `signalR.LogLevel.Trace` &ndash; 追蹤訊息。 記錄所有事件，包括中樞和用戶端之間傳輸的資料。

使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)設定記錄層級。 訊息會記錄到瀏覽器主控台。

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="additional-resources"></a>其他資源

* [JavaScript API 參考](/javascript/api/?view=signalr-js-latest)
* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [啟用 ASP.NET Core 中的跨源要求 (CORS)](xref:security/cors)
