---
title: ASP.NET Core SignalR JavaScript 用戶端
author: rachelappel
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: d0eba401b3152d510b9db092169e83f6da2a0523
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938040"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript 用戶端

作者：[Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>SignalR 用戶端封裝安裝

SignalR JavaScript 用戶端程式庫會以傳遞[npm](https://www.npmjs.com/)封裝。 如果您使用 Visual Studio，執行`npm install`從**Package Manager Console**在根資料夾中。 適用於 Visual Studio Code，請從執行命令**整合式終端機**。

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm 安裝的套件內容*node_modules\\ @aspnet\signalr\dist\browser* 資料夾。 建立新的資料夾，名為*signalr*下方*wwwroot\\lib*資料夾。 複製*signalr.js*的檔案*wwwroot\lib\signalr*資料夾。

## <a name="use-the-signalr-javascript-client"></a>使用 SignalR JavaScript 用戶端

參考中的 SignalR JavaScript 用戶端`<script>`項目。

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>連線至中樞

下列程式碼會建立並啟動連線。 中樞的名稱不區分大小寫。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>跨原始來源的連線

一般而言，瀏覽器會載入連線從與要求的頁面相同的網域。 不過，有些情況下連接到另一個網域時所需。

若要防止惡意網站從另一個網站讀取敏感性資料[跨原始來源連線](xref:security/cors)預設會停用。 若要允許跨原始來源要求，讓它在`Startup`類別。

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫中樞方法

JavaScript 用戶端呼叫的公用方法上的中樞使用`connection.invoke`。 `invoke`方法接受兩個引數：

* 中樞方法的名稱。 下列範例中，在中樞名稱是`SendMessage`。
* 中樞的方法中定義的任何引數。 在下列範例中，引數名稱是`message`。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>用戶端方法呼叫來自中樞

若要從中樞接收訊息，定義方法，使用`connection.on`方法。

* JavaScript 用戶端方法的名稱。 下列範例中，在方法名稱是`ReceiveMessage`。
* 中樞會將傳遞給方法的引數。 在下列範例中，引數值是`message`。

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

在上述程式碼`connection.on`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR 判斷用戶端来呼叫的方法比對方法的名稱，並定義中的引數`SendAsync`和`connection.on`。

> [!NOTE]
> 最佳做法，呼叫`connection.start`之後`connection.on`因此會收到任何訊息之前，會註冊您的處理常式。

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

鏈結`catch`方法的結尾`connection.start`方法來處理用戶端的錯誤。 使用`console.error`瀏覽器的主控台輸出錯誤。

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。 使用指定的記錄層級和更新版本，則會記錄訊息。 可用的記錄層級如下所示：

* `signalR.LogLevel.Error` ： 錯誤訊息。 記錄檔`Error`只有訊息。
* `signalR.LogLevel.Warning` ： 可能的錯誤警告訊息。 記錄檔`Warning`，和`Error`訊息。
* `signalR.LogLevel.Information` ： 不會出現錯誤狀態訊息。 記錄檔`Information`， `Warning`，和`Error`訊息。
* `signalR.LogLevel.Trace` ： 追蹤訊息。 記錄所有事件，包括中樞和用戶端之間傳輸的資料。

使用`configureLogging`方法`HubConnectionBuilder`設定記錄層級。 訊息會記錄到瀏覽器主控台。

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>相關資源

* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [啟用 ASP.NET Core 中的跨源要求 (CORS)](xref:security/cors)
