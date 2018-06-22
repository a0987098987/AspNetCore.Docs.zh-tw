---
title: ASP.NET Core SignalR JavaScript 用戶端
author: rachelappel
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: 9ea12dc21abfef6d2e6f3fcc455d8aa58ecaa011
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271940"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript 用戶端

作者：[Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>安裝 SignalR 用戶端套件

SignalR JavaScript 用戶端程式庫會傳遞做為[npm](https://www.npmjs.com/)封裝。 如果您使用 Visual Studio，執行`npm install`從**Package Manager Console**根資料夾中。 針對 Visual Studio 程式碼，從執行命令**整合的終端機**。

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm 安裝中的封裝內容*node_modules\\ @aspnet\signalr\dist\browser* 資料夾。 建立新的資料夾，名為*signalr*下*wwwroot\\lib*資料夾。 複製*signalr.js*檔案*wwwroot\lib\signalr*資料夾。

## <a name="use-the-signalr-javascript-client"></a>使用 SignalR JavaScript 用戶端

參考中的 SignalR JavaScript 用戶端`<script>`項目。

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>連線至中樞

下列程式碼會建立並啟動連線。 中樞的名稱是不區分大小寫。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>跨原始連線

通常，瀏覽器，從相同的網域要求的頁面載入連線。 不過，有一些需要連接到另一個網域時的情況。

若要防止惡意網站讀取敏感性資料，從其他站台，[跨原始連線](xref:security/cors)預設會停用。 若要允許跨原始要求，在中啟用它`Startup`類別。

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>從用戶端呼叫 hub 方法

JavaScript 用戶端呼叫公用方法上透過集線器使用`connection.invoke`。 `invoke`方法接受兩個引數：

* 中樞方法的名稱。 在下列範例中，為中樞名稱`SendMessage`。
* 中樞方法中定義的任何引數。 在下列範例中，引數名稱是`message`。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>從中樞呼叫用戶端方法

若要從中樞接收訊息，定義方法，使用`connection.on`方法。

* JavaScript 用戶端方法的名稱。 下列範例中，在方法名稱是`ReceiveMessage`。
* 中樞將傳遞至方法的引數。 在下列範例中，引數值是`message`。

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

在上述程式碼`connection.on`時伺服器端程式碼會呼叫它使用執行`SendAsync`方法。

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR 判斷用戶端来呼叫的方法藉由比對的方法名稱和引數中定義`SendAsync`和`connection.on`。

> [!NOTE]
> 最佳做法，呼叫`connection.start`之後`connection.on`使您的處理常式會註冊之前收到任何訊息。

## <a name="error-handling-and-logging"></a>錯誤處理和記錄

鏈結`catch`方法的結尾`connection.start`方法來處理用戶端的錯誤。 使用`console.error`瀏覽器的主控台輸出的錯誤。

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

安裝程式的用戶端記錄追蹤，藉由傳遞要在建立連接時，記錄的記錄器和事件類型。 使用指定的記錄層級及更高版本，則會記錄訊息。 可用的記錄層級如下所示：

* `signalR.LogLevel.Error` ： 錯誤訊息。 記錄檔`Error`只訊息。
* `signalR.LogLevel.Warning` ： 可能的錯誤警告訊息。 記錄檔`Warning`，和`Error`訊息。
* `signalR.LogLevel.Information` ： 不會發生錯誤狀態訊息。 記錄檔`Information`， `Warning`，和`Error`訊息。
* `signalR.LogLevel.Trace` ： 追蹤訊息。 記錄所有項目，包括資料中心和用戶端之間傳輸。

使用`configureLogging`方法`HubConnectionBuilder`來設定記錄層級。 訊息會記錄到瀏覽器主控台。

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>相關資源

* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
* [啟用跨原始要求 (CORS) 中 ASP.NET Core](xref:security/cors)