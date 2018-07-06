---
uid: signalr/overview/older-versions/troubleshooting
title: SignalR 疑難排解 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: 本文說明開發 SignalR 應用程式的常見問題。
ms.author: aspnetcontent
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: a869c04a18be6d3917eb93ec98a8a568b7dc5dba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826753"
---
<a name="signalr-troubleshooting-signalr-1x"></a><span data-ttu-id="8a8a5-103">SignalR 疑難排解 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="8a8a5-103">SignalR Troubleshooting (SignalR 1.x)</span></span>
====================
<span data-ttu-id="8a8a5-104">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8a8a5-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="8a8a5-105">本文件說明使用 SignalR 的常見疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-105">This document describes common troubleshooting issues with SignalR.</span></span>


<span data-ttu-id="8a8a5-106">本文件包含下列各節。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-106">This document contains the following sections.</span></span>

- [<span data-ttu-id="8a8a5-107">以無訊息方式失敗呼叫用戶端與伺服器之間的方法</span><span class="sxs-lookup"><span data-stu-id="8a8a5-107">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="8a8a5-108">其他連線問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-108">Other connection issues</span></span>](#other)
- [<span data-ttu-id="8a8a5-109">編譯和伺服器端錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-109">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="8a8a5-110">Visual Studio 問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-110">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="8a8a5-111">Internet Information Services 的問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-111">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="8a8a5-112">Azure 的問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-112">Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="8a8a5-113">以無訊息方式失敗呼叫用戶端與伺服器之間的方法</span><span class="sxs-lookup"><span data-stu-id="8a8a5-113">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="8a8a5-114">本章節描述針對方法呼叫失敗不會有意義的錯誤訊息的用戶端與伺服器之間的可能原因。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-114">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="8a8a5-115">SignalR 應用程式中，伺服器會有任何用戶端會實作; 方法的相關資訊當伺服器叫用的用戶端方法，方法名稱和參數資料傳送到用戶端，和方法，就會執行只存在於伺服器指定的格式。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-115">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="8a8a5-116">如果沒有符合的方法會找到在用戶端，會發生任何事，而且不會引發錯誤訊息的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-116">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="8a8a5-117">若要進一步調查用戶端未呼叫的方法，您可以開啟之前呼叫 start 方法上的中樞，以查看哪些呼叫來自伺服器的記錄。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-117">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="8a8a5-118">若要啟用記錄中的 JavaScript 應用程式，請參閱[如何啟用用戶端記錄 （JavaScript 用戶端版本）](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-118">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="8a8a5-119">若要啟用記錄功能在.NET 用戶端應用程式，請參閱[如何啟用用戶端記錄 （.NET 用戶端版本）](../guide-to-the-api/hubs-api-guide-net-client.md#logging)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-119">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="8a8a5-120">拼錯的方法、 不正確的方法簽章或不正確的中樞名稱</span><span class="sxs-lookup"><span data-stu-id="8a8a5-120">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="8a8a5-121">如果名稱或被呼叫的方法簽章不完全符合適當的方法，用戶端上，呼叫會失敗。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-121">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="8a8a5-122">請確認伺服器所呼叫的方法名稱符合用戶端上方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-122">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="8a8a5-123">此外，SignalR 建立中樞 proxy 使用依照 camel 命名法大小寫的方法，為適用於 JavaScript，因此呼叫的方法`SendMessage`會呼叫伺服器上`sendMessage`中用戶端 proxy。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-123">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="8a8a5-124">如果您使用`HubName`您伺服器端程式碼中的屬性，請確認所使用的名稱，符合用來在用戶端建立中樞的名稱。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-124">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="8a8a5-125">如果您不要使用`HubName`屬性，則驗證中的 JavaScript 用戶端中樞的名稱是依照 camel 命名法大小寫慣例，而不是 ChatHub chatHub 等。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-125">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="8a8a5-126">重複的用戶端上的方法名稱</span><span class="sxs-lookup"><span data-stu-id="8a8a5-126">Duplicate method name on client</span></span>

<span data-ttu-id="8a8a5-127">請確認，您不需要重複的方法只有大小寫不同的用戶端上。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-127">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="8a8a5-128">如果用戶端應用程式有一個方法，叫做`sendMessage`，確認沒有也呼叫的方法`SendMessage`以及。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-128">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="8a8a5-129">在用戶端上遺漏的 JSON 剖析器</span><span class="sxs-lookup"><span data-stu-id="8a8a5-129">Missing JSON parser on the client</span></span>

<span data-ttu-id="8a8a5-130">SignalR 需要 JSON 剖析器要出現序列化伺服器與用戶端之間的呼叫。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-130">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="8a8a5-131">如果您的用戶端沒有內建的 JSON 剖析器 （例如 Internet Explorer 7)，您必須包含在應用程式中。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-131">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="8a8a5-132">您可以下載 JSON 剖析器[此處](http://nuget.org/packages/json2)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-132">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="8a8a5-133">混用中樞 PersistentConnection 語法</span><span class="sxs-lookup"><span data-stu-id="8a8a5-133">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="8a8a5-134">SignalR 使用兩種通訊模式： 中樞和 PersistentConnections。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-134">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="8a8a5-135">呼叫這兩個通訊模型的語法是不同的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-135">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="8a8a5-136">如果您已新增中樞伺服器程式碼中，確認所有用戶端程式碼會使用適當的中樞語法。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-136">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="8a8a5-137">**建立 PersistentConnection JavaScript 用戶端中的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-137">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="8a8a5-138">**建立 Javascript 用戶端中的中樞 Proxy 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-138">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="8a8a5-139">**C# 伺服端程式碼會將路由對應到 PersistentConnection**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-139">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="8a8a5-140">**C# 伺服端程式碼，路由至中樞，或對應至多個中樞有多個應用程式**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-140">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="8a8a5-141">開始之前會加入訂用帳戶的連線</span><span class="sxs-lookup"><span data-stu-id="8a8a5-141">Connection started before subscriptions are added</span></span>

<span data-ttu-id="8a8a5-142">如果可以從伺服器呼叫的方法加入至 proxy 之前啟動中樞的連接，就不會收到訊息。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-142">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="8a8a5-143">下列 JavaScript 程式碼將不會啟動中樞正確：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-143">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="8a8a5-144">**不正確 JavaScript 用戶端程式碼，不允許中樞接收訊息**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-144">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="8a8a5-145">相反地，加入方法的訂用帳戶呼叫開始之前：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-145">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="8a8a5-146">**正確加入至中樞的 訂用帳戶的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-146">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="8a8a5-147">中樞 proxy 上遺漏的方法名稱</span><span class="sxs-lookup"><span data-stu-id="8a8a5-147">Missing method name on the hub proxy</span></span>

<span data-ttu-id="8a8a5-148">請確認訂閱用戶端上，在伺服器上定義的方法。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-148">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="8a8a5-149">即使伺服器定義的方法，它必須仍會加入至用戶端 proxy。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-149">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="8a8a5-150">方法可以透過下列方式新增至用戶端 proxy (請注意，此方法會加入至`client`中樞，而不是中樞直接的成員):</span><span class="sxs-lookup"><span data-stu-id="8a8a5-150">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="8a8a5-151">**將方法新增至中樞 proxy 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-151">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="8a8a5-152">中樞或中樞方法未宣告為公用</span><span class="sxs-lookup"><span data-stu-id="8a8a5-152">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="8a8a5-153">若要顯示在用戶端上，在中樞實作和方法必須宣告為`public`。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-153">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="8a8a5-154">從不同的應用程式存取中樞</span><span class="sxs-lookup"><span data-stu-id="8a8a5-154">Accessing hub from a different application</span></span>

<span data-ttu-id="8a8a5-155">SignalR 中樞只能透過實作 SignalR 用戶端的應用程式存取。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-155">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="8a8a5-156">SignalR 無法交互操作，與其他的通訊程式庫 （例如 SOAP 或 WCF web 服務。）如果沒有可供您的目標平台的 SignalR 用戶端，您無法直接存取伺服器的端點。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-156">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="8a8a5-157">以手動方式將資料序列化</span><span class="sxs-lookup"><span data-stu-id="8a8a5-157">Manually serializing data</span></span>

<span data-ttu-id="8a8a5-158">SignalR 會自動使用 JSON 序列化程式的方法參數那里的不需要自行進行此作業。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-158">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="8a8a5-159">遠端未在 OnDisconnected 函式中的用戶端上執行的中樞方法</span><span class="sxs-lookup"><span data-stu-id="8a8a5-159">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="8a8a5-160">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-160">This behavior is by design.</span></span> <span data-ttu-id="8a8a5-161">當`OnDisconnected`是呼叫，中樞已進入`Disconnected`不允許進一步的中樞方法呼叫的狀態。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-161">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="8a8a5-162">**C# 伺服端程式碼正確執行 OnDisconnected 事件中的 程式碼**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-162">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a><span data-ttu-id="8a8a5-163">已達到連線限制</span><span class="sxs-lookup"><span data-stu-id="8a8a5-163">Connection limit reached</span></span>

<span data-ttu-id="8a8a5-164">當用戶端作業系統，例如 Windows 7 上使用完整版的 IIS，10 連接是加諸的限制。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-164">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="8a8a5-165">當使用用戶端作業系統，請改用 IIS Express 若要避免這項限制。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-165">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="8a8a5-166">跨網域連線設定不正確</span><span class="sxs-lookup"><span data-stu-id="8a8a5-166">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="8a8a5-167">如果跨網域連接未正確設定 （的 SignalR URL 不在與裝載網頁位於相同網域的連線），沒有錯誤訊息，則連線可能失敗。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-167">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="8a8a5-168">如需如何啟用跨網域通訊的詳細資訊，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-168">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="8a8a5-169">使用 NTLM (Active Directory) 不在.NET 用戶端的連線</span><span class="sxs-lookup"><span data-stu-id="8a8a5-169">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="8a8a5-170">如果連接未正確設定，使用網域安全性的.NET 用戶端應用程式中的連接可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-170">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="8a8a5-171">若要使用 SignalR 網域環境中，將必要的連接屬性，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-171">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="8a8a5-172">**C# 用戶端程式碼實作連線認證**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-172">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="8a8a5-173">其他連線問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-173">Other connection issues</span></span>

<span data-ttu-id="8a8a5-174">本章節描述的原因和解決方案的特定徵狀或在連接期間發生的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-174">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="8a8a5-175">「 在傳送資料前，必須呼叫啟動 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-175">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="8a8a5-176">如果程式碼會在開始連接之前，先將 SignalR 物件參考，通常可以看到此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-176">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="8a8a5-177">處理常式和類似裝設，會呼叫方法，在伺服器上定義必須新增連線完成後。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-177">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="8a8a5-178">請注意，呼叫`Start`是非同步的因此程式碼之後呼叫可能會執行它之前完成。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-178">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="8a8a5-179">若要連接完全啟動之後，加入處理常式，最好是將它們放到做為參數傳遞至 start 方法的回呼函式：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-179">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="8a8a5-180">**正確地將參考 SignalR 物件的事件處理常式的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-180">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="8a8a5-181">如果仍然正在參考 SignalR 物件時，就會停止連線，也會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-181">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="8a8a5-182">「 永久 301 已移動 」 或者 「 暫時 302 已移動 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-182">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="8a8a5-183">如果專案包含名為 SignalR 會干擾自動建立 proxy 的資料夾，可能會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-183">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="8a8a5-184">若要避免這個錯誤，不使用名為的資料夾`SignalR`在您的應用程式或關閉自動 proxy 產生關閉。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-184">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="8a8a5-185">請參閱[產生的 Proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-185">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="8a8a5-186">在.NET 或 Silverlight 用戶端的 「 403 禁止 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-186">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="8a8a5-187">在其中跨網域通訊未正確啟用的跨網域環境中可能會發生這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-187">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="8a8a5-188">如需如何啟用跨網域通訊的詳細資訊，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-188">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="8a8a5-189">若要建立跨網域連接 Silverlight 用戶端中的，請參閱[來自 Silverlight 用戶端的跨網域連線](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-189">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="8a8a5-190">「 找不到 404 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-190">"404 Not Found" error</span></span>

<span data-ttu-id="8a8a5-191">有多種原因會導致此問題。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-191">There are several causes for this issue.</span></span> <span data-ttu-id="8a8a5-192">請確認下列各項：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-192">Verify all of the following:</span></span>

- <span data-ttu-id="8a8a5-193">**中樞 proxy 位址參考的格式不正確：** 參考產生的中樞 proxy 位址的格式不正確，如果經常看到此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-193">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="8a8a5-194">確認會正確地設定中樞位址的參考。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-194">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="8a8a5-195">請參閱[如何參考動態產生的 proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-195">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="8a8a5-196">**將路由新增至應用程式，然後再加入中樞路由：** 如果您的應用程式使用其他路由，請確認新增的第一個路由會呼叫`MapHubs`。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-196">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapHubs`.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="8a8a5-197">「 500 內部伺服器錯誤 」</span><span class="sxs-lookup"><span data-stu-id="8a8a5-197">"500 Internal Server Error"</span></span>

<span data-ttu-id="8a8a5-198">這是種泛泛的錯誤，可能有各種不同的原因。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-198">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="8a8a5-199">錯誤的詳細資料應該會出現在伺服器的事件記錄檔，或可透過偵錯伺服器。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-199">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="8a8a5-200">藉由開啟詳細的錯誤，伺服器上可取得更詳細的錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-200">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="8a8a5-201">如需詳細資訊，請參閱 <<c0> [ 如何處理中樞類別中的錯誤](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-201">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="8a8a5-202">「 TypeError: &lt;hubType&gt;是未定義 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-202">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="8a8a5-203">如果將產生此錯誤的呼叫`MapHubs`，不會正確。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-203">This error will result if the call to `MapHubs` is not made properly.</span></span> <span data-ttu-id="8a8a5-204">請參閱[如何註冊 SignalR 的路由，並設定 SignalR 選項](../guide-to-the-api/hubs-api-guide-server.md#route)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-204">See [How to register the SignalR route and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="8a8a5-205">JsonSerializationException 未處理使用者程式碼</span><span class="sxs-lookup"><span data-stu-id="8a8a5-205">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="8a8a5-206">請確認您傳送給您的方法的參數不包含非可序列化的型別 （例如檔案控制代碼或資料庫連接）。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-206">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="8a8a5-207">如果您需要在您不想要傳送至用戶端 （無論是安全性或序列化的原因），使用伺服器端物件上使用成員`JSONIgnore`屬性。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-207">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="8a8a5-208">「 通訊協定錯誤： 未知的傳輸 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-208">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="8a8a5-209">如果用戶端不支援 SignalR 使用的傳輸，可能會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-209">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="8a8a5-210">請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)所在的瀏覽器可以使用與 SignalR 的資訊。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-210">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="8a8a5-211">「 JavaScript 中樞 proxy 的產生具有已停用。 」</span><span class="sxs-lookup"><span data-stu-id="8a8a5-211">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="8a8a5-212">如果會發生此錯誤`DisableJavaScriptProxies`時也包括在動態產生之 proxy 的參考設定`signalr/hubs`。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-212">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="8a8a5-213">如需有關如何手動建立 proxy 的詳細資訊，請參閱 <<c0> [ 產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-213">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="8a8a5-214">"的格式不正確的連接識別碼 」 或 「 使用者身分識別不能變更期間作用中的 SignalR 連線 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-214">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="8a8a5-215">如果使用驗證時，並停止連線之前，登出用戶端，可能會出現此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-215">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="8a8a5-216">解決方法是停止前用戶端登出 SignalR 連線。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-216">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="8a8a5-217">「 無法攔截錯誤： SignalR： 找不到 jQuery。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-217">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="8a8a5-218">請確定 jQuery 參考 SignalR.js 檔案前 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-218">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="8a8a5-219">SignalR JavaScript 用戶端需要執行的 jQuery。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-219">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="8a8a5-220">請確認您參考 jQuery 是正確的所使用的路徑有效，且參考至 jQuery 是 signalr 參考之前。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-220">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="8a8a5-221">「 無法攔截 TypeError： 無法讀取屬性 '&lt;屬性&gt;' 未定義 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-221">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="8a8a5-222">從沒有 jQuery 或參考正確的中樞 proxy 會產生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-222">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="8a8a5-223">請確認您到 jQuery 和中樞 proxy 的參考正確，所使用的路徑有效，且參考 jQuery 是之前的中樞 proxy 的參考。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-223">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="8a8a5-224">中樞 proxy 的預設參考看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-224">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="8a8a5-225">**正確地參考中樞 proxy 的 HTML 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-225">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="8a8a5-226">「 使用者程式碼未處理 RuntimeBinderException 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-226">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="8a8a5-227">可能會發生此錯誤時的不正確的多載`Hub.On`用。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-227">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="8a8a5-228">如果方法具有傳回值，傳回的型別必須指定為泛型類型參數：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-228">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="8a8a5-229">**（沒有產生的 proxy) 的用戶端上定義的方法**</span><span class="sxs-lookup"><span data-stu-id="8a8a5-229">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="8a8a5-230">連線識別碼不一致或頁面載入之間的連線中斷</span><span class="sxs-lookup"><span data-stu-id="8a8a5-230">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="8a8a5-231">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-231">This behavior is by design.</span></span> <span data-ttu-id="8a8a5-232">因為中樞物件裝載在的頁面物件中，當頁面重新整理時，就會終結中樞。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-232">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="8a8a5-233">多頁應用程式需要維護使用者與連線識別碼的關聯，以便將其載入頁面之間保持一致。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-233">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="8a8a5-234">可以在伺服器上儲存的連線識別碼`ConcurrentDictionary`物件或資料庫。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-234">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="8a8a5-235">"Value cannot be null 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-235">"Value cannot be null" error</span></span>

<span data-ttu-id="8a8a5-236">目前不支援伺服器端方法，以選擇性的參數;如果省略選擇性參數，則此方法將會失敗。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-236">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="8a8a5-237">如需詳細資訊，請參閱 <<c0> [ 選擇性參數](https://github.com/SignalR/SignalR/issues/324)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-237">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="8a8a5-238">「 Firefox 無法連接到伺服器&lt;地址&gt;」 在 Firebug 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-238">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="8a8a5-239">如果 WebSocket 傳輸的交涉失敗，而另一種傳輸會改為使用相同，就可以在 Firebug 中看到此錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-239">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="8a8a5-240">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-240">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="8a8a5-241">在.NET 用戶端應用程式中的 「 遠端憑證無效根據驗證程序 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-241">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="8a8a5-242">如果您的伺服器需要自訂用戶端憑證，然後您可以加入 x509certificate 連接之前提出要求。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-242">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="8a8a5-243">將憑證新增至連線使用`Connection.AddClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-243">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="8a8a5-244">驗證逾時之後，會卸除連接</span><span class="sxs-lookup"><span data-stu-id="8a8a5-244">Connection drops after authentication times out</span></span>

<span data-ttu-id="8a8a5-245">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-245">This behavior is by design.</span></span> <span data-ttu-id="8a8a5-246">連線處於作用中; 時，就無法修改驗證認證若要重新整理認證，連接必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-246">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="8a8a5-247">使用 jQuery Mobile 時，兩次呼叫 OnConnected</span><span class="sxs-lookup"><span data-stu-id="8a8a5-247">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="8a8a5-248">jQuery Mobile 的`initializePage`函式會強制重新執行，每個頁面中的指令碼以建立第二個連接。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-248">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="8a8a5-249">此問題的解決方案包括：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-249">Solutions for this issue include:</span></span>

- <span data-ttu-id="8a8a5-250">包含您的 JavaScript 檔案之前的 jQuery Mobile 的參考。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-250">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="8a8a5-251">停用`initializePage`函式，藉由設定`$.mobile.autoInitializePage = false`。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-251">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="8a8a5-252">等候完成之前啟動連線初始化頁面。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-252">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="8a8a5-253">訊息會顯示在 Silverlight 應用程式使用伺服器傳送事件</span><span class="sxs-lookup"><span data-stu-id="8a8a5-253">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="8a8a5-254">使用伺服器在 Silverlight 上傳送事件時，會延遲訊息。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-254">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="8a8a5-255">啟動連線時，若要強制長時間輪詢要改為使用中，使用下列：</span><span class="sxs-lookup"><span data-stu-id="8a8a5-255">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="8a8a5-256">不限次數 」 權限遭拒 」 使用框架通訊協定</span><span class="sxs-lookup"><span data-stu-id="8a8a5-256">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="8a8a5-257">這是已知的問題，並說明[此處](https://github.com/SignalR/SignalR/issues/1963)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-257">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="8a8a5-258">這個徵兆可能會看到使用最新的 JQuery 程式庫;因應措施是降級至 JQuery 1.8.2 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-258">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="8a8a5-259">編譯和伺服器端錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-259">Compilation and server-side errors</span></span>

 <span data-ttu-id="8a8a5-260">下節包含可能的解決方案，編譯器和伺服器端執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-260">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="8a8a5-261">中樞執行個體的參考為 null</span><span class="sxs-lookup"><span data-stu-id="8a8a5-261">Reference to Hub instance is null</span></span>

<span data-ttu-id="8a8a5-262">針對每個連接所建立的中樞執行個體，因為您無法中樞執行的個體在程式碼中自行建立。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-262">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="8a8a5-263">若要呼叫方法，從中樞本身之外，請參閱[如何呼叫方法的用戶端和管理中樞類別外的群組](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)為濆爧髍孮中樞內容的參考。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-263">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="8a8a5-264">HTTPContext.Current.Session 為 null</span><span class="sxs-lookup"><span data-stu-id="8a8a5-264">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="8a8a5-265">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-265">This behavior is by design.</span></span> <span data-ttu-id="8a8a5-266">SignalR 不支援 ASP.NET 工作階段狀態，因為啟用的工作階段狀態，就會破壞雙工通訊。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-266">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="8a8a5-267">若要覆寫任何合適的方法</span><span class="sxs-lookup"><span data-stu-id="8a8a5-267">No suitable method to override</span></span>

<span data-ttu-id="8a8a5-268">如果您使用程式碼，從較舊的文件或部落格，您會看到此錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-268">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="8a8a5-269">請確認您所未參考的已變更或已被取代的方法名稱 (例如`OnConnectedAsync`)。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-269">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="8a8a5-270">HostContextExtensions.WebSocketServerUrl 為 null</span><span class="sxs-lookup"><span data-stu-id="8a8a5-270">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="8a8a5-271">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-271">This behavior is by design.</span></span> <span data-ttu-id="8a8a5-272">這個成員已被取代，而且不應該使用。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-272">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="8a8a5-273">「 已將名為 'signalr.hubs' 已在路由集合中 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8a8a5-273">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="8a8a5-274">將會看到此錯誤，如果`MapHubs`兩次呼叫您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-274">This error will be seen if `MapHubs` is called twice by your application.</span></span> <span data-ttu-id="8a8a5-275">某些範例應用程式會呼叫`MapHubs`直接在全域應用程式檔案中; 其他人進行的呼叫包裝函式類別中。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-275">Some example applications call `MapHubs` directly in the global application file; others make the call in a wrapper class.</span></span> <span data-ttu-id="8a8a5-276">請確定您的應用程式不會兩者。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-276">Ensure that your application does not do both.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="8a8a5-277">Visual Studio 問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-277">Visual Studio issues</span></span>

<span data-ttu-id="8a8a5-278">本節說明在 Visual Studio 中遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-278">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="8a8a5-279">指令碼文件] 節點不會出現在 [方案總管</span><span class="sxs-lookup"><span data-stu-id="8a8a5-279">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="8a8a5-280">某些我們的教學課程將您導向至 [指令碼文件] 節點，在 [方案總管] 中偵錯時。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-280">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="8a8a5-281">這個節點由 JavaScript 偵錯工具所產生，並只會出現在偵錯在 Internet Explorer; 中的瀏覽器用戶端時如果使用 Chrome 或 Firefox，不會出現的節點。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-281">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="8a8a5-282">如果正在執行的另一個用戶端偵錯工具，例如 Silverlight 偵錯工具，也將無法執行 JavaScript 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-282">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="8a8a5-283">SignalR 不適 Visual Studio 2008 或更早版本</span><span class="sxs-lookup"><span data-stu-id="8a8a5-283">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="8a8a5-284">這種行為是設計上的預期行為。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-284">This behavior is by design.</span></span> <span data-ttu-id="8a8a5-285">SignalR 需要.NET Framework 4 或更新版本;這需要在 Visual Studio 2010 或更新版本開發 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-285">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="8a8a5-286">IIS 問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-286">IIS issues</span></span>

<span data-ttu-id="8a8a5-287">本章節包含使用 Internet Information Services 的問題。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-287">This section contains issues with Internet Information Services.</span></span>

### <a name="web-site-crashes-after-maphubs-call"></a><span data-ttu-id="8a8a5-288">網站當機之後 MapHubs 呼叫</span><span class="sxs-lookup"><span data-stu-id="8a8a5-288">Web site crashes after MapHubs call</span></span>

<span data-ttu-id="8a8a5-289">SignalR 的最新版本已修正此問題。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-289">This issue has been fixed in the latest version of SignalR.</span></span> <span data-ttu-id="8a8a5-290">確認要更新您使用 NuGet 的安裝，來使用 SignalR 的最新的版本。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-290">Verify that you are using the latest released version of SignalR by updating your installation using NuGet.</span></span>

<a id="azure"></a>

## <a name="azure-issues"></a><span data-ttu-id="8a8a5-291">Azure 的問題</span><span class="sxs-lookup"><span data-stu-id="8a8a5-291">Azure issues</span></span>

<span data-ttu-id="8a8a5-292">本章節包含使用 Microsoft Azure 的問題。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-292">This section contains issues with Microsoft Azure.</span></span>

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="8a8a5-293">未接收訊息透過 Azure 後擋板之後變更的主題名稱</span><span class="sxs-lookup"><span data-stu-id="8a8a5-293">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="8a8a5-294">使用者可設定並非由 Azure 的後擋板的主題。</span><span class="sxs-lookup"><span data-stu-id="8a8a5-294">The topics used by the Azure backplane are not intended to be user-configurable.</span></span>
