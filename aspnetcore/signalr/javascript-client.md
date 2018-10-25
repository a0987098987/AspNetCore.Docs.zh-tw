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
ms.sourcegitcommit: ce6b6792c650708e92cdea051a5d166c0708c7c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "46483045"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="18b58-103">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="18b58-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="18b58-104">作者：[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="18b58-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="18b58-105">ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。</span><span class="sxs-lookup"><span data-stu-id="18b58-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="18b58-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18b58-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="18b58-107">SignalR 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="18b58-107">Install the SignalR client package</span></span>

<span data-ttu-id="18b58-108">SignalR JavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)套件形式傳遞。</span><span class="sxs-lookup"><span data-stu-id="18b58-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="18b58-109">如果您使用 Visual Studio，可以在**Package Manager Console**的根資料夾中執行`npm install`。</span><span class="sxs-lookup"><span data-stu-id="18b58-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="18b58-110">如果您使用 Visual Studio Code，請從**整合式終端機**中執行命令。</span><span class="sxs-lookup"><span data-stu-id="18b58-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="18b58-111">npm 會將套件內容安裝在 *node_modules\\@aspnet\signalr\dist\browser* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="18b58-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="18b58-112">在 *wwwroot\\lib* 資料夾下，建立名為 *signalr* 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="18b58-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="18b58-113">將 *signalr.js* 檔案複製到 *wwwroot\lib\signalr* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="18b58-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="18b58-114">使用 SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="18b58-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="18b58-115">在 `<script>` 元素中參考 SignalR JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="18b58-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="18b58-116">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="18b58-116">Connect to a hub</span></span>

<span data-ttu-id="18b58-117">下列程式碼會建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="18b58-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="18b58-118">中樞的名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="18b58-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="18b58-119">跨原始來源的連線</span><span class="sxs-lookup"><span data-stu-id="18b58-119">Cross-origin connections</span></span>

<span data-ttu-id="18b58-120">一般而言，瀏覽器會從與所要求之頁面相同的網域載入連線。</span><span class="sxs-lookup"><span data-stu-id="18b58-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="18b58-121">不過，有些情況下會需要連線到另一個網域的連線。</span><span class="sxs-lookup"><span data-stu-id="18b58-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="18b58-122">若要防止惡意網站從另一個網站讀取敏感性資料，預設會停用[跨來源連線](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="18b58-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="18b58-123">若要允許跨來源要求，請在 `Startup` 類別中啟用它。</span><span class="sxs-lookup"><span data-stu-id="18b58-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="18b58-124">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="18b58-124">Call hub methods from client</span></span>

<span data-ttu-id="18b58-125">JavaScript 用戶端會透過 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection) [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) 方法呼叫中樞的公用方法。</span><span class="sxs-lookup"><span data-stu-id="18b58-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="18b58-126">`invoke` 方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="18b58-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="18b58-127">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="18b58-127">The name of the hub method.</span></span> <span data-ttu-id="18b58-128">在下列範例中，在中樞的方法名稱是`SendMessage`。</span><span class="sxs-lookup"><span data-stu-id="18b58-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="18b58-129">中樞的方法中定義的任何引數。</span><span class="sxs-lookup"><span data-stu-id="18b58-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="18b58-130">在下列範例中，引數名稱是`message`。</span><span class="sxs-lookup"><span data-stu-id="18b58-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="18b58-131">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="18b58-131">Call client methods from hub</span></span>

<span data-ttu-id="18b58-132">若要從中樞接收訊息，請使用 `HubConnection` 的 [on](/javascript/api/%40aspnet/signalr/hubconnection#on) 方法來定義方法。</span><span class="sxs-lookup"><span data-stu-id="18b58-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="18b58-133">JavaScript 用戶端方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="18b58-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="18b58-134">下列範例中，在方法名稱是`ReceiveMessage`。</span><span class="sxs-lookup"><span data-stu-id="18b58-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="18b58-135">中樞會將傳遞給方法的引數。</span><span class="sxs-lookup"><span data-stu-id="18b58-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="18b58-136">在下列範例中，引數值是`message`。</span><span class="sxs-lookup"><span data-stu-id="18b58-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="18b58-137">當伺服器端程式碼使用 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 方法來呼叫它時，會執行 `connection.on` 中的上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="18b58-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="18b58-138">SignalR 透過比對 `SendAsync` 與 `connection.on` 中定義的方法名稱與引數，來判斷要呼叫哪個用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="18b58-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="18b58-139">最佳做法是，在 `on` 之後呼叫 `HubConnection` 的 [start](/javascript/api/%40aspnet/signalr/hubconnection#start) 方法。</span><span class="sxs-lookup"><span data-stu-id="18b58-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="18b58-140">這麼做可確保您的處理常式會在註冊之後，才會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="18b58-141">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="18b58-141">Error handling and logging</span></span>

<span data-ttu-id="18b58-142">在 `start` 方法的結尾鏈結 `catch` 方法以處理用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="18b58-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="18b58-143">您可以使用 `console.error` 輸出錯誤至瀏覽器的主控台。</span><span class="sxs-lookup"><span data-stu-id="18b58-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="18b58-144">藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。</span><span class="sxs-lookup"><span data-stu-id="18b58-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="18b58-145">使用指定的記錄層級和更新版本，則會記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="18b58-146">可用的記錄層級如下所示：</span><span class="sxs-lookup"><span data-stu-id="18b58-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="18b58-147">`signalR.LogLevel.Error` &ndash; 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="18b58-148">記錄檔`Error`只有訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="18b58-149">`signalR.LogLevel.Warning` &ndash; 可能的錯誤的相關警告訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="18b58-150">會記錄 `Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="18b58-151">`signalR.LogLevel.Information` &ndash; 沒有錯誤的狀態訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="18b58-152">會記錄 `Information`、`Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="18b58-153">`signalR.LogLevel.Trace` &ndash; 追蹤訊息。</span><span class="sxs-lookup"><span data-stu-id="18b58-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="18b58-154">記錄所有事件，包括中樞和用戶端之間傳輸的資料。</span><span class="sxs-lookup"><span data-stu-id="18b58-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="18b58-155">使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="18b58-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="18b58-156">訊息會記錄到瀏覽器主控台。</span><span class="sxs-lookup"><span data-stu-id="18b58-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="additional-resources"></a><span data-ttu-id="18b58-157">其他資源</span><span class="sxs-lookup"><span data-stu-id="18b58-157">Additional resources</span></span>

* [<span data-ttu-id="18b58-158">JavaScript API 參考</span><span class="sxs-lookup"><span data-stu-id="18b58-158">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="18b58-159">中樞</span><span class="sxs-lookup"><span data-stu-id="18b58-159">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="18b58-160">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="18b58-160">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="18b58-161">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="18b58-161">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="18b58-162">啟用 ASP.NET Core 中的跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="18b58-162">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
