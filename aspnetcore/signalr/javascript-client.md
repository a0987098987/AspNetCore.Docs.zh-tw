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
ms.openlocfilehash: 43b2cacf9f415ec422a00b28246f30c8ad74de29
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440853"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a><span data-ttu-id="08354-103">ASP.NET核心SignalRJavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="08354-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="08354-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="08354-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="08354-105">ASP.NET核心SignalRJavaScript 用戶端庫使開發人員能夠調用伺服器端集線器代碼。</span><span class="sxs-lookup"><span data-stu-id="08354-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="08354-106">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="08354-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-client-package"></a><span data-ttu-id="08354-107">安裝SignalR用戶端套件</span><span class="sxs-lookup"><span data-stu-id="08354-107">Install the SignalR client package</span></span>

<span data-ttu-id="08354-108">JavaScriptSignalR用戶端庫作為[npm](https://www.npmjs.com/)包提供。</span><span class="sxs-lookup"><span data-stu-id="08354-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="08354-109">以下各節概述了安裝用戶端庫的不同方法。</span><span class="sxs-lookup"><span data-stu-id="08354-109">The following sections outline different ways to install the client library.</span></span>

### <a name="install-with-npm"></a><span data-ttu-id="08354-110">使用 npm 安裝</span><span class="sxs-lookup"><span data-stu-id="08354-110">Install with npm</span></span>

<span data-ttu-id="08354-111">如果使用 Visual Studio,則在根資料夾中運行**包管理器主控台**中的以下命令。</span><span class="sxs-lookup"><span data-stu-id="08354-111">If using Visual Studio, run the following commands from **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="08354-112">對於可視化工作室代碼,請從**整合終端**運行以下命令。</span><span class="sxs-lookup"><span data-stu-id="08354-112">For Visual Studio Code, run the following commands from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

```bash
npm init -y
npm install @microsoft/signalr
```

<span data-ttu-id="08354-113">npm 在*node_modules\\*資料夾中安裝包內容。</span><span class="sxs-lookup"><span data-stu-id="08354-113">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="08354-114">在*wwwroot\\lib*資料夾下創建名為*信號器*的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="08354-114">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="08354-115">將*Signalr.js*檔案複製到*wwwroot_lib_signalr*資料夾。</span><span class="sxs-lookup"><span data-stu-id="08354-115">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```bash
npm init -y
npm install @aspnet/signalr
```

<span data-ttu-id="08354-116">npm 在*node_modules\\*資料夾中安裝包內容。</span><span class="sxs-lookup"><span data-stu-id="08354-116">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="08354-117">在*wwwroot\\lib*資料夾下創建名為*信號器*的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="08354-117">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="08354-118">將*Signalr.js*檔案複製到*wwwroot_lib_signalr*資料夾。</span><span class="sxs-lookup"><span data-stu-id="08354-118">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

<span data-ttu-id="08354-119">引用`<script>`SignalR元素 中的 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="08354-119">Reference the SignalR JavaScript client in the `<script>` element.</span></span> <span data-ttu-id="08354-120">例如：</span><span class="sxs-lookup"><span data-stu-id="08354-120">For example:</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

### <a name="use-a-content-delivery-network-cdn"></a><span data-ttu-id="08354-121">使用內容提供網路 (CDN)</span><span class="sxs-lookup"><span data-stu-id="08354-121">Use a Content Delivery Network (CDN)</span></span>

<span data-ttu-id="08354-122">要使用沒有 npm 先決條件的用戶端庫,請引用用戶端庫的 CDN 託管副本。</span><span class="sxs-lookup"><span data-stu-id="08354-122">To use the client library without the npm prerequisite, reference a CDN-hosted copy of the client library.</span></span> <span data-ttu-id="08354-123">例如：</span><span class="sxs-lookup"><span data-stu-id="08354-123">For example:</span></span>

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/3.1.3/signalr.min.js"></script>
```

<span data-ttu-id="08354-124">用戶端函式庫在以下 CDN 上可用:</span><span class="sxs-lookup"><span data-stu-id="08354-124">The client library is available on the following CDNs:</span></span>

::: moniker range=">= aspnetcore-3.0"

* [<span data-ttu-id="08354-125">恩傑斯</span><span class="sxs-lookup"><span data-stu-id="08354-125">cdnjs</span></span>](https://cdnjs.com/libraries/microsoft-signalr)
* [<span data-ttu-id="08354-126">jsDelivr</span><span class="sxs-lookup"><span data-stu-id="08354-126">jsDelivr</span></span>](https://www.jsdelivr.com/package/npm/@microsoft/signalr)
* [<span data-ttu-id="08354-127">unpkg</span><span class="sxs-lookup"><span data-stu-id="08354-127">unpkg</span></span>](https://unpkg.com/@microsoft/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="08354-128">恩傑斯</span><span class="sxs-lookup"><span data-stu-id="08354-128">cdnjs</span></span>](https://cdnjs.com/libraries/aspnet-signalr)
* [<span data-ttu-id="08354-129">jsDelivr</span><span class="sxs-lookup"><span data-stu-id="08354-129">jsDelivr</span></span>](https://www.jsdelivr.com/package/npm/@aspnet/signalr)
* [<span data-ttu-id="08354-130">unpkg</span><span class="sxs-lookup"><span data-stu-id="08354-130">unpkg</span></span>](https://unpkg.com/@aspnet/signalr@next/dist/browser/signalr.min.js)

::: moniker-end

### <a name="install-with-libman"></a><span data-ttu-id="08354-131">使用 LibMan 安裝</span><span class="sxs-lookup"><span data-stu-id="08354-131">Install with LibMan</span></span>

<span data-ttu-id="08354-132">[LibMan](xref:client-side/libman/index)可用於從 CDN 託管的用戶端庫安裝特定的用戶端庫檔。</span><span class="sxs-lookup"><span data-stu-id="08354-132">[LibMan](xref:client-side/libman/index) can be used to install specific client library files from the CDN-hosted client library.</span></span> <span data-ttu-id="08354-133">例如,僅將已小的 JavaScript 檔添加到專案中。</span><span class="sxs-lookup"><span data-stu-id="08354-133">For example, only add the minified JavaScript file to the project.</span></span> <span data-ttu-id="08354-134">有關該方法的詳細資訊,請參閱[SignalR添加用戶端庫](xref:tutorials/signalr#add-the-signalr-client-library)。</span><span class="sxs-lookup"><span data-stu-id="08354-134">For details on that approach, see [Add the SignalR client library](xref:tutorials/signalr#add-the-signalr-client-library).</span></span>

## <a name="connect-to-a-hub"></a><span data-ttu-id="08354-135">連接到集線器</span><span class="sxs-lookup"><span data-stu-id="08354-135">Connect to a hub</span></span>

<span data-ttu-id="08354-136">以下代碼創建並啟動連接。</span><span class="sxs-lookup"><span data-stu-id="08354-136">The following code creates and starts a connection.</span></span> <span data-ttu-id="08354-137">中心的名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="08354-137">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="08354-138">交叉來源連線</span><span class="sxs-lookup"><span data-stu-id="08354-138">Cross-origin connections</span></span>

<span data-ttu-id="08354-139">通常,瀏覽器將連接與請求的頁面從同一域載入。</span><span class="sxs-lookup"><span data-stu-id="08354-139">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="08354-140">但是,有時需要連接到另一個域。</span><span class="sxs-lookup"><span data-stu-id="08354-140">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="08354-141">為了防止惡意網站從其他網站讀取敏感資料,預設情況下將禁用[跨源連接](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="08354-141">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="08354-142">要允許跨源請求,請在類中`Startup`啟用它。</span><span class="sxs-lookup"><span data-stu-id="08354-142">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="08354-143">從用戶端呼叫中心方法</span><span class="sxs-lookup"><span data-stu-id="08354-143">Call hub methods from client</span></span>

<span data-ttu-id="08354-144">JavaScript客戶端透過[HubConnect](/javascript/api/%40aspnet/signalr/hubconnection)的[呼叫](/javascript/api/%40aspnet/signalr/hubconnection#invoke)方法在集線器上調用公共方法。</span><span class="sxs-lookup"><span data-stu-id="08354-144">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="08354-145">該方法`invoke`接受兩個參數:</span><span class="sxs-lookup"><span data-stu-id="08354-145">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="08354-146">中心方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="08354-146">The name of the hub method.</span></span> <span data-ttu-id="08354-147">在下面的範例中,中心上的方法名稱為`SendMessage`。</span><span class="sxs-lookup"><span data-stu-id="08354-147">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="08354-148">在中心方法中定義的任何參數。</span><span class="sxs-lookup"><span data-stu-id="08354-148">Any arguments defined in the hub method.</span></span> <span data-ttu-id="08354-149">在下面的範例中,參數名稱稱為`message`。</span><span class="sxs-lookup"><span data-stu-id="08354-149">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="08354-150">範例代碼使用除 Internet Explorer 以外的所有主要瀏覽器的當前版本中支援的箭頭函數語法。</span><span class="sxs-lookup"><span data-stu-id="08354-150">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="08354-151">如果在*無伺服器模式下*SignalR使用 Azure 服務,則無法從用戶端呼叫集線器方法。</span><span class="sxs-lookup"><span data-stu-id="08354-151">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="08354-152">有關詳細資訊,請參閱[SignalR服務文件](/azure/azure-signalr/signalr-concept-serverless-development-config)。</span><span class="sxs-lookup"><span data-stu-id="08354-152">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="08354-153">此方法`invoke`傳回 JavaScript[承諾](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)。</span><span class="sxs-lookup"><span data-stu-id="08354-153">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="08354-154">當`Promise`伺服器上的方法傳回時,使用傳回值(如果有)解析 。</span><span class="sxs-lookup"><span data-stu-id="08354-154">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="08354-155">如果伺服器上的方法引發錯誤,`Promise`則 拒絕使用錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="08354-155">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="08354-156">使用`then`本身`catch`上的`Promise`和方法來處理這些情況(`await`或語法)。</span><span class="sxs-lookup"><span data-stu-id="08354-156">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="08354-157">該方法`send`返回JAVAScript。 `Promise`</span><span class="sxs-lookup"><span data-stu-id="08354-157">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="08354-158">將`Promise`解解訊息發送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="08354-158">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="08354-159">如果傳送訊息有錯誤`Promise`, 則拒絕使用錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="08354-159">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="08354-160">使用`then`本身`catch`上的`Promise`和方法來處理這些情況(`await`或語法)。</span><span class="sxs-lookup"><span data-stu-id="08354-160">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="08354-161">使用`send`不會等到伺服器收到消息。</span><span class="sxs-lookup"><span data-stu-id="08354-161">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="08354-162">因此,無法從伺服器返回數據或錯誤。</span><span class="sxs-lookup"><span data-stu-id="08354-162">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="08354-163">從中心呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="08354-163">Call client methods from hub</span></span>

<span data-ttu-id="08354-164">要從集線器接收消息,請使用的[on](/javascript/api/%40aspnet/signalr/hubconnection#on)`HubConnection`方法定義方法。</span><span class="sxs-lookup"><span data-stu-id="08354-164">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="08354-165">JavaScript 用戶端方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="08354-165">The name of the JavaScript client method.</span></span> <span data-ttu-id="08354-166">在下面的範例中,方法名稱稱為`ReceiveMessage`。</span><span class="sxs-lookup"><span data-stu-id="08354-166">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="08354-167">中心傳遞給方法的參數。</span><span class="sxs-lookup"><span data-stu-id="08354-167">Arguments the hub passes to the method.</span></span> <span data-ttu-id="08354-168">在下面的範例中,參數值為`message`。</span><span class="sxs-lookup"><span data-stu-id="08354-168">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="08354-169">當伺服器端代碼`connection.on`使用[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)方法調用它時,前面的代碼將運行。</span><span class="sxs-lookup"><span data-stu-id="08354-169">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="08354-170">以符合與中`SendAsync`定義的名稱與參數來決定要呼叫的用戶端方法`connection.on`。</span><span class="sxs-lookup"><span data-stu-id="08354-170"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="08354-171">最佳做法是調用`HubConnection``on`後 上的[開始](/javascript/api/%40aspnet/signalr/hubconnection#start)方法。</span><span class="sxs-lookup"><span data-stu-id="08354-171">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="08354-172">這樣做可確保在收到任何消息之前註冊處理程式。</span><span class="sxs-lookup"><span data-stu-id="08354-172">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="08354-173">錯誤處理和記錄</span><span class="sxs-lookup"><span data-stu-id="08354-173">Error handling and logging</span></span>

<span data-ttu-id="08354-174">將`catch`方法連結到方法的末尾,`start`以處理用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="08354-174">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="08354-175">用於`console.error`將錯誤輸出到瀏覽器的主控台。</span><span class="sxs-lookup"><span data-stu-id="08354-175">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=50)]

<span data-ttu-id="08354-176">通過傳遞記錄器和事件類型來在建立連接時記錄客戶端日誌跟蹤。</span><span class="sxs-lookup"><span data-stu-id="08354-176">Set up client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="08354-177">消息記錄與指定的日誌級別和更高。</span><span class="sxs-lookup"><span data-stu-id="08354-177">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="08354-178">可用紀錄等級如下:</span><span class="sxs-lookup"><span data-stu-id="08354-178">Available log levels are as follows:</span></span>

* <span data-ttu-id="08354-179">`signalR.LogLevel.Error`&ndash;錯誤消息。</span><span class="sxs-lookup"><span data-stu-id="08354-179">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="08354-180">僅`Error`記錄郵件。</span><span class="sxs-lookup"><span data-stu-id="08354-180">Logs `Error` messages only.</span></span>
* <span data-ttu-id="08354-181">`signalR.LogLevel.Warning`&ndash;有關潛在錯誤的警告消息。</span><span class="sxs-lookup"><span data-stu-id="08354-181">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="08354-182">日誌`Warning``Error`和消息。</span><span class="sxs-lookup"><span data-stu-id="08354-182">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="08354-183">`signalR.LogLevel.Information`&ndash;狀態消息,無錯誤。</span><span class="sxs-lookup"><span data-stu-id="08354-183">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="08354-184">日誌`Information``Warning``Error`和消息。</span><span class="sxs-lookup"><span data-stu-id="08354-184">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="08354-185">`signalR.LogLevel.Trace`&ndash;跟蹤消息。</span><span class="sxs-lookup"><span data-stu-id="08354-185">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="08354-186">記錄所有內容,包括集線器和客戶端之間傳輸的數據。</span><span class="sxs-lookup"><span data-stu-id="08354-186">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="08354-187">使用[HubConnectBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上的[配置記錄](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)方法設定紀錄等級。</span><span class="sxs-lookup"><span data-stu-id="08354-187">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="08354-188">消息將記錄到瀏覽器控制台。</span><span class="sxs-lookup"><span data-stu-id="08354-188">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="08354-189">重新連線客戶端</span><span class="sxs-lookup"><span data-stu-id="08354-189">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="08354-190">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="08354-190">Automatically reconnect</span></span>

<span data-ttu-id="08354-191">JavaScriptSignalR用戶端可以配置為`withAutomaticReconnect`使用[HubConnectBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)上的方法自動重新連接。</span><span class="sxs-lookup"><span data-stu-id="08354-191">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="08354-192">默認情況下,它不會自動重新連接。</span><span class="sxs-lookup"><span data-stu-id="08354-192">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="08354-193">沒有任何參數,`withAutomaticReconnect()`將用戶端配置為分別等待 0、2、10 和 30 秒,然後再嘗試每次重新連接嘗試,在四次嘗試失敗後停止。</span><span class="sxs-lookup"><span data-stu-id="08354-193">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="08354-194">在開始任何重新連線嘗試之前,`HubConnection``HubConnectionState.Reconnecting`將轉換為`onreconnecting`狀態 並觸發其回調,而不是`Disconnected`轉換到 狀態並`onclose`觸發其回調,就像未配置自動`HubConnection`重新連接一樣。</span><span class="sxs-lookup"><span data-stu-id="08354-194">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="08354-195">這提供了一個機會來警告使用者連接已丟失並禁用 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="08354-195">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting(error => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="08354-196">如果用戶端在前四次嘗試中成功重新連接,則將`HubConnection`轉換回`Connected`狀態並觸發`onreconnected`其回調。</span><span class="sxs-lookup"><span data-stu-id="08354-196">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="08354-197">這提供了一個機會,通知用戶連接已重新建立。</span><span class="sxs-lookup"><span data-stu-id="08354-197">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="08354-198">由於連接對伺服器看起來完全新,因此將為`connectionId``onreconnected`回調提供新的連接。</span><span class="sxs-lookup"><span data-stu-id="08354-198">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="08354-199">如果`onreconnected`配置為[跳過協商](xref:signalr/configuration#configure-client-options)`connectionId`,`HubConnection`則回調的參數將未定義。</span><span class="sxs-lookup"><span data-stu-id="08354-199">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected(connectionId => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="08354-200">`withAutomaticReconnect()`不會設定`HubConnection`以 重試初始啟動失敗,因此需要手動處理啟動失敗:</span><span class="sxs-lookup"><span data-stu-id="08354-200">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="08354-201">如果用戶端在前四次嘗試中未成功重新連接,則將`HubConnection`轉換為`Disconnected`狀態並觸發其[關閉](/javascript/api/%40aspnet/signalr/hubconnection#onclose)回調。</span><span class="sxs-lookup"><span data-stu-id="08354-201">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="08354-202">這提供了一個機會,通知使用者連接已永久丟失,並建議刷新頁面:</span><span class="sxs-lookup"><span data-stu-id="08354-202">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose(error => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="08354-203">為了在斷開連接或更改重新連接計時之前設定自定義的重新連接嘗試次數,`withAutomaticReconnect`請接受表示延遲(以毫秒為單位)的數位陣列,以等待,然後再開始每次重新連接嘗試。</span><span class="sxs-lookup"><span data-stu-id="08354-203">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="08354-204">前面的範例配置`HubConnection`要在連接丟失後立即開始嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="08354-204">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="08354-205">對於默認配置也是如此。</span><span class="sxs-lookup"><span data-stu-id="08354-205">This is also true for the default configuration.</span></span>

<span data-ttu-id="08354-206">如果第一次重新連接嘗試失敗,第二次重新連接嘗試也將立即開始,而不是像預設配置中那樣等待 2 秒。</span><span class="sxs-lookup"><span data-stu-id="08354-206">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="08354-207">如果第二次重新連接嘗試失敗,第三次重新連接嘗試將在 10 秒內開始,這再次類似於默認配置。</span><span class="sxs-lookup"><span data-stu-id="08354-207">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="08354-208">然後,自定義行為會在第三次重新連接嘗試失敗后停止,而不是像在預設配置中那樣在 30 秒內嘗試再次重新連接嘗試,從而再次偏離默認行為。</span><span class="sxs-lookup"><span data-stu-id="08354-208">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="08354-209">如果希望對自動重新連接嘗試的計時和次數進行更多控制,請`withAutomaticReconnect`接受`IRetryPolicy`實現介面的物件,該物件具有名為`nextRetryDelayInMilliseconds`的 單個方法。</span><span class="sxs-lookup"><span data-stu-id="08354-209">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="08354-210">`nextRetryDelayInMilliseconds`使用類型`RetryContext`獲取單個參數。</span><span class="sxs-lookup"><span data-stu-id="08354-210">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="08354-211">有`RetryContext`三個屬性: `previousRetryCount` `elapsedMilliseconds``retryReason`與`number`分別為`number``Error`a 和 。</span><span class="sxs-lookup"><span data-stu-id="08354-211">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="08354-212">在第一次重新連接嘗試之前,`previousRetryCount``elapsedMilliseconds`兩 者都將為`retryReason`零, 並且將是導致連接丟失的錯誤。</span><span class="sxs-lookup"><span data-stu-id="08354-212">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="08354-213">每次失敗的重試嘗試(`previousRetryCount`將遞增為 1)`elapsedMilliseconds`後, 將更新以反映到目前為止重新連接所花費的時間(以毫秒為`retryReason`單位), 並且將是導致上次重新連接嘗試失敗的錯誤。</span><span class="sxs-lookup"><span data-stu-id="08354-213">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="08354-214">`nextRetryDelayInMilliseconds`必須傳回表示在下一次重新連接嘗試之前等待的毫秒數的數位`null``HubConnection`, 或者是否應停止重新連接。</span><span class="sxs-lookup"><span data-stu-id="08354-214">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="08354-215">或者,您可以編寫代碼,這些代碼將手動重新連接用戶端,如[手動重新連接](#manually-reconnect)中所示。</span><span class="sxs-lookup"><span data-stu-id="08354-215">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="08354-216">手動重新連接</span><span class="sxs-lookup"><span data-stu-id="08354-216">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="08354-217">在 3.0 之前SignalR,的 JavaScript 用戶端不會自動重新連接。</span><span class="sxs-lookup"><span data-stu-id="08354-217">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="08354-218">您必須編寫代碼,以手動重新連接用戶端。</span><span class="sxs-lookup"><span data-stu-id="08354-218">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="08354-219">以下代碼展示了典型的手動重新連接方法:</span><span class="sxs-lookup"><span data-stu-id="08354-219">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="08354-220">創建函數(在本例中為`start`函數)以啟動連接。</span><span class="sxs-lookup"><span data-stu-id="08354-220">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="08354-221">在`start`連接`onclose`的事件處理程序中調用函數。</span><span class="sxs-lookup"><span data-stu-id="08354-221">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="08354-222">實際實現將使用指數級回退或在放棄之前重試指定次數。</span><span class="sxs-lookup"><span data-stu-id="08354-222">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08354-223">其他資源</span><span class="sxs-lookup"><span data-stu-id="08354-223">Additional resources</span></span>

* [<span data-ttu-id="08354-224">JavaScript API 參考</span><span class="sxs-lookup"><span data-stu-id="08354-224">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="08354-225">JavaScript 教學</span><span class="sxs-lookup"><span data-stu-id="08354-225">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="08354-226">WebPack 與 TypeScript 教學</span><span class="sxs-lookup"><span data-stu-id="08354-226">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="08354-227">樞紐</span><span class="sxs-lookup"><span data-stu-id="08354-227">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="08354-228">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="08354-228">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="08354-229">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="08354-229">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="08354-230">跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="08354-230">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="08354-231">[AzureSignalR服務無伺服器文件](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="08354-231">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
