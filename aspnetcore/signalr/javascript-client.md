---
<span data-ttu-id="f203d-101">標題： ' ASP.NET Core SignalR javascript 用戶端 ' 作者：描述： ' ASP.NET Core SignalR JavaScript 用戶端的總覽 '。</span><span class="sxs-lookup"><span data-stu-id="f203d-101">title: 'ASP.NET Core SignalR JavaScript client' author: description: 'Overview of ASP.NET Core SignalR JavaScript client.'</span></span>
<span data-ttu-id="f203d-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f203d-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f203d-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f203d-103">'Blazor'</span></span>
- <span data-ttu-id="f203d-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f203d-104">'Identity'</span></span>
- <span data-ttu-id="f203d-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f203d-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="f203d-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f203d-106">'Razor'</span></span>
- <span data-ttu-id="f203d-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f203d-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="f203d-108">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="f203d-108">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="f203d-109">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f203d-109">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f203d-110">ASP.NET Core 的 SignalR JavaScript 用戶端程式庫可讓開發人員呼叫伺服器端中樞程式碼。</span><span class="sxs-lookup"><span data-stu-id="f203d-110">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="f203d-111">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="f203d-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="f203d-112">安裝 SignalR 用戶端套件</span><span class="sxs-lookup"><span data-stu-id="f203d-112">Install the SignalR client package</span></span>

<span data-ttu-id="f203d-113">SignalRJavaScript 用戶端程式庫會以[npm](https://www.npmjs.com/)封裝的形式提供。</span><span class="sxs-lookup"><span data-stu-id="f203d-113">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="f203d-114">下列各節概述安裝用戶端程式庫的不同方式。</span><span class="sxs-lookup"><span data-stu-id="f203d-114">The following sections outline different ways to install the client library.</span></span>

### <a name="install-with-npm"></a><span data-ttu-id="f203d-115">使用 npm 安裝</span><span class="sxs-lookup"><span data-stu-id="f203d-115">Install with npm</span></span>

<span data-ttu-id="f203d-116">如果使用 Visual Studio，請在根資料夾中，從 [**套件管理員主控台**] 執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="f203d-116">If using Visual Studio, run the following commands from **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="f203d-117">針對 Visual Studio Code，請從**整合式終端**機執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="f203d-117">For Visual Studio Code, run the following commands from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

```bash
npm init -y
npm install @microsoft/signalr
```

<span data-ttu-id="f203d-118">npm 會在\*node_modules \\ @microsoft\signalr\dist\browser \*資料夾中安裝封裝內容。</span><span class="sxs-lookup"><span data-stu-id="f203d-118">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="f203d-119">在*wwwroot \\ lib*資料夾底下，建立名為*signalr*的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="f203d-119">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="f203d-120">將*signalr*複製到*wwwroot\lib\signalr*資料夾。</span><span class="sxs-lookup"><span data-stu-id="f203d-120">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```bash
npm init -y
npm install @aspnet/signalr
```

<span data-ttu-id="f203d-121">npm 會在\*node_modules \\ @aspnet\signalr\dist\browser \*資料夾中安裝封裝內容。</span><span class="sxs-lookup"><span data-stu-id="f203d-121">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="f203d-122">在*wwwroot \\ lib*資料夾底下，建立名為*signalr*的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="f203d-122">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="f203d-123">將*signalr*複製到*wwwroot\lib\signalr*資料夾。</span><span class="sxs-lookup"><span data-stu-id="f203d-123">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

<span data-ttu-id="f203d-124">參考 SignalR 元素中的 JavaScript 用戶端 `<script>` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-124">Reference the SignalR JavaScript client in the `<script>` element.</span></span> <span data-ttu-id="f203d-125">例如：</span><span class="sxs-lookup"><span data-stu-id="f203d-125">For example:</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

### <a name="use-a-content-delivery-network-cdn"></a><span data-ttu-id="f203d-126">使用內容傳遞網路（CDN）</span><span class="sxs-lookup"><span data-stu-id="f203d-126">Use a Content Delivery Network (CDN)</span></span>

<span data-ttu-id="f203d-127">若要使用不含 npm 必要條件的用戶端程式庫，請參考 CDN 主控的用戶端程式庫複本。</span><span class="sxs-lookup"><span data-stu-id="f203d-127">To use the client library without the npm prerequisite, reference a CDN-hosted copy of the client library.</span></span> <span data-ttu-id="f203d-128">例如：</span><span class="sxs-lookup"><span data-stu-id="f203d-128">For example:</span></span>

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/3.1.3/signalr.min.js"></script>
```

<span data-ttu-id="f203d-129">用戶端程式庫可在下列 Cdn 中取得：</span><span class="sxs-lookup"><span data-stu-id="f203d-129">The client library is available on the following CDNs:</span></span>

::: moniker range=">= aspnetcore-3.0"

* [<span data-ttu-id="f203d-130">cdnjs</span><span class="sxs-lookup"><span data-stu-id="f203d-130">cdnjs</span></span>](https://cdnjs.com/libraries/microsoft-signalr)
* [<span data-ttu-id="f203d-131">jsDelivr</span><span class="sxs-lookup"><span data-stu-id="f203d-131">jsDelivr</span></span>](https://www.jsdelivr.com/package/npm/@microsoft/signalr)
* [<span data-ttu-id="f203d-132">unpkg</span><span class="sxs-lookup"><span data-stu-id="f203d-132">unpkg</span></span>](https://unpkg.com/@microsoft/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="f203d-133">cdnjs</span><span class="sxs-lookup"><span data-stu-id="f203d-133">cdnjs</span></span>](https://cdnjs.com/libraries/aspnet-signalr)
* [<span data-ttu-id="f203d-134">jsDelivr</span><span class="sxs-lookup"><span data-stu-id="f203d-134">jsDelivr</span></span>](https://www.jsdelivr.com/package/npm/@aspnet/signalr)
* [<span data-ttu-id="f203d-135">unpkg</span><span class="sxs-lookup"><span data-stu-id="f203d-135">unpkg</span></span>](https://unpkg.com/@aspnet/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

### <a name="install-with-libman"></a><span data-ttu-id="f203d-136">使用 LibMan 安裝</span><span class="sxs-lookup"><span data-stu-id="f203d-136">Install with LibMan</span></span>

<span data-ttu-id="f203d-137">[LibMan](xref:client-side/libman/index)可以用來從 CDN 裝載的用戶端程式庫安裝特定的用戶端程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="f203d-137">[LibMan](xref:client-side/libman/index) can be used to install specific client library files from the CDN-hosted client library.</span></span> <span data-ttu-id="f203d-138">例如，只將縮減 JavaScript 檔案新增至專案。</span><span class="sxs-lookup"><span data-stu-id="f203d-138">For example, only add the minified JavaScript file to the project.</span></span> <span data-ttu-id="f203d-139">如需該方法的詳細資訊，請參閱[新增 SignalR 用戶端程式庫](xref:tutorials/signalr#add-the-signalr-client-library)。</span><span class="sxs-lookup"><span data-stu-id="f203d-139">For details on that approach, see [Add the SignalR client library](xref:tutorials/signalr#add-the-signalr-client-library).</span></span>

## <a name="connect-to-a-hub"></a><span data-ttu-id="f203d-140">連接到中樞</span><span class="sxs-lookup"><span data-stu-id="f203d-140">Connect to a hub</span></span>

<span data-ttu-id="f203d-141">下列程式碼會建立並啟動連接。</span><span class="sxs-lookup"><span data-stu-id="f203d-141">The following code creates and starts a connection.</span></span> <span data-ttu-id="f203d-142">中樞的名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="f203d-142">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,28-51)]

### <a name="cross-origin-connections"></a><span data-ttu-id="f203d-143">跨原始來源連接</span><span class="sxs-lookup"><span data-stu-id="f203d-143">Cross-origin connections</span></span>

<span data-ttu-id="f203d-144">通常，瀏覽器會載入與所要求頁面來自相同網域的連接。</span><span class="sxs-lookup"><span data-stu-id="f203d-144">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="f203d-145">不過，在某些情況下需要連接到另一個網域。</span><span class="sxs-lookup"><span data-stu-id="f203d-145">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="f203d-146">為了防止惡意網站從另一個網站讀取敏感性資料，預設會停用[跨原始來源](xref:security/cors)連線。</span><span class="sxs-lookup"><span data-stu-id="f203d-146">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="f203d-147">若要允許跨原始來源要求，請在類別中加以啟用 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-147">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="f203d-148">從用戶端呼叫中樞方法</span><span class="sxs-lookup"><span data-stu-id="f203d-148">Call hub methods from client</span></span>

<span data-ttu-id="f203d-149">JavaScript 用戶端會透過[HubConnection](/javascript/api/%40aspnet/signalr/hubconnection)的[invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke)方法，在中樞上呼叫公用方法。</span><span class="sxs-lookup"><span data-stu-id="f203d-149">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="f203d-150">`invoke`方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="f203d-150">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="f203d-151">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f203d-151">The name of the hub method.</span></span> <span data-ttu-id="f203d-152">在下列範例中，中樞上的方法名稱是 `SendMessage` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-152">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="f203d-153">中樞方法中定義的任何引數。</span><span class="sxs-lookup"><span data-stu-id="f203d-153">Any arguments defined in the hub method.</span></span> <span data-ttu-id="f203d-154">在下列範例中，引數名稱是 `message` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-154">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="f203d-155">範例程式碼使用箭號函式語法，在 Internet Explorer 以外的所有主要瀏覽器的目前版本中都受到支援。</span><span class="sxs-lookup"><span data-stu-id="f203d-155">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="f203d-156">如果您是 SignalR 在*無伺服器模式*中使用 Azure 服務，則無法從用戶端呼叫中樞方法。</span><span class="sxs-lookup"><span data-stu-id="f203d-156">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="f203d-157">如需詳細資訊，請參閱[ SignalR 服務檔](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="f203d-157">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="f203d-158">方法會傳回 `invoke` JavaScript[承諾](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)。</span><span class="sxs-lookup"><span data-stu-id="f203d-158">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="f203d-159">`Promise`當伺服器上的方法傳回時，會使用傳回值（如果有的話）來解析。</span><span class="sxs-lookup"><span data-stu-id="f203d-159">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="f203d-160">如果伺服器上的方法擲回錯誤，則 `Promise` 會拒絕並顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-160">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="f203d-161">`then` `catch` 在本身上使用和方法 `Promise` 來處理這些案例（或 `await` 語法）。</span><span class="sxs-lookup"><span data-stu-id="f203d-161">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="f203d-162">方法會傳回 `send` JavaScript `Promise` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-162">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="f203d-163">`Promise`當訊息已傳送到伺服器時，就會解決。</span><span class="sxs-lookup"><span data-stu-id="f203d-163">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="f203d-164">如果傳送訊息時發生錯誤，則 `Promise` 會拒絕，並顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-164">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="f203d-165">`then` `catch` 在本身上使用和方法 `Promise` 來處理這些案例（或 `await` 語法）。</span><span class="sxs-lookup"><span data-stu-id="f203d-165">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="f203d-166">使用 `send` 並不會等到伺服器收到訊息為止。</span><span class="sxs-lookup"><span data-stu-id="f203d-166">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="f203d-167">因此，不可能從伺服器傳回資料或錯誤。</span><span class="sxs-lookup"><span data-stu-id="f203d-167">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="f203d-168">從中樞呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="f203d-168">Call client methods from hub</span></span>

<span data-ttu-id="f203d-169">若要從中樞接收訊息，請使用的[on](/javascript/api/%40aspnet/signalr/hubconnection#on)方法來定義方法 `HubConnection` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-169">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="f203d-170">JavaScript 用戶端方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f203d-170">The name of the JavaScript client method.</span></span> <span data-ttu-id="f203d-171">在下列範例中，方法名稱是 `ReceiveMessage` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-171">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="f203d-172">中樞傳遞至方法的引數。</span><span class="sxs-lookup"><span data-stu-id="f203d-172">Arguments the hub passes to the method.</span></span> <span data-ttu-id="f203d-173">在下列範例中，引數值為 `message` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-173">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="f203d-174">`connection.on`當伺服器端程式碼使用[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)方法呼叫它時，中的上述程式碼就會執行。</span><span class="sxs-lookup"><span data-stu-id="f203d-174">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="f203d-175">藉由比對和中定義的方法名稱和引數，來判斷要呼叫的用戶端方法 `SendAsync` `connection.on` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-175"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="f203d-176">最佳做法是在之後呼叫[start](/javascript/api/%40aspnet/signalr/hubconnection#start)方法 `HubConnection` `on` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-176">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="f203d-177">這麼做可確保您的處理常式會在收到任何訊息之前註冊。</span><span class="sxs-lookup"><span data-stu-id="f203d-177">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="f203d-178">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="f203d-178">Error handling and logging</span></span>

<span data-ttu-id="f203d-179">將 `catch` 方法連結至方法的結尾 `start` ，以處理用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="f203d-179">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="f203d-180">使用將 `console.error` 錯誤輸出至瀏覽器的主控台。</span><span class="sxs-lookup"><span data-stu-id="f203d-180">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=50)]

<span data-ttu-id="f203d-181">設定用戶端記錄追蹤，方法是在建立連接時傳遞記錄器和事件種類來記錄。</span><span class="sxs-lookup"><span data-stu-id="f203d-181">Set up client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="f203d-182">訊息會記錄到指定的記錄層級和更新版本。</span><span class="sxs-lookup"><span data-stu-id="f203d-182">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="f203d-183">可用的記錄層級如下：</span><span class="sxs-lookup"><span data-stu-id="f203d-183">Available log levels are as follows:</span></span>

* <span data-ttu-id="f203d-184">`signalR.LogLevel.Error`&ndash;錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-184">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="f203d-185">`Error`僅記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-185">Logs `Error` messages only.</span></span>
* <span data-ttu-id="f203d-186">`signalR.LogLevel.Warning`&ndash;可能發生錯誤的警告訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-186">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="f203d-187">記錄檔 `Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-187">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="f203d-188">`signalR.LogLevel.Information`&ndash;沒有錯誤的狀態訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-188">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="f203d-189">記錄 `Information` 、 `Warning` 和 `Error` 訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-189">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="f203d-190">`signalR.LogLevel.Trace`&ndash;追蹤訊息。</span><span class="sxs-lookup"><span data-stu-id="f203d-190">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="f203d-191">記錄所有專案，包括中樞和用戶端之間傳輸的資料。</span><span class="sxs-lookup"><span data-stu-id="f203d-191">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="f203d-192">在[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上使用[configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法來設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="f203d-192">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="f203d-193">訊息會記錄到瀏覽器主控台。</span><span class="sxs-lookup"><span data-stu-id="f203d-193">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="f203d-194">重新連線用戶端</span><span class="sxs-lookup"><span data-stu-id="f203d-194">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="f203d-195">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="f203d-195">Automatically reconnect</span></span>

<span data-ttu-id="f203d-196">的 JavaScript 用戶端 SignalR 可以設定為使用 `withAutomaticReconnect` [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上的方法自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="f203d-196">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="f203d-197">根據預設，它不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="f203d-197">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="f203d-198">如果沒有任何參數， `withAutomaticReconnect()` 則會先將用戶端設定為等待0、2、10和30秒，再嘗試重新連線嘗試，並在四次失敗嘗試之後停止。</span><span class="sxs-lookup"><span data-stu-id="f203d-198">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="f203d-199">開始任何重新連線嘗試之前， `HubConnection` 會轉換成 `HubConnectionState.Reconnecting` 狀態並引發其 `onreconnecting` 回呼，而不是轉換成 `Disconnected` 狀態並觸發其 `onclose` 回呼，例如 `HubConnection` 未設定自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="f203d-199">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="f203d-200">這會讓使用者有機會警告連接已遺失，並停用 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="f203d-200">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting(error => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="f203d-201">如果用戶端在前四次嘗試中成功重新連接， `HubConnection` 將會轉換回 `Connected` 狀態並引發其 `onreconnected` 回呼。</span><span class="sxs-lookup"><span data-stu-id="f203d-201">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="f203d-202">這讓您有機會通知使用者已重新建立連接。</span><span class="sxs-lookup"><span data-stu-id="f203d-202">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="f203d-203">由於連接看起來就是伺服器的全新連線，因此 `connectionId` 會提供新的給 `onreconnected` 回呼。</span><span class="sxs-lookup"><span data-stu-id="f203d-203">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="f203d-204">`onreconnected` `connectionId` 如果 `HubConnection` 設定為[略過協商](xref:signalr/configuration#configure-client-options)，回呼的參數將會是未定義的。</span><span class="sxs-lookup"><span data-stu-id="f203d-204">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected(connectionId => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="f203d-205">`withAutomaticReconnect()`不會將設定 `HubConnection` 為重試初始啟動失敗，因此必須手動處理啟動失敗：</span><span class="sxs-lookup"><span data-stu-id="f203d-205">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="f203d-206">如果用戶端在前四次嘗試中未成功重新連接， `HubConnection` 將會轉換成 `Disconnected` 狀態並引發其[onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose)回呼。</span><span class="sxs-lookup"><span data-stu-id="f203d-206">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="f203d-207">這讓您有機會通知使用者連線已永久遺失，並建議重新整理頁面：</span><span class="sxs-lookup"><span data-stu-id="f203d-207">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose(error => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="f203d-208">為了在中斷連線或變更重新連線時間之前設定自訂的重新連線嘗試次數，會 `withAutomaticReconnect` 接受代表在開始每次重新連線嘗試之前等待的延遲（以毫秒為單位）的數位陣列。</span><span class="sxs-lookup"><span data-stu-id="f203d-208">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="f203d-209">上述範例會將設定 `HubConnection` 為，以便在連接中斷之後立即開始嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="f203d-209">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="f203d-210">這也適用于預設設定。</span><span class="sxs-lookup"><span data-stu-id="f203d-210">This is also true for the default configuration.</span></span>

<span data-ttu-id="f203d-211">如果第一次重新連線嘗試失敗，第二次重新連線嘗試也會立即開始，而不是等待2秒，就像在預設設定中一樣。</span><span class="sxs-lookup"><span data-stu-id="f203d-211">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="f203d-212">如果第二次重新連線嘗試失敗，第三次重新連線嘗試會在10秒內啟動，這再次是預設設定。</span><span class="sxs-lookup"><span data-stu-id="f203d-212">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="f203d-213">然後，自訂行為會在第三次重新連線嘗試失敗之後停止，而不是在另一個30秒嘗試重新連線嘗試，就會再次從預設行為分歧，而不是在預設設定中再試一次。</span><span class="sxs-lookup"><span data-stu-id="f203d-213">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="f203d-214">如果您想要更充分掌控自動重新連線嘗試的時間和數目，則會 `withAutomaticReconnect` 接受一個物件 `IRetryPolicy` 來執行介面，此介面具有名為的單一方法 `nextRetryDelayInMilliseconds` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-214">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="f203d-215">`nextRetryDelayInMilliseconds`接受具有類型的單一引數 `RetryContext` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-215">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="f203d-216">`RetryContext`有三個屬性： `previousRetryCount` 、 `elapsedMilliseconds` 和 `retryReason` `number` 分別為、 `number` 和 `Error` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-216">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="f203d-217">第一次重新連線嘗試之前， `previousRetryCount` 和都 `elapsedMilliseconds` 是零，而且 `retryReason` 會是造成連接遺失的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f203d-217">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="f203d-218">每次重試失敗之後，將會 `previousRetryCount` 遞增一次， `elapsedMilliseconds` 將會更新，以反映到目前為止的重新連線時間量（以毫秒為單位），而 `retryReason` 會是導致上次重新連線嘗試失敗的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f203d-218">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="f203d-219">`nextRetryDelayInMilliseconds`必須傳回數位，代表下一次重新連接嘗試之前要等候的毫秒數，或者 `null` ，如果應該停止重新連接，則為 `HubConnection` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-219">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="f203d-220">或者，您可以撰寫程式碼，以手動方式重新連線用戶端，如[手動重新](#manually-reconnect)連線中所示。</span><span class="sxs-lookup"><span data-stu-id="f203d-220">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="f203d-221">手動重新連線</span><span class="sxs-lookup"><span data-stu-id="f203d-221">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="f203d-222">在3.0 之前，的 JavaScript 用戶端 SignalR 不會自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="f203d-222">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="f203d-223">您必須撰寫程式碼，以手動方式重新連線用戶端。</span><span class="sxs-lookup"><span data-stu-id="f203d-223">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="f203d-224">下列程式碼示範一般手動重新連接方法：</span><span class="sxs-lookup"><span data-stu-id="f203d-224">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="f203d-225">建立函式（在此案例中為函式 `start` ）來啟動連接。</span><span class="sxs-lookup"><span data-stu-id="f203d-225">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="f203d-226">呼叫 `start` 連接的事件處理常式中的函式 `onclose` 。</span><span class="sxs-lookup"><span data-stu-id="f203d-226">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="f203d-227">實際執行會使用指數退避，或在放棄之前重試指定的次數。</span><span class="sxs-lookup"><span data-stu-id="f203d-227">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f203d-228">其他資源</span><span class="sxs-lookup"><span data-stu-id="f203d-228">Additional resources</span></span>

* [<span data-ttu-id="f203d-229">JavaScript API 參考</span><span class="sxs-lookup"><span data-stu-id="f203d-229">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="f203d-230">JavaScript 教學課程</span><span class="sxs-lookup"><span data-stu-id="f203d-230">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="f203d-231">WebPack 和 TypeScript 教學課程</span><span class="sxs-lookup"><span data-stu-id="f203d-231">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="f203d-232">中樞</span><span class="sxs-lookup"><span data-stu-id="f203d-232">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f203d-233">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="f203d-233">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="f203d-234">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="f203d-234">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="f203d-235">跨原始來源要求（CORS）</span><span class="sxs-lookup"><span data-stu-id="f203d-235">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="f203d-236">[Azure SignalR 服務無伺服器檔](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="f203d-236">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
