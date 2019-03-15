---
title: ASP.NET Core SignalR JavaScript 用戶端
author: bradygaster
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/javascript-client
ms.openlocfilehash: a0980dca2eb8d483a9d9f1c5667fb74ee06364f0
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978338"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="e9960-103">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e9960-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="e9960-104">作者：[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e9960-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="e9960-105">ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e9960-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="e9960-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e9960-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="e9960-107">SignalR 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="e9960-107">Install the SignalR client package</span></span>

<span data-ttu-id="e9960-108">SignalR JavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)套件形式傳遞。</span><span class="sxs-lookup"><span data-stu-id="e9960-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="e9960-109">如果您使用 Visual Studio，可以在**Package Manager Console**的根資料夾中執行`npm install`。</span><span class="sxs-lookup"><span data-stu-id="e9960-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="e9960-110">如果您使用 Visual Studio Code，請從**整合式終端機**中執行命令。</span><span class="sxs-lookup"><span data-stu-id="e9960-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="e9960-111">npm 會將套件內容安裝在 *node_modules\\@aspnet\signalr\dist\browser* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e9960-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="e9960-112">在 *wwwroot\\lib* 資料夾下，建立名為 *signalr* 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="e9960-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="e9960-113">將 *signalr.js* 檔案複製到 *wwwroot\lib\signalr* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e9960-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="e9960-114">使用 SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e9960-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="e9960-115">在 `<script>` 元素中參考 SignalR JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e9960-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="e9960-116">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="e9960-116">Connect to a hub</span></span>

<span data-ttu-id="e9960-117">下列程式碼會建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="e9960-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="e9960-118">中樞的名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e9960-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="e9960-119">跨原始來源的連線</span><span class="sxs-lookup"><span data-stu-id="e9960-119">Cross-origin connections</span></span>

<span data-ttu-id="e9960-120">一般而言，瀏覽器會從與所要求之頁面相同的網域載入連線。</span><span class="sxs-lookup"><span data-stu-id="e9960-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="e9960-121">不過，有些情況下會需要連線到另一個網域的連線。</span><span class="sxs-lookup"><span data-stu-id="e9960-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="e9960-122">若要防止惡意網站從另一個網站讀取敏感性資料，預設會停用[跨來源連線](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="e9960-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="e9960-123">若要允許跨來源要求，請在 `Startup` 類別中啟用它。</span><span class="sxs-lookup"><span data-stu-id="e9960-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="e9960-124">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="e9960-124">Call hub methods from client</span></span>

<span data-ttu-id="e9960-125">JavaScript 用戶端會透過 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection) [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) 方法呼叫中樞的公用方法。</span><span class="sxs-lookup"><span data-stu-id="e9960-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="e9960-126">`invoke` 方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="e9960-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="e9960-127">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="e9960-127">The name of the hub method.</span></span> <span data-ttu-id="e9960-128">在下列範例中，在中樞的方法名稱是`SendMessage`。</span><span class="sxs-lookup"><span data-stu-id="e9960-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="e9960-129">中樞的方法中定義的任何引數。</span><span class="sxs-lookup"><span data-stu-id="e9960-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="e9960-130">在下列範例中，引數名稱是`message`。</span><span class="sxs-lookup"><span data-stu-id="e9960-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="e9960-131">範例程式碼會使用目前版本的 Internet Explorer 以外的所有主要瀏覽器都支援的箭號函式語法。</span><span class="sxs-lookup"><span data-stu-id="e9960-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="e9960-132">如果您使用 Azure SignalR 服務中*無伺服器模式*，您無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="e9960-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="e9960-133">如需詳細資訊，請參閱 < [SignalR 服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="e9960-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="e9960-134">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="e9960-134">Call client methods from hub</span></span>

<span data-ttu-id="e9960-135">若要從中樞接收訊息，請使用 `HubConnection` 的 [on](/javascript/api/%40aspnet/signalr/hubconnection#on) 方法來定義方法。</span><span class="sxs-lookup"><span data-stu-id="e9960-135">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="e9960-136">JavaScript 用戶端方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="e9960-136">The name of the JavaScript client method.</span></span> <span data-ttu-id="e9960-137">下列範例中，在方法名稱是`ReceiveMessage`。</span><span class="sxs-lookup"><span data-stu-id="e9960-137">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="e9960-138">中樞會將傳遞給方法的引數。</span><span class="sxs-lookup"><span data-stu-id="e9960-138">Arguments the hub passes to the method.</span></span> <span data-ttu-id="e9960-139">在下列範例中，引數值是`message`。</span><span class="sxs-lookup"><span data-stu-id="e9960-139">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="e9960-140">當伺服器端程式碼使用 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 方法來呼叫它時，會執行 `connection.on` 中的上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="e9960-140">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="e9960-141">SignalR 透過比對 `SendAsync` 與 `connection.on` 中定義的方法名稱與引數，來判斷要呼叫哪個用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="e9960-141">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="e9960-142">最佳做法是，在 `on` 之後呼叫 `HubConnection` 的 [start](/javascript/api/%40aspnet/signalr/hubconnection#start) 方法。</span><span class="sxs-lookup"><span data-stu-id="e9960-142">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="e9960-143">這麼做可確保您的處理常式會在註冊之後，才會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-143">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="e9960-144">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="e9960-144">Error handling and logging</span></span>

<span data-ttu-id="e9960-145">在 `start` 方法的結尾鏈結 `catch` 方法以處理用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="e9960-145">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="e9960-146">您可以使用 `console.error` 輸出錯誤至瀏覽器的主控台。</span><span class="sxs-lookup"><span data-stu-id="e9960-146">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="e9960-147">藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。</span><span class="sxs-lookup"><span data-stu-id="e9960-147">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="e9960-148">使用指定的記錄層級和更新版本，則會記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-148">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="e9960-149">可用的記錄層級如下所示：</span><span class="sxs-lookup"><span data-stu-id="e9960-149">Available log levels are as follows:</span></span>

* <span data-ttu-id="e9960-150">`signalR.LogLevel.Error` &ndash; 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-150">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="e9960-151">記錄檔`Error`只有訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-151">Logs `Error` messages only.</span></span>
* <span data-ttu-id="e9960-152">`signalR.LogLevel.Warning` &ndash; 可能的錯誤的相關警告訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-152">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="e9960-153">會記錄 `Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-153">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="e9960-154">`signalR.LogLevel.Information` &ndash; 沒有錯誤的狀態訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-154">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="e9960-155">會記錄 `Information`、`Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-155">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="e9960-156">`signalR.LogLevel.Trace` &ndash; 追蹤訊息。</span><span class="sxs-lookup"><span data-stu-id="e9960-156">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="e9960-157">記錄所有事件，包括中樞和用戶端之間傳輸的資料。</span><span class="sxs-lookup"><span data-stu-id="e9960-157">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="e9960-158">使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="e9960-158">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="e9960-159">訊息會記錄到瀏覽器主控台。</span><span class="sxs-lookup"><span data-stu-id="e9960-159">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="e9960-160">重新連線用戶端</span><span class="sxs-lookup"><span data-stu-id="e9960-160">Reconnect clients</span></span>

<span data-ttu-id="e9960-161">SignalR 的 JavaScript 用戶端不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="e9960-161">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="e9960-162">您必須撰寫程式碼會以手動方式重新連接您的用戶端。</span><span class="sxs-lookup"><span data-stu-id="e9960-162">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="e9960-163">下列程式碼示範一個典型的重新連線的方法：</span><span class="sxs-lookup"><span data-stu-id="e9960-163">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="e9960-164">函式 (在此情況下，`start`函式) 建立以啟動的連線。</span><span class="sxs-lookup"><span data-stu-id="e9960-164">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="e9960-165">呼叫`start`函式中的連線`onclose`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="e9960-165">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="e9960-166">真實世界實作會使用指數退避法，或指定的次數後放棄重試一次。</span><span class="sxs-lookup"><span data-stu-id="e9960-166">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e9960-167">其他資源</span><span class="sxs-lookup"><span data-stu-id="e9960-167">Additional resources</span></span>

* [<span data-ttu-id="e9960-168">JavaScript API 參考</span><span class="sxs-lookup"><span data-stu-id="e9960-168">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="e9960-169">JavaScript 教學課程</span><span class="sxs-lookup"><span data-stu-id="e9960-169">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e9960-170">WebPack 和 TypeScript 的教學課程</span><span class="sxs-lookup"><span data-stu-id="e9960-170">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="e9960-171">中樞</span><span class="sxs-lookup"><span data-stu-id="e9960-171">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e9960-172">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e9960-172">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e9960-173">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="e9960-173">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="e9960-174">跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="e9960-174">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="e9960-175">Azure SignalR 服務的無伺服器文件</span><span class="sxs-lookup"><span data-stu-id="e9960-175">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
