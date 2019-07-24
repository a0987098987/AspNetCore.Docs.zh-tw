---
title: ASP.NET Core SignalR JavaScript 用戶端
author: bradygaster
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/28/2019
uid: signalr/javascript-client
ms.openlocfilehash: f314e1fe0ef0ea73a28b332404a09f2956524132
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412376"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="4a9f2-103">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="4a9f2-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="4a9f2-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="4a9f2-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="4a9f2-105">ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="4a9f2-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a9f2-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="4a9f2-107">SignalR 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="4a9f2-107">Install the SignalR client package</span></span>

<span data-ttu-id="4a9f2-108">SignalR JavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)套件形式傳遞。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="4a9f2-109">如果您使用 Visual Studio，可以在**Package Manager Console**的根資料夾中執行`npm install`。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="4a9f2-110">如果您使用 Visual Studio Code，請從**整合式終端機**中執行命令。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="4a9f2-111">npm 會將套件內容安裝在 *node_modules\\@microsoft\signalr\dist\browser* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="4a9f2-112">在 *wwwroot\\lib* 資料夾下，建立名為 *signalr* 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="4a9f2-113">將 *signalr.js* 檔案複製到 *wwwroot\lib\signalr* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="4a9f2-114">npm 會將套件內容安裝在 *node_modules\\@aspnet\signalr\dist\browser* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="4a9f2-115">在 *wwwroot\\lib* 資料夾下，建立名為 *signalr* 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="4a9f2-116">將 *signalr.js* 檔案複製到 *wwwroot\lib\signalr* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="4a9f2-117">使用 SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="4a9f2-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="4a9f2-118">在 `<script>` 元素中參考 SignalR JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="4a9f2-119">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="4a9f2-119">Connect to a hub</span></span>

<span data-ttu-id="4a9f2-120">下列程式碼會建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="4a9f2-121">中樞的名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="4a9f2-122">跨原始來源的連線</span><span class="sxs-lookup"><span data-stu-id="4a9f2-122">Cross-origin connections</span></span>

<span data-ttu-id="4a9f2-123">一般而言，瀏覽器會從與所要求之頁面相同的網域載入連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="4a9f2-124">不過，有些情況下會需要連線到另一個網域的連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="4a9f2-125">若要防止惡意網站從另一個網站讀取敏感性資料，預設會停用[跨來源連線](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="4a9f2-126">若要允許跨來源要求，請在 `Startup` 類別中啟用它。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="4a9f2-127">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="4a9f2-127">Call hub methods from client</span></span>

<span data-ttu-id="4a9f2-128">JavaScript 用戶端會透過 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection)[invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) 方法呼叫中樞的公用方法。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="4a9f2-129">          `invoke` 方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="4a9f2-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="4a9f2-130">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-130">The name of the hub method.</span></span> <span data-ttu-id="4a9f2-131">在下列範例中，在中樞的方法名稱是`SendMessage`。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="4a9f2-132">中樞的方法中定義的任何引數。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="4a9f2-133">在下列範例中，引數名稱是`message`。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="4a9f2-134">範例程式碼使用箭號函式語法, 在 Internet Explorer 以外的所有主要瀏覽器的目前版本中都受到支援。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="4a9f2-135">如果您使用*無伺服器模式*中的 Azure SignalR Service, 就無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="4a9f2-136">如需詳細資訊, 請參閱[SignalR Service 檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="4a9f2-137">方法會傳回 JavaScript[承諾。](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) `invoke`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="4a9f2-138">當伺服器上的方法傳回時,會使用傳回值(如果有的話)來解析。`Promise`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="4a9f2-139">如果伺服器上的方法擲回錯誤, `Promise`則會拒絕並顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="4a9f2-140">在本身上`catch`使用和方法來處理這些案例 (或`await`語法)。 `then` `Promise`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="4a9f2-141">方法會傳回 JavaScript `Promise`。 `send`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="4a9f2-142">當訊息已傳送到伺服器時,就會解決。`Promise`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="4a9f2-143">如果傳送訊息時發生錯誤, `Promise`則會拒絕, 並顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="4a9f2-144">在本身上`catch`使用和方法來處理這些案例 (或`await`語法)。 `then` `Promise`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="4a9f2-145">使用`send`並不會等到伺服器收到訊息為止。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="4a9f2-146">因此, 不可能從伺服器傳回資料或錯誤。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="4a9f2-147">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="4a9f2-147">Call client methods from hub</span></span>

<span data-ttu-id="4a9f2-148">若要從中樞接收訊息，請使用 `HubConnection` 的 [on](/javascript/api/%40aspnet/signalr/hubconnection#on) 方法來定義方法。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="4a9f2-149">JavaScript 用戶端方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="4a9f2-150">下列範例中，在方法名稱是`ReceiveMessage`。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="4a9f2-151">中樞會將傳遞給方法的引數。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="4a9f2-152">在下列範例中，引數值是`message`。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="4a9f2-153">當伺服器端程式碼使用 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 方法來呼叫它時，會執行 `connection.on` 中的上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="4a9f2-154">SignalR 透過比對 `SendAsync` 與 `connection.on` 中定義的方法名稱與引數，來判斷要呼叫哪個用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-154">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="4a9f2-155">最佳做法是，在 `on` 之後呼叫 `HubConnection` 的 [start](/javascript/api/%40aspnet/signalr/hubconnection#start) 方法。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="4a9f2-156">這麼做可確保您的處理常式會在註冊之後，才會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="4a9f2-157">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="4a9f2-157">Error handling and logging</span></span>

<span data-ttu-id="4a9f2-158">在 `start` 方法的結尾鏈結 `catch` 方法以處理用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="4a9f2-159">您可以使用 `console.error` 輸出錯誤至瀏覽器的主控台。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="4a9f2-160">藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="4a9f2-161">使用指定的記錄層級和更新版本，則會記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="4a9f2-162">可用的記錄層級如下所示：</span><span class="sxs-lookup"><span data-stu-id="4a9f2-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="4a9f2-163">`signalR.LogLevel.Error` &ndash; 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="4a9f2-164">記錄檔`Error`只有訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="4a9f2-165">`signalR.LogLevel.Warning` &ndash; 可能的錯誤的相關警告訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="4a9f2-166">會記錄 `Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="4a9f2-167">`signalR.LogLevel.Information` &ndash; 沒有錯誤的狀態訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="4a9f2-168">會記錄 `Information`、`Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="4a9f2-169">`signalR.LogLevel.Trace` &ndash; 追蹤訊息。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="4a9f2-170">記錄所有事件，包括中樞和用戶端之間傳輸的資料。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="4a9f2-171">使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="4a9f2-172">訊息會記錄到瀏覽器主控台。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="4a9f2-173">重新連線用戶端</span><span class="sxs-lookup"><span data-stu-id="4a9f2-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="4a9f2-174">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="4a9f2-174">Automatically reconnect</span></span>

<span data-ttu-id="4a9f2-175">適用于 SignalR 的 JavaScript 用戶端可以設定為使用[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上`withAutomaticReconnect`的方法自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="4a9f2-176">根據預設, 它不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="4a9f2-177">如果沒有任何參數`withAutomaticReconnect()` , 則會先將用戶端設定為等待0、2、10和30秒, 再嘗試重新連線嘗試, 並在四次失敗嘗試之後停止。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="4a9f2-178">開始任何重新連線嘗試之前, `HubConnection`會轉換`HubConnectionState.Reconnecting`成狀態並引發其`onreconnecting`回呼, 而不是轉換為`Disconnected`狀態並觸發其`onclose`回呼, 例如`HubConnection`未設定自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="4a9f2-179">這會讓使用者有機會警告連接已遺失, 並停用 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="4a9f2-180">如果用戶端在前四次嘗試中成功重新連接`HubConnection` , 將會轉換回`Connected`狀態並引發其`onreconnected`回呼。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="4a9f2-181">這讓您有機會通知使用者已重新建立連接。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="4a9f2-182">由於連接看起來就是伺服器的全新連線, 因此`connectionId`會提供新的`onreconnected`給回呼。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="4a9f2-183">如果設定為`connectionId` [略過協商](xref:signalr/configuration#configure-client-options),回呼的參數將會是未定義的。`onreconnected` `HubConnection`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="4a9f2-184">`withAutomaticReconnect()`不會將`HubConnection`設定為重試初始啟動失敗, 因此必須手動處理啟動失敗:</span><span class="sxs-lookup"><span data-stu-id="4a9f2-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="4a9f2-185">如果用戶端在前四次嘗試中未成功重新連接`HubConnection` , 將會轉換`Disconnected`成狀態並引發其[onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose)回呼。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="4a9f2-186">這讓您有機會通知使用者連線已永久遺失, 並建議重新整理頁面:</span><span class="sxs-lookup"><span data-stu-id="4a9f2-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="4a9f2-187">為了在中斷連線或變更重新連線時間之前設定自訂的重新連線嘗試次數`withAutomaticReconnect` , 會接受代表在開始每次重新連線嘗試之前等待的延遲 (以毫秒為單位) 的數位陣列。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="4a9f2-188">上述範例`HubConnection`會將設定為, 以便在連接中斷之後立即開始嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="4a9f2-189">這也適用于預設設定。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="4a9f2-190">如果第一次重新連線嘗試失敗, 第二次重新連線嘗試也會立即開始, 而不是等待2秒, 就像在預設設定中一樣。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="4a9f2-191">如果第二次重新連線嘗試失敗, 第三次重新連線嘗試會在10秒內啟動, 這再次是預設設定。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="4a9f2-192">然後, 自訂行為會在第三次重新連線嘗試失敗之後停止, 而不是在另一個30秒嘗試重新連線嘗試, 就會再次從預設行為分歧, 而不是在預設設定中再試一次。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="4a9f2-193">如果您想要更充分掌控自動重新連線嘗試的時間和數目, `withAutomaticReconnect`則會接受一個物件`IRetryPolicy`來執行介面, 此介面具有名`nextRetryDelayInMilliseconds`為的單一方法。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="4a9f2-194">`nextRetryDelayInMilliseconds`接受具有類型`RetryContext`的單一引數。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="4a9f2-195">`previousRetryCount` `retryReason` 有三`Error`個屬性: `elapsedMilliseconds`、和分別為、`number`和。 `number` `RetryContext`</span><span class="sxs-lookup"><span data-stu-id="4a9f2-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="4a9f2-196">第一次重新連線嘗試之前`previousRetryCount` , `elapsedMilliseconds`和都`retryReason`是零, 而且會是造成連接遺失的錯誤。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="4a9f2-197">每次重試`previousRetryCount`失敗之後, 將會遞增一次, 將會更新, `elapsedMilliseconds`以反映到目前為止的重新連線時間量 ( `retryReason`以毫秒為單位), 而且將會是導致上次重新連線嘗試失敗的錯誤。無法.</span><span class="sxs-lookup"><span data-stu-id="4a9f2-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="4a9f2-198">`nextRetryDelayInMilliseconds`必須傳回數位, 代表下一次重新連接嘗試之前要等候的毫秒數, `null`或者, `HubConnection`如果應該停止重新連接, 則為。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        })
    .build();
```

<span data-ttu-id="4a9f2-199">或者, 您可以撰寫程式碼, 以手動方式重新連線用戶端, 如[手動重新](#manually-reconnect)連線中所示。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="4a9f2-200">手動重新連線</span><span class="sxs-lookup"><span data-stu-id="4a9f2-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="4a9f2-201">在3.0 之前, SignalR 的 JavaScript 用戶端不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="4a9f2-202">您必須撰寫程式碼會以手動方式重新連接您的用戶端。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="4a9f2-203">下列程式碼示範一般手動重新連接方法:</span><span class="sxs-lookup"><span data-stu-id="4a9f2-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="4a9f2-204">函式 (在此情況下，`start`函式) 建立以啟動的連線。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="4a9f2-205">呼叫`start`函式中的連線`onclose`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="4a9f2-206">真實世界實作會使用指數退避法，或指定的次數後放棄重試一次。</span><span class="sxs-lookup"><span data-stu-id="4a9f2-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a9f2-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="4a9f2-207">Additional resources</span></span>

* [<span data-ttu-id="4a9f2-208">JavaScript API 參考</span><span class="sxs-lookup"><span data-stu-id="4a9f2-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="4a9f2-209">JavaScript 教學課程</span><span class="sxs-lookup"><span data-stu-id="4a9f2-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4a9f2-210">WebPack 和 TypeScript 的教學課程</span><span class="sxs-lookup"><span data-stu-id="4a9f2-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="4a9f2-211">中樞</span><span class="sxs-lookup"><span data-stu-id="4a9f2-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4a9f2-212">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="4a9f2-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="4a9f2-213">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="4a9f2-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="4a9f2-214">跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="4a9f2-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="4a9f2-215">Azure SignalR Service 無伺服器檔</span><span class="sxs-lookup"><span data-stu-id="4a9f2-215">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
