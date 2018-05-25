---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x 集線器 API 指南 JavaScript 用戶端 |Microsoft 文件
author: pfletcher
description: 本文件提供適用於 SignalR JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) applic 1.1 版使用集線器 API 的簡介...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="ce37a-103">SignalR 1.x 集線器 API 指南 JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="ce37a-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="ce37a-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ce37a-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="ce37a-105">本文件提供適用於 SignalR JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) 應用程式在 1.1 版使用集線器 API 的簡介。</span><span class="sxs-lookup"><span data-stu-id="ce37a-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="ce37a-106">SignalR 中樞應用程式開發介面可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="ce37a-107">伺服端程式碼，並定義用戶端，可以呼叫的方法，呼叫用戶端執行的方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="ce37a-108">用戶端程式碼，並定義可在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="ce37a-109">SignalR 會負責為您的用戶端-伺服器一切細節。</span><span class="sxs-lookup"><span data-stu-id="ce37a-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="ce37a-110">SignalR 也能提供較低層級應用程式開發介面呼叫持續連線。</span><span class="sxs-lookup"><span data-stu-id="ce37a-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="ce37a-111">如需簡介 SignalR、 集線器及持續連線，或示範如何建置完整的 SignalR 應用程式的教學課程，請參閱[SignalR-快速入門](../getting-started/index.md)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="ce37a-112">總覽</span><span class="sxs-lookup"><span data-stu-id="ce37a-112">Overview</span></span>

<span data-ttu-id="ce37a-113">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="ce37a-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="ce37a-114">產生的 proxy，它會為您</span><span class="sxs-lookup"><span data-stu-id="ce37a-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="ce37a-115">使用產生的 proxy 的時機</span><span class="sxs-lookup"><span data-stu-id="ce37a-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="ce37a-116">用戶端安裝</span><span class="sxs-lookup"><span data-stu-id="ce37a-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="ce37a-117">如何參考動態產生的 proxy</span><span class="sxs-lookup"><span data-stu-id="ce37a-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="ce37a-118">如何建立 SignalR 的實體檔案產生 proxy</span><span class="sxs-lookup"><span data-stu-id="ce37a-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="ce37a-119">如何建立連接</span><span class="sxs-lookup"><span data-stu-id="ce37a-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="ce37a-120">$。 connection.hub 是相同的物件會建立該 $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="ce37a-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="ce37a-121">非同步執行的 start 方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="ce37a-122">如何建立跨定義域連線</span><span class="sxs-lookup"><span data-stu-id="ce37a-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="ce37a-123">如何設定連接</span><span class="sxs-lookup"><span data-stu-id="ce37a-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="ce37a-124">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="ce37a-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="ce37a-125">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="ce37a-126">如何取得 Hub 類別的 proxy</span><span class="sxs-lookup"><span data-stu-id="ce37a-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="ce37a-127">如何定義伺服器可以呼叫的用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="ce37a-128">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="ce37a-129">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="ce37a-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="ce37a-130">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="ce37a-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="ce37a-131">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="ce37a-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="ce37a-132">如需如何將伺服器或.NET 用戶端進行程式設計的文件，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="ce37a-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="ce37a-133">SignalR 中樞 API 指南-伺服器</span><span class="sxs-lookup"><span data-stu-id="ce37a-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="ce37a-134">SignalR 中樞 API 指南-.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="ce37a-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="ce37a-135">應用程式開發介面參考主題的連結是.NET 4.5 版的 API。</span><span class="sxs-lookup"><span data-stu-id="ce37a-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="ce37a-136">如果您使用.NET 4，請參閱[API 主題.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="ce37a-137">產生的 proxy，它會為您</span><span class="sxs-lookup"><span data-stu-id="ce37a-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="ce37a-138">您可以程式進行通訊與 SignalR 服務或不使用 proxy 為您產生 SignalR JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ce37a-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="ce37a-139">Proxy 會為您是簡化您用來連接，伺服器會呼叫寫入方法，並在伺服器上呼叫方法的程式碼的語法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="ce37a-140">當您撰寫程式碼呼叫伺服器方法時，產生的 proxy 可讓您使用看起來像是您執行了區域函式的語法： 您可以撰寫`serverMethod(arg1, arg2)`而不是`invoke('serverMethod', arg1, arg2)`。</span><span class="sxs-lookup"><span data-stu-id="ce37a-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="ce37a-141">如果您輸入錯誤的伺服器方法名稱，產生的 proxy 語法也可讓立即和可理解的用戶端時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ce37a-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="ce37a-142">如果您手動建立定義 proxy 的檔案，則您也可以撰寫程式碼呼叫伺服器方法取得 IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="ce37a-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="ce37a-143">例如，假設您有下列的中樞類別在伺服器上：</span><span class="sxs-lookup"><span data-stu-id="ce37a-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="ce37a-144">下列程式碼範例顯示的 JavaScript 程式碼看起來像叫用`NewContosoChatMessage`方法在伺服器和收到的引動過程上`addContosoChatMessageToPage`來自伺服器的方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="ce37a-145">**使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="ce37a-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="ce37a-146">**不產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="ce37a-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="ce37a-147">使用產生的 proxy 的時機</span><span class="sxs-lookup"><span data-stu-id="ce37a-147">When to use the generated proxy</span></span>

<span data-ttu-id="ce37a-148">如果您想要註冊的伺服器呼叫的用戶端方法的多個事件處理常式，您無法使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ce37a-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="ce37a-149">否則，您可以選擇使用產生的 proxy，或未根據您的程式碼撰寫喜好設定。</span><span class="sxs-lookup"><span data-stu-id="ce37a-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="ce37a-150">如果您選擇不使用它，您不需要參考中的"signalr/中樞 」 URL`script`用戶端程式碼中的項目。</span><span class="sxs-lookup"><span data-stu-id="ce37a-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="ce37a-151">用戶端安裝</span><span class="sxs-lookup"><span data-stu-id="ce37a-151">Client setup</span></span>

<span data-ttu-id="ce37a-152">JavaScript 用戶端需要 jQuery 和 SignalR core JavaScript 檔案的參考。</span><span class="sxs-lookup"><span data-stu-id="ce37a-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="ce37a-153">1.6.4 或主要的更新版本，例如 1.7.2、 1.8.2 或 1.9.1，必須是 jQuery 版本。</span><span class="sxs-lookup"><span data-stu-id="ce37a-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="ce37a-154">如果您決定使用產生的 proxy，您也需要 SignalR 產生 proxy JavaScript 檔案的參考。</span><span class="sxs-lookup"><span data-stu-id="ce37a-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="ce37a-155">下列範例顯示參考在 HTML 頁面會使用產生的 proxy，可能會尋找。</span><span class="sxs-lookup"><span data-stu-id="ce37a-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="ce37a-156">這些參考必須包含在此順序： jQuery SignalR core 之後，與 SignalR proxy first、 last。</span><span class="sxs-lookup"><span data-stu-id="ce37a-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="ce37a-157">如何參考動態產生的 proxy</span><span class="sxs-lookup"><span data-stu-id="ce37a-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="ce37a-158">在上述範例中，產生的 SignalR proxy 的參考是動態產生的 JavaScript 程式碼，不以實體檔案。</span><span class="sxs-lookup"><span data-stu-id="ce37a-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="ce37a-159">SignalR 的 JavaScript 程式碼建立即時 proxy 和做"signalr/中樞 」 URL 的回應中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ce37a-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="ce37a-160">如果您指定不同的基底 URL 的 SignalR 連線中伺服器上您`MapHubs`方法，以動態方式產生的 proxy 檔案的 URL 是自訂的 URL 與"/ 集線器 」 附加。</span><span class="sxs-lookup"><span data-stu-id="ce37a-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="ce37a-161">Windows 8 （Windows 市集） JavaScript 用戶端，請使用實體的 proxy 檔案而不是動態產生。</span><span class="sxs-lookup"><span data-stu-id="ce37a-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="ce37a-162">如需詳細資訊，請參閱[如何建立 SignalR 的實體檔案產生 proxy](#manualproxy)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="ce37a-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="ce37a-163">在 ASP.NET MVC 4 Razor 檢視中，請參考將 proxy 檔案參考中的應用程式根目錄使用波狀符號：</span><span class="sxs-lookup"><span data-stu-id="ce37a-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="ce37a-164">如需使用 MVC 4 的 SignalR 的詳細資訊，請參閱[SignalR 和 MVC 4 入門](tutorial-getting-started-with-signalr-and-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="ce37a-165">在 ASP.NET MVC 3 Razor 檢視中，使用`Url.Content`供您的 proxy 檔案參考：</span><span class="sxs-lookup"><span data-stu-id="ce37a-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="ce37a-166">ASP.NET Web Form 應用程式中，使用`ResolveClientUrl`檔案參考您的 proxy，或透過使用應用程式根目錄相對路徑 （開頭波狀符號） ScriptManager 註冊它：</span><span class="sxs-lookup"><span data-stu-id="ce37a-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="ce37a-167">一般而言，使用相同的方法來指定您用於 CSS 或 JavaScript 檔案的 「 signalr/中樞 」 URL。</span><span class="sxs-lookup"><span data-stu-id="ce37a-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="ce37a-168">如果您指定的 URL，而不使用波狀符號，在某些情況下您的應用程式正常運作時，您在 Visual Studio 中使用 IIS Express 測試，但部署至完整 IIS 時，會因 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="ce37a-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="ce37a-169">如需詳細資訊，請參閱**根層級資源的解析參考**中[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)MSDN 網站上的。</span><span class="sxs-lookup"><span data-stu-id="ce37a-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="ce37a-170">當您執行 Visual Studio 2012 中 web 專案在偵錯模式中，如果您使用 Internet Explorer 瀏覽器，您可以看到在 proxy 檔**方案總管 中**下**指令碼文件**，如下所示下圖。</span><span class="sxs-lookup"><span data-stu-id="ce37a-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![在 [方案總管] 的 JavaScript 產生之 proxy 檔案](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="ce37a-172">若要查看檔案的內容，請按兩下**集線器**。</span><span class="sxs-lookup"><span data-stu-id="ce37a-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="ce37a-173">如果您不使用 Visual Studio 2012 和 Internet Explorer 中，或如果您不在偵錯模式中，您也可以取得檔案的內容瀏覽至 「 signalR/中樞 」 URL。</span><span class="sxs-lookup"><span data-stu-id="ce37a-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="ce37a-174">例如，如果您的網站執行在`http://localhost:56699`，請移至`http://localhost:56699/SignalR/hubs`瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="ce37a-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="ce37a-175">如何建立 SignalR 的實體檔案產生 proxy</span><span class="sxs-lookup"><span data-stu-id="ce37a-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="ce37a-176">為動態產生之 proxy 的替代方案，您可以建立 proxy 程式碼的實體檔案，並參照該檔案。</span><span class="sxs-lookup"><span data-stu-id="ce37a-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="ce37a-177">您可以這樣做，以便控制快取，或結合在一起的行為，或當您在撰寫程式碼對伺服器方法呼叫時使用 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="ce37a-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="ce37a-178">若要建立 proxy 檔案時，執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="ce37a-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="ce37a-179">安裝[Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="ce37a-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="ce37a-180">開啟命令提示字元並瀏覽至*工具*SignalR.exe 檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce37a-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="ce37a-181">[工具] 資料夾位於下列位置：</span><span class="sxs-lookup"><span data-stu-id="ce37a-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="ce37a-182">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ce37a-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="ce37a-183">路徑您 *.dll*通常是*bin*專案資料夾中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce37a-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="ce37a-184">此命令會建立名為*立即轉譯 server.js*相同資料夾中*signalr.exe*。</span><span class="sxs-lookup"><span data-stu-id="ce37a-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="ce37a-185">Put*立即轉譯 server.js*檔中適當的資料夾，在您的專案中，視您的應用程式，將它重新命名並加入它的參考取代"signalr/中樞"參考。</span><span class="sxs-lookup"><span data-stu-id="ce37a-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="ce37a-186">如何建立連接</span><span class="sxs-lookup"><span data-stu-id="ce37a-186">How to establish a connection</span></span>

<span data-ttu-id="ce37a-187">您可以建立連線之前，您必須建立連線物件、 建立 proxy 和註冊事件處理常式可以從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="ce37a-188">設定 proxy 和事件處理常式，來建立連接呼叫`start`方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="ce37a-189">如果您使用產生的 proxy，您不需要在自己的程式碼中建立的連接物件，因為產生的 proxy 程式碼會為您。</span><span class="sxs-lookup"><span data-stu-id="ce37a-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="ce37a-190">**（與產生的 proxy) 建立連線**</span><span class="sxs-lookup"><span data-stu-id="ce37a-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="ce37a-191">**建立連線 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="ce37a-192">範例程式碼會使用預設 「 / signalr 」 來連線到 SignalR 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="ce37a-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="ce37a-193">如需如何指定不同的基底 URL，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="ce37a-194">通常您會註冊事件處理常式，然後再呼叫`start`方法，以建立連接。</span><span class="sxs-lookup"><span data-stu-id="ce37a-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="ce37a-195">如果您想要註冊一些事件處理常式建立連接後，您可以這樣做，但您必須註冊至少其中一個會在呼叫之前您事件而`start`方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="ce37a-196">其中一個原因是應用程式中，可以有許多集線器，但不會想要觸發`OnConnected`上每個中樞，如果您打算只使用其中一個事件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="ce37a-197">建立連接時，中樞 proxy 上的用戶端方法的存在是什麼告知觸發 SignalR`OnConnected`事件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="ce37a-198">如果您未註冊任何事件處理常式，然後再呼叫`start`方法，您將能夠叫用中樞，但是中樞的方法`OnConnected`方法不會被呼叫，而且沒有用戶端將會叫用方法從伺服器。</span><span class="sxs-lookup"><span data-stu-id="ce37a-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="ce37a-199">$。 connection.hub 是相同的物件會建立該 $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="ce37a-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="ce37a-200">當您使用產生的 proxy 時，您可以看到範例中，從`$.connection.hub`指的是連接物件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="ce37a-201">這是您藉由呼叫取得的相同物件`$.hubConnection()`時您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ce37a-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="ce37a-202">產生的 proxy 程式碼會執行下列陳述式來建立您的連接：</span><span class="sxs-lookup"><span data-stu-id="ce37a-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![產生的 proxy 檔案中建立的連接](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="ce37a-204">當您使用產生的 proxy 時，您可以執行任何動作`$.connection.hub`，您可以使用連接物件時您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ce37a-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="ce37a-205">非同步執行的 start 方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="ce37a-206">`start`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="ce37a-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="ce37a-207">它會傳回[jQuery 延期物件](http://api.jquery.com/category/deferred-object/)，這表示，您可以加入回呼函式所呼叫的方法，例如`pipe`， `done`，和`fail`。</span><span class="sxs-lookup"><span data-stu-id="ce37a-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="ce37a-208">如果您想要執行連線建立之後的程式碼，例如伺服器方法呼叫，將該程式碼中的回呼函式，或從回呼函式中呼叫它。</span><span class="sxs-lookup"><span data-stu-id="ce37a-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="ce37a-209">`.done`回呼方法執行之後已建立的連線，以及任何程式碼之後，您必須您`OnConnected`執行完成在伺服器上的事件處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="ce37a-210">如果您將 「 立即連線 」 陳述式前面範例中的為下一行程式碼之後`start`方法呼叫 (不在`.done`回呼)、`console.log`會執行列之前已建立的連線，如下列所示範例：</span><span class="sxs-lookup"><span data-stu-id="ce37a-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![錯誤的方式來撰寫執行連線建立之後的程式碼](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="ce37a-212">如何建立跨定義域連線</span><span class="sxs-lookup"><span data-stu-id="ce37a-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="ce37a-213">通常如果瀏覽器中載入的頁面上，從`http://contoso.com`，SignalR 連線是在相同網域中，在`http://contoso.com/signalr`。</span><span class="sxs-lookup"><span data-stu-id="ce37a-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="ce37a-214">如果將頁面從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連接。</span><span class="sxs-lookup"><span data-stu-id="ce37a-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="ce37a-215">基於安全性理由，預設會停用跨網域連線。</span><span class="sxs-lookup"><span data-stu-id="ce37a-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="ce37a-216">若要建立跨網域連線，請確定在伺服器上，確定已啟用跨網域連線，並指定連接 URL 當您建立連線物件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="ce37a-217">SignalR 會使用適當的技術進行跨網域連線，例如[JSONP](http://en.wikipedia.org/wiki/JSONP)或[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="ce37a-218">在伺服器上，啟用跨網域連線選取該選項，當您呼叫`MapHubs`方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="ce37a-219">在用戶端，請指定的 URL，或當您建立連接物件 （不含產生的 proxy) 時才能呼叫 start 方法 （具有產生的 proxy)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="ce37a-220">**指定的跨網域連線 （產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="ce37a-221">**指定跨網域 （不含產生的 proxy) 連接的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="ce37a-222">當您使用`$.hubConnection`建構函式，您不需要包含`signalr`在 URL 中因為它會自動加入 (除非您指定`useDefaultUrl`為`false`)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="ce37a-223">您可以建立多個連線到不同的端點。</span><span class="sxs-lookup"><span data-stu-id="ce37a-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="ce37a-224">不需要設定`jQuery.support.cors`設為 true，在程式碼中。</span><span class="sxs-lookup"><span data-stu-id="ce37a-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![不會設定為 true jQuery.support.cors](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="ce37a-226">SignalR 處理使用 JSONP 或 CORS。</span><span class="sxs-lookup"><span data-stu-id="ce37a-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="ce37a-227">設定`jQuery.support.cors`來為 true，則停用 JSONP，因為這會導致 SignalR 假設瀏覽器支援 CORS。</span><span class="sxs-lookup"><span data-stu-id="ce37a-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="ce37a-228">當您要連線到 localhost URL 時，Internet Explorer 10 將不會考慮它跨網域連線，以便應用程式會在本機使用 IE 10 即使您沒有啟用伺服器上的跨網域連線。</span><span class="sxs-lookup"><span data-stu-id="ce37a-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="ce37a-229">如需使用 Internet Explorer 9 中的跨網域的連接資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="ce37a-230">如需使用 Chrome 的跨網域的連接資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="ce37a-231">範例程式碼會使用預設 「 / signalr 」 來連線到 SignalR 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="ce37a-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="ce37a-232">如需如何指定不同的基底 URL，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="ce37a-233">如何設定連接</span><span class="sxs-lookup"><span data-stu-id="ce37a-233">How to configure the connection</span></span>

<span data-ttu-id="ce37a-234">建立連線之前，您可以指定查詢字串參數，或指定的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="ce37a-235">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="ce37a-235">How to specify query string parameters</span></span>

<span data-ttu-id="ce37a-236">如果您想要在用戶端連線時，將資料傳送到伺服器，您可以查詢字串參數加入連接物件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="ce37a-237">下列範例顯示如何在用戶端程式碼中設定的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="ce37a-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="ce37a-238">**（與產生的 proxy) 呼叫 start 方法之前設定的查詢字串值**</span><span class="sxs-lookup"><span data-stu-id="ce37a-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="ce37a-239">**設定查詢字串值，然後再呼叫 start 方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="ce37a-240">下列範例會示範如何讀取伺服器程式碼中的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="ce37a-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="ce37a-241">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-241">How to specify the transport method</span></span>

<span data-ttu-id="ce37a-242">連接的程序的一部分，SignalR 用戶端通常會與交涉的伺服器和用戶端到伺服器來判斷最佳支援的傳輸。</span><span class="sxs-lookup"><span data-stu-id="ce37a-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="ce37a-243">如果您已經知道您想要使用的傳輸，您可以略過此交涉程序指定傳輸方法，當您呼叫`start`方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="ce37a-244">**指定傳輸方法 （具有產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="ce37a-245">**指定傳輸方法 （不含產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="ce37a-246">或者，您可以指定多個傳輸方法，您想要再試一次他們的 SignalR 的順序：</span><span class="sxs-lookup"><span data-stu-id="ce37a-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="ce37a-247">**指定自訂傳輸後援配置 （與產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="ce37a-248">**指定自訂傳輸後援配置 （不含產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="ce37a-249">您可以使用下列值來指定傳輸方法：</span><span class="sxs-lookup"><span data-stu-id="ce37a-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="ce37a-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="ce37a-250">"webSockets"</span></span>
- <span data-ttu-id="ce37a-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="ce37a-251">"foreverFrame"</span></span>
- <span data-ttu-id="ce37a-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="ce37a-252">"serverSentEvents"</span></span>
- <span data-ttu-id="ce37a-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="ce37a-253">"longPolling"</span></span>

<span data-ttu-id="ce37a-254">下列範例顯示如何找出連接正在使用哪一種傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="ce37a-255">**顯示連接 （使用產生的 proxy) 所使用的傳輸方法的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="ce37a-256">**顯示連線 （不含產生的 proxy) 所使用的傳輸方法的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="ce37a-257">如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-如何取得用戶端的相關資訊的內容屬性從](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="ce37a-258">如需傳輸與後援的詳細資訊，請參閱[簡介 SignalR 的傳輸與後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="ce37a-259">如何取得 Hub 類別的 proxy</span><span class="sxs-lookup"><span data-stu-id="ce37a-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="ce37a-260">您建立每個連接物件會封裝包含一或多個 Hub 類別的 SignalR 服務的連接相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ce37a-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="ce37a-261">中樞類別與通訊，您會使用 proxy 物件，您自行建立 （如果您不使用產生的 proxy） 或為您所產生。</span><span class="sxs-lookup"><span data-stu-id="ce37a-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="ce37a-262">在用戶端 proxy 名稱會是中樞類別名稱的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="ce37a-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="ce37a-263">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。</span><span class="sxs-lookup"><span data-stu-id="ce37a-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="ce37a-264">**在伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="ce37a-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="ce37a-265">**取得中樞的產生的用戶端 proxy 的參考**</span><span class="sxs-lookup"><span data-stu-id="ce37a-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="ce37a-266">**建立 Hub 類別 （而不產生的 proxy) 的用戶端的 proxy**</span><span class="sxs-lookup"><span data-stu-id="ce37a-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="ce37a-267">如果裝飾中樞類別`HubName`屬性，請使用完整名稱，而不會變更大小寫。</span><span class="sxs-lookup"><span data-stu-id="ce37a-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="ce37a-268">**具有 HubName 屬性的伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="ce37a-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="ce37a-269">**取得中樞的產生的用戶端 proxy 的參考**</span><span class="sxs-lookup"><span data-stu-id="ce37a-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="ce37a-270">**建立 Hub 類別 （而不產生的 proxy) 的用戶端的 proxy**</span><span class="sxs-lookup"><span data-stu-id="ce37a-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="ce37a-271">如何定義伺服器可以呼叫的用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="ce37a-272">若要定義伺服器可以呼叫與中樞的方法，加入事件處理常式的中樞 proxy 使用`client`屬性產生的 proxy，或呼叫`on`方法，如果您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ce37a-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="ce37a-273">參數可以是複雜的物件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="ce37a-274">加入事件處理常式，才能呼叫`start`方法，以建立連接。</span><span class="sxs-lookup"><span data-stu-id="ce37a-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="ce37a-275">(如果您想要加入事件處理常式後呼叫`start`方法，請參閱中的附註[如何建立連接](#establishconnection)稍早在此文件，並使用定義方法，而不使用產生的 proxy 所顯示的語法。)</span><span class="sxs-lookup"><span data-stu-id="ce37a-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="ce37a-276">方法名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ce37a-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="ce37a-277">例如，`Clients.All.addContosoChatMessageToPage`在伺服器上將會執行`AddContosoChatMessageToPage`， `addContosoChatMessageToPage`，或`addcontosochatmessagetopage`用戶端上。</span><span class="sxs-lookup"><span data-stu-id="ce37a-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="ce37a-278">**（與產生的 proxy) 的用戶端上定義的方法**</span><span class="sxs-lookup"><span data-stu-id="ce37a-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="ce37a-279">**定義方法 （具有產生的 proxy) 用戶端上的替代方式**</span><span class="sxs-lookup"><span data-stu-id="ce37a-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="ce37a-280">**（不產生的 proxy，或加入之後呼叫 start 方法時） 的用戶端上定義的方法**</span><span class="sxs-lookup"><span data-stu-id="ce37a-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="ce37a-281">**伺服端程式碼呼叫的方法，用戶端**</span><span class="sxs-lookup"><span data-stu-id="ce37a-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="ce37a-282">下列範例中包含複雜的物件做為方法參數。</span><span class="sxs-lookup"><span data-stu-id="ce37a-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="ce37a-283">**採用的複雜物件 （產生的 proxy) 的用戶端上定義的方法**</span><span class="sxs-lookup"><span data-stu-id="ce37a-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="ce37a-284">**會採用複雜物件 （不含產生的 proxy) 的用戶端上定義的方法**</span><span class="sxs-lookup"><span data-stu-id="ce37a-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="ce37a-285">**定義的複雜物件的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="ce37a-286">**伺服端程式碼呼叫的用戶端方法，使用複雜的物件**</span><span class="sxs-lookup"><span data-stu-id="ce37a-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="ce37a-287">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="ce37a-287">How to call server methods from the client</span></span>

<span data-ttu-id="ce37a-288">若要從用戶端呼叫伺服器方法，使用`server`屬性產生的 proxy 或`invoke`方法上的中樞 proxy，如果您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="ce37a-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="ce37a-289">傳回值或參數可以是複雜的物件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="ce37a-290">方法名稱的 camel 案例版本通過集線器上。</span><span class="sxs-lookup"><span data-stu-id="ce37a-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="ce37a-291">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。</span><span class="sxs-lookup"><span data-stu-id="ce37a-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="ce37a-292">下列範例顯示如何呼叫沒有傳回值為伺服器方法，以及如何呼叫沒有傳回值為伺服器方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="ce37a-293">**伺服器具有屬性的方法沒有 HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="ce37a-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="ce37a-294">**伺服端程式碼定義的複雜物件傳入參數**</span><span class="sxs-lookup"><span data-stu-id="ce37a-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="ce37a-295">**叫用伺服器方法 （具有產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="ce37a-296">**叫用伺服器 （不含方法產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="ce37a-297">如果裝飾中樞方法與`HubMethodName`屬性，請使用該名稱而不會變更大小寫。</span><span class="sxs-lookup"><span data-stu-id="ce37a-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="ce37a-298">**伺服器方法**HubMethodName 屬性</span><span class="sxs-lookup"><span data-stu-id="ce37a-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="ce37a-299">**叫用伺服器方法 （具有產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="ce37a-300">**叫用伺服器 （不含方法產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="ce37a-301">前面的範例示範如何呼叫沒有傳回值為伺服器方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="ce37a-302">下列範例會示範如何呼叫傳回的值為伺服器方法。</span><span class="sxs-lookup"><span data-stu-id="ce37a-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="ce37a-303">**伺服端程式碼具有傳回值的方法**</span><span class="sxs-lookup"><span data-stu-id="ce37a-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="ce37a-304">**用於的庫存類別**傳回值</span><span class="sxs-lookup"><span data-stu-id="ce37a-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="ce37a-305">**叫用伺服器方法 （具有產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="ce37a-306">**叫用伺服器 （不含方法產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="ce37a-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="ce37a-307">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="ce37a-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="ce37a-308">SignalR 提供下列的連線，您可以控制它們的存留期事件：</span><span class="sxs-lookup"><span data-stu-id="ce37a-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="ce37a-309">`starting`： 會透過連線傳送任何資料之前引發。</span><span class="sxs-lookup"><span data-stu-id="ce37a-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="ce37a-310">`received`： 在此連接上收到任何資料時引發。</span><span class="sxs-lookup"><span data-stu-id="ce37a-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="ce37a-311">提供已接收的資料。</span><span class="sxs-lookup"><span data-stu-id="ce37a-311">Provides the received data.</span></span>
- <span data-ttu-id="ce37a-312">`connectionSlow`： 用戶端偵測到低速或經常卸除連接時引發。</span><span class="sxs-lookup"><span data-stu-id="ce37a-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="ce37a-313">`reconnecting`： 基礎傳輸可讓您開始重新連線時引發。</span><span class="sxs-lookup"><span data-stu-id="ce37a-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="ce37a-314">`reconnected`： 基礎傳輸已重新連接時引發。</span><span class="sxs-lookup"><span data-stu-id="ce37a-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="ce37a-315">`stateChanged`： 連線狀態變更時引發。</span><span class="sxs-lookup"><span data-stu-id="ce37a-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="ce37a-316">提供舊的狀態和新的狀態 （連線、 已連線、 Reconnecting 或已中斷連接）。</span><span class="sxs-lookup"><span data-stu-id="ce37a-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="ce37a-317">`disconnected`： 當連接已中斷連接時引發。</span><span class="sxs-lookup"><span data-stu-id="ce37a-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="ce37a-318">例如，如果您想要連線的問題，可能會導致明顯的延遲時顯示警告訊息，處理`connectionSlow`事件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="ce37a-319">**處理 connectionSlow 事件 （與產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="ce37a-320">**處理 connectionSlow 事件 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="ce37a-321">如需詳細資訊，請參閱[了解與 SignalR 中處理連接存留期事件](index.md)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="ce37a-322">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="ce37a-322">How to handle errors</span></span>

<span data-ttu-id="ce37a-323">提供 SignalR JavaScript 用戶端`error`可以加入的處理常式的事件。</span><span class="sxs-lookup"><span data-stu-id="ce37a-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="ce37a-324">您也可以使用 fail 方法加入處理常式所導致從伺服器方法引動過程的錯誤。</span><span class="sxs-lookup"><span data-stu-id="ce37a-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="ce37a-325">如果您未明確啟用在伺服器上的詳細的錯誤訊息，在發生錯誤之後，傳回 SignalR 的例外狀況物件包含錯誤的相關基本資訊。</span><span class="sxs-lookup"><span data-stu-id="ce37a-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="ce37a-326">例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解用途，在伺服器上使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce37a-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="ce37a-327">下列範例會示範如何加入錯誤事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="ce37a-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="ce37a-328">**加入錯誤處理常式 （與產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="ce37a-329">**加入錯誤處理常式 （而不產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="ce37a-330">下列範例顯示如何處理來自方法引動過程的錯誤。</span><span class="sxs-lookup"><span data-stu-id="ce37a-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="ce37a-331">**控制代碼 （與產生的 proxy) 的方法引動過程發生錯誤**</span><span class="sxs-lookup"><span data-stu-id="ce37a-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="ce37a-332">**控制代碼 （不含產生的 proxy) 的方法引動過程發生錯誤**</span><span class="sxs-lookup"><span data-stu-id="ce37a-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="ce37a-333">如果方法引動過程失敗，`error`也會引發事件，因此您的程式碼`error`方法處理常式並在`.fail`回呼方法會執行。</span><span class="sxs-lookup"><span data-stu-id="ce37a-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="ce37a-334">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="ce37a-334">How to enable client-side logging</span></span>

<span data-ttu-id="ce37a-335">若要啟用用戶端登入連線，`logging`屬性上的連接物件，才能呼叫`start`方法，以建立連接。</span><span class="sxs-lookup"><span data-stu-id="ce37a-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="ce37a-336">**啟用記錄 （具有產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="ce37a-337">**啟用記錄功能 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="ce37a-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="ce37a-338">若要查看記錄檔，開啟瀏覽器的開發人員工具並移至 [主控台] 索引標籤。教學課程中，顯示逐步指示和螢幕擷取畫面，顯示如何執行這項操作，請參閱[伺服器廣播透過 ASP.NET Signalr-Enable Logging](index.md)。</span><span class="sxs-lookup"><span data-stu-id="ce37a-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
