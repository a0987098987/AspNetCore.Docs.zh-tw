---
title: ASP.NET Core SignalR JavaScript 用戶端
author: bradygaster
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: f1f072e63928502fa1bad62e808ff035e57f2fd3
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983009"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="00c8f-103">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="00c8f-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="00c8f-104">作者：[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="00c8f-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="00c8f-105">ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。</span><span class="sxs-lookup"><span data-stu-id="00c8f-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="00c8f-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00c8f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="00c8f-107">SignalR 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="00c8f-107">Install the SignalR client package</span></span>

<span data-ttu-id="00c8f-108">SignalR JavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)套件形式傳遞。</span><span class="sxs-lookup"><span data-stu-id="00c8f-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="00c8f-109">如果您使用 Visual Studio，可以在**Package Manager Console**的根資料夾中執行`npm install`。</span><span class="sxs-lookup"><span data-stu-id="00c8f-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="00c8f-110">如果您使用 Visual Studio Code，請從**整合式終端機**中執行命令。</span><span class="sxs-lookup"><span data-stu-id="00c8f-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="00c8f-111">npm 會將套件內容安裝在 *node_modules\\@aspnet\signalr\dist\browser* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="00c8f-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="00c8f-112">在 *wwwroot\\lib* 資料夾下，建立名為 *signalr* 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="00c8f-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="00c8f-113">將 *signalr.js* 檔案複製到 *wwwroot\lib\signalr* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="00c8f-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="00c8f-114">使用 SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="00c8f-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="00c8f-115">在 `<script>` 元素中參考 SignalR JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="00c8f-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="00c8f-116">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="00c8f-116">Connect to a hub</span></span>

<span data-ttu-id="00c8f-117">下列程式碼會建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="00c8f-118">中樞的名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="00c8f-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="00c8f-119">跨原始來源的連線</span><span class="sxs-lookup"><span data-stu-id="00c8f-119">Cross-origin connections</span></span>

<span data-ttu-id="00c8f-120">一般而言，瀏覽器會從與所要求之頁面相同的網域載入連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="00c8f-121">不過，有些情況下會需要連線到另一個網域的連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="00c8f-122">若要防止惡意網站從另一個網站讀取敏感性資料，預設會停用[跨來源連線](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="00c8f-123">若要允許跨來源要求，請在 `Startup` 類別中啟用它。</span><span class="sxs-lookup"><span data-stu-id="00c8f-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="00c8f-124">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="00c8f-124">Call hub methods from client</span></span>

<span data-ttu-id="00c8f-125">JavaScript 用戶端會透過 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection) [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) 方法呼叫中樞的公用方法。</span><span class="sxs-lookup"><span data-stu-id="00c8f-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="00c8f-126">`invoke` 方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="00c8f-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="00c8f-127">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="00c8f-127">The name of the hub method.</span></span> <span data-ttu-id="00c8f-128">在下列範例中，在中樞的方法名稱是`SendMessage`。</span><span class="sxs-lookup"><span data-stu-id="00c8f-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="00c8f-129">中樞的方法中定義的任何引數。</span><span class="sxs-lookup"><span data-stu-id="00c8f-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="00c8f-130">在下列範例中，引數名稱是`message`。</span><span class="sxs-lookup"><span data-stu-id="00c8f-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="00c8f-131">範例程式碼會使用目前版本的 Internet Explorer 以外的所有主要瀏覽器都支援的箭號函式語法。</span><span class="sxs-lookup"><span data-stu-id="00c8f-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="00c8f-132">如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="00c8f-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="00c8f-133">如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="00c8f-134">`invoke`方法會傳回 JavaScript[承諾](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-134">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="00c8f-135">`Promise`解決的傳回值 （如果有的話） 的伺服器上的此方法傳回時。</span><span class="sxs-lookup"><span data-stu-id="00c8f-135">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="00c8f-136">如果在伺服器上的方法擲回錯誤，`Promise`拒絕並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-136">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="00c8f-137">使用`then`並`catch`上的方法`Promise`本身來處理這些情況下 (或`await`語法)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-137">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="00c8f-138">`send`方法會傳回 JavaScript `Promise`。</span><span class="sxs-lookup"><span data-stu-id="00c8f-138">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="00c8f-139">`Promise`解決伺服器傳送此訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-139">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="00c8f-140">如果沒有傳送訊息時，出現錯誤`Promise`拒絕並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-140">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="00c8f-141">使用`then`並`catch`上的方法`Promise`本身來處理這些情況下 (或`await`語法)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-141">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="00c8f-142">使用`send`不等候，直到伺服器收到的訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-142">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="00c8f-143">因此，不可能從伺服器傳回的資料或錯誤。</span><span class="sxs-lookup"><span data-stu-id="00c8f-143">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="00c8f-144">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="00c8f-144">Call client methods from hub</span></span>

<span data-ttu-id="00c8f-145">若要從中樞接收訊息，請使用 `HubConnection` 的 [on](/javascript/api/%40aspnet/signalr/hubconnection#on) 方法來定義方法。</span><span class="sxs-lookup"><span data-stu-id="00c8f-145">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="00c8f-146">JavaScript 用戶端方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="00c8f-146">The name of the JavaScript client method.</span></span> <span data-ttu-id="00c8f-147">下列範例中，在方法名稱是`ReceiveMessage`。</span><span class="sxs-lookup"><span data-stu-id="00c8f-147">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="00c8f-148">中樞會將傳遞給方法的引數。</span><span class="sxs-lookup"><span data-stu-id="00c8f-148">Arguments the hub passes to the method.</span></span> <span data-ttu-id="00c8f-149">在下列範例中，引數值是`message`。</span><span class="sxs-lookup"><span data-stu-id="00c8f-149">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="00c8f-150">當伺服器端程式碼使用 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 方法來呼叫它時，會執行 `connection.on` 中的上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="00c8f-150">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="00c8f-151">SignalR 透過比對 `SendAsync` 與 `connection.on` 中定義的方法名稱與引數，來判斷要呼叫哪個用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="00c8f-151">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="00c8f-152">最佳做法是，在 `on` 之後呼叫 `HubConnection` 的 [start](/javascript/api/%40aspnet/signalr/hubconnection#start) 方法。</span><span class="sxs-lookup"><span data-stu-id="00c8f-152">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="00c8f-153">這麼做可確保您的處理常式會在註冊之後，才會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-153">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="00c8f-154">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="00c8f-154">Error handling and logging</span></span>

<span data-ttu-id="00c8f-155">在 `start` 方法的結尾鏈結 `catch` 方法以處理用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="00c8f-155">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="00c8f-156">您可以使用 `console.error` 輸出錯誤至瀏覽器的主控台。</span><span class="sxs-lookup"><span data-stu-id="00c8f-156">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="00c8f-157">藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。</span><span class="sxs-lookup"><span data-stu-id="00c8f-157">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="00c8f-158">使用指定的記錄層級和更新版本，則會記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-158">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="00c8f-159">可用的記錄層級如下所示：</span><span class="sxs-lookup"><span data-stu-id="00c8f-159">Available log levels are as follows:</span></span>

* <span data-ttu-id="00c8f-160">`signalR.LogLevel.Error` &ndash; 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-160">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="00c8f-161">記錄檔`Error`只有訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-161">Logs `Error` messages only.</span></span>
* <span data-ttu-id="00c8f-162">`signalR.LogLevel.Warning` &ndash; 可能的錯誤的相關警告訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-162">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="00c8f-163">會記錄 `Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-163">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="00c8f-164">`signalR.LogLevel.Information` &ndash; 沒有錯誤的狀態訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-164">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="00c8f-165">會記錄 `Information`、`Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-165">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="00c8f-166">`signalR.LogLevel.Trace` &ndash; 追蹤訊息。</span><span class="sxs-lookup"><span data-stu-id="00c8f-166">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="00c8f-167">記錄所有事件，包括中樞和用戶端之間傳輸的資料。</span><span class="sxs-lookup"><span data-stu-id="00c8f-167">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="00c8f-168">使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="00c8f-168">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="00c8f-169">訊息會記錄到瀏覽器主控台。</span><span class="sxs-lookup"><span data-stu-id="00c8f-169">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="00c8f-170">重新連線用戶端</span><span class="sxs-lookup"><span data-stu-id="00c8f-170">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="00c8f-171">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="00c8f-171">Automatically reconnect</span></span>

<span data-ttu-id="00c8f-172">SignalR 的 JavaScript 用戶端可以設定為使用自動重新`withAutomaticReconnect`方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-172">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="00c8f-173">它不會依預設會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-173">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="00c8f-174">如果沒有任何參數，`withAutomaticReconnect()`設定用戶端一直等候 0、 2、 10 和 30 秒，分別是然後再嘗試每次重新連接嘗試，四個失敗的嘗試之後停止。</span><span class="sxs-lookup"><span data-stu-id="00c8f-174">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="00c8f-175">開始任何重新連接嘗試之前`HubConnection`會轉為`HubConnectionState.Reconnecting`狀態，並引發其`onreconnecting`回呼，而不是轉換為`Disconnected`狀態，並觸發其`onclose`回呼喜歡`HubConnection`未自動重新連線設定。</span><span class="sxs-lookup"><span data-stu-id="00c8f-175">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="00c8f-176">這便有機會時警告使用者的連線已遺失，並在停用 UI 項目。</span><span class="sxs-lookup"><span data-stu-id="00c8f-176">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="00c8f-177">如果用戶端順利重新連線內的前四個的試`HubConnection`就會轉換回`Connected`狀態，並引發其`onreconnected`回呼。</span><span class="sxs-lookup"><span data-stu-id="00c8f-177">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="00c8f-178">這便有機會以通知的使用者已重新建立連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-178">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="00c8f-179">連線看起來完全新增到伺服器，因為新`connectionId`將提供給`onreconnected`回呼。</span><span class="sxs-lookup"><span data-stu-id="00c8f-179">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="00c8f-180">`onreconnected`回呼的`connectionId`參數將會是未定義，如果`HubConnection`已設定為[略過交涉](xref:signalr/configuration#configure-client-options)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-180">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="00c8f-181">`withAutomaticReconnect()` 不會設定`HubConnection`重試初始啟動時失敗，因此需要手動處理啟動失敗：</span><span class="sxs-lookup"><span data-stu-id="00c8f-181">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="00c8f-182">如果用戶端不會成功地重新連接內的前四個的試`HubConnection`會轉為`Disconnected`狀態，並引發其[onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose)回呼。</span><span class="sxs-lookup"><span data-stu-id="00c8f-182">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="00c8f-183">這讓您有機會通知的使用者此連接已經永久遺失，並建議重新整理頁面：</span><span class="sxs-lookup"><span data-stu-id="00c8f-183">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

<span data-ttu-id="00c8f-184">若要設定自訂的數字的中斷連線之前重新連接嘗試，或變更重新連線延遲時間，`withAutomaticReconnect`接受代表數字以毫秒為單位的延遲開始每次重新連接嘗試之前等候的陣列。</span><span class="sxs-lookup"><span data-stu-id="00c8f-184">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="00c8f-185">上述範例會設定`HubConnection`啟動連線將會中斷之後，立即嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-185">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="00c8f-186">這也是預設組態，則為 true。</span><span class="sxs-lookup"><span data-stu-id="00c8f-186">This is also true for the default configuration.</span></span>

<span data-ttu-id="00c8f-187">如果第一次重新連接嘗試失敗，第二次重新連接嘗試將也會立即開始而不是等到 2 秒，像是在預設組態中。</span><span class="sxs-lookup"><span data-stu-id="00c8f-187">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="00c8f-188">如果第二次重新連接嘗試失敗，第三次重新連接嘗試就會啟動在 10 秒，這又是預設組態類似。</span><span class="sxs-lookup"><span data-stu-id="00c8f-188">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="00c8f-189">自訂行為然後分離一次的預設行為藉由停止之後第三個重新連接嘗試失敗，而不是嘗試其中一個更重新連接嘗試中像是在預設組態中的另一個 30 秒。</span><span class="sxs-lookup"><span data-stu-id="00c8f-189">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="00c8f-190">如果您想要更進一步控制時間和數字的自動重新連線嘗試`withAutomaticReconnect`物件，實作會接受`IReconnectPolicy`介面，其具有單一方法，名為`nextRetryDelayInMilliseconds`。</span><span class="sxs-lookup"><span data-stu-id="00c8f-190">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IReconnectPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="00c8f-191">`nextRetryDelayInMilliseconds` 接受兩個引數，`previousRetryCount`和`elapsedMilliseconds`，這是兩個數字。</span><span class="sxs-lookup"><span data-stu-id="00c8f-191">`nextRetryDelayInMilliseconds` takes two arguments, `previousRetryCount` and `elapsedMilliseconds`, which are both numbers.</span></span> <span data-ttu-id="00c8f-192">第一次重新連接嘗試中前, 兩者`previousRetryCount`和`elapsedMilliseconds`會是零。</span><span class="sxs-lookup"><span data-stu-id="00c8f-192">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero.</span></span> <span data-ttu-id="00c8f-193">在每次失敗的重試之後,`previousRetryCount`會遞增 1 和`elapsedMilliseconds`將會更新以反映所花費的時間重新連線到目前為止，以毫秒為單位。</span><span class="sxs-lookup"><span data-stu-id="00c8f-193">After each failed retry attempt, `previousRetryCount` will be incremented by one and `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds.</span></span>

<span data-ttu-id="00c8f-194">`nextRetryDelayInMilliseconds` 必須傳回任一數字表示的毫秒數下一次重新連接嘗試之前要等待或`null`如果`HubConnection`應該停止重新連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-194">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="00c8f-195">或者，您可以在其中撰寫程式碼所示的手動將重新連線您的用戶端[以手動方式重新連線](#manually-reconnect)。</span><span class="sxs-lookup"><span data-stu-id="00c8f-195">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="00c8f-196">以手動方式重新連線</span><span class="sxs-lookup"><span data-stu-id="00c8f-196">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="00c8f-197">在 3.0 之前，SignalR 的 JavaScript 用戶端不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-197">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="00c8f-198">您必須撰寫程式碼會以手動方式重新連接您的用戶端。</span><span class="sxs-lookup"><span data-stu-id="00c8f-198">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="00c8f-199">下列程式碼示範一個典型的手動重新連線的方法：</span><span class="sxs-lookup"><span data-stu-id="00c8f-199">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="00c8f-200">函式 (在此情況下，`start`函式) 建立以啟動的連線。</span><span class="sxs-lookup"><span data-stu-id="00c8f-200">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="00c8f-201">呼叫`start`函式中的連線`onclose`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="00c8f-201">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="00c8f-202">真實世界實作會使用指數退避法，或指定的次數後放棄重試一次。</span><span class="sxs-lookup"><span data-stu-id="00c8f-202">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="00c8f-203">其他資源</span><span class="sxs-lookup"><span data-stu-id="00c8f-203">Additional resources</span></span>

* [<span data-ttu-id="00c8f-204">JavaScript API 參考</span><span class="sxs-lookup"><span data-stu-id="00c8f-204">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="00c8f-205">JavaScript 教學課程</span><span class="sxs-lookup"><span data-stu-id="00c8f-205">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="00c8f-206">WebPack 和 TypeScript 的教學課程</span><span class="sxs-lookup"><span data-stu-id="00c8f-206">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="00c8f-207">中樞</span><span class="sxs-lookup"><span data-stu-id="00c8f-207">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="00c8f-208">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="00c8f-208">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="00c8f-209">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="00c8f-209">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="00c8f-210">跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="00c8f-210">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="00c8f-211">Azure SignalR 服務的無伺服器文件</span><span class="sxs-lookup"><span data-stu-id="00c8f-211">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
