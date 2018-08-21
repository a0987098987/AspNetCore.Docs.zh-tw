---
title: ASP.NET Core SignalR JavaScript 用戶端
author: tdykstra
description: ASP.NET Core SignalR JavaScript 用戶端的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095420"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="47deb-103">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="47deb-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="47deb-104">作者：[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="47deb-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="47deb-105">ASP.NET Core SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞的程式碼。</span><span class="sxs-lookup"><span data-stu-id="47deb-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="47deb-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47deb-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="47deb-107">SignalR 用戶端封裝安裝</span><span class="sxs-lookup"><span data-stu-id="47deb-107">Install the SignalR client package</span></span>

<span data-ttu-id="47deb-108">SignalR JavaScript 用戶端程式庫會以傳遞[npm](https://www.npmjs.com/)封裝。</span><span class="sxs-lookup"><span data-stu-id="47deb-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="47deb-109">如果您使用 Visual Studio，執行`npm install`從**Package Manager Console**在根資料夾中。</span><span class="sxs-lookup"><span data-stu-id="47deb-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="47deb-110">適用於 Visual Studio Code，請從執行命令**整合式終端機**。</span><span class="sxs-lookup"><span data-stu-id="47deb-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="47deb-111">Npm 安裝的套件內容*node_modules\\@aspnet\signalr\dist\browser* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47deb-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="47deb-112">建立新的資料夾，名為*signalr*下方*wwwroot\\lib*資料夾。</span><span class="sxs-lookup"><span data-stu-id="47deb-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="47deb-113">複製*signalr.js*的檔案 *wwwroot\lib\signalr* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47deb-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="47deb-114">使用 SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="47deb-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="47deb-115">參考中的 SignalR JavaScript 用戶端`<script>`項目。</span><span class="sxs-lookup"><span data-stu-id="47deb-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="47deb-116">連線至中樞</span><span class="sxs-lookup"><span data-stu-id="47deb-116">Connect to a hub</span></span>

<span data-ttu-id="47deb-117">下列程式碼會建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="47deb-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="47deb-118">中樞的名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="47deb-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="47deb-119">跨原始來源的連線</span><span class="sxs-lookup"><span data-stu-id="47deb-119">Cross-origin connections</span></span>

<span data-ttu-id="47deb-120">一般而言，瀏覽器會載入連線從與要求的頁面相同的網域。</span><span class="sxs-lookup"><span data-stu-id="47deb-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="47deb-121">不過，有些情況下連接到另一個網域時所需。</span><span class="sxs-lookup"><span data-stu-id="47deb-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="47deb-122">若要防止惡意網站從另一個網站讀取敏感性資料[跨原始來源連線](xref:security/cors)預設會停用。</span><span class="sxs-lookup"><span data-stu-id="47deb-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="47deb-123">若要允許跨原始來源要求，讓它在`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="47deb-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="47deb-124">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="47deb-124">Call hub methods from client</span></span>

<span data-ttu-id="47deb-125">JavaScript 用戶端呼叫的公用方法上的中樞使用`connection.invoke`。</span><span class="sxs-lookup"><span data-stu-id="47deb-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="47deb-126">`invoke`方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="47deb-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="47deb-127">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="47deb-127">The name of the hub method.</span></span> <span data-ttu-id="47deb-128">下列範例中，在中樞名稱是`SendMessage`。</span><span class="sxs-lookup"><span data-stu-id="47deb-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="47deb-129">中樞的方法中定義的任何引數。</span><span class="sxs-lookup"><span data-stu-id="47deb-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="47deb-130">在下列範例中，引數名稱是`message`。</span><span class="sxs-lookup"><span data-stu-id="47deb-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="47deb-131">用戶端方法呼叫來自中樞</span><span class="sxs-lookup"><span data-stu-id="47deb-131">Call client methods from hub</span></span>

<span data-ttu-id="47deb-132">若要從中樞接收訊息，定義方法，使用`connection.on`方法。</span><span class="sxs-lookup"><span data-stu-id="47deb-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="47deb-133">JavaScript 用戶端方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="47deb-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="47deb-134">下列範例中，在方法名稱是`ReceiveMessage`。</span><span class="sxs-lookup"><span data-stu-id="47deb-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="47deb-135">中樞會將傳遞給方法的引數。</span><span class="sxs-lookup"><span data-stu-id="47deb-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="47deb-136">在下列範例中，引數值是`message`。</span><span class="sxs-lookup"><span data-stu-id="47deb-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="47deb-137">在上述程式碼`connection.on`會在伺服器端程式碼會呼叫使用時執行`SendAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="47deb-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="47deb-138">SignalR 判斷用戶端来呼叫的方法比對方法的名稱，並定義中的引數`SendAsync`和`connection.on`。</span><span class="sxs-lookup"><span data-stu-id="47deb-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="47deb-139">最佳做法，呼叫`connection.start`之後`connection.on`因此會收到任何訊息之前，會註冊您的處理常式。</span><span class="sxs-lookup"><span data-stu-id="47deb-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="47deb-140">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="47deb-140">Error handling and logging</span></span>

<span data-ttu-id="47deb-141">鏈結`catch`方法的結尾`connection.start`方法來處理用戶端的錯誤。</span><span class="sxs-lookup"><span data-stu-id="47deb-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="47deb-142">使用`console.error`瀏覽器的主控台輸出錯誤。</span><span class="sxs-lookup"><span data-stu-id="47deb-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="47deb-143">藉由傳遞要在進行連接時，記錄的記錄器和事件類型的安裝程式用戶端記錄追蹤。</span><span class="sxs-lookup"><span data-stu-id="47deb-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="47deb-144">使用指定的記錄層級和更新版本，則會記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="47deb-145">可用的記錄層級如下所示：</span><span class="sxs-lookup"><span data-stu-id="47deb-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="47deb-146">`signalR.LogLevel.Error` ： 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="47deb-147">記錄檔`Error`只有訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="47deb-148">`signalR.LogLevel.Warning` ： 可能的錯誤警告訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="47deb-149">記錄檔`Warning`，和`Error`訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="47deb-150">`signalR.LogLevel.Information` ： 不會出現錯誤狀態訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="47deb-151">記錄檔`Information`， `Warning`，和`Error`訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="47deb-152">`signalR.LogLevel.Trace` ： 追蹤訊息。</span><span class="sxs-lookup"><span data-stu-id="47deb-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="47deb-153">記錄所有事件，包括中樞和用戶端之間傳輸的資料。</span><span class="sxs-lookup"><span data-stu-id="47deb-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="47deb-154">使用`configureLogging`方法`HubConnectionBuilder`設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="47deb-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="47deb-155">訊息會記錄到瀏覽器主控台。</span><span class="sxs-lookup"><span data-stu-id="47deb-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="47deb-156">相關資源</span><span class="sxs-lookup"><span data-stu-id="47deb-156">Related resources</span></span>

* [<span data-ttu-id="47deb-157">中樞</span><span class="sxs-lookup"><span data-stu-id="47deb-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="47deb-158">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="47deb-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="47deb-159">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="47deb-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="47deb-160">啟用 ASP.NET Core 中的跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="47deb-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
