---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端 |Microsoft Docs
author: pfletcher
description: 本文件提供使用 signalr 第 2 版中 JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) applicat 的中樞 API 的簡介...
ms.author: aspnetcontent
ms.date: 09/28/2015
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: ed25843f5eb6145d29ef90f6205715bdd341d1a4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839722"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="acec2-103">ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="acec2-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="acec2-104">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="acec2-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="acec2-105">本文件提供使用 signalr 第 2 版中 JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) 應用程式的中樞 API 的簡介。</span><span class="sxs-lookup"><span data-stu-id="acec2-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="acec2-106">SignalR 中樞 API 可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。</span><span class="sxs-lookup"><span data-stu-id="acec2-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="acec2-107">在伺服器程式碼中，您定義可由用戶端，呼叫的方法，呼叫用戶端執行的方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="acec2-108">在用戶端程式碼中，您定義可以在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="acec2-109">SignalR 會處理所有為您的用戶端-伺服器配管。</span><span class="sxs-lookup"><span data-stu-id="acec2-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="acec2-110">SignalR 也提供一個名為持續連線的較低層級 API。</span><span class="sxs-lookup"><span data-stu-id="acec2-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="acec2-111">如需 SignalR、 中樞和持續連線，請參閱 < [SignalR 簡介](../getting-started/introduction-to-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="acec2-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="acec2-112">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="acec2-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="acec2-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="acec2-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="acec2-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="acec2-114">.NET 4.5</span></span>
> - <span data-ttu-id="acec2-115">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="acec2-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="acec2-116">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="acec2-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="acec2-117">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="acec2-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="acec2-118">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="acec2-118">Questions and comments</span></span>
> 
> <span data-ttu-id="acec2-119">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="acec2-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="acec2-120">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="acec2-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="acec2-121">總覽</span><span class="sxs-lookup"><span data-stu-id="acec2-121">Overview</span></span>

<span data-ttu-id="acec2-122">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="acec2-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="acec2-123">產生的 proxy，它做什麼，</span><span class="sxs-lookup"><span data-stu-id="acec2-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="acec2-124">使用產生的 proxy 的時機</span><span class="sxs-lookup"><span data-stu-id="acec2-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="acec2-125">用戶端安裝程式</span><span class="sxs-lookup"><span data-stu-id="acec2-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="acec2-126">如何參考動態產生的 proxy</span><span class="sxs-lookup"><span data-stu-id="acec2-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="acec2-127">如何建立 signalr 的實體檔案產生 proxy</span><span class="sxs-lookup"><span data-stu-id="acec2-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="acec2-128">如何建立連線</span><span class="sxs-lookup"><span data-stu-id="acec2-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="acec2-129">$。 connection.hub 是相同物件會建立該 $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="acec2-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="acec2-130">非同步執行的 start 方法</span><span class="sxs-lookup"><span data-stu-id="acec2-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="acec2-131">如何建立跨網域的連線</span><span class="sxs-lookup"><span data-stu-id="acec2-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="acec2-132">如何設定連線</span><span class="sxs-lookup"><span data-stu-id="acec2-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="acec2-133">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="acec2-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="acec2-134">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="acec2-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="acec2-135">如何取得 Hub 類別的 proxy</span><span class="sxs-lookup"><span data-stu-id="acec2-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="acec2-136">如何定義伺服器可以呼叫用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="acec2-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="acec2-137">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="acec2-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="acec2-138">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="acec2-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="acec2-139">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="acec2-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="acec2-140">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="acec2-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="acec2-141">如需如何撰寫程式的伺服器或.NET 用戶端的文件，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="acec2-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="acec2-142">SignalR 中樞 API 指南-伺服器</span><span class="sxs-lookup"><span data-stu-id="acec2-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="acec2-143">SignalR 中樞 API 指南-.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="acec2-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="acec2-144">（雖然沒有.NET 用戶端在.NET 4.0 的 SignalR 2），並僅用於.NET 4.5 SignalR 2 伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="acec2-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="acec2-145">產生的 proxy，它做什麼，</span><span class="sxs-lookup"><span data-stu-id="acec2-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="acec2-146">您可以編寫 JavaScript 用戶端通訊的 SignalR 服務，或不使用 SignalR 會為您產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="acec2-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="acec2-147">Proxy 會為您是簡化您的程式碼使用連線，寫入方法之伺服器呼叫，在伺服器上呼叫方法的語法。</span><span class="sxs-lookup"><span data-stu-id="acec2-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="acec2-148">當您撰寫程式碼呼叫伺服器方法時，產生的 proxy 可讓您使用看起來就好像您正在執行的區域函式的語法： 您可以撰寫`serverMethod(arg1, arg2)`而不是`invoke('serverMethod', arg1, arg2)`。</span><span class="sxs-lookup"><span data-stu-id="acec2-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="acec2-149">如果您拼錯伺服器方法名稱，產生的 proxy 語法也可讓立即且容易理解的用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="acec2-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="acec2-150">如果您以手動方式建立可定義 proxy 的檔案，則您也可以撰寫會呼叫伺服器方法的程式碼取得 IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="acec2-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="acec2-151">例如，假設您在伺服器上有下列的中樞類別：</span><span class="sxs-lookup"><span data-stu-id="acec2-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="acec2-152">下列程式碼範例顯示的 JavaScript 程式碼看起來像什麼叫用`NewContosoChatMessage`方法，在伺服器和收到的引動過程上`addContosoChatMessageToPage`來自伺服器的方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="acec2-153">**使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="acec2-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="acec2-154">**不產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="acec2-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="acec2-155">使用產生的 proxy 的時機</span><span class="sxs-lookup"><span data-stu-id="acec2-155">When to use the generated proxy</span></span>

<span data-ttu-id="acec2-156">如果您想要註冊的伺服器呼叫的用戶端方法的多個事件處理常式，您無法使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="acec2-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="acec2-157">否則，您可以選擇使用產生的 proxy，或不會根據您的程式碼撰寫喜好設定。</span><span class="sxs-lookup"><span data-stu-id="acec2-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="acec2-158">如果您選擇不使用它，您不需要參考中的 「 signalr/中樞 」 URL`script`用戶端程式碼中的項目。</span><span class="sxs-lookup"><span data-stu-id="acec2-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="acec2-159">用戶端安裝程式</span><span class="sxs-lookup"><span data-stu-id="acec2-159">Client setup</span></span>

<span data-ttu-id="acec2-160">JavaScript 用戶端必須參考 jQuery 和 SignalR core JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="acec2-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="acec2-161">JQuery 版本必須是 1.6.4 或主要更新版本的詳細資訊，例如 1.7.2、 1.8.2 或 1.9.1。</span><span class="sxs-lookup"><span data-stu-id="acec2-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="acec2-162">如果您決定要使用產生的 proxy，您也需要將產生的 SignalR proxy JavaScript 檔案的參考。</span><span class="sxs-lookup"><span data-stu-id="acec2-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="acec2-163">下列範例顯示參考可能會如下所示在 HTML 頁面，會使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="acec2-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="acec2-164">這些參考必須包含在此順序： jQuery 在那之後，SignalR core SignalR proxy 名字、 姓氏。</span><span class="sxs-lookup"><span data-stu-id="acec2-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="acec2-165">如何參考動態產生的 proxy</span><span class="sxs-lookup"><span data-stu-id="acec2-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="acec2-166">在上述範例中，產生的 SignalR proxy 的參考是動態產生的 JavaScript 程式碼，不以實體檔案。</span><span class="sxs-lookup"><span data-stu-id="acec2-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="acec2-167">SignalR 即時 proxy 建立的 JavaScript 程式碼，並提供給用戶端以回應至 「 signalr/中樞 」 的 URL。</span><span class="sxs-lookup"><span data-stu-id="acec2-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="acec2-168">如果您在伺服器上指定不同的基底 URL，SignalR 連線的您`MapSignalR`方法，以動態方式產生之 proxy 檔案的 URL 是您自訂的 URL，使用"/ 中樞"附加到它。</span><span class="sxs-lookup"><span data-stu-id="acec2-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="acec2-169">Windows 8 （Windows 市集） JavaScript 用戶端，請使用實體的 proxy 檔案，而不是動態產生。</span><span class="sxs-lookup"><span data-stu-id="acec2-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="acec2-170">如需詳細資訊，請參閱 <<c0> [ 如何建立 signalr 的實體檔案產生 proxy](#manualproxy)本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="acec2-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="acec2-171">在 ASP.NET MVC 4 或 5 的 Razor 檢視中，請參考您 proxy 檔案參考中的應用程式根目錄使用波狀符號：</span><span class="sxs-lookup"><span data-stu-id="acec2-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="acec2-172">如需使用 MVC 5 中的 SignalR 的詳細資訊，請參閱[開始使用 SignalR 和 MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="acec2-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="acec2-173">在 ASP.NET MVC 3 Razor 檢視中，使用`Url.Content`供您的 proxy 檔案參考：</span><span class="sxs-lookup"><span data-stu-id="acec2-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="acec2-174">在 ASP.NET Web Forms 應用程式中，使用`ResolveClientUrl`您的 proxy 檔案的參考，或透過使用應用程式的根相對路徑 （開頭為波狀符號） ScriptManager 來註冊它：</span><span class="sxs-lookup"><span data-stu-id="acec2-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="acec2-175">一般而言，使用相同的方法來指定您用於 CSS 或 JavaScript 檔案的 「 signalr/中樞 」 URL。</span><span class="sxs-lookup"><span data-stu-id="acec2-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="acec2-176">如果您指定的 URL，而不需使用波狀符號，在某些情況下您的應用程式會正常運作時您在 Visual Studio 中使用 IIS Express 測試，但部署至完整 IIS 時，將會失敗並傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="acec2-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="acec2-177">如需詳細資訊，請參閱 <<c0>  **解析根層級資源的參考**中[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="acec2-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="acec2-178">當您執行 Visual Studio 2013 中 web 專案在偵錯模式中，如果您使用 Internet Explorer 作為您的瀏覽器時，您可以看到中的 proxy 檔案**方案總管**下方**指令碼文件**，如下所示下圖。</span><span class="sxs-lookup"><span data-stu-id="acec2-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![在 [方案總管] 中的 JavaScript 產生之 proxy 檔案](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="acec2-180">若要查看檔案的內容，請按兩下**中樞**。</span><span class="sxs-lookup"><span data-stu-id="acec2-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="acec2-181">如果您不使用 Visual Studio 2012 或 2013年和 Internet Explorer 中，或如果您不在偵錯模式中，您也可以瀏覽至 「 signalR/中樞 」 的 URL 來取得檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="acec2-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="acec2-182">例如，如果您的站台還在執行`http://localhost:56699`，請移至`http://localhost:56699/SignalR/hubs`瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="acec2-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="acec2-183">如何建立 signalr 的實體檔案產生 proxy</span><span class="sxs-lookup"><span data-stu-id="acec2-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="acec2-184">為動態產生之 proxy 的替代方案，您可以建立 proxy 程式碼的實體檔案，並參考該檔案。</span><span class="sxs-lookup"><span data-stu-id="acec2-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="acec2-185">您可以控制快取或統合的行為，如，或當您撰寫的程式碼對伺服器方法的呼叫時，取得 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="acec2-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="acec2-186">若要建立 proxy 檔案，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="acec2-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="acec2-187">安裝[Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="acec2-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="acec2-188">開啟命令提示字元並瀏覽至*工具*SignalR.exe 檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="acec2-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="acec2-189">[工具] 資料夾位於下列位置：</span><span class="sxs-lookup"><span data-stu-id="acec2-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="acec2-190">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="acec2-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="acec2-191">通往您 *.dll*通常*bin*專案資料夾中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="acec2-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="acec2-192">此命令會建立名為的檔案*server.js*相同的資料夾中*signalr.exe*。</span><span class="sxs-lookup"><span data-stu-id="acec2-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="acec2-193">放*server.js*檔案中適當的資料夾，在您的專案、 將它重新命名為適用於您的應用程式，並新增對它的參考，來取代 「 signalr/中樞 」 參考。</span><span class="sxs-lookup"><span data-stu-id="acec2-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="acec2-194">如何建立連線</span><span class="sxs-lookup"><span data-stu-id="acec2-194">How to establish a connection</span></span>

<span data-ttu-id="acec2-195">您可以建立連線之前，您必須建立連線物件、 建立 proxy，並註冊事件處理常式，您可以從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="acec2-196">設定 proxy 和事件處理常式，來建立此連接呼叫`start`方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="acec2-197">如果您使用產生的 proxy，您不需要在自己的程式碼中建立的連接物件，因為產生的 proxy 程式碼會為您。</span><span class="sxs-lookup"><span data-stu-id="acec2-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="acec2-198">**（與產生的 proxy) 建立連線**</span><span class="sxs-lookup"><span data-stu-id="acec2-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="acec2-199">**建立連線 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="acec2-200">範例程式碼會使用預設值"/ signalr 」 來連線到您的 SignalR 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="acec2-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="acec2-201">如需有關如何指定不同的基底 URL 的資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-/signalr URL](hubs-api-guide-server.md#signalrurl)。</span><span class="sxs-lookup"><span data-stu-id="acec2-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="acec2-202">根據預設，中樞位置是目前的伺服器;如果您要連接到不同的伺服器，指定的 URL，然後再呼叫`start`方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="acec2-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="acec2-203">通常您會註冊事件處理常式，然後再呼叫`start`方法來建立連線。</span><span class="sxs-lookup"><span data-stu-id="acec2-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="acec2-204">如果您想要註冊一些事件處理常式建立連接後，您可以這麼做，但您必須註冊您的事件處理常式，然後再呼叫其中`start`方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="acec2-205">這個的其中一個原因是應用程式，可以有許多的中樞，但您不想要觸發`OnConnected`上每個中樞，如果您打算只使用其中一個事件。</span><span class="sxs-lookup"><span data-stu-id="acec2-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="acec2-206">中樞 proxy 上的用戶端方法的存在時建立連線時，是什麼告訴 SignalR 來觸發`OnConnected`事件。</span><span class="sxs-lookup"><span data-stu-id="acec2-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="acec2-207">如果您未註冊任何事件處理常式，然後再呼叫`start`方法中，您將能夠叫用方法，在中樞中，但是中樞的`OnConnected`方法不會被呼叫，而且沒有用戶端會叫用方法從伺服器。</span><span class="sxs-lookup"><span data-stu-id="acec2-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="acec2-208">$。 connection.hub 是相同物件會建立該 $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="acec2-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="acec2-209">當您使用產生的 proxy 時，您可以看到範例中，從`$.connection.hub`參考的連接物件。</span><span class="sxs-lookup"><span data-stu-id="acec2-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="acec2-210">這是您藉由呼叫取得的相同物件`$.hubConnection()`時您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="acec2-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="acec2-211">產生的 proxy 程式碼會執行下列陳述式來建立您的連接：</span><span class="sxs-lookup"><span data-stu-id="acec2-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![在產生之 proxy 檔案中建立的連線](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="acec2-213">當您使用產生的 proxy 時，您可以執行任何動作`$.connection.hub`，您可以使用連接物件時您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="acec2-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="acec2-214">非同步執行的 start 方法</span><span class="sxs-lookup"><span data-stu-id="acec2-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="acec2-215">`start`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="acec2-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="acec2-216">它會傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)，這表示您可以藉由呼叫方法，像是新增回呼函式`pipe`， `done`，和`fail`。</span><span class="sxs-lookup"><span data-stu-id="acec2-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="acec2-217">如果您有您想要在連線建立之後執行的程式碼，例如伺服器方法呼叫，該程式碼放入回呼函式，或從回呼函式呼叫它。</span><span class="sxs-lookup"><span data-stu-id="acec2-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="acec2-218">`.done`回呼方法之後所建立的連線，以及任何程式碼之後，您必須執行您`OnConnected`事件處理常式方法，在伺服器上的完成執行。</span><span class="sxs-lookup"><span data-stu-id="acec2-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="acec2-219">如果您將 「 現在已連線 」 陳述式將前述範例中為程式碼之後的下一行`start`方法呼叫 (不是在`.done`回呼)，則`console.log`列將之前，先執行連線，如下列所示範例：</span><span class="sxs-lookup"><span data-stu-id="acec2-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![錯誤的方式，撰寫程式碼執行連線之後，](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="acec2-221">如何建立跨網域的連線</span><span class="sxs-lookup"><span data-stu-id="acec2-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="acec2-222">通常如果瀏覽器載入頁面上，從`http://contoso.com`，SignalR 連線位於相同的網域， `http://contoso.com/signalr`。</span><span class="sxs-lookup"><span data-stu-id="acec2-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="acec2-223">如果頁面上，從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連線。</span><span class="sxs-lookup"><span data-stu-id="acec2-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="acec2-224">基於安全性理由，預設會停用跨網域的連線。</span><span class="sxs-lookup"><span data-stu-id="acec2-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="acec2-225">Signalr 1.x 中的，跨網域要求是由單一 EnableCrossDomain 旗標控制。</span><span class="sxs-lookup"><span data-stu-id="acec2-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="acec2-226">這個旗標控制 JSONP 及 CORS 要求。</span><span class="sxs-lookup"><span data-stu-id="acec2-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="acec2-227">所有的 CORS 支援較大的彈性，已經移除了 SignalR 的伺服器元件 （JavaScript 用戶端仍然使用 CORS 通常如果它偵測到瀏覽器支援它），以及新的 OWIN 中介軟體已可支援這些案例。</span><span class="sxs-lookup"><span data-stu-id="acec2-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="acec2-228">如果 JSONP 需要在用戶端 （以支援跨網域要求在較舊的瀏覽器中），它必須明確啟用 splittunneling`EnableJSONP`上`HubConfiguration`物件到`true`，如下所示。</span><span class="sxs-lookup"><span data-stu-id="acec2-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="acec2-229">JSONP 是預設為停用，其會比 CORS 較不安全。</span><span class="sxs-lookup"><span data-stu-id="acec2-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="acec2-230">**Microsoft.Owin.Cors 加入您的專案：** 若要安裝此程式庫，請在套件管理員主控台中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="acec2-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="acec2-231">此命令會新增 2.1.0 版本的套件至您的專案。</span><span class="sxs-lookup"><span data-stu-id="acec2-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="acec2-232">呼叫 UseCors</span><span class="sxs-lookup"><span data-stu-id="acec2-232">Calling UseCors</span></span>

 <span data-ttu-id="acec2-233">下列程式碼片段示範如何實作 SignalR 2 中的跨網域連接。</span><span class="sxs-lookup"><span data-stu-id="acec2-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="acec2-234">**實作 SignalR 2 中的跨網域要求**</span><span class="sxs-lookup"><span data-stu-id="acec2-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="acec2-235">下列程式碼示範如何啟用 CORS 或 JSONP SignalR 2 專案中。</span><span class="sxs-lookup"><span data-stu-id="acec2-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="acec2-236">此程式碼範例會使用`Map`並`RunSignalR`而不是`MapSignalR`，如此一來，只會針對需要 CORS 支援 SignalR 要求的 CORS 中介軟體會執行 (而不是針對在指定的路徑上的所有流量`MapSignalR`。)對應也可以用於任何其他中介軟體需要執行特定的 URL 前置詞，而不是整個應用程式。</span><span class="sxs-lookup"><span data-stu-id="acec2-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="acec2-237">不需要設定`jQuery.support.cors`設為 true，在您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="acec2-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![未將 jQuery.support.cors 設定為 true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="acec2-239">SignalR 處理 CORS 的使用。</span><span class="sxs-lookup"><span data-stu-id="acec2-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="acec2-240">設定`jQuery.support.cors`來為 true，則停用 JSONP，因為這會導致 SignalR 假設瀏覽器支援 CORS。</span><span class="sxs-lookup"><span data-stu-id="acec2-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="acec2-241">當您要連線至 localhost URL 時，Internet Explorer 10 不會將它視為跨網域連接，好讓應用程式能夠在本機搭配 IE 10 即使您尚未啟用伺服器上的跨網域連線。</span><span class="sxs-lookup"><span data-stu-id="acec2-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="acec2-242">如需使用 Internet Explorer 9 中的跨網域的連線資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)。</span><span class="sxs-lookup"><span data-stu-id="acec2-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="acec2-243">如需使用 Chrome 中的跨網域的連線資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)。</span><span class="sxs-lookup"><span data-stu-id="acec2-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="acec2-244">範例程式碼會使用預設值"/ signalr 」 來連線到您的 SignalR 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="acec2-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="acec2-245">如需有關如何指定不同的基底 URL 的資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-/signalr URL](hubs-api-guide-server.md#signalrurl)。</span><span class="sxs-lookup"><span data-stu-id="acec2-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="acec2-246">如何設定連線</span><span class="sxs-lookup"><span data-stu-id="acec2-246">How to configure the connection</span></span>

<span data-ttu-id="acec2-247">建立連線之前，您可以指定查詢字串參數，或指定的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="acec2-248">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="acec2-248">How to specify query string parameters</span></span>

<span data-ttu-id="acec2-249">如果您想要將資料傳送至伺服器，用戶端連線時，您可以加入連接物件來查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="acec2-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="acec2-250">下列範例示範如何在用戶端程式碼中設定查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="acec2-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="acec2-251">**在呼叫 start 方法 （使用產生的 proxy) 之前設定的查詢字串值**</span><span class="sxs-lookup"><span data-stu-id="acec2-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="acec2-252">**設定查詢字串值，然後再呼叫 start 方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="acec2-253">下列範例示範如何讀取伺服器程式碼中的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="acec2-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="acec2-254">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="acec2-254">How to specify the transport method</span></span>

<span data-ttu-id="acec2-255">連接的程序的一部分，通常與伺服器，以判斷最佳傳輸所支援的伺服器和用戶端交涉，SignalR 用戶端。</span><span class="sxs-lookup"><span data-stu-id="acec2-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="acec2-256">如果您已經知道您想要使用哪一個的傳輸，您可以略過此交涉程序指定的傳輸方法，當您呼叫`start`方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="acec2-257">**指定的傳輸方法 （產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="acec2-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="acec2-258">**指定傳輸方法 （不含產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="acec2-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="acec2-259">或者，您可以指定多個傳輸方法，您想要嘗試的 SignalR 的順序：</span><span class="sxs-lookup"><span data-stu-id="acec2-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="acec2-260">**指定自訂傳輸後援配置 （使用產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="acec2-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="acec2-261">**指定自訂傳輸後援配置 （不含產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="acec2-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="acec2-262">您可以使用下列值來指定傳輸方法：</span><span class="sxs-lookup"><span data-stu-id="acec2-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="acec2-263">「 webSockets"</span><span class="sxs-lookup"><span data-stu-id="acec2-263">"webSockets"</span></span>
- <span data-ttu-id="acec2-264">「 foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="acec2-264">"foreverFrame"</span></span>
- <span data-ttu-id="acec2-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="acec2-265">"serverSentEvents"</span></span>
- <span data-ttu-id="acec2-266">「 longPolling"</span><span class="sxs-lookup"><span data-stu-id="acec2-266">"longPolling"</span></span>

<span data-ttu-id="acec2-267">下列範例顯示如何找出連接正在使用哪一種傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="acec2-268">**顯示連接 （使用產生的 proxy) 所使用的傳輸方法的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="acec2-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="acec2-269">**顯示連接 （不含產生的 proxy) 所使用的傳輸方法的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="acec2-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="acec2-270">如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-Server-如何取得用戶端的資訊，從內容屬性](hubs-api-guide-server.md#contextproperty)。</span><span class="sxs-lookup"><span data-stu-id="acec2-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="acec2-271">如需有關傳輸與後援的詳細資訊，請參閱[SignalR-傳輸和後援簡介](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="acec2-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="acec2-272">如何取得 Hub 類別的 proxy</span><span class="sxs-lookup"><span data-stu-id="acec2-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="acec2-273">每個您所建立的連線物件會封裝包含一或多個中樞類別 SignalR 服務的連接相關資訊。</span><span class="sxs-lookup"><span data-stu-id="acec2-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="acec2-274">與 Hub 類別進行通訊，您會使用 proxy 物件，您會建立您自己 （如果您不使用產生的 proxy） 或其為您產生。</span><span class="sxs-lookup"><span data-stu-id="acec2-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="acec2-275">在用戶端 proxy 的名稱會是中樞類別名稱的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="acec2-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="acec2-276">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。</span><span class="sxs-lookup"><span data-stu-id="acec2-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="acec2-277">**在伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="acec2-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="acec2-278">**取得產生的用戶端 proxy 的參考中樞**</span><span class="sxs-lookup"><span data-stu-id="acec2-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="acec2-279">**建立中樞類別 （不含產生的 proxy) 的用戶端 proxy**</span><span class="sxs-lookup"><span data-stu-id="acec2-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="acec2-280">如果裝飾您的中樞類別與`HubName`屬性，請使用完整名稱，而不變更大小寫。</span><span class="sxs-lookup"><span data-stu-id="acec2-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="acec2-281">**HubName 屬性的伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="acec2-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="acec2-282">**取得產生的用戶端 proxy 的參考中樞**</span><span class="sxs-lookup"><span data-stu-id="acec2-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="acec2-283">**建立中樞類別 （不含產生的 proxy) 的用戶端 proxy**</span><span class="sxs-lookup"><span data-stu-id="acec2-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="acec2-284">如何定義伺服器可以呼叫用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="acec2-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="acec2-285">若要定義伺服器可以從中樞呼叫的方法，加入事件處理常式的中樞 proxy 使用`client`屬性產生的 proxy，或是呼叫`on`方法，如果您未使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="acec2-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="acec2-286">參數可以是複雜物件。</span><span class="sxs-lookup"><span data-stu-id="acec2-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="acec2-287">新增事件處理常式，然後再呼叫`start`方法來建立連線。</span><span class="sxs-lookup"><span data-stu-id="acec2-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="acec2-288">(如果您想要新增事件處理常式之後呼叫`start`方法，請參閱中的附註[如何建立連接](#establishconnection)稍早在此文件，並使用定義方法，而不需使用產生的 proxy 所顯示的語法。)</span><span class="sxs-lookup"><span data-stu-id="acec2-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="acec2-289">方法名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="acec2-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="acec2-290">例如，`Clients.All.addContosoChatMessageToPage`伺服器上將會執行`AddContosoChatMessageToPage`， `addContosoChatMessageToPage`，或`addcontosochatmessagetopage`用戶端上。</span><span class="sxs-lookup"><span data-stu-id="acec2-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="acec2-291">**定義用戶端 （使用產生的 proxy) 上的方法**</span><span class="sxs-lookup"><span data-stu-id="acec2-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="acec2-292">**若要定義方法 （具有產生的 proxy) 用戶端上的替代方式**</span><span class="sxs-lookup"><span data-stu-id="acec2-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="acec2-293">**定義方法，在用戶端 （而不需要產生的 proxy，或新增之後呼叫 start 方法時）**</span><span class="sxs-lookup"><span data-stu-id="acec2-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="acec2-294">**伺服端程式碼，以呼叫用戶端方法**</span><span class="sxs-lookup"><span data-stu-id="acec2-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="acec2-295">下列範例中包含複雜物件作為方法參數。</span><span class="sxs-lookup"><span data-stu-id="acec2-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="acec2-296">**定義方法，採用複雜的物件 （與產生的 proxy) 的用戶端上**</span><span class="sxs-lookup"><span data-stu-id="acec2-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="acec2-297">**定義可接受 （不含產生的 proxy) 的複雜物件的用戶端上的方法**</span><span class="sxs-lookup"><span data-stu-id="acec2-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="acec2-298">**定義的複雜物件的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="acec2-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="acec2-299">**伺服端程式碼會呼叫使用複雜物件的用戶端方法**</span><span class="sxs-lookup"><span data-stu-id="acec2-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="acec2-300">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="acec2-300">How to call server methods from the client</span></span>

<span data-ttu-id="acec2-301">若要從用戶端呼叫伺服器方法，使用`server`屬性所產生的 proxy 或`invoke`中樞 proxy，如果您未使用產生的 proxy 上的方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="acec2-302">傳回值或參數可以是複雜的物件。</span><span class="sxs-lookup"><span data-stu-id="acec2-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="acec2-303">在中樞傳入的方法名稱的 camel 命名法大小寫版本。</span><span class="sxs-lookup"><span data-stu-id="acec2-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="acec2-304">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。</span><span class="sxs-lookup"><span data-stu-id="acec2-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="acec2-305">下列範例會示範如何呼叫伺服器方法沒有傳回值，以及如何呼叫沒有傳回值為伺服器方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="acec2-306">**伺服器具有屬性的方法沒有 HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="acec2-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="acec2-307">**伺服端程式碼定義複雜的物件傳入的參數**</span><span class="sxs-lookup"><span data-stu-id="acec2-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="acec2-308">**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="acec2-309">**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="acec2-310">如果裝飾適用的中樞方法的`HubMethodName`屬性，請使用該名稱，而不變更大小寫。</span><span class="sxs-lookup"><span data-stu-id="acec2-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="acec2-311">**伺服器方法**HubMethodName 屬性</span><span class="sxs-lookup"><span data-stu-id="acec2-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="acec2-312">**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="acec2-313">**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="acec2-314">上述範例示範如何呼叫沒有傳回值為伺服器方法。</span><span class="sxs-lookup"><span data-stu-id="acec2-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="acec2-315">下列範例示範如何呼叫伺服器方法，其傳回值。</span><span class="sxs-lookup"><span data-stu-id="acec2-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="acec2-316">**伺服端程式碼之方法的傳回值**</span><span class="sxs-lookup"><span data-stu-id="acec2-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="acec2-317">**用於的庫存類別**傳回值</span><span class="sxs-lookup"><span data-stu-id="acec2-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="acec2-318">**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="acec2-319">**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="acec2-320">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="acec2-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="acec2-321">SignalR 提供以下連線，您可以處理存留期事件：</span><span class="sxs-lookup"><span data-stu-id="acec2-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="acec2-322">`starting`： 透過連線傳送任何資料之前引發。</span><span class="sxs-lookup"><span data-stu-id="acec2-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="acec2-323">`received`： 在此連接上收到任何資料時引發。</span><span class="sxs-lookup"><span data-stu-id="acec2-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="acec2-324">提供已接收的資料。</span><span class="sxs-lookup"><span data-stu-id="acec2-324">Provides the received data.</span></span>
- <span data-ttu-id="acec2-325">`connectionSlow`： 當用戶端偵測到較慢或經常卸除連接時引發。</span><span class="sxs-lookup"><span data-stu-id="acec2-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="acec2-326">`reconnecting`： 基礎傳輸可讓您開始重新連線時引發。</span><span class="sxs-lookup"><span data-stu-id="acec2-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="acec2-327">`reconnected`： 當基礎傳輸已重新連接時引發。</span><span class="sxs-lookup"><span data-stu-id="acec2-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="acec2-328">`stateChanged`： 連線狀態變更時引發。</span><span class="sxs-lookup"><span data-stu-id="acec2-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="acec2-329">提供的舊狀態和新的狀態 （連接、 已連線、 正在重新連線或已中斷連線）。</span><span class="sxs-lookup"><span data-stu-id="acec2-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="acec2-330">`disconnected`： 當連接已中斷連線時引發。</span><span class="sxs-lookup"><span data-stu-id="acec2-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="acec2-331">例如，如果您想要顯示警告訊息，可能會造成明顯延遲的連線問題時，處理`connectionSlow`事件。</span><span class="sxs-lookup"><span data-stu-id="acec2-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="acec2-332">**（使用產生的 proxy) 來處理 connectionSlow 事件**</span><span class="sxs-lookup"><span data-stu-id="acec2-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="acec2-333">**處理 connectionSlow 事件 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="acec2-334">如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件 SignalR](handling-connection-lifetime-events.md)。</span><span class="sxs-lookup"><span data-stu-id="acec2-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="acec2-335">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="acec2-335">How to handle errors</span></span>

<span data-ttu-id="acec2-336">SignalR JavaScript 用戶端提供`error`可以加入的處理常式的事件。</span><span class="sxs-lookup"><span data-stu-id="acec2-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="acec2-337">您也可以使用 fail 方法將從伺服器方法引動過程所產生的錯誤處理常式。</span><span class="sxs-lookup"><span data-stu-id="acec2-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="acec2-338">如果您未明確啟用在伺服器上的詳細的錯誤訊息，SignalR 在發生錯誤之後所傳回的例外狀況物件包含有關錯誤的最少資訊。</span><span class="sxs-lookup"><span data-stu-id="acec2-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="acec2-339">例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解的目的，在伺服器上使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="acec2-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="acec2-340">下列範例示範如何新增錯誤事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="acec2-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="acec2-341">**新增錯誤處理常式 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="acec2-342">**新增錯誤處理常式 （而不需要產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="acec2-343">下列範例示範如何處理來自方法引動過程的錯誤。</span><span class="sxs-lookup"><span data-stu-id="acec2-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="acec2-344">**控制代碼 （與產生的 proxy) 的方法引動過程發生錯誤**</span><span class="sxs-lookup"><span data-stu-id="acec2-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="acec2-345">**控制代碼 （不含產生的 proxy) 的方法引動過程發生錯誤**</span><span class="sxs-lookup"><span data-stu-id="acec2-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="acec2-346">如果在方法引動過程失敗，`error`也會引發事件，因此您的程式碼`error`方法處理常式和`.fail`方法回呼會執行。</span><span class="sxs-lookup"><span data-stu-id="acec2-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="acec2-347">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="acec2-347">How to enable client-side logging</span></span>

<span data-ttu-id="acec2-348">若要啟用用戶端登入連線，請設定`logging`屬性上的連接物件，然後再呼叫`start`方法來建立連線。</span><span class="sxs-lookup"><span data-stu-id="acec2-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="acec2-349">**啟用記錄 （與產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="acec2-350">**啟用記錄功能 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="acec2-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="acec2-351">若要查看記錄檔，開啟您的瀏覽器開發人員工具，並移至 [主控台] 索引標籤。顯示的逐步指示和螢幕擷取畫面示範如何執行這項操作的教學課程，請參閱[伺服器廣播與 ASP.NET Signalr-Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging)。</span><span class="sxs-lookup"><span data-stu-id="acec2-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
