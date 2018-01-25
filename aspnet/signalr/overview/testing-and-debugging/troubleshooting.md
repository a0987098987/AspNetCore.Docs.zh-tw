---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: "SignalR 疑難排解 |Microsoft 文件"
author: pfletcher
description: "本文說明開發 SignalR 應用程式的一般問題。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 2394ee81f4592417a034e47db6eefd3e4b91a9af
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="signalr-troubleshooting"></a><span data-ttu-id="e887b-103">SignalR 疑難排解</span><span class="sxs-lookup"><span data-stu-id="e887b-103">SignalR Troubleshooting</span></span>
====================
<span data-ttu-id="e887b-104">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e887b-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="e887b-105">本文件說明與 SignalR 的常見疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="e887b-105">This document describes common troubleshooting issues with SignalR.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="e887b-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="e887b-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="e887b-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e887b-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="e887b-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e887b-108">.NET 4.5</span></span>
> - <span data-ttu-id="e887b-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="e887b-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="e887b-110">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="e887b-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="e887b-111">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e887b-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="e887b-112">問題和註解</span><span class="sxs-lookup"><span data-stu-id="e887b-112">Questions and comments</span></span>
> 
> <span data-ttu-id="e887b-113">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="e887b-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="e887b-114">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="e887b-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="e887b-115">本文件包含下列各節。</span><span class="sxs-lookup"><span data-stu-id="e887b-115">This document contains the following sections.</span></span>

- [<span data-ttu-id="e887b-116">呼叫以無訊息模式失敗的用戶端和伺服器之間的方法</span><span class="sxs-lookup"><span data-stu-id="e887b-116">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="e887b-117">設定 IIS websockets，ping/pong 來偵測無作用的用戶端</span><span class="sxs-lookup"><span data-stu-id="e887b-117">Configuring IIS websockets to ping/pong to detect a dead client</span></span>](#pong)
- [<span data-ttu-id="e887b-118">其他連線問題</span><span class="sxs-lookup"><span data-stu-id="e887b-118">Other connection issues</span></span>](#other)
- [<span data-ttu-id="e887b-119">編譯和伺服器端錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-119">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="e887b-120">Visual Studio 問題</span><span class="sxs-lookup"><span data-stu-id="e887b-120">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="e887b-121">網際網路資訊服務的問題</span><span class="sxs-lookup"><span data-stu-id="e887b-121">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="e887b-122">Microsoft Azure 問題</span><span class="sxs-lookup"><span data-stu-id="e887b-122">Microsoft Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="e887b-123">呼叫以無訊息模式失敗的用戶端和伺服器之間的方法</span><span class="sxs-lookup"><span data-stu-id="e887b-123">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="e887b-124">本章節描述針對方法呼叫失敗而無意義的錯誤訊息的用戶端與伺服器之間的可能原因。</span><span class="sxs-lookup"><span data-stu-id="e887b-124">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="e887b-125">中的 SignalR 應用程式中，伺服器會有任何資訊的方法，用戶端會實作;伺服器叫用時用戶端方法，方法名稱和參數資料傳送至用戶端，以及它存在於伺服器指定的格式時，才執行此方法。</span><span class="sxs-lookup"><span data-stu-id="e887b-125">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="e887b-126">如果沒有符合的方法上找到用戶端，會發生任何事，且沒有錯誤訊息會在伺服器上引發。</span><span class="sxs-lookup"><span data-stu-id="e887b-126">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="e887b-127">若要進一步調查的用戶端未呼叫的方法，您可以開啟之前呼叫，以查看哪些呼叫集線器上的 start 方法都來自伺服器的記錄。</span><span class="sxs-lookup"><span data-stu-id="e887b-127">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="e887b-128">若要啟用記錄至 JavaScript 應用程式中，請參閱[如何啟用用戶端記錄 （JavaScript 用戶端版本）](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)。</span><span class="sxs-lookup"><span data-stu-id="e887b-128">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="e887b-129">若要啟用.NET 用戶端應用程式中的記錄，請參閱[如何啟用用戶端記錄 （.NET 用戶端版本）](../guide-to-the-api/hubs-api-guide-net-client.md#logging)。</span><span class="sxs-lookup"><span data-stu-id="e887b-129">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="e887b-130">拼錯的方法、 不正確的方法簽章或不正確的中樞名稱</span><span class="sxs-lookup"><span data-stu-id="e887b-130">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="e887b-131">如果名稱或呼叫方法的簽章不完全符合適當的方法，用戶端上，呼叫會失敗。</span><span class="sxs-lookup"><span data-stu-id="e887b-131">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="e887b-132">請確認伺服器所呼叫的方法名稱符合用戶端上方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="e887b-132">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="e887b-133">此外，SignalR 建立中樞 proxy 使用 camel 命名法的大小寫慣例的方法，為適合在 JavaScript 中，因此呼叫的方法，`SendMessage`會呼叫在伺服器上`sendMessage`中用戶端 proxy。</span><span class="sxs-lookup"><span data-stu-id="e887b-133">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="e887b-134">如果您使用`HubName`屬性在伺服器端程式碼中，請確認所使用的名稱符合用來在用戶端建立中樞的名稱。</span><span class="sxs-lookup"><span data-stu-id="e887b-134">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="e887b-135">如果您未使用`HubName`屬性，請確認在 JavaScript 用戶端中樞的名稱是依照 camel 命名法的大小寫慣例，而不是 ChatHub chatHub 等。</span><span class="sxs-lookup"><span data-stu-id="e887b-135">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="e887b-136">重複的用戶端上的方法名稱</span><span class="sxs-lookup"><span data-stu-id="e887b-136">Duplicate method name on client</span></span>

<span data-ttu-id="e887b-137">請確認，您不需要重複的方法只有大小寫不同的用戶端上。</span><span class="sxs-lookup"><span data-stu-id="e887b-137">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="e887b-138">如果用戶端應用程式呼叫的方法`sendMessage`，確認沒有也稱為方法`SendMessage`以及。</span><span class="sxs-lookup"><span data-stu-id="e887b-138">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="e887b-139">在用戶端上遺漏的 JSON 剖析器</span><span class="sxs-lookup"><span data-stu-id="e887b-139">Missing JSON parser on the client</span></span>

<span data-ttu-id="e887b-140">SignalR 要求必須有序列化呼叫伺服器與用戶端之間的 JSON 剖析器。</span><span class="sxs-lookup"><span data-stu-id="e887b-140">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="e887b-141">如果您的用戶端並沒有內建的 JSON 剖析器 （例如 Internet Explorer 7)，您必須包含在應用程式中。</span><span class="sxs-lookup"><span data-stu-id="e887b-141">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="e887b-142">您可以下載 JSON 剖析器[這裡](http://nuget.org/packages/json2)。</span><span class="sxs-lookup"><span data-stu-id="e887b-142">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="e887b-143">混用中樞 PersistentConnection 語法</span><span class="sxs-lookup"><span data-stu-id="e887b-143">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="e887b-144">SignalR 使用兩種通訊模式： 集線器和 PersistentConnections。</span><span class="sxs-lookup"><span data-stu-id="e887b-144">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="e887b-145">呼叫這兩個通訊模型的語法是在用戶端程式碼中的不同。</span><span class="sxs-lookup"><span data-stu-id="e887b-145">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="e887b-146">如果您在伺服器上的程式碼中加入集線器，請確認所有用戶端程式碼會使用適當的中樞語法。</span><span class="sxs-lookup"><span data-stu-id="e887b-146">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="e887b-147">**建立 PersistentConnection JavaScript 用戶端中的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="e887b-147">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="e887b-148">**建立 Javascript 用戶端中的中樞 Proxy 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="e887b-148">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="e887b-149">**C# 伺服器程式碼會對應至 PersistentConnection 路由**</span><span class="sxs-lookup"><span data-stu-id="e887b-149">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="e887b-150">**C# 伺服器程式碼對應路由至中樞，或多個集線器如果您有多個應用程式**</span><span class="sxs-lookup"><span data-stu-id="e887b-150">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="e887b-151">連接啟動之後才會加入訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="e887b-151">Connection started before subscriptions are added</span></span>

<span data-ttu-id="e887b-152">如果中樞的連線已啟動，可以從伺服器呼叫的方法加入至 proxy 之前，不會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="e887b-152">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="e887b-153">下列 JavaScript 程式碼將不會啟動中樞正確：</span><span class="sxs-lookup"><span data-stu-id="e887b-153">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="e887b-154">**不正確用戶端的 JavaScript 程式碼將不允許接收的訊息中心**</span><span class="sxs-lookup"><span data-stu-id="e887b-154">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="e887b-155">請改為加入的方法訂用帳戶呼叫開始之前：</span><span class="sxs-lookup"><span data-stu-id="e887b-155">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="e887b-156">**正確地將訂閱加入至中樞的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="e887b-156">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="e887b-157">中樞 proxy 上遺漏的方法名稱</span><span class="sxs-lookup"><span data-stu-id="e887b-157">Missing method name on the hub proxy</span></span>

<span data-ttu-id="e887b-158">請確認訂閱在用戶端上，在伺服器上定義的方法。</span><span class="sxs-lookup"><span data-stu-id="e887b-158">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="e887b-159">即使伺服器定義的方法，它必須仍加入至用戶端 proxy。</span><span class="sxs-lookup"><span data-stu-id="e887b-159">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="e887b-160">方法可以下列方式加入至用戶端 proxy (請注意，方法會加入至`client`成員的中樞，不中樞直接):</span><span class="sxs-lookup"><span data-stu-id="e887b-160">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="e887b-161">**將方法加入至中樞 proxy 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="e887b-161">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="e887b-162">集線器或未宣告為公用的中樞方法</span><span class="sxs-lookup"><span data-stu-id="e887b-162">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="e887b-163">若要顯示在用戶端上，實作中樞和方法必須宣告為`public`。</span><span class="sxs-lookup"><span data-stu-id="e887b-163">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="e887b-164">從不同的應用程式存取中樞</span><span class="sxs-lookup"><span data-stu-id="e887b-164">Accessing hub from a different application</span></span>

<span data-ttu-id="e887b-165">SignalR 中樞只在透過實作 SignalR 用戶端應用程式中存取。</span><span class="sxs-lookup"><span data-stu-id="e887b-165">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="e887b-166">SignalR 無法交互操作與其他的通訊程式庫 （例如 SOAP 或 WCF web 服務。）如果沒有可用的目標平台的 SignalR 用戶端，您無法直接存取伺服器的端點。</span><span class="sxs-lookup"><span data-stu-id="e887b-166">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="e887b-167">以手動方式序列化的資料</span><span class="sxs-lookup"><span data-stu-id="e887b-167">Manually serializing data</span></span>

<span data-ttu-id="e887b-168">SignalR 會自動使用 JSON 序列化程式方法的參數那里的不需要自行進行。</span><span class="sxs-lookup"><span data-stu-id="e887b-168">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="e887b-169">並未在 OnDisconnected 函式中的用戶端上執行的遠端中樞方法</span><span class="sxs-lookup"><span data-stu-id="e887b-169">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="e887b-170">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-170">This behavior is by design.</span></span> <span data-ttu-id="e887b-171">當`OnDisconnected`是呼叫，中樞已經輸入`Disconnected`不允許進一步的中樞方法呼叫的狀態。</span><span class="sxs-lookup"><span data-stu-id="e887b-171">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="e887b-172">**C# 伺服器正確執行的程式碼的程式碼 OnDisconnected 事件**</span><span class="sxs-lookup"><span data-stu-id="e887b-172">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a><span data-ttu-id="e887b-173">不一致的時間在引發 OnDisconnect</span><span class="sxs-lookup"><span data-stu-id="e887b-173">OnDisconnect not firing at consistent times</span></span>

<span data-ttu-id="e887b-174">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-174">This behavior is by design.</span></span> <span data-ttu-id="e887b-175">當使用者嘗試巡覽離具有使用中的 SignalR 連線的網頁時，SignalR 用戶端接著便可通知用戶端連接將會停止伺服器的最佳嘗試。</span><span class="sxs-lookup"><span data-stu-id="e887b-175">When a user attempts to navigate away from a page with an active SignalR connection, the SignalR client will then make a best-effort attempt to notify the server that the client connection will be stopped.</span></span> <span data-ttu-id="e887b-176">如果 SignalR 用戶端的最大速率來連線伺服器的嘗試失敗，伺服器將處置之後可設定的連接`DisconnectTimeout`以後，在這段`OnDisconnected`事件就會引發。</span><span class="sxs-lookup"><span data-stu-id="e887b-176">If the SignalR client's best-effort attempt fails to reach the server, the server will dispose of the connection after a configurable `DisconnectTimeout` later, at which time the `OnDisconnected` event will fire.</span></span> <span data-ttu-id="e887b-177">如果 SignalR 用戶端的最佳嘗試會成功，`OnDisconnected`會立即引發事件。</span><span class="sxs-lookup"><span data-stu-id="e887b-177">If the SignalR client's best-effort attempt is successful, the `OnDisconnected` event will fire immediately.</span></span>

<span data-ttu-id="e887b-178">如需設定資訊`DisconnectTimeout`設定，請參閱[處理連接的存留期事件： DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)。</span><span class="sxs-lookup"><span data-stu-id="e887b-178">For information on setting the `DisconnectTimeout` setting, see [Handling connection lifetime events: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).</span></span>

### <a name="connection-limit-reached"></a><span data-ttu-id="e887b-179">已達到連線限制</span><span class="sxs-lookup"><span data-stu-id="e887b-179">Connection limit reached</span></span>

<span data-ttu-id="e887b-180">當用戶端作業系統，例如 Windows 7 上使用 IIS 的完整版本，則會加上 10 連線限制。</span><span class="sxs-lookup"><span data-stu-id="e887b-180">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="e887b-181">當使用用戶端作業系統，請改用 IIS Express 若要避免這項限制。</span><span class="sxs-lookup"><span data-stu-id="e887b-181">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="e887b-182">適當地設定跨網域連線</span><span class="sxs-lookup"><span data-stu-id="e887b-182">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="e887b-183">如果跨網域連線 （連線的 SignalR URL 不是在與裝載網頁位於相同網域中） 未正確設定，連線可能會失敗，沒有錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e887b-183">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="e887b-184">如需如何啟用跨網域通訊的資訊，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="e887b-184">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="e887b-185">使用 NTLM (Active Directory) 無法在.NET 用戶端中運作的連線</span><span class="sxs-lookup"><span data-stu-id="e887b-185">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="e887b-186">如果連接未正確設定，在.NET 用戶端應用程式中使用網域安全性的連接可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="e887b-186">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="e887b-187">若要在網域環境中使用 SignalR，設定必要的連接屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="e887b-187">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="e887b-188">**C# 用戶端程式碼實作連線認證**</span><span class="sxs-lookup"><span data-stu-id="e887b-188">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a><span data-ttu-id="e887b-189">設定 IIS websockets，ping/pong 來偵測無作用的用戶端</span><span class="sxs-lookup"><span data-stu-id="e887b-189">Configuring IIS websockets to ping/pong to detect a dead client</span></span>

<span data-ttu-id="e887b-190">SignalR 的伺服器不知道，是否用戶端是無作用或不，而它們是依賴來自基礎 websocket 連線失敗的通知，也就是 OnClose 回呼。</span><span class="sxs-lookup"><span data-stu-id="e887b-190">SignalR servers don't know if the client is dead or not and they rely on notification from the underlying websocket for connection failures, that is, the OnClose callback.</span></span> <span data-ttu-id="e887b-191">這個問題的解決方案之一是設定為您執行 ping/pong IIS websocket。</span><span class="sxs-lookup"><span data-stu-id="e887b-191">One solution to this problem is to configure IIS websockets to do the ping/pong for you.</span></span> <span data-ttu-id="e887b-192">這可確保如果意外地中斷，將會關閉您的連線。</span><span class="sxs-lookup"><span data-stu-id="e887b-192">This ensures that your connection will close if it breaks unexpectedly.</span></span> <span data-ttu-id="e887b-193">如需詳細資訊，請參閱[這 stackoverflow 文章](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)。</span><span class="sxs-lookup"><span data-stu-id="e887b-193">For more information see [this stackoverflow post](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).</span></span>

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="e887b-194">其他連線問題</span><span class="sxs-lookup"><span data-stu-id="e887b-194">Other connection issues</span></span>

<span data-ttu-id="e887b-195">本章節描述的原因和解決方案的特定徵狀或在連接期間發生的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e887b-195">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="e887b-196">「 傳送資料之前，必須呼叫啟動 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-196">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="e887b-197">如果程式碼會參考 SignalR 物件啟動連接之前，通常會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-197">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="e887b-198">處理常式和之類裝設，呼叫在伺服器上定義的方法必須將連接完成後。</span><span class="sxs-lookup"><span data-stu-id="e887b-198">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="e887b-199">請注意，呼叫`Start`是非同步的因此程式碼之後呼叫可能會執行之前完成。</span><span class="sxs-lookup"><span data-stu-id="e887b-199">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="e887b-200">若要加入處理常式完全啟動連線之後，最好是將它們放到做為參數傳遞給 start 方法的回呼函式：</span><span class="sxs-lookup"><span data-stu-id="e887b-200">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="e887b-201">**正確加入參考 SignalR 物件的事件處理常式的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="e887b-201">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="e887b-202">如果連線停止 SignalR 物件仍然會被參考時，也會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-202">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="e887b-203">"301 已永久移動 」 或者 「 暫時移動 302 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-203">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="e887b-204">如果專案包含名為 SignalR，自動建立的 proxy 會干擾的資料夾，可能會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-204">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="e887b-205">若要避免這個錯誤，不使用資料夾，稱為`SignalR`在您的應用程式或關閉自動 proxy 產生關閉中。</span><span class="sxs-lookup"><span data-stu-id="e887b-205">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="e887b-206">請參閱[產生 Proxy 和它為您做](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e887b-206">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="e887b-207">在.NET 或 Silverlight 用戶端的 「 403 禁止 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-207">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="e887b-208">在跨網域通訊未正確啟用跨網域環境中可能會發生這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-208">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="e887b-209">如需如何啟用跨網域通訊的資訊，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="e887b-209">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="e887b-210">若要建立跨定義域中的連接 Silverlight 用戶端，請參閱[Silverlight 用戶端進行跨網域連線](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)。</span><span class="sxs-lookup"><span data-stu-id="e887b-210">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="e887b-211">「 404 找不到 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-211">"404 Not Found" error</span></span>

<span data-ttu-id="e887b-212">有多種原因會導致此問題。</span><span class="sxs-lookup"><span data-stu-id="e887b-212">There are several causes for this issue.</span></span> <span data-ttu-id="e887b-213">請確認下列各項：</span><span class="sxs-lookup"><span data-stu-id="e887b-213">Verify all of the following:</span></span>

- <span data-ttu-id="e887b-214">**中樞 proxy 位址參照的格式不正確：**如果產生的中樞 proxy 位址的參考的格式不正確，通常會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-214">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="e887b-215">請確認中樞位址的參考都正確。</span><span class="sxs-lookup"><span data-stu-id="e887b-215">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="e887b-216">請參閱[如何參考動態產生的 proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e887b-216">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="e887b-217">**將路由加入至應用程式，然後再加入中樞路由：**如果應用程式使用其他路由，請確認新增的第一個路由是呼叫`MapSignalR`。</span><span class="sxs-lookup"><span data-stu-id="e887b-217">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapSignalR`.</span></span>
- <span data-ttu-id="e887b-218">**無副檔名的 url 使用 IIS 7 或 7.5，而不進行更新：**使用 IIS 7 或 7.5 需要更新無副檔名的 url，讓伺服器可提供存取中樞定義`/signalr/hubs`。</span><span class="sxs-lookup"><span data-stu-id="e887b-218">**Using IIS 7 or 7.5 without the update for extensionless URLs:** Using IIS 7 or 7.5 requires an update for extensionless URLs so that the server can provide access to the hub definitions at `/signalr/hubs`.</span></span> <span data-ttu-id="e887b-219">您可以找到更新[這裡](https://support.microsoft.com/kb/980368)。</span><span class="sxs-lookup"><span data-stu-id="e887b-219">The update can be found [here](https://support.microsoft.com/kb/980368).</span></span>
- <span data-ttu-id="e887b-220">**IIS 快取已過期或已損毀：**若要確認快取內容不是已過期，請在 PowerShell 視窗中，清除快取中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="e887b-220">**IIS cache out of date or corrupt:** To verify that the cache contents are not out of date, enter the following command in a PowerShell window to clear the cache:</span></span>

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a><span data-ttu-id="e887b-221">「 500 內部伺服器錯誤 」</span><span class="sxs-lookup"><span data-stu-id="e887b-221">"500 Internal Server Error"</span></span>

<span data-ttu-id="e887b-222">這是非常一般錯誤，可能會有各種不同的原因。</span><span class="sxs-lookup"><span data-stu-id="e887b-222">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="e887b-223">錯誤詳細資料應該會出現在伺服器的事件記錄檔，或可以找到偵錯伺服器。</span><span class="sxs-lookup"><span data-stu-id="e887b-223">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="e887b-224">藉由開啟詳細的錯誤，伺服器上可能會取得更詳細的錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="e887b-224">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="e887b-225">如需詳細資訊，請參閱[如何處理中樞類別中的錯誤](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)。</span><span class="sxs-lookup"><span data-stu-id="e887b-225">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

<span data-ttu-id="e887b-226">如果防火牆或 proxy 設定不正確，造成重寫的要求標頭，也普遍出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-226">This error is also commonly seen if a firewall or proxy is not configured properly, causing the request headers to be rewritten.</span></span> <span data-ttu-id="e887b-227">方案是確定防火牆或 proxy 上啟用了連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="e887b-227">The solution is to make sure that port 80 is enabled on the firewall or proxy.</span></span>

### <a name="unexpected-response-code-500"></a><span data-ttu-id="e887b-228">「 非預期的回應碼： 500 」</span><span class="sxs-lookup"><span data-stu-id="e887b-228">"Unexpected response code: 500"</span></span>

<span data-ttu-id="e887b-229">如果應用程式中使用的.NET framework 版本不符合 Web.Config 中指定的版本，可能會發生這個錯誤。解決方法是確認.NET 4.5 用於應用程式設定和 Web.Config 檔。</span><span class="sxs-lookup"><span data-stu-id="e887b-229">This error may occur if the version of .NET framework used in the application does not match the version specified in Web.Config. The solution is to verify that .NET 4.5 is used in both the application settings and the Web.Config file.</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="e887b-230">「 TypeError: &lt;hubType&gt;是未定義 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-230">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="e887b-231">如果將產生此錯誤的呼叫`MapSignalR`進行不正確。</span><span class="sxs-lookup"><span data-stu-id="e887b-231">This error will result if the call to `MapSignalR` is not made properly.</span></span> <span data-ttu-id="e887b-232">請參閱[如何註冊 SignalR 中介軟體和設定 SignalR 選項](../guide-to-the-api/hubs-api-guide-server.md#route)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e887b-232">See [How to register SignalR Middleware and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="e887b-233">JsonSerializationException 未處理的使用者程式碼</span><span class="sxs-lookup"><span data-stu-id="e887b-233">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="e887b-234">請確認您傳送至您的方法的參數不包含非可序列化的型別 （例如檔案控制代碼或資料庫連接）。</span><span class="sxs-lookup"><span data-stu-id="e887b-234">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="e887b-235">如果您需要使用的成員，您不想要傳送至用戶端 （或是安全性序列化的原因），使用伺服器端物件上`JSONIgnore`屬性。</span><span class="sxs-lookup"><span data-stu-id="e887b-235">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="e887b-236">"通訊協定錯誤： 未知的傳輸 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-236">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="e887b-237">如果用戶端不支援 SignalR 使用的傳輸，可能會發生這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-237">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="e887b-238">請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)所在瀏覽器可以搭配 SignalR 的資訊。</span><span class="sxs-lookup"><span data-stu-id="e887b-238">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="e887b-239">「 JavaScript Hub proxy 產生具有已停用 」。</span><span class="sxs-lookup"><span data-stu-id="e887b-239">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="e887b-240">如果會發生此錯誤`DisableJavaScriptProxies`時也包括在動態產生之 proxy 的參考設定`signalr/hubs`。</span><span class="sxs-lookup"><span data-stu-id="e887b-240">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="e887b-241">如需有關如何手動建立 proxy 的詳細資訊，請參閱[產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。</span><span class="sxs-lookup"><span data-stu-id="e887b-241">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="e887b-242">"的格式不正確的連接識別碼 」 或 「 使用者識別無法在使用中的 SignalR 連線期間變更 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-242">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="e887b-243">如果使用驗證時，並停止連線之前，登出用戶端，可能會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-243">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="e887b-244">解決方法是停止前用戶端登出 SignalR 連線。</span><span class="sxs-lookup"><span data-stu-id="e887b-244">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="e887b-245">「 未能攔截的錯誤： SignalR： 找不到的 jQuery。</span><span class="sxs-lookup"><span data-stu-id="e887b-245">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="e887b-246">請確定 jQuery 參考 SignalR.js 檔案前 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-246">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="e887b-247">SignalR JavaScript 用戶端所需執行的 jQuery。</span><span class="sxs-lookup"><span data-stu-id="e887b-247">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="e887b-248">請確認您對 jQuery 參考是正確、 所使用的路徑有效，以及 signalr 參考之前，會參考 jQuery。</span><span class="sxs-lookup"><span data-stu-id="e887b-248">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="e887b-249">「 未能攔截 TypeError： 無法讀取屬性 '&lt;屬性&gt;' 的未定義 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-249">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="e887b-250">產生此錯誤沒有 jQuery 或參考正確的中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="e887b-250">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="e887b-251">請確認您對 jQuery 和中樞 proxy 的參考正確，所使用的路徑有效，且參考 jQuery 是之前的中樞 proxy 的參考。</span><span class="sxs-lookup"><span data-stu-id="e887b-251">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="e887b-252">中樞 proxy 的預設參考看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="e887b-252">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="e887b-253">**正確參考中樞 proxy 的 HTML 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="e887b-253">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="e887b-254">「 使用者程式碼未處理 RuntimeBinderException 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-254">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="e887b-255">可能會發生此錯誤時的不正確的多載`Hub.On`用。</span><span class="sxs-lookup"><span data-stu-id="e887b-255">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="e887b-256">如果方法具有傳回值，必須指定的傳回型別為泛型型別參數：</span><span class="sxs-lookup"><span data-stu-id="e887b-256">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="e887b-257">**（不含產生的 proxy) 用戶端上定義的方法**</span><span class="sxs-lookup"><span data-stu-id="e887b-257">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="e887b-258">連接識別碼不一致或載入頁面之間的連線中斷</span><span class="sxs-lookup"><span data-stu-id="e887b-258">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="e887b-259">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-259">This behavior is by design.</span></span> <span data-ttu-id="e887b-260">中樞物件裝載在的頁面物件，因為當頁面重新整理時，就會終結中樞。</span><span class="sxs-lookup"><span data-stu-id="e887b-260">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="e887b-261">多頁的應用程式需要維護使用者與連線識別碼的關聯，如此它們才會載入頁面之間的一致性。</span><span class="sxs-lookup"><span data-stu-id="e887b-261">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="e887b-262">可以在伺服器上儲存的連線識別碼`ConcurrentDictionary`物件或資料庫。</span><span class="sxs-lookup"><span data-stu-id="e887b-262">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="e887b-263">"值不能是 null 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-263">"Value cannot be null" error</span></span>

<span data-ttu-id="e887b-264">目前不支援伺服器端方法，以選擇性參數。如果省略選擇性參數，則該方法會失敗。</span><span class="sxs-lookup"><span data-stu-id="e887b-264">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="e887b-265">如需詳細資訊，請參閱[選擇性參數](https://github.com/SignalR/SignalR/issues/324)。</span><span class="sxs-lookup"><span data-stu-id="e887b-265">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="e887b-266">「 Firefox 無法連接到伺服器&lt;位址&gt;"firebug 這類錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-266">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="e887b-267">這則錯誤訊息可以示 firebug 這類如果交涉的 WebSocket 傳輸失敗，而改為使用另一個的傳輸。</span><span class="sxs-lookup"><span data-stu-id="e887b-267">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="e887b-268">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-268">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="e887b-269">.NET 用戶端應用程式中的 「 遠端憑證不正確的驗證程序根據 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-269">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="e887b-270">如果您的伺服器需要自訂用戶端憑證，則您可以加入 x509certificate 連線進行要求的作業之前。</span><span class="sxs-lookup"><span data-stu-id="e887b-270">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="e887b-271">將憑證新增至連線使用`Connection.AddClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="e887b-271">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="e887b-272">驗證逾時之後，會卸除連接</span><span class="sxs-lookup"><span data-stu-id="e887b-272">Connection drops after authentication times out</span></span>

<span data-ttu-id="e887b-273">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-273">This behavior is by design.</span></span> <span data-ttu-id="e887b-274">無法修改驗證認證，當連線為作用中;若要重新整理的認證，連接必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e887b-274">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="e887b-275">使用 jQuery Mobile 時，兩次呼叫 OnConnected</span><span class="sxs-lookup"><span data-stu-id="e887b-275">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="e887b-276">jQuery Mobile 的`initializePage`函式會強制重新執行，每一頁中的指令碼以便建立第二個連接。</span><span class="sxs-lookup"><span data-stu-id="e887b-276">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="e887b-277">此問題的解決方案包括：</span><span class="sxs-lookup"><span data-stu-id="e887b-277">Solutions for this issue include:</span></span>

- <span data-ttu-id="e887b-278">包含在 JavaScript 檔案之前的 jQuery Mobile 的參考。</span><span class="sxs-lookup"><span data-stu-id="e887b-278">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="e887b-279">停用`initializePage`函式，藉由設定`$.mobile.autoInitializePage = false`。</span><span class="sxs-lookup"><span data-stu-id="e887b-279">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="e887b-280">等候頁面，即可完成初始化之前啟動連線。</span><span class="sxs-lookup"><span data-stu-id="e887b-280">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="e887b-281">使用伺服器傳送事件的 Silverlight 應用程式中發生延遲訊息</span><span class="sxs-lookup"><span data-stu-id="e887b-281">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="e887b-282">使用伺服器上 Silverlight 傳送事件時，就會延遲的訊息。</span><span class="sxs-lookup"><span data-stu-id="e887b-282">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="e887b-283">啟動連線時，若要強制長輪詢改為使用中，使用下列：</span><span class="sxs-lookup"><span data-stu-id="e887b-283">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="e887b-284">使用 「 權限遭拒 」 永遠框架通訊協定</span><span class="sxs-lookup"><span data-stu-id="e887b-284">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="e887b-285">這是已知的問題，並描述[這裡](https://github.com/SignalR/SignalR/issues/1963)。</span><span class="sxs-lookup"><span data-stu-id="e887b-285">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="e887b-286">這個徵兆可能會看到使用最新的 JQuery 程式庫。因應措施是降級 JQuery 1.8.2 您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e887b-286">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a><span data-ttu-id="e887b-287">「 InvalidOperationException： 不是有效的 web socket 要求。</span><span class="sxs-lookup"><span data-stu-id="e887b-287">"InvalidOperationException: Not a valid web socket request.</span></span>

<span data-ttu-id="e887b-288">如果使用 WebSocket 通訊協定，但是網路 proxy 正在修改的要求標頭，則可能會發生這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-288">This error may occur if the WebSocket protocol is used, but the network proxy is modifying the request headers.</span></span> <span data-ttu-id="e887b-289">解決方法是設定為允許連接埠 80 上的 WebSocket proxy。</span><span class="sxs-lookup"><span data-stu-id="e887b-289">The solution is to configure the proxy to allow WebSocket on port 80.</span></span>

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a><span data-ttu-id="e887b-290">「 例外狀況：&lt;方法名稱&gt;方法無法解析"當用戶端呼叫在伺服器上的方法</span><span class="sxs-lookup"><span data-stu-id="e887b-290">"Exception: &lt;method name&gt; method could not be resolved" when client calls method on server</span></span>

<span data-ttu-id="e887b-291">這個錯誤可能起因於使用，則無法探索 JSON 承載，例如陣列中的資料類型。</span><span class="sxs-lookup"><span data-stu-id="e887b-291">This error can result from using data types that cannot be discovered in a JSON payload, such as Array.</span></span> <span data-ttu-id="e887b-292">因應措施是使用資料型別，便可探索的 JSON，例如 IList。</span><span class="sxs-lookup"><span data-stu-id="e887b-292">The workaround is to use a data type that is discoverable by JSON, such as IList.</span></span> <span data-ttu-id="e887b-293">如需詳細資訊，請參閱[.NET 用戶端無法呼叫具有陣列參數的中樞方法](https://github.com/SignalR/SignalR/issues/2672)。</span><span class="sxs-lookup"><span data-stu-id="e887b-293">For more information, see [.NET Client unable to call hub methods with array parameters](https://github.com/SignalR/SignalR/issues/2672).</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="e887b-294">編譯和伺服器端錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-294">Compilation and server-side errors</span></span>

 <span data-ttu-id="e887b-295">下節包含可能的解決方案，編譯器和伺服器端執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-295">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="e887b-296">中樞執行個體的參考為 null</span><span class="sxs-lookup"><span data-stu-id="e887b-296">Reference to Hub instance is null</span></span>

<span data-ttu-id="e887b-297">針對每個連接所建立的 hub 執行個體，因為您無法中樞執行的個體在程式碼中自行建立。</span><span class="sxs-lookup"><span data-stu-id="e887b-297">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="e887b-298">若要從中樞之外本身的中樞上呼叫方法，請參閱[如何呼叫方法的用戶端和管理群組從中樞類別之外](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)如何取得的中樞內容的參考。</span><span class="sxs-lookup"><span data-stu-id="e887b-298">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="e887b-299">HTTPContext.Current.Session 為 null</span><span class="sxs-lookup"><span data-stu-id="e887b-299">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="e887b-300">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-300">This behavior is by design.</span></span> <span data-ttu-id="e887b-301">SignalR 不支援 ASP.NET 工作階段狀態，因為啟用工作階段狀態雙工訊息會中斷。</span><span class="sxs-lookup"><span data-stu-id="e887b-301">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="e887b-302">若要覆寫任何合適的方法</span><span class="sxs-lookup"><span data-stu-id="e887b-302">No suitable method to override</span></span>

<span data-ttu-id="e887b-303">如果您使用程式碼從舊版的文件或部落格，可能會看到這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="e887b-303">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="e887b-304">請確認未參考的已變更或已被取代的方法名稱 (例如`OnConnectedAsync`)。</span><span class="sxs-lookup"><span data-stu-id="e887b-304">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="e887b-305">HostContextExtensions.WebSocketServerUrl is null</span><span class="sxs-lookup"><span data-stu-id="e887b-305">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="e887b-306">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-306">This behavior is by design.</span></span> <span data-ttu-id="e887b-307">這個成員已被取代，不應使用。</span><span class="sxs-lookup"><span data-stu-id="e887b-307">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="e887b-308">「 名稱為 'signalr.hubs' 的路由已經在路由集合中 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-308">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="e887b-309">如果將會出現此錯誤`MapSignalR`兩次呼叫您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e887b-309">This error will be seen if `MapSignalR` is called twice by your application.</span></span> <span data-ttu-id="e887b-310">某些範例應用程式呼叫`MapSignalR`直接啟動類別; 在其他人進行的呼叫包裝函式類別中。</span><span class="sxs-lookup"><span data-stu-id="e887b-310">Some example applications call `MapSignalR` directly in the Startup class; others make the call in a wrapper class.</span></span> <span data-ttu-id="e887b-311">請確定您的應用程式不會兩者。</span><span class="sxs-lookup"><span data-stu-id="e887b-311">Ensure that your application does not do both.</span></span>

### <a name="websocket-is-not-used"></a><span data-ttu-id="e887b-312">不是 WebSocket</span><span class="sxs-lookup"><span data-stu-id="e887b-312">WebSocket is not used</span></span>

<span data-ttu-id="e887b-313">如果您已確認您的伺服器和用戶端符合 WebSocket 的需求 (列在[支援的平台](../getting-started/supported-platforms.md)文件)，您必須在伺服器上啟用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="e887b-313">If you have verified that your server and clients meet the requirements for WebSocket (listed in the [Supported Platforms](../getting-started/supported-platforms.md) document), you will need to enable WebSocket on your server.</span></span> <span data-ttu-id="e887b-314">執行此作業可以指示[這裡](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。</span><span class="sxs-lookup"><span data-stu-id="e887b-314">Instructions for doing this can be found [here](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).</span></span>

### <a name="connection-is-undefined"></a><span data-ttu-id="e887b-315">$.connection 是未定義</span><span class="sxs-lookup"><span data-stu-id="e887b-315">$.connection is undefined</span></span>

<span data-ttu-id="e887b-316">此錯誤表示，在頁面上的指令碼未正常運作，載入或無法連線到中樞 proxy，或不正確地存取。</span><span class="sxs-lookup"><span data-stu-id="e887b-316">This error indicates that either the scripts on a page are not being loaded properly, or the hub proxy is not reachable or is being accessed incorrectly.</span></span> <span data-ttu-id="e887b-317">請確認您的頁面上的指令碼參考對應指令碼載入您的專案，且，存取在瀏覽器的 /signalr/hubs 當伺服器執行。</span><span class="sxs-lookup"><span data-stu-id="e887b-317">Verify that the script references on your page correspond to the scripts loaded in your project, and that /signalr/hubs can be accessed in a browser when the server is running.</span></span>

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a><span data-ttu-id="e887b-318">找不到編譯動態運算式所需的一個或多個型別</span><span class="sxs-lookup"><span data-stu-id="e887b-318">One or more types required to compile a dynamic expression cannot be found</span></span>

<span data-ttu-id="e887b-319">此錯誤表示`Microsoft.CSharp`遺漏程式庫。</span><span class="sxs-lookup"><span data-stu-id="e887b-319">This error indicates that the `Microsoft.CSharp` library is missing.</span></span> <span data-ttu-id="e887b-320">將它加入**組件-&gt;Framework**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e887b-320">Add it in the **Assemblies-&gt;Framework** tab.</span></span>

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a><span data-ttu-id="e887b-321">無法從 Clients.Caller 強型別集線器; 或 Visual Basic 中存取呼叫者狀態"從類型 'String' 類型 'Task (Of Object)' 轉換無效 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="e887b-321">Caller state cannot be accessed from Clients.Caller in Visual Basic or in a strongly typed hub; "Conversion from type 'Task(Of Object)' to type 'String' is not valid" error</span></span>

<span data-ttu-id="e887b-322">若要存取 Visual Basic 或強型別中樞中的呼叫端狀態，請使用`Clients.CallerState`屬性 （SignalR 2.1 中引進） 而非`Clients.Caller`。</span><span class="sxs-lookup"><span data-stu-id="e887b-322">To access caller state in Visual Basic or in a strongly typed hub, use the `Clients.CallerState` property (introduced in SignalR 2.1) instead of `Clients.Caller`.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="e887b-323">Visual Studio 問題</span><span class="sxs-lookup"><span data-stu-id="e887b-323">Visual Studio issues</span></span>

<span data-ttu-id="e887b-324">本章節描述在 Visual Studio 中遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="e887b-324">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="e887b-325">指令碼文件] 節點沒有出現在 [方案總管</span><span class="sxs-lookup"><span data-stu-id="e887b-325">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="e887b-326">有些教學課程將您導向到 「 指令碼文件 」 節點，在 [方案總管] 中偵錯時。</span><span class="sxs-lookup"><span data-stu-id="e887b-326">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="e887b-327">此節點由 JavaScript 偵錯工具所產生，才會出現在 Internet Explorer 中，瀏覽器用戶端偵錯時如果使用 Chrome 或 Firefox，不會出現在節點。</span><span class="sxs-lookup"><span data-stu-id="e887b-327">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="e887b-328">如果正在執行另一個用戶端偵錯工具，例如 Silverlight 偵錯工具，也將無法執行 JavaScript 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="e887b-328">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="e887b-329">SignalR 無法運作 Visual Studio 2008 或更早版本</span><span class="sxs-lookup"><span data-stu-id="e887b-329">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="e887b-330">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="e887b-330">This behavior is by design.</span></span> <span data-ttu-id="e887b-331">SignalR 需要.NET Framework 4 或更新版本。這需要 SignalR 應用程式來開發在 Visual Studio 2010 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e887b-331">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span> <span data-ttu-id="e887b-332">SignalR 的伺服器元件需要.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="e887b-332">The server component of SignalR requires .NET Framework 4.5.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="e887b-333">IIS 問題</span><span class="sxs-lookup"><span data-stu-id="e887b-333">IIS issues</span></span>

<span data-ttu-id="e887b-334">本章節包含 Internet Information Services 的問題。</span><span class="sxs-lookup"><span data-stu-id="e887b-334">This section contains issues with Internet Information Services.</span></span>

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a><span data-ttu-id="e887b-335">在 Visual Studio 程式開發伺服器上，但未在 IIS 中的 SignalR 運作方式</span><span class="sxs-lookup"><span data-stu-id="e887b-335">SignalR works on Visual Studio development server, but not in IIS</span></span>

<span data-ttu-id="e887b-336">SignalR 也支援 IIS 7.0 和 7.5，但必須加入無副檔名 Url 的支援。</span><span class="sxs-lookup"><span data-stu-id="e887b-336">SignalR is supported on IIS 7.0 and 7.5, but support for extensionless URLs must be added.</span></span> <span data-ttu-id="e887b-337">若要加入支援無副檔名 Url，請參閱[https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)</span><span class="sxs-lookup"><span data-stu-id="e887b-337">To add support for extensionless URLs, see [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)</span></span>

<span data-ttu-id="e887b-338">SignalR 要求 （未安裝 ASP.NET 在 IIS 預設情況下） 的伺服器上安裝 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="e887b-338">SignalR requires ASP.NET to be installed on the server (ASP.NET is not installed on IIS by default).</span></span> <span data-ttu-id="e887b-339">若要安裝 ASP.NET，請參閱[ASP.NET 下載](https://www.asp.net/downloads)。</span><span class="sxs-lookup"><span data-stu-id="e887b-339">To install ASP.NET, see [ASP.NET Downloads](https://www.asp.net/downloads).</span></span>

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a><span data-ttu-id="e887b-340">Microsoft Azure 問題</span><span class="sxs-lookup"><span data-stu-id="e887b-340">Microsoft Azure issues</span></span>

<span data-ttu-id="e887b-341">本章節包含使用 Microsoft Azure 的問題。</span><span class="sxs-lookup"><span data-stu-id="e887b-341">This section contains issues with Microsoft Azure.</span></span>

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a><span data-ttu-id="e887b-342">FileLoadException 時裝載 Azure 背景工作角色中的 SignalR</span><span class="sxs-lookup"><span data-stu-id="e887b-342">FileLoadException when hosting SignalR in an Azure Worker Role</span></span>

<span data-ttu-id="e887b-343">裝載 Azure 背景工作角色中的 SignalR 例外狀況可能會造成 「 無法載入檔案或組件 ' Microsoft.Owin，Version = 2.0.0.0"。</span><span class="sxs-lookup"><span data-stu-id="e887b-343">Hosting SignalR in an Azure Worker Role might result in the exception "Could not load file or assembly 'Microsoft.Owin, Version=2.0.0.0".</span></span> <span data-ttu-id="e887b-344">這是 NuGet; 的已知的問題Azure 背景工作角色專案中，則不會自動新增繫結重新導向。</span><span class="sxs-lookup"><span data-stu-id="e887b-344">This is a known issue with NuGet; Binding redirects are not added automatically in Azure Worker Role projects.</span></span> <span data-ttu-id="e887b-345">若要修正此問題，您可以手動新增繫結重新導向。</span><span class="sxs-lookup"><span data-stu-id="e887b-345">To fix this, you can add the binding redirects manually.</span></span> <span data-ttu-id="e887b-346">加入下列行以`app.config`背景工作角色專案檔。</span><span class="sxs-lookup"><span data-stu-id="e887b-346">Add the following lines to the `app.config` file for your Worker Role project.</span></span>

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="e887b-347">會在收到訊息透過 Azure 後擋板之後變更的主題名稱</span><span class="sxs-lookup"><span data-stu-id="e887b-347">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="e887b-348">使用 Azure 的後擋板的主題會在內部，維護它們不是使用者可進行設定。</span><span class="sxs-lookup"><span data-stu-id="e887b-348">The topics used by the Azure backplane are maintained internally; they are not intended to be user-configurable.</span></span>
